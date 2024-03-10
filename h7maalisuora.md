# h7 Maalisuora

## a) Hello world
- Tein simppelin "Hello world!"-komennon Pythonilla. Olin jo tunnilla luonut Python-komentoja varten koneeni juurihakemistoon uuden kansion ja loin sinne komentoa varten micro-editorilla hello.py-tiedoston seuraavalla tavalla:
![Screenshot_2024-03-09_13-16-27.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-09_13-16-27.png)
- Ajoin komennon python-kansiossa ja se tulosti "Hello world"-tekstin
![Screenshot_2024-03-09_13-22-37.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-09_13-22-37.png)

## b) Uusi komento Linuxiin
- Tein uuden komennon Linuxiin samalla tavalla kuin tunnilla näytettiin esimerkkinä eli komento tulostaa tervehdyksen, koneeni käyttäjänimen, koneen nimen ja päivämäärätiedot.
![Screenshot_2024-03-10_12-13-05.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-10_12-13-05.png)
- Testasin komentoa bash-komennolla
![Screenshot_2024-03-10_12-20-18.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-10_12-20-18.png)
- Annoin komennolle execute-oikeudet komennolla    ```chmod +x tervehdys.sh```
- Lisäsin komennon /usr/local/bin-polkuun syöttämällä komennon
    ```sudo mv tervehdys.sh /usr/local/bin/tervehdys```
- Testasin parissa eri kansiossa, toimiiko komento ja se toimi. Seuraavassa kuvassa testitapaus Documents-kansiossa:
![Screenshot_2024-03-10_12-45-10.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-10_12-45-10.png)

## c) Labraharjoitus
- Tein harjoitusmielessä vanhan laboratoriokokeen viime vuoden Linux-palvelimet kurssilta (https://terokarvinen.com/2023/linux-palvelimet-2023-arvioitava-laboratorioharjoitus/?fromSearch=laboratorioharjoitus). Loin tehtävää varten uuden tyhjän virtuaalikoneen ja tein sille normaalit alkuasetukset ja asensin päivitystiedostot, palomuurin sekä micro-editorin.
- Jätin harjoitusta tehdessäni välistä a)-, b)- ja c)-osat, koska ne eivät olleet olennaisia harjoitusluontoista tehtävää tehdessä ja aloitin kohdasta d), jossa piti luoda uusi komento kaikkien käyttäjien käytettäväksi.
### Uusi Linux-komento
- Tein uuden komennon Linuxiin samalla kaavalla kuin aiemmin tässä tehtävässä eli loin komennolle uuden tiedoston hey.sh
![Screenshot_2024-03-10_17-02-12.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-10_17-02-12.png)
- Testasin sitä bash-komennolla:
![Screenshot_2024-03-10_17-06-47.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-10_17-06-47.png)
- Annoin execute-oikeudet:    ```chmod +x hey.sh```
- Komento uuteen polkuun:    ```sudo mv hey.sh /usr/local/bin/hey```
- Testasin komennon ja se toimi
![Screenshot_2024-03-10_17-37-44.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-10_17-37-44.png)
### Apache web-palvelimen asennus
- Seuraavaksi aloin tekemään laboratorioharjoituksen kohtaa f), jossa piti asentaa Apache web-palvelin ja luoda sinne uusi käyttäjä Erkki Esimerkki ja käyttäjälle uusi kotisivu
- Asensin Apachen:    ```$sudo apt-get install apache2```
- Apache päälle:    ```$sudo systemctl enable apache2 --now```
- Apachen testisivu tuli näkyviin localhostiin
![Screenshot_2024-03-10_18-13-57.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-10_18-13-57.png)
- Luodaan uusi käyttäjä erkki:
![Screenshot_2024-03-10_18-26-04.png](https://github.com/JukkaLak/linuxpalvelimet-k24/blob/main/Screenshot_2024-03-10_18-26-04.png)
- Yritin vielä luoda uudelle käyttäjälle uutta kotisivua, mutta se ei onnistunut ja minulla loppui aika ja kärsivällisyys, joten tehtävä jäi jälleen kesken.

## d) Uusi virtuaalikone labraharjoitusta varten
- Loin tulevaa labraharjoitusta varten uuden virtuaalikoneen samaan tapaan kuin aiemmin kurssilla eli käyttöliittymäksi 64 bittinen Debian 12, muistia 4000 MB:ta ja 60 GB:n kovalevy. Alkuasetusten teon jälkeen tein päivitykset ja latasin palomuurin.





