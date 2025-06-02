# Bachelorarbeit: Entwicklung eines modularen, datenschutzfreundlichen und lokalen Sprachassistenten

## Mögliche Titelvorschläge

- "Entwicklung eines datenschutzfreundlichen Sprachassistenten auf Basis freier Software"
- "Vergleich von ASR-Engines für Offline-Sprachassistenten"
- "Modularer Sprachassistent für den lokalen Einsatz: Architektur und Prototyp"
- "Sprachverarbeitung ohne Cloud: Konzeption und Evaluation eines lokalen Sprachassistenten"
- "Custom Wakeword Detection in Embedded Systems: Architektur, Training und Einsatz"
- "Open Source Sprachassistent: Ein systemischer Vergleich freier Komponenten"
- ...

## Hintergrund

Sprachassistenten wie Alexa, Siri oder Google Assistant haben sich im Alltag vieler Menschen etabliert. Allerdings sind diese Systeme oft cloudgebunden, datenschutzrechtlich bedenklich und schwer individualisierbar. Dies eröffnet spannende Möglichkeiten für die Entwicklung eines eigenen, lokal laufenden Sprachassistenten mit freier Wahl der Komponenten.

### Problemstellung

- Cloudverbindungen können auch öfter mal gestört oder abgebrochen sein.
  - Ein lokaler Sprachassistent würde zumindest lokal funktionierende Befehle bearbeiten und lokal verfügbare Informationen zur Verfügung stellen können.
- Datenschutzrechtlich hört ein Sprachassistent dauerhaft mit und wartet, bis ein sog. Hotword (auch trigger word oder wake word) erkannt wird.
  - Ist wirklich garantiert, dass nur Daten aufgezeichnet und an eine Cloud gesendet werden, wenn ein Hotword verwendet wurde oder lauscht das System dauerhaft mit?
  - Anmerkung: Deutsche Begriffe sind Aktivierungswort, Aufwachwort, Aufwachbefel, Triggerwort, usw.
- Sprachassistenten sind oft nur beschränkt individualisierbar, Anwendungen werden eingestellt oder eingeschränkt.
  - Google Nest ermöglicht es oft bspw. Smart Home Geräte zwar anzubinden, diese können darüber aber nicht gesteuert werden, sondern meist nur Zustände abgefragt werden.
  - Amazon hat bspw. ihre Policy geändert und seitdem wird das openHAB Skill aus Sicherheitsgründen nicht mehr unterstützt, damit wäre ein Smart Home, welches über openHAB gesteuert wurde nicht mehr bedienbar.

## Ziel der Arbeit

Ziel der Arbeit ist die Entwicklung und prototypische Umsetzung eines lokalen Sprachassistenzsystems auf Basis freier Software und Hardware. Der Fokus liegt auf Modularität, Erweiterbarkeit und Datenschutzfreundlichkeit. Es sollen verschiedene Komponenten (Spracherkennung, Hotword-Erkennung, Intent-Parsing, Text-to-Speech) integriert und ggf. evaluiert werden.

## Teilziele

- Aufbau einer modularen Architektur für Sprachassistenz
- Integration von Hotword-Detection, ASR (Automatic Speech Recognition, hier: Speech-to-Text), NLU (Natural Language Understanding, hier: Intent-Erkennung ist eine Teilmenge des Natural Language Processings), TTS (Text-to-Speech)
- (Optional) Vergleich verschiedener frei verfügbarer ASR/NLU/TTS-Frameworks hinsichtlich:
  - Latenz
  - Genauigkeit
  - Ressourcenverbrauch
- Anpassbarkeit durch neue Triggerwörter und Intents
- Betrachtung datenschutzrechtlicher Implikationen (lokal vs. Cloud)
- (Optional) Einbindung einfacher Aktionen (z. B. Musik steuern, Termine vorlesen, lokale Geräte steuern)

## Mögliche Forschungsschwerpunkte

- Vergleich von Offline-STT-Engines: `Whisper`, `Vosk`, `DeepSpeech`
- Evaluation von NLU-Systemen: `Rasa`, `Snips`, Transformer-basierte Lösungen
- Training eigener Hotword-Modelle mit `Porcupine`, `Mycroft Precise`
- Einfluss der Hardware auf Performance (z. B. Raspberry Pi vs. Jetson Nano)
- Kombination regelbasierter und ML-basierter Dialogmodelle
- Lokaler Datenschutz vs. Cloud-Performance

## Eingesetzte Technologien (Auswahl)

- Programmiersprache: **Python**
- Hotword-Erkennung: `Porcupine`, `Mycroft Precise`
- Speech-to-Text: `Whisper`, `Vosk`, `DeepSpeech`
- NLU: `Rasa`, `Snips`, eigene ML-Modelle mit `transformers`
- TTS: `Coqui TTS`, `eSpeak`, `Festival`
- Plattform: **Raspberry Pi 5**, ggf. Mini-PC

## Voraussetzungen

- Interesse an Sprachverarbeitung, KI und Embedded Systems
- Gute Kenntnisse in Python
- Grundverständnis maschinellen Lernens und Linux-Systemen
- Motivation zur Arbeit mit Open-Source-Technologien

## Betreuung & Rahmen

- Diese Arbeit eignet sich besonders für Studierende der Informatik, Medieninformatik oder verwandter Studiengänge.
- Umfang: Bachelorarbeit (ca. 16 Wochen Vollzeit)
- Bei Interesse ist eine Weiterentwicklung zur Masterarbeit denkbar.
