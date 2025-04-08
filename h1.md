# h1 Kybertappoketju

x) 
1. Darknet Diaries jakso 133: I'm the real Connor, May 2, 2023
   Jaksossa perehdyttiin Conorin tarinaan kenen identiteetti varastettiin. Hän sai sähköpostia, että joku esiintyy hänenä etätyöhaasttatteluissa. Hakkerit käyttivät Conorin identiteetiä saadaakseen etätöitä, koska hänellä oli vakuuttava github historia. Hakkerit olivat tehneet CV:n conorin tietojen perusteella ja se tiedot täsmästi todella hyvin.
2. Raportti käsittelee Intrusion kill chain mallia APT-uhkia vastaan. Raportissa on kolme esimerkkiä hyökkäyksistä Lockheed Martinia kohtaan mitkä tapahtuivat kaikki 2009. He esittelevät miten hyökkäykset estettiin ja miten mallia voidaan jatkossa käyttää hyväksi uhkien estämistä varten.

3. Videoilla selitettiin mitä Active Reconnaissance tarkoittaa. Videoilla myös tutustuttii suosittuihin työkaluihin, millä voit kerätä tietoa ennen tunkeutumista. Videoilla myös opetettii hieman miten nmappia käytetään. 

4. Sivulla on tietoa rikostapauksesta, jossa henkilö oli porttiskannannut Osuuspankin tietojärjestelmiä vuonna 1998. Tekijä joutui maksamaan yhteensä 75000 markkaaa korvauksia.

a) Minulla oli jo virtualbox asennettuna, piti vain ladata Kali Linux ISO image ja laittaa se virtual boxiin. Meni normaalisti Kali Install Wizardin läpi. En kohdannut mitään ongelmia asennuksessa.

b)  Käytin komentoa sudo ifconfig eth0 down. Jonka jälkeen ping 8.8.8.8 palautti: Network is unreachable

c) nmap : Työkalusovellus mitä komennon kutsumiseen käytetään.
   -T4: Timing template. tällä parametrilla voidaan säätää kuinka "aggressiivinen" skannaus on. Mitä isompi numero sitä nopeampi. [Lähde](https://nmap.org/book/performance-timing-templates.html)
   -A: Tämä parametri ottaa käyttöön käyttöjärjestelmä tunnistuksen. [Lähde](https://nmap.org/book/man-os-detection.html)
   localhost: Viimeisenä parametrina tulee skannattava kohde. Tässä tapauksessa se on localhost eli paikallinen kone.
Runnasin komennon, se kesti 2.19 sekuntia. Kaikki portit olivat kiinni, koska tietokone ei ollut verkossa. -A ei tunnistanut käyttöjärjestelmää (Too many fingerprints match this host to give specific OS details).

d) Asensin demo sovellukset apache jaa openssh serverin. [Apache kali](https://www.kali.org/tools/apache2/) [OpenSSH](https://www.kali.org/tools/openssh/). Käynnistin molemmat komennolla enable ja sen jälkeen start. 
Käynnistin uudestaan porttiskannauksen, ja nyt tulokseksi tuli kaksi avointaa porttia. OpenSSH on 22/TCP ja Apache 80/TCP. Skannaus kesti nyt 8.45 sekuntia.

e) Asensin metasploitable 2 virtuaalikoneeseen. Latasin sen sivulta [SourceForce](https://sourceforge.net/projects/metasploitable/). Asennus oli helppo. Loin uuden virtualbox instanssin ja valitsin asetuksista existing hard drive ja pistin siihen lataamani metasploitable.vmdk tiedoston.

f) 1. Aloitin vaihtamalla molempien virtuaalikoneiden network asetukset siten, että laitoin Kali Linux koneeseen kaksi adapteria ensimmäin oli NAT ja toinen Host only adapter. Metasploitable 2 koneeseen laitoin asetukseksi Host-only adapter. Näin saan Kali koneen verkkoon, mutta sen voi kytkeä pois päältä. Kokeilin pingata Kali koneella metasploit konetta ja sain vastauksen.

g) Juoksin nmap -sn 192.168.73.0/24 komennon ja se palautti 4 eri hostia 27.99 sekunnissa. Kaksi niistä oli virtuualikoneita. Kokeilin näitä kumpaakin weppiselaimella ja 192.168.73.3 oli oikea IP osoite. 

f) Suoritin porttiskannauksen ja sain paljon erilaisia tuloksia. Käytin [tätä](https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide#backdoors) sivua avuksi ymmärtämään mitkä portit voivat sisältää takaovia tai haavoittuvaisuuksia. Portissa 21 on käytössä suosittu FTP serveri, joka sisältää takaportin. Lähettämällä hymiön :) käyttäjänimeksi se avaa shelling portissa 6200.  
Portissa 6667 juoksee UnrealIRCD IRC daemon. Lähettämällä sinne kirjaimet AB se käynnistää shellin. 
