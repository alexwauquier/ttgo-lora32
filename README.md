<div align="center">
  <h1>TTGO LoRa32 – Hello LoRaWAN :globe_with_meridians:</h1>

  <h3>Envoi d’un message "Hello, World!" via LoRaWAN avec la carte LoRa32 V2.1_1.6</h3>

  <h4>
    <a href="#memo-à-propos-du-projet">À propos du projet</a>
    •
    <a href="#triangular_flag_on_post-pour-commencer">Pour commencer</a>
    •
    <a href="#link-liens-utiles">Liens utiles</a>
  </h4>
</div>

<h2>:bookmark_tabs: Table des matières</h2>

- [:memo: À propos du projet](#memo-à-propos-du-projet)
  - [:dart: Objectif](#dart-objectif)
  - [:books: Bibliothèques utilisées](#books-bibliothèques-utilisées)
- [:triangular\_flag\_on\_post: Pour commencer](#triangular_flag_on_post-pour-commencer)
  - [:satellite: Qu’est-ce que LoRa et LoRaWAN ?](#satellite-quest-ce-que-lora-et-lorawan-)
  - [:toolbox: Prérequis](#toolbox-prérequis)
  - [:floppy\_disk: Installation de PlatformIO sur VS Code](#floppy_disk-installation-de-platformio-sur-vs-code)
  - [:package: Mise en place du projet](#package-mise-en-place-du-projet)
  - [:wrench: Variables à modifier](#wrench-variables-à-modifier)
- [:file\_folder: Fichier platformio.ini](#file_folder-fichier-platformioini)
  - [:control\_knobs: Détail des paramètres](#control_knobs-détail-des-paramètres)
  - [:gear: Détails des build\_flags](#gear-détails-des-build_flags)
- [:link: Liens utiles](#link-liens-utiles)
  - [:package: Matériel](#package-matériel)
  - [:books: Bibliothèques](#books-bibliothèques)
  - [:pushpin: Autres](#pushpin-autres)

## :memo: À propos du projet

### :dart: Objectif

Ce projet montre comment utiliser la carte LoRa32 V2.1_1.6 pour se connecter à un réseau LoRaWAN et transmettre un message "Hello, World!" périodiquement. Il a été réalisé dans un contexte de découverte de la technologie LoRa, en particulier pour des applications IoT basse consommation à longue portée. Il repose sur l'utilisation de PlatformIO ainsi que sur la pile LMIC de MCCI pour gérer la communication LoRaWAN.

Il peut servir de base pédagogique, d’exemple de projet LoRa ou de prototype pour une solution IoT plus complexe.

> [!NOTE] Pourquoi utiliser PlatformIO plutôt qu'Arduino IDE ?
> PlatformIO simplifie la gestion des bibliothèques, centralise leur configuration dans le fichier `platformio.ini` et les télécharge automatiquement, ce qui facilite la mise en place du projet.

### :books: Bibliothèques utilisées

Les bibliothèques suivantes sont nécessaires au bon fonctionnement du projet :

| Bibliothèque                     | Utilité                          |
| -------------------------------- | -------------------------------- |
| `Adafruit GFX Library`           | Bibliothèque graphique générique |
| `Adafruit SSD1306`               | Gestion de l'écran OLED          |
| `MCCI LoRaWAN LMIC library`      | Gestion de la pile LoRaWAN       |

## :triangular_flag_on_post: Pour commencer

### :satellite: Qu’est-ce que LoRa et LoRaWAN ?

- **LoRa** (Long Range) est une technologie de modulation radio qui permet des communications sans fil à longue portée avec une faible consommation d'énergie. Elle est idéale pour les objets connectés (IoT) dans des environnements où la couverture réseau est limitée.

- **LoRaWAN** (LoRa Wide Area Network) est un protocole de communication réseau qui fonctionne par-dessus LoRa. Il définit la manière dont les appareils se connectent à des passerelles pour transmettre les données vers un réseau centralisé (Network Server), avec des mécanismes de sécurité, d’authentification et de gestion des données.

> [!TIP] Conseil
> Pour une vue d’ensemble complète, vous pouvez consulter cette présentation de l’[architecture LoRaWAN](https://www.thethingsnetwork.org/docs/lorawan/architecture/), proposée par The Things Network.

### :toolbox: Prérequis

- **Visual Studio Code**
- Extension **PlatformIO IDE**
- Carte **LoRa32 V2.1_1.6**
- Accès à un serveur **LoRaWAN** comme **The Things Network** ou **ChirpStack**
- Vos identifiants LoRaWAN :
  - `AppEUI` (aussi appelé `JoinEUI`) : identifiant unique de l'application
  - `DevEUI` : identifiant unique de l'appareil (End Device)
  - `AppKey` : clé de chiffrement pour l’activation OTAA

### :floppy_disk: Installation de PlatformIO sur VS Code

1. Ouvrir **Visual Studio Code**.
2. Aller dans l’onglet **Extensions**.
3. Rechercher et installer **PlatformIO IDE**.
4. Une fois installée, redémarrer VS Code.

### :package: Mise en place du projet

1. Cloner ce dépôt :

```
git clone https://github.com/alexwauquier/ttgo-lora32.git
```

2. Ouvrir le dossier cloné avec Visual Studio Code.
3. PlatformIO devrait détecter automatiquement le projet et installer les dépendances définies dans le fichier `platformio.ini`.

### :wrench: Variables à modifier

```cpp
static const u1_t PROGMEM APPEUI[8]  = { YOUR_APPEUI };
static const u1_t PROGMEM DEVEUI[8]  = { YOUR_DEVEUI };
static const u1_t PROGMEM APPKEY[16] = { YOUR_APPKEY };
```

> [!WARNING] Attention à l’ordre des octets
> Les plateformes comme **The Things Network** (TTN) et **ChirpStack** affichent les identifiants (`AppEUI`, `DevEUI`, etc.) en **MSB** (Most Significant Byte first).
> 
> Cependant, la bibliothèque **LMIC** lit ces identifiants en **LSB** (*Least Significant Byte first*), c’est-à-dire du byte le moins significatif au plus significatif.  
> Il est donc nécessaire d’**inverser l’ordre des octets** pour `AppEUI` et `DevEUI`.
>
> Seule l'`AppKey` doit rester en **MSB** car la bibliothèque **LMIC** la lit dans cet ordre.
> 
> **Exemple :**
> 
> Si l'interface **TTN** ou **ChirpStack** affiche :
> 
> ```txt
> DevEUI: 70B3D57ED00201A0
> ```
> 
> Il faudra écrire dans le code :
> 
> ```cpp
> { 0xA0, 0x01, 0x02, 0xD0, 0x7E, 0xD5, 0xB3, 0x70 }
> ```

## :file_folder: Fichier platformio.ini

```ini
[env:ttgo-lora32-v21]
platform = espressif32
board = ttgo-lora32-v21
framework = arduino
monitor_speed = 115200
lib_deps = 
    adafruit/Adafruit GFX Library@^1.12.1
    adafruit/Adafruit SSD1306@^2.5.14
    mcci-catena/MCCI LoRaWAN LMIC library@^5.0.1
build_flags = 
    -D ARDUINO_LMIC_PROJECT_CONFIG_H_SUPPRESS
    -D CFG_eu868=1
    -D CFG_sx1276_radio=1
```

### :control_knobs: Détail des paramètres

- `platform = espressif32` : plateforme cible (ESP32 d’Espressif)
- `board = ttgo-lora32-v21` : carte utilisée (TTGO LoRa32 V2.1_1.6)
- `framework = arduino` : utilisation du framework Arduino
- `monitor_speed = 115200` : vitesse du moniteur série (en bauds)

> [!NOTE] Version TTGO LoRa32
> Le nom officiel de la carte est **LoRa32 V2.1_1.6** mais elle porte l’inscription **T3_V1.6.1** sur le circuit imprimé.

### :gear: Détails des build_flags

- `ARDUINO_LMIC_PROJECT_CONFIG_H_SUPPRESS` : désactive la recherche du fichier de configuration `project_config.h` de la bibliothèque **LMIC**
- `CFG_eu868=1` : utilise la bande de fréquence **EU868**, adaptée à l’Europe
- `CFG_sx1276_radio=1` : précise que la puce radio utilisée est une **SX1276**

## :link: Liens utiles

### :package: Matériel

- [GitHub TTGO LoRa32 V2.1](https://github.com/LilyGO/TTGO-LoRa32-V2.1)

### :books: Bibliothèques

- [GitHub Adafruit GFX Library](https://github.com/adafruit/Adafruit-GFX-Library)
- [GitHub Adafruit SSD1306](https://github.com/adafruit/Adafruit_SSD1306)
- [GitHub MCCI LoRaWAN LMIC library](https://github.com/mcci-catena/arduino-lmic)

### :pushpin: Autres

- [Documentation PlatformIO ](https://docs.platformio.org/en/latest/)
- [Documentation The Things Network](https://www.thethingsnetwork.org/docs/)
- [Wikipédia Numérotation des bits (LSB et MSB)](https://fr.wikipedia.org/wiki/Num%C3%A9rotation_des_bits)
