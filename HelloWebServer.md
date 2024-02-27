# x) Artikkelitiivistelmät 

# Name-based Virtual Host Support

- IP-pohjainen virtuaali-isäntä käyttää yhteyden IP-osoitteita oikean virtuaali-isännän määrittämiseen ja tätä varten jokaisella samassa yhteydessä olevalla isännällä on oltava erillinen IP-osoite, jotta se voisi palvella oikeaa isäntää.
- Nimipohjaisessa virtuaali-isännöinnissä palvelin luottaa siihen, että käyttäjä määrittää itse kunkin isännän nimen HTTP-otsikoinnissa ja tämän myötä moni eri isäntä voi jakaa keskenään saman IP-soitteen.
- Nimipohjainen virtuaali-isännöinti on helpompaa veraattuna IP-pohjaiseen isännöintiin, koska käyttäjän tarvitsee vain määrittää DNS-palvelin yhdistämään jokainen eri virtuaali-isäntä oikeaan IP-soitteeseen nimen perusteella ja sen jälkeen määrittää Apachen HTTP-palvelin tunnistamaan eri isännät.
- Nimipohjainen virtuaali-isännöinti perustuu IP-pohjaisen virtuaalipalvelimen valinta-algoritmiin, joka tarkoittaa sitä, että oikea palvelin etsitään vain virtuaalisten isäntien väliltä IP-osoitteen mukaan.
- Palvelin valitsee sopivan nimipohjaisen virtuaali-isännän siten, että nimipohjainen IP-resoluutio valitsee vain sopivimman virtuaali-isännän parhaan IP-osoitteen mukaan.
- Kun pyyntö tulee palvelimelle, palvelin löytää parhaiten vastaavan <VirtualHost>-argumentin perustuen pyynnön lähettäjän IP-osoitteeseen ja sen käyttämään porttiin.
- Jos useampi kuin yksi isäntä jakaa saman IP-osoitteen ja portin, Apache vertaa edelleen niiden ServerName- ja ServerAlias-tietoja palvelimen nimeen.
- Jos vastaavaa ServerName- tai ServerAlias-nimeä ei löydy, käytetään ensimmäistä vastaavaa palvelinta
- Kun nimipohjaista virtuaali-isäntiä aletaan luomaan, ensimmäiseksi luodaan <VirtualHost>-lohko jokaiselle eri isännälle ja sen sisälle vähintään ServerName-käskyn, joka määrittää mitä isäntää palvellaan, ja sen lisäksi DocumentRoot-käskyn, joka näyttää, missä tiedostojärjestelmässä kyseisen isännän tiedot sijaitsevat
- Myös ServerAlias-komento on hyvä olla, jos haluaa että palvelin on käytettävissä usealla eri nimellä

# Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address

Tässä artikkelissa kerrotaan, kuinka voidaan luoda monta eri web-sivua saman IP-osoitteen alle. Seuraavaksi kerrotaan askel askeleelta, miten se käytännössä tehdään.
- Ensimmäiseksi asennetaan Apachen web-serveri komennolla:
    ```$ sudo apt-get -y install apache2```
- Sen jälkeen vaihdetaan oletuksena oleva web-osoite komennolla:
    ```echo "Default"|sudo paakayttaja /var/www/html/index.html```
- Seuraavaksi lisätään nimipohjainen virtuaali-isäntä seuraavilla komennoilla:
-   ```sudoedit /etc/apache2/sites-available/esimerkki.example.com.conf```
-   ```$ cat /etc/apache2/sites-available/esimerkki.example.com.conf```
- Tämän jälkeen sinulle avautuu koodieditori, jossa syötetään seuraavat komennot:
-   ```
        <VirtualHost *:80>
         ServerName esimerkki.example.com
         ServerAlias www.esimerkki.example.com
         DocumentRoot /home/xubuntu/publicsites/esimerkki.example.com
         <Directory /home/xubuntu/publicsites/esimerkki.example.com>
           Require all granted
         </Directory>
        </VirtualHost>
     ```
- Tämän jälkeen syötä komennot:
-   ```$ sudo a2ensite esimerkki.example.com```
-   ```$ sudo systemctl restart apache2```
- Seuraavaksi luodaan web-sivu normaalin käyttäjän oikeuksilla komennoilla:
-   ```$ mkdir -p /home/xubuntu/publicsites/esimerkki.example.com/```
-   ```$ echo pyora > /home/xubuntu/publicsites/pyora.example.com/index.html```
- Seuraavaksi testataan web-sivun toimivuutta komennoilla:
-    ```$ curl -H 'Host: esimerkki.example.com' localhost```
-    ```$ curl localhost```
- Oikeassa elämässä web-sivun nimen voi vuokrata joltakin palvelutarjoajalta, mutta tässä tapauksessa nimitiedot syötetään itse komennoilla
-    ```$ sudoedit /etc/hosts```
-    ```
     $ cat /etc/hosts
     127.0.0.1 localhost
     127.0.1.1 xubuntu
     127.0.0.1 esimerkki.example.com
     # ...
     ```
- Tämän jälkeen sivustojen toimivuutta voi kokeilla selaimessa syöttämällä URL:n esimerkiksi:
-     http://localhost/
-     http://esimerkki.example.com

# a) Web-palvelimen luonti
Olin jo valmiiksi luonut itselleni Apache web-palvelimen ja se tapahtui siten, että syötin komentorivillä komennon
    ```$ sudo apt-get -y install apache2```
Tämän jälkeen kokeilin selaimella vastaako palvelin osoitteessa http://localhost ja minulle avautui seuraavanlainen näkymä:
![Screenshot_2024-02-03_15-17-21.png](https://github.com/JukkaLak/h3HelloWebServer/blob/main/Screenshot_2024-02-03_15-17-21.png)

Tämä siis tarkoitti sitä, että palvelin toimii. Tämän jälkeen poistin esimerkkisivun syöttämällä komennon 
    ``echo "Default"|sudo tee /var/www/html/index.html``
Tämän jälkeen selainnäkymä näytti tältä:
![Screenshot_2024-02-04_11-28-00.png](https://github.com/JukkaLak/h3HelloWebServer/blob/main/Screenshot_2024-02-04_11-28-00.png)

# b) Lokin analysointi
Ensiksi katsoin access.log tiedot komennolla 
    ``sudo tail /var/log/apache2/access.log``
Tämän jälkeen komentoriviin tuli seuraavat tiedot:
![Screenshot_2024-02-04_14-40-06.png](https://github.com/JukkaLak/h3HelloWebServer/blob/main/Screenshot_2024-02-04_14-40-06.png)
Tästä listauksesta tulee ilmi se, että palvelimelle on tehty GET-pyyntöjä, eli palvelinta on kutsuttu osoitteesta http://localhost ja se on vastannut kutsuihin. 
Seuraavaksi katsoin error.log -tiedot komennolla
    ``sudo tail /var/log/apache2/error.log``
Tämän jälkeen tulostui seuraavat tiedot:
![Screenshot_2024-02-04_14-52-33.png](https://github.com/JukkaLak/h3HelloWebServer/blob/main/Screenshot_2024-02-04_14-52-33.png)

Tästä käy ilmi, että mitään virhetilanteita ei ole ehtinyt tulla ja palvelin toimii normaalisti.

# c) Uuden nimipohjaisen virtuaali-isännän asennus
Aloin asentamaan uutta nimipohjaista virtuaali-isäntää seuraamalla sivuston https://www.linuxcapable.com/how-to-install-apache-on-debian-linux/ tutoriaalia. Aluksi loin uuden kansion komennolla 
    ``sudo mkdir /var/www/esimerkki.example.com``
Tämän jälkeen syötin komennot
    ``sudo chown -R $USER:$USER /var/www/esimerkki.example.com`` ja
    ``sudo chmod -R 755 /var/www/esimerkki.example.com``
Tämän jälkeen muokkasin sivuston etusivunäkymää micro-editorissa ja kirjoitin sinne seuraavanlaista html-koodia:
![Screenshot_2024-02-04_15-23-09.png](https://github.com/JukkaLak/h3HelloWebServer/blob/main/Screenshot_2024-02-04_15-23-09.png)

Tämän jälkeen avasin jälleen micro-editorin komennolla 
    ```$ sudo micro /etc/apache2/sites-available/esimerkki.example.com.conf```
Syötin editorissa aluksi tällaiset tiedot:
![Screenshot_2024-02-04_15-45-32.png](https://github.com/JukkaLak/h3HelloWebServer/blob/main/Screenshot_2024-02-04_15-45-32.png)

Tämän jälkeen yritin ajaa komennon 
    ```sudo a2ensite esimerkki.example.com```ja käynnistää Apachen uudelleen ja kun testasin sivuston toimintaa, se toimi aluksi pelkästään localhostissa mutta ei osoitteessa http://esimerkki.example.com ja yritin vielä korjata tilannetta, mutta lopulta sivu ei toiminut ollenkaan, enkä saanut tätä ongelmaa ratkaistua.
    







