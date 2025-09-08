# Tiivistelmä

- Nimipohjainen virtuaalipalvelin on yleensä yksinkertaisempi kuin IP-osoite pohjainen
- Nimipohjainen virtuaalipalvelin helpottaa IP-osoitteiden kysyntää
- ServerAliaksen avulla yhden sivun voi laittaa vastaamaan useampaan eri nimeen

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

## UFW asennus

Apache2 palvelin hyväksyy saapuvat http/https-pyynnöt julkisista IP-osoitteista, joten tarvitaan palomuuria rajoittamaan ja hallitsemaan liikennettä.

Jos UFW ei ole asennettuna vielä niin sen voi asentaa komennolla

```

sudo apt-get install ufw

```

Ennen UFW:n sallimista kannattaa avata ssh komennolla `sudo ufw allow ssh` tai `sudo ufw allow 22/tcp` varsinkin jos käytät etäyhteydellä palvelinkonetta.

UFW sallitaan komennolla `sudo ufw enable`.

Sallitaan http(80) portti

```
sudo ufw allow 80/tcp
```

Vahvistetaan, että säännöt ovat aktiivisia komennolla `sudo ufw status verbose`.

Pitäisi näkyä seuraavat säännöt

```

To          Action      From
22/tcp      ALLOW       Anywhere
80/tcp      ALLOW       Anywhere

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

 Errorlog ${APACHE_LOG_DIR}/error-hattu.example.log
 Customlog ${APACHE_LOG_DIR}/access-hattu.example.log combined

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

Lisätään seuraavat rivit

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

Lisätään index.html tiedosto uuteen hakemistoon sekä jotain tekstiä näytettäväksi selaimessa

```

nano /home/janne/public-sites/hattu.example.com/index.html

```

![kuva09](/pictures/h3/hattu6.png)

---

Tarkistetaan että hakemiston oikeudet ovat kunnossa ja Apachella on pääsy lukemaan hakemiston tiedostoja.

`ls -la /home/janne/public-sites/hattu.example.com` -komennolla nähdään, että index.html-tiedostolla on lukuoikeudet(r) muille käyttäjille.

![kuva04](/pictures/h3/hattu1.png)

---

`ls -la |grep public-sites` -komennolla näemme, että public-sites-hakemistolla on sekä luku(r)- että suoritusoikeudet(x) muille käyttäjille.

![kuva05](/pictures/h3/hattu2.png)

---

`ls -la /home/janne/public-sites/ |grep hattu.example.com` -komento näyttää, että myös hattu.example.com-hakemistolla sekä luku- että suoritusoikeudet muille käyttäjille.

![kuva06](/pictures/h3/hattu3.png)

---

`ls -la /home/ |grep janne` -komennolla näemme, että käyttäjän janne omalla hakemistolla on suoritusoikeudet(x) muille käyttäjille niinkuin kuuluukin olla, jotta apache voi lukea tarvittavia tiedostoja. Jos suoritusoikeudet muille käyttäjille puuttuu, niin ne voi lisätä komennolla `sudo chmod o+x /home` .

![kuva07](/pictures/h3/hattu4.png)

---

`ls -la / |grep home` -komennolla voimme tarkistaa vielä, että kotihakemistolla (/home) on myös suoritusoikeudet(x) muille käyttäjille.

![kuva08](/pictures/h3/hattu5.png)

---

Nyt kun hakemistojen ja tiedostojen oikeudet ovat kunnossa niin voimme kokeilla palvelimen toimivuutta.

```

curl hattu.example.com

```

Terminaalissa näkyy hakemistossa /home/janne/public-sites/hattu.example.com olevan index.html-tiedoston sisältö.

![kuva10](/pictures/h3/hattu7.png)

---

Ja kun selaimella navigoidaan osoitteeseen `http://hattu.example.com` tiedoston sisältö näkyy selaimessa.

![kuva11](/pictures/h3/hattu8.png)

---

Tämä sivu piti saada myös näkymään `http://localhost` osoitteessa, niin ratkaisin asian niin, että ensin uudelleen nimesin `/var/www/html/index.html` tiedoston, koska se on tiedosto jonka apache näyttää selaimessa oletuksena osoitteelle `localhost` ja se sijaitsee hakemistossa `/var/www/html`

```

sudo mv /var/www/html/index.html /var/www/html/copyindex.html

```

Tämän jälkeen linkkasin `/home/janne/public-sites/hattu.example.com/index.html` tiedoston `/var/www/html`-hakemistoon tekemällä siitä symbolic linkin

```

sudo ln -s /home/janne/public-sites/hattu.example.com/index.html /var/www/html/index.com

```

Ja nyt terminaalin komento `curl localhost` sekä selaimessa osoite `http://localhost` näyttää tekemäni hattu.example.com/index.html tiedoston sisällön

![kuva15](/pictures/h3/hattu15.png)

---

Nyt kun muokkaan alkuperäistä tiedostoa, niin myös linkattu tiedosto muuttuu ja oletussivu `localhost` päivittyy samalla

```

nano /home/janne/public-sites/hattu.example.com/index.html

```

![kuva16](/pictures/h3/hattu16.png)

![kuva17](/pictures/h3/hattu17.png)

---

# Validi HTML5 sivu

Seuraavaksi olisi tarkoitus tehdä oikea validi html5 sivu.

Avataan tekemämme index.html-tiedosto ja muokataan siitä oikea html5 sivu.

```

nano /home/janne/public-sites/hattu.example.com/index.html

```

![kuva12](/pictures/h3/hattu9.png)

---

![kuva13](/pictures/h3/hattu10.png)

---

![kuva14](/pictures/h3/hattu11.png)

---

# Curl ja curl -I

Esimerkki `curl` ja `curl -I` komennoista

![kuva18](/pictures/h3/curl1.png)

`curl -I` näyttää http-pyynnön otsaketietoja.  
Esimerkiksi ensimmäisellä rivillä pyynnön vastauskoodi 200 tarkoittaa ok eli pyydetty resurssi (localhost) löytyi.  
Toisella rivillä on pyynnön aikaleima eli milloin pyyntö on lähetetty serverille.  
Sitten on tieto palvelimesta (Apache) ja sen käyttöjärjestelmästä (Debia).  
Sen jälkeen on tieto milloin pyydettyä resurssia (localhost) on viimeksi muokattu.  
Viimeisellä rivillä kerrotaan resurssin sisällön tyyppi (Content-type), tässä tapauksessa se on tekstiä ja html koodia.

---

# Kaksi weppisaittia samalla koneella

Vapaaehtoisena tehtävänä oli saada tietokone vastaamaan usealle eri sivulle eri nimistä.  
Luennolla jo teimme harjoituksena site1.com sivun ja tämän lisäksi tein vielä myothersite.com nimisen sivun virtuaalikoneelle yllä olevaa ohjetta noudattamalla.  
Tosin näistä sivuista en tehnyt html5 sivuja, vaan lisäsin vain testin vuoksi jotain tekstiä näytettäväksi.

`/etc/hosts` tiedostossa löytyy tarvittavat nimiohjaukset.

![kuva19](/pictures/h3/hosts1.png)

---

`/etc/apache2/sites-available/` hakemistossa on kaikkien sivujen konfiguraatiotiedostot

![kuva20](/pictures/h3/hosts2.png)

---

Ja sekä terminaalin `curl` että selain näyttävät site1.com ja myothersite.com sivujen sisällön.

![kuva21](/pictures/h3/hosts3.png)

![kuva22](/pictures/h3/hosts4.png)

---

# Lähteet

- Tehtävänanto: https://terokarvinen.com/linux-palvelimet/
- Apache2 Web Server: https://github.com/johannaheinonen/johanna-test-repo/blob/main/linux-03092025.md
- Name Based Virtual Hosts on Apache: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/
- Name-based Virtual Host Support: https://httpd.apache.org/docs/2.4/vhosts/name-based.html
- Short HTML5 page: https://terokarvinen.com/2012/short-html5-page/
- HTML validator: https://validator.w3.org
