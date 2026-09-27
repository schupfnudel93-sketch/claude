# 05 – Inhaltsseiten, Blog, Formulare, Rechtstexte (printmypet.de)

Stand: 27.09.2026 · Live-Theme `gid://shopify/OnlineStoreTheme/208344613202` („PMP: Futterrechner v5.1 (FREIGABE)“) · Quelle: Shopify-Admin-GraphQL (nur gelesen, nichts geändert). Die Website selbst war nicht erreichbar. Alle Aussagen zum Rendering stammen deshalb aus Code und Daten, nicht aus einem Browsertest.

**Kennzeichnung:** **[BELEGT]** = direkt in Code oder Daten nachgewiesen (Datei, Zeile oder Query). **[VERMUTET]** = aus dem Code abgeleitet, aber nicht im Browser geprüft, oder rechtliche bzw. fachliche Einschätzung. Vor dem Fix bitte gegenprüfen.

Lokale Kopien aller gelesenen Theme-Dateien liegen unter `scratchpad/theme/…`. Die Zeilennummern unten beziehen sich auf diese Kopien (= Live-Inhalt, ohne den 9-zeiligen Auto-Header der JSON-Templates).

---

## 1. IST-Zustand

### 1.1 Shopify-Seiten (alle 12, alle veröffentlicht)

| Handle | Titel | Template | Veröffentlicht | Zweck | Body wird angezeigt? |
|---|---|---|---|---|---|
| contact | Kontakt | page.contact | ja (10.07.) | Kontaktformular + 2 FAQ | Body leer, Sections rendern alles |
| ueber-uns | Über uns | page.ueber-uns | ja | Story, Tierschutz, Team, YouTube | **nein** (Template ohne `main-page`; Body nur Meta-Fallback) |
| so-funktionierts | So funktioniert's | page.so-funktionierts | ja | 3 Schritte, Vorher/Nachher, Qualität, FAQ | **nein** |
| tierschutz | Wohin ein Teil unseres Gewinns geht | page.tierschutz | ja | 20 % vom Gewinn, Voting-Ablauf, YouTube, FAQ | **nein** |
| sonderwunsch | Sonderwunsch: Wir drucken jedes Motiv | page.sonderwunsch | ja | Anfrageformular + Ablauf | **nein** |
| youtube | YouTube: Hundewissen, das wirklich hilft | page.youtube | ja | Video-Hub, Blog, Newsletter | **nein** |
| buecher | Unsere Bücher | page.buecher | ja | 10 Amazon-Bücher, Blog | **nein** (Body enthält eine ältere Inline-HTML-Version mit nur 2 Ratgebern und 6 Malbüchern) |
| faq | Häufige Fragen | page.faq | ja | 8 FAQ + Sonderwunsch-Formular | **nein** |
| welpen-startpaket | Welpen-Startpaket | page.welpen-startpaket | ja | Lead-Magnet (Brevo DOI) | **nein** (Body enthält einen alten Brevo-iframe) |
| danke-welpen-startpaket | Fast geschafft | page (Standard) | ja, noindex | Danke-Seite nach Anmeldung | ja (`main-page`) |
| startpaket-download | Dein Startpaket | page (Standard) | ja, noindex | PDF-Download nach DOI | ja |
| futterrechner | Futterrechner für Hunde: Wie viel Futter braucht mein Hund? | page.futterrechner | ja (26.09.) | Rechner, Mail-Plan, Buch-CTA | nein (Intro kommt aus der Section-Einstellung) |

**Fehlend als Seite:** Impressum, Datenschutz, AGB, Widerruf und Versand. Das ist in Ordnung, sie existieren als **Shop-Policies** (siehe 1.3) und werden im Footer über `shop.policies` verlinkt (`sections/footer.liquid` Z. 54–60) **[BELEGT]**.

### 1.2 Blogs

| Blog | Artikel | Bemerkung |
|---|---|---|
| magazin | 57 (54 veröffentlicht, 3 geplant: `futtermenge-hund-berechnen` 28.09., `hund-zu-dick-figur-idealgewicht` 01.10., `hundefutter-mit-reis-kartoffeln-strecken` 05.10.) | Alle 57 mit Beitragsbild und Alt-Text. Kein Artikel nutzt das Metafeld `custom.faq` oder `custom.related_products`. |
| news | 0 | Leerer Blog, `/blogs/news` zeigt „Bald gibt es hier Lesestoff.“ |

- 52 von 54 veröffentlichten Artikeln haben Shop-Links im Text. Ohne direkten Shop-Link: `hund-allein-lassen-lernen`, `welpen-checkliste-erste-3-tage`. Beide erhalten aber über `main-article` den Produktblock **[BELEGT]**.
- 36 Artikel verlinken Amazon mit `?tag=printmypet-21` und tragen den Hinweis „Die Links zu unseren Büchern sind Partnerlinks“ **[BELEGT]**.
- 1 Artikel bettet YouTube direkt per iframe ein (`welpenerziehung-5-regeln`, youtube-nocookie, **ohne Zwei-Klick-Lösung**) **[BELEGT]**.
- Viele Artikel verlinken alte, **archivierte** Produkt-Handles (`hundeportrait-poster` 9×, `hundeportrait-poster-holzrahmen` 9×, `hundeportrait-acrylglas` 8×, `hundeportrait-leinwand` 7×, Katzen- und Pferde-Varianten). Für alle gibt es URL-Redirects auf die neuen Produkte, es entsteht also **kein 404**, aber ein Redirect-Hop **[BELEGT]**.

### 1.3 Shop-Policies

| Typ | Titel | Sprache/Qualität |
|---|---|---|
| CONTACT_INFORMATION | Kontakt | DE, Creator Advisory SL, Valencia, NIF B26838318 |
| LEGAL_NOTICE | Impressum | DE, „§ 5 DDG“, nur Firma, Adresse, E-Mail und NIF (230 Zeichen) |
| PRIVACY_POLICY | Datenschutzerklärung | DE, **Shopify-Standardvorlage** (Stand 23.09.2026) |
| REFUND_POLICY | Widerrufsrecht | DE, individuell, gut |
| SHIPPING_POLICY | Versand | DE, individuell, aber **ohne Zeiten und Preise** |
| TERMS_OF_SERVICE | AGB | **ENGLISCH, Shopify-US-Vorlage** |

### 1.4 Relevante Theme-Einstellungen (`config/settings_data.json`)

`adsense_client_id = ca-pub-9257305213000088`, `adsense_blog_only = true`, `youtube_api_key = ""` (wird im Theme **nirgends** verwendet), `amazon_tag = ""`, `brevo_mode = form`, `donation_percent = 20`, `cart_free_shipping_threshold = 0`, `ga4_measurement_id = ""`.

### 1.5 Zentrale Fakten im Vergleich (Konsistenz)

| Fakt | Wo | Aussage |
|---|---|---|
| Spende | überall (Announcement, Footer, Über uns, Tierschutz, FAQ, Bücher, Startpaket) | 20 % vom **Gewinn**, einheitlich ✔ |
| Lieferzeit | Announcement-Bar, so-funktionierts (Schritt 3), FAQ f3, Meta-Description FAQ | 5 bis 9 Werktage ✔ · Seiten-Body so-funktionierts (nicht gerendert): 3 bis 7 · Versand-Policy: **keine Angabe** |
| Versandkosten | FAQ f5 | „DE 5,99 €, ab 150 € frei, EU 13,99 €, Schweiz und weltweit 19,99 €, gerahmte Poster 5,69–10,99 €“ → **passt nicht zu den Versandprofilen** (siehe P0-4) |
| Liefergebiet | Versand-Policy | „DE, AT und CH“ ↔ Versandzonen: ganze EU plus US, JP, AU, GB, CA usw. ↔ FAQ „weltweit“ |
| Firmensitz/Druck | Policies: Valencia (ES) · Theme: „Druck in Europa“, „Werkstätten in der EU … keine Zollüberraschungen“ | Versand-Policy: „Fertigung im Lieferland nicht bei jeder Bestellung möglich“, CH „Einfuhrabgaben können anfallen“ |
| Anzahl Stile | so-funktionierts „13 Stile“, Locale `step2_text` „13 Stile“, Hauptmenü 13 Stil-Kollektionen | `settings.art_styles` nennt 11 · Vorher/Nachher-Galerie zeigt 7 |
| Vorschau-Prozess | FAQ/Kontakt/so-funktionierts: **Live-Vorschau auf der Produktseite** | Widerrufs-Policy und Meta so-funktionierts: **„Vorschau per E-Mail vor dem Druck, gedruckt wird erst nach deinem Ja“** |
| Kissen/Decken | so-funktionierts Schritt 3, FAQ f8, Sonderwunsch-Dropdown: „kommen bald“ | Kollektion `wohnen` hat **aktive** Kissen, Kuscheldecken und Sherpa-Decken |
| Antwortzeit | Kontakt „meist innerhalb von 24 Stunden“, Sonderwunsch „24 Stunden“ | FAQ-Section-Default „meist innerhalb weniger Stunden“ (auf so-funktionierts und faq sichtbar) · Meta Kontakt „innerhalb weniger Stunden“ |

---

## 2. Befunde

### P0: kaputt, direkter Umsatzverlust oder Rechtsrisiko

#### P0-1 · Mobile: Bücher ab Nr. 4 und FAQ ab Nr. 6 sind unsichtbar (globale CSS-Regel für die Startseite greift überall) **[BELEGT]**
- **Datei:** `assets/theme.css`, Block „Mobile diet: shorter landing page on phones“:
  ```css
  @media (max-width:749px){
    .books>:nth-child(n+4),.testimonials>:nth-child(n+4),.featured-collection .product-grid>:nth-child(n+5),.faq .accordion>:nth-child(n+6){display:none}
  ```
- **Wirkung:**
  - `/pages/buecher`: mobil sind nur **3 von 10** Büchern sichtbar (b10 Hundeernährung, b1, b9). „Welcher Hund passt wirklich zu dir?“ und alle 6 Malbücher fehlen. Die Filter-Chips „Katze“, „Pferd“ und „Kinder“ zeigen mobil **nichts**, weil die Treffer auf Position 5 bis 10 liegen und das CSS sie zusätzlich ausblendet. Das kostet direkt Amazon-Umsatz.
  - `/blogs/magazin` (Section `books` in `templates/blog.json`): 3 Bücher, die Regel greift dort zufällig nicht.
  - `/pages/faq`: mobil fehlen **f6 „Kann ich zurückgeben?“, f7 „Tierschutz“, f8 „Welche Produkte gibt es?“**. Das FAQPage-JSON-LD enthält sie trotzdem, es gibt also Schema-Inhalte, die mobil nicht sichtbar sind.
- **Fix:** Regel auf die Startseite begrenzen: `.template-index .books>…, .template-index .faq .accordion>…` (die Body-Klasse `template-index` existiert, `layout/theme.liquid` Z. 124). Alternativ die Regel für `.books` und `.faq` ganz entfernen.

#### P0-2 · AGB sind eine englische US-Shopify-Vorlage **[BELEGT]** / Rechtsfolgen **[VERMUTET, anwaltlich prüfen]**
- **Ort:** Shop-Policy `TERMS_OF_SERVICE` (23.510 Zeichen, „OVERVIEW Welcome to Printmypet! …“, „Section 22 – Governing law … federal and state or territorial courts“, „title and risk of loss passes to you“ bei Übergabe an den Frachtführer, „age of majority in your state or province“).
- **Problem:** Das ist ein deutschsprachiger B2C-Shop. Englische AGB mit US-Klauseln sind für deutsche Verbraucher in weiten Teilen unwirksam und abmahnfähig. Der Gefahrübergang bei Übergabe an den Frachtführer widerspricht § 475 Abs. 2 BGB. Die AGB beschreiben weder den Vertragsschluss auf Deutsch noch die Vorschau- und Freigabelogik, und sie passen nicht zu Widerruf und Versand.
- **Fix:** Deutsche AGB (Anbieter Creator Advisory S.L., Verbraucher DE/AT, Vertragsschluss, Personalisierung/Freigabe, Lieferzeiten, Gewährleistung, Rechtswahl mit Verbraucherschutz-Vorbehalt), z. B. über Händlerbund, IT-Recht-Kanzlei oder Trusted Shops. Alte Vorlage ersetzen.

#### P0-3 · Datenschutzerklärung ist die Shopify-Standardvorlage, eingesetzte Dienste fehlen **[BELEGT]**
- **Ort:** Policy `PRIVACY_POLICY`. Wortsuche: YouTube 0, AdSense/Google 0, Brevo 0, Newsletter 0, Amazon 0, Gelato 0, Speicherdauer 0.
- **Tatsächlich im Einsatz** (laut Theme): Google AdSense (`layout/theme.liquid` Z. 22–57), YouTube-nocookie-Embeds (`assets/youtube-hub.js`, Artikel `welpenerziehung-5-regeln`), Brevo-Formulare mit DOI (Newsletter, Startpaket, Futterrechner mit ca. 90 Datenfeldern inkl. Hundename und Gesundheitsangaben), Shopify-Kontaktformular, Judge.me, teeinblue, Gelato (Metafelder vorhanden).
- **Fix:** DSE je Dienst ergänzen (Zweck, Rechtsgrundlage, Empfänger, Drittland, Speicherdauer), Verantwortlicher Creator Advisory S.L., Aufsichtsbehörde (AEPD bzw. Hinweis auf Beschwerderecht), Newsletter- und DOI-Protokollierung, Futterrechner-Mail. Generator verwenden oder anwaltlich prüfen lassen.

#### P0-4 · Versandkosten- und Liefergebiet-Angaben widersprechen sich und stimmen nicht mit dem Checkout überein **[BELEGT]**
- **FAQ** `templates/page.faq.json` → `sections.faq.blocks.f5.settings.answer`: „Deutschland: 5,99 €, ab 150 € versandkostenfrei. EU: 13,99 €. Schweiz und weltweit: 19,99 €. Für einige gerahmte Poster … 5,69 bis 10,99 €“.
- > **KORREKTUR (Gegenprüfung 27.09. per `deliveryProfiles`):** Alle 19 Gelato-Versandprofile enthalten **0 Varianten**. Für alle Produkte gilt das Standardprofil (DE 5,99 € · gratis ab 150 € · Express 9,99 € · EU 13,99 € · International 19,99 €). Die folgende Aussage zu eigenen Gelato-Preisen für Wandbilder ist damit **falsch**. Richtiger Fix: den Satz zu 5,69–10,99 € aus FAQ f5 und `product.json` acc2 streichen.
- **Versandprofile (Admin):** Das Standardprofil entspricht dem (5,99 €, 0 € ab 150 €, Express 9,99 €, EU 13,99 €, International 19,99 €). Die **Wandbilder** laufen aber über 9 Gelato-Profile ohne Gratis-Schwelle: DE 5,69 / 8,89 / 6,99 / 10,99 / 11,99 / 5,69 / 10,19 / 11,19 / 8,39 €. Also **nicht nur gerahmte Poster**, **nicht maximal 10,99 €**, und **„ab 150 € versandkostenfrei“ gilt für das Hauptprodukt nicht**. CH kostet dort bis 52,85 €.
- **Versand-Policy:** „Wir liefern derzeit nach Deutschland, Österreich und in die Schweiz“ ↔ Versandzonen enthalten die ganze EU plus US, JP, AU, GB, CA, SG, NZ, NO usw.
- **Risiko:** Irreführende Preisangaben (PAngV/UWG) und Kundenärger im Checkout, der zu Kaufabbrüchen führt.
- **Fix:** FAQ f5 auf korrekte Aussagen reduzieren („Die Versandkosten hängen vom Produkt ab, zum Beispiel Poster ab 5,69 €, Leinwand ab 5,69 €, gerahmte Leinwand ab 8,39 €. Du siehst sie vor dem Bezahlen im Checkout.“, Gratis-Schwelle nur mit Einschränkung nennen). Versand-Policy um Liefergebiete, Lieferzeiten und Kostentabelle ergänzen. Liefergebiet zwischen Policy und Versandzonen angleichen, also entweder die Zonen einschränken oder die Policy erweitern.

#### P0-5 · Amazon-Affiliate: widersprüchliche Kennzeichnung und verlorene Provisionen **[BELEGT]**
- `settings.amazon_tag = ""`, deshalb hängt `sections/amazon-books.liquid` Z. 22–28 **keinen** Tag an. Auf `/pages/buecher` tragen nur 1 von 10 Links einen Tag (b10, hart codiert `?tag=printmypet-21`). **9 Buch-Links ohne Tag**, dazu die 3 Bücher auf `/blogs/magazin`.
- Die Blog-Übersicht (`templates/blog.json` → Section `books` ohne `affiliate_note`) zeigt deshalb den Locale-Default `sections.books.affiliate_note`: **„Wir sind kein Amazon-Partner und erhalten keine Provision.“** Gleichzeitig tragen 36 Artikel, der Futterrechner und die Bücherseite `tag=printmypet-21` und den Hinweis „Partnerlinks“. Auf derselben Domain stehen damit zwei gegensätzliche Aussagen. Das Amazon-Pflichtstatement („Als Amazon-Partner verdiene ich an qualifizierten Verkäufen“) fehlt überall **[VERMUTET: Pflicht laut Amazon-PartnerNet-Richtlinien, bitte prüfen]**.
- **Fix:** `settings.amazon_tag = printmypet-21` setzen, falls das der echte Tag ist. Locale-Default `sections.books.affiliate_note` (de.default.json) auf den Partnerlink-Text plus Amazon-Pflichtsatz ändern. In `page.buecher.json` und `blog.json` bei allen Blöcken `"sponsored": true` setzen. Zusätzlich den Amazon-Satz in Artikeln und im Futterrechner ergänzen.

#### P0-6 · So funktioniert's: Material-Leiste unter Vorher/Nachher ist leer, alle 15 verlinkten Produkte sind archiviert **[BELEGT: Status]** / leeres Rendering **[VERMUTET, hohe Sicherheit]**
- **Datei:** `templates/page.so-funktionierts.json` → `sections.transformation.blocks.mat-*` (15 Blöcke: `hundeportrait-poster`, `…-poster-holzrahmen`, `…-leinwand`, `…-leinwand-gerahmt`, `…-acrylglas`, dasselbe für katzen- und pferdeportrait). **Alle 15 sind ARCHIVED.**
- `sections/transformation-showcase.liquid`: Wenn Material-Blöcke existieren, wird **nur** die Block-Liste gerendert (`{%- if materials.size > 0 -%}`). Archivierte Produkte liefern im Produkt-Setting `blank`, dann wird nichts ausgegeben. Der eingebaute Fallback über die Kollektion (Tag `produkt:<key>`, zeigt nie archivierte Produkte) wird dadurch **blockiert**.
- **Wirkung:** Die wichtigste Brücke „Stil gefallen → Produkt anklicken“ fehlt auf der Erklärseite.
- **Fix:** Die 15 `mat-*`-Blöcke samt Einträgen in `block_order` löschen. Dann greift automatisch `species_collections` (Default `Hund:hundeportraits,Katze:katzenportraits,Pferd:pferdeportraits`), und aktive Produkte mit `produkt:*`-Tags sind vorhanden (für Hund geprüft: 11 aktive).

#### P0-7 · Impressum unvollständig **[BELEGT: Inhalt]** / Pflichtangaben **[VERMUTET, anwaltlich prüfen]**
- **Ort:** Policy `LEGAL_NOTICE`: nur „Creator Advisory S.L., Adresse, E-Mail, Website, NIF“.
- **Es fehlen:** Vertretungsberechtigte Person (Administrador/Geschäftsführer), Registereintrag (Registro Mercantil de Valencia, Tomo/Folio/Hoja), USt-IdNr. im EU-Format (ES B26838318, falls VIES-registriert) und ggf. ein zweiter schneller Kontaktweg. Die Firmierung ist uneinheitlich: „Creator Advisory SL“ (Widerruf, Kontakt) ↔ „Creator Advisory S.L.“ (Impressum). Auf Über uns stehen im nicht gerenderten Body „Tobias & Alexander, Gründer“, im Impressum niemand.
- **Fix:** Impressum ergänzen und die Firmierung vereinheitlichen.

---

### P1: deutlicher Conversion-, Vertrauens- oder Rechtsnachteil

#### P1-1 · Inhaltsseiten ohne Weg in den Shop (keine Produkt- oder Kollektions-CTA) **[BELEGT]**
`grep collections|/products/` ergibt 0 Treffer in `futterrechner.liquid`, `youtube-hub.liquid`, `amazon-books.liquid`, `donation-story.liquid` und `pmp-freebie-landing.liquid`.
| Seite | CTAs heute | Fehlt |
|---|---|---|
| `/pages/futterrechner` (neu, SEO-Traffic) | nur Buch (Amazon), Mail-Plan | **Kein Hinweis auf Hundeportraits**, z. B. unter dem Ergebnis: „Dein Hund als Kunstwerk“ → `/collections/hundeportraits` |
| `/pages/tierschutz` | „So läuft die Abstimmung“ (#voting, kaputt, P1-6), YouTube | Shop-CTA („Mit jedem Portrait unterstützt du …“) |
| `/pages/ueber-uns` | nur „Mehr zum Tierschutz“ | `rich-text.cta` / `image-with-text.cta` sind leer. Button „Portrait gestalten“ → `/collections/hundeportraits` setzen |
| `/pages/youtube` | Abonnieren, Blog, Newsletter | Shop-CTA |
| `/pages/buecher` | Amazon | Portrait-Teaser |
| `/pages/welpen-startpaket` | Formular | ok (Lead-Seite). Die Danke- und Download-Seite verlinkt `/collections/tierportraits` ✔ |
- **Fix:** Pro Template eine `rich-text`- oder `image-with-text`-Section mit `cta`/`cta_url` → `shopify://collections/hundeportraits` (bzw. `mit-foto`) ergänzen. Im Futterrechner einen kleinen Block nach `data-fr-result` einfügen.

#### P1-2 · Fly-in „Welpen-Startpaket“ erscheint auch im Shop und auf Formularseiten **[BELEGT]**
- **Datei:** `snippets/pmp-freebie-flyin.liquid` Z. 7–33. Ausgeblendet wird nur auf product, cart, search, customers, Policies, Startpaket-, Danke-, Download- und Kontaktseite, auf Kollektionen mit „katze“/„pferd“ im Handle und auf Katzen- und Pferdeartikeln.
- **Es erscheint also auf** `/collections/hundeportraits`, `/collections/wandbilder`, allen `stil-*`- und Geschenk-Kollektionen, der Startseite, `/pages/sonderwunsch`, `/pages/faq`, `/pages/futterrechner` und `/pages/so-funktionierts`. Käufer werden mitten in der Kaufphase zu einem kostenlosen PDF weggelenkt.
- **Mobil:** Die Leiste ist fixiert über die volle Breite unten (`.pmp-flyin{right:.75rem;left:.75rem;bottom:.75rem}` in `theme.css`, Freebie-Block) und überdeckt den unteren Bildschirmteil, auf dem Futterrechner vermutlich Eingabefelder bzw. den Button „Futtermenge berechnen“ **[VERMUTET]**. Schließen-Button: `font-size:1.4rem; padding:.25rem .55rem`, also etwa 32×34 px und damit unter 44 px **[VERMUTET, berechnet]**. Esc und Frequency-Cap (Sitzung, 14 bzw. 90 Tage) sind vorhanden ✔.
- **Fix:** Nur auf `template.name == 'article'` (Hund/Welpe) und `blog` zeigen, **nie** auf `collection`, `index` oder `page` außer ausgewählten Seiten. Schließen-Button auf min. 44×44 px. Optional mobil erst nach 60 % Scrolltiefe.

#### P1-3 · Garantie- und Prozessversprechen widersprechen sich (Widerruf, Vorschau, Neudruck) **[BELEGT: Texte]** / Rechtsfolge **[VERMUTET]**
- `page.so-funktionierts.json` → `faq.blocks.f2`: „… gilt unsere **Zufriedenheitsgarantie: Wir drucken neu**.“
- `page.faq.json` → `f6`: „Bei Druckfehlern oder Transportschäden drucken wir kostenlos neu.“ (Also nur bei Mängeln.)
- Widerrufs-Policy: „Ersatz bei Mängeln … Vorschau **per E-Mail**, bevor irgendetwas produziert wird … Änderungen kostenlos, ohne Begrenzung auf eine Runde.“
- FAQ f2 und Kontakt f2: „Vorschau entsteht **live auf der Produktseite** … Beim Wunschbild per E-Mail.“
- Meta-Description so-funktionierts: „kostenlose Vorschau per E-Mail“.
- **Risiko:** Eine „Garantie“ ohne Bedingungen ist nach § 479 BGB auslegungsfähig zugunsten des Kunden. Die Widerrufs-Policy beschreibt einen anderen Ablauf als die Seiten, und der Ausschluss des Widerrufs wird mit genau diesem Ablauf begründet.
- **Fix:** Eine einheitliche Formulierung festlegen (Live-Vorschau vor dem Kauf, bei Wunschbildern E-Mail-Freigabe; Neudruck bei Druckfehlern oder Transportschäden) und in so-funktionierts f2, FAQ f2/f6, Kontakt f2, Widerrufs-Policy („Was wir freiwillig zusagen“) und Meta-Description angleichen. Das Wort „Zufriedenheitsgarantie“ streichen oder mit Bedingungen versehen.

#### P1-4 · „Kommt bald“ für Produkte, die schon im Shop sind **[BELEGT]**
- so-funktionierts `how.blocks.h3.text`: „Webdecke & Kissen kommen bald“.
- FAQ `f8`: „Webdecken, Kissen und Schlüsselanhänger kommen bald.“
- `sections/custom-request.liquid` Schema-Default `products`: „Webdecke (kommt bald), Kissen (kommt bald)“ (in beiden Templates nicht überschrieben, also live).
- Kollektion `wohnen`: 14 aktive Produkte (Kissen, Kissenbezug, Kuscheldecke, Sherpa-Decke …).
- **Fix:** Texte aktualisieren, „Kissen & Decken“ mit Link `/collections/wohnen` nennen. Im Dropdown die Optionen „Kissen“ und „Decke“ ohne „kommt bald“ aufnehmen (Setting `products` in beiden Templates setzen).

#### P1-5 · Qualitätsblock auf So funktioniert's zeigt ungeprüfte Default-Behauptungen **[BELEGT: Default]** / Wahrheitsgehalt **[VERMUTET]**
- `page.so-funktionierts.json` → `quality` (image-with-text) hat **keine** Texteinstellungen, deshalb rendert der Schema-Default aus `sections/image-with-text.liquid`: „Museumsleinwand mit 12 Farben, Acrylglas mit 5 mm Stärke, **Poster aus 100 % Baumwolle**. **Kein Sublimationsdruck**, der nach einem Jahr verblasst.“
- Poster von Gelato sind Papier. Tassen und Stoffprodukte werden bei POD üblicherweise im Sublimations- oder DTG-Verfahren gedruckt. Das Risiko einer irreführenden Produktangabe (UWG) ist hoch.
- **Fix:** Eigene, belegbare Texte in `quality.settings.heading/text` setzen (Materialangaben aus den Gelato-Produktdaten). Button `cta` → Wandbilder.

#### P1-6 · Tierschutz: toter Anker, Zeitplan widersprüchlich, Voting-Sektion zeigt fremde Videos **[BELEGT]**
- `page.tierschutz.json` → `story.settings.cta_url = "#voting"`, im ganzen Theme gibt es aber kein `id="voting"` (grep). Der Button bewirkt nichts. **Fix:** in `donation-story.liquid` Z. 29 `<div id="voting" …>` ergänzen oder `cta_url` auf `#tierschutz` bzw. die Voting-Zwischenüberschrift setzen.
- Zeitplan: Text „**Im Oktober 2027** stellen wir … Projekte vor. Ihr stimmt ab“ ↔ Schritt v1 „**Ab Oktober 2027 sammeln wir Vorschläge**“ ↔ v3 „**Im November 2027** läuft das Voting“ ↔ FAQ f2 „Jeden Oktober rufen wir … auf“. **Fix:** eine Chronologie festlegen (Okt. Vorschläge → Nov. Voting → Dez. Auszahlung) und überall gleich schreiben.
- YouTube-Hub auf Tierschutz: Überschrift „Voting auf YouTube / Hier findet die Abstimmung statt“. Die 3 kuratierten Blöcke werden **ignoriert**, weil die Metafelder `pmp.youtube_popular` und `pmp.youtube_latest` befüllt sind (`youtube-hub.liquid` Z. 10–19, 51–76). Angezeigt werden Rasse-Videos (Berner, Schäferhund, Dobermann …). **Fix:** eine Einstellung „Metafelder ignorieren“ ergänzen oder den Hub auf dieser Seite durch einen schlichten Kanal-/Abo-CTA ersetzen.
- Newsletter-Perk `newsletter.perk_3` „**Stimmrecht beim Tierschutz-Voting**“ ↔ Tierschutzseite „Voting live auf YouTube, jede Stimme zählt gleich“. Das suggeriert, dass man ohne Newsletter kein Stimmrecht hat. **Fix:** in „Erinnerung zum Tierschutz-Voting“ umformulieren.
- **[VERMUTET, rechtlich]** Die Werbung mit „20 % vom Gewinn“ sollte die Bemessungsgrundlage nennen (z. B. „Jahresüberschuss vor Steuern der Creator Advisory S.L.“), sonst ist das Versprechen nicht nachprüfbar (UWG, Transparenz bei Spendenwerbung).

#### P1-7 · „Über uns“: Aussagen widersprechen der Versand-Policy **[BELEGT]**
- `page.ueber-uns.json` → `team.settings.text`: „Jede Bestellung wird vor dem Druck geprüft. Unsere Werkstätten stehen in der EU: kurze Wege, faire Löhne, **keine Zollüberraschungen**.“
- Versand-Policy: „eine Fertigung im Lieferland ist nicht bei jeder Bestellung möglich … Lieferungen in die Schweiz: Einfuhrabgaben … können anfallen“. Bei aktiven Versandzonen US, JP, AU usw. produziert Gelato in der Regel lokal, also nicht in der EU **[VERMUTET]**.
- „Premium-Druck aus Europa“ (so-funktionierts h3, Announcement-Bar „Druck in Europa“) gilt dann nur für EU-Kunden.
- **Fix:** „Für Kunden in Deutschland und Österreich produzieren wir in der EU“ bzw. „keine Zollüberraschungen innerhalb der EU“. „Jede Bestellung wird vor dem Druck geprüft“ nur stehen lassen, wenn es tatsächlich passiert.

#### P1-8 · YouTube-Einbettung im Artikel ohne Einwilligung **[BELEGT]** / DSGVO **[VERMUTET]**
- Artikel `welpenerziehung-5-regeln`: `<iframe src="https://www.youtube-nocookie.com/embed/wI5EeP7ZnDQ" … loading="lazy">`. Das lädt beim Scrollen ohne Klick Google-Ressourcen (IP-Übertragung). Der Theme-Hub macht es richtig (Zwei-Klick, `assets/youtube-hub.js` Z. 155–160).
- **Fix:** iframe durch ein Vorschaubild mit „Video laden“ ersetzen (dasselbe Markup wie im Hub, z. B. `<div data-youtube-hub>` mit `<template data-yt-manual>`) oder einen Shortcode-Snippet im `main-article` ergänzen.
- Nebenbei (P2): Die YouTube-Einwilligung wird dauerhaft in `localStorage` gespeichert (`pmp_yt_consent`), ohne Widerrufsmöglichkeit.

#### P1-9 · AdSense: Einbindung korrekt begrenzt, aber voraussichtlich ohne Ertrag im EWR **[BELEGT: Code]** / Ertrag **[VERMUTET]**
- ✔ `layout/theme.liquid` Z. 22–57: Das Script lädt nur bei `request.page_type == 'article'` oder `'blog'` und erst nach `Shopify.customerPrivacy.marketingAllowed()`. **Nicht auf Produktseiten, nicht auf Inhaltsseiten.** Es gibt keine manuellen `<ins class="adsbygoogle">`-Slots, also laufen, wenn überhaupt, Auto-Ads.
- ✗ Google verlangt seit 16.01.2024 für personalisierte **und** nicht personalisierte Anzeigen im EWR/UK eine **IAB-TCF-v2.2-zertifizierte CMP**. Das Shopify-Cookie-Banner ist keine solche CMP, daher werden im EWR voraussichtlich nur eingeschränkte oder keine Anzeigen ausgeliefert.
- ✗ Die Datenschutzerklärung nennt AdSense nicht (siehe P0-3).
- ✗ Auto-Ads (Anker unten, Vignette) konkurrieren mobil mit dem Fly-in, dem Produktblock und der Seiten-CTA und lenken vom Shop weg **[VERMUTET]**.
- `/ads.txt` ist per Redirect auf `cdn.shopify.com/…/ads.txt` umgeleitet (URL-Redirect vorhanden). Laut IAB-Spezifikation sind Redirects außerhalb der Root-Domain nicht autoritativ **[VERMUTET: Google akzeptiert es in der Praxis oft; im AdSense-Konto prüfen]**.
- **Entscheidung nötig:** Entweder eine TCF-CMP einführen (z. B. Consentmo oder Pandectes mit TCF), in die DSE aufnehmen und auf mobile Anker-Ads verzichten, oder AdSense abschalten (`adsense_client_id` leeren), weil der Blog vor allem Portrait-Käufer bringen soll.

#### P1-10 · Kontakt- und Sonderwunsch-Formular: kein Foto-Upload, Versprechen ohne Grundlage **[BELEGT]**
- `sections/custom-request.liquid`: Shopify-`contact`-Form, Pflichtfelder Name, E-Mail, Idee und Datenschutz-Checkbox, Erfolgsmeldung `sections.request.success` ✔, Fehlerausgabe ✔.
- Es gibt **keinen Datei-Upload**. Der Hinweis lautet „Fotos kannst du direkt nach dem Absenden per E-Mail anhängen. **Wir melden uns mit deiner Anfrage-Nummer.**“ (`locales/de.default.json` → `sections.request.attach_note`). Shopify-Kontaktformulare erzeugen keine Anfragenummer, das Versprechen ist also ohne manuelles Zutun falsch **[VERMUTET]**.
- Bei Sonderwünschen hängt die Conversion am Foto. Zwischen Formular und E-Mail geht ein Teil der Leads verloren **[VERMUTET]**.
- Spam-Schutz: Es gibt keinen eigenen Honeypot. Shopify setzt auf Kontaktformularen standardmäßig hCaptcha ein, wenn es in *Einstellungen → Präferenzen → Spamschutz* aktiv ist **[VERMUTET, im Admin prüfen]**.
- **Fix:** „Anfrage-Nummer“ streichen. Den Hinweis mit `mailto:`-Link und Betreff vorbelegen (`?subject=Sonderwunsch%20–%20Fotos`). Mittelfristig Shopify Forms oder eine Upload-App (z. B. Uploadery) bzw. den teeinblue-Upload nutzen.
- Kontakt `main-contact`: Pflichtfelder ✔, Bestellnummer optional ✔, Erfolgsmeldung ✔. Die Checkbox „Ich habe die Datenschutzerklärung gelesen“ als Pflichtfeld ist rechtlich unnötig und kostet Conversion (P2). `s.address` ist leer, Firma und Adresse fehlen auf der Kontaktseite (P2).

#### P1-11 · Futterrechner: Kopplung Ergebnis und Newsletter, Erfolgsmeldung immer „ok“, verlinkter Artikel noch nicht online **[BELEGT]**
- Einwilligungstext (`sections/futterrechner.liquid` Z. 398): „Ich möchte **mein Ergebnis und den Newsletter** … erhalten.“ Das Ergebnis gibt es per Mail nur zusammen mit dem Newsletter. Das Kopplungsverbot (Art. 7 Abs. 4 DSGVO) ist strittig **[VERMUTET]**. **Fix:** zwei Checkboxen (Pflicht: Ergebnis per Mail, optional: Newsletter) oder klar als „Newsletter mit Futterplan“ deklarieren. Brevo-Liste entsprechend trennen.
- Mail-Versand per `fetch(…, {mode:'no-cors'})` (Z. ~1478): Die Antwort ist nicht lesbar, deshalb wird **immer** „Fast geschafft …“ angezeigt, auch wenn Brevo ablehnt. Das gilt genauso für `assets/newsletter.js` Z. 51 (Newsletter und Startpaket). **Fix [VERMUTET machbar]:** Brevo-Form im iframe/JSONP-Modus nutzen oder den vorhandenen `brevo_proxy_url`-Weg (`/apps/pmp/newsletter`) aktivieren, der den Status zurückgibt.
- `templates/page.futterrechner.json` → `blog_url = /blogs/magazin/futtermenge-hund-berechnen…`. Der Artikel ist **geplant für 28.09.2026** und bis dahin 404 (Z. 438 „Die ganze Rechnung Schritt für Schritt“). Erledigt sich morgen, wenn die Veröffentlichung klappt. Bitte am 28.09. prüfen. (Der Figur-Link Z. 85–92 ist korrekt abgesichert.)
- Über 90 versteckte Felder (inkl. `HUND_NAME` und Gesundheitsangaben wie Herz, Niere, Diabetes) gehen an Brevo. Das muss in die DSE (P0-3).

#### P1-12 · Blog und Artikel: CTA-Platzierung mobil **[BELEGT: Layout]** / Wirkung **[VERMUTET]**
- `assets/theme.css`: mobil `.article{grid-template-columns:1fr}` und `.article__side{position:static}`. Die Seitenleiste mit **Inhaltsverzeichnis und Shop-CTA** („Dein Tier als Kunstwerk / Foto hochladen“) landet mobil **ganz am Ende** hinter Autorenbox und Share-Leiste. Das Inhaltsverzeichnis am Artikelende ist nutzlos, die CTA sieht kaum jemand.
- Seiten-CTA-Ziel `templates/article.json` → `main.settings.cta_url = shopify://collections/all` (322 Produkte, inkl. T-Shirts und Tassen). Besser `hundeportraits` bzw. dynamisch wie beim Produktblock.
- **Fix:** Mobil das TOC als ausklappbares `<details>` **vor** `.rte` setzen. Die Seiten-CTA mobil nach dem 2. `<h2>` einfügen (JS existiert bereits für das TOC) oder als schmale Sticky-Leiste zeigen (nicht gleichzeitig mit dem Fly-in). `cta_url` → `shopify://collections/hundeportraits`.

#### P1-13 · YouTube-Sync offenbar seit 22 Tagen still **[BELEGT: Datum]** / Ursache **[VERMUTET]**
- Shop-Metafeld `pmp.youtube_latest` wurde zuletzt am **05.09.2026** aktualisiert, das neueste Video darin ist vom 02.09.2026. `pmp.youtube_popular` ebenfalls am 05.09. Der Kanaltext verspricht „Dienstags Rasseportrait, samstags Hunde verstehen“, und laut Section-Schema läuft die Routine „YouTube-Sync“ täglich.
- `settings.youtube_api_key` ist leer, wird aber **vom Theme nicht genutzt** (nur `settings_schema.json`). Die Anzeige hängt allein an den Metafeldern. Ohne Metafelder würden die Blöcke genutzt (Platzhalter ohne Bild, weil es keine Thumbnails gibt). Die Gruppe „Neu auf dem Kanal“ wäre auf Tierschutz und Über uns leer (nur 3 Blöcke).
- **Fix:** Die Routine prüfen (außerhalb des Themes). Das Feld `youtube_api_key` entfernen oder im Schema als „wird nicht verwendet“ kennzeichnen.

#### P1-14 · Newsletter-Versprechen „10 % Rabatt“ ohne erkennbaren Willkommenscode **[VERMUTET]**
- Badge `claim` „10 % Rabatt“, `newsletter.perk_1` „10 % auf deine erste Bestellung“, `article.json` → „10 % auf dein erstes Portrait“.
- In Shopify aktiv sind nur `LKW10` und `DANKE10 Paketbeileger`, einen erkennbaren Newsletter-Code gibt es nicht. Vielleicht verschickt Brevo einen dieser Codes. **Prüfen**, ob die Willkommensmail einen gültigen Code enthält. Sonst ist das Versprechen irreführend.

---

### P2: Optik, Konsistenz, Feinschliff

| # | Ort | Befund | Fix | Status |
|---|---|---|---|---|
| P2-1 | Seiten `danke-welpen-startpaket`, `startpaket-download` (Body) | Link „Das Rasse-Lexikon“ → `/blogs/rasse-lexikon`. Den Blog gibt es nicht, der Redirect führt auf `/blogs/magazin`, also nicht zu den Rassen. | Link → `/blogs/magazin/tagged/rasseportraits` | BELEGT |
| P2-2 | dieselben Seiten (Template `page` → `main-page`) | **Zwei H1**: `main-page` rendert `page.title` als H1, der Body enthält ein eigenes `<h1 style="font-size:40px">`. Das Inline-Styling (dunkler Verlauf, Gold) passt nicht zum Theme (weiß/rot). | Body-`<h1>` → `<p class="h2">` oder eigenes Template ohne Titel. Farben auf Theme-Tokens umstellen | BELEGT |
| P2-3 | `startpaket-download` | Der PDF-Link ist eine öffentliche CDN-URL (`…/PrintMyPet_Die-ersten-30-Tage.pdf`). Wer die Seite kennt, lädt ohne Opt-in. Die Seite ist noindex ✔. | Akzeptabel. Optional die Download-Seite nur per Brevo-Link mit Token öffnen | BELEGT |
| P2-4 | Page-Bodies ueber-uns, so-funktionierts, buecher, welpen-startpaket, tierschutz, sonderwunsch, youtube, faq | Werden nicht gerendert (Templates ohne `main-page`), enthalten aber veraltete Aussagen („Line-Art, Fantasy“, „3 bis 7 Werktage“, „Design personalisieren“, alter Brevo-iframe, nur 2 Ratgeber). Aus ihnen wird die Meta-Description nur dann erzeugt, wenn kein `global.description_tag` gesetzt ist. Alle Seiten haben einen Description-Tag ✔. | Bodies leeren oder auf eine kurze, aktuelle Zusammenfassung kürzen (Verwechslungsgefahr für spätere Bearbeiter, interne Suche) | BELEGT |
| P2-5 | `page.youtube` Meta „dienstags und samstags neu“ ↔ Body „jede Woche neu“ ↔ Hub-Default-Text „jede Woche neu“ | uneinheitlich | angleichen | BELEGT |
| P2-6 | FAQ-Section-Default `text` („meist innerhalb weniger Stunden“) auf so-funktionierts und faq ↔ Kontakt/Sonderwunsch „24 Stunden“ ↔ Kontakt „Mo bis Fr, 9 bis 18 Uhr“ | uneinheitlich | `faq.settings.text` in beiden Templates auf „meist innerhalb von 24 Stunden (Mo bis Fr)“ setzen | BELEGT |
| P2-7 | FAQ f3 „Digitale Portraits … innerhalb von 24 Stunden per E-Mail“ ↔ Produkt-Handle `dein-hund-als-digitales-kunstwerk-sofort-download` | „Sofort“ vs. 24 h | Produkttitel oder FAQ angleichen | BELEGT |
| P2-8 | Stile | „13 Stile“ (so-funktionierts, Locale) passt zu 13 Stil-Kollektionen, aber `settings.art_styles` listet 11 (ohne Anime, Original) und die Galerie zeigt 7 | `art_styles` ergänzen (falls noch genutzt). Galerie um Street-Art, Retro, Pop-Art, Sketch, Anime ergänzen (Bilder `pmp222-*` existieren laut index.json) | BELEGT |
| P2-9 | `sections.transformation.step3_text` „Zehn Materialien … dazu der Acryl-Aufsteller“ ↔ `material_order` mit 11 Einträgen | Zählung | „Elf Wandbild-Varianten“ oder Zahl weglassen | BELEGT |
| P2-10 | Futterrechner `fr-figur` Option 1 und 2 | Identischer Text („sehr dünn, Rippen, Wirbelsäule, Becken deutlich zu sehen“) | Stufe 1: „abgemagert, keine Fettreserven, Muskelschwund“; Stufe 2: „sehr dünn …“ | BELEGT |
| P2-11 | Futterrechner Figur-Abweichung `ABW = {3:.25, 1/2:.35}` | Bei Stufe 3 wird mit +33 % Idealgewicht gerechnet. Üblich (WSAVA, 9er-Skala) sind etwa 10 % pro Stufe, Stufe 3 also ca. 10–20 % unter Ideal. Das Ergebnis liegt für „dünn“ vermutlich zu hoch; Weg B zielt zusätzlich +10 %. Laut Code-Kommentar „aus dem Buch“. | Fachlich gegenprüfen. Plausibilität des Kernpfads (20 kg, Stufe 5, Faktor 110, 374 kcal) ergibt ca. 250 g/Tag ✔ | VERMUTET (fachlich) |
| P2-12 | Futterrechner Alter „Welpe“ bzw. „tragend“ **und** Besonderheiten „Welpe im Wachstum“ bzw. „trächtig oder säugend“ | Doppelte Abfrage | Checkboxen `WACHSTUM` und `TRAECHTIG` aus `bes` entfernen (Alter deckt es ab) | BELEGT |
| P2-13 | Futterrechner mobil | Sehr langes Formular (Ziel, Name, Alter, Gewicht, Figur, Windhund, 9 Besonderheiten, Alltag, 7 Futterformen, Energie/Inhaltsstoffe, Beilagen, Zusätze, Leckerlis, Mahlzeiten). Radios haben `min-height:44px` ✔, `inputmode="decimal"` ✔, Fehlertexte pro Feld mit `role=alert` ✔, Disclaimer „Startwert … keine tierärztliche Beratung“ ✔ (aber nur `fr-small`). H1 = Seitentitel mit 58 Zeichen bei `clamp(2.4rem…)`, auf 360 px etwa 5–6 Zeilen. | Seitentitel (SEO) und H1 trennen: H1 z. B. „Futterrechner für Hunde“, darunter die Frage als Lead. „Ausführlich“-Felder hinter `<details>` einklappen. Disclaimer über das Ergebnis setzen | BELEGT / Wirkung VERMUTET |
| P2-14 | Bücher mobil | `.books{grid-template-columns:1fr 1fr}` → Buchkarten ca. 155 px breit. b10 hat Beschreibung, 3 Bullets, 2 Buttons und Hinweis, das ergibt sehr lange, schmale Karten | Mobil `1fr` für Karten mit `description`, oder Beschreibung mobil ausblenden | BELEGT (CSS) / Optik VERMUTET |
| P2-15 | Blog-Kategorien `templates/blog.json` → `categories` | „Welpen“ (6) erfasst nicht „Welpe“ (4: u. a. `welpe-erste-nacht`, `praegezeit-welpe-sozialisierung`). Es gibt keine Chips „Geschenke“ oder „Ernährung“, obwohl das die kaufnächsten Themen sind (Weihnachts- und Geschenkartikel, Futter-Serie). | Tags vereinheitlichen (`Welpe` → `Welpen`), Chips „Geschenke“ und „Ernährung“ ergänzen | BELEGT |
| P2-16 | `snippets/article-card.liquid` Z. 2–12 | Kategorie-Tag = erster Tag alphabetisch (z. B. „Alleinbleiben“, „Angst“, „Beagle“) und verlinkt auf Mini-Tag-Seiten (noindex) | Tags „Kategorie:…“ pflegen oder die erste Übereinstimmung mit `blog.json.categories` nehmen | BELEGT |
| P2-17 | `sections/main-article.liquid` Z. 41 | „Aktualisiert“ erscheint praktisch immer (`updated_at > published_at` schon bei minimalem Abstand) | Nur zeigen, wenn die Differenz über 1 Tag liegt (Vergleich über `date: '%s'`) | BELEGT |
| P2-18 | Artikel: Links auf archivierte Handles (`hundeportrait-poster` usw., ca. 45 Links) | Redirect-Hop, `katzenportrait-acrylglas` usw. ebenfalls | Per Suchen/Ersetzen auf die neuen Handles `personalisiertes-{tier}portrait-…` umstellen (Redirect-Tabelle als Mapping nutzen) | BELEGT |
| P2-19 | Blog `news` (0 Artikel) | Leere, indexierbare Seite `/blogs/news` | Blog löschen oder Redirect auf `/blogs/magazin` | BELEGT |
| P2-20 | `page.welpen-startpaket` → `blog` (featured-blog) | Zeigt die 3 neuesten Artikel (aktuell Weihnachten, Prägezeit, Hundeportrait verschenken) statt Welpen-Artikeln | Section um einen Tag-Filter erweitern oder feste Artikel wählen | BELEGT |
| P2-21 | `page.buecher` → Filter-Chips | Chip „Pferd“ trifft nur das Pferde-Malbuch, „Katze“ nur das Katzen-Malbuch. Mobil ohnehin kaputt (P0-1) | Nach dem Fix von P0-1 okay | BELEGT |
| P2-22 | Newsletter-Einwilligung `newsletter.consent_html` | „… und **akzeptiere** die Datenschutzerklärung“: Eine DSE „akzeptiert“ man nicht. Die Startpaket-Einwilligung koppelt PDF und Tipps | „Hinweise zum Datenschutz: …“; Kopplung offen benennen („Das Startpaket gibt es zusammen mit unseren Tipps“) | VERMUTET (rechtlich) |
| P2-23 | Double-Opt-in-Hinweise | ✔ vorhanden: `newsletter.trust` „Double-Opt-in · Kein Spam · Abmelden mit einem Klick“, Erfolgstext „Bitte klicke auf den Bestätigungslink“, Danke-Seite mit Absender `newsletter@printmypet.de` | – | BELEGT |
| P2-24 | `danke-welpen-startpaket` | Wird vermutlich nie angesteuert: Das Theme-Formular sendet per `fetch` (Ajax) und zeigt die Erfolgsmeldung inline, eine Weiterleitung gibt es nicht | Nach Erfolg `location.href='/pages/danke-welpen-startpaket'` (nur für das Startpaket-Formular) oder Seite löschen | VERMUTET |
| P2-25 | Shipping-Policy ohne Lieferzeiten | Theme sagt überall „5 bis 9 Werktage“, die Policy verweist nur auf Produkt und Checkout | Zeiten in die Policy übernehmen (DE/AT, EU, CH, international) | BELEGT |
| P2-26 | Widerruf ↔ AGB | Die Widerrufs-Policy nennt „Online-Streitbeilegung: nicht bereit …“ ✔ (die OS-Plattform ist seit 20.07.2025 abgeschaltet, ein Link wäre falsch ✔). Die AGB (englisch) verweisen auf die „Refund Policy“ | Mit der AGB-Neufassung erledigt (P0-2) | BELEGT |
| P2-27 | YouTube-Consent | `localStorage pmp_yt_consent='1'` gilt dauerhaft, ohne Widerruf | Link „Videos wieder blockieren“ im Footer bzw. bei den Cookie-Einstellungen | BELEGT |
| P2-28 | Breadcrumbs | Fehlen auf so-funktionierts, tierschutz, ueber-uns, sonderwunsch, faq, contact, youtube, buecher (Sections rendern keine) | Optional: `{% render 'breadcrumbs' %}` in die jeweils erste Section oder ein kleines Breadcrumb-Section-Template | BELEGT |
| P2-29 | `sections/main-article.liquid` Z. 82 | Pinterest-Share ohne `media=`-Parameter, der Pin hat dann kein Bild | `&media={{ article.image | image_url: width: 1000 | prepend: 'https:' | url_encode }}` | BELEGT |
| P2-30 | Kontakt | `main-contact.address` leer, Firma und Adresse erscheinen nicht auf der Kontaktseite. Die Pflicht-Checkbox „Datenschutzerklärung gelesen“ ist unnötig | Adresse (Creator Advisory S.L., Valencia) eintragen. Checkbox durch einen Hinweistext ersetzen | BELEGT |

---

## 3. Was gut ist (bitte nicht kaputt machen)

- AdSense ist technisch sauber auf Blog und Artikel beschränkt und erst nach Marketing-Consent aktiv (`layout/theme.liquid` Z. 22–57). Auf Produktseiten gibt es **kein** AdSense ✔.
- YouTube-Hub mit Zwei-Klick-Lösung, gespiegelten Thumbnails auf Shopify-CDN und youtube-nocookie ✔.
- Newsletter/Brevo: Honeypot plus Brevo-`email_address_check`, E-Mail-Validierung trotz `novalidate`, Schutz gegen Doppelbindung (`window.__pmpNewsletterReady`), DOI-Texte ✔.
- Futterrechner: saubere Feldvalidierung mit Bereichen (Gewicht 2–80 kg, Energie 50–650 kcal bzw. 200–2720 kJ, Ration 10–3000 g), Abfangen von „Leckerlis ≥ Bedarf“ und von nicht berechenbarer Energie, Welpen und tragende Hündinnen werden bewusst **nicht** berechnet, sondern mit Tierarzt-Hinweis versehen, Buch-Anker `#hundeernaehrung` funktioniert (b10 steht an erster Stelle) ✔.
- Alle Artikel haben Beitragsbild mit Alt-Text, der Produktblock am Artikelende wählt Hund, Katze oder Pferd nach Tag ✔. Für alle alten Blog-/Produkt-URLs gibt es Redirects ✔.
- Spendenangabe 20 % vom Gewinn ist auf allen Seiten einheitlich ✔. Lieferzeit 5–9 Werktage ist auf allen sichtbaren Stellen einheitlich ✔.
- Danke- und Download-Seiten sind `noindex` (`snippets/meta-tags.liquid` Z. 64) ✔.

---

## 4. Empfohlene Reihenfolge für die Umsetzungs-Session

1. **P0-1** CSS-Regel auf `.template-index` begrenzen (1 Zeile, sofort sichtbarer Effekt auf Bücher und FAQ mobil).
2. **P0-6** 15 `mat-*`-Blöcke aus `page.so-funktionierts.json` entfernen.
3. **P0-5** `amazon_tag` setzen, Locale `sections.books.affiliate_note` korrigieren, `sponsored: true`.
4. **P0-4** FAQ f5 (Versandkosten) korrigieren. Versand-Policy: Liefergebiet und Zeiten (Policy-Änderung im Admin).
5. **P1-4, P1-5, P1-3, P1-7** Texte angleichen (Kissen/Decken, Qualitätsblock, Garantie/Vorschau, EU/Zoll).
6. **P1-1** Shop-CTAs auf Futterrechner, Tierschutz, Über uns, YouTube, Bücher.
7. **P1-2** Fly-in-Ausschlussliste (collection, index, Formularseiten), Schließen-Button 44 px.
8. **P1-6** `#voting`-Anker und Zeitplan Tierschutz.
9. **P1-8** Artikel-iframe durch Zwei-Klick ersetzen.
10. **P0-2, P0-3, P0-7** Rechtstexte (AGB deutsch, DSE mit allen Diensten, Impressum vollständig). Nicht vom Theme-Agenten „erfinden“ lassen, sondern Generator oder Anwalt.
11. **P1-9** Entscheidung AdSense (TCF-CMP oder abschalten).
12. P2-Liste nach Aufwand.
