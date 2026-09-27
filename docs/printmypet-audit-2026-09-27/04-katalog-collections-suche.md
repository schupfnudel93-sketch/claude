# Audit 04: Katalog-Struktur, Collections, Suche (printmypet.de)

Stand: 27.09.2026 · Live-Theme `gid://shopify/OnlineStoreTheme/208344613202` ("PMP: Futterrechner v5.1 (FREIGABE)") · Zugriff nur lesend über das Shopify-Admin-API (die Website selbst war aus der Umgebung nicht erreichbar, es gibt also **keine Screenshots und keinen Test im echten Mobil-Browser**).

**Kennzeichnung**
- **[BELEGT]**: direkt aus Theme-Code oder Admin-Daten abgelesen bzw. berechnet.
- **[VERMUTET]**: Schluss aus Code und Daten zur Darstellung oder zum Nutzerverhalten, im Browser nicht geprüft.
- **[NICHT PRÜFBAR]**: mit dem Admin-API nicht einsehbar.

"Live" heißt in diesem Bericht: Produktstatus `ACTIVE` **und** im Vertriebskanal "Onlineshop" veröffentlicht. Die `productsCount`-Werte im Admin zählen archivierte Produkte und Entwürfe mit und sind deshalb deutlich höher als das, was Kunden sehen.

---

## 0. Kurzfazit

1. **Der wichtigste Kauf-Button zeigt fast nichts [BELEGT].** "Portrait gestalten" (mobil im Menü-Drawer), der Hero-CTA, "Eigenes Foto verwandeln" und "Jetzt starten" auf der Startseite führen alle auf `/collections/schaufenster`. Von den 22 Produkten dort sind **20 archiviert**. Live bleiben nur 2: das *digitale* Hundeportrait und der Kalender 2027. **Kein einziges Wandbild.** Dieselbe Collection füllt den Cross-Sell im Warenkorb und die Empfehlungen auf der 404-Seite.
2. **Die Kernprodukte fehlen in wichtigen Collections [BELEGT].** Die 33 personalisierten Wandbilder plus die 2 Wunschbilder stecken weder in "Personalisiert → Mit deinem Foto" noch in "Weihnachten" oder "Geburtstag". Die Weihnachts-Collection kündigt im Text "Wandbilder auf Leinwand, im Holzrahmen oder als Poster" an, enthält aber 0 davon.
3. **Die Menülogik ist falsch verdrahtet [BELEGT].** Mobil führt "Alle Stile ansehen" zu *Aquarell*, "Alle Geschenke ansehen" zu *Weihnachten* und "Alle Für dein Zuhause ansehen" zu Kissen/Decken (dort fehlen Tassen und Näpfe). Die Tierart ist kein eigener Einstieg. Handyhüllen, Mauspads, Taschen, Baby & Kind, lustige Motive und der Geschenkgutschein stehen in keinem Menü.
4. **Altlasten-Collections [BELEGT].** `tierportraits` hat den SEO-Titel "Personalisierte Tierportraits vom Foto", zeigt aber genau 1 Produkt, den digitalen Download. `frontpage` und `schaufenster` sind zu 90 % archiviert.
5. **Filter mobil [BELEGT, Wirkung VERMUTET].** Jeder Klick auf eine Checkbox lädt die Seite nach 250 ms neu. Mobil schließt sich dadurch der Filter-Drawer nach jeder einzelnen Auswahl.
6. **Produktkarten [BELEGT].** Zwei CSS-Regeln widersprechen sich (4:5 randlos gegen 1:1 vollständig), am Ende gewinnt 4:5 randlos. Querformat-Bilder von Textilien werden stark beschnitten. Die Titel sind 40 bis 58 Zeichen lang, das unterscheidende Material steht erst am Ende. Die Stil-Angabe widerspricht sich ("13 Stile" auf der Karte, "12 Stile" im Titel der Tasse). Badges erscheinen nie, weil kein Live-Produkt die Tags `Bestseller` oder `Neu` trägt.

---

## 1. IST-Zustand

### 1.1 Alle Collections (47)

Legende: **Admin** = `productsCount` im Admin (inkl. archiviert/Entwurf), **Live** = für Kunden sichtbar, **OS** = im Onlineshop veröffentlicht, **Menü** = von `main-menu` verlinkt, **Sort.** = Standardsortierung.

| Handle | Titel | Admin | Live | Art | Bild | Beschreibung | SEO-Titel/-Beschr. | OS | Menü | Sort. | Bemerkung |
|---|---|---|---|---|---|---|---|---|---|---|---|
| schaufenster | Schaufenster | 22 | **2** | manuell | – | **leer** | ja | ja | nein | Bestseller | **Header-CTA, Hero-CTA, Cross-Sell, 404.** Live nur Digital-Download + Kalender |
| frontpage | Startseite | 16 | **2** | manuell | – | leer | ja | ja | nein | Bestseller | wie schaufenster; in list-collections ausgeblendet |
| tierportraits | Tierportraits | 65 | **1** | manuell | ja | ja | ja | ja | nein | Bestseller | Live nur der Digital-Download. SEO "Personalisierte Tierportraits vom Foto" |
| bestseller | Bestseller | 4 | 4 | manuell | – | ja | **nein** | ja | nein | manuell | Startseite "Bestseller" (Leinwand Hund, Acryl-Aufsteller Hund, Holzrahmen Katze, Kalender) |
| alle-produkte | Alle Produkte | 322 | 117 | smart (Preis > 0) | – | ja | ja | ja | nein | Bestseller | Startseite "Alle Produkte" |
| wandbilder | Wandbilder | 49 | 33 | smart (typ:portrait UND Typ Wandbild) | – | ja | ja | ja | ja | **Bestseller** | Reihenfolge beginnt mit 13 × Pferd (siehe B-07) |
| hundeportraits | Hundeportraits | 43 | 11 | smart (tier:hund UND typ:portrait) | ja | ja | ja | ja | ja | manuell | 11 Materialien, Leinwand zuerst |
| katzenportraits | Katzenportraits | 55 | 11 | smart | ja | ja | ja | ja | ja | manuell | |
| pferdeportraits | Pferdeportraits | 49 | 11 | smart | ja | ja | ja | ja | ja | manuell | |
| fuer-hundemenschen | Geschenke für Hundemenschen | 201 | 76 | smart (tier:hund) | – | ja (lang) | ja | ja | ja (unter Geschenke) | Bestseller | vollständigstes "Hund"-Sortiment |
| fuer-katzenmenschen | Geschenke für Katzenmenschen | 160 | 57 | smart (tier:katze) | – | ja (lang) | ja | ja | ja | Bestseller | |
| fuer-pferdemenschen | Geschenke für Pferdemenschen | 120 | 42 | smart (tier:pferd) | – | ja (lang) | ja | ja | ja | Bestseller | |
| geschenke-weihnachten | Weihnachtsgeschenke für Tierliebhaber | 118 | 60 | smart (anlass:weihnachten) | – | ja (lang) | ja | ja | ja | Bestseller | **0 Wandbilder**, obwohl der Text sie verspricht |
| geschenke-geburtstag | Geburtstagsgeschenke für Tierliebhaber | 75 | 39 | smart (anlass:geburtstag) | – | ja (lang) | ja | ja | ja | Bestseller | **0 Wandbilder**. Text: "Poster ab knapp 15 Euro", tatsächlich ab 23,99 € |
| geschenke-erinnerung | Erinnerung an ein Tier, das gegangen ist | 68 | 35 | smart (anlass:erinnerung) | ja | ja (lang) | ja | ja | ja | manuell | nur Wandbilder (33 + 2 Wunschbild) |
| geschenke-einzug | Einzugsgeschenke & Wandbilder fürs neue Zuhause | 46 | 33 | smart | – | ja (lang) | ja | ja | ja | Bestseller | identisch mit `wandbilder` (gleiche Regel) |
| personalisierte-geschenke | Personalisierte Geschenke | 238 | 84 | smart (Tag personalisiert) | – | ja | ja | ja | nein | Bestseller | Startseiten-Kachel "Geschenke" |
| mit-foto | Mit deinem Foto | 133 | 29 | smart (personalisierung:mit-foto) | – | **leer** | **nein** | ja | ja | Bestseller | **enthält keines der 35 Wandbilder** |
| ohne-foto | Sofort lieferbare Motive | 81 | 45 | smart (personalisierung:ohne-foto) | – | **leer** | **nein** | ja | ja | Bestseller | Titel "sofort lieferbar" ist bei Print-on-Demand irreführend |
| geschenkideen-lustige-motive | Geschenkideen & Lustige Motive | 96 | 34 | manuell | – | ja | ja | ja | **nein** | Bestseller | Spruch-Tassen und Shirts, nirgends verlinkt |
| kleidung-fuer-tierliebhaber | Kleidung für Tierliebhaber | 88 | 22 | **manuell** | – | ja | ja | ja | ja | Bestseller | manuell gepflegt, neue Textilien fehlen leicht |
| t-shirts-fur-tierliebhaber | T-Shirts für Tierliebhaber | 44 | 14 | smart | ja | ja | nein | ja | ja | Bestseller | |
| hoodies-sweatshirts-fur-tierliebhaber | Hoodies & Sweatshirts für Tierliebhaber | 53 | 17 | smart | ja | ja | nein | ja | ja | Bestseller | |
| baby-kind-geschenke-mit-tiermotiven | Baby & Kind: Geschenke mit Tiermotiven | 8 | 5 | smart | ja | ja | nein | ja | **nein** | Bestseller | |
| tassen | Tassen & Becher | 46 | 22 | smart | – | ja | ja | ja | ja (unter "Für dein Zuhause") | manuell | |
| wohnen | Kissen, Decken & Wohnaccessoires | 20 | 14 | smart | – | ja ("…und Mauspads", enthält aber keine) | nein | ja | ja (2 × verlinkt) | Bestseller | |
| futternaepfe | Futternäpfe | 4 | 3 | smart | – | **leer** | nein | ja | ja | Bestseller | |
| accessoires | Accessoires mit Tiermotiven | 14 | 9 | smart | – | ja ("Schürzen, Socken, Schlüsselanhänger", live sind Halstücher, Notizbücher, Sticker, Flagge, Socken) | nein | ja | ja | Bestseller | |
| halstuecher | Halstücher für Hunde und Katzen | 4 | 4 | smart | – | **leer** | nein | ja | ja | Bestseller | |
| handyhuellen | Handyhüllen mit Tiermotiven | 7 | 2 | smart | – | ja | nein | ja | **nein** | Bestseller | steht in den Suchvorschlägen, aber in keinem Menü |
| mauspads | Mauspads mit Tiermotiven | 6 | 2 | smart | – | ja | nein | ja | **nein** | Bestseller | |
| tragetaschen | Taschen mit Tiermotiven | 10 | 4 | smart | – | ja | nein | ja | **nein** | Bestseller | |
| kalender | Haustier-Kalender | 1 | 1 | smart | – | ja | nein | **nein** | nein | Bestseller | das Menü verlinkt direkt das Produkt |
| futtermatten | Futtermatten | 2 | 0 | smart | – | ja | nein | **nein** | nein | Bestseller | tot |
| stil-aquarell | Aquarell-Portraits | 134 | 59 | smart (Tag ODER Titel enthält "Aquarell") | ja | ja | ja | ja | ja | manuell | 33 Wandbilder + 26 Textil/Deko/Tasse |
| stil-oelgemaelde | Klassische Ölgemälde | 114 | 59 | smart | ja | ja | ja | ja | ja | manuell | |
| stil-royal | Royale Portraits | 113 | 59 | smart | ja | ja | ja | ja | ja | manuell | |
| stil-cartoon | Cartoon-Portraits | 124 | 59 | smart | ja | ja | ja | ja | ja | manuell | |
| stil-3d-cartoon | 3D-Cartoon-Portraits | 112 | 59 | smart | ja | ja | ja | ja | ja | manuell | |
| stil-minimal | Minimal-Portraits | 118 | 59 | smart | ja | ja | ja | ja | ja | manuell | |
| stil-streetart | Street-Art-Portraits | 112 | 59 | smart | ja | ja | ja | ja | ja | manuell | |
| stil-retro | Retro-Portraits | 112 | 59 | smart | ja | ja | ja | ja | ja | manuell | |
| stil-popart | Pop-Art-Portraits | 112 | 59 | smart | ja | ja | ja | ja | ja | manuell | |
| stil-sketch | Sketch-Portraits | 112 | 59 | smart | ja | ja | ja | ja | ja | manuell | |
| stil-anime | Anime-Portraits | 39 | 36 | smart | ja | ja | ja | ja | ja | manuell | Wandbilder + 3 Tassen |
| stil-herzensbild | Herzensbild-Portraits | 33 | 33 | smart (Tag UND Typ Wandbild) | ja | ja | ja (Marke "PrintMyPet" statt "Print my Pet") | ja | ja | manuell | ohne Tassen, anders als Anime/Original |
| stil-original | Original: Dein Foto ohne Stil | 39 | 36 | smart | ja | ja | ja | ja | ja | manuell | |

**Leer für Kunden:** `futtermatten` (0, nicht veröffentlicht). Praktisch leer: `tierportraits` (1), `schaufenster` (2), `frontpage` (2), `handyhuellen` (2), `mauspads` (2). [BELEGT]
**Ohne Bild (21):** accessoires, alle-produkte, bestseller, einzug, futtermatten, futternaepfe, geburtstag, fuer-hunde/-katzen/-pferdemenschen, geschenkideen-lustige-motive, halstuecher, handyhuellen, wohnen, kleidung, mauspads, mit-foto, ohne-foto, schaufenster, frontpage, tragetaschen, tassen, wandbilder, weihnachten. [BELEGT]
**Ohne Beschreibung:** schaufenster, frontpage, mit-foto, ohne-foto, futternaepfe, halstuecher. [BELEGT]
**Ohne eigenen SEO-Titel/-Beschreibung (Shopify nimmt dann Titel und Beschreibung):** accessoires, baby-kind, bestseller, futtermatten, futternaepfe, halstuecher, handyhuellen, hoodies, kalender, wohnen, mauspads, mit-foto, ohne-foto, t-shirts, tragetaschen. [BELEGT]
**Metafeld `custom.intro`:** bei keiner Collection gesetzt. Das Intro wird also immer aus der Beschreibung geschnitten (siehe B-10). [BELEGT]

### 1.2 Produkte

- 322 Produkte gesamt: **117 live**, 3 aktiv, aber nicht im Onlineshop (`personalisiertes-{hunde,katzen,pferde}portrait-digital`), 12 Entwürfe, 190 archiviert. [BELEGT]
- **Live-Produkte ohne Collection: keine.** Jedes steckt mindestens in `alle-produkte`. Ohne eine Produkttyp- oder Tier-Portrait-Collection sind nur 4 Produkte: `dein-hund-als-digitales-kunstwerk-sofort-download`, `geschenkgutschein`, `wunschbild-von-deinem-tier`, `wunschbild-als-acryl-aufsteller`. Diese 4 sind über das Menü nur schwer zu finden. Der Gutschein und das Wunschbild (jedes Tier, mehrere Tiere) stehen in keinem Menü, das digitale Portrait nur unter "Wandbilder". [BELEGT]
- Live nach Typ: Wandbild 35, Tasse 22, Shirt 13 (plus T-Shirt, Hoodie, Sweatshirt usw.), Kissen 6, Kuscheldecke 6, Futternapf 3, Handyhülle 2, Mauspad 2, Kalender 1, Digitaler Download 1, Gutschein 1 und weitere. [BELEGT]
- Stil-Tags: Wandbilder tragen 13 Stil-Tags, die 3 Foto-Tassen 13 (Titel: "12 Stile"), die personalisierten Textilien und Deko 10 (ohne Anime, Herzensbild, Original). In `config/settings_data.json` → `art_styles` stehen noch **11** Stile, darunter "Aquarell mit Herz", das es als Tag und Collection nicht gibt. Auf der Startseite heißt es "12 Stile plus Original". [BELEGT]
- Tags `Bestseller` oder `Neu`: bei **keinem** Live-Produkt. [BELEGT]
- Streichpreise (compare-at): bei keinem Live-Produkt gesetzt. [BELEGT]
- 189 URL-Weiterleitungen sind angelegt (z. B. `/products/hundeportrait-leinwand` → `/products/personalisiertes-hundeportrait-auf-leinwand`). Für `/collections/tierportraits` gibt es keine, weil die Collection noch existiert. [BELEGT]

### 1.3 Menübaum `main-menu` (Header Desktop und mobiler Drawer)

Alle Links sind als absolute `https://printmypet.de/...`-URLs vom Typ HTTP angelegt, nicht als Shopify-Ressource. Ändert sich ein Handle, bricht der Link still. [BELEGT]

```
[Mobil oben im Drawer: Button "Portrait gestalten" -> /collections/schaufenster (2 Live-Produkte)]
1  Personalisiert            -> /collections/mit-foto
   ├ Mit deinem Foto         -> /collections/mit-foto        (29, ohne Wandbilder!)
   └ Sofort lieferbare Motive-> /collections/ohne-foto       (45)
2  Wandbilder                -> /collections/wandbilder      (33)   [Mega-Promo: "Wandbilder: Leinwand, Poster, Acryl" -> wandbilder]
   ├ Hundeportraits          -> hundeportraits (11)
   ├ Katzenportraits         -> katzenportraits (11)
   ├ Pferdeportraits         -> pferdeportraits (11)
   ├ Alle Wandbilder         -> wandbilder (doppelt zum "Alle Wandbilder ansehen" im Drawer)
   └ Digitales Portrait      -> /products/dein-hund-als-digitales-kunstwerk-sofort-download
3  Kleidung                  -> kleidung-fuer-tierliebhaber (22)
   ├ T-Shirts                -> t-shirts-fur-tierliebhaber (14)
   ├ Hoodies und Sweatshirts -> hoodies-sweatshirts-fur-tierliebhaber (17)
   └ Alle Kleidung           -> kleidung-fuer-tierliebhaber (doppelt)
4  Für dein Zuhause          -> wohnen (14; ohne Tassen/Näpfe)
   ├ Tassen und Becher       -> tassen (22)
   ├ Kissen und Decken       -> wohnen (identisch mit dem Oberpunkt)
   └ Futternäpfe             -> futternaepfe (3)
5  Accessoires               -> accessoires (9)
   └ Halstücher              -> halstuecher (4)
6  Stile                     -> stil-aquarell (!)
   ├ Aquarell, Klassisches Ölgemälde, Royal, Cartoon, 3D-Cartoon, Minimal,
   │ Street-Art, Retro, Pop-Art, Anime, Herzensbild, Sketch, Original   (13 Links, alle ok)
7  Geschenke                 -> geschenke-weihnachten (!)
   ├ Weihnachten, Geburtstag, Erinnerung, Zum Einzug
   └ Für Hundemenschen, Für Katzenmenschen, Für Pferdemenschen
8  Kalender 2027             -> /products/haustier-wandkalender-2027
9  Wissen                    -> /blogs/magazin
   └ Magazin, Futterrechner, So funktioniert's, Häufige Fragen, Unsere Bücher, YouTube, Tierschutz, Über uns
10 Gratis-Startpaket         -> /pages/welpen-startpaket
```

**Header-Blöcke:** Mega-Promo "Tierportraits" (Titel passt zu keinem Menüpunkt, der Punkt heißt "Personalisiert", der Block **wird also nie angezeigt**, siehe B-15) → hundeportraits. Mega-Promo "Produkte" (ebenfalls kein Menüpunkt dieses Namens) → wandbilder. [BELEGT: `sections/header.liquid` vergleicht `block.settings.menu_title == link.title`, `header-group.json` hat `menu_title` "Tierportraits" bzw. "Produkte"]

**Footer:** `footer` (Alle Wandbilder, Kalender 2027, So funktioniert's, Magazin, Suche, Über uns, Kontakt), `footer-wissen`, `footer-service` (inkl. Sonderwunsch, Suche).

**Linkprüfung:** Alle Menü-Ziele existieren, sind veröffentlicht und haben mindestens 3 Live-Produkte. Tote Links gibt es nicht. Die Probleme sind inhaltlicher Art (Abschnitt 2). [BELEGT]

**Nirgends im Menü:** handyhuellen, mauspads, tragetaschen, baby-kind, geschenkideen-lustige-motive, personalisierte-geschenke, bestseller, alle-produkte, Geschenkgutschein, Wunschbild bzw. Sonderwunsch (nur im Footer), "andere Tiere". [BELEGT]

### 1.4 Wer verlinkt worauf (Templates)

| Stelle | Ziel | Live-Inhalt |
|---|---|---|
| Header-CTA "Portrait gestalten" (Desktop ab 990 px; mobil nur als erster Button im Drawer, `theme.css` Z. 927 blendet ihn im Header aus) | schaufenster | 2 (Digital + Kalender) |
| Startseite Hero "Portrait gestalten" | schaufenster | 2 |
| Startseite Verwandlung "Eigenes Foto verwandeln" | schaufenster | 2 |
| Startseite So-funktioniert's "Jetzt starten" | schaufenster | 2 |
| Startseiten-Kacheln Hunde/Katzen/Pferde ("Portraits & Geschenke") | hunde-/katzen-/pferdeportraits | je 11 Wandbilder, **keine Geschenke** |
| Startseiten-Kachel Geschenke | personalisierte-geschenke | 84 |
| Startseite "Alle Produkte" | alle-produkte | 117 |
| Startseite Bestseller | bestseller | 4 (gut) |
| Startseite Stilkarten (12) | stil-* | ok |
| Warenkorb Cross-Sell (`settings.cart_cross_sell_collection`) | schaufenster | 2 |
| 404 "Bestseller" | schaufenster | 2 |
| 404 Button "Bestseller ansehen" | `routes.all_products_collection_url` = /collections/all | alles, unsortiert |

---

## 2. Befunde

Priorität: **P0** = kostet jetzt direkt Umsatz oder ist sachlich falsch · **P1** = deutliche Hürde bei Conversion oder Auffindbarkeit · **P2** = Feinschliff.

### P0

**B-01 · P0 · Collection `schaufenster` (Header-CTA, Hero, Cross-Sell, 404) [BELEGT]**
*Problem:* 20 von 22 Produkten sind archiviert. Live bleiben `dein-hund-als-digitales-kunstwerk-sofort-download` (Typ Digitaler Download) und `haustier-wandkalender-2027`. Wer auf "Portrait gestalten" tippt, sieht also kein einziges Wandbild, keine Katze und kein Pferd. Das ist der wichtigste Einstieg des Shops, auf dem Handy der erste Button im Menü und der Hero-CTA. Der Cross-Sell im Warenkorb und die 404-Empfehlungen greifen auf dieselbe Collection zu. Weil die Collection nicht leer ist, zeigt das Theme auch keinen Leer-Zustand.
*Fix (eine Variante wählen):*
(a) `schaufenster` in eine **smarte Collection** umbauen (Regel: Tag `typ:portrait` ODER `typ:wunschbild`) mit **manueller Sortierung**: Hund Leinwand, Katze Leinwand, Pferd Leinwand, Hund Poster im Holzrahmen … Wunschbild ans Ende. Den Kalender und das digitale Portrait bei Bedarf manuell ergänzen. Oder
(b) alle CTAs auf eine neue Landingpage "Portrait gestalten" umstellen: Tier wählen (3 große Kacheln → hunde-/katzen-/pferdeportraits), darunter "Anderes Tier / mehrere Tiere → Wunschbild". Betroffene Einstellungen: `sections/header-group.json` → `header.settings.cta_url`; `templates/index.json` → `hero.settings.cta_primary_url`, `transformation.settings.cta_url`, `how.settings.cta_url`; `config/settings_data.json` → `cart_cross_sell_collection` (Empfehlung: `bestseller`); `templates/404.json` → `bestsellers.settings.collection` (Empfehlung: `bestseller`).
Zusätzlich die Admin-Collection bereinigen: archivierte Produkte aus den manuellen Collections `schaufenster`, `frontpage` und `tierportraits` entfernen, damit Zähler und Admin wieder stimmen.

**B-02 · P0 · Collection `mit-foto` ("Personalisiert → Mit deinem Foto") [BELEGT]**
*Problem:* Die Regel lautet Tag `personalisierung:mit-foto`. Keines der 35 Live-Wandbilder (33 Tier-Portraits + 2 Wunschbild) trägt diesen Tag, sie haben `perso:foto` bzw. keinen. Der erste Menüpunkt zeigt so 29 Produkte (Textilien, Deko, Kalender, 1 Tasse), aber nicht das Kernprodukt. Außerdem fehlen 2 der 3 Foto-Tassen.
*Fix:* Allen Foto-Produkten `personalisierung:mit-foto` geben (35 Wandbilder, 3 Foto-Tassen `personalisierte-{hunde,katzen,pferde}tasse-mit-foto`) oder die Regel auf "Tag `personalisierung:mit-foto` ODER `perso:foto` ODER `typ:portrait`" erweitern. Sortierung auf manuell stellen, Wandbilder zuerst.

**B-03 · P0 · Collections `geschenke-weihnachten` und `geschenke-geburtstag` [BELEGT]**
*Problem:* Weihnachten hat 60 Live-Produkte, **0 Wandbilder**. Geburtstag hat 39, davon 22 Tassen und **0 Wandbilder**. Beide Texte versprechen Leinwände und gerahmte Bilder. Im Geburtstagstext steht "Poster ab knapp 15 Euro", das günstigste Live-Poster kostet aber 23,99 €. Eine falsche Preisangabe im Shoptext ist ein Abmahnrisiko. Weihnachten ist ab Oktober die wichtigste Saison-Collection, und dort fehlt ausgerechnet das Hauptgeschenk.
*Fix:* Den 33 Portrait-Wandbildern und den 2 Wunschbildern die Tags `anlass:weihnachten` und `anlass:geburtstag` geben. Beide Collections auf **manuelle Sortierung** stellen (Wandbilder Hund/Katze vorn, dann Tassen, Decken, Gutschein). Den Satz "Poster ab knapp 15 Euro" durch den echten Preis ersetzen ("ab 23,99 €") oder die Zahl weglassen.

**B-04 · P0 · Collection `tierportraits` [BELEGT]**
*Problem:* Die URL mit dem stärksten Suchbegriff ("Personalisierte Tierportraits vom Foto") zeigt 1 Produkt, den digitalen Download. 64 von 65 Produkten sind archiviert. In `/collections` (list-collections) erscheint sie als Kachel "Tierportraits · 1 Produkte". [VERMUTET: Google und alte Links landen hier.]
*Fix:* Entweder in eine smarte Collection umbauen (Tag `typ:portrait` ODER `typ:wunschbild`) und so zur zentralen "Alle Portraits"-Seite machen, oder löschen und eine Weiterleitung `/collections/tierportraits` → `/collections/wandbilder` anlegen. Empfehlung: umbauen, dann ersetzt sie `wandbilder`. Siehe Zielstruktur in Abschnitt 3.

### P1

**B-05 · P1 · `sections/header.liquid`, mobiler Drawer, Link `general.show_all` (ca. Z. 83: `<a href="{{ link.url }}"><strong>{{ 'general.show_all' | t: title: link.title }}</strong></a>`) zusammen mit den Zielen im Menü [BELEGT]**
*Problem:* Der erste Link in jedem mobilen Untermenü heißt "Alle {Titel} ansehen" und führt auf die URL des Oberpunkts. Daraus werden: "Alle Stile ansehen" → **nur Aquarell**, "Alle Geschenke ansehen" → **nur Weihnachten**, "Alle Für dein Zuhause ansehen" → nur Kissen/Decken (ohne Tassen, Näpfe), "Alle Personalisiert ansehen" (grammatisch schief) → mit-foto (ohne Wandbilder), "Alle Wissen ansehen" → Blog. Bei Wandbilder und Kleidung steht der Link doppelt ("Alle Wandbilder ansehen" plus "Alle Wandbilder").
*Fix:* (1) Den Oberpunkten richtige Sammelziele geben: "Stile" → neue Seite `/pages/stile` oder die Startseite mit Anker `/#stile`; "Geschenke" → `personalisierte-geschenke` bzw. eine neue Geschenk-Collection; "Für dein Zuhause" → neue smarte Collection `zuhause` (Typen Tasse, Kissen, Kuscheldecke, Handtuch, Weihnachtsschmuck, Futternapf, Mauspad). (2) Im Liquid den "Alle …"-Link nur ausgeben, wenn kein Kind dieselbe URL hat (`unless link.links contains url`), oder die Kinder "Alle Wandbilder" und "Alle Kleidung" aus dem Menü nehmen. (3) Bei Oberpunkten, die kein Sammelziel haben ("Wissen"), den Link weglassen: Menü-Link auf `#` setzen und im Liquid `if link.url != '#'` prüfen.

**B-06 · P1 · Informationsarchitektur `main-menu` [BELEGT, Wirkung VERMUTET]**
*Problem:* Mobil stehen 10 Oberpunkte plus CTA im Drawer. Nach **Tierart** gibt es keinen Einstieg: "Hund" steckt einmal unter Wandbilder (nur 11 Wandbilder) und einmal unter Geschenke → "Für Hundemenschen" (das eigentliche Hunde-Sortiment mit 76 Produkten). Nach **Produkt**: Tassen stehen unter "Für dein Zuhause". Handyhülle, Mauspad, Taschen, Baby & Kind, Notizbuch, Sticker, Flagge und Gutschein fehlen ganz. Leinwand und Poster sind nicht getrennt ansteuerbar (Wandbilder zeigt 33 Karten, 3 Tiere × 11 Materialien gemischt). "Andere Tiere" (Kaninchen, Vogel, mehrere Tiere) findet man nicht, obwohl das Wunschbild genau das abdeckt. "Kalender 2027" und "Gratis-Startpaket" belegen Oberpunkte.
*Fix:* Siehe Zielstruktur in Abschnitt 3. Maximal 6 Oberpunkte: **Portrait gestalten**, Nach Tier, Produkte, Stile, Anlässe & Geschenke, Wissen. Kalender und Startpaket als Unterpunkte bzw. Promo-Kachel.

**B-07 · P1 · Collection `wandbilder`, Standardsortierung "Bestseller" [BELEGT]**
*Problem:* Mit Bestseller-Sortierung liefert das Admin-API die ersten 13 Positionen als Pferdeportraits (Acryl-Aufsteller, Hartschaum, Holz …). Nach den Mega-Promos ("Dein Hund als Kunstwerk") ist aber der Hund die Hauptzielgruppe. [VERMUTET: Der Storefront zeigt dieselbe Reihenfolge, weil es kaum Verkaufsdaten gibt.] Dasselbe gilt für `personalisierte-geschenke`: Pferd vor Katze vor Hund bei den Wandbildern.
*Fix:* `wandbilder`, `personalisierte-geschenke`, `fuer-*menschen` und `geschenke-*` auf **manuelle Sortierung** stellen: Hund Leinwand, Katze Leinwand, Hund Poster im Holzrahmen, Katze Poster im Holzrahmen, Pferd Leinwand … Die günstigen Varianten (Poster ab 23,99 €) früh zeigen, damit der Einstiegspreis sichtbar ist.

**B-08 · P1 · `sections/main-collection.liquid`, Script am Ende (ca. Z. 122–123: `form.addEventListener('change', () => { … setTimeout(() => form.submit(), 250); })`) [BELEGT, Wirkung VERMUTET]**
*Problem:* Jede Änderung an einem Filter schickt das Formular nach 250 ms ab und lädt die Seite neu. Mobil liegt das Formular im Drawer `#FiltersDrawer` (siehe `place()`). Nach **jedem** Tipp auf eine Checkbox lädt die Seite also neu, der Drawer ist zu, und man steht wieder oben. Zwei Filter zu kombinieren (z. B. "Katze" + "Leinwand") braucht dreimal Öffnen. Einen "Anwenden"-Button gibt es nur in `<noscript>`.
*Fix:* Mobil (`mq.matches`) nicht automatisch abschicken, sondern im `.drawer__panel` einen festen Fuß mit "X Produkte anzeigen" (submit) und "Zurücksetzen" einbauen. Auto-Submit nur am Desktop. Besser noch: Ergebnisse per `fetch(url + '&section_id=…')` nachladen, ohne die Seite neu zu laden.
*[NICHT PRÜFBAR]:* Welche Filter in der App "Search & Discovery" aktiv sind (Produkttyp, Tier, Stil, Preis), ist per Admin-API nicht auslesbar. Gibt es außer "Verfügbarkeit" keinen Filter, rendert das Theme gar kein Filterformular (`usable_filters == 0`), und der Filter-Button bleibt versteckt. **Morgen im Theme-Editor bzw. in Search & Discovery prüfen.** Empfohlene Filter: Tier (Tag `tier:`), Produkt (Produkttyp bzw. Tag `produkt:`), Stil (Tag `Stil:`), Preis.

**B-09 · P1 · Produktkarte, Bildformat: `assets/theme.css` Z. 977–979 gegen `assets/pmp-222.css` (Regeln `.card--product .card__img-wrap{aspect-ratio:4/5}` und `.card--product .card__img-wrap img{…object-fit:cover}`) [BELEGT, optische Wirkung VERMUTET]**
*Problem:* theme.css will Karten "vollständig" (1:1, `contain`, weißer Grund) zeigen. pmp-222.css wird danach geladen (`layout/theme.liquid`) und setzt 4:5 mit `cover`. `object-fit:cover` gewinnt zusätzlich über die höhere Spezifität (`… img` gegen `.card__img`). Ergebnis: Alle Karten sind 4:5 randlos beschnitten. Querformat-Motive (2048×1448: Hoodie, Sweatjacke, Sweatshirt, Babybody, Kinder-T-Shirt, Stofftasche Natur) verlieren rund 44 % der Breite, quadratische (Tassen-Bundles, Kissen, Decken, Handyhüllen) 20 %. [VERMUTET: Produkte werden angeschnitten, und in gemischten Rastern (Weihnachten, Hundemenschen) wirkt das unruhig.]
*Fix:* Die tote Regel in theme.css Z. 977–979 entfernen. Mockups der Nicht-Wandbild-Produkte im Format 4:5 neu erzeugen, oder `object-fit:contain` mit `background:#fff` nur für `.card--product:not(.card--wall)` setzen (Klasse im Snippet nach `product.type == 'Wandbild'` vergeben).

**B-10 · P1 · `sections/main-collection.liquid` Z. 4–17 (Intro aus der Beschreibung) [BELEGT]**
*Problem:* `collection.description | strip_html` fügt zwischen `</p><p>` kein Leerzeichen ein. Aus "…auf einmal.</p><p><strong>Welche Größe…" wird "auf einmal.Welche Größe". Der Satz-Split auf `'. '` greift dort nicht, deshalb erscheint im Intro sichtbar zusammengeklebter Text, z. B.:
- `geschenke-einzug`: "…füllt beides auf einmal.Welche Größe passt wohin?"
- `geschenke-geburtstag`: "…die nicht im Schrank verschwinden.Für jedes Budget: Poster ab knapp 15 Euro…" (enthält zusätzlich den falschen Preis aus B-03)
- `fuer-hundemenschen`: "Genau für die ist diese Auswahl.Was hier drin ist: …"
- `fuer-katzenmenschen`: "…trotzdem gut.Was hier drin ist: …"
- `fuer-pferdemenschen`: "…keins hängt an der Wand.Was hier drin ist: …"
Bei langen Intros greift außerdem `truncate: 260` mitten im Satz, obwohl der Kommentar "never cut mid-sentence" verspricht.
*Fix:* Vor `strip_html` die Absätze trennen: `assign desc_plain = collection.description | replace: '</p>', '</p> ' | replace: '<br>', ' ' | strip_html | strip_newlines | strip`. Besser noch: für alle Collections mit langer Beschreibung das Metafeld `custom.intro` pflegen (1 Satz, höchstens 120 Zeichen). Das Theme unterstützt das schon, es ist nur nirgends befüllt.

**B-11 · P1 · Mobiler Kopfbereich der Collection: `theme.css` Z. 967 (`.collection-hero__image{display:none}` unter 990 px) plus Intro [VERMUTET]**
*Problem:* Mobil stehen vor dem ersten Produkt Breadcrumb, Eyebrow "Kategorie", H1, ein Intro mit bis zu 260 Zeichen (≈ 6–8 Zeilen bei 360 px und 16–17 px Schrift) und die Toolbar. Die Produkte beginnen vermutlich erst unterhalb des ersten Bildschirms. Gleichzeitig ist ausgerechnet bei den Stil-Collections das Collection-Bild (Stil-Beispiele Hund/Katze/Pferd) mobil ausgeblendet, obwohl es dort das stärkste Verkaufsargument ist.
*Fix:* Mobil das Intro auf 2 Zeilen kürzen (`-webkit-line-clamp:2` plus "Mehr"), Eyebrow "Kategorie" ausblenden. Für `stil-*` das Bild mobil als flaches Banner zeigen (`aspect-ratio:16/7`, `max-height:180px`), z. B. `.template-collection .collection-hero__image` nur dann einblenden, wenn `collection.handle contains 'stil-'` (Klasse im Liquid setzen).

**B-12 · P1 · `snippets/product-card.liquid`, Kartentitel Z. ca. 88 (`{{ product.title }}`) [BELEGT, Lesbarkeit VERMUTET]**
*Problem:* Die Wandbild-Titel sind 39 bis 58 Zeichen lang und beginnen alle gleich ("Personalisiertes Katzenportrait als Poster im Metallrahmen"). Im 2-Spalten-Raster (Z. 499 `repeat(2,1fr)`, bei 360 px ≈ 160 px Kartenbreite) brechen sie auf 3–4 Zeilen um. Das Unterscheidende (Material) steht ganz am Ende. Die Kartenhöhen variieren, das Raster wird unruhig. Die Tassen-Sprüche haben bis zu 71 Zeichen.
*Fix:* Kurzen Kartentitel aus einem Metafeld (`custom.card_title`, z. B. "Poster im Metallrahmen") verwenden, mit Fallback auf `product.title`. Titel per CSS `-webkit-line-clamp:2` begrenzen. In Tier-Collections das Tier weglassen, weil es sich aus der Collection ergibt.

**B-13 · P1 · `snippets/product-card.liquid` Z. 40–60 (Stil-Eyebrow) plus Produktdaten [BELEGT]**
*Problem:* Bei mehr als 2 Stil-Tags zeigt die Karte "N Stile". Heraus kommen: Wandbilder "13 Stile", Foto-Tassen "13 Stile", obwohl der Titel "…mit Foto: 12 Stile" sagt, Textilien "10 Stile". In `settings.art_styles` stehen 11 Stile, darunter "Aquarell mit Herz", das es nicht mehr gibt. Auf der Startseite heißt es "12 Stile plus Original", in den Collection-Texten "13 Stile". Innerhalb einer Stil-Collection (z. B. Aquarell) steht auf jeder Karte "13 Stile" statt "Aquarell", der Kontext geht verloren.
*Fix:* (1) Die Zählweise einheitlich festlegen, Empfehlung "12 Stile + Original". Die Titel der Tassen und `settings.art_styles` angleichen ("Aquarell mit Herz" → "Herzensbild", Anime und Original ergänzen). (2) In `stil-*`-Collections als Eyebrow den Stilnamen zeigen: `if collection.handle contains 'stil-'` → Collection-Titel ohne "-Portraits".

**B-14 · P1 · 404-Seite: `sections/main-404.liquid` Z. 6–7 und `templates/404.json` [BELEGT]**
*Problem:* Der Button "Bestseller ansehen" führt auf `routes.all_products_collection_url` (= /collections/all, unsortiert, alles). Der Block "Vielleicht das hier?" zieht aus `schaufenster` und zeigt deshalb nur 2 Karten (Digital + Kalender). Es gibt kein Suchfeld und keine Einstiege nach Tier oder Kategorie. Wegen der 190 archivierten Produkte landen vermutlich viele Besucher auf alten Links. Die 189 Weiterleitungen fangen einen Teil davon ab.
*Fix:* Button 2 auf `/collections/bestseller` bzw. die neue "Portrait gestalten"-Seite richten. `templates/404.json` → `bestsellers.settings.collection` = `bestseller`. In `main-404.liquid` ein Suchformular (wie in `main-search.liquid`) und 4 Chips einbauen (Hundeportrait, Katzenportrait, Pferdeportrait, Geschenke).

**B-15 · P1 · Mega-Promos in `sections/header-group.json` [BELEGT]**
*Problem:* `header.liquid` zeigt einen Promo-Block nur an, wenn `block.settings.menu_title == link.title`. Die Blöcke heißen "Tierportraits" und "Produkte", im `main-menu` gibt es diese Titel aber nicht (dort stehen "Personalisiert", "Wandbilder", …). **Beide Promos werden nie gerendert** (betrifft nur Desktop). Im Kontext-Briefing war angenommen, dass sie aktiv sind.
*Fix:* `menu_title` auf die echten Titel setzen (z. B. mega1 → "Wandbilder", mega2 → "Geschenke") oder die Menü-Titel anpassen.

**B-16 · P1 · Collection-Titel `ohne-foto` = "Sofort lieferbare Motive" [BELEGT]**
*Problem:* Alles wird erst nach der Bestellung gedruckt (Lieferzeit laut Theme "5 bis 9 Werktage"). "Sofort lieferbar" weckt eine falsche Erwartung und ist rechtlich heikel (Angaben zur Lieferzeit). [VERMUTET: Rückfragen oder Beschwerden.]
*Fix:* Umbenennen in "Motive ohne Foto" oder "Designs mit Namen". Beschreibung und SEO-Text ergänzen.

**B-17 · P1 · Suche, Leer-Zustand: `sections/main-search.liquid` Z. ca. 14–15 [BELEGT]**
*Problem:* Bei 0 Treffern erscheint nur der Satz "Nichts gefunden für „…“. Versuch es mit „Hund“, „Katze“ oder „Tasse“." Diese Vorschläge sind nicht verlinkt, es gibt keine Kategorien und keine Bestseller. Suchbegriffe wie "Regenbogenbrücke", "Leinwand Katze", "Kaninchen", "Gutschein" oder "Geschenk" laufen ins Leere, soweit Titel und Tags sie nicht enthalten. [VERMUTET für einzelne Begriffe, weil die Storefront-Suche nicht getestet werden konnte.]
*Fix:* Im Leer-Zustand Chips (`settings.search_suggestions`) als Links, die Kategorie-Kacheln (Hund, Katze, Pferd, Geschenke) und `featured-collection` mit `bestseller` rendern. In der App "Search & Discovery" Synonyme pflegen: Regenbogenbrücke / verstorben / Gedenken → Erinnerung; Bild / Gemälde / Foto auf Leinwand → Leinwand; Handyhülle / Handycase / iPhone; Becher / Tasse; Gutschein / Geschenkkarte.

**B-18 · P1 · Menüpunkte für Umsatzbringer fehlen [BELEGT]**
*Problem:* Der Geschenkgutschein (25–200 €, Last-Minute-Umsatz zu Weihnachten) und das Wunschbild (62,99–247,99 €, das einzige Produkt für "andere Tiere" und "mehrere Tiere") sind über kein Menü erreichbar. Handyhüllen stehen in den Suchvorschlägen, aber in keinem Menü.
*Fix:* Unter "Anlässe & Geschenke" den Punkt "Geschenkgutschein" ergänzen. Unter "Nach Tier" den Punkt "Anderes Tier / mehrere Tiere → Wunschbild". Unter "Produkte" die Punkte "Handyhüllen", "Taschen", "Baby & Kind", "Schreibtisch (Mauspad, Notizbuch)".

### P2

**B-19 · P2 · `snippets/product-card.liquid` Z. 75 und Z. 88 (`href="{{ product.url }}"`) [BELEGT]**
*Problem:* Ohne `| within: collection` führt der Link auf `/products/…`. `snippets/breadcrumbs.liquid` zeigt auf der Produktseite die Collection nur, wenn `collection` gesetzt ist. Der Breadcrumb "Start / Hundeportraits / …" fehlt also immer, und mobil gibt es keinen Rückweg in die Kategorie. (Canonical bleibt bei `within` korrekt.)
*Fix:* `{{ product.url | within: collection }}` an beiden Stellen. Der Parameter `?stil=` aus `theme.js` bleibt davon unberührt.

**B-20 · P2 · Badges: `snippets/product-card.liquid` Z. 26–33 [BELEGT]**
*Problem:* Die Badges hängen an den Tags `Bestseller` und `Neu`. Kein Live-Produkt hat einen davon. Die Collection `bestseller` existiert, wird aber nicht ausgewertet.
*Fix:* Den 4 Produkten aus `bestseller` den Tag `Bestseller` geben oder im Snippet `if product.collections contains bestseller_collection` prüfen (teurer, deshalb Tag bevorzugen). Neue Stile (Anime, Herzensbild) mit `Neu` taggen.

**B-21 · P2 · Sortieren verliert den Preisfilter: `sections/main-collection.liquid` Z. 42 [BELEGT]**
*Problem:* Die versteckten Felder übernehmen nur `f.active_values`. Ein Preisbereichsfilter hat keine `active_values` (er nutzt `min_value`/`max_value`), deshalb fällt er beim Umsortieren weg.
*Fix:* Im Sortierformular zusätzlich `if f.type == 'price_range'` → `min_value`/`max_value` als hidden inputs mitgeben.

**B-22 · P2 · Toolbar mobil nicht sticky: `theme.css` Z. 478–483 [VERMUTET]**
*Problem:* Bei 24 Produkten (12 Zeilen à 2 Karten) und langen Titeln muss man zum Filtern oder Sortieren weit nach oben scrollen.
*Fix:* Mobil `.toolbar{position:sticky;top:var(--h);z-index:5}` oder einen schwebenden "Filter & Sortieren"-Button unten.

**B-23 · P2 · Leer-Zustand der Collection: `sections/main-collection.liquid` Z. 102–104 [BELEGT]**
*Problem:* Der Text "Keine Produkte für diese Filter." und der Button "Alle zurücksetzen" führen auf `collection.url`. Ist die Collection selbst leer (ohne Filter), zeigt der Link auf sich selbst, eine Sackgasse. Aktuell ist keine veröffentlichte Collection leer. `futtermatten` (0) ist nicht veröffentlicht.
*Fix:* Unterscheiden: Wenn Filter aktiv sind → "Filter zurücksetzen". Sonst → Text "Hier kommt bald etwas" plus `featured-collection` `bestseller` plus Link zur Startseite.

**B-24 · P2 · Pagination: `snippets/pagination.liquid`, `per_page` 24 in `templates/collection.json` [BELEGT]**
*Problem:* Stil-Collections (59) und Weihnachten (60) haben 3 Seiten. Die Zahlen-Buttons sind 2,6 rem groß (ok). Das Umblättern lädt die Seite neu und springt nach oben. "Mehr laden" gibt es nicht.
*Fix:* Mobil einen "Weitere Produkte laden"-Button, der die nächste Seite per `fetch` anhängt. Die Links bleiben für SEO erhalten.

**B-25 · P2 · Suchergebnisseite mischt Typen: `sections/main-search.liquid` Z. 17–26 [BELEGT]**
*Problem:* Produkte, Artikel und Seiten landen im selben `product-grid--4`. Seiten-Karten haben kein Bild, deshalb gibt es ungleiche Zeilen. Der Zähler `search.results_count` zählt alles.
*Fix:* Die Ergebnisse gruppieren: zuerst Produkte (Raster), darunter "Artikel" und "Seiten" als Listen. Alternativ standardmäßig `type=product` setzen und Artikel über einen eigenen Tab anbieten.

**B-26 · P2 · Predictive Search: `sections/predictive-search.liquid` Z. 7 und `assets/theme.js` (Block "Predictive search") [BELEGT]**
*Problem:* (a) Der Preis erscheint ohne "ab" (`product.price` = Mindestpreis), obwohl die Varianten bis 208,99 € gehen. Die Karten zeigen "ab". Das ist uneinheitlich. (b) Ein Link "Alle Ergebnisse für „…“ anzeigen" fehlt, mobil bleibt nur die Enter-Taste der Tastatur. (c) Seiten (Futterrechner, FAQ, Sonderwunsch) werden nicht gesucht (`resources[type]=product,article,collection`). (d) Nach dem Schließen springt der Fokus nicht zum Such-Button zurück (`Drawers.close`).
*Fix:* (a) `if product.price_varies` → "ab " davorsetzen. (b) Am Ende `<a class="btn btn--ink btn--block" href="{{ routes.search_url }}?q={{ predictive_search.terms | url_encode }}">` einfügen. (c) `page` in `resources[type]` aufnehmen. (d) In `Drawers.open` das auslösende Element merken und in `close` den Fokus dorthin zurückgeben.

**B-27 · P2 · Such-Overlay mobil: `snippets/search-overlay.liquid` und `theme.css` Z. 241–256 [BELEGT, Verhalten VERMUTET]**
*Positiv [BELEGT]:* Der Fokus wird synchron im Tap-Handler gesetzt (Event `drawer:open` → `input.focus()`), die Tastatur sollte also auch auf iOS aufgehen. Die Schrift ist 1,4 rem (> 16 px), iOS zoomt deshalb nicht. Schließen geht über das X, den Hintergrund und Escape. Die Chips kommen aus `settings.search_suggestions`.
*Problem:* Das Panel hat `margin:6vh auto` und `max-height:85vh`. Bei offener Tastatur verdeckt die Tastatur den unteren Teil (vh schrumpft auf iOS nicht). [VERMUTET] Nach dem Tippen bleiben die "Beliebt"-Chips über den Ergebnissen stehen und schieben diese nach unten.
*Fix:* Mobil das Panel an den oberen Rand setzen (`margin:0; border-radius:0 0 var(--radius-card) var(--radius-card); max-height:100dvh`). `.search-overlay__suggestions` ausblenden, sobald `#PredictiveResults` Inhalt hat.

**B-28 · P2 · Filter-Drawer ohne zugänglichen Namen: `sections/main-collection.liquid` Z. 116 [BELEGT]**
*Fix:* `aria-labelledby` auf das `.drawer__title` mit einer `id` setzen.

**B-29 · P2 · `sections/main-list-collections.liquid` (/collections) [BELEGT]**
*Problem:* Die Seite zeigt ungefiltert alle rund 44 veröffentlichten Collections mit Produkten, in Shopify-Standardreihenfolge: auch `schaufenster`, `tierportraits` ("1 Produkte"), `alle-produkte`, `bestseller`, `mit-foto` und 21 Kacheln ohne Bild (Platzhalter). Die Seite ist nirgends verlinkt, aber per URL und Google erreichbar.
*Fix:* Nur eine kuratierte Liste (Setting "Collection-Liste" oder Blöcke) nach der Zielstruktur ausgeben, oder `/collections` per Weiterleitung auf eine Übersichtsseite lenken. Außerdem "1 Produkte" → Pluralform (`count` mit `one`/`other` in der Locale).

**B-30 · P2 · Doppelte bzw. verwirrende Collections [BELEGT]**
- `geschenke-einzug` hat dieselbe Regel wie `wandbilder` (typ:portrait UND Typ Wandbild), also identischen Inhalt. Als Anlass-Seite mit eigenem Text ist das ok. Es sollte aber manuell sortiert sein und in Zukunft auch Kissen oder Decken enthalten dürfen.
- `frontpage`, `schaufenster`, `bestseller`, `tierportraits`, `alle-produkte`: fünf "Sammel"-Collections, drei davon Altlasten. → `frontpage` und `schaufenster` bereinigen (B-01), `tierportraits` umbauen (B-04).
- `personalisierte-geschenke` (Tag `personalisiert`) gegen `mit-foto` gegen `ohne-foto` gegen `geschenkideen-lustige-motive` (manuell): Die Grenzen sind für Kunden nicht nachvollziehbar, und die Tag-Pflege ist uneinheitlich (`personalisiert` 44 × und `Personalisiert` 40 ×, `perso:foto` neben `personalisierung:mit-foto`).
- `kleidung-fuer-tierliebhaber` ist manuell, `t-shirts` und `hoodies` sind smart. Neue Textilien fehlen leicht in "Kleidung". → Smart machen (Tag `typ:kleidung` ODER Typen T-Shirt, Shirt, Hoodie, Sweatshirt, Sweatjacke, Babybody, Kinderbekleidung, Kinder-T-Shirt).
- `wohnen`: Der Text nennt Mauspads, die Collection enthält keine. `accessoires`: Der Text nennt Schürzen und Schlüsselanhänger, live gibt es keine. → Texte anpassen.
- `stil-herzensbild` hat die Regel "UND Typ Wandbild" und damit ohne Tassen (33), `stil-anime` und `stil-original` enthalten Tassen (36). → Vereinheitlichen.
- Die Stil-Titel sind uneinheitlich ("Klassische Ölgemälde" gegen "X-Portraits"). Die Marke im SEO-Titel wechselt zwischen "Print my Pet" und "PrintMyPet" (stil-herzensbild).
- `kalender` und `futtermatten` sind nicht veröffentlicht. `futtermatten` kann weg. `kalender` ist ok, weil das Menü das Produkt direkt verlinkt.

**B-31 · P2 · Menülinks als absolute HTTP-URLs [BELEGT]**
*Problem:* Alle `main-menu`-Einträge haben den Typ `HTTP` mit Domain. Ändert sich ein Handle, bricht der Link ohne Warnung. `link.active` bzw. die Markierung "aktueller Menüpunkt" hängt zudem an der URL-Gleichheit.
*Fix:* Die Einträge als Menü-Ressource (Collection, Produkt, Seite) neu anlegen.

**B-32 · P2 · Preis-Snippet, latentes Risiko: `snippets/price.liquid` Z. 12–13 [BELEGT, derzeit ohne Wirkung]**
*Problem:* Auf Karten wird `product.compare_at_price_max` mit `product.price_min` verglichen. Hätte nur die teuerste Variante einen Streichpreis, zeigte die Karte "ab 23,99 € ~~208,99 €~~", ein irreführender Rabatt (PAngV/UWG). Aktuell hat kein Live-Produkt einen Streichpreis.
*Fix:* Auf Karten den Streichpreis nur zeigen, wenn `product.compare_at_price_min > product.price_min` bzw. die günstigste Variante selbst reduziert ist.

**B-33 · P2 · SEO-Pflege der Collections [BELEGT]**
*Problem:* 15 Collections haben keinen eigenen SEO-Titel und keine SEO-Beschreibung (Liste in 1.1). 6 haben gar keine Beschreibung. `mit-foto` und `ohne-foto` stehen im Menü, haben aber weder Text noch SEO-Daten.
*Fix:* Je 1 Satz Intro (`custom.intro`), eine Beschreibung mit 150 bis 300 Wörtern (erscheint laut Section unter dem Raster) sowie SEO-Titel und -Beschreibung.

**Positiv (so lassen) [BELEGT]:** 2-Spalten-Raster mobil (`theme.css` Z. 499). Hover-Zweitbild und "Ansehen"-Overlay nur bei `(hover:hover)`, bei Touch ausgeblendet (Z. 130, 136, 138). Die ganze Karte ist per Tap klickbar (`theme.js` "Whole product card clickable"). Tilt nur bei feinem Zeiger. Nur die ersten 8 Karten animieren. Preis mit "ab" bei Preisspannen. Der Verfügbarkeitsfilter ist für Print-on-Demand bewusst ausgeblendet. Das Stil-Bild auf der Karte in `stil-*`-Collections kommt aus `pmp.stil_bilder`, für Wandbilder gepflegt (13 Bilder am Beispiel der Hunde-Leinwand), für Textilien nicht. Es gibt 189 Weiterleitungen für alte Produkt-URLs. Die Suchschrift ist groß genug, iOS zoomt nicht.

---

## 3. Findet man mobil in 2–3 Taps, was PrintMyPet bietet? (IST)

Tap 1 = Burger öffnen, Tap 2 = Oberpunkt aufklappen, Tap 3 = Link.

| Ziel | Weg heute | Taps | Ergebnis |
|---|---|---|---|
| Hund (alles) | Burger → Geschenke → Für Hundemenschen | 3 | ok, aber versteckt unter "Geschenke". "Wandbilder → Hundeportraits" zeigt nur 11 Wandbilder |
| Katze / Pferd | analog | 3 | wie Hund |
| Andere Tiere / mehrere Tiere | – | – | **nicht auffindbar** (Wunschbild in keinem Menü) |
| Leinwand oder Poster | Burger → Wandbilder → Alle | 3 | 33 gemischte Karten, Filter unklar (B-08) |
| Tasse | Burger → Für dein Zuhause → Tassen | 3 | Tassen erwartet man nicht unter "Zuhause" |
| Hoodie | Burger → Kleidung → Hoodies | 3 | ok |
| Handyhülle | nur über die Suche | 2 (Lupe, Chip) | nicht im Menü |
| Stil (13) | Burger → Stile → Stil | 3 | ok. "Alle Stile ansehen" führt aber zu Aquarell (B-05) |
| Geschenk allgemein | Burger → Geschenke → "Alle Geschenke ansehen" | 3 | **landet bei Weihnachten**, ohne Wandbilder (B-03, B-05) |
| Gedenken / Regenbogenbrücke | Burger → Geschenke → Erinnerung | 3 | ok (35 Wandbilder, einfühlsamer Text). Das Wort "Regenbogenbrücke" kommt nicht vor (B-17) |
| Geschenkgutschein | – | – | **nicht im Menü** |
| "Portrait gestalten" | Burger → roter Button | 2 | **zeigt Digital-Download + Kalender** (B-01) |

**Urteil [BELEGT/VERMUTET]:** Stile und Anlässe erreicht man in 3 Taps. Tierart und Produktart sind uneinheitlich einsortiert. Der wichtigste Weg ("Portrait gestalten") und die Sammel-Links ("Alle … ansehen") führen an die falsche Stelle.

---

## 4. Vorschlag: Katalog- und Navigationsstruktur

### 4.1 Collections (Soll)

**Einstieg und Sammlung**
- `portraits` (neu, oder das umgebaute `tierportraits`): smart, Tag `typ:portrait` ODER `typ:wunschbild`, manuell sortiert. **Ziel aller "Portrait gestalten"-CTAs**, falls keine eigene Landingpage kommt.
- `bestseller` (manuell, 8–12 Produkte, Mischung Hund/Katze/Pferd + Tasse + Kalender): Startseite, Cross-Sell, 404.
- `alle-produkte`: bleibt.
- Entfernen oder leeren: `schaufenster`, `frontpage` (oder auf smart umstellen), `futtermatten`. `tierportraits` → umbauen oder weiterleiten.

**Nach Tier** (smart über `tier:`, Wandbilder oben, manuell sortiert)
- `hund` = das heutige `fuer-hundemenschen` (Titel "Für Hundemenschen: Portraits & Geschenke"), `katze`, `pferd` analog. Die Handles können bleiben. Titel und Sortierung anpassen.
- Wandbild-Collections pro Tier: `hundeportraits`, `katzenportraits`, `pferdeportraits` (bleiben).
- "Anderes Tier / mehrere Tiere" → Produkt `wunschbild-von-deinem-tier` bzw. die Seite `/pages/sonderwunsch`.

**Nach Produkt** (smart über Produkttyp bzw. `produkt:`)
- Wandbilder: `wandbilder` (alle), optional Unter-Collections `leinwand` (Tag `produkt:leinwand` ODER `produkt:leinwand-gerahmt`), `poster` (`produkt:poster*`), `acryl-alu-holz` (`produkt:acrylglas`, `aluminiumdruck`, `holzdruck`, `hartschaumplatte`, `aufsteller`). Die Tags sind schon vorhanden [BELEGT].
- `tassen`, `kleidung` (smart machen) → `t-shirts`, `hoodies`, `baby-kind`
- `zuhause` (neu, smart: Kissen, Kuscheldecke, Handtuch, Weihnachtsschmuck, Futternapf, Mauspad)
- `accessoires` (Halstücher, Taschen, Handyhüllen, Notizbuch, Sticker, Flagge, Socken)
- `digital` (digitales Portrait), Kalender (Produkt), Gutschein (Produkt)

**Nach Stil**: die 13 `stil-*` bleiben, Regeln vereinheitlichen, als Übersicht die Startseite `#stile` oder eine neue Seite `/pages/stile`.

**Nach Anlass**: `geschenke-weihnachten`, `geschenke-geburtstag`, `geschenke-erinnerung` (Menü-Titel "In Erinnerung / Regenbogenbrücke"), `geschenke-einzug`, dazu `geschenkideen-lustige-motive` (Menü-Titel "Lustige Sprüche") und Gutschein. **Allen Wandbildern die Anlass-Tags geben** (B-03).

**Personalisierung**: `mit-foto` / `ohne-foto` (umbenennen in "Motive mit Namen") sind besser als **Filter** aufgehoben denn als Menü-Oberpunkt.

### 4.2 Hauptmenü (Soll, mobil in dieser Reihenfolge)

```
[Button] Portrait gestalten        -> /collections/portraits (oder Landingpage "Tier wählen")
1 Nach Tier                        -> (kein "Alle"-Link)
  ├ Hund                           -> fuer-hundemenschen
  ├ Katze                          -> fuer-katzenmenschen
  ├ Pferd                          -> fuer-pferdemenschen
  └ Anderes Tier / mehrere Tiere   -> /products/wunschbild-von-deinem-tier
2 Wandbilder                       -> wandbilder
  ├ Leinwand · Poster · Holzrahmen · Acryl, Alu & Holz
  ├ Hundeportraits · Katzenportraits · Pferdeportraits
  └ Digitales Portrait · Kalender 2027
3 Weitere Produkte                 -> alle-produkte
  ├ Tassen & Becher · Kleidung · Baby & Kind
  ├ Kissen & Decken · Futternäpfe
  └ Handyhüllen · Taschen · Halstücher · Accessoires
4 Stile                            -> /pages/stile (bzw. /#stile)
  └ 12 Stile + Original
5 Geschenke                        -> personalisierte-geschenke
  ├ Weihnachten · Geburtstag · Zum Einzug
  ├ In Erinnerung (Regenbogenbrücke)
  ├ Lustige Sprüche
  └ Geschenkgutschein
6 Wissen & Service                 -> /blogs/magazin
  └ Magazin · Futterrechner · Gratis-Startpaket · So funktioniert's · FAQ · Tierschutz · Über uns
```
Ergebnis: jedes Ziel in höchstens 3 Taps. Die Tierart steht als erste Wahl oben (der wichtigste Filter des Kunden). "Alle …"-Links zeigen immer auf eine echte Sammelseite.

### 4.3 Reihenfolge für morgen (Umsetzung)

1. **Admin-Daten, kein Code (P0):** `schaufenster` umbauen oder die CTAs umhängen (B-01). Tags `personalisierung:mit-foto`, `anlass:weihnachten` und `anlass:geburtstag` für die 35 Wandbilder und die Foto-Tassen setzen (B-02, B-03). Preisangabe im Geburtstagstext korrigieren. `tierportraits` umbauen (B-04). Cross-Sell und 404 auf `bestseller` (B-01, B-14). Sortierungen auf manuell (B-07).
2. **Menü (Admin):** neue Struktur nach 4.2, Links als Ressourcen (B-06, B-18, B-31). `menu_title` der Mega-Promos korrigieren (B-15). `ohne-foto` umbenennen (B-16).
3. **Theme-Code:** Intro-Fix (B-10), Filter-Drawer mit "Anwenden" (B-08), "Alle …"-Link-Logik (B-05), Bildformat-Konflikt (B-09), Kartentitel/Eyebrow (B-12, B-13), `within: collection` (B-19), 404 und Such-Leerzustand (B-14, B-17), Predictive-Search-Link (B-26).
4. **Prüfen, was hier nicht ging [NICHT PRÜFBAR]:** Filterkonfiguration in Search & Discovery. Echte Darstellung bei 360/390/430 px (Kartenhöhen, Hero-Höhe, Such-Overlay mit Tastatur). Reihenfolge der Bestseller-Sortierung im Storefront. Tatsächliche Suchtreffer für "Leinwand", "Regenbogenbrücke", "Gutschein".
