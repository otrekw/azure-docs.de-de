---
title: Streamen von Azure-Überwachungsdaten an einen Event Hub und externe Partner
description: Erfahren Sie, wie Sie Ihre Azure-Überwachungsdaten an einen Event Hub streamen, um die Daten in einem SIEM- oder Analysetool von Partnern abzurufen.
services: azure-monitor
author: bwren
ms.author: bwren
ms.topic: conceptual
ms.date: 07/15/2020
ms.openlocfilehash: 7592935afadc88c4b9e0e5f3c5f9c83d42c63209
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107768739"
---
# <a name="stream-azure-monitoring-data-to-an-event-hub-or-external-partner"></a>Streamen von Azure-Überwachungsdaten an einen Event Hub oder externen Partner

Azure Monitor bietet eine vollständige Lösung für die Stapelüberwachung für Anwendungen und Dienste in Azure, in anderen Clouds und lokal. Sie sollten diese Daten nicht nur mit Azure Monitor analysieren und für verschiedene Überwachungsszenarios nutzen, sondern sie auch an andere Überwachungstools in Ihrer Umgebung senden. In den meisten Fällen ist die effektivste Methode zum Streamen von Überwachungsdaten an externe Tools die Verwendung von [Azure Event Hubs](../../event-hubs/index.yml). Dieser Artikel enthält eine kurze Beschreibung der Vorgehensweise und listet dann einige der Partner auf, an die Sie Daten senden können. Einige davon weisen eine besondere Integration in Azure Monitor auf und können in Azure gehostet werden.  

## <a name="create-an-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespace

Bevor Sie das Streaming für eine Datenquelle konfigurieren, müssen Sie [einen Event Hubs-Namespace und einen Event Hub erstellen](../../event-hubs/event-hubs-create.md). Dieser Namespace und Event Hub sind das Ziel für sämtliche Ihrer Überwachungsdaten. Ein Event Hubs-Namespace ist eine logische Gruppierung von Event Hubs mit derselben Zugriffsrichtlinie, er ähnelt damit einem Speicherkonto mit mehreren Blobs. Beachten Sie die folgenden Hinweise zum Event Hubs-Namespace und zu den Event Hubs, die Sie zum Streamen von Überwachungsdaten verwenden:

* Über die Anzahl der Durchsatzeinheiten können Sie den Durchsatz für Ihre Event Hubs erhöhen. In der Regel ist nur eine Durchsatzeinheit erforderlich. Wenn Sie aufgrund steigender Protokollnutzung hochskalieren müssen, können Sie die Anzahl der Durchsatzeinheiten für den Namespace manuell erhöhen oder die automatische Aufstockung aktivieren.
* Über die Anzahl von Partitionen können Sie den Verbrauch vieler Consumer parallelisieren. Eine einzelne Partition kann bis zu 20 MBit/s oder ungefähr 20.000 Nachrichten pro Sekunde unterstützen. Je nach Tool, das die Daten nutzt, ist es auch möglich, Daten von mehreren Partitionen zu verarbeiten. Wenn Sie sich nicht sicher sind, welche Anzahl von Partitionen festgelegt werden soll, ist es sinnvoll, mit vier Partitionen zu beginnen.
* Legen Sie die Nachrichtenvermerkdauer für Ihren Event Hub auf mindestens 7 Tage fest. Wenn Ihr Tool mehr als einen Tag ausfällt, wird dadurch sichergestellt, dass das Tool für bis zu 7 Tage alte Ereignisse dort fortfahren kann, wo es aufgehört hatte.
* Sie sollten die Standardconsumergruppe für Ihren Event Hub verwenden. Es ist nur dann erforderlich, andere Consumergruppen zu erstellen oder eine separate Consumergruppe zu verwenden, wenn Sie zwei verschiedenen Tools verwenden möchten, um dieselben Daten vom selben Event Hub zu verarbeiten.
* Für das Azure-Aktivitätsprotokoll wählen Sie einen Event Hubs-Namespace aus. Azure Monitor erstellt dann in diesem Namespace den Event Hub _insights-logs-operational-logs_. Für andere Protokolltypen können Sie entweder einen vorhandenen Event Hub verwenden oder Azure Monitor für jede Protokollkategorie einen Event Hub erstellen lassen.
* In der Regel müssen die ausgehenden Ports 5671 und 5672 auf dem Computer oder im VNET offen sein, der bzw. das die Daten vom Event Hub verarbeitet.

## <a name="monitoring-data-available"></a>Verfügbare Überwachungsdaten
Im Artikel [Quellen für Überwachungsdaten für Azure Monitor](../agents/data-sources.md) werden die unterschiedlichen Datenebenen für Azure-Anwendungen und die jeweils verfügbaren Arten von Überwachungsdaten beschrieben. In der folgenden Tabelle werden die einzelnen Ebenen aufgeführt und es wird beschrieben, wie diese Daten an einen Event Hub gestreamt werden können. Über die Links erhalten Sie weitere Informationen.

| Tarif | Daten | Methode |
|:---|:---|:---|
| [Azure-Mandant](../agents/data-sources.md#azure-tenant) | Azure Active Directory-Überwachungsprotokolle | Konfigurieren Sie eine Diagnoseeinstellung für Ihren AAD-Mandanten. Weitere Informationen finden Sie unter [Tutorial: Streamen von Azure Active Directory-Protokollen an einen Azure Event Hub](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md). |
| [Azure-Abonnement](../agents/data-sources.md#azure-subscription) | Azure-Aktivitätsprotokoll | Erstellen Sie ein Protokollprofil zum Exportieren von Ereignissen des Aktivitätsprotokolls an Event Hubs.  Ausführliche Informationen finden Sie unter [Streamen von Protokollen der Azure-Plattform an Azure Event Hubs](../essentials/resource-logs.md#send-to-azure-event-hubs). |
| [Azure-Ressourcen](../agents/data-sources.md#azure-resources) | Plattformmetriken<br> Ressourcenprotokolle |Beide Arten von Daten werden mithilfe einer Ressourcendiagnoseeinstellung an einen Event Hub gesendet. Ausführliche Informationen finden Sie unter [Streamen von Azure-Ressourcenprotokollen an einen Event Hub](../essentials/resource-logs.md#send-to-azure-event-hubs). |
| [Betriebssystem (Gast)](../agents/data-sources.md#operating-system-guest) | Virtuelle Azure-Computer | Installieren Sie die [Azure-Diagnoseerweiterung](../agents/diagnostics-extension-overview.md) auf virtuellen Windows- und Linux-Computern in Azure. Weitere Informationen zu virtuellen Windows-Computern finden Sie unter [Streamen von Azure-Diagnosedaten auf dem langsamsten Pfad mithilfe von Event Hubs](../agents/diagnostics-extension-stream-event-hubs.md) und zu virtuellen Linux-Computern unter [Verwenden der Linux-Diagnoseerweiterung zum Überwachen von Metriken und Protokollen](../../virtual-machines/extensions/diagnostics-linux.md#protected-settings). |
| [Anwendungscode](../agents/data-sources.md#application-code) | Application Insights | Application Insights bietet keine direkte Methode zum Streamen von Daten an Event Hubs. Sie können einen [fortlaufenden Export](../app/export-telemetry.md) der Application Insights-Daten an ein Speicherkonto einrichten und die Daten dann mithilfe einer Logik-App an einen Event Hub senden, wie unter [Manuelles Streamen mit einer Logik-App](#manual-streaming-with-logic-app) beschrieben. |

## <a name="manual-streaming-with-logic-app"></a>Manuelles Streamen mit einer Logik-App
Für Daten, die sich nicht direkt an einen Event Hub streamen lassen, können Sie einen Azure-Speicher nutzen und dann eine zu bestimmten Zeiten ausgelöste Logik-App verwenden, die [die Daten aus dem Blob-Speicher holt](../../connectors/connectors-create-api-azureblobstorage.md#add-action) und [sie als Nachricht an den Event Hub überträgt](../../connectors/connectors-create-api-azure-event-hubs.md#add-action). 


## <a name="partner-tools-with-azure-monitor-integration"></a>Partnertools mit Azure Monitor-Integration

Die Weiterleitung Ihrer Überwachungsdaten an einen Event Hub mit Azure Monitor ermöglicht eine einfache Integration in externe SIEM- und Überwachungstools. Hier einige Beispiele für Tools mit Azure Monitor-Integration:

| Tool | In Azure gehostet | BESCHREIBUNG |
|:---|:---| :---|
|  IBM QRadar | Nein | Das DSM und Event Hub-Protokoll von Microsoft Azure sind zum Download auf der [Website des IBM-Supports](https://www.ibm.com/support) erhältlich. Weitere Informationen zur Integration in Azure finden Sie unter [QRadar DSM configuration (QRadar DSM-Konfiguration)](https://www.ibm.com/docs/en/dsm?topic=options-configuring-microsoft-azure-event-hubs-communicate-qradar). |
| Splunk | Nein | Das [Microsoft Azure-Add-On für Splunk](https://splunkbase.splunk.com/app/3757/) steht als Open-Source-Projekt in der Splunkbase zur Verfügung. <br><br> Falls Sie kein Add-On in Ihrer Splunk-Instanz installieren können, z. B. bei Verwendung eines Proxys oder bei Ausführung in Splunk Cloud, können Sie diese Ereignisse an die HTTP-Ereignissammlung von Splunk weiterleiten. Verwenden Sie dazu die [Azure-Funktion für Splunk](https://github.com/Microsoft/AzureFunctionforSplunkVS), die durch neue Nachrichten im Event Hub ausgelöst wird. |
| sumologic | Nein | Anweisungen zum Einrichten von SumoLogic für die Nutzung von Daten aus einem Event Hub finden Sie unter [Collect Logs for the Azure Audit App from Event Hub (Sammeln von Protokollen für die Azure Audit App aus einem Event Hub)](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub). |
| ArcSight | Nein | Der intelligente Azure Event Hub-Connector von ArcSight wird im Rahmen dieser [ArcSight-Sammlung von intelligenten Connectors](https://community.softwaregrp.com/t5/Discussions/Announcing-General-Availability-of-ArcSight-Smart-Connectors-7/m-p/1671852) zur Verfügung gestellt. |
| Syslog-Server | Nein | Wenn Sie Azure Monitor-Daten direkt an einen Syslog-Server streamen möchten, können Sie eine [auf einer Azure-Funktion basierende Lösung](https://github.com/miguelangelopereira/azuremonitor2syslog/) nutzen.
| LogRhythm | Nein| Anweisungen zum Einrichten von LogRhythm zum Erfassen von Protokollen aus einem Event Hub sind [hier](https://logrhythm.com/six-tips-for-securing-your-azure-cloud-environment/) verfügbar. 
|Logz.io | Ja | Weitere Informationen finden Sie unter [Erste Schritte bei der Überwachung und Protokollierung mithilfe von Logz.io für in Azure ausgeführte Java-Apps](/azure/developer/java/fundamentals/java-get-started-with-logzio).

Möglicherweise sind auch andere Partner verfügbar. Eine vollständigere Liste aller Azure Monitor-Partner und ihrer Funktionen finden Sie unter [Azure Monitor – integrierte Partnerlösungen](../partners.md).

## <a name="next-steps"></a>Nächste Schritte
* [Archivieren Sie das Aktivitätsprotokoll in einem Speicherkonto.](./activity-log.md#legacy-collection-methods)
* [Lesen Sie die Übersicht über das Azure-Aktivitätsprotokoll.](../essentials/platform-logs-overview.md)
* [Richten Sie eine Warnung ein, die auf einem Aktivitätsprotokollereignis basiert.](../alerts/alerts-log-webhook.md)
