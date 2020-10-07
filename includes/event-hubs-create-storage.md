---
title: include file
description: include file
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 10/16/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: bf107da82fb3f772a341e70ce472f08ea674a450
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "75692558"
---
### <a name="create-a-storage-account-for-event-processor-host"></a>Erstellen eines Speicherkontos für den Ereignisprozessorhost
Der Ereignisprozessorhost ist ein intelligenter Agent, mit dem das Empfangen von Ereignissen von Event Hubs vereinfacht wird, indem permanente Prüfpunkte und parallele Empfangsvorgänge verwaltet werden. Für das Setzen von Prüfpunkten benötigt der Ereignisprozessorhost ein Speicherkonto. Im folgenden Beispiel wird veranschaulicht, wie Sie ein Speicherkonto erstellen und die zugehörigen Zugriffsschlüssel beziehen:

1. Wählen Sie im Menü des Azure-Portals die Option **Ressource erstellen** aus.

    ![Erstellen eines Ressourcenmenüelements, Microsoft Azure-Portal](./media/event-hubs-create-storage/create-resource.png)

2. Wählen Sie **Storage** > **Speicherkonto** aus.
   
    ![Auswählen eines Speicherkontos, Microsoft Azure-Portal](./media/event-hubs-create-storage/select-storage-account.png)

3. Führen Sie auf der Seite **Speicherkonto erstellen** die folgenden Schritte aus: 

   1. Geben Sie den **Namen des Speicherkontos** ein.
   2. Wählen Sie ein Azure-**Abonnement** aus, das den Event Hub enthält.
   3. Wählen Sie die **Ressourcengruppe** aus, die den Event Hub enthält, oder erstellen Sie sie.
   4. Wählen Sie einen **Speicherort** für die Erstellung der Ressource aus. 
   5. Klicken Sie auf **Überprüfen + erstellen**.
   
        ![Überprüfen + erstellen, Erstellen eines Speicherkontos, Microsoft Azure-Portal](./media/event-hubs-create-storage/review-create.png)

4. Überprüfen Sie auf der Seite **Überprüfen + erstellen** die Werte, und wählen Sie **Erstellen** aus. 

    ![Speicherkontoeinstellungen überprüfen und erstellen, Microsoft Azure-Portal](./media/event-hubs-create-storage/create-storage-account.png)
5. Wenn die Meldung **Bereitstellungen erfolgreich**  in Ihren Benachrichtigungen angezeigt wird, wählen Sie **Zu Ressource wechseln** aus, um die Seite „Speicherkonto“ zu öffnen. Alternativ können Sie **Bereitstellungsdetails** erweitern und dann die neue Ressource in der Ressourcenliste auswählen.  

    ![Zu Ressource wechseln, Speicherkontobereitstellung, Microsoft Azure-Portal](./media/event-hubs-create-storage/go-to-resource.png) 
6. Wählen Sie **Container**aus.

    ![Auswählen des Blobcontainerdiensts, Speicherkonten, Microsoft Azure-Portal](./media/event-hubs-create-storage/select-blob-container-service.png)
7. Wählen Sie oben **+ Container** aus, geben Sie einen **Namen** für den Container ein, und wählen Sie **OK** aus. 

    ![Erstellen eines neuen Blobcontainers, Speicherkonten, Microsoft Azure-Portal](./media/event-hubs-create-storage/create-new-blob-container.png)
8. Wählen Sie im Menü der Seite **Speicherkonto** die Option **Zugriffsschlüssel** aus, und kopieren Sie den Wert von **key1**.

    Speichern Sie die folgenden Werte im Editor oder an einem anderen temporären Speicherort.
    - Name des Speicherkontos
    - Zugriffsschlüssel für das Speicherkonto
    - Name des Containers
