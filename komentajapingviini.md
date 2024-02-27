# h2 Komentaja Pingviini

# x)
- Artikkelissa Command Line Basics Revisited käydään läpi tärkeimmät Linux-komentorivin komennot
- Komento $ pwd näyttää sen hakemiston, jossa käyttäjä tällä hetkellä työskentelee ($-merkkiä ei kirjoiteta)
- Komento $ ls näyttää kaikki kansiot ja tiedostot, mitä hakemistossa on
- Hakemistojen välillä liikutaan komennolla "cd hakemistonnimi/", esimerkiksi kun haluaa siirtyä hakemistoon Documents syötetään komento "cd Documents/"
- Kun halutaan siirtyä takaisin juurihakemistoon, syötetään komento "cd .."
- Tekstitiedostoja voi selata komennolla "less tiedostonnimi.txt"
- Tekstitiedostoja voi luoda komennolla nano-tekstieditorilla
- Uuden hakemiston voi luoda komennolla "mkdir hakemistonnimi" ja hakemiston nimeä voi muuttaa komennolla "mv vanhanimi uusinimi"
- Hakemisto kopioidaan komennolla "cp -r"
- Tyhjä hakemisto poistetaan komennolla "rmdir hakemistonnimi"
- Tiedostoja poistetaan komennolla "rm tiedostonnimi"
- Hakemisto sisältöineen poistetaan komennolla "rm -r hakemistonnimi"

# a) Micro-editorin asennus
Asensin micro-editorin koneelleni syöttämällä komentorivityökalussa komennon "sudo apt-get -y install micro".

# b) Rauta
Yritin aluksi syöttää tehtävänannon mukaisesti komennon "sudo lshw -short -sanitize", mutta komentorivityökalu ei tunnistanut tätä komentoa joten jouduin lataamaan lshw:n komennolla "sudo apt-get -y install lshw. Syötin komennon "sudo lshw -short -sanitize" uudestaan ja minulle avautui seuraavanlainen listaus: ![Näyttökuva (9)](https://github.com/JukkaLak/h2komentajapingviini/blob/main/N%C3%A4ytt%C3%B6kuva%20(9).png)
Tärkeimmät tiedot, mitä listasta voi esimerkkinä mainita ovat mm. koneen muistin tiedot, jotka näkyvät luokan memory kuvauksessa, prosessorin tiedot ja niin edelleen.

Loppuosa tehtävästä jäi tekemättä, koska minulle tuli ylitsepääsemättömiä ongelmia enkä päässyt eteenpäin.
