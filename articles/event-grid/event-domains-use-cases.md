---
title: Anwendungsfälle für Ereignisdomänen in Azure Event Grid
description: In diesem Artikel werden einige Anwendungsfälle für die Verwendung von Ereignisdomänen in Azure Event Grid beschrieben.
ms.topic: conceptual
ms.date: 03/04/2021
ms.openlocfilehash: 24a338717b44567bad0ec1575d98ddaeada71113
ms.sourcegitcommit: 5163ebd8257281e7e724c072f169d4165441c326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2021
ms.locfileid: "112413619"
---
# <a name="use-cases-for-event-domains-in-azure-event-grid"></a>Anwendungsfälle für Ereignisdomänen in Azure Event Grid
In diesem Artikel werden einige Anwendungsfälle für die Verwendung von Ereignisdomänen in Azure Event Grid beschrieben. 

## <a name="use-case-1"></a>Anwendungsfall 1 
[!INCLUDE [event-grid-domain-example-use-case.md](./includes/event-grid-domain-example-use-case.md)]

## <a name="use-case-2"></a>Anwendungsfall 2
Systemthemen sind auf maximal 500 Ereignisabonnements begrenzt. Sollten Sie mehr als 500 Ereignisabonnements für ein Systemthema benötigen, können Sie Domänen verwenden. 

Angenommen, Sie haben ein Systemthema für Azure Blob Storage erstellt und müssen mehr als 500 Abonnements für das Thema erstellen, dies ist jedoch aufgrund der Einschränkung (500 Abonnements pro Thema) nicht möglich. In diesem Fall können Sie die folgende Lösung mit einer Ereignisdomäne verwenden. 

1. Erstellen Sie eine Domäne. Diese kann bis zu 100.000 Themen unterstützen, und die einzelnen Domänenthemen können jeweils 500 Ereignisabonnements haben. Bei diesem Modell sind also 500 × 100.000 Ereignisabonnements möglich. 
1. Erstellen Sie ein Azure-Funktionsabonnement für das Azure Storage-Systemthema. Wenn die Funktion Ereignisse von Azure Storage empfängt, kann sie Ereignisse anreichern und in einem geeigneten Domänenthema veröffentlichen. So kann die Funktion beispielsweise den Blobdateinamen analysieren, um das Zieldomänenthema zu ermitteln, und das Ereignis im entsprechenden Domänenthema veröffentlichen. 

:::image type="content" source="./media/event-domains-use-cases/use-case-2.jpg" alt-text="Azure Event Grid-Domänen: Anwendungsfall 2":::


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Einrichten von Ereignisdomänen, zum Erstellen von Themen, zum Erstellen von Ereignisabonnements und zum Veröffentlichen von Ereignissen finden Sie unter [Verwalten von Ereignisdomänen](./how-to-event-domains.md).
