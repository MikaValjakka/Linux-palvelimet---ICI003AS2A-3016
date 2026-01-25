# Debian 13:n Asennus Oracle VM VirtualBoxiin

Tämä raportti tarjoaa vaiheittaisen oppaan Debian 13 Linux-jakelun asentamiseen virtuaalikoneeseen Oracle VM VirtualBox Manager -ohjelmistolla. Ohje on suunniteltu selkeäksi ja yksityiskohtaiseksi, jotta sitä voi seurata kuka tahansa, jolla on perustiedot tietokoneen käytöstä. Oleta, että käytät Windows-, macOS- tai Linux-käyttöjärjestelmää isäntäkoneena (host). Jos kohtaat ongelmia, tarkista VirtualBoxin virallinen dokumentaatio tai Debianin asennusopas.

## Edellytykset
   - **Isäntäkoneen vaatimukset:** Vähintään 4 GB RAM (suositellaan 8 GB+), 20 GB vapaata levytilaa, ja prosessori, joka tukee virtualisointia (VT-x/AMD-V). Tarkista BIOS/UEFI-asetuksista, että virtualisointi on päällä.
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
5. Asennuksen jälkeen käynnistä VirtualBox Manager. Jos kysytään, asenna Extension Pack (ladattavissa samalta sivustolta) lisäominaisuuksia varten (esim. USB-tuki).
   VirtualBox näyttää nyt tältä:
   
   

   ![Oracle VM VirtualBox](https://github.com/MikaValjakka/Linux-palvelimet---ICI003AS2A-3016/blob/main/pictures/WB_1.jpg)
