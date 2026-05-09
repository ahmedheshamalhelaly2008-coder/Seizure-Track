# Seizure-Track 🧠⚡

> Smart wearable system that detects epileptic seizures and sends real-time alerts to doctors and family members.

---

## What It Does

Seizure-Track is a wearable device worn on the wrist that monitors a patient's motion and heart rate. When a seizure is detected, it:

- Triggers a **local buzzer** to alert nearby people
- Sends a **WhatsApp message** to the family or doctor instantly
- Logs **accelerometer + BPM data** to a Google Sheet for medical tracking
- Monitors **battery level** and signals when charging is needed

---

## Hardware Required

| Component | Purpose |
|-----------|---------|
| ESP32 | Main microcontroller + WiFi |
| MPU-6050 | Motion / accelerometer sensor |
| Heart rate sensor | BPM monitoring |
| Buzzer | Local audio alert |
| LED (x2) | Power indicator + low battery indicator |
| LiPo battery | Portable power |
| 3D-printed case | Housing |

---

## How It Works

```
Patient wears device
       ↓
MPU-6050 reads acceleration every second
       ↓
Total acceleration > 3.4g?
       ↓ YES
Buzzer ON → WhatsApp alert → Log to Google Sheet
       ↓ (when motion returns to normal)
Buzzer OFF → WhatsApp "Returned Normal" → Log to Google Sheet
```

---

## Setup

### 1. Install Libraries (Arduino IDE)

- `WiFi.h` — built-in with ESP32
- `HTTPClient.h` — built-in with ESP32
- `UrlEncode` — install via Library Manager
- `Wire.h` — built-in

### 2. Configure Your Credentials

Open `wearable/wearable.ino` and fill in your details:

```cpp
const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";
String phoneNumber = "+YOUR_PHONE_NUMBER";  // include country code
String apiKey = "YOUR_CALLMEBOT_API_KEY";
const String GOOGLE_SCRIPT_ID = "YOUR_GOOGLE_APPS_SCRIPT_URL";
```

### 3. Set Up WhatsApp Alerts (CallMeBot)

1. Add the number **+34 644 71 75 17** to your WhatsApp contacts
2. Send this message: `I allow callmebot to send me messages`
3. You'll receive your API key — paste it into `apiKey`

### 4. Set Up Google Sheets Logging

1. Create a new Google Sheet
2. Go to **Extensions → Apps Script**
3. Deploy a Web App that accepts `accel`, `bpm`, and `alert` as URL parameters
4. Paste the deployed URL into `GOOGLE_SCRIPT_ID`

### 5. Upload to ESP32

- Select board: `ESP32 Dev Module`
- Upload `wearable/wearable.ino`

---

## Pin Configuration

| Pin | Connected To |
|-----|-------------|
| 26 | Buzzer |
| 14 | Power LED |
| 27 | Charging LED |
| 25 | Heart rate sensor (analog) |
| 4 | Battery voltage divider |
| SDA/SCL | MPU-6050 (I2C) |

---

## Adjusting Sensitivity

The seizure detection threshold is set in the code:

```cpp
const float PANIC_THRESHOLD = 3.4;  // in g-force
```

- **Lower value** → more sensitive (may cause false alarms)
- **Higher value** → less sensitive (may miss mild seizures)
- Recommended range: `2.5` – `4.0` depending on patient

---

## Project Structure

```
seizure-track/
├── wearable/
│   └── wearable.ino    # Main ESP32 firmware
└── README.md
```

---

## Results

- **92%** detection accuracy during testing
- Successfully detected seizure-style tremors while ignoring normal movements (walking, adjusting position)
- Real-time alerts delivered in under 3 seconds

---

## Built With

- [ESP32](https://www.espressif.com/) — microcontroller
- [MPU-6050](https://invensense.tdk.com/) — 6-axis motion sensor
- [CallMeBot](https://www.callmebot.com/) — WhatsApp notification API
- [Google Apps Script](https://script.google.com/) — data logging

---

## Team

| Name | Role |
|------|------|
| Mariam Hesham | Software & Hardware |

---

## License

This project is open source and available under the [MIT License](LICENSE).
