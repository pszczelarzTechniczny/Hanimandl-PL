# 🍯 HaniMandl — Automatyczny Dozownik Miodu
# 🍯 HaniMandl — Automatic Honey Dispenser

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Platform: ESP32](https://img.shields.io/badge/Platform-ESP32-green.svg)](https://www.espressif.com/)
[![Version](https://img.shields.io/badge/Version-0.2.13-orange.svg)]()

---

## 📌 Oryginalne repozytorium

> **Oryginalny kod źródłowy, pełna historia wersji, schematy elektryczne oraz wszystkie pliki projektu znajdują się pod adresem:**
>
> ### 🔗 [https://github.com/hiveeyes/hanimandl](https://github.com/hiveeyes/hanimandl)
>
> Niniejszy plik README oraz towarzysząca mu dokumentacja zostały opracowane jako materiał uzupełniający do projektu HaniMandl. Wszelkie prawa do kodu źródłowego należą do oryginalnych autorów wymienionych poniżej.

---

## 📌 Original Repository

> **The original source code, full version history, wiring diagrams and all project files are available at:**
>
> ### 🔗 [https://github.com/hiveeyes/hanimandl](https://github.com/hiveeyes/hanimandl)
>
> This README and accompanying documentation were created as supplementary material for the HaniMandl project. All rights to the source code belong to the original authors listed below.

---

## 📖 Opis

HaniMandl to automatyczny dozownik (naleśnik) miodu oparty na mikrokontrolerze ESP32 (płytka Heltec WiFi Kit 32). Urządzenie steruje serwomechanizmem otwierającym kran spustowy, jednocześnie monitorując masę napełnianego słoika za pośrednictwem czujnika wagi HX711. Dzięki temu możliwe jest precyzyjne, automatyczne napełnianie słoików zadaną ilością miodu.

---

## 📖 Description

HaniMandl is an automatic honey dispenser based on the ESP32 microcontroller (Heltec WiFi Kit 32 board). The device controls a servo motor that opens a honey gate valve, while simultaneously monitoring the weight of the jar being filled via an HX711 load cell sensor. This enables precise, automated filling of jars to a target weight.

---

## ✨ Główne funkcje

| Funkcja | Opis |
|---|---|
| 🏺 5 rozmiarów słoików | Konfigurowalne przez użytkownika |
| ⚡ Tryb automatyczny | Autostart, autokorekta naftropfenu |
| 🖐️ Tryb ręczny | Pełna kontrola kąta otwarcia serwomechanizmu |
| 📺 Wyświetlacz OLED | 128×64 px, SSD1306, I2C |
| 🔢 Licznik słoików | Licznik sesji i licznik całkowity |
| 📡 OTA via GitHub | Aktualizacja firmware przez WiFi |
| 🔊 Buzzer | Sygnały dźwiękowe statusu |
| 💾 Pamięć ustawień | Flash NVS (Preferences), zachowana po resecie zasilania |

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 🏺 5 jar sizes | User-configurable |
| ⚡ Automatic mode | Autostart, drip autocorrection |
| 🖐️ Manual mode | Full servo opening angle control |
| 📺 OLED display | 128×64 px, SSD1306, I2C |
| 🔢 Jar counter | Session counter and total counter |
| 📡 OTA via GitHub | Firmware update over WiFi |
| 🔊 Buzzer | Status audio signals |
| 💾 Settings memory | Flash NVS (Preferences), retained after power cycle |

---

## 🔧 Wymagania sprzętowe

- **Mikrokontroler:** Heltec WiFi Kit 32 (ESP32) — V1, V2 lub V3
- **Czujnik wagi:** Moduł HX711 + belka tensometryczna
- **Napęd:** Serwomechanizm sterujący kranem quetschhahn
- **Sterowanie:** Enkoder obrotowy KY-040 lub HW-040
- **Przyciski:** START, STOP
- **Przełącznik:** 3-pozycyjny (SETUP – WYŁ – PRACA)
- **Buzzer:** Aktywny piezoelektryczny

> ℹ️ Przy wszystkich wejściach cyfrowych aktywowane są wewnętrzne pull-downy — zewnętrzne rezystory nie są potrzebne.

---

## 🔧 Hardware Requirements

- **MCU:** Heltec WiFi Kit 32 (ESP32) — V1, V2 or V3
- **Load cell:** HX711 module + strain gauge beam
- **Actuator:** Servo motor controlling the quetschhahn gate valve
- **Input:** Rotary encoder KY-040 or HW-040
- **Buttons:** START, STOP
- **Switch:** 3-position (SETUP – OFF – RUN)
- **Buzzer:** Active piezo buzzer

> ℹ️ Internal pull-downs are enabled on all digital inputs — no external resistors required.

---

## 📁 Struktura plików

```
hanimandl/
├── hani-mandl-PL.ino           # Główny plik programu
├── src/
│   ├── ota/
│   │   ├── OtaGithubClient.h   # Klient GitHub Releases
│   │   ├── OtaGithubClient.cpp
│   │   ├── OtaStateMachine.h   # Maszyna stanów OTA
│   │   └── OtaStateMachine.cpp
│   ├── storage/
│   │   ├── AppPreferences.h    # Zapis danych WiFi w NVS
│   │   └── AppPreferences.cpp
│   └── wifi/
│       ├── WifiConfigPortal.h  # Captive portal konfiguracyjny
│       └── WifiConfigPortal.cpp
└── README.md
```

---

## 📁 File Structure

```
hanimandl/
├── hani-mandl-PL.ino           # Main program file
├── src/
│   ├── ota/
│   │   ├── OtaGithubClient.h   # GitHub Releases HTTP client
│   │   ├── OtaGithubClient.cpp
│   │   ├── OtaStateMachine.h   # OTA state machine
│   │   └── OtaStateMachine.cpp
│   ├── storage/
│   │   ├── AppPreferences.h    # WiFi credentials NVS storage
│   │   └── AppPreferences.cpp
│   └── wifi/
│       ├── WifiConfigPortal.h  # WiFi config captive portal
│       └── WifiConfigPortal.cpp
└── README.md
```

---

## 🚀 Szybki start

### 1. Konfiguracja sprzętu

Przed wgraniem firmware ustaw wersję płytki Heltec w pliku `.ino`:

```cpp
#define HARDWARE_LEVEL 3   // 1 = Heltec V1 | 2 = Heltec V2 | 3 = Heltec V3 (domyślne)
```

### 2. Wymagane biblioteki

Zainstaluj przez Arduino Library Manager:

| Biblioteka | Źródło |
|---|---|
| `U8g2` | Arduino Library Manager |
| `HX711 Arduino Library` | Bogdan Necula, Andreas Motl |
| `ESP32Servo` | Arduino Library Manager |
| `Preferences` | Wbudowana w ESP32 BSP |

### 3. Kalibracja wagi

Po pierwszym uruchomieniu:

1. Przestaw przełącznik na **SETUP**
2. Wybierz **Kalibracja** enkoderem → naciśnij enkoder (OK)
3. Opróżnij wagę → naciśnij OK
4. Ustaw masę odważnika (domyślnie 500 g) → połóż odważnik → naciśnij OK

### 4. Ustawianie tary

1. W SETUP wybierz **Tara**
2. Wybierz rozmiar słoika enkoderem
3. Postaw pusty słoik na wadze → naciśnij OK

---

## 🚀 Quick Start

### 1. Hardware setup

Before flashing, set the Heltec board version in the `.ino` file:

```cpp
#define HARDWARE_LEVEL 3   // 1 = Heltec V1 | 2 = Heltec V2 | 3 = Heltec V3 (default)
```

### 2. Required libraries

Install via Arduino Library Manager:

| Library | Source |
|---|---|
| `U8g2` | Arduino Library Manager |
| `HX711 Arduino Library` | Bogdan Necula, Andreas Motl |
| `ESP32Servo` | Arduino Library Manager |
| `Preferences` | Built-in ESP32 BSP |

### 3. Scale calibration

After first boot:

1. Move the switch to **SETUP**
2. Select **Kalibracja** with the encoder → press encoder (OK)
3. Empty the scale → press OK
4. Set calibration weight with encoder (default 500 g) → place weight → press OK

### 4. Jar tare

1. In SETUP select **Tara**
2. Select jar size with encoder
3. Place empty jar on scale → press OK

---

## ⚙️ Opcje konfiguracji kodu

```cpp
#define HARDWARE_LEVEL 3          // Wersja płytki: 1, 2 lub 3
#define ROTARY_SCALE 2            // KY-040 = 2 | HW-040 = 1
#define USE_ROTARY                // Enkoder obrotowy (domyślne)
// #define USE_POTI               // Potencjometr zamiast enkodera
// #define SERVO_ERWEITERT        // Stary sprzęt z potencjometrem
// #define QUETSCHHAHN_LINKS      // Kran lewostronny — odwraca kierunek servo
// #define FEHLERKORREKTUR_WAAGE  // Korekcja skoków odczytu wagi
#define MAXIMALGEWICHT 1500       // Maksymalna mierzona masa (g)
```

---

## ⚙️ Code Configuration Options

```cpp
#define HARDWARE_LEVEL 3          // Board version: 1, 2 or 3
#define ROTARY_SCALE 2            // KY-040 = 2 | HW-040 = 1
#define USE_ROTARY                // Rotary encoder (default)
// #define USE_POTI               // Potentiometer instead of encoder
// #define SERVO_ERWEITERT        // Old hardware with potentiometer
// #define QUETSCHHAHN_LINKS      // Left-side valve — reverses servo direction
// #define FEHLERKORREKTUR_WAAGE  // Weight reading spike correction
#define MAXIMALGEWICHT 1500       // Maximum measured weight (g)
```

---

## 📡 Moduł OTA — aktualizacje przez WiFi

Moduły `OtaGithubClient`, `OtaStateMachine`, `AppPreferences` i `WifiConfigPortal` umożliwiają bezprzewodową aktualizację firmware bezpośrednio z GitHub Releases.

**Proces aktualizacji:**

1. Urządzenie tworzy punkt dostępowy Wi-Fi (AP)
2. Połącz się z AP telefonem lub komputerem
3. Otwórz przeglądarkę → wpisz dane sieci WiFi → zapisz
4. Urządzenie łączy się z siecią i sprawdza dostępność nowej wersji na GitHub
5. Jeśli dostępna — naciśnij **START** aby zainstalować aktualizację
6. Po instalacji urządzenie restartuje się automatycznie

---

## 📡 OTA Module — WiFi Updates

The `OtaGithubClient`, `OtaStateMachine`, `AppPreferences` and `WifiConfigPortal` modules enable wireless firmware updates directly from GitHub Releases.

**Update process:**

1. The device creates a Wi-Fi access point (AP)
2. Connect to the AP from your phone or PC
3. Open a browser → enter WiFi network credentials → save
4. The device connects to the network and checks GitHub for a new release
5. If available — press **START** to install the update
6. After installation the device restarts automatically

---

## 👥 Autorzy

Projekt HaniMandl powstał dzięki współpracy społeczności pszczelarzy w grupie Facebook **„Imkerei und Technik. Eigenbau"**.

| Autor | Wkład |
|---|---|
| **Marc Vasterling** | Twórca oryginalnej wersji (2018) |
| **Marc Wetzel** | Refaktoryzacja i dokumentacja (2019) |
| **Clemens Gruber** | Adaptacja do Heltec WiFi Kit 32, wsparcie V3 (2019, 2023) |
| **Andreas Holzhammer** | Enkoder, autokorekta, servo, OTA i wiele innych (2020–2022) |
| **Marc Junker** | Obsługa enkodera rotary (2020) |
| **Johannes Kuder** | Licznik napełnień, buzzer (2020) |
| **Jeremias Bruker** | Konfigurowalne typy słoików, odwrotne servo (2020) |

---

## 👥 Authors

The HaniMandl project was built by the beekeeping community in the Facebook group **"Imkerei und Technik. Eigenbau"**.

| Author | Contribution |
|---|---|
| **Marc Vasterling** | Creator of the original version (2018) |
| **Marc Wetzel** | Refactoring and documentation (2019) |
| **Clemens Gruber** | Adaptation for Heltec WiFi Kit 32, V3 support (2019, 2023) |
| **Andreas Holzhammer** | Encoder, autocorrect, servo, OTA and many more (2020–2022) |
| **Marc Junker** | Rotary encoder support (2020) |
| **Johannes Kuder** | Fill counter, buzzer (2020) |
| **Jeremias Bruker** | Configurable jar types, reversed servo (2020) |

---

## 📜 Licencja

```
Copyright (C) 2018–2023  Marc Vasterling, Marc Wetzel, Clemens Gruber,
                          Marc Junker, Andreas Holzhammer, Johannes Kuder,
                          Jeremias Bruker

Ten program jest wolnym oprogramowaniem: możesz go redystrybuować
i/lub modyfikować na warunkach Powszechnej Licencji Publicznej GNU
opublikowanej przez Free Software Foundation — w wersji 3 lub
(według uznania) dowolnej późniejszej wersji.

Program jest dystrybuowany w nadziei, że będzie użyteczny,
ale BEZ JAKIEJKOLWIEK GWARANCJI. Szczegóły w licencji GPL.
```

Niniejsze oprogramowanie jest wolnym oprogramowaniem na warunkach licencji **GNU General Public License v3**. Możesz je swobodnie używać, kopiować, modyfikować i dystrybuować — pod warunkiem zachowania tej samej licencji w pracach pochodnych oraz dołączenia pełnego kodu źródłowego. Oprogramowanie dostarczane jest **bez jakichkolwiek gwarancji**.

📄 Pełny tekst licencji: [https://www.gnu.org/licenses/gpl-3.0.html](https://www.gnu.org/licenses/gpl-3.0.html)

---

## 📜 License

```
Copyright (C) 2018–2023  Marc Vasterling, Marc Wetzel, Clemens Gruber,
                          Marc Junker, Andreas Holzhammer, Johannes Kuder,
                          Jeremias Bruker

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <https://www.gnu.org/licenses/>.
```

This software is free software licensed under the **GNU General Public License v3**. You are free to use, copy, modify and distribute it — provided that derivative works carry the same license and include the full source code. The software is provided **without any warranty of any kind**.

📄 Full license text: [https://www.gnu.org/licenses/gpl-3.0.html](https://www.gnu.org/licenses/gpl-3.0.html)

---

## 🔗 Linki

| | |
|---|---|
| 📦 **Oryginalne repozytorium** | [https://github.com/hiveeyes/hanimandl](https://github.com/hiveeyes/hanimandl) |
| 🌐 **Mellifer.pl** | [https://mellifer.pl](https://mellifer.pl) |
| 🐝 **PszczelaRzTechniczny.pl** | [https://www.pszczelarzтechniczny.pl](https://www.pszczelarzтechniczny.pl) |
| 📋 **GNU GPL v3** | [https://www.gnu.org/licenses/gpl-3.0.html](https://www.gnu.org/licenses/gpl-3.0.html) |

---

## 🔗 Links

| | |
|---|---|
| 📦 **Original repository** | [https://github.com/hiveeyes/hanimandl](https://github.com/hiveeyes/hanimandl) |
| 🌐 **Mellifer.pl** | [https://mellifer.pl](https://mellifer.pl) |
| 🐝 **PszczelaRzTechniczny.pl** | [https://www.pszczelarzтechniczny.pl](https://www.pszczelarzтechniczny.pl) |
| 📋 **GNU GPL v3** | [https://www.gnu.org/licenses/gpl-3.0.html](https://www.gnu.org/licenses/gpl-3.0.html) |

---

> *„Mój kod może każdy dowolnie używać, zmieniać i wgrywać gdzie chce, dopóki nie podpisuje go własnym imieniem."*
>
> — Marc Vasterling, twórca HaniMandl

---

> *"Anyone is free to use, modify and upload my code wherever they want, as long as they don't put their own name on it."*
>
> — Marc Vasterling, creator of HaniMandl
