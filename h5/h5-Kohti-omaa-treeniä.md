# h5 Kohti omaa treeniä

## Rauta & HostOS

- Asus X570 ROG Crosshair VIII Dark Hero AM4
- AMD Ryzen 5800X3D
- G.Skill DDR4 2x16gb 3200MHz CL16
- 2x SK hynix Platinum P41 2TB PCIe NVMe Gen4
- Sapphire Radeon RX 7900 XT NITRO+ Vapor-X
- Windows 11 Home 24H2

**Tehtävän aloitusaika 2.5.2025 kello 08:00**

## x) Lue/katso/kuuntele ja tiivistä

### Karvinen 2025: Start Your Research with a Review Article

### Review

## HackTheBox
Tunnarit oli tarpeellista tehdä ja viritellä OpenVPN kautta VPN yhteys pystyyn. HackTheBox ohjeet oli selkeät, latailla sopiva vaihtoehto ja suorittaa se OpenVPN.

![K1](1.png)

Kuvasta poiketen, käytin UDP valintaa. Lataamisen jälkeen terminaali auki ja suorittaminen tapahtui `sudo openvpn starting_point_Hulluduunari.ovpn`

![K2](2.png)

Ja hommahan lähti hyvin toimimaan.

![K3](3.png)

Pingasin vielä testiksi VPN Serveriä, että homma rokkaa.

![K4](4.png)

Samaan syssyyn suorittelin myös muutaman tehtävän ennen Dancing, niin sai vähän tuntumaa miten homma toimii. Näistä ei sen tarkempaa raportointia.

![K5](5.png)

(Ryan Gordon 2025)
## a) HTB Dancing
Tehtävän aloittamista varten pystyyn kohdekone.

![K6](6.png)

Ja tietenkin testit perään, että kone ylipäätänsä toimii.

![K7](7.png)

#### Task 1: What does the 3-letter acronym SMB stand for?

Lyhyenne SMB vastaus oli Server Message Block.

![K8](8.png)

#### Task 2: What port does SMB use to operate at?

SMB operoi tyypillisesti TCP/445.

![K9](9.png)

#### Task 3: What is the service name for port 445 that came up in our Nmap scan?

Seuraavassa tehtävässä olikin tarpeellsita selvitellä, millainen service 445 portista löytyy. Ei muuta kuin skannaamaan nmapilla kohdekoneen osoitetta: `sudo nmap 10.129.61.53`

![K11](11.png)

Ja sieltähän me löydetään portista 445 **microsoft-ds**

![K12](12.png)

#### Task 4: What is the 'flag' or 'switch' that we can use with the smbclient utility to 'list' the available shares on Dancing?

Ei muuta kuin terminaalista smbclient käyttöohjeet auki, `smbclient -help` ei varsinaisesti ollut oikea vaihtoehto, mutta se listasin silti tarpeelliset tiedot ja nähdäänkin miten **-L** on kyseinen listaus vaihtoehto.

![K14](14.png)
![K15](15.png)

#### Task 5: How many shares are there on Dancing?

Edellisessä tehtävässä opittiinkin se, miten listaus toteutaan niin kokeillaan myös käytännössä `smbclient -L 10.129.61.53`

![K17](17.png)

Ja vaikka salasana ei ollut tiedossa, saatiin kuitenkin tietoon se kuinka monta sharesia sielä on, eli **4**

![K18](18.png)

#### Task 6: What is the name of the share we are able to access in the end with a blank password? 

Mihin vaihtoehdoista pääsee sisälle pelkällä tyhjällä salasanalla? Smbclient ei ollut kovin tuttu, joten lähdin kokeilemaan miten kirjautua sisään.

![K19](19.png)

Nopeasti selvisi, että \ merkkejä puuttuu, mutta en oikeen saanut help vaihtoehdoista vastausta miten kirjautua sisään niin googlettamalla löytyi [Mediumin artikkeli](https://medium.com/@ibo1916a/smbclient-command-2803de274e46) mistä löytyi oikea tapa kirjautua sisään.

![K20](20.png)

Ja kuten nähdään IPC$ pääsi sisään ilman salasanaa. Käydäänpäs antamassa se vastauksena.

![K21](21.png)

Ei ollutkaan se. Testiksi vielä pääseekö WorkSharesille ja pääseehän sinnekkin ilman salasanaa, joten vastaus oli lopulta **WorkShares**!

![K22](22.png)
![K23](23.png)

#### Task 7: What is the command we can use within the SMB shell to download the files we find? 

Nyt kun meillä oli yhteys auki smb, kokeilin ensin tietenkin help komentoa ja sieltähän löytyi vastaus tehtävään eli **get**

![K25](25.png)
![K26](26.png)

#### Submit root flag

Lähdin tutkimaan tarkemmin, mitä kansiorakenteista löytyy `ls` komennolla

![K28](28.png)

Löytyi kaksi käyttäjää ja tutkin ensin mitä Amy.J alta löytyy.

![K29](29.png)

worknotes.txt voisi olla hyvin lippu, joten ladataan se talteen `get` komennolla.

![K30](30.png)

Tarkistetaan vielä, mitä Jmaes.P alta löytyy.

![K31](31.png)

Ahaa, lippu! Otetaan talteen sekin.

![K32](32.png)

Ja kun tarkastellaan niitä oman koneen puolella, nähdään ettei worknotes sisällä oikeastaan mitään järkevää, mutta puolestaan flag.txt sisältää tehtävän lipun.

![K33](33.png)

Ja näin ollen koko Dancing saatiin suoritettua!

![K34](34.png)

(Ibrahim Atasoy 2023; SMB Wikipedia)
## b) HTB Responder
Homma alkuun käynnistelemällä kohdekone Responder.

![K35](35.png)

#### Task 1: When visiting the web service using the IP address, what is the domain that we are being redirected to? 

Lähdin selvittämään tehtävää ihan puhtaasti suunnistamalla Firefoxilla IP osoitteeseen. Vastaukseksi saatiin **unika.htb**, mutta sivulla ei näkynyt mitään.

![K37](37.png)
![K38](38.png)

#### Task 2: Which scripting language is being used on the server to generate webpages?

En oikein saanut alkuun kiinni, että miten pitäisi selvittää scripting language, jos en saanut koko sivulla mitään näkymään. Aikaisempia tehtäviä myötäillen, ajattelin porttiskannata osoittteen ja katsoa mitä löytyy.

![K39](39.png)

Okei, Apache toki kiinnitti huomion. Ei kuitenkaan ollut minkään sortin käsitystä, miksi sivustolla ei näy mitään. Lähdin tarkastelemaan ensin googlea, mutta lopulta ihan ohjetta miten tehtävä tehdään ja sielähän olikin mainita, että kyseesä on Name-Based Virtual Hosting ja /etc/hosts on asetettava domaini niin hostname toimii oikein.

![K40](40.png)
![K41](41.png)

Nyt kun saatiin sivulle jotain näkyviin, alkuun tarkastelin aika pitkään eri elementtejä ihan Firefoxin omalla Inspector toolilla.

![K42](42.png)

Tässä olinkin jumissa pidemmän tovin ja mitään ei varsinaisesti löytynyt. Klikkailin eri vaihtoehtoja läpi ja oikeastaan puhtaasti sattumalla huomasin kun vaihtaa sivuston kieltä tapahtuu ainoa muutos osoiterivilläkin.

![K43](43.png)

Ja sieltähän tosissaan pilkistää **php** vastaukseksi

![K44](44.png)

#### Task 3: What is the name of the URL parameter which is used to load different language versions of the webpage? 

Tämän vastaus löytyy luonnollisesti samasta URL osoitteesta kuin aikaisempi tehtävä, eli kyseessä on **page**

![K45](45.png)

#### Task 4: Which of the following values for the `page` parameter would be an example of exploiting a Local File Include (LFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe" 

Tehtävänannosta herätteli kyllä heti kelloja kyseinen "path traversal" tyyppinen näkymä. Lähdin kuitenkin tutkimaan, mikä tarkalleen on LFI vulnerability ja löysin artikkeliksi [Vaadatan blog postauksen](https://www.vaadata.com/blog/exploiting-an-lfi-local-file-inclusion-vulnerability-and-security-tips/) aiheesta. Eli LFI tavalla hyökkääjä pääsee kirjoittamaan, tässä tapauksessa page= perään omaa haluamaansa tekstiä ohjelmaan. Syötehän mitä haetaan annetaan jo perjaatteessa tehtävänannossa, joten kokeillaan `page=../../../../../../../../windows/system32/drivers/etc/hosts`

![K47](47.png)

Näinhän se toimii ja olotettavasti vastaus tehtäväänkin pitäisi olla sama, mutta ilman page=.

![K48](48.png)

#### Task 5: Which of the following values for the `page` parameter would be an example of exploiting a Remote File Include (RFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe"

Alotin tehtävään tutustumisen saman sivun [blog postauksella](https://www.vaadata.com/blog/what-is-rfi-remote-file-inclusion-exploitations-and-security-tips/) kuin edellisessä LFI tehtävässä oli. Kun LFI hyökkäys on mahdollinen, voidaan RFI hyökkäys teoteuttaa. Käytännössä hyökkääjällä on omalla sivulla .php tiedosto mistä hyökkäys lopulta suoritetaan. Kun LFI on todettu toimivaksi, voidaan RFI hyökkäys toteuttaa suoraan hyökkääjän omalta palvelimelta. PHP tiedostossa voi olla vaikka systeemin komentoja mitä suoritetaan tai muuta haitallista. Testasin tätä teoriassa `//10.10.14.6/somefile` mikä oli annettu jo tehtävänannossakin.

![K50](50.png)

Tehtävä oli ehkä oikein? Itselle jäi se kuitenkin hieman epäselväksi, joten tarkastelin asiaa vielä läpijuoksuohjeesta ja sielä olikin sivuttu tarkemmin [include](https://www.php.net/manual/en/function.include.php) liittyviä riskejä. Vastaus tehtävään oli siis lopulta: **//10.10.14.6/somefile**

![K52](52.png)

#### Task 6: What does NTLM stand for? 

NTLM on lyhenne New Technology Lan Manager.

#### Which flag do we use in the Responder utility to specify the network interface? 

**Tehtävän lopetusaika X.2.2025 kello XX:XX. Aktiivista työskentelyä yhteensä noin X tuntia XX minuuttia.**

## Lähteet
Karvinen T 2025. h5 Kohti omaa treeniä. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/tunkeutumistestaus/ Luettu 2.5.2025

Ryan Gordon 2025. HackTheBox - Introduction to Starting Point. Luettavissa: https://help.hackthebox.com/en/articles/6007919-introduction-to-starting-point#h_04938711ab Luettu 2.5.2025

Wikipedia 2025. Server Message Block. Luettavissa: https://en.wikipedia.org/wiki/Server_Message_Block Luettu 2.5.2025

Ibrahim Atasoy 2023. Smbclient command. Luettavissa: https://medium.com/@ibo1916a/smbclient-command-2803de274e46 Luettu 2.5.2025




TÄMÄN JÄLKEEN KOHTAAN B.

Maha Yassin 2024. How to Identify the Programming Language of a Website. Luettavissa: https://profiletree.com/how-to-identify-the-programming-language/ Luettu 2.5.2025

Arnaud Pascal 2023. Exploiting an LFI (Local File Inclusion) Vulnerability and Security Tips. Luettavissa: https://www.vaadata.com/blog/exploiting-an-lfi-local-file-inclusion-vulnerability-and-security-tips/ Luettu 2.5.2025

Renaud Cayol. What is RFI? Remote File Inclusion Exploitations and Security Tips. Luettavissa: https://www.vaadata.com/blog/what-is-rfi-remote-file-inclusion-exploitations-and-security-tips/ Luettu 2.5.2025

PHP Documents. Luettavissa: https://www.php.net/manual/en/function.include.php Luettu 2.5.2025

NTLM Wikipedia. Luettavissa: https://en.wikipedia.org/wiki/NTLM Luettu 2.5.2025





https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion

https://www.kali.org/tools/responder/

https://terokarvinen.com/2022/cracking-passwords-with-hashcat/

https://github.com/nurminenkasper/Tunkeutumistestaus/blob/main/h4/h4-Levi%C3%A4m%C3%A4ss%C3%A4.md

https://www.openwall.com/john/doc/

https://www.kali.org/tools/evil-winrm/

https://www.reddit.com/r/windows/comments/jfvpxq/linux_cat_alternative_for_cmd_on_windows/


