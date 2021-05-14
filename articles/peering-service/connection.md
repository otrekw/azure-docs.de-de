---
title: Azure Peering Service-Verbindung
description: Erfahren Sie etwas über Microsoft Azure Peering Service-Verbindungen.
services: peering-service
author: gthareja
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 04/07/2021
ms.author: gatharej
ms.openlocfilehash: 9463a0967b9cfc25f189dd3efac80e26428866fa
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108202657"
---
# <a name="peering-service-connection"></a>Peering Service-Verbindung

Als Verbindung wird in der Regel ein logischer Satz von Informationen bezeichnet, der einen Peering Service identifiziert. Er wird über die folgenden Attribute definiert:

- Logischer Name
- Konnektivitätspartner
- Primärer Dienststandort des Konnektivitätspartners
- Standort des Sicherungsdienstes des Konnektivitätspartners
- Physischer Standort des Kunden
- IP-Präfixe

Der Kunde kann je nach Anforderung eine einzelne oder mehrere Verbindungen herstellen. Eine Verbindung wird auch als Einheit für die Telemetriesammlung verwendet. Um beispielsweise Telemetriewarnungen zu abonnieren, muss der Kunde die Verbindung definieren, die überwacht werden soll.

> [!Note]
> Wenn Sie sich für Peering Service registrieren, analysieren wir Ihre Windows- und Microsoft 365-Telemetrie, um Latenzmessungen für die ausgewählten Präfixe bereitzustellen. Aktuell werden die Telemetriedaten nur für /24 oder größere Größenpräfixe unterstützt.
>Weitere Informationen zur Verbindungstelemetrie finden Sie unter [Peering Service-Verbindungstelemetrie](connection-telemetry.md).
>

## <a name="how-to-create-a-peering-service-connection"></a>Erstellen einer Peering Service-Verbindung

**Szenario:** Angenommen, eine Zweigstelle ist auf mehrere geografische Standorte verteilt, wie in der folgenden Abbildung dargestellt. Hier muss der Kunde einen logischen Namen, einen Dienstanbieternamen (SP), seinen physischen Standort und IP-Präfixe, die mit einer einzelnen Verbindung (im Kundenbesitz oder vom Dienstanbieter zugeordnet) verknüpft sind, bereitstellen.  Die primären Standorte und Sicherungsdienststandorte mit einem Partner helfen bei der Definition des bevorzugten Dienststandorts für den Kunden. Dieser Vorgang muss wiederholt werden, um einen Peering Service für andere Speicherorte zu erstellen.

![Georedundante Verbindungen](./media/peering-service-connection/peering-service-connections.png)

> [!Note]
> Eine Filterung nach Bundesstaat für den physischen Standort des Kunden wird in Betracht gezogen, wenn sich die Verbindung in den USA befindet.
>

## <a name="next-steps"></a>Nächste Schritte

Eine ausführliche Anleitung zum Registrieren der Peering Service-Verbindung finden Sie unter [Erstellen des Peering Service mithilfe des Azure-Portals](azure-portal.md).

Informationen zur Peering Service-Verbindungstelemetrie finden Sie unter [Peering Service-Verbindungstelemetrie](connection-telemetry.md).

Informationen zum Zugreifen auf Telemetriedaten finden Sie unter [Zugreifen auf Verbindungtelemetriedaten](measure-connection-telemetry.md).
