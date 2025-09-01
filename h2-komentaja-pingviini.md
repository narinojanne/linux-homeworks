# Tiivistelmä

- Komentorivi on helppo tapa siirtyä hakemistosta toiseen
- Komentorivin avulla voi nopeasti ja helposti luoda uusia ja muokata olemassa olevia tiedostoja
- Komentorivillä voi kätevästi asentaa ja päivittää ohjelmia sekä käyttöjärjestelmän
- Komentorivillä voi käyttää ohjelmia

# Micro

![kuva01](/pictures/h2/micro%20asennus.png)

Micro-editorin voi asentaa ajamalla komennon: sudo apt-get install micro.
Olinkin jo asentanut tämän aikaisemmin.

---

![kuva02](/pictures/h2/micro-start.png)

Micro-editor käynnistyy komennolla: micro ja avaa tyhjän tiedoston.

---

![kuva03](/pictures/h2/micro-testi.png)

Micro-editor käytössä.

---

![kuva04](/pictures/h2/micro-testi2.png)

Ctrl + g avaa ohje valikon.

---

![kuva05](/pictures/h2/micro-testi3.png)

Ctrl + s tallentaa tiedoston. Alakulmassa annetaan tiedostolle nimi.

---

![kuva06](/pictures/h2/micro-testi4.png)

Tiedoston tallennus onnistui.
Tiedosto tallentui minulla siihen hakemistoon missä olin kun käynnistin micro-editorin. Ctrl + q lopettaa ohjelman.

---

![kuva07](/pictures/h2/micro-testi5.png)

Käytin komentoa: find -name testimicro.txt selvittääkseni tiedoston sijainnin.

---

![kuva07](/pictures/h2/micro-testi6.png)

Komento: micro testimicro.txt avaa tiedoston

---

![kuva08](/pictures/h2/micro-testi7.png)

Tiedosto avattuna uudelleen. Ctrl + q lopettaa ohjelman.

---

# APT

- Ohjelmia voi asentaa monta kerralla antamalla apt-get install komennolle asennettavien ohjelmien nimet.

![kuva09](/pictures/h2/install-multiple.png)

Asensin ohjelmat cowsay, cmatrix ja lolcat käyttämällä komentoa: sudo apt-get install cowsay cmatrix lolcat.

---

![kuva10](/pictures/h2/install-multiple2.png)

Piti asentaa myös monta riippuvuutta.

---

![kuva11](/pictures/h2/lolcat1.png)

Komento: lolcat -h avaa lolcatin help näkymän ja lisää siihen värejä.  
Komento: date | lolcat -a näyttää päivämäärän animoidusti vaihtuvilla väreillä, kesto pari sekuntia.

---

![kuva12](/pictures/h2/cowsay1.png)

Komento: cowsay Hello! piirtää terminaaliin lehmän kuvan joka sanoo Hello!

---

![kuva13](/pictures/h2/cmatrix1.png)

---

![kuva14](/pictures/h2/cmatrix2.png)

Komennolla: cmatrix terminaaliin ilmestyy Matrix elokuvan tyylinen näkymä jossa vihreitä merkkejä tippuu näytöllä alaspäin.

---

# FHS

# The Friendly M

# Pipe

# Rauta

- Komennolla: lshw -short -sanitize saa listattua käytössä olevan koneen raudan.

![kuva15](/pictures/h2/install-lshw1.png)

Minun piti asentaa lshw, se onnistui komennolla: sudo apt-get install lshw

---

![kuva16](/pictures/h2/lshw1.png)

Minulla näyttäisi olevan virtuaalikoneessani ainakin 4GB ram-muistia, Intelin i5 prosessori, 37GB tallennustilaa,  
CD-rom asema, hiiri ja näppäimistö.

---

# Lähteet:

- Tehtävänanto: https://terokarvinen.com/linux-palvelimet/
- Micro-editor kotisivu: https://micro-editor.github.io/
- Micro-editor asennusohjeita: https://github.com/zyedidia/micro#installation
- Cowsay: https://www.linux.fi/wiki/Cowsay ja https://itsfoss.com/cowsay/
- Cmatrix: https://itsfoss.com/using-cmatrix/
- Lolcat: https://www.tecmint.com/lolcat-color-output-linux-terminal/
