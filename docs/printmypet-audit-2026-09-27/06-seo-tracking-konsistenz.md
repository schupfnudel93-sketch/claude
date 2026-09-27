# 06: Querschnitt SEO, Texte, Sprache, Tracking, Konsistenz, Performance

Audit printmypet.de, Stand 27.09.2026, nur lesend über das Shopify-Admin-MCP.
Live-Theme: `PMP: Futterrechner v5.1 (FREIGABE)`, gid://shopify/OnlineStoreTheme/208344613202.
Alle 130 Theme-Dateien (templates, sections, snippets, assets/*.js, locales, config) wurden per Wildcard geladen und lokal durchsucht. Eine Kopie liegt unter `scratchpad/theme/`.

**Legende:** **[B]** = belegt (Datei, Key oder API-Wert gesehen). **[V]** = vermutet oder nicht verifizierbar, weil die Website von hier aus nicht erreichbar ist. Prioritäten: **P0** = kostet jetzt Umsatz, Messbarkeit oder schafft rechtliches Risiko. **P1** = bald beheben. **P2** = aufräumen oder verbessern.

---

## 0. Die Fakten, gegen die alles geprüft wurde (Soll-Werte aus Admin-Daten)

| Thema | Realer Wert | Quelle |
|---|---|---|
| Versand DE | Standard 5,99 €, **gratis ab 150 €** (Bedingung TOTAL_PRICE ≥ 150), Express 9,99 € | deliveryProfiles, „Allgemeines Profil“ [B] |
| Versand EU (26 Länder inkl. AT, ES) | 13,99 €, **keine** Gratisgrenze | dto. [B] |
| Versand „International“ (14 Länder: AE, AU, CA, CH, GB, HK, IL, JP, KR, MY, NO, NZ, SG, US) | 19,99 € | dto. [B] |
| Gelato-Profile (10 Stück: Small/Large Posters, Framed Posters, Canvas …) | **0 Varianten zugeordnet**, haben aber je eine Zone „Worldwide/ROW“ | deliveryProfiles [B] |
| `shop.shipsToCountries` | ca. 240 Länder (inkl. RU, BY, AF …) | shop [B]. Wird vermutlich durch die leeren Gelato-ROW-Zonen aufgebläht [V] |
| Märkte | Germany (primär, DE), Österreich & Schweiz, EU-Erweiterung (25 Länder) aktiv, UK inaktiv. webPresence bei allen = null (gleiche Domain) | markets [B] |
| Sprachen | **nur `de` veröffentlicht**. `en` ist angelegt, aber unveröffentlicht. **es/fr/it sind im Shop gar nicht angelegt** | shopLocales [B] |
| Stile (Theme-Setting `art_styles`) | 11: Aquarell, Aquarell mit Herz, Cartoon, 3D-Cartoon, Minimal, Ölgemälde, Royal, Street-Art, Retro, Pop-Art, Sketch | settings_data [B] |
| Stil-Kollektionen | 12 Stile + „Original“ = 13 (Aquarell, Ölgemälde, Royal, Cartoon, 3D-Cartoon, Minimal, Street-Art, Retro, Pop-Art, Sketch, **Herzensbild**, **Anime**, Original). `stil-aquarell-mit-herz` wird per 301 auf `stil-aquarell` umgeleitet | collections, urlRedirects [B] |
| Spende | `donation_percent` 20, überall „vom Gewinn“ | settings_data + Texte [B] |
| Produkte | 120 aktiv / 322 gesamt. 4.523 Varianten bei den aktiven. Preise 12,99 € bis 103,99 € | products [B] |
| Rabattcodes | nur `LKW10` (10 %) und `DANKE10` (10 %, Paketbeileger). Automatisch: „Weihnachtsaktion –15 %“ (SCHEDULED) | codeDiscountNodes [B] |
| Bewertungen | Kein aktives Produkt hat `reviews.rating_count` | metafields [B] |
| Installierte Apps | Messaging, Gelato, Prodigi, Printify, Printful, Teeinblue, Judge.me, Search & Discovery, Digital Products, PrintMyPet Automation, 2× Claude, 1× ChatGPT-MCP. **Weder „Google & YouTube“ noch „Facebook & Instagram“** | appInstallations [B] |
| Shop-Name | „PrintMyPet“. Die Marke steht im Theme als „Print my Pet“, im Blog als „Print My Pet“ | shop, meta-tags [B] |

---

## 1. Widerspruchs-Tabelle

### 1a. Lieferzeit

| Aussage | Fundstelle | Wert |
|---|---|---|
| Lieferzeit | `locales/de.default.json` → `products.product.delivery_estimate`, `delivery_estimate_ready` | „5 bis 9 Werktagen“ |
| Lieferzeit | `sections/header-group.json` → announcement `a3` | „Druck in Europa · Lieferung in 5 bis 9 Werktagen“ |
| Lieferzeit | `templates/product.json` `trust.t2` + `acc2.content`; `product.teeinblue.json` `t2` + Zeile 77; `product.personalized-fixed-style.json` `t2` | „5 bis 9 Werktagen“ |
| Lieferzeit, rechnerisch falsch | `templates/index.json` → FAQ „Wie lange dauert die Lieferung?“ | „Druck 2 bis 4 Werktage, Versand 1 bis 3 Werktage. Insgesamt meist **5 bis 9**“ (2–4 + 1–3 ergibt **3–7**) |
| Lieferzeit, anders zerlegt | `sections/faq.liquid` Schema-Default | „Vorschau 1 bis 2 Tage, Druck 2 bis 4, Versand 1 bis 3 … insgesamt 5 bis 9“ (ergibt 4–9) |
| Lieferzeit | `sections/how-it-works.liquid` Preset-Default (Zeile 34) | „**3 bis 7 Werktagen**“ + „Leinwand, Webdecke, Kissen oder Hoodie“ |
| Lieferzeit, strukturierte Daten | `snippets/schema-org.liquid` → `shippingDetails` | handling 1–4 + transit 3–6 = **4–10 Tage** |
| Lieferzeit Digital | `templates/page.faq.json`, locale `delivery_estimate_digital_portrait`, `main-product.liquid` Zeilen 336 und 346 | „innerhalb von 24 Stunden“ bzw. „in der Regel innerhalb von 24 Stunden an Werktagen“ |
| Lieferzeit: Platzhalter | `templates/product.personalized-fixed-style.json` → `acc2.content` | „Die Lieferzeit und Versandkosten werden **vor Veröffentlichung** anhand des aktuellen Gelato-Profils bestätigt.“ (Das Template nutzt derzeit kein aktives Produkt [B]) |

### 1b. Versandkosten und Gratisgrenze

| Aussage | Fundstelle | Wert |
|---|---|---|
| Gratisversand-Leiste im Warenkorb | `config/settings_data.json` → `cart_free_shipping_threshold` | **0** (Leiste aus) |
| Gratisgrenze real | Allgemeines Profil, Zone DE | **150 €, nur DE** |
| Versandkosten | `templates/product.json` `acc2`, `product.teeinblue.json` Zeile 77 | „DE 5,99 €, ab 150 € versandkostenfrei“ (stimmt) |
| Ausnahme gerahmte Poster | `templates/product.json` `acc2` + `templates/page.faq.json` „Was kostet der Versand?“ | „einige gerahmte Poster: eigene Pauschale **5,69 bis 10,99 €**“. **Stimmt nicht mehr:** alle Gelato-Profile haben 0 Varianten [B] |
| Versandgebiet | `page.faq.json` | „EU: 13,99 €. Schweiz und **weltweit**: 19,99 €“. Real bekommen nur 14 Nicht-EU-Länder diesen Tarif, „weltweit“ stimmt nicht [B] |
| Versandgebiet | **Versand-Richtlinie** (`scratchpad/policies.txt`, SHIPPING_POLICY) | „Wir liefern derzeit nach **Deutschland, Österreich und in die Schweiz**.“ Widerspricht der EU-Zone, der International-Zone, dem aktiven Markt „EU-Erweiterung“ und dem FAQ [B] |
| Zoll | `templates/index.json` FAQ + `sections/faq.liquid` + `page.ueber-uns.json` | „Wir produzieren in der EU. **Kein Zoll**, keine Überraschungen“. Versand-Richtlinie sagt dagegen: „Schweiz … können Einfuhrabgaben … anfallen“. CH ist ein aktiver Markt [B] |
| Theme-Kopien | Themes „PMP: Versand 79 € 23.09.“ und „Versand 150 € + elf Stile 23.09.“ | Historie mit **79 €** als Grenze. Live-Wert ist 150 € [B] |

### 1c. Anzahl der Stile

| Aussage | Fundstelle | Wert |
|---|---|---|
| Stilliste im Produkt-Picker | `settings_data.json` → `art_styles` (genutzt in `snippets/style-picker.liquid`) | **11** (mit „Aquarell mit Herz“, **ohne Anime**) |
| Fallback im Picker | `snippets/style-picker.liquid` Zeile 15 | **7** Stile |
| Startseite How-it-works | `templates/index.json` → `how.h2` | „**13 Stile**“ |
| So funktioniert's | `templates/page.so-funktionierts.json` → `how.h2` | „**13 Stile**“ |
| Transformation | `locales/de.default.json` → `sections.transformation.step2_text` | „**13 Stile**“ |
| Style-Showcase | `templates/index.json` → `styles.eyebrow` | „**12 Stile plus Original**“ (12 Blöcke, u. a. Anime, Herzensbild) |
| Style-Showcase Default | `sections/style-showcase.liquid` Zeile 50 | „13 Stile“ |
| Kollektions-SEO | Kollektionen `hundeportraits`, `katzenportraits`, `pferdeportraits` → seo.description | „in **13 Stilen**“ |
| EN/ES/FR/IT | `locales/*.json` → `sections.transformation.step2_text` | „**Seven** styles / Siete / Sept / Sette“ |
| Code-Kommentare | `snippets/product-card.liquid` Zeile 36, `assets/theme.css` Zeile 988 | „7 Stile“, „sieben Stile“ |
| Name des Stils | `art_styles` „Aquarell mit Herz“ vs. Kollektion „Herzensbild“ vs. Redirect `stil-aquarell-mit-herz` → `stil-aquarell` | drei Namen für einen Stil |

### 1d. Spende (20 % vom Gewinn)

| Aussage | Fundstelle | Wert |
|---|---|---|
| Überall | announcement `a2`, footer-group, locale `donation_html`, `page.tierschutz.json`, `page.faq.json`, `page.ueber-uns.json`, index (hero pill, marquee, how, faq), `product.json` `u?`, `collection.json` | „20 % … **Gewinn**“, einheitlich [B] |
| Doppelte Quelle | `settings.donation_percent` = 20 wird nur in locale-Platzhaltern genutzt. Rund 20 weitere Stellen haben „20 %“ fest im Text | Änderungsrisiko: der Wert müsste an allen Stellen einzeln geändert werden (P2) |
| Voting-Termin | `page.tierschutz.json`: Schritt 1 „**Ab Oktober 2027 sammeln wir eure Vorschläge**“ vs. Fließtext „Im Oktober 2027 **stellen wir … konkrete Projekte vor**. Ihr stimmt ab“ | Oktober ist einmal Vorschlagsphase, einmal Projektvorstellung (P2) |

### 1e. „Druck in Europa“ vs. „Deutschland“ vs. EU

| Aussage | Fundstelle | Wert |
|---|---|---|
| „Druck in Europa“ / „Gedruckt in Europa“ | header `a3`, marquee, footer, product/collection trust, usp | Europa |
| „Druck & Versand aus der EU“ | `templates/index.json` hero `t2`, `sections/hero.liquid` Default | EU |
| „Unsere **Werkstätten** stehen in der EU … faire Löhne“ | `page.ueber-uns.json` Zeile 50 | eigene Werkstätten? Es ist Print-on-Demand über Partner [V: Formulierung überzieht] |
| Versand-Richtlinie | SHIPPING_POLICY | „eine Fertigung im Lieferland ist nicht bei jeder Bestellung möglich“ |
| Produktions-Apps | Printful, Printify, Prodigi zusätzlich installiert | Ob alle Artikel in Europa gedruckt werden, lässt sich nicht prüfen. Bei US/JP-Bestellungen druckt Gelato vermutlich lokal [V] |
| Kein „Deutschland“-Druckversprechen gefunden | alle Dateien | Gut: kein Widerspruch „Deutschland“ vs. „Europa“ [B] |

### 1f. Vorschau-Versprechen (Live vs. E-Mail vs. 48 h)

| Aussage | Fundstelle | Wert |
|---|---|---|
| Live-Vorschau vor der Bestellung | index hero/marquee/how/FAQ, `page.faq.json`, `page.so-funktionierts.json`, locale `delivery_estimate`, `main-product.liquid` Zeile 168 | aktuelles Modell (Teeinblue) |
| Vorschau **per E-Mail innerhalb 48 h** | `locales/en|es|fr|it.json` → `products.product.delivery_estimate` („Preview within 48 hours“) | altes Modell |
| dto. | `sections/faq.liquid` Default, `sections/main-product.liquid` Zeile 565 Default `text` („innerhalb von 48 Stunden eine kostenlose Vorschau“) | altes Modell (Defaults) |
| Vorschau per E-Mail | **Seiten-SEO** `so-funktionierts` → description_tag: „kostenlose Vorschau **per E-Mail**“ | altes Modell, **sichtbar in Google** (P1) |
| Vorschau per E-Mail vor Druck, immer | **Widerrufs-Richtlinie** (REFUND_POLICY): „Du bekommst dein fertiges Motiv per E-Mail, bevor irgendetwas produziert wird. Gedruckt wird erst nach deinem Ja.“ | widerspricht dem Live-Vorschau-Modell (P1, Rechtstext) |
| Locale `products.preview.note` | de.default.json | „Die echte Stil-Verwandlung machen wir nach der Bestellung, **und** schicken dir die Vorschau per E-Mail“. Gilt nur für Nicht-Teeinblue-Produkte. Das Komma vor „und“ ist falsch |
| `cart.trust.preview` | de: „Live-Vorschau vor der Bestellung“ / en: „Free preview before printing“ | unterschiedliche Versprechen je Sprache |

### 1g. Zufriedenheitsgarantie vs. Rückgabe

| Aussage | Fundstelle | Wert |
|---|---|---|
| „Zufriedenheitsgarantie: Nicht glücklich? Wir drucken neu.“ | `product.json` u4, `collection.json`, `product.teeinblue.json`, `sections/usp-row.liquid`, `hero.liquid` Default | Neudruck **bei Nichtgefallen** |
| „falls das fertige Bild nicht passt, gilt unsere Zufriedenheitsgarantie: Wir drucken neu“ | `page.so-funktionierts.json` Zeile 420 | dto. |
| „Bei **Druckfehlern oder Transportschäden** drucken wir kostenlos neu“ | `page.faq.json` „Kann ich zurückgeben?“ | Neudruck **nur bei Mängeln** |

→ Das sind zwei verschiedene Leistungsversprechen. Eine „Garantie“ muss nach § 443 BGB klar umrissen sein. **P1**, Wortlaut vereinheitlichen.

### 1h. Produktsortiment

| Aussage | Fundstelle | Wert |
|---|---|---|
| „**Webdecke & Kissen kommen bald**“ | `page.so-funktionierts.json` → `how.h3` | Es gibt aber **aktive** Kissen (`kissen-*`, `personalisiertes-kissen-mit-deinem-tier`, `personalisierter-kissenbezug…`) und Decken (`decke-*`, `personalisierte-kuscheldecke…`, `sherpa-decke`) [B] |
| „Webdecken, Kissen und Schlüsselanhänger kommen bald“ | `page.faq.json` „Welche Produkte gibt es?“ | dto. |
| „Leinwand, Webdecke, Kissen oder Hoodie“ | `sections/how-it-works.liquid` Default | Default-Text |
| „Zehn Materialien … dazu der Acryl-Aufsteller“ | de locale `step3_text`, index `how.h3` | stimmt (10 Wandbild-Ausführungen + Aufsteller) [B] |
| „Poster, framed poster, canvas, framed canvas or acrylic glass“ (5) | en/es/fr/it `step3_text` | veraltet |
| „Seven styles“ | en/es/fr/it | veraltet |

### 1i. Newsletter-Versprechen

| Aussage | Fundstelle | Wert |
|---|---|---|
| „10 % auf deine erste Bestellung“ | locale `newsletter.perk_1`, `newsletter-form.liquid` Badge „10 %“, `newsletter-brevo.liquid` Default, index `newsletter.heading` „Willkommensrabatt“, `article.json` „10 % auf dein erstes Portrait“ | Versprechen |
| Passender Shopify-Code | codeDiscountNodes | **Kein Willkommens-Code gefunden**, nur `LKW10` und `DANKE10`. Ob Brevo einen dieser Codes verschickt, ist offen [V]. **P1 prüfen**, sonst wird ein Rabatt versprochen, den es nicht gibt |
| „Einmal pro Woche“ | index `newsletter.text` | nicht prüfbar [V] |

### 1j. Antwortzeit

| Aussage | Fundstelle |
|---|---|
| „Antwort innerhalb von 24 Stunden“ | index `request.r1`, `page.faq.json`, `page.sonderwunsch.json`, locale `sections.request.success` |
| „meist innerhalb weniger Stunden“ | Kontakt-Seite SEO-Description, `sections/faq.liquid` Default |
| „Antwort meist innerhalb von 24 Stunden“ | `sections/main-contact.liquid` Default |

P2, eine Aussage wählen.

### 1k. Preise „ab X €“

Im Theme gibt es keine fest eingetragenen „ab X €“-Produktpreise [B]. Die einzigen €-Beträge sind die Versandkosten (s. 1b). Die Preise kommen dynamisch (`products.product.from` = „ab“). Hier ist nichts zu tun.

---

## 2. Tracking-Befunde

| # | Prio | Fundstelle | Problem | Fix |
|---|---|---|---|---|
| T1 | **P0** | `settings_data.json` `ga4_measurement_id` = "" + appInstallations | **Es gibt keine GA4-Messung und keine Umsatz- oder Conversion-Messung** [B]. Die „Google & YouTube“-App ist nicht installiert, also gibt es auch keinen Google-Ads-Conversion-Tag und keinen Merchant-Center-Feed (Free Listings, Shopping). | Die **Google & YouTube-App** installieren und darüber GA4 + Google Ads + Merchant Center verbinden. Die App misst über Shopifys Web-Pixel und damit auch Checkout und Kauf. **Die ID nicht zusätzlich ins Theme-Feld eintragen**, sonst werden Seitenaufrufe doppelt gezählt. |
| T2 | **P0** | `layout/theme.liquid` Zeile 58–64 | Selbst mit ausgefüllter ID würde der Theme-Snippet **keinen Kauf messen**. Er sendet nur `config` (Seitenaufrufe) und keine E-Commerce-Events (view_item, add_to_cart, begin_checkout, purchase). Checkout und Danke-Seite laufen außerhalb des Themes. | Kauf-Tracking ausschließlich über App oder Custom Pixel (Einstellungen → Kundenereignisse). Den Theme-Snippet entweder entfernen oder nur als Fallback für Nicht-Shop-Seiten behalten. |
| T3 | **P1** | appInstallations, `settings.social_facebook` leer | **Kein Meta-Pixel und keine Conversions API**. Instagram ist aber ein aktiver Kanal (`social_instagram`). Kein Pinterest-Tag (Pinterest-Profil ist verlinkt), kein TikTok-Pixel (TikTok ist verlinkt). | Vor bezahlter Werbung die „Facebook & Instagram“-App (Pixel + CAPI) installieren. Pinterest und TikTok nur, wenn dort geworben wird. |
| T4 | P1 | alle Dateien: keine `Shopify.analytics.publish` / Custom Events | Lead-Ziele werden nirgends gemessen: Newsletter-Anmeldung (Brevo), Welpen-Startpaket-Download, Futterrechner-Nutzung, Sonderwunsch-Anfrage. | In `assets/newsletter.js` bei Erfolg `Shopify.analytics.publish('newsletter_signup', {...})` aufrufen, ebenso für Freebie, Futterrechner und Anfrage. Im Custom Pixel an GA4 weiterleiten. |
| T5 | P2 | interne Links mit `utm_*`: `pmp-freebie-flyin.liquid` 37/44, `freebie-inline.liquid` 21/24, `index.json` freebie-Banner, `page.futterrechner.json` 21, `futterrechner.liquid` 88 | UTM-Parameter auf **internen** Links überschreiben in GA4 die echte Quelle der Sitzung, z. B. wird aus Google-Organic „site/flyin“. | Für interne Links `?ref=flyin` bzw. data-Attribute plus Custom Event verwenden, UTMs entfernen. |
| T6 | P2 (ok) | `theme.liquid` AdSense + GA4-Block | Der Consent ist sauber gelöst: Default `denied`, Update über die Shopify Customer Privacy API, AdSense wird erst nach `marketingAllowed` geladen und nur im Blog [B]. | Nichts zu tun. Ob der Shopify-Cookie-Banner in den Kundendatenschutz-Einstellungen für EU aktiv ist, konnte ich nicht prüfen [V]. |
| T7 | P2 | urlRedirects `/ads.txt` → `https://cdn.shopify.com/...ads.txt` | Die ads.txt wird per Redirect auf eine fremde Domain ausgeliefert. Nach IAB-Spezifikation gelten nur Redirects innerhalb der Root-Domain [V: Google akzeptiert es bei Shopify meist]. | AdSense-Konto auf „ads.txt autorisiert“ prüfen. |
| T8 | P2 | theme.liquid | `google_site_verification` ist gesetzt und wird nur ausgegeben, wenn `content_for_header` sie nicht schon enthält [B], also richtig gelöst. | – |

---

## 3. SEO-Befunde

| # | Prio | Fundstelle | Problem | Fix |
|---|---|---|---|---|
| S1 | **P1** | `snippets/schema-org.liquid` Organization `contactPoint.availableLanguage` | Behauptet `["de","en","fr","es","it"]`. Veröffentlicht ist nur Deutsch [B]. | Auf `["de"]` setzen oder dynamisch aus `shop.published_locales` erzeugen. |
| S2 | P1 | `schema-org.liquid` Offer `shippingDetails` | Nur `addressCountry: "DE"`, **kein `shippingRate`**, Lieferzeit 4–10 Tage statt der kommunizierten 5–9. Google meldet dann „fehlendes Feld“, und Merchant-Listings zeigen keinen Versandpreis. | `shippingRate` {value 5.99, currency EUR} für DE ergänzen, ggf. AT/EU 13,99 als weitere `OfferShippingDetails`. Zeiten auf handling 2–4 / transit 1–3 bzw. 5–9 gesamt angleichen. |
| S3 | P1 | `schema-org.liquid` `offers` `limit: 20` | Aktive Produkte haben im Schnitt rund 38 Varianten (4.523 / 120) [B]. Bei vielen Produkten fehlen daher Offers, und die Preisspanne ist falsch. | Auf `AggregateOffer` (lowPrice/highPrice/offerCount) umstellen oder nur die ausgewählte Variante als Offer ausgeben. |
| S4 | P1 | `schema-org.liquid` `hasMerchantReturnPolicy.applicableCountry` | Nur `DE, AT, CH`, geliefert wird aber in 26 EU-Länder + 14 weitere. | Länderliste an die Märkte anpassen oder die Rückgaberichtlinie im Merchant Center pflegen. |
| S5 | P1 | Seiten-SEO `so-funktionierts` description_tag | „kostenlose Vorschau per E-Mail“, veraltet (s. 1f). | Text anpassen: „Live-Vorschau vor der Bestellung“. |
| S6 | P1 | 15 Kollektionen ohne SEO-Titel/Description: `mauspads`, `handyhuellen`, `tragetaschen`, `futtermatten`, `t-shirts-fur-tierliebhaber`, `hoodies-sweatshirts-fur-tierliebhaber`, `baby-kind-geschenke-mit-tiermotiven`, `wohnen`, `accessoires`, `kalender`, `futternaepfe`, `halstuecher`, `mit-foto`, `ohne-foto`, `bestseller` | Das Meta-Fallback nimmt `collection.description`. Wenn die leer ist, greift `shop.description` (sieht auf allen gleich aus). | SEO-Titel und Description pflegen (≤ 60 / ≤ 155 Zeichen). |
| S7 | P1 | Kollektions-SEO `hundeportraits`/`katzenportraits`/`pferdeportraits` | „in 13 Stilen“, s. Stil-Widerspruch 1c. | Nach der Stil-Entscheidung angleichen. |
| S8 | P2 | 3 aktive Produkte ohne SEO-Titel: `kaninchen-socken-pfotenpause`, `weihnachtstasse-mit-namen-unterm-baum-wie-jedes-jahr`, `tasse-mit-spruch-manche-sagen-tierhaare-ich-sage-zuhause`; 1 ohne SEO-Description (`kaninchen-socken-pfotenpause`) | Fallback greift, ist aber nicht optimiert. | Pflegen. |
| S9 | P2 | 24 Produkt-SEO-Titel > 60 Zeichen (max. 71: `dein-hund-als-digitales-kunstwerk-sofort-download`), 13 Descriptions > 160 | Titel werden in Google abgeschnitten. | Kürzen. |
| S10 | P2 | Markenschreibweise: Shop-Name „PrintMyPet“, og:site_name „PrintMyPet“, Theme-Suffix „Print my Pet“, Blog-Meta „Print My Pet“, Produkt-SEO-Titel 77× „\| PrintMyPet“, 21× „\| Print my Pet“, 22× ohne | Drei Schreibweisen verwässern die Marke. | Eine Schreibweise festlegen (empfohlen „Print my Pet“ wie im Logo, oder „PrintMyPet“ wie der Shop-Name) und überall angleichen. `meta-tags.liquid` erkennt beide ohne Leerzeichen und hängt nichts doppelt an [B]. |
| S11 | P2 | `snippets/meta-tags.liquid` `og:price:amount` | `money_without_currency` liefert beim Shop-Format „€{{amount_with_comma_separator}}“ „39,99“. OG und Pinterest erwarten „39.99“ [V: Format aus PMP-227-Kommentar abgeleitet]. | `product.price \| divided_by: 100.0` verwenden. |
| S12 | P2 | Blog `news` mit 0 Artikeln | Leere, indexierbare Seite `/blogs/news` [V: ob verlinkt]. | Blog löschen oder `noindex` in `meta-tags.liquid` ergänzen (`blog.articles_count == 0`). |
| S13 | P2 | Blog-Bild-Alt-Texte `malinois`, `berner-sennenhund`, `dobermann`, `rhodesian-ridgeback`, `yorkshire-terrier`, `boxer`, `cavalier…`, `cocker…`, `weimaraner` | Alt-Text ist nur der Titel („X: Charakter, Haltung und Alltag mit der Rasse“) und beschreibt das Bild nicht. | Bildbeschreibende Alt-Texte schreiben. |
| S14 | ok | Produktbilder | **Alle 1.113 Medien der 120 aktiven Produkte haben Alt-Text** [B]. Viele Alt-Texte wiederholen sich (bis zu 10× derselbe), P2. | Optional variieren (Ansicht/Raum/Detail). |
| S15 | ok | urlRedirects: 189 | **Alle Produkt-Ziele sind aktive Produkte** [B]. Die alte Blog-Struktur `/blogs/rasse-lexikon/*` ist sauber umgeleitet. | – |
| S16 | P2 | Archivierte Produkte ohne Redirect, z. B. `halsband-*` (5), `futternapf-*-alt` (3), `beagle-tragetasche-nur-kurz-gassi`, `katzen-mauspad-homeoffice-katzenaufsicht`, `sherpa-fleecedecke-mit-tiermotiv…`, `giclee-tierportrait…`, `premium-poster-mit-tiermotiv…` u. a. (Liste nicht vollständig, nur die erste Seite von 322 geprüft) | Falls diese URLs indexiert waren, liefern sie 404 [V]. | In der Search Console „Nicht gefunden“ prüfen und gezielt umleiten. |
| S17 | P2 | Kollektion `futtermatten`: beide Produkte archiviert | Wird zwar durch `products_count == 0` auf noindex gesetzt, steht aber evtl. noch im Menü [V]. | Aus der Navigation nehmen oder weiterleiten. |
| S18 | ok | `meta-tags.liquid` | Canonical, noindex-Regeln für Suche, 404, Warenkorb, Tags, leere Kollektionen, Danke-Seiten; hreflang-Logik korrekt (greift erst bei > 1 Sprache) [B]. | – |
| S19 | P2 | Seiten `danke-welpen-startpaket`, `startpaket-download` | Ohne SEO, aber auf noindex [B]. | – |
| S20 | P2 | `schema-org.liquid` Article `author` | Immer `@type: Organization`, auch wenn `article.author` eine Person ist. | Bei Personennamen `Person` verwenden (E-E-A-T). |
| S21 | P2 | `schema-org.liquid` + Judge.me-App-Embed | Judge.me kann eigenes Product- oder Review-JSON-LD ausgeben, dann gibt es zwei Product-Knoten [V]. Aktuell hat kein Produkt Bewertungen [B]. | Nach der ersten Bewertung mit dem Rich-Results-Test prüfen. |

---

## 4. Sprach- und Markt-Befunde

| # | Prio | Fundstelle | Problem | Fix |
|---|---|---|---|---|
| L1 | **P1** | shopLocales vs. `locales/en|es|fr|it.json` | Nur **de** ist veröffentlicht. `en` ist angelegt und unveröffentlicht, es/fr/it sind nicht angelegt [B]. Die vier Theme-Übersetzungen sind aktuell toter Code, und inhaltlich veraltet (s. L2). | Entscheidung treffen: (a) vorerst **nur DE**, dann ist nichts zu tun außer S1 (Schema) und L2 (vor einem späteren Launch). (b) EN/ES/FR/IT launchen: Übersetzungen aktualisieren, dazu Produkt- und Seitentexte über Translate & Adapt, sonst entsteht ein halbdeutscher Shop. |
| L2 | P1 (vor jedem Sprach-Launch) | en/es/fr/it.json | Es **fehlen die Keys** `products.product.delivery_estimate_wunschbild` und `trust_wunschbild`, die in `main-product.liquid` genutzt werden. Shopify zeigt dann die deutschen Texte. Veraltet sind: „Seven styles“, 5 statt 10 Materialien, „Preview within 48 hours“. `it`: „consegna **in** 5 a 9 giorni“ ist falsch, richtig wäre „da 5 a 9“. | Alle vier Dateien gegen `de.default.json` abgleichen. |
| L3 | P1 | Markt „EU-Erweiterung“ (25 Länder) + Versandzone EU 13,99 € vs. **Versand-Richtlinie „nur DE, AT, CH“** | Der Rechtstext widerspricht dem, was der Checkout erlaubt [B]. | Richtlinie auf „DE, AT, CH, EU-Länder und ausgewählte weitere Länder (siehe Checkout)“ ändern, oder die Märkte und Zonen verkleinern. Das ist eine Geschäftsentscheidung. |
| L4 | P1 | `shipsToCountries` ≈ 240 Länder, verursacht durch die leeren Gelato-Profile mit ROW-Zonen [V] | Kunden aus nicht belieferten Ländern kommen eventuell bis in den Checkout und sehen dort „kein Versand verfügbar“ [V]. Auch der Merchant-Center-Feed kann falsche Länder übernehmen. | Die 10 leeren Gelato-Versandprofile löschen, aber nur nach Prüfung, ob die Gelato-App sie braucht. |
| L5 | P2 | AT liegt in der EU-Zone (13,99 €), obwohl DACH der Kernmarkt ist; Gratisversand gilt nur für DE | Österreich zahlt mehr als das Doppelte. Die Ansage „Lieferung in 5 bis 9 Werktagen“ nennt keine Versandkosten. | Eigene Zone AT, z. B. 7,99 € / gratis ab 150 €, oder bewusst so lassen (Geschäftsentscheidung). |
| L6 | P2 | Du/Sie | Deutsche Theme-Texte duzen durchgehend [B], nur „Ihr stimmt ab“ als Plural, das ist korrekt. FR verwendet „vous“, das ist dort kulturell üblich. Die Datenschutzerklärung (Shopify-Vorlage) siezt, die Versand-/Widerrufsrichtlinien duzen. | Optional: Datenschutz-Einleitung duzen. |
| L7 | P2 | `sections/footer.liquid` show_localization | Der Sprachwähler erscheint nicht (nur 1 Sprache), der **Länderwähler erscheint** (3 Märkte). Alles läuft in EUR, der Wähler bringt dem Kunden also nichts [V: CH eventuell CHF]. | Ausblenden, solange es keine Preis- oder Sprachunterschiede gibt. |
| L8 | P2 | `snippets/pmp-freebie-flyin.liquid` | Wird richtig nur bei `de` angezeigt [B]. | – |

### Tippfehler und Stil in `locales/de.default.json` [B]

- `products.preview.note`: „…nach der Bestellung**,** und schicken dir…“: Komma streichen.
- `password.enter`: „Eintreten“ wirkt ungewohnt. „Shop betreten“ oder „Weiter“ klingt natürlicher.
- `cart.shipping_bar.reached`: Emoji „🎉“. Da die Leiste aus ist, spielt das keine Rolle.
- `general.search.no_results`: „Versuch es mit …“. Umgangssprachlich, passt zur Du-Ansprache und ist ok.
- Sonst keine Tippfehler gefunden. Die Aussagen zu Lieferzeit, Spende und Vorschau sind im DE-Locale in sich stimmig, bis auf „13 Stile“ (s. 1c).

---

## 5. Performance-Befunde (mobil)

| # | Prio | Fundstelle | Problem | Fix |
|---|---|---|---|---|
| P1 | P1 [V] | `assets/theme.css` Zeile 819 `.has-grain::after` + `settings.enable_grain = true` | Das Vollbild-Overlay `position:fixed; inset:0; mix-blend-mode:multiply; z-index:9998` zwingt auf jedem Scroll-Frame den ganzen Viewport in die Blend-Komposition. Auf schwachen Android-Geräten ruckelt das typischerweise. | Auf Mobilgeräten abschalten: `@media (hover:none){.has-grain::after{display:none}}`, oder `mix-blend-mode` entfernen und nur die Opazität nutzen. In Lighthouse/DevTools prüfen. |
| P2 | P2 | `layout/theme.liquid` | Zwei render-blockierende CSS-Dateien: `theme.css` (90,7 KB, unminifiziert) und `pmp-222.css` (6,9 KB). | Zusammenführen und minifizieren. Critical CSS für den Hero ist optional. |
| P3 | ok | `assets/theme.js` | Scroll-Handler sind alle `passive:true` und laufen über rAF [B]. Tilt, Magnetic, Cursor, Spotlight nur bei `(hover:hover) and (pointer:fine)`, also mobil aus [B]. Reveal und Counter nutzen IntersectionObserver. Elemente im ersten Viewport werden sofort sichtbar geschaltet (kein LCP-Verzug). `prefers-reduced-motion` wird respektiert. | Nichts zu tun. |
| P4 | P2 | `theme.js` Reveal-Init | `getBoundingClientRect()` für jedes `[data-reveal]`-Element beim Laden, abwechselnd mit `classList.add`. Das kann Layout-Thrashing auslösen [V, gering]. | Erst alle Rects lesen, dann schreiben. |
| P5 | P2 | `snippets/image.liquid` | srcset 360–2400, `sizes` wird übergeben, lazy als Default, `image_tag` setzt width/height (kein CLS) [B]. Im Hero sind 3 Bilder `eager`, eins mit `fetchpriority=high` [B]. | Optional: Hero-Bild 2 und 3 auf mobil `lazy`, falls sie unter dem Fold liegen [V]. |
| P6 | P2 | `sections/marquee.liquid` + theme.js Zeile 490 | Die Duplikate werden per JS erzeugt und sind `aria-hidden`. Screenreader-Text ist vorhanden, reduced-motion wird beachtet [B]. Ohne JS läuft die Animation mit einer Lücke. | Unkritisch. |
| P7 | P2 | `assets/pmp-freebie-cover.png` (152 KB) | Wird **nirgends referenziert**. Alle Stellen nutzen `pmp-freebie-cover.jpg` (49 KB) [B]. | PNG löschen. Optional das JPG über `image_url` als WebP ausliefern (bei 247×320 wären es etwa 15 KB). |
| P8 | P2 | App-Embeds in `settings_data.json`: Judge.me core + Teeinblue personalizer, beide global aktiv | Judge.me lädt Skript und CSS auf allen Seiten, **obwohl es keine einzige Bewertung gibt** [B]. Teeinblue wird global geladen, gebraucht nur auf Produktseiten [V]. | Judge.me-Embed bis zu den ersten Bewertungen deaktivieren. Bei Teeinblue prüfen, ob der Embed auf Produktseiten beschränkt werden kann. |
| P9 | P2 | `layout/theme.liquid` | `product-form.js` und `live-preview.js` laden auf **allen** Produkt-Templates, auch auf Teeinblue-Produkten ohne eigene Live-Vorschau [B]. | Bedingt laden (nur wenn `[data-live-preview]` gerendert wird). |
| P10 | P2 | `sections/futterrechner.liquid` 111 KB inkl. Inline-Script/Style | Betrifft nur die Futterrechner-Seite, dort aber viel Inline-Code, der nicht gecacht wird. | JS/CSS in Assets auslagern (werden dann gecacht). |
| P11 | ok | Font-Preload | Heading- und Body-Font werden vorgeladen, alle `font_face` haben `font_display: swap` [B]. | – |
| P12 | [V] | scriptTags | Die Abfrage wurde verweigert (Access denied). Ob Legacy-Skripte von Printful, Printify oder Prodigi auf der Storefront geladen werden, ist **nicht geprüft**. | Im Browser (Netzwerk-Tab) oder mit erweitertem Scope prüfen. Nicht genutzte POD-Apps deinstallieren. |

---

## 6. Umsatz-relevante Querschnitts-Punkte

1. **P0: Blindflug.** Ohne GA4/Ads/Meta ist nicht messbar, welcher Kanal Umsatz bringt (T1–T3).
2. **P1: Gratisversand kommunizieren.** Real gilt „gratis ab 150 € in DE“, aber `cart_free_shipping_threshold = 0`, also sieht niemand die Fortschrittsleiste. Die Preise liegen bei 12,99–103,99 €, 150 € erreicht man nur mit mehreren Artikeln. Mit Leiste würde die Grenze als Anreiz für größere Warenkörbe wirken. **Achtung:** Das Setting gilt global. `cart-drawer.liquid` und `cart.js` müssten zusätzlich prüfen, ob `localization.country.iso_code == 'DE'`, sonst wird AT/EU Gratisversand versprochen. Eine Alternative ist eine geringere Grenze (die Theme-Kopie „79 €“ zeigt, dass das schon erwogen wurde).
3. **P1: Newsletter-10 %** (1i). Ein versprochener Rabatt ohne passenden Code beschädigt Vertrauen und bringt ggf. Abmahnrisiko.
4. **P1: Merchant Center / Google Shopping** fehlt komplett (keine Google & YouTube-App). Für Geschenkartikel vor Weihnachten ist das ein großer kostenloser Kanal.
5. P2: Die Judge.me-Bewertungsanfrage aktivieren. Es gibt null Bewertungen, damit auch keine Sterne im Suchergebnis und keinen Social Proof.

---

## 7. Aufräum-Liste

| # | Prio | Was | Detail |
|---|---|---|---|
| A1 | **P1** | **Theme-Kopien** | **18 Themes** (1 MAIN + 17 unveröffentlicht). Shopify erlaubt maximal 20, das Limit ist also fast erreicht [B]. Zum Löschen geeignet (älter als live und in ihm aufgegangen): „Karten-Label 18.09.“, „Kaufbutton Einzelvariante 18.09.“, „Aussagen 19.09.“, „Striche 19.09.“, „Striche 22.09.“, „**Versand 79 € 23.09.**“, „Gedruckt in Europa 23.09.“, „**Versand 150 € + elf Stile 23.09.**“, „Kopie von PMP: Versand 150 € + elf Stile“, „Futterrechner + Buch 26.09.“, „Futterrechner v3 + Wandbilder 27.09.“, „Optik 222“, „Optik 222 R3“, „227“. **Nicht ungeprüft löschen:** „**PMP: 233 (FREIGABE)**“ (zuletzt 27.09. 22:07 geändert, **nach** dem Live-Theme um 21:32, eventuell noch nicht übernommene Arbeit), „Futterrechner v5 (FREIGABE)“ (direkter Vorgänger), „**BACKUP MAIN 26.09. vor Futterrechner**“ (als Rückfall 1–2 Wochen behalten). |
| A2 | P1 | Leere Gelato-Versandprofile | 10 Profile mit 0 Varianten, aber mit ROW-Zonen (s. L4). Vorher klären, ob die Gelato-App sie neu anlegt. |
| A3 | P2 | Nicht genutzte POD-Apps | Printful, Printify und Prodigi sind installiert. `pod-routing.liquid` kennt nur gelato, prodigi, printify und manual. **Printful wird im Code nicht genutzt** [B]. Ob Prodigi und Printify genutzt werden: alle aktiven Produkte haben `pod.provider` = null, also greift der Default Gelato [B]. |
| A4 | P2 | Assets | `assets/pmp-freebie-cover.png` (unbenutzt) löschen. |
| A5 | P2 | Templates | `templates/product.personalized-fixed-style.json` wird von keinem aktiven Produkt genutzt und enthält Platzhaltertext. Löschen oder den Text korrigieren. |
| A6 | P2 | Section-Defaults mit veralteten Aussagen | `sections/faq.liquid` (48 h, 1–2 Tage Vorschau), `sections/how-it-works.liquid` (3–7 Werktage, Webdecke/Kissen), `sections/main-product.liquid` Zeile 565 (48 h), `sections/style-showcase.liquid` („13 Stile“), `sections/hero.liquid`. Sie wirken nur beim Neu-Hinzufügen einer Section, werden aber sonst später versehentlich live. Defaults angleichen. |
| A7 | P2 | Locales en/es/fr/it | Aktualisieren oder bewusst liegen lassen (s. L1/L2). |
| A8 | P2 | Blog `news` (0 Artikel) | Löschen. |
| A9 | P2 | Code-Kommentare „7 Stile“/„sieben Stile“ | `snippets/product-card.liquid` Zeile 36, `assets/theme.css` Zeile 988, `style-picker.liquid`-Fallback (7 Stile). |
| A10 | P2 | Hartkodierte „20 %“ | Rund 20 Textstellen statt `settings.donation_percent` (s. 1d). Bei einer Änderung des Prozentsatzes müssen alle angepasst werden. |

---

## 8. Konkrete Fix-Reihenfolge für die Umsetzungs-Session (Vorschlag)

1. **Entscheidungen vom Inhaber einholen** (nicht selbst treffen): (a) Wie viele Stile gelten offiziell, 12 + Original = 13? Gehören Anime und Herzensbild in `art_styles`? (b) Liefergebiet: nur DACH oder EU + International? Davon hängen Versand-Richtlinie, Märkte und FAQ ab. (c) Zufriedenheitsgarantie: Neudruck bei Nichtgefallen, ja oder nein? (d) Gibt es den 10-%-Willkommenscode, und welcher ist es? (e) Welche Theme-Kopien dürfen gelöscht werden?
2. **Tracking (P0)**: Google & YouTube-App installieren (manuell im Admin, das geht nicht per API). Theme-GA4-Feld leer lassen.
3. **Texte angleichen** (nur Theme-JSON und Seiten-SEO, geringes Risiko):
   - index FAQ „Druck 2 bis 4 … Versand 1 bis 3 … insgesamt 5 bis 9“ → z. B. „Druck 2 bis 4 Werktage, Versand 2 bis 5 Werktage, insgesamt meist 5 bis 9 Werktage“.
   - „Kein Zoll“ in index FAQ, faq.liquid und ueber-uns → „Innerhalb der EU kein Zoll. Bei Lieferungen in die Schweiz können Einfuhrabgaben anfallen.“
   - „Webdecke & Kissen kommen bald“ (so-funktionierts `how.h3`, page.faq „Welche Produkte gibt es?“) streichen bzw. aktualisieren.
   - FAQ „Versand“: den Satz zur Gelato-Sonderpauschale 5,69–10,99 € streichen, „weltweit“ durch „ausgewählte Länder außerhalb der EU“ ersetzen.
   - `product.json` acc2: Klammer „Ausnahme: einige gerahmte Poster …“ streichen.
   - Stil-Zahl nach Entscheidung (a) in `index.json` how.h2 und styles.eyebrow, `page.so-funktionierts.json` how.h2, de-Locale `step2_text`, Kollektions-SEO ×3 und `art_styles`.
   - Seiten-SEO `so-funktionierts`: „per E-Mail“ → „Live-Vorschau“.
4. **Schema-Fixes** (S1–S4) in `snippets/schema-org.liquid`.
5. **Gratisversand-Leiste** nur für DE (Umsatz, s. Abschnitt 6 Punkt 2).
6. **Performance**: Grain auf mobil aus, PNG löschen, Judge.me-Embed aus.
7. **Rechtstexte** (Versand- und Widerrufs-Richtlinie) an das echte Modell anpassen. Das ist Aufgabe des Inhabers bzw. Rechtsberatung, keine Theme-Änderung.
8. **Aufräumen** der Theme-Kopien nach Freigabe (A1).

---

*Nicht prüfbar aus dieser Umgebung:* Live-Rendering, PageSpeed/CWV-Messwerte, geladene Drittanbieter-Skripte (scriptTags: Access denied), Cookie-Banner-Konfiguration, Search-Console-Daten, Inhalt der Brevo-Mails.
