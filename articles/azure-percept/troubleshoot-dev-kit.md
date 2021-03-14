---
title: Behandeln allgemeiner Probleme mit Azure Percept DK und IoT Edge
description: Hier finden Sie Tipps zur Problembehandlung für einige der häufigsten Probleme während des Onboardings.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: a6d099e8d267c9fe03e0bb676276e7a4ab8157ab
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102521525"
---
# <a name="azure-percept-dk-dev-kit-troubleshooting"></a>Problembehandlung für Azure Percept DK (Development Kit)

Hier finden Sie allgemeine Tipps zur Problembehandlung für Azure Percept DK.

## <a name="general-troubleshooting-commands"></a>Allgemeine Befehle für die Problembehandlung

Gehen Sie zum Ausführen dieser Befehle wie folgt vor: 
1. Stellen Sie eine Verbindung mit dem [WLAN-Zugriffspunkt des Development Kits](./quickstart-percept-dk-set-up.md) her.
1. [Stellen Sie eine SSH-Verbindung mit dem Development Kit her.](./how-to-ssh-into-percept-dk.md)
1. Geben Sie die Befehle in das SSH-Terminal ein.

Verwenden Sie die folgende Syntax, um Ausgaben zur weiteren Analyse an eine TXT-Datei umzuleiten:

```console
[command] > [file name].txt
```

Nach dem Umleiten einer Ausgabe an eine TXT-Datei kopieren Sie die Datei über SCP auf Ihren Host-PC:

```console
scp [remote username]@[IP address]:[remote file path]/[file name].txt [local host file path]
```

```[local host file path]``` bezieht sich auf den Speicherort auf dem Host-PC, an den Sie die TXT-Datei kopieren möchten. ```[remote username]``` ist der SSH-Benutzername, der im Rahmen des [Setups](./quickstart-percept-dk-set-up.md) ausgewählt wurde. Wenn Sie auf der Windows-Willkommensseite keine SSH-Anmeldung eingerichtet haben, lautet der Remotebenutzername ```root```.

Weitere Informationen zu den Azure IoT Edge-Befehlen finden Sie in der [Problembehandlungsdokumentation für Azure IoT Edge-Geräte](https://docs.microsoft.com/azure/iot-edge/troubleshoot).

|Kategorie:         |Befehl:                    |Funktion:                  |
|------------------|----------------------------|---------------------------|
|OS                |```cat /etc/os-release```         |Überprüfen der Mariner-Imageversion |
|OS                |```cat /etc/os-subrelease```      |Überprüfen der abgeleiteten Imageversion |
|OS                |```cat /etc/adu-version```        |Überprüfen der ADU-Version |
|Temperatur       |```cat /sys/class/thermal/thermal_zone0/temp``` |Überprüfen der Temperatur des Development Kits |
|WLAN             |```journalctl -u hostapd.service``` |Überprüfen von SoftAP-Protokollen|
|WLAN             |```journalctl -u wpa_supplicant.service``` |Überprüfen von WLAN-Dienstprotokollen |
|WLAN             |```journalctl -u ztpd.service```  |Überprüfen von Dienstprotokollen für die Zero Touch-WLAN-Bereitstellung |
|WLAN             |```journalctl -u systemd-networkd``` |Überprüfen von Mariner-Netzwerkstapelprotokollen |
|WLAN             |```/data/misc/wifi/hostapd_virtual.conf``` |Überprüfen der Konfigurationsdetails des WLAN-Zugriffspunkts |
|OOBE              |```journalctl -u oobe -b```       |Überprüfen von OOBE-Protokollen |
|Telemetrie         |```azure-device-health-id```      |Ermitteln der eindeutigen Telemetriehardware-ID |
|Azure IoT Edge          |```sudo iotedge check```          |Ausführen von Konfigurations- und Konnektivitätsüberprüfungen bei allgemeinen Problemen |
|Azure IoT Edge          |```sudo iotedge logs [container name]``` |Überprüfen von Containerprotokollen (beispielsweise Module für Sprache und maschinelles Sehen) |
|Azure IoT Edge          |```sudo iotedge support-bundle --since 1h``` |Erfassen von Modulprotokollen, Azure IoT Edge Security Manager-Protokollen und Containerengine-Protokollen sowie der JSON-Ausgabe von ```iotedge check``` und anderer nützlicher Debuginformationen aus der letzten Stunde |
|Azure IoT Edge          |```sudo journalctl -u iotedge -f``` |Anzeigen der Protokolle von Azure IoT Edge Security Manager |
|Azure IoT Edge          |```sudo systemctl restart iotedge``` |Neustarten des Azure IoT Edge-Sicherheits-Daemon |
|Azure IoT Edge          |```sudo iotedge list```           |Auflisten der bereitgestellten Azure IoT Edge-Module |
|Andere             |```df [option] [file]```          |Anzeigen von Informationen zum verfügbaren/gesamten Speicherplatz in angegebenen Dateisystemen |
|Andere             |```ip route get 1.1.1.1```        |Anzeigen von Geräte-IP-Adresse und Schnittstelleninformationen |
|Andere             |```ip route get 1.1.1.1 \| awk '{print $7}'``` <br> ```ifconfig [interface]``` |Nur Anzeigen der Geräte-IP-Adresse |


Die WLAN-Befehle vom Typ ```journalctl``` können zum folgenden Einzelbefehl kombiniert werden:

```console
journalctl -u hostapd.service -u wpa_supplicant.service -u ztpd.service -u systemd-networkd -b
```

## <a name="docker-troubleshooting-commands"></a>Befehle für die Behandlung von Docker-Problemen

|Befehl:                        |Funktion:                  |
|--------------------------------|---------------------------|
|```docker ps``` |[Zeigt die ausgeführten Container an.](https://docs.docker.com/engine/reference/commandline/ps/) |
|```docker images``` |[Zeigt die Images auf dem Gerät an.](https://docs.docker.com/engine/reference/commandline/images/)|
|```docker rmi [image id] -f``` |[Löscht ein Image vom Gerät.](https://docs.docker.com/engine/reference/commandline/rmi/) |
|```docker logs -f edgeAgent``` <br> ```docker logs -f [module_name]``` |[Erstellt Containerprotokolle des angegebenen Moduls.](https://docs.docker.com/engine/reference/commandline/logs/) |
|```docker image prune``` |[Entfernt alle nicht zugeordneten Images.](https://docs.docker.com/engine/reference/commandline/image_prune/) |
|```watch docker ps``` <br> ```watch ifconfig [interface]``` |Überprüfen des Downloadstatus von Docker-Containern |

## <a name="usb-updating"></a>USB-Aktualisierung

|Error:                                    |Lösung:                                               |
|------------------------------------------|--------------------------------------------------------|
|„LIBUSB_ERROR_XXX“ beim Ausführen eines USB-Flashs per UUU |Dieser Fehler ist auf einen USB-Verbindungsfehler während der Aktualisierung per UUU zurückzuführen. Wenn das USB-Kabel nicht ordnungsgemäß mit den USB-Anschlüssen von PC oder PE-10X verbunden ist, tritt ein Fehler dieser Art auf. Stecken Sie das USB-Kabel auf beiden Seiten aus und wieder ein, und wackeln Sie am Kabel, um sich zu vergewissern, dass es ordnungsgemäß angeschlossen ist. Dadurch lässt sich das Problem fast immer beheben. |

## <a name="azure-percept-dk-carrier-board-led-states"></a>LED-Statusanzeigen der Azure Percept DK-Trägerplatine

An der Oberseite des Gehäuses der Trägerplatine befinden sich drei kleine LEDs. Die LED 1 ist mit einem Wolkensymbol, die LED 2 mit einem WLAN-Symbol und die LED 3 mit einem Ausrufezeichen gekennzeichnet. Die folgende Tabelle enthält Informationen zur jeweiligen LED-Statusanzeige:

|LED             |State      |BESCHREIBUNG                      |
|----------------|-----------|---------------------------------|
|LED 1 (IoT Hub) |An (leuchtet) |Das Gerät ist mit einer IoT Hub-Instanz verbunden. |
|LED 2 (WLAN)   |Langsames Blinken |Das Gerät ist bereit, von Wi-Fi Easy Connect konfiguriert zu werden und kündigt sich bei einem Konfigurator an. |
|LED 2 (WLAN)   |Schnelles Blinken |Die Authentifizierung war erfolgreich, und das Gerät wird zugeordnet. |
|LED 2 (WLAN)   |An (leuchtet) |Authentifizierung und Zuordnung waren erfolgreich. Das Gerät ist mit einem WLAN verbunden. |
|LED 3           |Nicht verfügbar         |Die LED wird nicht verwendet. |


