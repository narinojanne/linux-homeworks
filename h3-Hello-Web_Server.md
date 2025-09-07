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

Sallitaan Apache2:n käynnistyä automaattisesti koneen käynnistyksen yhteydessä komennolla

```

sudo systemctl enable --now apache2

```

---

# Loki

Komennolla `sudo tail -f /var/log/apache2/access.log` saa lokin päälle, josta voi tarkastella palvelimen tapahtumia terminaalissa

![kuva02](/pictures/h3/loki2.png)

Selaimen päivittäminen tuottaa lokiin tämännäköisen rivin

![kuva03](/pictures/h3/loki1.png)

- Ensin rivillä näkyy localhost ip-numero (127.0.0.1)
- Seuraavaksi on aikaleima milloin sivun päivitys on tapahtunut
- Sitten kerrotaan, että palvelimelle on lähetetty GET pyyntö ja pyyntöön vastattu koodilla 200 joka tarkoittaa OK
- Lopuksi kerrotaan vielä, että selaimena on Mozilla 5.0:n Firefox versio 128.0

---

# Etusivu uusiksi

Luodaan uusi _name based virtual host_ ja laitetaan oma sivu näkymään suoraan palvelimen etusivulla `http://localhost`  
Tehdään uuden sivun konfiguraatio tiedosto jotta Apache2 osaa näyttää sivun

```

sudoedit /etc/apache2/sites-available/hattu.example.com.conf

```

Lisätään tiedostoon säännöt verkkoliikennettä varten sekä omat lokit virheille ja tapahtumille

```

<VirtualHost *:80>

 ServerName hattu.example.com
 ServerAlias www.hattu.example.com
 DocumentRoot /home/janne/public-sites/hattu.example.com

 <Directory /home/xubuntu/publicsites/hattu.example.com>
   Require all granted
 </Directory>

 ErrorLog ${APACHE_LOG_DIR}/error-hattu.example.log
 CustomSite ${APACHE_LOG_DIR}/access-hattu.example.log combined

</VirtualHost>

```

Sallitaan sivu palvelimelle

```

sudo a2ensite hattu.example.com

```

Muokataan /etc/hosts tiedostoa

```

sudoedit /etc/hosts

```

Ja lisätään rivit

```

127.0.0.1 hattu.example.com
127.0.0.1 www.hattu.example.com

```

Ja uudelleen käynnistetään Apache2 palvelin, koska teimme muutoksia asetuksiin

```

sudo systemctl restart apache2

```

Tehdään hakemisto omia sivuja varten omaan käyttäjä hakemistoon komennolla

```

mkdir /home/janne/public-sites/hattu.example.com

```

Tarkistetaan että hakemiston oikeudet ovat kunnossa ja Apachella pääsy lukemaan hakemiston tiedostoja.

`ls -la /home/janne/public-sites/hattu.example.com` -komennolla nähdään, että index.html-tiedostolla on lukuoikeudet(r) muille käyttäjille.

![kuva04](/pictures/h3/hattu1.png)

---

`ls -la |grep public-sites` -komennolla näemme, että public-sites-hakemistolla on sekä luku(r)- että suoritusoikeudet(x) muille käyttäjille.

![kuva05](/pictures/h3/hattu2.png)

---

`ls -la /home/janne/public-sites/ |grep hattu.example.com` -komento näyttää, että myös hattu.example.com-hakemistolla sekä luku- että suoritusoikeudet muille käyttäjille.

![kuva06](/pictures/h3/hattu3.png)

---

`ls -la /home/ |grep janne` -komennolla näemme, että käyttäjän janne omalla hakemistolla on suoritusoikeudet(x) muille käyttäjille niinkuin kuuluukin olla, jotta apache  
voi lukea tarvittavia tiedostoja. Jos suoritusoikeudet muille käyttäjille puuttuu, niin ne voi lisätä komennolla `sudo chmod o+x /home` .

![kuva07](/pictures/h3/hattu4.png)

---

`ls -la / |grep home` -komennolla voimme tarkistaa vielä, että kotihakemistolla (/home) on myös suoritusoikeudet(x) muille käyttäjille.

![kuva08](/pictures/h3/hattu5.png)

---

# Lähteet

- Tehtävänanto: https://terokarvinen.com/linux-palvelimet/
- Apache2 Web Server: https://github.com/johannaheinonen/johanna-test-repo/blob/main/linux-03092025.md
