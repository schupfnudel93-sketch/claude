# ⚠️ DIESE DATEI IST FÜR: **CHATGPT / CODEX**
# NICHT für Claude (Cowork) und NICHT für Claude Code.

**Stand:** 03.10.2026 · **Absender:** Claude (Cowork-Analyse) im Auftrag von Tobi
**Ziel:** Print my Pet bekommt in 30 Tagen **200 echte Newsletter-Abonnenten** (Hundepost, Futterrechner, Welpen-Startpaket). Technik und Mailstrecken stehen. Es fehlen Publikum und Aufräumarbeiten.
**Du arbeitest im Browser (Brevo, Postiz, Shopify, YouTube). Claude hat dafür keine Klick-Rechte.**

---

## 0. Spielregeln (hart, bitte vor dem Start lesen)

1. **Nichts an echte Abonnenten senden.** Keine Kampagne versenden oder terminieren, bevor Tobi freigibt. Entwürfe sind erlaubt.
2. **Keine neuen laufenden Kosten.** Kein Tarif-Upgrade, kein neues Tool, kein Abo. Bei Bedarf Tobi fragen und die Kosten nennen.
3. **Vor jedem Löschen sichern.** Notiere ID, Name und Inhalt (Screenshot oder Export). Gelöscht wird nur, was unten ausdrücklich erlaubt ist.
4. **Postiz arbeitet in UTC.** Lokal ist bis 24.10.2026 CEST (UTC+2), ab 25.10.2026 CET (UTC+1). Beispiel: `15:00 UTC` = `17:00` lokal. Vor jedem Urteil über Uhrzeiten erst umrechnen.
5. **Amazon-Affiliate-Posts werden nie beworben.** Partner-Tag `printmypet-21`, Kennzeichnung „(Werbung / Partnerlink)" plus „Als Amazon-Partner verdiene ich an qualifizierten Verkäufen."
6. **Nichts raten.** Wenn eine Einstellung anders aussieht als beschrieben, stoppen, Screenshot machen und im Bericht melden.

---

## 1. Ausgangslage (von Claude geprüft, nicht neu erfinden)

**Brevo** (Konto tbpersonaltraining.nbg@gmail.com, Firma Creator Advisory S.L., Free-Tarif, ca. 300 Mails pro Tag):
- Sender „Print my Pet" = `newsletter@printmypet.de`.
- Listen: Hundepost (ID 16, 1 Kontakt), Futterrechner (ID 15, 7), Print my Pet – Welpen-Startpaket (ID 10, 5). Das sind überwiegend Testkontakte.
- 72 Kontakte insgesamt, davon 34 Test-Aliase (`name+xyz@gmail.com`).
- 149 Templates, darunter Altlasten. Eine Strecke von 14 Mails (Tag 2 bis Tag 365), Willkommen F1 bis F6, Hundepost G1 bis G3.
- **Hundepost-Entwürfe Nr. 1 bis 6** (Kampagnen-IDs 184 bis 189), Nr. 1 für **Di 13.10.2026, 18:30**, danach alle 2 Wochen.
- Gesund aus der Erde: Ausgabe „September 2" (Kampagne 75) steht auf **suspended**.

**Postiz:** 19 Kanäle. Bei Print my Pet sind 791 Posts eingeplant oder als Entwurf, davon **493 ohne sichtbaren Hinweis auf Futterrechner, Hundepost, Startpaket oder Shop**. Die Liste liegt in `postiz_pmp_cta_analyse_2026-10-03.csv` (Spalte `hat_cta_0_1` = 0 heißt: kein CTA im Text).

**Fehlgeschlagen (ERROR), ohne Fehlermeldung:**
- Pinterest `printmypetinfo`, 4 Pins (Tassen), 01.10., ca. 20:57–20:58 UTC.
- Facebook `PrintmyPet`, 27.09., 16:00 UTC.
- Threads `print.mypet`, 30.09., 15:00 UTC.
- Instagram `Print my Pet`, 30.09., 16:00 UTC.
- (Nicht Print my Pet, nur der Vollständigkeit halber: TB Personal Training Instagram 25.09. und Pinterest 13.09.)

---

## 2. Aufträge, nach Priorität

### A. Brevo aufräumen (Aufwand ca. 30 Minuten)

A1. **Sicher löschen:** Template **149** („TEST Enthaelt-Pruefung (loeschen)").

A2. **Erst prüfen, dann löschen:** Templates **148, 150 bis 155** („Futterrechner Ergebnis" und v2 bis v6) sowie **157** und **165** (inaktiv).
- Öffne Automationen/Workflows und finde heraus, welche Version die Futterrechner-Strecke wirklich nutzt (vermutlich v6, ID 154).
- **Diese eine Version behalten**, jedes Template löschen, das in **keiner** Automation und keiner Kampagne vorkommt.
- Melde in einer Tabelle: ID, Name, „in Nutzung ja/nein", „gelöscht ja/nein".

A3. **Testkontakte kennzeichnen statt löschen:** Lege ein Attribut oder Tag `TEST` an und setze es bei allen 34 Kontakten mit `+` in der Adresse. Diese Adressen brauchen wir noch zum Testen. Ziel: Echte Abonnenten lassen sich später sauber filtern.

A4. **Leere Listen löschen**, nur diese: „Ihre erste Liste" (ID 2), „Creator Advisory" (ID 6), „In Ruhe erklärt – Newsletter" (ID 14), „VyriaGym – Trainingsplan-Anfragen" (ID 12). **Nicht löschen:** „Bücher-Warteliste GadE" (ID 13) und alle Listen mit Kontakten.

A5. **Hundepost Nr. 1 (Kampagne 184) prüfen, nicht senden:** Empfängerliste, Absender, Betreff, Vorschautext, Links und UTM-Parameter. Melde, ob eine Empfängerliste hinterlegt ist und ob Testkontakte mitgehen würden.

A6. **Gesund aus der Erde, Kampagne 75 „September 2" (suspended):** Finde den Grund heraus (Zustellfehler, Limit, manuell gestoppt) und melde ihn. **Nicht fortsetzen**, Tobi entscheidet.

A7. **Anmeldeformulare prüfen:** Gibt es für Hundepost, Futterrechner und Welpen-Startpaket aktive Formulare? Welche URLs, welche Liste, Double-Opt-in an? Lege die Links in eine Tabelle. Die brauchen wir in Abschnitt C.

### B. Postiz: nur Verbindung prüfen, nichts Altes ändern

**Tobi-Vorgabe (03.10.): Keine alten Posts ändern.** Weder veröffentlichte noch fehlgeschlagene noch bereits eingeplante Posts werden angefasst, nicht in Text, Medien, Datum oder Kanal. Der bestehende Plan bleibt exakt so, wie er ist.

B1. **Pinterest prüfen:** Ist `printmypetinfo` noch verbunden, oder meldet Postiz „Verbindung abgelaufen"? Bei Ablauf neu verbinden. Teste nur mit einem **neuen** Entwurf.

B2. **Die 7 fehlgeschlagenen Print-my-Pet-Posts** (siehe Abschnitt 1): **nicht neu planen und nicht ändern.** Nur melden, was die Ursache ist, und Tobi entscheiden lassen.

### C. Anmelde-CTAs: nur für die Zukunft

Grundlage: `postiz_pmp_cta_analyse_2026-10-03.csv`. Sie dient **nur zur Orientierung**, welche Kanäle bisher zu wenig CTA haben.

1. **Bestehende Posts (Queue, Entwürfe, veröffentlicht): nicht bearbeiten.**
2. **Gilt ab jetzt für alle neuen Posts**, die wir nach dem bereits geplanten Zeitraum anlegen. Der letzte eingeplante Termin steht in der CSV (Spalte `publish_utc`, größter Wert). Neue Posts beginnen danach und bekommen ihren CTA direkt beim Anlegen.
3. **Vorschläge statt Änderungen:** Füge der CSV eine Spalte `cta_vorschlag` hinzu, mit einem Satz je Post ohne CTA aus den nächsten 14 Tagen. Angewendet wird nichts, Tobi gibt später frei.
4. **Link-in-Bio prüfen** (Instagram, TikTok, Threads): Zeigt er auf Futterrechner oder Startpaket? Wenn nicht, melden. Das ist die schnellste Hebelwirkung, ohne einen einzigen Post zu ändern.

**Texte für künftige Posts:** Ein CTA pro Post, ein Satz, ohne Marktschreierei. Beispiele zum Anpassen:
- Hund: „Wie viel Futter braucht dein Hund wirklich? Der kostenlose Futterrechner: [Link]"
- Welpe: „Gratis Startpaket für die ersten 30 Tage mit Welpe: [Link]"
- Instagram/TikTok: „Link in Bio".
- YouTube (neue Videos): Anmelde-Link und Satz in die Beschreibung, **vor** den drei Hashtags am Ende. Tags 5 bis 12, keine Hashtags im Titel.

### D. Shopify und Website (nur Klick-Einstellungen, kein Code)

D1. **Einwilligung zur Hundepost bei der Bestellung:** Einstellungen → Kasse → Marketing-Optionen. „E-Mail-Marketing" im Checkout anbieten, Häkchen nicht vorausgewählt (DSGVO). Melde, ob das schon aktiv ist.

D2. **Shopify mit Brevo verbinden**, falls die Integration fehlt: Kunden mit Einwilligung sollen automatisch in die Hundepost-Liste. Melde den Stand und ob die Integration kostenlos ist. **Keine kostenpflichtige App installieren.**

D3. **Footer-Anmeldung:** Gibt es im Footer und im Blog („Magazin") ein Anmeldefeld für die Hundepost? Wenn nicht, nur melden. Das baut Claude Code als Code-Änderung.

### E. Gesund aus der Erde (Nebenschauplatz, nur Prüfung)

Melde nur: Wie viele aktive Abonnenten hat Liste 9? Gibt es in den letzten 3 Ausgaben einen Button mit einem Produktangebot? Wenn nein, Tobi informieren. Wir bauen das später um.

---

## 3. Bericht an Tobi (am Ende, kurz)

Ein Dokument mit:
1. Tabelle der gelöschten und behaltenen Templates und Listen (ID, Name, Grund).
2. Ergebnis zu A5 bis A7 (Hundepost Nr. 1, Kampagne 75, Formular-Links).
3. Postiz: Ursache der Fehler und Stand der Pinterest-Verbindung (keine Posts geändert).
4. Ergebnis zu Link-in-Bio und die Spalte `cta_vorschlag` in der CSV.
5. Alles, was nicht wie beschrieben aussah, mit Screenshot.
6. Offene Entscheidungen für Tobi, jeweils mit deiner Empfehlung.

**Nicht tun:** Kampagnen senden, Tarife ändern, Kontakte außerhalb von A3 und A4 löschen, Amazon-Posts bewerben, bestehende Posts ändern oder neu planen, Texte neu erfinden, die bestehenden Mailstrecken umbauen. Die sind fertig und im Markenstil gebaut.
