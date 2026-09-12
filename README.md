# ⚡ Blitz-LightningTracker

A Home Assistant blueprint that sends rich notifications with maps when lightning strikes nearby. Designed specifically for the **Blitzortung Lightning Detector** integration.

## ✨ Features

- 🌩️ **Real-time lightning detection** - Get notified on every strike inside your chosen distance, not just the first one of a storm
- 🧭 **Distance + direction** - "Strike detected 4.2 km away to the NE in Springfield"
- 📍 **Interactive maps** - Tap the notification to open the strike in Google Maps
- 🗺️ **Google Maps integration** - Optional static map image, with your own device pinned next to the strike
- 📱 **Mobile notifications** - Any number of phones/tablets, plus your own notify groups
- ⚙️ **Highly configurable** - Distance, cooldown, Android channel/timeout, "only closer strikes"

## 📋 Prerequisites

### Required:
- **Home Assistant 2024.10 or newer** with the companion app installed
- [**Blitzortung Lightning Detector**](https://github.com/mrk-its/homeassistant-blitzortung) integration configured
  (or [the fork](https://github.com/zacharyd3/homeassistant-blitzortung), which can follow a device tracker instead of a fixed location)
- A Blitzortung **distance** sensor (e.g. `sensor.home_lightning_distance`)

### Optional (but recommended):
- **Azimuth sensor** (e.g. `sensor.home_lightning_azimuth`) - adds the compass direction to the message
- **Area sensor** - adds the place name to the message. Either:
  - `sensor.latest_lightning_strike_area` from the bundled [`sensors.yaml`](sensors.yaml), **or**
  - `sensor.<name>_lightning_area` if your Blitzortung integration provides one (the fork does)
- **`sensor.latest_lightning_strike_entity_id`** from [`sensors.yaml`](sensors.yaml) - only used as a fallback for the map coordinates if your distance sensor has no `lat`/`lon` attributes
- **`input_number.lightning_last_distance`** from [`input_number.yaml`](input_number.yaml) - only needed if you turn on *Only Notify for Closer Strikes*
- **A static map provider** - for map images in notifications. Tapping the notification opens Google Maps either way; this is only for the picture embedded in it.
  - **[Geoapify](https://myprojects.geoapify.com)** - no credit card needed, 3,000 credits/day free. Sign up, create a project, copy the API key.
  - **[Google Maps Static API](https://console.cloud.google.com/apis/credentials)** - enable "Maps Static API". Note that Google Cloud requires a billing account with a card on file even to stay inside the free tier, and prepaid cards are often rejected.
  - **Custom** - any other provider, via a URL template (see below).

> **Units:** the blueprint uses whatever unit your distance sensor reports. If Home Assistant is set to imperial, the sensor reports miles, so **Maximum Distance** is in miles too.

## 🚀 Installation

### Method 1: Direct Import (Easiest)
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fzacharyd3%2FBlitz-LightningTracker%2Fblob%2Fmain%2Flightning_tracker.yaml)

   
[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://buymeacoffee.com/zacharyd3)

2. **Add the helper sensors** (skip the area/entity-id sensors if your integration already provides an area sensor):

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

3. **Edit the `User-Agent`** in the REST sensor to something that identifies you - Nominatim blocks generic user agents
4. **Restart Home Assistant** to load the new sensors
5. **Configure the blueprint** with your Blitzortung sensors and mobile device

### Method 2: Manual Installation
1. Download [`lightning_tracker.yaml`](lightning_tracker.yaml)
2. Copy it to `/config/blueprints/automation/zacharyd3/lightning_tracker.yaml`
   (blueprints live in a per-author subfolder - creating `zacharyd3/` is part of the step)
3. Proceed to follow steps 2 - 5 above.

## 🔀 Using the custom integration fork

[My fork of the Blitzortung integration](https://github.com/zacharyd3/homeassistant-blitzortung) can follow a device tracker instead of a fixed location, and ships its own geocoded area sensor. If you run it, configure the blueprint like this:

| Setting | Value |
|---|---|
| **Lightning Area Sensor** | `sensor.<name>_lightning_area` (the integration's own) |
| **Latest Strike Entity ID Sensor** | leave empty - the distance sensor's `lat`/`lon` attributes cover the map |
| **Refresh Area Sensor Before Notifying** | off - that sensor updates itself |

You can then skip the REST sensor in [`sensors.yaml`](sensors.yaml) entirely. The only helper you might still want is [`input_number.yaml`](input_number.yaml), and only if you turn on *Only Notify for Closer Strikes*.

> This setup used to live on a separate `beta` branch. It doesn't need one any more - the three settings above are the whole difference - so that branch has been retired. Its final state is preserved on the [`archive/beta`](https://github.com/zacharyd3/Blitz-LightningTracker/tree/archive/beta) branch.

## ⚙️ Configuration

### Blitzortung sensors:
- **Lightning Distance Sensor** *(required)* - your Blitzortung distance sensor
- **Lightning Azimuth Sensor** - adds the compass direction to the message
- **Lightning Area Sensor** - adds the place name to the message
- **Latest Strike Entity ID Sensor** - fallback source for the map coordinates

### Notification:
- **Maximum Distance** - notify below this distance, in your distance sensor's unit (default: 7.5)
- **Notification Title** - default: "Lightning Strike Nearby!"
- **Mobile Devices** - pick any number of companion-app devices
- **Custom Notify Services** - comma-separated notify services, e.g. `notify.family_devices, notify.all_phones`

### Additional options:
- **Notification Channel / Timeout** - Android notification channel and auto-dismiss time
- **Include Map Image** - static map image in the notification
- **Static Map Provider** - `Google`, `Geoapify`, or `Custom`
- **Static Map API Key** - the key for whichever provider you picked
- **Custom Map URL Template** - only for `Custom`. Placeholders `{lat}`, `{lon}`, `{device_lat}`, `{device_lon}`, `{key}` are substituted before sending, e.g. `https://example.com/map?c={lat},{lon}&k={key}`
- **Show Device Location on Map** - pins your first selected device and zooms to fit both points
- **Only Notify for Closer Strikes** - suppress strikes that are further away than the last alert
- **Closer-Strike Reset** - how long a quiet period has to be before that comparison starts over (default: 30 min)
- **Last Distance Helper** - the `input_number` used for the comparison above
- **Refresh Area Sensor Before Notifying** - leave on for the bundled REST sensor, off if your area sensor self-updates
- **Cooldown Period** - minimum time between notifications (default: 1.5 min)

## 🔧 Blitzortung Integration Setup

If you haven't set up Blitzortung yet:

1. In Home Assistant: **Settings** > **Devices & Services** > **Add Integration**
2. Search for "Blitzortung Lightning Detector"
3. Configure with your location coordinates
4. Wait for sensors to populate with data

## 🛠️ Troubleshooting

### No notifications at all?
- Check **Settings > Automations > (your automation) > Traces** - the trace shows which condition stopped it
- Make sure at least one **Mobile Device** or **Custom Notify Service** is set. If neither is, the automation writes a warning to the log instead of notifying
- Custom notify services must exist. Check **Developer Tools > Actions** and search for `notify.`

### Notifications stop after the first strike?
- That was the old behaviour, caused by a `numeric_state` trigger only firing when the value *crosses* the threshold. The blueprint now triggers on every sensor update and filters by distance in a condition - re-import the blueprint if you are on an older copy

### "Only Notify for Closer Strikes" went quiet forever?
- It resets itself after the **Closer-Strike Reset** period (default 30 minutes without a notification), so a new storm always gets through. Lower that value if you want it to reset sooner

### REST Sensor Not Working?
- The area sensor only calls Nominatim when the blueprint asks it to, so it stays "unknown" until the first strike
- **Change the User-Agent** in the REST sensor to your name/project - Nominatim blocks generic agents
- Verify internet connection for OpenStreetMap Nominatim API calls
- If your integration provides its own area sensor, point the blueprint at that and delete the REST sensor

### No Map Images?
- Check that **Include Map Image** is on and that **Static Map Provider** matches the key you pasted - a Geoapify key in Google mode (or vice versa) fails silently, since the phone fetches the image and never reports the error back
- The map needs strike coordinates. These come from the `lat`/`lon` attributes of your distance sensor, or from `sensor.latest_lightning_strike_entity_id` - check both in **Developer Tools > States**
- Fastest way to debug: copy the `image` URL out of the automation trace (**Traces > Changed Variables > map_image_url**) and open it in a browser. The provider's error comes back as readable text
- Google only: confirm billing is enabled on the Cloud project and that it is the **Maps Static API** that's enabled, not Maps JavaScript or Maps Embed

### Wrong Device Selected?
- The blueprint builds the notify service from the device name as `notify.mobile_app_<slugified name>`
- If the device was renamed in Home Assistant, the companion app's service still uses the **original** name. Check **Developer Tools > Actions** for the real service name and use **Custom Notify Services** instead

### Sensors Not Found?
- Ensure the Blitzortung integration is working and its sensors have states
- Check **Developer Tools > States** for entities starting with `sensor.` and containing `lightning`

## 🤝 Contributing

Found a bug or want to add features? 

1. Fork this repository
2. Create a feature branch
3. Submit a pull request

## ⭐ If this helped you, please star this repo!

---

**Made with love for the Home Assistant community**
