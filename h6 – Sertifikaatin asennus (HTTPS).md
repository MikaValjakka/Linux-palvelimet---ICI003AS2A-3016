# h6 – Sertifikaatin asennus (HTTPS)

## Tavoite

Tässä harjoituksessa otetaan käyttöön HTTPS (Secure HTTP) Apache2‑palvelimelle Hetzner VPS:llä käyttäen Let’s Encryptiä ja Certbotia. Lopputuloksena verkkosivusto toimii turvallisesti osoitteissa:

* [https://mikavee.xyz](https://mikavee.xyz)
* [https://www.mikavee.xyz](https://www.mikavee.xyz)

Sertifikaatti uusiutuu automaattisesti.

---

## Lähtötilanne

* Palvelin: Hetzner VPS (Debian 13)
* Web-palvelin: Apache2
* Domain: mikavee.xyz (Namecheap)
* HTTP toimii portissa 80
* Apache VirtualHost on jo määritetty

---

## 1. Palomuurin tarkistus ja porttien avaus

HTTPS vaatii portin 443. Lisäksi portti 80 on välttämätön Let’s Encryptin validointia ja sertifikaatin uusimista varten.

```bash
sudo ufw allow 22/tcp   # SSH
sudo ufw allow 80/tcp   # HTTP (Let’s Encrypt validation)
sudo ufw allow 443/tcp  # HTTPS
```

Tarkistetaan tila:

```bash
sudo ufw status
```

Odotettu tulos:

```
22/tcp   ALLOW
80/tcp   ALLOW
443/tcp  ALLOW
```

---

## 2. Certbotin ja Apache‑pluginin asennus

Certbot asennetaan **vain palvelimelle**, jossa Apache pyörii.

```bash
sudo apt update
sudo apt install certbot python3-certbot-apache
```

---

## 3. Sertifikaatin hankinta ja asennus

Ajetaan Certbot Apache‑pluginilla:

```bash
sudo certbot --apache
```

Certbot:

* tunnistaa Apache VirtualHostit
* kysyy mille domaineille sertifikaatti tehdään
* hakee sertifikaatin Let’s Encryptiltä
* muokkaa Apache-konfiguraatiot automaattisesti
* asettaa HTTP → HTTPS ‑uudelleenohjauksen

Jos komento päättyy ilman virheitä, HTTPS on käytössä.

---

## 4. Apache-konfiguraation tarkistus

Tarkistetaan ettei konffissa ole virheitä:

```bash
sudo apachectl -t
sudo systemctl reload apache2
```

Certbot loi automaattisesti HTTPS‑VirtualHostin (portti 443) ja uudelleenohjauksen portista 80.

---

## 5. Toiminnan testaus

Selaimella:

* [https://mikavee.xyz](https://mikavee.xyz)
* [https://www.mikavee.xyz](https://www.mikavee.xyz)

Odotettu tulos:

* Sivusto latautuu
* Selaimessa näkyy lukko 🔒
* HTTP ohjautuu automaattisesti HTTPS:ään

---

## 6. Sertifikaatin tila ja uusiminen

Tarkistetaan sertifikaatit:

```bash
sudo certbot certificates
```

Testataan automaattinen uusinta (dry‑run):

```bash
sudo certbot renew --dry-run
```

Certbot asentaa myös systemd‑timerin, joka hoitaa uusinnan automaattisesti.
### Bonuksena voidaan sivu testata esim. SSLLabs

---

## Yhteenveto

* HTTPS otettu käyttöön Let’s Encryptillä
* Apache konffattu automaattisesti Certbotin avulla
* Portit 80 ja 443 ovat auki
* Sertifikaatti uusiutuu automaattisesti
* Sivusto on production‑valmis

---

## Huomiot

* Porttia 80 **ei saa sulkea**, ellei käytetä DNS‑01 validointia
* Sertifikaatteja ei tarvitse hallita käsin
* Certbot on suositeltu ja turvallinen tapa HTTPS:n käyttöönottoon

---

**Harjoitus: h6 – Sertifikaatin asennus (HTTPS)**
