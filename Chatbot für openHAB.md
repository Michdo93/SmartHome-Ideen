


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

‍### Ein Beispiel für Entities in der Praxis

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
