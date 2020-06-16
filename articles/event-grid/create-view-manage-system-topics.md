---
title: Erstellen, Anzeigen und Verwalten von Systemthemen in Azure Event Grid
description: In diesem Artikel wird gezeigt, wie Sie vorhandene Systemthemen anzeigen und Azure Event Grid-Systemthemen mithilfe des Azure-Portals erstellen können.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: spelluru
ms.openlocfilehash: ac22afb351973397960e66c2a8fe86031e0b213a
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84316341"
---
# <a name="create-view-and-manage-system-topics-in-azure-event-grid"></a>Erstellen, Anzeigen und Verwalten von Systemthemen in Azure Event Grid
In diesem Artikel wird gezeigt, wie Sie Folgendes durchführen:

- Erstellen eines Systemthemas
- Anzeigen aller vorhandenen Systemthemen 
- Löscht ein Systemthema
- Erstellen eines Ereignisabonnements für ein Systemthema


## <a name="create-a-system-topic"></a>Erstellen eines Systemthemas
Sie können ein Systemthema für eine Azure-Ressource auf zwei Arten erstellen:

- Mithilfe der Ressourcenseite, z. B. der Seite "Speicherkonto" oder "Event Hubs-Namespace". 
- Mithilfe der Seite **Event Grid-Systemthemen**. 

Ein Beispiel für die Erstellung eines Systemthemas unter Verwendung einer Ressourcenseite finden Sie in dieser [Schnellstartanleitung](blob-event-quickstart-portal.md) (Registerkarte **Ereignisse** einer Ressourcenseite im Azure-Portal). Die folgenden Schritte dienen zum Erstellen eines Systemthemas unter Verwendung der Seite **Event Grid-Systemthemen**. 

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

> [!NOTE]
> Wenn Sie zuvor ein Abonnement für ein Ereignis erstellt haben, das durch Azure-Quellen ausgelöst wurde, hat der Event Grid-Dienst automatisch ein Systemthema mit einem zufällig generierten Namen erstellt. Jetzt können Sie beim Erstellen des Themas einen Namen für das Systemthema angeben. Sie können diese Systemthemaressource verwenden, um Metriken und Diagnoseprotokolle zu ermitteln.

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
