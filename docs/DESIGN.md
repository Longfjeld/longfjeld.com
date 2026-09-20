# Designprinsipper for longfjeld.com

Dette dokumentet er den samlede referansen for visuell utforming, typografi og grafiske prinsipper på longfjeld.com.

Prinsippene bygger på klassisk typografi fra bøker og magasiner, tilpasset webens krav til responsivitet, tilgjengelighet og robust gjengivelse.

## 1. Mål og visuell retning

`longfjeld` er overordnet branding. Prosjektene skal være hovedinnholdet.

Designet skal være:

- enkelt
- lyst
- rolig
- moderne
- responsivt
- tydelig uten å være sterkt dekorativt

Struktur skal først og fremst oppstå gjennom gode typografiske forhold, nærhet og hierarki, ikke gjennom unødvendige grafiske virkemidler.

Forskjeller i størrelse, vekt, avstand, farge og andre virkemidler skal ikke være større enn nødvendig for å gjøre innholdets struktur tydelig.

## 2. Typografisk grunnsystem

### 2.1 Skrifter

`longfjeld` i `.brand` bruker **Cormorant Garamond**. Fonten er valgt fordi den gir ønsket karakter og ligaturer, blant annet i `fj`-kombinasjonen.

Resten av grensesnittet bruker systemfont:

```css
-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif
```

Dette gir god lesbarhet og lar `longfjeld` beholde sin egen typografiske identitet.

### 2.2 Typografisk hierarki

Hierarkiet skal være tydelig, men begrenset.

Det skal brukes:

- få og tydelige typografiske nivåer
- tilstrekkelig kontrast mellom nivåene
- luft der den har en strukturell funksjon
- konsekvente typografiske forhold gjennom hele nettstedet

Forskjeller mellom brødtekst, mellomtitler, hovedtitler og eventuell displaytekst skal være store nok til at strukturen forstås umiddelbart, men ikke større enn nødvendig.

Nye typografiske nivåer skal ikke opprettes dersom eksisterende nivåer kan uttrykke strukturen tilfredsstillende.

Helheten skal oppleves som ett sammenhengende typografisk system.

## 3. Nærhet, kontekst og vertikal rytme

**Fysisk nærhet på siden skal gjenspeile nærhet i innhold og kontekst.**

Elementer som hører sammen, skal visuelt oppfattes som en gruppe. Elementer som representerer et skifte i innhold, nivå eller kontekst, skal ha tilstrekkelig avstand til at skillet oppfattes naturlig.

Avstand skal brukes som et strukturelt virkemiddel, ikke som dekorasjon.

Dette gjelder blant annet:

- overskrift og tilhørende tekst
- tekstlinjer som sammen utgjør ett budskap
- kort og innholdet i kortet
- separate seksjoner
- navigasjon og hovedinnhold

Avstanden mellom tekstlige elementer skal danne et bevisst og konsekvent system. Et generelt CSS-system for spacing kan brukes som utgangspunkt, men skal ikke overstyre typografiske og optiske vurderinger.

**Optisk riktig avstand er viktigere enn matematisk lik avstand.**

## 4. Bokstavmellomrom

Bokstavmellomrom skal vurderes optisk og i sammenheng med:

- skriftgrad
- skrifttype
- vekt
- linjeavstand
- tekstens funksjon
- gjengivelsen på skjerm

Stor skrift skal ikke automatisk få større `letter-spacing`.

Ved store skriftgrader blir de optiske mellomrommene mellom bokstavene tydeligere, og displaytekst kan derfor i enkelte skrifter fungere best med normal eller noe strammere bokstavavstand.

Skrifter med støtte for optisk størrelse (`opsz`) kan håndtere deler av denne tilpasningen selv.

Tracking skal derfor justeres ut fra den faktiske skriften og den visuelle helheten, ikke etter en generell regel om at større skrift krever større bokstavmellomrom.

## 5. Linjeavstand, tekstblokker og linjelengde

Linjeavstand skal vurderes sammen med skriftgrad, linjelengde og tekstens funksjon.

Displaytekst og korte overskrifter kan ha tettere linjeavstand enn brødtekst, men linjene skal ikke oppleves som presset sammen. Brødtekst skal ha tilstrekkelig linjeavstand til komfortabel lesing over flere linjer.

Ved vurdering av en tekstblokk skal følgende sees i sammenheng:

- `font-size`
- `line-height`
- `letter-spacing`
- `font-weight`
- linjelengde
- avstand før og etter blokken

Ingen av disse egenskapene bør justeres isolert dersom problemet gjelder den samlede typografiske balansen.

Brødtekst skal ikke bli unødvendig bred på store skjermer. Korte tekster og overskrifter skal heller ikke få linjebryting som svekker budskapets naturlige rytme.

Linjebryting skal vurderes på flere skjermbredder og ikke optimaliseres utelukkende for én bestemt visning.

## 6. Optiske korreksjoner

CSS gjør det enkelt å bygge matematiske systemer for størrelse og avstand. Slike systemer skal brukes som hjelpemiddel, ikke som fasit.

To matematisk like mellomrom er ikke nødvendigvis optisk like.

Mindre optiske korreksjoner er ønskelige når dette gir bedre:

- balanse
- rytme
- gruppering
- lesbarhet
- visuell sammenheng

Slike avvik skal være bevisste og ikke utvikle seg til en samling tilfeldige enkeltverdier.

## 7. Glass-effekt og flater

Glass brukes med måte.

Det viktigste er:

- svak transparens
- moderat `backdrop-filter: blur(...)`
- lyse, tynne kanter
- svært diskrete skygger

Store deler av innholdet ligger direkte på bakgrunnen. Dette hindrer at nettstedet ser ut som en samling med UI-kort.

Grafiske flater skal støtte innholdets struktur og funksjon, ikke konkurrere med teksten.

## 8. Bakgrunn

Bakgrunnen består av:

1. en lys, nøytral gradient
2. en diffus kjølig glød
3. en diffus varm glød

Glødene gir glassflatene noe å filtrere uten å konkurrere med teksten.

Dekorative elementer skal ikke skape et visuelt hierarki som ikke finnes i innholdet.

## 9. Innholdshierarki

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

Det visuelle hierarkiet skal understøtte denne innholdsrekkefølgen uten å introdusere flere nivåer enn nødvendig.

## 10. Responsivitet

Web har ikke én fast satsflate. Typografien og den grafiske strukturen skal derfor fungere på blant annet:

- telefon
- nettbrett
- bærbar datamaskin
- større skjermer

Det brukes vanlig CSS med ett hovedbrekkpunkt ved `760px`.

På mindre skjermer:

- headeren går fra horisontal til vertikal
- prosjektnavigasjonen blir tre like brede felt
- prosjektinngangene på forsiden går fra tre kolonner til én
- tekststørrelser reduseres gradvis med `clamp()`

Det finnes ikke egne mobilfiler eller separat mobilside.

Skriftgrader, linjelengder, avstander og linjebryting skal vurderes responsivt. Målet er å bevare det samme visuelle hierarkiet og den samme typografiske karakteren på tvers av skjermstørrelser, ikke nødvendigvis identiske mål.

## 11. Tilgjengelighet og robusthet

Løsningen bruker:

- semantisk HTML
- vanlig lenkenavigasjon
- synlig tastaturfokus
- `aria-current="page"` på aktivt prosjekt
- støtte for `prefers-reduced-motion`
- forbedret kontrast ved `prefers-contrast: more`

Typografien skal tåle variasjoner i:

- nettleser
- operativsystem
- skjermtetthet
- font-rendering
- viewport
- zoom og brukerens tekststørrelse

Unødvendig bruk av faste høyder og absolutte plasseringer som kan få tekst eller struktur til å bryte sammen, skal unngås.

Lesbarhet og innholdets struktur skal ha prioritet foran nøyaktig geometrisk gjengivelse.

## 12. Grafiske elementer

De samme prinsippene for hierarki og nærhet gjelder grafisk innhold.

Grafiske virkemidler skal støtte innholdets struktur og funksjon. Dekorative elementer skal ikke konkurrere med teksten eller skape et visuelt hierarki som ikke finnes i innholdet.

Typografi og grafikk skal oppleves som deler av det samme visuelle systemet.

Nye visuelle effekter skal bare legges til dersom de forbedrer forståelse eller navigasjon.

Unngå spesielt:

- mange glasskort
- sterke skygger
- kontinuerlige animasjoner
- dekorasjon som konkurrerer med prosjektene

## 13. Prosjektspesifikke føringer

### 13.1 `.brand`

Utformingen av teksten **«longfjeld»** knyttet til `.brand` beholdes som den er inntil annet besluttes.

Den skal ikke automatisk endres som følge av generelle justeringer i nettstedets øvrige typografi.

### 13.2 Hero-tekst

Teksten:

> Små. Nyttige. Verktøy.

er den valgte hero-teksten på forsiden.

Hero-teksten skal ha tydeligere optisk luft enn den tidligere utformingen. Linjeavstand, bokstavmellomrom, skriftgrad, vekt og avstand til `PROSJEKTER` og ingressen skal vurderes som én komposisjon. Linjebryting skal i utgangspunktet være responsiv og ikke tvinges uten en tydelig typografisk grunn.

Det skal ikke gjøres en isolert endring av `letter-spacing` uten at helheten vurderes.

### 13.3 Felles venstreakse og innholdsbredde

På nettbrett og større skjermer skal `.brand` og hovedinnholdet følge en tydelig felles venstreakse. Dette gjelder blant annet `PROSJEKTER`, hovedoverskrift og prosjektinnhold.

Layouten kan utnytte mer av tilgjengelig bredde enn brødteksten. Brødtekst skal fortsatt holdes på en moderat linjelengde, mens overskrifter, navigasjon og prosjektkort kan bruke en bredere layoutkolonne.

På telefon beholdes den etablerte kompakte layouten så langt som mulig.

### 13.4 Prosjektkort på forsiden

Prosjektkort skal ha konsekvent avstand mellom tittel og beskrivelse. Forskjeller i tekstmengde skal primært gi forskjellig restplass nederst i kortet, ikke forskjellig avstand mellom tittel og tekst.

Beskrivelsen for Tekstilig er:

> Et praktisk kartotek over tekstiler du har på lager.

## 14. Kontroll ved videre utvikling

Ved typografiske eller grafiske endringer skal følgende spørsmål brukes som kontroll:

1. Hva er den innholdsmessige relasjonen mellom elementene?
2. Gjenspeiler den fysiske avstanden denne relasjonen?
3. Er hierarkiet tydelig uten å være sterkere enn nødvendig?
4. Er skriftgrad, linjeavstand, bokstavmellomrom og vekt balansert som en helhet?
5. Fungerer løsningen på både små og store skjermer?
6. Er løsningen robust ved zoom og variasjoner i font-rendering?
7. Er eventuelle avvik fra spacing-systemet typografisk begrunnet?
8. Tilfører grafiske virkemidler struktur eller bare dekorasjon?
9. Forbedrer en ny visuell effekt faktisk forståelse eller navigasjon?

Målet er ikke matematisk ensartethet, men **typografisk sammenheng, lesbarhet, funksjon og ro**.
