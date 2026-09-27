# Audit 02: Startseite printmypet.de (Fokus Mobil 360–430 px)

Stand: 27.09.2026 · Live-Theme `gid://shopify/OnlineStoreTheme/208344613202` · nur gelesen, nichts geändert.

**Wie die Befunde belegt sind:**
- **[CODE]**: direkt aus Theme-Dateien belegt (Datei, Zeile/Selektor/JSON-Key angegeben).
- **[DATEN]**: per Admin-GraphQL belegt (Produkte, Collections, Dateien, Metafelder).
- **[SCHÄTZUNG]**: aus CSS-Werten berechnet (Pixelmaße), live prüfen.
- **[VERMUTET]**: plausibel, aber ohne Browser nicht prüfbar. Muss live geprüft werden.

Die Website selbst war aus der Umgebung nicht erreichbar. Alle Aussagen zur Optik sind aus CSS/Liquid abgeleitet.

Hinweis zu fehlenden Settings: Viele Sektionen in `templates/index.json` haben nicht alle Settings gesetzt (z. B. `custom-request` hat `"settings":{}`). Shopify nimmt dann den `default` aus dem `{% schema %}`. Deshalb stehen unten bei solchen Texten die Schema-Defaults als Live-Text. Das gilt als [CODE], sollte aber einmal live gegengelesen werden.

---

## 0. Kurzfazit

1. **P0: Der Haupt-CTA führt auf eine fast leere, falsche Seite.** „Portrait gestalten“ (Header, Hero, Vorher/Nachher, So funktioniert's) zeigt auf `/collections/schaufenster`. Von den 22 Produkten dort sind 20 **ARCHIVED**. Sichtbar bleiben nur der *Wandkalender 2027* und *Dein Hund als digitales Kunstwerk*. Keins der 33 aktiven Wandbild-Portraits ist dabei. [DATEN]
2. **P1: Mobil sieht man above the fold kein einziges Tierbild.** Auf Mobil kommt der Hero-Text vor den Bildern. Die Bilder starten ca. 850 px unter dem Seitenanfang. [SCHÄTZUNG]
3. **P1: Die Seite ist mobil extrem lang.** 15 Sektionen, grob 18.000–20.000 px (ca. 22–25 Bildschirmhöhen). Viel Inhalt ist doppelt: zwei Stil-Galerien mit denselben 12 Hundebildern, zwei „3 Schritte“-Blöcke, drei Content-Sektionen (YouTube, Blog, Bücher) hintereinander, Freebie-Banner plus Freebie-Fly-in. [CODE]/[SCHÄTZUNG]
4. **P1: Auf der Seite steht nirgends „ab X €“.** Der erste Preis erscheint in Sektion 6 (Bestseller). Ein Preisanker wäre möglich: Poster ab 23,99 €, Tasse ab 19,99 €, Sticker ab 12,99 €. [DATEN]
5. **P1: Das Sortiment ist kaum sichtbar.** Es gibt 120 aktive Produkte: Tassen, Shirts, Hoodies, Decken, Kissen, Handyhüllen, Kalender, Gutschein, Digitalportrait, Wunschbild. Die Startseite zeigt fast nur Wandbilder. Der Hero-Text nennt nur Leinwand, Poster, Holzrahmen und Acrylglas. [CODE]/[DATEN]
6. **P1: Der Bücher-Filter ist mobil kaputt.** Bei „Katze“, „Pferd“ und „Kinder“ bleibt die Liste leer, weil CSS ab dem 4. Buch alles ausblendet. [CODE]
7. **P1: Es gibt keinerlei Social Proof.** Judge.me ist installiert, aber keins der 120 aktiven Produkte hat eine Bewertung. Auf der Startseite steht auch keine Garantie, obwohl PDP und Collection eine „Zufriedenheitsgarantie“ versprechen. [DATEN]/[CODE]
8. **Widersprüche:** Die Zahl der Stile variiert: 11 (globales Setting), 12 plus Original, 13, bei Kleidungsprodukten „10 Stile“. „20 % vom Gewinn“ steht 8-mal auf der Seite, „Live-Vorschau“ 7-mal. Es heißt mal „EU“, mal „Europa“. Die Antwortzeit ist einmal „24 Stunden“, einmal „wenige Stunden“. [CODE]

---

## 1. IST-Zustand: Sektionen der Startseite in Reihenfolge

Quelle: `templates/index.json` → `"order"`. Es gibt keine deaktivierten Sektionen (`"disabled"` taucht nicht auf). Folgende Sektionsdateien existieren im Theme, werden aber **nicht** auf der Startseite genutzt: `usp-row`, `testimonials`, `projects`, `image-with-text`, `rich-text`, `pmp-wunschbild-beispiele`, `custom-liquid`.

Außerhalb von `index.json` erscheinen oben die Header-Gruppe (Announcement plus Header) und global über `layout/theme.liquid` das **Freebie-Fly-in** (`snippets/pmp-freebie-flyin.liquid`). Das Fly-in erscheint auch auf der Startseite, sobald man ≥ 35 % oder ≥ 900 px gescrollt hat.

| # | Key / Typ | Überschrift (Eyebrow) | Kernaussage | CTA(s) → Ziel |
|---|---|---|---|---|
| – | Announcement (header-group) | – | „20 % unseres Gewinns gehen an den Tierschutz“ · „Druck in Europa · Lieferung in 5 bis 9 Werktagen“ | – |
| – | Header | – | Der Header-CTA „Portrait gestalten“ ist **mobil ausgeblendet** (`theme.css` `@media (max-width:989px){.header__cta{display:none}}`) | → /collections/schaufenster |
| 1 | `hero` / hero | „Personalisierte Tierportraits“ · H1 „Dein Tier. / Dein Kunstwerk. / *Für immer.*“ | Lead (262 Zeichen): Handyfoto → Portrait im Lieblingsstil, gedruckt auf Leinwand, Poster, im Holzrahmen oder auf Acrylglas, dazu Live-Vorschau. Trust: Live-Vorschau · Druck & Versand aus der EU · 20 % vom Gewinn. Sticker „Live-Vorschau“, Pill „20 % vom Gewinn…“, YouTube-Karte (Variante **B**, groß) | „Portrait gestalten“ → schaufenster; „So funktioniert's“ (Icon **play**) → #so-funktionierts |
| 2 | `marquee` | – | Laufband (schwarz): Live-Vorschau · 20 % Tierschutz · Gedruckt in Europa · Jedes Motiv möglich · Hund · Katze · Pferd | – |
| 3 | `transformation` / transformation-showcase | „Die Verwandlung“ · „Vom Handyfoto zum *Kunstwerk*“ | Tabs für Tierart (3) × Stil (12), Vorher/Nachher-Schieber, Material-Reihe (11 Produkte pro Tierart), 3 Schritte (aus der Locale), Beweis „schlechtes Handyfoto“ | „Eigenes Foto verwandeln“ → schaufenster; „Alle Stile ansehen“ → #stile |
| 4 | `freebie` / pmp-freebie-banner | „Kostenlos · 50 Seiten · PDF“ · „Die ersten *30 Tage* mit Welpe“ | Welpen-Startpaket, 4 Bullets | „Startpaket gratis sichern“ → /pages/welpen-startpaket |
| 5 | `tiles` / collection-tiles | „Für wen?“ · „Hund, Katze, Pferd: *jedes Tier* ein Original“ | 4 Kacheln: Hunde, Katzen, Pferde, Geschenke | Kacheln → hunde-/katzen-/pferdeportraits, personalisierte-geschenke; „Alle Produkte“ → alle-produkte |
| 6 | `bestsellers` / featured-collection | „Bestseller“ · „Was gerade *Herzen erobert*“ | 4 Produkte aus `bestseller`: Hund Leinwand ab 39,99 €, Katze Holzrahmen ab 49,99 €, Hund Acryl-Aufsteller ab 46,99 €, Wandkalender 49,99 € | „Alle ansehen“ → /collections/bestseller |
| 7 | `how` / how-it-works | „So funktioniert's“ · „Drei Schritte bis zum *Gänsehaut-Moment*“ | Foto hochladen → Stil wählen („**13 Stile**“) → Druck, 5–9 WT, 20 % Tierschutz | „Jetzt starten“ → schaufenster |
| 8 | `styles` / style-showcase (dunkel) | „**12 Stile plus Original**“ · „Welcher Stil passt zu *deinem Tier*?“ | 12 Stilkarten mit Vorher-Umschalter. Text: „**Fahre über eine Karte**…“ | Karten → /collections/stil-* |
| 9 | `request` / custom-request (Settings leer → Schema-Defaults) | „Sonderwunsch“ · „Wir drucken *jedes Motiv*, auf Wunsch“ | Mehrere Tiere, Regenbogenbrücke usw., Antwort in 24 h. **Komplettes Kontaktformular** mit Produkt-Dropdown | „Anfrage senden“ (contact-Form) |
| 10 | `donation` / donation-story (Texte = Defaults) | „Unser Tierschutz-Beitrag“ · „20 % unseres Gewinns gehen an *Tiere, die es brauchen*“ | Community wählt auf YouTube. Hinweis: „Wir sind 2026 gestartet… erstmals 2027 ausgezahlt“ | „Mehr zum Tierschutz“ → /pages/tierschutz; „Zum Voting-Kanal“ → YouTube (extern) |
| 11 | `youtube` / youtube-hub (Heading = Default) | „YouTube“ · „Hundewissen, das *wirklich hilft*“ | 3 beliebteste plus 3 neueste Videos (aus Metafeldern), Kanal-Box, Playlist | „Abonnieren“ → YouTube |
| 12 | `blog` / featured-blog | „Wissen“ · „Ehrliches Wissen für *Tiermenschen*“ | 3 neueste Magazin-Artikel (57 Artikel im Blog) | „Alle Artikel“ → /blogs/magazin |
| 13 | `books` / amazon-books | „Unsere Bücher“ · „Lesestoff für *Tiermenschen*, und ihre Kinder“ | 9 Bücher (mobil nur 3 sichtbar), Filter-Chips | „Bei Amazon ansehen“ (extern) |
| 14 | `faq` / faq (Heading = Default) | „Fragen & Antworten“ · „Alles, was du *wissen willst*“ | 4 Fragen: Foto, Vorschau, Lieferzeit, Tierschutz | „Frage stellen“ → /pages/contact |
| 15 | `newsletter` / newsletter-brevo | Badge „10 % Rabatt“ · „Willkommensrabatt & *Foto-Tipps*“ | Wöchentlich Foto-Tricks, Stile, Voting | „Anmelden“ (Brevo-Form) |

**Geprüfte Link- und Datenziele [DATEN]:** Alle verlinkten Collections existieren: schaufenster, bestseller, hunde-/katzen-/pferdeportraits, personalisierte-geschenke, alle-produkte und alle 12 `stil-*`. Das gilt auch für die Seiten tierschutz, contact und welpen-startpaket sowie den Blog `magazin`. Alle referenzierten `shop_images` sind vorhanden und READY: Hero 1–3, 36 Nachher-Bilder `pmp222-*`, 36 Vorher-Bilder `pmp225-vorher-*`, Kacheln, Spendenbild, 9 Buchcover, Freebie-Cover, Beweisbilder. Pro Tierart gibt es genau 11 aktive Wandbild-Produkte mit passendem `produkt:`-Tag, die Material-Reihe ist also vollständig. Die Bestseller-Collection hat 4 aktive Produkte. Einzige Datenpanne ist **schaufenster** (siehe P0-1).

---

## 2. 5-Sekunden-Test mobil (Erstbesucher, 390 × 844)

| Frage | Beantwortet? | Beleg |
|---|---|---|
| Was ist das? | Teilweise. „Personalisierte Tierportraits“ steht nur als kleine Eyebrow (.78rem, Versalien). Die H1 ist emotional, aber ohne Produkt. Der Lead erklärt es, ist aber 7 Zeilen lang. | `hero.liquid`, `theme.css:.eyebrow`, `.lead` |
| Für wen (Tierarten, Geschenk)? | Nein. Hund, Katze und Pferd tauchen erst im Laufband bzw. in Sektion 5 auf. „Geschenk“ fehlt im Hero komplett. | index.json |
| Preis ab? | Nein. Erster Preis in Sektion 6 (ca. 5.500 px Scrolltiefe). | [SCHÄTZUNG] |
| Wie bestelle ich? | Der CTA ist sichtbar, **führt aber auf die kaputte Schaufenster-Seite**. | P0-1 |
| Warum vertrauen? | 20 % Tierschutz, EU-Druck, Live-Vorschau: ja. Bewertungen, Garantie, Zahlungsarten, Bestellzahlen: nein. | – |
| Was bietet PMP noch? | Nein. Tassen, Kleidung, Decken, Gutschein, Digitalportrait, Wunschbild und Futterrechner fehlen oder sind kaum sichtbar. Magazin, YouTube und Bücher liegen ganz unten. | – |

---

## 3. Befunde

### P0: kaputt oder direkter Umsatzverlust

#### P0-1 · Haupt-CTA „Portrait gestalten“ führt auf eine fast leere Collection mit falschen Produkten [DATEN]
- **Wo:** `templates/index.json` → `sections.hero.settings.cta_primary_url`, `sections.transformation.settings.cta_url`, `sections.how.settings.cta_url` (alle `shopify://collections/schaufenster`). Außerdem `sections/header-group.json` → `header.settings.cta_url` und global `config/settings_data.json` → `cart_cross_sell_collection: "schaufenster"` (Warenkorb-Cross-Sell, gehört nicht zu meinem Bereich, ist aber dasselbe Problem).
- **Problem:** Collection `schaufenster` ist manuell befüllt, hat 22 Produkte und wurde am 26.09.2026 21:42 UTC aktualisiert. **20 davon sind ARCHIVED** (alte Produkte wie „Hundeportrait: Poster“, „Katzenportrait: Leinwand“ usw.). Aktiv sind nur `haustier-wandkalender-2027` und `dein-hund-als-digitales-kunstwerk-sofort-download`. Die Collection-Seite rendert nur veröffentlichte, aktive Produkte. Wer „Portrait gestalten“ tippt, sieht also **2 Produkte: einen Kalender und eine Digitaldatei**. Die 33 aktiven Wandbild-Portraits (Leinwand, Poster, Acryl…) fehlen. Mobil ist das der wichtigste Klickpfad, denn der Header-CTA ist dort ausgeblendet und die Hero-Buttons sind der erste CTA.
- **Fix (eine der Optionen):**
  - **A (Daten, empfohlen):** `schaufenster` neu befüllen: 33 aktive Wandbild-Portraits (11 × Hund/Katze/Pferd), `wunschbild-von-deinem-tier`, 2–3 Tassen oder Hoodie, digitales Portrait. Archivierte Produkte entfernen. Oder die Collection auf Smart-Regeln umstellen, z. B. `tag = typ:portrait` UND `type = Wandbild`. Die Collection `wandbilder` hat bereits genau diese Regeln.
  - **B (Theme-Settings):** Alle 4 CTA-URLs auf `shopify://collections/wandbilder` umstellen. Vorher live prüfen, ob dort nur aktive Produkte in guter Reihenfolge stehen (`productsCount` 49 zählt auch archivierte).
  - **C (besser für Conversion, aber mehr Aufwand):** Der Primär-CTA führt zu einer Tierart-Auswahl (3 große Kacheln Hund/Katze/Pferd). Die gibt es mit Sektion 5 schon, also kann der Hero-CTA auch auf `#fuer-wen` springen.
- **Nebenbefund:** `featured-collection.liquid` zeigt bei leerer Collection 4 **Fake-Karten mit „ab 39,90 €“ und „Beispielprodukt“**. Das passiert, sobald die Bestseller-Produkte archiviert werden, so wie es bei `schaufenster` passiert ist. Siehe P2-15.

### P1: wichtig (Conversion, Verständlichkeit, klare Fehler)

#### P1-1 · Hero mobil: kein Bild above the fold, der CTA kommt knapp [SCHÄTZUNG, live prüfen]
- **Wo:** `assets/theme.css` Z. 261 (`.hero__inner{grid-template-columns:1.05fr .95fr}`) und Z. 282 (`@media (max-width:989px){.hero__inner{grid-template-columns:1fr}.hero__visual{…aspect-ratio:1/1…}}`), dazu `sections/hero.liquid`.
- **Rechnung bei 390 × 844:** Announcement 40 px + Header 74 px (`--h:4.6rem`) + Hero-Padding 32 px + Eyebrow ca. 35 px. Die H1 ist 46,4 px groß (`.h0{font-size:clamp(2.9rem,7.2vw,6.4rem)}`, bei 390 px greift das Minimum 2.9rem). Bei 3–4 Zeilen sind das ca. 135–180 px. „Dein Kunstwerk.“ passt bei 358 px Innenbreite nur knapp in eine Zeile. Dann 22 px Abstand, der Lead mit 7 Zeilen à 26 px (ca. 182 px), 32 px Abstand und der Button mit 55 px. **Der Primär-CTA endet bei ca. 630 px.** Der Sekundär-Button bricht in eine zweite Reihe um (`.btn{white-space:nowrap}` plus `btn--lg`). Danach kommen die Trust-Liste (3 Zeilen, ca. 125 px) und 32 px Gap. **Das erste Bild beginnt bei ca. 850 px**, also unterhalb des sichtbaren Bereichs (in Safari ca. 750 px nutzbar). Bei 360 × 640 liegt auch der CTA unter dem Fold.
- **Problem:** Ein visuelles Produkt (Kunstportraits) startet mobil mit einer reinen Textwand. Der „Wow“-Moment fehlt.
- **Fix-Vorschlag (live testen):**
  ```css
  @media (max-width:749px){
    .hero{padding-top:1rem}
    .hero__visual{order:-1;aspect-ratio:4/3;max-width:none;margin-bottom:.5rem}
    .hero__title{font-size:clamp(2.2rem,10.5vw,2.9rem)}
    .hero__lead{font-size:1rem;margin-bottom:1.2rem}
    .hero__actions .btn--primary{flex:1 1 100%}
    .hero__actions .btn--secondary{display:none}          /* oder als Textlink */
    .hero__pill,.hero__yt{display:none}                    /* Bild-Collage entrümpeln */
    .hero__trust{margin-top:1.2rem;gap:.5rem 1rem;font-size:.82rem}
  }
  ```
  Dazu einen kürzeren Lead in `index.json` → `hero.settings.text`, z. B.:
  > „Aus deinem Handyfoto wird ein Portrait in 12 Kunststilen – als Leinwand, Poster, Tasse oder Hoodie. Mit Live-Vorschau. Ab 23,99 €.“

  Den Preis vorher gegen den günstigsten aktiven Wandbild-Preis prüfen: Hunde-Poster laut [DATEN] ab 23,99 €.

#### P1-2 · YouTube-Karte (Variante B) im Hero lenkt vom Kauf ab und führt aus dem Shop [CODE]
- **Wo:** `index.json` → `hero.settings.yt_badge: "b"`, dazu `sections/hero.liquid` (`.hero__yt`, `target="_blank"`) und `assets/pmp-222.css` (`.hero__yt--b{top:-3.6rem;…min-width:18rem}`, mobil `top:-2.6rem; max-width:92%`).
- **Problem:** An der wertvollsten Stelle der Seite steht eine große Karte „Folge uns auf YouTube / Rasseportraits und Hundewissen“ (Defaults, weil `yt_title` und `yt_sub` nicht gesetzt sind) mit externem Link. Mobil sitzt sie 2,6 rem über der Bildfläche und überlappt [VERMUTET] die Trust-Liste darüber (Gap ist nur ca. 32 px, die Karte ist ca. 80 px hoch). Außerdem stapeln sich auf 358 px Breite 3 Bilder, Sticker, Pill und YouTube-Karte.
- **Fix:** `yt_badge: "none"` (oder `"a"` nur ab Desktop). YouTube hat eine eigene Sektion und ein Header-Icon.

#### P1-3 · Kein Preisanker, keine Garantie, keine Versandkosten auf der Startseite [CODE]/[DATEN]
- **Problem:** Auf der ganzen Startseite steht bis Sektion 6 kein Preis. Versandkosten (laut `templates/product.json` acc2: 5,99 € in DE, ab 150 € frei) fehlen komplett. Die auf PDP und Collection versprochene „Zufriedenheitsgarantie – Nicht glücklich? Wir drucken neu.“ (`templates/collection.json` usp u4, `product.json` trust t3) fehlt auf der Startseite. Das Schema-Preset im Hero hatte sie noch („Zufriedenheitsgarantie“, `hero.liquid` presets).
- **Fix:**
  - `index.json` → `hero.blocks.t1.settings.text` (Live-Vorschau ist sowieso doppelt, siehe P1-8) ersetzen durch „Poster ab 23,99 €, Leinwand ab 39,99 €“ oder „Ab 23,99 € · Versand 5,99 €“.
  - Dazu einen 4. Trust-Block (max. 4 erlaubt) mit Icon `shield` und Text „Zufriedenheitsgarantie: Wir drucken neu“, **nur wenn intern bestätigt**, weil es ein rechtlich bindendes Versprechen ist.
  - In der FAQ eine Frage „Was kostet der Versand?“ ergänzen (siehe P1-11).

#### P1-4 · Das Sortiment wird nicht erklärt: „Was bietet PrintMyPet alles?“ [CODE]/[DATEN]
- **Daten:** 120 aktive Produkte. Neben 33 Wandbildern und 4 Acryl-Aufstellern gibt es ca. 20 Tassen, ca. 20 Shirts, Hoodies, Sweat und Kinderkleidung, Decken, Kissen, Handyhüllen, Taschen, Sticker, Baumschmuck, Mauspad, Flagge, Kalender 2027, Gutschein, 4 Digitalportraits und 2 Wunschbild-Produkte.
- **Problem:** Hero-Lead und So-funktioniert's nennen nur Wandbild-Materialien. Die 4 Kacheln zeigen Tierarten plus „Geschenke“. Tassen, Kleidung, Decken, Gutschein, Digitalportrait, Wunschbild und Futterrechner kommen auf der Startseite nicht vor. Im Sonderwunsch-Dropdown stehen „Webdecke (kommt bald), Kissen (kommt bald)“, obwohl Kissen und Decken längst aktiv sind (siehe P1-10).
- **Fix:** Die Sektion `tiles` (collection-tiles, max. 8 Blöcke) auf zwei Ebenen erweitern oder eine zweite Kachelreihe „Was wir drucken“ anlegen. Beispiel für Blöcke (Bilder aus vorhandenen Produktbildern, Untertitel mit „ab“-Preis laut [DATEN]):
  - Wandbilder · „Leinwand, Poster, Acryl · ab 23,99 €“ → /collections/wandbilder
  - Tassen · „mit Foto oder Spruch · ab 19,99 €“ → Tassen-Collection (Handle live prüfen)
  - Kleidung · „Shirt, Hoodie, Kids · ab 24,99 €“
  - Decken & Kissen · „ab 28,99 €“
  - Geschenkgutschein · „ab 25 €“ → /products/geschenkgutschein
  - Wunschbild · „Deine Idee als Bild“ → /products/wunschbild-von-deinem-tier

  Die Kachelreihe mobil als horizontalen Scroller mit `scroll-snap` bauen (Muster wie `.yt__grid` in `theme.css` Z. 681).

#### P1-5 · Zwei Stil-Galerien mit denselben 12 Bildern [CODE]
- **Wo:** Sektion 3 `transformation` (36 Shots, Hund zuerst, gleiche Bilder `pmp222-hund-*`/`pmp225-vorher-hund-*`) und Sektion 8 `styles` (12 Karten mit genau denselben `pmp222-hund-*`- und `pmp225-vorher-hund-*`-Bildern).
- **Problem:** Mobil kostet das zusammen ca. 4.400 px [SCHÄTZUNG]. Der Nutzer sieht zweimal dieselben 12 Hundebilder mit Vorher/Nachher.
- **Fix:** Eine der beiden entfernen oder stark kürzen. Empfehlung: `transformation` behalten, weil sie interaktiv ist, Tierarten und Materialien zeigt und mit Produkten verlinkt. `styles` entweder aus `order` entfernen oder mobil als kompakten horizontalen Scroller umbauen:
  ```css
  @media (max-width:749px){
    #stile .grid--3{display:flex;overflow-x:auto;scroll-snap-type:x mandatory;gap:.8rem;
      margin-inline:calc(var(--gutter)*-1);padding:0 var(--gutter) .5rem;scrollbar-width:none}
    #stile .grid--3::-webkit-scrollbar{display:none}
    #stile .grid--3>*{flex:0 0 68%;scroll-snap-align:start}
  }
  ```

#### P1-6 · Style-Showcase mobil: 12 Karten untereinander, Hover-Text, falsche `sizes` [CODE]
- **Wo:** `sections/style-showcase.liquid`. Grid `grid--3` wird mobil 2-spaltig (`theme.css` Z. 69). 12 Karten ergeben 6 Reihen, ca. 1.900 px [SCHÄTZUNG].
- **Text:** `index.json` → `styles.settings.text` = „…**Fahre über eine Karte**, um das Handyfoto dahinter zu sehen…“. Auf Touch-Geräten gibt es kein Hover. Mobil erscheint stattdessen ein Button „Vorher“ (`.card__flip-btn`).
  → Fix: „Tippe auf **Vorher**, um das Handyfoto zu sehen.“ (oder geräteneutral: „Vorher/Nachher: Tippe oder fahre über eine Karte.“)
- **Bildgrößen:** `render 'image' … sizes: '(min-width: 990px) 33vw, 100vw'`, aber mobil ist die Karte ca. 50vw breit. Ab 1200 px sind es 4 Spalten (`pmp-222.css`). Pro Karte werden **2 Bilder** gerendert (nachher plus vorher, beide lazy). Bei DPR 3 zieht der Browser ca. 1080–1120 px statt 540 px, bei 24 Bildern also rund doppelt so viele Bytes wie nötig.
  → Fix: `sizes: '(min-width:1200px) 25vw, (min-width:990px) 33vw, 50vw'` (bei Umbau zum Scroller `70vw`).
- **HTML:** `<button class="card__flip-btn">` steckt in einem `<a class="card">`. Interaktive Elemente in Links sind ungültiges HTML. Das funktioniert heute nur wegen `preventDefault` im Capture-Listener (`theme.js`, Abschnitt „Style cards: Vorher/Nachher flip“). Screenreader und Tastatur verhalten sich uneinheitlich.
  → Fix: Die Karte als `<article>` bauen, Link und Button getrennt.

#### P1-7 · Vorher/Nachher-Sektion mobil sehr lang: Chip-Wolke und doppelte 3 Schritte [CODE]
- **Wo:** `sections/transformation-showcase.liquid`, `theme.css` Z. 296–340 (`.ba__tabs{flex-wrap:wrap}`), Locale `sections.transformation.step1..3`.
- **Problem mobil:** Die Reihenfolge mobil ist Heading → 3 Tierart-Chips → **12 Stil-Chips umgebrochen (ca. 4–5 Reihen, ca. 200 px)** → Bühne 4:5 (ca. 450 px) → Material-Reihe → **3 Schritte (01–03)** → Beweis-Box → 2 Buttons. Das sind ca. 2.200 px [SCHÄTZUNG]. Die 3 Schritte wiederholen fast wörtlich die Sektion 7 „So funktioniert's“. Die Material-Thumbnails haben 0,68 rem Schrift (ca. 11 px) und keine Preise.
- **Fix:**
  ```css
  @media (max-width:749px){
    #verwandlung .ba__tabs--styles{flex-wrap:nowrap;overflow-x:auto;scroll-snap-type:x proximity;
      margin-inline:calc(var(--gutter)*-1);padding:0 var(--gutter) .3rem;scrollbar-width:none}
    #verwandlung .ba__tabs--styles::-webkit-scrollbar{display:none}
    #verwandlung .ba__tabs--styles .chip{flex:0 0 auto;scroll-snap-align:start}
    #verwandlung .ba__steps{display:none}          /* 3 Schritte stehen in #so-funktionierts */
    #verwandlung .ba__product{font-size:.75rem}
  }
  ```
  Optional in der Material-Reihe den Preis zeigen (`{% render 'price', product: mprod %}`, klein). Das wäre ein Preisanker direkt am Produkt.
- **Positiv [CODE]:** Der Schieber funktioniert auf Touch. Die Bühne hat `touch-action:pan-y`, `pointerdown` setzt den Split, Pointer-Capture und `pointercancel` sind abgefangen. Vertikales Scrollen über der Bühne bleibt möglich. Tab-Wechsel lädt 1200-px-Bilder ohne srcset nach (`swap()` entfernt `srcset`). Mobil ist das vertretbar (Originale 1120 × 1400).

#### P1-8 · Textdoppelungen und Widersprüche [CODE]
| Thema | Vorkommen (Startseite plus global) | Bewertung |
|---|---|---|
| „20 % vom Gewinn“ | Announcement, Hero-Trust t3, Hero-Pill, Marquee m2, How h3, Donation (Heading, Stat, Text), FAQ f4 = **8×** | zu oft; auf 3× reduzieren (Announcement, Donation, FAQ) |
| „Live-Vorschau“ | Hero-Lead, Hero-Trust t1, Hero-Sticker, Marquee m1, How h2, Styles-Text, FAQ f2 = **7×** | auf 3× reduzieren |
| Anzahl Stile | Styles-Eyebrow „**12 Stile plus Original**“, How h2 „**13 Stile**“, Locale `transformation.step2_text` „**13 Stile**“, Produktkarten zählen `Stil:`-Tags inkl. `Stil:Original` → „**13 Stile**“, Kleidungsprodukte haben nur 10 Stil-Tags → „**10 Stile**“, globales Setting `art_styles` = **11** (mit „Aquarell mit Herz“, ohne Anime/Herzensbild) | **widersprüchlich**. Einheitlich „12 Kunststile“ (plus „Original“ als Option) festlegen. `product-card.liquid` sollte `Stil:Original` beim Zählen ignorieren; `settings.art_styles` an die 12 Stile angleichen (Befund für den PDP-Audit) |
| EU vs. Europa | Hero-Trust „Druck & Versand aus der **EU**“, FAQ f3 „in der **EU**. Kein Zoll“ vs. Announcement, Marquee und How „**Europa**“ | vereinheitlichen. „Kein Zoll“ nur behalten, wenn Gelato garantiert innerhalb der EU produziert (Gelato-Routing kann UK/CH/NO nutzen) [VERMUTET] |
| Antwortzeit | Sonderwunsch „Antwort innerhalb von 24 Stunden“ vs. FAQ-Text (Default) „Wir antworten meist innerhalb weniger Stunden“ | angleichen, z. B. „werktags innerhalb von 24 Stunden“ |
| Lieferzeit | Announcement, How h3, FAQ f3: jeweils 5–9 Werktage | konsistent ✓ |
| Materialien | Hero nennt 4, Transformation, How und Locale „zehn Materialien + Aufsteller“ | inhaltlich ok, der Hero ist aber unvollständig (P1-4) |
| Markenname | „PrintMyPet“ (Style Herzensbild), „**PrintmyPet**“ (`youtube.settings.channel_name`), „**Print My Pet**“ (Donation-Default-Text, Buchtitel, Blog-Meta) | eine Schreibweise festlegen (P2) |

#### P1-9 · Bücher-Filter mobil leer [CODE, sicher]
- **Wo:** `assets/theme.css` Mobile-Diet (`@media (max-width:749px){.books>:nth-child(n+4){display:none}…}`), dazu der Filter in `assets/theme.js` („Filter chips that show/hide items by data-cat“: `it.hidden = …`).
- **Problem:** Mobil sind nur die ersten 3 Bücher sichtbar (alle 3 Hund: b1, b9, b2). Der Filter setzt nur das `hidden`-Attribut. Die CSS-Regel blendet Kind 4–9 trotzdem aus. **„Katze“ (b4 = Kind 5), „Pferd“ (b5 = Kind 6) und „Kinder“ (b6–b8 = Kind 7–9) zeigen mobil eine leere Fläche.** „Hund“ zeigt 3 statt 4. Außerdem bleiben bei 3 Büchern im 2-Spalten-Raster (`.books{grid-template-columns:1fr 1fr}`) ein Buch allein in der letzten Zeile stehen.
- **Fix (minimal):**
  ```js
  // theme.js im Filter-Handler, nach dem Setzen von hidden:
  (document.querySelector(g.dataset.filterTarget) || g.parentElement).classList.toggle('is-filtered', v !== 'all');
  ```
  ```css
  @media (max-width:749px){
    .books>:nth-child(n+4){display:none}
    .books.is-filtered>*:not([hidden]){display:grid}   /* Filter hebt die Diät auf */
    .books>:nth-child(n+5){display:none}                /* alternativ 4 statt 3 Bücher → kein Waisenkind */
  }
  ```
  **Empfehlung:** Die Bücher von der Startseite nehmen und in einen Content-Hub-Teaser mit Link auf /pages/buecher verschieben (siehe Reihenfolge).

#### P1-10 · Sonderwunsch: volles Formular mitten auf der Startseite, veraltete Optionen [CODE]
- **Wo:** `index.json` → `request` (`"settings":{}`, also Schema-Defaults aus `sections/custom-request.liquid`).
- **Probleme:**
  - Das komplette Kontaktformular (Name, E-Mail, Dropdown, Textarea, Checkbox, Button) kostet mobil ca. 1.350 px [SCHÄTZUNG] und liegt zwischen Stilen und Tierschutz.
  - Das Dropdown (Default `products`) enthält „Noch offen, Leinwand, Poster, T-Shirt, Hoodie, Tasse, Handyhülle, Tasche, **Webdecke (kommt bald), Kissen (kommt bald)**“. Kissen und Decken sind längst aktive Produkte [DATEN]. Acrylglas, Holzrahmen, Aufsteller und Kalender fehlen.
  - Man kann keine Fotos hochladen („Fotos kannst du direkt nach dem Absenden per E-Mail anhängen“). Das ist Reibung.
  - Es gibt längst ein kaufbares Produkt `wunschbild-von-deinem-tier` (teeinblue, mit Upload) und `wunschbild-als-acryl-aufsteller` [DATEN]. Die Startseite verlinkt keins davon.
- **Fix:** Die Formular-Sektion auf der Startseite durch einen kompakten Teaser ersetzen. Das geht als `image-with-text`-Sektion oder mit `pmp-wunschbild-beispiele` (vorhanden, 6 Beispielbilder aus `product.json`): „Deine Idee als Bild · Mehrere Tiere, Kostüm, Regenbogenbrücke“ plus CTA „Wunschbild gestalten“ → /products/wunschbild-von-deinem-tier und Textlink „Individuelle Anfrage“ → /pages/sonderwunsch. Falls das Formular bleibt: `products` in index.json explizit setzen, ohne „kommt bald“.

#### P1-11 · FAQ deckt kaufentscheidende Fragen nicht ab [CODE]
- **Wo:** `index.json` → `faq.blocks` f1–f4.
- **Fehlt:** Versandkosten (5,99 €, ab 150 € frei, laut `product.json`), Preise und Größen, **Widerruf bei personalisierten Produkten** (§ 312g Abs. 2 Nr. 1 BGB, kein Widerrufsrecht; Erwartungsmanagement), „Kann ich direkt an die beschenkte Person liefern?“, „Mehrere Tiere auf einem Bild?“ (steht im Schema-Preset, wird auf der Startseite aber nicht genutzt).
- **Fix:** 2–3 Fragen ergänzen (mobil werden ab der 6. Frage alle ausgeblendet, `.faq .accordion>:nth-child(n+6){display:none}`, also max. 5 Fragen). Beispiel:
  > **Was kostet der Versand?** Innerhalb Deutschlands 5,99 €, ab 150 € Bestellwert versandkostenfrei. Einige gerahmte Poster haben eine eigene Pauschale, die im Checkout angezeigt wird.

  Den Text 1:1 aus `product.json` acc2 übernehmen, damit beide Stellen gleich lauten.

#### P1-12 · Freebie (Welpen-PDF) zu früh und doppelt [CODE]
- **Wo:** `index.json` → `order` Position 4 (`freebie`, direkt nach der Verwandlung, vor Kacheln und Bestsellern), dazu `snippets/pmp-freebie-flyin.liquid` (wird auf `template.name == 'index'` **nicht** unterdrückt, erscheint mobil ab 900 px Scroll als Leiste unten) und die Buch-Hints b1/b9 („…kostenlose 50-Seiten-Startpaket“).
- **Problem:** Ein Lead-Magnet für Welpenbesitzer steht an Position 4 der Kaufstrecke. Katzen- und Pferdebesitzer sowie Geschenkkäufer verlieren den Faden. Das Fly-in überdeckt mobil unten den Inhalt, während die Bestseller oder die Verwandlung sichtbar sind.
- **Fix:** `freebie` in `order` nach unten verschieben (nach FAQ oder in den Content-Hub). Im Fly-in-Snippet die Startseite ausnehmen, solange das Banner dort steht:
  ```liquid
  if template.name == 'index'
    assign hide = true
  endif
  ```

#### P1-13 · Content-Block am Ende zu lang: YouTube, Blog, Bücher, Newsletter plus Footer-Newsletter [CODE]/[SCHÄTZUNG]
- **Problem:** Sektionen 11–13 kommen mobil zusammen auf ca. 3.750 px. Das sind drei Content-Angebote, die aus dem Shop hinausführen (YouTube, Amazon) oder nichts verkaufen. Der Futterrechner (`/pages/futterrechner`, vorhanden) wird gar nicht erwähnt. Laut Kommentar in `snippets/newsletter-form.liquid` gibt es zusätzlich ein Footer-Newsletter-Formular, also [VERMUTET] 2 Newsletter-Formulare direkt hintereinander.
- **Fix:** YouTube, Blog und Bücher durch **einen** kompakten „Mehr von PrintMyPet“-Hub ersetzen: 4–5 Karten als horizontaler Scroller mit Magazin (neuester Artikel), YouTube (bestes Video), Bücher, Futterrechner, Welpen-Startpaket. Die bestehenden Seiten /pages/youtube, /pages/buecher und /blogs/magazin tragen die Details. Den Newsletter nur einmal zeigen, entweder als Sektion oder im Footer.

#### P1-14 · Kein Social Proof, keine Bewertungen [DATEN]
- **Daten:** Bei allen 120 aktiven Produkten sind `reviews.rating` und `rating_count` leer. `product-card.liquid` zeigt Sterne nur, wenn die Metafelder existieren, deshalb erscheinen korrekt keine. 2026 gab es 10 Bestellungen. Die Sektion `testimonials.liquid` existiert, ist auf der Startseite nicht eingebunden (das ist richtig, solange es keine echten Stimmen gibt).
- **Problem:** Beim Punkt „Warum vertrauen?“ fehlt der stärkste Hebel. Die Bestseller-Überschrift „Was gerade *Herzen erobert*“ klingt nach Social Proof, ohne ihn zu belegen.
- **Fix:** **Keine erfundenen Reviews.** Stattdessen:
  1. Judge.me-Bewertungsanfragen nach der Zustellung aktivieren (App-Einstellung).
  2. Übergangsweise echte Belege nutzen: Die YouTube-Reichweite ist belegbar (beliebtestes Video 15.401 Aufrufe laut Metafeld `pmp.youtube_popular`).
  3. Die Bestseller-Überschrift neutraler formulieren: „Beliebte Portraits“.
  4. `testimonials` erst einbinden, wenn es echte Kundenstimmen mit Einwilligung gibt.

#### P1-15 · Mobil gibt es nach dem Hero keinen dauerhaften CTA [CODE]
- **Wo:** `theme.css` `@media (max-width:989px){.header__cta{display:none}}`. Der Header versteckt sich zusätzlich beim Runterscrollen (`theme.js` Header, `is-hidden` ab 260 px).
- **Problem:** In der ca. 20.000 px langen Seite ist mobil nur in einzelnen Sektionen ein CTA erreichbar.
- **Fix:** Eine schlanke Sticky-Leiste auf der Startseite (analog `.sticky-atc`, die es schon gibt) einblenden, sobald der Hero aus dem Viewport ist:
  ```liquid
  {%- comment -%} sections/hero.liquid, am Ende {%- endcomment -%}
  <div class="home-sticky" data-home-sticky hidden>
    <span><strong>Dein Tier als Kunstwerk</strong><small>ab 23,99 € · Live-Vorschau</small></span>
    <a class="btn btn--primary btn--sm" href="{{ s.cta_primary_url }}">{{ s.cta_primary }}</a>
  </div>
  <script>(function(){var b=document.querySelector('[data-home-sticky]'),h=document.querySelector('.hero');if(!b||!h||!('IntersectionObserver'in window))return;b.hidden=false;new IntersectionObserver(function(e){b.classList.toggle('is-visible',!e[0].isIntersecting)}).observe(h);})();</script>
  ```
  ```css
  .home-sticky{position:fixed;left:0;right:0;bottom:0;z-index:90;display:flex;align-items:center;justify-content:space-between;gap:.8rem;
    padding:.7rem var(--gutter) calc(.7rem + env(safe-area-inset-bottom));background:#fff;border-top:1px solid rgb(var(--c-line));
    transform:translateY(110%);transition:transform .4s var(--ease)}
  .home-sticky.is-visible{transform:none}
  .home-sticky small{display:block;font-size:.75rem;color:rgb(var(--c-ink-2))}
  @media (min-width:990px){.home-sticky{display:none}}
  ```
  Kollidiert mit dem Freebie-Fly-in (ebenfalls unten fixiert). Deshalb P1-12 umsetzen (Fly-in auf der Startseite aus).

#### P1-16 · Kein Geschenk- oder Saison-Anlass [DATEN]
- **Daten:** Die automatische Aktion „Weihnachtsaktion –15 %“ ist **SCHEDULED**. Es gibt den Kalender 2027, Weihnachtstasse, Baumschmuck und viele Produkte mit Tag `anlass:weihnachten`. Heute ist der 27.09.
- **Problem:** Die Startseite spricht nie von „Geschenk“ (außer einer Kachel), nennt keinen Bestellschluss für Weihnachten und zeigt den Gutschein nicht. Personalisierte Tierportraits sind aber vor allem ein Geschenkprodukt.
- **Fix:** Eine saisonale Sektion ab Oktober (collection-tiles oder image-with-text): „Das persönlichste Weihnachtsgeschenk · Bestellschluss für Lieferung bis Heiligabend: TT.MM.“ Das Datum aus Gelato-Laufzeiten (5–9 WT) plus Puffer ableiten, **das Datum muss das Team festlegen**. Kacheln: Wandbild, Tasse, Kalender 2027, Gutschein (Last-Minute). Die Hero-Eyebrow kann saisonal zu „Personalisierte Tierportraits · Das Geschenk für Tiermenschen“ werden.

#### P1-17 · Newsletter verspricht 10 % Rabatt: Code-Existenz prüfen [VERMUTET, live prüfen]
- **Wo:** `snippets/newsletter-form.liquid` (Badge `claim` Default „10 %“ / „Rabatt“), Locale `newsletter.perk_1` „10 % auf deine erste Bestellung“, `index.json` → `newsletter.settings.heading` „Willkommensrabatt & *Foto-Tipps*“.
- **Daten:** Aktive Rabattcodes sind nur `LKW10` und `DANKE10 Paketbeileger (PMP-228)`. Ein eindeutiger Willkommenscode ist im Shop nicht erkennbar. Die Brevo-Automation konnte ich nicht einsehen.
- **Fix:** Prüfen, ob Brevo nach dem Double-Opt-in einen gültigen Code verschickt. Sonst ist das eine irreführende Werbeaussage. Text-Detail: „die besten **Hunde**foto-Tricks“ schließt Katzen- und Pferdebesitzer aus, besser „Tierfoto-Tricks“.

### P2: nice to have / Feinschliff

1. **Hero-Sekundär-CTA mit Play-Icon, aber ohne Video** [CODE]: `hero.liquid` rendert `icon: 'play'` bei „So funktioniert's“ → #so-funktionierts. Das Icon suggeriert ein Video. Fix: `icon: 'chevron-down'` (das Icon wird in `main-collection.liquid` schon genutzt). Oder ein echtes 20-Sekunden-Video.
2. **Hero-Titel-Animation und LCP** [VERMUTET]: `.hero__title .line span{transform:translateY(110%);animation:rise 1s…}` (`theme.css` Z. 264). Mobil ist die H1 vermutlich das LCP-Element, weil die Bilder unter dem Fold liegen. Die Einblende-Animation kann LCP um bis zu ca. 1 s verzögern. Gleichzeitig laden 3 Hero-Bilder `eager` (Bild 1 mit `fetchpriority:high`), obwohl sie mobil unter dem Fold liegen. Live mit Lighthouse (Mobile) prüfen. Mögliche Lösung: `@media (max-width:749px){.hero__title .line span{animation:none;transform:none}}`, und Bild 2/3 mobil `lazy`.
3. **Marquee** [CODE]: wiederholt die Hero-Trust-Punkte direkt darunter. Besser entfernen oder mit neuer Information füllen („Poster ab 23,99 €“, „12 Kunststile“, „Tassen · Shirts · Decken“, „Gutschein ab 25 €“).
4. **Grain-Overlay** [VERMUTET]: `.has-grain::after{position:fixed;inset:0;mix-blend-mode:multiply;z-index:9998}` über der ganzen Seite. Mobil kann das die Scroll-Performance kosten (Composite-Layer mit Blend). Testweise mobil ausschalten: `@media (max-width:749px){.has-grain::after{display:none}}`.
5. **Blog-Raster mobil mit 3 Karten in 2 Spalten** [CODE]: `featured-blog` `limit:3`, `grid--3` wird mobil 2-spaltig, die 3. Karte steht allein. Außerdem `article-card.liquid` mit `sizes '… 100vw'` bei 50vw Breite. Fix: mobil horizontaler Scroller oder `limit: 2`. `sizes` auf `(min-width:990px) 33vw, 50vw` setzen.
6. **Bestseller-Karten** [DATEN/CODE]: Titel sind sehr lang („Personalisiertes Katzenportrait als Poster im Holzrahmen“) und brechen in ca. 170 px Spalte auf 3–4 Zeilen um. Die Eyebrow zeigt „13 Stile“ (inkl. Original, siehe P1-8). Keine Karte trägt den Tag `Bestseller`, also gibt es kein Badge. Fix: kurze Kartentitel über ein Metafeld (z. B. `custom.kurztitel`) oder CSS `-webkit-line-clamp:2`.
7. **Leer-Fallback mit Fake-Preisen** [CODE]: `featured-collection.liquid` zeigt bei leerer Collection „Beispielprodukt 1–4“ mit „ab 39,90 €“. Wenn das live auftaucht, ist das eine falsche Preisangabe. Fix: Im `else`-Zweig nichts rendern und die Sektion ausblenden (`{%- if c == blank or c.products.size == 0 -%}{%- return -%}`… bzw. den Wrapper nicht ausgeben). Dasselbe gilt für `featured-blog.liquid` („Beispielartikel“).
8. **Donation-Bild mobil 4:5 in voller Breite** [CODE]: `.donation__visual{aspect-ratio:4/5}` ergibt ca. 450 px Bild. Mobil reicht 4:3: `@media (max-width:749px){.donation__visual{aspect-ratio:4/3}}`. Der Zähler „20 %“ startet bei „0 %“ und zählt erst bei 50 % Sichtbarkeit hoch (ok). Button „Zum Voting-Kanal“ verlinkt auf YouTube, obwohl der Voting-Ablauf ausgeschaltet ist (`show_vote:false`). Das Wort „Voting“ ist ohne Kontext unklar, besser „Tierschutz auf YouTube“.
9. **YouTube-Hub: „Neu auf dem Kanal“ ist veraltet** [DATEN]: Metafeld `pmp.youtube_latest` wurde zuletzt am 05.09.2026 aktualisiert, das neueste Video ist vom 02.09. Laut Schema-Text schreibt die Routine „täglich“. Die YouTube-Sync-Routine prüfen. Text-Doppelung: Default-Text „…jede Woche neu“ vs. `channel_text` „Dienstags… samstags…“.
10. **Rechtschreibung und Stil** [CODE]:
    - `books.settings.heading`: „Lesestoff für *Tiermenschen*, und ihre Kinder“ → „Lesestoff für *Tiermenschen* und ihre Kinder“ (Komma vor „und“ falsch).
    - `youtube.settings.channel_text`: „…Gesundheit, und die Nachteile…“ → „…Gesundheit und die Nachteile…“.
    - `hero.settings.text`: „drucken es auf Leinwand, Poster, **in den Holzrahmen** oder auf Acrylglas“ ist holprig → „als Leinwand, Poster, im Holzrahmen oder auf Acrylglas“.
    - `youtube.settings.channel_name`: „PrintmyPet“ → einheitliche Marke (P1-8).
    - Stilname: „Klassisches Ölgemälde“ (Startseite) vs. „Ölgemälde“ (Tag) vs. „Klassische Ölgemälde“ (Collection-Titel). Vereinheitlichen.
11. **Tile „Geschenke“** [DATEN]: Die Collection `personalisierte-geschenke` (Regel Tag `personalisiert`) meldet 238 Produkte, davon viele archiviert. Live prüfen, ob die ersten Produkte dort attraktiv sind, denn sortiert wird nach BEST_SELLING.
12. **Stil-Kacheln verlinken auf `stil-*` mit manueller Sortierung** [DATEN]: z. B. `stil-aquarell` hat 134 Produkte, viele davon archiviert. Live prüfen, ob die erste Reihe aktive Wandbilder zeigt (gehört zum Collection-Audit).
13. **Kachel-Bilder als PNG 3291 × 4096** [DATEN]: `tile-*.png` und `donation-hero.png` sind große PNGs. Shopify liefert über `image_url` in der Regel WebP bzw. AVIF aus, trotzdem wären JPG-Originale in Web-Auflösung schlanker. Live in DevTools prüfen, welches Format ankommt.
14. **Announcement mobil** [CODE]: 2 Items in einer horizontal scrollbaren Leiste ohne sichtbaren Hinweis (`overflow-x:auto`, Scrollbar versteckt). Das 2. Item (Lieferzeit) sieht man mobil vermutlich nur angeschnitten [VERMUTET]. Besser rotieren lassen oder nur ein Item.
15. **Leere Zustände** [CODE/DATEN]: Aktuell ist nirgends ein Platzhalter sichtbar. Alle Bilder sind gesetzt, Blog und Bestseller haben Inhalt. Das Risiko bleibt bei künftigem Archivieren (siehe 7).
16. **FAQ-JSON-LD** [CODE]: `faq.liquid` gibt ein `FAQPage`-Schema aus. Ok. Falls `/pages/faq` dieselben Fragen ebenfalls als FAQPage ausgibt, ist das doppelt (Befund für den SEO-Audit).
17. **„Live-Vorschau“-Versprechen** [DATEN/CODE, live prüfen]: `assets/live-preview.js` ist laut Kommentar „Dummy-Logik“, die nur Platzierung zeigt (Locale `products.preview.note`: „Die echte Stil-Verwandlung machen wir nach der Bestellung“). **Alle** Portrait-, Tassen- und Kleidungsprodukte haben aber eine teeinblue-Kampagne (`teeinblue.campaign_version` gesetzt) und damit laut Theme-Kommentar die echte Live-Vorschau. Die Aussage der Startseite ist damit für die Hauptprodukte **wahrscheinlich korrekt**. Live an 1–2 Produkten prüfen, ob teeinblue wirklich das stilisierte Bild zeigt. Nicht zutreffend ist sie für: Kalender 2027, Digitalportraits (`personalisiertes-*-digital`), Deko- und Spruch-Artikel ohne Foto.

---

## 4. Seitenlänge mobil (Schätzung bei 390 px)

| Sektion | ca. Höhe |
|---|---|
| Announcement + Header | 115 px |
| 1 Hero | 1.150 px |
| 2 Marquee | 55 px |
| 3 Verwandlung | 2.200 px |
| 4 Freebie | 1.000 px |
| 5 Kacheln | 700 px |
| 6 Bestseller | 850 px |
| 7 So funktioniert's | 1.150 px |
| 8 Stile (12 Karten) | 2.250 px |
| 9 Sonderwunsch-Formular | 1.350 px |
| 10 Tierschutz | 1.100 px |
| 11 YouTube | 1.450 px |
| 12 Blog | 950 px |
| 13 Bücher | 1.350 px |
| 14 FAQ | 750 px |
| 15 Newsletter | 750 px |
| **Summe (ohne Footer)** | **ca. 17.000–19.000 px (ca. 22–25 Bildschirme)** |

Grundlage: `--section-pad` = clamp(62px, 7vw, 112px), mobil ×0,8 ≈ 50 px oben und unten; `--gutter` = 16 px. Alle Werte [SCHÄTZUNG].

---

## 5. Vorschlag: optimierte mobile Reihenfolge

Ziel: Die Seite beantwortet in den ersten 2 Screens *Was, Für wen, Ab welchem Preis, Wie bestellen* und endet nach ca. 10–12 Screens.

| Neu # | Sektion (Key) | Änderung |
|---|---|---|
| 1 | **Hero** (`hero`) | Mobil Bild zuerst (4:3), kurzer Lead mit „ab 23,99 €“, 1 Primär-CTA → **funktionierende** Collection (P0-1). Trust: Preis ab · Europa-Druck, 5–9 WT · 20 % Tierschutz · (Garantie, falls bestätigt). YouTube-Karte aus. |
| 2 | **Vorher/Nachher** (`transformation`) | Stil-Chips als Scroller, ohne die 3 Schritte, Material-Reihe mit Preisen. |
| 3 | **Für wen & was** (`tiles`, erweitert) | Hund, Katze, Pferd plus Produktwelten (Wandbild, Tasse, Kleidung, Decke/Kissen, Gutschein, Wunschbild) mit „ab“-Preisen. Mobil als Scroller. |
| 4 | **Bestseller / Beliebt** (`bestsellers`) | Neutralere Überschrift, kurze Titel. |
| 5 | **So funktioniert's** (`how`) | Bleibt (einzige 3-Schritte-Stelle). Text „12 Kunststile“. |
| 6 | **(Saison) Geschenk/Weihnachten** | Neu, ab Oktober: Bestellschluss, Gutschein, Kalender. |
| 7 | **Wunschbild-Teaser** (statt `request`-Formular) | Beispielbilder, CTA zum Wunschbild-Produkt, Link auf /pages/sonderwunsch. |
| 8 | **Tierschutz** (`donation`) | Kompakter (Bild 4:3), 1 CTA. |
| 9 | **FAQ** (`faq`) | Plus Versandkosten, Widerruf bei Personalisierung, Geschenkversand. |
| 10 | **Mehr von PrintMyPet** (neu, ersetzt `youtube`, `blog`, `books`, `freebie`) | 5 Karten im Scroller: Magazin, YouTube, Bücher, Futterrechner, Welpen-Startpaket. |
| 11 | **Newsletter** (`newsletter`) | Nur, wenn im Footer kein zweites Formular steht. Rabattcode prüfen. |
| – | Entfallen/verschoben | `marquee` (oder neu befüllen), `styles` (mobil ausblenden oder als Scroller unter Position 2), Freebie-Fly-in auf der Startseite. |

Umsetzung in `templates/index.json` → `"order"` (Beispiel ohne neue Sektionen):
```json
"order": ["hero","transformation","tiles","bestsellers","how","donation","faq","blog","newsletter"]
```
Die Sektionen `marquee`, `freebie`, `styles`, `request`, `youtube` und `books` bleiben mit `"disabled": true` im JSON erhalten, damit man sie schnell wieder einschalten kann. Neue Sektionen (Produktwelten, Saison, Wunschbild-Teaser, Content-Hub) müssen vorher angelegt werden.

---

## 6. Reihenfolge der Arbeiten für die Umsetzungs-Session

1. **P0-1** Schaufenster neu befüllen oder CTA-Ziele umstellen (Hero, Transformation, How, Header, Cart-Cross-Sell).
2. **P1-2** `yt_badge: "none"`. **P1-3** Preis- und Garantie-Trust im Hero. **P1-1** Hero-Mobile-CSS plus kürzerer Lead.
3. **P1-8** Texte vereinheitlichen (Stile, EU/Europa, Antwortzeit, Marke). **P2-10** Rechtschreibung.
4. **P1-9** Bücher-Filter-Fix (oder Bücher von der Startseite nehmen). **P1-6** `sizes` und Hover-Text. **P1-7** Chip-Scroller.
5. **P1-12** Freebie nach unten, Fly-in auf der Startseite aus. **P1-15** Sticky-CTA.
6. **P1-4 / P1-10 / P1-13 / P1-16** Sektionen umbauen (Produktwelten, Wunschbild-Teaser, Content-Hub, Saison). Neue Reihenfolge nach Abschnitt 5.
7. **P1-11** FAQ ergänzen. **P1-17** Rabattcode prüfen. **P1-14** Bewertungsprozess starten.
8. Danach live mit 360/390/430 px prüfen: Above-the-fold, Overlaps, Lighthouse-LCP, Fly-in-Kollision.

---

### Anhang: gelesene Dateien und Abfragen
- Theme: `templates/index.json`, `sections/{hero,marquee,pmp-freebie-banner,transformation-showcase,collection-tiles,featured-collection,how-it-works,style-showcase,custom-request,donation-story,faq,featured-blog,newsletter-brevo,youtube-hub,amazon-books,header-group.json,main-collection,main-product (Auszug)}.liquid`, `snippets/{product-card,image,placeholder,price,geld,section-heading,button,hl-text,article-card,newsletter-form,css-variables,pmp-freebie-flyin}.liquid`, `assets/{theme.css,pmp-222.css,theme.js,live-preview.js (Kopf)}`, `layout/theme.liquid`, `locales/de.default.json`, `config/settings_data.json`, `templates/{product,collection}.json`.
- Admin-GraphQL: Collections (Handles, productsCount, Regeln, Produkte samt Status), 120 aktive Produkte (Preise, Tags, Review- und teeinblue-Metafelder), Dateien (alle referenzierten Bilder), Blog `magazin` (57 Artikel, 3 neueste), Seiten, Shop-Metafelder `pmp.youtube_*`, Rabatte, Bestellanzahl 2026.
