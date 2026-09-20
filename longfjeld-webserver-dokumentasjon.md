# longfjeld.com – dokumentasjon av gjenværende webserver

> **Oppdatert:** 2026-09-13  
> **Status:** Akkordia-applikasjonen og tilhørende backend, brukerdata og tilgangsstyring er avviklet.  
> **Formål:** Serveren skal kun publisere en enkel statisk nettside på `longfjeld.com` / `www.longfjeld.com`, med eventuelle statiske undersider.

---

## 1. Målbilde

Serveren har etter oppryddingen én oppgave:

- publisere statiske HTML-, CSS-, JavaScript- og bildefiler
- tilby nettstedet over HTTP og HTTPS
- bruke Apache HTTP Server fra Homebrew
- bruke Let's Encrypt-sertifikat for HTTPS
- bruke macOS PF til å videresende standardportene 80/443 til Apache på 8080/8443

Det skal **ikke** lenger finnes noen Akkordia-backend, API, database, brukeradministrasjon eller serverbasert lagring av appdata.

Prinsipiell arkitektur:

```text
Internett
    |
    | TCP 80 / 443
    v
macOS PF
    |
    +-- 80  -> 8080
    +-- 443 -> 8443
    |
    v
Apache HTTP Server (Homebrew)
    |
    +-- HTTP  :8080
    +-- HTTPS :8443
    |
    v
/usr/local/var/www/
    |
    +-- index.html
    +-- CSS / JavaScript / bilder
    +-- statiske undersider
```

---

## 2. Det som er fjernet

Følgende tilhørte den tidligere Akkordia-løsningen og skal ikke lenger være en del av servermiljøet:

- Node.js/Express-backend på `127.0.0.1:3000`
- Akkordia API (`/api`, `/app` osv.)
- reverse proxy fra Apache til Node
- Akkordia PWA-filer på serveren
- service worker og manifest for den gamle serverdistribuerte PWA-en
- serverbasert sangbibliotek
- JSON-filer med sang-, band- og setlistedata
- SQLite-database
- SQLite WAL/SHM-filer
- admin-/audit-data
- tilgangsforespørsler og invitasjoner
- Basic Authentication for Akkordia
- `.htpasswd`
- `.htgroup`
- Akkordia-brukere og bandgrupper
- wrappers for administrasjon av htpasswd/htgroup
- Akkordia-spesifikk sudoers-konfigurasjon
- Node launchd-tjenesten
- eventuell gammel Python-app/backend
- app-spesifikke logger
- SMTP-/e-postkonfigurasjon som lå i Node-appens `.env`

Det nye PWA-oppsettet og dets data er dermed uavhengig av denne serveren.

---

## 3. Komponenter som fortsatt skal finnes

### 3.1 Apache HTTP Server

Webserveren er Homebrew-versjonen av Apache HTTP Server.

Historisk konfigurasjonsplassering:

```text
/usr/local/etc/httpd/httpd.conf
```

SSL-konfigurasjon:

```text
/usr/local/etc/httpd/extra/httpd-ssl.conf
```

DocumentRoot:

```text
/usr/local/var/www
```

Apache lytter på:

```text
HTTP:  8080
HTTPS: 8443
```

Dette gjør at Apache kan kjøre uten å binde direkte til de privilegerte portene 80 og 443.

### 3.2 macOS PF

PF brukes til lokal portvideresending:

```text
TCP 80  -> TCP 8080
TCP 443 -> TCP 8443
```

Dette er fortsatt nødvendig så lenge Apache kjører på 8080/8443.

PF skal derfor **ikke** avinstalleres som del av Akkordia-oppryddingen.

### 3.3 PF ved oppstart

Følgende LaunchDaemon kan beholdes:

```text
/Library/LaunchDaemons/com.longfjeld.pfboot.plist
```

Formålet er å sørge for at PF er aktivert og konfigurasjonen lastes etter omstart.

Dette er nå generell webserver-infrastruktur og ikke Akkordia-funksjonalitet.

### 3.4 Let's Encrypt / Certbot

HTTPS skal fortsatt brukes for hovednettstedet.

Tidligere dokumentert sertifikatplassering:

```text
/etc/letsencrypt/live/www.longfjeld.com/fullchain.pem
/etc/letsencrypt/live/www.longfjeld.com/privkey.pem
```

Sertifikatet omfatter:

```text
www.longfjeld.com
longfjeld.com
```

Certbot-oppsettet skal beholdes slik at sertifikatet fortsatt fornyes.

Tidligere dokumentert fornyelsesscript:

```text
/usr/local/bin/certbot-renew-and-restart-httpd.sh
```

og LaunchDaemon:

```text
/Library/LaunchDaemons/com.longfjeld.certbot-renew.plist
```

Disse er generell HTTPS-infrastruktur og skal derfor ikke fjernes sammen med Akkordia.

---

## 4. Webinnhold

Alt innhold som fortsatt skal publiseres ligger under:

```text
/usr/local/var/www/
```

Hovedsiden er normalt:

```text
/usr/local/var/www/index.html
```

Eksempel:

```text
/usr/local/var/www/
├── index.html
├── style.css
├── script.js
├── bilder/
│   └── ...
└── underside/
    └── index.html
```

En katalog som:

```text
/usr/local/var/www/om/
```

med:

```text
/usr/local/var/www/om/index.html
```

vil normalt være tilgjengelig som:

```text
https://www.longfjeld.com/om/
```

Serveren trenger ingen Node-, Python-, PHP- eller databasekomponent for slike sider.

---

## 5. Filrettigheter

Statiske webfiler må være lesbare for Apache.

En enkel baseline er:

```bash
sudo find /usr/local/var/www -type d -exec chmod 755 {} \;
sudo find /usr/local/var/www -type f -exec chmod 644 {} \;
```

Dette skal brukes med omtanke dersom det senere legges andre typer data under DocumentRoot.

Det anbefales å holde **kun offentlig webinnhold** under:

```text
/usr/local/var/www
```

Sensitive filer, passord, databaser og konfigurasjonshemmeligheter skal ikke lagres der.

---

## 6. Nettverksflyt

Ekstern trafikk følger denne kjeden:

```text
Klient
  |
  | https://www.longfjeld.com:443
  v
Ruter/NAT
  |
  | TCP 443 til serveren
  v
macOS PF
  |
  | rdr 443 -> 8443
  v
Apache
  |
  | TLS + statisk fil
  v
/usr/local/var/www
```

HTTP følger tilsvarende:

```text
80 -> 8080
```

Apache kan eventuelt videresende HTTP til HTTPS avhengig av gjeldende konfigurasjon.

---

## 7. Porter

Etter oppryddingen forventes:

| Port | Prosess/funksjon | Status |
|---|---|---|
| 80 | PF redirect | Beholdes |
| 443 | PF redirect | Beholdes |
| 8080 | Apache HTTP | Beholdes |
| 8443 | Apache HTTPS | Beholdes |
| 3000 | Tidligere Node/Akkordia | Skal ikke lytte |
| 8000 | Tidligere eventuell Python-app | Skal ikke lytte |

Kontroller med:

```bash
sudo lsof -nP -iTCP:8080 -sTCP:LISTEN
sudo lsof -nP -iTCP:8443 -sTCP:LISTEN
sudo lsof -nP -iTCP:3000 -sTCP:LISTEN
sudo lsof -nP -iTCP:8000 -sTCP:LISTEN
```

Forventet resultat:

- 8080: Apache
- 8443: Apache
- 3000: ingen prosess
- 8000: ingen prosess

---

## 8. Kontroll av PF

Vis status:

```bash
sudo pfctl -s info | head -n 10
```

Vis NAT/redirect-regler:

```bash
sudo pfctl -sn
```

Det relevante sluttbildet er redirect av:

```text
80 -> 8080
443 -> 8443
```

Det skal ikke være nødvendig med PF-regler for Node-port 3000 eller en Python-backend på 8000.

### Test konfigurasjonen før reload

```bash
sudo pfctl -nf /etc/pf.conf
```

Hvis testen er OK:

```bash
sudo pfctl -f /etc/pf.conf
```

Hvis PF ikke er aktiv:

```bash
sudo pfctl -e
```

---

## 9. Kontroll av Apache

### Syntax-test

Kjør alltid før reload:

```bash
sudo apachectl -t
```

Forventet:

```text
Syntax OK
```

### Reload uten full stopp

```bash
sudo apachectl graceful
```

### Virtual Hosts

```bash
sudo apachectl -S
```

eller:

```bash
sudo apachectl -t -D DUMP_VHOSTS
```

### Kontroller lytteporter

```bash
sudo lsof -nP -iTCP:8080 -sTCP:LISTEN
sudo lsof -nP -iTCP:8443 -sTCP:LISTEN
```

---

## 10. Kontroll av nettstedet

Fra selve serveren:

```bash
curl -I http://127.0.0.1:8080/
```

HTTPS:

```bash
curl -k -I https://127.0.0.1:8443/
```

Fra en annen maskin:

```bash
curl -I https://www.longfjeld.com/
```

Eventuelt:

```bash
curl -I https://longfjeld.com/
```

Kontroller også nettstedet i en vanlig nettleser.

---

## 11. Kontroll etter omstart

Etter reboot bør følgende kontrolleres i denne rekkefølgen.

### 1. PF

```bash
sudo pfctl -s info | head -n 5
sudo pfctl -sn
```

PF skal være aktiv, og redirect for 80/443 skal være til stede.

### 2. Apache

```bash
sudo lsof -nP -iTCP:8080 -sTCP:LISTEN
sudo lsof -nP -iTCP:8443 -sTCP:LISTEN
```

### 3. Gamle backend-porter

```bash
sudo lsof -nP -iTCP:3000 -sTCP:LISTEN
sudo lsof -nP -iTCP:8000 -sTCP:LISTEN
```

Begge skal være tomme.

### 4. Lokal HTTPS-test

```bash
curl -k -I https://127.0.0.1:8443/
```

### 5. Ekstern test

```bash
curl -I https://www.longfjeld.com/
```

---

## 12. Kontroll av at Akkordia-backend faktisk er borte

Følgende skal normalt **ikke** eksistere etter avviklingen:

```text
/usr/local/var/www/akkordia/
/usr/local/var/www/akkordia-data/
/usr/local/webapps/nodeapp/
/usr/local/webapps/pyapp/
/usr/local/etc/httpd/.htpasswd
/usr/local/etc/httpd/.htgroup
/usr/local/sbin/akkordia-htpasswd
/usr/local/sbin/akkordia-htgroup
/etc/sudoers.d/akkordia
/Library/LaunchDaemons/com.longfjeld.nodeapp.plist
/Library/LaunchDaemons/com.longfjeld.pyapp.plist
```

Kontroll:

```bash
for p in \
  /usr/local/var/www/akkordia \
  /usr/local/var/www/akkordia-data \
  /usr/local/webapps/nodeapp \
  /usr/local/webapps/pyapp \
  /usr/local/etc/httpd/.htpasswd \
  /usr/local/etc/httpd/.htgroup \
  /usr/local/sbin/akkordia-htpasswd \
  /usr/local/sbin/akkordia-htgroup \
  /etc/sudoers.d/akkordia \
  /Library/LaunchDaemons/com.longfjeld.nodeapp.plist \
  /Library/LaunchDaemons/com.longfjeld.pyapp.plist
do
  if [ -e "$p" ]; then
    echo "FINNES FORTSATT: $p"
  else
    echo "OK - borte: $p"
  fi
done
```

---

## 13. Kontroll av Apache for gamle appregler

Etter oppryddingen bør aktiv Apache-konfigurasjon ikke inneholde Akkordia-spesifikke regler som:

```text
/akkordia
/api
/app
ProxyPass til 127.0.0.1:3000
AuthUserFile
AuthGroupFile
X-Remote-User
```

Søk:

```bash
sudo grep -RniE \
'akkordia|127\.0\.0\.1:3000|ProxyPass|AuthUserFile|AuthGroupFile|X-Remote-User' \
/usr/local/etc/httpd
```

Et treff på `ProxyPass` er ikke automatisk feil dersom reverse proxy senere brukes til noe annet. Etter den beskrevne forenklingen forventes imidlertid normalt ingen reverse proxy.

---

## 14. Logger

Apache sine tidligere dokumenterte logger er:

```text
/usr/local/var/log/httpd/error_log
/usr/local/var/log/httpd/access_log
```

Ved problemer:

```bash
tail -n 100 /usr/local/var/log/httpd/error_log
```

og:

```bash
tail -n 100 /usr/local/var/log/httpd/access_log
```

Gamle Node-/Python-logger har ingen funksjon etter avviklingen og kan slettes.

---

## 15. Sertifikatfornyelse

Kontroller installerte sertifikater:

```bash
sudo certbot certificates
```

En trygg test av fornyelsen kan normalt gjøres med:

```bash
sudo certbot renew --dry-run
```

Hvis det eksisterende oppsettet bruker:

```text
/usr/local/bin/certbot-renew-and-restart-httpd.sh
```

bør dette scriptet fortsatt sørge for at Apache får lest inn det nye sertifikatet etter vellykket fornyelse.

Kontroller LaunchDaemon:

```bash
sudo launchctl print system/com.longfjeld.certbot-renew
```

dersom denne fortsatt er installert med label:

```text
com.longfjeld.certbot-renew
```

---

## 16. Enkel publisering av nye sider

For en ny underside:

```bash
sudo mkdir -p /usr/local/var/www/testside
sudo nano /usr/local/var/www/testside/index.html
```

Eksempel:

```html
<!doctype html>
<html lang="no">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Testside</title>
</head>
<body>
  <h1>Testside</h1>
  <p>Denne siden serveres statisk av Apache.</p>
</body>
</html>
```

Siden vil da kunne nås på:

```text
https://www.longfjeld.com/testside/
```

Ingen Apache-restart er nødvendig når bare statiske filer endres.

---

## 17. Backup

Etter forenklingen er backupbehovet betydelig mindre.

Det viktigste er:

### Webinnhold

```text
/usr/local/var/www/
```

### Apache-konfigurasjon

```text
/usr/local/etc/httpd/
```

### PF-konfigurasjon

```text
/etc/pf.conf
```

samt eventuelle egne PF-anchor-filer som faktisk brukes.

### LaunchDaemons

Relevant generell serverkonfigurasjon kan blant annet omfatte:

```text
/Library/LaunchDaemons/com.longfjeld.pfboot.plist
/Library/LaunchDaemons/com.longfjeld.certbot-renew.plist
```

Let's Encrypt-materiale kan også inngå i systembackup, men private nøkler skal behandles som sensitive data.

Det er ikke lenger behov for backup av Akkordia-database eller serverbaserte Akkordia JSON-data fordi disse komponentene er avviklet.

---

## 18. Feilsøking

### Nettstedet svarer ikke eksternt

Kontroller først:

```bash
sudo pfctl -s info | head -n 5
sudo pfctl -sn
```

Deretter:

```bash
sudo lsof -nP -iTCP:8080 -sTCP:LISTEN
sudo lsof -nP -iTCP:8443 -sTCP:LISTEN
```

Hvis Apache virker lokalt:

```bash
curl -k -I https://127.0.0.1:8443/
```

men ikke eksternt, ligger problemet sannsynligvis før Apache, eksempelvis PF, lokal nettverkskonfigurasjon eller ruter/NAT.

### HTTP virker, men HTTPS virker ikke

Kontroller:

```bash
sudo apachectl -t
sudo apachectl -S
```

og:

```bash
sudo lsof -nP -iTCP:8443 -sTCP:LISTEN
```

Kontroller deretter sertifikatet:

```bash
sudo certbot certificates
```

### Apache gir 403

Kontroller fil- og katalogrettigheter:

```bash
ls -la /usr/local/var/www
```

For vanlig offentlig statisk innhold kan baseline gjenopprettes med:

```bash
sudo find /usr/local/var/www -type d -exec chmod 755 {} \;
sudo find /usr/local/var/www -type f -exec chmod 644 {} \;
```

### Apache-konfigurasjonen er endret

Test alltid først:

```bash
sudo apachectl -t
```

og bruk deretter:

```bash
sudo apachectl graceful
```

---

## 19. Sikkerhetsmodell etter forenklingen

Den gamle løsningen hadde serverbasert:

- brukerautentisering
- passord
- grupper
- API
- Node-runtime
- SMTP-legitimasjon
- SQLite
- JSON-data
- skriveoperasjoner fra klienter

Alt dette økte angrepsflaten.

Den gjenværende løsningen er i prinsippet en **read-only statisk webserver** fra Internett-perspektiv.

Det betyr at serveren ikke skal tilby noen funksjon for eksterne brukere til å:

- opprette konto
- logge inn i Akkordia
- skrive data
- laste opp sanger
- endre JSON
- kalle Akkordia API
- administrere brukere eller band

Den viktigste sikkerhetsoppgaven blir derfor å holde:

- macOS
- Homebrew Apache
- Certbot og øvrige nødvendige pakker

oppdatert, og å unngå å legge sensitive filer i DocumentRoot.

---

## 20. Ønsket sluttstatus

En rask sluttsjekk:

```text
[OK] www.longfjeld.com viser statisk hovedside
[OK] HTTPS fungerer
[OK] Let's Encrypt-sertifikat er gyldig
[OK] Apache lytter på 8080 og 8443
[OK] PF videresender 80 -> 8080
[OK] PF videresender 443 -> 8443
[OK] PF starter igjen etter reboot
[OK] Certbot-fornyelse er beholdt
[OK] Node lytter ikke på 3000
[OK] Python-backend lytter ikke på 8000
[OK] /akkordia er fjernet
[OK] /akkordia-data er fjernet
[OK] SQLite-data er fjernet
[OK] Akkordia JSON-data er fjernet
[OK] .htpasswd og .htgroup er fjernet
[OK] Akkordia launchd-jobber er fjernet
[OK] Akkordia reverse proxy er fjernet
[OK] Akkordia BasicAuth er fjernet
```

---

## 21. Kort driftsprosedyre

For vanlig drift er dette i praksis nok:

### Kontroller webserver

```bash
sudo apachectl -t
sudo lsof -nP -iTCP:8080 -sTCP:LISTEN
sudo lsof -nP -iTCP:8443 -sTCP:LISTEN
```

### Kontroller PF

```bash
sudo pfctl -s info | head -n 5
sudo pfctl -sn
```

### Test nettsted

```bash
curl -I https://www.longfjeld.com/
```

### Etter endring i Apache-konfigurasjon

```bash
sudo apachectl -t && sudo apachectl graceful
```

### Etter endring i vanlige HTML/CSS/JS-filer

Ingen restart er nødvendig.

---

# Oppsummering

Serveren er etter avviklingen av Akkordia-backenden redusert til en tradisjonell statisk webserver:

```text
PF -> Apache -> statiske filer
```

PF håndterer standardportene 80 og 443, Apache håndterer HTTP/HTTPS på 8080/8443, og Let's Encrypt/Certbot sørger for TLS-sertifikatet.

Det finnes ikke lenger noen serverbasert Akkordia-database, brukeradministrasjon, API eller appdata på denne serveren.
