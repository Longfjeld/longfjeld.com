# Publisering til GitHub Pages

Denne veiledningen forutsetter at eksisterende repository og GitHub Pages-oppsett allerede er etablert.

Lokal arbeidskatalog:

```text
/Users/persteinar/Library/Mobile Documents/com~apple~CloudDocs/Koding/GitHub/www.longfjeld.com
```

## 1. Gå til prosjektkatalogen

```bash
cd "/Users/persteinar/Library/Mobile Documents/com~apple~CloudDocs/Koding/GitHub/www.longfjeld.com"
```

## 2. Kontroller hvilken branch du står på

```bash
git branch --show-current
```

For dagens oppsett skal publiseringsbranchen normalt være:

```text
main
```

## 3. Kontroller status før du gjør noe mer

```bash
git status
```

Les resultatet før du fortsetter. Ikke overskriv lokale endringer du ønsker å beholde.

## 4. Kontroller `CNAME`

Filen:

```text
CNAME
```

skal finnes i roten dersom GitHub Pages publiserer fra branch og custom domain er satt til `www.longfjeld.com`.

Innholdet i denne leveransen er:

```text
www.longfjeld.com
```

Ikke legg både `www.longfjeld.com` og `longfjeld.com` i samme `CNAME`-fil.

## 5. Forhåndsvis lokalt

Fra prosjektkatalogen:

```bash
python3 -m http.server 8000
```

Åpne:

```text
http://localhost:8000/
```

Kontroller i denne rekkefølgen:

1. Forsiden.
2. `/oppskrifter/`.
3. `/akkordia/`.
4. `/tekstilig/`.
5. Mobilvisning.
6. Navigasjon mellom alle sider.

Stopp serveren med `Ctrl-C`.

## 6. Se hvilke filer som er endret

```bash
git status
```

Valgfritt:

```bash
git diff
```

## 7. Legg endringene til staging

```bash
git add .
```

## 8. Kontroller staging

```bash
git status
```

Kontroller at bare filer som faktisk skal publiseres er med.

## 9. Commit

Eksempel:

```bash
git commit -m "Redesign longfjeld.com with Minimal Glass"
```

## 10. Push til GitHub

```bash
git push origin main
```

Når GitHub Pages er konfigurert med `Deploy from a branch`, vil push til valgt publiseringsbranch utløse ny publisering.

## 11. Kontroller GitHub Pages

I GitHub-repository:

```text
Settings → Pages
```

Kontroller at **Build and deployment** fortsatt peker til:

```text
Source: Deploy from a branch
Branch: main
Folder: / (root)
```

## 12. Kontroller publisert nettsted

Test:

```text
https://www.longfjeld.com/
https://www.longfjeld.com/oppskrifter/
https://www.longfjeld.com/akkordia/
https://www.longfjeld.com/tekstilig/
```

Kontroller både desktop og mobil.

## 13. Custom domain og DNS

Nettstedets web-DNS må være satt opp for GitHub Pages, mens MX/TXT/CNAME-poster som tilhører iCloud Mail skal beholdes uendret.

GitHub anbefaler at custom domain verifiseres i GitHub-kontoens Pages-innstillinger. Dersom dagens custom domain allerede fungerer, er det normalt ikke nødvendig å gjøre DNS-endringer ved vanlige innholdsoppdateringer.

## 14. Viktig om `CNAME`

Ved branch-basert GitHub Pages-publisering må `CNAME`-filen beholdes i publiseringsroten. Hvis den utilsiktet slettes i en senere commit, kan custom domain-oppsettet bli påvirket.

## 15. Ingen build-prosess

Dette prosjektet trenger ikke:

```text
npm
Node.js
GitHub Actions build workflow
Jekyll
```

GitHub Pages kan publisere HTML- og CSS-filene direkte fra repoet.



Shortlist - github kommandoer:


```
git status
git diff (valgfritt)
git add .
git status
git commit -m "Tekst til Commit"
git push origin main
```
Zip fra Commit:

```
git archive --format=zip --output=../longfjeld-current.zip HEAD
```

