# Innhold og redigering

Denne veiledningen beskriver de vanligste endringene i nettstedet. Stegene er bevisst skrevet i rekkefølge.

## 1. Åpne prosjektkatalogen

Prosjektet ligger lokalt her:

```text
/Users/persteinar/Library/Mobile Documents/com~apple~CloudDocs/Koding/GitHub/www.longfjeld.com
```

I Terminal:

```bash
cd "/Users/persteinar/Library/Mobile Documents/com~apple~CloudDocs/Koding/GitHub/www.longfjeld.com"
```

## 2. Åpne filene i BBEdit

De viktigste filene er:

```text
index.html                 Forsiden
oppskrifter/index.html     Oppskrifter
akkordia/index.html        Akkordia
tekstilig/index.html       Tekstilig
style.css                  Felles design for alle sider
```

## 3. Endre tekst på en prosjektside

Åpne ønsket `index.html`.

Teksten er vanlig HTML og kan redigeres direkte. Eksempel:

```html
<h2>Set-listen først</h2>
<p>Akkordia er laget for bruk i band ...</p>
```

Endre bare teksten mellom HTML-tagene dersom du ikke ønsker å endre strukturen.

## 4. Endre kortteksten på forsiden

Åpne rotfilen:

```text
index.html
```

Finn prosjektet, for eksempel:

```html
<span class="project-link__name">Akkordia</span>
...
<span class="project-link__desc">Set-lister for band ...</span>
```

Endre teksten i `project-link__desc`.

## 5. Endre enkle designverdier

Åpne:

```text
style.css
```

Øverst ligger blokken:

```css
:root {
    ...
}
```

Dette fungerer som et enkelt kontrollpanel for blant annet:

- bakgrunnsfarger
- tekstfarger
- glass-transparens
- blur
- bakgrunnsglød
- maksimal innholdsbredde
- hjørneradius

Eksempel:

```css
--glass-blur: 20px;
```

Mindre verdi gir mindre blur. Større verdi gir mer blur.

## 6. Forhåndsvis lokalt

Det enkleste er å åpne `index.html` direkte i nettleser.

For å teste URL-strukturen mer realistisk kan du starte en enkel lokal webserver fra prosjektkatalogen:

```bash
python3 -m http.server 8000
```

Åpne deretter:

```text
http://localhost:8000/
```

Stopp serveren med `Ctrl-C` i Terminal.

## 7. Kontroller mobilvisning

I Safari kan du bruke Responsive Design Mode.

Kontroller minst:

1. Forsiden.
2. Alle tre prosjektsidene.
3. At toppnavigasjonen ikke klipper tekst.
4. At prosjektkortene ligger under hverandre på smal skjerm.
5. At det ikke finnes horisontal scrolling.

## 8. Publiser

Når endringene er kontrollert, fortsett med:

```text
docs/PUBLISERING-GITHUB-PAGES.md
```
