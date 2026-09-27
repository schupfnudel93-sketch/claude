# 01: Globales Layout, Navigation, globales CSS/JS (Fokus mobil 360 bis 430 px)

Audit-Stand: 27.09.2026. Live-Theme `gid://shopify/OnlineStoreTheme/208344613202` ("PMP: Futterrechner v5.1 (FREIGABE)").
Grundlage: Quellcode aus dem Shopify-Admin-API (nur gelesen) sowie Menü-, Collection-, Seiten- und Policy-Daten per GraphQL. Die Website selbst war aus der Umgebung **nicht** erreichbar. Nichts wurde live gerendert.

**Kennzeichnung**
- **[BELEGT]**: steht so im Code oder in den Daten und ist eindeutig.
- **[BERECHNET]**: aus Code-Werten errechnet (Pixelmaße). Sehr wahrscheinlich richtig, trotzdem live mit 360/375/390 px gegenprüfen.
- **[VERMUTET]**: plausibel, hängt aber von Browser, Inhalt oder Apps ab. **Muss live geprüft werden.**

Zeilennummern bei `assets/theme.css` beziehen sich auf die aktuelle Datei (1017 Zeilen). Für Liquid- und JS-Dateien sind Code-Ausschnitte angegeben, weil die Zeilen dort beim Editieren wandern.

---

## 0. Kurzfazit (Top 12)

| # | Prio | Befund | Status |
|---|---|---|---|
| 1 | **P0** | Mobiler Header ist bei 360 bis 375 px **breiter als der Bildschirm**. Das Warenkorb-Icon wird rechts abgeschnitten, weil das Logo mit 62 px Höhe auch mobil gilt und vier Icons plus 1,5rem-Gaps dazukommen. | BERECHNET |
| 2 | **P1** | Auf dem Handy gibt es **keinen Kauf-CTA im Header**. "Portrait gestalten" ist unter 990 px ausgeblendet (auch bei 1280 bis 1439 px). | BELEGT |
| 3 | **P1** | Mega-Menü-Promos sind wirkungslos: Die Blöcke heißen "Tierportraits" und "Produkte", im Menü gibt es diese Titel nicht. | BELEGT |
| 4 | **P1** | Die Menülogik ist teils falsch. "Stile" führt auf Aquarell, "Geschenke" auf Weihnachten. Mobil entstehen Texte wie "Alle Für dein Zuhause ansehen" und "Alle Wissen ansehen", außerdem doppelte "Alle …"-Links. | BELEGT |
| 5 | **P1** | USPs sind in der Navigation versteckt: Sonderwunsch fehlt im Hauptmenü ganz, "So funktioniert's" und "Tierschutz" liegen tief unter "Wissen". | BELEGT |
| 6 | **P1** | Die Schnellsuche findet **keine Seiten** (Futterrechner, Tierschutz, Sonderwunsch, Kontakt, FAQ). | BELEGT |
| 7 | **P1** | Die Ankündigungsleiste ist mobil ein horizontal scrollbarer Streifen ohne sichtbaren Scrollbalken. Die zweite Botschaft (Druck in Europa, Lieferzeit) sieht praktisch niemand. | BELEGT/VERMUTET |
| 8 | **P1** | Drawer (Menü, Warenkorb) ignorieren `safe-area-inset-bottom`. Der Checkout-Bereich im Cart-Drawer liegt auf dem iPhone unter dem Home-Balken. Das Freebie-Fly-in verliert seinen Safe-Area-Abstand durch eine spätere Regel. | BELEGT/VERMUTET |
| 9 | **P1** | Race Condition im Drawer: Schnelles Schließen und Wiederöffnen (unter 500 ms) versteckt den Drawer, während die Seite gesperrt bleibt. Außerdem fehlen Fokus-Rückgabe und `aria-expanded`. | BELEGT |
| 10 | **P2** | Lange deutsche Wörter in `.h0`/`.h1` (46 bzw. 38 px mobil) werden nicht umbrochen, es gibt kein `hyphens`/`overflow-wrap`. `body{overflow-x:clip}` schneidet sie stillschweigend ab. | VERMUTET (abhängig vom Inhalt) |
| 11 | **P2** | iOS-Zoom bei Selects unter 16 px (Länder-/Sprachwahl im Footer, Sortierung in Collections). Die Suche-Schließen-Taste ist nur 20×20 px groß. | BELEGT |
| 12 | **P2** | Footer-Spalte "Shop" enthält kaum Shop-Links, dafür 5 Dubletten zu den anderen Spalten. Social-Icons im Menü-Drawer sind ungestylt (vertikal, Rahmen unsichtbar). | BELEGT |

Positiv (kein Handlungsbedarf, **[BELEGT]**):
- Cursor, Tilt, Magnetic und Spotlight sind auf Touch sauber aus (CSS `(hover:hover) and (pointer:fine)` und JS `fine`).
- `prefers-reduced-motion` wird weitgehend respektiert.
- `section_padding 112` ist mobil **kein** Problem: 62 px, auf dem Handy × 0,8 = rund 50 px.
- AdSense lädt nur im Blog und erst nach Marketing-Consent.
- Alle Skripte haben `defer`.
- Alle Menüziele existieren und sind im Onlineshop veröffentlicht.
- Alle 6 Rechtstexte sind als Shopify-Policies mit Inhalt vorhanden.

---

## 1. IST-Zustand

### 1.1 Aufbau `layout/theme.liquid`
Reihenfolge im `<body>`:
1. Inline-Fix teeinblue-Geldformat
2. Skip-Link
3. `.pmp-cursor` (nur wenn `enable_cursor`)
4. `.scroll-progress`
5. `header-group` (Announcement plus Header plus Menü-Drawer)
6. `<main id="MainContent">`
7. `footer-group`
8. Cart-Drawer `#CartDrawer`
9. Search-Overlay
10. Toast
11. Freebie-Fly-in
12. Icon-Sprite
13. Skripte (`theme.js`, `cart.js`, `newsletter.js` immer mit `defer`; `product-form.js` und `live-preview.js` nur auf Produktseiten)

Weitere Punkte:
- CSS: `theme.css` (90 KB, render-blocking) und `pmp-222.css` (7 KB) auf **jeder** Seite.
- Fonts: Bricolage Grotesque n7 (Headlines), Archivo n4 (Body) und Fraunces i4 (Akzent) über den Shopify-CDN. Zusätzlich werden Bold-, Italic- und "bolder"-Varianten als `@font-face` geladen.
- `viewport-fit=cover` ist gesetzt. Deshalb **muss** das Theme Safe-Areas selbst berücksichtigen (siehe Befund N-8).

### 1.2 Header-Verhalten
- `.header-section` ist `position:sticky; top:0; z-index:120` (Z. 180). Die Announcement-Leiste scrollt weg und ist nicht sticky.
- Headerhöhe: `--h:4.6rem` (73,6 px), gescrollt `4rem` (64 px) (Z. 183, 185).
- Hide-on-scroll: Ab 260 px Scrolltiefe verschwindet der Header beim Runterscrollen und erscheint beim Hochscrollen wieder (`theme.js` → `header.classList.add('is-hidden')`).
- Unter 990 px: Burger (links), Logo, Aktionen (YouTube, Suche, Konto, Warenkorb). Der Header-CTA ist ausgeblendet (Z. 927).
- 990 bis 1279 px: ebenfalls Burger, CTA sichtbar (Z. 1011 bis 1016).
- 1280 bis 1439 px: Desktop-Navigation, **CTA ausgeblendet** (Z. 1008 bis 1010).
- Ab 1440 px: Desktop-Navigation mit CTA.

### 1.3 Menübaum `main-menu` (Stand API) mit Zielprüfung
Alle Ziele existieren und sind im Kanal "Onlineshop" veröffentlicht **[BELEGT]**. Die Zahl in Klammern ist die Produktanzahl der Collection.

```
Hauptmenü (10 Top-Level-Punkte!)
├─ Personalisiert → /collections/mit-foto (133)
│   ├─ Mit deinem Foto → /collections/mit-foto (133)          ← gleiches Ziel wie Parent
│   └─ Sofort lieferbare Motive → /collections/ohne-foto (81) ← widerspricht "Personalisiert"
├─ Wandbilder → /collections/wandbilder (49)
│   ├─ Hundeportraits → /collections/hundeportraits (43)
│   ├─ Katzenportraits → /collections/katzenportraits (55)
│   ├─ Pferdeportraits → /collections/pferdeportraits (49)
│   ├─ Alle Wandbilder → /collections/wandbilder              ← Dublette zum auto. "Alle Wandbilder ansehen" (mobil)
│   └─ Digitales Portrait → /products/dein-hund-als-digitales-kunstwerk-sofort-download ← kein Wandbild
├─ Kleidung → /collections/kleidung-fuer-tierliebhaber (88)
│   ├─ T-Shirts → t-shirts-fur-tierliebhaber (44)
│   ├─ Hoodies und Sweatshirts → hoodies-sweatshirts-fur-tierliebhaber (53)
│   └─ Alle Kleidung → kleidung-fuer-tierliebhaber          ← Dublette (mobil)
├─ Für dein Zuhause → /collections/wohnen (20)
│   ├─ Tassen und Becher → /collections/tassen (46)
│   ├─ Kissen und Decken → /collections/wohnen              ← gleiches Ziel wie Parent
│   └─ Futternäpfe → /collections/futternaepfe (4)
├─ Accessoires → /collections/accessoires (14)
│   └─ Halstücher → /collections/halstuecher (4)             ← einziges Kind; Handyhüllen (2 aktive Produkte) nirgends verlinkt
├─ Stile → /collections/stil-aquarell (134)                  ← Parent zeigt auf EINEN Stil
│   └─ 13 Stile: Aquarell, Klassisches Ölgemälde, Royal, Cartoon, 3D-Cartoon, Minimal,
│      Street-Art, Retro, Pop-Art, Anime (39), Herzensbild (33), Sketch, Original (39)
├─ Geschenke → /collections/geschenke-weihnachten (118)      ← Parent = Weihnachten
│   ├─ Weihnachten, Geburtstag (75), Erinnerung (68), Zum Einzug (46)
│   └─ Für Hundemenschen (201), Für Katzenmenschen (160), Für Pferdemenschen (120)
├─ Kalender 2027 → /products/haustier-wandkalender-2027 (Produkt, aktiv)
├─ Wissen → /blogs/magazin
│   └─ Magazin, Futterrechner, So funktioniert's, Häufige Fragen, Unsere Bücher,
│      YouTube, Tierschutz, Über uns
└─ Gratis-Startpaket → /pages/welpen-startpaket               (als rote "Pill" hervorgehoben)
```

Die Mega-Promo-Blöcke in `sections/header-group.json` heißen `menu_title: "Tierportraits"` und `"Produkte"`. **Keiner der beiden Titel kommt im Menü vor** (siehe N-3).

**Footer** (`sections/footer-group.json`): Die Überschrift wird im Theme gesetzt, darunter das jeweilige Menü.
```
"Shop" (Menü footer):         Alle Wandbilder · Kalender 2027 · So funktioniert's · Magazin · Suche · Über uns · Kontakt
"Wissen & Community" (footer-wissen): Gratis: Welpen-Startpaket · Magazin · Futterrechner · YouTube-Kanal · Unsere Bücher · Tierschutz-Versprechen · Über uns
"Service" (footer-service):   So funktioniert's · Häufige Fragen · Sonderwunsch anfragen · Kontakt · Suche
Footer-Bottom: PAngV-Zeile ("inkl. MwSt., zzgl. Versandkosten" → /policies/shipping-policy),
               © Jahr, alle shop.policies mit Inhalt, Cookie-Einstellungen, Länder-/Sprachwahl, Zahlungs-Icons
```
Weitere Punkte:
- Dubletten über die Spalten hinweg: So funktioniert's (2×), Magazin (2×), Über uns (2×), Kontakt (2×), Suche (2×).
- Shopify-Policies mit Inhalt **[BELEGT]**: Kontakt (CONTACT_INFORMATION), Impressum (LEGAL_NOTICE), Datenschutzerklärung, Widerrufsrecht, Versand, AGB.
- Ob Liquid `shop.policies` auch *Impressum* und *Kontakt* ausgibt, ist **[VERMUTET]**. Das muss live im Footer geprüft werden, denn das Impressum ist Pflicht (siehe N-17).
- Ein weiteres Blog `news` existiert und ist nirgends verlinkt. Inhalt nicht geprüft.

### 1.4 Z-Index-Karte [BELEGT]
| Ebene | z-index | Datei/Zeile |
|---|---|---|
| Sticky-ATC (Produkt, mobil), Cart-Stickybar | 90 | theme.css 442, 946 |
| Freebie-Fly-in | 95 | 823 |
| Announcement-Section | 119 | 181 |
| Header-Section (sticky) | 120 | 180 |
| Mega-Menü (im Header-Stacking-Context) | 130 | 182 |
| Scroll-Progress | 150 | 790 |
| Drawer (Menü, Warenkorb) | 200 | 220 |
| Search-Overlay | 250 | 241 |
| Toast | 300 | 788 |
| Skip-Link | 1000 | 23 |
| Grain-Overlay (`body::after`, fixed, Vollbild) | 9998 | 819 |
| Pfoten-Cursor | 9999 | 811 |

Die Reihenfolge ist logisch, echte Konflikte sehe ich im Code nicht:
- Das Fly-in ist auf Produkt- und Warenkorbseiten per Liquid ausgeblendet, kollidiert also nicht mit Sticky-ATC oder Stickybar.
- Das Fly-in prüft vor dem Einblenden, ob ein Drawer oder die Suche offen ist.
- **[VERMUTET]**: Das Fly-in (unten, mobil über die volle Breite) und der Shopify-Cookie-Banner (ebenfalls unten) können sich überlagern. Live prüfen.

### 1.5 Breakpoints (alle in theme.css / pmp-222.css)
`max 420` (Newsletter-Zeile untereinander) · `max 600` (Newsletter-Badge, Freebie compact, ba__proof, Landing) · **`max 749`** (Handy: Announcement scrollbar, 1-spaltige Grids, Footer 1-spaltig, Mobile-Diät, Fly-in als Leiste) · **`max 989` / `min 990`** (Tablet: Burger, Produkt 1-spaltig, CTA aus) · `max 1180` (Nav-Link kleiner) · `min 1200` (Stile 4 Spalten) · `990 bis 1279` (Burger auch auf kleinen Laptops) · `1280 bis 1439` (CTA aus) · `1280 bis 1599` / `990 bis 1799` (Nav-Abstände enger).

Zusätzlich: `(hover:hover)`, `(hover:none)`, `(hover:hover) and (pointer:fine)`, `prefers-reduced-motion`, `@supports (animation-timeline: view())`.

### 1.6 Wichtige Design-Tokens (settings_data.json) [BELEGT]
- Farben: bg `#FFFFFF`, soft `#F7F5F1`, ink `#0F0E0D`, ink-2 `#5F5A55`, line `#E7E2DB`, accent `#E3261C`, accent-deep `#B5170F`, accent-soft `#FDEDEA`.
- `base_font_size 17`, `page_width 1400`, `section_padding 112`, `radius 14`, `radius_card 22`.
- **`logo_height 62`**: In der Einstellung steht "(Desktop)", der Wert gilt aber überall.
- Logo-Datei `printmypet-logo.png` ist 2000×1321 px (Seitenverhältnis 1,514). Bei 62 px Höhe ist es also 94 px breit.
- `customerAccounts: OPTIONAL`, deshalb wird das Konto-Icon gerendert.
- `search_suggestions`: Hundeportrait, Katzenportrait, Leinwand, Tasse, Hoodie, Handyhülle.
- Gutter: `clamp(1rem,4vw,2.5rem)`, also 16 px bei 360 bis 400 px Breite.
- Sektionsabstand: `--section-pad: clamp(62px, 7vw, 112px)`. Mobil ergibt das 62 px, durch die Mobile-Diät (Z. 802) × 0,8 = **rund 50 px**.

---

## 2. Befunde im Detail

### N-1 · P0 · Header mobil: horizontaler Überlauf, Warenkorb abgeschnitten [BERECHNET]
**Dateien:** `assets/theme.css` Z. 186, 189, 194, 217; `sections/header.liquid`; `config/settings_data.json` (`logo_height: 62`); `assets/pmp-222.css` (`.header__yt`)

**Code:**
```css
.header__inner{display:grid;grid-template-columns:auto 1fr auto;...;gap:1.5rem}           /* Z.186 */
.header__logo img{height:var(--logo-h,2.6rem);width:auto;max-width:none}                    /* Z.189, --logo-h = 62px */
.icon-btn{width:2.7rem;height:2.7rem}                                                        /* Z.194 = 43,2px */
@media (max-width:989px){...header__inner{grid-template-columns:auto auto 1fr}}              /* Z.217 */
```
Unter 990 px ist der Gap weiterhin 1,5rem (24 px). Eine mobile Überschreibung gibt es nicht, nur 990 bis 1799 px bekommt `.75rem`.

**Rechnung**, Mindestbreite des Header-Inhalts:
- Burger 43,2
- Logo 94 (62 × 1,514)
- Aktionen 4 × 43,2 + 3 × 2,4 = 180 (YouTube, Suche, Konto, Warenkorb)
- 2 Gaps × 24 = 48
- **Summe 365 px**

Das Grid kann nicht schrumpfen: Das Logo hat `max-width:none`, und die `1fr`-Spalte hat als Minimum die Breite ihres Inhalts.

| Viewport | Platz im Container | Überlauf | Über den Bildschirmrand hinaus |
|---|---|---|---|
| 360 | 328 | 37 px | **21 px**, Cart-Icon etwa halb abgeschnitten |
| 375 | 343 | 22 px | **6 px** |
| 390 | 358 | 7 px | 0 (steht im rechten Gutter, Header asymmetrisch) |
| 414/430 | 381/396 | passt | – |

`body{overflow-x:clip}` (Z. 12) versteckt den Überlauf. Es entsteht also kein Scrollbalken, der Inhalt wird einfach abgeschnitten. Auf iOS unter 16 wird `clip` ignoriert, dort ist die ganze Seite horizontal wackelig **[VERMUTET]**.

Zusätzlich: Gescrollt ist der Header nur 64 px hoch, das Logo 62 px. Das Logo "klebt" oben und unten fast am Rand.

**Fix** (am Ende von `theme.css` oder in `pmp-222.css`):
```css
@media (max-width:749px){
  .header{--h:3.75rem}
  .header.is-scrolled{--h:3.5rem}
  .header__inner{gap:.5rem}
  .header__logo img{height:40px}          /* ≈ 61 px breit */
  .header__yt{display:none}               /* YouTube steht im Drawer-Fuß (social-links) */
  .header__actions .icon-btn[href*="account"]{display:none} /* Konto ist im Drawer-Fuß vorhanden */
}
```
Ergebnis bei 360 px: 43 + 61 + (2 × 43 + 2) + 2 × 8 = 208 px. Damit bleiben rund 120 px für einen kompakten CTA (siehe N-2).

Sauberer wäre eine eigene Einstellung `logo_height_mobile` in `settings_schema.json` plus `--logo-h-m` in `css-variables.liquid`.

---

### N-2 · P1 · Kein Kauf-CTA im mobilen Header (Conversion) [BELEGT]
**Datei:** `assets/theme.css` Z. 926 und 927, Z. 1008 bis 1010
```css
@media (max-width:989px){.header__cta{display:none}}
@media (min-width:1280px) and (max-width:1439px){.header__cta{display:none}}
```
Auf dem Handy erreicht man "Portrait gestalten" nur über Burger und dann den CTA oben im Drawer.

Auch bei 1280 bis 1439 px (z. B. verbreitete 1366er- und 1440er-Laptops knapp darunter) fehlt der CTA. **Sichtbar ist der Header-CTA nur bei 990 bis 1279 und ab 1440 px.**

**Fix:** Nach N-1 einen kompakten CTA einblenden.
```css
@media (max-width:989px){
  .header__cta{display:inline-flex;padding:.55rem .8rem;font-size:.8rem;margin-right:.1rem}
}
```
Kürzerer Text für mobil, z. B. `<span class="hide-sm">Portrait </span>gestalten`, oder ein eigenes Setting `cta_text_mobile`.

Bei 1280 bis 1439 px den CTA behalten und stattdessen das Menü straffen (siehe N-5: 10 Top-Level-Punkte auf 6 bis 7 reduzieren). Danach mit Screenshots bei 1280, 1366 und 1440 px prüfen.

---

### N-3 · P1 · Mega-Menü-Promos greifen nie [BELEGT]
**Dateien:** `sections/header-group.json` (Blöcke `mega1` mit `menu_title: "Tierportraits"` und `mega2` mit `"Produkte"`); `sections/header.liquid`:
```liquid
{%- if block.settings.menu_title == link.title -%}{%- assign promo = block -%}{%- endif -%}
```
Die Top-Level-Titel lauten "Personalisiert", "Wandbilder", "Kleidung", "Für dein Zuhause", "Accessoires", "Stile", "Geschenke", "Wissen". Es gibt also keinen Treffer, und **auf dem Desktop erscheint in keinem Mega-Menü ein Bild**. Die Bilder `tile-dogs.png` und `hero-portrait-dog.jpg` sind toter Content.

Mobil werden Promos grundsätzlich nicht gerendert, dort gibt es nur `<details>` mit Links.

**Fix:** Die `menu_title` der Blöcke auf echte Titel setzen, z. B. mega1 → "Wandbilder" (Bild Hund, Ziel `/collections/hundeportraits`) und mega2 → "Personalisiert" oder "Geschenke". Optional einen dritten Block für "Stile" ergänzen.

Optional mobil: Die Promo als erste Kachel in `.mnav__sub` rendern, klein mit 16:9-Bild und `loading="lazy"`. Dafür dieselbe Promo-Zuordnung wie im Desktop-Loop in den Drawer-Loop übernehmen.

---

### N-4 · P1 · Mobile Menülogik: falsche Ziele, kaputte Texte, Dubletten [BELEGT]
**Datei:** `sections/header.liquid` (Drawer):
```liquid
<a href="{{ link.url }}"><strong>{{ 'general.show_all' | t: title: link.title }}</strong></a>
```
Der Locale-Text lautet `"Alle {{ title }} ansehen"`. Im mobilen Drawer steht deshalb:
- "Alle **Personalisiert** ansehen" (grammatisch falsch)
- "Alle **Für dein Zuhause** ansehen" (falsch)
- "Alle **Wissen** ansehen" → führt zum Magazin (irreführend)
- "Alle **Stile** ansehen" → führt zu **/collections/stil-aquarell**. Es gibt keine Stil-Übersicht.
- "Alle **Geschenke** ansehen" → führt zu **Weihnachten**
- Unter "Wandbilder": "Alle Wandbilder ansehen" **und** der Menüpunkt "Alle Wandbilder". Gleiches bei Kleidung. Das sind Dubletten.
- Unter "Für dein Zuhause": "Kissen und Decken" hat dasselbe Ziel wie der Parent.
- Unter "Personalisiert": "Mit deinem Foto" hat dasselbe Ziel wie der Parent.

Auf dem Desktop führt ein Klick auf "Stile" bzw. "Geschenke" ebenfalls zu Aquarell bzw. Weihnachten. Auf Touch-Geräten mit Desktop-Navigation öffnet der erste Tap nur.

**Fix:**
1. Den generierten Text neutral formulieren: `"show_all": "Alles aus {{ title }}"`. Noch besser: den Link nur rendern, wenn **kein** Kind dasselbe Ziel hat.
   ```liquid
   {%- assign dup = false -%}{%- for c in link.links -%}{%- if c.url == link.url -%}{%- assign dup = true -%}{%- endif -%}{%- endfor -%}
   {%- unless dup or link.url contains '/blogs/' -%}<a …>Alles aus {{ link.title }}</a>{%- endunless -%}
   ```
2. Im Admin: Parent "Stile" auf eine Übersicht zeigen lassen, z. B. eine neue Seite `/pages/stile` oder die Home-Sektion `/#stile`. Parent "Geschenke" auf eine Sammel-Collection `geschenke` oder auf `/collections/fuer-hundemenschen` usw. zeigen lassen.
3. Die Dubletten "Alle Wandbilder" und "Alle Kleidung" aus dem Menü löschen. "Kissen und Decken" hier umbenennen in "Kissen, Decken & Deko".

---

### N-5 · P1 · Informationsarchitektur: Was bietet PrintMyPet? [BELEGT, Bewertung]
Fragestellung: Versteht man schnell, was PrintMyPet alles bietet?

Probleme in der aktuellen Struktur:
- **10 Top-Level-Punkte.** Mobil sind das 10 große Zeilen à ca. 66 px, also rund 660 px. Der Drawer-Fuß mit Konto und Social liegt unterhalb des Falzes.
- **"Personalisiert"** ist als Einstieg unklar, denn fast alles ist personalisiert. Das Kind "Sofort lieferbare Motive" widerspricht dem Parent.
- **Tierart als Einstieg fehlt.** Hund, Katze und Pferd tauchen nur unter "Wandbilder" (und als "Für …menschen" unter Geschenke) auf. Tassen, Kleidung usw. lassen sich nicht nach Tierart finden.
- **Sonderwunsch** (starker Umsatzhebel) steht **nicht im Hauptmenü**, nur im Footer.
- **So funktioniert's** und **Tierschutz** (Kern-USP "20 %") sind Unterpunkte 3 und 7 von "Wissen".
- **Kontakt** fehlt im Hauptmenü.
- **Handyhüllen** (2 aktive Produkte: iPhone und Samsung) sind nicht verlinkt, "Accessoires" hat nur "Halstücher".
- "Digitales Portrait" steht unter "Wandbilder".
- Der rote "Gratis-Startpaket"-Pill ist das einzige farbig hervorgehobene Nav-Element (Z. 858 bis 860, 929 bis 930). Er zieht auf dem Desktop Aufmerksamkeit vom Kauf ab. Das ist eine Designentscheidung, bitte abwägen.

**Vorschlag Zielstruktur** (nur Admin-Menü, ohne Code; Handles existieren alle):
```
Portrait gestalten (CTA bleibt separat)
1. Tierportraits      → /collections/mit-foto
   Hund · Katze · Pferd · Digitales Portrait · Sofort lieferbare Motive
2. Produkte           → /collections/schaufenster (oder all)
   Wandbilder (Leinwand/Poster/Acryl) · Tassen · Kleidung (T-Shirts, Hoodies) · Kissen & Decken · Accessoires (Halstücher, Handyhüllen) · Futternäpfe · Kalender 2027
3. Stile              → /pages/stile (neu) bzw. Stil-Übersicht
   13 Stile …
4. Geschenke          → /collections/geschenke-… (Sammel-Collection)
   Anlässe + Für Hunde-/Katzen-/Pferdemenschen
5. So funktioniert's  → /pages/so-funktionierts   (Top-Level!)
6. Sonderwunsch       → /pages/sonderwunsch       (Top-Level!)
7. Wissen             → /blogs/magazin
   Magazin · Futterrechner · YouTube · Bücher · Gratis-Startpaket · FAQ
8. Tierschutz 20 %    → /pages/tierschutz         (Top-Level oder als Badge)
```
Mit dieser Struktur passen auch die Mega-Promo-Titel "Tierportraits" und "Produkte" wieder (N-3 erledigt sich dann).

Zusätzlich im mobilen Drawer unter dem CTA eine Zeile mit 3 Mini-USPs anzeigen ("20 % an Tierschutz · Vorschau vor Druck · 5 bis 9 Werktage"). So versteht man das Angebot sofort.

---

### N-6 · P1 · Schnellsuche findet keine Seiten [BELEGT]
**Datei:** `assets/theme.js` (Predictive Search):
```js
fetch(`${window.PMP.routes.predictiveSearch}?q=…&resources[type]=product,article,collection&resources[limit]=5&section_id=predictive-search`
```
`sections/predictive-search.liquid` rendert nur Produkte, Collections und Artikel.

Wer "Futterrechner", "Tierschutz", "Sonderwunsch", "Kontakt", "Versand" oder "FAQ" tippt, bekommt "Nichts gefunden …", sofern kein Artikel passt.

**Fix:**
```js
resources[type]=product,collection,article,page
```
```liquid
{%- if predictive_search.resources.pages.size > 0 -%}
  <div class="ps-group"><p class="ps-group__title">{{ 'general.search.page' | t }}</p>
  {%- for p in predictive_search.resources.pages -%}<a href="{{ p.url }}" class="ps-item"><span class="ps-item__title">{{ p.title }}</span></a>{%- endfor -%}</div>
{%- endif -%}
```
Die Bedingung "keine Treffer" um `pages.size == 0` erweitern. Der Locale-Key `general.search.page` ("Seite") existiert bereits.

Zusätzlich (P2):
- Unter den Ergebnissen einen Link "Alle Ergebnisse für „…“ anzeigen" (`/search?q=`).
- In der Suche fehlt ein sichtbarer Absende-Button. Mobil geht es per Tastatur-"Los", ist aber nicht offensichtlich.
- Die "Beliebt"-Chips um "Futterrechner" oder "Sonderwunsch" ergänzen. Die Chips führen auf die Suchseite.

---

### N-7 · P1 · Ankündigungsleiste mobil: zweite Botschaft unsichtbar [BELEGT/VERMUTET]
**Datei:** `assets/theme.css` Z. 173 bis 177
```css
.announcement__item{...white-space:nowrap}
@media (max-width:749px){.announcement__inner{justify-content:flex-start;overflow-x:auto;scrollbar-width:none}...::-webkit-scrollbar{display:none}}
```
Mobil ist das **kein Laufband und kein Umbruch**, sondern ein horizontal wischbarer Streifen ohne Scrollbalken.

Item 1 "♥ 20 % unseres Gewinns gehen an den Tierschutz" ist bei 13,1 px Schrift etwa 320 px breit, der Platz beträgt 328 px **[VERMUTET, schriftabhängig]**. Item 2 ("Druck in Europa · Lieferung in 5 bis 9 Werktagen") liegt komplett rechts außerhalb. Kaum jemand wischt dort.

In `theme.js` gibt es keine Logik zu `data-announcement`.

**Fix (empfohlen):** Rotierende Einblendung mobil, mit reduced-motion-Fallback.
```css
@media (max-width:749px){
  .announcement__inner{display:grid;justify-content:center;overflow:hidden}
  .announcement__item{grid-area:1/1;justify-content:center;opacity:0;animation:ann-rot 10s infinite}
  .announcement__item:nth-child(2){animation-delay:5s}
  @keyframes ann-rot{0%,4%{opacity:0}8%,46%{opacity:1}50%,100%{opacity:0}}
}
@media (max-width:749px) and (prefers-reduced-motion:reduce){
  .announcement__item{animation:none;opacity:1;white-space:normal;text-align:center}
  .announcement__inner{display:flex;flex-direction:column;gap:.15rem}
}
```
Die Animation-Delays passen für genau 2 Einträge. Bei mehr Einträgen die Delays per Liquid setzen: `style="animation-delay:{{ forloop.index0 | times: 5 }}s"` und Dauer = n × 5 s.

Alternative ohne Animation: Texte kürzen ("20 % Gewinn an den Tierschutz" / "Druck in EU · 5 bis 9 Werktage") und untereinander stapeln (2 Zeilen, ca. 48 px).

Das Item mit Link auf `/pages/tierschutz` bzw. `/policies/shipping-policy` versehen (das `url`-Feld ist leer). Das bringt Vertrauen plus Klickziel.

---

### N-8 · P1 · Safe-Area (iPhone Home-Indicator) in Drawern, Toast, Fly-in [BELEGT/VERMUTET]
`viewport-fit=cover` ist gesetzt (theme.liquid). Berücksichtigt wird `env(safe-area-inset-*)` nur bei `.sticky-atc` (Z. 442), `.cart-stickybar` (Z. 946) und `.pmp-flyin` (Z. 834). Dabei gilt:
- **Fly-in:** Z. 969 überschreibt die Safe-Area-Polsterung aus Z. 834 wieder:
  ```css
  @media (max-width:749px){.pmp-flyin{padding:.85rem 2.2rem .85rem .9rem}…}   /* Z.969 – padding-shorthand killt padding-bottom:calc(… + env()) */
  ```
  Die Leiste steht mit `bottom:.75rem` also im Home-Indicator-Bereich **[BELEGT]**.
- **Drawer-Fuß** (`.drawer__foot`, Z. 230): Im Cart-Drawer sitzen dort "Zur Kasse" und "Warenkorb ansehen". Kein Safe-Area-Padding. Das Panel ist `top:0;bottom:0` (Z. 222). Der letzte Link liegt nur etwa 19 px über der Unterkante, der iPhone-Home-Balken braucht 34 px **[VERMUTET: Überlappung sichtbar]**.
- **Toast** (Z. 788): `bottom:1.5rem` ohne Safe-Area.
- Querformat: Links und rechts (Notch) wird nirgends berücksichtigt. Header, Drawer-Panel links und Announcement sind betroffen (P2).

**Fix:**
```css
.drawer__foot{padding-bottom:calc(1.2rem + env(safe-area-inset-bottom))}
.drawer__panel{padding-top:env(safe-area-inset-top)}
.toast{bottom:calc(1.5rem + env(safe-area-inset-bottom))}
@media (max-width:749px){.pmp-flyin{padding-bottom:calc(.85rem + env(safe-area-inset-bottom))}}   /* NACH Z.969 */
.page-width{padding-inline:env(safe-area-inset-left) env(safe-area-inset-right)} /* optional, Querformat */
```

---

### N-9 · P1 · Drawer-JS: Race Condition, Fokus, ARIA, Scroll-Lock [BELEGT/VERMUTET]
**Datei:** `assets/theme.js`, Objekt `Drawers`

a) **Race Condition [BELEGT]:**
```js
close(el){ el.classList.remove('is-open'); document.body.style.overflow=''; …; setTimeout(() => (el.hidden = true), 500); }
open(el){ el.hidden = false; … raf(() => el.classList.add('is-open')); document.body.style.overflow='hidden'; … }
```
Der Timeout aus `close` wird in `open` nicht gelöscht. Ablauf: Menü zu, innerhalb von 500 ms wieder auf (Doppeltipp, Backdrop, dann Burger). Nach dem Timeout ist der Drawer `hidden`, `body` bleibt aber auf `overflow:hidden`. Ergebnis: **Seite scrollt nicht mehr, Menü unsichtbar.** Ebenso beim Cart-Drawer: Ein Produkt wird hinzugefügt und `cart.js` öffnet den Drawer, kurz nachdem er geschlossen wurde.

**Fix:**
```js
open(el){ if(!el) return; clearTimeout(el._hideT); el.hidden=false; el._opener=document.activeElement; … }
close(el){ …; el._hideT=setTimeout(()=>{el.hidden=true;},500); el._opener?.focus?.(); }
```

b) **Fokus-Rückgabe fehlt [BELEGT]:** Nach dem Schließen landet der Fokus nirgends. Screenreader- und Tastaturnutzer verlieren die Position. Den Fix siehe oben (`_opener`).

c) **ARIA [BELEGT]:** Der Burger `button.header__burger` und der Warenkorb-Button haben kein `aria-expanded` und kein `aria-controls="MenuDrawer"`. Beim Öffnen und Schließen `aria-expanded` am Auslöser setzen.

d) **Scroll-Lock [VERMUTET]:** Nur `body.style.overflow='hidden'`. Auf älteren iOS-Safari-Versionen (unter 16) scrollt die Seite hinter dem Drawer trotzdem. Das `html`-Element wird nicht gesperrt, und es gibt keine Scrollbar-Kompensation (Desktop springt um die Scrollbar-Breite). Live auf iPhone prüfen.

Robuster:
```js
document.documentElement.style.overflow='hidden'; document.body.style.overflow='hidden';
document.documentElement.style.scrollbarGutter='stable';
```
Den Lock nur aufheben, wenn **kein** anderes Overlay mehr offen ist:
```js
if(!document.querySelector('.drawer.is-open,.search-overlay.is-open')) …
```

e) **Kleinigkeit:** `body.classList.contains('menu-open')` wird im Header-Scroll-Code abgefragt, aber nirgends gesetzt. Das ist toter Code.

f) **Einfacher `requestAnimationFrame`** direkt nach `hidden=false`: Die Slide-Transition kann ausfallen, weil der Browser Start- und Endzustand im selben Frame berechnet [VERMUTET]. Doppeltes rAF oder `void el.offsetWidth` davor.

---

### N-10 · P2 · Mobile Menü-Drawer: Details [BELEGT]
- **Social-Icons im Drawer-Fuß ungestylt:** `social-links` wird mit `class: 'social--drawer'` gerendert. Dafür gibt es **keine CSS-Regel**. `display:flex` gilt nur für `.footer .social` (Z. 603). Die Icons stehen deshalb **untereinander**. Außerdem hat `.social a` (Z. 604) einen weißen Rahmen `rgb(255 255 255/.15)`, der auf dem hellen Drawer-Fuß unsichtbar ist.
  ```css
  .social--drawer{display:flex;gap:.3rem;margin-top:.8rem}
  .social--drawer a{border-color:rgb(var(--c-line));color:rgb(var(--c-ink))}
  ```
- **Akkordeons schließen sich nicht gegenseitig.** Öffnet man "Stile" (14 Links) und "Geschenke", wird der Drawer sehr lang. Mit `name="mnav"` am `<details>` (exklusives Akkordeon, moderne Browser) oder per JS lösen.
- **Kein Hinweis auf die aktuelle Seite** im Drawer (kein `is-current` / `aria-current`).
- **Top-Level-Links** haben `font-size:1.35rem` Headline-Font mit Zeilenhöhe 1,6, also rund 66 px pro Zeile. Mit 7 bis 8 Punkten (N-5) passt das Menü in einen Screen.
- Mini-USP-Zeile im Drawer ergänzen (siehe N-5).

---

### N-11 · P2 · Typografie auf kleinen Screens [BELEGT/VERMUTET]
**Datei:** `assets/theme.css` Z. 28 bis 33

Kleinste Werte der `clamp`-Angaben: `.h0` 2,9rem = **46,4 px**, `.h1` 2,4rem = **38,4 px**, `.h2` 1,9rem = 30,4 px (rem = 16 px; das `html` hat keine eigene Größe, 17 px gelten nur für `body`).
- Es gibt **kein** `hyphens` und kein `overflow-wrap` für Headlines (grep: nur `.newsletter .section-heading__title{overflow-wrap:anywhere}` in Z. 922).
- Wörter wie "Weihnachtsgeschenke", "Haustierportraits" oder "Tierliebhaber" sind bei 38 bis 46 px Bricolage Bold breiter als 328 px [VERMUTET, inhaltsabhängig]. `body{overflow-x:clip}` schneidet sie ab, statt umzubrechen.
- `lang` ist gesetzt (`request.locale.iso_code` = "de"), `hyphens:auto` funktioniert also.
```css
.h0,.h1,.h2,.h3,.section-heading__title,.product__title,.article__title{overflow-wrap:break-word;hyphens:auto;-webkit-hyphens:auto}
@media (max-width:420px){.h0{font-size:clamp(2.3rem,11vw,2.9rem)}.h1{font-size:clamp(2rem,9.5vw,2.4rem)}}
```

**Zu kleine Schrift** [BELEGT]:
- Z. 633 bei 600 px und darunter: `.newsletter__badge span{font-size:.5rem}` = **8 px** (Newsletter-Badge im Footer und in der Startseiten-Sektion).
- `.yt-card__new` .62rem = 9,9 px.
- `.ba__proof-fig figcaption` .62rem.
- `.newsletter--footer .newsletter__badge span` .5rem.

Mindestens 11 px (0,7rem) verwenden.

---

### N-12 · P2 · iOS-Autozoom bei Formularfeldern unter 16 px [BELEGT]
Schriftgrößen der Felder:
- `.input`, `.textarea`, `.select`: erben 17 px, OK.
- Suche 1,4rem: OK.
- Newsletter 1rem: OK.
- **`.localization select`** (Z. 609): `.85rem` = 13,6 px. iOS zoomt beim Antippen hinein. Das betrifft den Footer, sofern mehr als eine Sprache oder ein Land aktiv ist. Es gibt Locales en/es/fr/it; ob sie veröffentlicht sind, ist [VERMUTET].
- **`.toolbar__sort select`** (Z. 482): `.9rem` = 14,4 px, Collection-Sortierung.
```css
@media (max-width:989px){.localization select,.toolbar__sort select{font-size:16px}}
```

---

### N-13 · P2 · Tap-Targets unter 44 px [BELEGT]
| Element | Größe | Ort |
|---|---|---|
| `.search-form__close` (Suche schließen) | **ca. 20×20 px**. Es gibt keine CSS-Regel, der Button-Reset setzt `padding:0` (Z. 15). | snippets/search-overlay.liquid |
| `.pmp-flyin__close` (×) | ca. 31×30 px (Z. 831) | Freebie-Fly-in |
| `.icon-btn` | 43,2 px (Z. 194), knapp | Header, Drawer |
| `.chip` (Such-Chips, Filter) | ca. 38 px hoch (Z. 111) | Suche, Blog, Bücher |
| Footer-Links `.footer__links a` | ca. 27 px hoch, Abstand 8,8 px | Footer |
| Footer-Rechtslinks `.footer__legal a` | ca. 21 px hoch (0,82rem) | Footer |
| `.social a` | 40 px | Footer |

**Fix:**
```css
.search-form__close,.pmp-flyin__close{min-width:44px;min-height:44px;display:inline-flex;align-items:center;justify-content:center}
.icon-btn{width:2.75rem;height:2.75rem}
@media (max-width:749px){.footer__links a,.footer__legal a{display:inline-block;padding:.35rem 0}.chip{min-height:40px}}
```

---

### N-14 · P2 · Kontraste [BELEGT, berechnet nach WCAG]
| Kombination | Kontrast | Bewertung |
|---|---|---|
| Akzent #E3261C auf Weiß | 4,61:1 | AA knapp bestanden (Normaltext) |
| Weiß auf Akzent (Primär-Button) | 4,61:1 | AA knapp |
| **Akzent auf Soft #F7F5F1** | **4,23:1** | **AA nicht bestanden für kleinen Text.** Betrifft `.card__eyebrow` (.74rem), `.yt-playlist__kicker`, `.project__year`, `.landing__part-kicker`, `.book__cat` auf `.section--soft` / `.yt__channel`. |
| Grau #5F5A55 auf Weiß / Soft | 6,82 / 6,26 | OK |
| Akzent auf Ink (Fokusring im Footer) | 4,19:1 | OK für Nicht-Text (≥3:1) |
| Footer-Titel (Weiß 55 %) auf Ink | 6,27:1 | OK |
| **Linienfarbe #E7E2DB als Input-Rahmen auf Weiß** | **1,29:1** | **WCAG 1.4.11 nicht bestanden.** Formularfelder, Newsletter-Zeile und Variant-Pills sind kaum als Felder erkennbar. |

**Fix:**
- Kleine Akzent-Labels auf Soft-Flächen in `accent-deep` #B5170F setzen (5,97:1 auf Accent-Soft, auf Soft ca. 6,2:1).
- Für Eingabefelder einen eigenen Rahmen-Token, z. B. `--c-field-line: 143 136 128` (ca. 3:1):
  ```css
  .input,.textarea,.select,.newsletter__row,.variant-option label,.qty{border-color:rgb(143 136 128)}
  ```

---

### N-15 · P2 · Fokus-Stile und Accessibility-Details [BELEGT]
- Das globale `:focus-visible` (Z. 21) ist gut.
- `.search-form__input:focus{outline:0}` (Z. 248): Es gibt keinen Ersatz-Indikator, die Unterlinie der Form ändert sich nicht. Fix: `.search-form:focus-within{border-bottom-color:rgb(var(--c-ink))}`.
- `.input:focus` nutzt als Ersatz Rahmen `ink` plus Schatten 6 %. Das ist OK.
- Header-Warenkorb: `<span data-cart-bubble aria-hidden="true">` umschließt `cart-count.liquid`, das selbst einen `visually-hidden`-Text "{{count}} Artikel im Warenkorb" enthält. Dieser Text wird so für Screenreader **versteckt**. Fix: `aria-hidden` vom Wrapper entfernen, denn die sichtbare Zahl hat bereits eigenes `aria-hidden`.
- Mega-Menü (Desktop): Escape schließt es nicht. Beim Schließen per Klick wird `aria-expanded` nicht zurückgesetzt, nur im `close()`-Timeout. Außerdem sitzt `aria-haspopup` auf einem `<a href>`, besser wäre ein separater `<button>` für das Aufklappen.
- `role="complementary"` am Fly-in ist OK. Das Fly-in schließt per Escape, ist aber nicht im Fokus-Fluss angekündigt. Für P2 genügt das.

---

### N-16 · P2 · Footer: Logik, Dubletten, Länge mobil [BELEGT]
- Die Spalte **"Shop"** enthält nur 2 echte Shop-Links (Alle Wandbilder, Kalender), dazu So funktioniert's, Magazin, Suche, Über uns und Kontakt, **alles Dubletten** zu den anderen Spalten. Es fehlen Kleidung, Tassen, Geschenke, Stile, Sonderwunsch und Handyhüllen.
- Die Brand-Spalte sagt zweimal dasselbe: Der Text "20 % unseres Gewinns gehen an Tierschutzprojekte …" und direkt darunter der Badge "20 % unseres Gewinns gehen an den Tierschutz". Einen Satz im Text streichen oder den Badge mit `/pages/tierschutz` verlinken.
- Mobil (unter 750 px) ist alles einspaltig (Z. 614): Newsletter, Brand, Social, 3 Menüs mit insgesamt 19 Links, Bottom. Das ist sehr lang.

**Fix:**
1. Menü `footer` im Admin neu befüllen: Alle Tierportraits (`mit-foto`) · Wandbilder · Tassen · Kleidung · Geschenke · Stile · Kalender 2027 · Sonderwunsch.
2. Mobil 2-spaltig:
   ```css
   @media (max-width:749px){.footer__top{grid-template-columns:1fr 1fr;gap:2rem 1.2rem}.footer__brand{grid-column:1/-1}}
   ```
   Alternativ die Menüs mobil als `<details>` rendern.

### N-17 · P1 (rechtlich) · Rechtstexte im Footer live prüfen [VERMUTET]
`sections/footer.liquid` rendert Rechtslinks ausschließlich per `{%- for policy in shop.policies -%}` (nur Policies mit Inhalt), plus Cookie-Einstellungen.

Laut API existieren **Impressum (LEGAL_NOTICE), Kontakt (CONTACT_INFORMATION), Datenschutzerklärung, Widerrufsrecht, Versand, AGB**, alle mit Inhalt.

Nicht sicher ist, ob Liquid `shop.policies` auch *Impressum* und *Kontaktinformationen* zurückgibt. Historisch lieferte das Objekt nur privacy, refund, shipping und terms (plus subscription).

**Live prüfen:** Steht "Impressum" im Footer-Bottom?

Falls nicht, das Impressum explizit ergänzen:
```liquid
{%- if shop.legal_notice_policy != blank -%}<a href="{{ shop.legal_notice_policy.url }}">{{ shop.legal_notice_policy.title }}</a>{%- endif -%}
```
Falls das Objekt `legal_notice_policy` nicht verfügbar ist, den Link fest verdrahten: `/policies/legal-notice`. Eine eigene Impressum-*Seite* gibt es nicht (Seitenliste geprüft). Das Impressum darf mobil nicht mehr als 2 Klicks entfernt sein.

Hinweis: Das Fly-in schließt `page.handle == 'impressum'` aus, die Seite existiert aber nicht. Das ist harmlos.

---

### N-18 · P2 · "Mobile-Diät" versteckt Inhalte ohne "Mehr anzeigen" [BELEGT]
`assets/theme.css` Z. 799 bis 803:
```css
@media (max-width:749px){
  .books>:nth-child(n+4),.testimonials>:nth-child(n+4),.featured-collection .product-grid>:nth-child(n+5),.faq .accordion>:nth-child(n+6){display:none}
  .books{grid-template-columns:1fr 1fr}
```
- **FAQ:** Ab Frage 6 unsichtbar, ohne Button. Das gilt überall, wo die FAQ-Sektion genutzt wird, also auch auf `/pages/faq` (`templates/page.faq.json`). Wer das Handy nutzt, sieht Antworten nicht, die im FAQ-Schema stehen [VERMUTET: welche Templates `.faq .accordion` nutzen, gehört zum Seiten-Audit].
- **Bücher:** 3 Bücher im 2-Spalten-Grid ergeben eine verwaiste Zelle. Auf der Bücher-Seite fehlen damit mobil alle Bücher ab Nr. 4. Die Filter-Chips filtern trotzdem nur die sichtbaren.
- **Testimonials** ab Nr. 4 und Produkte ab Nr. 5 werden ausgeblendet.

**Fix:** Die Diät nur auf die Startseite beschränken (`.template-index .books…`) und/oder einen "Alle anzeigen"-Button ergänzen. Für Bücher `:nth-child(n+5)` verwenden (4 Stück = 2 volle Reihen). **Nie** auf `template-page-faq` und `template-page-buecher` anwenden.

---

### N-19 · P2 · Performance global [BELEGT/VERMUTET]
1. **Grain-Overlay** (Z. 819): `body::after{position:fixed;inset:0;z-index:9998;mix-blend-mode:multiply;background:SVG feTurbulence}`. Eine Vollbild-Ebene mit Blend-Mode **über allem** (auch über Drawern und Suche) zwingt den Browser, bei jedem Scroll-Frame die ganze Seite zu überblenden. Zusammen mit `backdrop-filter:blur(14px)` im Sticky-Header (Z. 183) und im Drawer-Backdrop (Z. 221) kostet das auf schwachen Android-Geräten spürbar Frames [VERMUTET: im Chrome DevTools Performance-Panel mit 4× CPU-Throttling messen].
   ```css
   @media (max-width:749px),(pointer:coarse){.has-grain::after{display:none}}
   /* oder: statisches 180px-PNG ohne mix-blend-mode, opacity .035 */
   ```
2. **Doppelte Font-Preloads [BELEGT]:** `layout/theme.liquid` hat 2 manuelle `<link rel="preload">` (Heading und Body) **und** `snippets/css-variables.liquid` erzeugt dieselben 2 noch einmal per `preload_tag`. Das sind 4 Preload-Tags für 2 Dateien und erzeugt Konsolenwarnungen. Einen Satz entfernen, am besten den in `theme.liquid`.
3. **Font-Anzahl:** Heading n7 plus "bolder" (n8/n9), Body n4, Bold, Italic und Fraunces i4 ergeben bis zu 6 WOFF2-Dateien pro Seite. Fraunces wird nur für `.accent-word` und Zitate gebraucht. Prüfen, ob "bolder" und Body-Italic wirklich verwendet werden [VERMUTET].
4. **CSS:** 90 KB `theme.css` enthält Produkt-, Blog-, YouTube-, Bücher-, Landing- und Futterrechner-nahe Regeln für jede Seite und blockiert das Rendern. Mittelfristig nach Template aufteilen (z. B. `product.css`, `blog.css`) oder kritisches CSS inline einbinden (P2, größerer Umbau).
5. **Parallax-JS** (`[data-parallax]`) läuft auch auf Touch-Geräten (nur `!reduced` wird geprüft, `fine` nicht), mit `getBoundingClientRect` pro Frame. Prüfen, ob Sektionen `data-parallax` nutzen. Falls ja, `&& fine` ergänzen oder auf CSS `animation-timeline` umstellen (wie Z. 903 bis 910, die schon auf ≥990 px begrenzt ist).
6. **AdSense/GA4:** korrekt gegatet (Blog-only plus Consent). GA4 ist leer. **Kein Handlungsbedarf.**

---

### N-20 · P2 · JS-Robustheit [BELEGT]
`assets/theme.js` ist **eine** IIFE. Wirft ein Block einen Fehler, fallen alle danach folgenden Features aus (Predictive Search, Marquee-Duplikat, Localization, Größen-Guide, Event `pmp:ready`).

Fehlende Null-Checks:
- Upload: `input.addEventListener(...)`, `drop.addEventListener(...)`, `img.src` ohne Prüfung, ob `[data-upload-input]`, `[data-upload-drop]` und `[data-upload-img]` existieren.
- Size-Guide: `frame.style…` ohne Prüfung von `frame`.
- Predictive Search: `r.ok` wird nicht geprüft. Bei 4xx/5xx bleibt die Liste stumm leer.

**Fix:** Jeden Feature-Block in `try { … } catch (e) { console.warn('[PMP]', e) }` kapseln (eine kleine Hilfsfunktion `safe(name, fn)`) und die Null-Checks ergänzen.

---

### N-21 · P2 · Hover-Effekte auf Touch ("sticky hover") [BELEGT]
`.btn:hover{transform:translateY(-2px)}` (Z. 95), `.step:hover`, `.book:hover`, `.landing__part:hover`, `.mega__list li a:hover`, `.tile:hover img` und weitere stehen **nicht** in `@media (hover:hover)`. Auf iOS/Android bleibt der Hover-Zustand nach dem Tippen hängen: Buttons bleiben 2 px angehoben, Kacheln bleiben gezoomt.

Fix: Die Hover-Transforms in `@media (hover:hover){…}` verschieben, wie es bei `.card` (Z. 129) schon gemacht ist.

---

### N-22 · P2 · Search-Overlay mobil [BELEGT/VERMUTET]
- `.search-overlay__panel{margin:6vh auto 0;max-height:85vh}` (Z. 243): Mit offener iOS-Tastatur ist `vh` der Layout-Viewport. Das Panel kann unter die Tastatur reichen, die Ergebnisse sind dann nur per inneres Scrollen erreichbar [VERMUTET]. Fix: `max-height:calc(100dvh - 2rem)` und mobil `margin-top:.75rem`.
- Schließen-Button 20 px (siehe N-13).
- Die `input`-Chips führen auf `/search?q=…` und nicht direkt auf Collections. "Hundeportrait" wäre besser direkt `/collections/hundeportraits`. Dafür bräuchte es ein neues Setting im Format `Label|URL`.

---

### N-23 · P2 · Sonstiges [BELEGT]
- `var(--h,4.6rem)` wird in `.product__gallery`, `.facets`, `.article__side` und `.cart-page__side` für `top` genutzt. `--h` ist aber nur auf `.header` definiert, nicht auf `:root`, also greift immer der Fallback 4,6rem. Betrifft nur den Desktop (die Elemente sind dort sticky). Fix: `:root{--h:4.6rem}` plus `.header.is-scrolled` bzw. `.is-hidden` per JS spiegeln, oder einfach so lassen.
- `body{overflow-x:clip}` (Z. 12): Safari unter 16 kennt `clip` nicht. Fallback: `html,body{overflow-x:hidden}` ist tabu, weil es Sticky kaputt macht. Besser die Überlaufquellen beseitigen (N-1, N-11). Keine Aktion nötig, außer N-1 und N-11 zu fixen.
- Inkonsistenz Stil-Anzahl: `settings.art_styles` nennt 11 Stile, das Menü "Stile" 13, `locales/de.default.json` → `sections.transformation.step2_text` sagt "13 Stile". Einheitlich machen (gehört inhaltlich auch zum Startseiten-Audit).
- Die Einstellung `logo_height` heißt "Logo-Höhe (Desktop)", gilt aber auch mobil. Das Label ist irreführend, siehe N-1.
- `cart_free_shipping_threshold: 0` blendet die Versandleiste aus. Das Schema-Default ist 69 €. Das ist bewusst so, ich erwähne es nur zur Information. Die Versandkosten sollten im Cart-Drawer vor dem Checkout sichtbar sein. Aktuell steht dort nur "Versand im Checkout" (Conversion: unbekannte Versandkosten sind Abbruchgrund Nr. 1, **gehört zum Warenkorb-Audit**).

---

## 3. Was ausdrücklich OK ist [BELEGT]
- **Cursor/Tilt/Magnetic/Spotlight/Galerie-Zoom:** CSS `.pmp-cursor{display:none}` greift nur bei `(hover:hover) and (pointer:fine)`. Im JS ist alles an `fine && !reduced` gekoppelt. Auf Touch ist also nichts aktiv.
- **prefers-reduced-motion:** Scroll-Behavior, Marquee, Reveal, Hero-Zeilen, Buttons, Cursor, Fly-in und Parallax per `animation-timeline` werden abgeschaltet. JS-Reveal und Counter respektieren die Einstellung. Einzige Lücke: die `--split`-Reveal-Transition beim Vorher/Nachher-Slider (Z. 898), das ist unkritisch.
- **Sektionsabstand:** mobil rund 50 px (siehe 1.6). **Keine Änderung nötig.**
- **Reveal-on-scroll:** Elemente im ersten Viewport sind sofort sichtbar (kein LCP-Delay), mit Sicherheitsnetz nach 1,8 s und bfcache-Fix.
- **Scripts:** alle `defer`. Produkt-Skripte laden nur auf Produktseiten. GA4 und AdSense erst nach Consent. AdSense nur im Blog (`adsense_blog_only: true`).
- **Menüziele:** alle 37 geprüften Collections, Produkte und Seiten existieren und sind im Onlineshop veröffentlicht. Keine 404-Links im Menü.
- **Skip-Link**, `main tabindex=-1`, Fokus-Trap in Drawern (Tab/Shift+Tab), Escape schließt Drawer und Suche.
- **Horizontal scrollende Bereiche** (YouTube-Grid Z. 681 mit negativem Margin, `.ba__products`, `.cross-sell__list`, `.gallery__thumbs`) haben eigenes `overflow-x:auto`. Kein Seiten-Überlauf.
- **Marquee:** Die Kopie ist `aria-hidden`, pausiert bei Hover und ist bei reduced-motion aus.

---

## 4. Empfohlene Umsetzungsreihenfolge für morgen
1. **N-1 plus N-2** (Header mobil: Logo 40 px, Gap, YouTube und Konto ausblenden, kompakter CTA). Danach **Screenshots bei 360, 375, 390 und 430 px**.
2. **N-9a/b/c** (Drawer-Race, Fokus, ARIA): kleine JS-Änderung.
3. **N-8** (Safe-Area-CSS, 4 Zeilen).
4. **N-7** (Announcement mobil rotierend).
5. **N-6** (Suche plus Seiten).
6. **N-4, N-5, N-3** (Menü im Admin umbauen, Promo-Titel anpassen, "show_all"-Logik). **Vorher mit dem Betreiber abstimmen.** Menü-Änderungen sind Admin-Daten und nicht Theme-Code.
7. **N-17** Impressum im Footer live verifizieren.
8. P2 nach Zeit: N-10 (Drawer-Social), N-11 (Hyphens), N-12 (iOS-Zoom), N-13 (Tap-Targets), N-14 (Kontraste), N-18 (Mobile-Diät), N-19 (Grain mobil aus, doppelte Preloads), N-20, N-21, N-22.

**Live-Prüfliste (was diese Analyse nicht sehen konnte):**
- Header bei 360 und 375 px: Ist das Cart-Icon abgeschnitten?
- Announcement: Ist Item 2 sichtbar?
- Cart-Drawer auf dem iPhone: Liegt "Warenkorb ansehen" unter dem Home-Balken?
- Scroll-Lock auf iOS
- Impressum im Footer
- Lange H1 auf Collection-Seiten (z. B. "Weihnachtsgeschenke für Tierliebhaber")
- Fly-in zusammen mit dem Cookie-Banner
- Wird die Localization-Auswahl gerendert?
- Scroll-Performance mit Grain auf einem Android-Mittelklassegerät
