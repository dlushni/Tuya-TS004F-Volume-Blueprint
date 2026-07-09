# Tuya-TS004F-Volume-Blueprint
Tuya ERS-10TZBVK-AA rotary knob ZHA volume control blueprint

# Tuya-TS004F-Volume-Blueprint

Tuya ERS-10TZBVK-AA rotary knob ZHA volume control blueprint for Home Assistant.

This blueprint allows you to easily bind a Tuya TS004F wireless rotary knob (commonly sold under various white-label brands) to your media players. It natively decodes the Zigbee (`zha_event`) payloads to provide smooth, scaled volume control without needing complex Node-RED flows or raw YAML templating.

## ✨ Features

* **Short Click:** Toggles play/pause for a designated media player.
* **Standard Rotation:** Controls the volume of your main speaker.
* **Pressed Rotation:** Controls the volume of an optional secondary speaker.
* **Smart Acceleration:** Slow, deliberate turns provide precise 1% volume adjustments. Fast spins multiply the excess distance to allow for rapid volume changes.
* **Safety Limits:** Built-in math caps the volume at 100% and floors it at 0% to prevent Home Assistant API errors.

## 📋 Prerequisites

1. **Home Assistant** (Core)
2. **ZHA (Zigbee Home Automation)** integration active and running.
3. A paired **Tuya TS004F** device (Manufacturer: `_TZ3000_qja6nq5z`).
4. Tuya knob must be in **Dimmer Mode** for the rotation events to function correctly (tripple click to switch modes).

## 🚀 Installation

1. In Home Assistant, navigate to **Settings** > **Automations & Scenes** > **Blueprints**.
2. Click **Import Blueprint** in the bottom right.
3. Paste the URL of the `ts004f_volume_knob.yaml` file from this repository.
4. Click **Preview Blueprint** and then **Import Blueprint**.

*Alternatively, you can manually copy the `ts004f_volume_knob.yaml` file into your Home Assistant `config/blueprints/automation/` directory and restart Home Assistant or reload your automations.*

## ⚙️ Configuration Parameters

When creating an automation from this blueprint, you will be prompted to configure the following:

| Parameter | Type | Description |
| :--- | :--- | :--- |
| **Controller** | Required | The Tuya TS004F device in your ZHA integration generating the events. |
| **Main Volume (Speaker)** | Required | The `media_player` entity you want to control with standard left/right rotations. |
| **Secondary Volume (Player)** | Optional | The `media_player` entity you want to control when pressing down and rotating the knob. |
| **Play/Pause Player** | Required | The `media_player` entity to toggle (play/pause) when the knob is short-clicked. |
| **Acceleration Multiplier** | Required | Scales large rotations. A standard base turn is always exactly a 1% volume change. Any rotation distance *beyond* the base turn is multiplied by this number. |

## 🛠️ How it Works under the Hood

Unlike standard UI device triggers that strip away Zigbee parameters, this blueprint listens directly to the `zha_event` bus. 
* It intercepts the physical action and routes it to one of three independent sequences: `toggle`, `step` (rotate), or `step_color_temp` (press & rotate).
* For rotations, it extracts the `step_mode` (direction) and `step_size` (distance). 
* It applies a piecewise template to the distance, separating the base turn from the excess speed, applies your custom scaling fraction, and safely pushes the new volume state to your target media player.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
