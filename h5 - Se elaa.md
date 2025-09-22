### h5 Se elää! [Tehtävänanto](https://terokarvinen.com/sovellusten-hakkerointi/#h5-se-elaa-lari)

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

## a) Lab 1

Alkuun gdb asennus:

    sudo apt-get update
    sudo apt-get install -y gdb

Uusi kansio työstettäville tehtäville, lab1.zip lataus ja purku:

    mkdir lab && cd lab
    wget https://terokarvinen.com/sovellusten-hakkerointi/lab1.zip
    unzip lab1.zip

Puretun kansion sisältö:

<img width="446" height="178" alt="image" src="https://github.com/user-attachments/assets/453d8ef5-4f83-4640-97ed-f17d3b1c5afc" />

Vilkaisin mitä ohjelma tekee ja mitä lähdekoodi gdb_example1.c pitää sisällään:

<img width="346" height="451" alt="image" src="https://github.com/user-attachments/assets/b1ec6a5d-cdd3-4cba-8f2d-ad2dd27f3ce9" />

Tarkistus, että ohjelmassa on debug_info käytössä `file gdb_example1`

<img width="1074" height="82" alt="image" src="https://github.com/user-attachments/assets/5c932375-95a6-4aae-a1ed-b992d60d6bce" />

Seuraavaksi ajoin ohjelman debuggerissa:

    gdb gdb_example1

Debuggerissa `lay next` ja Enter kerran antaa seuraavanlaisen näkymän, joka helpotti omaa työskentelyäni:

<img width="1075" height="708" alt="image" src="https://github.com/user-attachments/assets/4c0bef56-5c61-4cb7-bed7-5ea64af9d9fd" />

Ajamalla gdb:ssä `run` näyttää seuraavaa:

<img width="617" height="138" alt="image" src="https://github.com/user-attachments/assets/9b929ce5-be40-4fce-8500-13ffe1daa8ba" />

Ohjelma siis tulostaa `Khoor/#zruog1` ja seuraavalla rivillä antaa jonkinlaisen virheilmoituksen:

`Program received signal SIGSEGV, Segmentation fault.`

Googlailin hakutuloksia  `what is SIGSEGV, Segmentation fault.` Löysin keskustelua aiheesta [Stackoverflowsta](https://stackoverflow.com/questions/1564372/what-causes-a-sigsegv) sekä Wikipedian [artikkelin](https://en.wikipedia.org/wiki/Segmentation_fault).

Stackoverflow -sivulta löytyi maininta: "A segfault basically means you did something bad with pointers."

Wikipedian artikkelista selviää että kyseessä on segmentointivirhe, joka johtuu siitä että ohjelmisto yrittää käyttää muistialuetta, johon sillä ei ole oikeutta.

Käynnistin debuggerin uudelleen, asetin breakpointin main funktioon `break main`. Tämän jälkeen `run` ja siirryin `next` -komennolla lähdekoodin rivi kerrallaan selvittämään missä virhe tapahtuu.

<img width="620" height="361" alt="image" src="https://github.com/user-attachments/assets/ecdf1805-f1a2-45c4-88dc-2b2cf276b13e" />

Ohjelma kaatuu siis tuossa rivillä 18 `print_scrambled(bad_message);` kun kutsutaan funktiota `printf("%c", (*message)+i);` rivillä 7.

<img width="482" height="404" alt="image" src="https://github.com/user-attachments/assets/5401ffc3-c3ce-483e-b734-84ffaf2f5274" />

`run` ja `backtrace` päästään samaan päätelmään.

<img width="646" height="193" alt="image" src="https://github.com/user-attachments/assets/f15485f4-5d59-4c3c-90db-7c31c21f5815" />

Päättelin, että ohjelma toimii siten, että se hakee bad_message ja good_message arvot (rivit 14 ja 15) ja muuttaa jokaisen merkin arvoa ASCII taulussa +3 (message + i, jossa i = 3).

Täten `Hello, world` kääntyy tuohon kryptiseen viestiin `Khoor/#zruog1`, mutta bad_message arvolla NULL kaataa ohjelman ja seuraa virheilmoitus. Ongelma voidaan korjata antamalla bad_messagelle uusi arvo.

Annoin bad_message:lle uuden arvon "qlfjff", jonka pitäisi kääntyä siis muotoon "toimii".

<img width="424" height="363" alt="image" src="https://github.com/user-attachments/assets/1dc63637-422c-4cee-a7dc-b1b244dff704" />

Tallensin muutokset, ajoin Makefilen: `make` ja tarkistin lopputuloksen:

<img width="457" height="199" alt="image" src="https://github.com/user-attachments/assets/f81f955c-bc8f-4bdd-afd8-165163485fa8" />

## b) Lab 2

Lab2 lataus ja purku:

    wget https://terokarvinen.com/sovellusten-hakkerointi/lab2.zip
    unzip lab2.zip

Sisältö:

<img width="412" height="334" alt="image" src="https://github.com/user-attachments/assets/e70ee0ad-24d8-4162-9103-38bb709feb8c" />

README.md sisältö:

<img width="1234" height="704" alt="image" src="https://github.com/user-attachments/assets/0224c3ce-d9b9-492d-8828-e7a3da2094b2" />

Kokeilin ensin ajaa ohjelman ja katsoa mitä se tekee `./passtr2o`:

<img width="429" height="213" alt="image" src="https://github.com/user-attachments/assets/8035fad3-3670-45ae-8c25-8628df46cde4" />

Ohjelma pyytää salasanaa, annoin salasanaksi "testi".

tutkiskelin lisää komennolla `file passtr2o`:

<img width="1231" height="79" alt="image" src="https://github.com/user-attachments/assets/9c0d4461-801a-4139-acbd-cc06a6d6fa52" />

huomioitavaa: not stripped, mutta mukana ei ole debug infoa.

Ajoin ohjelman debuggerissa.

<img width="619" height="190" alt="image" src="https://github.com/user-attachments/assets/4b1adfcd-3a69-472d-94c1-2e11822ef9b0" />

`list` ei luonnollisesti antanut mitään. `run` ajaa ohjelman ja syötin tällä kertaa salasanaksi "salasana".

Jotta saisin ohjelmasta jotain irti:

    break main # break point main funktioon
    run # ajetaan
    disassemble main # funktion disassembly koodi näkyviin

<img width="700" height="796" alt="image" src="https://github.com/user-attachments/assets/1f0e132c-7ad7-4c44-bbc1-4ad5fd00cf65" />

call -kutsuja löytyy yhteensä viisi. Päättelin että ensimmäinen call `puts@plt` tulostaa "What's the password?" (puts komento). Toinen call `__isoc_scanf@plt` lukee käyttäjän syötteen (scanf -funktio).

Viides call `printf@plt` täytyy tulostaa "Sorry, no bonus." taikka oikean salasanan kanssa halutun flagin.

Kysymysmerkeiksi jää siis toinen ja kolmas call -kutsu `mAsdf3a` ja `EaseEAs`, jotka liittyvät salasanan tarkistamiseen.

Asetin breakpointin ensin tuohon `mAsdf3a` ja sen jälkeen disassemble:

<img width="581" height="606" alt="image" src="https://github.com/user-attachments/assets/7da92344-a1f2-4590-b0fb-fab4953dede7" />

Googlaamalla 'registry jne' selvisi, että se tulee sanoista "Jump if Not Equal" [(lähde)](https://www.quora.com/What-does-JNE-do-in-assembly)

cmp tarkoittaa 'Compare' = vertaile. Eli tuossa verrataan rekistereiden %r12d ja %edx arvoja, ja hypätään kohtaan mAsdf3a+85 mikäli ne eivät täsmää.

Tässä kohtaa jäin jumiin, enkä keksinyt miten edetä tehtävän ratkaisemiseksi.

### Lähteet

Karvinen, T. 13.8.2025. Sovellusten hakkerointi. Luettavissa: https://terokarvinen.com/sovellusten-hakkerointi/

Low Level. GDB is REALLY easy! Find Bugs in Your Code with Only A Few Commands. Katsottavissa: https://www.youtube.com/watch?v=Dq8l1_-QgAc

Quora. What does JNE do in assembly?. Luettavissa: https://www.quora.com/What-does-JNE-do-in-assembly

Stackoverflow. What causes a SIGSEGV?. Luettavissa: https://stackoverflow.com/questions/1564372/what-causes-a-sigsegv

Wikipedia. Segmentation fault. Luettavissa: https://en.wikipedia.org/wiki/Segmentation_fault
