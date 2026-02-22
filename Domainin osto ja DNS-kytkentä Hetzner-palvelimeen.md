# Domainin osto ja DNS-kytkentä Hetzner-palvelimeen

Tämä ohje kuvaa vaihe vaiheelta, miten domain ostetaan Namecheapistä ja liitetään Hetznerissä olevaan Debian + Apache -palvelimeen.

---

## 1. Domainin osto

1. Mene domain-rekisteröijän sivulle (esim. Namecheap)
2. Hae haluamasi domain (esim. `mikavee.xyz`)
3. Osta domain **ilman** lisäpalveluita:

   * ei hostingia
   * ei sähköpostia
   * ei site builderia
4. WHOIS Privacy kannattaa pitää päällä

Domain on nyt rekisteröity, mutta ei vielä osoita mihinkään.

---

## 2. DNS-hallintaan siirtyminen

1. Kirjaudu Namecheapiin
2. Mene **Domain List**
3. Valitse domain (esim. `mikavee.xyz`)
4. Avaa **Advanced DNS**

---

## 3. Oletus-DNS-tietueiden poistaminen

Namecheap lisää oletuksena parkki- ja ohjaustietueita, jotka on poistettava.

Poista seuraavat, jos ne ovat olemassa:

* CNAME Record

  * Host: `www`
  * Target: `parkingpage.namecheap.com`

* URL Redirect Record

  * Host: `@`
  * Destination URL: `http://www.mikavee.xyz/`

Nämä estävät oman palvelimen käytön.

---

## 4. Oikeiden A-tietueiden lisääminen

Lisää seuraavat **A Record** -tietueet:

### Juuri-domain

* Type: A Record
* Host: `@`
* Value: `SERVERISI IP OSOITE`
* TTL: Automatic

### www-alidomain

* Type: A Record
* Host: `www`
* Value: `SERVERISI IP OSOITE`
* TTL: Automatic

Lopputuloksena DNS-listassa tulee olla vain:

```
A   @     (SERVERISI IP OSOITE)
A   www   (SERVERISI IP OSOITE)
```

---

## 5. DNS:n toiminnan tarkistus

DNS:n päivittyminen kestää yleensä muutamasta minuutista noin puoleen tuntiin.

Tarkista palvelimella:

```bash
ping mikavee.xyz
```

Tai tarkemmin:

```bash
dig mikavee.xyz +short
```

Jos vastauksena on Hetzner-palvelimen IP-osoite, DNS toimii oikein.

---

## 6. Apache VirtualHost (lyhyesti)

Kun DNS osoittaa oikein, Apache voidaan konfiguroida domainille.

VirtualHost-tiedosto:

```
/etc/apache2/sites-available/mikavee.xyz.conf
```

Sisältö:

```
<VirtualHost *:80>
    ServerName mikavee.xyz
    ServerAlias www.mikavee.xyz

    DocumentRoot /var/www/mikavee

    <Directory /var/www/mikavee>
        Require all granted
    </Directory>
</VirtualHost>
```

Ota sivusto käyttöön:

```bash
sudo a2ensite mikavee.xyz
sudo systemctl reload apache2
```

---

## 7. Valmius HTTPS:lle

Kun:

* domain osoittaa Hetznerin IP:hen
* Apache vastaa HTTP:llä

palvelin on valmis HTTPS-varmenteen asennukseen (Let’s Encrypt + Certbot).

HTTPS käsitellään seuraavassa ohjeessa.

---

## Yhteenveto

* Domain ja palvelin pidetään erillään
* DNS yhdistää domainin IP-osoitteeseen
* Apache hoitaa varsinaisen sisällön
* HTTPS lisätään vasta kun DNS toimii

Tämä on tuotantotason ja siirrettävä ratkaisu.
