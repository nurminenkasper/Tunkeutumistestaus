# h1 Kybertappoketju

## Rauta & HostOS

- Asus X570 ROG Crosshair VIII Dark Hero AM4
- AMD Ryzen 5800X3D
- G.Skill DDR4 2x16gb 3200MHz CL16
- 2x SK hynix Platinum P41 2TB PCIe NVMe Gen4
- Sapphire Radeon RX 7900 XT NITRO+ Vapor-X
- Windows 11 Home 24H2

## x) Lue/katso/kuuntele ja tiivistä

### PODCAST?

### Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain.

### 4.3 Surveying Essential Tools for Active Reconnaissance
- Videossa keskustellaan porttiskannauksesta ja siihen liittyvistä työkaluista
- **Nmap** on vakain ja monipuolisin porttiskanneri, myös suosituin.
- **Masscan** on puolestaan nopein porttiskanneri
- **Udpprotoscanner** on nopea UDP porttien skannaaja
- Videolla näytetään ja kerrotaan runsaasti kaikkien kolmen mainitun skannerin käytön komentoja ja esimerkkejä siitä miten ne toimii
- Videon lopussa käydään läpi EyeWitness työkalun käyttöä, joka auttaa priorisoimaan skannattuja web sivuja.

(Jon Sternstein 2019)

### KKO 2003:36

## a) Asenna Kali virtuaalikoneeseen
Kali Linux tarjoaa suoran VirtualBox asennuksen kuvakkeena, joten valitsin valmiin Pre-built VM vaihtoehdon asennukseksi. Tuorein viimeisin versio 2025.1. Asennuksessa ei mitään ongelmia.

![K1](1.png)

## b) Irrota Kali-virtuaalikone verkosta
Alkuun oli tarpeen selvitellä, että virtuaalikone on lähtökohtaisesti verkossa. Samalla tarkistelin, mikä mahtaa olla käytössä olevan verkkokortin nimi.

![K2](2.png)

Itse käytin verkosta irroittamiseen suoraa terminaalin komentoa, joka poistaa verkkokortin käytöstä. Toki vaihtoehtoisesti sen voi ja myöhemmin teinkin sen suoraan VirtualBoxin asetuksista poistamalla virtuaalinen kaapeli, mikä taitaa olla loppupeleissä parempi vaihtoehto. Testinä vielä pinganus osoitteeseen 8.8.8.8, jotta nähdään irroittautumisen toimineen.

![K3](3.png)
![K3_2](14.png)

Luonnollisesti, kun nostetaan terminaalista yhteys takaisin tai yhdistetään virtuaalinen kaapeli saadaan jälleen verkkoyhteys toimimaan.

![K4](4.png)

## c) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi
Tavoitteena oli skannata oman verkon TCP-portteja. Analysoidaan hieman tarkemmin, mitä syötetty **nmap -T4 -A localhost** tekee.

- **nmap**: nmap valintakomento / ohjelman nimi, että saadaan edes ohjelma käyttöön.
- **-T4**: -T0-5 valinnalla saadaan määritettyä kuinka porttiskannaus ajoitetaan. -T0 ollen hitain vaihtoehto ja -T5 nopein. Tässä skannauksessa käytetty -T4 vaihtoehto on manuaalin mukaan luokkaa "Aggressive scan".
- **-A**: on version detection valinta. Lisäämällä -A syötteeseen, porttiskannaus toimittaa esimerkiksi OS detection, version detection, script scanning ja traceroute lisäykset skannaukseen.
- **localhost**: Toimittaa kohteen virkaa. Tähän voisi vaihtaa minkä tahansa IP osoitteen mitä skannataan, mutta koska kohteena on oma tietokone voidaan käyttää luonnollisesti myös localhost vaihtoehtoa.

Kyseessä on kokonaisuudessaan melko tyypillinen npam porttiskannaus.

![K5](5.png)

Tuloksista voidaan tässä tapauksessa päätellä esimerkiksi seuraavia:

- **Starting Nmap 7.95**: Nmap käynnistyy ja lisäksi kerrottu versionumero sekä ajankohta
- **Host is up**: Vahvistus siitä, että kohde on tavoitettavissa
- **Other addresses for localhost**: Muita kuin localhostin osoitetta ei skannattu
- **All 1000 scanned ports on localhost**: Tässä tapauksessa localhostin kaikki skannatut portit ovat suljettuna, joten mitään tuloksia ei löytynyt.
- **Not shown**: 1000 suljettua tcp porttia ei näytetty
- **Too many fingerprints**: Nmap ei pysty löytämään tarkempia tietoja käyttöjärjestelmästä
- **Network Distances**: 0 hops, koska tapauskessa skannataan omaa verkkoa.
- **OS and Service detection performed**: Tämä on vain muistutus siitä, että OS and Service detection suoritettiin.
- **Nmap done**: Lpuksi vielä raportti siitä, kuinka monta IP-osoitetta skannattiin ja kuinka kauan skannauksessa kesti. Tässä tapauksessa yksi osoite ja 1.90 sekunttia.

## d) Asenna kaksi vapaavalintaista demonia ja skannaa uudelleen

## e) Asenna Metasploitable 2 virtuaalikoneeseen

## f) Tee koneiden välille virtuaaliverkko

## g) Etsi Metasploitable porttiskannaamalla

## h) Porttiskannaa Metasploitable huolellisesti ja kaikki portit


## Työaika
Tehtävien tekemiseen mennyt aika 30.3.2025 kello 16:30 - 19:15

Raportin kirjoitusaika 31.3.2025 kello 09:15 - XXXX

Aktiivista työskentelyä yhteensä noin XX tuntia XX minuuttia.

## Lähteet
https://terokarvinen.com/tunkeutumistestaus/

https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf

Jon Sternstein 2019 https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/

https://finlex.fi/fi/oikeuskaytanto/korkein-oikeus/ennakkopaatokset/2003/36

https://tuomasvalkamo.com/PenTestCourse/week-2/

https://www.kali.org/get-kali/#kali-virtual-machines

https://sourceforge.net/projects/metasploitable/

https://docs.rapid7.com/metasploit/metasploitable-2

