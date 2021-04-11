---
title: Azure Industrial IoT-Plattform
description: Dieser Artikel enthält eine Übersicht über die Industrial IoT-Plattform und die zugehörigen Komponenten.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: conceptual
ms.date: 3/22/2021
ms.openlocfilehash: 24932b17c630190cb3121d5310a865758fa6a920
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104801552"
---
# <a name="what-is-the-industrial-iot-iiot-platform"></a>Was ist die IIoT-Plattform (Industrial IoT)?

Die Azure Industrial IoT-Plattform ist eine Suite von Microsoft mit in Azure bereitgestellten Modulen und Diensten. Diese Module und Dienste sind vollständig offen. Wir verwenden insbesondere das verwaltete PaaS-Angebot (Platform-as-a-Service) von Azure, per MIT-Lizenz lizenzierte Open-Source-Software, offene internationale Standards für Kommunikation (OPC UA, AMQP, MQTT, HTTP) und Schnittstellen (Open API) sowie offene Industriedatenmodelle (OPC UA) im Edgebereich und in der Cloud.

## <a name="enabling-shopfloor-connectivity"></a>Ermöglichen von Konnektivität für Produktionsstätten 

Die Azure Industrial IoT-Plattform bietet industrielle Konnektivität für Produktionsanlagen (einschließlich Ermittlung von Ressourcen mit OPC UA-Unterstützung), normalisiert ihre Daten im OPC UA-Format und überträgt Ressourcenttelemetriedaten im PubSub-Format von OPC UA an Azure. Dort werden die Telemetriedaten in einer Clouddatenbank gespeichert. Darüber hinaus ermöglicht die Plattform sicheren Zugriff auf die Produktionsanlagen per OPC UA über die Cloud. Und sie bietet integrierte Funktionen für die Geräteverwaltung (einschließlich Sicherheitskonfiguration). Die OPC UA-Funktion basiert auf Docker-Containertechnologie und ermöglicht eine unkomplizierte Bereitstellung und Verwaltung. Für Ressourcen ohne OPC UA-Unterstützung haben wir mit führenden Industriekonnektivitätsanbietern zusammengearbeitet und Ihnen dabei geholfen, ihre OPC UA-Adapter Software für Azure IoT Edge zu portieren. Diese Adapter sind im Azure Marketplace verfügbar.

## <a name="industrial-iot-components-iot-edge-modules-and-cloud-microservices"></a>Industrial IoT-Komponenten: IoT Edge-Module und cloudbasierte Microservices

Die Edge-Dienste werden als Azure IoT Edge-Module implementiert und lokal ausgeführt. Die cloudbasierten Microservices werden als ASP.NET-Microservices mit einer REST-Schnittstelle implementiert und in verwalteten Azure Kubernetes Service-Instanzen oder eigenständig in Azure App Service ausgeführt. Sowohl für Edge- als auch für Clouddienste wurden vorgefertigte Docker-Container in Microsoft Container Registry (MCR) bereitgestellt, um den Kunden diesen Schritt abzunehmen. Die Edge- und Clouddienste nutzen sich gegenseitig und müssen daher gemeinsam verwendet werden. Es stehen auch benutzerfreundliche Bereitstellungsskripts zur Verfügung, mit denen sich die gesamte Plattform mit nur einem Befehl bereitstellen lässt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, was die Azure Industrial IoT-Plattform ist, können Sie sich als Nächstes mit OPC Publisher befassen:

> [!div class="nextstepaction"]
> [Was ist OPC Publisher?](overview-what-is-opc-publisher.md)