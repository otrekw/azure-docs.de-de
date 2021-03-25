---
title: Systemfunktionen für Azure Monitor-Protokolle
description: Schreiben von benutzerdefinierten Abfragen für Azure Monitor-Protokolle unter Verwendung von Systemfunktionen
ms.topic: conceptual
ms.date: 03/01/2021
ms.openlocfilehash: 1d26adfd2bd1a3fc1506a334b4b661b66172192d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102510412"
---
# <a name="system-functions-on-azure-monitor-logs"></a>Systemfunktionen für Azure Monitor-Protokolle

Azure Backup bietet eine Reihe von Funktionen, so genannte Systemfunktionen oder Lösungsfunktionen, die standardmäßig in den Arbeitsbereichen von Log Analytics (LA) verfügbar sind.
 
Diese Funktionen verarbeiten Daten in den [Rohtabellen von Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model) in Log Analytics und geben formatierte Daten zurück, mit denen Sie mit einfachen Abfragen problemlos Informationen aus all Ihren auf die Sicherung bezogenen Entitäten abrufen können. Benutzer können Parameter an diese Funktionen übergeben, um die von diesen Funktionen zurückgegebenen Daten zu filtern. 

Es empfiehlt sich, Systemfunktionen für die Abfrage Ihrer Sicherungsdaten in LA-Arbeitsbereichen für die Erstellung von benutzerdefinierten Berichten zu verwenden, da sie, wie im folgenden Abschnitt beschrieben, eine Reihe von Vorteilen bieten.

## <a name="benefits-of-using-system-functions"></a>Vorteile der Verwendung von Systemfunktionen

* **Einfachere Abfragen**: Mithilfe von Funktionen können Sie die Anzahl der benötigten Joins in Ihren Abfragen reduzieren. Standardmäßig geben die Funktionen vereinfachte Schemas zurück, die alle Informationen zu der abgefragten Entität (Sicherungsinstanz, Auftrag, Tresor usw.) enthalten. Wenn Sie beispielsweise eine Liste erfolgreicher Sicherungsaufträge nach Sicherungselementname und zugehörigem Container benötigen, erhalten Sie durch einen einfachen Aufruf der Funktion **_AzureBackup_getJobs()** alle diese Informationen für jeden Auftrag. Bei einer direkten Abfrage der Rohtabellen müssten Sie hingegen mehrere Joins zwischen den Tabellen [AddonAzureBackupJobs](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model#addonazurebackupjobs) und [CoreAzureBackup](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model#coreazurebackup) durchführen.

* **Reibungsloserer Übergang vom Diagnoseereignis einer Vorgängerversion**: Die Verwendung von Systemfunktionen hilft Ihnen bei einem reibungslosen Übergang vom [Diagnoseereignis einer Vorgängerversion](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events#legacy-event) („AzureBackupReport“ im AzureDiagnostics-Modus) zu den [ressourcenspezifischen Ereignissen](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events#diagnostics-events-available-for-azure-backup-users). Bei allen von Azure Backup bereitgestellten Systemfunktionen können Sie einen Parameter angeben, mit dem Sie wählen können, ob die Funktion nur Daten aus den ressourcenspezifischen Tabellen abfragen soll oder Daten sowohl aus der Tabelle der Vorgängerversion als auch aus den ressourcenspezifischen Tabellen abfragen soll (mit Deduplizierung der Datensätze).
    * Wenn Sie erfolgreich zu den ressourcenspezifischen Tabellen migriert haben, können Sie wählen, ob die Tabelle der Vorgängerversion von der Abfrage durch die Funktion ausgeschlossen werden soll.
    * Wenn Sie sich gerade in der Migration befinden und sich einige Daten, die Sie für die Analyse benötigen, in der Tabelle der Vorgängerversion befinden, können Sie die Tabelle der Vorgängerversion einbeziehen. Wenn der Übergang abgeschlossen ist und Sie die Daten aus der Tabelle der Vorgängerversion nicht mehr benötigen, können Sie den Wert des an die Funktion übergebenen Parameters einfach in Ihren Abfragen aktualisieren, um die Tabelle der Vorgängerversion auszuschließen.
    * Wenn Sie noch nur die Tabelle der Vorgängerversion verwenden, funktionieren die Funktionen trotzdem, auch wenn Sie die Tabelle der Vorgängerversion über denselben Parameter einbeziehen. Es wird jedoch empfohlen, frühestmöglich [auf die ressourcenspezifischen Tabellen umzusteigen](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events#steps-to-move-to-new-diagnostics-settings-for-a-log-analytics-workspace).

* **Reduzieren die Möglichkeit, dass benutzerdefinierte Abfragen abbrechen**: Wenn Azure Backup Verbesserungen am Schema der zugrunde liegenden LA-Tabellen einführt, um zukünftige Berichtsszenarien zu ermöglichen, wird die Definition der Funktionen ebenfalls aktualisiert, um die Schemaänderungen zu berücksichtigen. Wenn Sie also Systemfunktionen zum Erstellen von benutzerdefinierten Abfragen verwenden, werden Ihre Abfragen nicht abbrechen, selbst wenn das zugrunde liegenden Schema der Tabellen sich ändert.

> [!NOTE]
> Die Systemfunktionen werden von Microsoft verwaltet, und ihre Definitionen können von den Benutzern nicht bearbeitet werden. Wenn Sie bearbeitbare Funktionen benötigen, können Sie [gespeicherte Funktionen](https://docs.microsoft.com/azure/azure-monitor/logs/functions) in LA erstellen.

## <a name="types-of-system-functions-offered-by-azure-backup"></a>Von Azure Backup bereitgestellte Systemfunktionstypen

* **Kernfunktionen**: Dies sind Funktionen, mit denen Sie alle wichtigen Azure Backup-Entitäten abfragen können, z. B. Sicherungsinstanzen, Tresore, Richtlinien, Aufträge und Abrechnungsentitäten. Die Funktion **_AzureBackup_getBackupInstances** gibt z. B. eine Liste aller in Ihrer Umgebung vorhanden Sicherungsinstanzen zum letzten Abschlusstag (in UTC) zurück. Die Parameter und das zurückgegebene Schema für jede dieser Kernfunktionen werden im Folgenden in diesem Artikel zusammengefasst.

* **Trendfunktionen**: Dies sind Funktionen, die Verlaufsdatensätze für Ihre sicherungsbezogenen Entitäten (z. B. Sicherungsinstanzen, Abrechnungsgruppen) zurückgeben und es Ihnen ermöglichen, tägliche, wöchentliche und monatliche Trendinformationen zu wichtigen Metriken (z. B. Anzahl, verbrauchter Speicherplatz) in Bezug auf diese Entitäten abzurufen. Die Parameter und das zurückgegebene Schema für jede dieser Trendfunktionen werden im Folgenden in diesem Artikel zusammengefasst.

> [!NOTE]
> Derzeit geben die Systemfunktionen Daten bis zum letzten Abschlusstag (in UTC) zurück. Daten für den aktuellen Teiltag werden nicht zurückgegeben. Wenn Sie also Datensätze für den aktuellen Tag abrufen möchten, müssen Sie die LA-Rohtabellen verwenden.


## <a name="list-of-system-functions"></a>Liste der Systemfunktionen

### <a name="core-functions"></a>Kernfunktionen

#### <a name="_azurebackup_getvaults"></a>_AzureBackup_GetVaults()

Diese Funktion gibt die Liste aller Recovery Services-Tresore in ihrer Azure-Umgebung zurück, die dem LA-Arbeitsbereich zugeordnet sind.

**Parameter**

| **Parametername** | **Beschreibung** | **Erforderlich?** | **Beispielwert** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Verwenden Sie diesen Parameter zusammen mit dem RangeEnd-Parameter nur, wenn Sie alle tresorbezogenen Datensätze im Zeitraum von „RangeStart“ bis „RangeEnd“ abrufen müssen. Standardmäßig weisen „RangeStart“ und „RangeEnd“ den Wert „Null“ auf, wodurch die Funktion nur den letzten Datensatz für jeden Tresor abruft. | N | „2021-03-03 00:00:00“ |
| RangeEnd     | Verwenden Sie diesen Parameter zusammen mit dem RangeStart-Parameter nur, wenn Sie alle tresorbezogenen Datensätze im Zeitraum von „RangeStart“ bis „RangeEnd“ abrufen müssen. Standardmäßig weisen „RangeStart“ und „RangeEnd“ den Wert „Null“ auf, wodurch die Funktion nur den letzten Datensatz für jeden Tresor abruft. | N |„2021-03-10 00:00:00“|
| VaultSubscriptionList   | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Abonnements zu filtern, für die Sicherungsdaten vorhanden sind. Durch Angabe einer kommagetrennten Liste von Abonnement-IDs als Parameter für diese Funktion können Sie nur die Tresore abrufen, die sich in den angegebenen Abonnements befinden. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Abonnements sucht. | N | „00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111“|
| VaultLocationList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Regionen zu filtern, für die Sicherungsdaten vorhanden sind. Durch Angabe einer kommagetrennten Liste von Regionen als Parameter für diese Funktion können Sie nur die Tresore abrufen, die sich in den angegebenen Regionen befinden. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Regionen sucht. | N | „eastus,westus“|
| VaultList    |Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Tresoren zu filtern. Durch Angabe einer kommagetrennten Liste von Tresornamen als Parameter für diese Funktion können Sie Datensätze abrufen, die nur zu den angegebenen Tresoren gehören. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Tresoren sucht. | N |„vault1,vault2,vault3“|
| VaultTypeList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion auf Datensätze eines bestimmten Tresortyps zu filtern. Derzeit ist der einzige unterstützte Tresortyp „Microsoft.RecoveryServices/vaults“ (der Standardwert dieses Parameters). | N | „Microsoft.RecoveryServices/vaults“|
| ExcludeLegacyEvent  | Verwenden Sie diesen Parameter, um auszuwählen, ob Daten in der AzureDiagnostics-Tabelle der Vorgängerversion abgefragt werden sollen oder nicht. Wenn der Wert dieses Parameters „false“ lautet, fragt die Funktion Daten aus der AzureDiagnostics-Tabelle und den ressourcenspezifischen Tabellen ab. Wenn der Wert dieses Parameters „true“ lautet, fragt die Funktion nur Daten aus den ressourcenspezifischen Tabellen ab. Der Standardwert ist true. | N | true |

**Zurückgegebene Felder**

| **Feldname** | **Beschreibung** |
| -------------- | --------------- |
| UniqueId | Primärschlüssel, der die eindeutige ID des Tresors bezeichnet |
| Id | Azure Resource Manager-ID (ARM) des Tresors |
| Name | Name des Tresors |
| SubscriptionId | ID des Abonnements, in dem sich der Tresor befindet |
| Location | Speicherort, an dem sich der Tresor befindet |
| VaultStore_StorageReplicationType | Speicherreplikationstyp, der dem Tresor zugeordnet ist |
| Tags | Tags des Tresors |
| TimeGenerated | Zeitstempel des Datensatzes |
| Typ |  Typ des Tresors (Microsoft.RecoveryServices/vaults)|

#### <a name="_azurebackup_getpolicies"></a>_AzureBackup_GetPolicies()

Diese Funktion gibt die Liste der Sicherungsrichtlinien zurück, die in ihrer Azure-Umgebung verwendet werden, sowie ausführliche Informationen zu den einzelnen Richtlinien, wie z. B. Datenquellentyp, Speicherreplikationstyp usw.

**Parameter**

| **Parametername** | **Beschreibung** | **Erforderlich?** | **Beispielwert** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Verwenden Sie diesen Parameter zusammen mit dem RangeStart-Parameter nur, wenn Sie alle richtlinienbezogenen Datensätze im Zeitraum von „RangeStart“ bis „RangeEnd“ abrufen müssen. Standardmäßig weisen „RangeStart“ und „RangeEnd“ den Wert „Null“ auf, wodurch die Funktion nur den letzten Datensatz für jede Richtlinie abruft. | N | „2021-03-03 00:00:00“ |
| RangeEnd     | Verwenden Sie diesen Parameter zusammen mit dem RangeStart-Parameter nur, wenn Sie alle richtlinienbezogenen Datensätze im Zeitraum von „RangeStart“ bis „RangeEnd“ abrufen müssen. Standardmäßig weisen „RangeStart“ und „RangeEnd“ den Wert „Null“ auf, wodurch die Funktion nur den letzten Datensatz für jede Richtlinie abruft. | N |„2021-03-10 00:00:00“|
| VaultSubscriptionList   | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Abonnements zu filtern, für die Sicherungsdaten vorhanden sind. Durch die Angabe einer durch Trennzeichen getrennten Liste von Abonnement-IDs als Parameter für diese Funktion können Sie nur die Richtlinien abrufen, die in den angegebenen Abonnements enthalten sind. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Abonnements sucht. | N | „00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111“|
| VaultLocationList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Regionen zu filtern, für die Sicherungsdaten vorhanden sind. Durch Angabe einer kommagetrennten Liste von Regionen als Parameter für diese Funktion können Sie nur die Richtlinien abrufen, die sich in den angegebenen Regionen befinden. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Regionen sucht. | N | „eastus,westus“|
| VaultList    |Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Tresoren zu filtern. Durch Angabe einer kommagetrennten Liste von Tresornamen als Parameter für diese Funktion können Sie Datensätze von Richtlinien abrufen, die nur zu den angegebenen Tresoren gehören. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen von Richtlinien in allen Tresoren sucht. | N |„vault1,vault2,vault3“|
| VaultTypeList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion auf Datensätze eines bestimmten Tresortyps zu filtern. Derzeit ist der einzige unterstützte Tresortyp „Microsoft.RecoveryServices/vaults“ (der Standardwert dieses Parameters). | N | „Microsoft.RecoveryServices/vaults“|
| ExcludeLegacyEvent  | Verwenden Sie diesen Parameter, um auszuwählen, ob Daten in der AzureDiagnostics-Tabelle der Vorgängerversion abgefragt werden sollen oder nicht. Wenn der Wert dieses Parameters „false“ lautet, fragt die Funktion Daten aus der AzureDiagnostics-Tabelle und den ressourcenspezifischen Tabellen ab. Wenn der Wert dieses Parameters „true“ lautet, fragt die Funktion nur Daten aus den ressourcenspezifischen Tabellen ab. Der Standardwert ist true. | N | true |
| BackupSolutionList | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Sicherungslösungen zu filtern, die in ihrer Azure-Umgebung verwendet werden. Wenn Sie zum Beispiel „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM“ als Wert dieses Parameters angeben, gibt die Funktion nur Datensätze zurück, die sich auf Elemente beziehen, die mit Sicherung von Azure-VMs, SQL in Azure-VM-Sicherung oder DPM nach Azure Backup gesichert wurden. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion Datensätze zurückgibt, die zu allen von Sicherungsberichten unterstützten Sicherungslösungen gehören (unterstützte Werte sind „Sicherung von Azure-VMs“, „SQL in Azure-VM-Sicherung“, „SAP HANA in Azure-VM-Sicherung“, „Azure Storage (Azure Files)-Sicherung“, „Azure Backup-Agent“, „DPM“, „Azure Backup Server“ oder eine durch Komma getrennte Kombination dieser Werte). | N | „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM,Azure Backup-Agent“ |

**Zurückgegebene Felder**

| **Feldname** | **Beschreibung** |
| -------------- | --------------- |
| UniqueId | Primärschlüssel, der die eindeutige ID der Richtlinie bezeichnet |
| Id | Azure Resource Manager-ID (ARM) der Richtlinie |
| Name | Name der Richtlinie |
| Sicherungslösung | Sicherungslösung, der die Richtlinie zugeordnet ist. Beispielsweise „Azure-VM-Sicherung, SQL“ in „Azure-VM-Sicherung“ usw. |
| TimeGenerated | Zeitstempel des Datensatzes |
| VaultUniqueId | Fremdschlüssel, der auf den der Richtlinie zugeordneten Tresor verweist |
| VaultResourceId | Azure Resource Manager-ID (ARM) des der Richtlinie zugeordneten Tresors |
| VaultName | Name des der Richtlinie zugeordneten Tresors |
| VaultTags | Tags des der Richtlinie zugeordneten Tresors |
| VaultLocation | Speicherort des der Richtlinie zugeordneten Tresors |
| VaultSubscriptionId | Abonnement-ID des der Richtlinie zugeordneten Tresors |
| VaultStore_StorageReplicationType | Speicherreplikationstyp des Tresors, der der Richtlinie zugeordnet ist |
| VaultType | Typ des Tresors (Microsoft.RecoveryServices/vaults) |
| ExtendedProperties | Zusätzliche Eigenschaften der Richtlinie |

#### <a name="_azurebackup_getjobs"></a>_AzureBackup_GetJobs()

Diese Funktion gibt eine Liste aller Aufträge im Zusammenhang mit Sicherungen und Wiederherstellungen zurück, die in einem bestimmten Zeitraum ausgelöst wurden, zusammen mit ausführlichen Informationen zu jedem Auftrag, z. B. Auftragsstatus, Auftragsdauer, übertragene Daten usw.

**Parameter**

| **Parametername** | **Beschreibung** | **Erforderlich?** | **Beispielwert** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Verwenden Sie diesen Parameter zusammen mit dem RangeEnd-Parameter, um die Liste aller Aufträge abzurufen, die im Zeitraum von „RangeStart“ bis „RangeEnd“ gestartet wurden. | J | „2021-03-03 00:00:00“ |
| RangeEnd     | Verwenden Sie diesen Parameter zusammen mit dem RangeStart-Parameter, um die Liste aller Aufträge abzurufen, die im Zeitraum von „RangeStart“ bis „RangeEnd“ gestartet wurden. | J |„2021-03-10 00:00:00“|
| VaultSubscriptionList   | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Abonnements zu filtern, für die Sicherungsdaten vorhanden sind. Durch Angabe einer kommagetrennten Liste von Abonnement-IDs als Parameter für diese Funktion können Sie nur die Aufträge abrufen, die Tresoren in den angegebenen Abonnements zugeordnet sind. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Abonnements sucht. | N | „00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111“|
| VaultLocationList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Regionen zu filtern, für die Sicherungsdaten vorhanden sind. Durch Angabe einer kommagetrennten Liste von Regionen als Parameter für diese Funktion können Sie nur die Aufträge abrufen, die Tresoren in den angegebenen Abonnements zugeordnet sind. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Regionen sucht. | N | „eastus,westus“|
| VaultList    | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Tresoren zu filtern. Durch Angabe einer kommagetrennten Liste von Tresornamen als Parameter für diese Funktion können Sie Aufträge abrufen, die nur zu den angegebenen Tresoren gehören. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Aufträgen in allen Tresoren sucht. | N |„vault1,vault2,vault3“|
| VaultTypeList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion auf Datensätze eines bestimmten Tresortyps zu filtern. Derzeit ist der einzige unterstützte Tresortyp „Microsoft.RecoveryServices/vaults“ (der Standardwert dieses Parameters). | N | „Microsoft.RecoveryServices/vaults“|
| ExcludeLegacyEvent  | Verwenden Sie diesen Parameter, um auszuwählen, ob Daten in der AzureDiagnostics-Tabelle der Vorgängerversion abgefragt werden sollen oder nicht. Wenn der Wert dieses Parameters „false“ lautet, fragt die Funktion Daten aus der AzureDiagnostics-Tabelle und den ressourcenspezifischen Tabellen ab. Wenn der Wert dieses Parameters „true“ lautet, fragt die Funktion nur Daten aus den ressourcenspezifischen Tabellen ab. Der Standardwert ist true. | N | true |
| BackupSolutionList | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Sicherungslösungen zu filtern, die in ihrer Azure-Umgebung verwendet werden. Wenn Sie zum Beispiel „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM“ als Wert dieses Parameters angeben, gibt die Funktion nur Datensätze zurück, die sich auf Elemente beziehen, die mit Sicherung von Azure-VMs, SQL in Azure-VM-Sicherung oder DPM nach Azure Backup gesichert wurden. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion Datensätze zurückgibt, die zu allen von Sicherungsberichten unterstützten Sicherungslösungen gehören (unterstützte Werte sind „Sicherung von Azure-VMs“, „SQL in Azure-VM-Sicherung“, „SAP HANA in Azure-VM-Sicherung“, „Azure Storage (Azure Files)-Sicherung“, „Azure Backup-Agent“, „DPM“, „Azure Backup Server“ oder eine durch Komma getrennte Kombination dieser Werte). | N | „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM,Azure Backup-Agent“ |
| JobOperationList | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Auftragstyp zu filtern. Beispiel: Sicherung oder Wiederherstellung. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Sicherungs- und Wiederherstellungsaufträgen sucht. | N | „Backup“ | 
| JobStatusList | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Auftragsstatus zu filtern. Beispiel: „Completed“ (Abgeschlossen), „Failed“ (Fehler) usw. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach allen Aufträgen sucht, unabhängig von ihrem Status. | N | „Failed,CompletedWithWarnings“ |
| JobFailureCodeList | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Fehlercode zu filtern. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach allen Aufträgen sucht, unabhängig von ihrem Fehlercode. | N | „Success“
| DatasourceSetName | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion auf eine bestimmte übergeordnete Ressource zu filtern. Um zum Beispiel SQL in Azure VM-Sicherungsinstanzen zurückzugeben, die zum virtuellen Computer „testvm“ gehören, geben Sie _testvm_ als Wert dieses Parameters an. Standardmäßig lautet der Wert „*“, wodurch die Funktion nach Datensätzen in allen Sicherungsinstanzen sucht. | N | „testvm“ |
| BackupInstanceName | Verwenden Sie diesen Parameter, um anhand des Namens nach Aufträgen in einer bestimmten Sicherungsinstanz zu suchen. Standardmäßig lautet der Wert „*“, wodurch die Funktion nach Datensätzen in allen Sicherungsinstanzen sucht. | N | „testvm“ |
| ExcludeLog | Verwenden Sie diesen Parameter, um Protokollaufträge von der Rückgabe durch die Funktion auszuschließen (verbessert die Abfrageleistung). Standardmäßig lautet der Wert dieses Parameters „true“, wodurch die Funktion Protokollaufträge ausschließt. | N | true


**Zurückgegebene Felder**

| **Feldname** | **Beschreibung** |
| -------------- | --------------- |
| UniqueId | Primärschlüssel, der die eindeutige ID des Auftrags bezeichnet |
| OperationCategory | Kategorie des ausgeführten Vorgangs. Beispiel: „Backup“, „Restore“ |
| Vorgang | Details des ausgeführten Vorgangs. Beispiel: „Log“ (für Protokollsicherung)|
| Status | Status des Auftrags. Beispiel: „Completed“, „Failed“, „CompletedWithWarnings“ |
| ErrorTitle | Fehlercode des Auftrags |
| StartTime | Datum und Uhrzeit, zu dem der Auftrag gestartet wurde |
| DurationInSecs | Dauer des Auftrags in Sekunden |
| DataTransferredInMBs | Vom Auftrag übertragene Daten in MB |
| RestoreJobRPDateTime | Datum und Uhrzeit der Erstellung des Wiederherstellungspunkts, der wiederhergestellt wird |
| RestoreJobRPLocation | Speicherort des Wiederherstellungspunkts, der wiederhergestellt wird |
| BackupInstanceUniqueId | Fremdschlüssel, der auf die dem Auftrag zugeordnete Sicherungsinstanz verweist |
| BackupInstanceId | Azure Resource Manager-ID (ARM) der dem Auftrag zugeordneten Sicherungsinstanz |
| BackupInstanceFriendlyName | Name der dem Auftrag zugeordnete Sicherungsinstanz |
| DatasourceResourceId | Azure Resource Manager-ID (ARM) der dem Auftrag zugeordneten zugrunde liegenden Datenquelle. Beispiel: Azure Resource Manager-ID (ARM) der VM |
| DatasourceFriendlyName | Anzeigename der Datenquelle, die dem Auftrag zugeordnet ist |
| DatasourceType | Typ der Datenquelle, die dem Auftrag zugeordnet ist. Beispiel: „Microsoft.Compute/virtualMachines“ |
| BackupSolution | Sicherungslösung, der dem Auftrag zugeordnet ist. Beispielsweise „Azure-VM-Sicherung, SQL“ in „Azure-VM-Sicherung“ usw. |
| DatasourceSetResourceId | Azure Resource Manager-ID (ARM) der übergeordneten Ressource der Datenquelle (wo zutreffend). Für eine SQL in Azure-VM-Datenquelle enthält dieses Feld beispielsweise die Azure Resource Manager-ID (ARM) der VM, in der sich die SQL-Datenbank befindet |
| DatasourceSetType | Typ der übergeordneten Ressource der Datenquelle (wo zutreffend). Für eine SAP HANA in Azure VM-Datenquelle lautet dieses Feld beispielsweise „Microsoft.Compute/virtualMachines“, da die übergeordnete Ressource eine Azure-VM ist |
| VaultResourceId | Azure Resource Manager-ID (ARM) des dem Auftrag zugeordneten Tresors |
| VaultUniqueId | Fremdschlüssel, der auf den dem Auftrag zugeordneten Tresor verweist |
| VaultName | Name des dem Auftrag zugeordneten Tresors |
| VaultTags | Tag des dem Auftrag zugeordneten Tresors |
| VaultSubscriptionId | Abonnement-ID des dem Auftrag zugeordneten Tresors |
| VaultLocation | Speicherort des dem Auftrag zugeordneten Tresors |
| VaultStore_StorageReplicationType | Speicherreplikationstyp des Tresors, der dem Auftrag zugeordnet ist |
| VaultType | Typ des Tresors (Microsoft.RecoveryServices/vaults) |
| TimeGenerated | Zeitstempel des Datensatzes |

#### <a name="_azurebackup_getbackupinstances"></a>_AzureBackup_GetBackupInstances()

Diese Funktion gibt die Liste der Sicherungsinstanzen zurück, die ihren Recovery Services-Tresoren zugeordnet sind, zusammen mit ausführlichen Informationen zu jeder Sicherungsinstanz, z. B. Cloudspeicherverbrauch, zugehörige Richtlinie usw.

**Parameter**

| **Parametername** | **Beschreibung** | **Erforderlich?** | **Beispielwert** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Verwenden Sie diesen Parameter zusammen mit dem RangeEnd-Parameter nur, wenn Sie alle Datensätze im Zusammenhang mit Sicherungsinstanzen im Zeitraum von „RangeStart“ bis „RangeEnd“ abrufen müssen. Standardmäßig weisen „RangeStart“ und „RangeEnd“ den Wert „Null“ auf, wodurch die Funktion nur den letzten Datensatz für jede Sicherungsinstanz abruft. | N | „2021-03-03 00:00:00“ |
| RangeEnd     | Verwenden Sie diesen Parameter zusammen mit dem RangeStart-Parameter nur, wenn Sie alle Datensätze im Zusammenhang mit Sicherungsinstanzen im Zeitraum von „RangeStart“ bis „RangeEnd“ abrufen müssen. Standardmäßig weisen „RangeStart“ und „RangeEnd“ den Wert „Null“ auf, wodurch die Funktion nur den letzten Datensatz für jede Sicherungsinstanz abruft. | N |„2021-03-10 00:00:00“|
| VaultSubscriptionList   | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Abonnements zu filtern, für die Sicherungsdaten vorhanden sind. Durch die Angabe einer durch Trennzeichen getrennten Liste von Abonnement-IDs als Parameter für diese Funktion können Sie nur die Sicherungsinstanzen abrufen, die in den angegebenen Abonnements enthalten sind. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Abonnements sucht. | N | „00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111“|
| VaultLocationList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Regionen zu filtern, für die Sicherungsdaten vorhanden sind. Durch die Angabe einer durch Trennzeichen getrennten Liste von Regionen als Parameter für diese Funktion können Sie nur die Sicherungsinstanzen abrufen, die in den angegebenen Abonnements enthalten sind. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Regionen sucht. | N | „eastus,westus“|
| VaultList    |Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Tresoren zu filtern. Durch Angabe einer kommagetrennten Liste von Tresornamen als Parameter für diese Funktion können Sie Datensätze von Sicherungsinstanzen abrufen, die nur zu den angegebenen Tresoren gehören. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen von Sicherungsinstanzen in allen Tresoren sucht. | N |„vault1,vault2,vault3“|
| VaultTypeList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion auf Datensätze eines bestimmten Tresortyps zu filtern. Derzeit ist der einzige unterstützte Tresortyp „Microsoft.RecoveryServices/vaults“ (der Standardwert dieses Parameters). | N | „Microsoft.RecoveryServices/vaults“|
| ExcludeLegacyEvent  | Verwenden Sie diesen Parameter, um auszuwählen, ob Daten in der AzureDiagnostics-Tabelle der Vorgängerversion abgefragt werden sollen oder nicht. Wenn der Wert dieses Parameters „false“ lautet, fragt die Funktion Daten aus der AzureDiagnostics-Tabelle und den ressourcenspezifischen Tabellen ab. Wenn der Wert dieses Parameters „true“ lautet, fragt die Funktion nur Daten aus den ressourcenspezifischen Tabellen ab. Der Standardwert ist true. | N | true |
| BackupSolutionList | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Sicherungslösungen zu filtern, die in ihrer Azure-Umgebung verwendet werden. Wenn Sie zum Beispiel „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM“ als Wert dieses Parameters angeben, gibt die Funktion nur Datensätze zurück, die sich auf Elemente beziehen, die mit Sicherung von Azure-VMs, SQL in Azure-VM-Sicherung oder DPM nach Azure Backup gesichert wurden. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion Datensätze zurückgibt, die zu allen von Sicherungsberichten unterstützten Sicherungslösungen gehören (unterstützte Werte sind „Sicherung von Azure-VMs“, „SQL in Azure-VM-Sicherung“, „SAP HANA in Azure-VM-Sicherung“, „Azure Storage (Azure Files)-Sicherung“, „Azure Backup-Agent“, „DPM“, „Azure Backup Server“ oder eine durch Komma getrennte Kombination dieser Werte). | N | „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM,Azure Backup-Agent“ |
| ProtectionInfoList | Verwenden Sie diesen Parameter, um auszuwählen, ob nur die aktiv geschützten Sicherungsinstanzen berücksichtigt werden sollen, oder ob auch die Instanzen eingeschlossen werden sollen, für die der Schutz beendet wurde, und die Instanzen, für die die anfängliche Sicherung aussteht. Unterstützte Werte sind „Protected“, „ProtectionStopped“, „InitialBackupPending“ oder eine kommagetrennte Kombination dieser Werte. Standardmäßig lautet der Wert „*“, wodurch die Funktion nach allen Sicherungsinstanzen sucht, unabhängig von den Schutzdetails. | N | „Protected“ |
| DatasourceSetName | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion auf eine bestimmte übergeordnete Ressource zu filtern. Um zum Beispiel SQL in Azure VM-Sicherungsinstanzen zurückzugeben, die zum virtuellen Computer „testvm“ gehören, geben Sie _testvm_ als Wert dieses Parameters an. Standardmäßig lautet der Wert „*“, wodurch die Funktion nach Datensätzen in allen Sicherungsinstanzen sucht. | N | „testvm“ |
| BackupInstanceName | Verwenden Sie diesen Parameter, um anhand des Namens nach einer bestimmten Sicherungsinstanz zu suchen. Standardmäßig lautet der Wert „*“, sodass die Funktion nach allen Sicherungsinstanzen sucht. | N | „testvm“ |
| DisplayAllFields | Verwenden Sie diesen Parameter, um auszuwählen, ob nur eine Teilmenge der von der Funktion zurückgegebenen Felder abgerufen werden soll. Wenn der Wert dieses Parameters „false“ lautet, entfernt die Funktion Informationen, die sich auf Speicher und Aufbewahrungspunkt beziehen, aus der Ausgabe der Funktion. Dies ist nützlich, wenn Sie diese Funktion als Zwischenschritt in einer größeren Abfrage verwenden und die Leistung der Abfrage optimieren müssen, indem Sie Spalten eliminieren, die Sie für die Analyse nicht benötigen. Standardmäßig lautet der Wert dieses Parameters „true“, wodurch die Funktion alle Felder zurückgibt, die sich auf die Sicherungsinstanz beziehen. | N | true |

**Zurückgegebene Felder**

| **Feldname** | **Beschreibung** |
| -------------- | --------------- |
| UniqueId | Primärschlüssel, der die eindeutige ID der Sicherungsinstanz bezeichnet |
| Id | Azure Resource Manager-ID (ARM) der Sicherungsinstanz |
| FriendlyName | Anzeigename der Sicherungsinstanz |
| ProtectionInfo | Informationen zu den Schutzeinstellungen der Sicherungsinstanz. Zum Beispiel, „Schutz konfiguriert“, „Schutz beendet“, „Erste Sicherung ausstehend“ |
| LatestRecoveryPoint | Datum und Uhrzeit des letzten der Sicherungsinstanz zugeordneten Wiederherstellungspunkts |
| OldestRecoveryPoint | Datum und Uhrzeit des ältesten der Sicherungsinstanz zugeordneten Wiederherstellungspunkts |
| SourceSizeInMBs | Front-End-Größe der Sicherungsinstanz in MB |
| VaultStore_StorageConsumptionInMBs | Gesamter, von der Sicherungsinstanz in der Vault-Standard-Ebene belegter Cloudspeicher |
| DataSourceFriendlyName | Anzeigename der Datenquelle, die der Sicherungsinstanz entspricht |
| BackupSolution | Sicherungslösung, der die Sicherungsinstanz zugeordnet ist. Beispielsweise „Azure-VM-Sicherung, SQL“ in „Azure-VM-Sicherung“ usw. |
| DatasourceType | Typ der Datenquelle, die der Sicherungsinstanz entspricht. Beispiel: „Microsoft.Compute/virtualMachines“ |
| DatasourceResourceId | Azure Resource Manager-ID (ARM) der zugrunde liegenden Datenquelle, die der Sicherungsinstanz entspricht. Beispiel: Azure Resource Manager-ID (ARM) der VM |
| DatasourceSetFriendlyName | Anzeigename der übergeordneten Ressource der Datenquelle (wo zutreffend). Für eine SQL in Azure-VM-Datenquelle enthält dieses Feld beispielsweise der Name der VM, in der sich die SQL-Datenbank befindet |
| DatasourceSetResourceId | Azure Resource Manager-ID (ARM) der übergeordneten Ressource der Datenquelle (wo zutreffend). Für eine SQL in Azure-VM-Datenquelle enthält dieses Feld beispielsweise die Azure Resource Manager-ID (ARM) der VM, in der sich die SQL-Datenbank befindet |
| DatasourceSetType | Typ der übergeordneten Ressource der Datenquelle (wo zutreffend). Für eine SAP HANA in Azure VM-Datenquelle lautet dieses Feld beispielsweise „Microsoft.Compute/virtualMachines“, da die übergeordnete Ressource eine Azure-VM ist |
| PolicyName | Name der der Sicherungsinstanz zugeordneten Richtlinie |
| PolicyUniqueId | Fremdschlüssel, der auf die der Sicherungsinstanz zugeordnete Richtlinie verweist  |
| PolicyId | Azure Resource Manager-ID (ARM) der der Sicherungsinstanz zugeordneten Richtlinie |
| VaultResourceId | Azure Resource Manager-ID (ARM) des der Sicherungsinstanz zugeordneten Tresors |
| VaultUniqueId | Fremdschlüssel, der auf den der Sicherungsinstanz zugeordneten Tresor verweist |
| VaultName | Name des der Sicherungsinstanz zugeordneten Tresors |
| VaultTags | Tag des der Sicherungsinstanz zugeordneten Tresors |
| VaultSubscriptionId | Abonnement-ID des der Sicherungsinstanz zugeordneten Tresors |
| VaultLocation | Speicherort des der Sicherungsinstanz zugeordneten Tresors |
| VaultStore_StorageReplicationType | Speicherreplikationstyp des der Sicherungsinstanz zugeordneten Tresors |
| VaultType | Typ des Tresors (Microsoft.RecoveryServices/vaults) |
| TimeGenerated | Zeitstempel des Datensatzes |

#### <a name="_azurebackup_getbillinggroups"></a>_AzureBackup_GetBillingGroups()

Diese Funktion gibt eine Liste aller sicherungsbezogenen Abrechnungsentitäten (Abrechnungsgruppen) zusammen mit Informationen zu wichtigen Abrechnungskomponenten wie der Front-End-Größe und dem gesamten Cloudspeicher zurück.

**Parameter**

| **Parametername** | **Beschreibung** | **Erforderlich?** | **Beispielwert** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Verwenden Sie diesen Parameter zusammen mit dem RangeEnd-Parameter nur, wenn Sie alle Datensätze im Zusammenhang mit Abrechnungsgruppen im Zeitraum von „RangeStart“ bis „RangeEnd“ abrufen müssen. Standardmäßig weisen „RangeStart“ und „RangeEnd“ den Wert „Null“ auf, wodurch die Funktion nur den letzten Datensatz für jede Abrechnungsgruppe abruft. | N | „2021-03-03 00:00:00“ |
| RangeEnd     | Verwenden Sie diesen Parameter zusammen mit dem RangeStart-Parameter nur, wenn Sie alle Datensätze im Zusammenhang mit Abrechnungsgruppen im Zeitraum von „RangeStart“ bis „RangeEnd“ abrufen müssen. Standardmäßig weisen „RangeStart“ und „RangeEnd“ den Wert „Null“ auf, wodurch die Funktion nur den letzten Datensatz für jede Abrechnungsgruppe abruft. | N |„2021-03-10 00:00:00“|
| VaultSubscriptionList   | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Abonnements zu filtern, für die Sicherungsdaten vorhanden sind. Durch die Angabe einer durch Trennzeichen getrennten Liste von Abonnement-IDs als Parameter für diese Funktion können Sie nur die Abrechnungsgruppen abrufen, die in den angegebenen Abonnements enthalten sind. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Abonnements sucht. | N | „00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111“|
| VaultLocationList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Regionen zu filtern, für die Sicherungsdaten vorhanden sind. Durch die Angabe einer durch Trennzeichen getrennten Liste von Regionen als Parameter für diese Funktion können Sie nur die Abrechnungsgruppen abrufen, die in den angegebenen Abonnements enthalten sind. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Regionen sucht. | N | „eastus,westus“|
| VaultList    |Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Tresoren zu filtern. Durch Angabe einer kommagetrennten Liste von Tresornamen als Parameter für diese Funktion können Sie Datensätze von Sicherungsinstanzen abrufen, die nur zu den angegebenen Tresoren gehören. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen von Abrechnungsgruppen in allen Tresoren sucht. | N |„vault1,vault2,vault3“|
| VaultTypeList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion auf Datensätze eines bestimmten Tresortyps zu filtern. Derzeit ist der einzige unterstützte Tresortyp „Microsoft.RecoveryServices/vaults“ (der Standardwert dieses Parameters). | N | „Microsoft.RecoveryServices/vaults“|
| ExcludeLegacyEvent  | Verwenden Sie diesen Parameter, um auszuwählen, ob Daten in der AzureDiagnostics-Tabelle der Vorgängerversion abgefragt werden sollen oder nicht. Wenn der Wert dieses Parameters „false“ lautet, fragt die Funktion Daten aus der AzureDiagnostics-Tabelle und den ressourcenspezifischen Tabellen ab. Wenn der Wert dieses Parameters „true“ lautet, fragt die Funktion nur Daten aus den ressourcenspezifischen Tabellen ab. Der Standardwert ist true. | N | true |
| BackupSolutionList | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Sicherungslösungen zu filtern, die in ihrer Azure-Umgebung verwendet werden. Wenn Sie zum Beispiel „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM“ als Wert dieses Parameters angeben, gibt die Funktion nur Datensätze zurück, die sich auf Elemente beziehen, die mit Sicherung von Azure-VMs, SQL in Azure-VM-Sicherung oder DPM nach Azure Backup gesichert wurden. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion Datensätze zurückgibt, die zu allen von Sicherungsberichten unterstützten Sicherungslösungen gehören (unterstützte Werte sind „Sicherung von Azure-VMs“, „SQL in Azure-VM-Sicherung“, „SAP HANA in Azure-VM-Sicherung“, „Azure Storage (Azure Files)-Sicherung“, „Azure Backup-Agent“, „DPM“, „Azure Backup Server“ oder eine durch Komma getrennte Kombination dieser Werte). | N | „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM,Azure Backup-Agent“ |
| BillingGroupName | Verwenden Sie diesen Parameter, um nach einer bestimmten Abrechnungsgruppe anhand ihres Namens zu suchen. Standardmäßig lautet der Wert „*“, sodass die Funktion nach allen Abrechnungsgruppen sucht. | N | „testvm“ |

**Zurückgegebene Felder**

| **Feldname** | **Beschreibung** |
| -------------- | --------------- |
| UniqueId | Primärschlüssel, der die eindeutige ID der Abrechnungsgruppe bezeichnet |
| FriendlyName | Anzeigename der Abrechnungsgruppe |
| Name | Name der Abrechnungsgruppe |
| Typ | Typ der Abrechnungsgruppe. Beispielsweise „ProtectedContainer“ oder „BackupItem“ |
| SourceSizeInMBs | Front-End-Größe der Abrechnungsgruppe in MB |
| VaultStore_StorageConsumptionInMBs | Gesamter, von der Abrechnungsgruppe in der Vault-Standard-Ebene belegter Cloudspeicher |
| BackupSolution | Sicherungslösung, der die Abrechnungsgruppe zugeordnet ist. Beispielsweise „Azure-VM-Sicherung, SQL“ in „Azure-VM-Sicherung“ usw. |
| VaultResourceId | Azure Resource Manager-ID (ARM) des der Abrechnungsgruppe zugeordneten Tresors |
| VaultUniqueId | Fremdschlüssel, der auf den der Abrechnungsgruppe zugeordneten Tresor verweist |
| VaultName | Name des der Abrechnungsgruppe zugeordneten Tresors |
| VaultTags | Tags des der Abrechnungsgruppe zugeordneten Tresors |
| VaultSubscriptionId | Abonnement-ID des der Abrechnungsgruppe zugeordneten Tresors |
| VaultLocation | Speicherort des der Abrechnungsgruppe zugeordneten Tresors |
| VaultStore_StorageReplicationType | Speicherreplikationstyp des der Abrechnungsgruppe zugeordneten Tresors |
| VaultType | Typ des Tresors (Microsoft.RecoveryServices/vaults) |
| TimeGenerated | Zeitstempel des Datensatzes |
| ExtendedProperties | Zusätzliche Eigenschaften der Abrechnungsgruppe |

### <a name="trend-functions"></a>Trendfunktionen

#### <a name="_azurebackup_getbackupinstancestrends"></a>_AzureBackup_GetBackupInstancesTrends()

Diese Funktion gibt Verlaufsdatensätze für jede Sicherungsinstanz zurück, was Ihnen ermöglicht, wichtige tägliche, wöchentliche und monatliche Trends in Bezug auf die Anzahl der Sicherungsinstanzen und den Speicherverbrauch auf mehreren Granularitätsebenen anzeigen können.

**Parameter**

| **Parametername** | **Beschreibung** | **Erforderlich?** | **Beispielwert** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Verwenden Sie diesen Parameter zusammen mit dem RangeEnd-Parameter zum Abrufen aller Datensätze im Zusammenhang mit Sicherungsinstanzen im Zeitraum von „RangeStart“ bis „RangeEnd“. | J | „2021-03-03 00:00:00“ |
| RangeEnd     | Verwenden Sie diesen Parameter zusammen mit dem RangeStart-Parameter zum Abrufen aller Datensätze im Zusammenhang mit Sicherungsinstanzen im Zeitraum von „RangeStart“ bis „RangeEnd“. | J |„2021-03-10 00:00:00“|
| VaultSubscriptionList   | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Abonnements zu filtern, für die Sicherungsdaten vorhanden sind. Durch die Angabe einer durch Trennzeichen getrennten Liste von Abonnement-IDs als Parameter für diese Funktion können Sie nur die Sicherungsinstanzen abrufen, die in den angegebenen Abonnements enthalten sind. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Abonnements sucht. | N | „00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111“|
| VaultLocationList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Regionen zu filtern, für die Sicherungsdaten vorhanden sind. Durch die Angabe einer durch Trennzeichen getrennten Liste von Regionen als Parameter für diese Funktion können Sie nur die Sicherungsinstanzen abrufen, die in den angegebenen Abonnements enthalten sind. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Regionen sucht. | N | „eastus,westus“|
| VaultList    |Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Tresoren zu filtern. Durch Angabe einer kommagetrennten Liste von Tresornamen als Parameter für diese Funktion können Sie Datensätze von Sicherungsinstanzen abrufen, die nur zu den angegebenen Tresoren gehören. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen von Sicherungsinstanzen in allen Tresoren sucht. | N |„vault1,vault2,vault3“|
| VaultTypeList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion auf Datensätze eines bestimmten Tresortyps zu filtern. Derzeit ist der einzige unterstützte Tresortyp „Microsoft.RecoveryServices/vaults“ (der Standardwert dieses Parameters). | N | „Microsoft.RecoveryServices/vaults“|
| ExcludeLegacyEvent  | Verwenden Sie diesen Parameter, um auszuwählen, ob Daten in der AzureDiagnostics-Tabelle der Vorgängerversion abgefragt werden sollen oder nicht. Wenn der Wert dieses Parameters „false“ lautet, fragt die Funktion Daten aus der AzureDiagnostics-Tabelle und den ressourcenspezifischen Tabellen ab. Wenn der Wert dieses Parameters „true“ lautet, fragt die Funktion nur Daten aus den ressourcenspezifischen Tabellen ab. Der Standardwert ist true. | N | true |
| BackupSolutionList | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Sicherungslösungen zu filtern, die in ihrer Azure-Umgebung verwendet werden. Wenn Sie zum Beispiel „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM“ als Wert dieses Parameters angeben, gibt die Funktion nur Datensätze zurück, die sich auf Elemente beziehen, die mit Sicherung von Azure-VMs, SQL in Azure-VM-Sicherung oder DPM nach Azure Backup gesichert wurden. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion Datensätze zurückgibt, die zu allen von Sicherungsberichten unterstützten Sicherungslösungen gehören (unterstützte Werte sind „Sicherung von Azure-VMs“, „SQL in Azure-VM-Sicherung“, „SAP HANA in Azure-VM-Sicherung“, „Azure Storage (Azure Files)-Sicherung“, „Azure Backup-Agent“, „DPM“, „Azure Backup Server“ oder eine durch Komma getrennte Kombination dieser Werte). | N | „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM,Azure Backup-Agent“ |
| ProtectionInfoList | Verwenden Sie diesen Parameter, um auszuwählen, ob nur die aktiv geschützten Sicherungsinstanzen berücksichtigt werden sollen, oder ob auch die Instanzen eingeschlossen werden sollen, für die der Schutz beendet wurde, und die Instanzen, für die die anfängliche Sicherung aussteht. Unterstützte Werte sind „Protected“, „ProtectionStopped“, „InitialBackupPending“ oder eine kommagetrennte Kombination dieser Werte. Standardmäßig lautet der Wert „*“, wodurch die Funktion nach allen Sicherungsinstanzen sucht, unabhängig von den Schutzdetails. | N | „Protected“ |
| DatasourceSetName | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion auf eine bestimmte übergeordnete Ressource zu filtern. Um zum Beispiel SQL in Azure VM-Sicherungsinstanzen zurückzugeben, die zum virtuellen Computer „testvm“ gehören, geben Sie _testvm_ als Wert dieses Parameters an. Standardmäßig lautet der Wert „*“, wodurch die Funktion nach Datensätzen in allen Sicherungsinstanzen sucht. | N | „testvm“ |
| BackupInstanceName | Verwenden Sie diesen Parameter, um anhand des Namens nach einer bestimmten Sicherungsinstanz zu suchen. Standardmäßig lautet der Wert „*“, sodass die Funktion nach allen Sicherungsinstanzen sucht. | N | „testvm“ |
| DisplayAllFields | Verwenden Sie diesen Parameter, um auszuwählen, ob nur eine Teilmenge der von der Funktion zurückgegebenen Felder abgerufen werden soll. Wenn der Wert dieses Parameters „false“ lautet, entfernt die Funktion Informationen, die sich auf Speicher und Aufbewahrungspunkt beziehen, aus der Ausgabe der Funktion. Dies ist nützlich, wenn Sie diese Funktion als Zwischenschritt in einer größeren Abfrage verwenden und die Leistung der Abfrage optimieren müssen, indem Sie Spalten eliminieren, die Sie für die Analyse nicht benötigen. Standardmäßig lautet der Wert dieses Parameters „true“, wodurch die Funktion alle Felder zurückgibt, die sich auf die Sicherungsinstanz beziehen. | N | true |
| AggregationType | Verwenden Sie diesen Parameter, um die zeitliche Granularität anzugeben, mit der Daten abgerufen werden sollen. Wenn der Wert dieses Parameters „Täglich“ lautet, gibt die Funktion einen Datensatz pro Sicherungsinstanz pro Tag zurück, sodass Sie die täglichen Trends des Speicherverbrauchs und der Front-End-Größe analysieren können. Wenn der Wert dieses Parameters „Wöchentlich“ lautet, gibt die Funktion einen Datensatz pro Sicherungsinstanz pro Woche zurück, sodass Sie wöchentliche Trends analysieren können. Entsprechend können Sie „Monatlich“ angeben, um monatliche Trends zu analysieren. Standardwert ist „Täglich“. Wenn Sie Daten über größere Zeiträume betrachten, empfiehlt es sich, „Wöchentlich“ oder „Monatlich“ zu verwenden, um die Abfrageleistung zu verbessern und die Trendanalyse zu erleichtern. | N | „Wöchentlich“ |

**Zurückgegebene Felder**

| **Feldname** | **Beschreibung** |
| -------------- | --------------- |
| UniqueId | Primärschlüssel, der die eindeutige ID der Sicherungsinstanz bezeichnet |
| Id | Azure Resource Manager-ID (ARM) der Sicherungsinstanz |
| FriendlyName | Anzeigename der Sicherungsinstanz |
| ProtectionInfo | Informationen zu den Schutzeinstellungen der Sicherungsinstanz. Zum Beispiel, „Schutz konfiguriert“, „Schutz beendet“, „Erste Sicherung ausstehend“ |
| LatestRecoveryPoint | Datum und Uhrzeit des letzten der Sicherungsinstanz zugeordneten Wiederherstellungspunkts |
| OldestRecoveryPoint | Datum und Uhrzeit des ältesten der Sicherungsinstanz zugeordneten Wiederherstellungspunkts |
| SourceSizeInMBs | Front-End-Größe der Sicherungsinstanz in MB |
| VaultStore_StorageConsumptionInMBs | Gesamter, von der Sicherungsinstanz in der Vault-Standard-Ebene belegter Cloudspeicher |
| DataSourceFriendlyName | Anzeigename der Datenquelle, die der Sicherungsinstanz entspricht |
| BackupSolution | Sicherungslösung, der die Sicherungsinstanz zugeordnet ist. Beispielsweise „Azure-VM-Sicherung, SQL“ in „Azure-VM-Sicherung“ usw. |
| DatasourceType | Typ der Datenquelle, die der Sicherungsinstanz entspricht. Beispiel: „Microsoft.Compute/virtualMachines“ |
| DatasourceResourceId | Azure Resource Manager-ID (ARM) der zugrunde liegenden Datenquelle, die der Sicherungsinstanz entspricht. Beispiel: Azure Resource Manager-ID (ARM) der VM |
| DatasourceSetFriendlyName | Anzeigename der übergeordneten Ressource der Datenquelle (wo zutreffend). Für eine SQL in Azure-VM-Datenquelle enthält dieses Feld beispielsweise der Name der VM, in der sich die SQL-Datenbank befindet |
| DatasourceSetResourceId | Azure Resource Manager-ID (ARM) der übergeordneten Ressource der Datenquelle (wo zutreffend). Für eine SQL in Azure-VM-Datenquelle enthält dieses Feld beispielsweise die Azure Resource Manager-ID (ARM) der VM, in der sich die SQL-Datenbank befindet |
| DatasourceSetType | Typ der übergeordneten Ressource der Datenquelle (wo zutreffend). Für eine SAP HANA in Azure VM-Datenquelle lautet dieses Feld beispielsweise „Microsoft.Compute/virtualMachines“, da die übergeordnete Ressource eine Azure-VM ist |
| PolicyName | Name der der Sicherungsinstanz zugeordneten Richtlinie |
| PolicyUniqueId | Fremdschlüssel, der auf die der Sicherungsinstanz zugeordnete Richtlinie verweist  |
| PolicyId | Azure Resource Manager-ID (ARM) der der Sicherungsinstanz zugeordneten Richtlinie |
| VaultResourceId | Azure Resource Manager-ID (ARM) des der Sicherungsinstanz zugeordneten Tresors |
| VaultUniqueId | Fremdschlüssel, der auf den der Sicherungsinstanz zugeordneten Tresor verweist |
| VaultName | Name des der Sicherungsinstanz zugeordneten Tresors |
| VaultTags | Tag des der Sicherungsinstanz zugeordneten Tresors |
| VaultSubscriptionId | Abonnement-ID des der Sicherungsinstanz zugeordneten Tresors |
| VaultLocation | Speicherort des der Sicherungsinstanz zugeordneten Tresors |
| VaultStore_StorageReplicationType | Speicherreplikationstyp des der Sicherungsinstanz zugeordneten Tresors |
| VaultType | Typ des Tresors (Microsoft.RecoveryServices/vaults) |
| TimeGenerated | Zeitstempel des Datensatzes |

#### <a name="_azurebackup_getbillinggroupstrends"></a>_AzureBackup_GetBillingGroupsTrends()

Diese Funktion gibt Verlaufsdatensätze für jede Abrechnungsentität zurück, was Ihnen ermöglicht, wichtige tägliche, wöchentliche und monatliche Trends in Bezug auf die Front-End-Größe und den Speicherverbrauch auf mehreren Granularitätsebenen anzeigen können.

**Parameter**

| **Parametername** | **Beschreibung** | **Erforderlich?** | **Beispielwert** |
| -------------------- | ------------- | --------------- | ----------------- |
| RangeStart     | Verwenden Sie diesen Parameter zusammen mit dem RangeEnd-Parameter zum Abrufen aller Datensätze im Zusammenhang mit Abrechnungsgruppen im Zeitraum von „RangeStart“ bis „RangeEnd“. | J | „2021-03-03 00:00:00“ |
| RangeEnd     | Verwenden Sie diesen Parameter zusammen mit dem RangeStart-Parameter zum Abrufen aller Datensätze im Zusammenhang mit Abrechnungsgruppen im Zeitraum von „RangeStart“ bis „RangeEnd“. | J |„2021-03-10 00:00:00“|
| VaultSubscriptionList  | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Abonnements zu filtern, für die Sicherungsdaten vorhanden sind. Durch die Angabe einer durch Trennzeichen getrennten Liste von Abonnement-IDs als Parameter für diese Funktion können Sie nur die Abrechnungsgruppen abrufen, die in den angegebenen Abonnements enthalten sind. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Abonnements sucht. | N | „00000000-0000-0000-0000-000000000000,11111111-1111-1111-1111-111111111111“|
| VaultLocationList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Regionen zu filtern, für die Sicherungsdaten vorhanden sind. Durch die Angabe einer durch Trennzeichen getrennten Liste von Regionen als Parameter für diese Funktion können Sie nur die Abrechnungsgruppen abrufen, die in den angegebenen Abonnements enthalten sind. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen in allen Regionen sucht. | N | „eastus,westus“|
| VaultList    |Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Tresoren zu filtern. Durch Angabe einer kommagetrennten Liste von Tresornamen als Parameter für diese Funktion können Sie Datensätze von Sicherungsinstanzen abrufen, die nur zu den angegebenen Tresoren gehören. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion nach Datensätzen von Abrechnungsgruppen in allen Tresoren sucht. | N |„vault1,vault2,vault3“|
| VaultTypeList     | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion auf Datensätze eines bestimmten Tresortyps zu filtern. Derzeit ist der einzige unterstützte Tresortyp „Microsoft.RecoveryServices/vaults“ (der Standardwert dieses Parameters). | N | „Microsoft.RecoveryServices/vaults“|
| ExcludeLegacyEvent  | Verwenden Sie diesen Parameter, um auszuwählen, ob Daten in der AzureDiagnostics-Tabelle der Vorgängerversion abgefragt werden sollen oder nicht. Wenn der Wert dieses Parameters „false“ lautet, fragt die Funktion Daten aus der AzureDiagnostics-Tabelle und den ressourcenspezifischen Tabellen ab. Wenn der Wert dieses Parameters „true“ lautet, fragt die Funktion nur Daten aus den ressourcenspezifischen Tabellen ab. Der Standardwert ist true. | N | true |
| BackupSolutionList | Verwenden Sie diesen Parameter, um die Ausgabe der Funktion für einen bestimmten Satz von Sicherungslösungen zu filtern, die in ihrer Azure-Umgebung verwendet werden. Wenn Sie zum Beispiel „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM“ als Wert dieses Parameters angeben, gibt die Funktion nur Datensätze zurück, die sich auf Elemente beziehen, die mit Sicherung von Azure-VMs, SQL in Azure-VM-Sicherung oder DPM nach Azure Backup gesichert wurden. Standardmäßig lautet der Wert dieses Parameters „*“, wodurch die Funktion Datensätze zurückgibt, die zu allen von Sicherungsberichten unterstützten Sicherungslösungen gehören (unterstützte Werte sind „Sicherung von Azure-VMs“, „SQL in Azure-VM-Sicherung“, „SAP HANA in Azure-VM-Sicherung“, „Azure Storage (Azure Files)-Sicherung“, „Azure Backup-Agent“, „DPM“, „Azure Backup Server“ oder eine durch Komma getrennte Kombination dieser Werte). | N | „Sicherung von Azure-VMs,SQL in Azure-VM-Sicherung,DPM,Azure Backup-Agent“ |
| BillingGroupName | Verwenden Sie diesen Parameter, um nach einer bestimmten Abrechnungsgruppe anhand ihres Namens zu suchen. Standardmäßig lautet der Wert „*“, sodass die Funktion nach allen Abrechnungsgruppen sucht. | N | „testvm“ |
| AggregationType | Verwenden Sie diesen Parameter, um die zeitliche Granularität anzugeben, mit der Daten abgerufen werden sollen. Wenn der Wert dieses Parameters „Täglich“ lautet, gibt die Funktion einen Datensatz pro Abrechnungsgruppe pro Tag zurück, sodass Sie die täglichen Trends des Speicherverbrauchs und der Front-End-Größe analysieren können. Wenn der Wert dieses Parameters „Wöchentlich“ lautet, gibt die Funktion einen Datensatz pro Sicherungsinstanz pro Woche zurück, sodass Sie wöchentliche Trends analysieren können. Entsprechend können Sie „Monatlich“ angeben, um monatliche Trends zu analysieren. Standardwert ist „Täglich“. Wenn Sie Daten über größere Zeiträume betrachten, empfiehlt es sich, „Wöchentlich“ oder „Monatlich“ zu verwenden, um die Abfrageleistung zu verbessern und die Trendanalyse zu erleichtern. | N | „Wöchentlich“ |

**Zurückgegebene Felder**

| **Feldname** | **Beschreibung** |
| -------------- | --------------- |
| UniqueId | Primärschlüssel, der die eindeutige ID der Abrechnungsgruppe bezeichnet |
| FriendlyName | Anzeigename der Abrechnungsgruppe |
| Name | Name der Abrechnungsgruppe |
| Typ | Typ der Abrechnungsgruppe. Beispielsweise „ProtectedContainer“ oder „BackupItem“ |
| SourceSizeInMBs | Front-End-Größe der Abrechnungsgruppe in MB |
| VaultStore_StorageConsumptionInMBs | Gesamter, von der Abrechnungsgruppe in der Vault-Standard-Ebene belegter Cloudspeicher |
| BackupSolution | Sicherungslösung, der die Abrechnungsgruppe zugeordnet ist. Beispielsweise „Azure-VM-Sicherung, SQL“ in „Azure-VM-Sicherung“ usw. |
| VaultResourceId | Azure Resource Manager-ID (ARM) des der Abrechnungsgruppe zugeordneten Tresors |
| VaultUniqueId | Fremdschlüssel, der auf den der Abrechnungsgruppe zugeordneten Tresor verweist |
| VaultName | Name des der Abrechnungsgruppe zugeordneten Tresors |
| VaultTags | Tags des der Abrechnungsgruppe zugeordneten Tresors |
| VaultSubscriptionId | Abonnement-ID des der Abrechnungsgruppe zugeordneten Tresors |
| VaultLocation | Speicherort des der Abrechnungsgruppe zugeordneten Tresors |
| VaultStore_StorageReplicationType | Speicherreplikationstyp des der Abrechnungsgruppe zugeordneten Tresors |
| VaultType | Typ des Tresors (Microsoft.RecoveryServices/vaults) |
| TimeGenerated | Zeitstempel des Datensatzes |
| ExtendedProperties | Zusätzliche Eigenschaften der Abrechnungsgruppe |

## <a name="sample-queries"></a>Beispielabfragen

Im Folgenden finden Sie einige Beispielabfragen, die Ihnen beim Einstieg in die Verwendung von Systemfunktionen helfen.

- Alle fehlgeschlagenen Azure-VM-Sicherungsaufträge in einem bestimmten Zeitraum

    ````Kusto
    _AzureBackup_GetJobs("2021-03-05", "2021-03-06") //call function with RangeStart and RangeEnd parameters set, and other parameters with default value
    | where BackupSolution=="Azure Virtual Machine Backup" and Status=="Failed"
    | project BackupInstanceFriendlyName, BackupInstanceId, OperationCategory, Status,  JobStartDateTime=StartTime, JobDuration=DurationInSecs/3600, ErrorTitle, DataTransferred=DataTransferredInMBs
    ````

- Alle SQL-Protokollsicherungsaufträge in einem bestimmten Zeitraum

    ````Kusto
    _AzureBackup_GetJobs("2021-03-05", "2021-03-06","*","*","*","*",true,"*","*","*","*","*","*",false) //call function with RangeStart and RangeEnd parameters set, ExcludeLog parameter as false, and other parameters with default value
    | where BackupSolution=="SQL in Azure VM Backup" and Operation=="Log"
    | project BackupInstanceFriendlyName, BackupInstanceId, OperationCategory, Status,  JobStartDateTime=StartTime, JobDuration=DurationInSecs/3600, ErrorTitle, DataTransferred=DataTransferredInMBs
    ````

- Wöchentlicher Trend des für den virtuellen Computer „testvm“ verbrauchten Sicherungsspeichers

    ````Kusto
    _AzureBackup_GetBackupInstancesTrends("2021-01-01", "2021-03-06","*","*","*","*",false,"*","*","*","*",true, "Weekly") //call function with RangeStart and RangeEnd parameters set, AggregationType parameter as Weekly, and other parameters with default value
    | where BackupSolution == "Azure Virtual Machine Backup"
    | where FriendlyName == "testvm"
    | project TimeGenerated, VaultStore_StorageConsumptionInMBs
    | render timechart 
    ````

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zu Azure Backup-Berichten](https://docs.microsoft.com/azure/backup/configure-reports)
