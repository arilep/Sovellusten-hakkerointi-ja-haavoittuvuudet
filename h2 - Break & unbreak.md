### h2 Break & unbreak [Tehtävänanto](https://terokarvinen.com/sovellusten-hakkerointi/#h2-break--unbreak-tero)

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

## x) Lue/katso/kuuntele ja tiivistä

A01 Broken Access Control
- A01:2021 Broken Access Control has moved up from the fifth position to being the most serious security risk.

Find Hidden Web Directories - Fuzz URLs with Fuff
- Fuff is a web fuzzer tool by joohoi.
- Web Servers often have hidden (secret) web directories.
- Finding these paths can be done manually, which is slow. Ffuf can do it automatically and fast.

Access control vulnerabilities and privilege escalation
- Different access control types include: vertical access controls, horizontal access controls and context-dependent access controls
- Potential risk of errors: access control design decisions have to be made by humans

Writing a report - checklist
- Reproducible
- Report the environment
- Precise (commands, timestamps, testing)
- Readable (subheaders, careful language)
- Sources!
- Standard texts (licences)
- Mistakes (fabrication, plagiarism)

## a) Murtaudu 010-staff-only

Aloitin puhtaalta pöydältä, asentamalla uuden virtuaalikoneen debian001 aiemmin mainituilla spekseillä [näiden](https://github.com/arilep/h1-Oma-Linux/blob/main/h1-oma-linux.md) ohjeiden mukaan.

Ensimmäisenä ajoin päivitykset: `sudo apt-get update`

Jonka jälkeen wget, unzip ja micron asennus tulevia työvaiheita varten: `sudo apt-get -y install wget unzip micro`

Sitten latasin tehtävänannon zip-tiedoston `wget https://terokarvinen.com/hack-n-fix/teros-challenges.zip`

ja purin sen `unzip teros-challenges.zip`

<img width="694" height="165" alt="image" src="https://github.com/user-attachments/assets/ee87b0d9-f4d8-4f00-a01a-0875fce21929" />

Seuraavaksi siirryin hakemistoon `cd challenges/010-staff-only/`

`python3 staff-only.py` antoi herjan: ModuleNotFoundError: No module named 'flask'

Tehtävissä tarvittavien python3 flask ja flask-sqlalchemy lisäosan asennus: `sudo apt-get -y install python3-flask python3-flask-sqlalchemy`

Ajoin uudelleen `python3 staff-only.py`

<img width="1029" height="187" alt="image" src="https://github.com/user-attachments/assets/2b735bde-9338-47f8-ab4e-9e788b730fab" />

URL riville http://127.0.0.1:5000 ja pääsin seuraavalle sivulle:

<img width="1269" height="688" alt="image" src="https://github.com/user-attachments/assets/544aff09-013f-4690-813d-776e8c5c8c1d" />

Syöttämällä kenttään pin-koodin '123' palauttaa sivusto "Your password is Somedude".

Ensimmäinen ongelma: kenttä ei salli mitään muuta kuin numeroita. Siirryin Developer Toolsiin (pikanäppäin F12) tutkailemaan josko tämän pystyisi ohittamaan.

Ctrl+Shift+C pikavalinnalla elementin valinta työkalu esiin ja klikkasin tuota syötekenttää.

<img width="1276" height="799" alt="image" src="https://github.com/user-attachments/assets/09373639-e498-493c-a25c-4cc800a59904" />

'input type = "number"' kenttä kiinnitti huomioni ja pyyhkäisinkin tuon 'number' pätkän riviltä pois. Nyt sain taklattua ensimmäisen ongelmani, ja pääsin syöttämään kenttään muutakin kuin numeroita.

Tutkailin materiaaleista uudelleen videota [What is SQL injection? Web Security Academy](https://www.youtube.com/watch?v=wX6tszfgYp4) josta sain vihiä, ja lähdin testailemaan syötteellä ' OR 1=1 --

<img width="685" height="174" alt="image" src="https://github.com/user-attachments/assets/f0c342ef-151d-4142-9ab4-b257e9c1ac6c" />

Edistystä, sain selville salasanan 'foo'. Tässä kohtaa löi hieman tyhjää, ja turvauduin [pikkuvinkkeihin](https://terokarvinen.com/hack-n-fix/#tips).

Vinkeistä löytyikin vihje, jota olimme käsitelleet tunnilla: "Often only the first row of results is shown". Tätä hyödyntämällä sain kasattua itselleni lauseen ' OR 1=1 LIMIT 2,1 -- ja pääsin haluttuun lopputulokseen:

<img width="980" height="167" alt="image" src="https://github.com/user-attachments/assets/7b84d9ec-cf32-4151-8ec1-08648036ef35" />

## b) Korjaa 010-staff-only haavoittuvuus lähdekoodista

Korjausta vaativat kohdat löytyivät 010-staff-only -kansion tiedostosta staff-only.py `micro staff-only.py`

<img width="1174" height="630" alt="image" src="https://github.com/user-attachments/assets/9d39b46b-19be-4d92-b90c-fbd0bfe1bf7b" />

Hain [täältä](https://askdatdude.github.io/diary/entries/diary.html?entry=SH24-002&week=) vinkkejä ja tein tarvittavat korjaukset:

<img width="617" height="168" alt="image" src="https://github.com/user-attachments/assets/4b85822d-6ab3-4043-85fb-d4a32145ac23" />

Nyt toistamalla a) kohdan vaiheet salasanaan ei päästä enää käsiksi:

<img width="715" height="267" alt="image" src="https://github.com/user-attachments/assets/b3b6c240-eb18-4b4d-906c-c54a2163d368" />


## c) Ratkaise dirfuzt-1

Seuraavaa tehtävää varten tarvitsen ffuf -työkalua

`sudo apt-get update`
`sudo apt-get install ffuf`

Sanakirjana hyödynsin Daniel Miessler:n sanakirjaa (https://github.com/danielmiessler/SecLists)

`wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt`

Tämän jälkeen harjoitusmaalin lataus komennolla: `wget https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/dirfuzt-1`

Ja sitten vielä suoritusoikeudet: `chmod u+x dirfuzt-1`

Ennen tätä vaihetta irrotin virtuaalikoneeni verkosta varmuuden vuoksi

<img width="816" height="512" alt="image" src="https://github.com/user-attachments/assets/c54c646b-5229-437b-8a4d-7d8ef2ba5776" />

Cable connected -kohdasta täppä pois ja tarkistin ettei virtuaalikoneella ole verkkoyhteyttä:

<img width="532" height="184" alt="image" src="https://github.com/user-attachments/assets/ad94b625-0364-43aa-9c1c-7a85569636a2" />

Ja nyt ajetaan: `./dirfuzt-1`

<img width="329" height="71" alt="image" src="https://github.com/user-attachments/assets/8b09c39c-fad5-4317-9726-a6017068b8cf" />

Sitten selaimeen tuo kyseinen URL 127.0.0.2:8000

<img width="631" height="199" alt="image" src="https://github.com/user-attachments/assets/3e0473ac-975b-45d3-b9fa-0fc35b8a9a06" />

Sitten testasin ajaa komennon `ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ`

<img width="816" height="553" alt="image" src="https://github.com/user-attachments/assets/e79aa17a-5014-49f5-9751-bc8a86dc1943" />

Ruutu täyttyi tuloksista, joista valtaosa vaikutti olevan nopealla silmäyksellä kooltaan 154 (Size: 154). Ajetaan uudelleen, mutta filtteröidään 154 byten kokoiset tulokset pois.

`ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ -fs 154`

 <img width="819" height="547" alt="image" src="https://github.com/user-attachments/assets/dad27cf2-1d90-4327-ab89-a9ea0033e908" />

Uskoisin olevani jäljillä. Vilkaisin eri tuloksia:

Ensimmäiset flagit löytyvät .git alkuisten hakemistojen takaa:

<img width="445" height="218" alt="image" src="https://github.com/user-attachments/assets/b34033ea-5d58-4005-a620-7c7f2d2c392b" />

Ja toinen haluttu tulos wp-admin -hakemistosta:

<img width="507" height="215" alt="image" src="https://github.com/user-attachments/assets/ca6a064c-ae03-4deb-b87c-c91b834f65cc" />

## d) Murtaudu 020-your-eyes-only

Alkajaisiksi siirryin hakemistoon `cd challenges/020-your-eyes-only/`

Asennetaan virtualenv `sudo apt-get -y install virtualenv` ja luodaan virtuaaliympäristö `virtualenv virtualenv/ -p python3 --system-site-packages`

Aktivoidaan juuri asennettu virtuaaliympäristö `source virtualenv/bin/activate`. Kyseisessä ympäristössä python -komennot ja pip asentavat paketit vain tähän ympäristöön.

Seuraavaksi etenin ohjeiden mukaan Djangon asennukseen: `cat requirements.txt` ja `pip install -r requirements.txt` ja tarkastin lopuksi ohjelmistoversion `django-admin --version`

<img width="804" height="446" alt="image" src="https://github.com/user-attachments/assets/942b08f4-b34c-4de1-b858-6cd3ff58c6c2" />

Siirryin kansioon josta manage.py löytyy `cd logtin/` ja päivitin tietokannan `./manage.py makemigrations; ./manage.py migrate`

Jälleen VIRTUAALIKONE POIS VERKOSTA tässä kohtaa varmuuden vuoksi.

Varmistin että verkkoyhteys on pois käytöstä ja sitten etenin `./manage.py runserver`

'Starting development server at http://127.0.0.1:8000/' siirryin tuohon osoitteeseen ja pääsinkin työstettävälle sivustolle

<img width="1274" height="557" alt="image" src="https://github.com/user-attachments/assets/35c1f0ac-58f5-4f8f-8146-dab39ed8c5ea" />

Ajattelin lähteä liikkeelle ajamalla `ffuf -w common.txt -u http://127.0.0.1:8000/FUZZ` ja katsoa mitä saan selville.

<img width="1045" height="578" alt="image" src="https://github.com/user-attachments/assets/280d775a-6de7-44bd-b48c-66a1155c6e00" />

admin-console vaikuttaa erittäin lupaavalta, ja johdatti minut tänne:

<img width="1271" height="728" alt="image" src="https://github.com/user-attachments/assets/9fd69eff-fcf0-4025-8dac-6532727bc5d8" />

Päätin testata Register painiketta ja katsoa pääsenkö etenemään.

Syötin uuden käyttäjätunnuksen ja salasanan.

Tämän jälkeen kirjautumissivulle ja testasin pääsenkö sisään.

<img width="1074" height="645" alt="image" src="https://github.com/user-attachments/assets/c139c50c-9734-4a96-810b-68f0cfcdd03d" />

"Jaahas" -hetki tässä kohden:

<img width="1290" height="582" alt="image" src="https://github.com/user-attachments/assets/159662d5-33a9-4f01-984d-db0d6c9d5c91" />

'Show my personal data' -painike vei tällaiselle sivulle:

<img width="1267" height="721" alt="image" src="https://github.com/user-attachments/assets/4e9b082c-c80e-45ea-a24a-0ca952972cfd" />

Admin dashboard -painikkeesta kuitenkin paljastui ainoastaan teksti '403 Forbidden'

En päässyt etenemään, päätin selata [vinkkejä](https://terokarvinen.com/hack-n-fix/#tips) ja sieltä se löytyi: "Some views are available to all. Some are available to logged in users. Some only for admin."

Ei muuta kuin admin-console uudelleen URL -kenttään sisäänkirjautuneena käyttäjänä:

<img width="1277" height="509" alt="image" src="https://github.com/user-attachments/assets/04bf0afa-eb89-49b1-9545-0a049fc50cc7" />


## e) Korjaa 020-your-eyes-only haavoittuvuus

[Vinkkien](https://terokarvinen.com/hack-n-fix/#tips) siivittämänä löysin vikakohdan täältä: `~/challenges/020-your-eyes-only/logtin/hats/views.py`

Kuvassa maalattuna korjausta vaativa lisäys:

<img width="1169" height="630" alt="image" src="https://github.com/user-attachments/assets/fa12ae03-f532-4082-a417-5ec4063db98b" />

Tallennus ja tämän jälkeen siirryin toteamaan, että korjaus toimi kuten toivottu:

<img width="572" height="188" alt="image" src="https://github.com/user-attachments/assets/76cd3935-9b1b-4644-ac9c-e249b2e6a3dd" />

### Lähteet

Karvinen, T. 4.6.2006. Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/.

Karvinen, T. 10.5.2023. Find Hidden Web Directories - Fuzz URLs with ffuf. Luettavissa: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/.

Karvinen, T. Hack'n Fix. Luettavissa: https://terokarvinen.com/hack-n-fix/.

Karvinen, T. 13.8.2025. Sovellusten hakkerointi. Luettavissa: https://terokarvinen.com/sovellusten-hakkerointi/#h2-break--unbreak-tero.

Miessler, D. SecLists. Luettavissa: https://github.com/danielmiessler/SecLists.

Niinemets, R. Hacker Diary. Luettavissa: https://askdatdude.github.io/diary/entries/diary.html?entry=SH24-002&week=.

OWASP: OWASP Top 10. Luettavissa: https://owasp.org/Top10/A01_2021-Broken_Access_Control/.

Portswigger. What is SQL injection? Katsottavissa: https://www.youtube.com/watch?v=wX6tszfgYp4.

Portswigger. Access control vulnerabilities and privilege escalation. Luettavissa: https://portswigger.net/web-security/access-control.










