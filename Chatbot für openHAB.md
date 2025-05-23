# Konzeption und Implementierung eines webbasierten Chatbots mit trainierbarem KI-Modul zur Interaktion mit openHAB

---

## 🎓 **Titelvorschläge für die Bachelorthesis**

1. **Entwicklung eines KI-gestützten Chatbots für Smart-Home-Steuerung mit Flask und selbsttrainierbarem Modell**
2. **Konzeption und Implementierung eines webbasierten Chatbots mit trainierbarem KI-Modul zur Interaktion mit openHAB**
3. **Webbasierte Benutzer- und Adminoberfläche für ein dynamisch lernfähiges Chatbot-System im Smart-Home-Kontext**
4. **Design und Umsetzung eines selbstlernenden Chatbots mit dynamischem Training über eine Admin-Weboberfläche**
5. **Architektur eines benutzerzentrierten KI-Systems zur Chat-basierten Steuerung von Smart Homes**
6. **Aufbau eines anpassbaren, KI-basierten Dialogsystems mit Flask und dynamischem Modelltraining für Smart Homes**
7. **Entwicklung eines KI-gestützten Chatbots mit dynamischem Training und Web-Oberfläche zur Smart-Home-Interaktion**

---

## 📣 **Ausschreibungstext für die Bachelorthesis**

### **Beschreibung:**

Im Rahmen dieser Bachelorarbeit soll ein interaktives Chatbot-System für die Steuerung von Smart-Home-Geräten über **openHAB** entwickelt werden. Die Besonderheit: Das System soll nicht auf statisch definierten Regeln oder externen KI-Diensten (wie ChatGPT) basieren, sondern eine **eigenständig trainierbare KI** verwenden, die über eine **Weboberfläche von Administratoren gepflegt und erweitert werden kann**.

---

### **Ziele der Arbeit:**

* Entwurf und Umsetzung einer **Flask-basierten Webanwendung** mit zwei Benutzerrollen: Nutzer (Chat) und Administrator (Training).
* Integration einer **KI-Komponente**, die auf Basis von Benutzereingaben trainiert werden kann.
* Implementierung einer **Datenbank** zur Speicherung von Trainingsdaten, Chatverläufen und Benutzerinformationen.
* Aufbau einer **Admin-Oberfläche**, über die Intents, Entitäten und Beispiele eingegeben werden können.
* Dynamische Auslösung des Trainingsprozesses per Button in der Weboberfläche.
* Anbindung an openHAB über die REST API zur Ausführung realer Aktionen im Smart-Home-System.

---

### **Technologien & Tools:**

* **Programmiersprache**: Python
* **Frameworks**: Flask (Web), optional TensorFlow/Rasa/spaCy/Hugging Face (für die KI)
* **Datenbank**: SQLite oder MySQL
* **KI-Training**: Dynamisch über das Admin-Interface
* **API-Kommunikation**: openHAB REST API

---

### **Voraussetzungen:**

* Gute Kenntnisse in Python
* Erste Erfahrungen mit Webentwicklung (Flask, HTML/JS)
* Interesse an künstlicher Intelligenz und maschinellem Lernen
* Selbstständiges Arbeiten und kreative Problemlösung

---

### **Ergebnis der Arbeit:**

Ein **vollständig funktionierender Prototyp** eines KI-gestützten, webbasierten Chatbot-Systems zur Steuerung eines Smart Homes. Die Arbeit kann später erweitert und als Open-Source-Projekt veröffentlicht werden.

---

## ✅ Vorteile der Trennung von Benutzer-Chat, Admin-Panel und zentraler API

### 1. **Modularität**

* **Frontend 1: Benutzer-Chat**

  * Eigene, schmale Oberfläche nur für Endnutzer.
* **Frontend 2: Admin-Panel**

  * Separat, geschützt, mit anderen Funktionen (z. B. Datenpflege, Training).

→ Beide greifen auf dieselbe **zentralisierte API** zu.

---

### 2. **Zentrale Kommunikationsschnittstelle (API-Gateway)**

Ein zentrales Backend (Flask API) dient als Schnittstelle für alle Clients:

```
Clients:
- Benutzer-Chat (Web)
- Admin-Panel (Web)
- Sprachassistent (z. B. Alexa, Raspberry Pi, Android-App)
- Mobile App
→ alle über eine einheitliche REST-API
```

Diese API regelt:

* Authentifizierung (JWT, OAuth, etc.)
* Datenverwaltung (Trainingsdaten, Logs)
* Chat-Dispatch: Anfrage → Modell → Antwort
* Trigger für Modelltraining
* Geräte-Interaktion (über openHAB)

---

### 3. **Sprachassistent einfach integrierbar**

Dies wäre in einem weiteren Projekt (weitere Thesis umsetzbar).

Es gilt:

* Chat-Eingabe → ersetzt durch **Speech-to-Text**
* Chat-Ausgabe → ersetzt durch **Text-to-Speech**

Dazu braucht der Client (z. B. in Python oder als App):

* Mikrofon → STT (z. B. Google Speech, Whisper)
* Text senden → REST-API
* Antwort empfangen → TTS (z. B. pyttsx3, gTTS, ElevenLabs) → Lautsprecher

---

## 🧱 Empfehlung: Systemaufbau (logisch)

![picture alt](https://raw.githubusercontent.com/Michdo93/SmartHome-Ideen/refs/heads/main/screenshots/chatbot.png)

### Idee

Aufbau eines **KI-gestützten Chatbots mit einer Web-Oberfläche** zur Verwaltung und **Training** in einem System wie **Flask**. Der Workflow würde sicherstellen, dass **Admins** das Modell trainieren können, ohne tief in den Code einzugreifen, sondern alles über eine Benutzeroberfläche (Admin-Dashboard) steuern. Auch das dynamische **Trainieren des Modells** basierend auf neuen Benutzereingaben ist eine spannende Herausforderung.

Warum dieses Vorgehen?

Ein Smart-Home-System wie openHAB wird immer wieder erweitert. Es kommen neue Geräte und somit Things hinzu, zudem es entsprechend immer wieder neuere Items gibt. Eventuell werden alte Geräte ausgetauscht und gar entfernt, was dazu führt, dass man die dazugehörigen Things und Items ebenfalls löscht. Für den Chatbot bedeutet dies, dass er natürlich immer nur auf den aktuellen Stand des Smart-Homes reagieren soll. Ein Chatbot, welches Informationen von alten Geräten ausgeben möchte, würde zum einen nichts bringen und zum anderen vermutlich gar keien Informationen zurückliefern können. Ebenfalls wäre der Versuch Geräte zu steuern, die gar nicht vorhanden sind fatal. Ein neues Gerät, welches man steuern möchte, könnte dann ebenfalls nicht über den Chatbot gesteuert werden.

Neben dem technisch-funktionalen Vorgehen stellt man im Laufe der Verwendung vielleicht auch fest, dass man Optimierungen haben möchte. Man kann das Modell ja nicht nur neu trainieren, weil Geräte hinzugefügt oder gelöscht wurden, sondern man möchte vielleicht andere Eingaben nutzen, mögliche Synonyme usw., weil man ja viele Sätze nie gleich formuliert. Mit unterschiedlichen Sätzen möchte man aber am Ende unter Umstände das gleiche Ergebnis erzielen. Wenn man dies entsprechend konfigurieren und trainieren kann, hilft dies, die Anwendung benutzerfreundlicher und individueller für den/die Anwender zu gestalten.

---

### **Projektplanung und Struktur**

#### 1. **Systemüberblick**

* **Frontend (Flask + HTML/JS)**:

  * Web-Oberfläche mit einem Login-System.
  * **Benutzer-Chat**: Normale Benutzer können mit dem Bot interagieren.
  * **Admin-Panel**: Admins können Daten für das Training hinzufügen und das Modell neu trainieren.
  * **Datenbank (z. B. SQLite/MySQL)**: Speichern der Trainingsdaten, Chatverläufe, Synonym-Liste, Benutzeraccounts und Modelle.

* **Backend (Flask + Python)**:

  * Flask-Anwendung für die Kommunikation mit dem Frontend und die Verarbeitung der Anfragen.
  * **Modelltraining**: Das Backend wird in der Lage sein, das Modell basierend auf den eingegebenen Daten zu trainieren.
  * **Modell-API**: Ein API-Endpunkt, der auf Chat-Anfragen reagiert, basierend auf einem trainierten Modell.

* **Modell-Training (TensorFlow / Rasa / spaCy / Hugging Face)**:

  * Das Modell wird auf Basis der hinzugefügten Trainingsdaten **dynamisch** trainiert.

#### 2. **Benötigte Daten für das Modell-Training**

Um das Modell zu trainieren, benötigen wir **beispielhafte Fragen (Intents)** und **Entitäten**, die das Modell später klassifizieren soll. Diese Daten können durch **manuelles Hinzufügen** im Admin-Bereich oder durch **automatisches Lernen** aus den Benutzer-Chat-Eingaben generiert werden.

##### Beispiel für Trainingsdaten (strukturierte Form)

Die **Trainingsdaten** bestehen aus zwei Hauptkomponenten:

* **Intents**: Was der Benutzer zu erreichen versucht (z. B. „Temperatur abfragen“).
* **Entitäten**: Bestimmte Informationen aus der Nachricht (z. B. „Wohnzimmer“ als Raumname).

###### Beispiel:

```json
{
    "intents": [
        {
            "intent": "Temperatur_abfragen",
            "examples": [
                "Wie ist die Temperatur im Wohnzimmer?",
                "Was ist die Temperatur im Schlafzimmer?",
                "Wie warm ist es im Wohnzimmer?"
            ],
            "entities": [
                {"entity": "Raum", "value": "Wohnzimmer"},
                {"entity": "Raum", "value": "Schlafzimmer"}
            ]
        },
        {
            "intent": "Licht_steuern",
            "examples": [
                "Schalte das Licht im Flur ein.",
                "Mach das Licht im Wohnzimmer an.",
                "Licht aus im Flur."
            ],
            "entities": [
                {"entity": "Gerät", "value": "Flur_Licht"},
                {"entity": "Gerät", "value": "Wohnzimmer_Licht"}
            ]
        }
    ]
}
```

##### Erklärungen:

* **Intent**: Die allgemeine Bedeutung der Anfrage (z. B. „Temperatur\_abfragen“ oder „Licht\_steuern“).
* **Examples**: Beispiele für Nachrichten, die dem Intent zugeordnet werden.
* **Entities**: Bestimmte Entitäten (z. B. „Wohnzimmer“, „Flur\_Licht“) aus den Beispielen.

---



---

---

## 🧠 Zusammenfassung:

* ✅ Trennung von Benutzer-UI und Admin-UI (ggf. kann man ja noch andere Dienste, wie einen Sprachassistenten anbinden).
* ✅ Zentrale API mit klaren Funktionen
* ✅ Zukunftssicher durch Sprachintegration und weitere Clients
* ✅ Bessere Sicherheit, Wartbarkeit und Erweiterbarkeit

# Hintergrundwissen

Aus:

[https://www.melibo.de/blog/was-sind-intent-und-entity](https://www.melibo.de/blog/was-sind-intent-und-entity)

## Intent

Intents, zu Deutsch „Absichten“, sind Zwecke oder Ziele, die in den Eingaben eines Kunden zum Ausdruck kommen, um z.B. eine Frage zu einer Retoure zu stellen. Durch die Erkennung der Absicht, die sich in der Kundeneingabe ausdrückt, versucht der KI-Chatbot den richtigen Dialog zu finden und die passende Ausgabe zu wählen. Dafür nutzen KI-Chatbots maschinelles Lernen, um in natürlicher Sprache die vorher definierte Absicht (Intent) zu erkennen. [1] Einfach gesagt, Intents sind Fragen der User:innen, die dem Chatbot zu einem speziellen Thema gestellt werden und der Versuch des KI-Chatbots, die passende Antwort zu erkennen, um das Problem zu lösen bzw. die Frage zu beantworten.

### Wie funktionieren Intents?

Bestehende Anbieter wie unter anderem der IBM Watson Assistant [1], Rasa [2] oder Microsoft LUIS [3] basieren alle meist auf dem Prinzip der Intent-Ausgabe. Bevor der Chatbot Intents erkennen kann, müssen erst mal alle Absichten der User:innen definiert werden. Hierfür ist es wichtig, dass man seine Kundenanfragen erst mal identifiziert und seinen Use-Case richtig versteht. Nachdem der Intent-Katalog erstellt und der Bot online genommen wurde, werden die User:innen dem Chatbot Fragen stellen. Jede Anfrage der User:innen durchläuft das sogenannte Intent-Matching, also der Zuordnung der Anfrage aus den gesamten Inhalten des Chatbots. Dabei wird anhand von NLP (Natural Language Processing) ein Confidence-Score berechnet, um anhand von Wahrscheinlichkeiten die passende Antwort auszugeben.

## Confidence-Score

Ein kurzer Exkurs zum Thema Confidence-Scores. Die Confidence-Scores liegen zwischen 0 und 1 und geben an, zu wie viel Prozent der Chatbot ein Intent erkannt hat. Zu jeder gestellten Frage der User:innen berechnet der Chatbot also einen Confidence-Score und versucht auf dieser Grundlage, durch Wahrscheinlichkeiten, die richtige Antwort an die User:innen auszugeben. Die Confidence-Scores sind meistens voreingestellt und liegen zwischen 0,6 und 0,7. Das heißt, dass der Chatbot Antworten nur dann ausgibt, wenn die Erkennungswahrscheinlichkeit bei mindestens 60 % liegt. Nehmen wir nun als Beispiel an, dass User:innen die Frage stellen „Was kannst du so?“, um zu erfahren, welche Themen der Chatbot überhaupt beantworten kann. Der KI-Chatbot erkennt zu 89 % Prozent den Intent „Was kannst du?“. In diesem Beispiel gibt der Chatbot die passende Antwort aus und beantwortet somit die Frage.

### Bestandteile eines Intents

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

## Entity

Im Unterschied zu Intents dienen Entitys oder auch Entities dazu, Informationen der User:innen aus der natürlichen Sprache zu extrahieren. Jedes Entity verfügt über eine Reihe von Eigenschaften, die mit ihr verbunden sind. Dabei kannst du auf Informationen deines Entities zugreifen. Wie bei einem Intent gibt der Chatbot an, wie hoch der Confidence-Score liegt. Im Unterschied zu Intents liegt der Confidence-Score, aber bei 0 oder 1. Unabhängig davon, ob und wie die Erkennung des Entitys eingestellt ist, haben sogenannte System-Entites immer eine Erkennung von 1. Jedes Entity besitzt einen Wert, einen sogenannten Entitätswert. Bei der Erstellung von Entities ist es notwendig, dass neben dem Wert auch Typen definiert werden. Unter Typen versteht man allgemein Synonyme, also Wörter, die sich ebenfalls auf dasselbe Vorhaben beziehen, es nur anders umschreiben. Je mehr Synonyme ein Entity hat, desto besser die Erkennung des Chatbots [4].

Grundsätzlich unterscheiden wir zwischen System-Entities und Customize-Entities. Die System-Entites sind voreingestellt, das heißt im System bereits enthalten. Darunter fallen etwa Zahlen, Uhrzeiten oder Adressen. Diese Entities sind besonders beliebt und wurden in der Vergangenheit besonders häufig verwendet. Die Customize-Entities dagegen sind selbst definierte Werte, die auf den jeweiligen Use-Case angepasst werden.

### Ein Beispiel für Entities in der Praxis

🙎‍♂️: „Wann kommt mein Produkt Chatbot-Experte in der Landstraße 5 an?“

### Entities in diesem Beispiel:

Produkt Chatbot Experte (Customize-Entity product_type)
Landstraße 5 (Sytem-Entity street_adress)

### Vorteile von Entites:

Mit Entities kann man User:innen durch Chat Flows in Form von Buttons navigieren, schnell Synonyme definieren und mehrere Anfragen auf einmal verarbeiten. Die System-Entitys helfen außerdem dabei, die beliebten Anfragen zum Thema Ort, Zeit und Adresse abzufangen.

[1]: https://cloud.ibm.com/docs/assistant?topic=assistant-intents
[2]: https://rasa.com/open-source/
[3]: https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-concept-utterance
[4]: https://cloud.ibm.com/docs/assistant?topic=assistant-expression-language
