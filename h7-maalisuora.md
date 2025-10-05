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

Kopioimalla scriptin sijaintiin `/usr/local/bin/` scripti saadaan ajettavaksi suoraan tiedostonimellä sekä kaikkien muiden käyttäjien ajettavaksi myös.

![kuva6](/pictures/h7/bash6x.png)

---

#Lähteet

- Tehtävänanto: https://terokarvinen.com/linux-palvelimet/#h7-maalisuora
- Tero Karvinen: Hello World Python3, Bash, C, C++, Go, Lua, Ruby, Java – Programming Languages on Ubuntu 18.04: https://terokarvinen.com/2018/hello-python3-bash-c-c-go-lua-ruby-java-programming-languages-on-ubuntu-18-04/
- Tero Karvinen: Shell Scripting: https://terokarvinen.com/2007/12/04/shell-scripting-4/
- Johanna Heinonen: Linux Shell Scripting Basics: https://github.com/johannaheinonen/johanna-test-repo/blob/main/linux-01102025.md
