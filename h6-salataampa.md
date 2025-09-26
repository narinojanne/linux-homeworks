# Salataan

## Let's Encrypt

Tehtävänä oli hankkia ja asentaa palvelimelle ilmainen TLS-sertifikaatti Let's Encryptiltä sekä todentaa, että se toimii.  
Ihan ensiksi varmistin, että käytössä olevat verkkotunnukseni toimivat ja näkyvät selaimessa sekä `curl` komennolla.

Ajoin terminaalissa komennot `curl jannenarinen.com` ja `curl www.jannenarinen.com` sekä kokeilin selaimella molemmat osoitteet ja ne näyttivät toimivan.

![kuva1](/pictures/h6/lets1.png)

---

Kokeilin myös alidomainit `oikea.jannenarinen.com` ja `vasen.jannenarinen.com`. Ne toimivat myös.  
Alidomainien ohjaukset olin tehnyt ilman www-etuliitettä, joten `www.oikea.jannenarinen` osoitetta ei löydy.

---

Seuraavaksi otin SSH-yhteyden palvelinkoneeseen ja suoritin päivityskomennot `sudo apt-get update` ja `sudo apt-get dist-upgrade`.  
Sitten olikin aika alkaa asentamaan `certbot` joka automatisoi TLS-sertifikaatin luomisen ja asetukset.  
Päätin kokeilla samaa järjestystä kuin luennolla Tero esimerkissä näytti eli ensin reikä tulimuuriin `https` liikennettä varten.

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

Ja `curl https://jannenarinen.com` näyttää taas sivun sisällön oikein.

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

![kuva27](/pictures/h6/lets27.png)

![kuva28](/pictures/h6/lets28.png)

---

# Lähteet

- Tehtävänanto: https://terokarvinen.com/linux-palvelimet/#h6-salataampa
- Let's Encrypt: https://letsencrypt.org
- Let's Encrypt How It Works: https://letsencrypt.org/how-it-works/
- Apache Basic Configuration Example: https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample
- Let's Encrypt CAA: https://letsencrypt.org/fi/docs/caa/
- Community Let's Encrypt: https://community.letsencrypt.org/t/migration-from-ocsp-to-crl/239517
- Mozilla Strict Transport Security: https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Strict-Transport-Security
