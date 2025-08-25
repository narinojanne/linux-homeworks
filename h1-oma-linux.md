# Raportin tiivistelmä

- raportin tulee olla toistettava
- raportista tulee selvitä mitä teit
- raportissa on hyvä olla kellonajat
- raportista pitää selvitä onnistuitko vai epäonnistuitko tehtävässä ja miten todensit onnistumisen tai epäonnistumisen
- pidä raportti helppolukuisena, muista väliotsikot ja ole huolellinen oikeinkirjoituksen kanssa
- muista viitata lähteisiin
- älä kirjoita valheita
- älä plagioi
- älä käytä ilman lupaa toisten kuvia

# Linuxin asentaminen virtuaalikoneeseen

## VirtualBox

Aloitin asennuksen lataamalla VirtualBoxin asennustiedoston [täältä](https://www.virtualbox.org/wiki/Downloads) ja käynnistämällä asennuksen.  
Asennuksen alussa sain ilmoituksen puuttuvista python core ja win32api.bindings paketeista. Googletin mitä tämä tarkoittaa ja sain selville, että näitä ei välttämättä tarvita.  
Tieto löytyy [täältä](https://forums.virtualbox.org/viewtopic.php?t=108582).  
Aloitin VirtualBoxin asennuksen uudelleen ja jätin python paketin pois asennuksesta, jolloin asennus meni läpi ja VirtualBox lähti myös käyntiin.

## Debian asennus

Seuraavaksi latasin Debian 13 levykuvan Johannan [ohjeiden](https://github.com/johannaheinonen/johanna-test-repo/blob/main/linux-20082025.md) mukaisesti.  
Latauksen jälkeen tarkistin, että lataamani tiedoston sha512sum merkkijono on sama kuin lataussivulta löytyvä sha512sum.  
Aloitin uuden virtuaalikoneen asentamisen seuraamalla samaa Johannan ohjetta, jonka avulla latasin Debian levykuvan.  
Määritin virtuaalikoneelle asennettavan käyttöjärjestelmän ja käytettävät resurssit, jonka jälkeen käynnistin virtuaalikoneen ja käyttöjärjestelmän live-asennuksen, jolla testasin verkon ja näppäimistön toimivuudet Johannan ohjeen mukaan.  
Muistin myös Teron vinkin luennolta, komennolla setxkbmap fi sain muutettua näppäimistön asettelun live-asennuksessa. Kun olin todennut, että kaikki toimii, niin aloitin varsinaisen käyttöjärjestelmän asennuksen.  
Varsinaisen käyttöjärjestelmän asennuksen aloitin klikkaamalla live-asennuksen työpöydältä löytyvää Install Debian kuvaketta, joka avasi Calamares installerin asennusta varten.  
Valitsin asennuksen aikana käyttöjärjestelmän kielen, oman sijaintini, näppäimistön kieliasetuksen, levyn alustamisen sekä käyttäjänimen ja salasanan. Tämän asennuksen jälkeen ei tarvinnut erikseen populoida lähderepositorioita.  
Asennuksen loputtua uudelleen käynnistys kuitenkin jumittui jotenkin, eikä mitään tapahtunut ainakaan viiteen minuuttiin ja yritin painella Done nappia, jotta uudelleen käynnistys alkaisi.  
En päässyt käsiksi mihinkään valikkoihin, joten päädyin pakottamaan virtuaalikoneen sammutuksen. En tiedä oliko tästä haittaa asennukselle vai ei, mutta virtuaalikone lähti kuitenkin käyntiin, kun sen käynnisti uudelleen.  
Lopuksi vielä asensin Teron ohjeiden mukaan ufw palomuurin ja VirtualBox Guest Additionsin jolla saa virtuaalikoneen resoluution paremmaksi. Teron ohje löytyy [täältä](https://terokarvinen.com/2021/install-debian-on-virtualbox/).

## Kuvat asennuksesta

![kuva1](/pictures/newVM2.png)
