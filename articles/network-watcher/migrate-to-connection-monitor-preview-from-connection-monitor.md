---
title: Migrieren von Verbindungsmonitor zu Verbindungsmonitor (Vorschau)
titleSuffix: Azure Network Watcher
description: Erfahren Sie, wie Sie einen Verbindungsmonitor zu Verbindungsmonitor (Vorschau) migrieren.
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
ms.openlocfilehash: ddf6e326df876229d32ef15983f76879836f1fca
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88701311"
---
# <a name="migrate-to-connection-monitor-preview-from-connection-monitor"></a>Migrieren von Verbindungsmonitor zu Verbindungsmonitor (Vorschau)

Sie können vorhandene Verbindungsmonitore mit nur einem Mausklick und ohne Ausfallzeit zum neuen und verbesserten Verbindungsmonitor (Vorschau) migrieren. Unter [Verbindungsmonitor (Vorschau)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview) erfahren Sie mehr über die Vorteile.

## <a name="key-points-to-note"></a>Wichtige Hinweise

* Funktionen von Agents und Firewalleinstellungen bleiben unverändert erhalten (keine Aktion erforderlich). 
* Vorhandene Verbindungsmonitore werden „Verbindungsmonitor (Vorschau) -> Testgruppe > Testformat“ zugeordnet. Sie können auf *Bearbeiten* klicken, um die Eigenschaften des neuen Verbindungsmonitors anzuzeigen und zu ändern. Laden Sie die Vorlage herunter, um Änderungen am Verbindungsmonitor vorzunehmen und diese über Azure Resource Manager zu übermitteln. 
* Virtuelle Azure-Computer mit Network Watcher-Erweiterung senden Daten an Arbeitsbereich und Metriken. Verbindungsmonitore stellen die Daten über die neuen Metriken „ChecksFailedPercent (Vorschau)“ und „RoundTripTimeMs (Vorschau)“ (anstatt über die veralteten Metriken „ProbesFailedPercent“ und „AverageRoundTripMs“) zur Verfügung. 
* Überwachungsdaten
    * Warnungen: Warnungen werden im Rahmen der Migration zu neuen Metriken migriert.
    * Dashboards und Integrationen: Sie müssen die Metriken manuell bearbeiten. 
    
## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie einen benutzerdefinierten Arbeitsbereich verwenden, stellen Sie sicher, dass Network Watcher im Abonnement und in der Region des Log Analytics-Arbeitsbereichs aktiviert ist. 

## <a name="steps-to-migrate-from-connection-monitor-to-connection-monitor-preview"></a>Schritte zum Migrieren eines Verbindungsmonitors zu Verbindungsmonitor (Vorschau)

1. Klicken Sie auf „Verbindungsmonitor“, und navigieren Sie zu „Verbindungsmonitore migrieren“, um Verbindungsmonitore von der älteren zur neuen Lösung zu migrieren.

    ![Der Screenshot zeigt die Migration von Verbindungsmonitoren zu Verbindungsmonitor (Vorschau)](./media/connection-monitor-2-preview/migrate-cm-to-cm-preview.png)
    
1. Wählen Sie das Abonnement und die Verbindungsmonitore aus, und klicken Sie auf „Ausgewählte migrieren“. Migration vorhandener Verbindungsmonitore zu Verbindungsmonitor (Vorschau) mit nur einem Mausklick 
1. Sie können die Eigenschaften der Verbindungsmonitore anpassen, den Standardarbeitsbereich ändern, die Vorlage herunterladen und den Status der Migration überprüfen. 
1. Nach dem Start der Migration werden die folgenden Änderungen vorgenommen: 
    1. Aus der Azure Resource Manager-Ressource wird der neuere Verbindungsmonitor.
        1. Name, Region und Abonnement des Verbindungsmonitors bleiben unverändert erhalten. Daher hat dies keine Auswirkung auf die Ressourcen-ID.
        1. Sofern keine Anpassung vorhanden ist, wird in der Region und im Abonnement des Verbindungsmonitors ein Log Analytics-Standardarbeitsbereich erstellt. In diesem Arbeitsbereich werden die Überwachungsdaten gespeichert. Außerdem werden Testergebnisdaten in Metriken gespeichert.
        1. Jeder Test wird zu einer Testgruppe namens *defaulttestgroup* migriert.
        1.  Quell- und Zielendpunkte werden in der erstellten Testgruppe erstellt und verwendet. Die Standardnamen lauten *defaultSourceEndpoint* und *defaultDestinationEndpoint*.
        1. Der Zielport und das Testintervall werden in die Testkonfiguration namens *defaultTestConfiguration* verschoben. Das Protokoll wird anhand der Portwerte festgelegt. Erfolgsschwellenwerte und andere optionale Eigenschaften bleiben leer.
    1. Metrikwarnungen werden zu den Metrikwarnungen von Verbindungsmonitor (Vorschau) migriert. Da die Metriken unterschiedlich sind (<link to metric section in the doc>), ist diese Änderung erforderlich.
    1. Die migrierten Verbindungsmonitore werden in der alten Verbindungsmonitorlösung nicht mehr angezeigt. Sie sind nur noch in Verbindungsmonitor (Vorschau) verfügbar und können nur dort verwendet werden.
    1. Alle internen Integrationen wie Dashboards in Power BI, Grafana oder Integrationen mit SIEM-Systemen müssen vom Benutzer direkt migriert werden. Dies ist der einzige manuelle Schritt, den der Benutzer zum Migrieren dieser Einrichtung ausführen muss.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum [Migrieren von Netzwerkleistungsmonitor zu Verbindungsmonitor (Vorschau)](migrate-to-connection-monitor-preview-from-network-performance-monitor.md)
* Informationen zum [Erstellen eines Verbindungsmonitors (Vorschau) mithilfe des Azure-Portals](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview-create-using-portal)
