---
title: Konfigurieren von Firewalls für Azure Percept und Sicherheitsempfehlungen
description: Weitere Informationen zum Konfigurieren von Firewalls für Azure Percept und den Sicherheitsempfehlungen
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: conceptual
ms.date: 03/25/2021
ms.openlocfilehash: cfc20a30104e24a3950c71bdd8377544803d2f25
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105604412"
---
# <a name="azure-percept-firewall-configuration-and-security-recommendations"></a>Konfigurieren von Firewalls für Azure Percept und Sicherheitsempfehlungen

Lesen Sie die nachstehenden Richtlinien, um Informationen zum Konfigurieren von Firewalls und allgemeinen bewährten Sicherheitsmethoden von Azure Percept zu finden.

## <a name="configuring-firewalls-for-azure-percept-dk"></a>Konfigurieren von Firewalls für Azure Percept DK

Falls Verbindungen von Azure Percept DK-Geräten in Ihrem Netzwerksetup explizit zugelassen werden müssen, sehen Sie sich die folgende Komponentenliste an.

Diese Prüfliste ist ein Ausgangspunkt für Firewallregeln:

|URL (* = Platzhalter)|Ausgehende TCP-Ports|Verwendung|
|-------------------|------------------|---------|
|*.auth.azureperceptdk.azure.net|443|Authentifizierung und Autorisierung für Azure DK-SOM|
|*.auth.projectsantacruz.azure.net|443|Authentifizierung und Autorisierung für Azure DK-SOM|

Machen Sie sich außerdem mit der Liste der [von Azure IoT Edge verwendeten Verbindungen](https://docs.microsoft.com/azure/iot-edge/production-checklist#allow-connections-from-iot-edge-devices) vertraut.

## <a name="additional-recommendations-for-deployment-to-production"></a>Weitere Empfehlungen für die Bereitstellung in der Produktion

Azure Percept DK bietet bereits standardmäßig eine Vielzahl von Sicherheitsfunktionen. Für Bereitstellungen in der Produktion empfiehlt Microsoft neben den leistungsstarken Sicherheitsfeatures des aktuellen Release Folgendes:

- Starker physischer Schutz des eigentlichen Geräts
- Verschlüsselung ruhender Daten ist aktiviert
- Kontinuierliche Überwachung des Gerätestatus und schnelle Reaktion auf Warnungen
- Beschränkung der Anzahl von Administratoren mit Zugriff auf das Gerät

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hier finden Sie weitere Informationen zur Sicherheit von Azure Percept.](./overview-percept-security.md)