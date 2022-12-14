# Tehtävänanto

Tämä projekti liittyy Tero Karvisen [Palvelinten hallinta](https://terokarvinen.com/2022/palvelinten-hallinta-2022p2/) kurssin h6 - kohta d) oma suola -tehtävänantoon.

## Projektin tarkoitus sekä ympäristö

Tässä työversiossa tähtäimenä on luoda Salt-moduuli, joka asentaa kahdelle minion-koneelle Gimp -kuvankäsittelyohjelman sekä Blender -mallinnusohjelman. Myöhemmin jatkotyöstän projektia lisäämällä moduuliin Gimpin sekä Blenderin oletusasetuksien muokkauksia.
Teen projektin VirtualBox-virtualisointiohjelmalla pyörivillä virtuaalikoneilla, jotka loin juuri tätä projektia varten.

- Masterkone: Xubuntu 22.04.1
- Minion1: Xubuntu 22.04.1
- Minion2: Debian 11.5.0

## Projektikuvaus

En kuvaa tässä tehtävässä kovin tarkasti Saltin asennusta ja käyttöönottoa, sillä se on kuvattu aikaisemmassa tehtävässä.
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

Blender ei kuitenkaan käynnisty kummallakaan minionilla. Joudun selvittämään ongelmaa jatkossa.



