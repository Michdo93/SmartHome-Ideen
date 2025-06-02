# Bachelorarbeit: Entwicklung eines modularen, datenschutzfreundlichen und lokalen Sprachassistenten

---

## Mögliche Titelvorschläge

- "Entwicklung eines datenschutzfreundlichen Sprachassistenten auf Basis freier Software"
- "Vergleich von ASR-Engines für Offline-Sprachassistenten"
- "Modularer Sprachassistent für den lokalen Einsatz: Architektur und Prototyp"
- "Sprachverarbeitung ohne Cloud: Konzeption und Evaluation eines lokalen Sprachassistenten"
- "Custom Wakeword Detection in Embedded Systems: Architektur, Training und Einsatz"
- "Open Source Sprachassistent: Ein systemischer Vergleich freier Komponenten"
- ...

---

## Hintergrund

Sprachassistenten wie Alexa, Siri oder Google Assistant haben sich im Alltag vieler Menschen etabliert. Allerdings sind diese Systeme oft cloudgebunden, datenschutzrechtlich bedenklich und schwer individualisierbar. Dies eröffnet spannende Möglichkeiten für die Entwicklung eines eigenen, lokal laufenden Sprachassistenten mit freier Wahl der Komponenten.

---

### Problemstellung

- Cloudverbindungen können auch öfter mal gestört oder abgebrochen sein.
  - Ein lokaler Sprachassistent würde zumindest lokal funktionierende Befehle bearbeiten und lokal verfügbare Informationen zur Verfügung stellen können.
- Datenschutzrechtlich hört ein Sprachassistent dauerhaft mit und wartet, bis ein sog. Hotword (auch trigger word oder wake word) erkannt wird.
  - Ist wirklich garantiert, dass nur Daten aufgezeichnet und an eine Cloud gesendet werden, wenn ein Hotword verwendet wurde oder lauscht das System dauerhaft mit?
  - Anmerkung: Deutsche Begriffe sind Aktivierungswort, Aufwachwort, Aufwachbefel, Triggerwort, usw.
- Sprachassistenten sind oft nur beschränkt individualisierbar, Anwendungen werden eingestellt oder eingeschränkt.
  - Google Nest ermöglicht es oft bspw. Smart Home Geräte zwar anzubinden, diese können darüber aber nicht gesteuert werden, sondern meist nur Zustände abgefragt werden.
  - Amazon hat bspw. ihre Policy geändert und seitdem wird das openHAB Skill aus Sicherheitsgründen nicht mehr unterstützt, damit wäre ein Smart Home, welches über openHAB gesteuert wurde nicht mehr bedienbar.

---

## Ziel der Arbeit

Ziel der Arbeit ist die Entwicklung und prototypische Umsetzung eines lokalen Sprachassistenzsystems auf Basis freier Software und Hardware. Der Fokus liegt auf Modularität, Erweiterbarkeit und Datenschutzfreundlichkeit. Es sollen verschiedene Komponenten (Spracherkennung, Hotword-Erkennung, Intent-Parsing, Text-to-Speech) integriert und ggf. evaluiert werden. Es muss eine Anbindung zur openHAB REST API möglich sein.

---

## Teilziele

- Aufbau einer modularen Architektur für Sprachassistenz
- Integration von Hotword-Detection, ASR (Automatic Speech Recognition, hier: Speech-to-Text), NLU (Natural Language Understanding, hier: Intent-Erkennung ist eine Teilmenge des Natural Language Processings), TTS (Text-to-Speech)
- (Optional) Vergleich verschiedener frei verfügbarer ASR/NLU/TTS-Frameworks hinsichtlich:
  - Latenz
  - Genauigkeit
  - Ressourcenverbrauch
- Anpassbarkeit durch neue Triggerwörter und Intents
- Betrachtung datenschutzrechtlicher Implikationen (lokal vs. Cloud)
- Anbindung an die openHAB REST API
  - Triggersatz soll bspw.
    - Command an ein openHAB Item schicken und darüber dann ein Gerät steuern
    - State von einem openHAB Item abfragen und per Text-to-Speech diesen Systemzustand ausgeben
- eigene REST API zur Verfügung stellen
  - ein Endpunkt könnte zum Beispiel genutzt werden, dass man einen Text schickt, der dann per Text-to-Speech ausgegeben wird.
  - ...
- (Optional) Einbindung einfacher Aktionen (z. B. Timer/Wecker setzen, Terminerinnerungen speichern & Termine vorlesen, Wetter abfragen, "das Internet durchsuchen")-
- (Optional) Einbindung von Geräten unabhängig von Smart Home Systemen
  - Zum Beispiel die Python Library `SoCo` (Sonos Controller)
    - Man könnte die `play_uri`-Funktion nutzen, dass die Text-to-Speech-Ausgabe nicht über einen Lautsprecher am entwickelten Gerät erfolgt, sondern über einen Sonos-Lautsprecher. Entsprechend muss dies konfigurierbar und eintsellbar sein.

---

## Mögliche Forschungsschwerpunkte

- Vergleich von Offline-STT-Engines: `Whisper`, `Vosk`, `DeepSpeech`
- Evaluation von NLU-Systemen: `Rasa`, `Snips`, Transformer-basierte Lösungen
- Training eigener Hotword-Modelle mit `Porcupine`, `Mycroft Precise`
- Einfluss der Hardware auf Performance (z. B. Raspberry Pi vs. Jetson Nano)
- Kombination regelbasierter und ML-basierter Dialogmodelle
- Lokaler Datenschutz vs. Cloud-Performance

---

## Eingesetzte Technologien (Auswahl)

- Programmiersprache: **Python**
- Hotword-Erkennung: `Porcupine`, `Mycroft Precise`
- Speech-to-Text: `Whisper`, `Vosk`, `DeepSpeech`
- NLU: `Rasa`, `Snips`, eigene ML-Modelle mit `transformers`
- TTS: `Coqui TTS`, `eSpeak`, `Festival`
- Plattform: **Raspberry Pi 5**, ggf. Mini-PC

(Unten sieht man, dass es hier verschiedene Alternativen zur Auswahl gibt. Auch mit Erfahrungen während der Bearbeitung darf hier natürlich die Technologien getauscht werden.)

---

## Voraussetzungen

- Interesse an Sprachverarbeitung, KI und Embedded Systems
- Gute Kenntnisse in Python
- Grundverständnis maschinellen Lernens und Linux-Systemen
- Motivation zur Arbeit mit Open-Source-Technologien

---

## Betreuung & Rahmen

- Diese Arbeit eignet sich besonders für Studierende der Informatik, Medieninformatik oder verwandter Studiengänge.
- Umfang: Bachelorarbeit (ca. 16 Wochen Vollzeit)
- Bei Interesse ist eine Weiterentwicklung zur Masterarbeit denkbar.

---

## Beispielaufbau & Vorgehensweise

Ein eigener Sprachassistent ist ein spannendes Projekt – technisch anspruchsvoll, aber mit vielen Freiheiten in der Gestaltung. Unten findest du eine systematische Übersicht über alle benötigten **Komponenten**: Hardware, Software, KI-Module und die empfohlene Toolchain.

---

### 🔧 **1. Hardware**

#### ✅ *Mindestanforderungen (für lokale Verarbeitung)*:

* **Mini-PC oder Einplatinenrechner**:

  * *Raspberry Pi 5 (2GB/4GB/8GB/16GB)* – Einsteigerfreundlich, für einfache Anwendungen ausreichend.
  * *Alternativen*: NVIDIA Jetson Nano (wenn du KI lokal trainieren willst), oder ein Mini-PC wie Intel NUC.
* **Mikrofon**:

  * *USB-Mikrofon* mit guter Aufnahmequalität (z. B. Samson Go Mic, ReSpeaker 2-Mic HAT für Raspberry Pi).
  * *Array-Mikrofone* (für bessere Erkennung und Geräuschunterdrückung).
* **Lautsprecher**:

  * Per Klinke oder USB anschließbar. Auch kleine Bluetooth-Speaker sind möglich.

> 🔁 Empfehlung: Beginne mit einem USB-Mikrofon + Pi 5 oder Mini-PC zum Testen, bevor du in Mikrofon-Arrays oder Jetson investierst.

---

### 🧠 **2. Software-Komponenten & Architektur**

Die Sprachassistenz umfasst mehrere Stufen:

| **Schritt**                   | **Funktion**                           | **Empfohlene Tools/Libs**                                        |
| ----------------------------- | -------------------------------------- | ---------------------------------------------------------------- |
| **1. Hotword Detection**      | Erkennung des Aktivierungswortes       | `Porcupine`, `Snowboy`, `Mycroft Precise`, `Picovoice`           |
| **2. Spracherkennung (ASR)**  | Umwandlung von Sprache in Text         | `Vosk`, `Whisper`, `DeepSpeech`, Google STT (Cloud)              |
| **3. NLU (Intent-Erkennung)** | Bedeutung/Text in Aktion umwandeln     | `Rasa`, `Snips NLU`, eigene Modelle mit `Transformers`           |
| **4. Antwortgenerierung**     | Aktion ausführen oder Antwort erzeugen | Eigene Logik / GPT-Anbindung / Regelbasiert                      |
| **5. Text-zu-Sprache (TTS)**  | Text in Sprache umwandeln              | `Coqui TTS`, `Festival`, `eSpeak`, Google TTS, `ResponsiveVoice` |

Neben der Sprachassistenz benötigt man meiner Meinung nach `Flask` für die `REST API` und für eine `Weboberfläche` des Sprachassistenzen. Wahrscheinlich konfiguriert man über die Weboberfläche Triggersätze. Entsprechend wird auch eine Datenbank (`SQLite` mit `SQLAlchemy`-Anbindung) für Passwörter und Regeln benötigt.

#### Beispielvergleich

Die Auswahl von verschiedenen Libraries kann ja Vor- und Nachteile hervorheben.

##### Hotword Detection

| **Library** | **Pro**     | **Contra** |
|-------------|-------------|------------|
| `Porcupine` | * Python 3  | * Bedingt cloudbasiert (kann sein, dass man tatsächlich die Cloudanbindung nutzen muss). |
|             | * kostenlos |            |
| `Snowboy`   | * kostenlos | * Python 2 |
|             | * lokal     | * Training eingestellt/nicht mehr verfügbar (zumindest mein Wissenstand, kann mich auch täuschen.) |
| `Mycroft Precise` | * Python 3 (& Python 2) | |
|                   | * kostenlos | |
|                   | * lokal | |
| `Picovoice` | * Python 3 | * cloudbasiert |
|             | * kostenlos | * depracated (man soll stattdessen `Porcupine` verwenden. |

Dies ist jetzt nur eine kleine Übersicht, was man machen kann im Vergleich. Am besten Quellen ranziehen (z. B. Webseite, Doku, usw.). Man vergleicht am Besten `Python 3 / Python 2` (`Python 2` ist ein Ausschlusskriterium). In der auf NodeJS-basierenden Software `MagicMirror` wird für die Spracherkennung `Snowboy` eingesetzt. Dort ist als `Hotword` bereits `MagicMirror` vortrainiert. Ein neues Hotword kann nicht mehr trainiert werden, weil `Snowboy` deprecated ist und der Service für das Training meines Wissens sogar eingestellt wurde. Selbstverständlich schaut man sich an, ob etwas cloudbasiert funktioniert, lokal oder teilweise cloudbasiert ist. Ebenfalls wichtig ist ja, ob etwas kostenlos ist oder nicht. Hin und wieder gibt es ja auch Abomodelle, dass man vielleicht 1000 Requests kostenlos hat oder Centbeträge für einzelne Requests zahlt, usw. Manche Libraries nutzen `C`, `C++` im Background und es wird nur ein `Python-Wrapper` verwendet. Dies bedeutet, dass das Training performanter und hardwarenäher ist.

##### ASR/TTS

Es gibt extrem viele Libraries für `Text-to-Speech`. Die meisten benötigen eine Cloud. Also wirklich fast alle. Dies ist ein klarer Nachteil. Manche sind immer noch auf Basis von `Python 2`. Einige unterstützen sowohl `Python 2`, als auch `Python 3`. Manche gehen natürlich nur mit `Python 3`. Ergo kann man auch hier alles was `Python 3` nutzt als Vorteil ansehen. Das Ergebnis, bzw. die Sprachqualität von cloudbasierten Libraries ist ganz klar besser. Die lokal laufenden Systeme klingen im Deutschen so, als würde ein Amerikaner Deutsch reden und dann klingt es sehr technisch, mechanisch nach einem Roboter, während cloudbasierte Ergebnisse meist einem menschlichen Sprecher sehr nahe kommt. Je nachdem dauert die Umwandlung lange. Bei cloudbasierten Lösungen gibt es welche, die sehr schnell gute Ergebnisse zurückliefern. Cloudbasiert und damit viel Rechenleistung bedeutet aber tatsächlich nicht zwangsläufig, dass es schnell geht, bis die Datei generiert wird. Auch hier gibt es möglicherweise Latzenzen. Lokal laufende Lösungen haben zwar keine Latzen zwecks Netzwerkauslastung oder schlechter Internetverbindung, sind aber oft auch langsam, weil die Rechenkapazität geringer ist, als in einer Cloud. Man kann vergleichen, welche Datenformate generiert werden (`wav`, `mp3`, usw.). Ich denke für die meisten Szenarien ist es egal, welches Audioformat generiert wird. Wenn man aber bspw. Sonos-Lautsprecher verwenden möchte, dann kann es sein, dass nicht jedes Audioformat unterstütz wird. Vor- und Nachteile in der Bewertung kann es sein, ob Sprecherstimmen (verschiedene Frauen- oder Männerstimmen) konfigurierbar sind, ob Sprachen konfigurierbar sind oder ob `Speech Synthesis Markup Language` (`SSML`) unterstützt wird. `SSML` ist ein ganz klarer Vorteil und gerade im Vergleich zu `Amazon Alexa`, wäre es vielleicht sogar sehr sinnvoll, da `Alexa` dies unterstützt. 

Es können auch weitere Kritieren noch für einen Vergleich herangezogen werden.

Anmerkung: Es gibt verschiedene Libraries, die im Hintergrund aber bspw. denselben Service wie `Google STT` verwenden. Das heißt, viele Libraries stellen zu manch einer Library kaum eine echte Alternative dar.

##### NLP

Selbstverständlich kommt hier auch wieder `Python 2` vs. `Python 3` vor, wenn man die Libraries vergleicht. Da es hier um KI geht, ist auch entscheidend, ob `CPU` oder `GPU` verwendet werden kann. Gemessen an der schwachen Leistung des Endgeräts (z. B. Raspberry Pi), würde es hier auch Sinn ergeben, ob man eine `TPU`-Unterstützung oder andere Coprozessoren verwenden könnte. Es gibt auch KIs, die über eine Cloud trainiert werden. Bei manchen KIs, in einer Cloud lädt man Daten hoch, trainiert sie, lädt Daten herunter und kann die trainierten Daten zu seiner Anwendung hinzufügen. Dies wäre dann zumindest bedingt offline und lokalfähig, weil man nur beim Training eine Internetverbindung zwingend benötigt hat. Es gibt KIs, die sind leichtgewichtig und besonders gut für `Raspberry Pi's` geeignet.

##### Antwortgenerierung

Ich denke dies kann man mehrschichtig aufbauen. Als erstes nutzt man z. B. die regelbasierten Antworten, wenn dort keine Regel auftaucht können Websuche oder KIs angebunden sein, usw. Hier kann man auch Vor- und Nachteile diskutieren. Eine eigene KI zu trainieren edeutet, dass man sehr vielseitig trainieren müsste und immens viele Daten bräuchte. Ein `GPT` (`Generative Pre-trained Transformer`) bedeutet ja in der Regel immer eine Cloud-Lösung (es gibt auch Docker-Container, die man lokal laufen lassen und trainieren kann).

### Speech-to-Text

Nicht selten sind es auch dieselben Cloud-Dienstleister, wie bei `Text-to-Speech`. Ich behaupte, es sind auch sehr vegleichbare und ähnliche Kriterien, sowie dieselben Vor- und Nachteile.

---

### 🧠 **3. KI-Modelle: Wofür und wie trainieren?**

#### Du brauchst KI für:

* **Hotword Detection** (Custom Wakewords)
* **Intent-Erkennung** (z. B. “Wie wird das Wetter morgen?”)
* (Optional) **Dialog-Management** (Kontext verstehen)
  * Man kann dies Regelbasiert machen. Empfehlenswert wäre eine Oberfläche, bei der ich Triggersätze hinzufüge. Sogenannte `WENN-DANN`-Regel sind im Smart Home Gang und Gäbe. Man wählt bspw. `WENN` aus und schreibt einen Triggersatz, klickt dann auf `DANN` und wählt bspw. `openHAB Command` und sendet einen `Command` an ein openHAB Item oder man wählt bspw. `openHAB State` und macht eine Statusabfrage von einem openHAB Item.
  * Grundsätzlich kann man es so aufbauen, dass sobald ein Kontext nicht verstanden wird, eine KI versucht zu verstehen, was gemeint ist oder eine Websuche machen, sobald etwas erkannt wird, was in keiner Regel hinterlegt ist.
  * Man kann auch im Regelsystem direkt integrieren, dass z. B. Termineerinnerungen gesetzt werden sollen, das Web durchsucht werden soll, usw. Man kann ja auch absichtlich Triggersätze bauen, die nicht zwangsläufig mit Smart Home zu tun haben müssen.

##### Modelle:

* Hotword-Erkennung → kleine CNNs oder vortrainierte Modelle (Snowboy, Porcupine).
* Intent-Erkennung → NLP-Modelle wie BERT oder einfache Klassifikatoren (z. B. `scikit-learn`, `spaCy`, `Rasa`).
* Für Antwortgenerierung kannst du GPT lokal oder per API (OpenAI, HuggingFace) einbinden.

##### Training:

* Hotword-Erkennung: ca. 20–50 Audiobeispiele pro Wakeword.
* Intent-Erkennung: Beispiel-Intents in YAML/JSON, evtl. Feintuning mit `Rasa` oder `Transformers`.

> 💡 Für viele Anwendungsfälle ist kein eigenes Training notwendig, da es gute **pretrained Modelle** gibt.

---

### 💻 **4. Entwicklungsumgebung**

#### 🔤 **Programmiersprache:**

* **Python**: Klare Empfehlung, da fast alle relevanten Frameworks hier existieren.
* **Alternative** (falls Performance kritisch ist): Rust oder C++ (z. B. für Low-Level Audio-Handling).

#### 🧰 **Empfohlene Frameworks**:

* `speech_recognition` (Wrapper für verschiedene STT-Systeme)
* `Rasa` (NLU/Dialogmanagement)
* `transformers` von HuggingFace (für moderne NLP)
* `PyTorch` oder `TensorFlow` (für eigene Modelle)
* `Flask` oder `FastAPI`
  * Ich empfehle `Flask`, da man hier nicht nur eine `REST API` bauen kann, sondern auch eine Weboberfläche. Über die Weboberfläche kann man ja bspw. Triggersätze konfigurieren. 
* `pyaudio` oder `sounddevice` (für Mikrofonzugriff)
* `python-openhab-rest-client` (für openHAB)
* `SQLAlchemy` (für Datenbanken)

---

### 🧱 **5. Architekturübersicht (Ablauf)**

```plaintext
[Mikrofon] → [Hotword Detection] → [Spracherkennung (ASR)] → [Intent-Erkennung (NLU)]
            → [Antwort (Aktion/Dialog)] → [Text-To-Speech] → [Lautsprecher]
```

#### Beispiel-Stack (lokal lauffähig):

* **Hotword**: Porcupine (Wakeword: „Hey Kiro“)
* **ASR**: Vosk (lokal) oder Whisper
* **NLU**: Rasa (mit YAML-Intents)
* **TTS**: Coqui TTS
* **Logik**: Python-Controller, der Aktionen oder Antworten triggert

---

### 🧪 **6. Zusätzliche Tipps**

* **Modular entwickeln**: Trenne jeden Schritt in eigene Komponenten (Hotword, STT, NLU, TTS).
* **Offline-fähigkeit prüfen**: Whisper, Vosk, Rasa, Coqui können lokal laufen.
* **Sicherheit**: Wenn du eine Cloud-Lösung einsetzt (z. B. OpenAI API), achte auf Datenschutz.
* **Trigger/Custom Intents**: Rasa erlaubt einfache Erweiterung mit neuen „Stories“ oder „Intents“ ohne tiefes KI-Training.

Anmerkung: Cloud- oder Web-Anbindungen können ja das System jederzeit erweitern. In Abgrenzung zu gängigen Sprachassistenzen läuft dieser ja trotzdem lokal. Sollte es Internetprobleme geben, dann verzichtet man nur auf einen Teil der Funktionalitäten, aber die Basisfunktionen gehen noch, während bei einem typischen Sprachassistent ohne Internetverbindung nichts mehr funktionieren würde.

---

### 🚀 Einstiegsvorschlag (für Prototypen)

1. **Raspberry Pi 5 + USB-Mikrofon**
2. Hotword: `Porcupine` (kostenloser Wakeword-Engine)
3. STT: `Whisper` oder `Vosk`
4. NLU: `Rasa` (mit einfachen Intents)
5. TTS: `Coqui TTS` oder Google TTS API
6. Alles orchestriert mit `Python`

---

## Hintergrundwissen

### Unterschied Chatbot vs. Sprachassistent

Prinzipiell gilt:

```
Chateingabe ≙ Spracheingabe (Speech-to-Text)
Chatausgabe ≙ Sprachausgabe (Text-to-Speech)
```

Entspricht der Aufbau von einem Chatbot wirklich dem Aufbau eines Sprachassistenten? Nicht wirklich. Sie funktionieren sehr ähnlich. Ein Großteil der Architektur kann gleich aufgebaut werden. Man kann auch `Fuzzy Matching` verwenden. Es wird `NLP` in beiden Systemen angewandt. Auch ein Sprachassistent benötigt dringend ein `Intent`. Am Beispiel `Fuzzy Matching` wird deutlich, dass dies in einem Chatbot sehr viel notwendiger sein wird, als in einem Sprachassistenten. Tippfehler kann man bspw. mit `Fuzzy Matching` korrigieren, nicht gehörte oder verrauschte Worte, können damit jedoch nicht korrigiert werden. Bei einem Chatbot kann es mal entstehen, dass einzelne Buchstaben in einem Wort fehlen oder verdreht sind, bei einer Spracherkennung hingegen wird ein Wort entweder erkannt oder nicht erkannt.

### Fuzzy Matching

Das **Fuzzy Matching** kann man im deutschen als unscharfe Suche betiteln. Es ist eine Klasse von String-Matching-Algorithmen, mit der man bestimmte Zeichenketten (Strings) in einer längeren Zeichenkette oder einem Text suchen bzw. finden können sollte.  Es wird daher auch oft als Fuzzy-Suche oder Fuzzy-String-Suche betitelt.

Aus:

[https://www.klippa.com/de/blog/informativ/fuzzy-matching-de/](https://www.klippa.com/de/blog/informativ/fuzzy-matching-de/)

Mit Fuzzy-Matching kann ich darauf reagieren, wenn z. B. nur 80% eines Wortes/Satzes richtig erkannt wurde. Hier mal Beispiele.

```
Gesucht: Bestellnummer

Richtig wären:
Bestellnr.
Bestelnummer
Bestellnumer
Bestellnummmer
...
```

Also mögliche Tippfehler oder auch Abkürzungen sollen ja darufhin deuten, dass ein- und dasselbe Worte gemeint ist. In Zusammenhang mit Synonymen würde man durch Fuzzy Matching dem Benutzer so extrem viele Eingabemöglichkeiten ermöglichen. Man formuliert ja nicht nur Sätze um, man vertippt sich auch mal oder kürzt einzelne Wörter ab.

**Fuzzy Matching** nimmt also Korrekturen vor. Dies könnten im allgemeinen die nachfolgenden sein:

* **Einfügen** – Hinzufügen eines Buchstabens zur Vervollständigung des Wortes (z. B. `Rechnun` wird zu `Rechnung`)
* **Löschen** – Entfernen eines Buchstabens aus einem Wort (z. B. `Rechnnung` wird zu `Rechnung`)
* **Substitution** – Vertauschen eines Buchstabens, um ein Wort zu korrigieren (z. B. `Technung` wird zu `Rechnung`)
* **Transposition** – Vertauschen von Buchstaben, um ein Wort zu korrigieren (z. B. `Rehcnung` wird zu `Rechnung`)

Jeder Korrektur, die durchgeführt werden muss, wird eine „Bearbeitungsdistanz“ von 1 zugeschrieben. Die Bearbeitungsdistanzen beeinflussen die oben erwähnte Trefferquote. Wenn Sie beispielsweise eine Zeichenfolge mit 11 Zeichen haben und 2 Korrekturen vornehmen müssen, beträgt die endgültige Trefferquote **81,81 %**.

```
Berechnung: 100%- 2 / 11= 81.81%  
```

Neben diesen Korrekturen kann **Fuzzy Matching** auch verwendet werden, um Zeichensetzungen, zusätzliche Wörter und fehlende Leerzeichen in Zeichenketten oder Texten zu korrigieren.

---

#### Fuzzy-Matching-Algorithmen

Fuzzy Matching fällt in die Kategorie der Methoden, für die es keinen spezifischen Algorithmus gibt, der alle Szenarien und Anwendungsfälle abdeckt. Daher werden wir einige der am häufigsten verwendeten und zuverlässigsten Fuzzy-Matching-Algorithmen für die Suche nach ungefähren Datenübereinstimmungen behandeln:

* Levenshtein-Distanz (LD)
* Hamming-Distanz (HD)
* Damerau-Levenshtein

---

##### Levenshtein-Distanz

Die **Levenshtein-Distanz (LD)** ist eine Fuzzy-Matching-Technik, die zwei Zeichenfolgen beim Vergleich und der Suche nach einer Übereinstimmung berücksichtigt. Je höher der Wert der Levenshtein-Distanz ist, desto weiter sind die beiden Zeichenfolgen oder „Begriffe“ von einer identischen Übereinstimmung entfernt.

Wie erhalten wir nun den Wert der Levenshtein-Distanz? Die LD zwischen den beiden Zeichenfolgen entspricht der Anzahl der Änderungen, die erforderlich sind, um eine Zeichenfolge in die andere umzuwandeln. Für die LD gelten das Einfügen, Löschen und Ersetzen eines einzelnen Zeichens als Bearbeitungsoperationen.

Nehmen wir an, Sie möchten die LD zwischen „Rechnungsnummer“ und „Rechnungs-Nr.“ messen. Der Abstand zwischen den beiden Begriffen ist „1 x u“, „2 x m“ und „1 x e“, was einem Abstand von 4 entsprechen würde. Warum? Weil Sie diese Zeichen hinzufügen müssten, um eine Übereinstimmung zu erreichen. Siehe die Beispiele unten.

---

###### Levenshtein-Abstand Beispiel

> **Rechnungnummer** → Rechnung**s**nummer (Einfügung von „**s**“) – Abstand: 1  
> **Rechnung numr** → Rechnungsnu**m**m**e**r (Einfügung von „**m**“ & „**e**“) – Abstand: 2  
> **Rechnung nr** → Rechnungsn**u****m****m****e**r (Einfügung von „**u, m, m, e**“) – Abstand: 4

---

##### Hamming-Distanz

Die **Hamming-Distanz (HD)** unterscheidet sich nicht allzu sehr von der Levenshtein-Distanz. Die Hamming-Distanz wird häufig verwendet, um den Abstand zwischen zwei gleich langen Textabschnitten zu berechnen.

Die HD-Methode basiert auf der **ASCII**-Tabelle (American Standard Code for Information Interchange). Zur Berechnung des Abstandswertes verwendet der Hamming-Distanz-Algorithmus die Tabelle, um den Binärcode zu bestimmen, der jedem Buchstaben in den Zeichenketten zugeordnet ist.

---

###### Hamming-Abstand-Beispiel

Nehmen wir die folgenden Textzeichenfolgen „Number“ und „Lumber“ als Beispiel. Wenn wir versuchen, den HD zwischen den Zeichenfolgen zu bestimmen, ist der Abstand nicht 1, wie es mit dem Levenshtein-Algorithmus der Fall wäre. Stattdessen würde er 10 betragen. Das liegt daran, dass die ASCII-Tabelle einen Binärcode von **(1001110)** für den Buchstaben **N** und **(1001100)** für den Buchstaben **L** anzeigt.

Beispielrechnung:

> **D** = N – L = 1001110 – 1001100 = **10**

---

##### Damerau-Levenshtein

Das Damerau-Levenshtein-Verfahren misst auch den Abstand zwischen zwei Wörtern, indem es die erforderlichen Änderungen misst, die vorgenommen werden müssen, um ein Wort an das andere anzupassen. Diese Änderungen hängen von der Anzahl der Operationen ab, wie z. B. Einfügung, Löschung oder Ersetzung eines einzelnen Zeichens oder Transposition zweier benachbarter Zeichen.

Hier unterscheidet sich die Damerau-Levenshtein-Distanz von der regulären Levenshtein-Distanz, da sie zusätzlich zu den Einzelzeichen-Editieroperationen, auch Transpositionen berücksichtigt, um eine ungefähre Übereinstimmung zu finden (Fuzzy Match).

---

###### Damerau-Levenshtein Beispiel

> **Zeichenfolge 1:** Re<strong>ch</strong>nun<strong>g</strong>  
> **Zeichenfolge 2:** Re<strong>hc</strong>nun  
>   
> **Operation 1:** Transposition → Vertauschen der Zeichen „**h**“ und „**c**“  
> **Operation 2:** Einfügen eines „**g**“ am Ende der Zeichenfolge 2


Da zwei Operationen erforderlich waren, um die beiden Wörter identisch zu gestalten, **beträgt der Abstand 2**. Vereinfacht ausgedrückt zählt jede Operation wie Einfügung, Löschung, Transposition usw. als ein Abstand von „1“. Mit der Levenshtein-Distanz müssten Sie jedoch drei Korrekturen vornehmen, was einem Abstand von 3 entspricht.

Alle oben genannten Fuzzy-Matching-Algorithmen unterscheiden sich natürlich in der Art und Weise, wie die Bearbeitungsdistanz berechnet wird. Dies ist der Grund, warum es keinen FM-Algorithmus gibt, der für alle geeignet ist. Von den drei vorgestellten Algorithmen ist die Levenshtein-Distanz jedoch der am häufigsten verwendete FM-Algorithmus in der Datenverwaltung und Datenwissenschaft.

Empfehlung: In Python kann man die [fuzzywuzzy-Bibliothek](https://github.com/seatgeek/fuzzywuzzy) testen oder einen eigenen Algorithmus implementieren.

---

### Intent, Entity, Confidence Score

Aus:

[https://www.melibo.de/blog/was-sind-intent-und-entity](https://www.melibo.de/blog/was-sind-intent-und-entity)

---

#### Intent

Intents, zu Deutsch „Absichten“, sind Zwecke oder Ziele, die in den Eingaben eines Kunden zum Ausdruck kommen, um z.B. eine Frage zu einer Retoure zu stellen. Durch die Erkennung der Absicht, die sich in der Kundeneingabe ausdrückt, versucht der KI-Chatbot den richtigen Dialog zu finden und die passende Ausgabe zu wählen. Dafür nutzen KI-Chatbots maschinelles Lernen, um in natürlicher Sprache die vorher definierte Absicht (Intent) zu erkennen. [1] Einfach gesagt, Intents sind Fragen der User:innen, die dem Chatbot zu einem speziellen Thema gestellt werden und der Versuch des KI-Chatbots, die passende Antwort zu erkennen, um das Problem zu lösen bzw. die Frage zu beantworten.

---

##### Wie funktionieren Intents?

Bestehende Anbieter wie unter anderem der IBM Watson Assistant [1], Rasa [2] oder Microsoft LUIS [3] basieren alle meist auf dem Prinzip der Intent-Ausgabe. Bevor der Chatbot Intents erkennen kann, müssen erst mal alle Absichten der User:innen definiert werden. Hierfür ist es wichtig, dass man seine Kundenanfragen erst mal identifiziert und seinen Use-Case richtig versteht. Nachdem der Intent-Katalog erstellt und der Bot online genommen wurde, werden die User:innen dem Chatbot Fragen stellen. Jede Anfrage der User:innen durchläuft das sogenannte Intent-Matching, also der Zuordnung der Anfrage aus den gesamten Inhalten des Chatbots. Dabei wird anhand von NLP (Natural Language Processing) ein Confidence-Score berechnet, um anhand von Wahrscheinlichkeiten die passende Antwort auszugeben.

---

#### Confidence-Score

Ein kurzer Exkurs zum Thema Confidence-Scores. Die Confidence-Scores liegen zwischen 0 und 1 und geben an, zu wie viel Prozent der Chatbot ein Intent erkannt hat. Zu jeder gestellten Frage der User:innen berechnet der Chatbot also einen Confidence-Score und versucht auf dieser Grundlage, durch Wahrscheinlichkeiten, die richtige Antwort an die User:innen auszugeben. Die Confidence-Scores sind meistens voreingestellt und liegen zwischen 0,6 und 0,7. Das heißt, dass der Chatbot Antworten nur dann ausgibt, wenn die Erkennungswahrscheinlichkeit bei mindestens 60 % liegt. Nehmen wir nun als Beispiel an, dass User:innen die Frage stellen „Was kannst du so?“, um zu erfahren, welche Themen der Chatbot überhaupt beantworten kann. Der KI-Chatbot erkennt zu 89 % Prozent den Intent „Was kannst du?“. In diesem Beispiel gibt der Chatbot die passende Antwort aus und beantwortet somit die Frage.

---

##### Bestandteile eines Intents

Ein Intent besteht also aus dem: Intentnamen, dem User-Input, dem Confidence-Score und einer Antwort. Abhängig von der genutzten Technologie können noch weitere Bestandteile dazu kommen, wie Variablen, Actions und Entities.

Ein Beispiel für Intents

(1) User-Input: Frage eines Users

🙎‍♂️: „Vorteile eines Chatbots“
🙎‍♂️: „Was können Chatbots besonders gut?“
🙎‍♂️: „Warum sollte ich einen Chatbot holen?“

(2) Confidence-Score und (3) Intentnamen „Vorteile Chatbot“: Berechnung der Wahrscheinlichkeit anhand vom User-Input

🧮 : 100 % Erkennung des Intents „Vorteile Chatbot“

(4) Antwort des Chatbots auf die Frage „Vorteile Chatbots“

🤖 : Zu den Vorteilen von Chatbots gehören unter anderem und abhängig von der Branche: Automatisierung von Prozessen, wodurch Fehler beim Support reduziert sowie Zeit und Geld eingespart werden können. Verkürzte Wartezeiten für den Kunden. 24/7 Kundensupport. Effizientere Strukturen. Weniger manueller Aufwand für dich.

---

#### Entity

Im Unterschied zu Intents dienen Entitys oder auch Entities dazu, Informationen der User:innen aus der natürlichen Sprache zu extrahieren. Jedes Entity verfügt über eine Reihe von Eigenschaften, die mit ihr verbunden sind. Dabei kannst du auf Informationen deines Entities zugreifen. Wie bei einem Intent gibt der Chatbot an, wie hoch der Confidence-Score liegt. Im Unterschied zu Intents liegt der Confidence-Score, aber bei 0 oder 1. Unabhängig davon, ob und wie die Erkennung des Entitys eingestellt ist, haben sogenannte System-Entites immer eine Erkennung von 1. Jedes Entity besitzt einen Wert, einen sogenannten Entitätswert. Bei der Erstellung von Entities ist es notwendig, dass neben dem Wert auch Typen definiert werden. Unter Typen versteht man allgemein Synonyme, also Wörter, die sich ebenfalls auf dasselbe Vorhaben beziehen, es nur anders umschreiben. Je mehr Synonyme ein Entity hat, desto besser die Erkennung des Chatbots [4].

Grundsätzlich unterscheiden wir zwischen System-Entities und Customize-Entities. Die System-Entites sind voreingestellt, das heißt im System bereits enthalten. Darunter fallen etwa Zahlen, Uhrzeiten oder Adressen. Diese Entities sind besonders beliebt und wurden in der Vergangenheit besonders häufig verwendet. Die Customize-Entities dagegen sind selbst definierte Werte, die auf den jeweiligen Use-Case angepasst werden.

---

##### Ein Beispiel für Entities in der Praxis

🙎‍♂️: „Wann kommt mein Produkt Chatbot-Experte in der Landstraße 5 an?“

---

##### Entities in diesem Beispiel:

Produkt Chatbot Experte (Customize-Entity product_type)
Landstraße 5 (Sytem-Entity street_adress)

---

##### Vorteile von Entites:

Mit Entities kann man User:innen durch Chat Flows in Form von Buttons navigieren, schnell Synonyme definieren und mehrere Anfragen auf einmal verarbeiten. Die System-Entitys helfen außerdem dabei, die beliebten Anfragen zum Thema Ort, Zeit und Adresse abzufangen.

[1]: https://cloud.ibm.com/docs/assistant?topic=assistant-intents
[2]: https://rasa.com/open-source/
[3]: https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-concept-utterance
[4]: https://cloud.ibm.com/docs/assistant?topic=assistant-expression-language

---

#### Speech Synthesis Markup Language (SSML)

**SSML (Speech Synthesis Markup Language)** ist eine XML-basierte Auszeichnungssprache, mit der du **Text-to-Speech (TTS)**-Ausgaben präzise steuern kannst – also wie ein Sprachsynthesizer den gesprochenen Text betonen, pausieren oder variieren soll.

---

##### 🧠 Was kann man mit SSML machen?

Mit SSML kannst du steuern:

* 🗣 **Aussprache** (`<phoneme>`)
* ⏸ **Pausen** (`<break>`)
* 🎵 **Tonhöhe, Lautstärke, Sprechgeschwindigkeit** (`<prosody>`)
* 📢 **Sprachstil / Emphasis** (`<emphasis>`, `<voice>`)
* 🌍 **Sprache und Stimme** (`<lang>`, `<voice>`)
* 🔁 **Audioeinspielung** (`<audio>` – je nach Engine)

---

##### ✅ Einfaches Beispiel

```xml
<speak>
  Hallo! <break time="500ms"/> Wie kann ich dir helfen?
</speak>
```

⏱ Fügt eine **halbe Sekunde Pause** ein.

---

##### 📌 Häufig genutzte SSML-Tags (Auswahl)

| Tag          | Funktion                                | Beispiel                                              |
| ------------ | --------------------------------------- | ----------------------------------------------------- |
| `<speak>`    | Wurzel jeder SSML-Ausgabe               | `<speak>Text hier</speak>`                            |
| `<break>`    | Fügt Pause ein                          | `<break time="300ms"/>`                               |
| `<prosody>`  | Steuert Tonhöhe, Lautstärke, Tempo      | `<prosody pitch="+10%" rate="slow">...</prosody>`     |
| `<emphasis>` | Betonung auf Wort/Satz                  | `<emphasis level="strong">wichtig</emphasis>`         |
| `<phoneme>`  | Definiert Lautschrift                   | `<phoneme alphabet="ipa" ph="ˈhæloʊ">Hallo</phoneme>` |
| `<lang>`     | Schaltet auf andere Sprache             | `<lang xml:lang="en-US">Hello!</lang>`                |
| `<audio>`    | Spielt Audiodatei ab (nur in Cloud-TTS) | `<audio src="sound.mp3"/>`                            |

---

##### 🎙 Beispiel: Mehrsprachiger Text mit Betonung

```xml
<speak>
  Willkommen bei deinem Sprachassistenten. <break time="300ms"/>
  <emphasis level="strong">Heute</emphasis> ist ein schöner Tag.
  <lang xml:lang="en-US">The weather is sunny and warm.</lang>
</speak>
```

---

##### ⚙️ Kompatibilität

| TTS-Engine            | SSML-Unterstützung | Hinweis                               |
| --------------------- | ------------------ | ------------------------------------- |
| **Google TTS**        | ✅ Vollständig      | auch `<audio>`-Einbindung möglich     |
| **Amazon Polly**      | ✅ Sehr gut         | unterstützt viele `<voice>`-Stile     |
| **Coqui TTS**         | ⚠️ Teilweise       | `<break>`, `<prosody>` oft verfügbar  |
| **eSpeak / Festival** | ❌ Kaum             | meist keine native SSML-Unterstützung |

---

##### 🛠 Tipps für die Praxis

* SSML hilft, die **Natürlichkeit und Verständlichkeit** von Sprachausgaben zu steigern.
* Nutze es gezielt bei langen Dialogen, Aufzählungen oder multilingualem Content.
* Für lokale Assistenten mit z. B. `pyttsx3` ist SSML oft **nicht direkt nutzbar** – dort musst du ggf. Pausen und Tonhöhe manuell steuern.

---

Ja, **es gibt mehrere Text-to-Speech-Lösungen, die vollständig lokal arbeiten**, also **ohne Cloudverbindung** funktionieren. Das ist besonders wichtig für Datenschutz, Offlinefähigkeit und Embedding in Systeme wie deinen Sprachassistenten.

---

##### 🗣️ **Lokale TTS-Libraries – ohne Cloud**

Hier ist eine Übersicht:

| Library / Tool | Sprache | Lokal | Qualität | SSML-Unterstützung     | Bemerkung                              |
| -------------- | ------- | ----- | -------- | ---------------------- | -------------------------------------- |
| **Coqui TTS**  | Python  | ✅     | ⭐⭐⭐⭐☆    | Teilweise (SSML-light) | Moderne TTS, sehr anpassbar            |
| **eSpeak NG**  | C/C++   | ✅     | ⭐⭐☆☆☆    | ❌                      | Extrem leichtgewichtig, viele Sprachen |
| **Festival**   | C++     | ✅     | ⭐⭐☆☆☆    | ❌                      | Veraltet, aber stabil                  |
| **RHVoice**    | C++     | ✅     | ⭐⭐⭐☆☆    | ❌                      | Gute Qualität, Open Source             |
| **pyttsx3**    | Python  | ✅     | ⭐⭐☆☆☆    | ❌                      | Wrapper um native Engines              |
| **MaryTTS**    | Java    | ✅     | ⭐⭐⭐☆☆    | ✅                      | Alte, aber robuste Plattform mit SSML  |
| **OpenTTS**    | Docker  | ✅     | ⭐⭐⭐⭐☆    | Ja (je nach Backend)   | Aggregiert Coqui, MaryTTS, etc.        |

---

##### 🔍 Kurze Details zu den wichtigsten:

###### 🔸 **Coqui TTS**

* Modernes, Deep-Learning-basiertes System (Fork von Mozilla TTS)
* Offline trainierbar oder fertige Modelle nutzbar
* CLI & Python API
* Stimmen für viele Sprachen, inkl. Deutsch
* Eingeschränkte SSML-ähnliche Features (`<break>`, `rate`, `pitch` per CLI/Args)

👉 Empfohlen für dein Projekt, wenn du **Qualität + Lokalität** brauchst

---

###### 🔸 **eSpeak NG**

* Extrem leicht (\~2MB)
* Unterstützt 100+ Sprachen, aber Robot-ähnliche Stimme
* Ideal für embedded / low-resource-Systeme

👉 Für Prototypen oder Systeme mit **minimaler Leistung**

---

###### 🔸 **pyttsx3**

* Python-Wrapper um native TTS-Engines (Windows: SAPI5, macOS: NSSpeechSynthesizer, Linux: espeak)
* Einfach zu benutzen, aber begrenzte Kontrolle über Stimme und Stil
* Keine SSML-Unterstützung, nur rudimentäres Timing

👉 Gut für **schnelle Demos** oder klassische Desktop-Sprachsynthese

---

###### 🔸 **MaryTTS**

* Java-basiert, mit Webserver & REST-API
* Unterstützt echtes SSML
* Deutsche Stimme enthalten
* Längere Startzeit, komplexer Aufbau

👉 Für **akademische oder stabile serverseitige Anwendungen**

---

##### ✅ Empfehlung für Bachelorarbeit

| Ziel                                     | Empfehlung                         |
| ---------------------------------------- | ---------------------------------- |
| Moderne TTS, lokale Nutzung, erweiterbar | **Coqui TTS**                      |
| Einfache Integration, geringe Ressourcen | **pyttsx3** oder **eSpeak NG**     |
| Vollständige SSML-Verarbeitung lokal     | **MaryTTS** oder Coqui (teilweise) |

---
