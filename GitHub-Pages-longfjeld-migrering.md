# Migrering av longfjeld.com til GitHub Pages + iCloud Mail

> **Oppdatert:** 2026-09-13  
> **Mål:** Flytte `www.longfjeld.com` og `longfjeld.com` fra egen webserver til GitHub Pages, beholde iCloud Mail for domenet, og opprette den nye adressen `oppskrifter@longfjeld.com`.

---

# 1. Målbilde

Etter migreringen skal løsningen se slik ut:

```text
                        +----------------------+
www.longfjeld.com ----->|                      |
longfjeld.com --------->|    GitHub Pages      |
                        |  statisk nettsted    |
                        +----------------------+

longfjeld.com MX ------> iCloud Mail
                        |
                        +--> eksisterende adresser
                        +--> oppskrifter@longfjeld.com
```

Det betyr at:

- GitHub Pages publiserer nettsiden.
- GitHub håndterer HTTPS for nettsiden.
- iCloud Mail fortsetter å håndtere e-post for `@longfjeld.com`.
- Den gamle Mac-serveren er ikke lenger nødvendig for webpublisering.
- Apache, PF, Certbot og port forwarding hjemme kan avvikles når migreringen er verifisert.

---

# 2. Viktig før du begynner

## Ikke slett e-postrelaterte DNS-poster

Når nettstedet flyttes til GitHub Pages, skal du **ikke** fjerne eller endre eksisterende:

- MX-poster for iCloud Mail
- SPF-relaterte TXT-poster
- DKIM-relaterte CNAME/TXT-poster
- Apple-verifikasjonsposter
- andre DNS-poster som iCloud har bedt deg opprette

Webtrafikk og e-post styres av forskjellige DNS-poster.

Det er derfor helt normalt at for eksempel:

```text
www.longfjeld.com    CNAME  -> GitHub
longfjeld.com        A/AAAA -> GitHub

longfjeld.com        MX     -> iCloud
```

eksisterer samtidig.

---

# 3. Før migreringen – ta vare på nettstedet

Finn filene som faktisk skal beholdes fra den gamle serveren.

Den tidligere DocumentRoot var:

```text
/usr/local/var/www
```

Kopier bare innholdet som fortsatt hører til den enkle nettsiden.

Ikke ta med gammel Akkordia-funksjonalitet som:

```text
akkordia/
akkordia-data/
nodeapp/
.htpasswd
.htgroup
```

En enkel lokal struktur kan eksempelvis være:

```text
www.longfjeld.com/
├── index.html
├── style.css
├── bilder/
│   └── ...
├── oppskrifter/
│   └── index.html
└── annet/
    └── index.html
```

Kontroller lokalt at alle lenker, CSS-filer og bilder bruker relative eller korrekte absolutte stier.

---

# 4. Opprett GitHub-repository

Logg inn på GitHub med kontoen som skal eie nettstedet.

I dette eksempelet brukes GitHub-brukeren:

```text
Longfjeld
```

Opprett et nytt repository.

Et passende navn kan være:

```text
longfjeld.com
```

Repository kan for eksempel bli:

```text
Longfjeld/longfjeld.com
```

For enklest bruk av GitHub Pages kan repository være offentlig.

Legg nettsidefilene i roten av repository:

```text
index.html
style.css
bilder/
...
```

Det må finnes en:

```text
index.html
```

i publiseringsroten.

---

# 5. Last opp nettstedet

Du kan bruke GitHub-nettsiden eller Git.

Eksempel med Git fra en lokal mappe:

```bash
cd /Users/persteinar/Library/Mobile\ Documents/com\~apple\~CloudDocs/Koding/GitHub/www.longfjeld.com

git init
git add .
git commit -m "Initial longfjeld.com website"
git branch -M main
git remote add origin https://github.com/Longfjeld/longfjeld.com.git
git push -u origin main
```

Tilpass repository-URL dersom du velger et annet navn.

---

# 6. Aktiver GitHub Pages

I repository:

```text
Settings
  -> Pages
```

Under **Build and deployment**:

```text
Source:
Deploy from a branch
```

Velg:

```text
Branch: main
Folder: / (root)
```

Lagre.

GitHub publiserer deretter nettstedet på en midlertidig GitHub Pages-adresse, typisk:

```text
https://longfjeld.github.io/longfjeld.com/
```

Test denne adressen før DNS endres.

Kontroller blant annet:

- hovedside
- undersider
- bilder
- CSS
- JavaScript
- interne lenker

Ikke gå videre før GitHub Pages-versjonen fungerer.

---

# 7. Verifiser domenet hos GitHub

GitHub anbefaler at domenet verifiseres før det brukes som custom domain.

Gå til GitHub-kontoens:

```text
Settings
  -> Pages
  -> Verified domains
```

Velg:

```text
Add a domain
```

Legg inn:

```text
longfjeld.com
```

GitHub viser nå en TXT-post som skal legges inn hos DNS-leverandøren.

Den vil ha et navn omtrent som:

```text
_github-pages-challenge-Longfjeld.longfjeld.com
```

og en GitHub-generert TXT-verdi.

**Bruk den eksakte verdien GitHub viser.**

Etter at posten er opprettet, kan den kontrolleres fra Terminal:

```bash
dig _github-pages-challenge-Longfjeld.longfjeld.com TXT
```

Når GitHub har godkjent domenet:

```text
Verified
```

bør TXT-posten beholdes permanent.

---

# 8. Angi custom domain i GitHub Pages

Gå tilbake til repository:

```text
Longfjeld/longfjeld.com
```

og:

```text
Settings
  -> Pages
```

Under **Custom domain**, skriv:

```text
www.longfjeld.com
```

Lagre.

Jeg anbefaler `www.longfjeld.com` som primæradresse.

GitHub anbefaler selv `www`-subdomene fordi dette bruker CNAME og er mindre avhengig av GitHubs IP-adresser.

Når både apex-domenet og `www` er konfigurert korrekt i DNS, kan GitHub håndtere redirect mellom dem.

---

# 9. DNS for www.longfjeld.com

Hos DNS-leverandøren oppretter eller endrer du:

```text
Type:   CNAME
Name:   www
Target: longfjeld.github.io
```

Noen DNS-leverandører vil vise hele navnet som:

```text
www.longfjeld.com
```

Target skal være GitHub-brukerens Pages-domene:

```text
longfjeld.github.io
```

Ikke bruk repository-stien:

```text
longfjeld.github.io/longfjeld.com
```

i CNAME-posten.

CNAME peker bare på vertsnavnet.

---

# 10. DNS for longfjeld.com uten www

For apex/root-domenet:

```text
longfjeld.com
```

kan GitHub Pages bruke A-poster.

Opprett følgende fire A-poster:

```text
Type   Name   Value
A      @      185.199.108.153
A      @      185.199.109.153
A      @      185.199.110.153
A      @      185.199.111.153
```

Disse er GitHub Pages sine dokumenterte IPv4-adresser per september 2026.

Hvis DNS-leverandøren støtter IPv6, kan du i tillegg bruke:

```text
Type   Name   Value
AAAA   @      2606:50c0:8000::153
AAAA   @      2606:50c0:8001::153
AAAA   @      2606:50c0:8002::153
AAAA   @      2606:50c0:8003::153
```

Alternativt kan enkelte DNS-leverandører bruke ALIAS/ANAME for apex.

For en enkel og leverandøruavhengig konfigurasjon er GitHubs A-poster et godt valg.

---

# 11. Fjern gamle webserver-poster

Når GitHub Pages er konfigurert, skal gamle DNS-poster som sender webtrafikken til hjemme-serverens offentlige IP fjernes.

Se spesielt etter gamle:

```text
A
AAAA
CNAME
```

for:

```text
@
www
```

som peker til den gamle serveren.

Det skal ikke stå både gamle og nye A/AAAA-poster parallelt, fordi klienter da kan treffe feil server.

## Ikke berør MX/TXT for iCloud

Mens du gjør dette:

**Ikke slett e-postpostene.**

DNS-sonen vil etter migreringen typisk inneholde både GitHub- og Apple-relaterte poster.

---

# 12. Kontroller DNS

På Mac kan du kontrollere `www`:

```bash
dig www.longfjeld.com CNAME
```

Forventet target:

```text
longfjeld.github.io.
```

Kontroller apex:

```bash
dig longfjeld.com A
```

Forventet:

```text
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Hvis IPv6 er konfigurert:

```bash
dig longfjeld.com AAAA
```

---

# 13. Kontroller at iCloud Mail fortsatt er intakt

Kontroller MX:

```bash
dig longfjeld.com MX
```

Disse postene skal fortsatt være de iCloud Mail-oppsettet ditt bruker.

Du kan også vise TXT-postene:

```bash
dig longfjeld.com TXT
```

Poenget er:

```text
web-DNS -> GitHub
mail-DNS -> Apple/iCloud
```

De to løsningene skal eksistere samtidig.

---

# 14. Vent på GitHub DNS-sjekk

Gå tilbake til:

```text
Repository
  -> Settings
  -> Pages
```

GitHub vil kontrollere DNS-konfigurasjonen.

Når den er godkjent skal custom domain vise:

```text
www.longfjeld.com
```

DNS-endringer kan være raske, men full propagasjon kan ta tid avhengig av tidligere TTL.

---

# 15. Aktiver HTTPS

Når GitHub har godkjent DNS og fått utstedt sertifikat, aktiver:

```text
Enforce HTTPS
```

under:

```text
Settings -> Pages
```

Deretter skal:

```text
http://www.longfjeld.com
```

ende på:

```text
https://www.longfjeld.com
```

Test:

```bash
curl -I https://www.longfjeld.com/
```

og:

```bash
curl -I https://longfjeld.com/
```

Begge skal til slutt fungere.

---

# 16. Kontroller mixed content

Når HTTPS er aktivert, må nettsiden ikke laste ressurser eksplisitt over HTTP.

Unngå for eksempel:

```html
<script src="http://..."></script>
<link rel="stylesheet" href="http://...">
<img src="http://...">
```

Bruk:

```text
https://
```

eller relative lenker.

---

# 17. Opprett oppskrifter@longfjeld.com i iCloud

Domenet `longfjeld.com` er allerede ment å fortsette som et iCloud Mail custom email domain.

Hvis domenet allerede er korrekt konfigurert i iCloud, trenger du **normalt ingen nye DNS-endringer** bare for å lage en ny adresse.

## Logg inn

Gå til:

```text
https://www.icloud.com/
```

og åpne:

```text
iCloud+
  -> Custom Email Domain
```

Velg:

```text
longfjeld.com
```

---

# 18. Legg til den nye adressen

Finn personen/Apple-kontoen som skal bruke adressen.

Velg å legge til en e-postadresse.

Opprett:

```text
oppskrifter@longfjeld.com
```

Apple tillater per september 2026 opptil tre aktive e-postadresser per person per custom domain.

Hvis kontoen allerede bruker tre adresser på `longfjeld.com`, må en av dem eventuelt fjernes eller adressen knyttes til en annen person som domenet deles med.

Fullfør oppsettet i iCloud.

---

# 19. Test den nye e-postadressen

Etter opprettelsen:

1. Send en e-post fra en ekstern konto til:

```text
oppskrifter@longfjeld.com
```

2. Kontroller at den mottas i iCloud Mail.

3. Send deretter en melding **fra**:

```text
oppskrifter@longfjeld.com
```

til en ekstern adresse.

4. Kontroller at avsenderen vises korrekt.

På Apple-enheter der iCloud Mail er aktivert skal den nye adressen bli tilgjengelig som avsenderadresse.

---

# 20. Hvis longfjeld.com ikke allerede er konfigurert som iCloud Custom Email Domain

Dette punktet er bare relevant dersom domenet mot formodning ikke allerede er fullført hos Apple.

Gå til:

```text
iCloud+
  -> Custom Email Domain
  -> Add a domain you own
```

Legg inn:

```text
longfjeld.com
```

Apple vil deretter vise hvilke DNS-poster som kreves.

**Bruk Apples aktuelle verdier som vises i oppsettet.**

Ikke gjett eller kopier gamle MX/TXT-verdier fra en annen veiledning.

Apple kan kreve blant annet:

- MX
- TXT
- DKIM-relaterte poster
- verifikasjonspost

Disse kan sameksistere med GitHub Pages sine A/CNAME-poster.

---

# 21. Anbefalt DNS-sluttbilde

Prinsipielt bør DNS se omtrent slik ut:

```text
# WEB

www    CNAME   longfjeld.github.io

@      A       185.199.108.153
@      A       185.199.109.153
@      A       185.199.110.153
@      A       185.199.111.153

# Eventuelt IPv6

@      AAAA    2606:50c0:8000::153
@      AAAA    2606:50c0:8001::153
@      AAAA    2606:50c0:8002::153
@      AAAA    2606:50c0:8003::153

# GITHUB DOMAIN VERIFICATION

_github-pages-challenge-Longfjeld
       TXT     <verdien GitHub oppgir>

# MAIL

@      MX      <eksisterende iCloud MX-poster>
@      TXT     <eksisterende iCloud/SPF/verifikasjonsposter>
...            <eksisterende Apple DKIM-/andre poster>
```

**Ikke erstatt Apple-postene med eksempler. Behold de fungerende verdiene som allerede finnes.**

---

# 22. Full funksjonstest før gammel server slås av

Test først i vanlig nettleser:

```text
https://www.longfjeld.com
```

Deretter:

```text
https://longfjeld.com
```

Test alle aktuelle undersider.

Kontroller så:

```bash
curl -I https://www.longfjeld.com/
curl -I https://longfjeld.com/
```

Kontroller DNS:

```bash
dig www.longfjeld.com CNAME
dig longfjeld.com A
dig longfjeld.com MX
dig longfjeld.com TXT
```

Test e-post begge veier:

```text
ekstern adresse -> oppskrifter@longfjeld.com
oppskrifter@longfjeld.com -> ekstern adresse
```

---

# 23. Slå av gammel webserver som test

Når alt ovenfor fungerer:

1. Stopp den gamle Mac-serveren helt.
2. Test nettsiden fra en annen Internett-forbindelse, for eksempel mobilnett.
3. Test både `www.longfjeld.com` og `longfjeld.com`.
4. Test e-post igjen.

Hvis alt fortsatt fungerer, har du bekreftet at ingen produksjonsfunksjon lenger er avhengig av serveren.

---

# 24. Deretter kan Mac-en reinstalles

Når GitHub Pages og iCloud Mail er verifisert, kan den gamle Intel-Mac-en resettes uten å gjenoppbygge webservermiljøet.

Du trenger ikke lenger:

- Homebrew Apache
- PF-portredirect for web
- port forwarding i ruteren
- Certbot
- Let's Encrypt på Mac-en
- Node
- Express
- SQLite
- Akkordia backend
- webserver launchd-jobber
- lokal webserverbackup

GitHub håndterer webpublisering og TLS.

Apple håndterer e-post.

---

# 25. Fremtidig publisering

Etter migreringen oppdateres nettstedet ved å endre filene i GitHub-repository.

Eksempel:

```bash
git add .
git commit -m "Oppdater nettsiden"
git push
```

GitHub Pages bygger/publiserer endringen automatisk.

Det er ingen server du må logge inn på, ingen Apache som skal reloades og ingen sertifikater du selv må fornye.

---

# 26. Anbefalt endelig arkitektur

```text
                     DNS
                      |
          +-----------+-----------+
          |                       |
          v                       v
     GitHub Pages             iCloud Mail
          |                       |
          |                       |
 www.longfjeld.com        @longfjeld.com
 longfjeld.com                   |
          |                 +----+------------------+
          |                 |                       |
  HTML / CSS / JS      eksisterende adresser   oppskrifter@
```

Dette er den anbefalte sluttløsningen fordi den fjerner behovet for å drifte en Internett-eksponert Intel-Mac bare for å levere statiske nettsider.
