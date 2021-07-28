---
title: Übersicht über Azure Industrial IoT
description: Dieser Artikel enthält eine Übersicht über Industrial IoT. Hier finden Sie Informationen zu den in IIoT enthaltenen Konnektivitäts- und Sicherheitskomponenten für Produktionsstätten.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: overview
ms.date: 3/22/2021
ms.openlocfilehash: 902cd7c9cf0f682541be86e82a1d51525ad5b3f9
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110678029"
---
# <a name="what-is-industrial-iot-iiot"></a>Was ist Industrial IoT (IIoT)?

![Industrial IoT](media/overview-what-is-Industrial-IoT/icon-255-px.png)

Microsoft Azure Industrial Internet of Things (IIoT) ist eine Suite von Azure-Modulen und -Diensten, die die Leistungsfähigkeit der Cloud in Industrie- und Fertigungsanlagen integrieren. Dank branchenüblicher offener Schnittstellen wie [OPC UA (Open Platform Communications Unified Architecture)](https://opcfoundation.org/about/opc-technologies/opc-ua/) lassen sich mit Azure IIoT Daten von Ressourcen und Sensoren (einschließlich solcher, die bereits in Ihrem Werk verwendet werden) in die Azure-Cloud integrieren. In der Cloud gespeicherte Daten können schneller und flexibler als Feedback für die Entwicklung transformativer Geschäfts- und Industrieprozesse genutzt werden.

## <a name="discover-register-and-manage-your-industrial-assets-with-azure"></a>Ermitteln, Registrieren und Verwalten Ihrer Industrieanlagen mit Azure

Über die Azure Industrial IoT-Plattform können Anlagenbetreiber Server mit OPC UA-Unterstützung in einem Fabriknetzwerk ermitteln und sie bei Azure IoT Hub registrieren. Betriebspersonal kann von überall auf der Welt Ereignisse auf Fertigungsbereichsebene abonnieren und auf sie reagieren. Darüber hinaus können sie Warnungen und Alarme empfangen und in Echtzeit darauf reagieren.

IIoT bietet eine Reihe von Microservices, die OPC UA-Funktionen implementieren. Durch die REST-APIs der Microservices wird die Edge-Seite der OPC UA-Dienste gespiegelt. Deshalb können Ihre Cloudanwendungen Serveradressräume durchsuchen oder Variablen lesen/schreiben sowie Methoden unter Verwendung von HTTPS und einfachen OPC UA-JSON-Nutzdaten ausführen. Die Edge-Dienste werden als Azure IoT Edge-Module implementiert und lokal ausgeführt. Die cloudbasierten Microservices werden als ASP.NET-Microservices mit einer REST-Schnittstelle implementiert und in verwalteten Azure Kubernetes Services-Instanzen oder eigenständig in Azure App Service ausgeführt. Sowohl für Edge- als auch für Clouddienste bietet IIoT vordefinierte Docker-Container in der Microsoft Container Registry (MCR). Die Edge- und Clouddienste nutzen sich gegenseitig und müssen daher gemeinsam verwendet werden. IIoT bietet auch benutzerfreundliche Bereitstellungsskripts, mit denen sich die gesamte Plattform mit nur einem Befehl bereitstellen lässt.

Darüber hinaus können die REST-APIs mit einer beliebigen Programmiersprache über die verfügbar gemachte OpenAPI-Spezifikation (Swagger) verwendet werden. Dies bedeutet, dass Entwickler bei der Integration von OPC UA in Cloudverwaltungslösungen die Technologie wählen können, die am besten für ihre Qualifikation, Interessen und Architekturentscheidungen geeignet ist. So kann beispielsweise ein Full-Stack-Webentwickler, der eine Anwendung für ein Alarm- und Ereignisdashboard entwickelt, die Logik für die Reaktion auf Ereignisse in JavaScript oder TypeScript schreiben, ohne sich mit einem OPC UA SDK oder mit C, C++, Java oder C# befassen zu müssen.

## <a name="manage-certificates-and-trust-groups"></a>Verwalten von Zertifikaten und Vertrauensgruppen

Azure Industrial IoT verwaltet OPC UA-Anwendungszertifikate und Vertrauenslisten von Industrieanlagen und Steuerungssystemen, um die Kommunikation zwischen OPC UA-Client und -Server zu schützen. Die Lösung steuert, welcher Client mit welchem Server kommunizieren darf. Für die Speicherung privater Schlüssel und das Signieren von Zertifikaten wird Azure Key Vault genutzt, was hardwarebasierte Sicherheit (HSM) ermöglicht.

## <a name="industrial-iot-components"></a>Industrial IoT-Komponenten

Azure IIoT-Lösungen setzen sich aus bestimmten Komponenten zusammen. Hierzu zählen folgende:

- **Mindestens eine Azure IoT Hub-Instanz**
- **IoT Edge-Geräte**
- **Industrial Edge-Module**

### <a name="iot-hub"></a>IoT Hub
Die [Azure IoT Hub-Instanz](https://azure.microsoft.com/services/iot-hub/) fungiert als zentraler Nachrichtenhub für die sichere bidirektionale Kommunikation zwischen einer beliebigen IoT-Anwendung und den von ihr verwalteten Geräten. Hierbei handelt es sich um eine offene und flexible Cloudplattform in Form eines Diensts (PaaS), die Open-Source-SDKs und mehrere Protokolle unterstützt. 

Durch Zusammenfassen Ihrer Industrie- und Geschäftsdaten in einer IoT Hub-Instanz können Sie Ihre Daten sicher speichern, Geschäfts- und Effizienzanalysen dafür durchführen und entsprechende Berichte generieren. Darüber hinaus können Sie auch Microsoft Azure-Dienste und -Tools wie [Power BI](https://powerbi.microsoft.com) für Ihre konsolidierten Daten verwenden.

### <a name="iot-edge-devices"></a>IoT Edge-Geräte
Die [Edge-Dienste](https://azure.microsoft.com/services/iot-edge/) werden als Azure IoT Edge-Module implementiert und lokal ausgeführt. Die cloudbasierten Microservices werden als ASP.NET-Microservices mit einer REST-Schnittstelle implementiert und in verwalteten Azure Kubernetes Service-Instanzen oder eigenständig in Azure App Service ausgeführt. Sowohl für Edge- als auch für Clouddienste wurden vorgefertigte Docker-Container in Microsoft Container Registry (MCR) bereitgestellt, um den Kunden diesen Schritt abzunehmen. Die Edge- und Clouddienste nutzen sich gegenseitig und müssen daher gemeinsam verwendet werden. Es stehen auch benutzerfreundliche Bereitstellungsskripts zur Verfügung, mit denen sich die gesamte Plattform mit nur einem Befehl bereitstellen lässt.

Ein IoT Edge-Gerät setzt sich aus Edge-Runtime und Edge-Modulen zusammen.
- **Edge-Module** sind Docker-Container und somit die kleinste Berechnungseinheit. Beispiele wären etwa OPC Publisher und OPC Twin. 
- Ein **Edge-Gerät** wird zur Bereitstellung solcher Module verwendet, die als Vermittler zwischen OPC UA-Server und IoT Hub in der Cloud fungieren.

### <a name="industrial-edge-modules"></a>Industrial Edge-Module
- **OPC Publisher:** OPC Publisher wird in IoT Edge ausgeführt. Das Modul stellt eine Verbindung mit OPC UA-Servern her und veröffentlicht JSON-codierte Telemetriedaten von diesen Servern im OPC UA-PubSub-Format in Azure IoT Hub. Es können alle vom Azure IoT Hub Client SDK unterstützten Transportprotokolle (HTTPS, AMQP und MQTT) verwendet werden.
- **OPC Twin:** OPC Twin umfasst Microservices, die über Azure IoT Edge und IoT Hub eine Verbindung zwischen der Cloud und dem Fabriknetzwerk herstellen. OPC-Zwilling bietet Erkennungs- und Registrierungsfunktionen und ermöglicht die Fernsteuerung von Industriegeräten über Rest-APIs. Für OPC Twin wird kein OPC UA SDK (OPC Unified Architecture) benötigt. Die Programmiersprache ist unabhängig und kann in einen serverlosen Workflow integriert werden.
- **Ermittlung:** Das durch die Discoverer-Identität dargestellte Ermittlungsmodul stellt Ermittlungsdienste am Edge bereit (einschließlich OPC UA-Serverermittlung). Wenn die Ermittlung konfiguriert und aktiviert ist, sendet das Modul die Ergebnisse eines Überprüfungstests über den IoT Edge- und IoT Hub-Telemetriepfad an den Onboardingdienst. Der Dienst verarbeitet die Ergebnisse und aktualisiert alle zugehörigen Identitäten in der Registrierung.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie erfahren haben, was Industrial IoT ist, können Sie sich jetzt über OPC Publisher genauer informieren oder mit der Bereitstellung der IIoT-Plattform beginnen:

> [!div class="nextstepaction"]
> [Was ist OPC Publisher?](overview-what-is-opc-publisher.md)

> [!div class="nextstepaction"]
> [Bereitstellen der Industrial IoT-Plattform](tutorial-deploy-industrial-iot-platform.md)