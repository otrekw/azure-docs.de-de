---
title: Grundlegendes zur IP-Adresse Ihres IoT-Hubs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die IP-Adresse Ihres IoT-Hubs und die zugehörigen Eigenschaften abfragen. Die IP-Adresse Ihres IoT-Hubs kann sich bei verschiedenen Szenarien wie z.B. einer Notfallwiederherstellung oder einem regionalen Failover ändern.
author: philmea
ms.author: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: f05be2725ef766bb1e5fd7f2624e754a2e21698a
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75563172"
---
# <a name="iot-hub-ip-addresses"></a>IP-Adressen von IoT Hub

Die IP-Adresspräfixe von öffentlichen IoT Hub-Endpunkten werden regelmäßig unter dem [Diensttag](../virtual-network/service-tags-overview.md) _AzureIoTHub_ veröffentlicht. Sie können mithilfe dieser IP-Adresspräfixe die Konnektivität zwischen IoT Hub und Ihren Geräten oder Netzwerkressourcen steuern, um eine Vielzahl von Netzwerkisolationszielen zu implementieren:

| Zielsetzung | Anwendbare Szenarien | Vorgehensweise |
|------|-----------|----------|
| Sicherstellen, dass Ihre Geräte und Dienste nur mit IoT Hub-Endpunkten kommunizieren | [Gerät-zu-Cloud](./iot-hub-devguide-messaging.md)- und [Cloud-zu-Gerät](./iot-hub-devguide-messages-c2d.md)-Messaging, [direkte Methoden](./iot-hub-devguide-direct-methods.md), [Geräte- und Modulzwillinge](./iot-hub-devguide-device-twins.md) sowie [Gerätestreams](./iot-hub-device-streams-overview.md) | Verwenden Sie _AzureIoTHub_- und _EventHub_-Diensttags, um IoT Hub- und Event Hub-IP-Adresspräfixe zu ermitteln sowie um Zulassungsregeln für die Firewalleinstellung Ihrer Geräte und Dienste für diese IP-Adresspräfixe entsprechend zu konfigurieren. Legen Sie Datenverkehr auf andere Ziel-IP-Adressen ab, mit denen die Geräte oder Dienste nicht kommunizieren sollen. |
| Sicherstellen, dass Ihr IoT Hub-Geräteendpunkt Verbindungen nur von Ihren Geräten und Netzwerkressourcen empfängt | [Gerät-zu-Cloud](./iot-hub-devguide-messaging.md)- und [Cloud-zu-Gerät](./iot-hub-devguide-messages-c2d.md)-Messaging, [direkte Methoden](./iot-hub-devguide-direct-methods.md), [Geräte- und Modulzwillinge](./iot-hub-devguide-device-twins.md) sowie [Gerätestreams](./iot-hub-device-streams-overview.md) | Verwenden Sie die [IP-Filterfunktion](iot-hub-ip-filtering.md) von IoT Hub, um Verbindungen zwischen Ihren Geräten und den IP-Adressen der Netzwerkressourcen zu ermöglichen (siehe den Abschnitt zu [Einschränkungen](#limitations-and-workarounds)). | 
| Sicherstellen, dass die benutzerdefinierten Endpunktressourcen Ihrer Routen (Speicherkonten, Service Bus und Event Hubs) nur von ihren Netzwerkressourcen aus erreichbar sind | [Nachrichtenrouting](./iot-hub-devguide-messages-d2c.md) | Befolgen Sie die Anleitung Ihrer Ressource, um die Konnektivität einzuschränken (z. B. über [Firewallregeln](../storage/common/storage-network-security.md), [private Links](../private-link/private-endpoint-overview.md) oder [Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md)). Verwenden Sie _AzureIoTHub_-Diensttags, um IoT Hub-IP-Adresspräfixe zu ermitteln und Zulassungsregeln für diese IP-Präfixe für die Firewallkonfiguration ihrer Ressource hinzuzufügen (siehe den Abschnitt zu [Einschränkungen](#limitations-and-workarounds)). |



## <a name="best-practices"></a>Bewährte Methoden

* Beim Hinzufügen von Zulassungsregeln in der Firewallkonfiguration Ihrer Geräte empfiehlt sich die Bereitstellung bestimmter [Ports, die von anwendbaren Protokollen genutzt werden](./iot-hub-devguide-protocols.md#port-numbers).

* Die IP-Adresspräfixe von IoT Hub können sich möglicherweise ändern. Solche Änderungen werden regelmäßig mithilfe von Diensttags veröffentlicht, bevor sie in Kraft treten. Daher ist es wichtig, geeignete Prozesse zu entwickeln, um regelmäßig die neuesten Diensttags abzurufen und zu verwenden. Dieser Prozess kann mithilfe der [Diensttagermittlungs-API](../virtual-network/service-tags-overview.md#service-tags-on-premises) automatisiert werden.

* Verwenden Sie das Tag *AzureIoTHub.[Regionsname]* , um die IP-Präfixe zu ermitteln, die von IoT Hub-Endpunkten in einer bestimmten Region verwendet werden. Achten Sie zur Einbeziehung von Rechenzentrums-Notfallwiederherstellung oder [regionalem Failover](iot-hub-ha-dr.md) darauf, dass die Konnektivität mit IP-Präfixen der geografisch gekoppelten Region Ihrer IoT Hub-Instanz ebenfalls aktiviert ist.


## <a name="limitations-and-workarounds"></a>Einschränkungen und Problemumgehungen

* Die IoT Hub-IP-Filterfunktion hat einen Grenzwert von 10 Regeln. Dieser Grenzwert kann über Anforderungen, die vom Azure-Kundensupport ausgelöst wurden, angehoben werden. 

* Ihre konfigurierten [IP-Filterregeln](iot-hub-ip-filtering.md) werden nur auf Ihre IoT Hub-IP-Endpunkte angewendet und nicht auf den integrierten Event Hub-Endpunkt Ihres IoT Hubs. Wenn Sie außerdem anfordern, dass die IP-Filterung auf den Event Hub, auf dem Ihre Nachrichten gespeichert sind, angewendet werden soll, können Sie dafür Ihre eigene Event Hub-Ressource verwenden und dort Ihre gewünschten IP-Filterregeln direkt konfigurieren. Hierzu müssen Sie Ihre eigene Event Hub-Ressource bereitstellen und das [Nachrichtenrouting](./iot-hub-devguide-messages-d2c.md) so einrichten, dass Ihre Nachrichten an diese Ressource statt an den integrierten Event Hub Ihres IoT Hubs gesendet werden. Und schließlich: Wie in der vorstehenden Tabelle erläutert, müssen Sie zum Aktivieren der Nachrichtenroutingfunktionen auch Konnektivität zwischen den IP-Adresspräfixen des IoT Hubs und Ihrer bereitgestellten Event Hub-Ressource zulassen.

* Beim Routing an ein Speicherkonto ist das Zulassen von Datenverkehr aus den IP-Adresspräfixen des IoT Hubs nur möglich, wenn sich das Speicherkonto in einer anderen Region als Ihr IoT Hub befindet.

## <a name="support-for-ipv6"></a>Unterstützung für IPv6 

IPv6 wird auf IoT Hub derzeit nicht unterstützt.
