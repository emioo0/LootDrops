# LootDrops (Paper 1.21.4)

Automatische Lootdrops für dein Minecraft-Netzwerk: zufällige Drops in der Overworld, coole Effekte & Titel, konfigurierbarer Loot, Admin-Commands – und optional eigene **WorldEdit-Schematics** als Drop-Modell.

> **Kurz:** Das Plugin kann auf **öffentlichen Servern frei genutzt** werden.  
> **ABER:** Wenn durch das Plugin **Geld eingenommen** wird (Shop, Ränge, Spenden, In-Game-Käufe etc.), **bitte vorab Kontakt aufnehmen**, um die weitere Nutzung zu besprechen (siehe *Nutzung & Monetarisierung*).

---

## Features

- ⏱️ **Auto-Drops**: Standardmäßig alle 30 Minuten (Timer startet neu, wenn ein Drop verschwindet).
- 🗺️ **Fairer Spawn**: Zufällige Position innerhalb ±2000 Blöcken ab Welt-Spawn, Mindestabstand, **kein Ozean**, **kein Wasser**, **nicht im Baum**, Luft-Schutzzone (keine Blätter/Logs/Decken direkt über der Kiste).
- 📦 **Schematic-Support (optional)**: Wenn **WorldEdit/FAWE** installiert ist und `plugins/LootDrops/Ballon.schem` existiert, wird **deine Schematic** gespawned (Chest-Position wird automatisch erkannt).  
  Ohne WorldEdit nutzt das Plugin einen hübschen **Fallback-Drop** aus Code.
- 🎆 **Effekte & Sounds**: Beim Spawn und beim Entfernen (inkl. Broadcast-Nachricht & Titel).
- 🧰 **Loot**: Zufallsliste für PVP-Items; Limits für Iron/Diamond-Armor/Weapons; random Slots in der Kiste; optional Haltbarkeits-Variationen.
- 🧹 **Despawn**: **Sobald die Kiste einmal geöffnet und wieder geschlossen wird**, verschwindet der Drop mit Effekten – unabhängig vom Restinhalt.
- 🔧 **Saubere Struktur**: Manager, Listener, Tasks, Utils, Loot-Konfiguration, klare Trennung.

---

## Kompatibilität

- **Server:** Paper **1.21.4**
- **Java:** **21**
- **Optional:** WorldEdit **oder** FastAsyncWorldEdit (für `.schem`)

---

## Installation

1. Jar in den `plugins/`-Ordner legen.
2. (Optional) **WorldEdit/FAWE** installieren, wenn du `.schem` nutzen willst.
3. Server starten (erzeugt `plugins/LootDrops/`).
4. (Optional) **Eigene Schematic** als  
   `plugins/LootDrops/Ballon.schem`  
   ablegen (Dateiname **muss exakt** so heißen).  
   > Die Schematic sollte **genau eine Chest** enthalten. Diese wird als Loot-Kiste verwendet.
5. Server neu starten oder Plugin reloaden.

---

## Commands & Permissions

### Commands
- `/lootadmin reload` – Konfiguration & Schematic neu laden  
- `/lootadmin force here` – Drop **hier** sofort spawnen  
- `/lootadmin remove` – aktiven Drop entfernen  
- `/lootadmin status` – Status/Koordinaten anzeigen  
- `/lootdrop` – Spielerinfo (z. B. Koordinaten/Status)

### Permissions
```text
lootdrops.admin  (default: op)
lootdrops.use    (default: true)
````

---

## Konfiguration (Auszug)

Datei: `plugins/LootDrops/config.yml`

```yaml
settings:
  world: "world"
  spawn-radius: 2000
  min-distance-from-spawn: 100

# Limits pro Drop (Caps)
loot:
  limits:
    iron-armor-max: 2
    diamond-armor-max: 1
    weapons-max: 1

  # Anzahl Loot-Rolls pro Drop (Range)
  rolls:
    min: 4
    max: 8

  # Optionale Haltbarkeits-Variation bei damageable Items
  allow-durability-variance: true
  max-damage-fraction: 0.35

  # Beispiel-Pool (du kannst beliebig erweitern/kürzen)
  pool:
    - material: DIAMOND
      min: 1
      max: 3
      weight: 3
      damageable: false

    - material: IRON_HELMET
      min: 1
      max: 1
      weight: 4
      damageable: true

    - material: IRON_CHESTPLATE
      min: 1
      max: 1
      weight: 4
      damageable: true

    - material: IRON_LEGGINGS
      min: 1
      max: 1
      weight: 4
      damageable: true

    - material: IRON_BOOTS
      min: 1
      max: 1
      weight: 4
      damageable: true

    - material: IRON_SWORD
      min: 1
      max: 1
      weight: 6
      damageable: true

    - material: BOW
      min: 1
      max: 1
      weight: 5
      damageable: true

    - material: ARROW
      min: 16
      max: 48
      weight: 8
      damageable: false

    - material: COBWEB
      min: 8
      max: 24
      weight: 10
      damageable: false

    - material: ENDER_PEARL
      min: 2
      max: 6
      weight: 6
      damageable: false

    - material: GOLDEN_APPLE
      min: 1
      max: 3
      weight: 5
      damageable: false

    - material: COOKED_BEEF
      min: 16
      max: 32
      weight: 10
      damageable: false
```

**Wie funktioniert’s?**

* `rolls.min/max` bestimmt, wie viele Einträge gewürfelt werden.
* `weight` steuert die Wahrscheinlichkeit (höher = häufiger).
* Caps (`iron-armor-max`, `diamond-armor-max`, `weapons-max`) begrenzen Sets (z. B. Full-Iron wird verhindert).
* Items landen **zufällig verteilt** in unterschiedlichen Slots.

---

## Schematic-Hinweise

* Datei: `plugins/LootDrops/Ballon.schem`
* Unterstützt WorldEdit & FAWE-`.schem` (neuere Formate).
* Der Drop wird so gepastet, dass **die Chest exakt auf dem berechneten Boden** steht.
* **Luft wird ignoriert** → Terrain bleibt.
* Typische Markerblöcke (z. B. rotes Wool/Concrete, `LIGHT`, `BARRIER`, `STRUCTURE_VOID`) werden **automatisch entfernt**.
* Beim Entfernen des Drops werden **alle gesetzten Blöcke** auf ihren **Originalzustand** zurückgesetzt (kein kaputtes Terrain).

---

## Bauen (Maven)

Voraussetzungen: **JDK 21**, Maven.

```bash
mvn clean package
```

### Wichtige POM-Einträge (bereits enthalten)

* `paper-api:1.21.4-R0.1-SNAPSHOT` (scope `provided`)
* `worldedit-bukkit:7.2.18` (scope `provided`)
* Repositories: Paper & EngineHub

---

## Roadmap (Ideen)

* Mehrere Schematics zufällig aus `/schematics/` wählen
* Coords-Hint im Chat klickbar machen (Kompass/Waypoint)
* BossBar während aktiver Drops
* Per-World Einstellungen & Blacklist-Biomes

---

## Support

* **Bugs/Feature-Requests:** GitHub Issues
* **Kontakt:** Bitte hier deine E-Mail/Discord eintragen.

---

## Nutzung & Monetarisierung

* Die Nutzung ist **kostenlos**, auch auf **öffentlichen Servern**.
* Wenn dein Server durch dieses Plugin **Einnahmen** generiert (z. B. Shop, Ränge, Spenden, In-Game-Käufe),
  **kontaktiere den Autor vorab**, um die weitere Nutzung zu besprechen und eine **Genehmigung** einzuholen.

Formulierungsvorschlag für dein Server-Impressum/Repo:

> *Dieses Plugin „LootDrops“ wird mit Erlaubnis genutzt.
> Bei kommerzieller Nutzung bitte vorab den Autor kontaktieren.*

---

## Lizenz

© Emilio – Alle Rechte vorbehalten.
Kostenlose Nutzung auf öffentlichen Servern erlaubt; **kommerzielle Nutzung nur nach Absprache** (siehe oben).
