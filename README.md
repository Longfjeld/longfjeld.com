# longfjeld.com

Statisk nettsted for `www.longfjeld.com`, publisert med GitHub Pages.

Nettstedet presenterer prosjektene:

- Oppskrifter
- Akkordia
- Tekstilig

Designretningen er **Minimal Glass**: et rolig, lyst grensesnitt inspirert av moderne glass-/blur-effekter, men med prosjektinnholdet i fokus.

## Teknologi

Nettstedet bruker bare:

- HTML5
- CSS
- Google Fonts for `Cormorant Garamond`
- GitHub Pages

Det finnes ingen build-prosess, JavaScript-avhengighet, Node.js-avhengighet eller database.

## Filstruktur

```text
www.longfjeld.com/
├── .gitignore
├── .nojekyll
├── 404.html
├── CNAME
├── README.md
├── index.html
├── style.css
├── oppskrifter/
│   └── index.html
├── akkordia/
│   └── index.html
├── tekstilig/
│   └── index.html
└── docs/
    ├── DESIGN.md
    ├── INSTALLASJON-I-EKSISTERENDE-REPO.md
    ├── INNHOLD-OG-REDIGERING.md
    ├── MIGRERING-FRA-HYPE.md
    └── PUBLISERING-GITHUB-PAGES.md
```

## Lokal plassering

Prosjektet er lokalt lagret i:

```text
/Users/persteinar/Library/Mobile Documents/com~apple~CloudDocs/Koding/GitHub/www.longfjeld.com
```

I Terminal må mellomrommet i `Mobile Documents` escapes eller hele stien settes i anførselstegn:

```bash
cd "/Users/persteinar/Library/Mobile Documents/com~apple~CloudDocs/Koding/GitHub/www.longfjeld.com"
```

## Dokumentasjon

Start her:

1. `docs/INSTALLASJON-I-EKSISTERENDE-REPO.md` – første utskifting i dagens Git-repository.
2. `docs/INNHOLD-OG-REDIGERING.md` – vanlige endringer i tekst og design.
3. `docs/PUBLISERING-GITHUB-PAGES.md` – kontroll, commit, push og publisering.
4. `docs/MIGRERING-FRA-HYPE.md` – bakgrunn for overgangen fra Hype.
5. `docs/DESIGN.md` – prinsippene bak Minimal Glass-uttrykket.
