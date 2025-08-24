## H1 - Korkeat standardit [Tehtävänanto](https://terokarvinen.com/sovellusten-hakkerointi/#h1-korkeat-standardit-lari)

### A)

3.2 Attack - Hyökkäys

- Hyökkäyksen tarkoituksena on yritys tuhota, altistaa, muokata, lamauttaa, varastaa taikka saada luvaton pääsy
hyökkäyksen kohteen tietoihin ja/tai omaisuuteen.

3.31 Information security incident - Tietoturvapoikkeama

- Yksittäinen tai useampi ei-toivottu/odottamaton tapahtuma, jolla on merkittävä riski yrityksen liiketoimintaan
ja tietoturvaan.

3.56 Requirement - Vaatimus/edellytys

- Pakollinen tai yleisesti edellytetty määritelty tarve tai vaatimus.

3.58 Review - Tarkastelu

- Tarkoituksena arvioida soveltuvuus, riittävyys ja tehokkuus aihealueelta ennalta asetettujen tavoitteiden
saavuttamiseksi.

3.77 Vulnerability - Haavoittuvuus

- Kohteessa tai kontrollissa oleva haavoittuvuus, jota yksi tai useampi uhka voi hyväksikäyttää.

### B)

ISO 27034-1 Standardi kokonaisuus käsittelee sovellusturvallisuutta. Standardi on jaettu kuuteen osaan (parts).

1. osassa otetaan yleiskatsaus sovellusturvallisuuteen. Osassa käsitellään määritelmät, konseptit, periaatteet ja prosessit liittyen sovellusturvallisuuteen.

2. osassa syvennytään organisaation normatiiviseen viitekehykseen, sen komponentteihin sekä organisaatio tason prosesseihin ja sen hallintaan. Osassa selitetään yhteydet näiden prosessien välillä, toiminnot niihin liittyen ja näiden tukitoiminnot. Osassa käsitellään kuinka organisaation tulisi implementoida ja integroida ISO/IEC 27034 sen olemassa oleviin toimintoihin.

3. osassa syvennytään sovellus projektin prosesseihin: sovellusvaatimukset, sovellusympäristö, sovelluksen turvallisuus riskien arviointi, sovelluksen normatiivisen viitekehyksen toteuttaminen ja ylläpito sekä näiden huomioiminen koko sovelluksen elinkaaren ajan. Luvussa käsitellään näiden prosessien yhteydet, toiminnot ja riippuvuussuhteet ja kuinka ne tuovat turvaa sovellusprojektiin.

4. osassa käsitellään sovellusturvallisuuden todentaminen ja sertifiointi prosessi, joka mittaa sovelluksen todellista luottamustasoa ja vertaa sitä organisaation valitsemaan tavoiteltuun sovelluksen luottamustasoon.

5. osassa käydään läpi protokollat ja XML (Extensible Markup Langauge) kaavio sovellusturvallisuuden kontrollia (ASC, Application Security Control) varten, joka pohjautuu ISO/IEC TS 15000 -sarjaan ebXML (Electronic business eXtensible Markup Language). Sitä hyödynnetään organisaatioissa todentamaan organisaation ASC:n ja muiden komponenttien tietorakenne ISO/IEC 27034 mukaan ja auttamaan jakelun automatisoinnissa, päivityksessä ja käytössä.

6. osassa tarpeen mukaan tarjotaan esimerkkejä sovellusturvallisuuden kontrolleista jotka on räätälöity tietyille sovellusturvallisuuden vaatimuksille.



### C)

Laatulöpinät podcastissa esitellään kuusi väittämää:
[Laatulöpinät podcast](https://www.arter.fi/podcast/laatulopinat-podcast-tietoturvallisuus-ohjelmistokehityksessa-tarkastele-kokonaisuutta-ja-hyodynna-viitekehykset/)

<img width="827" height="387" alt="image" src="https://github.com/user-attachments/assets/0f5f7487-db89-444e-8a50-b9a004fb1121" />

1. Olen väitteestä samaa mieltä. Tietoturvan kehityksestä huolimatta uusia uhkia löytyy jatkuvasti. Inhimillisyys ja erehtyminen mainittiin myös riskitekijänä.

2. Tästä myös samaa mieltä. Tavoitteeseen pääsemiseksi vuorovaikutus ja yhteistyö ovat välttämättömiä. Tämä pätee kaikessa yritystoiminnassa yleisesti muutenkin. 

3. Automaatiotestaus on mielestäni tehokas työkalu, kunhan siihen ei luoteta sokeasti. Työtunteja voidaan säästää huomattava määrä, kun ymmärretään automaatiotestauksen rajat ja mahdollisuudet. Hyvä renki manuaalitestauksen ohella.

4. Tämä lienee ikuisuuskysymys. Täytyisi löytää se kultainen keskitie, jolloin käyttäjä pystyy toimimaan tietoturvallisesti ja käyttäjäkokemuksen kannalta ohjelmisto olisi mahdollisimman nopea ja helppokäyttöinen. Liian vaikea käytettävyys on sinällään myös jo riski tietoturvan kannalta. Ohjelmistoa suunniteltaessa jatkuva palautteen kerääminen testikäyttäjiltä antaa hyvää suuntaa lopputuloksen kannalta.

5. Hyvin pitkälti samaa mieltä. Podcastissa mainittiin saavutettavuus, joka tulisi ottaa huomioon myös ohjelmistoissa, jotka eivät sisällä kovin arkaluonteista tietoa. Kuitenkin hyvin arkaluonteista sisältöä käsittelevät ohjelmistot tulisi suunnitella huomattavasti tiukemmiksi kuin muut ohjelmistot. Mielestäni jokainen ohjelmisto tulisi kuitenkin suunnitella tietoturva edellä.

6. Pitää varmaankin paikkansa. Ohjelmistokehittäjä toki tuntee oman ohjelmistonsa läpikotaisin, ja kuten podcastissakin hyvin mainittiin, niin "tieto lisää tuskaa". Olettaisin, että pätee tässä yhteydessä hyvinkin pitkälle. Muiden tekemiä ohjelmistoja ei välttämättä osaa katsella yhtälailla kriittisesti kuin omaa tuotostaan.




### D)

## Ympäristö

Kannettavassa tietokoneessa Windows 10 jota käytän toistaiseksi, kunnes se väistyy Linuxin tieltä (Debian tai Kali Linux).
Virtualisointiohjelmisto luultavasti päivittyy pian myös toiseen ohjelmistoon. 

PC

- Käyttöjärjestelmä: Windows 10 Home 64-bit

- Suoritin: Intel(R) Core(TM) i7-4710HQ

- Muisti: 12GB RAM

- Näytönohjain: Intel(R) HD Graphics 4600

- Virtualisointiohjelmisto: Oracle VM VirtualBox 7.0

- Internet yhteys: DNA Netti 600 M

Virtuaalikone

- ISO Image: Debian live 12.9.0 amd 64 xfce
- Version: Debian 12 Bookworm (64-bit)
- Base Memory: 4000 MB
- Processors: 1
- Virtual HD: 60 GB
- Video Memory: 128 MB




## Toimia, joilla varmistua siitä ettei ympäristöön päästä tunkeutumaan / etteivät mahdolliset haittaohjelmat pääse leviämään ympäristön ulkopuolelle

- Tarpeen vaatiessa verkkoyhteys pois käytöstä virtuaalikoneelta (Enable Network Adapter taikka Cable Connected ei valittuina)
- Virtuaalikoneen asetuksista hyödynnän sisäistä verkkoa (Attached to: Internal Network), jolla saan eristettyä virtuaalikoneen omaan testiympäristöön.
- Kaikki epäilyttävät tiedostot analysoin eristetyssä ympäristössä.
- Poistan käytöstä kaikki jako-ominaisuudet (clipboard, folders, resources, drivers jne.).
- Pidän kaikki ohjelmistot päivitettyinä ja palomuurin toiminnassa.

### Lähteet:

Arter Oy. Laatulöpinät-podcast: Tietoturvallisuus ohjelmistokehityksessä. Kuunneltavissa: https://www.arter.fi/podcast/laatulopinat-podcast-tietoturvallisuus-ohjelmistokehityksessa-tarkastele-kokonaisuutta-ja-hyodynna-viitekehykset/.

Haaga-Helia ammattikorkeakoulu 2025. Haaga-Helia ammattikorkeakoulun Intranet. Sovellusten hakkerointi ja haavoittuvuudet Tero ja Lari -opintojakson materiaali Moodlessa.

Karvinen, T. 13.8.2025. Sovellusten hakkerointi - 2025 alkusyksy. Luettavissa https://terokarvinen.com/sovellusten-hakkerointi/.

Reddit. Use VirtualBox as a sandbox. Luettavissa: https://www.reddit.com/r/virtualbox/comments/10j630o/use_virtualbox_as_a_sandbox/.

Virtualbox.org. Virtual Networking. Luettavissa: https://www.virtualbox.org/manual/ch06.html#network_internal.






