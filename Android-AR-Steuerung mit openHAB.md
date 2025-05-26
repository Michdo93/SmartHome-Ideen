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

<img src="https://raw.githubusercontent.com/Michdo93/SmartHome-Ideen/refs/heads/main/screenshots/ar_openhab.png" alt="ar_openhab" width="50%">

## Systemaufbau

### openHAB Config UI

Eine einfache View, in der man die notwendigen Daten für die REST API von openHAB speichert. Man muss wählen können zwischen `Basic Authentication` oder `API Token`:

#### Basic Authentication

* Benutzername der openHAB-Instanz
* Passwort der openHAB-Instanz
* URL der openHAB-Instanz

#### API Token

* API-Token der openHAB-Instanz
* URL der openHAB-Instanz

### AR-Szene mit Kamerastream und 3D UI-Overlay mit Menü

Eine View, in der man sieht, was die Kamera des Smartphones zeigt. Sobald man über ein 3D-Objekt fährt, welches bedient werden kann, öffnet sich in dieser View ein Frame mit der Bedienung zu diesem Gerät. Das 3D-Objekt kann bspw. mit Unity erstellt sein und durch die Anbindung von Vuforia über ein Mapping erkannt werden. Damit dies funktioniert, müsste man die trainierte Datenbank von Vuforia bei neu hinzugefügten Geräte ebenfalls aktualisieren. Ebenfalls zu überlegen ist, dass für die App grundlegend fertige 3D-Objekte in Unity schon vorgefertigt sind, damit wenn ein Gerät auch erkannt wird, dieses 3D-Objekt angezeigt werden kann. In der Gerätedatenbank müsste man dann diesem Gerät ein 3D-Objekt und Bilder zur Erkennung im Kamerastream hinzufügen. Als Alternative kann bspw. auch ARCore verwendet werden.

In der AR-Szene mit Kamerastream soll hauptsächlich ein Gerätematching stattfinden. Das bedeutet, die Kamera muss einen Vergleich mit der Gerätedatenbank machen. Wenn ein Gerät zugeordnet werden kann, dann kann über den dort ebenfalls gespeicherten Itemnamen die openHAB REST API den Status von Items dieses Gerätes anzeigen und es gleichzeitig ermöglichen, Commands an Items dieses Gerätes zu senden. Am besten fragt man das Group-Item ab oder gibt vor, dass nur ein Itemname gespeichert werden kann und dass dieses Item vom Item Type Group sein muss.

Über die REST-API erhält man außerdem dann zu jedem Item in dieser Group, deren Item Types, deren State Description und deren Command Description. Anhand dieser Informationen lässt sich ein Fragment erzeugen, welches entsprechend den Item-State wiedergibt und die Command-Bedienmöglichkeiten bereit stellt.

Wichtige für die Bedienung von openHAB Items ist, dass die Item Types immer gleich bedient werden können. Heißt hier kann man auch Vorlagen (Templates) und Klassen anlegen, die man dann wieder verwendet.

### Gerätedatenbank

Klassischerweise implementiert man hier `CRUD`-Operationen

* CREATE
  * Man hat ein Formular, bei dem man den Namen des Geräts angibt und den Itemnamen für das openHAB Group Item.
  * Man hat je Gerät eine eigene Galerie an Bilder.
    * Ich muss mindestens ein Bild zu jedem Gerät schießen.
    * Vielleicht macht irgendwo ein Limit von 4-6 Bilder Sinn.
    * Die Bilder müssen für ein Training verwendet werden können.
* READ
  * Wird beim Gerätematching in der AR-Szene benötigt.
* UPDATE
  * Man kann jederzeit zu dem Gerät den Namen anpassen oder auch den Itemnamen ändern.
  * Man kann jederzeit zu dem Gerät Bilder wieder löschen, neue hinzufügen oder "ersetzen". 
* DELETE
  * Man kann Geräte auch wieder komplett löschen.
    * Heißt sowohl die Bilder, als auch der Name und Itemname verschwinden aus der Datenbank. 

Wozu benötigt man alle CRUD-Methoden?

* In deinem Smart Home kann man immer wieder mal alte Geräte durch neuere ersetzen.
* In deinem Smart Home kann man die Namensgebungsstruktur seiner Items anpassen.
* In deinem Smart Home kannst du neue Geräte hinzufügen.
* In deinem Smart Home kannst du alte Geräte entfernen, ohne sie durch neuere zu ersetzen.
* Du kannst versehentlich dich auch mal bei einem Gerät vertan haben und das falsche fotografiert haben bzw. bei einem fotografierten Gerät gedacht haben, dass dessen Name anders sei (z. B. mehrere gleiche Lampen).
* ...

Ein Smart Home System bleibt selten konstant. Man richtet es ja nicht nur ein einziges mal ein und über 20-30 Jahren sind alle Geräte gleich!

Das Speichern der Bilder für deine Gerätedatenbank ist ein zentraler Aspekt deines Projekts – vor allem, wenn du Geräte durch Bildabgleich erkennen willst. Hier ist ein Überblick über mögliche Optionen, deren Vor- und Nachteile sowie ein Vorschlag für den Aufbau deiner Gerätedatenbank inklusive Bildspeicherung.

#### 📦 **Wie und wo die Bilder gespeichert werden können**

##### 🟢 **1. Speicherung in SQLite mit Pfad-Referenz (empfohlen)**

**Vorgehen:**

* Du speicherst die Bilder **als Dateien im internen Speicher oder App-spezifischen Speicher** (z. B. `/data/data/<package>/files/devices/`).
* In der SQLite-Datenbank speicherst du **nur den Pfad zum Bild**, zusammen mit anderen Gerätedaten.

**Vorteile:**

* Geringere Datenbankgröße
* Schnellere Zugriffszeiten
* Einfach zu verwalten und backupfähig
* Gute Integration in bestehende Android-Architektur

**Nachteile:**

* Zusätzliche File-Management-Logik notwendig

---

##### 🔴 **2. Speicherung direkt als BLOB in SQLite**

**Vorgehen:**

* Das Bild (z. B. JPEG oder PNG) wird als **Byte-Array** direkt in einem BLOB-Feld in SQLite gespeichert.

**Vorteile:**

* Alles in einer Datei (praktisch für kleine Datenmengen)
* Kein extra File-System-Handling

**Nachteile:**

* Datenbank kann sehr groß und träge werden
* Längere Ladezeiten bei vielen Geräten/Bildern

➡ **Nur sinnvoll für sehr kleine Bildmengen oder Prototyping**

---

##### 🟡 **3. Speicherung über ContentProvider oder MediaStore (optional/fortgeschritten)**

* Für öffentlich zugängliche Bilder (z. B. wenn du willst, dass der User sie auch in seiner Galerie sieht)
* Aufwändiger und meist nicht notwendig, wenn alles in der App bleibt

---

#### 🧩 **Vorgeschlagene Datenbankstruktur (SQLite mit Pfad zu Bilddateien)**

```sql
Tabelle: devices

| id  | name         | openhab_item_id | image_paths                     |
|-----|--------------|------------------|---------------------------------|
| 1   | Wohnzimmerlampe | Switch_Light_1 | /files/devices/lamp1_1.jpg;... |
```

**Feldbeschreibung:**

* `id`: Primärschlüssel
* `name`: Anzeigename des Geräts
* `openhab_item_id`: Name des Items in der openHAB REST-API
* `image_paths`: Semikolon-getrennte Liste von Pfaden zu gespeicherten Bildern (alternativ: eigene Tabelle für Bilder)

---

#### 🔄 **Alternative: Normalisierte Struktur**

Falls du mehrere Bilder je Gerät hast und du sauber bleiben willst:

```sql
Tabelle: devices
| id  | name         | openhab_item_id |
|-----|--------------|------------------|
| 1   | Wohnzimmerlampe | Switch_Light_1 |

Tabelle: device_images
| id  | device_id | image_path                    |
|-----|-----------|-------------------------------|
| 1   | 1         | /files/devices/lamp1_1.jpg    |
| 2   | 1         | /files/devices/lamp1_2.jpg    |
```

➡ Diese Struktur ist besser skalierbar und sauberer im Zugriff.

---

#### 🔧 **Wie speicherst du ein Bild in Kotlin/Android?**

##### Speichern eines Bildes im Dateisystem:

```kotlin
fun saveBitmapToInternalStorage(context: Context, bitmap: Bitmap, filename: String): String {
    val file = File(context.filesDir, filename)
    FileOutputStream(file).use { out ->
        bitmap.compress(Bitmap.CompressFormat.PNG, 100, out)
    }
    return file.absolutePath
}
```

##### Speichern des Pfads in SQLite:

Mit `Room` oder klassisch mit `SQLiteOpenHelper`.

---

#### 📌 Empfehlung für dein Projekt

| Kriterium               | Empfehlung                              |
| ----------------------- | --------------------------------------- |
| Anzahl Bilder < 50      | SQLite mit Pfad oder sogar BLOB möglich |
| Anzahl Bilder > 50–100+ | SQLite mit Pfad & File-System           |
| Zukunftssicherheit      | Normalisierte Struktur mit Bildtabelle  |
| Performance wichtig     | Pfadbasierte Speicherung                |

---

Gerne! Hier ist ein vollständiges **Room-Datenbankmodell** für dein AR/openHAB-Projekt mit Unterstützung für:

* Geräteverwaltung
* Mehrere Bilder pro Gerät (Referenzbilder)
* Speicherung der Bildpfade im Dateisystem

---

#### 📦 **1. Datenbankstruktur (Room)**

##### 🗂 `DeviceEntity.kt`

```kotlin
import androidx.room.Entity
import androidx.room.PrimaryKey

@Entity(tableName = "devices")
data class DeviceEntity(
    @PrimaryKey(autoGenerate = true) val id: Long = 0,
    val name: String,
    val openhabItemId: String
)
```

---

##### 🖼 `DeviceImageEntity.kt`

```kotlin
import androidx.room.Entity
import androidx.room.ForeignKey
import androidx.room.PrimaryKey

@Entity(
    tableName = "device_images",
    foreignKeys = [ForeignKey(
        entity = DeviceEntity::class,
        parentColumns = ["id"],
        childColumns = ["deviceId"],
        onDelete = ForeignKey.CASCADE
    )]
)
data class DeviceImageEntity(
    @PrimaryKey(autoGenerate = true) val id: Long = 0,
    val deviceId: Long,
    val imagePath: String // z. B. /data/data/.../files/devices/lamp1_1.jpg
)
```

---

#### 🔁 **2. DAO-Interfaces**

##### `DeviceDao.kt`

```kotlin
import androidx.room.*
import kotlinx.coroutines.flow.Flow

@Dao
interface DeviceDao {

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertDevice(device: DeviceEntity): Long

    @Query("SELECT * FROM devices")
    fun getAllDevices(): Flow<List<DeviceEntity>>

    @Query("SELECT * FROM devices WHERE id = :id")
    suspend fun getDeviceById(id: Long): DeviceEntity?
}
```

---

##### `DeviceImageDao.kt`

```kotlin
import androidx.room.*

@Dao
interface DeviceImageDao {

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertImage(image: DeviceImageEntity): Long

    @Query("SELECT * FROM device_images WHERE deviceId = :deviceId")
    suspend fun getImagesForDevice(deviceId: Long): List<DeviceImageEntity>

    @Delete
    suspend fun deleteImage(image: DeviceImageEntity)
}
```

---

#### 🧩 **3. Datenbank-Klasse**

##### `AppDatabase.kt`

```kotlin
import androidx.room.Database
import androidx.room.RoomDatabase

@Database(entities = [DeviceEntity::class, DeviceImageEntity::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun deviceDao(): DeviceDao
    abstract fun deviceImageDao(): DeviceImageDao
}
```

---

#### 🚀 **4. Nutzung in deiner App (z. B. ViewModel oder Repository)**

```kotlin
val db = Room.databaseBuilder(
    context,
    AppDatabase::class.java,
    "device_database"
).build()

val newDevice = DeviceEntity(name = "Wohnzimmerlampe", openhabItemId = "Light_Livingroom")
val deviceId = db.deviceDao().insertDevice(newDevice)

val imagePath = saveBitmapToInternalStorage(context, bitmap, "lamp1_1.jpg")
db.deviceImageDao().insertImage(DeviceImageEntity(deviceId = deviceId, imagePath = imagePath))
```

---

#### ✅ Vorteile dieser Lösung

* Skalierbar: Beliebig viele Bilder pro Gerät
* Sicher: Geräte und Bilder sind logisch verknüpft
* Kompatibel: Ideal für AR mit Bildvergleich
* Persistenz: Einfach zu sichern oder exportieren

---
