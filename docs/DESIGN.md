# Design – Minimal Glass

## 1. Mål

`longfjeld` er overordnet branding. Prosjektene skal være hovedinnholdet.

Designet skal derfor være:

- enkelt
- lyst
- rolig
- moderne
- responsivt
- tydelig uten å være sterkt dekorativt

## 2. Typografi

`longfjeld` bruker **Cormorant Garamond**. Fonten er valgt fordi den gir ønsket karakter og ligaturer, blant annet i `fj`-kombinasjonen.

Resten av grensesnittet bruker systemfont:

```css
-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif
```

Dette gir god lesbarhet og lar `longfjeld` beholde sin egen typografiske identitet.

## 3. Glass-effekten

Glass brukes med måte.

Det viktigste er:

- svak transparens
- moderat `backdrop-filter: blur(...)`
- lyse, tynne kanter
- svært diskrete skygger

Store deler av innholdet ligger direkte på bakgrunnen. Dette hindrer at nettstedet ser ut som en samling med UI-kort.

## 4. Bakgrunn

Bakgrunnen består av:

1. en lys, nøytral gradient
2. en diffus kjølig glød
3. en diffus varm glød

Glødene gir glassflatene noe å filtrere uten å konkurrere med teksten.

## 5. Hierarki

På forsiden er rekkefølgen:

```text
longfjeld
prosjektnavigasjon
felles introduksjon
prosjektinnganger
```

På prosjektsider er rekkefølgen:

```text
longfjeld
prosjektnavigasjon
prosjektnavn
kort forklaring
nøkkelord
utdypende seksjoner
```

## 6. Responsivitet

Det brukes vanlig CSS med ett hovedbrekkpunkt ved `760px`.

På mindre skjermer:

- headeren går fra horisontal til vertikal
- prosjektnavigasjonen blir tre like brede felt
- prosjektinngangene på forsiden går fra tre kolonner til én
- tekststørrelser reduseres gradvis med `clamp()`

Det finnes ikke egne mobilfiler eller separat mobilside.

## 7. Tilgjengelighet

Løsningen bruker:

- semantisk HTML
- vanlig lenkenavigasjon
- synlig tastaturfokus
- `aria-current="page"` på aktivt prosjekt
- støtte for `prefers-reduced-motion`
- forbedret kontrast ved `prefers-contrast: more`

## 8. Prinsipp for videre utvikling

Nye visuelle effekter bør bare legges til dersom de forbedrer forståelse eller navigasjon.

Unngå spesielt:

- mange glasskort
- sterke skygger
- kontinuerlige animasjoner
- dekorasjon som konkurrerer med prosjektene
