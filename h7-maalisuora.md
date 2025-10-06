# Maalisuora

## Hei maailma kolmella kielellä

Tehtävänä oli kirjoittaa ja ajaa "Hei maailma" kolmella eri kielellä.

### Bash

Aloitin tekemällä ihan vain `Bash` ohjelman, joka tulostaa tekstin `"Hei maailma"`.  
`Bash` scriptejä ajettaessa ei tarvitse erikseen komentaa `bash` koska Linux tunnistaa automaattisesti nämä komennot.

Ensin tein tiedoston scriptille.

```
micro heimaailma.sh
```

![kuva1](/pictures/h7/bash1.png)

---

Lisäsin rivit.

```
#!/bin/bash
echo "Hei maailma"
```

![kuva2](/pictures/h7/bash2.png)

---

Ensimmäisellä rivillä kerrotaan minkä kielen scripti ajetaan, tässä tapauksessa `bash`. Tätä riviä kutsutaan myös nimellä `shebang`.

---

Tein scriptin ajettavaksi antamalla suoritusoikeudet tiedostoon.

```
chmod a+x heimaailma.sh
```

![kuva3](/pictures/h7/bash3.png)

---

Ja sitten vain ajoin scriptin komennolla.

```
./heimaailma.sh
```

![kuva4](/pictures/h7/bash4.png)

---

Koska scriptin sijainti ei `$PATH`in tiedossa, pitää kertoa, että ajettava scripti löytyy tästä hakemistosta lisäämällä `./` komentoon.

---

Ja teksti tulostui terminaaliin

![kuva5](/pictures/h7/bash5.png)

---

Kopioimalla scriptin sijaintiin `/usr/local/bin/` scripti saadaan ajettavaksi suoraan tiedostonimellä.  
Scriptin pitäisi nyt tulla myös kaikkien muiden käyttäjien ajettavaksi.

![kuva6](/pictures/h7/bash6x.png)

---

### Python

Seuraavaksi päätin tehdä `Python` kielellä saman ohjelman.  
`Python` scriptejä ajetaan komentamalla `python3` ja sitten script tiedoston nimi.

Aloitin tarkistamalla onko `python` asennettuna ja olihan se.

```
python3 --version
```

![kuva7](/pictures/h7/python1.png)

---

Taas tehdään tiedosto scriptille.

```
micro heimaailma.py
```

![kuva8](/pictures/h7/python2.png)

---

Lisätään `shebang` ja komento joka tulostaa taas tekstin `Hei maailma` terminaaliin.

```
#!/bin/python
print ("Hei maailma")
```

![kuva9](/pictures/h7/python3.png)

---

Nyt `shebang` rivillä kerrotaan, että halutaan ajaa python kielen scripti.  
En tällä kertaa yksilöinyt erikseen `python3` joka olisi varmaankin varmempi scriptin ajamisessa, mutta tämä toimi myös näin.

---

Tehdään scripti ajettavaksi antamalla suoritusoikeudet tiedostoon.

```
chmod a+x heimaailma.py
```

![kuva10](/pictures/h7/python4.png)

---

Ja ajetaan scripti. Nyt komennetaan `python3` koska kyseessä on `python` kielen scripti.

```
python3 heimaailma.py
```

![kuva11](/pictures/h7/python5.png)

---

Teksti tulostui odotetusti.

![kuva12](/pictures/h7/python6.png)

---

### C++

Kolmanneksi valitsin `C++` kielen scriptin. `C++` kielen scriptit pitää ensin kääntää, että niitä voi ajaa.

Ensin tarkistin onko `C++` kääntäjä asennettuna. Se oli myös jo asennettuna.

```
g++ --version
```

![kuva13](/pictures/h7/c++1.png)

---

Tein tiedoston scriptille.

```
micro heimaailma.cpp
```

![kuva14](/pictures/h7/c++2.png)

---

Lisäsin scriptin. Nyt ei tarvita `shebang` riviä, koska scripti käännetään ennen ajamista.

```
#include <iostream>
int main() {
    std::cout << "Hei C++ maailma" << std::endl;
    return 0;
}
```

![kuva15](/pictures/h7/c++3.png)

---

Scriptin kääntäminen.

```
g++ heimaailma.cpp -o heimaailmac
```

![kuva16](/pictures/h7/c++4.png)

---

Ja scriptin ajaminen vaatii taas `./` jolla kerrotaan, että scripti sijaitsee tässä hakemistossa, jossa ollaan tällä hetkellä.

```
./heimaailmac
```

![kuva17](/pictures/h7/c++5.png)

---

Ja teksti tulostui niin kuin pitikin. C++ scriptille ei tarvinnut lisätä erikseen suoritus oikeuksia.

![kuva18](/pictures/h7/c++6.png)

---

## Uusi komento Linuxiin kaikkien ajettavaksi

Tässä tehtävässä piti tehdä itse uusi komento ja laittaa se Linuxiin niin, että kaikki käyttäjät voivat ajaa sitä.

### Uusi komento

Ensin tein uuden käyttäjän ilman `sudo` oikeuksia.

```
sudo adduser narin
```

![kuva19](/pictures/h7/uusi1.png)

![kuva33](/pictures/h7/uusi19.png)

---

Tein `bash` komennon joka tervehtii nykyistä käyttäjää ja kertoo sen hetkisen päivämäärän ja kellonajan.

```
micro tervehdi.sh
```

![kuva20](/pictures/h7/uusi2.png)

![kuva21](/pictures/h7/uusi3.png)

---

Kokeilin, että komento toimii niin kuin pitääkin.

```
./tervehdi.sh
```

![kuva22](/pictures/h7/uusi5.png)

---

Seuraavaksi vaihdoin käyttäjää ja kokeilin voinko ajaa uuden tervehdys komennon toisella käyttäjällä.

```
su narin
```

![kuva23](/pictures/h7/uusi6.png)

![kuva24](/pictures/h7/uusi7.png)

![kuva25](/pictures/h7/uusi9.png)

![kuva26](/pictures/h7/uusi10.png)

---

Ei onnistunut vielä.

---

Vaihdoin käyttäjää uudelleen ja kokeilin, että komento vielä löytyy ja toimii.

```
su janne
```

![kuva27](/pictures/h7/uusi11.png)

![kuva28](/pictures/h7/uusi12.png)

![kuva29](/pictures/h7/uusi13.png)

---

Komento oli vielä olemassa ja toimi myös.

---

Kopioin uuden komennon `/usr/local/bin/` hakemistoon, josta se on kaikkien käyttäjien ajettavissa. Tämä pitää tehdä `sudo` oikeuksilla.  
Kokeilin myös, että komennon voi ajaa ilman `./` alkua.

```
sudo cp tervehdi.sh /usr/local/bin/
```

```
tervehdi.sh
```

![kuva30](/pictures/h7/uusi15.png)

---

Taas käyttäjän vaihto ja uusi kokeilu komennon ajamiseksi.

![kuva31](/pictures/h7/uusi17.png)

---

Se toimii!

---

Kokeilin myös ajaa aiemmin tekemäni `heimaailma.sh` jonka kopioin kaikkien ajettavaksi aikaisemmin.

![kuva32](/pictures/h7/uusi18.png)

---

Sekin toimii!

---

#Lähteet

- Tehtävänanto: https://terokarvinen.com/linux-palvelimet/#h7-maalisuora
- Tero Karvinen: Hello World Python3, Bash, C, C++, Go, Lua, Ruby, Java – Programming Languages on Ubuntu 18.04: https://terokarvinen.com/2018/hello-python3-bash-c-c-go-lua-ruby-java-programming-languages-on-ubuntu-18-04/
- Tero Karvinen: Shell Scripting: https://terokarvinen.com/2007/12/04/shell-scripting-4/
- Johanna Heinonen: Linux Shell Scripting Basics: https://github.com/johannaheinonen/johanna-test-repo/blob/main/linux-01102025.md
