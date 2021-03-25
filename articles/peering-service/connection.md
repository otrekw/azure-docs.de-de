---
title: Azure Peering Service-Verbindung
description: Erfahren Sie etwas über Microsoft Azure Peering Service-Verbindungen.
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 0540fa9c7a29456aaf343adce83509b2d0e390e4
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91398919"
---
# <a name="peering-service-connection"></a>Peering Service-Verbindung

Als Verbindung wird in der Regel ein logischer Satz von Informationen bezeichnet, der einen Peering Service identifiziert. Er wird über die folgenden Attribute definiert:

- Logischer Name
- Konnektivitätspartner
- Physischer Standort des Kunden
- IP-Präfixe

Der Kunde kann je nach Anforderung eine einzelne oder mehrere Verbindungen herstellen. Eine Verbindung wird auch als Einheit für die Telemetriesammlung verwendet. Um beispielsweise Telemetriewarnungen zu abonnieren, muss der Kunde die Verbindung definieren, die überwacht werden soll.

> [!Note]
> Wenn Sie sich für Peering Service registrieren, analysieren wir Ihre Windows- und Microsoft 365-Telemetrie, um Latenzmessungen für die ausgewählten Präfixe bereitzustellen.
>Weitere Informationen zur Verbindungstelemetrie finden Sie unter [Peering Service-Verbindungstelemetrie](connection-telemetry.md).
>

>## <a name="how-to-register-a-connection"></a>Registrieren einer Verbindung

**Szenario:** Angenommen, eine Zweigstelle ist auf mehrere geografische Standorte verteilt, wie in der folgenden Abbildung dargestellt. Hier muss der Kunde einen logischen Namen, einen Dienstanbieternamen (SP), seinen physischen Standort und IP-Präfixe, die mit einer einzelnen Verbindung (im Kundenbesitz oder vom Dienstanbieter zugeordnet) verknüpft sind, bereitstellen. Dieser Vorgang muss wiederholt werden, um Peering Service für separate georedundante Verbindungen zu registrieren.

![Georedundante Verbindungen](./media/peering-service-connection/peering-service-connections.png)

> [!Note]
> Eine Filterung nach Bundesstaat für den physischen Standort des Kunden wird in Betracht gezogen, wenn sich die Verbindung in den USA befindet.
>

## <a name="next-steps"></a>Nächste Schritte

Eine ausführliche Anleitung zum Registrieren der Peering Service-Verbindung finden Sie unter [Registrieren von Peering Service – Azure-Portal](azure-portal.md).

Informationen zur Peering Service-Verbindungstelemetrie finden Sie unter [Peering Service-Verbindungstelemetrie](connection-telemetry.md).

Informationen zum Messen der Telemetrie finden Sie unter [Messen der Verbindungstelemetrie](measure-connection-telemetry.md).
