---
title: Exportieren von Azure Security Center-Warnungen und -Empfehlungen in SIEMs | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie den fortlaufenden Export von Sicherheitswarnungen und -empfehlungen in SIEMs einrichten.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: e12fc5d92cfc850e1d049bc11286c0c863e718b0
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75459192"
---
# <a name="export-security-alerts-and-recommendations-preview"></a>Exportieren von Sicherheitswarnungen und -empfehlungen (Vorschau)

Azure Security Center generiert detaillierte Sicherheitswarnungen und -empfehlungen. Sie können diese im Portal oder über programmgesteuerte Tools anzeigen. Möglicherweise müssen Sie diese Informationen auch exportieren oder an andere Überwachungstools in Ihrer Umgebung senden. 

In diesem Artikel wird der Satz von Tools (Vorschau) beschrieben, mit denen Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich fortlaufend exportieren können.

Mit diesen Tools können Sie folgende Aktionen ausführen:

* Generieren ausführlicher Berichte als CSV
* Exportieren in Log Analytics-Arbeitsbereiche
* Exportieren in Azure Event Hubs (für Integrationen mit Drittanbieter-SIEMs)

## <a name="setting-up-a-continuous-export"></a>Einrichten eines fortlaufenden Exports

1. Klicken Sie auf der Security Center-Randleiste auf **Preise & Einstellungen**.

1. Wählen Sie das Abonnement aus, für das Sie den Datenexport konfigurieren möchten.
    
1. Wählen Sie in der Randleiste der Seite „Einstellungen“ für dieses Abonnement **Fortlaufender Export (Vorschau)** aus.

    [![Exportoptionen in Azure Security Center](media/continuous-export/continuous-export-options-page.png)](media/continuous-export/continuous-export-options-page.png#lightbox) Hier werden die Exportoptionen angezeigt. Es gibt eine Registerkarte für jedes verfügbare Exportziel. 

1. Wählen Sie den Datentyp, den Sie exportieren möchten, und dann Filter für die einzelnen Typen aus (z.B. nur Warnungen mit hohem Schweregrad exportieren).

1. Wählen Sie im Bereich „Exportziel“ aus, wo die Daten gespeichert werden sollen. Daten können in einem Ziel in einem anderen Abonnement gespeichert werden (z.B. in einer Central Event Hub-Instanz oder in einem zentralen Log Analytics-Arbeitsbereich).

1. Klicken Sie auf **Speichern**.

## <a name="continuous-export-through-azure-event-hubs"></a>Fortlaufender Export über Azure Event Hubs  

> [!NOTE]
> Die effektivste Methode zum Streamen von Überwachungsdaten an externe Tools ist in den meisten Fällen die Verwendung von Azure Event Hubs. [Dieser Artikel](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs) enthält eine kurze Beschreibung der Vorgehensweise beim Streamen von Überwachungsdaten aus verschiedenen Quellen an einen Event Hub und Links zu ausführlichen Anleitungen.

> [!NOTE]
> Wenn Sie Security Center-Warnungen zuvor mithilfe des Azure-Aktivitätsprotokolls in eine SIEM-Lösung exportiert haben, wird diese Methodik durch das folgende Verfahren ersetzt.

Die Ereignisschemas der exportierten Datentypen finden Sie bei den [Event Hub-Ereignisschemas](https://aka.ms/ASCAutomationSchemas).

### <a name="to-integrate-with-a-siem"></a>So führen Sie die Integration in eine SIEM-Lösung durch 

Nachdem Sie den fortlaufenden Export der ausgewählten Security Center-Daten in Azure Event Hubs konfiguriert haben, können Sie anhand der folgenden Anweisungen den entsprechenden Connector in Ihrer SIEM-Lösung einrichten.

Befolgen Sie die Anweisungen für Ihre SIEM-Lösung auf [dieser Seite](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/?cdn=disable), und verwenden Sie den relevanten Connector:

* **Splunk**: Verwenden Sie das [Azure Monitor-Add-On für Splunk](https://splunkbase.splunk.com/app/3534/)
* **IBM QRadar**: Verwenden Sie [eine manuell konfigurierte Protokollquelle](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/com.ibm.dsm.doc/t_logsource_microsoft_azure_event_hubs.html)
* **ArcSight**: Verwenden Sie [SmartConnector](https://community.microfocus.com/t5/ArcSight-Connectors/SmartConnector-for-Microsoft-Azure-Monitor-Event-Hub/ta-p/1671292)

Wenn Sie **Azure Sentinel** einsetzen, verwenden Sie den hier angebotenen nativen [Datenconnector](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center) für Azure Security Center-Warnungen.

Wenn Sie die kontinuierlich exportierten Daten automatisch aus dem konfigurierten Event Hub in Azure Data Explorer verschieben möchten, befolgen Sie zudem die Anweisungen unter [Erfassen von Daten aus Event Hub in Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/ingest-data-event-hub).


## <a name="continuous-export-to-a-log-analytics-workspace"></a>Fortlaufender Export in einen Log Analytics-Arbeitsbereich

Um Daten in einen Log Analytics-Arbeitsbereich exportieren zu können, müssen Sie in Ihrem Arbeitsbereich Log Analytics-Lösungen im Security Center-Tarif „Free“ oder „Standard“ aktiviert haben. Wenn Sie das Azure-Portal verwenden, wird die Security Center-Lösung im Free-Tarif automatisch aktiviert, sobald Sie den fortlaufenden Export aktivieren. Wenn Sie die Einstellungen für den fortlaufenden Export jedoch programmgesteuert konfigurieren, müssen Sie den Tarif „Free“ oder „Standard“ für den erforderlichen Arbeitsbereich in **Preise & Einstellungen** manuell auswählen.  

### <a name="log-analytics-tables-and-schemas"></a>Log Analytics-Tabellen und -Schemas

Sicherheitswarnungen und -empfehlungen werden in den Tabellen *SecurityAlert* und *SecurityRecommendations* gespeichert. Der Name der Log Analytics-Lösung, in der diese Tabellen enthalten sind, hängt davon ab, ob Sie den Tarif „Free“ oder „Standard“ nutzen (siehe [Preise](security-center-pricing.md)): Security('Security and Audit') oder SecurityCenterFree.

![Die Tabelle *SecurityAlert* in Log Analytics](./media/continuous-export/log-analytics-securityalert-solution.png)

Die Ereignisschemas der exportierten Datentypen finden Sie bei den [Log Analytics-Tabellenschemas](https://aka.ms/ASCAutomationSchemas).

###  <a name="view-exported-security-alerts-and-recommendations-in-azure-monitor"></a>Anzeigen von exportierten Sicherheitswarnungen und Empfehlungen in Azure Monitor

In einigen Fällen können Sie die exportierten Sicherheitswarnungen und/oder Empfehlungen in [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview) anzeigen. 

Azure Monitor bietet eine einheitliche Warnungsbenutzeroberfläche für eine Vielzahl von Azure-Warnungen, einschließlich Diagnoseprotokoll, Metrikwarnungen und benutzerdefinierte Warnungen, die auf Log Analytics-Arbeitsbereichsabfragen basieren.

Konfigurieren Sie eine Warnungsregel, die auf Log Analytics-Abfragen (Protokollwarnung) basiert, um Warnungen und Empfehlungen von Security Center in Azure Monitor anzuzeigen:

1. Klicken Sie auf der Seite **Warnungen** von Azure Monitor auf **Neue Warnungsregel**.

    ![Seite „Warnungen“ von Azure Monitor](./media/continuous-export/azure-monitor-alerts.png)

1. Konfigurieren Sie auf der Seite „Regel erstellen“ Ihre neue Regel (auf die gleiche Weise, wie Sie eine [Protokollwarnungsregel in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log) konfigurieren würden):

    * Wählen Sie als **Ressource** den Log Analytics-Arbeitsbereich aus, in den Sie Sicherheitswarnungen und Empfehlungen exportiert haben.

    * Wählen Sie als **Bedingung** die Option **Benutzerdefinierte Protokollsuche** aus. Konfigurieren Sie auf der Seite, die angezeigt wird, die Abfrage, den Rückblickzeitraum und den Häufigkeitszeitraum. In der Suchabfrage können Sie *SecurityAlert* oder *SecurityRecommendation* eingeben, um die Datentypen abzufragen, in die Security Center kontinuierlich exportiert, wenn Sie die Funktion „Fortlaufender Export in Log Analytics“ aktivieren. 
    
    * Konfigurieren Sie optional die [Aktionsgruppe](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups), die Sie auslösen möchten. Aktionsgruppen können das Senden von E-Mails-, ITSM-Tickets, WebHooks und vieles mehr auslösen.
    ![Azure Monitor-Warnungsregel](./media/continuous-export/azure-monitor-alert-rule.png)

Nun sehen Sie in Azure Monitor-Warnungen neue Azure Security Center-Warnungen oder -Empfehlungen (abhängig von Ihrer Konfiguration), wobei eine Aktionsgruppe automatisch ausgelöst wird (sofern vorhanden).

## <a name="manual-one-time-export-of-security-alerts"></a>Manueller einmaliger Export von Sicherheitswarnungen

Wenn Sie einen CSV-Bericht für Warnungen oder Empfehlungen herunterladen möchten, öffnen Sie die Seite **Sicherheitswarnungen** oder **Empfehlungen**, und klicken Sie auf **CSV-Bericht herunterladen (Vorschau)** .

[![Herunterladen von Warnungsdaten als CSV-Datei](media/continuous-export/download-alerts-csv.png)](media/continuous-export/download-alerts-csv.png#lightbox)

> [!NOTE]
> Diese Berichte enthalten Warnungen und Empfehlungen für Ressourcen in den aktuell ausgewählten Abonnements.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie fortlaufende Exporte ihrer Empfehlungen und Warnungen konfigurieren. Außerdem haben Sie gelernt, wie Sie Ihre Warnungsdaten als CSV-Datei herunterladen. 

Verwandtes Material finden Sie in der folgenden Dokumentation: 

- [Dokumentation zu Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/)
- [Dokumentation zu Azure Sentinel](https://docs.microsoft.com/azure/sentinel/)
- [Azure Monitor-Dokumentation](https://docs.microsoft.com/azure/azure-monitor/)
- [Datentypenschemas für Workflowautomatisierung und fortlaufenden Export](https://aka.ms/ASCAutomationSchemas)
