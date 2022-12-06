# Tehtävänanto

Tämä projekti liittyy Tero Karvisen [Palvelinten hallinta](https://terokarvinen.com/2022/palvelinten-hallinta-2022p2/) kurssin h6 - kohta d) oma suola -tehtävänantoon.

## Projektin tarkoitus sekä ympäristö

Tässä työversiossa tähtäimenä on luoda Salt-moduuli, joka asentaa kolmelle minion-koneelle Gimp -kuvankäsittelyohjelman.
Teen projektin VirtualBox-virtualisointiohjelmalla pyörivillä virtuaalikoneilla, jotka loin juuri tätä projektia varten.

- Masterkone: Xubuntu 22.04.1
- Minion1: Xubuntu 22.04.1
- Minion2: Debian 11.5.0
- Minion3: Windows 10 Home

## Projektikuvaus

En kuvaa tässä tehtävässä kovin tarkasti Saltin asennusta ja käyttöönottoa, sillä se on kuvattu aikaisemmassa tehtävässä.
Salt master-servicen asensin master-koneeksi tarkoitetulle xUbuntu-koneelle, sekä Salt minion-servicen kolmelle minoneiksi tarkoitetuille koneille.
Sitten määrittelin minioneille masterin ip-osoitteen jotta ne osaavat ottaa yhteyden masteriin, sekä annoin minioneille yllä olevan listan mukaiset id:t.



