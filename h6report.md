# Tehtävänanto

Tämä projekti liittyy Tero Karvisen [Palvelinten hallinta](https://terokarvinen.com/2022/palvelinten-hallinta-2022p2/) kurssin h6 - kohta d) oma suola -tehtävänantoon.

## Projektin tarkoitus sekä ympäristö

Tässä työversiossa tähtäimenä on luoda Salt-moduuli, joka asentaa kahdelle minion-koneelle Gimp -kuvankäsittelyohjelman sekä muuttaa oletusasetuksia.
Teen projektin VirtualBox-virtualisointiohjelmalla pyörivillä virtuaalikoneilla, jotka loin juuri tätä projektia varten.

- Masterkone: Xubuntu 22.04.1
- Minion1: Xubuntu 22.04.1
- Minion2: Debian 11.5.0

## Projektikuvaus

En kuvaa tässä tehtävässä kovin tarkasti Saltin asennusta ja käyttöönottoa, sillä se on kuvattu aikaisemmassa tehtävässä.
Salt master-servicen asensin master-koneeksi tarkoitetulle xUbuntu-koneelle, sekä Salt minion-servicen minoneiksi tarkoitetuille koneille.
Sitten määrittelin minioneille masterin ip-osoitteen jotta ne osaavat ottaa yhteyden masteriin, sekä annoin minioneille yllä olevan listan mukaiset id:t. Tämän jälkeen uudelleenkäynnistin minion-koneiden minion-servicet ja hyväksyin minioneiden salt-keyt master-koneella.

### Salt moduuli

Loin /srv/salt/ -hakemistoon uuden kansion nimeltä gimp. Tämän sisään loin nano-tekstieditorilla init.sls -tiedoston, johon määrittelin Gimpin asennuksen:

> `Gimp:
>>   pkg.installed`

Sitten testasin toimiiko tämä moduuli komennolla:

`sudo salt '*' state.apply gimp`





