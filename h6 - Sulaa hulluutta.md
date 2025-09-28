### h6 Sulaa hulluutta [Tehtävänanto](https://terokarvinen.com/sovellusten-hakkerointi/#h6-sulaa-hulluutta-lari)

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

## Lab 0

Kurssin moodle sivulta kävin lataamassa tutkittavan tiedoston h1.jpg

<img width="383" height="197" alt="image" src="https://github.com/user-attachments/assets/04e98320-6ba2-458a-b969-97795ae574cb" />

### File

`file h1.jpg` näyttää seuraavaa:

<img width="1074" height="86" alt="image" src="https://github.com/user-attachments/assets/4c77928c-7abb-4709-a2d0-9ebef2065d00" />

- Tiedosto näyttäisi olevan JPEG-muodossa (JPEG image data)
- Kuvan resoluutio 1024x1024
- Tiedosto sisältää Exif metatietoja

<br></br>

### Strings

strings -komento antoi älyttömän pitkän listan, joten karsin tulokset 10 merkin pituuteen `strings -n 10 h1.jpg`

<img width="471" height="761" alt="image" src="https://github.com/user-attachments/assets/53f2cb2c-016e-4a8e-9a65-8b67c0fd60a9" />

<br></br>

Tuosta litaniasta mielenkiintoisimmat tulokset karsittuna:

<img width="391" height="708" alt="image" src="https://github.com/user-attachments/assets/56fde4d5-6191-4b54-b4a8-a9502897002e" />

Tuloksissa toistuu useaan kertaan .xml, .xmlPK ja word. Viitteitä word -tiedostoon taikka pakattuun tiedostoon?

<br></br>

### ExifTool

Googlaamalla `jpg file analyze on kali` löysin mielenkiintoisen [videon](https://www.youtube.com/watch?v=qu0ZvBzNmHQ). Videolla hyödynnetään ExifTool -nimistä työkalua kuvan metatietojen tutkimiseen. ExifTool [github-linkki](https://github.com/exiftool/exiftool).

ExifTool löytyi Kalista esiasennettuna: 

<img width="660" height="162" alt="image" src="https://github.com/user-attachments/assets/556ab847-382e-4683-9276-89d11d7cec2e" />

<br></br>

Ajetaan `exiftool h1.jpg`:

<img width="519" height="550" alt="image" src="https://github.com/user-attachments/assets/01380f99-a767-472f-a76a-1c6456a9bfc4" />

Tuloksista ei paljastunut paljoa uutta. Voidaan todeta sama tiedostoformaatti kuin aiemminkin, kuvan koko 838 kB.

## Lab 1

### Binwalk

Löytyi versio 2.4.3 esiasennettuna:

<img width="610" height="369" alt="image" src="https://github.com/user-attachments/assets/40e86533-a584-4a5d-9ffd-241961397bbc" />

    binwalk h1.jpg

<img width="1025" height="566" alt="image" src="https://github.com/user-attachments/assets/63f38860-c2c8-4edf-9940-7a74b113e93e" />

Aiemmasta osiosta tuttuja .xml ja word tietoja, jotka näyttäisivät olevan pakattuina, kuten aiemmin epäilinkin. Koitin ajaa vielä tunnilla läpikäydyn komennon `binwalk -eM h1.jpg` (e = extract, M = matryoshka), eli automaattinen ja rekursiivinen purku.

    binwalk -eM h1.jpg

<img width="1233" height="528" alt="image" src="https://github.com/user-attachments/assets/864a0a27-3552-4806-b94f-314a43fadcc7" />

Tiedostosta saatiin nyt purettua _h1.jpg.extracted.

Tutkin seuraavaksi mitä se sisältää:

<img width="451" height="313" alt="image" src="https://github.com/user-attachments/assets/a0cfc227-7e45-42e7-8d42-efc0cf7b7d00" />

Hakemistosta paljastui jotain mielenkiintoista; 494F5.zip joka paljastui Microsoft Word -tiedostoksi.

`cat` tai `micro` ei ollut tässä kohtaa hyödyksi (satoja rivejä epämääräisiä merkkijonoja), joten googlaamalla `How to open microsoft word documents on Linux` hakutuloksista löytyi LibreOffice ja asensin sen:

    sudo apt-get install -y libreoffice

Tämän jälkeen ajoin `libreoffice 494F5.zip` ja sain seuraavan virheilmoituksen:

<img width="519" height="303" alt="image" src="https://github.com/user-attachments/assets/7750573d-7694-4479-b455-448c399e11b7" />

Testasin valitsemalla 'Yes' ja libreoffice onnistui avaamaan tiedoston:

<img width="1403" height="762" alt="image" src="https://github.com/user-attachments/assets/e9f3903b-9605-44a4-ab1a-8e5b313b0d9c" />


## Lab 2









### Lähteet

Iso-Anttila, L. Karvinen, T. s.a. Sovellusten hakkerointi ja haavoittuvuudet -opintojakson esitysmateriaali Moodlessa. Haaga-Helia ammattikorkeakoulu.

Karvinen, T. 13.8.2025. Sovellusten hakkerointi. https://terokarvinen.com/sovellusten-hakkerointi/

Offa. Android FOSS. https://github.com/offa/android-foss

Skylot. JADX. https://github.com/skylot/jadx

Konloch. Bytecode-viewer. https://github.com/Konloch/bytecode-viewer/

HackHunt. How Hackers Extract Metadata from Photos | Kali Linux. https://www.youtube.com/watch?v=qu0ZvBzNmHQ

ExifTool. ExifTool by Phil Harvey. https://github.com/exiftool/exiftool
