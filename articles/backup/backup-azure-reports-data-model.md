---
title: Datenmodell für Azure Backup-Diagnoseereignisse
description: Dieses Datenmodell bezieht sich auf den ressourcenspezifischen Modus zum Senden von Diagnoseereignissen an Log Analytics (LA).
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 0d75af6d2b41aad0b5f821dd1f6409b30f7ca531
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102499593"
---
# <a name="data-model-for-azure-backup-diagnostics-events"></a>Datenmodell für Azure Backup-Diagnoseereignisse

> [!NOTE]
>
> Zum Erstellen von benutzerdefinierten Berichtsansichten wird empfohlen, anstelle der unten aufgeführten Rohtabellen die [Systemfunktionen für Azure Monitor-Protokolle](backup-reports-system-functions.md) zu verwenden.

## <a name="coreazurebackup"></a>CoreAzureBackup

Diese Tabelle enthält Informationen zu den wichtigsten Sicherungsentitäten wie Tresoren und Sicherungselementen.

| **Feld**                         | **Datentyp** | **Beschreibung**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| resourceId                        | Text          | Ressourcenbezeichner der Daten, die erfasst werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors |
| Vorgangsname                     | Text          | Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: BackupItem, BackupItemAssociation oder ProtectedContainer |
| Category                          | Text          | Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die mithilfe von Push an Azure Monitor-Protokolle übermittelt werden. Beispiel: CoreAzureBackup |
| AgentVersion                      | Text          | Versionsnummer der Agent-Sicherung oder des Schutz-Agents (im Fall von SC DPM und MABS) |
| AzureBackupAgentVersion           | Text          | Version des Azure Backup-Agenten auf dem Sicherungsverwaltungsserver |
| AzureDataCenter                   | Text          | Rechenzentrum, in dem sich der Tresor befindet                       |
| BackupItemAppVersion              | Text          | Anwendungsversion des Sicherungselements                       |
| BackupItemFriendlyName            | Text          | Anzeigename des Sicherungselements                             |
| BackupItemName                    | Text          | Name des Sicherungselements                                      |
| BackupItemProtectionState         | Text          | Schutzstatus des Sicherungselements                          |
| BackupItemFrontEndSize            | Text          | Front-End-Größe des Sicherungselements                            |
| BackupItemType                    | Text          | Typ des Sicherungselements. Beispiel: VM und FileFolder             |
| BackupItemUniqueId                | Text          | Eindeutiger Bezeichner des Sicherungselements                         |
| BackupManagementServerType        | Text          | Typ des Sicherungsverwaltungsservers, z. B. in MABS und SC DPM     |
| BackupManagementServerUniqueId    | Text          | Feld, um den Sicherungsverwaltungsserver eindeutig zu bestimmen      |
| BackupManagementType              | Text          | Anbietertyp für den Server, der den Sicherungsauftrag ausführt. Beispielsweise IaaSVM, FileFolder. |
| BackupManagementServerName        | Text          | Name des Servers für die Sicherungsverwaltung                         |
| BackupManagementServerOSVersion   | Text          | Betriebssystemversion des Servers für die Sicherungsverwaltung                   |
| BackupManagementServerVersion     | Text          | Version des Servers für die Sicherungsverwaltung                      |
| LatestRecoveryPointLocation       | Text          | Speicherort des aktuellsten Wiederherstellungspunkts des Sicherungselements    |
| LatestRecoveryPointTime           | Datetime      | Datum und Uhrzeit des aktuellsten Wiederherstellungspunkts des Sicherungselements   |
| OldestRecoveryPointLocation       | Text          | Speicherort des ältesten Wiederherstellungspunkts des Sicherungselements    |
| OldestRecoveryPointTime           | Datetime      | Datum und Uhrzeit des aktuellsten Wiederherstellungspunkts des Sicherungselements   |
| PolicyUniqueId                    | Text          | Eindeutige ID zur Bezeichnung der Richtlinie                             |
| ProtectedContainerFriendlyName    | Text          | Anzeigename des geschützten Servers                        |
| ProtectedContainerLocation        | Text          | Gibt an, ob der geschützte Container ein lokaler Container ist oder sich in Azure befindet |
| ProtectedContainerName            | Text          | Name des geschützten Containers                            |
| ProtectedContainerOSType          | Text          | Betriebssystemtyp des geschützten Containers                          |
| ProtectedContainerOSVersion       | Text          | Betriebssystemversion des geschützten Containers                        |
| ProtectedContainerProtectionState | Text          | Schutzstatus des geschützten Containers                  |
| ProtectedContainerType            | Text          | Gibt an, ob der geschützte Container ein Server oder ein Container ist  |
| ProtectedContainerUniqueId        | Text          | Eindeutige ID, mithilfe derer der geschützte Container für alles außer VMs bestimmt wird, die mithilfe von DPM, MABS geschützt wurden |
| ProtectedContainerWorkloadType    | Text          | Typ des gesicherten geschützten Containers. Beispielsweise IaaSVMContainer. |
| ProtectionGroupName               | Text          | Name der Schutzgruppe, in der das Sicherungselement geschützt wird, (falls zutreffend) für System Center Data Protection Manager (SC DPM) und Microsoft Azure Backup Server (MABS) |
| ResourceGroupName                 | Text          | Ressourcengruppe der Ressource (z. B. Recovery Services-Tresor) für erfasste Daten |
| SchemaVersion                     | Text          | Dieses Feld gibt die aktuelle Version des Schemas an. Dies ist **V2**. |
| SecondaryBackupProtectionState    | Text          | Gibt an, ob der sekundäre Schutz für das Sicherungselement aktiviert ist  |
| State                             | Text          | Status des Sicherungselementobjekts. Beispielsweise Active (Aktiv), Deleted (Gelöscht). |
| StorageReplicationType            | Text          | Typ der Speicherreplikation für den Tresor. Beispiel: GeoRedundant. |
| SubscriptionId                    | Text          | Abonnementbezeichner der Ressource (z. B. Recovery Services-Tresor), für die Daten erfasst werden |
| VaultName                         | Text          | Name des Tresors                                            |
| VaultTags                         | Text          | Tags, die der Tresorressource zugeordnet sind                    |
| VaultUniqueId                     | Text          | eindeutiger Bezeichner des Tresors                             |
| SourceSystem                      | Text          | Quellsystem der aktuellen Daten: Azure                  |

## <a name="addonazurebackupalerts"></a>AddonAzureBackupAlerts

Diese Tabelle enthält Details zu warnungsbezogenen Feldern.

| **Feld**                      | **Datentyp** | **Beschreibung**                                              |
| :----------------------------- | ------------- | ------------------------------------------------------------ |
| resourceId                     | Text          | Eindeutiger Bezeichner der Ressource, zu der Daten gesammelt werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors |
| Vorgangsname                  | Text          | Name des aktuellen Vorgangs. Beispiel: Warnung            |
| Category                       | Text          | Kategorie der Diagnosedaten, die mithilfe von Push an Azure Monitor-Protokolle übermittelt werden (z. B. AddonAzureBackupAlerts) |
| AlertCode                      | Text          | Code für die eindeutige Identifizierung eines Warnungstyps                     |
| AlertConsolidationStatus       | Text          | Erkennen, ob die Warnung eine konsolidierte Warnung ist oder nicht         |
| AlertOccurrenceDateTime        | Datetime      | Datum und Uhrzeit der Warnungserstellung                     |
| AlertRaisedOn                  | Text          | Entitätstyp, aufgrund dessen die Warnung ausgelöst wurde                        |
| AlertSeverity                  | Text          | Schweregrad der Warnung Beispiel: Critical (Kritisch).                 |
| AlertStatus                    | Text          | Status der Warnung. Beispiel: Active (Aktiv).                     |
| AlertTimeToResolveInMinutes    | Number        | Zeit, die benötigt wurde, um eine Warnung zu klären. Leer für aktive Warnungen.     |
| AlertType                      | Text          | Warnungstyp. Beispiel: Backup (Sicherung).                           |
| AlertUniqueId                  | Text          | eindeutiger Bezeichner der generierten Warnung                    |
| BackupItemUniqueId             | Text          | eindeutiger Bezeichner des Sicherungselements, das der Warnung zugeordnet ist |
| BackupManagementServerUniqueId | Text          | Feld für die eindeutige Identifizierung des Servers für die Sicherungsverwaltung, durch den das Sicherungselement geschützt wird (falls zutreffend) |
| BackupManagementType           | Text          | Anbietertyp für den Server zur Durchführung des Sicherungsjobs (z. B. IaaSVM und FileFolder) |
| CountOfAlertsConsolidated      | Number        | Anzahl konsolidierter Warnungen, wenn es sich um eine konsolidierten Warnung handelt  |
| ProtectedContainerUniqueId     | Text          | eindeutiger Bezeichner des geschützten Servers, der der Warnung zugeordnet ist |
| RecommendedAction              | Text          | Empfohlene Aktion, um eine Warnung zu klären                      |
| SchemaVersion                  | Text          | Aktuelle Version des Schemas, z. B. **V2**            |
| State                          | Text          | Aktueller Status des Warnungsobjekts, z.B. „Aktiv“, „Gelöscht“ |
| StorageUniqueId                | Text          | Eindeutige ID, mithilfe derer die Speicherentität bestimmt wird                |
| VaultUniqueId                  | Text          | eindeutige ID zum Bestimmen des Tresors, auf den sich die Warnung bezieht    |
| SourceSystem                   | Text          | Quellsystem der aktuellen Daten: Azure                    |

## <a name="addonazurebackupprotectedinstance"></a>AddonAzureBackupProtectedInstance

Diese Tabelle enthält grundlegende Felder zu geschützten Instanzen.

| **Feld**                      | **Datentyp** | **Beschreibung**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| resourceId                     | Text          | eindeutiger Bezeichner der Ressource, zu der Daten gesammelt werden. Beispiel: eine Ressourcen-ID des Recovery Services-Tresors |
| Vorgangsname                  | Text          | Name des Vorgangs (z. B. ProtectedInstance)         |
| Category                       | Text          | Kategorie der Diagnosedaten, die mithilfe von Push an Azure Monitor-Protokolle übertragen werden (z. B. AddonAzureBackupProtectedInstance) |
| BackupItemUniqueId             | Text          | Eindeutige ID des Sicherungselements                                 |
| BackupManagementServerUniqueId | Text          | Feld zur eindeutigen Identifizierung des Servers für die Sicherungsverwaltung, durch den das Sicherungselement geschützt wird (falls zutreffend) |
| BackupManagementType           | Text          | Anbietertyp für den Server zur Durchführung des Sicherungsjobs (z. B. IaaSVM und FileFolder) |
| ProtectedContainerUniqueId     | Text          | eindeutige ID zum Identifizieren des geschützten Containers, in dem der Auftrag ausgeführt wird |
| ProtectedInstanceCount         | Text          | Anzahl geschützter Instanzen für das zugeordnete Sicherungselement oder den geschützten Container an diesem Datum und zu dieser Uhrzeit |
| SchemaVersion                  | Text          | Aktuelle Version des Schemas, z. B. **V2**            |
| State                          | Text          | Status des Sicherungselementobjekts (z. B. „Aktiv“ oder „Gelöscht“) |
| VaultUniqueId                  | Text          | eindeutiger Bezeichner des geschützten Tresors, der der geschützten Instanz zugeordnet ist |
| SourceSystem                   | Text          | Quellsystem der aktuellen Daten: Azure                    |

## <a name="addonazurebackupjobs"></a>AddonAzureBackupJobs

Diese Tabelle enthält Details zu auftragsbezogenen Feldern.

| **Feld**                      | **Datentyp** | **Beschreibung**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| resourceId                     | Text          | Ressourcenbezeichner der Daten, die erfasst werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors |
| Vorgangsname                  | Text          | Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: Auftrag    |
| Category                       | Text          | Dieses Feld stellt die Kategorie der Diagnosedaten dar, die per Push an Azure Monitor-Protokolle übermittelt werden – AddonAzureBackupJobs |
| AdhocOrScheduledJob            | Text          | Feld, um anzugeben, ob der Auftrag „Ad-hoc“ oder „Geplant“ ist           |
| BackupItemUniqueId             | Text          | Eindeutige ID, mithilfe derer das Sicherungselement bestimmt wird, das mit der Speicherentität verbunden ist |
| BackupManagementServerUniqueId | Text          | Eindeutige ID, mithilfe derer das Sicherungselement bestimmt wird, das mit der Speicherentität verbunden ist |
| BackupManagementType           | Text          | Anbietertyp für die Sicherung, zu der dieser Auftrag gehört, z.B. IaaSVM, FileFolder |
| DataTransferredInMB            | Number        | Für diesen Auftrag übertragene Daten in MB                          |
| JobDurationInSecs              | Number        | Gesamtdauer des Auftrags in Sekunden                                |
| JobFailureCode                 | Text          | Zeichenfolge mit dem Fehlercode zum Angeben des Grunds des Auftragsfehlers    |
| JobOperation                   | Text          | Vorgang, für den Auftrag ausgeführt wird (z. B. Sicherung, Wiederherstellung oder Sicherungskonfiguration) |
| JobOperationSubType            | Text          | Untertyp des Auftragvorgangs. Beispiel: „Protokoll“ bei Protokollsicherungsauftrag |
| JobStartDateTime               | Datetime      | Datum und Uhrzeit des Starts des Auftrags                       |
| Auftragsstatus                      | Text          | Status des beendeten Auftrags, z.B. „Abgeschlossen“, „Fehler“   |
| JobUniqueId                    | Text          | Eindeutige ID zur Bezeichnung des Auftrags                                |
| ProtectedContainerUniqueId     | Text          | Eindeutiger Bezeichner des geschützten Servers, der dem Auftrag zugeordnet ist |
| RecoveryJobDestination         | Text          | Ziel eines Wiederherstellungsauftrags, an dem die Daten wiederhergestellt werden   |
| RecoveryJobRPDateTime          | Datetime      | Datum und Uhrzeit der Erstellung des Wiederherstellungspunkts, der wiederhergestellt wird |
| RecoveryJobLocation            | Text          | Speicherort des Wiederherstellungspunkts, der wiederhergestellt wird |
| RecoveryLocationType           | Text          | Typ des Wiederherstellungsspeicherorts                                |
| SchemaVersion                  | Text          | Aktuelle Version des Schemas, z. B. **V2**            |
| State                          | Text          | Aktueller Status des Auftragsobjekts, z.B. „Aktiv“, „Gelöscht“ |
| VaultUniqueId                  | Text          | Eindeutiger Bezeichner des geschützten Tresors, der dem Auftrag zugeordnet ist |
| SourceSystem                   | Text          | Quellsystem der aktuellen Daten: Azure                    |

## <a name="addonazurebackuppolicy"></a>AddonAzureBackupPolicy

Diese Tabelle enthält Details zu richtlinienbezogenen Feldern.

| **Feld**                       | **Datentyp**  | **Beschreibung**                                              |
| ------------------------------- | -------------- | ------------------------------------------------------------ |
| resourceId                      | Text           | eindeutiger Bezeichner der Ressource, zu der Daten gesammelt werden. Beispiel: eine Ressourcen-ID des Recovery Services-Tresors |
| Vorgangsname                   | Text           | Name des Vorgangs (z. B. Richtlinie oder PolicyAssociation) |
| Category                        | Text           | Kategorie der Diagnosedaten, die mithilfe von Push an Azure Monitor-Protokolle übermittelt werden (z. B. AddonAzureBackupPolicy) |
| BackupDaysOfTheWeek             | Text           | Tag der Woche, für den Sicherungen geplant sind            |
| BackupFrequency                 | Text           | Häufigkeit der Ausführung von Sicherungen. Beispiel: „täglich“ oder „wöchentlich“ |
| BackupManagementType            | Text           | Anbietertyp für den Server, der den Sicherungsauftrag ausführt. Beispiel: IaaSVM und FileFolder |
| BackupManagementServerUniqueId  | Text           | Feld zur eindeutigen Identifizierung des Servers für die Sicherungsverwaltung, durch den das Sicherungselement geschützt wird (falls zutreffend) |
| BackupTimes                     | Text           | Datum und Uhrzeit geplanter Sicherungen                     |
| DailyRetentionDuration          | Ganze Zahl   | Gesamte Beibehaltungsdauer in Tagen für konfigurierte Sicherungen      |
| DailyRetentionTimes             | Text           | Konfiguration von Datum und Uhrzeit der täglichen Beibehaltung            |
| DiffBackupDaysOfTheWeek         | Text           | Wochentage für differenzielle Sicherungen für SQL im Azure Backup-Dienst für VMs |
| DiffBackupFormat                | Text           | Format für differenzielle Sicherungen für SQL in Sicherungen von Azure-VMs   |
| DiffBackupRetentionDuration     | Decimal Number | Aufbewahrungsdauer für differenzielle Sicherungen für SQL im Azure Backup-Dienst für VMs |
| DiffBackupTime                  | Time           | Zeit für differenzielle Sicherungen für SQL im Azure Backup-Dienst für VMs     |
| LogBackupFrequency              | Decimal Number | Häufigkeit von Protokollsicherungen für SQL                            |
| LogBackupRetentionDuration      | Decimal Number | Aufbewahrungsdauer für Protokollsicherungen für SQL im Azure Backup-Dienst für VMs |
| MonthlyRetentionDaysOfTheMonth  | Text           | Wochen des Monats, in denen die monatliche Beibehaltung konfiguriert ist.  Zum Beispiel „erste“, „letzte“ |
| MonthlyRetentionDaysOfTheWeek   | Text           | Tage der Woche, die für die monatliche Beibehaltung ausgewählt sind              |
| MonthlyRetentionDuration        | Text           | Gesamte Beibehaltungsdauer in Monaten für konfigurierte Sicherungen    |
| MonthlyRetentionFormat          | Text           | Typ der Konfiguration für die monatliche Beibehaltung. Beispiel: „daily“ für tägliche Ausführung, „weekly“ für wöchentliche Ausführung |
| MonthlyRetentionTimes           | Text           | Konfiguration von Datum und Uhrzeit der monatlichen Beibehaltung           |
| MonthlyRetentionWeeksOfTheMonth | Text           | Wochen des Monats, in denen die monatliche Beibehaltung konfiguriert ist.   Zum Beispiel „erste“, „letzte“ |
| PolicyName                      | Text           | Name der definierten Richtlinie                                   |
| PolicyUniqueId                  | Text           | Eindeutige ID zur Bezeichnung der Richtlinie                             |
| PolicyTimeZone                  | Text           | Zeitzone, in der die Richtlinienzeitfelder in den Protokollen angegeben werden |
| RetentionDuration               | Text           | Beibehaltungsdauer für konfigurierte Sicherungen                    |
| RetentionType                   | Text           | Aufbewahrungstyp                                            |
| SchemaVersion                   | Text           | Dieses Feld gibt die aktuelle Version des Schemas an: **V2** |
| State                           | Text           | Aktueller Status des Richtlinienobjekts. Beispiel: Active (Aktiv) oder Deleted (Gelöscht) |
| SynchronisationFrequencyPerDay  | Ganze Zahl   | Anzahl der Synchronisierungen einer Dateisicherung pro Tag für SC DPM und MABS |
| VaultUniqueId                   | Text           | eindeutige ID des Tresors, zu dem diese Richtlinie gehört          |
| WeeklyRetentionDaysOfTheWeek    | Text           | Tage der Woche, die für die wöchentliche Beibehaltung ausgewählt sind               |
| WeeklyRetentionDuration         | Decimal Number | Gesamte wöchentliche Aufbewahrungsdauer in Wochen für konfigurierte Sicherungen |
| WeeklyRetentionTimes            | Text           | Konfiguration von Datum und Uhrzeit der wöchentlichen Beibehaltung            |
| YearlyRetentionDaysOfTheMonth   | Text           | Datumsangaben des Monats, die für die jährliche Beibehaltung ausgewählt sind             |
| YearlyRetentionDaysOfTheWeek    | Text           | Tage der Woche, die für die jährliche Beibehaltung ausgewählt sind               |
| YearlyRetentionDuration         | Decimal Number | Gesamte Beibehaltungsdauer in Jahren für konfigurierte Sicherungen     |
| YearlyRetentionFormat           | Text           | Typ der Konfiguration für die jährliche Aufbewahrung (z. B. „Täglich“ für tagesbasierte und „Wöchentlich“ für wochenbasiert Aufbewahrung) |
| YearlyRetentionMonthsOfTheYear  | Text           | Monate des Jahres, die für die jährliche Beibehaltung ausgewählt sind             |
| YearlyRetentionTimes            | Text           | Konfiguration von Datum und Uhrzeit der jährlichen Beibehaltung            |
| YearlyRetentionWeeksOfTheMonth  | Text           | Wochen des Monats, die für die jährliche Beibehaltung ausgewählt sind             |
| SourceSystem                    | Text           | Quellsystem der aktuellen Daten: Azure                    |

## <a name="addonazurebackupstorage"></a>AddonAzureBackupStorage

Diese Tabelle enthält Details zu speicherbezogenen Feldern.

| **Feld**                      | **Datentyp** | **Beschreibung**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| resourceId                     | Text          | Ressourcenbezeichner der Daten, die erfasst werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors |
| Vorgangsname                  | Text          | Dieses Feld repräsentiert den Namen des aktuellen Vorgangs (z. B. Speicher oder StorageAssociation). |
| Category                       | Text          | Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die mithilfe von Push an Azure Monitor-Protokolle übermittelt werden (z. B. AzureBackupReport). |
| BackupItemUniqueId             | Text          | eindeutige ID zum Identifizieren des Sicherungselements für VMs, die mithilfe von DPM oder MABS gesichert werden |
| BackupManagementServerUniqueId | Text          | Feld zur eindeutigen Identifizierung des Servers für die Sicherungsverwaltung, durch den das Sicherungselement geschützt wird (falls zutreffend) |
| BackupManagementType           | Text          | Anbietertyp für den Server, der den Sicherungsauftrag ausführt. Beispiel: IaaSVM und FileFolder |
| PreferredWorkloadOnVolume      | Text          | Workload, für die dieses Volume der bevorzugte Speicher ist      |
| ProtectedContainerUniqueId     | Text          | Eindeutiger Bezeichner des geschützten Containers, der dem Sicherungselement zugeordnet ist |
| SchemaVersion                  | Text          | Version des Schemas. Beispiel: **V2**                   |
| State                          | Text          | Status des Sicherungselementobjekts. Beispiel: Active (Aktiv) oder Deleted (Gelöscht) |
| StorageAllocatedInMBs          | Number        | Speichergröße, die vom entsprechenden Sicherungselement im entsprechenden Speicher des Typs „Datenträger“ zugewiesen wird |
| StorageConsumedInMBs           | Number        | Speichergröße, die vom entsprechenden Sicherungselement im entsprechenden Speicher benötigt wird |
| StorageName                    | Text          | Name der Speicherentität. Beispiel: E:\                      |
| StorageTotalSizeInGBs          | Text          | Gesamtgröße des von der Speicherentität verwendeten Speichers in GB     |
| StorageType                    | Text          | Typ des Speichers, z. B. Cloud-, Volumen-, Datenträgerspeicher             |
| StorageUniqueId                | Text          | Eindeutige ID, mithilfe derer die Speicherentität bestimmt wird                |
| VaultUniqueId                  | Text          | eindeutige ID zum Identifizieren des Tresors, der mit der Speicherentität verbunden ist |
| VolumeFriendlyName             | Text          | Anzeigename des Speichervolumes                          |
| SourceSystem                   | Text          | Quellsystem der aktuellen Daten: Azure                    |

## <a name="valid-operation-names-for-each-table"></a>Gültige Vorgangsnamen für jede Tabelle

Jedem Datensatz in den obigen Tabellen ist ein **Vorgangsname** zugeordnet. Ein Vorgangsname beschreibt den Typ des Datensatzes (und gibt außerdem an, welche Felder in der Tabelle für diesen Datensatz aufgefüllt werden). Jede Tabelle (Kategorie) unterstützt einen oder mehrere unterschiedliche Vorgangsnamen. Im Folgenden finden Sie eine Zusammenfassung der unterstützten Vorgangsnamen für jede der obigen Tabellen.

| **Tabellenname/Kategorie**                   | **Unterstützte Vorgangsnamen** | **Beschreibung**              |
| ------------------------------------------- | ------------------------------|----------------------------- |
| CoreAzureBackup | BackupItem | Hiermit wird ein Datensatz dargestellt, der alle Details eines bestimmten Sicherungselements enthält (z. B. ID, Name, Typ usw.). |
| CoreAzureBackup | BackupItemAssociation | Hiermit wird eine Zuordnung zwischen einem Sicherungselement und dem zugeordneten geschützten Container dargestellt (falls zutreffend). |
| CoreAzureBackup | BackupItemFrontEndSizeConsumption | Hiermit wird eine Zuordnung zwischen einem Sicherungselement und der Front-End-Größe dargestellt. |
| CoreAzureBackup | ProtectedContainer | Hiermit wird ein Datensatz dargestellt, der alle Details eines bestimmten geschützten Containers enthält (z. B. ID, Name, Typ usw.). |
| CoreAzureBackup | ProtectedContainerAssociation | Hiermit wird eine Zuordnung zwischen einem geschützten Container und dem für die Sicherung verwendeten Tresor dargestellt. |
| CoreAzureBackup | Tresor | Hiermit wird ein Datensatz dargestellt, der alle Details eines bestimmten Tresors enthält (z. B. ID, Name, Tags, Speicherort usw.). |
| CoreAzureBackup | RecoveryPoint | Hiermit wird ein Datensatz dargestellt, der den ältesten und den neuesten Wiederherstellungspunkt für ein bestimmtes Sicherungselement enthält. |
| AddonAzureBackupJobs | Auftrag |  Hiermit wird ein Datensatz dargestellt, der alle Details eines bestimmten Auftrags enthält (z. B. Auftragsvorgang, Startzeit, Status usw.). |
| AddonAzureBackupAlerts | Warnung | Hiermit wird ein Datensatz dargestellt, der alle Details einer bestimmten Warnung enthält (z. B. Erstellungszeit der Warnung, Schweregrad, Status usw.).  |
| AddonAzureBackupStorage | Storage | Hiermit wird ein Datensatz dargestellt, der alle Details einer bestimmten Speicherentität enthält (z. B. Speichername, Typ usw.) |
| AddonAzureBackupStorage | StorageAssociation | Hiermit wird eine Zuordnung zwischen einem Sicherungselement und dem gesamten vom Sicherungselement verbrauchten Cloudspeicher dargestellt. |
| AddonAzureBackupProtectedInstance | ProtectedInstance | Hiermit wird ein Datensatz dargestellt, der die Anzahl der geschützten Instanzen für jeden Container oder jedes Sicherungselement enthält. Bei der Sicherung virtueller Azure-Computer ist die Anzahl geschützter Instanzen auf Sicherungselementebene verfügbar, für andere Workloads auf Ebene des geschützten Containers. |
| AddonAzureBackupPolicy | Policy |  Hiermit wird ein Datensatz dargestellt, der alle Details einer Sicherung und Aufbewahrungsrichtlinie enthält (z. B. ID, Name, Aufbewahrungseinstellungen usw.). |
| AddonAzureBackupPolicy | PolicyAssociation | Hiermit wird eine Zuordnung zwischen einem Sicherungselement und der darauf angewendeten Sicherungsrichtlinie dargestellt. |   

Häufig müssen Sie Joins zwischen verschiedenen Tabellen sowie unterschiedlichen Datensatzgruppen ausführen, die Teil derselben Tabelle sind (unterscheiden sich durch Vorgangsnamen), um alle für die Analyse erforderlichen Felder zu erhalten. Informationen zu den ersten Schritten finden Sie in den [Beispielabfragen](./backup-azure-monitoring-use-azuremonitor.md#sample-kusto-queries). 

## <a name="next-steps"></a>Nächste Schritte

- [Informationen zum Senden von Diagnosedaten an Log Analytics](./backup-azure-diagnostic-events.md)
- [Informationen zum Schreiben von Abfragen für ressourcenspezifische Tabellen](./backup-azure-monitoring-use-azuremonitor.md#sample-kusto-queries)