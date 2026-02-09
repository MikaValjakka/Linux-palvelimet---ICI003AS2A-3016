# Hetzner Debian 13 Web Server – Asennusraportti (Apache + UFW + käyttäjämalli)

Tässä raportissa kuvataan vaihe vaiheelta, kuinka Hetzner Cloud -palvelimelle perustetaan turvallinen ja tuotantokelpoinen Apache2-web-palvelin Debian 13 -käyttöjärjestelmällä.

---

## 1. SSH ja Palvelimen luonti Hetznerissä

1. Kirjaudu Hetzner Cloud Consoleen
2. Luo uusi palvelin:

   * OS: **Debian 13**
   * Tyyppi: edullinen CX / CPX -malli
- Koska kyseessä on harjoitustyö: Aluksi sopisi Shared Cost-Optimized, eli halvin mahdollisin:
**VCPU 2
RAM 4 GB
NVMe SSD 40 GB
Traffic incl. 20 TB
IPv4 KYLLÄ
Hourly € 0.007
Monthly € 4.38
CX23Intel ® / AMD**
3. Lisää oma **SSH public key** palvelimen luonnin yhteydessä
  ```
ssh-keygen -t ed25519 -C "sähköpostiosoitteesi@esimerkki.com"
  ```
- **-t ed25519:** Määrittää avaimen tyypin (turvallisin ja tehokkain).
- **-C "kommentti":** Lisää kommentin (esim. sähköpostisi tai kuvaus kuten "hetzner-harjoitus"), jotta tunnistat avaimen myöhemmin.
- Komento kysyy tallennuspaikkaa. Paina Enter hyväksyäksesi oletusarvon **(~/.ssh/id_ed25519)**
- Seuraavaksi se kysyy passphrasen (salasana avaimelle).
Suosittelen vahvasti asettamaan vahvan salasanan (esim. 12+ merkkiä, sekoitus kirjaimia, numeroita, symboleja). Tämä suojaa yksityisen avaimen, vaikka joku pääsisi koneellesi.
**MUISTA TÄMÄ**

Jos et halua passphrasea (ei suositella turvallisuussyistä), paina Enter kahdesti.

- Avainpari luodaan:
* Yksityinen avain: ~/.ssh/id_ed25519 (tai id_rsa) – ÄLÄ JAA TÄTÄ KENELLEKÄÄN!
* Julkinen avain: ~/.ssh/id_ed25519.pub (tai id_rsa.pub) – Tämän kopioit Hetzneriin.

4. Paina Add SSH Key (tai vastaava "Lisää SSH-avain").

Kun palvelin on luotu, kirjaudu sisään:

```bash
ssh root@SERVER_IP
```
Nyt Palvelin kysyy SSH passphrasea **Muistathan?!**

Olet sisällä serverissäsi. Onneksi olkoon.

---

## 2. Järjestelmän päivitys

```bash
apt update && apt upgrade -y
```

---

## 3. Apache2 asennus

```bash
apt install apache2 -y
```

Tarkista:

```bash
systemctl status apache2
```

Testaa selaimessa:

```
http://SERVER_IP
```

---

## 4. Käyttäjän luonti (mikavee)

Luodaan normaali käyttäjä, joka hallinnoi sivuja:

```bash
adduser mikavee
adduser mikavee sudo
adduser mikavee adm
```

Kirjaudu käyttäjälle:

```bash
su - mikavee
```

---

## 5. Web-kansion luonti

Luodaan projektikansio Apachea varten:

```bash
sudo mkdir /var/www/mikavee
sudo chown -R www-data:www-data /var/www/mikavee
sudo chmod -R 755 /var/www/mikavee
```

Luo testisivu:

```bash
sudo nano /var/www/mikavee/index.html
```

Esimerkkisisältö:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Mikavee testisivu</title>
</head>
<body>
  <h1>Hei maailma!</h1>
  <p>Tämä on testisivu Apache2:lla.</p>
</body>
</html>
```

---

## 6. Apache VirtualHost -konfiguraatio

Luo konffitiedosto:

```bash
sudo nano /etc/apache2/sites-available/mikavee.conf
```

Sisältö:

```apache
<VirtualHost *:80>
    ServerName SERVER_IP
    DocumentRoot /var/www/mikavee

    <Directory /var/www/mikavee>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/mikavee_error.log
    CustomLog ${APACHE_LOG_DIR}/mikavee_access.log combined
</VirtualHost>
```

Testaa syntaksi:

```bash
sudo apache2ctl configtest
```

Aktivoi sivusto:

```bash
sudo a2ensite mikavee.conf
sudo systemctl reload apache2
```

---

## 7. Firewall (UFW)

Asenna ja aktivoi palomuuri:

```bash
sudo apt install ufw -y
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

Tarkista:

```bash
sudo ufw status
```

---


## 8. Käyttäjämalli ja turvallisuus

Web-sivujen rakenne:

```
/var/www/mikavee
```

Oikeudet:

* Omistaja: **www-data** (Apache)
* Muokkaus: **mikavee sudo:n kautta**
* Hallinta: **root**

Tämä malli:

* estää vahingot
* suojaa käyttäjien kotihakemistoja
* vastaa tuotantotason web-palvelimia

---



## Yhteenveto

Tällä mallilla rakennettiin:

* Turvallinen Linux-palvelin
* Apache2 web server
* Monikäyttäjäinen hosting-arkkitehtuuri
* Firewall-suojaus
* Valmius HTTPS:lle

Tämä rakenne vastaa oikeaa tuotantotason web-hostingia.

---
