# Sri Mangalam Soshigaya — Site

Static single-file marketing site for **Sri Mangalam A/C Soshigaya-Okura** (スリマンガラム 祖師ヶ谷大蔵店), an award-winning South Indian / Chettinad restaurant in Tokyo's Setagaya City. Tabelog Hyakumeiten 2024.

- 📍 3-33-2 Soshigaya, Setagaya City, Tokyo (2 min from Soshigaya-Okura Station)
- 📞 +81 3-6676-2812
- 📸 [@srimangalam_soshigaya](https://www.instagram.com/srimangalam_soshigaya/)

## How to view

This is a deliberately old-fashioned, single-file static page. No build step, no server required.

```bash
# Open directly
xdg-open index.html         # Linux
open index.html             # macOS

# …or serve over HTTP (recommended, so the IntersectionObserver and font preloads behave like in production)
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Architecture

Everything ships from a single `index.html` (~2k lines) with inline `<style>` and `<script>`. There is no bundler, no framework, no transpiler.

- **Themes** — 6 themes via `data-theme` on `<html>`: `dark`, `light`, `saffron`, `rainbow`, `calm`, `nature`. Defined as CSS custom-property blocks at the top of the inline `<style>`. Switched from the bottom-nav theme menu and persisted to `localStorage` as `theme-preference`.
- **i18n** — 7 languages via `data-{lang}` attributes on `.i18n` elements: `en`, `jp`, `hi`, `ta`, `te`, `ml`, `kn`. The bottom-nav language menu writes the active code to `localStorage` as `lang-preference`; the inline script swaps `textContent` from the matching `data-*` attribute on every `.i18n` element. To add a translation, just add another `data-XX="..."` attribute next to the existing ones — no separate locale files.
- **Liquid Glass nav** — bottom pill with spring-physics morphing and three submenus (sections / language / theme). Activates compact mode on scroll-down.
- **Active section indicator** — `IntersectionObserver` over every `<section>` and `<header>` with `id`, with the bottom nav showing the current section's icon + label.

## Asset paths

- `assets/gallery/` — photos and short clips of the restaurant, dishes, and Chef Mahalingam. Filenames are dated `YYYY-MM-DD_HH-MM-SS_UTC[_n].{jpg,mp4}`.
- Images are referenced via plain relative paths from `index.html`.

See `context.md` for restaurant background, Chef Mahalingam's culinary lineage, and the original Council research notes that informed the site copy.

## Contributing

The whole site is one HTML file. When editing copy, **mind the `data-{lang}` attributes** — if you change the English of an `.i18n` element, update the matching language attributes in the same tag, or at minimum the `data-jp` (the most relevant non-English audience for a Tokyo restaurant). Hindi/Tamil/Telugu/Malayalam/Kannada coverage is in progress.

When adding sections, follow the existing pattern:
1. `<section id="your-id" class="container">…</section>`
2. Add a corresponding `<a href="#your-id" class="submenu-item i18n">` inside `#internal-nav-sub` so the submenu lists it.
3. Optionally add a `<div class="current-indicator hidden" data-section="your-id">` inside `#menu-toggle` so the active-section indicator can highlight it.

The `References/` folder (Instagram archive, council docs) is gitignored — it's local-only research material and shouldn't be committed.
