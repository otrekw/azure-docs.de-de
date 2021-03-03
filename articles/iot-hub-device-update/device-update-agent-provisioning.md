---
title: Bereitstellen des Device Update for Azure IoT Hub-Agents | Microsoft-Dokumentation
description: Bereitstellen des Device Update for Azure IoT Hub-Agents
author: ValOlson
ms.author: valls
ms.date: 2/16/2021
ms.topic: how-to
ms.service: iot-hub-device-update
ms.openlocfilehash: 79bac3f057412f3973121f48cd735f72d0a97d04
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678482"
---
# <a name="device-update-agent"></a>Device Update-Agent

Device Update for IoT Hub unterstützt zwei Arten von Updates: imagebasiert und paketbasiert. 

* Imageupdates bieten ein höheres Maß an Vertrauen in den Endzustand des Geräts. Es ist in der Regel einfacher, die Ergebnisse eines Imageupdates zwischen einer Vorproduktions- und einer Produktionsumgebung zu replizieren, da dies nicht die gleichen Herausforderungen wie bei Paketen und deren Abhängigkeiten mit sich bringt. Aufgrund ihrer atomischen Natur kann auch ein A/B-Failovermodell problemlos übernommen werden. 
* Paketbasierte Updates sind gezielte Updates, die nur eine bestimmte Komponente oder Anwendung auf dem Gerät ändern. Dies führt daher zu einem geringeren Verbrauch an Bandbreite und verringert die Zeit zum Herunterladen und Installieren des Updates. Paketupdates ermöglichen in der Regel eine geringere Ausfallzeit der Geräte beim Anwenden eines Updates und vermeiden den Mehraufwand beim Erstellen von Images. 

Folgen Sie den unten aufgeführten Links, um sich über das Erstellen, Ausführen und Ändern des Device Update-Agents zu informieren.

## <a name="build-the-device-update-agent"></a>Erstellen des Device Update-Agents

Befolgen Sie die Anweisungen, um den Device Update-Agent aus der Quelle zu [erstellen](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-build-agent-code.md).

## <a name="run-the-device-update-agent"></a>Ausführen des Device Update-Agents

Nachdem der Agent erfolgreich erstellt wurde, ist es an der Zeit, den Agent [auszuführen](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-run-agent.md).

## <a name="modifying-the-device-update-agent"></a>Ändern des Device Update-Agents

Nehmen Sie nun die Änderungen vor, die für die Einbindung des Agents in Ihr Image erforderlich sind.  Anleitungen finden Sie unter der Vorgehensweise zum [Ändern](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-modify-the-agent-code.m) des Device Update-Agents.

### <a name="troubleshooting-guide"></a>Handbuch zur Problembehandlung

Wenn Probleme auftreten, lesen Sie den [Leitfaden zur Problembehandlung](troubleshoot-device-update.md) für Device Update for IoT Hub, um mögliche Probleme zu beheben und die erforderlichen Informationen für Microsoft zu erfassen.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die folgenden vordefinierten Images und Binärdateien für eine einfache Demonstration von Device Update for IoT Hub.  

[Imageupdate: Erste Schritte mit Raspberry Pi 3 B und dem Yocto-Referenzimage](device-update-raspberry-pi.md)

[Imageupdate: Erste Schritte mit Ubuntu (18.04 x64) Simulator Reference Agent](device-update-simulator.md)

[Paketaktupdate: Erste Schritte mit Ubuntu Server 18.04 x64 Package-Agent](device-update-ubuntu-agent.md)

