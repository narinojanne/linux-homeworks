# Verkkotunnukesn rekisteröiminen ja ohjaaminen

Tehtävänä oli tällä kertaa laittaa julkinen nimi osoittamaan omaan koneeseen, jonka tulkitsen tässä tarkoittavan aiemmin luotua virtuaalipalvelinta.  
Luennolla käytettiin esimerkkinä [namecheap](https://www.namecheap.com/) nimistä palvelua ja tuo näytti kyllä ihan pätevältä ja simppeliltä palvelulta käyttää.  
Itse päädyin kuitenkin ostamaan nimipalvelun [zoner](https://www.zoner.fi/) nimiseltä yritykseltä ja tämä siitä syystä, että olin jo aiemmin kesällä hommannut kyseiseltä yritykseltä jannenarinen.fi verkkotunnuksen ja ajattelin, että on yksinkertaisempaa ottaa samalta palveluntarjoajalta toinenkin tunnus.

## Verkkotunnuksen rekisteröiminen

Aloitin verkkotunnuksen rekisteröinnin navigoimalla osoitteeseen https://zoner.fi ja syöttämällä halutun verkkotunnuksen etusivulla olevaan `tarkista verkkotunnuksesi...` laatikkoon.

![kuva01](/pictures/h5/zonerdns1x.png)

---

Palvelu näyttää suositut verkkotunnukset listana ja tunnuksen perässä näkyy onko tunnus saatavilla.  
Tässä esimerkkinä käytän jannenarinen.net tunnusta, koska jannenarinen.com tunnuksen rekisteröinnistä en huomannut ottaa kuvakaappauksia, mutta homma etenee samalla tavalla kuitenkin.  
Lisätään osoite ostoskoriin ja jatketaan eteenpäin.

![kuva02](/pictures/h5/zonerdns2x.png)

![kuva03](/pictures/h5/zonerdns3x.png)

---

En valinnut lisäpalveluita vaikka niitäkin olisi saatavilla ja jatkoin eteenpäin.

![kuva04](/pictures/h5/zonerdns4x.png)

---

Seuraavaksi voisi syöttää alennuskoodin jos sellainen olisi, minulla ei ollut joten jatkoin eteenpäin.

![kuva05](/pictures/h5/zonerdns5x.png)

---

Sitten piti täyttää omat tiedot ja valita laskutustapa sekä hyväksyä palvelun käyttöehdot. Laskutustavaksi valitsin tässä sähköpostin, mutta sen voi muuttaa jälkikäteen korttimaksuksi jos haluaa.  
Omiin tietoihin pitää täyttää myös henkilötunnus kokonaisuudessaan, jolla varmistetaan henkilöllisyys.

![kuva06](/pictures/h5/zonerdns6x.png)

![kuva07](/pictures/h5/zonerdns7x.png)

![kuva08](/pictures/h5/zonerdns7x2.png)

---

Asiakastietojen täyttämisen jälkeen annetaan vielä mahdollisuus vaihtaa laskutustapaa, laskutustavan voi vaihtaa myös palvelun omissa tiedoissa myöhemmin ja omat laskut näkyvät palvelun laskutus osiossa.

![kuva09](/pictures/h5/zonerdns8x.png)

---

Tämän jälkeen pitää vielä suorittaa sähköpostiosoitteen vahvistaminen, jonka jälkeen palveluun voi kirjautua sähköpostiin saapuneilla tunnuksilla.

## Verkkotunnuksen ohjaaminen

Tehtävässä piti ohjata verkkotunnus omalle koneelle, joten aloitetaan kirjautumalla palveluun.

![kuva10](/pictures/h5/zonerdns9.png)

---

Valitaan omat tuotteet

![kuva11](/pictures/h5/zonerdns10x.png)

---

Valitaan verkkotunnuksen oikeassa reunassa kolme pistettä ja avautuvasta valikosta `Hallitse verkkotunnusta` ja päästään verkkotunnuksen yleiskatsaus sivulle.

![kuva12](/pictures/h5/zonerdns11x.png)

![kuva13](/pictures/h5/zonerdns12x.png)

![kuva14](/pictures/h5/zonerdns13.png)

---

Valitaan `DNS` ja päästään DNS-hallinta sivulle, jossa voidaan lisätä uusi DNS-tietue tai muokata ja poistaa olemassa olevia tietueita.

![kuva15](/pictures/h5/zonerdns14x.png)

![kuva16](/pictures/h5/zonerdns15x.png)

---

Olemassa olevaa tietutetta voi muokata valitsemalla tietueen lopussa olevista kolmesta pisteestä `Muokkaa` vaihtoehdon.  
Minä päätin lisätä uuden tietueen joten valitsin `Uusi tietue` painikkeen.

![kuva17](/pictures/h5/zonerdns16x.png)

---

Tässä valitsin tietueen tyypiksi A, nimenä oli valmiiksi 'Domain name' joka ohjaa verkkotunnuksen `jannenarinen.com`, TTL-arvoksi jätin 600, joka oli oletuksena ja lisäsin kohde IP-osoitteeseen virtuaalipalvelimeni IP-osoitteen. Ja sitten vain painetaan `Lisää` painiketta. Vahvistus kertoo, että DNS-tietueen tallennus onnistui ja se myös näkyy DNS-tietuuiden listassa.

![kuva18](/pictures/h5/zonerdns17x.png)

![kuva19](/pictures/h5/zonerdns18x.png)

---

Lisäsin myös DNS-tietueen www.jannenarinen.com ohjausta varten, mutta tällä kertaa 'Nimi' kohtaan tuli 'www' joka ohjaa verkkotunnuksen `www.jannenarinen.com` liikenteen.

![kuva20](/pictures/h5/zonerdns19x.png)

![kuva21](/pictures/h5/zonerdns20x.png)

---

Poistin DNS-tietueet, jotka eivät ohjaudu omalle virtuaalipalvelimelleni valitsemalla tietueen lopusta kolme pistettä ja vaihtoehdon `Poista` ja vahvistamalla tietueen poistamisen.

![kuva22](/pictures/h5/zonerdns21x.png)

![kuva23](/pictures/h5/zonerdns22.png)

---

Tietueiden poistamista ei olisi tarvinnut tehdä jos olisin vain muokannut tietueita.

---

Lopuksi DNS-tietueissa on jäljellä vain ne mitkä lisäsin

![kuva24](/pictures/h5/zonerdns23x.png)

---

Nyt verkkotunnusten `jannenarinen.com` ja `www.jannenarinen.com` pitäisi ohjautua omalle virtuaalipalvelimelleni, mutta vasta kun palvelimellani on tehty konfiguraatioita.

## Apachen konfiguroiminen virtuaalipalvelimella

Ensin otin SSH-yhteyden palvelimelle.  
Yhteyden jälkeen tein .conf tiedoston `jannenarinen.com` verkkotunnukselle, jonka perusteella apache ohjaa palvelimen verkkolikennettä.

```
sudoedit /etc/apache2/sites-available/jannenarinen.com.conf
```

Lisäsin tiedostoon seuraavat tiedot.

```

<VirtualHost *:80>

    ServerName jannenarinen.com
    ServerAlias www.jannenarinen.com
    DocumentRoot /home/janne/public-sites/jannenarinen.com/

    <Directory /home/janne/public-sites/jannenarinen.com/>
        Require all granted
    </Directory>

    Errorlog ${APACHE_LOG_DIR}/error-jannenarinen.log
    Customlog ${APACHE_LOG_DIR}/access-jannenarinen.log combined

</VirtualHost>

```

Loin hakemiston jannenarinen.com sivustoa varten.

```
mkdir /home/janne/public-sites/jannenarinen.com/
```

Lisäsin hakemistoon `index.html` tiedoston, josta tulee sivuston etusivu.

```
micro /home/janne/public-sites/jannenarinen.com/index.html
```

![kuva25](/pictures/h5/apache1.png)

---

Tässä vaiheessa on hyvä vielä tarkistaa, että apachella on pääsy hakemistoon `public-sites` ja sen tiedostoihin.  
Tässä [JohannaHeinonen](https://github.com/johannaheinonen/johanna-test-repo/blob/main/linux-03092025.md) ohje, jonka avulla saa hienosti tarkistettua oikeudet.

![kuva26](/pictures/h5/johannaheinonen-apache-permissions.png)

---

Oman kansioni oikeudet näyttivät olevan kunnossa.

![kuva27](/pictures/h5/apache2.png)

---

Muokataan vielä `/etc/hosts` tiedostoon verkkotunnusten ohjaus.

```
sudoedit /etc/hosts
```

![kuva28](/pictures/h5/apache3.png)

---

Sitten vielä sallitaan sivusto `sudo a2ensite jannenarinen.com.conf`.

Ja uudelleen käynnistetään Apache2 `sudo systemctl restart apache2`.

Nyt `curl jannenarinen.com` ja selaimen pitäisi näyttää tekemäni `/home/janne/public-sites/jannenarinen.com/index.html` tiedosto.

---

![kuva29](/pictures/h5/apache4.png)

Sehän toimii!

---
