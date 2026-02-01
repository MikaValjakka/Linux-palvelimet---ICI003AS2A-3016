 Apache2 Name-Based Virtual Host -- Asennus ja Konfigurointi (Linux)

## 1. Name-based vs IP-based Virtual Hosts

### Ennen: IP-pohjainen palvelinhaku

    IP-osoite1 → sivu1.com
    IP-osoite2 → sivu2.com

**Ongelma:** IP-osoitteiden niukkuus.

IPv4 koostuu neljästä luvusta (0--255):

    256 × 256 × 256 × 256 = 4 294 967 296 osoitetta

Tämä ei riitä globaalin internetin tarpeisiin.

### Ratkaisu: Name-based Virtual Hosts

    IP-osoite1 → sivu1.com
    IP-osoite1 → sivu2.com

Useat verkkotunnukset voivat jakaa saman IP-osoitteen, koska selain
lähettää HTTP-pyynnössä `Host`-headerin.

------------------------------------------------------------------------

## 2. Miten Apache valitsee oikean VirtualHostin?

1.  **IP + portti**\
2.  **ServerName ja ServerAlias**\
3.  **Fallback** → jos nimeä ei löydy, Apache käyttää ensimmäistä
    määriteltyä VirtualHostia.

    <VirtualHost *:80>

→ Hyväksyy kaikki IP-osoitteet portissa 80 (name-based hostingin ydin).

**ServerName on pakollinen**, muuten Apache käyttää koneen omaa
hostnamea, mikä voi johtaa virheelliseen sivustovalintaan.

------------------------------------------------------------------------

## 3. Apache2:n asennus

``` bash
sudo apt-get update
sudo apt-get install apache2
```

Testaus selaimessa:

    http://localhost
    http://127.0.0.1

------------------------------------------------------------------------

## 4. Apache-hakemistot

Konfiguraatiot sijaitsevat:

    /etc/apache2

Tärkeimmät kansiot:

-   `sites-available/` → kaikki määritellyt sivustot
-   `sites-enabled/` → aktiiviset sivustot (symboliset linkit)

------------------------------------------------------------------------

## 5. VirtualHost-konfiguraation luonti

Luodaan tiedosto:

    /etc/apache2/sites-available/mikavee.conf

Sisältö:

``` apache
<VirtualHost *:80>
    ServerName mikavee.example.com
    ServerAlias www.mikavee.example.com

    DocumentRoot "/home/mikavee/publicsites/mikavee/"

    <Directory "/home/mikavee/publicsites/mikavee/">
        Require all granted
    </Directory>
</VirtualHost>
```

### Selitys

-   `ServerName` → päädomain
-   `ServerAlias` → vaihtoehtoiset domainit
-   `DocumentRoot` → kansio, josta Apache lukee sivuston sisällön
-   `<Directory>` → antaa selaimelle luvan lukea tiedostot

------------------------------------------------------------------------

## 6. Sivuston aktivointi

``` bash
sudo a2dissite 000-default.conf
sudo a2ensite mikavee.conf
sudo systemctl reload apache2
```

**Selitys:**

-   `a2dissite` → poistaa oletussivun
-   `a2ensite` → aktivoi oman sivuston
-   `reload` → lataa uudet asetukset

------------------------------------------------------------------------

## 7. Kansioiden luonti ja oikeudet

``` bash
mkdir -p /home/mikavee/publicsites/mikavee
chmod ugo+x /home/mikavee/publicsites
```

**Tarkoitus:** Apache tarvitsee luku- ja suoritusoikeudet kansioihin.

------------------------------------------------------------------------

## 8. Testisivun luonti

``` bash
cd /home/mikavee/publicsites/mikavee
echo "Hello World 🖤" > index.html
```

Selain näyttää nyt:

    Hello World 🖤

------------------------------------------------------------------------

## 9. Toimintaketju

   ### Browser → DNS/hosts → Apache → VirtualHost → DocumentRoot → index.html
    
1. Browser:   Käyttäjä kirjoittaa selaimeen osoitteen (esim. mikavee.example.com)

2. DNS / /etc/hosts:
   Selain selvittää IP-osoitteen joko DNS-palvelimelta tai paikallisesta /etc/hosts-tiedostosta

3. Apache:
   Selain muodostaa TCP-yhteyden IP-osoitteeseen porttiin 80 ja lähettää HTTP-pyynnön

4. VirtualHost:
   Apache valitsee oikean <VirtualHost>-lohkon Host-headerin perusteella

5. DocumentRoot:
   Apache lukee konfiguraatiossa määritellyn DocumentRoot-kansion

6. index.html:
   Apache etsii oletustiedoston (index.html) ja palauttaa sen selaimelle

------------------------------------------------------------------------

## Yhteenveto

Tässä työssä rakennettiin Apache2:lle name-based virtual host -ympäristö
alusta alkaen:

-   IP-ongelman ymmärtäminen
-   Apache-asennus
-   VirtualHost-konfiguraatio
-   Sivuston aktivointi
-   Hakemistojen luonti
-   Testisivun julkaisu
