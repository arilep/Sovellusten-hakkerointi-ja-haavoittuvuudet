### h3 No strings attached [Tehtävänanto](https://terokarvinen.com/sovellusten-hakkerointi/#h3-no-strings-attached-tero)

## Tehtävissä käytetty ympäristö

### PC

- Käyttöjärjestelmä: Windows 11 Home 64-bit
- Suoritin: AMD Ryzen 7 5800X 8-Core Processor
- Muisti: 32GB RAM
- Näytönohjain: NVIDIA GeForce RTX 3080
- Virtualisointiohjelmisto: Oracle VM VirtualBox 7.0

- Internet yhteys: DNA Netti 600 M

### Virtuaalikone

- ISO Image: Debian live 12.9.0 amd 64 xfce
- Version: Debian 12 Bookworm (64-bit)
- Base Memory: 8000 MB
- Processors: 1
- Virtual HD: 60 GB
- Video Memory: 128 MB

## a) Strings
Ensin päivitysten ajo `sudo apt-get update` jonka jälkeen aloitin osion lataamalla tehtävänannon zip-tiedoston [ezbin-challenges.zip](https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip) virtuaalikoneelleni:

`wget https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip`

Paketin purku: `unzip ezbin-challenges.zip` ja sen sisältö:

<img width="798" height="455" alt="image" src="https://github.com/user-attachments/assets/3a8eb5c6-92be-4b3b-8f70-632b7569efab" />

<br></br>

Tämä osio käsittelee tuota passtr -hakemistoa, vilkaisin ensin mitä kansiosta löytyy:

<img width="482" height="140" alt="image" src="https://github.com/user-attachments/assets/d1f649cc-e673-44b6-974f-90396221e114" />

<br></br>

Ja sitten vielä vilkaisu README.md -tiedostoon:

<img width="803" height="525" alt="image" src="https://github.com/user-attachments/assets/c12bf4a2-0301-461d-ac53-044df105c434" />

<br></br>

Ajamalla passtr ohjelman: `./passtr` se kysyy salasanaa. Testasin syöttämällä salasanaksi 'password'.

<img width="477" height="124" alt="image" src="https://github.com/user-attachments/assets/cdc149e4-8dbc-498a-adf2-6e19dd9e04d2" />

<br></br>

`strings` -komennolla voidaan etsiä tiedostosta tulostettavia merkkijonoja. Oletuksena `strings` etsii vähintään neljän merkin pituisia merkkijonoja [(lähde)](https://www.ibm.com/docs/en/aix/7.2.0?topic=s-strings-command).

Ajetaan `strings passtr | nl` ja tutkitaan näitä merkkijonoja:

<img width="818" height="692" alt="image" src="https://github.com/user-attachments/assets/3c121ed0-266e-4368-ab86-1cc3fcc8829d" />

<br></br>

Rivejä tulostuu 82 kappaletta, joista rivi 18 vaikuttaa lupaavalta. Testasin kyseistä salasanaa ja sieltä löytyi lippu:

<img width="709" height="138" alt="image" src="https://github.com/user-attachments/assets/cebb911b-9f64-4a1a-8647-1646431b275e" />

## b) passtr.c -ohjelman uusi versio (Ei turvallinen!)

Suoritin C -osion ennen tätä vaihetta, joten ajattelin testata UPX-ohjelmaa ja pakata tiedoston.

Lähtötilanne `strings passtr | nl`:

<img width="800" height="535" alt="image" src="https://github.com/user-attachments/assets/ad5ff68e-c0e2-43c2-b11c-4c3c622fe3e2" />

<br></br>

Pakataan komennolla `upx passtr`:

<img width="793" height="253" alt="image" src="https://github.com/user-attachments/assets/bf631c74-7606-4a47-a967-7c84e9d0fd1c" />

<br></br>

ja katsotaan tulos `strings passtr | nl`:

<img width="803" height="413" alt="image" src="https://github.com/user-attachments/assets/64addd65-8455-4584-bc9b-4227495731ce" />

<br></br>

Riveiltä 21-22 löytyy salasanan "muruset". Riville 34 tulostuu myös tieto, että tiedosto on pakattu käyttäen UPX -ohjelmaa. Tämä vaihtoehto ei ole turvallinen, sillä salasanaan pääsee edelleen käsiksi vain purkamalla tiedoston komennolla `upx -d passtr`.

## b) Vaihtoehtoinen tapa hyödyntäen tekoälyä (Ei turvallinen!)

Lähtötilanne:

<img width="801" height="460" alt="image" src="https://github.com/user-attachments/assets/2ce81a23-763e-4c3a-ba7f-9eed3c9ee902" />

<br></br>

C-kieli on itselleni hieman vieras, joten kysäisin tekoälyltä (ChatGPT) vinkkejä promptilla:

'Miten obfuskoida kyseinen koodi, mitä vaihtoehtoisia tapoja siihen on?'

XOR-menetelmä tuli esiin vastauksissa ja päätin paneutua siihen. ChatGPT:llä generoitu korjaus näytti seuraavalta:

<img width="979" height="812" alt="image" src="https://github.com/user-attachments/assets/09d948c5-d4d2-4526-a15d-c0c2c7d71803" />

<br></br>

Salasana on nyt piilotettu XOR-menetelmällä, joka tulee sanoista eXclusive OR [(Exclusive or - Wikipediassa)](https://en.wikipedia.org/wiki/Exclusive_or). Salasanan merkit on korvattu laskutoimituksella. Esim. kirjain s -> 's'^0x12 antaa jonkin ei luettavan tavuarvon.
strcmp -funktio vertaa käyttäjän syötettä ja piilotettua salasanaa. 

Koodin kääntäminen onnistuu README.md ohjeiden mukaisesti:

<img width="1262" height="434" alt="image" src="https://github.com/user-attachments/assets/571abe47-9ff7-44ec-af69-6f837079066a" />

<br></br>

Salasanaa ei löydy enää selkokielisenä ja koodi toimii kuten kuuluukin:

<img width="821" height="515" alt="image" src="https://github.com/user-attachments/assets/b19ef880-ef17-45bf-a0cf-95dfaada6e39" />

<img width="711" height="115" alt="image" src="https://github.com/user-attachments/assets/40307232-1f56-482a-bd9e-9ac2c82da5e9" />

<br></br>

## c) Packd

Tarkastelin ~/challenges/packd -hakemiston sisältöä:

<img width="524" height="147" alt="image" src="https://github.com/user-attachments/assets/0bc98050-818f-40d6-944a-c9bb0431a673" />

<br></br>

README.md sisältö:

<img width="611" height="198" alt="image" src="https://github.com/user-attachments/assets/7615fe89-b54b-488d-84a5-2e910e75b44f" />

<br></br>

Kokeilin ajaa ohjelman `./packd` joka kysyi salasanaa. Kokeilumielessä syötin jälleen 'password'

<img width="438" height="126" alt="image" src="https://github.com/user-attachments/assets/e3ccac5f-c24d-4cd4-bd72-7d3f0819afec" />

<br></br>

Lähdin tutkimaan tiedostoa `strings packd | nl` avulla:

<img width="386" height="189" alt="image" src="https://github.com/user-attachments/assets/48459cab-9536-441a-a98d-d2e3bbf32bf2" />

<br></br>

Rivillä 23 löytyy 'piilos-An', testasin sitä salasana kenttään:

<img width="424" height="117" alt="image" src="https://github.com/user-attachments/assets/1b4ec0fd-6244-4bc7-8a6d-0dd26acacff5" />

<br></br>

Ei tulosta, palasin tarkastelemaan `strings packd` tuloksia. Riviltä 35 löytyi jotain mielenkiintoista:

<img width="803" height="90" alt="image" src="https://github.com/user-attachments/assets/d9b25b01-2ec0-45d6-8583-c10e7285deaa" />

<br></br>

Näyttäisi siltä, että tiedosto on pakattu UPX-ohjelmalla. Havaintona myös tehtävänannon nimi 'packd' vihjaa tähän suuntaan.

Kävin tutustumassa kyseiseen ohjelmaan osoitteessa: https://github.com/upx/upx

Kyseisen ohjelman lataus `sudo apt-get update` `sudo apt-get install upx`

googlaamalla 'upx commands' löysin manuaali sivun: https://www.mankier.com/1/upx

`upx --help` löytyy myös komennot:

<img width="799" height="319" alt="image" src="https://github.com/user-attachments/assets/dceb35a1-ddda-419d-b1c1-eca177df11d8" />

<br></br>

Komennolla `upx -d packd` pitäisi siis onnistua purkaminen. Kokeilin ajaa komennon:

<img width="799" height="258" alt="image" src="https://github.com/user-attachments/assets/b1e21d28-9765-4a42-9446-ce67269f8780" />

<br></br>

--> Unpacked 1 file. Kokeilin ajaa uudelleen strings -komennon `strings packd | nl`:

<img width="793" height="142" alt="image" src="https://github.com/user-attachments/assets/2d068fa0-861f-4476-90d6-b8e6adf1ac40" />

<br></br>

Koko salasana näyttää olevan nyt esillä. Testasin piilos-AnAnAs -salasanaa ja lippu löytyi:

<img width="728" height="121" alt="image" src="https://github.com/user-attachments/assets/3dbf7e20-4c46-43d9-bbce-210cf76e6187" />

### Lähteet

ChatGPT. https://chatgpt.com/. Syötteillä: 'Miten obfuskoida kyseinen koodi, mitä vaihtoehtoisia tapoja siihen on?'

IBM. strings Command. Luettavissa: https://www.ibm.com/docs/en/aix/7.2.0?topic=s-strings-command.

Karvinen, T. 13.8.2025. Sovellusten hakkerointi. Luettavissa: https://terokarvinen.com/sovellusten-hakkerointi/.

Mankier. upx - Man Page. Luettavissa: https://www.mankier.com/1/upx.

Oberhumer, M., Molnar, L. & Reiser, J. UPX - the Ultimate Packer for eXecutables. Luettavissa: https://github.com/upx/upx.

Wikipedia. Exclusive or. Luettavissa: https://en.wikipedia.org/wiki/Exclusive_or.
