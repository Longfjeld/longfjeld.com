# Typografiske prinsipper for longfjeld.com

Dette dokumentet beskriver de typografiske og visuelle prinsippene som
skal brukes som referanse ved videre utvikling av longfjeld.com.

Prinsippene bygger på klassisk typografi fra bøker og magasiner,
tilpasset de særlige kravene som gjelder for web.

## 1. Overordnet mål

Typografien skal være enkel, rolig og tydelig.

Struktur skal først og fremst oppstå gjennom gode typografiske forhold,
ikke gjennom unødvendig bruk av grafiske virkemidler.

Det skal brukes:

-   få og tydelige typografiske nivåer
-   tilstrekkelig kontrast mellom nivåene
-   luft der den har en strukturell funksjon
-   konsekvente typografiske forhold gjennom hele nettstedet

Forskjeller i størrelse, vekt, avstand og andre virkemidler skal ikke
være større enn nødvendig for å gjøre innholdets struktur tydelig.

## 2. Nærhet og kontekst

**Fysisk nærhet på siden skal gjenspeile nærhet i innhold og kontekst.**

Elementer som hører sammen, skal visuelt oppfattes som en gruppe.

Elementer som representerer et skifte i innhold, nivå eller kontekst,
skal ha tilstrekkelig avstand til at skillet oppfattes naturlig.

Avstand skal dermed brukes som et strukturelt virkemiddel, ikke som
dekorasjon.

Dette gjelder blant annet:

-   overskrift og tilhørende tekst
-   tekstlinjer som sammen utgjør ett budskap
-   kort og innholdet i kortet
-   separate seksjoner
-   navigasjon og hovedinnhold

## 3. Typografisk hierarki

Hierarkiet skal være tydelig, men begrenset.

Forskjeller mellom brødtekst, mellomtitler, hovedtitler og eventuell
displaytekst skal være store nok til at strukturen forstås umiddelbart,
men ikke større enn nødvendig.

Det skal unngås å skape nye typografiske nivåer dersom eksisterende
nivåer kan uttrykke strukturen tilfredsstillende.

Helheten skal oppleves som ett sammenhengende typografisk system.

## 4. Bokstavmellomrom

Bokstavmellomrom skal vurderes optisk og i sammenheng med:

-   skriftgrad
-   skrifttype
-   vekt
-   linjeavstand
-   tekstens funksjon
-   gjengivelsen på skjerm

Stor skrift skal ikke automatisk få større `letter-spacing`.

Ved store skriftgrader blir de optiske mellomrommene mellom bokstavene
tydeligere, og displaytekst kan derfor i enkelte skrifter fungere best
med normal eller noe strammere bokstavavstand.

Skrifter med støtte for optisk størrelse (`opsz`) kan håndtere deler av
denne tilpasningen selv.

Tracking skal derfor justeres ut fra den faktiske skriften og den
visuelle helheten, ikke etter en generell regel om at større skrift
krever større bokstavmellomrom.

## 5. Linjeavstand og tekstblokker

Linjeavstand skal vurderes sammen med skriftgrad, linjelengde og
tekstens funksjon.

Displaytekst og korte overskrifter kan ha tettere linjeavstand enn
brødtekst, men linjene skal ikke oppleves som presset sammen.

Brødtekst skal ha tilstrekkelig linjeavstand til komfortabel lesing over
flere linjer.

Ved vurdering av en tekstblokk skal følgende sees i sammenheng:

-   `font-size`
-   `line-height`
-   `letter-spacing`
-   `font-weight`
-   linjelengde
-   avstand før og etter blokken

Ingen av disse egenskapene bør justeres isolert dersom problemet
egentlig gjelder den samlede typografiske balansen.

## 6. Vertikal rytme

Avstanden mellom tekstlige elementer skal danne et bevisst og konsekvent
system.

Det gjelder særlig:

-   avstand mellom overskrift og tilhørende tekst
-   avstand mellom avsnitt
-   avstand før nye seksjoner
-   avstand mellom kort eller andre innholdsgrupper

Et generelt CSS-system for spacing kan brukes som utgangspunkt, men skal
ikke overstyre typografiske og optiske vurderinger.

**Optisk riktig avstand er viktigere enn matematisk lik avstand.**

## 7. Linjelengde og lesbarhet

Linjelengden skal tilpasses innhold og skjermstørrelse.

Brødtekst skal ikke bli unødvendig bred på store skjermer. Korte tekster
og overskrifter skal heller ikke få linjebryting som svekker budskapets
naturlige rytme.

Linjebryting skal vurderes på flere skjermbredder og ikke optimaliseres
utelukkende for én bestemt visning.

## 8. Responsiv typografi

Web har ikke én fast satsflate.

Typografien skal derfor fungere på blant annet:

-   telefon
-   nettbrett
-   bærbar datamaskin
-   større skjermer

Skriftgrader, linjelengder, avstander og linjebryting skal vurderes
responsivt.

Et typografisk forhold som fungerer på en stor skjerm, skal ikke uten
videre antas å fungere på en liten skjerm.

Målet er å bevare det samme visuelle hierarkiet og den samme
typografiske karakteren på tvers av skjermstørrelser, ikke nødvendigvis
identiske mål.

## 9. Robusthet og tilgjengelighet

Typografien skal tåle variasjoner i:

-   nettleser
-   operativsystem
-   skjermtetthet
-   font-rendering
-   viewport
-   zoom og brukerens tekststørrelse

Det skal derfor unngås unødvendig bruk av faste høyder og absolutte
plasseringer som gjør at tekst eller typografisk struktur bryter sammen
ved slike variasjoner.

Lesbarhet og innholdets struktur skal ha prioritet foran nøyaktig
geometrisk gjengivelse.

## 10. Optiske korreksjoner

CSS gjør det enkelt å bygge matematiske systemer for størrelse og
avstand. Slike systemer skal brukes som hjelpemiddel, ikke som fasit.

To matematisk like mellomrom er ikke nødvendigvis optisk like.

Det er derfor tillatt og ønskelig å gjøre mindre optiske korreksjoner
når dette gir bedre:

-   balanse
-   rytme
-   gruppering
-   lesbarhet
-   visuell sammenheng

Slike avvik bør være bevisste og ikke utvikle seg til en samling
tilfeldige enkeltverdier.

## 11. Grafiske elementer

De samme prinsippene for hierarki og nærhet gjelder grafisk innhold.

Grafiske virkemidler skal støtte innholdets struktur og funksjon.

Dekorative elementer skal ikke konkurrere med teksten eller skape et
visuelt hierarki som ikke finnes i innholdet.

Typografi og grafikk skal oppleves som deler av det samme visuelle
systemet.

## 12. Prosjektspesifikke føringer

### `.brand`

Utformingen av teksten **«longfjeld»** knyttet til `.brand` beholdes som
den er inntil annet besluttes.

Den skal ikke automatisk endres som følge av generelle justeringer i
nettstedets øvrige typografi.

### Hero-tekst

Teksten:

> Små verktøy.\
> Bygget for å være nyttige.

oppleves i dagens utforming som for tett.

Ved senere justering skal dette vurderes som et samlet typografisk
problem. Aktuelle egenskaper som skal vurderes sammen er blant annet
linjeavstand, bokstavmellomrom, skriftgrad, vekt og avstand til
omkringliggende elementer.

Det skal ikke gjøres en isolert endring av `letter-spacing` uten at
helheten vurderes.

## 13. Arbeidsprinsipp ved senere endringer

Ved typografiske eller grafiske endringer skal følgende spørsmål brukes
som kontroll:

1.  Hva er den innholdsmessige relasjonen mellom elementene?
2.  Gjenspeiler den fysiske avstanden denne relasjonen?
3.  Er hierarkiet tydelig uten å være sterkere enn nødvendig?
4.  Er skriftgrad, linjeavstand, bokstavmellomrom og vekt balansert som
    en helhet?
5.  Fungerer løsningen på både små og store skjermer?
6.  Er løsningen robust ved zoom og variasjoner i font-rendering?
7.  Er eventuelle avvik fra spacing-systemet typografisk begrunnet?
8.  Tilfører grafiske virkemidler struktur eller bare dekorasjon?

Målet er ikke matematisk ensartethet, men **typografisk sammenheng,
lesbarhet og ro**.
