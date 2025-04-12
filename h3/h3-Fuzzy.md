# h3 Fuzzy

## Rauta & HostOS

- Asus X570 ROG Crosshair VIII Dark Hero AM4
- AMD Ryzen 5800X3D
- G.Skill DDR4 2x16gb 3200MHz CL16
- 2x SK hynix Platinum P41 2TB PCIe NVMe Gen4
- Sapphire Radeon RX 7900 XT NITRO+ Vapor-X
- Windows 11 Home 24H2

**Tehtävän aloitusaika 12.4.2025 kello 11:30**

## x) Lue/katso/kuuntele ja tiivistä

### Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf
- Fuff on Joona "joohoi" Hoikkala kehittämät web fuzzer
- Artikkelissa Fuffataan piilotettuja kansiorakenteita, mutta Fuff avulla voi web fuzzata mitä vain ja löytää web-palvelimilta esimerkiksi GET-, POST-parametrejä, hakemistoja, otsikoita yms.
- Fuff on erittäin nopea ja Fuffia voi testata paikallisesti ilman internet-yhteyttä. Tärkeää onkin muistaa, että penetraatiotestauksen teknkiikoiden käyttö tulee suorittaa lain sekä eettisten perjaatteiden puitteissa.

(Karvinen 2023; Nurminen 2024)
### Hoikkala 2023: ffuf README.md

## a) Fuzzzz. Ratkaise dirfuz-1 artikkelista [Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf.](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)
Tämän olinkin jo toisella kurssilla ratkaissut, mutta muistin virkistämiseksi ratkaisin sen uudestaan. Alkuun oli tietenkin tarpeellista ladata itse **Fuff** ja kävin hakemassa sen suoraan GitHub sivustolta.

        wget https://github.com/ffuf/ffuf/releases/download/v2.1.0/ffuf_2.1.0_linux_amd64.tar.gz

![K1](1.png)

Puretaan ja testataan toimivuus.

        tar -xf ffuf_2.1.0_linux_amd64.tar.gz
        ./ffuf

![K2](2.png)

Tämän jälkeen oli tarpeellista asentaa itse Dirfuzt-1 harjoitusympäristö, mikä löytyi suoraa [Tero Karvisen verkkosivuilta](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/) ohjeiden kanssa.

        wget https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/dirfuzt-1
        chmod u+x dirfuzt-1
        ./dirfuzt-0

![K3](3.png)

Ffuf etsintää varten olin ladannut [SecLists](https://github.com/danielmiessler/SecLists) kirjaston nimeltä [common.txt](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/Miessler-etal-2023-SecLists/common.txt) mitä hyödynnetään. Ffuf käyntiin

        ./ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ

![K4](4_1.png)

Tuloksista nähdään, että lähes kaikissa Status on 200 eli ok ja Size 154 määrittää koon biteissä. Mutta yksi vastauksista okin .git 301/41. Mitä jos filtteröidään kaikki 154 bittiset vastaukset pois näkyvistä lisäämällä -fs 154 syötteen perään?

        ./ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ -fs 154

![K5](5.png)

Bingo. Jäljelle jää esimerkiksi wp-admin ja .git. Selaimeen osoitteen perään kyseiset rakenteet ja löydetään pari lippua.

(Karvinen 2023, 2025; Nurminen 2024)
## b) Fuff me. Asenna FuffMe-harjoitusmaali. [Karvinen 2023: Fuffme - Install Web Fuzzing Target on Debian](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian)


## c) - h) Ratkaise ffufme harjoitukset 

### c) Basic Content Discovery

### d) Content Discovery With Recursion

### e) Content Discovery With File Extensions

### f) No 404 Status

### g) Param Mining

### h) Rate Limited

**Tehtävän lopetusaika 12.4.2025 kello XXXX. Aktiivista työskentelyä yhteensä noin X tuntia XX minuuttia.**

## Lähteet
Karvinen T 2025. h3 Fuzzy. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/tunkeutumistestaus/ Luettu 12.4.2025

