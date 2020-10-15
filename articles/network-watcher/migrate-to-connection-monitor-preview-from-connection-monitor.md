---
title: Migrieren von Verbindungsmonitor zu Verbindungsmonitor (Vorschau)
titleSuffix: Azure Network Watcher
description: Erfahren Sie, wie Sie einen Verbindungsmonitor zu Verbindungsmonitor (Vorschau) migrieren.
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
ms.openlocfilehash: 05b42024529b8d9de3590488ecafbdf83283e007
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89441815"
---
# <a name="migrate-to-connection-monitor-preview-from-connection-monitor"></a>Migrieren von Verbindungsmonitor zu Verbindungsmonitor (Vorschau)

Sie können vorhandene Verbindungsmonitore mit wenigen Mausklicks und ohne Ausfallzeit zum neuen, verbesserten Verbindungsmonitor (Vorschau) migrieren. Weitere Informationen zu den Vorteilen finden Sie unter [Verbindungsmonitor (Vorschau)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview).

## <a name="key-points-to-note"></a>Wichtige Hinweise

Die Migration hilft beim Liefern der folgenden Ergebnisse:

* Die Funktionen von Agents und Firewalleinstellungen bleiben unverändert erhalten. Es sind keine Änderungen erforderlich. 
* Vorhandene Verbindungsmonitore werden „Verbindungsmonitor (Vorschau) > Testgruppe > Testformat“ zugeordnet. Sie können **Bearbeiten** auswählen, um die Eigenschaften des neuen Verbindungsmonitors anzuzeigen und zu ändern. Laden Sie eine Vorlage herunter, um Änderungen am Verbindungsmonitor vorzunehmen und diese über Azure Resource Manager zu übermitteln. 
* Virtuelle Azure-Computer mit der Network Watcher-Erweiterung senden Daten an den Arbeitsbereich und die Metriken. Verbindungsmonitore stellen die Daten über die neuen Metriken „ChecksFailedPercent (Vorschau)“ und „RoundTripTimeMs (Vorschau)“ (anstatt über die veralteten Metriken „ProbesFailedPercent“ und „AverageRoundTripMs“) zur Verfügung. 
* Datenüberwachung:
   * **Warnungen**: Wurden automatisch zu den neuen Metriken migriert.
   * **Analysen und Integrationen**: Erfordern die manuelle Bearbeitung des Satzes der Metriken. 
    
## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie einen benutzerdefinierten Arbeitsbereich verwenden, stellen Sie sicher, dass Network Watcher im Abonnement und in der Region des Log Analytics-Arbeitsbereichs aktiviert ist. 

## <a name="migrate-the-connection-monitors"></a>Migrieren der Verbindungsmonitore

1. Wählen Sie **Verbindungsmonitor** und dann **Verbindungsmonitore migrieren** aus, um die älteren Verbindungsmonitore zu den neueren zu migrieren.

    ![Der Screenshot zeigt die Migration von Verbindungsmonitoren zu Verbindungsmonitor (Vorschau).](./media/connection-monitor-2-preview/migrate-cm-to-cm-preview.png)
    
1. Wählen Sie Ihr Abonnement und die Verbindungsmonitore aus, die Sie migrieren möchten, und wählen Sie dann **Ausgewählte migrieren** aus. 

Sie haben mit nur wenigen Klicks die vorhandenen Verbindungsmonitore zu Verbindungsmonitor (Vorschau) migriert. 

Sie können jetzt die Eigenschaften von Verbindungsmonitor (Vorschau) anpassen, den Standardarbeitsbereich ändern, Vorlagen herunterladen und den Migrationsstatus überprüfen. 

Nach dem Starten der Migration werden die folgenden Änderungen durchgeführt: 
* Aus der Azure Resource Manager-Ressource wird der neuere Verbindungsmonitor.
    * Der Name, die Region und das Abonnement des Verbindungsmonitors bleiben unverändert erhalten. Die Ressourcen-ID wird nicht geändert.
    * Sofern der Verbindungsmonitor nicht angepasst wurde, wird im Abonnement und in der Region des Verbindungsmonitors ein Log Analytics-Standardarbeitsbereich erstellt. In diesem Arbeitsbereich werden die Überwachungsdaten gespeichert. Zudem werden in den Metriken die Testergebnisdaten gespeichert.
    * Jeder Test wird zu einer Testgruppe mit dem Namen *defaultTestGroup* migriert.
    * Quell- und Zielendpunkte werden in der neuen Testgruppe erstellt und verwendet. Die Standardnamen lauten *defaultSourceEndpoint* und *defaultDestinationEndpoint*.
    * Der Zielport und das Testintervall werden in eine Testkonfiguration mit dem Namen *defaultTestConfiguration* verschoben. Das Protokoll wird anhand der Portwerte festgelegt. Erfolgsschwellenwerte und andere optionale Eigenschaften bleiben leer.
* Metrikwarnungen werden zu den Metrikwarnungen von Verbindungsmonitor (Vorschau) migriert. Da die Metriken unterschiedlich sind, ist diese Änderung erforderlich. Weitere Informationen finden Sie unter [Überwachung der Netzwerkkonnektivität mit dem Verbindungsmonitor (Vorschau)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#metrics-in-azure-monitor).
* Die migrierten Verbindungsmonitore werden nicht mehr als ältere Verbindungsmonitor-Lösung angezeigt. Sie sind jetzt nur noch in Verbindungsmonitor (Vorschau) verfügbar.
* Externe Integrationen, z. B. Dashboards in Power BI und Grafana, sowie Integrationen mit SIEM-Systemen (Security Information and Event Management) müssen manuell migriert werden. Dies ist der einzige manuelle Schritt, den Sie zum Migrieren dieser Einrichtung ausführen müssen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Verbindungsmonitor (Vorschau) finden Sie in den folgenden Artikeln:
* [Migrieren von Netzwerkleistungsmonitor zu Verbindungsmonitor (Vorschau)](migrate-to-connection-monitor-preview-from-network-performance-monitor.md)
* [Erstellen eines Verbindungsmonitors (Vorschau) mithilfe des Azure-Portals](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview-create-using-portal)
