---
title: Aktivieren der verwalteten Identität im Azure Event Grid-Systemthema
description: In diesem Artikel wird beschrieben, wie Sie die verwaltete Dienstidentität für ein Azure Event Grid-Thema aktivieren.
ms.topic: how-to
ms.date: 03/25/2021
ms.openlocfilehash: d8219cf9cba4ce0a4fb5ddbcf695f1ec72de36fe
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106280512"
---
# <a name="assign-a-system-managed-identity-to-an-event-grid-system-topic"></a>Weisen Sie eine systemverwalteten Identität einem Event-Grid-Systemthema hinzu
In diesem Artikel erfahren Sie, wie Sie die systemverwaltete Identität für ein vorhandenes Event Grid-Systemthema aktivieren. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md).  

> [!IMPORTANT]
> Derzeit ist es nicht möglich, eine vom System verwaltete Identität zu aktivieren, wenn Sie ein neues System Thema erstellen, d. h. Wenn Sie ein Ereignis Abonnement für eine Azure-Ressource erstellen, die Systemthemen unterstützt. 


## <a name="use-azure-portal"></a>Verwenden des Azure-Portals
Das folgende Verfahren zeigt Ihnen, wie Sie die systemverwaltete Identität für ein Systemthema aktivieren. 

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Suchen Sie oben in der Suchleiste nach **Ereignis-Grid-Systemthemen**.
3. Wählen Sie das **benutzerdefinierte Thema** aus, für das Sie die verwaltete Identität aktivieren möchten. 
4. Wählen Sie im linken Menü **Identität** aus. Diese Option wird für ein System Thema, das sich am globalen Speicherort befindet, nicht angezeigt. 
5. **Aktivieren** Sie den Schalter, um die Identität zu aktivieren. 
1. Wählen Sie auf der Symbolleiste **Speichern** aus, um die Einstellung zu speichern. 

    :::image type="content" source="./media/managed-service-identity/identity-existing-system-topic.png" alt-text="Identitätsseite für ein Systemthema"::: 
1. Klicken Sie in der Bestätigungsmeldung auf **Ja**. 

    :::image type="content" source="./media/managed-service-identity/identity-existing-system-topic-confirmation.png" alt-text="Ordnen Sie Ihre Identität einem Systemthema zu - Bestätigung"::: 
1. Bestätigen Sie, dass Sie die Objekt-ID der vom System zugewiesenen verwalteten Identität sehen und einen Link zum Zuweisen von Rollen sehen. 

    :::image type="content" source="./media/managed-service-identity/identity-existing-system-topic-completed.png" alt-text="Zuweisen der Identität zu einem System Thema - abgeschlossen"::: 

## <a name="global-azure-sources"></a>Globale Azure-Quellen
Sie können die vom System verwaltete Identität nur für die regionalen Azure-Ressourcen aktivieren. Sie können Sie nicht für Systemthemen aktivieren, die mit globalen Azure-Ressourcen wie Azure-Abonnements, Ressourcengruppen oder Azure Maps verknüpft sind. Die Systemthemen für diese globalen Quellen sind auch keiner bestimmten Region zugeordnet. Sie sehen die Seite **Identität** nicht für das Systemthema, dessen Standort auf **Global** eingestellt ist. 

:::image type="content" source="./media/managed-service-identity/system-topic-location-global.png" alt-text="System Thema mit auf Global eingestelltem Standort"::: 



## <a name="next-steps"></a>Nächste Schritte
Fügen Sie die Identität einer geeigneten Rolle (z. B. Azure Service Bus-Datensender) für das Ziel (z. B. Service Bus-Warteschlange) zu. Ausführliche Schritte finden Sie unter [Gewähren des Zugriffs auf ein Event Grid-Ziel für die verwaltete Identität](add-identity-roles.md). 