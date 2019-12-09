---
title: Grundlegendes zur IP-Adresse Ihres IoT-Hubs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die IP-Adresse Ihres IoT-Hubs und die zugehörigen Eigenschaften abfragen. Die IP-Adresse Ihres IoT-Hubs kann sich bei verschiedenen Szenarien wie z.B. einer Notfallwiederherstellung oder einem regionalen Failover ändern.
author: philmea
ms.author: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: 5c77fb30ef60c1ad82d0a87442bc8af186c54321
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383910"
---
# <a name="iot-hub-ip-addresses"></a>IP-Adressen von IoT Hub

Die IP-Adresspräfixe von IoT Hub werden regelmäßig unter dem [Diensttag](../virtual-network/service-tags-overview.md) *AzureIoTHub* veröffentlicht. Ihre IoT-Geräte müssen über ausgehende Konnektivität mit Adresspräfixen verfügen, die unter dem Diensttag *AzureIoTHub* aufgeführt sind. Ihre IoT-Anwendungsdienste müssen zudem über ausgehende Konnektivität mit Adresspräfixen verfügen, die unter dem Diensttag *EventHub* aufgeführt sind.


## <a name="best-practices"></a>Bewährte Methoden

* Die IP-Adresspräfixe von IoT Hub können sich möglicherweise ändern. Solche Änderungen werden regelmäßig mithilfe von Diensttags veröffentlicht, bevor sie in Kraft treten. Daher ist es wichtig, geeignete Prozesse zu entwickeln, um regelmäßig die neuesten Diensttags abzurufen und zu verwenden. Dieser Prozess kann mithilfe der [Diensttagermittlungs-API](../virtual-network/service-tags-overview.md#service-tags-in-on-premises) automatisiert werden.
* Verwenden Sie das Tag *AzureIoTHub.[Regionsname]* , um die IP-Präfixe zu ermitteln, die von IoT Hub-Endpunkten in einer bestimmten Region verwendet werden. Achten Sie zur Einbeziehung von Rechenzentrums-Notfallwiederherstellung oder [regionalem Failover](iot-hub-ha-dr.md) darauf, dass die Konnektivität mit IP-Präfixen der geografisch gekoppelten Region Ihrer IoT Hub-Instanz ebenfalls aktiviert ist.


## <a name="support-for-ipv6"></a>Unterstützung für IPv6 

IPv6 wird auf IoT Hub derzeit nicht unterstützt.