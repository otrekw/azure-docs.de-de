---
title: Hochverfügbarkeit und Notfallwiederherstellung
titleSuffix: Azure Digital Twins
description: Informationen zu den Azure- und Azure Digital Twins-Features zum Erstellen von Azure IoT-Lösungen mit Hochverfügbarkeit und Notfallwiederherstellung.
author: baanders
ms.author: baanders
ms.date: 10/14/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: ac75a5b0b59a06855b7ee88d971c269ca915e429
ms.sourcegitcommit: d6e92295e1f161a547da33999ad66c94cf334563
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2020
ms.locfileid: "96763163"
---
# <a name="azure-digital-twins-high-availability-and-disaster-recovery"></a>Hochverfügbarkeit und Notfallwiederherstellung: Azure Digital Twins

Ein wichtiger Aspekt bei robusten IoT-Lösungen sind Geschäftskontinuität und Notfallwiederherstellung. Der Entwurf für **Hochverfügbarkeit (HA)** und **Notfallwiederherstellung (DR)** kann Ihnen beim Definieren und Erreichen angemessener Betriebszeitziele für Ihre Lösung helfen.

Dieser Artikel beschreibt die Funktionen für Hochverfügbarkeit und Notfallwiederherstellung, die der Dienst Azure Digital Twins bietet.

Azure Digital Twins unterstützt die folgenden Featureoptionen:
* *Hochverfügbarkeit zwischen Regionen*: Integrierte Redundanz, um die Betriebszeit des Diensts zu optimieren.
* *Regionsübergreifende Notfallwiederherstellung*: Failover zu einer geografisch gekoppelten Azure-Region im Falle eines unerwarteten Rechenzentrumsfehlers.

Allgemeine Azure-Anleitungen zum Entwerfen von Hochverfügbarkeit/Notfallwiederherstellung finden Sie auch im Abschnitt [*Bewährte Methoden*](#best-practices).

## <a name="intra-region-ha"></a>Regionale Hochverfügbarkeit
 
Azure Digital Twins bietet Hochverfügbarkeit zwischen Regionen, indem Redundanzen innerhalb des Diensts implementiert werden. Dies spiegelt sich in der [SLA des Diensts](https://azure.microsoft.com/support/legal/sla/digital-twins) für die Uptime wider. **Für die Entwickler einer Azure Digital Twins-Lösung entsteht kein zusätzlicher Aufwand, um die Vorteile dieser Hochverfügbarkeitsfunktionen zu nutzen.** Obwohl Azure Digital Twins eine relativ hohe Betriebszeitgarantie bietet, können hier wie bei jeder verteilten Verarbeitungsplattform auch vorübergehende Fehler auftreten. Entsprechende Wiederholungsrichtlinien sollten in die Komponenten integriert werden, die mit einer Cloudanwendung interagieren, um vorübergehende Fehler zu behandeln.

## <a name="cross-region-dr"></a>Regionenübergreifende Notfallwiederherstellung

Es kann in einigen seltenen Fällen dazu kommen, dass ein Rechenzentrum aufgrund von Stromausfällen oder anderen Ereignissen in der Region ausfällt. Solche Ereignisse sind selten, und bei solchen Ausfällen ist die oben beschriebene Hochverfügbarkeit zwischen Regionen nicht immer hilfreich. Azure Digital Twins löst dieses Problem durch ein von Microsoft initiiertes Failover.

Das **von Microsoft initiierte Failover** wendet Microsoft in seltenen Fällen an, um ein Failover für alle Azure Digital Twins-Instanzen einer betroffenen Region in die entsprechende geografisch gekoppelte Region auszuführen. Dieser Prozess ist eine Standardoption (Benutzer können sie also nicht abwählen), für die kein Eingriff des Benutzers erforderlich ist. Microsoft behält sich das Recht vor, zu bestimmen, wann diese Option angewendet wird. Dieser Mechanismus bedarf nicht der Zustimmung des Benutzers, bevor ein Failover für die Instanz des Benutzers ausgeführt wird.

>[!NOTE]
> Einige Azure-Dienste bieten außerdem eine zusätzliche Option namens **vom Kunden initiiertes Failover**, die es Kunden ermöglicht, ein Failover nur für Ihre Instanz zu initiieren, z. B. zum Ausführen eines Notfallwiederherstellungsdrills. Dieser Mechanismus wird zurzeit von Azure Digital Twins **nicht unterstützt**. 

## <a name="best-practices"></a>Bewährte Methoden

Bewährte Methoden für Hochverfügbarkeit/Notfallwiederherstellung finden Sie in den folgenden Azure-Anleitungen zu diesem Thema: 
* Der Artikel [*Geschäftskontinuität mit Azure – technische Dokumentation*](/azure/architecture/framework/resiliency/overview) bietet einen allgemeinen Überblick über Geschäftskontinuität und Notfallwiederherstellung. 
* Das Dokument [*Notfallwiederherstellung und Hochverfügbarkeit für Azure-Anwendungen*](/azure/architecture/framework/resiliency/backup-and-recovery) enthält Architekturanleitungen zu Strategien für Azure-Anwendungen in Bezug auf Notfallwiederherstellung und Hochverfügbarkeit.

## <a name="next-steps"></a>Nächste Schritte 

Erfahren Sie mehr über die ersten Schritte mit Azure Digital Twins-Lösungen:
 
* [*Was ist Azure Digital Twins?*](overview.md)
* [*Schnellstart: Erkunden eines Beispielszenarios*](quickstart-adt-explorer.md)