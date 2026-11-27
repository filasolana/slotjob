# SlotJob: Projektkontext (Stand 27.11.2026)

## Projekt

- Uni-Projekt HackWG 26, Gruppe 24: Filip Neufeld, Filip Zelinskyi, Erik Dames, Jesper Feyerabend, Carl Tiegges.
- Prototyp live auf https://slotjob.app (Cloudflare Pages).
- Idee: Studis arbeiten einzelne Schichten (Slots) statt fester Stellen. Slogan: "Schichten statt Stellen."
- Zwei Seiten: Studis buchen oder bewerben sich auf Slots. Betriebe stellen Slots ein und wählen Leute aus.
- Ort im Demo: Potsdam. Lohn 14 bis 15,50 €/h, Schichten 4 bis 5 Stunden.

## Prototyp heute

### Studi-Ansicht (Rudolf M., Maschinenbau)

- **Slots**
  - Oben "Dein Score": 5 Balken, einer je Rolle, Skala -100 bis 1000. Tippen auf eine Rolle zeigt, woraus der Score besteht, und die Punkteregeln in zwei Spalten (Plus, Minus).
  - Darunter "Freie Schichten" mit der Zeile "An deinen freien Tagen: Mo, Mi, Fr, Sa · Ändern".
  - Filter: "Verfügbar" und "Nicht verfügbar".
  - Slotkarte: Datum, Uhrzeit, Betrieb, Tätigkeit, Weg, Dauer, Bewerbungen, Rolle als Tag, Lohn. Der Knopf hängt vom Zustand ab:
    - "Sofort buchen", wenn der Slot verfügbar ist und der Score in seiner Rolle die Schwelle des Slots erreicht. Die Schicht steht dann direkt unter Zugesagt.
    - "Bewerben", wenn verfügbar, aber der Score zu niedrig ist. Darunter: "Sofort ab 500 · dein Score 140".
    - "Nicht verfügbar" mit rotem Grund und Link zum Beheben: "Kurs Kellner nötig" (Kurs ansehen oder Zeugnis hochladen), "Di ist kein freier Tag" (freie Tage ändern), "Nur für Informatik".
- **Kurse**: Service-Grundkurs (Kellner) und Barista-Basics (Barista), beide Gastronomie. Anmelden, dann "Kurs abschließen": Qualifikation plus 100 Score.
- **Schichten**: Zugesagt (sofort gebucht oder angenommen), Beworben, Abgeschlossen mit Sternebewertung des Betriebs.
- **Profil**: freie Tage (Mo bis So), BAföG-Freigrenze (Verdienst im Monat gegen 603 €), Qualifikationen, Zeugnis hochladen (zählt wie der Kurs).

### Betriebs-Ansicht (Trattoria Lupo)

- **Meine Slots**: je Slot Lohn, Rolle, "Sofort ab N" und Bewerber mit Score in dieser Rolle, Knopf "Auswählen". Bucht ein Studi sofort, steht dort "Sofort gebucht: Rudolf M. · Score 720".
- Nach der Schicht bewertet der Betrieb mit Sternen. Bei Rudolf ändert das sofort seinen Score (Schicht +20 plus Sternepunkte).
- **Neuer Slot**: Tätigkeit, Tag, Lohn, Beginn, Ende, Rolle, "Sofort ab Score" (Standard 500), "Auch ohne Erfahrung", "Kurs nötig".
- **Profil**: Bewertung von Studis, Hinweis auf Provision.

### Allgemein

- Oben rechts Umschalter "Studi | Betrieb".
- Farben: Gelb, Weiß, Fast-Schwarz. Nur heller Modus, gelbe Kopfzeile.
- Start: "Schichten statt Stellen." mit Auswahl "Ich studiere" oder "Ich biete Schichten an".

### Demo-Daten

- Rudolfs Score: Gastronomie 520, Handel und Lager 140, Events und Aufbau 0, Campus 0, Büro und IT 0.
  - Gastronomie: 14 Schichten +280, 11 × 5 Sterne +165, 3 × 4 Sterne +15, spontan eingesprungen +30, 3 × Wiederholungsjob +30.
  - Handel und Lager: 5 Schichten +100, 2 × 5 Sterne +30, Wiederholungsjob +10.
- 7 Slots, davon 4 verfügbar und 3 nicht verfügbar.
  - Service und Kasse (Gastronomie, Sofort ab 500): Sofort buchen.
  - Kellner:in Abendschicht (Gastronomie, Kurs Kellner): nicht verfügbar bis zum Kurs.
  - Lager und Leergut (Handel und Lager): Bewerben.
  - CAD-Modelle (Campus, nur Maschinenbau, Sofort ab 300): Bewerben.
  - Helpdesk (Büro und IT, Dienstag): nicht verfügbar.
  - Aufbau Konzert (Events und Aufbau): Bewerben.
  - Inventur (Handel und Lager, Donnerstag): nicht verfügbar.

### Demo-Ablauf (1 Minute)

1. Rudolf sieht seinen Score und bucht "Service und Kasse" sofort.
2. Die Kellner-Schicht ist nicht verfügbar. Er macht den Kellnerkurs (+100, Gastronomie 620) und bucht sie danach sofort.
3. Auf "Betrieb" umschalten: Die Trattoria sieht "Sofort gebucht: Rudolf M." und bewertet ihn mit 5 Sternen. Sein Score steigt um 35.

## Technik

- Eine Datei `site/index.html` (HTML, CSS, JavaScript), kein Server. Der Zustand liegt im Browser (localStorage, Schlüssel `slotjob-v2`). "Zurücksetzen" im Profil stellt die Demo-Daten wieder her.
- Repo mit `site/index.html`, `docs/plan.txt`, `docs/kontext.md`, `ideen/`. Deploy mit `python3 deploy.py` auf Cloudflare Pages.
- Score wird aus einer Ereignisliste berechnet (Rolle, Text, Punkte). Qualifikationen zählen einmal je Qualifikation, höchstens +200 je Rolle, Ergebnis begrenzt auf -100 bis 1000.

## Plan des Teams (docs/plan.txt)

1. **Instant-Zusage**: Wer die Bedingungen des Slots erfüllt und den Score erreicht, bekommt die Schicht sofort, ohne Bewerbung. Die Score-Schwelle je Rolle legt der Betreiber des Slots fest (Richtwert 500). Wer darunter liegt, auch Neueinsteiger, bewirbt sich mit anderen.
2. **Reputation**: Balken je Rolle ganz oben in Slots, -100 bis 1000, Start bei 0. Punktevorschlag:
   - Schicht erledigt +20
   - Bewertung 5 Sterne +15, 4 Sterne +5, 3 Sterne 0, 2 Sterne -10, 1 Stern -25
   - Spontan oder On-Duty eingesprungen +30
   - Wiederholungsjob beim selben Betrieb +10
   - Unentschuldigt gefehlt -100, Absage unter 24 h -30 (beides in allen Rollen)
   - Zu spät über 15 Minuten -10
   - Kurs oder Zeugnis +100; gleiche Qualifikation zählt nur einmal, höchstens +200 je Rolle
   - Beispiel: Kurs plus 12 Schichten mit 5 Sternen = 520
   - Minuspunkte halbieren sich nach 3 Monaten
3. **Rollen (5)**: Gastronomie (fest), Handel und Lager, Events und Aufbau, Campus, Büro und IT (Vorschläge). Kellner und Barista sind Qualifikationen in der Gastronomie.
4. **Filter**: Für mich verfügbar, Nicht verfügbar (mit Grund).
5. **Absagen**: mehr Geld für spontane Schichten (zahlt SlotJob, Absicherung für Betriebe). On-Duty: Leute auf Abruf springen bei Ausfall ein.
6. **Einarbeitung**: Wiederholungsjobs belohnen, z. B. bessere Chancen beim selben Betrieb.
7. **Betreuung**: Schichten-Manager bei SlotJob als Ansprechpartner für beide Seiten.
8. **Geld**: Provision pro Vermittlung.
9. **Vorteile für Betriebe**: Schichten sicherer besetzt (auch spontan), geringere Kosten durch unverbindliche Mitarbeiter, keine Recruiting-Kosten.

## Gebaut und offen

Gebaut:
- Score je Rolle, Balken, Herkunft der Punkte
- Sofort buchen ab der Schwelle des Slots, Schwelle setzt der Betrieb
- Filter verfügbar / nicht verfügbar mit Grund
- Kurs oder Zeugnis +100, einmal je Qualifikation
- Bewertung durch den Betrieb ändert den Score

Offen:
- Absage, Fehlen, Zuspätkommen als Ereignis auslösen (nur als Regel angezeigt)
- Minuspunkte verblassen nach 3 Monaten
- On-Duty, Spontan-Bonus, Schichten-Manager
- Provision (nur als Hinweis im Betriebsprofil)

## Offene Fragen

- Passen die Punktewerte und der Richtwert 500?
- Reicht "Kurs angemeldet" oder erst "abgeschlossen"? Im Prototyp: abgeschlossen.
- Sollen die vier vorgeschlagenen Rollen so bleiben?
- Wie verhindern wir, dass Betriebe den Score unfair drücken (z. B. Einspruch, Mindestanzahl Bewertungen)?
- Rechtlicher Rahmen für unverbindliche Mitarbeiter und Abruf (On-Duty).
- Weitere kritische Punkte und offene Fragen (Score vs. Einsteiger, Absage-Strafe, Geschäftsmodell, BAföG-Beschriftung): `docs/plan.txt` unter OFFEN.

## Weitere Unterlagen (für die Folien)

- `docs/vorgaben.md`: Prüfungsformat, Zeitplan, Abgabe (Folien-Upload Fr 27.11., 23:59)
- `docs/recherche.md`: Desk Research zu Problemen beim Nebenjob (Folie 1)
- `docs/interview-insights.md`: anonymisierte Insights und Zitate aus dem Interview (Folien 1 und 2)
- `docs/go-to-market.md`: Ideen für Phase 3 „Wachsen“ (Folie 5)
