---
title: Festlegen von Warnungen für Azure Event Grid-Metriken und -Aktivitätsprotokollvorgänge
description: In diesem Artikel wird beschrieben, wie Sie Warnungen zu Azure Event Grid-Metriken und -Aktivitätsprotokollvorgängen erstellen.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 48cb402e31435cb3e9390e8aeb461fcc5f90702f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100572008"
---
# <a name="set-alerts-on-azure-event-grid-metrics-and-activity-logs"></a>Festlegen von Warnungen für Azure Event Grid-Metriken und -Aktivitätsprotokolle
In diesem Artikel wird beschrieben, wie Sie Warnungen zu Azure Event Grid-Metriken und -Aktivitätsprotokollvorgängen erstellen. Sie können Warnungen zu Veröffentlichungs- und Übermittlungsmetriken für Azure Event Grid-Ressourcen (Themen und Domänen) erstellen. Für Systemthemen [erstellen Sie Warnungen mithilfe der Seite **Metriken**](#create-alerts-using-the-metrics-page).

## <a name="create-alerts-on-dead-lettered-events"></a>Erstellen von Warnungen für unzustellbare Ereignisse
Im folgenden Verfahren wird gezeigt, wie Sie eine Warnung für die Metrik **unzustellbare Ereignisse** für ein benutzerdefiniertes Thema erstellen. In diesem Beispiel wird eine E-Mail an den Besitzer der Azure-Ressourcengruppe gesendet, wenn die Anzahl der unzustellbaren Ereignisse für ein Thema 10 überschreitet. 

1. Wählen Sie auf der Seite **Event Grid-Thema** für Ihr Thema im Menü auf der linken Seite **Warnungen** aus, und wählen Sie dann **+Neue Warnungsregel** aus. 

    :::image type="content" source="./media/monitor-event-delivery/new-alert-button.png" alt-text="Seite „Warnungen“: Schaltfläche „Neue Warnungsregel“":::
2. Bestätigen Sie auf der Seite  **Warnungsregel erstellen**, dass Ihr Thema für die Ressource ausgewählt ist. Klicken Sie dann auf **Bedingung auswählen**. 

    :::image type="content" source="./media/monitor-event-delivery/alert-select-condition.png" alt-text="Seite „Warnungen“, Bedingung auswählen":::    
3. Führen Sie auf der Seite **Signallogik konfigurieren** die folgenden Schritte aus:
    1. Wählen Sie eine Metrik oder einen Aktivitätsprotokolleintrag aus. In diesem Beispiel werden **Unzustellbare Ereignisse** ausgewählt. 

        :::image type="content" source="./media/monitor-event-delivery/select-dead-lettered-events.png" alt-text="Auswählen von unzustellbaren Ereignissen":::        
    2. Wählen Sie Dimensionen aus (optional). 
        
        :::image type="content" source="./media/monitor-event-delivery/configure-signal-logic.png" alt-text="Signallogik konfigurieren":::        

        > [!NOTE]
        > Sie können die Schaltfläche **+** für **EventSubscriptionName** auswählen, um einen Ereignisabonnementnamen zum Filtern von Ereignissen anzugeben. 
    3. Scrollen Sie nach unten. Wählen Sie im Abschnitt **Warnungslogik** einen **Operator** und **Aggregationstyp** aus, geben Sie einen **Schwellenwert** ein, und wählen Sie dann **Fertig** aus. In diesem Beispiel wird eine Warnung ausgelöst, wenn die Gesamtzahl der unzustellbaren Ereignisse größer als 10 ist. 
    
        :::image type="content" source="./media/monitor-event-delivery/alert-logic.png" alt-text="Warnungslogik":::                
4. Navigieren Sie zurück zur Seite **Warnungsregel erstellen**, und klicken Sie auf **Aktionsgruppe auswählen**.

    :::image type="content" source="./media/monitor-event-delivery/select-action-group-button.png" alt-text="Schaltfläche „Aktionsgruppe auswählen“":::
5. Wählen Sie zum Erstellen einer neuen Aktionsgruppe **Aktionsgruppe erstellen** auf der Symbolleiste aus. Sie können auch eine vorhandene Aktionsgruppe auswählen.        
6. Führen Sie auf der Seite **Aktionsgruppe hinzufügen** die folgenden Schritte aus:
    1. Geben Sie einen **Namen für die Aktionsgruppe** ein.
    1. Geben Sie einen **Kurznamen** für die Aktionsgruppe ein.
    1. Wählen Sie ein **Azure-Abonnement** aus, in dem Sie die Aktionsgruppe erstellen möchten.
    1. Wählen Sie die **Azure-Ressourcengruppe** aus, in der Sie die Aktionsgruppe erstellen möchten.
    1. Geben Sie einen **Namen für die Aktion** ein. 
    1. Wählen Sie den **Aktionstyp** aus. In diesem Beispiel wird **E-Mail an Azure Resource Manager-Rolle** ausgewählt, dabei insbesondere die Rolle **Besitzer**. 
    1. Wählen Sie **OK**, um die Seite zu schließen. 
    
        :::image type="content" source="./media/monitor-event-delivery/add-action-group-page.png" alt-text="Seite „Aktionsgruppe hinzufügen“":::                   
7. Navigieren Sie zurück zur Seite **Warnungsregel erstellen**, geben Sie einen Namen für die Regel ein, und wählen Sie dann **Warnungsregel erstellen** aus.

    :::image type="content" source="./media/monitor-event-delivery/alert-rule-name.png" alt-text="Name der Warnungsregel":::  
8. Auf der Seite **Warnungen** des Themas wird nun einen Link zum Verwalten von Warnungsregeln angezeigt, wenn noch keine Warnungen vorhanden sind. Wenn Warnungen vorhanden sind, wählen Sie auf der Symbolleiste **Warnungsregeln verwalten** aus.  

    :::image type="content" source="./media/monitor-event-delivery/manage-alert-rules.png" alt-text="Verwalten von Warnungen":::

## <a name="create-alerts-on-other-metrics-or-activity-log-operations"></a>Erstellen von Warnungen zu anderen Metriken oder Aktivitätsprotokollvorgängen
Im vorherigen Abschnitt wurde gezeigt, wie Sie Warnungen für unzustellbare Ereignisse erstellen. Die Schritte zum Erstellen von Warnungen für andere Metriken oder Aktivitätsprotokollvorgänge sind ähnlich. 

Wenn Sie z. B. eine Warnung bei einem Übermittlungsfehlerereignis erstellen möchten, wählen Sie **Übermittlungsfehlerereignisse** auf der Seite **Signallogik konfigurieren** aus. 

:::image type="content" source="./media/set-alerts/delivery-failed-events.png" alt-text="Übermittlungsfehlerereignisse auswählen":::


## <a name="create-alerts-using-the-metrics-page"></a>Erstellen von Warnungen mithilfe der Seite „Metriken“
Sie können Warnungen auch mithilfe der Seite **Metriken** erstellen. Die Schritte sind ähnlich. Für Systemthemen können Sie nur die Seite **Metriken** verwenden, um Warnungen zu erstellen, da die Seite **Warnungen** nicht verfügbar ist. 

:::image type="content" source="./media/monitor-event-delivery/metric-page-create-alert-button.png" alt-text="Seite „Metriken“, Schaltfläche „Warnung erstellen“":::   
    

> [!NOTE]
> Dieser Artikel behandelt nicht alle einzelnen Schritte und Kombinationen, die Sie zum Erstellen einer Warnung verwenden können. Eine Übersicht der Warnungen finden Sie unter [Übersicht zu Warnungen](../azure-monitor/alerts/alerts-metric.md).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Ereignisübermittlungen und Wiederholungen finden Sie unter [Event Grid – Übermittlung und Wiederholung von Nachrichten](delivery-and-retry.md).
* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Um sich schnell mit der Verwendung von Event Grid vertraut zu machen, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).
