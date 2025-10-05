### h7 Uhagre2 [Tehtävänanto](https://terokarvinen.com/sovellusten-hakkerointi/#h7-uhagre2-tero)

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

## x)

1.1 Terminology

- Plaintext (cleartext) - Luettava viesti ennen salausta (= selväteksti).
- Encryption - Prosessi, jossa selväteksti muutetaan salatuksi viestiksi (plaintext -> ciphertext).
- Cipher - Viestin salaamiseen tai purkamiseen käytettävä algoritmi.
- Ciphertext - Salattu viesti.
- Decryption - Salatun viestin purkaminen selvätekstiksi (ciphertext -> plaintext).
- Cryptography - Tietojen salaamisen ja suojaamisen tieteenala.

1.4 Simple XOR

- XOR - eXclusive OR (a ^ a = 0, a ^ b = 1)
- Kirjan mukaan heikko salausmenetelmä, joka on mahdollista murtaa tietokoneella nopeasti.

1.7 Large Numbers

- Kirjassa käytetään valtavan suuria lukuja kryptografian havainnollistamiseen.

## a) Convert hex to base64

Tehtävän anto oli seuraavanlainen:

<img width="982" height="415" alt="image" src="https://github.com/user-attachments/assets/8b961130-72db-4829-a5e6-2ceefb4717e0" />

Vilkaisin ohjeita hieman näiltä sivuilta:

- https://docs.python.org/3/library/binascii.html
- https://docs.python.org/3/library/base64.html#module-base64

Suoritusjärjestys:
1. Hex -> Bytes (Hex-merkkijono tavuiksi)
2. Bytes -> Base64 (tavut Base64-merkkijonoksi)

<img width="1077" height="256" alt="image" src="https://github.com/user-attachments/assets/1fecc353-78fe-4689-84af-b9c955b4dd01" />

Tulostuu ensin tavuina ja sitten Base64-merkkijono:

<img width="691" height="89" alt="image" src="https://github.com/user-attachments/assets/b8b7e74d-4887-4b60-94ee-2999676b24dc" />

## b) Fixed XOR

Tehtävänanto:

<img width="967" height="348" alt="image" src="https://github.com/user-attachments/assets/c0b39211-51b3-41fd-9579-44dc4940633a" />

Tehtävä oli itselleni haastava. Kävin katsomassa vinkkejä miten tehtävää oli lähestytty [täältä](https://medium.com/@tarunrd77/cryptopals-cryptography-solutions-set-1-cf4df0132614) ja sain kasattua seuraavanlaisen ratkaisun:

<img width="1077" height="299" alt="image" src="https://github.com/user-attachments/assets/a86098b7-cac8-4dc0-af3f-b1f2ad127421" />

Tulosteet:

<img width="401" height="90" alt="image" src="https://github.com/user-attachments/assets/c842d579-5085-425a-b4ae-865fd8e834f2" />

## c) Single-byte XOR cipher

Tehtävänanto:

<img width="981" height="408" alt="image" src="https://github.com/user-attachments/assets/e163e393-2425-4f89-9021-ebff16155f93" />

Käytin tehtävään aikaa, mutta se meni liian haastavaksi. Kävin katsomassa malliratkaisua [täältä](https://medium.com/@tarunrd77/cryptopals-cryptography-solutions-set-1-cf4df0132614) ja kävin läpi miten se toimii:

<img width="1075" height="356" alt="image" src="https://github.com/user-attachments/assets/ae24c18b-f7a6-4a70-93a8-bb02436a5fdf" />

Malliratkaisun tuloste:

<img width="367" height="72" alt="image" src="https://github.com/user-attachments/assets/0865174b-7421-4dbc-a106-b749579a095d" />

## d) Detect single-character XOR

Tehtävänanto:

<img width="578" height="185" alt="image" src="https://github.com/user-attachments/assets/107b587a-f5d9-433b-b9a9-b373919b9042" />

Aloitin lataamalla tiedoston:

    wget https://cryptopals.com/static/challenge-data/4.txt

`cat 4.txt` selaamalla yksi rivi näyttää lyhyemmältä kuin muut:

<img width="503" height="148" alt="image" src="https://github.com/user-attachments/assets/81918ffe-f0f9-4a9a-a9c8-e80e625870a1" />

Muuttuja yleisimmille merkeille:

    common_letters = b"ETAOIN SHRDLU etaoin shrdlu "

Edellisen kohdan koodin korjaaminen tähän osioon sopivaksi meni aivan liian vaikeaksi ja jouduin turvautumaan chatGPT:n apuun tässä kohtaa.

Korjattu koodi:

<img width="1074" height="558" alt="image" src="https://github.com/user-attachments/assets/f36304cf-aa7b-469d-adaa-0d2bd609c04f" />

Tuloste:

<img width="605" height="104" alt="image" src="https://github.com/user-attachments/assets/3be32f2c-17dd-4250-b6be-3acd45720d9e" />

### Lähteet

Karvinen, T. 13.8.2025. Sovellusten hakkerointi. https://terokarvinen.com/sovellusten-hakkerointi/

NCC Group. the cryptopals crypto challenges. https://cryptopals.com/sets/1

Python. binascii — Convert between binary and ASCII. https://docs.python.org/3/library/binascii.html

Python. base64 — Base16, Base32, Base64, Base85 Data Encodings. https://docs.python.org/3/library/base64.html#module-base64

R.D.Tarun. 22.11.2024. Cryptopals Cryptography Solutions - Set 1. https://medium.com/@tarunrd77/cryptopals-cryptography-solutions-set-1-cf4df0132614
