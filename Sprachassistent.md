# Bachelorarbeit: Entwicklung eines modularen, datenschutzfreundlichen und lokalen Sprachassistenten

| | |
| --- | --- |
| **Art** | Bachelorarbeit (Erweiterung zur Masterarbeit denkbar) |
| **Schwerpunkte** | Sprachverarbeitung (Wakeword, STT, NLU, TTS), Embedded Systems, Datenschutz, REST |
| **Verwandte Ideen** | [Chatbot](Chatbot%20f%C3%BCr%20openHAB.md) – gleiche zentrale API, Text statt Sprache |

<!-- TOC -->
## Inhaltsverzeichnis

- [Mögliche Titelvorschläge](#mögliche-titelvorschläge)
- [Hintergrund](#hintergrund)
  - [Problemstellung im Detail](#problemstellung-im-detail)
- [Ziel der Arbeit](#ziel-der-arbeit)
- [Teilziele](#teilziele)
- [Mögliche Forschungsschwerpunkte](#mögliche-forschungsschwerpunkte)
- [Abgrenzung: openHAB bringt eigene Sprachdienste mit](#abgrenzung-openhab-bringt-eigene-sprachdienste-mit)
- [Eingesetzte Technologien (Auswahl)](#eingesetzte-technologien-auswahl)
- [Voraussetzungen](#voraussetzungen)
- [Betreuung & Rahmen](#betreuung--rahmen)
- [Beispielaufbau & Vorgehensweise](#beispielaufbau--vorgehensweise)
- [Mögliche Fragen](#mögliche-fragen)
  - [🔧 1. Hardware](#-1-hardware)
  - [🧠 2. Software-Komponenten & Architektur](#-2-software-komponenten--architektur)
  - [🧠 3. KI-Modelle: Wofür und wie trainieren?](#-3-ki-modelle-wofür-und-wie-trainieren)
  - [💻 4. Entwicklungsumgebung](#-4-entwicklungsumgebung)
  - [🧱 5. Architekturübersicht (Ablauf)](#-5-architekturübersicht-ablauf)
  - [🧪 6. Zusätzliche Tipps](#-6-zusätzliche-tipps)
  - [🚀 Einstiegsvorschlag (für Prototypen)](#-einstiegsvorschlag-für-prototypen)
- [Hintergrundwissen](#hintergrundwissen)
  - [Unterschied Chatbot vs. Sprachassistent](#unterschied-chatbot-vs-sprachassistent)
  - [Fuzzy Matching](#fuzzy-matching)
  - [Intent, Entity, Confidence Score](#intent-entity-confidence-score)
- [📑 Gliederungsvorschlag für die Bachelorarbeit](#-gliederungsvorschlag-für-die-bachelorarbeit)
  - [1. Einleitung](#1-einleitung)
  - [2. Theoretische Grundlagen](#2-theoretische-grundlagen)
  - [3. Anforderungsanalyse](#3-anforderungsanalyse)
  - [4. Konzept und Architektur](#4-konzept-und-architektur)
  - [5. Implementierung](#5-implementierung)
  - [6. Evaluation](#6-evaluation)
  - [7. Reflexion und Ausblick](#7-reflexion-und-ausblick)
  - [8. Fazit](#8-fazit)
  - [9. Anhang](#9-anhang)
  - [10. Literaturverzeichnis](#10-literaturverzeichnis)
- [Beispielprojekt](#beispielprojekt)
  - [📁 Projekt-Setup: voice_assistant/](#-projekt-setup-voice_assistant)
  - [🧠 Komponentenüberblick mit Mini-Codebeispielen](#-komponentenüberblick-mit-mini-codebeispielen)
  - [📦 requirements.txt](#-requirementstxt)
  - [🛠️ Was du jetzt damit machen kannst](#️-was-du-jetzt-damit-machen-kannst)
  - [🧠 Weitere Überlegungen für die Systemplanung](#-weitere-überlegungen-für-die-systemplanung)
  - [🧩 Integration in bestehende Systeme](#-integration-in-bestehende-systeme)
  - [🛠️ Entwicklung und Wartung](#️-entwicklung-und-wartung)
  - [📚 Weiterführende Ressourcen](#-weiterführende-ressourcen)
- [Sprachbefehl vs. Konversation](#sprachbefehl-vs-konversation)
  - [🧠 Theoretischer Durchlauf: Sprachbefehl statt Konversation](#-theoretischer-durchlauf-sprachbefehl-statt-konversation)
  - [🧠 Ziel: Konversationsfähiger Sprachassistent](#-ziel-konversationsfähiger-sprachassistent)
  - [📁 dialog_engine.py](#-dialog_enginepy)
- [🧠 Vergleich: Sprachbefehl vs. Konversation bei Sprachassistenten](#-vergleich-sprachbefehl-vs-konversation-bei-sprachassistenten)
  - [🔍 Interpretation:](#-interpretation)
  - [🧩 Fazit:](#-fazit-3)
- [Bonustipp](#bonustipp)
  - [ASR/STT](#asrstt)
  - [TTS](#tts)
  - [STTS und TTSS](#stts-und-ttss)
- [Vorarbeiten im Labor](#vorarbeiten-im-labor)
- [Grundlagen im Kompendium Informatik](#grundlagen-im-kompendium-informatik)
<!-- /TOC -->

---

## Mögliche Titelvorschläge

- "Entwicklung eines datenschutzfreundlichen Sprachassistenten auf Basis freier Software"
- "Entwicklung eines datenschutzfreundlichen Sprachassistenten auf Basis von Open-Source-Software"
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

### Problemstellung im Detail

- Cloudverbindungen können auch öfter mal gestört oder abgebrochen sein.
  - Ein lokaler Sprachassistent würde zumindest lokal funktionierende Befehle bearbeiten und lokal verfügbare Informationen zur Verfügung stellen können.
- Datenschutzrechtlich hört ein Sprachassistent dauerhaft mit und wartet, bis ein sog. Hotword (auch trigger word oder wake word) erkannt wird.
  - Ist wirklich garantiert, dass nur Daten aufgezeichnet und an eine Cloud gesendet werden, wenn ein Hotword verwendet wurde oder lauscht das System dauerhaft mit?
    - Rein theoretisch könnte man über Wireshark prüfen, ob ein Sprachassistent wie bspw. Alexa nicht ständig mit ihrer Cloud kommunziert. Die Annahme wäre natürlich sehr groß, dass wenn dauerhaft kommuniziert wird, dass Amazon abhört und lauscht, nicht nur nach einem Hotword aktiv Daten aufzeichnet.
  - Anmerkung: Deutsche Begriffe sind Aktivierungswort, Aufwachwort, Aufwachbefel, Triggerwort, usw.
- Sprachassistenten sind oft nur beschränkt individualisierbar, Anwendungen werden eingestellt oder eingeschränkt.
  - Google Nest ermöglicht es oft bspw. Smart Home Geräte zwar anzubinden, diese können darüber aber nicht gesteuert werden, sondern meist nur Zustände abgefragt werden.
  - Amazon hat bspw. ihre Policy geändert und seitdem wird das openHAB Skill aus Sicherheitsgründen nicht mehr unterstützt, damit wäre ein Smart Home, welches über openHAB gesteuert wurde nicht mehr bedienbar. (Aktuellen Stand vor der Ausschreibung prüfen.)

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

- Vergleich von Offline-STT-Engines: `Whisper` (verschiedene Modellgrößen und Implementierungen), `Vosk`
- Evaluation von NLU-Ansätzen: regelbasiert, klassische Klassifikatoren, `Rasa`, Transformer-basierte Lösungen, lokale LLMs
- Training eigener Hotword-Modelle mit `openWakeWord`, `Porcupine` oder `Rustpotter`
- Einfluss der Hardware auf Performance (z. B. Raspberry Pi vs. Jetson Nano)
- Kombination regelbasierter und ML-basierter Dialogmodelle
- Lokaler Datenschutz vs. Cloud-Performance

---

## Abgrenzung: openHAB bringt eigene Sprachdienste mit

openHAB besitzt eine eigene, modulare **Voice-Infrastruktur**: Keyword Spotter (z. B. Rustpotter, Porcupine), Speech-to-Text (z. B. **Whisper** lokal über whisper.cpp oder über eine kompatible API, Vosk), Human Language Interpreter (z. B. Standard-Interpreter, HABot) und Text-to-Speech (z. B. **Piper**), verbunden über das **Dialog Processing**. In der Community gibt es bereits vollständig lokale Sprachsteuerungen auf dieser Basis.

Für die Arbeit heißt das:

* Diese Infrastruktur muss im **Stand der Technik** beschrieben werden.
* Die Arbeit muss begründen, was ein **eigenständiger** Assistent darüber hinaus leistet (z. B. Unabhängigkeit vom Smart-Home-System, eigene REST API und Weboberfläche, frei konfigurierbare WENN-DANN-Regeln, Anbindung weiterer Dienste) – oder sie nutzt die openHAB-Dienste bewusst als Bausteine.

> **Aktualitätshinweis:** Diese Idee ist über einen längeren Zeitraum entstanden. In späteren Abschnitten genannte Werkzeuge wie **Snips NLU**, **DeepSpeech**, **Coqui STT**, **Mycroft** bzw. **Mycroft Precise** und **Snowboy** werden nicht mehr gepflegt; sie sind dort als Teil der Recherche stehen geblieben. Für neue Projekte die aktuellen Alternativen verwenden.

Überblick über die Verarbeitungskette und den Wartungsstatus lokaler Werkzeuge: [Sprachassistenten](https://github.com/Michdo93/Informatik/blob/main/KI%20%26%20Sprachverarbeitung/Sprachassistenten.md).

---

## Eingesetzte Technologien (Auswahl)

- Programmiersprache: **Python**
- Hotword-Erkennung: `openWakeWord`, `Porcupine`, `Rustpotter`
- Speech-to-Text: `Whisper` (z. B. `faster-whisper`, `whisper.cpp`), `Vosk`
- NLU: eigene Regeln, `scikit-learn`/`spaCy`, `Rasa`, eigene ML-Modelle mit `transformers`
- TTS: `Piper`, `eSpeak NG`, Coqui TTS (Community-Fork)
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

## Mögliche Fragen

- Sind Ressourcen für einen lokalen Sprachassistenz leistungsstark genug? (Last)
- Wie lange dauert es eine Antwort zu generieren? (Latenz)
- Wie präzise können Worte/Sätze erkannt werden? (Genauigkeit)
- Gibt es genügend kostenlose und lokale Frameworks und Libraries für die einzelnen Module eines Sprachassistenten (angenommen STT läuft lokal, bedeutet dies nicht, dass TTS auch lokal funktionieren könnte)?
- Wie häufig muss trainiert werden? Wird im laufenden Einsatz des Systems trainiert (z. B. `Reinforcement Learning`)?
- Was muss trainiert werden?
- Wie muss trainiert werden? (Zum Beispiel `online` vs. `offline`).
- Wie werden Trigger hinzugefügt?
- Wie kann ein Trigger zu einer Ausführung gelangen? (Zum Beispiel einen regelbasierten Ansatz verwenden).
- Benötige ich eine Hotword-Recognition?
  - Dies würde ich auch definitiv mit ja beantworten. Selbst wenn dieses System nicht datenschutzrechtlich bedenklich ist, sollte das System nicht dauerhaft zuhören. Hört dieses System dauerhaft zu, muss eine oder mehrere KIs die ganze Zeit analysieren, was das System ziemlich sicher überlasten würde.
- Wie lange darf eine Spracheingabe dauern?
  - Vielleicht machen 90-120 Sekunden als maximal Sinn, weil wenn ich zu viel rede, ist man einfach nicht präzise genug. Eine KI würde nicht mehr verstehen, worauf man hinaus will. Wenn man einen regelbasierten Ansatz hat, könnten aber z. B. 120 Sekunden schon viel zu lange sein, weil man dann in 120 Sekunden ja Wort für Wort denselben Satz sagen muss, wie der, der durch einen Trigger ausgelöst werden soll. Hier wäre dann je länger ein Satz ist, es umso schwieriger, dass nach Abzug von Fehlern der richtige Satz noch erkannt wird.
- Braucht eine Spracheingabe eine minimale Dauer?
  - Vielleicht macht es Sinn, dass man mindestens 15-20 Sekunden etwas sagen sollte, ansonsten wird dies verworfen, weil zu wenige Informationen vorhanden wären, als dass man damit etwas anfangen könnte. 
  - Anmerkung: Typische Sprachbefehle („Licht in der Küche an“) dauern nur **1–3 Sekunden**. Eine Mindestdauer von 15–20 Sekunden würde gerade die häufigsten Befehle verwerfen. Üblich ist, das Ende der Äußerung über eine **Sprachpause** zu erkennen (Voice Activity Detection) und nur sehr kurze Geräusche zu verwerfen. Das Whisper-Add-on von openHAB arbeitet z. B. mit einstellbarer minimaler und maximaler Aufnahmedauer sowie einer maximalen Stilledauer im Bereich weniger Sekunden.
- Brauche ich Abbruchkritierien?
  - Abbruchkriterien könnten Worte wie "Stop" oder "Halt" sein. Eventuell gekoppelt, dass man vorher erneut das Hotword sagen muss.
  - Ein Abbruchkriterium ist natürlich, dass es keinen Triggersatz gibt.
  - Weitere Abbruchkriterien sind, dass wenn eine KI analysiert oder eine Websuche gemacht wird, die eine Antwort liefert, dass dies eine Zeitüberschreitung darstellt. Ich denke niemand wartet 5-6 Minuten auf eine Antwort. Wäre hier also ebenfalls vielleicht 90-120 Sekunden sinnvoll für eine Zeitüberschreitung als Abbruchkriterium? Eine andere Zeitspanne?
- Sind Abbruchkritieren wie Worte auch eigene Hortwords oder Triggersätze?
- Wie lange muss ich Satzpausen zwischen Hotword und Triggerwort einplanen?
- Welche Hotwords eignen sich? Zum Beispiel Smart Home.
- Brauche ich mehrere Hotwords? Wenn ja, wie viele?
  - Ich würde sagen, mehr als 3-4 würden wenig Sinn ergeben. Wenn man aber bspw. Smart Home und openHAB verwendet, kann man individueller seinen Sprachassistenten ansprechen. 
- Muss ich Hotwords selbst wählen und damit verbunden dann neu trainieren können?
  - Ja, weil man einen individuellen Sprachassistenten nicht zwangsläufig im Smart Home verwenden will, dann wäre Smart Home möglicherweise das falsche Hotword.
  - Ja, weil man vielleicht wenn man openHAB verwendet openHAB als Hotword verwenden will oder wenn man Home Assistant oder ioBroker, usw. verwendet, diese Eigennamen als Hotword verwenden will.
-  Welche Libraries eignen sich?
-  Welche Frameworks eignen sich?
-  Ist die Sprachqualität von offline Libraries überhaupt zumutbar?
-  Wie füge ich Triggersätze hinzu?
-  Wie füge ich Regeln hinzu? (`WENN-DANN`-Regel sind denke ich für einen Teil der Anwendungsszenarieren definitiv notwendig).
-  Wie füge ich Funktionalitäten hinzu, die durch Trigger ausgeführt werden sollen.
-  Welche Funktionalitäten sollen denn ausgeführt werden können?
  - Zum Beispiel openHAB-Anbindung.
  - Zum Beispiel Wecker/Timer setzen.
  - Zum Beispiel Terminerinnerung setzen und an Termine erinnert werden.
- Müssen Mehrsprachigkeit berücksichtigt werden? (Nein, da es ein Prototyp ist. Man müsste sonst viel zu viel trainieren).
- Brauche ich eine REST API? Ja.
- Brauche ich eine Weboberfläche? Ja.
- Brauche ich eine Datenbank? Ja.
- Wie teste ich das System bzw. welche Tests kann ich machen und planen?
- ...

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

> ⚠️ Einige Werkzeuge dieser Tabelle werden **nicht mehr gepflegt** (u. a. Snowboy, DeepSpeech, Snips NLU, Mycroft Precise; das Unternehmen hinter Coqui TTS existiert nicht mehr). Aktuelle Alternativen und ihren Status listet [Sprachassistenten → Lokale Werkzeuge](https://github.com/Michdo93/Informatik/blob/main/KI%20%26%20Sprachverarbeitung/Sprachassistenten.md#lokale-werkzeuge-und-ihr-status).

Neben der Sprachassistenz benötigt man meiner Meinung nach `Flask` für die `REST API` und für eine `Weboberfläche` des Sprachassistenzen. Wahrscheinlich konfiguriert man über die Weboberfläche Triggersätze. Entsprechend wird auch eine Datenbank (`SQLite` mit `SQLAlchemy`-Anbindung) für Passwörter und Regeln benötigt.

#### Beispielvergleich

Die Auswahl von verschiedenen Libraries kann ja Vor- und Nachteile hervorheben.

##### Hotword Detection

###### ✅ Vergleich: **Lokale Hotword-Erkennungssysteme** (Stand 2026)

| **Library / System** | **Lizenz** | **Status** | **Besonderheiten** |
| --- | --- | --- | --- |
| 🔹 `openWakeWord` | Open Source | ✅ aktiv | Eigene Wakewords trainierbar (auch mit synthetischen Sprachdaten), u. a. in Home Assistant im Einsatz |
| 🔹 `Porcupine` (Picovoice) | proprietär | ✅ aktiv | Sehr effizient; eigene Wakewords über die Picovoice Console; **AccessKey** erforderlich, Nutzungsbedingungen beachten |
| 🔹 `Rustpotter` | Open Source | ✅ aktiv | In openHAB als Keyword Spotter verfügbar; Wakeword aus eigenen Aufnahmen |
| 🔸 `Mycroft Precise` | Open Source | ⚠️ kaum gepflegt | Das Unternehmen hinter Mycroft hat den Betrieb eingestellt; nur Community-Forks |
| 🔸 `Snowboy` | – | ❌ eingestellt (2020) | Kein Training eigener Wakewords mehr möglich |

###### 📌 Fazit / Empfehlung:

| Einsatzziel | Empfehlung |
| --- | --- |
| Lokaler Assistent mit eigenem, frei trainierbarem Wakeword | 🔹 **openWakeWord** |
| Minimaler Ressourcenbedarf, Lizenz akzeptabel | 🔹 **Porcupine** |
| Integration in openHAB | 🔹 **Rustpotter** (oder Porcupine) |
| Veraltete Tools vermeiden | ❌ Kein Snowboy, kein Mycroft Precise für neue Projekte |

---

Dies ist jetzt nur eine kleine Übersicht, was man machen kann im Vergleich. Am besten Quellen ranziehen (z. B. Webseite, Doku, usw.). Man vergleicht am Besten `Python 3 / Python 2` (`Python 2` ist ein Ausschlusskriterium). In der auf NodeJS-basierenden Software `MagicMirror` wird für die Spracherkennung `Snowboy` eingesetzt. Dort ist als `Hotword` bereits `MagicMirror` vortrainiert. Ein neues Hotword kann nicht mehr trainiert werden, weil `Snowboy` deprecated ist und der Service für das Training meines Wissens sogar eingestellt wurde. Selbstverständlich schaut man sich an, ob etwas cloudbasiert funktioniert, lokal oder teilweise cloudbasiert ist. Ebenfalls wichtig ist ja, ob etwas kostenlos ist oder nicht. Hin und wieder gibt es ja auch Abomodelle, dass man vielleicht 1000 Requests kostenlos hat oder Centbeträge für einzelne Requests zahlt, usw. Manche Libraries nutzen `C`, `C++` im Background und es wird nur ein `Python-Wrapper` verwendet. Dies bedeutet, dass das Training performanter und hardwarenäher ist.

##### ASR/TTS

Es gibt extrem viele Libraries für `Text-to-Speech`. Die meisten benötigen eine Cloud. Also wirklich fast alle. Dies ist ein klarer Nachteil. Manche sind immer noch auf Basis von `Python 2`. Einige unterstützen sowohl `Python 2`, als auch `Python 3`. Manche gehen natürlich nur mit `Python 3`. Ergo kann man auch hier alles was `Python 3` nutzt als Vorteil ansehen. Das Ergebnis, bzw. die Sprachqualität von cloudbasierten Libraries ist ganz klar besser. Die lokal laufenden Systeme klingen im Deutschen so, als würde ein Amerikaner Deutsch reden und dann klingt es sehr technisch, mechanisch nach einem Roboter, während cloudbasierte Ergebnisse meist einem menschlichen Sprecher sehr nahe kommt. Je nachdem dauert die Umwandlung lange. Bei cloudbasierten Lösungen gibt es welche, die sehr schnell gute Ergebnisse zurückliefern. Cloudbasiert und damit viel Rechenleistung bedeutet aber tatsächlich nicht zwangsläufig, dass es schnell geht, bis die Datei generiert wird. Auch hier gibt es möglicherweise Latzenzen. Lokal laufende Lösungen haben zwar keine Latzen zwecks Netzwerkauslastung oder schlechter Internetverbindung, sind aber oft auch langsam, weil die Rechenkapazität geringer ist, als in einer Cloud. Man kann vergleichen, welche Datenformate generiert werden (`wav`, `mp3`, usw.). Ich denke für die meisten Szenarien ist es egal, welches Audioformat generiert wird. Wenn man aber bspw. Sonos-Lautsprecher verwenden möchte, dann kann es sein, dass nicht jedes Audioformat unterstütz wird. Vor- und Nachteile in der Bewertung kann es sein, ob Sprecherstimmen (verschiedene Frauen- oder Männerstimmen) konfigurierbar sind, ob Sprachen konfigurierbar sind oder ob `Speech Synthesis Markup Language` (`SSML`) unterstützt wird. `SSML` ist ein ganz klarer Vorteil und gerade im Vergleich zu `Amazon Alexa`, wäre es vielleicht sogar sehr sinnvoll, da `Alexa` dies unterstützt. 

Es können auch weitere Kritieren noch für einen Vergleich herangezogen werden.

Anmerkung: Es gibt verschiedene Libraries, die im Hintergrund aber bspw. denselben Service wie `Google STT` verwenden. Das heißt, viele Libraries stellen zu manch einer Library kaum eine echte Alternative dar.

##### NLP

Selbstverständlich kommt hier auch wieder `Python 2` vs. `Python 3` vor, wenn man die Libraries vergleicht. Da es hier um KI geht, ist auch entscheidend, ob `CPU` oder `GPU` verwendet werden kann. Gemessen an der schwachen Leistung des Endgeräts (z. B. Raspberry Pi), würde es hier auch Sinn ergeben, ob man eine `TPU`-Unterstützung oder andere Coprozessoren verwenden könnte. Es gibt auch KIs, die über eine Cloud trainiert werden. Bei manchen KIs, in einer Cloud lädt man Daten hoch, trainiert sie, lädt Daten herunter und kann die trainierten Daten zu seiner Anwendung hinzufügen. Dies wäre dann zumindest bedingt offline und lokalfähig, weil man nur beim Training eine Internetverbindung zwingend benötigt hat. Es gibt KIs, die sind leichtgewichtig und besonders gut für `Raspberry Pi's` geeignet.

##### Antwortgenerierung

Ich denke dies kann man mehrschichtig aufbauen. Als erstes nutzt man z. B. die regelbasierten Antworten, wenn dort keine Regel auftaucht können Websuche oder KIs angebunden sein, usw. Hier kann man auch Vor- und Nachteile diskutieren. Eine eigene KI zu trainieren edeutet, dass man sehr vielseitig trainieren müsste und immens viele Daten bräuchte. Ein `GPT` (`Generative Pre-trained Transformer`) bedeutet ja in der Regel immer eine Cloud-Lösung (es gibt auch Docker-Container, die man lokal laufen lassen und trainieren kann).

##### Speech-to-Text

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

Es wird noch eine genauere Planung benötigt, wie ein solches System funktonieren soll.

---

### 🧱 **5. Architekturübersicht (Ablauf)**

```plaintext
[Mikrofon] → [Hotword Detection] → [Spracherkennung (ASR)] → [Intent-Erkennung (NLU)]
            → [Antwort (Aktion/Dialog)] → [Text-To-Speech] → [Lautsprecher]
```

Was hier in der Architektur nicht drinnen ist, ist was mit den Intents passiert. Mit diesen kann ich ja zum Beispiel openHAB steuern oder Zustände aus openHAB abfragen. Ich könnte ja auch andere Systeme anbinden. Außerdem muss ja gar keine Antwort verbal erfolgen. Bei einer `WENN-DANN`-Regel hätte ich hier, dass auf eine spezielle Spracheingabe eine spezielle Sprachausgabe erfolgen würde. Ich kann ja auch etwas anderes machen und erhalte dann eine Sprachausgabe oder ich habe eine Spracheingabe ohne Sprachausgabe und mache damit irgendetwas völlig anderes (z. B. Geräte bedienen).

Es fehlt zum Beispiel, ob es eine Benutzeroberfläche gibt, mit der man `WENN-DANN`-Regel konfiguriert. Es fehlt eine Architekturbetrachtung, ob Datenbanken oder andere Systeme angebunden sind. Es fehlt eine Architekturbetrachtung, wie ein KI-Model angebunden ist. Wie wird was überhaupt trainiert?

Dies bedarf noch einer genaueren Planung.

#### Beispiel-Stack (lokal lauffähig):

* **Hotword**: Porcupine (Wakeword: „Hey Kiro“)
* **ASR**: Vosk (lokal) oder Whisper
* **NLU**: eigene Regeln oder Rasa (mit YAML-Intents)
* **TTS**: Piper (oder Coqui TTS)
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
2. Hotword: `openWakeWord` oder `Porcupine` (AccessKey nötig)
3. STT: `Whisper` oder `Vosk`
4. NLU: `Rasa` (mit einfachen Intents)
5. TTS: `Piper` (lokal) oder ein Cloud-Dienst zum Vergleich
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

**Fuzzy Matching** (unscharfe Suche) vergleicht Zeichenketten über eine **Editierdistanz** – die Anzahl der Einfügungen, Löschungen, Ersetzungen (und ggf. Vertauschungen), die eine Zeichenkette in die andere überführen. Bekannte Verfahren sind Levenshtein, Damerau-Levenshtein, Hamming und Jaro-Winkler. Ausführlich mit Beispielen und Python-Code: [Fuzzy Matching](https://github.com/Michdo93/Informatik/blob/main/KI%20%26%20Sprachverarbeitung/Fuzzy%20Matching.md).

#### 🧠 **Wird Fuzzy Matching für einen Sprachassistenten benötigt?**

##### ✅ **Kurzantwort:**

**Ja, in bestimmten Komponenten eines Sprachassistenten kann Fuzzy Matching nützlich sein – aber es ist nicht zwingend erforderlich** und wird **nicht immer eingesetzt**, insbesondere dann nicht, wenn du moderne NLU- oder KI-Modelle verwendest.

---

#### 📌 **Wann ist Fuzzy Matching sinnvoll?**

Fuzzy Matching kommt dann zum Einsatz, wenn:

* **Benutzereingaben ungenau, variabel oder fehlerhaft** sind (z. B. Tippfehler, unterschiedliche Formulierungen).
* **Keine trainierten oder semantischen Modelle verfügbar** sind, etwa bei regelbasierten Assistenten.
* Du **einfach strukturierte Sprachbefehle mit statischen Antworten** hast.

##### Typische Einsatzszenarien:

| Anwendung                                                   | Fuzzy Matching notwendig? | Alternative bei moderner Architektur          |
| ----------------------------------------------------------- | ------------------------- | --------------------------------------------- |
| Menüsysteme / Chatbots mit festen Befehlen                  | ✅ Ja                      | Regelbasierte Intenterkennung                 |
| Sprachassistent mit NLU-Modell (z. B. Rasa)                 | ❌ Eher nein               | Verwendung von ML-basierten Parsers           |
| Wakeword-Erkennung                                          | ❌ Nein                    | Hier nutzt man z. B. CNNs, kein Text-Matching |
| Dateinamen oder Orte erkennen („Spiel *Bohemian Rhapsody*“) | ✅ Ja (optional)           | Levenshtein, Jaro-Winkler, etc.               |

---

#### 🔄 **Beispiel: Fuzzy Matching vs. NLU**

##### Ohne NLU:

```python
if "spiel musik" in user_input:
    do_music()
elif "spiel musick" in user_input:  # Tippfehler!
    do_music()  # geht nicht ohne Fuzzy Matching
```

##### Mit Fuzzy Matching:

```python
from rapidfuzz import fuzz      # actively maintained successor of fuzzywuzzy
if fuzz.partial_ratio("spiel musik", user_input) > 80:
    do_music()
```

##### Mit NLU (z. B. Rasa):

```yaml
- intent: play_music
  examples: |
    - spiel musik
    - spiel bitte musik
    - spiele ein lied
    - mach musik an
```

→ NLU erkennt die Absicht, auch wenn der Text variiert oder leicht fehlerhaft ist. Kein Fuzzy Matching nötig.

---

#### 🧩 **Fazit**

| Wenn du ...                            | Dann ...                                      |
| -------------------------------------- | --------------------------------------------- |
| ... nur einfache Textvergleiche machst | Fuzzy Matching kann helfen                    |
| ... moderne NLU-Modelle nutzt          | Fuzzy Matching ist **nicht nötig**            |
| ... Intents regelbasiert zuweist       | Fuzzy Matching kann Fehler abfedern           |
| ... mit Sprache (nicht Text) arbeitest | ASR-Fehler können durch NLU abgefangen werden |

---

#### 💡 Empfehlung für deine Bachelorarbeit:

* **Nutze Fuzzy Matching nur bei Bedarf**, z. B. für regelbasierte Systeme oder spezifische Trigger (z. B. Namen, Ortslisten).
* Bei **modernem Aufbau mit Rasa, transformers oder GPT-basierten Komponenten**: Verzichte darauf und verlasse dich auf semantische Modelle.

Ich denke in einer gut ausgearbeiteten Abschlussarbeit erläutert man diesen Verzicht auch, weil es sehr viel darüber sagt, was in der Theorie ein System alles können muss.

### Intent, Entity, Confidence Score

* **Intent:** die Absicht hinter einer Äußerung (z. B. `licht_schalten`) – eine Klasse aus einer festen Liste.
* **Entity:** Parameter, die aus der Äußerung extrahiert werden (Raum, Gerät, Wert).
* **Confidence:** wie sicher die Zuordnung ist; über **Schwellwerte** entscheidet man zwischen Ausführen, Rückfrage und Fallback.

Ausführlich mit Beispielen: [Intents, Entities & Confidence](https://github.com/Michdo93/Informatik/blob/main/KI%20%26%20Sprachverarbeitung/Intents%2C%20Entities%20%26%20Confidence.md).

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

Anmerkung: Gute Entwickler können auch Mechanismen entwickeln, wie man solche SSML-Tags selbst in Libraries umsetzt, die dies nicht von sich aus unterstützen. Sätze müssen dann dann unter Umständen aufgeteilt werden, mehrere Audiodateien erstellt und am Ende zusammengefügt werden. Dies wäre aber durchaus ein extrem komplexes und durchaus nicht sicheres und instabiles Prozedere.

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
| **Piper**      | C++/Python | ✅  | ⭐⭐⭐⭐☆    | ❌ (nur Text)           | Schnell, läuft auf dem Raspberry Pi, mehrere deutsche Stimmen; derzeit erste Wahl für lokale TTS |
| **Coqui TTS**  | Python  | ✅     | ⭐⭐⭐⭐☆    | Teilweise (SSML-light) | Moderne TTS, sehr anpassbar; Firma eingestellt (2024), Weiterentwicklung als Community-Fork |
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

👉 Qualitativ stark, aber ressourcenhungrig und nur noch als Community-Fork gepflegt; für den Raspberry Pi ist **Piper** meist die bessere Wahl

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
| Schnelle, lokale TTS auf schwacher Hardware | **Piper**                       |
| Moderne TTS, lokale Nutzung, erweiterbar | **Coqui TTS** (Community-Fork)     |
| Einfache Integration, geringe Ressourcen | **pyttsx3** oder **eSpeak NG**     |
| Vollständige SSML-Verarbeitung lokal     | **MaryTTS** oder Coqui (teilweise) |

---

## 📑 **Gliederungsvorschlag für die Bachelorarbeit**

Hier ist ein **Vorschlag für die Gliederung deiner Bachelorarbeit**, passend zum Thema *„Entwicklung eines lokalen Sprachassistenten“*. Die Gliederung ist modular, logisch aufgebaut und erlaubt sowohl technische Tiefe als auch theoretische Reflexion.

### 1. **Einleitung**

* Motivation und Relevanz des Themas
* Zielsetzung der Arbeit
* Abgrenzung und Umfang
* Aufbau der Arbeit

---

### 2. **Theoretische Grundlagen**

* Sprachassistenten: Definition und Bestandteile
* Überblick über aktuelle Systeme (Alexa, Siri, Mycroft, etc.)
  * Vor- und Nachteile (Datenschutz, Cloudanbindung, keine Smart Home Integration wie bspw. openHAB, usw.)
* Komponenten im Detail:

  * Hotword Detection
  * Automatic Speech Recognition (ASR)
    * Erkläre ebenfalls am besten STT 
  * Natural Language Understanding (NLU)
    * Erkläre ebenfalls am besten NLP und warum NLU eine Teilmenge ist. 
  * Text-to-Speech (TTS)
* Datenschutz & Offline-Verarbeitung
* Einordnung in bestehende Arbeiten (State of the Art)

Was nicht schaden kann ist, wenn man hier noch KI-Grundlagen irgendwie erläutert und erklärt. Man hat ja verschiedene Berührungspunkte, weil man ja auch Modelle trainiert. Eventuell muss man ja auch Begriffe wie `Reinforcement Learning` beschreiben. Spracherkennungssysteme neigen meist zu `Übertraining` (`overfitting`), was wäre dies denn überhaupt erst? Da kann man mit theoretische Grundlagen sehr tief ins Thema einsteigen.

---

### 3. **Anforderungsanalyse**

* Funktionale Anforderungen (z. B. Hotword, STT, NLU)
* Nicht-funktionale Anforderungen (z. B. Offline-Betrieb, Modularität)
* Zielplattform (z. B. Raspberry Pi vs. Mini-PC)
* Auswahlkriterien für Frameworks und Komponenten
* Smart Home Integration (z. B. openHAB)

---

### 4. **Konzept und Architektur**

* Architekturentwurf des Sprachassistenten
* Beschreibung der Systemkomponenten

  * Audioaufnahme & Verarbeitung
  * Modulinteraktion
* Kommunikationsschnittstellen
* Datenflussdiagramm

---

### 5. **Implementierung**

* Projektstruktur & Technologie-Stack
* Beschreibung der wichtigsten Module:

  * Hotword-Erkennung (z. B. Porcupine)
  * Spracherkennung (z. B. Whisper/Vosk)
  * Intent-Erkennung (z. B. Rasa/Regex)
  * Textausgabe (z. B. Coqui TTS)
* Erweiterbarkeit & Custom Intents
* Beispielabläufe / Interaktionen

Ich sage hier nur vielleicht. Man sollte sich nicht zu stark an diesen Libraries orientieren.

---

### 6. **Evaluation**

* Testmethodik (z. B. definierte Szenarien, Vergleich mit Cloudlösungen)
* Messkriterien:

  * Erkennungsrate
  * Latenzzeit
  * Ressourcenverbrauch
* Vergleich von Alternativen (z. B. Whisper vs. Vosk)
* Diskussion der Ergebnisse

Eine Machbarkeitsstudie macht man ja so oder so allgemein. In Bezug auf "Machtbarkeit" kann man ja zeigen, ob ein ehemaliges Szenario von Alexa und openHAB sich nun mit dem neu entwickelten System umsetzen lässt. Wenn ja, dann hat man es ja geschafft ein "Konkurrenzprodukt" zu entwickeln, welches cloudunabhängig funktioniert, somit lokal ist, datenschutztechtlich unbedenklich, usw.

---

### 7. **Reflexion und Ausblick**

* Bewertung der Zielerreichung
* Technische und konzeptionelle Herausforderungen
* Verbesserungspotenziale
* Ausblick auf mögliche Erweiterungen (z. B. Dialogmanagement, IoT-Anbindung)

Ich denke eins wird klar sein: Alexa nutzt bspw. verschiedene Skills. Mit diesen Skills lassen sich Apps, Anwendungen, Geräte von sehr vielen verschiedenen Anbietern integrieren. Auch, weil Skills communitybasiert oder durch Firmen/Organistationen entwickelt werden. Heißt viele Bedienmöglichkeiten und Szenarien über viele Skills/Anwendungen sind hier natürlich nicht integrierbar und am Ende nutzbar. Dies nimmt einen Nutzer auch eine gewisse Komfortabilität und einen gewissen Nutzen. Wären aber bspw. manche Skills umsetzbar oder müsste man die komplette Architektur des Prototypen umschreiben? Könnte man z. B. Geräte von Somfy, Philips Hue oder Sonos auch direkt anbinden oder geht dies nur über ein Smart Home System wie openHAB?

---

### 8. **Fazit**

* Zusammenfassung der Arbeit
* Wichtigste Erkenntnisse
* Persönliche Bewertung

---

### 9. **Anhang**

* Codeauszüge
* Konfigurationsdateien
* Screenshots / Ablaufdiagramme
* Testprotokolle

---

### 10. **Literaturverzeichnis**

* Fachliteratur
* Dokumentationen verwendeter Tools
* Onlinequellen mit Zugriffsdatum

---

## Beispielprojekt

Hier ist ein **Beispielprojekt für einen lokalen Sprachassistenten** mit Fokus auf Modularität, Verständlichkeit und Erweiterbarkeit – ideal als Startpunkt für deine Bachelorarbeit.

Anmerkung: Das ist vom zu entwickelten Prototypen noch meilenweit entfernt.

---

### 📁 Projekt-Setup: `voice_assistant/`

```plaintext
voice_assistant/
├── main.py                       # Einstiegspunkt: orchestration der Komponenten
├── config/
│   └── settings.yaml             # Konfiguration (Hotword, STT, NLU, TTS)
├── hotword/
│   └── detector.py               # Hotword-Erkennung (Porcupine / Precise)
├── stt/
│   └── transcriber.py            # Speech-to-Text (Whisper / Vosk)
├── nlu/
│   └── intent_parser.py          # Intent-Erkennung (Rasa / Regex)
├── tts/
│   └── synthesizer.py            # Text-to-Speech (Coqui TTS)
├── core/
│   └── actions.py                # Antwortlogik / Aktionshandler
├── utils/
│   └── audio_utils.py            # Mikrofonaufnahme, Audio-Konvertierung
├── data/
│   └── intents.yaml              # Trainingsdaten für Intents
└── requirements.txt              # Python-Abhängigkeiten
```

---

### 🧠 Komponentenüberblick mit Mini-Codebeispielen

#### 1. `main.py`

```python
from hotword.detector import wait_for_hotword
from stt.transcriber import transcribe_audio
from nlu.intent_parser import parse_intent
from tts.synthesizer import speak
from core.actions import handle_intent

def main():
    print("System bereit. Sag das Hotword...")
    while True:
        wait_for_hotword()
        audio = utils.audio_utils.record_input()
        text = transcribe_audio(audio)
        intent = parse_intent(text)
        response = handle_intent(intent)
        speak(response)

if __name__ == "__main__":
    main()
```

---

#### 2. `hotword/detector.py` (z. B. mit `pvporcupine`)

```python
import os

import pvporcupine
import pyaudio

def wait_for_hotword():
    # current Porcupine versions require a (free) AccessKey from the Picovoice Console
    porcupine = pvporcupine.create(
        access_key=os.environ["PICOVOICE_ACCESS_KEY"],
        keywords=["computer"],
    )
    audio_stream = pyaudio.PyAudio().open(
        rate=porcupine.sample_rate,
        channels=1,
        format=pyaudio.paInt16,
        input=True,
        frames_per_buffer=porcupine.frame_length,
    )
    while True:
        pcm = audio_stream.read(porcupine.frame_length)
        pcm = list(int.from_bytes(pcm[i:i+2], 'little', signed=True) for i in range(0, len(pcm), 2))
        if porcupine.process(pcm) >= 0:
            break
```

---

#### 3. `stt/transcriber.py` (mit `whisper`)

```python
import whisper

model = whisper.load_model("base")

def transcribe_audio(audio_path="input.wav"):
    result = model.transcribe(audio_path)
    return result["text"]
```

---

#### 4. `nlu/intent_parser.py` (Regel- oder Rasa-basiert)

```python
import yaml

with open("data/intents.yaml", "r") as f:
    intent_data = yaml.safe_load(f)

def parse_intent(text):
    text = text.lower()
    for intent, patterns in intent_data.items():
        for pattern in patterns:
            if pattern in text:
                return intent
    return "unknown"
```

Beispiel für `data/intents.yaml`:

```yaml
play_music:
  - spiel musik
  - mach musik an
  - spiele ein lied
get_time:
  - wie spät ist es
  - sag mir die uhrzeit
  - wie viel uhr
```

---

#### 5. `tts/synthesizer.py` (z. B. mit `pyttsx3` oder `coqui-tts`)

```python
import pyttsx3

engine = pyttsx3.init()

def speak(text):
    engine.say(text)
    engine.runAndWait()
```

---

#### 6. `core/actions.py`

```python
from datetime import datetime

def handle_intent(intent):
    if intent == "get_time":
        return f"Es ist {datetime.now().strftime('%H:%M')} Uhr."
    elif intent == "play_music":
        return "Okay, ich spiele Musik ab."
    else:
        return "Das habe ich leider nicht verstanden."
```

---

### 📦 `requirements.txt`

```txt
pvporcupine
pyaudio
whisper
torch
pyttsx3
pyyaml
```

---

### 🛠️ Was du jetzt damit machen kannst

* 🔄 Eigene Intents und Aktionen hinzufügen
* 🔍 Komponenten austauschen (z. B. Whisper → Vosk, pyttsx3 → Coqui TTS)
* 🧪 Performance- oder Vergleichsexperimente implementieren
* 🔐 Datenschutz-Konzept einbauen (lokale vs. Cloud-Verarbeitung vergleichen)

---

Du hast bereits eine beeindruckend detaillierte Planung für deinen lokalen Sprachassistenten erstellt. Dennoch gibt es einige zusätzliche Aspekte, die du in Betracht ziehen könntest, um dein System noch robuster und benutzerfreundlicher zu gestalten:

---

### 🧠 Weitere Überlegungen für die Systemplanung

#### 1. **Benutzererkennung und Personalisierung**

* **Stimmerkennung**: Implementiere eine Funktion zur Identifikation von Nutzern anhand ihrer Stimme, um personalisierte Dienste anzubieten.
* **Profilmanagement**: Ermögliche die Verwaltung mehrerer Benutzerprofile mit individuellen Einstellungen und Präferenzen.

#### 2. **Kontextbewusstsein**

* **Zeit- und Standortkontext**: Berücksichtige Faktoren wie Tageszeit oder den aktuellen Raum, um angemessene Antworten zu liefern.
* **Verlaufsspeicherung**: Speichere vergangene Interaktionen, um Folgefragen besser zu verstehen und zu beantworten.

#### 3. **Multimodale Interaktion**

* **Visuelle Rückmeldungen**: Integriere Displays oder LED-Anzeigen, um Informationen zusätzlich visuell darzustellen.
* **Touch-Interaktion**: Ergänze die Sprachsteuerung durch Touch-Interfaces für eine flexiblere Bedienung.([architektur-online][1])

#### 4. **Fehlertoleranz und Robustheit**

* **Fuzzy Matching**: Implementiere unscharfe Suchalgorithmen, um auch bei ungenauen Eingaben passende Ergebnisse zu liefern.
* **Fallback-Strategien**: Definiere klare Strategien für den Umgang mit nicht erkannten oder missverstandenen Befehlen.([Internet für Architekten][2])

#### 5. **Datenschutz und Sicherheit**

* **Lokale Datenverarbeitung**: Stelle sicher, dass alle Daten lokal verarbeitet werden, um die Privatsphäre der Nutzer zu schützen.
* **Verschlüsselung**: Nutze Verschlüsselungstechniken für gespeicherte Daten und Kommunikationskanäle.

---

### 🧩 Integration in bestehende Systeme

* **Smart-Home-Kompatibilität**: Stelle sicher, dass dein Assistent mit gängigen Smart-Home-Plattformen wie openHAB, kompatibel ist.
* **Offene Schnittstellen**: Biete APIs oder andere Schnittstellen an, um die Integration in andere Systeme zu erleichtern.([DIE WELT][3])

---

### 🛠️ Entwicklung und Wartung

* **Modularer Aufbau**: Strukturiere dein System modular, um einzelne Komponenten leichter aktualisieren oder austauschen zu können.
* **Dokumentation**: Erstelle umfassende Dokumentationen für Entwickler und Endnutzer, um die Bedienung und Weiterentwicklung zu erleichtern.

---

### 📚 Weiterführende Ressourcen

* **Open-Source-Projekte**: Analysiere bestehende Open-Source-Sprachassistenten wie Mycroft oder Rhasspy, um von deren Architektur und Funktionen zu lernen.
* **Fachliteratur**: Ziehe aktuelle Studien und Veröffentlichungen heran, um dein System auf dem neuesten Stand der Technik zu halten.

---

[1]: https://www.architektur-online.com/kolumnen/ki-am-bau-maschinell-planen-und-bauen?utm_source=chatgpt.com "KI am Bau: Maschinell planen und bauen - architektur-online"
[2]: https://internet-fuer-architekten.de/kategorien/fachartikel-und-meldungen/apps-fuer-planer/?utm_source=chatgpt.com "Apps für Planer - Internet für Architekten"
[3]: https://www.welt.de/230560877?utm_source=chatgpt.com "Funkstandards im Smart Home - das Problem mit der Kommunikation"

## Sprachbefehl vs. Konversation

Ein Sprachbefehl ist in der Regel ein Satz und darauf gibt es eine Antwort. Führt man ein Gespräch bzw. in einer Konversation, dann sind die beiden Gesprächspartner mit ihrem Dialog ständig im Wechsel. Mal redet die eine Seite mehrere Sätze, mal die andere. Mal redet eine Seite mal kürzer und die andere auch, mal redet die eine Seite kürzer und die andere Seite länger. Ein Gespräch bzw. eine Konversation ist damit viel vielschichtiger und komplizierter. Ein Sprachbefehl kann man sich somit als eine Ebene vorstellen. Zu einem Satz gibt es eine Antwort. Je nach Satz variiert die Antwort. Selbst wenn die Sprachantworten eine gewisse Varianz (z. B. durch `random`) aufweist, kann man dies exakt einer Ebenen zuordnen. Bei einem Gespräch hingegen entsprecht jeder Dialogwechsel einer neuen Ebene. Da es pro Ebene unter Umständen zu unterschiedlichen, durchaus auch zufälligen randomiserten Antwortmöglichkeiten kommen kann, braucht man auch viele Verzweigungen pro Ebene. Denkbar ist ja auch, dass je nach Verzweigung das Gespräch sich vom Kontext her verändert.

Ist so etwas mit einem Sprachassistenten überhaupt realisierbar? Stören vielleicht längere Antwortzeiten den Gesprächfluss?

Absolut – du beschreibst ein typisches **Command-and-Control-System**, das **eindeutige Sprachbefehle** verarbeitet (z. B. „Licht im Wohnzimmer an“) statt eine fließende Konversation wie mit ChatGPT zu führen.

---

### 🧠 Theoretischer Durchlauf: Sprachbefehl statt Konversation

#### 🎯 Ziel:

Verarbeitung von **einzelnen Sprachbefehlen (z. B. Satz)** → Aktion ausführen → Antwort optional

---

#### 🔄 Systemarchitektur (vereinfachtes Modell)

1. **Mikrofon**
   → Audioaufnahme

2. **Hotword-Erkennung (optional)**
   → z. B. „Computer“ oder „Hey Homey“

3. **ASR (Automatic Speech Recognition)**
   → Umwandlung: „Schalte das Licht im Wohnzimmer an“ → Text

4. **Intent-Erkennung**
   → Mapping des Satzes auf einen definierten Befehl:

   ```json
   {
     "intent": "light_on",
     "location": "wohnzimmer"
   }
   ```

5. **Ausführung**
   → Steuerbefehl an Smart-Home-System senden

6. **(Optional: Rückmeldung)**
   → z. B. „Licht im Wohnzimmer ist jetzt an.“

---

#### ✅ Was wird benötigt?

##### 🗣 Sprachverarbeitung

| Modul            | Tool/Lib                                                        | Lokal möglich? |
| ---------------- | --------------------------------------------------------------- | -------------- |
| ASR              | z. B. [Vosk](https://alphacephei.com/vosk/), Whisper, Coqui STT | ✅              |
| Intent-Erkennung | Rasa NLU, Snips NLU, RegEx-basiert, Duckling                    | ✅              |

---

##### 🧩 Intent-Erkennung: Wie?

Du brauchst eine Methode, die **vordefinierte Sätze** erkennt. Es gibt drei Wege:

1. ✅ **Regel- oder Musterbasiert (z. B. RegEx)**
   Ideal bei wenigen, klaren Befehlen.

   ```python
   re.match(r"schalte das licht im (?P<room>\w+) an", text)
   ```

2. ✅ **Slot-Filling / NLU (z. B. Snips oder Rasa)**
   Nutzt Trainingsdaten, erkennt flexible Variationen:

   * „Mach das Wohnzimmerlicht an“
   * „Bitte Licht einschalten im Wohnzimmer“

3. ⚠️ **LLM / KI-basiert (Whisper → GPT)**
   Overkill für simple Befehle, aber bei hoher Flexibilität denkbar.

---

#### 🎛 Vorteile dieses Ansatzes

* **Robust** bei klaren Befehlen
* **Geringe Rechenlast** – perfekt für Embedded Devices
* **Besser steuerbar** als ein dialogbasiertes System

---

#### 🔒 Datenschutz und Zuverlässigkeit

* Alles kann **lokal** laufen (z. B. Raspberry Pi 5)
* Kein Cloudzugriff notwendig
* Offlinefähig und DSGVO-konform

---

#### 🧠 Fazit

| Aspekt                  | Bewertung                                     |
| ----------------------- | --------------------------------------------- |
| Konzeptionell umsetzbar | ✅ Ja, mit bewährter Technik                   |
| Lokal realisierbar      | ✅ Völlig ohne Cloud möglich                   |
| Komplexität             | ⚖️ Mittel – je nach Flexibilität              |
| Erweiterbarkeit         | ✅ Modular mit Intent-Slots                    |
| Konversationsfähigkeit  | ❌ Nicht erforderlich, bewusst nicht verwendet |

---

### 🧠 Ziel: **Konversationsfähiger Sprachassistent**

Jetzt steigen wir in die **dialogorientierte Sprachinteraktion** ein – also **Konversation statt einzelner Sprachbefehle**. Das ist komplexer, aber auch deutlich flexibler und menschlicher in der Interaktion.

Statt:

> „Schalte das Licht im Wohnzimmer an.“

Sowas wie:

> **„Mir ist zu dunkel.“** → „Möchtest du das Licht im Wohnzimmer einschalten?“ → „Ja, bitte.“

---

#### 🧩 Architektur: Was braucht ein konversationsfähiges System?

Hier ist der **erweiterte Ablauf** im Vergleich zum „Befehl-only“-Modell:

1. 🎤 **Mikrofon**
   → Audioaufnahme

2. 🛑 **Hotword-Erkennung** (wie gehabt)
   → z. B. „Hey Luna“

3. 🗣 **ASR (Speech-to-Text)**
   → z. B. Whisper oder Vosk: Sprache → Text

4. 🤖 **Natural Language Understanding (NLU)**
   → Erkennen von Intents + Entitäten + Stimmungen
   → z. B. „Mir ist zu dunkel“ → Intent: `discomfort_light`

5. 💬 **Dialog-Manager (State & Kontext)**
   → Verwaltung des Gesprächsverlaufs und Kontexts
   → z. B.: „Dunkel“ erkannt → nächste Aktion: „Licht anbieten“
   → Kontext bleibt bestehen: Wenn später „Ja“ kommt, wird das Licht eingeschaltet

6. 🔄 **Antwortgenerierung (NLG)**
   → z. B. Templating („Möchtest du das Licht einschalten?“)
   → oder generativ (Chatbot-Style)

7. 🗣 **TTS (Text-to-Speech)**
   → Lokale Stimme antwortet

8. 🔧 **Aktion ausführen**
   → API-Aufruf, z. B. Lichtsteuerung

---

#### 🛠️ Was brauchst du zusätzlich (gegenüber Befehlssystem)?

| Funktion                               | Beschreibung                                          | Beispiele / Tools                                  |
| -------------------------------------- | ----------------------------------------------------- | -------------------------------------------------- |
| **Dialogmanager**                      | Merkt sich, was gesagt wurde & wie reagiert wurde     | Rasa Core, OpenDialog, eigene FSM                  |
| **Kontextbezug**                       | Z. B. „Mach es heller“ → Welcher Raum? Welches Gerät? | Kontexttracking via State                          |
| **Entitäten-Tracking**                 | Räume, Geräte, Uhrzeiten, ...                         | Rasa NLU, Snips, Duckling                          |
| **Bestätigungen / Rückfragen**         | Wenn Absicht nicht eindeutig ist                      | „Meinst du das Wohnzimmerlicht?“                   |
| **Stimmungs- und Intentionserkennung** | Für natürlichere Antworten                            | „Du klingst gestresst – soll ich Musik abspielen?“ |

---

#### 🔁 Beispiel-Dialog (mit Kontexthandling)

```text
Nutzer: Mir ist kalt.
Assistent: Möchtest du die Heizung im Wohnzimmer erhöhen?
Nutzer: Ja bitte.
→ Aktion: Heizung +2°C im Wohnzimmer
```

Oder komplexer:

```text
Nutzer: Ich hab Hunger.
Assistent: Soll ich dir ein Rezept vorschlagen oder etwas bestellen?
Nutzer: Vorschlag klingt gut.
Assistent: Wie wär’s mit Pasta mit Tomaten?
```

---

#### 🧠 Wie speichert man Kontext?

Je nach Framework gibt’s diese Möglichkeiten:

* **Finite-State-Machine**: Einfach, kontrolliert → gut für definierte Abläufe
* **Frame-based Systems** (Slots): Ähnlich wie Formulare
* **Generativ mit LLMs**: GPT oder ähnliche Modelle mit Kontextprompting (aufwendig)

---

#### ✅ Lokale Open-Source-Tools für Konversation

| Komponente        | Empfohlene Tools                |
| ----------------- | ------------------------------- |
| NLU + Dialog      | Rasa (NLU + Core), Snips NLU    |
| ASR               | Whisper, Vosk                   |
| TTS               | Coqui TTS, MaryTTS              |
| Intent-Management | YAML/JSON mit FSM / Rasa Domain |
| Memory/State      | Redis, SQLite, oder im RAM      |

---

#### ⚖️ Vergleich: Befehl vs. Konversation

| Aspekt             | Sprachbefehl-System  | Konversationssystem                    |
| ------------------ | -------------------- | -------------------------------------- |
| Einfachheit        | ✅ Sehr einfach       | ❌ Komplexer Dialogmanager nötig        |
| Natürlichkeit      | ❌ Eingeschränkt      | ✅ Sehr hoch                            |
| Fehleranfälligkeit | ✅ Kaum Kontextfehler | ❌ Kontext muss korrekt getrackt werden |
| Ressourcenbedarf   | ✅ Gering             | ❌ Höher (mehr RAM/CPU)                 |
| Lokale Umsetzung   | ✅ Einfach            | ✅ Möglich, aber aufwendiger            |

---

#### 📌 Fazit

Ein konversationsfähiger Sprachassistent ist:

* **Deutlich komplexer** (du brauchst Kontext, Rückfragen, State)
* **Realistisch umsetzbar lokal**, mit Tools wie **Rasa**, **Coqui**, **Whisper**
* Ideal für smarte Umgebungen, wo auch **implizite Wünsche** geäußert werden („Mir ist kalt“, „Ich bin müde“, ...)

---

#### Beispiel: Dialog-Engine mit Zustandsübergängen (FSM)

Diese Beispiel-Engine in Python simuliert einen **zustandsbasierten Sprachdialog** (Finite-State-Machine):

### 📁 `dialog_engine.py`

```python
class DialogEngine:
    def __init__(self):
        self.state = "START"

    def handle_input(self, user_input):
        if self.state == "START":
            if "hallo" in user_input.lower():
                self.state = "GREETING"
                return "Hallo! Wie kann ich dir helfen?"
            return "Bitte beginne mit einer Begrüßung."

        elif self.state == "GREETING":
            if "licht" in user_input.lower():
                self.state = "LIGHT_CONFIRM"
                return "Möchtest du das Licht einschalten?"
            return "Das habe ich nicht verstanden. Möchtest du das Licht einschalten?"

        elif self.state == "LIGHT_CONFIRM":
            if user_input.lower() in ["ja", "ja bitte"]:
                self.state = "END"
                return "Licht wird eingeschaltet. Noch etwas?"
            else:
                self.state = "END"
                return "Okay, kein Licht. Noch etwas?"

        elif self.state == "END":
            if "nein" in user_input.lower():
                self.state = "START"
                return "Okay, bis zum nächsten Mal!"
            else:
                self.state = "GREETING"
                return "Womit kann ich dir sonst helfen?"

# --- Demo-Nutzung ---
if __name__ == "__main__":
    engine = DialogEngine()
    while True:
        user = input("Du: ")
        reply = engine.handle_input(user)
        print("Assistent:", reply)
```

Hier wird ziemlich deutlich, dass die Varianz nicht groß ist. Je größer die Varianz, desto mehr if-Verzweigungen bzw. Verschachtelungen benötigt man. Je mehr Ebenen das Gespräch hat, desto komplizierter wird das Ganze und man muss deutlich mehr Sonderfälle berücksichtigen.

---

#### Beispiel: Minimaler Konversationsbaum in YAML (z. B. für Rasa)

Das folgende YAML definiert einen **einfachen Dialog** in einer Struktur, wie Rasa oder ein FSM-Parser sie nutzen könnte:

##### 📄 `dialogs.yaml`

```yaml
version: "1.0"
dialogs:
  - state: start
    input: "hallo"
    next: greeting
    response: "Hallo! Wie kann ich dir helfen?"

  - state: greeting
    input: "ich will das licht einschalten"
    next: confirm_light
    response: "Möchtest du das Licht jetzt einschalten?"

  - state: confirm_light
    input: "ja"
    next: light_on
    response: "Okay, das Licht wird eingeschaltet."

  - state: confirm_light
    input: "nein"
    next: end
    response: "Alles klar, kein Licht."

  - state: light_on
    input: "*"
    next: end
    response: "Kann ich sonst noch etwas für dich tun?"

  - state: end
    input: "nein"
    next: start
    response: "Bis später!"
```

Diese Datei kannst du mit einem simplen YAML-Interpreter oder selbstgeschriebenem FSM-Parser nutzen. Die `*`-Eingabe dient als Platzhalter für „alles andere“.

---

Hier ist ein **Minimaler Konversationsbaum** als **einfache textbasierte Baumstruktur** (Diagramm-artig). Dieser stellt denselben Dialogfluss wie im YAML-Beispiel dar – aber in **grafischer Baumform**, wie du ihn dir konzeptionell vorstellen würdest.

##### Beispiel: Minimaler Konversationsbaum – Baumstruktur (Dialoglogik)

```
START
 └── "hallo"
     ↓
GREETING
 └── "ich will das licht einschalten"
     ↓
CONFIRM_LIGHT
 ├── "ja"
 │    ↓
 │  LIGHT_ON
 │    └── "*"
 │         ↓
 │       END
 │         └── "nein" → START
 │
 └── "nein"
      ↓
    END
      └── "nein" → START
```

---

##### 📖 Erklärung der Zustände:

* **START**
  → Einstiegspunkt, wartet auf Begrüßung („hallo“)

* **GREETING**
  → Fragt: Was möchtest du tun? Erkennt: „Ich will das Licht einschalten“

* **CONFIRM\_LIGHT**
  → Fragt zur Sicherheit: „Möchtest du das Licht jetzt einschalten?“

* **LIGHT\_ON**
  → Antwort: „Okay, Licht wird eingeschaltet.“
  → Danach optionale Rückfrage: „Noch etwas?“

* **END**
  → Beendet Gespräch, Möglichkeit zum Neustart (z. B. bei „nein“)

---

##### 🧩 Erweiterungsmöglichkeiten:

* Füge z. B. weitere Intents wie „Temperatur ändern“, „Musik spielen“, etc. ein
* Baue Zustände für Unsicherheiten oder Wiederholungen ein
* Integriere Slot-Filling (z. B. Raum, Helligkeit, Uhrzeit)

---

## 🧠 Vergleich: Sprachbefehl vs. Konversation bei Sprachassistenten

Es gibt Sprachassistenten, die **über reine Sprachbefehle hinausgehen** und tatsächlich **mehrstufige Konversationen** ermöglichen – ähnlich wie Chatbots. Allerdings ist das **eher die Ausnahme als die Regel**, besonders bei **lokalen oder Open-Source-Systemen**.

Hier ist ein **vergleichender Überblick**, der zeigt, welche Sprachassistenten eher **befehlsgesteuert** agieren und welche eine **dialogorientierte Interaktion** (Konversation) ermöglichen:

| Sprachassistent                   | Typ                  | Befehlsgesteuert | Konversationsfähig | Konversationsspeicher   | Bemerkungen                                             |
| --------------------------------- | -------------------- | ---------------- | ------------------ | ----------------------- | ------------------------------------------------------- |
| **Amazon Alexa**                  | Cloud                | ✅ Ja             | ⚠️ Teilweise       | 🔁 Kontext begrenzt     | Nur mit aktivem Skill/Dialogmodell konversationsfähig   |
| **Google Assistant**              | Cloud                | ✅ Ja             | ⚠️ Teilweise       | 🔁 Nur kurzzeitig       | Kontext wird oft nach Antwort gelöscht                  |
| **Apple Siri**                    | Cloud                | ✅ Ja             | ❌ Nein             | ❌ Kein Kontext          | Sehr befehlsfokussiert                                  |
| **Mycroft**                       | Open Source (lokal)  | ✅ Ja             | ⚠️ Möglich         | ⚠️ Entwicklerabhängig   | FSMs und dialog module möglich                          |
| **Rhasspy**                       | Open Source (lokal)  | ✅ Ja             | ❌ Nein             | ❌ Kein interner Kontext | Fokus auf Einmal-Befehle                                |
| **Jarvis.ai / ChatGPT + TTS/STT** | Hybrid (lokal/cloud) | ✅ Ja             | ✅ Ja               | ✅ Vollständig           | Nutzt z. B. Whisper + ChatGPT für Konversationsverläufe |
| **Home Assistant Assist**         | Lokal/Cloud          | ✅ Ja             | ❌ (noch nicht)     | ❌ Kein Dialogspeicher   | Fokus auf Smarthome-Befehle                             |

---

### 🔍 Interpretation:

* Die meisten Sprachassistenten (vor allem kommerzielle) sind auf **Einzelbefehle** optimiert.
* **Echte Konversationen** wie bei einem Chatbot erfordern:

  * **Kontextspeicherung** über mehrere Turns hinweg
  * **Dialog-Management** (z. B. mit Zustandsautomaten oder Intenterkennung + Slot-Filling)
  * ggf. **LLM-Unterstützung** (Large Language Models)
* Systeme wie **ChatGPT + TTS/STT-Wrapper** oder eigene FSM-basierte Dialogsysteme bieten die **flexibelste Form echter Konversation**.

---

### 🧩 Fazit:

> Sprachassistenten wie **Siri, Alexa, Rhasspy oder Mycroft** sind primär auf **sprachgesteuerte Einzelbefehle** ausgelegt. Für **kontextbasierte Gespräche** brauchst du entweder ein System wie **ChatGPT**, ein eigenes LLM-Backend – oder du erweiterst Open-Source-Lösungen wie **Mycroft** mit Dialogmanagementlogik.

## Bonustipp

### ASR/STT

Um verbal ASR oder STT richtig zu beschreiben, empfiehlt es sich Worte wie "dekodieren" und "konvertieren" zu verwenden, weil es somit einfach technischer beschrieben ist:

> Automatic Speech Recognition (ASR) bzw. Speech-to-Text (STT) ist eine Kombination von Prozessen und Software, welches menschliche Sprache dekodiert und es in einen digitalen Text konvertiert.

### TTS

Um Verbal TTS zu beschreiben empfiehlt es sich ebenfalls Worte wie "kodieren" und "konvertieren" zu verwenden:

> Text-to-Speech ist eine Kombination von Prozessen und Software, die einen digitalen Text in synthetische menschliche Sprache kodiert und konvertiert.

### STTS und TTSS

Bei cloudbasierten Services spricht man auch gerne von `STTS` und `TTSS`, also von `Speech-to-Text-Services` und `Text-to-Speech-Services`.

---

Ein koversationsfähiger Sprachassistent würde wahrscheinlich den Rahmen einer Abschlussarbeit ordentlich spregen. Könnte aber in Anschluss zu einer guten vorangegangenen Abschlussarbeit ein sehr spannendes Thema für eine Masterarbeit werden.

---

## Vorarbeiten im Labor

Im Labor gibt es bereits einen Asterisk-Server mit Piper-TTS, eine Android-App zur Spracherkennung für openHAB und zwei Forks lokaler Sprachassistenten – siehe [Backlog → Lokale Sprachassistenten](https://github.com/Michdo93/Smart-Home-Labor-Backlog/blob/main/Experimente/Lokale-Sprachassistenten.md).

---

## Grundlagen im Kompendium Informatik

* [Sprachassistenten](https://github.com/Michdo93/Informatik/blob/main/KI%20%26%20Sprachverarbeitung/Sprachassistenten.md) – Verarbeitungskette, Befehl vs. Konversation, Wartungsstatus lokaler Werkzeuge, openHAB-Voice, SSML, Messgrößen (WER, Latenz)
* [Intents, Entities & Confidence](https://github.com/Michdo93/Informatik/blob/main/KI%20%26%20Sprachverarbeitung/Intents%2C%20Entities%20%26%20Confidence.md)
* [Fuzzy Matching](https://github.com/Michdo93/Informatik/blob/main/KI%20%26%20Sprachverarbeitung/Fuzzy%20Matching.md)
* [Machine-Learning-Grundlagen](https://github.com/Michdo93/Informatik/blob/main/KI%20%26%20Sprachverarbeitung/Machine-Learning-Grundlagen.md)
* [State-Pattern](https://github.com/Michdo93/Informatik/blob/main/Design%20Pattern/Verhaltensmuster/State.md) – Dialog als Zustandsautomat
* [systemd-Services](https://github.com/Michdo93/Informatik/blob/main/Linux%20%26%20Werkzeuge/systemd-Services.md) – den Assistenten als Dienst betreiben
