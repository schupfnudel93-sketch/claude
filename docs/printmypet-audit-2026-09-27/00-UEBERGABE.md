# PrintMyPet: Komplett-Audit vom 27.09.2026, Übergabe an Claude

**Lies das hier zuerst.** Dieses Dokument ist die Zusammenfassung. Die Details mit Datei, Zeile oder Selektor und fertigen Fix-Vorschlägen (oft mit Code) stehen in den sechs Teilberichten in diesem Ordner.

| Datei | Bereich |
|---|---|
| `01-layout-navigation.md` | Header, Mobilmenü, Footer, Ankündigungsleiste, globales CSS/JS, Drawer, Accessibility |
| `02-startseite.md` | Alle 15 Sektionen der Startseite, 5-Sekunden-Test mobil, neue Reihenfolge |
| `03-produkt-warenkorb.md` | Produktkatalog, Produktseite, Upload/Stil/teeinblue, Warenkorb, Preisangaben, Widerruf |
| `04-katalog-collections-suche.md` | Alle 47 Collections, Menübaum, Filter, Produktkarten, Suche, 404, Soll-Navigation |
| `05-inhaltsseiten.md` | Alle Pages, Rechtstexte, Futterrechner, Bücher, YouTube, Tierschutz, Blog, Formulare, Freebie |
| `06-seo-tracking-konsistenz.md` | Tracking, SEO, Sprachen/Märkte, Performance, Widerspruchs-Tabelle, Aufräumliste |

---

## 1. Wie geprüft wurde und was das bedeutet

- **Nur gelesen, nichts geändert.** Die Quellen waren das Live-Theme `PMP: Futterrechner v5.1 (FREIGABE)` (`gid://shopify/OnlineStoreTheme/208344613202`, alle 130 Dateien) und die Admin-Daten per Shopify-MCP: Produkte, Collections, Menüs, Pages, Policies, Versandprofile, Rabatte, Sprachen, Märkte und Apps.
- **Die Website selbst war nicht erreichbar**, weil das Netzwerk der Cloud-Umgebung sie gesperrt hat. Deshalb gibt es **keine Screenshots**. Pixelwerte zu mobilen Ansichten sind **aus dem CSS berechnet**.
- In den Teilberichten ist jeder Befund markiert: **BELEGT** (Code oder Daten gesehen), **BERECHNET** bzw. **SCHÄTZUNG** (aus CSS abgeleitet) oder **VERMUTET** (erst live bestätigen, dann fixen).
- Pfade wie `scratchpad/theme/` oder `scratchpad/data/` in den Teilberichten gibt es in einer neuen Session nicht mehr. Die Dateien bei Bedarf neu per MCP laden.
- Prioritäten: **P0** = kaputt, kostet jetzt Umsatz oder ist ein Rechtsrisiko. **P1** = wichtig für Conversion und Verständlichkeit. **P2** = Feinschliff.
- **Eine Korrektur aus der Gegenprüfung:** `05-inhaltsseiten.md` P0-4 behauptete, für Wandbilder gäbe es eigene Gelato-Versandpreise. Das ist **falsch**. Alle 19 Gelato-Profile haben 0 Varianten, für alle Produkte gilt das Standardprofil. Im Bericht ist die Stelle markiert.

### Die echten Fakten (Soll-Werte, gegen die alle Texte geprüft wurden)

| Thema | Wert laut Admin |
|---|---|
| Versand Deutschland | Standard 5,99 €, **gratis ab 150 €**, Express 9,99 € |
| Versand EU (26 Länder) | 13,99 €, keine Gratisgrenze |
| Versand International (14 Länder inkl. CH, UK, US) | 19,99 € |
| Produkte | 322 gesamt, **120 aktiv** (117 im Onlineshop veröffentlicht), 12 Entwürfe, 190 archiviert, davon 97 aktiv über teeinblue. Preise 12,99 bis 103,99 € |
| Collections | 47, keine davon leer verlinkt |
| Sprachen | nur `de` veröffentlicht (`en` angelegt, aber unveröffentlicht; es/fr/it gar nicht angelegt) |
| Stile | Theme-Setting 11, Stil-Collections 12 + Original = 13, teeinblue 13 |
| Rabattcodes | nur `LKW10`, `DANKE10`, dazu die automatische „Weihnachtsaktion –15 %“ (geplant) |
| Bewertungen | **0** bei allen aktiven Produkten (Judge.me installiert) |
| Tracking | GA4-ID leer, **keine** App „Google & YouTube“ oder „Facebook & Instagram“ |
| Themes | 18 von maximal 20 belegt |

---

## 2. Das Wichtigste in einem Satz

**Der Shop sieht durchdacht aus. Aber der wichtigste Button („Portrait gestalten“) führt auf eine fast leere Seite, auf ~97 Produkten fehlen vermutlich Pflichtangaben und es wird falsch bestellt, und ohne Tracking sieht niemand, was Umsatz bringt.** Das zuerst zu beheben bringt mehr als jeder Design-Feinschliff.

---

## 3. P0: sofort (Umsatz, Funktion, Recht)

| # | Problem | Belegt? | Details | Fix (Kurzform) |
|---|---|---|---|---|
| 1 | **„Portrait gestalten“ führt auf `/collections/schaufenster`: 20 von 22 Produkten archiviert**, übrig sind nur Kalender und Digitalportrait. Betroffen sind Header-CTA, Mobilmenü, Hero, Vorher/Nachher, So funktioniert's, leerer Warenkorb, Warenkorb-Cross-Sell und 404-Empfehlungen. | BELEGT | 02 P0-1, 03 P0-04, 04 P0 | Ziel-Collection festlegen (Inhaberentscheidung 6), `schaufenster` neu befüllen oder alle Links umstellen (`header-group.json` `cta_url`, `index.json`, `page.so-funktionierts.json`, `settings.cart_cross_sell_collection`, 404). |
| 2 | **teeinblue-Produkte (68): versteckte Theme-Felder werden mitgesendet.** Stil „Aquarell“ ist vorausgewählt, die Felder sind nur per CSS versteckt. Folge: falscher Stil in der Bestellung. | VERMUTET, Testkauf | 03 P0-01 | Testkauf und `/cart.js` ansehen. Theme-Felder in `main-product.liquid` bei teeinblue-Template gar nicht rendern (nicht nur per CSS verstecken). |
| 3 | **„inkl. MwSt. zzgl. Versand“ fehlt wahrscheinlich bei 97 teeinblue-Produkten** (Pflichtangabe, Abmahnrisiko) | VERMUTET, hohe Sicherheit | 03 P0-02 | CSS-Ausblendung für den Steuerhinweis zurücknehmen oder den Hinweis außerhalb des versteckten Containers ausgeben. |
| 4 | **Digitalprodukt ohne Zustimmung zum Erlöschen des Widerrufsrechts** (`dein-hund-als-digitales-kunstwerk-sofort-download`) | BELEGT | 03 P0-03 | Snippet `widerruf-digital` einbinden, Wortlaut rechtlich prüfen lassen. |
| 5 | **Cross-Sell im Warenkorb legt personalisierte Produkte ohne Foto und Stil ab** (1 Klick) | BELEGT | 03 P0-05 | Bei personalisierten Produkten „Ansehen“ statt „Hinzufügen“, oder nicht personalisierte Cross-Sell-Collection nutzen. |
| 6 | **Falsche Versprechen:** „Live-Vorschau“ auch beim Kalender (hat keine), dazu „Vorschau per E-Mail in 48 h“. Die Widerrufs-Policy beschreibt einen anderen Ablauf. | BELEGT | 03 P0-06, 05 P1-3 | Aussage produktabhängig machen, Policy an den echten Ablauf mit teeinblue-Live-Vorschau anpassen. |
| 7 | **Mobiler Header ist bei 360 bis 375 px zu breit, das Warenkorb-Icon wird abgeschnitten** (Logo 62 px hoch plus 4 Icons plus Abstände) | BERECHNET, live prüfen | 01 N-1 | Logo mobil etwa 40 px, Abstände kleiner, YouTube- und Konto-Icon mobil ausblenden. |
| 8 | **Globale CSS-Regel („Mobile-Diät“) versteckt Inhalte auf allen Seiten:** Bücher ab Nr. 4 und FAQ ab Frage 6 fehlen, der Bücher-Filter „Katze/Pferd/Kinder“ zeigt nichts. | BELEGT | 05 P0-1, 02 P1-9, 01 N-18 | Regel in `assets/theme.css` auf `.template-index` begrenzen (1 Zeile) und auf der Startseite „Mehr anzeigen“ ergänzen. |
| 9 | **So funktioniert's: alle 15 verlinkten Produkte sind archiviert**, die Produktleiste ist leer | BELEGT | 05 P0-6 | `mat-*`-Blöcke aus `page.so-funktionierts.json` löschen, dann greift der Collection-Fallback. |
| 10 | **Keine Umsatzmessung** (GA4 leer, keine Google- oder Meta-App) | BELEGT | 06 | Die App „Google & YouTube“ installieren (GA4, Ads-Conversions, Merchant Center mit kostenlosen Shopping-Einträgen), dazu die App „Facebook & Instagram“. Die ID **nicht** zusätzlich im Theme eintragen (sonst doppelte Messung, und der Theme-Code misst keine Käufe). |
| 11 | **Rechtstexte:** AGB sind die englische US-Vorlage, die Datenschutzerklärung ist die Standardvorlage (ohne AdSense, YouTube, Brevo, Amazon, Judge.me, teeinblue), das Impressum ist unvollständig | BELEGT (Inhalt) | 05 P0-2/3/7 | **Nicht selbst formulieren.** Rechtstexte-Dienst (z. B. Händlerbund, IT-Recht Kanzlei, eRecht24) oder Anwalt. Braucht Firmendaten vom Inhaber. |
| 12 | **Versand-Angaben widersprechen sich:** Policy sagt „nur DE/AT/CH“, geliefert wird in die EU und 14 weitere Länder. Der FAQ-Satz „gerahmte Poster 5,69–10,99 €“ ist veraltet, „kein Zoll“ gegenüber der Schweiz. | BELEGT | 03 P1-10, 05 P0-4, 06 | FAQ `f5` und `product.json` `acc2` korrigieren, Policy an die Zonen anpassen (Inhaberentscheidung 2). |
| 13 | **Anlass-Collections ohne Kernprodukt:** „Weihnachten“ (60), „Geburtstag“ (39) und „Mit deinem Foto“ enthalten **kein einziges Wandbild**, obwohl die Texte Leinwände versprechen. „Poster ab knapp 15 €“ ist falsch (ab 23,99 €). `tierportraits` hat 1 Live-Produkt. | BELEGT | 04 P0 | Wandbilder aufnehmen (Smart-Collection-Regeln), Texte korrigieren. **Vor Weihnachten besonders wichtig.** |
| 14 | **Amazon:** `amazon_tag` ist leer, 9 von 10 Buchlinks ohne Partner-Tag. Die Blogübersicht sagt „kein Amazon-Partner“, 36 Artikel nutzen aber `tag=printmypet-21`. | BELEGT | 05 P0-5 | `amazon_tag` = `printmypet-21` setzen (vom Inhaber bestätigen lassen), Kennzeichnung vereinheitlichen, `rel="sponsored"`. |

---

## 4. P1: wichtig (gruppiert, Details in den Teilberichten)

**Navigation und Header (01, 04)**
- Kein Kauf-CTA im mobilen Header („Portrait gestalten“ ist unter 990 px und bei 1280 bis 1439 px versteckt).
- Mega-Menü-Promos werden nie angezeigt, weil `menu_title` „Tierportraits“/„Produkte“ zu keinem Menüpunkt passt.
- Falsche Ziele: „Alle Stile ansehen“ führt auf Aquarell, „Alle Geschenke“ auf Weihnachten. Dazu kaputte Labels wie „Alle Für dein Zuhause ansehen“.
- Das Menü hat 10 Punkte, es fehlen Sonderwunsch, Handyhüllen/Mauspads/Taschen, Gutschein, Wunschbild und ein Einstieg **nach Tierart**. So funktioniert's und Tierschutz liegen zu tief. **Die Soll-Struktur mit 6 Oberpunkten steht in 04 §4.2.**
- Die Schnellsuche findet keine Seiten (Futterrechner, Sonderwunsch, Kontakt …).
- Die Ankündigungsleiste ist mobil ein wischbarer Streifen, die zweite Botschaft (Lieferzeit) sieht praktisch niemand.
- Drawer-JS: Wird ein Drawer geschlossen und innerhalb von 500 ms wieder geöffnet, verschwindet er und die Seite bleibt gesperrt. Fokus springt nicht zurück, `aria-expanded` fehlt.
- iPhone-Home-Balken (Safe Area) im Warenkorb-Drawer und Fly-in nicht berücksichtigt.

**Startseite (02)**
- Mobil ist above the fold **kein Tierbild** zu sehen (Text vor Bild, Bilder erst ab ca. 850 px). Die YouTube-Karte im Hero führt aus dem Shop.
- **Kein „ab X €“** auf der ganzen Startseite (Poster ab 23,99 €, Tassen ab 19,99 €), keine Versandkosten, keine Garantie.
- Die Seite ist mobil **ca. 17.000 bis 19.000 px lang (22 bis 25 Bildschirme)**. Doppelt sind 2 Stil-Galerien mit denselben 12 Bildern, 2 × „3 Schritte“ und 2 × Freebie. Am Ende folgen YouTube, Blog und Bücher hintereinander.
- Das Sortiment (Tassen, Kleidung, Decken, Kissen, Gutschein, Digital) wird kaum gezeigt. Es gibt kein Geschenk- oder Weihnachtsmodul, obwohl „–15 %“ bereits geplant ist.
- Kein Social Proof, keine dauerhafte mobile CTA-Leiste.
- **Die neue mobile Reihenfolge steht in 02 §5.**

**Produkt und Warenkorb (03)**
- Kein Schutz gegen Doppeltipp beim Warenkorb-Button. Die Sticky-Kaufleiste kommt zu spät und fehlt bei teeinblue vermutlich ganz.
- Galerie: kein Wischen, kein Zoom. Express-Checkout überspringt den Foto-Upload.
- Upload: keine Rückmeldung zu Fortschritt und Fehlern, HEIC-Problem.
- Im Warenkorb-Drawer liegt „Zur Kasse“ mobil unter dem sichtbaren Bereich. **Die Gratisversand-Leiste ist aus** (Schwelle 0), obwohl ab 150 € gratis verschickt wird.
- 13 Shirt-Produkte haben 256 Varianten, Liquid liefert nur 250.
- Digitalprodukt: „sofort“, „24 h“ und „nach Freigabe“ gleichzeitig.
- 14 aktive Produkte haben höchstens 2 Bilder. Der Tierschutz-Hinweis fehlt auf Produktseiten.

**Collections und Suche (04)**
- Filter mobil: Jede Checkbox lädt die Seite neu und der Drawer schließt sich.
- Produktkarten sind randlos 4:5, dadurch verlieren Querformat-Textilbilder ca. 44 %. Badges erscheinen nie (keine Tags `Bestseller`/`Neu`). Die Stilzahl auf Karten und im Titel widerspricht sich.
- Collection-Intro mit zusammengeklebtem Text („einmal.Welche“), weil `strip_html` die Leerzeichen verschluckt.
- 404: „Bestseller ansehen“ führt auf `/all`, ein Suchfeld fehlt. „Sofort lieferbare Motive“ ist bei Print-on-Demand irreführend. `wandbilder` beginnt mit 13 Pferden.
- 15 Collections ohne SEO-Titel und -Beschreibung.

**Inhaltsseiten (05)**
- Futterrechner, Tierschutz, Über uns, YouTube und Bücher haben **keinen einzigen Link in den Shop**.
- Das Welpen-Startpaket-Fly-in erscheint auch auf Shop- und Formularseiten, der Schließen-Button ist unter 44 px.
- Tierschutz: Anker `#voting` führt ins Leere, der Zeitplan widerspricht sich. Der YouTube-Sync steht seit 05.09.
- DSGVO: YouTube-iframe im Artikel `welpenerziehung-5-regeln` ohne Einwilligung. AdSense bringt ohne TCF-zertifizierte Consent-Lösung im EWR kaum Umsatz. Der Futterrechner koppelt Ergebnis und Newsletter-Einwilligung und meldet immer Erfolg.
- Sonderwunsch-Formular ohne Foto-Upload, die versprochene „Anfrage-Nummer“ wird nicht erzeugt.
- „Kissen und Decken kommen bald“, obwohl 14 davon aktiv sind.

**Widersprüche in den Texten (06, Tabelle dort)**
- Stile 11 / 12 + Original / 13 / „sieben“ (Fremdsprachen).
- Lieferzeit: „5–9 Werktage“, dagegen FAQ „2–4 + 1–3“ und Schema 4–10.
- Garantie „wir drucken neu“ gegen „nur bei Druckfehlern“.
- Newsletter verspricht 10 %, einen passenden Willkommenscode gibt es nicht.
- Marke in 3 Schreibweisen.
- Strukturierte Daten nennen 5 Sprachen, veröffentlicht ist nur Deutsch.

---

## 5. Entscheidungen des Inhabers (vor der Umsetzung klären)

Diese Punkte **nicht selbst entscheiden**, sondern gesammelt abfragen (am besten mit AskUserQuestion):

1. **Offizielle Stilzahl und -liste** (11 im Theme oder 13 wie teeinblue und Collections, inklusive Anime und Herzensbild?)
2. **Liefergebiet:** Policy auf EU plus International erweitern, oder Zonen auf DE/AT/CH einschränken?
3. **Umfang der Zufriedenheitsgarantie** (Neudruck immer oder nur bei Druckfehlern?)
4. **Newsletter-Willkommenscode** anlegen (z. B. `WILLKOMMEN10`, auch in Brevo) oder das 10-%-Versprechen streichen?
5. **Welche Theme-Kopien gelöscht werden dürfen** (18/20; „PMP: 233“ wurde nach dem Live-Theme geändert, nicht ungeprüft löschen)
6. **Ziel von „Portrait gestalten“:** `schaufenster` neu befüllen oder eine neue Collection wie `portrait-gestalten` mit allen aktiven Foto-Produkten?
7. **AdSense:** TCF-Consent-Tool einbauen oder abschalten?
8. **Rechtstexte:** welcher Dienst bzw. Anwalt, dazu Firmendaten (Geschäftsführer, Register, USt-IdNr.)
9. **Gratisversand-Schwelle** 150 € behalten (Theme-Kopien hießen auch „Versand 79 €“)? Danach die Leiste im Warenkorb einschalten.
10. **Amazon-Tag** `printmypet-21` bestätigen.

---

## 6. So arbeitest du morgen (Arbeitsregeln)

1. **Zuerst live prüfen.** Wenn `printmypet.de` und `cdn.shopify.com` im Netzwerk der neuen Session erlaubt sind (vom Inhaber eingetragen, bitte mit `curl -sI https://printmypet.de` testen), per Playwright Screenshots bei **360, 390 und 430 px** machen (Chromium liegt unter `/opt/pw-browsers`). Seiten:
   - Startseite, `/collections/schaufenster`, `/collections/wandbilder`, eine Stil-Collection
   - ein teeinblue-Produkt, das Digitalprodukt, der Kalender
   - Warenkorb-Drawer mit Artikel, `/pages/so-funktionierts`, `/pages/faq`, `/pages/buecher`, `/pages/futterrechner`, `/pages/tierschutz`
   - Suche, 404, Footer

   Damit alle als VERMUTET oder BERECHNET markierten Befunde bestätigen oder verwerfen. Den Testkauf bis zum Checkout durchführen, **nicht bezahlen**, dabei `/cart.js`-Properties prüfen (P0-2).
2. **Nie direkt am Live-Theme arbeiten.** Live-Theme duplizieren, benennen nach dem bestehenden Muster `PMP: Audit-Fixes 28.09. (FREIGABE)`, Änderungen per `themeFilesUpsert` nur auf der Kopie. Vorschau: `https://printmypet.de/?preview_theme_id=<ID>`. Veröffentlichen tut der Inhaber.
3. **Admin-Daten (Collections, Menüs, Policies, Produkte, Settings) wirken sofort live.** Diese Änderungen vorher als Liste zeigen und bestätigen lassen.
4. Theme-Limit beachten: 18/20. Vor dem Duplizieren ggf. eine alte Kopie löschen lassen (Inhaberentscheidung 5).
5. Blogartikel nur nach dem Skill `printmypet-blog` ändern.
6. Rechtstexte nicht erfinden (siehe P0 #11).

### Empfohlene Reihenfolge

1. **Schnelle Theme-Fixes (Kopie):** CSS-Regel auf `.template-index` begrenzen (P0 #8), `mat-*`-Blöcke löschen (#9), Header mobil (#7), Steuerhinweis teeinblue (#3), versteckte Felder bei teeinblue nicht rendern (#2), Widerrufs-Checkbox Digitalprodukt (#4), Cross-Sell (#5), Doppeltipp-Schutz, Drawer-Race-Condition, Safe Area.
2. **Admin-Daten nach Freigabe:** „Portrait gestalten“-Ziel (#1), Anlass-Collections mit Wandbildern (#13), Menü nach Soll-Struktur 04 §4.2, `amazon_tag` (#14), Gratisversand-Leiste, SEO-Texte für 15 Collections.
3. **Texte vereinheitlichen:** Stile, Lieferzeit, Versand, Garantie, Vorschau, „kommt bald“, Newsletter-Code (06 Widerspruchs-Tabelle ist die Checkliste).
4. **Startseite umbauen** nach 02 §5 (Bild zuerst, „ab X €“, Produktwelten, Weihnachtsmodul, Dubletten raus, Sticky-CTA mobil).
5. **Inhaltsseiten → Shop-CTAs**, Fly-in-Ausschlüsse, Tierschutz-Anker, YouTube Zwei-Klick-Lösung.
6. **Tracking-Apps** installieren (Inhaber, dauert etwa 15 Minuten).
7. **P2-Feinschliff** aus den Teilberichten.

---

## 7. Umsatz-Hebel über die Fehlerbehebung hinaus

Nach Wirkung sortiert, für Q4 2026 (Weihnachten ist für personalisierte Tiergeschenke die Hauptsaison):

1. **Tracking zuerst.** Ohne GA4 und Meta-Pixel ist jede Werbe- und Designentscheidung blind. Mit der App „Google & YouTube“ gibt es außerdem **kostenlose Google-Shopping-Einträge**, der günstigste Zusatzkanal.
2. **Weihnachts-Countdown:** „Bestellen bis [Stichtag] = Lieferung vor Heiligabend“ (Stichtag mit Gelato/teeinblue-Produktionszeiten abstimmen) in der Ankündigungsleiste, auf Produktseiten und im Warenkorb. Dazu ein Geschenk-Modul auf der Startseite und die –15-%-Aktion sichtbar machen. Personalisierte Produkte verkaufen sich in Q4 über **Deadlines**.
3. **Bewertungen aufbauen:** Judge.me-Bewertungsanfrage nach Lieferung aktivieren (am besten mit Foto-Anreiz, z. B. 10 % auf die nächste Bestellung). Bisherige Kunden direkt anschreiben. Null Bewertungen ist aktuell der größte Vertrauensmangel.
4. **Warenkorbwert:** Gratisversand-Leiste einschalten. 150 € ist bei Preisen ab 23,99 € sehr hoch, **79 € testen** (dafür gab es schon eine Theme-Kopie). Bundles „gleiches Motiv als Tasse oder Poster“ anbieten, weil das Motiv schon fertig ist und der Zusatzverkauf fast nichts kostet. Nach dem Kauf das Digitalportrait als Upsell anbieten.
5. **Den Content-Traffic zu Geld machen:** Futterrechner, 57 Blogartikel, Bücher und YouTube bringen Besucher, führen aber nicht in den Shop. Pro Seite ein passendes Produktmodul einbauen („Dein Hund als Kunstwerk, ab 23,99 €“). Den Amazon-Tag setzen, dann zahlen die Buchlinks sofort Provision.
6. **Aufräumen spart Zeit:** 190 archivierte Produkte, 18 Themes, 19 leere Gelato-Versandprofile (blähen die Lieferländer auf ca. 240 auf), ungenutzte Apps (Printful, Printify oder Prodigi prüfen, ob noch nötig). Weniger Altlasten bedeuten weniger Fehler wie beim `schaufenster`-Fall.
7. **Fremdsprachen:** Die en/es/fr/it-Dateien sind veraltet und nicht veröffentlicht. Entweder richtig machen (Translate & Adapt, EU-Markt ist aktiv) oder die Angaben aus den strukturierten Daten entfernen. Nicht halb.
