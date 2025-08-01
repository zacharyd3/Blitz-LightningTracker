# ⚡ Blitz-LightningTracker

A Home Assistant blueprint that sends rich notifications with maps when lightning strikes nearby. Designed specifically for the **Blitzortung Lightning Detector** integration.

## ✨ Features

- 🌩️ **Real-time lightning detection** - Get notified when strikes occur within your specified distance
- 📍 **Interactive maps** - Static map images in notifications with yellow lightning-themed pins
- 🧭 **Direction awareness** - Shows which direction the lightning struck (North/South/East/West)
- 📱 **Mobile notifications** - Works with Home Assistant mobile app
- ⚙️ **Highly configurable** - Customize distance, cooldown, and notification content
- 🗺️ **Google Maps integration** - Optional static map images (requires API key)

## 📋 Prerequisites

### Required:
- **Home Assistant** with mobile app installed
- [**Blitzortung Lightning Detector**](https://github.com/mrk-its/homeassistant-blitzortung) integration configured
- The following Blitzortung sensors:
  - Distance sensor (e.g., `sensor.home_lightning_distance`)
  - Azimuth sensor (e.g., `sensor.home_lightning_azimuth`)
- The following Helper Sensors (see install instructions for a template):
  - Latest Lightning Entity ID sensor(e.g., `latest_lightning_strike_entity_id`)
  - Latitude sensor (e.g., `sensor.latest_lightning_strike_latitude`)
  - Longitude sensor (e.g., `sensor.latest_lightning_strike_longitude`)
  - Area sensor (e.g., `sensor.latest_lightning_strike_area`)
  - Last Strike Distance helper (e.g., `input_number.lightning_last_distance`)

### Optional:
- **Google Maps Static API key** - For map images in notifications
  - Get one at: https://console.cloud.google.com/apis/credentials
  - Enable "Maps Static API"
  - Free tier should be more than enough

## 🚀 Installation

### Method 1: Direct Import (Easiest)
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fzacharyd3%2FBlitz-LightningTracker%2Fblob%2Fmain%2Flightning_tracker.yaml)

   
[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://buymeacoffee.com/zacharyd3)

2. **Add Required Sensors** (choose your method):

   **📁 If you have a separate `sensors.yaml` file:**
   - Download [`sensors.yaml`](sensors.yaml) 
   - Copy the contents and add them to your existing `sensors.yaml` file
   - Download [`input_number.yaml`](input_number.yaml)
   - Copy the contents and add it to your existing `input_number.yaml` file

   **⚙️ If you use `configuration.yaml` with existing yaml sensors:**
   - Download [`sensors.yaml`](sensors.yaml)
   - Copy the contents and add them under your existing `sensor:` section in `configuration.yaml`
   - Download [`input_number.yaml`](input_number.yaml)
   - Copy the contents and add them under your existing `input_number:` section in `configuration.yaml`

   **🆕 If you've never added YAML sensors before:**
   - Download [`configuration.yaml`](configuration.yaml)
   - Copy the contents and add them to the end of your existing `configuration.yaml`

3. **Restart Home Assistant** to load the new sensors
4. **Configure the blueprint** with your Blitzortung sensors and mobile device

### Method 2: Manual Installation
1. Download [`lightning_tracker.yaml`](lightning_tracker.yaml)
2. Copy to `/config/blueprints/automation/lightning_notification.yaml`
3. Proceed to follow steps 2 - 4 above.

### Method 3: Beta Installation
**!! [ONLY USE THE BETA IF YOU'VE ALSO UPDATED TO MY INTEGRATION](https://github.com/zacharyd3/homeassistant-blitzortung) !!**
<details>
  <summary>I'm aware this is a beta build and I've updated</summary>
  
[Check the beta branch for installation instructions](https://github.com/zacharyd3/Blitz-LightningTracker/tree/beta)
</details>

## ⚙️ Configuration

### Required Settings:
- **Lightning Distance Sensor** - Your Blitzortung distance sensor
- **Lightning Azimuth Sensor** - Your Blitzortung direction sensor
- **Lightning Area Sensor** - Sensor showing strike location name (Provided in sensors.yaml)
- **Lightning Latitude Sensor** - Strike latitude coordinates (Provided in sensors.yaml)
- **Lightning Longitude Sensor** - Strike longitude coordinates (Provided in sensors.yaml)
- **Mobile Device** - Select from dropdown of mobile app devices
- **Maximum Distance** - Distance in km to trigger notifications (default: 7.5km)

### Optional Settings:
- **Google Maps API Key** - For static map images
- **Include Map Image** - Toggle map images on/off
- **Cooldown Period** - Minutes between notifications (default: 1.5 min)

## 🔧 Blitzortung Integration Setup

If you haven't set up Blitzortung yet:

1. In Home Assistant: **Settings** > **Devices & Services** > **Add Integration**
2. Search for "Blitzortung Lightning Detector"
3. Configure with your location coordinates
4. Wait for sensors to populate with data

## 🛠️ Troubleshooting

### REST Sensor Not Working?
- Check that the coordinate sensors have valid data (not 0 or unknown)
- Verify internet connection for OpenStreetMap Nominatim API calls
- The area sensor updates every hour (`scan_interval: 3600`) to avoid API rate limits
- **Change the User-Agent** in the REST sensor to your name/project

### Missing Sensors Error?
- **Most common issue!** Make sure you've created the additional sensors in `configuration.yaml`
- Restart Home Assistant after adding sensors
- Check **Developer Tools > States** to verify sensors exist and have data

### Sensors Have Different Names?
- Your Blitzortung sensors might have different names than the examples
- Check **Developer Tools > States** for entities starting with `sensor.blitzortung_`
- Update the template sensors to match your actual entity names

### No Map Images?
- Verify your Google Maps API key is correct
- Ensure "Maps Static API" is enabled in Google Cloud Console
- Check that **Include Map Image** is enabled in blueprint config

### Wrong Device Selected?
- The blueprint auto-generates the notification service from your device selection
- Make sure your mobile device has the Home Assistant app installed and configured

### Sensors Not Found?
- Ensure Blitzortung integration is working and sensors have states
- Check sensor names match the pattern: `sensor.*_lightning_*`

## 🤝 Contributing

Found a bug or want to add features? 

1. Fork this repository
2. Create a feature branch
3. Submit a pull request

## ⭐ If this helped you, please star this repo!

---

**Made with love for the Home Assistant community**
