# pycupra_to_MagicMirror
Display some Information about my Cupra on my Magic Mirror

## 1. Abhängigkeiten

![Node Red](https://img.shields.io/badge/Node--Red-8F0000?style=flat&logo=nodered&logoColor=white) Vorhandene Node-Red Installation<br>
![Mosquitto](https://img.shields.io/badge/mosquitto-%233C5280.svg?style=flat&logo=eclipsemosquitto&logoColor=white) Mosquitto für MQTT<br>
Ausführbare [pycupra Installation](https://github.com/SteitzTo/pycupra_manual_de)<br>
[Magic Mirror Installation](https://github.com/MagicMirrorOrg/MagicMirror)<br>
[MQTT Modul für MagicMirror](https://github.com/ottopaulsen/MMM-MQTT)<br>
<br>
## 2. Node-Red Nodes

[Cron-plus](https://flows.nodered.org/node/node-red-contrib-cron-plus)<br>

## 3. Anzeigen auf dem Magic Mirror 

Status: Verschlossen/Grün   Offen/rot<br>
Restreichweite in km<br>
Tankinhalt in % (Bei unter 20% orange, bei unter 5% rot blinkend)<br>
Ölservice in Tagen / KM<br>
<br>
<br>

![20260226_202000](https://github.com/user-attachments/assets/b417a1ee-3bb8-4bad-9048-5a6219cacb62)


## 4. Erklärungen zum Node-Red Flow

<img width="1224" height="288" alt="Unbenannt" src="https://github.com/user-attachments/assets/93ee5f73-209c-4912-a793-c8ba8ee84b41" />


Mit CronPlus werden 2 Zeitfenster angesteuert:

Von 6:00-21:59 wird alle 15 Minuten das Python-Script ausgeführt und 30 Sekunden danach, die Text-Datei ausgelesen 

und mit der Funktions-Node verarbeitet, in HTML gewandelt und über MQTT an Mosquitto gesendet.

In der Zeit von 22:00-5:59 erfolgt die Ansteuerung nur jede volle Stunde.

Die Zeiten sind natürlich in der CronPlus-Node editier und anpassbar. 

Mein Mosquitto-Broker sendet dann die MQTT-Nachricht an das MQTT-Modul das Magic-Mirrors, wo die Daten dann angezeigt werden.

