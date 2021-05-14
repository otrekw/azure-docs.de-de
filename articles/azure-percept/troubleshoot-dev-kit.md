---
title: Behandeln allgemeiner Probleme mit Azure Percept DK und IoT Edge
description: Tipps zur Problembehandlung für einige der häufigeren Probleme mit dem Azure Percept DK
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 03/25/2021
ms.custom: template-how-to
ms.openlocfilehash: c9c62ec07873272b956877ec51d8765ae0bbd100
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105605636"
---
# <a name="azure-percept-dk-troubleshooting"></a>Azure Percept DK-Fehlerbehebung

Hier finden Sie allgemeine Tipps zur Problembehandlung für Azure Percept DK.

## <a name="general-troubleshooting-commands"></a>Allgemeine Befehle für die Problembehandlung

Um diese Befehle auszuführen, verbinden Sie sich per [SSH mit dem Dev-Kit](./how-to-ssh-into-percept-dk.md) und geben Sie die Befehle in die Eingabeaufforderung des SSH-Clients ein.

Verwenden Sie die folgende Syntax, um Ausgaben zur weiteren Analyse an eine TXT-Datei umzuleiten:

```console
sudo [command] > [file name].txt
```

Ändern Sie die Berechtigungen der .txt-Datei, sodass sie kopiert werden kann:

```console
sudo chmod 666 [file name].txt
```

Nach dem Umleiten einer Ausgabe an eine TXT-Datei kopieren Sie die Datei über SCP auf Ihren Host-PC:

```console
scp [remote username]@[IP address]:[remote file path]/[file name].txt [local host file path]
```

```[local host file path]``` bezieht sich auf den Speicherort auf dem Host-PC, an den Sie die TXT-Datei kopieren möchten. ```[remote username]``` ist der SSH-Benutzername, der im Rahmen des [Setups](./quickstart-percept-dk-set-up.md) ausgewählt wurde.

Weitere Informationen zu den Azure IoT Edge-Befehlen finden Sie in der [Problembehandlungsdokumentation für Azure IoT Edge-Geräte](../iot-edge/troubleshoot.md).

|Kategorie:         |Befehl:                    |Funktion:                  |
|------------------|----------------------------|---------------------------|
|OS                |```cat /etc/os-release```         |Überprüfen der Mariner-Imageversion |
|OS                |```cat /etc/os-subrelease```      |Überprüfen der abgeleiteten Imageversion |
|OS                |```cat /etc/adu-version```        |Überprüfen der ADU-Version |
|Temperatur       |```cat /sys/class/thermal/thermal_zone0/temp``` |Überprüfen der Temperatur des Development Kits |
|WLAN             |```sudo journalctl -u hostapd.service``` |Überprüfen von SoftAP-Protokollen|
|WLAN             |```sudo journalctl -u wpa_supplicant.service``` |Überprüfen von WLAN-Dienstprotokollen |
|WLAN             |```sudo journalctl -u ztpd.service```  |Überprüfen von Dienstprotokollen für die Zero Touch-WLAN-Bereitstellung |
|WLAN             |```sudo journalctl -u systemd-networkd``` |Überprüfen von Mariner-Netzwerkstapelprotokollen |
|WLAN             |```sudo cat /etc/hostapd/hostapd-wlan1.conf``` |Überprüfen der Konfigurationsdetails des WLAN-Zugriffspunkts |
|OOBE              |```sudo journalctl -u oobe -b```       |Überprüfen von OOBE-Protokollen |
|Telemetrie         |```sudo azure-device-health-id```      |Ermitteln der eindeutigen Telemetriehardware-ID |
|Azure IoT Edge          |```sudo iotedge check```          |Ausführen von Konfigurations- und Konnektivitätsüberprüfungen bei allgemeinen Problemen |
|Azure IoT Edge          |```sudo iotedge logs [container name]``` |Überprüfen von Containerprotokollen (beispielsweise Module für Sprache und maschinelles Sehen) |
|Azure IoT Edge          |```sudo iotedge support-bundle --since 1h``` |Erfassen von Modulprotokollen, Azure IoT Edge Security Manager-Protokollen und Containerengine-Protokollen sowie der JSON-Ausgabe von ```iotedge check``` und anderer nützlicher Debuginformationen aus der letzten Stunde |
|Azure IoT Edge          |```sudo journalctl -u iotedge -f``` |Anzeigen der Protokolle von Azure IoT Edge Security Manager |
|Azure IoT Edge          |```sudo systemctl restart iotedge``` |starten ie den Azure IoT Edge Security Daemon erneut |
|Azure IoT Edge          |```sudo iotedge list```           |Auflisten der bereitgestellten Azure IoT Edge-Module |
|Andere             |```df [option] [file]```          |Anzeigen von Informationen zum verfügbaren/gesamten Speicherplatz in angegebenen Dateisystemen |
|Andere             |`ip route get 1.1.1.1`        |Anzeigen von Geräte-IP-Adresse und Schnittstelleninformationen |
|Andere             |<code>ip route get 1.1.1.1 &#124; awk '{print $7}'</code> <br> `ifconfig [interface]` |Nur Anzeigen der Geräte-IP-Adresse |


Die WLAN-Befehle vom Typ ```journalctl``` können zum folgenden Einzelbefehl kombiniert werden:

```console
sudo journalctl -u hostapd.service -u wpa_supplicant.service -u ztpd.service -u systemd-networkd -b
```

## <a name="docker-troubleshooting-commands"></a>Befehle für die Behandlung von Docker-Problemen

|Befehl:                        |Funktion:                  |
|--------------------------------|---------------------------|
|```sudo docker ps``` |[Zeigt die ausgeführten Container an.](https://docs.docker.com/engine/reference/commandline/ps/) |
|```sudo docker images``` |[Zeigt die Images auf dem Gerät an.](https://docs.docker.com/engine/reference/commandline/images/)|
|```sudo docker rmi [image id] -f``` |[Löscht ein Image vom Gerät.](https://docs.docker.com/engine/reference/commandline/rmi/) |
|```sudo docker logs -f edgeAgent``` <br> ```sudo docker logs -f [module_name]``` |[Erstellt Containerprotokolle des angegebenen Moduls.](https://docs.docker.com/engine/reference/commandline/logs/) |
|```sudo docker image prune``` |[Entfernt alle nicht zugeordneten Images.](https://docs.docker.com/engine/reference/commandline/image_prune/) |
|```sudo watch docker ps``` <br> ```watch ifconfig [interface]``` |Überprüfen des Downloadstatus von Docker-Containern |

## <a name="usb-updates"></a>USB-Aktualisierungen

|Error:                                    |Lösung:                                               |
|------------------------------------------|--------------------------------------------------------|
|„LIBUSB_ERROR_XXX“ beim Ausführen eines USB-Flashs per UUU |Dieser Fehler ist auf einen USB-Verbindungsfehler während der Aktualisierung per UUU zurückzuführen. Wenn das USB-Kabel nicht ordnungsgemäß mit den USB-Anschlüssen des PC oder der Percept DK Trägerplatine verbunden ist, tritt ein Fehler dieser Art auf. Versuchen Sie, beide Enden des USB-Kabels zu trennen und wieder anzuschließen und rütteln Sie am Kabel, um eine sichere Verbindung zu gewährleisten. Dadurch lässt sich das Problem fast immer beheben. |

## <a name="azure-percept-dk-carrier-board-led-states"></a>LED-Statusanzeigen der Azure Percept DK-Trägerplatine

An der Oberseite des Gehäuses der Trägerplatine befinden sich drei kleine LEDs. Die LED 1 ist mit einem Wolkensymbol, die LED 2 mit einem WLAN-Symbol und die LED 3 mit einem Ausrufezeichen gekennzeichnet. Die folgende Tabelle enthält Informationen zur jeweiligen LED-Statusanzeige:

|LED             |State      |BESCHREIBUNG                      |
|----------------|-----------|---------------------------------|
|LED 1 (IoT Hub) |An (leuchtet) |Das Gerät ist mit einer IoT Hub-Instanz verbunden. |
|LED 2 (WLAN)   |Langsames Blinken |Das Gerät ist bereit, von Wi-Fi Easy Connect konfiguriert zu werden und kündigt sich bei einem Konfigurator an. |
|LED 2 (WLAN)   |Schnelles Blinken |Die Authentifizierung war erfolgreich, und das Gerät wird zugeordnet. |
|LED 2 (WLAN)   |An (leuchtet) |Authentifizierung und Zuordnung waren erfolgreich. Das Gerät ist mit einem WLAN verbunden. |
|LED 3           |Nicht verfügbar         |Die LED wird nicht verwendet. |