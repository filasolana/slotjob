# SlotJob: Projektkontext (Stand 27.11.2026)

## Projekt

- Uni-Projekt HackWG 26, Gruppe 24: Filip Neufeld, Filip Zelinskyi, Erik Dames, Jesper Feyerabend, Carl Tiegges.
- Prototyp live auf https://slotjob.app (Cloudflare Pages).
- Idee: Studis arbeiten einzelne Schichten (Slots) statt fester Stellen. Slogan: "Schichten statt Stellen."
- Zwei Seiten: Studis buchen Slots sofort, wenn ihr Verhalten und ihre Qualifikation reichen. Betriebe stellen Slots ein und setzen die zwei Schwellen. Es gibt keine Bewerbung.
- Ort im Demo: Potsdam. Lohn 14 bis 15,50 €/h, Schichten 4 bis 5 Stunden.

## Prototyp heute

### Studi-Ansicht (Rudolf M.)

- **Slots**
  - Oben "Dein Score" mit zwei Teilen. Verhalten: ein Balken für alle Rollen, 0 bis 100. Qualifikation: 5 Balken, einer je Rolle, 0 bis 1000. Tippen auf einen Balken zeigt, woraus der Wert besteht, und die Punkteregeln in zwei Spalten.
  - Darunter "Freie Schichten": alle offenen Schichten, jederzeit buchbar, sobald beide Schwellen erreicht sind. Es gibt keine festen Tage und keinen Wochenplan.
  - Filter: "Verfügbar" (beide Schwellen erreicht) und "Nicht verfügbar" (mit Grund, z. B. "Gastronomie 300 nötig, du hast 250").
  - Slotkarte: Datum, Uhrzeit, Betrieb, Tätigkeit, Weg, Dauer, Rolle als Tag, Lohn, darunter "Sofort ab: Verhalten 90 ✓ · Gastronomie 300 ✓". Der Knopf:
    - "Sofort buchen", wenn beide Schwellen erreicht sind. Die Schicht steht dann direkt unter Zugesagt.
    - "Nicht verfügbar", wenn eine Schwelle fehlt ("Gastronomie 300 nötig, du hast 250", mit Link "Arbeitszeugnis hochladen").
- **Schichten**: Zugesagt (sofort gebucht oder angenommen), Beworben, Abgeschlossen mit Sternebewertung des Betriebs.
- **Profil**: Minijob-Grenze (Verdienst im Monat gegen 603 €), anerkannte Arbeitszeugnisse, Arbeitszeugnis hochladen: man wählt eine der 5 Rollen, +100 in dieser Rolle, ein Zeugnis je Rolle. Es gibt keine Kurse und keinen Kurse-Tab.

### Betriebs-Ansicht (Pizzeria Mamma Mia)

- **Meine Slots**: je Slot Lohn, Rolle, "Sofort ab: Verhalten 80 · Qualifikation 500" und "Noch frei" oder "Gebucht". Bucht ein Studi, steht dort "Sofort gebucht: Rudolf M. · Verhalten 92 · Qualifikation 450".
- Nach der Schicht fragt die App den Betrieb zwei Dinge: "Pünktlich und zuverlässig?" Ja (+2 Verhalten) oder Nein (−5), und "Wie gut war die Arbeit?" mit Sternen (Schicht +10 Qualifikation, 5 Sterne +10, 1 bis 2 Sterne −10). Bei Rudolf ändert das sofort seine Werte.
- **Neuer Slot**: Tätigkeit, Tag, Lohn, Beginn, Ende, Rolle, "Sofort ab Verhalten" (Standard 70), "Sofort ab Qualifikation" (Standard 200; 0 heißt auch ohne Erfahrung).
- **Profil**: Bewertung von Studis, Stundensatz (SlotJob ist Arbeitgeber, kein Papierkram).

### Allgemein

- Oben rechts Umschalter "Studi | Betrieb".
- Farben: Gelb, Weiß, Fast-Schwarz. Nur heller Modus, gelbe Kopfzeile.
- Start: "Schichten statt Stellen." mit Auswahl "Ich studiere" oder "Ich biete Schichten an". "Ich studiere" führt direkt zu den Schichten, ohne weitere Fragen.

### Demo-Daten

- Rudolf, Verhalten 92: Start 80, 16 × pünktlich +32, spontan eingesprungen +5, 3 × zu spät −15, Absage unter 24 h −10.
- Rudolf, Qualifikation: Gastronomie 250 (14 Schichten +140, 11 × 5 Sterne +110), Handel und Lager 70, Events und Aufbau 0, Campus 0, Büro und IT 0.
- 7 Slots: 4 verfügbar, 3 nicht verfügbar (Kellner und Helpdesk wegen Qualifikation, Konzert wegen Verhalten).
  - Service und Kasse (Gastronomie, Verhalten 70, Qualifikation 0): Sofort buchen.
  - Kellner:in Abendschicht (Gastronomie, Verhalten 90, Qualifikation 300): nicht verfügbar (250), mit Arbeitszeugnis (350) Sofort buchen.
  - Lager und Leergut (Handel und Lager, 70, 0): Sofort buchen.
  - CAD-Modelle (Campus, 80, 0): Sofort buchen.
  - Aufbau Konzert (Events und Aufbau, 95, 0): nicht verfügbar, weil Verhalten 92.
  - Helpdesk (Büro und IT, 70, 100): nicht verfügbar. Inventur (Handel und Lager, 70, 0): Sofort buchen.

### Demo-Ablauf (1 Minute)

1. Rudolf sieht Verhalten 92 und seine Qualifikationen und bucht "Service und Kasse" sofort.
2. Bei der Kellner-Schicht fehlt Qualifikation (250 von 300). Er lädt sein Arbeitszeugnis vom Café hoch (+100, Gastronomie 350) und bucht sie danach sofort.
3. Auf "Betrieb" umschalten: Die Pizzeria Mamma Mia sieht "Sofort gebucht: Rudolf M.", klickt "Pünktlich: Ja" (+2 Verhalten) und gibt 5 Sterne (+20 Qualifikation).

## Technik

- Eine Datei `site/index.html` (HTML, CSS, JavaScript), kein Server. Der Zustand liegt im Browser (localStorage, Schlüssel `slotjob-v11`). "Zurücksetzen" im Profil stellt die Demo-Daten wieder her.
- Repo mit `site/index.html`, `docs/plan.txt`, `docs/kontext.md`, `ideen/`. Deploy mit `python3 deploy.py` auf Cloudflare Pages.
- Beide Scores werden aus einer Ereignisliste berechnet (Rolle oder Verhalten, Text, Punkte). Verhalten 0 bis 100. Qualifikation 0 bis 1000: ein Arbeitszeugnis je Rolle, Schichten höchstens +400.

## Plan des Teams (docs/plan.txt)

1. **Instant-Zusage** (entschieden 27.11.): Der Betrieb setzt je Slot zwei Schwellen, Verhalten (Standard 70) und Qualifikation in der Rolle. Beide erreicht: sofort buchen. Eine fehlt: nicht verfügbar. Keine Bewerbung. Neue starten mit Verhalten 80 und Qualifikation 0.
2. **Reputation, zwei Scores** (entschieden 27.11.):
   - Verhalten, einer pro Person, 0 bis 100, Start 80: pünktlich +2, spontan oder On-Duty +5, zu spät −5, Absage unter 24 h −10, gefehlt −30, Krankmeldung 0, Minuspunkte halbieren sich nach 3 Monaten.
   - Qualifikation je Rolle, 0 bis 1000, Start 0: Arbeitszeugnis +100 für die Rolle, die es abdeckt (ein Zeugnis je Rolle), keine Kurse, Schicht in der Rolle +10 (höchstens +400), 5 Sterne +10, 1 bis 2 Sterne −10.
   - Nach der Schicht: "Pünktlich und zuverlässig?" zählt für Verhalten, Sterne für die Arbeit zählen für Qualifikation.
3. **Rollen (5)**: Gastronomie (fest), Handel und Lager, Events und Aufbau, Campus, Büro und IT (Vorschläge).
4. **Filter**: Verfügbar (Sofort buchen), Nicht verfügbar (mit Grund). Kein Studiengang, weder im Score noch als Filter (entschieden 27.11.).
5. **Absagen**: mehr Geld für spontane Schichten (zahlt SlotJob, Absicherung für Betriebe). On-Duty: Leute auf Abruf springen bei Ausfall ein, nur freiwillig (Abruf ist gesetzlich nur mit 4 Tagen Vorlauf verpflichtend, § 12 TzBfG). Entschieden 27.11.: Krankmeldung kostet keine Punkte, wie im normalen Job; SlotJob schreibt die Schicht als Spontan-Schicht mit höherem Lohn neu aus und trägt die Mehrkosten aus der Marge.
6. **Einarbeitung**: Wiederholungsjobs belohnen, z. B. bessere Chancen beim selben Betrieb.
7. **Betreuung**: Schichten-Manager bei SlotJob als Ansprechpartner für beide Seiten.
8. **Geld** (entschieden 27.11.): Der Vertrag läuft über SlotJob. SlotJob ist Arbeitgeber der Studis (Zeitarbeit, Erlaubnis der Bundesagentur für Arbeit nötig) und verleiht sie pro Slot an Betriebe. Betriebe zahlen einen Stundensatz (Vorschlag ca. 25 €/h), SlotJob zahlt den Lohn; Einnahme ist die Marge pro Stunde, dazu eine Vermittlungsgebühr bei Festanstellung. Details: `docs/vertragsmodell.md`.
9. **Vorteile für Betriebe**. Pitch-Satz: „Betriebe zahlen nur die Schichten, die sie brauchen.“
   - Geringere Kosten (Hauptvorteil): keine festen Mitarbeiter, denen man jede Woche die gleichen Stunden geben muss
   - Schichten sicherer besetzt: Spontan-Schichten und On-Duty fangen Krankheit auf
   - Geprüfte Qualität: Verhalten zeigt Zuverlässigkeit, Qualifikation zeigt, ob jemand den Job kann
   - Kein Papierkram, keine Recruiting-Kosten: SlotJob ist Arbeitgeber

## Gebaut und offen

Gebaut:
- Zwei Scores: Verhalten (einer) und Qualifikation (je Rolle), Balken, Herkunft der Punkte
- Zwei Schwellen je Slot, Sofort buchen nur wenn beide erreicht, sonst nicht verfügbar
- Filter verfügbar / nicht verfügbar mit Grund
- Arbeitszeugnis +100 Qualifikation in der abgedeckten Rolle, ein Zeugnis je Rolle (Auswahl: die 5 Rollen)
- Betrieb bewertet Pünktlichkeit (Verhalten) und Arbeit (Qualifikation)
- Minijob-Grenze im Profil, Stundensatz im Betriebsprofil

Offen:
- Absage, Fehlen, Zuspätkommen als Ereignis auslösen (nur als Regel angezeigt)
- Minuspunkte verblassen nach 3 Monaten
- On-Duty, Spontan-Bonus, Schichten-Manager
- BAföG-Grenze optional später als extra Balken

## Offene Fragen

- Passen die Punktewerte, Verhalten Start 80 und Standard-Schwelle 70?
- Welche Arbeitszeugnisse decken welche Rolle ab, und wer prüft sie?
- Sollen die vier vorgeschlagenen Rollen so bleiben?
- Wie verhindern wir, dass Betriebe den Score unfair drücken (z. B. Einspruch, Mindestanzahl Bewertungen)?
- Rechtlicher Rahmen für unverbindliche Mitarbeiter und Abruf (On-Duty).
- Dürfen Betriebe die Verhaltensschwelle über 80 setzen (dann können Neue dort nicht buchen)?
- Keine Minuspunkte auch bei verlegter Prüfung, oder nur bei Krankheit?
- Weitere offene Fragen (Ausfallgeld, Stundensatz, Auszahlung, Konkurrenz, POV): `docs/plan.txt` unter OFFEN.

## Weitere Unterlagen (für die Folien)

- `docs/vorgaben.md`: Prüfungsformat, Zeitplan, Abgabe (Folien-Upload Fr 27.11., 23:59)
- `docs/recherche.md`: Desk Research zu Problemen beim Nebenjob (Folie 1)
- `docs/interview-insights.md`: anonymisierte Insights und Zitate aus dem Interview (Folien 1 und 2)
- `docs/go-to-market.md`: Ideen für Phase 3 „Wachsen“ (Folie 5)
- `docs/vertragsmodell.md`: Vertrag über SlotJob, Lohnnebenkosten, Beispielrechnung (Folie 4)
