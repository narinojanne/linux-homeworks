# Tiivistelmä

---

# Asenna Apache-weppipalvelin

Olin asentanut Apache-palvelimen jo luennon aikana kun teimme sen kanssa testauksia konfiguraatioihin.  
Asennus oli hyvin yksinkertainen ja suoraviivainen komennolla

```

sudo apt-get install apache2

```

Apache2 Default sivu aukesi kun navigoin selaimella osoitteeseen:  
`http://localhost`

![kuva01](/pictures/h3/apache-default1.png)

---

# Loki

Komennolla `sudo tail -f /var/log/apache2/access.log` saa lokin päälle, josta voi tarkastella palvelimen tapahtumia

![kuva02](/pictures/h3/loki2.png)

Selaimen päivittäminen tuottaa lokiin tämännäköisen rivin

![kuva03](/pictures/h3/loki1.png)

- Ensin rivillä näkyy localhost ip-numero (127.0.0.1)
- Seuraavaksi on aikaleima milloin sivun päivitys on tapahtunut
- Sitten kerrotaan, että palvelimelle on lähetetty GET pyyntö ja pyyntöön vastattu koodilla 200 joka tarkoittaa OK
- Lopuksi kerrotaan vielä, että selaimena on Mozilla 5.0:n Firefox versio 128.0

---

# Lähteet

- Tehtävänanto: https://terokarvinen.com/linux-palvelimet/
