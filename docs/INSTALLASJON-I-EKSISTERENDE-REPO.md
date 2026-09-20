# Installere den nye nettsiden i eksisterende Git-repository

Denne veiledningen gjelder første overgang fra den tidligere Hype-eksporten til den nye HTML/CSS-versjonen.

Den lokale Git-katalogen er:

```text
/Users/persteinar/Library/Mobile Documents/com~apple~CloudDocs/Koding/GitHub/www.longfjeld.com
```

Følg punktene i nummerrekkefølge.

## 1. Gå til eksisterende repository

```bash
cd "/Users/persteinar/Library/Mobile Documents/com~apple~CloudDocs/Koding/GitHub/www.longfjeld.com"
```

## 2. Kontroller at du står i riktig Git-repository

```bash
git status
```

Kommandoen skal vise status for nettstedets repository.

## 3. Kontroller at eksisterende arbeid er tatt vare på

Hvis `git status` viser lokale endringer du vil beholde, commit dem før du går videre.

Hvis arbeidskopien er ren, gå videre til punkt 4.

## 4. Opprett en sikkerhetsbranch fra dagens versjon

```bash
git branch backup-before-html-css-migration
```

Dette endrer ikke hvilken branch du står på. Det lager bare et navn som peker på dagens commit.

## 5. Pakk den nye ZIP-filen ut i en midlertidig katalog

Eksempel dersom ZIP-filen ligger i `Downloads`:

```bash
mkdir -p /tmp/longfjeld-new
unzip ~/Downloads/longfjeld-minimal-glass.zip -d /tmp/longfjeld-new
```

Ikke pakk den direkte over repoet ennå.

## 6. Fjern de gamle Hype-filene fra arbeidskopien

Fra repository-katalogen:

```bash
rm -rf index.hyperesources
rm -rf ww.longfjeld.com.hype
```

Disse filene ligger fortsatt i Git-historikken og i sikkerhetsbranchen fra punkt 4.

## 7. Kopier den nye leveransen inn i repository

```bash
cp -R /tmp/longfjeld-new/. .
```

Dette kopierer også skjulte filer som `.nojekyll` og `.gitignore`, men berører ikke repositoryets eksisterende `.git`-katalog fordi ZIP-en ikke inneholder `.git`.

## 8. Kontroller filstrukturen

```bash
find . -maxdepth 2 -type f | sort
```

Du skal blant annet se:

```text
./index.html
./style.css
./oppskrifter/index.html
./akkordia/index.html
./tekstilig/index.html
./404.html
./CNAME
./.nojekyll
./README.md
./docs/...
```

Du skal ikke lenger ha aktive kataloger med navn:

```text
index.hyperesources
ww.longfjeld.com.hype
```

## 9. Kontroller Git-endringene

```bash
git status
```

Du skal forvente:

- endret `index.html`
- slettede Hype-filer
- nye prosjektkataloger
- ny `style.css`
- nye dokumentasjonsfiler
- `CNAME` og `.nojekyll` dersom de ikke allerede fantes

## 10. Forhåndsvis lokalt

```bash
python3 -m http.server 8000
```

Åpne:

```text
http://localhost:8000/
```

Test forsiden og alle tre prosjektsidene.

Stopp serveren med `Ctrl-C`.

## 11. Commit og publiser

Når alt ser riktig ut, fortsett med:

```text
docs/PUBLISERING-GITHUB-PAGES.md
```

## 12. Tilbakerulling dersom du ikke vil beholde migreringen

Før migreringen er publisert kan du forkaste de lokale endringene på vanlig Git-måte.

Hvis migreringen er commit-et og du ønsker å inspisere den gamle versjonen, finnes den på:

```text
backup-before-html-css-migration
```

Ikke slett denne branchen før du er komfortabel med den nye løsningen.
