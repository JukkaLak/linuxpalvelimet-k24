# h6 DJ Ango

## x) Artikkelitiivistelmät

### Django 4 Instant Customer Database Tutorial
- Uuden asiakastietokanna luominen Django 4:n avulla aloitetaan lataamalla uusi virtuaaliympäristö ja sinne uusimmat Python-paketit:
     ```$ sudo apt-get -y install virtualenv```
- Sitten luodaan /env-kansio, joka sisältää uusimmat paketit, tässä tapauksessa käytetään Python 3:sta:
    ```$ virtualenv --system-site-packages -p python3 env/```
- Seuraavaksi laitetaan virtuaalityöympäristö päälle:
    ```$ source env/bin/activate```
- Python-asennukset on tehtävä nimenomaan virtuaaliympäristöön ja asennuksia ei saa missään tapauksessa tehdä sudo-oikeuksilla:
    ```$ which pip /home/kayttaja/env/bin/pip```
- Yksittäinen Python paketti lisätään tekstitiedostoon ja lopuksi testitiedosto ladataan pip-komennolla:
    ```$ micro requirements.txt # "django"```,
    ```$ cat requirements.txt```,
    ```$ pip install -r requirements.txt```
- Uusi web-projekti aloitetaan syöttämällä komento
    ```$ django-admin startproject yourproject```
- Web-sivuston toimivuutta voidaan testata navigoimalla projektin kansioon ja laittamalla sivusto päälle komennolla
    ```$ ./manage.py runserver```. Tämän jälkeen selaimessa pitäisi näkyä Djangon testisivu osoitteessa http://127.0.0.1:8000/
- Ylläpitäjän rajapinta luodaan päivittämällä ensin tietokannat komennoilla
    ```$ ./manage.py makemigrations``` ja
    ```$ ./manage.py migrate``` ja sen jälkeen luodaan käyttäjä:
    ```$ sudo apt-get install pwgen```,
    ```$ pwgen -s 20 1 # luodaan salasana```,
    ```$ ./manage.py createsuperuser```
- Uuden asiakastietokanna luominen aloitetaan luomalla /crm-kansio:
    ```$ ./manage.py startapp crm```
- Lisätään kansio INSTALLED_APPS-osioon settings.py-kansiossa:
    ```$ micro yourproject/settings.py```
- ```
  INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'crm', # lisää crm-kansio tähän
  ]                             
 - Sitten luodaan uusi tietokantataulu:
    ```$ micro crm/models.py```
- ```
  from django.db import models

  class Customer(models.Model):
    name = models.CharField(max_length=300)

- Jotta tietokanna voi nähdä ylläpitäjän oikeuksilla, ylläpitäjä täytyy rekisteröidä:
    ```$ micro crm/admin.py```
-    ```
     from django.contrib import admin
     from . import models

     admin.site.register(models.Customer)

- Aluksi tietokanta näyttää asiakaslistan numeroina, mutta asiakkaan nimet voidaan laittaa näkyville muokkaamalla models.py-tiedostossa olevaa Customer luokkaa:
    ```
    from django.db import models

    class Customer(models.Model):
      name = models.CharField(max_length=160)

      def __str__(self):	 
        return self.name	# tämä toiminto muuttaa asiakkaan arvon merkkijonoksi

### Deploy Django 4 - Production Install
- Staattinen testisivu luodaan luomalla juurihakemistoon uusi hakemistopolku testisisältöä varten:
    ```
       $ cd
       $ mkdir -p publicwsgi/yoursite/static/
       $ echo "Static site"|tee publicwsgi/yoursite/static/index.html
- Lisätään uusi VirtualHost:
    ```$ sudoedit /etc/apache2/sites-available/yoursite.conf```
-    ```
     <VirtualHost *:80>
	      Alias /static/ /home/user/publicwsgi/yoursite/static/
	      <Directory /home/user/publicwsgi/yoursite/static/>
		      Require all granted
	      </Directory>
      </VirtualHost>
- Laitetaan uusi sivu päälle ja laitetaan oletuksena oleva testisivu pois päältä ja käynnistetään Apache uudelleen
- Staattisten tiedostojen toimivuutta voidaan testata menemällä selaimella osoitteeseen http://localhost/static/ tai komentorivillä komennolla
    ```$ curl http://localhost/static/```
- Seuraavaksi luodaan uusi VirtualEnv-ympäristö ja luodaan sille uusi hakemisto publicwsgi-kansioon:
    ```
    $ sudo apt-get -y install virtualenv

    $ cd
    $ cd publicwsgi/

    $ virtualenv -p python3 --system-site-packages env
- Ladataan uusin Django versio laittamalla uusi virtuaaliympäristö päälle:
    ```$ source env/bin/activate```
- Djangon lataamista varten luodaan requirements.txt-tiedosto:
    ```
    $ micro requirements.txt

    django
- Seuraavaksi ladataan tiedosto:
    ```$ pip install -r requirements.txt```
- Seuraavaksi luodaan uusi Django-projekti:
    ```$ django-admin startproject yourproject```
- Yksinkertaisesta Django-projektista poiketen tässä esimerkissä on luotava kolme uutta hakemistopolkua, jotka ovat Django-projektin päähakemisto (TDIR), joka sisältää manage.py-tiedoston,
polku wsgi.py- tiedostoon (TWSGI) ja polku virtualenvin site-packages -tiedostoon (TVENV). Nämä polut kannattaa määritellä VirtualHost-tiedostoon.
- Seuraavaksi pitää ladata Apache WSGI-moduuli, jotta Apache voi tunnistaa, mitä WSGI-komennot tarkoittavat:
    ```$ sudo apt-get -y install libapache2-mod-wsgi-py3```
- Sitten tarkistetaan VirtualHost-tiedoston syntaksi configtest-komennolla ja käynnistetään Apache uudelleen
- Sivuston toimivuutta voidaan testata komentorivillä komennolla
    ```$ curl -s localhost|grep title``` ja voidaan testata, että onko kyseessä nimenomaan Apache-palvelin komennolla
    ```$ curl -sI localhost|grep Server```

## a) Yksinkertainen Django-esimerkkiohjelma

- Aloin tekemään yksinkertaista Django-ohjelmaa seuraamalla Tero Karvisen tekemää ohjetta https://terokarvinen.com/2022/django-instant-crm-tutorial/. Ensimmäiseksi latasin virtuaalikoneelleni uuden virtuaalisen työympäristön syöttämällä komennon
    ```$ sudo apt-get -y install virtualenv```.
- Seuraavaksi syötin komennon
    ```$ virtualenv --system-site-packages -p python3 env/```, joka loi uuden env/-kansion, joka sisältää kaikki viimeisimmät system-site-packages -kirjaston paketit. Varmistin myös, että lataan varmasti Python3-version paketit krijoittamalla komentoon python3, jotta en vahingossa sattuisi lataamaan vanhojen Python-versioiden paketteja.
- Seuraavaksi laitoin virtuaaliympäristön päälle komennolla
    ```$ source env/bin/activate```. Tarkistin vielä, että lataukset tapahtuvat varmasti virtuaaliympäristöön syöttämällä komennon
    ```$ which pip``` ja komennon tuottama tuloste osoitti, että olen oikeassa ympäristössä.
![Screenshot_2024-03-03_10-56-06.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_10-56-06.png)
- Sitten loin requirements.txt-tiedoston micro-editorilla
![Screenshot_2024-03-03_11-01-05.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_11-01-05.png)
- Latasin requirements.txt tiedoston ```pip install```-komennolla ja tarkistin vielä, minkä Django-version latasin.
![Screenshot_2024-03-03_11-06-11.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_11-06-11.png)
- Sitten loin jukkaco-nimisen projekti syöttämällä komennon
    ```django-admin startproject jukkaco```, navigoin projektin kansioon ja laitoin serverin päälle komennolla
    ```./manage.py runserver```.
![Screenshot_2024-03-03_11-18-51.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_11-18-51.png)
- Menin selaimessa serverin osoitteeseen ja siellä näkyi Djangon testisivu eli sovellus toimi
![Screenshot_2024-03-03_11-13-28.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_11-13-28.png)
- Seuraavaksi lähdin luomaan admin-rajapintaa. Syötin aluksi komennon
    ```./manage.py makemigrations```, joka tuotti ilmoituksen, josta kävi ilmi, että muutoksia ei ole havaittu. Sitten syötin ```./manage.py migrate```-komennon, joka tuotti pidemmän tulosteen uusista migraatioista
![Screenshot_2024-03-03_11-31-02.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_11-31-02.png)
- Sitten aloin luomaan uutta käyttäjää. Latasin ensin salasanagenerointiohjelman komennolla
    ```$ sudo apt-get install pwgen``` ja loin sillä itselleni uuden salasanan
- Sitten annoin uudelle käyttäjälle superuser-oikeudet komennolla
    ```$ ./manage.py createsuperuser```. Sitten minun piti syöttää käyttäjän tiedot ja jätin käyttäjänimiosion tyhjäksi ja siten käyttäjänimekseni tuli automaattisesti virtuaalikoneeni pääkäyttäjän tunnus. Sitten syötin sähköpostiosoitteen ja juuri generoimani salasanan.
- Kun superuser oli luotu, käynnistin serverin uudelleen ja menin selaimessa osoitteeseen 127.0.0.1:8000/admin/
![Screenshot_2024-03-03_11-43-07.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_11-43-07.png)
- Testasin vielä sisäänkirjautumista luomallani käyttäjätunnuksella ja se toimi
![Screenshot_2024-03-03_12-05-48.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_12-05-48.png)
- Sitten lähdin luomaan asiakastietokantaa. Loin ensi uuden crm/-kansion komennolla
    ```$ ./manage.py startapp crm``` ja sen jälkeen kävin lisäämässä sen settings.py-tiedoston INSTALLED_APPS-osioon
![Screenshot_2024-03-03_12-12-03.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_12-12-03.png)
- Sitten loin crm/-kansioon uuden models.py-tiedoston, johon loin uuden Customer-luokan asiakastietoja varten
![Screenshot_2024-03-03_12-18-34.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_12-18-34.png)
- Sitten syötin ```./manage.py makemigrations```-komennon, joka näytti tekemäni muutokset ja sitten syötin ```./manage.py migrate```-komennon, joka tallensi muutokset.
- Sitten rekisteröin admin-käyttäjäni
![Screenshot_2024-03-03_12-29-44.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_12-29-44.png)
- Laitoin serverin päälle ja kirjauduin sisään ja Customers-osio oli ilmestynyt näkyviin. Lisäsin vielä testinä kaksi uutta asiakasta
![Screenshot_2024-03-03_12-36-33.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_12-36-33.png)
- Koska halusin saada asiakkaiden nimet näkyviin numeroarvojen sijasta, kävin vielä muokkaamassa models.py tiedostoa
![Screenshot_2024-03-03_12-44-13.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_12-44-13.png)
- Muutosten jälkeen asiakkaiden nimet näkyivät tietokannassa
![Screenshot_2024-03-03_12-46-25.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_12-46-25.png)

## b) Djangon tuotantotyyppinen asennus

- Aloin tekemään Djangon tuotantotyyppistä asennusta seuraamalla Tero Karvisen ohjetta https://terokarvinen.com/2022/deploy-django/. Tehtävää varten tarvittavan Apache 2 web-palvelimen olin jo asentanut aiemmin ja olin korvannut Apachen testisivun omalla tekstillä.
- Loin omaan kotihakemistooni uuden hakemistopolun uutta staattista web-sivua varten komennolla
    ```$ mkdir -p publicwsgi/jukkaco/static/``` ja korvasin testisivun komennolla
    ```$ echo "Static site"|tee publicwsgi/jukkaco/static/index.html```
- Sitten lisäsin uuden VirtualHostin
![Screenshot_2024-03-03_14-26-08.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_14-26-08.png)
- Laitoin web-sivun päälle komennolla
    ```$ sudo a2ensite jukkaco.conf``` ja testasin ```/sbin/apache2ctl configtest```-komennolla sivun syntaksin toimivuutta ja syntaksi oli ok.
- Käynnistin Apachen uudelleen ja testasin staattisen web-sivun toimivuutta selaimessa osoitteessa http://localhost/static/ ja se toimi
![Screenshot_2024-03-03_14-29-38.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_14-29-38.png)
- Sitten aloin luomaan uutta virtuaalityöympäristöä. Navigoin publicwsgi/-kansioon ja loin sinne uuden hakemiston virtuaaliympäristöä varten komennolla
    ```$ virtualenv -p python3 --system-site-packages env```
- Sitten laitoin virtuaaliympäristön päälle ja tarkistin vielä varmuuden vuoksi, että olen oikeassa hakemistossa
![Screenshot_2024-03-03_14-49-23.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_14-49-23.png)
- Sitten loin uuden requirements.txt tiedoston Djangoa varten ja latasin sen samaan tapaan kuin edellisessä harjoituksessa
![Screenshot_2024-03-03_14-53-16.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_14-53-16.png)
- Aloin luomaan uutta Django-projektia ja käytin siinä pohjana aiemmassa harjoituksessa tekemääni jukkaco-projektia.
- Aloin yhdistämään Pythonia Apacheen muokkaamalla jukkaco.conf-tiedostoa siten, että määritin sinne hakemistopolut Django-projektin päähakemistoon, wsgi.py-tiedostoon ja virtualenvin site-packages -kansioon
![Screenshot_2024-03-03_15-30-55.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_15-30-55.png)
- Tämän jälkeen latasin Apachen WSGI-moduulin syöttämällä komennon
    ```sudo apt-get -y install libapache2-mod-wsgi-py3```
- Seuraavaksi tein cofigtest-komennon ja siitä kävi ilmi, että jukkaco.conf-tiedostossa oli syntaksivirhe ja korjasin sen ja korjauksen jälkeen tein configtest-komennon uudelleen ja syntaksi oli ok.
- Käynnistin Apachen uudelleen ja tämän jälkeen testasin web-sivuni toimivuutta
    ```curl -s localhost|grep title```-komennolla ja se tulosti seuraavan virheilmoituksen:
![Screenshot_2024-03-03_15-50-29.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-03_15-50-29.png)
- En saanut raporttia tehdessä tätä ongelmaa ratkaistua ja tehtävä jäi tältä osin kesken


  
  



       
