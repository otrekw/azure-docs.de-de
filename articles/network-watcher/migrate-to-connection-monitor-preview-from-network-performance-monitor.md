---
title: Migrieren von Netzwerkleistungsmonitor zu Verbindungsmonitor (Vorschau)
titleSuffix: Azure Network Watcher
description: Erfahren Sie, wie Sie von Netzwerkleistungsmonitor zu Verbindungsmonitor (Vorschau) migrieren.
services: network-watcher
documentationcenter: na
author: vinigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/2020
ms.author: vinigam
ms.openlocfilehash: 69dbb1dd4017c5acf9c195f5104741caee38c2b7
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88701308"
---
# <a name="migrate-to-connection-monitor-preview-from-network-performance-monitor"></a>Migrieren von Netzwerkleistungsmonitor zu Verbindungsmonitor (Vorschau)

Sie können Tests von Netzwerkleistungsmonitor mit nur einem Mausklick und ohne Ausfallzeit zum neuen und verbesserten Verbindungsmonitor (Vorschau) migrieren. Unter [Verbindungsmonitor (Vorschau)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview) erfahren Sie mehr über die Vorteile.

>[!NOTE]
> Nur Tests vom Dienstkonnektivitätsmonitor können zu Verbindungsmonitor (Vorschau) migriert werden.
>

## <a name="key-points-to-note"></a>Wichtige Hinweise

* Funktionen von lokalen Agents und Firewalleinstellungen bleiben unverändert erhalten. Es sind keine Änderungen erforderlich. Log Analytics-Agents, die auf virtuellen Azure-Computern installiert sind, müssen durch die Network Watcher-Erweiterung ersetzt werden.
* Vorhandene Test werden „Verbindungsmonitor (Vorschau) -> Testgruppe > Testformat“ zugeordnet. Sie können auf *Bearbeiten* klicken, um die Eigenschaften des neuen Verbindungsmonitors anzuzeigen und zu ändern. Laden Sie die Vorlage herunter, um Änderungen am Verbindungsmonitor vorzunehmen und diese über Azure Resource Manager zu übermitteln.
* Agents senden Daten sowohl an den Log Analytics-Arbeitsbereich als auch an Metriken.
* Überwachungsdaten
    * Daten in Log Analytics: Alle vor der Migration vorhandenen Daten befinden sich weiterhin in dem Arbeitsbereich, in dem der Netzwerkleistungsmonitor (NPM) in der Tabelle „NetworkMonitoring“ konfiguriert ist. Nach der Migration werden die Daten in den Tabellen „NetworkMonitoring“ und „ConnectionMonitor_CL“ im gleichen Arbeitsbereich gespeichert. Wenn die Tests von Netzwerkleistungsmonitor deaktiviert wurden, werden die Daten nur in der Tabelle „ConnectionMonitor_CL“ gespeichert.
    * Protokollbasierte Warnungen, Dashboards und Integrationen: Sie müssen die Abfragen, die auf der neuen Tabelle „ConnectionMonitor_CL“ basieren, manuell bearbeiten. Mithilfe dieses Links können Sie auch die Warnungen in Metriken neu erstellen. Die Möglichkeit zum automatischen Migrieren von protokollbasierten Warnungen in der Tabelle „NetworkMonitoring“ zu metrikbasierten Warnungen im Rahmen der Migration wird in Kürze verfügbar sein.
    
## <a name="prerequisites"></a>Voraussetzungen

*   Stellen Sie sicher, dass Network Watcher im Abonnement und in der Region des Log Analytics-Arbeitsbereichs aktiviert ist.
*   Virtuelle Azure-Computer, auf denen Log Analytics-Agents installiert sind, müssen mit der Network Watcher-Erweiterung aktiviert werden.

## <a name="steps-to-migrate-tests-from-network-performance-monitor-to-connection-monitor-preview"></a>Schritte zum Migrieren von Netzwerkleistungsmonitor zu Verbindungsmonitor (Vorschau)

1.  Klicken Sie auf „Verbindungsmonitor“, navigieren Sie zu „Tests von Netzwerkleistungsmonitor migrieren“, um Tests zu Verbindungsmonitor (Vorschau) zu migrieren.

    ![Der Screenshot zeigt die Migration von Tests von Netzwerkleistungsmonitor zu Verbindungsmonitor (Vorschau)](./media/connection-monitor-2-preview/migrate-npm-to-cm-preview.png)
    
1.  Wählen Sie Abonnement, Arbeitsbereich und das Netzwerkleistungsmonitor-Feature aus, das migriert werden soll. Derzeit können Sie nur Tests von Dienstkonnektivitätsmonitor migrieren.  
1.  Klicken Sie zum Migrieren von Tests auf „Importieren“.
1.  Nach dem Start der Migration werden die folgenden Änderungen vorgenommen: 
    1. Es wird eine neue Verbindungsmonitorressource erstellt.
        1. Pro Region und Abonnement wird ein Verbindungsmonitor erstellt. Bei Tests mit lokalen Agents hat der Name des neuen Verbindungsmonitors das Format <workspaceName> _”lokal“. Bei Tests mit Azure-Agents hat der Name des neuen Verbindungsmonitors das Format <workspaceName>_ <Name_der_Azure-Region>.
        1. Die Überwachungsdaten werden nun in dem Log Analytics-Arbeitsbereich, in dem Netzwerkleistungsmonitor aktiviert ist, und in einer neuen Tabelle namens „ConnectionMonitor_CL“ gespeichert. 
        1. Der Testname wird für den Testgruppennamen übernommen. Die Testbeschreibung wird nicht migriert.
        1. Quell- und Zielendpunkte werden in der erstellten Testgruppe erstellt und verwendet. Bei lokalen Agents werden die Endpunkte im Format <workspaceName> _„Endpunkt“_ <FQDN of on-premises machine> benannt. Bei Azure müssen Sie, wenn die zu migrierenden Tests Agents enthalten, die nicht ausgeführt werden, die Agents aktivieren und erneut migrieren.
        1. Der Zielport und das Testintervall werden in die Testkonfiguration namens „TC“ _<testname>“ und „TC“_ <testname>_„AppThresholds“ verschoben. Das Protokoll wird anhand der Portwerte festgelegt. Erfolgsschwellenwerte und andere optionale Eigenschaften bleiben leer.
    1. Netzwerkleistungsmonitor wird nicht deaktiviert. Daher senden migrierte Tests weiterhin Daten an die Tabellen „NetworkMonitoring“ und „ConnectionMonitor_CL“. Mit diesem Schritt wird sichergestellt, dass vorhandene protokollbasierte Warnungen und Integrationen nicht beeinträchtigt werden. Das automatische Migrieren von protokollbasierten Warnungen in der Tabelle „NetworkMonitoring“ zu metrikbasierten Warnungen im Rahmen der Migration wird in Kürze verfügbar sein.
    1. Der neu erstellte Verbindungsmonitor wird in Verbindungsmonitor (Vorschau) angezeigt.
1.  Nach der Migration müssen Sie die Tests in Netzwerkleistungsmonitor manuell deaktivieren. Bis zum Deaktivieren der Tests werden Ihnen diese weiterhin in Rechnung gestellt. Stellen Sie beim Deaktivieren von Netzwerkleistungsmonitor sicher, dass Sie die Warnungen in der Tabelle „ConnectionMonitor_CL“ neu erstellen oder Metriken verwenden. Achten Sie auch darauf, dass alle externen Integrationen wie Dashboards in Power BI, Grafana oder Integrationen mit SIEM-Systemen in die Tabelle „ConnectionMonitor_CL“ migriert werden müssen.


## <a name="next-steps"></a>Nächste Schritte

* Informationen zum [Migrieren von Verbindungsmonitor zu Verbindungsmonitor (Vorschau)](migrate-to-connection-monitor-preview-from-connection-monitor.md)
* Informationen zum [Erstellen eines Verbindungsmonitors (Vorschau) mithilfe des Azure-Portals](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview-create-using-portal)
