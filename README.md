# 🌡️ Vorausschauender Hitzeschutz – Home Assistant Blueprint

Ein Blueprint, das die Rollläden / Jalousien **einer Fassade** automatisch verschattet,
**bevor** sich die Räume aufheizen – gesteuert über die **Tages-Temperaturvorhersage**
und den **Sonnenstand**.

## Features

- **Pro Fassade einmal angelegt** – steuert alle Verschattungen einer Seite gemeinsam
- **Dem Sonnenlauf folgend** – über das Azimut-Fenster verschattet jede Seite genau
  dann, wenn die Sonne sie trifft (Ost morgens, Süd mittags, West nachmittags)
- **Rollläden und Jalousien mischbar** – pro Gerät wird automatisch erkannt, ob
  **Position** und/oder **Lamellen-/Kippwinkel** unterstützt wird
- **Vorausschauend** – nutzt das vorhergesagte Tagesmaximum, nicht nur den Istwert
- **Motor-/Batterieschonung** – Mindestpause und Positionstoleranz je Rollladen
- **Anwesenheits-Modus** – mildere Verschattung, wenn jemand zu Hause ist
- **Push-Benachrichtigungen** – optional bei Verschatten/Zurückfahren

## Verschattet wird nur, wenn ALLE Bedingungen erfüllt sind

- vorhergesagtes **Tagesmaximum** ≥ Schwelle (vorausschauend)
- **aktuelle Außentemperatur** ≥ Schwelle (kein Verschatten an noch kühlen Hitzetagen)
- Sonne im **Azimut-Fenster** der Fassade
- Sonne über der **Mindesthöhe** (Elevation)
- optional: **Außen-Helligkeit/Einstrahlung** über Schwelle (kein Verschatten bei Bewölkung)
- innerhalb des **Aktiv-Zeitfensters**

> 💡 **Wichtig:** Raum-interne Lux-Sensoren (z.B. von Bewegungsmeldern) eignen sich
> **nicht** als Tor – sobald verschattet ist, messen sie kein Sonnenlicht mehr.
> Nutze stattdessen einen **Außen**-Solar-/Einstrahlungssensor.

## Installation

### Über HACS (empfohlen)

1. HACS öffnen → **Automations** → Drei-Punkte-Menü → **Custom repositories**
2. URL: `https://github.com/Tschaegged/waermeschutz-blueprint` – Kategorie: **Automation**
3. Blueprint suchen und importieren

### Manuell

[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/Tschaegged/waermeschutz-blueprint/main/blueprints/automation/waermeschutz_rollladen.yaml)

Oder direkt in Home Assistant:
**Einstellungen → Automationen → Blueprints → Blueprint importieren** und folgende URL eingeben:

```
https://raw.githubusercontent.com/Tschaegged/waermeschutz-blueprint/main/blueprints/automation/waermeschutz_rollladen.yaml
```

## Voraussetzungen

- Home Assistant **2024.10** oder neuer (nutzt die Action `weather.get_forecasts`)
- Eine **Wetterentität** mit Tagesvorhersage (z.B. `met`, `dwd_weather`)
- Aktivierte **`sun.sun`**-Entität (liefert Azimut & Elevation)
- Optional: Außen-Helligkeits-/Einstrahlungssensor, `input_boolean` als Status-Helfer

## Wichtigste Optionen

| Option | Zweck | Standard |
|---|---|---|
| Rollläden / Jalousien dieser Fassade | Mehrfachauswahl – werden gemeinsam gesteuert | – |
| Wettervorhersage | Quelle des Tagesmaximums | – |
| Vorhersage-Schwelle | Tagesmaximum, ab dem aktiv | 26 °C |
| Azimut Beginn/Ende | Sonnenfenster der Fassade | 90° / 270° |
| Steuerungsart | Position / Tilt / Beides (pro Gerät auto-erkannt) | Position |
| Verschattungs-Position / -Kippwinkel | Zielwerte beim Verschatten | 25 % / 30 % |
| Offen-Position / -Kippwinkel | Rückfahrt | 100 % / 100 % |
| Rückfahrt nur Kippwinkel (je Jalousie) | Position bleibt bei der Rückfahrt unverändert | – |
| Außentemperatur-Sensor + Schwelle | Echtzeit-Bedingung | – / 22 °C |
| Mindest-Sonnenhöhe | kein Verschatten bei tiefer Sonne | 12° |
| Außen-Helligkeit + Schwelle | kein Verschatten bei Bewölkung (optional) | – / 30000 |
| Zusätzliche Bedingungen | eigene UND-Bedingungen | – |
| Aktiv ab/bis | Zeitfenster | 08:00 / 21:00 |
| Mindestpause / Positionstoleranz | Motor-/Batterieschonung (je Rollladen) | 10 min / 8 % |
| Anwesenheits-Entität + Position | mildere Verschattung bei Anwesenheit | – / 45 % |
| Status-Helfer | Rückfahrt nur bei eigener Verschattung | – |
| Benachrichtigungen | Push bei Verschatten/Zurückfahren | aus |

## Azimut-Fenster pro Himmelsrichtung (Richtwerte)

| Fassade | Azimut Beginn | Azimut Ende |
|---|---|---|
| Ost | 60° | 120° |
| Süd | 120° | 240° |
| Südwest | 150° | 270° |
| West | 240° | 300° |

## Lizenz

[MIT](LICENSE)
