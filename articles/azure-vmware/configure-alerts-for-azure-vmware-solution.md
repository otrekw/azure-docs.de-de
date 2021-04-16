---
title: Konfigurieren von Warnungen und arbeiten mit Metriken in Azure VMware Solution
description: Erfahren Sie, wie Benachrichtigungen zum Empfangen von Benachrichtigungen verwendet werden. Außerdem erfahren Sie mehr über das Arbeiten mit Metriken, um tiefere Erkenntnisse in Ihre Azure VMware-Lösung Private Cloud zu erhalten.
ms.topic: how-to
ms.date: 04/02/2021
ms.openlocfilehash: f021662658399111187e9963fc5caec434fabf4a
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106096667"
---
# <a name="configure-azure-alerts-in-azure-vmware-solution"></a>Konfigurieren von Azure-Warnungen in Azure VMware Solution 

In diesem Artikel erfahren Sie, wie Sie [Azure-Aktionsgruppen](/azure/azure-monitor/alerts/action-groups) in [Microsoft Azure Warnungen](/azure/azure-monitor/alerts/alerts-overvie) konfigurieren, um Benachrichtigungen über ausgelöste Ereignisse zu erhalten, die Sie definieren. Außerdem erfahren Sie mehr über die Verwendung von [Azure Monitor Metriken](/azure/azure-monitor/essentials/data-platform-metrics), um tiefere Erkenntnisse in Ihre Azure VMware-Lösung Private Cloud zu erhalten.


## <a name="supported-metrics-and-activities"></a>Unterstützte Metriken und Aktivitäten

Die folgenden Metriken sind über Azure Monitor Metriken sichtbar.

| **Signalname**                                                         | **Signaltyp** | **Überwachungsdienst** |
|-------------------------------------------------------------------------|-----------------|---------------------|
| Gesamtkapazität des Datenträgers mit dem Datenspeicher                                           | Metrik          | Plattform            |
| Prozentsatz des verwendeten Datenträgers mit dem Datenspeicher                                          | Metrik          | Plattform            |
| CPU-Prozentsatz                                                          | Metrik          | Plattform            |
| Durchschnittlicher effektiver Arbeitsspeicher                                                | Metrik          | Plattform            |
| Durchschnittlicher Overhead des Arbeitsspeichers                                                 | Metrik          | Plattform            |
| Durchschnittlicher gesamter Arbeitsspeicher                                                    | Metrik          | Plattform            |
| Durchschnittliche Arbeitsspeicherauslastung                                                    | Metrik          | Plattform            |
| Verwendeter Datenträger mit dem Datenspeicher                                                     | Metrik          | Plattform            |
| Alle Verwaltungsvorgänge                                           | Aktivitätsprotokoll    | Administrative      |
| Registrieren des Microsoft.AVS-Ressourcenanbieters. (Microsoft.AVS/privateClouds) | Aktivitätsprotokoll    | Administrative      |
| Eine PrivateCloud erstellen oder aktualisieren. (Microsoft.AVS/privateClouds)          | Aktivitätsprotokoll    | Administrative      |
| Eine PrivateCloud erstellen oder aktualisieren. (Microsoft.AVS/privateClouds)                    | Aktivitätsprotokoll    | Administrative      |

## <a name="configure-an-alert-rule"></a>Eine Benachrichtigungsregel konfigurieren
1. Wählen Sie auf der Private Cloud Azure VMware-Lösung die Option **Monitor**  >  **Warnungen** und dann **Neue Benachrichtigungsregel** aus.
 
   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/create-new-alert-rule.png" alt-text="Screenshot, der zeigt, wo Sie eine Warnungsregel in ihrer Azure VMware-Lösung Private Cloud konfigurieren können." lightbox="media/configure-alerts-for-azure-vmware-solution/create-new-alert-rule.png":::

   Ein neuer Konfigurationsbildschirm wird geöffnet, auf dem Sie Folgendes ausführen:
   - Definieren des Umfangs
   - Konfigurieren einer Bedingung
   - Einrichten der Aktionsgruppe
   - Festlegen der Details der Benachrichtigungsregel
    
   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/create-alert-rule-details.png" alt-text="Screenshot, der das Fenster „Warnungsregel erstellen“ zeigt." lightbox="media/configure-alerts-for-azure-vmware-solution/create-alert-rule-details.png":::

1. Wählen Sie unter **Umfang**, die Zielressource aus, die Sie überwachen möchten. Standardmäßig wurde die Azure VMware-Lösung Private Cloud, über die Sie das Menü Warnungen geöffnet haben, definiert.

1. Wählen Sie unter **Bedingung** die Option **Bedingung hinzufügen** aus, und wählen Sie im Fenster, das geöffnet wird, das Signal aus, das Sie für die Warnungsregel erstellen möchten. 

   In unserem Beispiel haben wir den **Prozentsatz des verwendeten Datenträger DataStore-Daten** Trägers ausgewählt, der aus einer SLA-Perspektive der [Azure VMware-Lösung](https://aka.ms/avs/sla) relevant ist. 

   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/configure-signal-logic-options.png" alt-text="Screenshot, der das Fenster Signallogik konfigurieren mit vordefinierten Signalnamen anzeigt."::: 

1. Definieren Sie die Logik, durch die die Warnung ausgelöst wird, und wählen Sie dann **abgeschlossen** aus. 

   In unserem Beispiel wurden nur der **Schwellenwert** und die **Frequenz der Auswertung** angepasst. 
   
   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/define-alert-logic-threshold.png" alt-text="Screenshot, der die Informationen für die ausgewählte Signallogik zeigt."::: 

1. Klicken Sie unter **Aktionen** auf **Add action groups** (Aktionsgruppen auswählen). Die Aktionsgruppe definiert, *wie* die Benachrichtigung empfangen wird und *wer* Sie empfängt.   Benachrichtigungen können per e-Mail, SMS, [Azure Mobile App-Pushbenachrichtigung](https://azure.microsoft.com/features/azure-portal/mobile-app/) oder Sprachnachricht empfangen werden.
 
   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/create-action-group.png" alt-text="Screenshot, der die vorhandenen Aktionsgruppen und den Speicherort für die Erstellung einer neuen Aktionsgruppe anzeigt.":::

1. Wählen Sie im Fenster, das geöffnet wird, **Aktionsgruppe erstellen** aus.

   >[!TIP]
   > Sie können auch eine vorhandene Aktionsgruppe auswählen.

   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/select-action-group-for-alert-rule.png" alt-text="Screenshot, der die Aktionsgruppen anzeigt, die für die Warnung ausgewählt werden sollen."::: 

 

 
1. Weisen Sie in dem Fenster, das geöffnet wird, auf der Registerkarte **Grundlagen** der Aktionsgruppe einen Namen und einen Anzeigenamen zu.

1. Wählen Sie auf der Registerkarte **Benachrichtigungen** den **Benachrichtigungstyp** und **Name** aus. Klicken Sie anschließend auf **OK**.

   Unser Beispiel basiert auf einer E-Mail-Benachrichtigung.

   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/create-action-group-notification-settings.png" alt-text="Screenshot mit den Einstellungen für E-Mail, SMS, Push und Spracheinstellungen für die Warnung." lightbox="media/configure-alerts-for-azure-vmware-solution/create-action-group-notification-settings.png":::     

1. (Optional) Konfigurieren Sie die **Aktionen**, wenn Sie proaktive Aktionen ausführen und Benachrichtigungen zu dem Ereignis erhalten möchten. Wählen Sie einen verfügbaren **Aktionstyp** aus, und wählen Sie dann **überprüfen + erstellen**. 
   - Automation Runbooks
   - Azure Functions – für benutzerdefinierte, ereignisgesteuerte serverlose Codeausführung
   - ITSM – für die Integration in einen Dienstanbieter wie ServiceNow zum Erstellen eines Tickets
   - Logik-App: für eine komplexere Workflow Orchestrierung
   - Webhooks: so wird ein Prozess in einem anderen Dienst auslöst

   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/create-action-group-action-type.png" alt-text="Screenshot, der das Fenster „Aktionsgruppe erstellen“ mit dem Fokus auf die Dropdown-Datei „Aktionstyp“ zeigt." lightbox="media/configure-alerts-for-azure-vmware-solution/create-action-group-action-type.png":::     

1. Geben Sie unter den **Details der Alarmregel** einen Namen, eine Beschreibung, eine Ressourcengruppe zum Speichern der Alarmregel und den Schweregrad an. Wählen Sie **Warnungsregel erstellen** aus.
   
   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/alert-rule-details.png" alt-text="Screenshot, der die Details für die Benachrichtigungsregel anzeigt."::: 
 
   Die Warnungsregel ist sichtbar und kann über die Azure-Portal verwaltet werden.

   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/existing-alert-rule.png" alt-text="Screenshot, der die neue Warnungsregel im Fenster „Regeln“ anzeigt." lightbox="media/configure-alerts-for-azure-vmware-solution/existing-alert-rule.png":::     

   Sobald eine Metrik den in einer Warnungsregel definierten Schwellenwert erreicht, wird das Menü **Warnungen** aktualisiert und sichtbar gemacht.

   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/threshold-alert.png" alt-text="Screenshot, der die Warnung anzeigt, nachdem der definierte Schwellenwert erreicht wurde." lightbox="media/configure-alerts-for-azure-vmware-solution/threshold-alert.png":::     

   Abhängig von der konfigurierten Aktionsgruppe erhalten Sie eine Benachrichtigung über das konfigurierte Medium. In unserem Beispiel haben wir die e-Mail-Adresse konfiguriert.
    
   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/alert-notification.png" alt-text="Screenshot einer Azure Monitor Warnung mit der Fehlerzeichenfolge und dem Ereignis „Datum und Uhrzeit“ ausgelöst."::: 

## <a name="work-with-metrics"></a>Überwachen mit Metriken

1. Wählen Sie in Ihrer Azure VMware Solution Private Cloud die Option **Monitoring** > **Metriken**. Wählen Sie dann die gewünschte Metrik aus der Dropdown-Dropdown-Option aus.
    
   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/monitoring-metrics.png" alt-text="Screenshot, der das Fenster „Metriken“ und einen Fokus auf das Dropdown Feld „Metrik“ zeigt." lightbox="media/configure-alerts-for-azure-vmware-solution/monitoring-metrics.png":::   

1. Sie können die Parameter des Diagramms ändern, z. b. den **Zeitbereich** oder die **Granularität der Zeit**. 

   Folgende andere Optionen sind verfügbar:
   - **Logs anbohren** und die Daten im zugehörigen Log Analytics-Arbeitsbereich abfragen
   - **Heften Sie dieses Diagramm** der Einfachheit halber an ein Azure-Dashboard.

   :::image type="content" source="media/configure-alerts-for-azure-vmware-solution/monitoring-metrics-time-range-granularity.png" alt-text="Screenshot, der die Optionen für Zeitbereich und Granularität der Zeit für die Metrik anzeigt." lightbox="media/configure-alerts-for-azure-vmware-solution/monitoring-metrics-time-range-granularity.png":::  
 
 
## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun eine Alarmregel für Ihre Azure VMware Solution Private Cloud konfiguriert haben, möchten Sie vielleicht noch mehr darüber erfahren:
- [Azure Monitor-Metriken](/azure/azure-monitor/essentials/data-platform-metrics)
- [Azure Monitor-Warnungen](/azure/azure-monitor/alerts/alerts-overview)
- [Azure-Aktionsgruppen](/azure/azure-monitor/alerts/action-groups)

Sie können auch mit einer der anderen Azure- [Lösungen](index.yml) für die VMware-Lösung fortfahren.





