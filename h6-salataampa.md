# Salataampa verkkoliikennettä

## Tiivistelmä

- VirtualHostiin polut sertifikaattitiedostoihin
- Let's Encrypt ja ACME-protokolla mahdollistaa https-palvelimen perustamisen ja automaattisen luotettavien varmenteiden hankkimisen
- Varmenteet ilman ihmisen toimia
- Varmenteiden pyytäminen, uusiminen ja peruuttaminen yksinkertaista valtuutetulla tilillä

## Let's Encrypt

Tehtävänä oli hankkia ja asentaa palvelimelle ilmainen TLS-sertifikaatti Let's Encryptiltä sekä todentaa, että se toimii.  
Ihan ensiksi varmistin, että käytössä olevat verkkotunnukseni toimivat ja näkyvät selaimessa sekä `curl` komennolla.  
Tässä käytin lähteenä omia muistiinpanojani Teron luennolta.

Ajoin terminaalissa komennot `curl jannenarinen.com` ja `curl www.jannenarinen.com` sekä kokeilin selaimella molemmat osoitteet ja ne näyttivät toimivan.

![kuva1](/pictures/h6/lets1.png)

---

Kokeilin myös alidomainit `oikea.jannenarinen.com` ja `vasen.jannenarinen.com`. Ne toimivat myös.  
Alidomainien ohjaukset olin tehnyt ilman www-etuliitettä, joten `www.oikea.jannenarinen` osoitetta ei löydy.

---

Seuraavaksi otin SSH-yhteyden palvelinkoneeseen ja suoritin päivityskomennot `sudo apt-get update` ja `sudo apt-get dist-upgrade`.  
Sitten olikin aika alkaa asentamaan `certbot` joka automatisoi TLS-sertifikaatin luomisen ja asetukset.  
Päätin kokeilla samaa järjestystä mitä Tero käytti luennolla esimerkissä, eli ensin reikä tulimuuriin `https` liikennettä varten.

```
sudo ufw allow 443/tcp
```

![kuva2](/pictures/h6/lets2.png)

---

Asennetaan `certbot` ja Apache-palvelimen `pyhton3-certbot-apache` paketti. Vastataan 'Y' eli yes jatketaanko kysymykseen.

```
sudo apt-get install certbot python3-certbot-apache
```

![kuva3](/pictures/h6/lets3.png)

---

Ja ajetaan certbotilla verkkotunnukset joille sertifikaatteja haetaan.

```
sudo certbot --apache --domains jannenarinen.com,www.jannenarinen.com,oikea.jannenarinen.com,vasen.jannenarinen.com
```

![kuva4](/pictures/h6/lets4.png)

---

Sähköpostin voi skipata enterillä.

![kuva5](/pictures/h6/lets5.png)

---

Hyväksytään palvelun ehdot 'Y'. Ja sitten odotellaan hetkinen.

![kuva6](/pictures/h6/lets6.png)

---

Ja jos kaikki meni niin kuin pitikin, niin pitäisi tulla onnitteluja uusista sertifikaateista haetuille verkkotunnuksille.

![kuva7](/pictures/h6/lets7x.png)

---

Nyt myös selaimessa pitäisi näkyä lukon kuva osoiterivillä ja kun kursorin vie lukon päälle niin pitäisi myös näkyä teksti `Verified by: Let's Encrypt`.  
Firefoxilla näyttää tältä

![kuva8](/pictures/h6/lets8.png)

![kuva9](/pictures/h6/lets9.png)

![kuva10](/pictures/h6/lets10.png)

![kuva13](/pictures/h6/lets13.png)

![kuva14](/pictures/h6/lets14.png)

---

Ja Chromella näyttää tältä

![kuva15](/pictures/h6/lets15.png)

![kuva11](/pictures/h6/lets11.png)

![kuva12](/pictures/h6/lets12.png)

---

Terminaalissa `curl jannenarinen.com` kertoo, että sivu on siirretty `https://jannenarinen.com/` osoitteeseen, joka on juuri salaamamme osoite.

![kuva16](/pictures/h6/lets16.png)

---

Ja `curl https://www.jannenarinen.com` näyttää taas sivun sisällön oikein.

![kuva17](/pictures/h6/lets17.png)

---

Certbot teki automaattisesti lisäyksiä sivujen conf tiedostoihin sekä lisäsi erilliset `-le-ssl.conf` päätteiset asetustiedostot `https` liikennettä varten.

```
cd /etc/apache2/sites-available
```

`ls`

`cat jannenarinen.com.conf`

Kuvasta nähdään, että `certbot` on lisännyt minun tekemään conf tiedostoon 'RewriteEngine' ja muita 'Rewrite' ohjauksia.

![kuva18](/pictures/h6/lets18.png)

---

`cat jannenarinen.com-le-ssl.conf`

Tässä `certbot`in automaattisesti tekemä erillinen `-le-ssl.conf` tiedosto.

![kuva19](/pictures/h6/lets19.png)

---

## A-rating

TLS piti myös testata jollain laadunvarmistustyökalulla, esimerkiksi [SSLLabs](https://www.ssllabs.com/ssltest/) testerillä.

![kuva24](/pictures/h6/lets24.png)

---

Tulokseksi tuli A.

![kuva20](/pictures/h6/lets20.png)

---

Tuli myös muutama virhe samalla, mutta äkkiseltään eivät vaikuta olevan kovin kriittisiä.

Tässä ainakin `DNS CAA` liittyy siihen, etten ole lisännyt `CAA` tietuetta DNS-tietoihin omalla palveluntarjoajallani. CAA-tietueella kerrotaan kuka tai mikä taho voi myöntää sertifikaatin. Lisätietoa CAA-tietueesta: [LetsEncrypt](https://letsencrypt.org/fi/docs/caa/).

Tuo toinen virhe `Revocation status Validation error CRL ERROR:IOException occurred` vaikuttaisi liittyvän `Let's Encryptin` ja `SSLLabs` väliseen liikenteeseen ainakin tämän [Community.letsencrypt](https://community.letsencrypt.org/t/migration-from-ocsp-to-crl/239517) keskusteluketjun perusteella.

![kuva21](/pictures/h6/lets21.png)

---

Tämä `Strict Transport Security (HSTS)` virhe liittyy Apache-palvelimen `https` ohjauksiin, jotka pitäisi erikseen vielä määritellä ja ottaa käyttöön Let's Encryptin lisäksi.  
[Mozillan](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Strict-Transport-Security) mukaan `HSTS` pakottaa selaimen aina käyttämään `https` yhteyttä.

![kuva30](/pictures/h6/lets30.png)

---

Nämä muut virheet viittaavaat suoraan selaimiin, jotka vaikuttavat olevan jo sen verran vanhoja versioita, että ne eivät osaa käsitellä sertifikaatteja.

![kuva22](/pictures/h6/lets22.png)

![kuva23](/pictures/h6/lets23.png)

---

Kirjauduin `Zoner` palveluun ja kävin lisäämässä DNS-hallinnassa CAA-tietueen verkkotunnuksen DNS-tietueihin.

![kuva25](/pictures/h6/lets25.png)

![kuva26](/pictures/h6/lets26.png)

---

Sitten kokeilin [SSLLabs](https://www.ssllabs.com/ssltest/) testin uudestaan ja tuloksena oli taas A, mutta nyt lisäyksenä oli tullut maininta CAA:sta.  
Jotta sain uudet testitulokset, piti painaa `Clear cache` yhteenvedon yläpuolella.

![kuva31](/pictures/h6/lets31.png)

![kuva27](/pictures/h6/lets27.png)

![kuva28](/pictures/h6/lets28.png)

---

Googletin tuota `HSTS` asetuksen käyttöönottoa ja löysin [geekforgeeks](https://www.geeksforgeeks.org/web-tech/how-to-enable-http-strict-transport-security-hsts-for-apache/) sivuilta jonkilaisen ohjeen jota ajattelin yrittää soveltaa, koska tuossa ohjeessa konfiguroidaan myös `nginx`:n asetuksia, mutta kokeilin vain tuota `Apache2` asetusten tekemistä.

Avataan `-le-ssl.conf` tiedosto.

```
sudoedit /etc/apache2/sites-available/jannenarinen.com-le-ssl.conf
```

Lisätään rivi

```
Header always set Strict-Transport-Security max-age=604800; includeSubDomains
```

Tässä asetetaan `Strict-Transport-Security` otsikko, joka ilmoittaa selaimelle, että kaikki yhteydet pitää olla `https` yhteyksiä.

`max-age=604800` on aika sekunteina, jonka aikana selain ottaa `https`:n käyttöön sivustolla, tässä ajaksi on määritelty viikko.

`includeSubDomains` määrittää, että `HSTS` koskee myös alidomaineja.

![kuva32](/pictures/h6/lets32.png)

---

Tähän `HSTS`:ään olisi vielä argumentti `preloads` olemassa, mutta siitä kerrotaan, että sitä ei kannata käyttää oletuksena joten jätin sen tässä vielä pois. Lisää infoa aiheesta: [HSTS Preload](https://hstspreload.org/).

---

Sitten vaan Apachen uudelleenkäynnistys.  
Apache ei halunnut uudelleenkäynnistyä vaan ilmoitti, että joku virhe on jossain.  
Yritin selvittää virhettä komennolla

```
systemctl status apache2.service
```

Tästä sain selville vain, että virhe olisi jossain tiedostossa `/etc/apache2/sites-enabled/` hakemistossa ja siellä virheellinen komento `Header`.

![kuva33](/pictures/h6/lets33.png)

---

Ajoin myös komennon

```
journactl -xeu apache2.service
```

Tästä selvisi sama asia, mutta kun levensi ruutua niin sain enemmän tekstiä esiin ja `jannenarinen.com-le-ssl.conf` tiedostossa on jokin kirjoitusvirhe tai jokin moduuli puuttuu palvelimen asetuksista.  
Tiedostossa oli myös virhe rivillä 15, mutta tämän huomasin vielä myöhemmin.

![kuva34](/pictures/h6/lets34.png)

---

Ensin aloitin moduuli virheen ratkaisun etsimisen ja se löytyi heti aiemmin tekemästäni hausta `how to enable hsts apache2` tekoälyn luomasta osiosta, jossa mainittiin `headers modulen` sallimisesta.

![kuva35](/pictures/h6/lets35.png)

---

Ja koska tekoälyyn ei voi sokeasti luottaa, niin tarkistin onko Apachen moduuleissa olemassa tuota moduulia.  
`cd /etc/apache2/mods-available/` ja `ls` niin sieltä kyllä löytyi tuo `headers module`.

![kuva36](/pictures/h6/lets36.png)

---

Sallitaan moduuli ja sen jälkeen se löytyy hakemistosta `/etc/apache2/mods-enabled/`.

```
sudo a2enmod headers
```

![kuva37](/pictures/h6/lets37.png)

![kuva38](/pictures/h6/lets38.png)

---

Uudelleenkäynnistys ei vieläkään onnistunut, joten taas ajoin `journactl -xeu apache2.service` komennon ja nyt huomasin, että conf tiedoston rivillä 15 on jokin virhe joka viittaa liian moneen argumenttiin jossain komennossa.

![kuva39](/pictures/h6/lets39.png)

---

Avasin conf tiedoston ja hetken pähkäilyn jälkeen ajattelin, että `Strict-Transport-Security` tarvitsee "" merkit, koska siinä on enemmän kuin yksi argumentti, toisin kuin `geeksforgeeks` ohjeessa jota käytin apuna.

![kuva40](/pictures/h6/lets40.png)

---

Tämä korjasi ongelman ja Apache uudelleenkäynnistyi.

![kuva41](/pictures/h6/lets41.png)

---

Kävin taas tekemässä SSLLabs testin ja tulos oli edelleen A, mutta nyt `Strict Transport Security (HSTS)` virhe oli muuttunut ilmoitukseksi, että aika on liian lyhyt. Eli ainakin jotain sain tässä säädettyä ja korjattua.

![kuva42](/pictures/h6/lets42.png)

![kuva43](/pictures/h6/lets43.png)

---

# Lähteet

- Tehtävänanto: https://terokarvinen.com/linux-palvelimet/#h6-salataampa. Katsottu: 26.9.2025.
- Let's Encrypt: https://letsencrypt.org. Katsottu: 26.9.2025.
- Let's Encrypt How It Works: https://letsencrypt.org/how-it-works/. Katsottu: 26.9.2025.
- Apache Basic Configuration Example: https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample. Katsottu: 26.9.2025.
- Let's Encrypt CAA: https://letsencrypt.org/fi/docs/caa/. Katsottu: 26.9.2025.
- Community Let's Encrypt: https://community.letsencrypt.org/t/migration-from-ocsp-to-crl/239517. Katsottu: 26.9.2025.
- Mozilla Strict Transport Security: https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Strict-Transport-Security. Katsottu: 26.9.2025.
- SSL Labs SSL Server Test: https://www.ssllabs.com/ssltest/. Katsottu 26.9.2025.
- HSTS Preload: https://hstspreload.org/. Katsottu 26.9.2025.
