# PROJEKT: Moja Galeria Hugo — Notatka kontekstowa

> Ostatnia aktualizacja: 9.11.2026. Wklej tę notatkę na początku nowej sesji z Claude, żeby zachować pełny kontekst projektu.

---

## Lokalizacja projektu

| | |
|---|---|
| **Katalog projektu** | `C:\!DATA\Tomek\fotos\hugo1` |
| **Zdjęcia źródłowe** | `C:\!DATA\Tomek\fotos\IMG4Hugo\` |

---

## Stack techniczny

| | |
|---|---|
| **Generator** | Hugo v0.156.0 (lokalnie), v0.145.0 (Cloudflare) |
| **Motyw** | `hugo-theme-gallery` — git submodule |
| **Źródło motywu** | https://github.com/nicokaiser/hugo-theme-gallery |
| **Język** | Polski (`pl`) |
| **Motyw UI** | ciemny (`defaultTheme = "dark"`) |

---

## GitHub

| | |
|---|---|
| **Konto** | `tladyko` |
| **Repozytorium** | https://github.com/tladyko/hugo-galeria |
| **Branch produkcyjny** | `master` |
| **Remote** | `https://github.com/tladyko/hugo-galeria.git` |

---

## Cloudflare (Workers – static assets)

| | |
|---|---|
| **Projekt** | `hugo-galeria-test1` |
| **Panel** | https://dash.cloudflare.com → Workers & Pages → hugo-galeria-test1 |
| **Adres strony** | https://hugo-galeria-test1.tladyko.workers.dev |
| **baseURL** | `https://hugo-galeria-test1.tladyko.workers.dev/` |
| **Build command** | `git submodule update --init --recursive && curl -fsSL https://github.com/gohugoio/hugo/releases/download/v0.145.0/hugo_extended_0.145.0_linux-amd64.tar.gz -o hugo.tar.gz && tar xzf hugo.tar.gz hugo && ./hugo --minify` |
| **Katalog wyjściowy** | `public` |
| **Autodeploy** | ✅ przy każdym `git push` do `master` |
| **Uwaga** | `cloudflare-pages.toml` w repo nieaktywny — Cloudflare używa `wrangler.jsonc` |

---

## Struktura katalogów

```
hugo1/
├── content/
│   ├── _index.md                   ← tytuł "Powidoki z Podróży" + opis
│   └── <galeria>/index.md + zdjęcia   ← lista w sekcji „Galerie”
├── layouts/
│   ├── partials/
│   │   ├── header.html             ← OVERRIDE: brak linku logo na homepage
│   │   ├── featured.html           ← OVERRIDE: slideshow zamiast karty
│   │   └── head-custom.html
│   └── _default/home.html
├── themes/
│   └── hugo-theme-gallery/         ← git submodule
├── .cloudflare/functions/.gitkeep
├── .gitignore                      (public/, resources/)
├── cloudflare-pages.toml           (nieaktywny)
├── hugo.toml                       ← główna konfiguracja
├── wrangler.jsonc                  ← konfiguracja Cloudflare build
└── PROJEKT.md                      ← ten plik
```

---

## Galerie — aktualne źródła zdjęć

| Nazwa w Hugo | Tytuł | Źródło | Zdjęć |
|---|---|---|---|
| `antarctic-2018` | Antarktyda 2018 | — | 127 |
| `bodie-ghost-town-2025` | Bodie Ghost Town 2025 | `IMG4Hugo\Bodie Ghost Town  2025\` | 16 |
| `featured` | *(slideshow, ukryty)* | `IMG4Hugo\large\` | 72 |
| `Gemini-2010` | Gemini 3 2010 | — | 50 |
| `great-canyon-2025` | Grand Canyon 2025 | — | 23 |
| `kult-2009` | KULT 2009 | — | 31 |
| `misc-usa-2025` | MISC USA 2025 | `IMG4Hugo\MISC USA 2025\` | 63 |
| `monument-valley-2025` | Monument Valley 2025 | — | 20 |
| `ostar-2024` | Ostar 2024 | `IMG4Hugo\Ostar 2024\` | 13 |
| `porto-2025` | Porto 2025 | — | 42 |
| `route66` | Route 66 2022 | — | 47 |
| `sequoia-park-2025` | Sequoia Park 2025 | `IMG4Hugo\Sequoia Park 2025\` | 18 |
| `svalbard-2014` | Svalbard 2014 | — | 71 |
| `szkocja-2012` | Szkocja 2012 | — | 99 |
| `taormina-2026` | Taormina 2026 | — | 49 |
| `white-sands-2025` | White Sands 2025 | — | 15 |
| | **RAZEM** | | **756** |

---

## Customizacje motywu (layouts/partials/)

### header.html — usunięty link logo
Na stronie głównej motyw domyślnie pokazuje tytuł serwisu jako link w lewym górnym rogu. Override usuwa ten link — nagłówek na homepage jest pusty (zostaje tylko menu jeśli zdefiniowane).

### featured.html — slideshow
Zamiast standardowej "karty" Featured Album, strona główna wyświetla pełnoszerokościowy slideshow:
- Zdjęcia z `content/featured/` (strona z `featured: true`)
- Losowa kolejność przy każdym załadowaniu (Fisher-Yates shuffle)
- Zmiana zdjęcia co **5 sekund** z efektem płynnego przejścia (CSS opacity)
- Wysokość: 65vh (desktop), 45vw (mobile)

---

## Jak dodać nową galerię

```powershell
# 1. Utwórz katalog i skopiuj zdjęcia
mkdir "C:\!DATA\Tomek\fotos\hugo1\content\nowa-galeria"
cp "C:\!DATA\Tomek\fotos\IMG4Hugo\Nowa Galeria\*.jpg" "C:\!DATA\Tomek\fotos\hugo1\content\nowa-galeria\"

# 2. Utwórz plik index.md
# Zawartość:
# ---
# title: "Tytuł Galerii"
# date: 2025-01-01
# ---

# 3. Sprawdź lokalnie
cd "C:\!DATA\Tomek\fotos\hugo1"
hugo server

# 4. Wypchnij → Cloudflare buduje automatycznie
git add content/nowa-galeria/
git commit -m "Dodano galerię: Nowa Galeria"
git push
```

## Jak dodać zdjęcia do slideshow

```powershell
# Skopiuj zdjęcia do content/featured/
cp "C:\ścieżka\*.jpg" "C:\!DATA\Tomek\fotos\hugo1\content\featured\"

git add content/featured/
git commit -m "Dodano zdjęcia do slideshow"
git push
```

---

## Ważne komendy

```powershell
# Lokalny podgląd
hugo server

# Build testowy (sprawdź błędy)
hugo

# Status git
git status

# Push na GitHub (= automatyczny deploy na Cloudflare)
git push
```

---

## Historia commitów

| Hash | Opis |
|---|---|
| `cd25db0` | Inicjalny commit: galeria fotograficzna Hugo |
| `2478c12` | Konfiguracja Cloudflare Pages (baseURL, cloudflare-pages.toml) |
| `2312f99` | Fix: wrangler.jsonc z poprawną instalacją Hugo Extended |
| `71adadf` | Dodano PROJEKT.md |
| `851429d` | Nowy wygląd homepage: tytuł, opis, slideshow, brak logo-linku; ostar→2024, usa +42 zdj. |
| `f1e677a` | Dodano 3 nowe galerie (Bodie, MISC USA, Sequoia) + slideshow 34 zdjęcia |
| `5a4b65f` | Zaktualizowano PROJEKT.md — pełna historia i aktualny stan |
| `9b86c15` | Nowe galerie, redesign homepage, usunięto USA 2025 |
| `8adda9b` | Dodano galerie: Svalbard 2014 |
| `89f72a2` | Dodano galerie: Antarktyda 2018 |
| `d2a6b99` | Dodano galerie: Gemini 3 2010 |
| `66a6358` | Dodano galerie: Gemini 3 2010 |
| `6ddb590` | Dodano galerie: KULT 2009 |
| `b0c2127` | Dodano galerie: Szkocja 2012 |
| `ac16ca6` | Trigger rebuild |
| `b9fc026` | Dodano galerie: Route 66 2022 |
| `398f3d0` | Dodano galerie: Marakesh 2025 |
| `90d1d65` | Dodano galerie: Porto 2025 |
| `a71d860` | Usunięto galerię Marakesh 2025; slideshow featured: +41 zdjęć, -JAS_1516 |
| `331a4b0` | Zwiększ timeout Hugo do 600s — fix build Cloudflare (gallery.html timeout) |
| `455077a` | Dodano galerie: Taormina 2026 |

---

## Uwagi techniczne

- Motyw wymaga **Hugo Extended** (używa SCSS) — ważne przy konfiguracji buildu
- Submoduł motywu musi być inicjalizowany przy klonowaniu: `git submodule update --init --recursive`
- `public/` i `resources/` są w `.gitignore` — generowane podczas buildu
- Galeria `featured` ma `private: true` — nie pojawia się jako kafelek na stronie głównej
- Build trwa długo przy wielu zdjęciach — w `hugo.toml` ustawiono `timeout = "600s"` (fix z 6.03.2026)
- Pierwsze buildy po dodaniu dużej liczby zdjęć trwają długo (generowanie miniatur); kolejne są szybkie dzięki cache
- Cloudflare buduje automatycznie po każdym `git push` do brancha `master`
