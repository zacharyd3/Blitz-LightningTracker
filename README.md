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
- **Blitzortung Lightning Detector** integration configured
- The following Blitzortung sensors:
  - Distance sensor (e.g., `sensor.home_lightning_distance`)
  - Azimuth sensor (e.g., `sensor.home_lightning_azimuth`)
- The following Helper Sensors (see github for templates):
  - Latitude sensor (e.g., `sensor.latest_lightning_strike_latitude`)
  - Longitude sensor (e.g., `sensor.latest_lightning_strike_longitude`)
  - Area sensor (e.g., `sensor.latest_lightning_strike_area`)

### Optional:
- **Google Maps Static API key** - For map images in notifications
  - Get one at: https://console.cloud.google.com/apis/credentials
  - Enable "Maps Static API"
  - Free tier should be more than enough

## 🚀 Installation

### Method 1: Direct Import (Easiest)
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fzacharyd3%2FBlitz-LightningTracker%2Fblob%2Fmain%2Flightning_tracker.yaml)

### Method 2: Manual Installation
1. Download [`lightning_notification.yaml`](lightning_notification.yaml)
2. Copy to `/config/blueprints/automation/lightning_notification.yaml`
3. Restart Home Assistant or reload automations
4. Find the blueprint in **Settings** > **Automations & Scenes** > **Blueprints**

## ⚙️ Configuration

### Required Settings:
- **Lightning Distance Sensor** - Your Blitzortung distance sensor
- **Lightning Azimuth Sensor** - Your Blitzortung direction sensor
- **Lightning Area Sensor** - Sensor showing strike location name
- **Lightning Latitude Sensor** - Strike latitude coordinates
- **Lightning Longitude Sensor** - Strike longitude coordinates
- **Mobile Device** - Select from dropdown of mobile app devices
- **Maximum Distance** - Distance in km to trigger notifications (default: 7.5km)

### Optional Settings:
- **Google Maps API Key** - For static map images (secure password field)
- **Notification Title** - Customize notification title
- **Include Map Image** - Toggle map images on/off
- **Cooldown Period** - Minutes between notifications (default: 1.5 min)

## 🛠️ Troubleshooting

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
