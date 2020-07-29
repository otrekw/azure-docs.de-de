---
title: Exportieren von Azure Security Center-Warnungen und -Empfehlungen in SIEMs | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie den fortlaufenden Export von Sicherheitswarnungen und -empfehlungen in SIEMs einrichten.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: memildin
ms.openlocfilehash: 7b0fbb7c4f680f9d732a63fff7b0b317c6cf1511
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86519696"
---
# <a name="export-security-alerts-and-recommendations"></a>Exportieren von Sicherheitswarnungen und -empfehlungen

Azure Security Center generiert detaillierte Sicherheitswarnungen und -empfehlungen. Sie können diese im Portal oder über programmgesteuerte Tools anzeigen. Möglicherweise müssen Sie diese Informationen auch exportieren oder an andere Überwachungstools in Ihrer Umgebung senden. 

In diesem Artikel werden die Tools beschrieben, mit denen Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich fortlaufend exportieren können.

Mit diesen Tools können Sie folgende Aktionen ausführen:

* Fortlaufender Export in Log Analytics-Arbeitsbereiche
* Fortlaufender Export in Azure Event Hubs (für Integrationen mit Drittanbieter-SIEMs)
* Export in das CSV-Format (einmalig)



## <a name="availability"></a>Verfügbarkeit

- Status des Release: **Allgemein verfügbar**
- Erforderliche Rollen und Berechtigungen:
    - **Leser** für das Abonnement, das die Exportkonfiguration enthält
    - **Rolle „Sicherheitsadministrator“** für die Ressourcengruppe (oder **Besitzer**)
    - Außerdem sind Schreibberechtigungen für die Zielressource erforderlich
- Clouds:   ✔ Kommerzielle Clouds   ✔ US Gov   ✘ China Gov, andere Gov-Cloud


## <a name="setting-up-a-continuous-export"></a>Einrichten eines fortlaufenden Exports

Die folgenden Schritte sind unabhängig davon erforderlich, ob Sie einen fortlaufenden Export in einen Log Analytics-Arbeitsbereich oder in Azure Event Hubs einrichten.

1. Wählen Sie auf der Security Center-Randleiste **Preise & Einstellungen** aus.

1. Wählen Sie das Abonnement aus, für das Sie den Datenexport konfigurieren möchten.
    
1. Wählen Sie auf der Randleiste der Seite „Einstellungen“ für dieses Abonnement **Fortlaufender Export** aus.

    [![Exportoptionen in Azure Security Center](media/continuous-export/continuous-export-options-page.png)](media/continuous-export/continuous-export-options-page.png#lightbox) Hier werden die Exportoptionen angezeigt. Es gibt eine Registerkarte für jedes verfügbare Exportziel. 

1. Wählen Sie den Datentyp, den Sie exportieren möchten, und dann Filter für die einzelnen Typen aus (z. B. nur Warnungen mit hohem Schweregrad exportieren).

1. Wählen Sie im Bereich „Exportziel“ aus, wo die Daten gespeichert werden sollen. Daten können in einem Ziel in einem anderen Abonnement gespeichert werden (z.B. in einer Central Event Hub-Instanz oder in einem zentralen Log Analytics-Arbeitsbereich).

1. Klicken Sie auf **Speichern**.



## <a name="configuring-siem-integration-via-azure-event-hubs"></a>Konfigurieren der SIEM-Integration über Azure Event Hubs

Azure Event Hubs eignet sich hervorragend, um Streamingdaten programmgesteuert zu nutzen. Für Azure Security Center-Warnungen und Empfehlungen ist dies die bevorzugte Methode für eine SIEM-Drittanbieterintegration.

> [!NOTE]
> Die effektivste Methode zum Streamen von Überwachungsdaten an externe Tools ist in den meisten Fällen die Verwendung von Azure Event Hubs. [Dieser Artikel](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs) enthält eine kurze Beschreibung der Vorgehensweise beim Streamen von Überwachungsdaten aus verschiedenen Quellen an einen Event Hub und Links zu ausführlichen Anleitungen.

> [!NOTE]
> Wenn Sie Security Center-Warnungen zuvor mithilfe des Azure-Aktivitätsprotokolls in eine SIEM-Lösung exportiert haben, wird diese Methodik durch das folgende Verfahren ersetzt.

Die Ereignisschemas der exportierten Datentypen finden Sie bei den [Event Hub-Ereignisschemas](https://aka.ms/ASCAutomationSchemas).


### <a name="to-integrate-with-a-siem"></a>So führen Sie die Integration in eine SIEM-Lösung durch 

Nachdem Sie den fortlaufenden Export der ausgewählten Security Center-Daten in Azure Event Hubs konfiguriert haben, können Sie den entsprechenden Connector für Ihre SIEM-Lösung einrichten.

* **Azure Sentinel** – Verwenden Sie den hier angebotenen nativen [Datenconnector](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center) für Azure Security Center-Warnungen.
* **Splunk**: Verwenden Sie das [Azure Monitor-Add-On für Splunk](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md)
* **IBM QRadar**: Verwenden Sie [eine manuell konfigurierte Protokollquelle](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/com.ibm.dsm.doc/t_dsm_guide_microsoft_azure_enable_event_hubs.html)
* **ArcSight**: Verwenden Sie [SmartConnector](https://community.microfocus.com/t5/ArcSight-Connectors/SmartConnector-for-Microsoft-Azure-Monitor-Event-Hub/ta-p/1671292)

Wenn Sie die kontinuierlich exportierten Daten automatisch aus dem konfigurierten Event Hub in Azure Data Explorer verschieben möchten, befolgen Sie zudem die Anweisungen unter [Erfassen von Daten aus Event Hub in Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/ingest-data-event-hub).



## <a name="continuous-export-to-a-log-analytics-workspace"></a>Fortlaufender Export in einen Log Analytics-Arbeitsbereich

Wenn Sie Azure Security Center-Daten in einem Log Analytics-Arbeitsbereich analysieren oder Azure-Warnungen gemeinsam mit Security Center verwenden möchten, richten Sie den fortlaufenden Export in Ihren Log Analytics-Arbeitsbereich ein.

Um Daten in einen Log Analytics-Arbeitsbereich exportieren zu können, müssen in Ihrem Arbeitsbereich Log Analytics-Lösungen von Security Center aktiviert sein. Wenn Sie das Azure-Portal verwenden, wird die Security Center-Lösung im Free-Tarif automatisch aktiviert, sobald Sie den fortlaufenden Export aktivieren. Wenn Sie die Einstellungen für den fortlaufenden Export jedoch programmgesteuert konfigurieren, müssen Sie den Tarif „Free“ oder „Standard“ für den erforderlichen Arbeitsbereich in **Preise & Einstellungen** manuell auswählen.  

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

Wenn Sie einen CSV-Bericht für Warnungen oder Empfehlungen herunterladen möchten, öffnen Sie die Seite **Sicherheitswarnungen** oder **Empfehlungen**, und klicken Sie auf **CSV-Bericht herunterladen**.

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
