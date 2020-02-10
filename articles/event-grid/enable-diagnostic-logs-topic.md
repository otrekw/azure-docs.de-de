---
title: 'Azure Event Grid: Aktivieren von Diagnoseprotokollen für ein Thema'
description: Dieser Artikel enthält schrittweise Anleitungen zum Aktivieren von Diagnoseprotokollen für ein Azure Event Grid-Thema.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: how-to
ms.date: 01/30/2020
ms.author: spelluru
ms.openlocfilehash: a32d26629bb9efed4a4f1f49eee2f0534e1873a9
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76960203"
---
#  <a name="diagnostic-logs-for-an-azure-event-grid-topic"></a>Diagnoseprotokolle für ein Azure Event Grid-Thema
Mit Diagnoseeinstellungen können Benutzer von Event Grid Fehlerprotokolle zu Veröffentlichungs- und Übermittlungsfehlern mit einer der folgenden Ressourcen erfassen und anzeigen: mit einem Azure-Speicherkonto, einem Event Hub oder einem Log Analytics-Arbeitsbereich. Dieser Artikel enthält schrittweise Anleitungen zum Aktivieren von Diagnoseprotokollen für ein Event Grid-Thema.

## <a name="prerequisites"></a>Voraussetzungen

- Ein bereitgestelltes Event Grid-Thema
- Ein bereitgestelltes Ziel zum Erfassen von Diagnoseprotokollen Es kann sich um eines der folgenden Ziele handeln:
    - Azure-Speicherkonto
    - Event Hub
    - Log Analytics-Arbeitsbereich


## <a name="steps-for-enabling-diagnostic-logs-for-a-topic"></a>Schritte zum Aktivieren von Diagnoseprotokollen für ein Thema

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an.
2. Navigieren Sie zum Event Grid-Thema, für das Sie Diagnoseprotokolleinstellungen aktivieren möchten. 
3. Wählen Sie unter **Überwachung** im linken Menü die Option **Diagnoseeinstellungen** aus.
4. Wählen Sie auf der Seite **Diagnoseeinstellungen** den Eintrag **Diagnoseeinstellung hinzufügen** aus. 
    
    ![Schaltfläche „Diagnoseeinstellung hinzufügen“](./media/enable-diagnostic-logs-topic/diagnostic-settings-add.png)
5. Geben Sie einen **Namen** für die Diagnoseeinstellung an. 

    ![Diagnoseeinstellungen: Name](./media/enable-diagnostic-logs-topic/diagnostic-settings-name.png)     
6. Aktivieren Sie mindestens eines der Erfassungsziele für die Protokolle, und konfigurieren Sie diese dann, indem Sie eine zuvor erstellte Erfassungsressource auswählen. 
    - Wenn Sie **In ein Speicherkonto archivieren** auswählen, wählen Sie **Speicherkonto - Konfigurieren** aus, und wählen Sie dann das Speicherkonto in Ihrem Azure-Abonnement aus. 

        ![Archivieren in ein Azure-Speicherkonto](./media/enable-diagnostic-logs-topic/archive-storage.png)
    - Wenn Sie **An einen Event Hub streamen** auswählen, wählen Sie **Event Hub - Konfigurieren** aus, und wählen Sie dann den Event Hubs-Namespace, den Event Hub und die Zugriffsrichtlinie aus. 
        ![An einen Event Hub streamen](./media/enable-diagnostic-logs-topic/archive-event-hub.png)
    - Wenn Sie **An Log Analytics senden** auswählen, wählen Sie den Log Analytics-Arbeitsbereich aus.
        ![An Log Analytics senden](./media/enable-diagnostic-logs-topic/send-log-analytics.png)
7. Wählen Sie im Abschnitt **Protokoll** die Optionen **DeliveryFailures** und **PublishFailures** aus. 
    ![Auswählen der Fehler](./media/enable-diagnostic-logs-topic/log-failures.png)
8. Wählen Sie **Speichern** aus. Wählen Sie in der rechten oberen Ecke **X** aus, um die Seite zu schließen. 
9. Vergewissern Sie sich nun auf der Seite **Diagnoseeinstellungen**, dass ein neuer Eintrag in der Tabelle **Diagnoseeinstellungen** angezeigt wird. 
    ![Diagnoseeinstellung in der Liste](./media/enable-diagnostic-logs-topic/diagnostic-setting-list.png)

     Sie können auch die Erfassung aller Metriken für das Thema aktivieren. 

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Hilfe benötigen, veröffentlichen Sie Ihr Problem im [Stack Overflow-Forum](https://stackoverflow.com/questions/tagged/azure-eventgrid), oder öffnen Sie ein [Supportticket](https://azure.microsoft.com/support/options/). 
