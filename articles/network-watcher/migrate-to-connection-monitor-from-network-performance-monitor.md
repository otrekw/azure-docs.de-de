---
title: Migrieren vom Netzwerkleistungsmonitor zum Verbindungsmonitor
titleSuffix: Azure Network Watcher
description: Erfahren Sie, wie Sie vom Netzwerkleistungsmonitor zum Verbindungsmonitor migrieren.
services: network-watcher
documentationcenter: na
author: vinynigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: vinigam
ms.openlocfilehash: e5053284de18740b761df3e5df256cc79d2e8f1c
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101698687"
---
# <a name="migrate-to-connection-monitor-from-network-performance-monitor"></a>Migrieren vom Netzwerkleistungsmonitor zum Verbindungsmonitor

> [!IMPORTANT]
> Ab dem 1. Juli 2021 ist es im Netzwerkleistungsmonitor nicht mehr möglich, neue Tests in einem vorhandenen Arbeitsbereich hinzuzufügen oder einen neuen Arbeitsbereich zu aktivieren. Sie können weiterhin die Tests verwenden, die vor dem 1. Juli 2021 erstellt wurden. Migrieren Sie Ihre Tests vor dem 29. Februar 2024 aus dem Netzwerkleistungsmonitor zum neuen Verbindungsmonitor in Azure Network Watcher, um Dienstunterbrechungen im Zusammenhang mit Ihren aktuellen Workloads zu minimieren.

Sie können Tests vom Netzwerkleistungsmonitor (NPM) mit nur einem Mausklick und ohne Ausfallzeit zum neuen, verbesserten Verbindungsmonitor migrieren. Weitere Informationen zu den Vorteilen finden Sie unter [Verbindungsmonitor](./connection-monitor-overview.md).


## <a name="key-points-to-note"></a>Wichtige Hinweise

Durch die Migration können die folgenden Ergebnisse erzielt werden:

* Die Funktion lokaler Agents und die Firewalleinstellungen bleiben unverändert erhalten. Es sind keine Änderungen erforderlich. Log Analytics-Agents, die auf Azure-VMs installiert sind, müssen durch die [Network Watcher-Erweiterung](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-windows) ersetzt werden.
* Vorhandene Test werden „Verbindungsmonitor“ > „Testgruppe“ > „Testformat“ zugeordnet. Wählen Sie **Bearbeiten** aus, um die Eigenschaften des neuen Verbindungsmonitors anzuzeigen und zu ändern. Laden Sie eine Vorlage herunter, um Änderungen vorzunehmen, und übermitteln Sie sie mit Azure Resource Manager.
* Agents senden Daten sowohl an den Log Analytics-Arbeitsbereich als auch an Metriken.
* Datenüberwachung:
   * **Daten in Log Analytics:** Vor der Migration verbleiben die Daten im Arbeitsbereich, in dem NPM in der Tabelle „NetworkMonitoring“ konfiguriert ist. Nach der Migration werden die Daten in den Tabellen „NetworkMonitoring“, „NWConnectionMonitorTestResult“ und „NWConnectionMonitorPathResult“ im selben Arbeitsbereich gespeichert. Nach dem Deaktivieren der Tests in NPM werden die Daten nur noch in den Tabellen „NWConnectionMonitorTestResult“ und „NWConnectionMonitorPathResult“ gespeichert.
   * **Protokollbasierte Warnungen, Dashboards und Integrationen**: Sie müssen die Abfragen anhand der neuen Tabellen „NWConnectionMonitorTestResult“ und „NWConnectionMonitorPathResult“ manuell bearbeiten. Informationen zum Neuerstellen der Warnungen in Metriken finden Sie unter [Überwachung der Netzwerkkonnektivität mit dem Verbindungsmonitor](./connection-monitor-overview.md#metrics-in-azure-monitor).
    
## <a name="prerequisites"></a>Voraussetzungen

* Stellen Sie sicher, dass Network Watcher in Ihrem Abonnement und in der Region des Log Analytics-Arbeitsbereichs aktiviert ist. 
* Falls Sie eine Azure-VM verwenden, die einer anderen Region/einem anderen Abonnement angehört als der Log Analytics Arbeitsbereich, stellen Sie sicher, dass Network Watcher für dieses Abonnement und diese Region aktiviert ist.   
* Virtuelle Azure-Computer, auf denen Log Analytics-Agents installiert sind, müssen mit der Network Watcher-Erweiterung aktiviert werden.

## <a name="migrate-the-tests"></a>Migrieren der Tests

Gehen Sie folgendermaßen vor, um von Netzwerkleistungsmonitor zu Verbindungsmonitor zu migrieren:

1. Wählen Sie in Network Watcher **Verbindungsmonitor** aus, und wählen Sie dann die Registerkarte **Migrate tests from NPM** (Tests von NPM migrieren) aus. 

    :::image type="content" source="./media/connection-monitor-2-preview/migrate-npm-to-cm-preview.png" alt-text="Migrieren von Tests von Netzwerkleistungsmonitor zu Verbindungsmonitor" lightbox="./media/connection-monitor-2-preview/migrate-npm-to-cm-preview.png":::
    
1. Wählen Sie in den Dropdownlisten Ihr Abonnement und den Arbeitsbereich und dann das NPM-Feature aus, das Sie migrieren möchten. 
1. Wählen Sie zum Migrieren der Tests **Importieren** aus.

Nach dem Beginn der Migration werden die folgenden Änderungen durchgeführt: 
* Es wird eine neue Verbindungsmonitorressource erstellt.
   * Pro Region und Abonnement wird ein Verbindungsmonitor erstellt. Bei Tests mit lokalen Agents weist der Name des neuen Verbindungsmonitors das Format `<workspaceName>_"workspace_region_name"` auf. Bei Tests mit Azure-Agents weist der Name des neuen Verbindungsmonitors das Format `<workspaceName>_<Azure_region_name>` auf.
   * Die Überwachungsdaten werden jetzt in dem Log Analytics-Arbeitsbereich, in dem NPM aktiviert ist, in neuen Tabellen namens „NWConnectionMonitorTestResult“ und „NWConnectionMonitorPathResult“ gespeichert. 
   * Der Testname wird als Name der Testgruppe übernommen. Die Testbeschreibung wird nicht migriert.
   * Quell- und Zielendpunkte werden in der neuen Testgruppe erstellt und verwendet. Für lokale Agents werden die Endpunkte als `<workspaceName>_<FQDN of on-premises machine>` formatiert.
   * Der Zielport und das Testintervall werden in eine Testkonfiguration namens `TC_<protocol>_<port>` und `TC_<protocol>_<port>_AppThresholds` verschoben. Das Protokoll wird anhand der Portwerte festgelegt. Für ICMP werden die Testkonfigurationen mit `TC_<protocol>` und `TC_<protocol>_AppThresholds` benannt. Falls Erfolgsschwellenwerte und andere optionale Eigenschaften festgelegt sind, werden sie migriert, andernfalls bleiben sie leer.
   * Wenn die Migrationstests Agents enthalten, die nicht ausgeführt werden, müssen Sie diese aktivieren und die Migration wiederholen.
* NPM wird nicht deaktiviert, daher können die migrierten Tests weiterhin Daten an die Tabellen „NetworkMonitoring“, „NWConnectionMonitorTestResult“ und „NWConnectionMonitorPathResult“ senden. Dadurch wird sichergestellt, dass vorhandene protokollbasierte Warnungen und Integrationen nicht beeinträchtigt werden.
* Der neu erstellte Verbindungsmonitor wird unter „Verbindungsmonitor“ angezeigt.

Achten Sie nach der Migration auf Folgendes:
* Deaktivieren Sie die Tests in NPM manuell. Bis dies geschehen ist, werden Ihnen diese weiterhin in Rechnung gestellt. 
* Wenn Sie NPM deaktivieren, erstellen Sie Ihre Warnungen für die Tabellen „NWConnectionMonitorTestResult“ und „NWConnectionMonitorPathResult“ neu, oder verwenden Sie Metriken. 
* Migrieren Sie externe Integrationen zu den Tabellen „NWConnectionMonitorTestResult“ und „NWConnectionMonitorPathResult“. Beispiele für externe Integrationen sind Dashboards in Power BI und Grafana sowie Integrationen mit SIEM-Systemen (Security Information and Event Management).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Verbindungsmonitor finden Sie in den folgenden Artikeln:
* [Migrieren von Verbindungsmonitor (klassisch) zu Verbindungsmonitor](./migrate-to-connection-monitor-from-connection-monitor-classic.md)
* [Erstellen eines Verbindungsmonitors mithilfe des Azure-Portals](./connection-monitor-create-using-portal.md)
