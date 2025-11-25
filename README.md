# homeassistant-blueprints
Blueprints i made for my Home Assistant

# AWTRIX Blueprints
Each of them can take multiples AWTRIX display as output.

## AWTRIX - Linky
Consist of two page (one for last day kWh, one for last day cost)
I am using myElectricalData entity provided via MQTT by [myelectricaldata_import](https://github.com/MyElectricalData/myelectricaldata_import)
(It provides the cost via attributes)

Need icons 42453 & 53070
![awtrix linky con](https://github.com/DrumSlayers/homeassistant-blueprints/assets/4050278/45426c94-9191-42c2-b8e4-b8cb712a0789)
![awtrix linky cost](https://github.com/DrumSlayers/homeassistant-blueprints/assets/4050278/f4b84bc7-2fc9-4a57-b2b7-ffeeb5180289)

## AWTRIX - Temp & Hum
Consist of two page (one for °c, one for % hum)
Take simple temp & hum sensors as input

Need icons 5836 & 2423
![awtrix temp](https://github.com/DrumSlayers/homeassistant-blueprints/assets/4050278/c0d90b28-74fd-4b3c-91da-78bf0c29b9b2)
![awtrix hum](https://github.com/DrumSlayers/homeassistant-blueprints/assets/4050278/1eb54b62-0979-4027-97ff-2a88dd81935f)

## AWTRIX Naolib Transport Next Tram/Bus Display
Displays Naolib (Nantes) waiting times for a specific stop and for each lines you want on your Awtrix device.

Publish a topic per line
![awtrix (1)](https://github.com/user-attachments/assets/99df84b3-777d-4806-96e5-539e39b35a82)

**Features:**
*   🚌 **Multi-Page:** Cycles through lines automatically (one page per line).
*   ⏱️ **Smart Filter:** Shows only the **next** bus for each destination.
*   🌈 **Visual Alerts:** Rainbow text when bus is "Proche" (close).
*   ⚠️ **Stale Data:** Shows a red bar if data isn't updated for 2 minutes.
*   🚊 **Per Line Icon:** JSON Configuration to set a specific icon for each transport lines/routes.

#### 📋 How To Setup
##### 1. Create the Sensor
You **must** add a `command_line` sensor to your `configuration.yaml`.
Replace `HAUB` with your stop code (find it via https://open.tan.fr/ewp/arrets.json/).

```yaml
command_line:
  - sensor:
      name: Naolib Haubans
      unique_id: naolib_haubans
      # Fetch JSON and transform array to object with keys "0", "1", etc.
      command: 'curl -s https://open.tan.fr/ewp/tempsattente.json/HAUB | python3 -c "import sys, json; print(json.dumps({str(i):x for i,x in enumerate(json.load(sys.stdin))}))"'
      value_template: "OK"
      scan_interval: 60
      json_attributes:
        - "0"
        - "1"
        - "2"
        - "3"
        - "4"
        - "5"
        - "6"
        - "7"
        - "8"
        - "9"
```

##### 2. Reload & Configure
1. Reload YAML configuration in Home Assistant.
2. Create this automation from the blueprint.
3. Select the sensor (e.g., `sensor.naolib_haubans`).
4. Configure Line Icons (see below).

##### 3. Icon Configuration (JSON)
In the "Line Icons" field, map your transport lines to Awtrix Icon IDs.

**Example:**
```json
{
  "5": "ligne-5",
  "C3": "ligne-c3",
  "default": "1309"
}
```
I've joined the two icons i use for reference in the icon/ folder
