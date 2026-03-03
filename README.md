# pycupra_to_MagicMirror
Display some Information about my Cupra on my Magic Mirror

## 1. Abhängigkeiten

![Node Red](https://img.shields.io/badge/Node--Red-8F0000?style=flat&logo=nodered&logoColor=white) Vorhandene Node-Red Installation<br>
![Mosquitto](https://img.shields.io/badge/mosquitto-%233C5280.svg?style=flat&logo=eclipsemosquitto&logoColor=white) Mosquitto für MQTT<br>
Ausführbare [pycupra Installation](https://github.com/SteitzTo/pycupra_manual_de) (getestet mit Version 0.2.14)<br>
[Magic Mirror Installation](https://github.com/MagicMirrorOrg/MagicMirror)<br>
[MQTT Modul für MagicMirror](https://github.com/ottopaulsen/MMM-MQTT)<br>
<br>
## 2. Node-Red Nodes

[Cron-plus](https://flows.nodered.org/node/node-red-contrib-cron-plus)<br>

## 3. Anzeigen auf dem Magic Mirror 

Fahrzeug: Parkt / Fährt <br>
Status: Verschlossen/grün   Offen/rot<br>
Restreichweite in km<br>
Tankinhalt in % (Bei unter 20% orange, bei unter 5% rot blinkend)<br>
Ölservice in Tagen / KM<br>
Inspektion in Tagen / KM (Bei unter 30 Tagen / 1500 km orange)<br>
Fahrzeugstandort über OpenStreetmap
<br>
<br>

<img width="740" height="970" alt="Unbenannt1" src="https://github.com/user-attachments/assets/08fbf19f-8b76-4b74-90f3-761236228e9d" />



## 4. Erklärungen zum Node-Red Flow

<img width="1224" height="288" alt="Unbenannt" src="https://github.com/user-attachments/assets/93ee5f73-209c-4912-a793-c8ba8ee84b41" />


Mit CronPlus werden 2 Zeitfenster angesteuert:

Von 6:00-21:59 wird alle 15 Minuten das Python-Script ausgeführt und 30 Sekunden danach, die Text-Datei ausgelesen 

und mit der Funktions-Node verarbeitet, in HTML gewandelt und über MQTT an Mosquitto gesendet.

In der Zeit von 22:00-5:59 erfolgt die Ansteuerung nur jede volle Stunde.

Die Zeiten sind natürlich in der CronPlus-Node editier und anpassbar. 

Mein Mosquitto-Broker sendet dann die MQTT-Nachricht an das MQTT-Modul das Magic-Mirrors, wo die Daten dann angezeigt werden.




## 5. Anzeige des Fahrzeugstandortes

wenn man möchte das zusätzlich der Fahrzeugstandort auf dem Bildschirm angezeigt wird ist folgendes nötig:

## 5.1 Abhänigkeiten

### 5.1.1 In Node-Red:
[Worldmap Node](https://flows.nodered.org/node/node-red-contrib-web-worldmap) 

### 5.1.2 Im Magic-Mirror
[MMM-WebView](https://github.com/Iketaki/MMM-WebView)

## 5.2 Installation in Node-Red

Die Worldmap-Node in Node-Red installieren und konfigurieren.

Danach die Flowdatei (pycupra_to_MM_with_map.json) herunterladen und in Node-Red importieren.

<img width="1279" height="414" alt="Unbenannt" src="https://github.com/user-attachments/assets/bdd6d283-01bb-4db0-b668-0b7e473b9b29" />

## Installation im Magic Mirror

Das MMM-Webview Module in das Modul Verzeichnis clonen.

In der config.js:

oben unterhalb von let config = {   einfügen:

```
electronOptions: {
    webPreferences: {
      webviewTag: true,
    },
  },
```
danach unterhalb von modules [
```
{
    module: "MMM-WebView",
    position: "top_right",
    header: " Fahrzeugstandort",
    config: {
        url: "http://IP_eurer_Node_Red_Installation:1880/worldmap/",
        width: "520px",
        height: "540px",
        autoRefresh: "true",
        autoRefreshInterval: "10 * 60 * 1000",
           },
},
```
Autorefresh hab ich auf 10 Minuten stehen. Bitte den Cronjob beachten, der in Node-Red die Daten ausliest und dann erst über MQTT und in die Wordmap Node sendet.









