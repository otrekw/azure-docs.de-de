---
title: Verhindern von Ratenbegrenzungsfehlern bei Vorgängen der Azure Cosmos DB-API für MongoDB
description: Erfahren Sie, wie Sie Ratenbegrenzungsfehler bei Vorgängen Ihrer Azure Cosmos DB-API für MongoDB mithilfe des SSR-Features (Server-Side Retry, serverseitige Wiederholung) vermeiden können.
author: gahl-levy
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 01/13/2021
ms.author: gahllevy
ms.openlocfilehash: 73c2aba3028f42621f241bd8f295e83e0ef96e68
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2021
ms.locfileid: "98540373"
---
# <a name="prevent-rate-limiting-errors-for-azure-cosmos-db-api-for-mongodb-operations"></a>Verhindern von Ratenbegrenzungsfehlern bei Vorgängen der Azure Cosmos DB-API für MongoDB
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Bei Vorgängen der Azure Cosmos DB-API für MongoDB treten möglicherweise Ratenbegrenzungsfehler (16500/429) auf, wenn sie den Durchsatzgrenzwert (in RUs) einer Sammlung überschreiten. 

Sie können das Feature für die serverseitige Wiederholung (Server-Side Retry, SSR) aktivieren, damit der Server diese Vorgänge automatisch wiederholt. Die Anforderungen werden nach einer kurzen Verzögerung für alle Sammlungen in Ihrem Konto wiederholt. Dieses Feature ist eine praktische Alternative zur Behandlung von Ratenbegrenzungsfehlern in der Clientanwendung.


## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Navigieren Sie zu Ihrem Konto für die Azure Cosmos DB-API für MongoDB.

1. Wechseln Sie im Abschnitt **Einstellungen** zum Bereich **Features**.

1. Wählen Sie **Serverseitige Wiederholung** aus.

1. Klicken Sie auf **Aktivieren**, um dieses Feature für alle Sammlungen in Ihrem Konto zu aktivieren.

:::image type="content" source="./media/prevent-rate-limiting-errors/portal-features-server-side-retry.png" alt-text="Screenshot des Features zur serverseitigen Wiederholung für die Azure Cosmos DB-API für MongoDB":::


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung bei häufigen Fehlern finden Sie in diesem Artikel:

* [Behandeln häufiger Probleme in der Azure Cosmos DB-API für MongoDB](mongodb-troubleshoot.md)
