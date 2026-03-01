# PROJEKT: Moja Galeria Hugo — Notatka kontekstowa

> Wklej tę notatkę na początku nowej sesji z Claude, żeby zachować pełny kontekst projektu.

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

## Cloudflare Pages

| | |
|---|---|
| **Projekt** | `hugo-galeria-test1` |
| **Panel** | https://dash.cloudflare.com → Workers & Pages → hugo-galeria-test1 |
| **baseURL** | `https://hugo-galeria.pages.dev/` |
| **Build command** | `git submodule update --init --recursive && curl -fsSL https://github.com/gohugoio/hugo/releases/download/v0.145.0/hugo_extended_0.145.0_linux-amd64.tar.gz -o hugo.tar.gz && tar xzf hugo.tar.gz hugo && ./hugo --minify` |
| **Katalog wyjściowy** | `public` |
| **Autodeploy** | ✅ przy każdym `git push` do `master` |

---

## Struktura katalogów

```
hugo1/
├── content/
│   ├── _index.md
│   ├── ostar-2025/          ← 13 zdjęć + index.md
│   │   └── index.md         title: "Ostar 2025", date: 2025-05-01
│   └── usa-2025/            ← 17 zdjęć + index.md
│       └── index.md         title: "USA 2025 Kalendarz", date: 2025-08-13
├── themes/
│   └── hugo-theme-gallery/  ← git submodule
├── .cloudflare/functions/.gitkeep
├── .gitignore               (public/, resources/)
├── cloudflare-pages.toml    (nieużywany — zastąpiony przez wrangler.jsonc)
├── hugo.toml                ← główna konfiguracja
├── wrangler.jsonc           ← konfiguracja Cloudflare build
└── PROJEKT.md               ← ten plik
```

---

## Galerie — źródła zdjęć

| Nazwa w Hugo | Tytuł | Źródło zdjęć | Liczba zdjęć |
|---|---|---|---|
| `ostar-2025` | Ostar 2025 | `C:\!DATA\Tomek\fotos\IMG4Hugo\Ostar 2025` | 13 |
| `usa-2025` | USA 2025 Kalendarz | `C:\!DATA\Tomek\fotos\IMG4Hugo\USA 2025 kalendarz\chosen` | 17 |

---

## Jak dodać nową galerię

```powershell
# 1. Utwórz katalog i skopiuj zdjęcia
mkdir "C:\!DATA\Tomek\fotos\hugo1\content\nowa-galeria"
cp "C:\ścieżka\do\zdjęć\*.jpg" "C:\!DATA\Tomek\fotos\hugo1\content\nowa-galeria\"

# 2. Utwórz plik index.md (wklej i edytuj)
# ---
# title: "Tytuł Galerii"
# date: 2025-01-01
# ---

# 3. Sprawdź lokalnie
cd "C:\!DATA\Tomek\fotos\hugo1"
hugo server

# 4. Wypchnij na GitHub → Cloudflare buduje automatycznie
git add .
git commit -m "Dodano galerię: nowa-galeria"
git push
```

---

## Ważne komendy

```powershell
# Lokalny podgląd
hugo server

# Build testowy
hugo

# Status git
git status

# Push na GitHub (= deploy na Cloudflare)
git push
```

---

## Historia commitów

| # | Opis |
|---|---|
| 1 | `cd25db0` — Inicjalny commit: galeria fotograficzna Hugo |
| 2 | `2478c12` — Konfiguracja Cloudflare Pages (baseURL, cloudflare-pages.toml) |
| 3 | `2312f99` — Fix: wrangler.jsonc z poprawną instalacją Hugo Extended |

---

## Uwagi techniczne

- Motyw wymaga **Hugo Extended** (używa SCSS) — ważne przy konfiguracji buildu
- Submoduł motywu musi być inicjalizowany przy każdym świeżym klonowaniu (`git submodule update --init --recursive`)
- `cloudflare-pages.toml` jest w repo ale nieaktywny — Cloudflare używa `wrangler.jsonc`
- `public/` i `resources/` są w `.gitignore` — generowane podczas buildu
