---
title: Verwalten von Azure Automation-Daten
description: Dieser Artikel enthält mehrere Themen zum Verwalten einer Azure Automation-Umgebung.  Er enthält derzeit die Themen "Datenaufbewahrung", "Sichern von Azure Automation" und "Notfallwiederherstellung in Azure Automation".
services: automation
ms.subservice: shared-capabilities
ms.date: 03/23/2020
ms.topic: conceptual
ms.openlocfilehash: f917e9c64a932d75fd0f6b14c9e0f35808467355
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984656"
---
# <a name="managing-azure-automation-data"></a>Verwalten von Azure Automation-Daten

Dieser Artikel enthält mehrere Themen zum Verwalten von Daten in einer Azure Automation-Umgebung.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="data-retention"></a>Beibehaltung von Daten

Wenn Sie eine Ressource in Azure Automation löschen, wird diese zu Überwachungszwecken für eine Anzahl von Tagen aufbewahrt, bevor sie endgültig gelöscht wird. In diesem Zeitraum kann die Ressource weder angezeigt noch verwendet werden. Diese Richtlinie gilt auch für Ressourcen, die zu einem gelöschten Automation-Konto gehören.

Die folgende Tabelle zeigt die Aufbewahrungsrichtlinie für unterschiedliche Ressourcen.

| Daten | Richtlinie |
|:--- |:--- |
| Konten |Ein Konto wird 30 Tage nach seiner Löschung durch den Benutzer endgültig entfernt. |
| Objekte |Ein Objekt wird 30 Tage nach seiner Löschung durch den Benutzer endgültig entfernt oder 30 Tage, nachdem ein Benutzer ein Konto gelöscht hat, das das Objekt enthält. |
| DSC-Knoten |Ein DSC-Knoten wird 30 Tage nach Aufhebung seiner Registrierung im Automation-Konto über das Azure-Portal oder mit dem Cmdlet [Unregister-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-3.7.0) in Windows PowerShell endgültig entfernt. Auch ein Knoten wird nach 30 Tagen endgültig entfernt, nachdem ein Benutzer das Konto gelöscht hat, das den Knoten enthält. |
| Aufträge |Ein Auftrag wird 30 Tage nach der Änderung gelöscht und endgültig entfernt, z. B. nachdem der Auftrag abgeschlossen, beendet oder angehalten wurde. |
| Module |Ein Modul wird 30 Tage nach seiner Löschung durch einen Benutzer endgültig entfernt oder 30 Tage, nachdem ein Benutzer das Konto gelöscht hat, das das Modul enthält. |
| Knotenkonfigurationen/MOF-Dateien |Eine alte Knotenkonfiguration wird 30 Tage nach dem Generieren einer neuen Knotenkonfiguration endgültig entfernt. |
| Knotenberichte |Ein Knotenbericht wird 90 Tage nach dem Generieren eines neuen Berichts für diesen Knoten endgültig entfernt. |
| Runbooks |Ein Runbook wird 30 Tage, nachdem ein Benutzer die Ressource gelöscht hat, endgültig entfernt oder 30 Tage, nachdem ein Benutzer das Konto gelöscht hat, das die Ressource enthält. |

Die Datenaufbewahrungsrichtlinie gilt für alle Benutzer und kann zurzeit nicht angepasst werden. Wenn Sie jedoch Daten für einen längeren Zeitraum aufbewahren müssen, können Sie [Azure Automation-Auftragsdaten an Azure Monitor-Protokolle weiterleiten](automation-manage-send-joblogs-log-analytics.md).

## <a name="data-backup"></a>Datensicherung

Wenn Sie ein Automation-Konto in Azure löschen, werden auch alle Objekt in dem Konto gelöscht. Zu den Objekten zählen Runbooks, Module, Konfigurationen, Einstellungen, Aufträge und Objekte. Sie können nicht mehr wiederhergestellt werden, nachdem das Konto gelöscht wurde. Sie können die Inhalte Ihres Automation-Kontos mithilfe der folgenden Informationen sichern, bevor Sie das Konto löschen.

### <a name="runbooks"></a>Runbooks

Sie können Ihre Runbooks entweder unter Verwendung des Azure-Portals oder mithilfe des Cmdlets [Get-AzureAutomationRunbookDefinition](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationrunbookdefinition) in Windows PowerShell in Skriptdateien exportieren. Sie können diese Skriptdateien in ein anderes Automation-Konto importieren, wie unter [Verwalten von Runbooks in Azure Automation](manage-runbooks.md) besprochen.

### <a name="integration-modules"></a>Integrationsmodule

Integrationsmodule können nicht aus Azure Automation exportiert werden. Sie müssen sie außerhalb des Automation-Kontos verfügbar machen.

### <a name="assets"></a>Objekte

Sie können keine Azure Automation-Objekte exportieren: Zertifikate, Verbindungen, Anmeldeinformationen, Zeitpläne und Variablen. Stattdessen können Sie das Azure-Portal und Azure-Cmdlets verwenden, um sich die Details dieser Objekte zu notieren. Verwenden Sie dann diese Details, um alle Objekte zu erstellen, die von Runbooks verwendet werden, die Sie in ein anderes Automation-Konto importieren.

Es ist nicht möglich, die Werte verschlüsselter Variablen oder die Kennwortfelder für Anmeldeinformationen mithilfe von Cmdlets abzurufen. Wenn Sie diese Werte nicht kennen, können Sie sie aus einem Runbook abrufen. Informationen zum Abrufen von Variablenwerten finden Sie unter [Variablenobjekte in Azure Automation](shared-resources/variables.md). Weitere Informationen zum Abrufen von Anmeldeinformationswerten finden Sie unter [Anmeldeinformationsobjekte in Azure Automation](shared-resources/credentials.md).

 ### <a name="dsc-configurations"></a>DSC-Konfigurationen

Sie können Ihre DSC-Konfigurationen unter Verwendung des Azure-Portals oder mithilfe des Cmdlets [Export-AzureRmAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/az.automation/export-azautomationdscconfiguration?view=azps-3.7.0
) in Windows PowerShell in Skriptdateien exportieren. Sie können diese Konfigurationen in ein anderes Automation-Konto importiert importieren und darin verwenden.

## <a name="geo-replication-in-azure-automation"></a>Georeplikation in Azure Automation

Georeplikation ist Standard in Azure Automation-Kontos. Bei der Einrichtung Ihres Kontos wählen Sie eine primäre Region aus. Der interne Automation-Georeplikationsdienst weist dem Konto automatisch eine sekundäre Region zu. Der Dienst sichert dann kontinuierlich Kontodaten aus der primären Region in der sekundären Region. Die vollständige Liste der primären und sekundären Regionen finden Sie unter [Business Continuity und Disaster Recovery (BCDR): Azure-Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

Die vom Automation-Georeplikationsdienst erstellte Sicherung ist eine vollständige Kopie der Automation-Objekte, -Konfigurationen usw. Diese Sicherung kann verwendet werden, wenn die primäre Region ausfällt und Daten verloren gehen. Microsoft versucht im unwahrscheinlichen Fall, dass Daten in einer primären Region verloren gehen, diese wiederherzustellen. Wenn das Unternehmen die primären Daten nicht wiederherstellen kann, verwendet es automatisches Failover und informiert Sie über die Situation über Ihr Azure-Abonnement. 

Externe Kunden können nicht direkt auf den Automation-Georeplikationsdienst zugreifen, wenn es zu einem regionalen Ausfall kommt. Wenn Sie Automation-Konfigurationen und -Runbooks während regionaler Ausfälle beibehalten möchten:

1. Wählen Sie eine sekundäre Region aus, die mit der geografischen Region Ihres primären Automation-Kontos gekoppelt werden soll.

2. Erstellen Sie ein Automation-Konto in der sekundären Region.

3. Exportieren Sie im primären Konto die Runbooks als Skriptdateien.

4. Importieren Sie die Runbooks in Ihr Automation-Konto in der sekundären Region.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu sicheren Objekten in Azure Automation finden Sie unter [Verschlüsseln sicherer Objekte in Azure Automation](automation-secure-asset-encryption.md).

* Weitere Informationen zur Georeplikation finden Sie unter [Erstellen und Verwenden der aktiven Georeplikation](https://docs.microsoft.com/azure/sql-database/sql-database-active-geo-replication).