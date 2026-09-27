# Audit 03: Produktseiten & Kauflogik (Warenkorb)

Shop: printmypet.de · Live-Theme `gid://shopify/OnlineStoreTheme/208344613202` · Stand der Daten: 27.09.2026
Quelle: Shopify Admin GraphQL (nur gelesen, nichts geändert). Die Website selbst war aus der Umgebung **nicht erreichbar**. Alles, was nur im Browser sichtbar wird (vor allem das Verhalten der teeinblue-App), ist als **[LIVE PRÜFEN]** gekennzeichnet.

Kennzeichnung:
- **[BELEGT]**: direkt aus Theme-Code oder Admin-Daten ablesbar
- **[VERMUTET / LIVE PRÜFEN]**: folgt aus dem Code, hängt aber vom Laufzeitverhalten einer App oder des Browsers ab

Rohdaten zum Nachprüfen liegen in `scratchpad/data/` (`all.json` = alle 132 aktiven und Entwurfs-Produkte mit Varianten und Metafeldern, `rows.json` = Kurzfassung, `theme.css`, `pmp-222.css`).

---

## TEIL A: IST-ZUSTAND

### A1. Katalog in Zahlen [BELEGT]

| Status | Anzahl |
|---|---|
| Produkte gesamt | 322 |
| ACTIVE | 120 (davon **117** im Onlineshop veröffentlicht, 3 aktiv, aber in keinem Kanal) |
| DRAFT | 12 |
| ARCHIVED | 190 |

- **97 aktive Produkte haben eine teeinblue-Kampagne** (Metafeld `teeinblue.campaign_version` oder `teeinblue.platform_product`), 68 davon mit Foto-Personalisierung.
- **Template:** 117 aktive Produkte nutzen `product.json` (Standard). `product.teeinblue` nutzen nur die 3 unveröffentlichten Digitalportraits. `product.digital-portrait.json` und `product.personalized-fixed-style.json` nutzt **kein aktives Produkt** (das zweite nur ein archiviertes). Beide sind verwaist.
- **Bewertungen:** Kein einziges Produkt hat eine Judge.me-Bewertung (`judgeme.review_widget_data.number_of_reviews = 0` bei allen 131).
- **Alt-Texte:** Kein aktives Produkt hat Medien ohne Alt-Text. Gut.
- **Beschreibungen:** Alle vorhanden. Die kürzesten haben 167 bis 260 Zeichen (Halstücher, Futternäpfe, Kissen, Decken, alle Entwürfe mit ca. 180 bis 220 Zeichen).
- **SEO-Beschreibung fehlt:** `kaninchen-socken-pfotenpause`.
- **Streichpreise:** keine (`compareAtPrice` überall leer). Kein Risiko durch falsche UVP.
- **Metafeld `pod.print_area`:** fehlt bei **allen** Produkten. Die eigene Live-Vorschau des Themes (`live_preview`-Block, `assets/live-preview.js`) erscheint deshalb **auf keiner Produktseite**.
- **Metafeld `custom.donation_verified`:** fehlt bei allen Produkten. Der Tierschutz-Hinweis (`donation`-Block) erscheint deshalb **auf keiner Produktseite**, obwohl Warenkorb, USP-Reihe und FAQ die „20 %“ überall nennen.

### A2. Aktive Produkte nach Gruppe [BELEGT]

TIB = teeinblue-Kampagne vorhanden. Perso = so, wie `main-product.liquid` das Produkt einordnet (Tag, sonst Metafeld, sonst Titel).

| Gruppe (Handle-Muster) | Anz. | Typ | Perso | TIB | Preis € | Varianten / Optionen | Bilder | Bemerkung |
|---|---|---|---|---|---|---|---|---|
| `personalisiertes-{hunde,katzen,pferde}portrait-auf-leinwand` | 3 | Wandbild | foto | ja | 39,99–92,99 | 10 · Format(2) Ausführung(1) Größe(5) | 12–13 | Kurztext verspricht „13 Stile“ |
| `…-auf-leinwand-mit-rahmen` | 3 | Wandbild | foto | ja | 81,99–208,99 | 15 · Format, Rahmenfarbe(3), Größe(5) | 12–15 | |
| `…-als-poster` | 3 | Wandbild | foto | ja | 23,99–43,99 | 10 | 9–10 | |
| `…-als-poster-im-holzrahmen` | 3 | Wandbild | foto | ja | 49,99–143,99 | 40 | 15–19 | |
| `…-als-poster-im-metallrahmen` | 3 | Wandbild | foto | ja | 50,99–132,99 | 5 | 8–9 | |
| `…-als-poster-mit-aufhangung` | 3 | Wandbild | foto | ja | 42,99–70,99 | 20 | 14–18 | |
| `…-auf-acrylglas` | 3 | Wandbild | foto | ja | 68,99–186,99 | 5 | 8–9 | |
| `…-auf-aluminium` | 3 | Wandbild | foto | ja | 52,99–138,99 | 5 | 8–9 | |
| `…-auf-hartschaumplatte` | 3 | Wandbild | foto | ja | 44,99–78,99 | 5 | 8 | |
| `…-auf-holz` | 3 | Wandbild | foto | ja | 66,99–155,99 | 5 | 8–9 | |
| `…-als-acryl-aufsteller` | 3 | Wandbild | foto | ja | 46,99–55,99 | 2 | 6 | |
| `wunschbild-von-deinem-tier` | 1 | Wandbild | foto (typ:wunschbild) | ja | 62,99–247,99 | 120 · Material(13) Rahmenfarbe(5) Größe(7) | 37 | |
| `wunschbild-als-acryl-aufsteller` | 1 | Wandbild | foto | ja | 85,99–94,99 | 2 | 4 | |
| Shirt-Designs (`dog-mom-…`, `cat-mom-…`, `who-let-the-dog-out-…` usw.) | 13 | Shirt | name/foto | ja | 28,99–81,99 | **256** · Kleidungsstück(3) Farbe(16) Größe(9) | 6 | **mehr als 250 Varianten, siehe P1-07** |
| Foto-Textil (`personalisiertes-t-shirt-…`, `-sweatshirt-`, `-hoodie-`, `-sweatjacke-`, Kinder, Baby) | 8 | div. | foto | ja | 24,99–81,99 | 6–91 | 1–7 | 5 davon mit nur **1 Bild** |
| Foto-Tassen `personalisierte-{hunde,katzen,pferde}tasse-mit-foto`, `the-dogfather-…` | 4 | Tasse | foto | ja | 19,99–39,99 | 3–7 | 12–20 | |
| Spruch- und Namens-Tassen | 18 | Tasse | none/name | ja | 19,99–39,99 | 3–7 | 12–20 | |
| Foto-Deko/Accessoires (Kissen, Kissenbezug, Decke, Sherpa, Mauspad, Notizbuch, Sticker, Flagge, Badetuch, Strandtasche, Stofftaschen, Handyhüllen, Christbaumschmuck) | 16 | div. | foto | ja | 12,99–152,99 | 1–58 | **1** bei 11 Produkten | |
| `haustier-wandkalender-2027` | 1 | Kalender | foto | **nein** | 49,99 | 1 | 12 | einziges aktives Foto-Produkt **ohne** teeinblue, nutzt Upload und Stilwahl des Themes |
| `dein-hund-als-digitales-kunstwerk-sofort-download` (ID 11109360795986) | 1 | Digitaler Download | foto (digital) | nein | 19,90 / 39,90 (Spezialwunsch) | 8 · Stil(8) | 8 | Theme-Upload; Liquid-Sonderfall per fest eingetragener Produkt-ID |
| `personalisiertes-{hunde,katzen,pferde}portrait-digital` | 3 | Digitaler Download | foto → none (TIB) | ja | 19,90 | 1 | 6 | **ACTIVE, aber 0 Veröffentlichungen**, Template `teeinblue` |
| Fertige Designs ohne Personalisierung (Kissen ×4, Decken ×4, Halstücher ×4, Futternäpfe ×3, Mauspad, Journal, Socken, Baumwolltasche, Herren-Sweatshirt) | 20 | div. | none | nein | 24,90–69,90 | 1–8 | 4–9 | Preisendung ,90 statt ,99 |
| `geschenkgutschein` | 1 | Geschenkgutschein | – | – | 25–200 | 5 | 5 | |

Entwürfe (12): `badehose-`, `badeschlappen-`, `futternapf-mit-foto`, `puzzle-`, `tuerschild-`, `foto-socken-mit-deinem-tier` (Preise **47,94 / 41,94 / 35,94 / 29,94** sehen nach ungerundetem Aufschlag aus), 3 Mauspads mit Foto (27,99, ohne TIB), 3 Merch-Artikel.

**Doppelte oder überlappende Produkte:** `personalisierte-stofftasche-mit-deinem-tier` (38,99, 1 Bild) und `personalisierte-stofftasche-mit-deinem-tier-natur` (30,99–31,99, 7 Bilder). Außerdem gibt es parallel das alte Digitalprodukt (`dein-hund-als-digitales-kunstwerk…`) und drei neue teeinblue-Digitalportraits (unveröffentlicht). Produkttypen sind uneinheitlich: `Shirt`/`T-Shirt`, `Halstuch`/`Hundehalstuch`, `Kinder-T-Shirt`/`Kinderbekleidung`.

**Preislogik:** Innerhalb der Wandbild-Familien sind die Preise stimmig gestaffelt. Auffällig ist nur, dass das Wunschbild-Poster 20×25 mit 62,99 € mehr als doppelt so viel kostet wie das normale Portrait-Poster (23,99 €). Das ist so gewollt (Wunschbild als Dienstleistung), wird aber auf der Seite nicht begründet [BELEGT: Preise; Begründung fehlt, LIVE PRÜFEN]. Unlogische Preise gibt es sonst keine. Nur die Preisendungen sind uneinheitlich: ,99 bei POD, ,90 bei fertigen Designs, ,94 in Entwürfen.

### A3. Versand: was tatsächlich gilt [BELEGT]

Lieferprofil „Allgemeines Profil“ (Standard, enthält alle Varianten):
- **DE:** Standard 5,99 € · Standard 0,00 € ab Warenwert ≥ 150 € · Express 9,99 €
- **EU (26 Länder):** 13,99 €, kein Gratisversand
- **International (CH, GB, US, AU, JP …):** 19,99 €
- Die Gelato-Profile („Small/Large Framed Posters“ usw.) enthalten **0 Varianten** und sind damit wirkungslos.

Die **Schwelle 150 € gilt live.** Die Zahl „79 €“ kommt in keinem geprüften Theme-Text vor. Die Theme-Einstellung `cart_free_shipping_threshold = 0` blendet die Gratisversand-Leiste im Warenkorb aus, obwohl es in DE ab 150 € tatsächlich Gratisversand gibt.

Widersprüche dazu: siehe P1-10 und P1-11.

### A4. So läuft der Kauf mobil heute (aus dem Code rekonstruiert)

Es gibt **drei verschiedene Kaufstrecken**, je nach Produkt.

**Strecke 1: teeinblue-Produkte (97 Stück, darunter alle Wandbilder, Tassen, Shirts).** Template `product.json`, App-Embed „teeinblue product-personalizer“ global aktiv (`settings_data.json` → blocks).
1. Galerie: teeinblue setzt eine eigene Galerie (`.tee-gallery`) in `.product__gallery`. Laut `pmp-222.css` (PMP-227) wurde für Handys eine Leerfläche unter der Galerie behoben. [LIVE PRÜFEN: Wischen und Zoom kommen von teeinblue]
2. Rechte Spalte: teeinblue ersetzt bzw. umhüllt das Theme-Formular (`form.tee-form-wrapper`). Laut `theme.css` („PMP-176: … Kaufknopf und Variantenauswahl des Themes bleiben verborgen“) blendet teeinblue alle Geschwister-Elemente nach `.tee-customization-wrapper` aus. Das Theme holt danach nur **Titel, Kurztext, Akkordeons** (theme.css, Ende) sowie **Trust-Liste und Judge.me-Block** (pmp-222.css) per `display:block/grid !important` zurück.
3. Reihenfolge laut CSS: Titel → (Bewertungs-Badge) → teeinblue-Gestalter (Preis, Optionen, Foto, Stil, Knöpfe) → Trust-Punkte → Kurztext → Akkordeons.
4. Der Warenkorb-Knopf kommt von teeinblue (`.tee-btn--atc`, rot eingefärbt in pmp-222.css). Wie danach der Warenkorb reagiert (Drawer oder Weiterleitung auf /cart), ist Sache von teeinblue. [LIVE PRÜFEN]

**Strecke 2: Theme-eigene Foto-Strecke.** Aktiv nur noch bei `haustier-wandkalender-2027` (physisch) und `dein-hund-als-digitales-kunstwerk-sofort-download` (digital).
Reihenfolge der Blöcke in `product.json`: Titel → Judge.me-Badge → Preis + „inkl. MwSt., zzgl. Versand“ → Kurztext → **Foto-Upload** (`snippets/upload-field`, optional, weil `upload_required = false`) + Namensfeld → **Stilwahl** (11 Chips aus `settings.art_styles`, „Aquarell“ vorausgewählt; beim Digitalprodukt entfällt sie, weil es eine Option „Stil“ gibt) → (Live-Vorschau, nie sichtbar) → Varianten → Spezialwunsch-Feld (nur Digitalprodukt, Variante „Spezialwunsch“) → Menge + „In den Warenkorb“ + Express-Checkout (`show_dynamic: true`) → Lieferzeitzeile → Trust (4 Kacheln) → (Spende, nie sichtbar) → Größenhilfe (nur `typ:portrait`) → Beschreibung → Akkordeons → Judge.me-Widget → Vorher/Nachher-Beispiele → Teilen. Danach folgen die Sektionen „Wunschbild-Beispiele“ (nur `typ:wunschbild`), „Ähnliche Produkte“ und die USP-Reihe.
Absenden: `assets/cart.js` fängt `submit` ab, prüft Pflicht-Upload (nur wenn `required`), 20-MB-Grenze und Spezialwunsch-Mindestlänge, schickt `FormData` per AJAX an `/cart/add` und öffnet den Drawer.

**Strecke 3: fertige Designs und Gutschein.** Wie Strecke 2, aber ohne Upload und Stilwahl.

**Sticky-Kaufleiste mobil** (`.sticky-atc`, nur < 990 px): erscheint erst, **nachdem** der Theme-Kaufknopf nach oben aus dem Bild gescrollt ist (`theme.js`, IntersectionObserver mit Bedingung `boundingClientRect.top < 0`).

---

## TEIL B: BEFUNDE

Prioritäten: **P0** = kaputt, Umsatzverlust oder Rechtsrisiko · **P1** = wichtig · **P2** = nice-to-have

### P0: kaputt, Umsatz oder Recht

#### P0-01 · Unsichtbare Theme-Felder werden bei teeinblue-Produkten mitgeschickt: falscher Stil, leere POD-Felder in der Bestellung
**Wo:** `sections/main-product.liquid`, Blöcke `upload`, `style_picker`, `buy_buttons` (→ `render 'pod-routing'`); `snippets/style-picker.liquid`; `snippets/pod-routing.liquid`
**Belegt:** Bei allen 68 teeinblue-Foto-Produkten ist `perso = 'foto'`. Tag `perso:foto` ist gesetzt, und `is_tib_digital` greift nur bei digitalen Produkten. Deshalb rendert das Theme **innerhalb desselben `<form>`**:
- ein zweites Upload-Feld `properties[Foto]` (leer),
- die Stilwahl `properties[Stil]` mit **„Aquarell“ als `checked`**. Die Liste hat 11 Stile, teeinblue dagegen 13 (Tags zeigen z. B. „Anime“, „Herzensbild“, „Original“),
- das Namensfeld `properties[Name]`,
- versteckte Felder `_pod_provider=gelato`, `_pod_variant_uid=""`, `_pod_workflow=ai-transform-then-print`.

Die Felder sind laut CSS nur **ausgeblendet** (display:none), nicht deaktiviert. Ausgeblendete Formularfelder zählen aber weiter zum Formular.
**Vermutet [LIVE PRÜFEN]:** Wenn teeinblue beim Warenkorb-Knopf das Formular serialisiert (übliches Vorgehen), landet in **jeder** Bestellposition „Stil: Aquarell“, egal welchen Stil die Kundin im Gestalter gewählt hat. Das ist ein direktes Risiko für Fehlproduktionen. Dazu kommt `_pod_workflow`, womit der eigene `backend/pod-router` die Bestellung zusätzlich anfassen könnte (Doppel-Fulfillment neben Gelato und teeinblue).
**Test:** Ein teeinblue-Produkt mit Stil „Cartoon“ in den Warenkorb legen, dann `/cart.js` aufrufen und die `properties` prüfen.
**Fix (sicher, auch wenn teeinblue nichts mitschickt):** Für teeinblue-Produkte Upload, Stilwahl, Namensfeld, Live-Vorschau und POD-Routing gar nicht erst rendern.
```liquid
{%- comment -%} oben in main-product.liquid, nach der is_tib_digital-Logik {%- endcomment -%}
assign has_tib = false
if product.metafields.teeinblue.campaign_version != blank or product.metafields.teeinblue.platform_product != blank or product.template_suffix == 'teeinblue'
  assign has_tib = true
endif
{%- comment -%} dann in den Blöcken: {%- endcomment -%}
{%- when 'upload' -%}{%- if perso == 'foto' and has_tib == false -%} … {%- endif -%}
{%- when 'style_picker' -%}{%- if perso == 'foto' and has_tib == false and has_style_option == false and fixed_art_style == blank -%} … 
{%- when 'buy_buttons' -%}{%- unless has_tib -%}{%- render 'pod-routing', … -%}{%- endunless -%}
```
Auch der hidden input `data-fixed-art-style` oben im Formular sollte `has_tib == false` voraussetzen.

#### P0-02 · Pflicht-Preisangabe „inkl. MwSt., zzgl. Versand“ fehlt sehr wahrscheinlich bei 97 teeinblue-Produkten
**Wo:** `main-product.liquid`, Block `price` (`<div {{ block.shopify_attributes }}>` ohne eigene Klasse); `assets/theme.css` (Ende, PMP-176); `assets/pmp-222.css` (PMP-227-Reihenfolge)
**Belegt:** Die CSS-Regeln, mit denen das Theme Elemente neben dem teeinblue-Gestalter wieder einblendet, erfassen nur `div:has(> h1.product__title)`, `.product__short.rte`, `details.accordion__item`, `ul.trust` und `.product__app-block`. Der Preisblock mit dem Hinweis `products.product.tax_shipping_html` („inkl. MwSt., zzgl. Versand“, Link auf `/policies/shipping-policy`) ist nicht dabei. Die Lieferzeitzeile im `buy_buttons`-Block ebenfalls nicht.
**Vermutet [LIVE PRÜFEN]:** teeinblue zeigt nur seinen eigenen Preis (`.tee-price--current`). Der Hinweis „inkl. MwSt. / zzgl. Versandkosten“ fehlt dann in der Nähe des Preises. Das ist eine Pflichtangabe nach § 6 PAngV (Deutschland) und Abmahnrisiko.
**Fix:** Den Hinweis mit eigener Klasse ausgeben und für teeinblue einblenden, direkt unter den Gestalter:
```liquid
{%- when 'price' -%}
<div class="product__price-block" {{ block.shopify_attributes }}> … <p class="muted product__tax-note">…</p></div>
```
```css
body.teeinblue-enabled:not(.teeinblue-platform-product-enabled) .product__info>form.tee-form-wrapper>.product__price-block{display:block!important;order:0}
body.teeinblue-enabled:not(.teeinblue-platform-product-enabled) .product__info>form.tee-form-wrapper>.product__price-block .price{display:none}/* Preis zeigt teeinblue */
```
Besser noch: den Hinweis per `insertAdjacentElement` direkt unter `.tee-price--current` setzen (so wie `widerruf-digital` sich über `.tee-form-actions` setzt).

#### P0-03 · Das veröffentlichte Digitalprodukt hat keine Zustimmung zum Erlöschen des Widerrufsrechts
**Wo:** Produkt `dein-hund-als-digitales-kunstwerk-sofort-download` (ID 11109360795986); `main-product.liquid`, Block `trust`: `{%- if is_tib_digital -%}{%- render 'widerruf-digital' -%}{%- endif -%}`
**Belegt:** Die Checkbox (`snippets/widerruf-digital.liquid`) erscheint nur bei `is_tib_digital`, also bei den drei **unveröffentlichten** teeinblue-Digitalportraits. Das einzige tatsächlich verkaufte Digitalprodukt hat keine Checkbox. Die Rückgabe-Richtlinie sagt aber: Das Widerrufsrecht bei digitalen Inhalten erlischt nur, wenn „du dem ausdrücklich zugestimmt und deine Kenntnis vom Erlöschen bestätigt hast“ (§ 356 Abs. 5 BGB). Ohne Checkbox besteht das Widerrufsrecht fort: 14 Tage, auch nach Lieferung der Datei. Außerdem steht im Snippet selbst: „ENTWURF: Wortlaut und Einsatz muss Tobi rechtlich bestätigen“.
Weitere Schwächen des Snippets:
- Die Zustimmung wird nur als **Warenkorb-Attribut** per `fetch('/cart/update.js')` gespeichert. Es wird nicht abgewartet, und Fehler werden verschluckt (`catch(e){}`). Das Attribut bleibt stehen, auch wenn das Digitalprodukt wieder entfernt wird.
- Das Attribut hängt nicht an der Position selbst.
- Express-Checkout umgeht die Sperre nicht, weil `show_dynamic:false` im Template `product.teeinblue` gesetzt ist. Im Standard-Template (`product.json`) ist Express aber **an** (siehe P1-04).

**Fix:** Die Checkbox für **alle** `is_digital`-Produkte (außer Gutschein) rendern, nicht nur für `is_tib_digital`. Bei Theme-Formularen zusätzlich als Positions-Eigenschaft mitschicken, z. B. `<input type="hidden" name="properties[_Zustimmung Widerruf]" value="…" disabled>`, das beim Anhaken aktiviert wird. In `cart.js` das Absenden ohne Haken blockieren. Den Wortlaut rechtlich freigeben lassen. Die Bestätigung auf dauerhaftem Datenträger (Bestellbestätigung per E-Mail) muss den Hinweis enthalten [LIVE PRÜFEN: Notification-Template].

#### P0-04 · Header-CTA, leerer Warenkorb und Cross-Sell zeigen auf eine fast leere Kollektion
**Wo:** `settings_data.json` → `cart_cross_sell_collection: "schaufenster"`; `sections/cart-drawer.liquid` und `sections/main-cart.liquid` („Jetzt stöbern“ → `/collections/schaufenster`); Header-CTA „Portrait gestalten“ → `/collections/schaufenster`
**Belegt:** Die Kollektion `schaufenster` enthält 22 Produkte, davon **20 ARCHIVIERT**. Aktiv sind nur `haustier-wandkalender-2027` und `dein-hund-als-digitales-kunstwerk-sofort-download`. Keines der 35 neuen Wandbild-Produkte ist darin.
Folgen:
- Der wichtigste CTA im Header führt zu 2 Produkten (sehr wahrscheinlich großer Conversion-Verlust).
- Cross-Sell im Drawer: siehe P0-05.

**Fix:** Die Kollektion neu befüllen, z. B. als **smarte Kollektion** (Bedingung: Tag `typ:portrait` und Status aktiv) oder mit manueller Auswahl der Bestseller-Wandbilder. Alternativ CTA und leeren Warenkorb auf `/collections/wandbilder` oder `hundeportraits` umstellen. (Die Kollektionsseiten selbst gehören zu einem anderen Bereich des Audits. Hier geht es um die Wirkung auf den Kaufablauf.)

#### P0-05 · Cross-Sell im Warenkorb legt personalisierte Produkte mit einem Klick ohne Foto und ohne Stil in den Warenkorb
**Wo:** `sections/cart-drawer.liquid` (Block `.cross-sell`); `assets/cart.js` → `[data-cross-sell-add]`
**Belegt:** Der Knopf „Hinzufügen“ schickt nur `id` und `quantity` der ersten verfügbaren Variante. Aus `schaufenster` kommen heute genau der Foto-Kalender (`perso:foto`) und das Digitalportrait (Variante „Aquarell“, braucht ein Foto). Die Kundin bekommt dadurch Bestellungen ohne Foto, ohne Stil, ohne Namen und ohne `_pod_workflow`. Beim Digitalprodukt fehlt außerdem die Widerrufs-Zustimmung.
**Fix:** Im Cross-Sell nur Produkte ohne Personalisierung per Klick hinzufügen. Für alle anderen einen Link „Gestalten →“ auf die Produktseite zeigen:
```liquid
{%- assign xs_perso = p.tags | join: ',' -%}
{%- if xs_perso contains 'perso:foto' or xs_perso contains 'perso:name' or p.metafields.teeinblue.campaign_version != blank or p.requires_selling_plan -%}
  <a class="btn btn--secondary btn--sm" href="{{ p.url }}">Gestalten</a>
{%- else -%}
  <button … data-cross-sell-add="{{ p.selected_or_first_available_variant.id }}">…</button>
{%- endif -%}
```
Zusätzlich eine eigene Kollektion `cross-sell` anlegen, z. B. Halstücher, Spruch-Tassen ohne Namen, Gutschein, Sticker, und `cart_cross_sell_collection` darauf setzen.

#### P0-06 · Irreführende Versprechen „Live-Vorschau vor der Bestellung“ bei Produkten ohne Live-Vorschau
**Wo:** `templates/product.json` → Block `trust.t1` „Live-Vorschau vor der Bestellung“; Locale `products.product.delivery_estimate` „Live-Vorschau vor der Bestellung · Lieferung in 5 bis 9 Werktagen“; USP-Reihe `u1` „Live-Vorschau: Du siehst dein Bild, bevor du bestellst.“; `cart.trust.preview`
**Belegt:** Eine Live-Vorschau gibt es nur über teeinblue. Beim **Wandkalender** (Theme-Strecke, kein `pod.print_area`) gibt es keine. Trotzdem stehen dort Trust-Kachel, Lieferzeile und USP mit „Live-Vorschau“. Gleichzeitig sagt der Standard-Kurztext (Schema-Default des Blocks `text`, von `product.json` nicht überschrieben): „Wir schicken dir innerhalb von 48 Stunden eine kostenlose Vorschau. Gedruckt wird erst nach deinem OK.“ Auf ein und derselben Seite stehen also zwei widersprüchliche Abläufe. Außerdem nennt der Kalendertitel „drei Stile“, während die Stilwahl 11 Stile anbietet.
Dasselbe gilt für die Rückgabe-Richtlinie („Vorschau per E-Mail, bevor irgendetwas produziert wird … erst nach deinem Ja“) gegenüber FAQ und teeinblue („Live-Vorschau vor der Bestellung“). Welcher Ablauf gilt bei teeinblue-Bestellungen: Druck sofort oder erst nach einer Freigabe per E-Mail? [LIVE PRÜFEN beim Betreiber]
**Fix:** Trust- und Lieferzeilen an die tatsächliche Strecke koppeln:
```liquid
{%- if has_tib -%}Live-Vorschau vor der Bestellung{%- elsif perso == 'foto' -%}Kostenlose Vorschau per E-Mail vor dem Druck{%- endif -%}
```
Locale `delivery_estimate` aufteilen, z. B. in `delivery_estimate_tib` und `delivery_estimate_email_preview`. Beim Kalender die Stilwahl auf die drei tatsächlich angebotenen Stile begrenzen, per Metafeld `custom.fixed_art_style` oder eigener Liste. Die Rückgabe-Richtlinie an den echten teeinblue-Ablauf anpassen.

---

### P1: wichtig

#### P1-01 · Kein Schutz gegen Doppelklick beim Warenkorb-Knopf (mobil mit Foto-Upload wahrscheinlich)
**Wo:** `assets/cart.js` → `Cart.add()` und der `submit`-Listener
**Belegt:** Es wird nur die Klasse `btn--loading` gesetzt. CSS dazu: nur `.btn--loading .btn__label{opacity:.5}`. Der Knopf wird **nicht** `disabled`, `pointer-events` bleibt an, und die Prüfung `if (btn && btn.disabled) return;` greift nie. Ein 8-MB-Foto über Mobilfunk braucht mehrere Sekunden. Ein zweiter Tipp legt das Produkt ein zweites Mal ab (Menge 2 oder zwei Positionen).
**Fix:**
```js
async add(formData, { open = true, button } = {}) {
  if (button) { if (button.dataset.busy) return null; button.dataset.busy = '1'; button.disabled = true; button.setAttribute('aria-busy','true'); const l = button.querySelector('.btn__label'); if (l) { button._label = l.textContent; l.textContent = T.adding; } }
  try { … } finally { if (button) { delete button.dataset.busy; button.disabled = false; button.removeAttribute('aria-busy'); const l = button.querySelector('.btn__label'); if (l && button._label) l.textContent = button._label; } }
}
```
Dazu CSS `.btn--loading{pointer-events:none}` und einen Spinner. `T.adding` („Wird hinzugefügt …“) existiert bereits in den Locales, wird aber nirgends benutzt.

#### P1-02 · Sticky-Kaufleiste: erscheint zu spät, fehlt bei teeinblue vermutlich ganz, und ihr Knopf kann ins Leere klicken
**Wo:** `main-product.liquid` (`.sticky-atc` am Ende); `assets/theme.js` → „Sticky add-to-cart (mobile)“
**Belegt:**
- Die Leiste erscheint erst, wenn der echte Knopf **oberhalb** des Bildschirms liegt. Beim ersten Scrollen durch Galerie, Upload und Stilwahl gibt es mobil keinen sichtbaren Kauf-CTA. Der Knopf steht bei der Theme-Strecke nach Upload, Tipps, Namensfeld, 11 Stil-Chips (4 Reihen) und Varianten, also weit unter dem sichtbaren Bereich.
- Bei teeinblue ist `[data-add-to-cart]` ausgeblendet. Ein ausgeblendetes Element meldet `boundingClientRect.top = 0`, deshalb wird die Leiste **nie** sichtbar [VERMUTET, LIVE PRÜFEN]. Würde sie erscheinen, klickte sie den versteckten Theme-Knopf und legte das Produkt **ohne teeinblue-Personalisierung** ab.
- Der Leistenknopf zeigt immer „In den Warenkorb“, auch wenn die Variante ausverkauft oder nicht verfügbar ist. Der Klick passiert dann stumm, ohne Rückmeldung.
- Bild und Preis stammen vom Produkt, nicht von der Variante.

**Fix:**
1. Die Leiste ab etwa 60 % Scrolltiefe der Galerie zeigen. Bei Personalisierungsprodukten beschriftet sie **„Jetzt gestalten“** und springt zum Upload bzw. zu `.tee-customization-wrapper` (`scrollIntoView`). Erst wenn alles ausgefüllt ist, heißt sie „In den Warenkorb“.
2. Bei teeinblue `.tee-btn--atc` als Ziel verwenden, nicht `[data-add-to-cart]`.
3. Zustand spiegeln: `btn.disabled` und Beschriftung aus `product-form.js` bei `variant:change` übernehmen.

#### P1-03 · Galerie der Theme-Strecke: mobil kein Wischen, kein Zoom, keine Vollbildansicht
**Wo:** `main-product.liquid` (`.gallery__main` mit nur einem `<img>`); `theme.js` → „Product gallery“; `theme.css` `.gallery__main{aspect-ratio:1/1}` + `object-fit:contain`
**Belegt:** Bildwechsel nur per Tipp auf ein Vorschaubild. Zoom nur für `fine`-Zeiger (Maus). Bei Touch gibt es weder Wischen noch Pinch oder Lightbox. Die Vorschaubilder sind 4,6 rem breit, ab dem 7. Bild zeigt eine Kachel „+N weitere“. Betroffen sind heute Kalender, Digitalprodukt, alle 20 fertigen Designs und der Gutschein. Bei teeinblue liefert die App ihre eigene Galerie.
**Fix:** Das Hauptbild durch einen CSS-Scroll-Snap-Slider ersetzen (ohne Bibliothek): alle Medien nebeneinander in `.gallery__main`, `overflow-x:auto; scroll-snap-type:x mandatory`, jedes Bild `scroll-snap-align:center; flex:0 0 100%`. Punkte oder Zähler „3/9“ dazu. Die Vorschaubilder scrollen per `scrollTo` zum passenden Bild. Tippen öffnet einen `<dialog>` in Vollbild mit `touch-action:pinch-zoom`.

#### P1-04 · Express-Checkout auf Foto-Produkten überspringt Upload und Prüfungen
**Wo:** `templates/product.json` → `buy.show_dynamic: true`; `main-product.liquid` Block `buy_buttons` → `{{ form | payment_button }}`
**Belegt:** Der Express-Checkout (Shop Pay, PayPal, Apple Pay) geht an `cart.js` vorbei. Der Code blendet ihn nur für die Variante „Spezialwunsch“ aus. Beim Wandkalender und beim Digitalprodukt (beide `product.json`) kann man also direkt kaufen. [VERMUTET: Datei-Uploads als Positions-Eigenschaft kommen über den Express-Weg nicht mit, LIVE PRÜFEN] Außerdem umgeht der Express-Weg die Widerrufs-Zustimmung aus P0-03.
**Fix:** `{%- if block.settings.show_dynamic and perso == 'none' and is_digital == false -%}`, oder ein eigenes Template für Foto-Produkte mit `show_dynamic:false`.

#### P1-05 · Foto-Upload: HEIC-Vorschau, fehlende Fehlermeldungen, kein Fortschritt
**Wo:** `snippets/upload-field.liquid`; `theme.js` → „Upload preview“; `cart.js`
**Belegt:**
- `accept="image/jpeg,image/png,image/heic,image/webp"` enthält keine Dateiendungen. Einige Android-Dateiauswahlen filtern nach Endung und zeigen HEIC dann nicht an. Empfohlen: `accept="image/*,.heic,.heif"`.
- In `show()` bricht der Code ab, wenn `file.type` nicht mit `image/` beginnt. Manche Android-Browser liefern bei HEIC einen leeren `type`. Dann gibt es **keine Vorschau und keine Meldung**, die Datei wird aber trotzdem mitgeschickt. Die Kundin weiß nicht, ob es geklappt hat.
- Chrome und Firefox können HEIC nicht darstellen. Die Vorschau wird dann ein kaputtes Bild. Einen `onerror`-Fallback wie in `cart-item` gibt es hier nicht.
- Andere Dateitypen (z. B. PDF per Drag&Drop) werden stumm angenommen.
- Fehler erscheinen nur als Toast (2,6 s, unten). Am Feld selbst steht nichts, es gibt kein `aria-invalid` und keinen Fokus.
- Es gibt keine Upload-Fortschrittsanzeige, weil `fetch` keinen Fortschritt meldet.
- Der Entfernen-Knopf sitzt **innerhalb** des `<label>` (verschachtelte interaktive Elemente, Barrierefreiheit). Er ist 1,8 rem groß, also unter 44 px Touch-Fläche.

**Fix:** Ein Status-Absatz unter dem Feld (`<p class="upload__status" role="status">`) zeigt „✓ foto.heic (4,2 MB) ausgewählt“ oder eine Fehlermeldung. Bei HEIC ohne darstellbare Vorschau steht dort „HEIC-Foto ausgewählt, Vorschau im Browser nicht möglich, wird trotzdem übertragen“. `img.onerror` → Platzhalter anzeigen. Den Entfernen-Knopf aus dem Label herausnehmen und auf 44 px vergrößern. Für den Fortschritt `XMLHttpRequest` mit `upload.onprogress` im Warenkorb-Aufruf nutzen, wenn eine Datei dabei ist. **Ungeklärt:** Wie groß darf eine Datei als Positions-Eigenschaft bei Shopify maximal sein? Die angegebenen 20 MB mit einem echten 15-MB-iPhone-Foto testen [LIVE PRÜFEN].

#### P1-06 · Stil-Auswahl: 11 Theme-Stile gegen 13 teeinblue-Stile, keine Beispielbilder, Aquarell stumm vorgewählt
**Wo:** `settings_data.json` → `art_styles` (11 Stile); `snippets/style-picker.liquid`
**Belegt:**
- Kurztexte (Metafeld `custom.kurztext`, 33 Produkte) versprechen „13 Stile“, und die Startseite sagt ebenfalls „13 Stile“ (Locale `sections.transformation.step2_text`). Die Theme-Liste hat 11 Stile, und die Namen weichen ab („Aquarell mit Herz“ gegenüber teeinblue „Herzensbild“; „Anime“ und „Original“ fehlen).
- Beispielbilder sucht die Stilwahl über Alt-Texte „Stil: <Name>“. Außer beim Digitalprodukt hat **kein** Produkt solche Alt-Texte, deshalb gibt es beim Kalender nur Farbkreise. Das Metafeld `pmp.stil_bilder`, das 33 Produkte haben, nutzt der Style-Picker nicht (nur `product-card`).
- „Aquarell“ ist vorausgewählt. Wer nicht aktiv wählt, bekommt Aquarell, ohne es zu merken.
- Die Stilnamen in `live-preview.js` (cartoon, pixar-3d, aquarell, pastell …) passen nicht zu den Handles der Theme-Stile (`olgemalde`, `3d-cartoon`, `royal` …). Das ist wirkungslos, solange es keine Live-Vorschau gibt, sollte bei deren Aktivierung aber nachgezogen werden.

**Fix:** Eine gemeinsame Stil-Liste festlegen. Die Stilwahl im Theme optional die Bilder aus `pmp.stil_bilder` lesen lassen (Dateiname `pmpstil-<stil>-…`). Beim Kalender auf die drei Kalenderstile begrenzen.

#### P1-07 · 13 Shirt-Produkte haben 256 Varianten, Liquid liefert nur 250
**Wo:** `main-product.liquid` → `<script data-variants-json>` iteriert über `product.variants`; `assets/product-form.js`
**Belegt:** Laut Shopify-Doku liefert `product.variants` höchstens 250 Varianten („we've restricted product.variants to return a maximum of 250 variants“). Betroffen sind `dog-mom-…`, `cat-mom-…`, `who-let-the-dog-out-…`, `the-walking-dad-…`, `my-dog-is-calling-…`, `my-dog-is-cooler-…`, `herz-mit-pfoten-…`, `tierportrait-als-brustmotiv-…`, `dogs-wine-…`, `cats-wine-…`, `crazy-dog-lady-…`, `crazy-cat-lady-…`, `pet-evolution-…`. Die letzten 6 Kombinationen (in Admin-Reihenfolge) fehlen im JSON. Die Theme-Auswahl meldet dafür „Nicht verfügbar“, und der Knopf wird gesperrt. Da teeinblue bei diesen Produkten die Variantenauswahl übernimmt, ist das heute vermutlich unsichtbar [LIVE PRÜFEN: teeinblue-Auswahl bei der letzten Farbe × größten Größe × Hoodie].
**Fix:** Kurzfristig die Varianten auf höchstens 250 reduzieren (z. B. eine Farbe weniger). Langfristig die Varianten-Auswahl auf `product_option_value` und `?option_values=` umbauen (Shopify-Leitfaden „Support high-variant products“).

#### P1-08 · Warenkorb-Drawer mobil: „Zur Kasse“ unter dem sichtbaren Bereich, Cross-Sell schiebt ihn weiter nach unten
**Wo:** `sections/cart-drawer.liquid`; `theme.css` `.drawer__panel{overflow-y:auto}` und `.drawer__foot` (nicht sticky)
**Belegt:** Das ganze Panel scrollt. Der Fußbereich mit Summe und „Zur Kasse“ steht **nach** den Artikeln **und** nach dem Cross-Sell-Block. Bei zwei personalisierten Artikeln (mit Eigenschaften-Zeilen) plus Cross-Sell sind das etwa 900 px oder mehr. Auf einem 360×740-Gerät muss man zum Checkout scrollen.
**Fix:**
```css
.drawer--cart .drawer__panel{overflow:hidden}
.drawer--cart #CartDrawerContent{display:flex;flex-direction:column;height:100%}
.drawer--cart .drawer__body{overflow-y:auto;flex:1;min-height:0}
.drawer--cart .drawer__foot{position:sticky;bottom:0;padding-bottom:calc(1.2rem + env(safe-area-inset-bottom))}
```
Den Cross-Sell **in** `.drawer__body` unter die Artikel verschieben, damit er mitscrollt und der Fuß fix bleibt. Das Notizfeld eingeklappt lassen (ist es schon).

#### P1-09 · Gratisversand-Leiste aus, obwohl ab 150 € (DE) Gratisversand gilt (verschenkter AOV-Hebel)
**Wo:** `settings_data.json` → `cart_free_shipping_threshold: 0`; `cart-drawer.liquid` (`{%- if threshold > 0 -%}`); `cart.js` → `shippingBar()`
**Belegt:** Die Leiste ist fertig gebaut, aber abgeschaltet. Die Summe im Warenkorb sagt nur „inkl. MwSt. · Versand im Checkout“.
**Fix:** Die Schwelle auf 150 setzen. Die Leiste nur bei deutscher Lieferadresse bzw. `localization.country.iso_code == 'DE'` zeigen, weil EU und International keinen Gratisversand haben:
```liquid
{%- if threshold > 0 and localization.country.iso_code == 'DE' -%}
```
Den Text der Summenzeile konkretisieren: „inkl. MwSt. · DE-Versand 5,99 €, ab 150 € gratis“. Beim Durchschnittspreis eines Wandbilds (≈ 60–90 €) ist das ein realistischer Hebel für den Zweitkauf.

#### P1-10 · Versandinfos widersprechen sich (Richtlinie gegen Profil gegen FAQ gegen Template)
**Belegt:**
- **Versand-Richtlinie** (`/policies/shipping-policy`, auf die „zzgl. Versand“ auf jeder Produktseite verlinkt): „Wir liefern derzeit nach Deutschland, Österreich und in die Schweiz.“ Nennt **keine Beträge** („werden im Checkout angezeigt“).
- **Lieferprofil:** liefert in alle EU-Länder und nach CH, GB, US, AU, JP usw.
- **FAQ** (`page.faq.json`) und **Akkordeon `acc2` in `product.json`:** „Für einige gerahmte Poster gilt eine eigene Versandpauschale (5,69 bis 10,99 €)“. Die Gelato-Profile enthalten aber 0 Varianten, der Satz ist also veraltet.
- `product.personalized-fixed-style.json` → `acc2`: „Die Lieferzeit und Versandkosten werden vor Veröffentlichung … bestätigt.“ (Platzhaltertext; das Template ist aber unbenutzt.)
- Lieferprofil DE: Die Rate „Standard 5,99 €“ hat **keine Obergrenze**. Ab 150 € sieht die Kundin im Checkout zwei Mal „Standard“ (5,99 € und 0,00 €).

**Rechtsrisiko:** Die Versandkosten müssen vor Beginn des Bestellvorgangs leicht erkennbar sein. Der verlinkte Pflicht-Link führt heute auf eine Seite **ohne** Beträge.
**Fix:** In die Versand-Richtlinie eine Kostentabelle aufnehmen (DE 5,99 € / ab 150 € frei / Express 9,99 €; EU 13,99 €; CH und weltweit 19,99 €) und das Liefergebiet korrigieren. Die Rate 5,99 € mit der Bedingung „Warenwert < 150 €“ versehen. Den Gelato-Satz aus FAQ und `product.json` entfernen.

#### P1-11 · Digitalprodukt: „Sofort-Download“, „24 Stunden“ und „nach deiner Freigabe“ gleichzeitig
**Wo:** Handle `dein-hund-als-digitales-kunstwerk-sofort-download`, Tags `sofort`, `download`; `main-product.liquid` Block `buy_buttons` → `product.id == 11109360795986` → Locale `delivery_estimate_digital_portrait` „Deine fertige Datei innerhalb von 24 Stunden“; Block `text` → Default `text_digital` „… senden dir eine Vorschau per E-Mail. Nach deiner Freigabe erhältst du die finale digitale Datei.“; Trust `trust_digital` „Finale Datei nach deiner Freigabe per E-Mail“; FAQ „Digitale Portraits bekommst du innerhalb von 24 Stunden“
**Belegt:** Drei verschiedene Lieferversprechen auf derselben Seite. Außerdem ist die Produkt-ID fest in `main-product.liquid` eingetragen, was bei Neuanlage oder Duplikat bricht.
**Fix:** Einen Ablauf festlegen und überall gleich formulieren. Die Sonderbehandlung per ID durch ein Metafeld `custom.lieferzeit_text` ersetzen. Den Handle umbenennen (301-Weiterleitung anlegen), weil „sofort-download“ irreführend ist. Und entscheiden: Die drei fertigen teeinblue-Digitalportraits (aktiv, aber unveröffentlicht) veröffentlichen und das alte Produkt archivieren, oder umgekehrt.

#### P1-12 · Keine Bewertungen, dafür viele leere Vertrauensflächen
**Belegt:** 0 Judge.me-Bewertungen bei allen Produkten. Badge und Widget stehen im Template, der Titel-Block zeigt ohne Metafeld `reviews.rating` nichts (korrekt, keine Fake-Sterne). Die Garantie („Zufriedenheitsgarantie: Nicht glücklich? Wir drucken neu.“) steht nur als Kachel, ohne Link auf konkrete Bedingungen. Die Rückgabe-Richtlinie sagt: Neudruck nur bei Mängeln oder Abweichung von der Freigabe. „Nicht glücklich? Wir drucken neu“ verspricht mehr als die Richtlinie.
**Fix:** Die Garantie-Formulierung an die Richtlinie angleichen, z. B. „Neudruck bei Druckfehlern oder Transportschäden“, oder die Richtlinie erweitern. Review-Anfragen in Judge.me aktivieren (E-Mail nach Zustellung, Foto-Bewertungen mit Rabatt-Anreiz). Solange es keine Bewertungen gibt, echte Kundenfotos (mit Einwilligung) als Beweis zeigen.

#### P1-13 · Warenkorb zeigt das Produktbild, nicht das personalisierte Motiv
**Wo:** `snippets/cart-item.liquid` (`item.image`, sonst Produktbild)
**Belegt:** Bei teeinblue-Positionen sieht die Kundin im Warenkorb das Katalogbild, nicht ihr Motiv. Welche Eigenschaften teeinblue setzt (z. B. ein Vorschau-Link), ist nicht bekannt [LIVE PRÜFEN: `/cart.js`].
**Fix:** Wenn eine Positions-Eigenschaft mit Vorschau-URL existiert (z. B. `_tib_preview`, `Preview`, `_customization_image`), sie als Thumbnail verwenden. Dieselbe Logik wie der bestehende `/uploads/`-Zweig, nur mit Unterstrich-Eigenschaft.

#### P1-14 · Tierschutz-Hinweis: auf Produktseiten nie sichtbar, im Warenkorb immer
**Wo:** `main-product.liquid` Block `donation` (nur mit `custom.donation_verified == true`, fehlt bei allen); `cart-drawer.liquid` und `main-cart.liquid` (`{{ settings.donation_percent }} % unseres Gewinns …` ohne Bedingung); USP-Reihe in `product.json` „20 % vom Gewinn“
**Belegt:** Die Absicht war offenbar, den Hinweis nur bei geprüften Produkten zu zeigen. USP und Warenkorb zeigen ihn aber pauschal. Das ist inkonsistent, und wenn die Spende nicht belegt ist, besteht Risiko wegen irreführender Werbung.
**Fix:** Eine Entscheidung treffen. Entweder das Metafeld bei allen Produkten setzen (dann erscheint der Produkt-Hinweis mit Link auf `/pages/tierschutz`), oder den Hinweis in Warenkorb und USP an dieselbe Bedingung bzw. eine globale Einstellung koppeln.

#### P1-15 · Elf aktive Produkte mit nur einem Bild
**Belegt:** `personalisierter-babybody-…`, `personalisierte-stofftasche-mit-deinem-tier`, `personalisierte-sherpa-decke-…`, `personalisiertes-mauspad-mit-deinem-tier`, `personalisierte-samsung-hulle-…` (die iPhone-Hülle hat 31), `personalisiertes-kleinkind-t-shirt-…`, `personalisiertes-kinder-sweatshirt-…`, `personalisierte-sticker-…`, `personalisierte-kuscheldecke-…`, `personalisiertes-notizbuch-…`, `personalisierte-flagge-…`, `personalisiertes-kissen-…`, `personalisierter-kissenbezug-…` (dazu `personalisierte-sweatjacke-…` mit 2).
**Fix:** Mindestens 4 Bilder: Mockup mit Beispieltier, Detail/Material, Anwendung/Raum, Größenvergleich.

---

### P2: nice-to-have

| # | Wo | Befund [BELEGT] | Fix |
|---|---|---|---|
| P2-01 | `product-form.js` `update()` | Der Streichpreis wird nur aktualisiert, wenn beim Laden schon ein `[data-price-compare]` existiert. Die Klasse `price--sale` wird nicht umgeschaltet. Heute ohne Wirkung (keine Streichpreise). | Das Element immer rendern (hidden) und die Klasse mitschalten. |
| P2-02 | `cart-item.liquid` / `theme.css` `.cart-item .qty{transform:scale(.9)}` | Mengen-Knöpfe im Warenkorb ca. 37 × 42 px, also unter 44 px. Der Mülleimer-Knopf hat kein Padding. | `scale` entfernen, `min-width/height:44px`. |
| P2-03 | `cart-drawer.liquid` Notizfeld | Die Notiz wird erst beim Checkout-Submit übernommen. Über „Warenkorb ansehen“ oder beim Schließen geht der Text verloren. | Bei `change` per `/cart/update.js` mit `{note}` speichern (ohne Debounce-Problem). |
| P2-04 | `widerruf-digital.liquid` | Das Warenkorb-Attribut bleibt nach dem Entfernen des Digitalprodukts stehen. Das Intervall läuft 20 s lang. | Das Attribut bei `cart:updated` ohne Digitalposition leeren. |
| P2-05 | Upload- und Spezialwunsch-Fehler | Nur Toast (2,6 s, `role=status`), kein Fehler am Feld. | `aria-invalid`, Fehlertext unter dem Feld, `focus()`. |
| P2-06 | Sticky-Leiste | Hat kein `aria-hidden`, solange sie unsichtbar ist (nur `visibility:hidden`, das reicht). Der Knopf hat keinen `aria-label` mit Produktname. | `aria-label="{{ product.title }} in den Warenkorb"`. |
| P2-07 | `main-product.liquid` Block `variant_picker` | `options_with_values` wird nicht für die Verfügbarkeit genutzt, die berechnet nur das JS. Ohne JS sind alle Werte klickbar (unkritisch). | Bei Umbau (P1-07) `value.available` nutzen. |
| P2-08 | `live-preview.js` | Wird auf jeder Produktseite geladen, obwohl der Block nirgends erscheint (kein `pod.print_area`). | Nur laden, wenn `[data-live-preview]` existiert, oder die Datei entfernen. |
| P2-09 | Produkttypen / Preisendungen | Uneinheitliche Typen (siehe A2). Entwürfe mit ,94-Preisen. | Vor dem Veröffentlichen der Entwürfe auf ,99 runden. Typen vereinheitlichen (wichtig für Filter und Google Shopping). |
| P2-10 | Verwaiste Templates | `product.digital-portrait.json`, `product.personalized-fixed-style.json` (mit Platzhaltertexten „vor Veröffentlichung bestätigt“) | Löschen oder dokumentieren. |
| P2-11 | Lieferprofil Express 9,99 € | Bei POD ist die Produktion der Engpass. Ob „Express“ 5–9 Werktage wirklich verkürzt, ist unklar. | Mit Gelato abgleichen, sonst Express entfernen oder mit realistischer Zeit beschriften. |
| P2-12 | Stilwahl mobil | 11 Chips in 3 Spalten = 4 Reihen (ca. 360 px Höhe) vor dem Kaufknopf. | Horizontal scrollbare Chip-Reihe mit Beispielbild darüber, oder die Auswahl bis zum Upload eingeklappt lassen. |
| P2-13 | `upload-field` Hinweis „Kein Foto zur Hand? Bestelle jetzt und schick es später“ | Sinnvoll, aber die Bestellung kommt dann ohne Foto herein, und es gibt keinen Prozess-Hinweis in der Bestellung. | Eine versteckte Eigenschaft `_Foto folgt=ja` setzen, wenn kein Foto gewählt ist. So bleibt es im Backend sichtbar. |
| P2-14 | `kaninchen-socken-pfotenpause` | Keine SEO-Beschreibung. | Nachtragen. |
| P2-15 | Upsell/AOV | Es gibt keine Bundles (z. B. „2. Motiv −20 %“, „Tasse + Poster“), keine Mengenrabatte und keine Geschenkverpackung. Der Newsletter verspricht 10 % Erstbestellrabatt. Auf der Produktseite wird das nicht beworben. | Shopify-Rabatt „Kaufe X, erhalte Y“ oder Mengenstaffel. Ein Hinweis „Zweites Tier? Im Warenkorb −15 %“ unter dem Kaufknopf. Die Gratisversand-Leiste (P1-09). Gutschein als Cross-Sell. |

---

## TEIL C: Checkliste für die Live-Prüfung (morgen, mit echtem Handy)

1. **teeinblue-Produkt** (`personalisiertes-hundeportrait-auf-leinwand`), iPhone Safari und Android Chrome:
   - Sind unter dem Gestalter noch Theme-Upload, Theme-Stilwahl oder Theme-Kaufknopf sichtbar? (P0-01)
   - Steht „inkl. MwSt., zzgl. Versand“ in Preisnähe? (P0-02)
   - In den Warenkorb legen, dann `/cart.js` öffnen: Gibt es `properties.Stil = "Aquarell"`, `_pod_workflow` oder `_pod_provider`? (P0-01)
   - Öffnet sich der Drawer, oder wird auf /cart weitergeleitet? Stimmt die Zähler-Blase?
   - Erscheint beim Scrollen die Sticky-Leiste? (P1-02)
   - Shirt mit 256 Varianten: letzte Farbe × XXL × Hoodie wählbar? (P1-07)
2. **Wandkalender**: Foto als HEIC (iPhone) und als 15-MB-JPG hochladen, Doppeltipp auf „In den Warenkorb“ (P1-01, P1-05). Express-Checkout-Knopf sichtbar? Kommt das Foto durch? (P1-04)
3. **Digitalprodukt**: Gibt es eine Widerrufs-Checkbox? (P0-03) Welche Lieferzeit steht wo? (P1-11)
4. **Drawer** mit 2 Positionen auf 360 × 740: Ist „Zur Kasse“ ohne Scrollen sichtbar? (P1-08) Cross-Sell-Klick → Position ohne Foto? (P0-05)
5. **Checkout** mit DE-Adresse und 160 € Warenwert: Stehen zwei „Standard“-Raten da? (P1-10)
6. **Bestellbestätigungs-E-Mail**: Enthält sie die Widerrufs-Zustimmung bei Digitalprodukten?

## TEIL D: Empfohlene Reihenfolge für die Umsetzung

1. P0-01 + P0-02 (ein Umbau in `main-product.liquid`: Flag `has_tib`, dazu Preis- und Steuerhinweis für teeinblue)
2. P0-04 + P0-05 (Kollektion `schaufenster` neu befüllen, Cross-Sell-Logik)
3. P0-03 (Widerrufs-Checkbox für alle digitalen Produkte; Wortlaut juristisch freigeben lassen)
4. P0-06 + P1-10 + P1-11 (Texte vereinheitlichen: Vorschau-Ablauf, Versand, Lieferzeit)
5. P1-01, P1-08, P1-09 (Warenkorb: Doppelklick, fixer Fuß, Gratisversand-Leiste)
6. P1-02, P1-03, P1-05 (mobil: Sticky-CTA, Galerie, Upload)
7. Rest
