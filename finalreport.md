Tämä projekti liittyy Tero Karvisen [Palvelinten hallinta](https://terokarvinen.com/2022/palvelinten-hallinta-2022p2/) kurssiin, jonka lopputyönä tuli tehdä oma Salt-moduuli.

## Projektin tarkoitus sekä ympäristö

Projektin tarkoituksena on luoda Salt-moduuli, joka asentaa kahdelle minion-koneelle Gimp -kuvankäsittelyohjelman sekä Blender -mallinnusohjelman. Myöhemmin jatkotyöstän projektia lisäämällä moduuliin Gimpin sekä Blenderin oletusasetuksien muokkauksia.
Teen projektin VirtualBox-virtualisointiohjelmalla pyörivillä virtuaalikoneilla, jotka loin juuri tätä projektia varten.

- Masterkone: Xubuntu 22.04.1
- Minion1: Xubuntu 22.04.1
- Minion2: Debian 11.5.0

## Projektikuvaus

En kuvaa tässä tehtävässä kovin tarkasti Saltin asennusta ja käyttöönottoa, sillä se on kuvattu aikaisemmin tekemissäni tehtävissä (esim. [linkki h6-tehtävääni](https://wordpress.com/post/nellilaajaranta.wordpress.com/136), kohta c).
Salt master-servicen asensin master-koneeksi tarkoitetulle xUbuntu-koneelle, sekä Salt minion-servicen minoneiksi tarkoitetuille koneille.
Sitten määrittelin minioneille masterin ip-osoitteen jotta ne osaavat ottaa yhteyden masteriin, sekä annoin minioneille yllä olevan listan mukaiset id:t. Tämän jälkeen uudelleenkäynnistin minion-koneiden minion-servicet ja hyväksyin minioneiden salt-keyt master-koneella.

### Salt moduuli

Loin /srv/salt/ -hakemistoon moduulille uuden kansion nimeltä **projekti**. Tämän sisään loin nano-tekstieditorilla init.sls -tiedoston, johon määrittelin Gimpin asennuksen:

> `Gimp:`
>>   `pkg.installed`
>
> `blender:`
>>   `pkg.installed`

![init.sls-tiedosto](https://github.com/nellilaajaranta/saltproject/blob/main/photos/initsls.jpg)

Sitten testasin toimiiko tämä moduuli komennolla:

`sudo salt '*' state.apply projekti`

![epäonnistunut tilan ajo](https://github.com/nellilaajaranta/saltproject/blob/main/photos/failedminion1.jpg)

Komento ei onnistunut täysin. Ensimmäisellä kerralla virtuaalikoneet kaatuivat ja asennus epäonnistui. Toisella kerralla Gimp asentui mutta Blenderin kohdalla tuli virheilmoitus "dpkg was interrupted, you must manually run 'dpkg --configure -a' to correct the problem." molempien minioneiden kohdalla. Ajoin siis seuraavaksi komennon:

`sudo salt '*' cmd.run 'dpkg --configure -a'`

Komento meni onnistuneesti läpi, joten kokeilin uudeleen ajaa moduulin. Se onnistui ja nyt molemmilla minioneilla on asennettuna sekä Gimp että Blender:

![minion1](https://github.com/nellilaajaranta/saltproject/blob/main/photos/minion1%20install.jpg)

![minion2](https://github.com/nellilaajaranta/saltproject/blob/main/photos/minion2%20install.jpg)

Blender ei kuitenkaan käynnisty kummallakaan minionilla. Päätin asentaa minioneille ssh:n, jotta pääsen niihin käsiksi masterilta ja voisin tarkastella sieltä käsin mikä voisi olla syynä siihen miksi Blender ei käynnisty.
Käytin ssh:n määrittelyyn Tero Karvisen ohjetta: [Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port](https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/)

Tein /srv/salt/ alle sudoeditillä sshd.sls-tiedoston, johon liitin ylläolevan Tero Karvisen ohjeen ssh-state-filen sisällön:

![ssh-state](https://github.com/nellilaajaranta/saltproject/blob/main/photos/ssh%20state.jpg)

sekä tein sshd_config -tiedoston, jonka sisältö löytyy myös samoista Tero Karvisen ohjeista.

![sshd-config](https://github.com/nellilaajaranta/saltproject/blob/main/photos/sshdconfig.jpg)

Sen jälkeen ajoin tilan minioneille komennolla:

`sudo salt '*' state.apply sshd`

ssh-tilan ajo onnistui ja minioneilla on nyt ssh-palvelu.

Otin ssh-yhteyden minion1 koneelle seuraavalla komennolla:

`ssh -X -p 8888 minion1@minion1-virtualbox`

-X parametri määrittää sen, että voin avata ssh-yhteydellä myös graafisen käyttöliittymän ohjelmia.

Sitten annoin komennon `blender` käynnistääkseni blenderin minionilla. Blenderr käynnistyi, ja koska masterilla ei ole blenderiä, on varmaa että se käynnistyy minionilta käsin -asennuksessa ei siis ole mennyt mitään pieleen. 
Avasin tämän jälkeen itse minion1 -koneen ja avasin blenderin terminalin kautta. Se antaa seruaavan virheilmoituksen:

![blendererror](https://github.com/nellilaajaranta/saltproject/blob/main/photos/blenderminion1.jpg)

Tämä tarkoittaa että minion-virtuaalikoneilla ei ole riittävän tehokas näytönohjain. Koske ongelma ei sinänsä liity itse Salt-märittelyyn, en korjaa ongelmaa sen enempää.
