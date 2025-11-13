# Auto Repository installation

[![Open your Home Assistant instance and show the add add-on repository dialog with a specific repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https://github.com/bob-tm/ha-devireg-mqtt-addon)

## Manul Repository installation

Press the button, or
1. Navigate to addons page on your HA instalaiton
2. Press three dots in the top right corner
3. Add repository `https://github.com/bob-tm/ha-devireg-mqtt-addon` to addon repositories
4. Search for `Devismart MQTT`, HA need several minutes to update list of addons. 
5. Install addon from addons page


# Devireg Mqtt Home Assistant add-on repository
Addon uses latest code from <https://github.com/bob-tm/ha-devi-mqtt> (my fork from original <https://github.com/igor-podpalchenko/ha-devi-mqtt>) 

See https://github.com/bob-tm/ha-devireg-mqtt-addon/blob/main/devismart-mqtt/DOCS.md for details

Tested on RPI4

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]
![Supports armhf Architecture][armhf-shield]
![Supports armv7 Architecture][armv7-shield]
![Supports i386 Architecture][i386-shield]
 -->

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[armhf-shield]: https://img.shields.io/badge/armhf-yes-green.svg
[armv7-shield]: https://img.shields.io/badge/armv7-yes-green.svg
[i386-shield]: https://img.shields.io/badge/i386-yes-green.svg


# Manual Template Editing
You can override default template sensors for you confg. For example cahnge climate presets, logic or simply rename sensors.

copy folder ```auto-discovery-templates``` 

from https://github.com/bob-tm/ha-devi-mqtt

to ```\\192.168.xxx.x\addon_configs\xxxxxx_devismart-mqtt\auto-discovery-templates```

than you can direcly edit json files inside this folder. Local ```auto-discovery-templates``` has more priority over build in addon. It will survive addon updates. 

To apply changes just restart Devismart MQTT addon
