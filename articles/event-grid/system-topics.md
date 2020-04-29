---
title: Systemthemen in Azure Event Grid
description: Hier werden die Systemthemen in Azure Event Grid beschrieben.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: spelluru
ms.openlocfilehash: 46bceeb31fa38068c6c4f9f3a86ed556ad39effb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81393154"
---
# <a name="system-topics-in-azure-event-grid"></a>Systemthemen in Azure Event Grid
Der Azure Event Grid-Dienst erstellt Systemthemen, wenn Sie ein erstes Ereignisabonnement für eine Azure-Ereignisquelle erstellen. Derzeit erstellt Event Grid keine Systemthemen für Themenquellen, die vor dem 15. März 2020 erstellt wurden. Für alle Themenquellen, die Sie nach diesem Datum erstellt haben, erstellt Event Grid automatisch Systemthemen. In diesem Artikel werden die **Systemthemen** in Azure Event Grid beschrieben.

> [!NOTE]
> Dieses Feature ist zurzeit nicht für die Azure Government-Cloud aktiviert. 

## <a name="overview"></a>Übersicht
Wenn Sie das erste Ereignisabonnement für eine Azure-Ereignisquelle wie ein Azure Storage-Konto erstellen, wird im Rahmen des Bereitstellungsprozesses des Abonnements eine zusätzliche Ressource des Typs **Microsoft.EventGrid/systemTopics** erstellt. Wenn das letzte Ereignisabonnement für die Azure-Ereignisquelle gelöscht wird, wird das Systemthema automatisch gelöscht.

Das Systemthema kann nicht für benutzerdefinierte Themenszenarios verwendet werden (z. B. Event Grid-Themen und Event Grid-Domänen). 

## <a name="location"></a>Position
Für Azure-Ereignisquellen, die sich in einer bestimmten Region bzw. einem bestimmten Speicherort befinden, wird das Systemthema am gleichen Speicherort erstellt wie die Azure-Ereignisquelle. Wenn Sie beispielsweise ein Ereignisabonnement für Azure Blob Storage in der Region „USA, Osten“ erstellen, wird das Systemthema in „USA, Osten“ erstellt. Für globale Azure-Ereignisquellen wie Azure-Abonnements, Ressourcengruppen oder Azure Maps erstellt Event Grid das Systemthema an einem **globalen** Speicherort. 

## <a name="resource-group"></a>Resource group 
Im Allgemeinen wird das Systemthema in derselben Ressourcengruppe erstellt, in der sich auch die Azure-Ereignisquelle befindet. Für Ereignisabonnements, die im Bereich des Azure-Abonnements erstellt wurden, wird das Systemthema unter der Ressourcengruppe **Default-EventGrid** erstellt. Wenn die Ressourcengruppe nicht vorhanden ist, wird diese von Azure Event Grid vor dem Systemthema erstellt. 

Wenn Sie versuchen, die Ressourcengruppe mit dem Speicherkonto zu löschen, wird das Systemthema in der Liste der betroffenen Ressourcen angezeigt.  

![Ressourcengruppe löschen](./media/system-topics/delete-resource-group.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln: 

- [Benutzerdefinierte Themen](custom-topics.md)
- [Domänen](event-domains.md)