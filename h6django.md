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
- Yksittäinen Python paketti lisätään tekstitiedostoon, esimerkiksi micro-editorilla, ja lopuksi testitiedosto ladataan pip-komennolla:
    ```$ sudo apt-get install micro```,
    ```$ micro requirements.txt # "django"```,
    ```$ cat requirements.txt```,
    ```$ pip install -r requirements.txt```
- Käytössä olevan Django-version voi tarkistaa komennolla
    ```$ django-admin --version```
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
- Sitten päivitettän tietokanta
- Jotta tietokanna voi nähdä ylläpitäjän oikeuksilla, ylläpitäjä täytyy rekisteröidä:
    ```$ micro crm/admin.py```
-    ```
     from django.contrib import admin
     from . import models

     admin.site.register(models.Customer)
- Sitten laitetaan serveri päälle ja tietokantaa voi tutkia ylläpitäjän oikeuksilla osoitteessa http://127.0.0.1:8000/admin/
- Aluksi tietokanta näyttää asiakaslistan numeroina, mutta asiakkaan nimet voidaan laittaa näkyville muokkaamalla models.py-tiedostossa olevaa Customer luokkaa:
    ```
    from django.db import models

    class Customer(models.Model):
      name = models.CharField(max_length=160)

      def __str__(self):	 
        return self.name	# tämä toiminto muuttaa asiakkaan arvon merkkijonoksi

### Deploy Django 4 - Production Install
- Djangon voi ladata tuotantotyyppisesti luomalla Apache web palvelimen. Ensin ladataan Apache 2, jos sitä ei ole vielä asennettuna:
    ```$ sudo apt-get -y install apache2```
- Korvataan testisivu:
    ```$ echo "Tekstiä"|sudo tee /var/www/html/index.html```
- Sivustolle luodaan sisältöä menemällä käyttäjän juurihakemistoon ja  luomalla uudet hakemistot testisiältöä varten ja lisäämällä tekstiä:
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
- Laitetaan uusi sivu päälle ja laitetaan oletuksena oleva testisivu pois päältä:
    ```
    $ sudo a2ensite yoursite.conf
    $ sudo a2dissite 000-default.conf
- Testataan konfiguraation toimivuus:
    ```$ /sbin/apache2ctl configtest```
- Kun konfiguraatio toimii, käynnistetään Apache uudelleen:
    ```$ sudo systemctl restart apache2```
- Staattisten tiedostojen toimivuutta voidaan testata menemällä selaimella osoitteeseen http://localhost/static/ tai komentorivillä komennolla
    ```$ curl http://localhost/static/```
- Seuraavaksi luodaan uusi VirtualEnv-ympäristö ja luodaan sille uusi hakemisto publicwsgi-kansioon:
    ```
    $ sudo apt-get -y install virtualenv

    $ cd
    $ cd publicwsgi/

    $ virtualenv -p python3 --system-site-packages env

- Kannataa lisätä system-site-packages -komentoon -p python3, jotta varmasti käytetään uusinta Python-versiota
- Ladataan uusin Django versio laittamalla uusi virtuaaliympäristö päälle:
    ```$ source env/bin/activate```
- Kannataa tarkistaa, että pip-asennusohjelma löytyy nimenomaan env/-hakemistosta
    ```
    $ which pip
    /home/user/publicwsgi/env/bin/pip
- Luodaan tekstitiedosto ja kirjoitetaan sinne django:
    ```
    $ micro requirements.txt

    django
- Seuraavaksi ladataan tiedosto:
    ```$ pip install -r requirements.txt```
- Tiedoston toimivuuden voi testata komennolla
    ```$ django-admin --version```
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
- Toimivuutta voi testata selaimessa menemällä localhost-osoitteeseen.

  
  



       
