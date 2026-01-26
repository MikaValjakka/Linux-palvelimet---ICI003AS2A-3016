# Debian 13:n Asennus Oracle VM VirtualBoxiin

Tämä raportti tarjoaa vaiheittaisen oppaan Debian 13 Linux-jakelun asentamiseen virtuaalikoneeseen Oracle VM VirtualBox Manager -ohjelmistolla. Ohje on suunniteltu selkeäksi ja yksityiskohtaiseksi, jotta sitä voi seurata kuka tahansa, jolla on perustiedot tietokoneen käytöstä. Oleta, että käytät Windows-, macOS- tai Linux-käyttöjärjestelmää isäntäkoneena (host). Jos kohtaat ongelmia, tarkista VirtualBoxin virallinen dokumentaatio tai Debianin asennusopas.

## Edellytykset

- **Isäntäkoneen käyttöjärjestelmä:** Windows / macOS / Linux
- **Oracle VM VirtualBox:** versio 7.x
- **Debian:** Debian 13 (amd64)
- **Vapaa levytila:** vähintään 20 GB
- **Muisti:** vähintään 4 GB (suositus 8 GB)
- Virtualisointi (VT-x / AMD-V) käytössä BIOS/UEFI:ssa
- **Internet-yhteys:** Tarvitaan ohjelmistojen ja ISO-tiedoston lataamiseen.
- **Oikeudet:** Asennuksessa tarvitaan järjestelmänvalvojan oikeuksia.


## Vaihe 1: Lataa ja Asenna Oracle VM VirtualBox
1. Mene Oracle VM VirtualBoxin viralliselle sivustolle: [https://www.virtualbox.org/](https://www.virtualbox.org/)
2. Klikkaa "Downloads" -osioon ja valitse versio, joka sopii isäntäkoneesi käyttöjärjestelmälle (esim. Windows hosts, macOS hosts tai Linux distributions).
3. Lataa uusin versio (esim. VirtualBox 7.x). Tiedoston koko on noin 100-200 MB.
4. Asenna ohjelma:
   - **Windows:** Kaksoisklikkaa .exe-tiedostoa ja seuraa asennusohjelman ohjeita. Hyväksy oletusasetukset.
   - **macOS:** Avaa .dmg-tiedosto, vedä VirtualBox.app Sovellukset-kansioon ja avaa se.
   - **Linux:** Seuraa jakelusi ohjeita (esim. Ubuntu: sudo dpkg -i virtualbox-*.deb).
5. Asennuksen jälkeen käynnistä VirtualBox Manager. Jos kysytään, asenna **Extension Pack** (ladattavissa samalta sivustolta) lisäominaisuuksia varten (esim. USB-tuki).
   - Extension Pack asennetaan, koska se tuo lisätoimintoja kuten USB-tuen ja parannetun hiiren integraation.
   
   **VirtualBox näyttää nyt tältä:**  
   
   ![Oracle VM VirtualBox](https://github.com/MikaValjakka/Linux-palvelimet---ICI003AS2A-3016/blob/main/pictures/WB_1.jpg)

# Debian 13: Asennus
## 1. Debian 13 ISO-tiedoston lataus

1. Siirry Debianin viralliselle sivulle:
   https://www.debian.org/distrib/
2. Lataa Debian 13 amd64 ISO (netinst tai DVD).
3. Tarkista ladatun ISO-tiedoston eheys SHA256-tarkistussummalla.

## 2. Virtuaalikoneen luominen

### 2.1 Uuden virtuaalikoneen luonti
- Valitse **Machine → New** tai paina **Ctrl + N**
- Name: Debian 13
- Type: Linux
- Version: Debian (64-bit)
- ISO Image: valitse ladattu Debian 13 ISO

### 2.2 Käyttäjä- ja kirjautumistiedot
-   Username: Valitse tarvittava tai vaadittava käyttäjätunnus
-   Password: Valitse oikea vahva salasana.
  
  Käyttäjä luodaan asennuksen yhteydessä, jotta järjestelmää ei tarvitse käyttää root-käyttäjällä päivittäisessä työskentelyssä.
  ![User and Password](https://github.com/MikaValjakka/Linux-palvelimet---ICI003AS2A-3016/blob/main/pictures/k%C3%A4ytt%C3%A4j%C3%A4tiedot.jpg)

### 2.3 Muisti ja prosessori
- Base Memory: 4096 MB (4 GB)
- Processors: 2
  
![Base memory](https://github.com/MikaValjakka/Linux-palvelimet---ICI003AS2A-3016/blob/main/pictures/basememory.jpg)

4 GB muistia riittää Debianin graafiseen työpöytäkäyttöön.
Kaksi prosessoriydintä parantaa järjestelmän reagointia
ilman että kuormittaa liikaa isäntäkoneen resursseja

### 2.4 Virtuaalinen levy
1. Valitse: "Create Virtual Hard Disk Now"
2. Koko (Size) 20 GB

![Virtual memory](https://github.com/MikaValjakka/Linux-palvelimet---ICI003AS2A-3016/blob/main/pictures/virtual_HD.jpg)

### 2.4 Summary / Yhteenveto
VirtualBox näyttää yhteenvedon tehdyistä valinnoista.
Tarkista asetukset ja hyväksy ne painamalla **Finish**.
-> Tässä vaihessa asennuksessa voi kestää hetki

![Summary](https://github.com/MikaValjakka/Linux-palvelimet---ICI003AS2A-3016/blob/main/pictures/Summary.jpg)

## 3 Asennuksen varmistaminen ja järjestelmän päivitys

Asennuksen onnistuminen varmistetaan kirjautumalla järjestelmään
ja suorittamalla peruspäivitykset terminaalissa.

### 3.1 Kirjautuminen ja terminaalin avaaminen

1. Kirjaudu sisään luodulla käyttäjätunnuksella.
2. Avaa terminaali (esim. Applications → Terminal).

### 3.2 Järjestelmän päivitys

Ennen järjestelmän käyttöä on suositeltavaa päivittää pakettiluettelot
ja asentaa saatavilla olevat päivitykset.

Suorita seuraavat komennot:
``` bash
sudo apt update
sudo apt upgrade
```
järjestelmä voi pyytää uudelleenkäynnistystä:
``` bash
sudo reboot
```



