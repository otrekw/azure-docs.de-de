---
title: Migrieren von Netzwerkleistungsmonitor zu Verbindungsmonitor (Vorschau)
titleSuffix: Azure Network Watcher
description: Erfahren Sie, wie Sie von Netzwerkleistungsmonitor zu Verbindungsmonitor (Vorschau) migrieren.
services: network-watcher
documentationcenter: na
author: vinynigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/2020
ms.author: vinigam
ms.openlocfilehash: dcbb82c1315e6150ddcfadbb52b2976447329b87
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89441832"
---
# <a name="migrate-to-connection-monitor-preview-from-network-performance-monitor"></a>Migrieren von Netzwerkleistungsmonitor zu Verbindungsmonitor (Vorschau)

Sie können Tests von Netzwerkleistungsmonitor (NPM) mit nur einem Mausklick und ohne Ausfallzeit zum neuen, verbesserten Verbindungsmonitor (Vorschau) migrieren. Weitere Informationen zu den Vorteilen finden Sie unter [Verbindungsmonitor (Vorschau)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview).

>[!NOTE]
> Nur Tests vom Dienstkonnektivitätsmonitor können zu Verbindungsmonitor (Vorschau) migriert werden.
>

## <a name="key-points-to-note"></a>Wichtige Hinweise

Durch die Migration können die folgenden Ergebnisse erzielt werden:

* Die Funktion lokaler Agents und die Firewalleinstellungen bleiben unverändert erhalten. Es sind keine Änderungen erforderlich. Log Analytics-Agents, die auf virtuellen Azure-Computern installiert sind, müssen durch die Network Watcher-Erweiterung ersetzt werden.
* Vorhandene Test werden „Verbindungsmonitor (Vorschau)“ > „Testgruppe“ > „Testformat“ zugeordnet. Wählen Sie **Bearbeiten** aus, um die Eigenschaften des neuen Verbindungsmonitors (Vorschau) anzuzeigen und zu ändern. Laden Sie eine Vorlage herunter, um Änderungen vorzunehmen, und übermitteln Sie sie mit Azure Resource Manager.
* Agents senden Daten sowohl an den Log Analytics-Arbeitsbereich als auch an Metriken.
* Datenüberwachung:
   * **Daten in Log Analytics:** Vor der Migration verbleiben die Daten im Arbeitsbereich, in dem NPM in der Tabelle „NetworkMonitoring“ konfiguriert ist. Nach der Migration werden die Daten in den Tabellen „NetworkMonitoring“ und „ConnectionMonitor_CL“ im selben Arbeitsbereich gespeichert. Nach dem Deaktivieren der Tests in NPM werden die Daten nur noch in der Tabelle „ConnectionMonitor_CL“ gespeichert.
   * **Protokollbasierte Warnungen, Dashboards und Integrationen:** Sie müssen die Abfragen anhand der neuen Tabelle „ConnectionMonitor_CL“ manuell bearbeiten. Informationen zum Neuerstellen der Warnungen in Metriken finden Sie unter [Überwachung der Netzwerkkonnektivität mit dem Verbindungsmonitor (Vorschau)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#metrics-in-azure-monitor).
    
## <a name="prerequisites"></a>Voraussetzungen

* Stellen Sie sicher, dass Network Watcher in Ihrem Abonnement und in der Region des Log Analytics-Arbeitsbereichs aktiviert ist.
* Virtuelle Azure-Computer, auf denen Log Analytics-Agents installiert sind, müssen mit der Network Watcher-Erweiterung aktiviert werden.

## <a name="migrate-the-tests"></a>Migrieren der Tests

Gehen Sie folgendermaßen vor, um von Netzwerkleistungsmonitor zu Verbindungsmonitor (Vorschau) zu migrieren:

1. Wählen Sie in Network Watcher **Verbindungsmonitor** aus, und wählen Sie dann die Registerkarte **Migrate tests from NPM** (Tests von NPM migrieren) aus. 

    ![Screenshot des Bereichs „Migrate tests from NPM“ (Tests von NPM migrieren) in Network Watcher | Verbindungsmonitor (Vorschau)](./media/connection-monitor-2-preview/migrate-npm-to-cm-preview.png)
    
1. Wählen Sie in den Dropdownlisten Ihr Abonnement und den Arbeitsbereich und dann das NPM-Feature aus, das Sie migrieren möchten. Derzeit können Sie Tests nur über Dienstkonnektivitätsmonitor migrieren.  
1. Wählen Sie zum Migrieren der Tests **Importieren** aus.

Nach dem Beginn der Migration werden die folgenden Änderungen durchgeführt: 
* Es wird eine neue Verbindungsmonitorressource erstellt.
   * Pro Region und Abonnement wird ein Verbindungsmonitor erstellt. Bei Tests mit lokalen Agents weist der Name des neuen Verbindungsmonitors das Format `<workspaceName>_"on-premises"` auf. Bei Tests mit Azure-Agents weist der Name des neuen Verbindungsmonitors das Format `<workspaceName>_<Azure_region_name>` auf.
   * Überwachungsdaten werden nun in dem Log Analytics-Arbeitsbereich, in dem NPM aktiviert ist, in der neuen Tabelle „ConnectionMonitor_CL“ gespeichert. 
   * Der Testname wird als Name der Testgruppe übernommen. Die Testbeschreibung wird nicht migriert.
   * Quell- und Zielendpunkte werden in der neuen Testgruppe erstellt und verwendet. Für lokale Agents werden die Endpunkte als `<workspaceName>_"endpoint"_<FQDN of on-premises machine>` formatiert. Wenn die migrierten Tests in Azure Agents enthalten, die nicht ausgeführt werden, müssen Sie diese aktivieren und die Migration wiederholen.
   * Der Zielport und das Testintervall werden in die Testkonfiguration mit den Namen *TC_\<testname>* und *TC_\<testname>_AppThresholds* verschoben. Das Protokoll wird anhand der Portwerte festgelegt. Erfolgsschwellenwerte und andere optionale Eigenschaften bleiben leer.
* NPM wird nicht deaktiviert, sodass die migrierten Tests weiterhin Daten an die Tabellen „NetworkMonitoring“ und „ConnectionMonitor_CL“ senden können. Dadurch wird sichergestellt, dass vorhandene protokollbasierte Warnungen und Integrationen nicht beeinträchtigt werden.
* Der neu erstellte Verbindungsmonitor wird unter „Verbindungsmonitor (Vorschau)“ angezeigt.

Achten Sie nach der Migration auf Folgendes:
* Deaktivieren Sie die Tests in NPM manuell. Bis dies geschehen ist, werden Ihnen diese weiterhin in Rechnung gestellt. 
* Stellen Sie beim Deaktivieren von NPM sicher, die Warnungen in der Tabelle „ConnectionMonitor_CL“ neu zu erstellen oder Metriken zu verwenden. 
* Migrieren Sie externe Integrationen in die Tabelle „ConnectionMonitor_CL“. Beispiele für externe Integrationen sind Dashboards in Power BI und Grafana sowie Integrationen mit SIEM-Systemen (Security Information and Event Management).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Verbindungsmonitor (Vorschau) finden Sie unter:
* [Migrieren von Verbindungsmonitor zu Verbindungsmonitor (Vorschau)](migrate-to-connection-monitor-preview-from-connection-monitor.md)
* [Erstellen eines Verbindungsmonitors (Vorschau) mithilfe des Azure-Portals](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview-create-using-portal)
