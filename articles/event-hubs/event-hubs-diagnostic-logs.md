---
title: Einrichten der Diagnoseprotokolle – Azure Event Hubs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Aktivitäts- und Diagnoseprotokolle für Event Hubs in Azure einrichten.
ms.topic: article
ms.date: 06/13/2021
ms.openlocfilehash: ecdd6d9bda9f468bf5ad1510971437838fb75ff8
ms.sourcegitcommit: 5163ebd8257281e7e724c072f169d4165441c326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2021
ms.locfileid: "112417197"
---
# <a name="set-up-diagnostic-logs-for-an-azure-event-hub"></a>Einrichten von Diagnoseprotokollen für Azure Event Hubs

Sie können zwei Typen von Protokollen für Azure Event Hubs anzeigen:

* **[Aktivitätsprotokolle](../azure-monitor/essentials/platform-logs-overview.md)** : Diese Protokolle enthalten Informationen zu Vorgängen, die für einen Auftrag ausgeführt werden. Diese Protokolle sind immer aktiviert. Aktivitätsprotokolleinträge können angezeigt werden, indem Sie **Aktivitätsprotokoll** im linken Bereich für Ihren Event Hub-Namespace im Azure-Portal auswählen. Beispiel: „Namespace erstellen oder aktualisieren“, „Event Hub erstellen oder aktualisieren“.

    ![Aktivitätsprotokoll für einen Event Hubs-Namespace](./media/event-hubs-diagnostic-logs/activity-log.png)
* **[Diagnoseprotokolle](../azure-monitor/essentials/platform-logs-overview.md)** : Diagnoseprotokolle bieten ausführlichere Informationen zu Vorgängen und Aktionen, die für Ihren Namespace mithilfe der API oder über Verwaltungsclients im Sprach-SDK ausgeführt werden. 
    
    Im folgenden Abschnitt erfahren Sie, wie Sie Diagnoseprotokolle für einen Event Hubs-Namespace aktivieren.

## <a name="enable-diagnostic-logs"></a>Aktivieren von Diagnoseprotokollen
Diagnoseprotokolle sind standardmäßig deaktiviert. Führen Sie folgende Schritte aus, um die Diagnoseprotokolle zu aktivieren:

1.  Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Event Hubs-Namespace. 
2. Wählen Sie im linken Bereich **Diagnoseeinstellungen** unter **Überwachung** aus, und wählen Sie dann **+ Diagnoseeinstellung hinzufügen** aus. 

    ![Seite „Diagnoseeinstellungen“: „Diagnoseeinstellung hinzufügen“](./media/event-hubs-diagnostic-logs/diagnostic-settings-page.png)
4. Wählen Sie im Abschnitt **Kategoriedetails** die **Typen von Diagnoseprotokollen** aus, die Sie aktivieren möchten. Weitere Informationen zu diesen Kategorien finden Sie weiter unten in diesem Artikel. 
5. Legen Sie im Abschnitt **Zieldetails** das gewünschte Archivierungsziel (Ziel) fest, beispielsweise ein Speicherkonto, einen Event Hub oder einen Log Analytics-Arbeitsbereich.

    ![Seite „Diagnoseeinstellungen hinzufügen“](./media/event-hubs-diagnostic-logs/aDD-diagnostic-settings-page.png)
6.  Wählen Sie auf der Symbolleiste **Speichern** aus, um die Diagnoseeinstellungen zu speichern.

    Neue Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel im Bereich **Diagnoseprotokolle** angezeigt.

    Weitere Informationen zum Konfigurieren der Diagnose finden Sie in der [Übersicht über Azure-Diagnoseprotokolle](../azure-monitor/essentials/platform-logs-overview.md).

## <a name="diagnostic-logs-categories"></a>Diagnoseprotokollkategorien
[!INCLUDE [event-hubs-diagnostic-log-schema](./includes/event-hubs-diagnostic-log-schema.md)]



## <a name="next-steps"></a>Nächste Schritte
- [Einführung in Event Hubs](./event-hubs-about.md)
- [Event Hubs-Beispiele](sdks.md)
- Erste Schritte mit Event Hubs
    - [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
    - [Java](event-hubs-java-get-started-send.md)
    - [Python](event-hubs-python-get-started-send.md)
    - [JavaScript](event-hubs-node-get-started-send.md)
