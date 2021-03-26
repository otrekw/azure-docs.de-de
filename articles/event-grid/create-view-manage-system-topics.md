---
title: Erstellen, Anzeigen und Verwalten von Systemthemen in Azure Event Grid (Portal)
description: In diesem Artikel wird gezeigt, wie Sie vorhandene Systemthemen anzeigen und Azure Event Grid-Systemthemen mithilfe des Azure-Portals erstellen können.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 2dbb0893668d72cfebf2d64e6515ff6ab6fed9bc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "86115110"
---
# <a name="create-view-and-manage-event-grid-system-topics-in-the-azure-portal"></a>Erstellen, Anzeigen und Verwalten von Event Grid-Systemthemen im Azure-Portal
In diesem Artikel wird beschrieben, wie Sie Systemthemen im Azure-Portal erstellen und verwalten. Eine Übersicht zu Systemthemen finden Sie unter [Systemthemen](system-topics.md).

## <a name="create-a-system-topic"></a>Erstellen eines Systemthemas
Sie können ein Systemthema für eine Azure-Ressource (Speicherkonto, Event Hubs-Namespace usw.) auf zweierlei Weise erstellen:

- Mithilfe der Seite **Ereignisse** einer Ressource, z. B. der Seite „Speicherkonto“ oder „Event Hubs-Namespace“. Wenn Sie die Seite **Ereignisse** im Azure-Portal verwenden, um ein Ereignisabonnement für ein Ereignis zu erstellen, das von einer Azure-Quelle ausgelöst wird (Beispiel: Azure Storage Konto), erstellt das Portal ein Systemthema für die Azure-Ressource und dann ein Abonnement für das Systemthema. Sie geben den Namen des Systemthemas an, wenn Sie zum ersten Mal ein Ereignisabonnement für die Azure-Ressource erstellen. Ab dem zweiten Mal wird Ihnen der Name des Systemthemas im schreibgeschützten Modus angezeigt. Weitere Informationen finden Sie unter [Schnellstart: Weiterleiten von Blob Storage-Ereignissen an einen Webendpunkt über das Azure-Portal](blob-event-quickstart-portal.md#subscribe-to-the-blob-storage). Dort finden Sie ausführliche Schritte.
- Mithilfe der Seite **Event Grid-Systemthemen**. Die folgenden Schritte dienen zum Erstellen eines Systemthemas unter Verwendung der Seite **Event Grid-Systemthemen**. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Geben Sie in das Suchfeld oben **Event Grid-Systemthemen** ein, und drücken Sie dann die **EINGABETASTE**. 

    ![Suchen nach Systemthemen](./media/create-view-manage-system-topics/search-system-topics.png)
3. Wählen Sie auf der Symbolleiste der Seite **Event Grid-Systemthemen** die Option **+ Hinzufügen** aus.

    ![Systemthema hinzufügen – Symbolleistenschaltfläche](./media/create-view-manage-system-topics/add-system-topic-menu.png)
4. Führen Sie auf der Seite **Event Grid-Systemthema erstellen** die folgenden Schritte aus:
    1. Wählen Sie den **Thementyp** aus. Im folgenden Beispiel ist die Option **Speicherkonten** ausgewählt. 
    2. Wählen Sie das **Azure-Abonnement** mit Ihrer Speicherkontoressource aus. 
    3. Wählen Sie die **Ressourcengruppe** aus, die über das Speicherkonto verfügt. 
    4. Wählen Sie das **Speicherkonto** aus. 
    5. Geben Sie einen **Namen** für das zu erstellende Systemthema ein. 
    
        > [!NOTE]
        > Sie können diesen Systemthemanamen verwenden, um Metriken und Diagnoseprotokolle zu durchsuchen.
    6. Klicken Sie auf **Überprüfen + erstellen**.

        ![Erstellen eines Systemthemas](./media/create-view-manage-system-topics/create-event-grid-system-topic-page.png)
    5. Überprüfen Sie die Einstellungen, und wählen Sie **Erstellen** aus. 
        
        ![Überprüfen und Erstellen eines Systemthemas](./media/create-view-manage-system-topics/system-topic-review-create.png)
    6. Nachdem die Bereitstellung erfolgreich durchgeführt wurde, wählen Sie **Zu Ressource wechseln** aus, um die Seite **Event Grid-Systemthema** für das von Ihnen erstellte Systemthema anzuzeigen. 

        ![Seite „Systemthema“](./media/create-view-manage-system-topics/system-topic-page.png)


## <a name="view-all-system-topics"></a>Anzeigen aller Systemthemen
Führen Sie die folgenden Schritte aus, um alle vorhandenen Event Grid-Systemthemen anzuzeigen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Geben Sie in das Suchfeld oben **Event Grid-Systemthemen** ein, und drücken Sie dann die **EINGABETASTE**. 

    ![Suchen nach Systemthemen](./media/create-view-manage-system-topics/search-system-topics.png)
3. Auf der Seite **Event Grid-Systemthemen** werden alle Systemthemen angezeigt. 

    ![Liste der Systemthemen](./media/create-view-manage-system-topics/list-system-topics.png)
4. Wählen Sie ein **Systemthema** aus der Liste aus, um Details dazu anzuzeigen. 

    ![Details zum Systemthema](./media/create-view-manage-system-topics/system-topic-details.png)

    Auf dieser Seite werden Details zum Systemthema angezeigt, z. B. die folgenden Informationen: 
    - Source. Name der Ressource, für die das Systemthema erstellt wurde.
    - Quellentyp Der Typ der Ressource. Beispiel: `Microsoft.Storage.StorageAccounts`, `Microsoft.EventHub.Namespaces`, `Microsoft.Resources.ResourceGroups` usw.
    - Alle für das Systemthema erstellten Abonnements.

    Diese Seite gestattet Vorgänge wie die folgenden:
    - Erstellen Sie ein Ereignisabonnement. Wählen Sie **+Ereignisabonnement** auf der Symbolleiste aus. 
    - Löschen Sie ein Ereignisabonnement. Wählen Sie **Löschen** auf der Symbolleiste aus. 
    - Fügen Sie Tags für das Systemthema hinzu. Wählen Sie **Tags** im linken Menü aus, und geben Sie Tagnamen und -werte an. 


## <a name="delete-a-system-topic"></a>Löscht ein Systemthema
1. Folgen Sie den Anweisungen im Abschnitt [Anzeigen von Systemthemen](#view-all-system-topics), um alle Systemthemen anzuzeigen, und wählen Sie das Systemthema aus, das Sie aus der Liste löschen möchten. 
2. Wählen Sie auf der Symbolleiste der Seite **Event Grid-Systemthema** die Option **Löschen** aus. 

    ![Systemthema – Schaltfläche „Löschen“](./media/create-view-manage-system-topics/system-topic-delete-button.png)
3. Wählen Sie auf der Bestätigungsseite **OK** aus, um den Löschvorgang zu bestätigen. Er löscht das Systemthema und auch alle Ereignisabonnements für das Systemthema.  

## <a name="create-an-event-subscription"></a>Erstellen eines Ereignisabonnements
1. Folgen Sie den Anweisungen im Abschnitt [Anzeigen von Systemthemen](#view-all-system-topics), um alle Systemthemen anzuzeigen, und wählen Sie das Systemthema aus, das Sie aus der Liste löschen möchten. 
2. Wählen Sie auf der Seite **Event Grid-Systemthema** auf der Symbolleiste **+ Ereignisabonnement** aus. 

    ![Systemthema – Schaltfläche „Ereignisabonnement hinzufügen“](./media/create-view-manage-system-topics/add-event-subscription-button.png)
3. Bestätigen Sie, dass die Felder **Thementyp**, **Quellressource** und **Themenname** automatisch ausgefüllt sind. Geben Sie einen Namen ein, wählen Sie einen **Endpunkttyp** aus, und geben Sie dann den **Endpunkt** an. Wählen Sie dann **Erstellen** aus, um das Ereignisabonnement zu erstellen. 

    ![Systemthema – Ereignisabonnement erstellen](./media/create-view-manage-system-topics/create-event-subscription.png)

## <a name="next-steps"></a>Nächste Schritte
Im Abschnitt [Systemthemen in Azure Event Grid](system-topics.md) erfahren Sie mehr über Systemthemen und Thementypen, die von Azure Event Grid unterstützt werden. 
