---
title: Gleichzeitigkeitssteuerung – Azure Marketplace
description: Strategien für die Gleichzeitigkeitssteuerung der Veröffentlichungs-APIs des Cloud-Partnerportals.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: emuench
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 1160d5568108e7936099cc5227b9e4cadbaddf36
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86520339"
---
# <a name="concurrency-control"></a>Gleichzeitigkeitssteuerung

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und funktionieren auch weiterhin. Der Übergang führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](./cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach dem Übergang zu Partner Center weiterhin funktioniert. CPP-APIs sollten nur für vorhandene Produkte verwendet werden, die bereits vor dem Übergang zu Partner Center integriert wurden. Neue Produkte sollten die Partner Center-APIs für die Übermittlung verwenden.

In jedem Aufruf einer Veröffentlichungs-API des Cloud-Partnerportals muss explizit angegeben sein, welche Strategie für die Gleichzeitigkeitssteuerung verwendet werden soll. Fehler beim Bereitstellen des **If-Match**-Headers führen zu einer HTTP 400-Fehlerantwort. Es gibt zwei Strategien für die Gleichzeitigkeitssteuerung.

-   **Optimistisch**: Der Client, der die Aktualisierung ausführt, prüft, ob die Daten geändert wurden, seitdem er sie letztmalig gelesen hat.
-   **Letzter gewinnt**: Der Client aktualisiert die Daten sofort, unabhängig davon, ob eine andere Anwendung sie seit dem letzten Lesevorgang geändert hat.

<a name="optimistic-concurrency-workflow"></a>Workflow für optimistische Gleichzeitigkeit
-------------------------------

Es empfiehlt sich, die optimistische Gleichzeitigkeitsstrategie mit dem folgenden Workflow zu verwenden, damit sichergestellt ist, dass keine unerwarteten Änderungen an Ihren Ressourcen vorgenommen werden.

1.  Rufen Sie eine Entität mit den APIs ab. Die Antwort enthält einen ETag-Wert, der die aktuell gespeicherte Version der Entität (zum Zeitpunkt der Antwort) kennzeichnet.
2.  Fügen Sie diesen ETag-Wert zum Zeitpunkt der Aktualisierung in den obligatorischen **If-Match**-Anforderungsheader ein.
3.  Die API vergleicht in einer atomischen Transaktion den ETag-Wert in der Anforderung mit dem aktuellen ETag-Wert der Entität.
    *   Sind die ETag-Werte unterschiedlich, gibt die API eine `412 Precondition Failed`-HTTP-Antwort zurück. Dieser Fehler kennzeichnet, dass entweder ein anderer Prozess die Entität aktualisiert hat, seit der Client sie zuletzt abgerufen hat, oder dass der in der Anforderung angegebene ETag-Wert falsch ist.
    *  Sind die ETag-Werte gleich oder enthält der **If-Match**-Header das Sternchen-Platzhalterzeichen (`*`), führt die API den angeforderten Vorgang aus. Der API-Vorgang aktualisiert auch den gespeicherten ETag-Wert der Entität.


> [!NOTE]
> Ist das Platzhalterzeichen (*) im **If-Match**-Header angegeben, verwendet die API die Gleichzeitigkeitsstrategie „Letzter gewinnt“. In diesem Fall erfolgt kein ETag-Vergleich, und die Ressource wird ohne jegliche Prüfungen aktualisiert. 
