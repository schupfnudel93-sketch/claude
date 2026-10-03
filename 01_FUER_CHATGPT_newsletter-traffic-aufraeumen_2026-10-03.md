# ⚠️ DIESE DATEI IST FÜR: **CHATGPT / CODEX**
# NICHT für Claude (Cowork) und NICHT für Claude Code.

**Stand:** 03.10.2026 · **Absender:** Claude (Cowork-Analyse) im Auftrag von Tobi
**Thema:** NUR das Newsletter-System von Print my Pet (Brevo, Anmeldung, Mailstrecken). **Nicht** Social, Postiz, Blog, YouTube oder Shop-Inhalte. Das ist bewusst ausgeklammert.
**Ziel:** Das System ist sauber, zustellbar und sendebereit, damit echte Abonnenten kommen können.
**Du arbeitest im Browser (Brevo, Shopify-Einstellungen). Claude hat dafür keine Klick-Rechte.**

---

## 0. Spielregeln (hart)

1. **Nichts an echte Abonnenten senden und keine Kampagne terminieren**, bevor Tobi freigibt. Entwürfe bearbeiten ist erlaubt.
2. **Keine neuen laufenden Kosten.** Kein Tarif-Upgrade, keine kostenpflichtige App. Bei Bedarf Tobi fragen und die Kosten nennen.
3. **Vor jedem Löschen sichern** (ID, Name, Screenshot). Gelöscht wird nur, was unten ausdrücklich erlaubt ist.
4. **Keine DNS-Änderungen** ohne Tobi. Du prüfst und meldest, ändern tut Tobi.
5. **Bestehende Mailstrecken nicht umbauen.** Design und Texte sind fertig (Markenstil Print my Pet). Es werden nur die unten genannten Fehler behoben.
6. **Nichts raten.** Sieht eine Einstellung anders aus als beschrieben: Screenshot, im Bericht melden.

---

## 1. Ausgangslage (von Claude geprüft)

**Brevo-Konto** tbpersonaltraining.nbg@gmail.com (Creator Advisory S.L., Free-Tarif, ca. 300 Mails pro Tag). Sender „Print my Pet" = `newsletter@printmypet.de`, aktiv.

**Listen (Print my Pet):** Hundepost (ID 16, 1 Kontakt), Futterrechner (ID 15, 7), Welpen-Startpaket (ID 10, 5). Überwiegend Testkontakte. Insgesamt 72 Kontakte, davon 34 Test-Aliase (`name+xyz@gmail.com`).

**Mailstrecken (Templates), von Claude geprüft:** 44 Vorlagen, alle Abmeldelinks vorhanden (wo nötig), alle Amazon-Links mit Partner-Tag `printmypet-21`, Variablen sauber. **Keine Fehler in den Strecken gefunden.**

**Hundepost-Entwürfe Nr. 1 bis 6** (Kampagnen-IDs 184 bis 189), alle an Liste 16, Nr. 1 für **Di 13.10.2026, 18:30**, danach alle 2 Wochen.

### Zwei Probleme in Hundepost Nr. 1 (Kampagne 184)

1. **Im Mailtext steht ein gelber Redaktionshinweis:** „[PRÜFEN: Corgi-Video (w9YTWQScoGE) laut Studio-Stand 20.09. geplant auf 06.10.; Vorschaubild erst ab Veröffentlichung abrufbar. Beschreibungssatz vor Versand gegen das Video prüfen. Shiba Inu (U8kulB1gf7I) erscheint am 13.10. selbst.]" Er steht in der Karte „Aus dem Kanal". **Würde er so versendet, sähen ihn die Leser.**
2. **Die Kampagne ist weder getestet noch terminiert:** `testSent = false`, `scheduledAt` leer. Sie läuft also nicht von allein am 13.10.

---

## 2. Aufträge, nach Priorität

### A. Hundepost Nr. 1 versandfertig machen (dringend, Termin 13.10.)

A1. **Gelben Hinweisblock entfernen** (Kampagne 184, Karte „Aus dem Kanal").
A2. **Corgi-Video prüfen:** Ist `https://www.youtube.com/watch?v=w9YTWQScoGE` öffentlich und das Vorschaubild abrufbar? Passt der Beschreibungssatz („Unser Rasseportrait: was einen Corgi ausmacht und für wen er passt.") zum Video? **Wenn das Video am 13.10. nicht öffentlich ist:** nicht entscheiden, sondern Tobi melden. Alternative wäre das Shiba-Inu-Video, das am 13.10. selbst erscheint.
A3. **Testversand** an Tobis Testadressen (nur diese, nicht an Liste 16) und Darstellung prüfen: Desktop, Handy, Bilder, Buttons, Abmeldelink, Absender „Print my Pet", Antwortadresse `kontakt@printmypet.de`.
A4. **Empfänger prüfen:** Liste 16 enthält heute 1 Kontakt, vermutlich ein Test. Melde Tobi, wer genau drin ist.
A5. **Nicht terminieren.** Tobi gibt frei. Danach terminiert er (oder du auf seinen Wunsch) für Di 13.10., 18:30. Zeitzone im Konto: Europe/Berlin.

Hundepost Nr. 2 bis 6: nur lesen und melden, ob weitere Platzhalter, gelbe Hinweise oder Videos mit unklarem Veröffentlichungstermin drinstehen. Nichts ändern.

### B. Brevo aufräumen

B1. **Sicher löschen:** Template **149** („TEST Enthaelt-Pruefung (loeschen)").

B2. **Erst prüfen, dann löschen:** Templates **148 und 150 bis 155** („Futterrechner Ergebnis", v2 bis v6) sowie **157** und **165** (inaktiv).
- Prüfe in Automationen/Workflows, welche Version die Futterrechner-Strecke nutzt (vermutlich v6, ID 154).
- **Diese Version behalten**, jedes Template löschen, das in keiner Automation und keiner Kampagne vorkommt.
- Melde als Tabelle: ID, Name, „in Nutzung ja/nein", „gelöscht ja/nein".

B3. **Testkontakte kennzeichnen statt löschen:** Lege ein Attribut oder Tag `TEST` an und setze es bei allen 34 Kontakten mit `+` in der Adresse. Sie werden noch zum Testen gebraucht.

B4. **Leere Listen löschen**, nur diese: „Ihre erste Liste" (ID 2), „Creator Advisory" (ID 6), „In Ruhe erklärt – Newsletter" (ID 14), „VyriaGym – Trainingsplan-Anfragen" (ID 12). **Nicht löschen:** „Bücher-Warteliste GadE" (ID 13) und alle Listen mit Kontakten.

### C. Zustellbarkeit prüfen (nur prüfen und melden)

C1. **Domain-Authentifizierung für `printmypet.de`** in Brevo (Senders, Domains & dedicated IPs): Sind **DKIM, SPF und DMARC** grün oder offen? Wenn offen: Melde die fehlenden DNS-Einträge im Wortlaut, **ändere nichts**. Das ist der wichtigste Hebel, damit Mails nicht im Spam landen. Benutzt der Sender `newsletter@printmypet.de` noch die Brevo-Standard-Signatur?
C2. **Double-Opt-in-Mails:** Es gibt zwei Futterrechner-Varianten (Template **147** im alten Design mit Arial und beigem Hintergrund, und **158** „v2" im Markenstil). Welche ist im Anmeldeformular und in der Automation hinterlegt? Wenn 147 im Einsatz ist, melde das, denn sie passt nicht zum Markenauftritt.
C3. **Antwortadresse und Impressumsfuß:** Stimmen Absender, Antwortadresse `kontakt@printmypet.de` und die Firmenadresse im Fuß (Creator Advisory S.L., Valencia)?

### D. Anmeldung und Datenfluss (der Weg zu echten Abonnenten)

D1. **Formulare:** Gibt es aktive Anmeldeformulare für Hundepost, Futterrechner und Welpen-Startpaket? Liste URL, Ziel-Liste und „Double-Opt-in an/aus".
D2. **Ende-zu-Ende-Test, einmal pro Strecke** mit einer **neuen Testadresse** `inruheerklaert+e2e-1@gmail.com`, `+e2e-2` usw. (Tobis Postfach). Reihenfolge je Strecke: Formular ausfüllen → Bestätigungsmail → Klick → Willkommensmail/Startpaket → Kontakt in der richtigen Liste mit den richtigen Attributen (z. B. `HUND_NAME`). **Gib Tobi die Adressen und Uhrzeiten durch**, damit Claude im Postfach nachsieht, ob die Mails ankommen und nicht im Spam landen. Seiten: `/pages/futterrechner` und `/pages/welpen-startpaket` auf printmypet.de.
D3. **Shopify-Einwilligung an der Kasse:** Einstellungen → Kasse → Marketing-Optionen. „E-Mail-Marketing" anbieten, Häkchen **nicht vorausgewählt** (DSGVO). Melde den Stand.
D4. **Shopify mit Brevo verbinden:** Gibt es eine Verbindung, die Kunden mit Einwilligung automatisch in die Hundepost-Liste überträgt? Wenn nein: Melde, welche kostenlose Möglichkeit es gibt. **Keine kostenpflichtige App installieren.**
D5. **Footer und Blog (Magazin) der Website:** Gibt es dort ein Anmeldefeld für die Hundepost? Nur melden. Der Einbau ist eine Code-Änderung (Claude Code).

---

## 3. Bericht an Tobi (am Ende, kurz)

1. Hundepost Nr. 1: Was wurde erledigt (A1 bis A4), was fehlt, ist das Corgi-Video am 13.10. öffentlich?
2. Tabelle der gelöschten und behaltenen Templates und Listen (ID, Name, Grund).
3. Zustellbarkeit: Stand von DKIM, SPF, DMARC, die fehlenden Einträge im Wortlaut.
4. Ende-zu-Ende-Test: je Strecke bestanden/nicht bestanden, mit den Testadressen und Uhrzeiten.
5. Shopify-Einwilligung und Brevo-Verbindung: Stand.
6. Alles, was nicht wie beschrieben aussah, mit Screenshot.
7. Offene Entscheidungen für Tobi, jeweils mit deiner Empfehlung.

**Nicht tun:** Kampagnen senden oder terminieren, Tarife ändern, DNS ändern, Kontakte außerhalb von B3 und B4 löschen, Mailstrecken umbauen, Social-/Postiz-Posts anfassen, Gesund aus der Erde oder andere Marken bearbeiten.
