# h5 Koko juttu
## a) Uuden virtuaalikoneen luonti ja web-palvelimen asennus
- Aloitin uuden tyhjän virtuaalikoneen luomisen antamalla sille nimen, lisäämällä Debian 12 ISO Imagen, jota olen käyttänyt myös aiemmin luomissani virtuaalikoneissani, ja versioksi valitsin
Debianin 64 bittisen käyttöliittymän. Muistin määräksi asetin 4000 MB:ta ja loin koneelle 60 GB:n virtuaalikovalevyn. Kun olin nämä asetukset tehnyt, käynnistin koneen.
- Kun kone oli käynnistynyt, avasin Debian installerin Debianin asentamista varten. Asetin koneen sijainniksi Helsingin, asetin suomenkielisen näppäimistön, valitsin Partitions valikossa
"Erase disk"-vaihtoehdon, lisäsin käyttäjätiedot ja salasanan ja annoin koneelle nimen. Tämän jälkeen tarkistin vielä yhteenveto-osiossa tekemäni asetukset ja aloitin asennuksen. Asennuksessa
meni noin kymmenen minuuttia ja kun se oli valmis, käynnistin virtuaalikoneen uudelleen. Uudelleen käynnistämisessä meni muutama minuutti.
- Uudelleenkäynnistyksen jälkeen avasin terminaalin ja aloin tekemään alkuasetuksia. Ensimmäiseksi hain tiedot saatavilla olevista päivityksistä komennolla
    ```$ sudo apt-get update```.
Sitten asensin kaikki tärkeimmät pävitykset komennolla
    ```$ sudo apt-get -y dist-upgrade```.
Kun päivitykset oli asennettu, asensin vielä palomuurin komennolla
    ```$ sudo apt-get -y install ufw``` ja kytkin sen päälle komennolla
    ```sudo ufw enable```.
- Seuraavaksi asensi Apache-weppipalvelimen komennolla
    ```$ sudo apt-get -y install apache2``` ja sen jälkeen asensin SSH-etähallintapalvelimen komennolla
    ```$ sudo apt-get install openssh-server```.
- Vuokrasin tätä tehtävää varten kokonaan uuden pilvipalvelimen DigitalOceanilta, koska olin unohtanut aiemmin tekemäni palvelimen salasanan enkä päässyt kirjautumaan sinne. Vuokrasin myös itselleni uuden domain-nimen Name.com-palvelusta ja hyödynsin tähän GitHub Education paketin krediittejä ja valitsin domain-nimeksi jukkalakkala.live.
- Uuteen palvelimeeni tein samat asetukset mitä olin aiemmin luomaani palvelimeen asettanut eli palvelimen sijainniksi valitsin Amsterdamin, käyttöliittymäksi 64 bittisen Deabian 12:sta, lisäsin tavallisen 1 GB:n prosessorin ja 25 GB:n SSD-kovalevyn. Sitten asetin juuren salasanan ja asetin hostnamen. Kun palvelin oli luotu asetin Name.comissa luomani domainin osoittamaan uuden palvelimen IP-osoitteeseen.
- Seuraavaksi avasin ssh-yhteyden uuteen virtuaalipalvelimeeni sen IP-osoitteen mukaan komennolla
    ```$ ssh root@161.35.148.7```   ja asensin sen jälkeen palomuurin sekä tein siihen reiän porttia varten komennolla
    ```$ sudo ufw allow 22/tcp``` ja laitoin palomuurin päälle.
- Seuraavaksi loin virtuaalipalvelimelle uuden käyttäjän komennolla
    ```$ sudo adduser jukka```, asetin käyttäjälle salasanan ja nimitiedot ja annoin uudelle käyttäjälle pääkäyttäjäoikeudet komennolla
    ```$ sudo adduser jukka sudo```.
- Sitten kokeilin ottaa uudessa terminaalissa ssh-yhteyden virtuaalipalvelimeen luomillani käyttäjätunnuksilla komennolla
    ```$ ssh jukka@161.35.148.7```, syötin salasanan ja yhteyden muodostus onnistui. Lopuksi lukitsin juuren komennolla
    ```$ sudo usermod --lock root```.
- Seuraavaksi asensin Apachen myös virtuaalipalvelimelleni ja tein palomuuriin uuden reiän komennolla
    ```$ sudo ufw allow 80/tcp``` ja tämän jälkeen testasin selaimessa, vastaako palvelin osoitteessa jukkalakkala.live ja minulle avutui Apachen testisivu eli palvelin toimi.
![Näyttökuva (46).png](https://github.com/JukkaLak/h5Kokojuttu/blob/main/N%C3%A4ytt%C3%B6kuva%20(46).png)
- Sitten korvasin testisivun syöttämällä komennon
    ```$echo Hello|sudo tee /var/www/html/index.html```
![Näyttökuva (47).png](https://github.com/JukkaLak/h5Kokojuttu/blob/main/N%C3%A4ytt%C3%B6kuva%20(47).png)
- Seuraavaksi aloin muokkaamaan etusivua name based virtual host -tekniikkaa hyödyntäen. Aluksi loin käyttäjäni kotihakemistoon uuden publicsites-nimisen kansion.
![Screenshot_2024-02-25_15-38-56.png](https://github.com/JukkaLak/h5Kokojuttu/blob/main/Screenshot_2024-02-25_15-38-56.png)
- Seuraavaksi aloin luomaan uutta nimipohjaista virtuaali-isäntää nimellä jukka.example.com seuraavalla tavalla:
![Screenshot_2024-02-25_15-49-57.png](https://github.com/JukkaLak/h5Kokojuttu/blob/main/Screenshot_2024-02-25_15-49-57.png)
![Screenshot_2024-02-25_15-46-49.png](https://github.com/JukkaLak/h5Kokojuttu/blob/main/Screenshot_2024-02-25_15-46-49.png)
- Tämän jälkeen laitoin uuden sivun päälle komennolla
    ```$ sudo a2ensite jukka.example.com``` ja käynnistin Apachen uudelleen. Uudelleenkäynnistyksen yhteydessä minulta vaadittiin autentikaatiota pilvipalvelimeni käyttäjältä
![Screenshot_2024-02-25_16-07-49.png](https://github.com/JukkaLak/h5Kokojuttu/blob/main/Screenshot_2024-02-25_16-07-49.png)
- Seuraavaksi laitoin oletuksena päällä olleen sivun "000-default.conf" pois päältä komennolla
    ```sudo a2dissite 000-default.conf``` ja käynnistin Apachen jälleen uudestaan.
- Tämän jälkeen minulle tuli jälleen kerran ylitsepääsemättömiä vaikeuksia saada sivu toimimaan, enkä osannut näitä ongelmia ratkaista, joten tehtävä jäi kesken.





