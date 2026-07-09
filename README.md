# Tuya-TS004F-Volume-Blueprint
Tuya ERS-10TZBVK-AA rotary knob ZHA volume control blueprint

# Tuya-TS004F-Volume-Blueprint

Tuya ERS-10TZBVK-AA rotary knob ZHA volume control blueprint for Home Assistant.

This blueprint allows you to easily bind a Tuya TS004F wireless rotary knob (commonly sold under various white-label brands) to your media players. It natively decodes the Zigbee (`zha_event`) payloads to provide smooth, scaled volume control without needing complex Node-RED flows or raw YAML templating.

## ✨ Features

* **Standard Rotation:** Controls the volume of your Main Speaker.
* **Pressed Rotation:** Controls the volume of an optional Secondary Speaker.
* **Adjustable Sensitivity:** Includes a scaling parameter to easily fine-tune how much the volume changes per click of the dial.
* **Safety Limits:** Built-in math caps the volume at 100% and floors it at 0% to prevent Home Assistant API errors.

## 📋 Prerequisites

1. **Home Assistant** (Core)
2. **ZHA (Zigbee Home Automation)** integration active and running.
3. A paired **Tuya TS004F** device (Manufacturer: `_TZ3000_qja6nq5z`).

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
| **Main Speaker** | Required | The `media_player` entity you want to control with standard left/right rotations. |
| **Secondary Speaker** | Optional | The `media_player` entity you want to control when pressing down and rotating the knob. |
| **Step Size Scaling** | Required | A multiplier for the volume step. By default (`1.0`), a physical rotation distance of 13 equates to a 1% volume change. Increase this number (e.g., `2.0` or `5.0`) to make the volume change faster with less turning. |

## 🛠️ How it Works under the Hood

Unlike standard UI device triggers that strip away Zigbee parameters, this blueprint listens directly to the `zha_event` bus. 
* It intercepts the `step` command for standard rotation and the `step_color_temp` command for pressed rotation. 
* It extracts the `step_mode` (direction) and `step_size` (distance), applies your custom scaling fraction, and safely pushes the new volume state to your target media player.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.