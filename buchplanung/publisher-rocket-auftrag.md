# Publisher Rocket: was ihr eingeben sollt

Ziel: Für jedes der 30 Themen belegen, ob es **Nachfrage**, **schlagbare Konkurrenz** und **Einnahmen** gibt.
Aufwand: ca. 2 bis 3 Stunden, gut auf zwei Personen aufteilbar (A: Nr. 1–15, B: Nr. 16–30).

## Grundeinstellungen (wichtig!)

- **Marktplatz: Amazon.de** (Germany) einstellen, nicht .com.
- Keyword-Suche: **Books** *und* **Kindle** (je ein Lauf, beides exportieren).
- Jede Ergebnistabelle mit **Export → CSV** speichern.
- Dateiname: `NR_werkzeug_keyword.csv`, z. B. `07_keyword_krafttraining-frauen.csv`,
  `07_competition_krafttraining-frauen.csv`, `07_category_fitness.csv`.
- Ablage: `buchplanung/rocket-daten/` (Upload über GitHub oder einfach ZIP hier in den Chat).

## Schritt 1: Keyword-Suche (Keyword Search)

Den **Start-Begriff** eingeben, die komplette Vorschlagsliste exportieren. Die Spalte „Zusatz“ nur prüfen, wenn
noch Zeit ist. Alle Begriffe stehen auch maschinenlesbar in [`rocket-keywords.csv`](rocket-keywords.csv).

| # | Start-Begriff | Zusatz |
|---|---|---|
| 1 | hund erinnerungsbuch | hundebuch zum ausfüllen |
| 2 | katze erinnerungsbuch | katzenbuch zum ausfüllen |
| 3 | gartenplaner 2027 | mondkalender garten 2027 |
| 4 | sternzeichen steinbock | geschenk steinbock |
| 5 | fragen für paare | paarbuch zum ausfüllen |
| 6 | hundekekse backen | hunde kochbuch |
| 7 | krafttraining frauen | wechseljahre abnehmen sport |
| 8 | trainingstagebuch | fitness tagebuch |
| 9 | protein kochbuch | abnehmen ohne hungern |
| 10 | bücher schreiben mit ki | kdp self publishing |
| 11 | chatgpt prompts | ki für selbstständige |
| 12 | sternzeichen wassermann | geschenk wassermann |
| 13 | sternzeichen liebe | sternzeichen partner |
| 14 | online dating | dating profil |
| 15 | bindungsangst | bindungsstile |
| 16 | sternzeichen fische | geschenk fische |
| 17 | dating ab 40 | neuanfang nach trennung |
| 18 | hochbeet | hochbeet anfänger |
| 19 | balkon gemüse | selbstversorger balkon |
| 20 | heilkräuter garten | heilpflanzen anbauen |
| 21 | sternzeichen widder | geschenk widder |
| 22 | wildkräuter | essbare wildpflanzen |
| 23 | fermentieren | einkochen |
| 24 | haustier verlust trauer | trauer hund |
| 25 | welpen tagebuch | welpe erstes jahr |
| 26 | narzissmus beziehung | verdeckter narzissmus |
| 27 | rückentraining | rückenschmerzen übungen |
| 28 | fit ab 60 | gleichgewichtstraining senioren |
| 29 | dating für männer | frauen ansprechen |
| 30 | ugc creator | instagram geld verdienen |

## Schritt 2: Konkurrenzanalyse (Competition Analyzer)

Für **jeden Start-Begriff** aus Schritt 1 (nur Books, Kindle reicht nicht) die Konkurrenzanalyse laufen lassen
und exportieren. Das sind 30 Läufe.

Zusätzlich diese **Konkurrenztitel** gezielt suchen (Titel eingeben, im Analyzer öffnen, Daten exportieren):

| Marke | Konkurrenz zum Nachschauen |
|---|---|
| Liebespsyche | „Das perfekte Buch für Fische“ (Serie, Verlag 3695…), „Astro Match“ (Moldovan), „Jein!“ (Stahl), „Verdeckter Narzissmus in Beziehungen“ (Müller) |
| Gesund aus der Erde | „Mein Hochbeet Jahr“ (Simon), „Der Selbstversorger“ (Storl), „Das Mondjahr 2027 Garten“ (Paungger) |
| PrintMyPet | „Hundetagebuch Erinnerungsbuch“, „Das Kochbuch für Deinen Hund“, „Impulskontrolle und Frustrationstoleranz bei Hunden“ |
| Creator Advisory | „ChatGPT – Das Praxisbuch“ (Bock/Knust), „Mit KI Geld verdienen 2026“ |
| TB/Virya | die Top 3 aus „Krafttraining Frauen“ (aus Schritt 2 übernehmen) |

## Schritt 3: Kategorien (Category Search)

Diese Kategorien auf amazon.de suchen und exportieren (Verkäufe pro Tag für Platz 1 und Platz 10):

Astrologie · Sternzeichen-Geschenkbücher · Dating · Frauen in Beziehungen · Partnerschaft & Liebe ·
Geschenkbücher für Hundefreunde · Hundeerziehung · Trauer · Gärtnern Obst & Gemüse · Urban Gardens ·
Kräuter · Fitness & Krafttraining · Diät & Abnehmen · Künstliche Intelligenz · Social Media · Selbst-Publishing

## Was ich danach ausrechne

Für jedes Thema einen Score aus:

1. **Nachfrage:** geschätzte Suchen pro Monat (deutsche Werte sind kleiner als US-Werte, deshalb vergleiche
   ich die Themen nur untereinander).
2. **Konkurrenz:** Rocket-Wettbewerbswert und **wie viele der Top 10 unter 100 Bewertungen haben**
   (der beste Hinweis darauf, dass man ein Thema knacken kann).
3. **Einnahmen:** durchschnittlicher Monatsumsatz der Top 5. Liegt er unter ca. 300 €, wird das Thema gestrichen.
4. **Nutzen für die Marke:** wird von uns gesetzt.

Ergebnis: endgültige Rangliste der 30 Bücher, Themen mit wenig Potenzial werden ersetzt. Pro Buch gibt es dann
Titel, Untertitel, 7 Keyword-Felder und Kategorien zum direkten Einfügen in KDP.

**Tipp:** Wenn Rocket für einzelne Funktionen kein amazon.de anbietet, bitte trotzdem .com exportieren und im
Dateinamen `_US` dazuschreiben. Das hilft für die spätere EN-Übersetzung.
