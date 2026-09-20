# Migrering fra Tumult Hype til standard HTML/CSS

## 1. Utgangspunkt

Tidligere nettsted var eksportert fra Tumult Hype 4 og bestod blant annet av:

```text
index.html
index.hyperesources/
ww.longfjeld.com.hype/
```

Hype-eksporten brukte en scene på omtrent 600 × 400 og hadde viewport konfigurert med fast bredde.

## 2. Ny modell

Nettstedet er nå bygget som statisk HTML/CSS uten Hype-runtime.

Aktiv struktur er:

```text
index.html
style.css
oppskrifter/index.html
akkordia/index.html
tekstilig/index.html
```

Fordelene er:

- vanlig responsiv webdesign
- direkte URL til hvert prosjekt
- enklere redigering i BBEdit
- mindre kode og færre avhengigheter
- bedre semantikk og tilgjengelighet
- naturlig bruk av GitHub Pages

## 3. Hva som ikke lenger trengs

Følgende filer fra Hype-eksporten trengs ikke for det nye nettstedet:

```text
index.hyperesources/
ww.longfjeld.com.hype/
```

De kan beholdes i Git-historikken, men bør ikke være del av aktiv publisering.

## 4. Første utskifting i eksisterende repo

Gjør dette i denne rekkefølgen:

1. Kontroller at alle eksisterende endringer er commit-et:

   ```bash
   git status
   ```

2. Ta eventuelt en egen sikkerhetsbranch:

   ```bash
   git branch backup-before-html-css-migration
   ```

3. Erstatt innholdet i repoet med filene fra den nye komplette ZIP-en.

4. Kontroller at følgende gamle kataloger er fjernet fra aktiv arbeidskopi:

   ```text
   index.hyperesources/
   ww.longfjeld.com.hype/
   ```

5. Kontroller endringene:

   ```bash
   git status
   git diff --stat
   ```

6. Forhåndsvis lokalt før commit.

7. Commit og push etter veiledningen i `PUBLISERING-GITHUB-PAGES.md`.

## 5. Tilbakerulling

Hvis du ønsker å gå tilbake til Hype-versjonen før den nye løsningen er godkjent, kan du bytte tilbake til sikkerhetsbranchen:

```bash
git switch backup-before-html-css-migration
```

Etter at den nye løsningen er tatt i bruk er vanlig Git-historikk tilstrekkelig for å hente tilbake eldre filer.
