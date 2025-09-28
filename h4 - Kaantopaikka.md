### h4 Kääntöpaikka [Tehtävänanto](https://terokarvinen.com/sovellusten-hakkerointi/#h4-kaantopaikka-tero)

## Tehtävissä käytetty ympäristö

### PC

- Käyttöjärjestelmä: Windows 11 Home 64-bit
- Suoritin: AMD Ryzen 7 5800X 8-Core Processor
- Muisti: 32GB RAM
- Näytönohjain: NVIDIA GeForce RTX 3080
- Virtualisointiohjelmisto: Oracle VM VirtualBox 7.0

- Internet yhteys: DNA Netti 600 M

### Virtuaalikone

- Kali Linux [Get Kali](https://www.kali.org/get-kali/#kali-installer-images)
- Base Memory: 8192 MB
- Processors: 1
- Virtual HD: 60 GB
- Video Memory: 128 MB


## x) Tiivistelmä

Videolla käytetyt ohjelmat ja komennot:

- file - check file type
- ltrace - A library call tracer
- strace - trace system calls and signals
- objdump - disassembler, view an executable in assembly form
- gef - Enhanced features for GDB (GNU DeBugger)
- ghidra - Graphical Reverse Engineering tool by NSA (National Security Agency)

## a) Ghidra asennus

Ghidran asennus komennolla:

    sudo apt-get install ghidra

Asennuksen yhteydessä tuli ilmoitus:

`After this operation, 911 MB of additional disk space will be used. Do you want to continue? [Y/n]` , jatkoin valitsemalla Y

Asennuksen päätteeksi tarkistin vielä ohjelman version:

    apt show ghidra

<img width="684" height="542" alt="image" src="https://github.com/user-attachments/assets/f2f18fdb-8720-439a-9467-c21e6417f550" />

## b) rever-C

Uusi virtuaalikone käytössä, joten alkuun latasin Teron sivuilta [ezbin-challenges.zip](https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip) kansion:

    wget https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip

<img width="1073" height="214" alt="image" src="https://github.com/user-attachments/assets/4edc029e-480f-4b5e-8222-399b5d8160fd" />

Paketin purku:

    unzip ezbin-challenges.zip

<img width="895" height="450" alt="image" src="https://github.com/user-attachments/assets/3bdfb6dd-e491-4049-9113-98c0c216d7b3" />

Siirryin polkuun ~/challenges/packd ja purin packd -tiedoston

    upx -d packd

Tämän jälkeen Ghidran käynnistys komennolla `ghidra`

<img width="1127" height="657" alt="image" src="https://github.com/user-attachments/assets/4f7182d0-03ac-40c8-af12-c30eb6725999" />

Uuden projektin luonti

- File
- New Project
- Non-Shared Project
- Project Name: packd

Seuraavaksi File > Import file ja hain /home/ap/challenges/packd polusta packd -tiedoston ja 'Select File To Import'

<img width="503" height="278" alt="image" src="https://github.com/user-attachments/assets/141eeb92-309a-4831-98bf-970f80bdc1e8" />

Import results summary ikkunasta jatkoin 'OK'

Tämän jälkeen tupla klikkaamalla projekti auki.

Avautuu ikkuna: `packd has not been analyzed. Would you like to analyze it now?`, josta jatkoin 'Yes'

Jatkoin oletuksilla ja 'Analyze'

Analysoinnin jälkeen C-käännös, oikea salasana löytyy riviltä 10:

<img width="683" height="354" alt="image" src="https://github.com/user-attachments/assets/097a9f0a-6dd6-49db-9785-f141315a8d7f" />

Uudelleen nimetty muuttujat 'tulos' ja 'salasana'

<img width="695" height="334" alt="image" src="https://github.com/user-attachments/assets/b8ba62ca-2840-4d3e-a563-b1073acfde24" />


Ohjelman toiminta:

```
undefined8 main(void)

{
  int tulos; // muuttuja, tallennetaan strcmp funktion arvo
  char salasana [32]; // merkkitaulukko, johon käyttäjän syöte tallennetaan
  
  puts("What\'s the password?"); // What's the password? tulostus
  __isoc99_scanf(&DAT_0010201d,salasana); // lukee käyttäjän syötteen ja tallentaa sen taulukkoon salasana
  tulos = strcmp(salasana,"piilos-AnAnAs"); // verrataan käyttäjän syötettä ja piilos-AnAnAs -salasanaa ja tallennetaan arvo muuttujaan tulos (0, positiivinen tai negatiivinen)
  if (tulos == 0) {
    puts("Yes! That\'s the password. FLAG{Tero-0e3bed0a89d8851da933c64fefad4ff2}"); // tulos arvolla 0 mennään tähän
  }
  else {
    puts("Sorry, no bonus."); // muilla arvoilla else-lohkoon
  }
  return 0;
}
```

## c) Jos väärinpäin

Siirryin polkuun ~/challenges/passtr ja avasin ghidran `ghidra`

Aiempia vaiheita toistaen uusi projekti 'passtr' ja analysoinnin tulos:

<img width="784" height="334" alt="image" src="https://github.com/user-attachments/assets/1a2ff867-6be8-4e41-9cc3-0090c7d7c422" />

Ohjelman toiminta näyttäisi olevan sama kuin b) kohdan tehtävässä.

Hyödyllisiä vinkkejä tehtävään katsoin [täältä](https://www.youtube.com/watch?v=8U6JOQnOOkg)

Assembly koodissa if-ehto vastaa vihreällä maalattua riviä:

<img width="1385" height="518" alt="image" src="https://github.com/user-attachments/assets/9a5e2e1b-b6ab-478b-a4c4-1c44d629d6eb" />

JNZ näyttäisi tarkoittavan 'Jump if Not Zero' [lähde](https://www.infosecinstitute.com/resources/secure-coding/how-to-control-the-flow-of-a-program-in-x86-assembly/#:~:text=JNZ%20(Jump%20if%20Not%20Zero,the%20sign%20flag%20being%20set))

Eli mikäli if (iVar1 == 0) ei toteudu, hypätään nuolen osoittamaan kohtaan.

Vastaavasti JZ = Jump if Zero

Joten tuolla kyseisellä JNZ -rivillä right-click ja 'Patch Instruction' päästää muokkaamaan. Vaihdetaan JNZ -> JZ ja katsotaan mitä tapahtuu.

<img width="663" height="345" alt="image" src="https://github.com/user-attachments/assets/d1db42b0-f497-45c2-9312-67007fe9bb07" />

Decompile ikkunassa nähdään, että muuttujan arvolla 0 (= salasanat täsmäävät) päädytään nyt virheilmoitukseen, ja arvolla != 0 tulostuu flagi.

Export toiminnolla viedään muutokset.

<img width="331" height="216" alt="image" src="https://github.com/user-attachments/assets/95b5422c-bbff-4555-8b99-cbbe1547f2a0" />

Ohjelma ajettavaan muotoon:

    chmod +x passtr

<img width="510" height="640" alt="image" src="https://github.com/user-attachments/assets/7004be3a-1418-4cf3-8dbb-b19b2fab5396" />

Sitten testaus oikealla salasanalla (antaa virheilmoituksen) ja sitten kahdella väärällä (joilla saadaan flagi):

<img width="597" height="341" alt="image" src="https://github.com/user-attachments/assets/5f3155ab-166e-4b01-ad1b-c6b2f43bc5ff" />


## d) Nora CrackMe

crackmes -repositorion kloonaus `clone https://github.com/NoraCodes/crackmes.git`

<img width="605" height="443" alt="image" src="https://github.com/user-attachments/assets/fc4d8338-519a-49fe-b7bd-580d04b2608b" />

README.md

<img width="1076" height="274" alt="image" src="https://github.com/user-attachments/assets/d99e0ba2-d14a-4d50-81ad-767be5098180" />

## e) Nora CrackMe 01

Aloitin seuraamalla README.md ohjeita ja ajoin komennon `make crackme01`

<img width="801" height="226" alt="image" src="https://github.com/user-attachments/assets/a2675012-c2d9-4bbd-a7bc-4873e10cfdde" />

Seuraavaksi ghidran pariin ja uusi projekti crackme01.

<img width="501" height="277" alt="image" src="https://github.com/user-attachments/assets/a092b257-fd3e-43d3-842c-53faa949a7d0" />

Analysoinnin jälkeen:

<img width="478" height="470" alt="image" src="https://github.com/user-attachments/assets/d49f0a1f-c1ca-4176-94b9-6094f61c48ef" />

Vastaus näyttäisi löytyvän tästä:

<img width="429" height="489" alt="image" src="https://github.com/user-attachments/assets/43403db8-222a-4045-a1fd-9a0db0dcb9c4" />

strncmp -funktio vertaa yhdeksää ensimmäistä käyttäjän syöttämää merkkiä salasanaan password1 [lähde](https://www.w3schools.com/c/ref_string_strncmp.php)

käytännössä siis password1 taikka password1 + mikä tahansa määrä merkkejä pitäisi kelvata salasanasta.

Testauksen tulos salasanoilla `password2, password1, password123456789`

<img width="318" height="249" alt="image" src="https://github.com/user-attachments/assets/e5e19445-f4eb-4c78-9321-3f4c2a7e5364" />

## e) Nora CrackMe 01e

Aloitetaan `make crackme01e`

<img width="596" height="130" alt="image" src="https://github.com/user-attachments/assets/bdcc9523-848a-4d57-a4f4-33a41a1423d2" />

Ghidralla analysoinnin tulos:

<img width="464" height="463" alt="image" src="https://github.com/user-attachments/assets/af76470e-c397-45e4-ba49-91876c0779f6" />

Vaikuttaisi vastaavalta kuin edellinen tehtävä salasanalla slm!paas.k, testasin sillä ja eteen tuli ongelma:

<img width="297" height="70" alt="image" src="https://github.com/user-attachments/assets/5b5c6516-4b84-4445-b87f-e60c6587473b" />

Löysin vinkkejä [täältä](https://superuser.com/questions/1774083/zsh-event-not-found-bin-bash) ja `man zshexpn` selvisi seuraavaa:

<img width="1058" height="126" alt="image" src="https://github.com/user-attachments/assets/e06e0bad-6f89-4f41-8031-31ef7caa3830" />

Löysin vinkin [täältä](https://unix.stackexchange.com/questions/33339/cant-use-exclamation-mark-in-bash) ja helpoin korjaus oli vain käyttää yksinkertaisia lainausmerkkejä '

<img width="298" height="89" alt="image" src="https://github.com/user-attachments/assets/47316d5e-da87-4279-8ca5-577fd064451b" />

## f) Nora Crackme 02

<img width="702" height="572" alt="image" src="https://github.com/user-attachments/assets/706c4764-541d-414a-9d10-c46d1488dafc" />

Tehtävä meni aika haastavaksi, enkä keksinyt ratkaisua. Pyysin chatGPT:tä avaamaan hieman kyseistä koodia promptilla 'selitä koodin rivit'.

Ratkaisu löytyy siis tältä riviltä:

<img width="479" height="559" alt="image" src="https://github.com/user-attachments/assets/429d1313-0ed2-49bb-9bd9-aa50e1332aab" />

eli kyseisessä tarkistuksessa verrataan cVar2 - 1 arvoa muuttujan cVar1 arvoon.

cVar2 muuttujassa käydään laskurin avulla merkkijono "password1" läpi merkki kerrallaan.

<img width="439" height="543" alt="image" src="https://github.com/user-attachments/assets/ea4a4287-c685-446d-975b-2fd65b9f2fc2" />

Tehtävän vastaus saadaan ottamalla salasana 'password1' ja vähentämällä jokaisen merkin arvoa yhdellä.

[ASCII table](https://www.ascii-code.com/) voidaan katsoa, mikä merkki vastaa mitäkin merkkiä, esimerkkinä taulukosta a - 1 = `

<img width="488" height="61" alt="image" src="https://github.com/user-attachments/assets/5c56f607-324e-4761-81b9-c8bb5596f4b5" />

näin ollen salasanasta password1 voidaan muodostaa oikea salasana seuraavasti:

```
p - 1 = o
a - 1 = `
s - 1 = r
s - 1 = r
w - 1 = v
o - 1 = n
r - 1 = q
d - 1 = c
1 - 1 = 0
```

Salasanan pitäisi olla o`rrvnqc0

<img width="273" height="81" alt="image" src="https://github.com/user-attachments/assets/2184971b-cd97-4a6d-820c-bf0d834cb80c" />

erikoismerkki aiheutti taas ongelmia (backtick), joten laitoin salasanan lainausmerkkeihin: 'o`rrvnqc0'

<img width="303" height="83" alt="image" src="https://github.com/user-attachments/assets/dfbbfa31-942e-4f84-aafb-0812ca3cbd92" />




### Lähteet

ascii-code.com. ASCII table. Luettavissa: https://www.ascii-code.com/.

ChatGPT. https://chatgpt.com/. Syötteellä: 'selitä koodin rivit'.

Hammond, J. GHIDRA for Reverse Engineering (PicoCTF 2022 #42 'bbbloat'). Katsottavissa: https://www.youtube.com/watch?v=oTD_ki86c9I.

Karvinen, T. 13.8.2025. Sovellusten hakkerointi. Luettavissa: https://terokarvinen.com/sovellusten-hakkerointi/.

Soonerbourne. Patching BInaries with Ghidra. Katsottavissa: https://www.youtube.com/watch?v=8U6JOQnOOkg.

Srinivas. 17.2.2021. How to control the flow of a program in x86 assembly. Luettavissa: https://www.infosecinstitute.com/resources/secure-coding/how-to-control-the-flow-of-a-program-in-x86-assembly/#:~:text=JNZ%20(Jump%20if%20Not%20Zero,the%20sign%20flag%20being%20set)

SuperUser. zsh: event not found: /bin/bash. Luettavissa: https://superuser.com/questions/1774083/zsh-event-not-found-bin-bash.

w3schools. C stdio scanf() Function. Luettavissa: https://www.w3schools.com/c/ref_stdio_scanf.php.

w3schools. C string strcmp() function. Luettavissa: https://www.w3schools.com/c/ref_string_strcmp.php.

w3schools. C string strncmp() function. Luettavissa: https://www.w3schools.com/c/ref_string_strncmp.php
