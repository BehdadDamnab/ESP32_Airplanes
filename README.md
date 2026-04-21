# ESP32 Airplanes ✈️

A real-time **flight tracking system** built for the ESP32 microcontroller.  
Connects to the **OpenSky Network API** to fetch live aircraft data within a custom geofenced area, and sends **Telegram notifications** when planes are detected overhead.

---

## Features

- **Live Flight Data** — fetches real-time aircraft vectors from the OpenSky Network API
- **Geofencing** — define a bounding box around your location to monitor only nearby aircraft
- **Telegram Notifications** — get alerts sent directly to your Telegram when a plane enters your zone
- **Dual-Core** — uses both ESP32 cores for simultaneous data fetching and processing
- **Efficient JSON Parsing** — optimized for embedded systems using `ArduinoJson`

---

## Hardware Requirements

| Component | Details |
|-----------|---------|
| Microcontroller | ESP32 Development Board |
| Connectivity | 2.4GHz Wi-Fi |

---

## How It Works

```
ESP32 connects to Wi-Fi
        │
        └── HTTP GET → OpenSky API (bounding box)
                │
                └── parse JSON response
                        │
                        ├── aircraft inside zone? → Telegram notification ✈️
                        └── log flight data (callsign, altitude, heading)
```

---

## Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/BehdadDamnab/ESP32_Airplanes.git
cd ESP32_Airplanes
```

### 2. Create `project_config.h`

Create a file named `project_config.h` inside the project folder and fill in your credentials:

```c
#define MY_SSID         "Your_WiFi_Name"
#define MY_PASSWORD     "Your_WiFi_Password"

// Your home location (latitude / longitude)
#define MY_HOME_LAT     00.0000
#define MY_HOME_LON     00.0000

// Bounding box — roughly 50km around your location
#define LAT_MIN         "00.00"
#define LAT_MAX         "00.00"
#define LON_MIN         "00.00"
#define LON_MAX         "00.00"

// Telegram bot credentials
#define TELEGRAM_TOKEN   "your_bot_token"
#define TELEGRAM_CHAT_ID "your_chat_id"
```

> ⚠️ **Never commit `project_config.h` to GitHub** — it contains your private credentials. It is already listed in `.gitignore`.

### 3. Flash to ESP32

Use **ESP-IDF** to build and flash:

```bash
idf.py build
idf.py flash
idf.py monitor
```

---

## Setting Up Telegram Notifications

1. Open Telegram and search for **@BotFather**
2. Send `/newbot` and follow the steps to create a bot
3. Copy the **token** → paste into `TELEGRAM_TOKEN`
4. Message your bot, then visit:
   `https://api.telegram.org/bot<TOKEN>/getUpdates`
   to find your **chat ID** → paste into `TELEGRAM_CHAT_ID`

---

## Finding Your Bounding Box

To find the coordinates for your bounding box (~50km around your location):

1. Go to [boundingbox.klokantech.com](https://boundingbox.klokantech.com)
2. Draw a box around your area
3. Copy the `LAT_MIN`, `LAT_MAX`, `LON_MIN`, `LON_MAX` values

---

## OpenSky Network API

This project uses the free [OpenSky Network API](https://opensky-network.org/):

```
GET https://opensky-network.org/api/states/all?lamin=LAT_MIN&lomin=LON_MIN&lamax=LAT_MAX&lomax=LON_MAX
```

Returns all aircraft within the bounding box with fields including callsign, altitude, velocity, and heading.

> The free API has a rate limit — do not poll more than once every 10 seconds.

---

## Project Structure

```
ESP32_Airplanes/
├── ESP32_Airplanes/     # Main source code
├── .gitignore           # Excludes project_config.h and build files
├── .gitattributes
└── README.md
```

---

## Dependencies

- [ESP-IDF](https://docs.espressif.com/projects/esp-idf/en/latest/) — Espressif IoT Development Framework
- [ArduinoJson](https://arduinojson.org/) — JSON parsing for embedded systems
- [OpenSky Network API](https://opensky-network.org/apidoc/) — free real-time flight data

---

## License

Open-source — feel free to use, modify, and contribute.

---

## Author

**Behdad Damnab** — [github.com/BehdadDamnab](https://github.com/BehdadDamnab)
