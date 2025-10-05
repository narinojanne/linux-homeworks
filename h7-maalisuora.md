# Maalisuora

## Hei maailma kolmella kielellä

Tehtävänä oli kirjoittaa ja ajaa "Hei maailma" kolmella eri kielellä.

### Bash

Aloitin tekemällä ihan vain `Bash` kielellä ohjelman, joka tulostaa tekstin `"Hei maailma"`.

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

#Lähteet

- Tehtävänanto: https://terokarvinen.com/linux-palvelimet/#h7-maalisuora
- Tero Karvinen: Hello World Python3, Bash, C, C++, Go, Lua, Ruby, Java – Programming Languages on Ubuntu 18.04: https://terokarvinen.com/2018/hello-python3-bash-c-c-go-lua-ruby-java-programming-languages-on-ubuntu-18-04/
- Tero Karvinen: Shell Scripting: https://terokarvinen.com/2007/12/04/shell-scripting-4/
- Johanna Heinonen: Linux Shell Scripting Basics: https://github.com/johannaheinonen/johanna-test-repo/blob/main/linux-01102025.md
