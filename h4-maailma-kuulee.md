# Tiivistelmä

- Virtuaalipalvelin on usein parempi vaihtoehto kuin oma fyysinen palvelin

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
