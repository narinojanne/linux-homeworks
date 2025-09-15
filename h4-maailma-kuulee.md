# Tiivistelmä

- Rekisteröidy käyttäjäksi virtuaalipalvelimen palveluntarjoajan sivuilla
- Vuokraa itsellesi sopiva virtuaalipalvelin
- Suojaa palvelin tulimuurilla
- Päivitä palvelimen ohjelmat uusimpiin
- Lukitse root-tunnus
- Asenna weppipalvelin, jotta voit julkaista omia nettisivuja
- Käytä aina hyviä ja vahvoja salasanoja, aina

---

# Oma virtuaalipalvelin

Tehtävänä oli siis vuokrata oma virtuaalipalvelin joltain palveluntarjoajalta.  
Luennolla mainittiin ainakin [upcloud](https://www.upcloud.com) ja [digitalocean](https://wwww.digitalocean.com).  
Näistä kahdesta valitsin upcloudin tähän tehtävään.  
Ensin oli tietenkin rekisteröidyttävä käyttäjäksi.

![kuva01](/pictures/h4/upcloud1.png)

---

Rekisteröitymisen olin tehnyt jo luennon aikana ja se oli aika simppeli projekti. Tarvitsi vain täyttää pyydetyt tiedot ja vahvistaa sähköpostiosoite sähköpostiin tulleen linkin kautta, tämän jälkeen pääsi kirjautumaan käyttäjätilille juuri luoduilla käyttäjänimellä ja salasanalla. Palveluun piti myös antaa maksukortin tiedot, jotta palveluntarjoaja voi todentaa, että kyseessä on oikea henkilö eikä tiliä veloiteta tässä vaiheessa.
Sitten ei muuta kuin virtuaalipalvelimen luomiseen.

Etsivulla löytyy `Deploy now` nappi josta prosessi lähtee liikkeelle. Painetaan nappia ja valitaan `Server`.

![kuva02](/pictures/h4/vps1.png)

![kuva03](/pictures/h4/vps2.png)

---

Seuraavaksi valitaan sijainti. Valitsin tähän Suomen.

![kuva04](/pictures/h4/vps3.png)

---

Sitten päästään valitsemaan palvelimen resursseja. Valitsin Developer välilehdeltä halvimman vaihtoehdon, koska sen pitäisi riittää ainakin tässä alkuvaiheessa ja tarvittaessa on helpompaa lisätä resursseja kuin vähentää.  
Tässä vaihtoehdossa saadaan käyttöön yksi ydin, 1GB verran ram-muistia, 10GB verran tallennustilaa ja hintaa on vain 3 euroa kuukaudessa.

![kuva05](/pictures/h4/vps4.png)

![kuva06](/pictures/h4/vps5.png)

---

Tallennustilan ja automaattiset varmuuskopiot jätin niin kuin ne oletuksena olivat.

![kuva07](/pictures/h4/vps6.png)

---

Käyttöjärjestelmäksi valitsin Debian 13, koska sitä olemme käyttäneet kurssilla ja se oli myös opettajan vahva suositus.

![kuva08](/pictures/h4/vps7.png)

---

Verkko ja lisävalinnat kohdat jätin myös niin kuin ne oletuksena olivat.

![kuva09](/pictures/h4/vps8.png)

---

Kirjautumistavaksi valitsin SSH-kirjautumisen. `Add new` painikkeella pääsee tallentamaan uuden SSH avaimen palvelimelle.  
Avatuvaan ikkunaan liitetään omalla koneella luotu SSH-avainparin julkinen avain.

![kuva10](/pictures/h4/vps9.png)

![kuva11](/pictures/h4/vps10.png)

---

SSH-avainparin luominen onnistuu omalla koneella käyttämällä komentoa

```
ssh-keygen
```

Minulla oli jo SSH-avaimet olemassa, mutta päällekirjoitin uudet.

![kuva12](/pictures/h4/vps11_ssh.png)

---

SSH-avaimet löytyvät .ssh -hakemistosta.  
`cd .ssh/`  
`ls`  
.pub päätteinen tiedosto on julkinen avain, joka on tarkoitus syöttää `Add an SSH key` ikkunan `SSH key` laatikkoon.  
Lopuksi painetaan `Save the SSH key` painiketta.

![kuva13](/pictures/h4/vps10.png)

---

Initializing script kohdan jätin taas koskematta, mutta Server configuration kohdassa uudelleen nimesin palvelimen.  
Sitten vain `Deploy` painikkeella eteenpäin ja palvelin luodaan ja käynnistetään.

![kuva14](/pictures/h4/vps12.png)

---

Palvelin löytyy `Servers` valikon alta.

![kuva15](/pictures/h4/vps13.png)

---

# Alkutoimet

Alkutoimina palvelimelle piti laittaa tulimuuri päälle, sulkea root-tunnus ja päivittää ohjelmat.

## Tulimuuri

Ensin yhdistin SSH:lla palvelimelle käyttämällä komentoa `ssh root@ip-osoite`. Palvelimen ip-osoite löytyy palvelimen tiedoista `Servers` valikosta.

```
ssh root@185.26.51.240
```

![kuva16](/pictures/h4/vps14.png)

---

Kun olin saanut yhteyden luotua palvelimelle niin päivitin palvelimen ohjelmat.

```
sudo apt-get update
```

```
sudo apt-get dist-upgrade
```

Ja asensin ufw:n palvelimelle

```
sudo apt-get install ufw
```

Ennen kuin laitoin ufw:n päälle niin avasin ssh yhteyden tulimuuriin, koska olen ssh yhteydellä kiinni palvelimessa niin ilman tätä reikää sulkisin oman yhteyteni palvelimeen.

```
sudo ufw allow 22/tcp
```

![kuva17](/pictures/h4/vps15.png)

---

Tulimuuri päälle komennolla

```
sudo ufw enable
```

Pitäisi tulla näkyviin teksti `Firewall is active and enabled on system startup`, joka kertoo, että ufw on käynnissä ja käynnistyy myös järjestelmän käynnistyessä.

---

## Root-tunnus kiinni

Aloitin root-tunnuksen sulkemisen luomalla uuden sudo käyttäjän palvelimelle ja lisäämällä salasanan käyttäjälle.

```
sudo adduser janne
```

![kuva18](/pictures/h4/vps16.png)

![kuva19](/pictures/h4/vps17.png)

---

Kokeilin toimiiko yhteys uudella käyttäjällä.

```
ssh janne@185.26.51.240
```

![kuva20](/pictures/h4/vps18.png)

Yhteys ei toiminut vielä, joten kopioin root:n ssh-asetukset uudelle käyttäjälle ja vaihdoin asetus-hakemiston omistajan käyttäjäksi.

```
sudo cp -rvn /root/.ssh/ /home/janne/
```

```
sudo chown -R janne:janne /home/janne/
```

![kuva21](/pictures/h4/vps19.png)

---

Uusi yritys ssh-yhteydelle käyttäjänä.

```
ssh janne@185.26.51.240
```

![kuva22](/pictures/h4/vps20.png)

Nyt yhteys toimii käyttäjänä.

---

Seuraavaksi lukitsin root-käyttäjän ja estin root-käyttäjän kirjautumisen palvelimelle.  
Ensin tunnus lukkoon.

```
sudo usermod --lock root
```

Estetään root kirjautuminen muokkaamalla asetustiedostoa.

```
sudoedit /etc/ssh/sshd_config
```

Etsitään rivi `PermitRootLogin yes` ja vaihdetaan `yes` tilalle `no`.

![kuva23](/pictures/h4/vps21.png)

![kuva24](/pictures/h4/vps22.png)

Tallennus ja poistuminen editorista `ctrl + x`, `Y` ja `enter`.

Uudelleen käynnistetään ssh, jotta uudet asetukset tulevat voimaan.

```
sudo service ssh restart
```

---

Kirjautuminen root-käyttäjänä ei nyt pitäisi onnistua enää, joten kokeillaan sitä.

```
ssh root@185.26.51.240
```

![kuva25](/pictures/h4/vps23.png)

Nyt root kirjautuminen ei onnistu enää.

---

## Weppipalvelin

Seuraavaksi piti asentaa virtuaalipalvelimelle weppipalvelin, korvata testisivu ja saada se näkymään julkisesti.

Asensin weppipalvelimeksi Apache2:n komennolla

```
sudo apt-get install apache2
```

Korvasin apachen oletuksena olevan testisivun sisällön tervehdykseen `Greetings from Janne` käyttämällä komentoa

```
echo Greetings from Janne | sudo tee /var/www/html/index.html
```

![kuva26](/pictures/h4/vps24_apache.png)

---

Testasin virtuaalipalvelimen `localhost` sisällön

```
curl localhost
```

![kuva27](/pictures/h4/vps25_apache2.png)

Tervehdys näkyy joten testisivun sisältö on korvattu.

---

Sitten navigoin selaimella virtuaalipalvelimeni ip-osoitteeseen `185.26.51.240`, mutta selain jäi vain lataamaan sivua eli weppipalvelimeni ei vastannut.

![kuva28](/pictures/h4/vps26_apache3.png)

---

Avasin ufw:stä portin http:tä varten komennolla

```
sudo ufw allow 80/tcp
```

![kuva29](/pictures/h4/vps27_apache4.png)

---

Sitten vain uudelleen kokeilemaan selaimella osoitetta `185.26.51.240` ja nyt tervehdys näkyy myös selaimella. Myös palvelimen `localhost` toimii edelleen.

![kuva30](/pictures/h4/vps28_apache5.png)

---

# Lähteet

- Tehtävänanto: https://terokarvinen.com/linux-palvelimet/#h4-maailma-kuulee. Luettu: 15.9.2025.
- First Steps on a New Virtual Private Server: https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/. Luettu: 15.9.2025.
- Susanna Lehto - Teoriasta käytäntöön pilvipalvelimen avulla: https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/. Luettu: 15.9.2025.
