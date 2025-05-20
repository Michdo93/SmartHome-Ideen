# Entwicklung eines Machine-Learning-basierten Vorhersagesystems für Smart-Home-Gerätezustände am Beispiel von openHAB

## Alternative Arbeitstitel

- Entwicklung eines Prognosesystems für Smart-Home-Zustände auf Basis historischer openHAB-Daten
- Zeitreihenbasierte Vorhersage von Smart-Home-Gerätezuständen mit Hilfe von Machine Learning und Persistenzdaten aus openHAB 
- Konzeption und prototypische Umsetzung eines Prognosedashboards zur Visualisierung von Gerätezuständen im Smart Home
- …

Der Titel der Thesis muss erst kurz vor Abgabe endgültig feststehen.

Das, was du vorhast – eine State-Prediction für openHAB-Items basierend auf historischen Zuständen – ist ein klassisches Machine-Learning-Problem im Smart-Home-Kontext. Du willst mit historischen Zustandsdaten, Zeitinformationen und ggf. Kontextdaten (z.B. Wetter, Anwesenheit) Wahrscheinlichkeiten vorhersagen, dass ein bestimmter State zu einem Zeitpunkt eintritt.

---

## 🛠️ **Empfohlene Technologien / Architektur**

### 1. **Backend-Sprache & Framework**

* **Python** (Empfohlen)

  * Riesige Auswahl an ML-Bibliotheken (Scikit-Learn, TensorFlow, PyTorch)
  * Gute Tools zur Datenverarbeitung (Pandas, NumPy)
  * REST-API-Anbindung (z.B. mit `requests`) --> inzwischen besteht ein eigener Client (`pip install python-openhab-rest-client`)
  * Optional Webserver mit **FastAPI** oder **Flask** zur Modellbereitstellung
* **Alternativ**: Node.js, falls du mehr im JavaScript-Umfeld arbeitest, aber für ML weniger geeignet.

Man kann natürlich auch eine andere Programmiersprache als Python verwenden. Python bietet sich aber bei KI/AI an, da es hier viele Frameworks gibt.

### 2. **Datenzugriff**

* **openHAB REST API** für Live-Daten, Item-Infos etc.
* **Persistence-Datenbank direkt** (z. B. InfluxDB, MySQL, PostgreSQL) für historische Daten:

  * Am besten über direkten Zugriff (z. B. via SQLAlchemy oder `influxdb-client-python`)
  * REST allein reicht **nicht** für viele Datenanalysezwecke, da die REST API nicht alle historischen Daten performant bereitstellt.
 
Man kann über die openHAB REST API nicht nur Items abrufen und ihrer Zustände (States), sondern diese können auch eine State-Description und Command-Description besitzen. Darüber weiß man dann, welche Zustände dieses Item annehmen könnte. Natürlich mit Einschränkungen, da z.B. beim Item Type `Number` alle Zahlen vorkommen könnten. Was man über die REST API aber zum Beispiel erfährt ist, ob eine Zahl zu einer Ganzzahl formatiert wurde, zu einer Fließkommazahl mit entsprechend vielen Nachkommastellen, usw.

In openHAB kann man für die Persistence mehrere Datenbanksysteme gleichzeitig nutzen und verschiedene Strategien konfigurieren. Zum Beispiel kann ich eine Strategie entwerfen, dass wenn ich das openHAB-System neustarte, der zuvor zuletzt gespeicherte Zustand verwendet wird. Ein Beispiel hierzu wäre, dass ich z.B. ein Fenster offen habe, openHAB stoppe und dann neustarte. Würde ich keine Persistence verwenden, würde das openHAB-System annehmen, dass das Fenster geschlossen ist. Wenn jetzt z.B. eine Innenjalousie herunterfahren würde, wüsste sie nicht, dass das Fenster offen steht und würde dagegen fahren. Außerdem bekommt openHAB in der Zeit, in dem es ausgeschalten ist, nicht mit, welche Zustände von welchen Geräten sich wie verändert hätten. Erst, wenn ein Gerät dies entsprechend kommuniziert. Manche Geräte kommunizieren alle paar Sekunden, andere nur, wenn eine Aktion bzw. Funktion ausgeführt wird. Bei den Strategien kann ich auch konfigurieren ob einzelne Items etwas in eine Datenbank speichern sollen, mehrere oder selbstverständlich alle. Ich kann auch in den Strategien eine Gruppe von Items etwas speichern lassen. Ein Item oder mehrere Items können nach verschiedenen Strategien abgehandelt werden und auch in verschiedenen Datenbanken gespeichert werden. Dann kann ich auch einstellen, ob Zustände von Items in gewissen Zeitintervallen (minütlich, stündlich, täglich) gespeichert werden sollen, nach jedem Command oder nach jedem State-Change, usw.

Für eine State-Prediction empfiehlt sich meiner Meinung nach eine Strategie für InfluxDB zu entwerfen und InfluxDB als Datenbank zu verwenden.

### 3. **Datenbank**

* **InfluxDB** (wenn du Zeitserien brauchst)
* **PostgreSQL / MySQL** (wenn du relationale, anreicherbare Daten brauchst)

Für eine State-Prediction empfiehlt sich meiner Meinung nach eine Strategie für InfluxDB zu entwerfen und InfluxDB als Datenbank zu verwenden.

### 4. **Machine Learning**

* **Scikit-learn** (gute Wahl für tabellarische Vorhersagen)
* **TensorFlow / Keras** (wenn du tiefer gehen willst, z. B. LSTM für Zeitreihen)
* **Prophet von Facebook** (einfaches Zeitreihenmodell, gute Baseline)

### 5. **Visualisierung & Dashboard**

* **Grafana** (für historische Werte und Vorhersagevergleich)
* **Streamlit / Dash** (zum schnellen UI-Prototyping)
* Optional: Integration mit **openHAB HABPanel** oder **MainUI**

Braucht man überhaupt eine Visualisierung? Welche Art an Visualisierung? Reicht eine Textausgabe? Braucht man Graphen/Schaubilder?

---

## 🧠 Beispiel-Features für ML-Modell

| Feature             | Beispiel                              |
| ------------------- | ------------------------------------- |
| Uhrzeit             | 09:00                                 |
| Wochentag           | Montag                                |
| Letzter Zustand     | OFF                                   |
| Historische Pattern | z. B. 10 Tage Kaffeebereitung um 9:00 |

Ziel: Wahrscheinlichkeit, dass `Kaffeevollautomat = ON` zu einem bestimmten Zeitpunkt.

Realistisch heißt ein `Switch Item` nicht `Kaffevollautomat`, sondern ein `Group Item`, in diesem gibt es dann für verschiedene Zubereitungsfunktionen ein `Switch Item`. Vermutlich gibt es auch ein `Switch Item`, um den Kaffevollautomat einzuschalten. Es gibt auch andere Item Types, wie vielleicht, wie voll der Wassertank ist oder auch verschiedene Items, um den Kaffevollautomaten zu reinigen, usw.

Ein anderes Beispiel wäre z.B. eine Lampe:

| Feature             | Beispiel                              |
| ------------------- | ------------------------------------- |
| Uhrzeit             | 21:00                                 |
| Wochentag           | Dienstag                                |
| Letzter Zustand     | ON                                   |
| Historische Pattern | z. B. Täglich um 21:00 Uhr ist das Licht im Wohnzimmer an |

---

## 🔗 Architektur-Schaubild

```plaintext
+------------------+
|  openHAB System  |
| (REST API --> Items)|
+--------+---------+
         |
         v
+------------------+              +------------------+
|   openHAB        |   REST API   |       DB         |
|   REST API       |<------------>|  (InfluxDB/MySQL)|
+------------------+              +------------------+
         |
         v
+---------------------------+
|   Data Collector (Python)|
| - Ruft Daten ab          |
| - Speichert sie lokal    |
+------------+-------------+
             |
             v
+---------------------------+
|   ML-Modell (Scikit-Learn)|
| - Trainiert auf Historie  |
| - Predict: 9:00 Uhr       |
+------------+-------------+
             |
             v
+------------------------------+
|  Prediction API (FastAPI)   |
| - Gibt Wahrscheinlichkeit    |
+-------------+----------------+
              |
              v
+------------------------------+
| Web-Dashboard / Grafana      |
| oder openHAB Integration     |
+------------------------------+
```

Ich kann Persistence-Informationen sowohl über die REST API, als auch über die Datenbank direkt abfragen. Die REST API ist für Persistence aber vermutlich eingeschränkt.

---

## ✅ Beispiel-Anwendungsfall

**"Was ist die Wahrscheinlichkeit, dass Kaffeemaschine morgen um 09:00 Uhr angeht?"**

1. Modell prüft:

   * Uhrzeit = 09:00
   * Wochentag = Dienstag
   * Historische Daten (z. B. 8 von 10 Tagen war das der Fall)
2. Modell sagt: **94% Wahrscheinlichkeit**

---

## 🧩 Weiterer Ausbau

* Online-Lernen: Modell lernt nach und nach weiter
* Modell für jedes wichtige Item individuell trainieren
* Kontextdaten: Wetter, Kalender, Urlaubsmodus
* Konfiguration der zu überwachenden Items per YAML/JSON (in einem aufwendigeren Projekt vielleicht eine eigene Weboberfläche und per Datenbank hinzufügen/löschen).

---

## 🎯 Ziel des Dashboards

> Der Nutzer soll auf einfache Weise Wahrscheinlichkeiten für zukünftige States von Items abfragen können – zu einer bestimmten Uhrzeit und einem bestimmten Datum.

---

## 💡 Grundfunktionen im Dashboard

### 🧩 **Variante A: Manuelle Abfrage**

* **Use Case**: „Was passiert am Mittwoch um 07:00?“
* **Interaktion**:

  1. User wählt ein `Item` (z. B. `Kaffeemaschine`)
  2. Wählt Datum & Uhrzeit (z. B. `2025-05-15 09:00`)
  3. Klickt auf „Wahrscheinlichkeit berechnen“
  4. **System zeigt**: „Wahrscheinlichkeit = 94%“

✅ Vorteile:

* Ressourcenschonend (nur Berechnung auf Nachfrage)
* Intuitiv verständlich
* Individuell, da man Datum und Uhrzeit mehrfach ändern kann

---

### 🧩 **Variante B: Kalender-Ansicht mit automatischer Prediction**

* **Use Case**: Überblick für den Tag
* **Interaktion**:

  1. User wählt Datum (z. B. heute, morgen)
  2. System zeigt für alle relevanten Items **über den Tag verteilt** Vorhersagen:

     ```
     06:00  Schlafzimmer Licht ON → 83%
     07:00  Kaffeemaschine ON → 95%
     08:00  Badezimmer Heizung ON → 40%
     ...
     ```

Dies kann man auch im Viertelstundentakt oder andere Intervalle einteilen. Idealerweise ist dies vielleicht sogar ebenfalls konfigurierbar.

✅ Vorteile:

* Übersicht über das ganze Smart Home
* Automatische Predictions im Batch (z. B. täglich über Nacht)

❗ Nachteil:

* Höherer Rechenaufwand (muss für viele Items/Uhrzeiten vorhersagen)
* Du brauchst ein performantes Modell oder Caching

Ggf. kann man auch eine Konfiguration machen, die manche Items in anderen Intervallen wahrscheinlichen Zustände berechnet.

---

### 🧩 **Variante C: Item-Detailansicht mit Heatmap**

* **Use Case**: Tiefenanalyse eines Items
* **Interaktion**:

  1. User wählt ein Item
  2. System zeigt eine **Heatmap oder Zeitleiste**:

     ```
     +-------+------+------+------+------+
     | Uhr   | 06:00| 07:00| 08:00| 09:00|
     +-------+------+------+------+------+
     | Prob. | 10%  | 65%  | 85%  | 95%  |
     +-------+------+------+------+------+
     ```

✅ Vorteile:

* Tolle Visualisierung
* Erkennt Muster visuell

---

## 🧠 Umsetzungsideen

| Feature                 | Umsetzung                                           |
| ----------------------- | --------------------------------------------------- |
| Item-Auswahl            | Dropdown oder Suchfeld                              |
| Datum/Uhrzeit           | Date/Time Picker                                    |
| Ergebnis-Anzeige        | Balken, Prozentwert, Icon (🔆🕘✅)                   |
| Automatische Vorhersage | Nach Zeitplan im Hintergrund (z. B. 1x täglich)     |
| Prediction Trigger      | Button oder Live-Update                             |
| Caching                 | Redis oder Datenbank für gespeicherte Predictions   |
| Visualisierung          | Plotly, Matplotlib (Python) oder direkt via Grafana |

---

## ⚙️ Performance-Strategie

### Wenn es viele Items gibt:

1. **Eingrenzen auf „relevante Items“**

   * Nur Items mit hohem Aktivitätslevel
   * Konfigurierbare „interessante Items“
2. **Batch-Predictions einmal täglich**

   * Z. B. alle States für morgen um 9:00 vorab berechnen
3. **Prediction-Caching**

   * Ergebnisse speichern, keine Live-Berechnung jedes Mal



---

## 🖼️ Prototyp-Skizze (Textbasiert)

```
[Dropdown: Kaffeemaschine]

[Date Picker: 2025-05-15]   [Time Picker: 09:00]

[Button: Vorhersage anzeigen]

-------------------------------------
📈 Vorhersage für Kaffeemaschine:
🕘 09:00 Uhr → 🔥 Wahrscheinlichkeit: 94%
```

Oder als Heatmap:

```
Kaffeemaschine am 2025-05-15:
06:00  ░░░░ 12%
07:00  ███░░ 67%
08:00  ██████ 88%
09:00  ███████ 94%
```

---

## ➕ Erweiterungen

* Notifications: „Wahrscheinlichkeit > 90% → Push-Nachricht“
* Automationen aus Prediction (z. B. Triggern von Regeln)
* Feedback-Button („Ist das korrekt eingetreten?“ → Reinforcement Learning)


