# ⚡ Blitz-LightningTracker

A Home Assistant blueprint that sends rich notifications with maps when lightning strikes nearby. Designed specifically for the **Blitzortung Lightning Detector** integration.

## ✨ Features

- 🌩️ **Real-time lightning detection** - Get notified when strikes occur within your specified distance
- 📍 **Interactive maps** - Static map images in notifications with yellow lightning-themed pins
- 📱 **Mobile notifications** - Works with Home Assistant mobile app and allows for multiple targets
- ⚙️ **Highly configurable** - Customize distance, cooldown, and notification content
- 🗺️ **Google Maps integration** - Optional static map images (requires API key)

## 📋 Prerequisites

### Required:
- **Home Assistant** with mobile app installed
- [**My Custom Blitzortung Lightning Detector**](https://github.com/zacharyd3/homeassistant-blitzortung) integration configured
- The following Blitzortung sensors:
  - Distance sensor (e.g., `sensor.home_lightning_distance`)
  - Azimuth sensor (e.g., `sensor.home_lightning_azimuth`)
  - Area sensor (e.g., `sensor.home_lightning_area`)
- The following Helper Sensors (see install instructions for a template):
  - Latest Lightning Entity ID sensor(e.g., `latest_lightning_strike_entity_id`)
  - Last Strike Distance helper (e.g., `input_number.lightning_last_distance`)

### Optional:
- **Google Maps Static API key** - For map images in notifications
  - Get one at: https://console.cloud.google.com/apis/credentials
  - Enable "Maps Static API"
  - Free tier should be more than enough

## 🚀 Installation

**!! [ONLY USE THE BETA IF YOU'VE ALSO UPDATED TO MY INTEGRATION](https://github.com/zacharyd3/homeassistant-blitzortung) !!**
  
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fzacharyd3%2FBlitz-LightningTracker%2Fblob%2Fmain%2Flightning_tracker_beta.yaml)

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

## ⚙️ Configuration

### Required Settings:
- **Lightning Distance Sensor** - Your Blitzortung distance sensor
- **Lightning Area Sensor** - Sensor showing strike location name (Provided in sensors.yaml)
- **Mobile Device** - Select from dropdown of mobile app devices
- **Maximum Distance** - Distance in km to trigger notifications (default: 7.5km)
- **Notification Method** - Choose how to send notifications (Single Device, Multiple Devices, Custom Notify Service)
- **Notification Title** - Title for the notification

### Optional Settings:
- **Google Maps API Key** - For static map images
- **Include Map Image** - Toggle map images on/off
- **Cooldown Period** - Minutes between notifications (default: 1.5 min)

## 🔧 Blitzortung Integration Setup

If you haven't set up Blitzortung yet:

1. Install [my custom version of the Blitzortung Integration](https://github.com/zacharyd3/homeassistant-blitzortung)
2. Configure with your location coordinates
3. Wait for sensors to populate with data

## 🛠️ Troubleshooting

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

## 🤝 [Contributing](https://buymeacoffee.com/zacharyd3)

Found a bug or want to add features? 

1. Fork this repository
2. Create a feature branch
3. Submit a pull request

## ⭐ If this helped you, please star this repo!

---

**Made with love for the Home Assistant community**
