---
title: Übersicht über Azure Industrial IoT
description: Dieser Artikel enthält eine Übersicht über Industrial IoT. Hier finden Sie Informationen zu den in IIoT enthaltenen Konnektivitäts- und Sicherheitskomponenten für Produktionsstätten.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: overview
ms.date: 3/22/2021
ms.openlocfilehash: 940391d26b5a8455fef01c8094cd08e05ab51290
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104787458"
---
# <a name="what-is-industrial-iot-iiot"></a>Was ist Industrial IoT (IIoT)?

IIoT (Industrial Internet of Things) ermöglicht die Verbesserung der Effizienz in der Industrie durch den Einsatz von IoT in der Fertigungsbranche.

![Industrial IoT](media/overview-what-is-Industrial-IoT/icon-255-px.png)

## <a name="improve-industrial-efficiencies"></a>Verbessern der Effizienz in der Industrie
Verbessern Sie Ihre operative Produktivität und Rentabilität mit Azure Industrial IoT. Reduzieren Sie den zeitaufwendigen Prozess des Zugriffs auf die Ressourcen vor Ort. Verbinden und überwachen Sie Ihre Industriemaschinen und -geräte in der Cloud – einschließlich Ihrer Computer, die bereits im Fertigungsbereich betrieben werden. Analysieren Sie Ihre IoT-Daten, um zu ermitteln, wie sich die Leistung des gesamten Standorts verbessern lässt.

## <a name="industrial-iot-components"></a>Industrial IoT-Komponenten

**IoT Edge-Geräte:** Ein IoT Edge-Gerät setzt sich aus Edge-Runtime und Edge-Modulen zusammen. 
- *Edge-Module* sind Docker-Container und somit die kleinste Berechnungseinheit. Beispiele wären etwa OPC Publisher und OPC Twin. 
- Ein *Edge-Gerät* wird zur Bereitstellung solcher Module verwendet, die als Vermittler zwischen OPC UA-Server und IoT Hub in der Cloud fungieren. Weitere Informationen zu IoT Edge finden Sie [hier](https://azure.microsoft.com/services/iot-edge/).

**IoT Hub:** Die IoT Hub-Instanz fungiert als zentraler Nachrichtenhub für die bidirektionale Kommunikation zwischen einer IoT-Anwendung und den von ihr verwalteten Geräten. Hierbei handelt es sich um eine offene und flexible Cloudplattform in Form eines Diensts, der Open-Source-SDKs und mehrere Protokolle unterstützt. Weitere Informationen zu IoT Hub finden Sie [hier](https://azure.microsoft.com/services/iot-hub/).

**Industrial Edge-Module**
- *OPC Publisher:* OPC Publisher wird in IoT Edge ausgeführt. Das Modul stellt eine Verbindung mit OPC UA-Servern her und veröffentlicht JSON-codierte Telemetriedaten von diesen Servern im OPC UA-PubSub-Format in Azure IoT Hub. Es können alle vom Azure IoT Hub Client SDK unterstützten Transportprotokolle (HTTPS, AMQP und MQTT) verwendet werden.
- *OPC Twin:* OPC Twin umfasst Microservices, die über Azure IoT Edge und IoT Hub eine Verbindung zwischen der Cloud und dem Fabriknetzwerk herstellen. OPC-Zwilling bietet Erkennungs- und Registrierungsfunktionen und ermöglicht die Fernsteuerung von Industriegeräten über Rest-APIs. Für OPC Twin wird kein OPC UA SDK (OPC Unified Architecture) benötigt. Die Programmiersprache ist unabhängig und kann in einen serverlosen Workflow integriert werden.
- *Ermittlung:* Das durch die Discoverer-Identität dargestellte Ermittlungsmodul stellt Ermittlungsdienste am Edge bereit (einschließlich OPC UA-Serverermittlung). Wenn die Ermittlung konfiguriert und aktiviert ist, sendet das Modul die Ergebnisse eines Überprüfungstests über den IoT Edge- und IoT Hub-Telemetriepfad an den Onboardingdienst. Der Dienst verarbeitet die Ergebnisse und aktualisiert alle zugehörigen Identitäten in der Registrierung.


**Ermitteln, Registrieren und Verwalten Ihrer Industrieanlagen mit Azure:** Mit Azure Industrial IoT können Anlagenbetreiber Server mit OPC UA-Unterstützung in einem Fabriknetzwerk ermitteln und sie bei Azure IoT Hub registrieren. Betriebspersonal kann von überall auf der Welt Ereignisse auf Fertigungsbereichsebene abonnieren und auf sie reagieren. Durch die REST-APIs der Microservices wird die Edge-Seite der OPC UA-Dienste gespiegelt. Sie werden durch OAuth-Authentifizierung und -Autorisierung auf AAD-Basis (Azure Active Directory) geschützt. Dadurch können Ihre Cloudanwendungen Serveradressräume durchsuchen oder Variablen lesen/schreiben sowie Methoden unter Verwendung von HTTPS und einfachen OPC UA-JSON-Nutzdaten ausführen.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun wissen, was Industrial IoT ist, können Sie sich als Nächstes mit der Industrial IoT-Plattform und mit OPC Publisher vertraut machen:

> [!div class="nextstepaction"]
> [Was ist die IIoT-Plattform (Industrial IoT)?](overview-what-is-industrial-iot-platform.md)

> [!div class="nextstepaction"]
> [Was ist OPC Publisher?](overview-what-is-opc-publisher.md)