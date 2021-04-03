---
title: Grundlegendes zur IP-Adresse Ihrer Instanz des IoT Device Provisioning-Diensts (DPS) | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Adresse und die zugehörigen Eigenschaften Ihres IoT Device Provisioning-Diensts (DPS) abfragen. Die IP-Adresse Ihrer DPS-Instanz kann für verschiedene Szenarien, z. B. Notfallwiederherstellung oder regionales Failover, variieren.
author: wesmc7777
ms.author: wesmc
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: f6afd5c4cc5aa0215f943979ae91389b39d449f6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "79237454"
---
# <a name="iot-hub-dps-ip-addresses"></a>IP-Adressen für IoT Hub DPS

Die IP-Adresspräfixe für die öffentlichen Endpunkte eines IoT Hub Device Provisioning-Diensts (DPS) werden regelmäßig unter dem [Diensttag](../virtual-network/service-tags-overview.md) _AzureIoTHub_ veröffentlicht. Sie können mithilfe dieser IP-Adresspräfixe die Konnektivität zwischen einer IoT DPS-Instanz und Geräten oder Netzwerkressourcen steuern, um verschiedene Ziele in Bezug auf die Netzwerkisolation zu erreichen:

| Zielsetzung | Vorgehensweise |
|------|----------|
| Sicherstellen, dass Ihre Geräte und Dienste nur mit IoT Hub DPS-Endpunkten kommunizieren | Verwenden Sie das Diensttag _AzureIoTHub_, um IoT Hub DPS-Instanzen zu ermitteln. Konfigurieren Sie in der Firewalleinstellung Ihrer Geräte und Dienste für diese IP-Adresspräfixe entsprechende Zulassungsregeln. Konfigurieren Sie Regeln zum Löschen des Datenverkehrs an andere IP-Zieladressen, für die Sie die Kommunikation mit Geräten oder Diensten nicht zulassen möchten. |
| Sicherstellen, dass Ihr IoT Hub DPS-Endpunkt Verbindungen nur von Ihren Geräten und Netzwerkressourcen empfängt | Verwenden Sie die [IP-Filterfunktion](iot-dps-ip-filtering.md) von IoT DPS, um Filterregeln für die APIs des Geräts und des DPS-Diensts zu erstellen. Sie können diese Filterregeln nutzen, um Verbindungen nur zwischen Ihren Geräten und den IP-Adressen der Netzwerkressourcen zuzulassen (siehe Abschnitt [Einschränkungen](#limitations-and-workarounds)). | 




## <a name="best-practices"></a>Bewährte Methoden

* Beim Hinzufügen von Zulassungsregeln in der Firewallkonfiguration Ihrer Geräte empfiehlt sich die Bereitstellung bestimmter [Ports, die von anwendbaren Protokollen genutzt werden](../iot-hub/iot-hub-devguide-protocols.md#port-numbers).

* Die IP-Adresspräfixe von IoT DPS-Instanzen können sich unter Umständen ändern. Solche Änderungen werden regelmäßig mithilfe von Diensttags veröffentlicht, bevor sie in Kraft treten. Daher ist es wichtig, geeignete Prozesse zu entwickeln, um regelmäßig die neuesten Diensttags abzurufen und zu verwenden. Dieser Prozess kann mithilfe der [Diensttagermittlungs-API](../virtual-network/service-tags-overview.md#service-tags-on-premises) automatisiert werden. Die Diensttagermittlungs-API befinden sich noch in der Vorschauphase, und in einigen Fällen wird ggf. nicht die vollständige Liste mit den Tags und IP-Adressen erzeugt. Bis zur allgemeinen Verfügbarkeit der Ermittlungs-API sollten Sie die [Diensttags im herunterladbaren JSON-Format](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files) verwenden. 

* Verwenden Sie das Tag *AzureIoTHub.[Regionsname]* , um die IP-Präfixe zu ermitteln, die von DPS-Endpunkten in einer bestimmten Region verwendet werden. Achten Sie zur Einbeziehung von Rechenzentrums-Notfallwiederherstellung oder [regionalem Failover](../iot-hub/iot-hub-ha-dr.md) darauf, dass die Konnektivität mit IP-Präfixen der geografisch gekoppelten Region Ihrer DPS-Instanz ebenfalls aktiviert ist.

* Das Einrichten von Firewallregeln für eine DPS-Instanz kann die Konnektivität blockieren, die in diesem Zusammenhang zum Ausführen von Azure CLI- und PowerShell-Befehlen erforderlich ist. Um diese Konnektivitätsprobleme zu vermeiden, können Sie Zulassungsregeln für die IP-Adresspräfixe Ihrer Clients hinzufügen, damit für CLI- oder PowerShell-Clients die Kommunikation mit Ihrer DPS-Instanz wieder zulässig ist.  


## <a name="limitations-and-workarounds"></a>Einschränkungen und Problemumgehungen

* Für die IP-Filterfunktion des DPS gilt ein Grenzwert von 100 Regeln. Dieser Grenzwert kann über Anforderungen, die vom Azure-Kundensupport ausgelöst wurden, angehoben werden. 

* Ihre konfigurierten [IP-Filterregeln](iot-dps-ip-filtering.md) werden nur auf Ihre DPS-Endpunkte und nicht auf die verknüpften IoT Hub-Endpunkte angewendet. Die IP-Filterung für verknüpfte IoT Hubs muss separat konfiguriert werden. Weitere Informationen finden Sie im Artikel zu den [IoT Hub-IP-Filterregeln](../iot-hub/iot-hub-ip-filtering.md).

## <a name="support-for-ipv6"></a>Unterstützung für IPv6 

IPv6 wird für IoT Hub oder DPS derzeit nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Konfiguration von IP-Adressen für DPS finden Sie unter:

* [Konfigurieren der IP-Filterung](iot-dps-ip-filtering.md)
