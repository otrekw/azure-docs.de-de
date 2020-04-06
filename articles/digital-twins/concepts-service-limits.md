---
title: Dienstbegrenzungen der Public Preview – Azure Digital Twins | Microsoft-Dokumentation
description: Erfahren Sie mehr über Begrenzungen bezüglich der Public Preview, des Abonnements, der Instanzen und Raten für Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/17/2020
ms.openlocfilehash: 5e323d8faa19ceb0712aa6183df17740ce2a0a1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370377"
---
# <a name="public-preview-service-limits"></a>Diensteinschränkungen der öffentlichen Vorschauversion

[!INCLUDE [digital-twins-preview-limit-alert](../../includes/digital-twins-preview-limit-alert.md)]

Für die Public Preview von Azure Digital Twins gelten die nachfolgend beschriebenen temporären Abonnement-, Instanz- und Anzahlgrenzwerte für vorhandene Kunden. Diese Einschränkungen sollen dazu beitragen, das Erlernen des neuen Diensts und seiner vielen Features zu vereinfachen, und sie werden durch die allgemeine Verfügbarkeit (GA) erhöht oder beseitigt.

## <a name="per-subscription-limits"></a>Grenzwerte pro Abonnement

In der Public Preview kann jedes Azure-Abonnement jeweils genau eine Azure Digital Twins-Instanz erstellen oder ausführen. Wenn Sie Ihre Instanz löschen, können Sie eine neue erstellen.

## <a name="per-instance-limits"></a>Grenzwerte pro Instanz

Der Reihe nach kann jede Azure Digital Twins-Instanz Folgendes haben:

- Genau eine eingebettete **IoTHub**-Ressource, die während der Dienstbereitstellung automatisch erstellt wird.
- Genau einen **EventHub**-Endpunkt für den Ereignistyp **DeviceMessage**
- Bis zu drei **EventHub**-, **ServiceBus**- oder **EventGrid**-Endpunkte der Ereignistypen **SensorChange**, **SpaceChange**, **TopologyOperation** oder **UdfCustom**

> [!NOTE]
> Einige Parameter, die normalerweise beim Erstellen der obigen Azure IoT-Entitäten definiert werden, sind während der Public Preview-Phase nicht erforderlich.
> - Die aktuellen API-Spezifikationen finden Sie in der [Swagger-Referenzdokumentation](./how-to-use-swagger.md).

## <a name="azure-digital-twins-management-api-limits"></a>Einschränkungen der Azure Digital Twins-Verwaltungs-API

Die Grenzwerte für die Anforderungsanzahl Ihrer Azure Digital Twins-Verwaltungs-API lauten wie folgt:

- 100 Anforderungen pro Sekunde an die Azure Digital Twins-Verwaltungs-API
- Bis zu 1.000 Objekte, die von einer einzelnen Azure Digital Twins-Verwaltungs-API-Abfrage zurückgegeben werden

> [!IMPORTANT]
> Beim Überschreiten des Grenzwerts von 1.000 Objekten tritt ein Fehler auf, und Sie müssen die Abfrage vereinfachen.

## <a name="user-defined-functions-rate-limits"></a>Ratenbegrenzungen für benutzerdefinierte Funktionen

Es gelten die folgenden Grenzwerte für die Gesamtanzahl der Aufrufe, die von benutzerdefinierten Funktionen an die Azure Digital Twins-Instanz gesendet werden:

- 400 Clientbibliotheksaufrufe pro Sekunde
- 100 **SendNotification**-Aufrufe pro Sekunde

> [!NOTE]
> Die folgenden Aktionen führen ggf. dazu, dass temporär zusätzliche Anzahlgrenzwerte angewendet werden:
> - Bearbeitungen an den Metadaten von Topologieobjekten
> - Aktualisierungen der Definition der benutzerdefinierten Funktion
> - Erstmaliges Senden von Telemetriedaten von Geräten

## <a name="device-telemetry-limits"></a>Grenzwerte für Gerätetelemetrie

Die folgenden Grenzwerte deckeln die Gesamtanzahl aller Nachrichten, die Ihre Geräte an Ihre Azure Digital Twins Instanz senden können:

- 100 Nachrichten pro Sekunde auf allen Geräten
-    25 Nachrichten pro Sekunde und Gerät

## <a name="next-steps"></a>Nächste Schritte

- Um ein Azure Digital Twins-Beispiel auszuprobieren, wechseln Sie zu [Schnellstart: Suchen nach verfügbaren Räumen mithilfe von Azure Digital Twins](./quickstart-view-occupancy-dotnet.md).
