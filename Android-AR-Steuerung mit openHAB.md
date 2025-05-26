# Augmented Reality-basierte Steuerung von Smart-Home-Geräten über openHAB auf Android

## Kurzbeschreibung:

Im Rahmen dieser Bachelorthesis soll eine Android-App entwickelt werden, die mithilfe von Augmented Reality (AR) Smart-Home-Geräte aus openHAB erkennt und interaktiv steuern kann. Die Geräte werden im Kamerabild identifiziert und mit einem überlagerten 3D-Interface versehen. Über dieses Interface soll der Benutzer Aktionen wie Ein-/Ausschalten, Werte einstellen oder Zustände abfragen können. Zudem soll eine Konfigurationsoberfläche implementiert werden, um die Verbindung zu openHAB über die REST-API herzustellen und neue Geräte mit Referenzbildern manuell einzupflegen.

## Ziele der Arbeit:

* Entwicklung einer Android-App mit AR-Funktionalität
* Integration einer REST-Schnittstelle zu openHAB
* Visualisierung von Geräten mittels 3D-Modellen und Interaktionsmenüs im Kamerabild
* Konfigurationsmenüs zum Hinzufügen neuer Geräte (per Fotos) und Verwaltung der REST-Verbindung zu openHAB
* Bilderkennung zum Abgleich von Geräten im Raum

## Anforderungen

### Funktionale Anforderungen:

1. Geräteerkennung über Kamera:
  * Bildbasierte Erkennung von physischen Geräten
  * Zuordnung zu einem bestimmten openHAB-Item anhand gespeicherter Bilder
2. Augmented Reality UI:
  * Anzeige eines 3D-Objekts (z. B. Schalter, Regler) über dem erkannten Gerät
  * Öffnen eines Menüs beim Tippen auf das AR-Objekt
3. openHAB-Anbindung:
  * Konfiguration der REST-API-Adresse
  * Kommunikation mit openHAB zur Steuerung (Commands) und Statusabfrage (States)
4. Konfigurationsmodul:
  * Hinzufügen neuer Geräte
  * Zuweisung von Bildern zur späteren Erkennung
  * Mapping zum openHAB-Itemnamen

### Nicht-funktionale Anforderungen:

    * Mobile-first Design (Android)
    * AR-Erkennung soll performant im Innenraum arbeiten
    * Erweiterbarkeit durch neue Gerätetypen

## Technologien & Frameworks (Auswahl):

### 📱 AR Frameworks:

* ARCore (Google)
  * Android-nativ
  * Gute Erkennung & Tracking-Fähigkeiten
  * Unterstützung für Augmented Images
* Unity + Vuforia
  * Plattformübergreifend, visuell mächtig
  * Leichtes Hinzufügen von 3D-Objekten und UI
  * Gute Unterstützung für Marker-basierte Erkennung
* 8thWall (WebAR, optional)
  * AR direkt im Browser
  * Könnte für prototypisches Web-Frontend genutzt werden

## 🌐 Smart-Home Integration:

* openHAB REST API
  * JSON-basierte API
  * Authentifizierung beachten (evtl. mit Tokens oder Basic Auth)

## 🧠 Bilderkennung & ML (optional):

* Firebase ML Kit
  * Offline-Bilderkennung möglich
* TensorFlow Lite
  * Für fortgeschrittenes Matching von Geräten
* OpenCV
  * Klassische Bilderkennung (z. B. Template Matching)

## 🧰 Weitere Technologien:

* Kotlin / Java – Native Android-Entwicklung
* Unity C# – Bei Unity-basiertem Ansatz
* Jetpack Compose – Für modernes Android-UI
* Room / SQLite – Lokale Persistenz von Geräte- und Konfigurationsdaten
* Retrofit / Volley – Für REST-Kommunikation mit openHAB

## 📊 Schaubild: Systemübersicht

![ar_openhab](https://raw.githubusercontent.com/Michdo93/SmartHome-Ideen/refs/heads/main/screenshots/ar_openhab.png)


