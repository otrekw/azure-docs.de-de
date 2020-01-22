---
title: Abfragen von Protokollen der Azure-Updateverwaltung
description: In diesem Artikel wird beschrieben, wie Sie die Protokolle für die Updateverwaltung in Ihrem Log Analytics-Arbeitsbereich abfragen.
services: automation
ms.subservice: update-management
ms.date: 01/10/2020
ms.topic: conceptual
ms.openlocfilehash: 5a1979b0e714f35694999c04e1f890b710d54ac9
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75867062"
---
# <a name="query-update-records-for-update-management-in-azure-monitor-logs"></a>Abfragen von Updatedatensätzen für die Updateverwaltung in Azure Monitor-Protokollen

Zusätzlich zu den Details, die in der Updateverwaltungslösung bereitgestellt werden, können Sie die in Ihrem Log Analytics-Arbeitsbereich gespeicherten Protokolle durchsuchen. Wählen Sie auf der Lösungsseite im linken Bereich **Protokolle** aus. Die Seite **Protokollsuche** wird geöffnet.

Weitere Informationen (etwa zum Anpassen der Abfragen oder Verwenden der Abfragen mit anderen Clients) finden Sie hier:  [Dokumentation zur Such-API von Log Analytics](https://dev.loganalytics.io/).

## <a name="update-records"></a>Updatedatensätze

Datensätze, die von der Updateverwaltung für Windows und Linux-VMs gesammelt werden sowie die Datentypen, die in Protokollsuchergebnissen angezeigt werden. In den folgenden Abschnitten werden diese Datensätze beschrieben.

### <a name="required-updates"></a>Erforderliche Updates

Ein Datensatz mit dem Typ `RequiredUpdate` wird erstellt, der die für einen Computer erforderlichen Updates darstellt. Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | Beschreibung | 
|----------|-------------|
| Computer | Vollqualifizierter Domänenname des berichtenden Computers. |
| KBID | ID des Knowledge Base-Artikels für das Windows Update. |
| ManagementGroupName | Name der Operations Management-Verwaltungsgruppe oder des Log Analytics-Arbeitsbereichs. | 
| Produkt | Die Produkte, auf die das Update anwendbar ist. | 
| PublishDate | Das Datum, an dem das Update zum Download und zur Installation von Windows Update bereit steht. |
| Server | | 
| SourceHealthServiceId | Eindeutiger Bezeichner für die Log Analytics Windows-Agent-ID. |
| SourceSystem | *OperationsManager* | 
| TenantId | Eindeutiger Bezeichner, der die Azure Active Directory-Instanz Ihrer Organisation darstellt. | 
| TimeGenerated | Datum und Uhrzeit der Erstellung des Datensatzes | 
| type | *Aktualisieren* | 
| UpdateClassification | Gibt den Typ der Updates an, die angewendet werden können. Windows:<br> *Kritische Updates*<br> *Sicherheitsupdates*<br> *Updaterollups*<br> *Feature Packs*<br> *Service Packs*<br> *Definitionsupdates*<br> *Tools*<br> *Updates*. Linux:<br> *Kritische Updates und Sicherheitsupdates*<br> *Andere* |
| UpdateSeverity | Bewertung des Schweregrads des Sicherheitsrisiko. Werte:<br> *Critical* (Kritisch)<br> *Wichtig*<br> *Mittel*<br> *Niedrig* |
| UpdateTitle | Der Titel des Updates.|

### <a name="update"></a>Aktualisieren

Ein Datensatz mit dem Typ `Update` wird erstellt, der die für einen Computer verfügbaren Updates sowie deren Installationsstatus darstellt. Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | Beschreibung | 
|----------|-------------|
| ApprovalSource | Gilt nur für das Windows-Betriebssystem. Der Wert ist *Microsoft Update*. |
| Genehmigt | *True* oder *False* |
| Klassifizierung | *Updates* |
| Computer | Vollqualifizierter Domänenname des berichtenden Computers. |
| ComputerEnvironment | *Azure* oder *Nicht-Azure*. |
| MSRCBulletinID | ID-Nummer des Sicherheitsbulletins | 
| MSRCSeverity | Bewertung des Schweregrads des Sicherheitsrisiko. Werte:<br> *Critical* (Kritisch)<br> *Wichtig*<br> *Mittel*<br> *Niedrig* |  
| KBID | ID des Knowledge Base-Artikels für das Windows Update. |
| ManagementGroupName | Name der Operations Management-Verwaltungsgruppe oder des Log Analytics-Arbeitsbereichs. |
| UpdateID | Eindeutiger Bezeichner des Softwareupdates. |
| RevisionNumber | Die Revisionsnummer einer bestimmten Revision eines Updates. |
| Optional | *True* oder *False* | 
| RebootBehavior | Das Neustartverhalten nach der Installation/Deinstallation eines Updates. |
| _ResourceId | Eindeutiger Bezeichner für die Ressource, der der Datensatz zugeordnet ist. |
| type | *Aktualisieren* |
| VMUUID | Eindeutiger Bezeichner für den virtuellen Computer. |
| MG | Eindeutiger Bezeichner für die Verwaltungsgruppe oder den Log Analytics-Arbeitsbereich. | 
| TenantId | Eindeutiger Bezeichner, der die Azure Active Directory-Instanz Ihrer Organisation darstellt. | 
| SourceSystem | *OperationsManager* | 
| TimeGenerated | Datum und Uhrzeit der Erstellung des Datensatzes | 
| SourceComputerId | Eindeutiger Bezeichner, der den Quellcomputer darstellt. | 
| Titel | Der Titel des Updates. |
| PublishedDate (UTC) | Das Datum, an dem das Update zum Download und zur Installation von Windows Update bereit steht.  |
| UpdateState | Der aktuelle Zustand des Updates. | 
| Produkt | Die Produkte, auf die das Update anwendbar ist. |
| SubscriptionId | Der eindeutige Bezeichner für das Azure-Abonnement. | 
| ResourceGroup | Name der Ressourcengruppe, deren Mitglied die Ressource ist. | 
| ResourceProvider | Gibt den Ressourcenanbieter an. | 
| Resource | Der Name der Ressource. | 
| ResourceType | Name des Ressourcentyps | 

### <a name="update-agent"></a>Agentupdate ausführen

Ein Datensatz mit dem Typ `UpdateAgent` wird erstellt, der Details zum Update-Agent auf dem Computer bereitstellt. Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | Beschreibung | 
|----------|-------------|
| AgeofOldestMissingRequiredUpdate | | 
| AutomaticUpdateEnabled | | 
| Computer | Vollqualifizierter Domänenname des berichtenden Computers. |
| DaySinceLastUpdateBucket | | 
| ManagementGroupName | Name der Operations Management-Verwaltungsgruppe oder des Log Analytics-Arbeitsbereichs. |
| OSVersion | Die Version des Betriebssystems. |
| Server | |
| SourceHealthServiceId | Eindeutiger Bezeichner für die Log Analytics Windows-Agent-ID. |
| SourceSystem | *OperationsManager* | 
| TenantId | Eindeutiger Bezeichner, der die Azure Active Directory-Instanz Ihrer Organisation darstellt. |
| TimeGenerated | Datum und Uhrzeit der Erstellung des Datensatzes |
| type | *Aktualisieren* | 
| WindowsUpdateAgentVersion | Version des Windows Update-Agents. |
| WSUSServer | Zeigt Fehler an, wenn der Windows Update-Agent ein Problem hat, das behandelt werden muss. |

### <a name="update-deployment-status"></a>Bereitstellungsstatus des Updates 

Ein Datensatz mit dem Typ `UpdateRunProgress` wird erstellt, der den Bereitstellungsstatus des Updates einer geplanten Bereitstellung nach Computer bereitstellt. Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | Beschreibung | 
|----------|-------------|
| Computer | Vollqualifizierter Domänenname des berichtenden Computers. |
| ComputerEnvironment | *Azure* oder *Nicht-Azure*. | 
| CorrelationId | Eindeutiger Bezeichner für die Runbookauftragsausführung für das Update. |
| EndTime | Der Zeitpunkt, an dem der Synchronisierungsprozess beendet wurde. | 
| ErrorResult | Generierter Windows Update-Fehlercode, wenn die Installation eines Updates fehlschlägt. | 
| InstallationStatus | Die möglichen Installationszustände eines Updates auf dem Clientcomputer, *In Arbeit*, *Erfolgreich*, *Teilweise fehlgeschlagen*. |
| KBID | ID des Knowledge Base-Artikels für das Windows Update. | 
| ManagementGroupName | Name der Operations Management-Verwaltungsgruppe oder des Log Analytics-Arbeitsbereichs. |
| OSType | Gibt den Typ des Betriebssystems an, entweder *Windows* oder *Linux*. | 
| Produkt | Die Produkte, auf die das Update anwendbar ist. |
| Resource | Der Name der Ressource. | 
| resourceId | Eindeutiger Bezeichner für die Ressource, der der Datensatz zugeordnet ist. |
| ResourceProvider | Gibt den Ressourcenanbieter an. | 
| ResourceType | Name des Ressourcentyps | 
| SourceComputerId | Eindeutiger Bezeichner, der den Quellcomputer darstellt. | 
| SourceSystem | *OperationsManager* |
| StartTime | Die Uhrzeit, für die die Installation des Updates geplant ist. |
| SubscriptionId | Der eindeutige Bezeichner für das Azure-Abonnement. | 
| SucceededOnRetry | Zeigt an, wann die Updateausführung beim ersten Versuch fehlgeschlagen ist und dass der aktuelle Vorgang ein Wiederholungsversuch ist. |
| TimeGenerated | Datum und Uhrzeit der Erstellung des Datensatzes |
| Titel | Der Titel des Updates. |
| type | *UpdateRunProgress* |
| UpdateId | Eindeutiger Bezeichner des Softwareupdates. |
| VMUUID | Eindeutiger Bezeichner für den virtuellen Computer. |
| _ResourceId | Eindeutiger Bezeichner für die Ressource, der der Datensatz zugeordnet ist. |

### <a name="update-summary"></a>Aktualisieren der Zusammenfassung 

Ein Datensatz mit dem Typ `UpdateSummary` wird erstellt, der eine Updatezusammenfassung nach Computer bereitstellt. Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | Beschreibung | 
|----------|-------------|
| Computer | Vollqualifizierter Domänenname des berichtenden Computers. |
| ComputerEnvironment | *Azure* oder *Nicht-Azure*. | 
| CriticalUpdatesMissing | Anzahl der fehlenden wichtigen Updates, die anwendbar sind. | 
| ManagementGroupName | Name der Operations Management-Verwaltungsgruppe oder des Log Analytics-Arbeitsbereichs. |
| NETRuntimeVersion | Version des auf dem Windows-Computer installierten .NET Frameworks. |
| OldestMissingSecurityUpdateBucket | | 
| OldestMissingSecurityUpdateInDays | |
| OsVersion | Die Version des Betriebssystems. |
| OtherUpdatesMissing | Anzahl der erkannten fehlenden Updates. |
| Resource |  Der Name der Ressource. | 
| ResourceGroup | Name der Ressourcengruppe, deren Mitglied die Ressource ist. |
| resourceId | Eindeutiger Bezeichner für die Ressource, der der Datensatz zugeordnet ist. |
| ResourceProvider | Gibt den Ressourcenanbieter an. |
| ResourceType | Name des Ressourcentyps |
| RestartPending | *True* und *False*. |
| SecurityUpdatesMissing | Anzahl der anwendbaren, fehlenden Sicherheitsupdates.| 
| SourceComputerId | Eindeutiger Bezeichner für den virtuellen Computer. |
| SourceSystem | *OpsManager* | 
| SubscriptionId | Der eindeutige Bezeichner für das Azure-Abonnement. |
| TimeGenerated | Datum und Uhrzeit der Erstellung des Datensatzes |
| TotalUpdatesMissing | Gesamtzahl der anwendbaren fehlenden Updates. | 
| type | *UpdateSummary* |
| VMUUID | Eindeutiger Bezeichner für den virtuellen Computer. |
| WindowsUpdateAgentVersion | Version des Windows Update-Agents. |
| WindowsUpdateSetting | Zeigt den Status des Windows Update-Agent an. Mögliche Werte:<br> *Geplante Installation*<br> *Vor der Installation benachrichtigen*<br> Fehler von fehlerhaftem WUA-Agent zurückgegeben. | 
| WSUSServer | Zeigt Fehler an, wenn der Windows Update-Agent ein Problem hat, das behandelt werden muss. |
| _ResourceId | Eindeutiger Bezeichner für die Ressource, der der Datensatz zugeordnet ist. |

## <a name="sample-queries"></a>Beispielabfragen

Die folgenden Abschnitte enthalten Beispielprotokollabfragen für Updatedatensätze, die für die Updateverwaltung erfasst werden.

### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Bestätigen der Integration von Nicht-Azure-Computern

Sie können eine der folgenden Protokollsuchen ausführen, um zu bestätigen, dass direkt verbundene Computer mit Azure Monitor-Protokollen kommunizieren.

#### <a name="linux"></a>Linux

```loganalytics
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```loganalytics
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

Auf einem Windows-Computer können Sie die folgenden Informationen überprüfen, um für den Agent die Konnektivität mit Azure Monitor-Protokollen zu bestätigen:

1. Öffnen Sie in der Systemsteuerung **Microsoft Monitoring Agent**. Auf der Registerkarte **Azure Log Analytics** zeigt der Agent die folgende Meldung an: **The Microsoft Monitoring Agent has successfully connected to Log Analytics.** (Der Microsoft Monitoring Agent hat erfolgreich eine Verbindung mit Log Analytics hergestellt.)
2. Öffnen Sie das Windows-Ereignisprotokoll. Navigieren Sie zu **Anwendungs- und Dienstprotokolle\Operations Manager**, und suchen Sie nach der Ereignis-ID 3000 und der Ereignis-ID 5002 aus der Quelle **Service Connector**. Mit diesen Ereignissen wird angegeben, dass für den Computer die Registrierung beim Log Analytics-Arbeitsbereich und die Konfiguration ausgeführt wurden.

Falls der Agent nicht mit Azure Monitor-Protokollen kommunizieren kann und für die Kommunikation mit dem Internet über eine Firewall oder einen Proxyserver konfiguriert ist, vergewissern Sie sich, dass die Firewall bzw. der Proxyserver ordnungsgemäß konfiguriert sind. Weitere Informationen zur Überprüfung der Firewall- oder Proxyserverkonfiguration finden Sie unter [Netzwerkkonfiguration für den Windows-Agent](../azure-monitor/platform/agent-windows.md) bzw. unter [Netzwerkkonfiguration für den Linux-Agent](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Wenn Ihre Linux-Systeme für die Kommunikation mit einem Proxy oder Log Analytics-Gateway konfiguriert sind und Sie diese Lösung integrieren, aktualisieren Sie die Berechtigungen für *proxy.conf*, um der Gruppe „omiuser“ Leseberechtigungen für die Datei zu erteilen. Führen Sie zu diesem Zweck die folgenden Befehle aus:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Für neu hinzugefügte Linux-Agents wird der Status **Aktualisiert** angezeigt, nachdem eine Bewertung ausgeführt wurde. Dieser Vorgang kann bis zu sechs Stunden dauern.

Wenn Sie bestätigen möchten, dass eine Operations Manager-Verwaltungsgruppe mit Azure Monitor-Protokollen kommuniziert, helfen Ihnen die Informationen unter [Überprüfen der Integration von Operations Manager mit Log Analytics](../azure-monitor/platform/om-agents.md#validate-operations-manager-integration-with-azure-monitor) weiter.

### <a name="single-azure-vm-assessment-queries-windows"></a>Bewertungsabfragen für einzelne virtuelle Azure-Computer (Windows)

Ersetzen Sie den VMUUID-Wert durch die VM-GUID des virtuellen Computers, den Sie abfragen. Sie können die VMUUID ermitteln, die verwendet werden soll, indem Sie die folgende Abfrage in Azure Monitor-Protokollen ausführen: `Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

#### <a name="missing-updates-summary"></a>Zusammenfassung fehlender Updates

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"b08d5afa-1471-4b52-bd95-a44fea6e4ca8"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

#### <a name="missing-updates-list"></a>Liste fehlender Updates

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"8bf1ccc6-b6d3-4a0b-a643-23f346dfdf82"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

### <a name="single-azure-vm-assessment-queries-linux"></a>Bewertungsabfragen für einzelne virtuelle Azure-Computer (Linux)

Für einige Linux-Distributionen besteht ein Konflikt der [Bytereihenfolge](https://en.wikipedia.org/wiki/Endianness) mit dem VMUUID-Wert, der auf Azure Resource Manager und die in Azure Mit-Protokollen gespeicherten Daten zurückzuführen ist. Die folgende Abfrage überprüft, ob eine Übereinstimmung für einen der Endianness-Werte vorliegt. Ersetzen Sie die VMUUID-Werte durch das big-endian- bzw. little-endian-Format der GUID, um die Ergebnisse ordnungsgemäß zurückzugeben. Sie können die VMUUID ermitteln, die verwendet werden soll, indem Sie die folgende Abfrage in Azure Monitor-Protokollen ausführen: `Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

#### <a name="missing-updates-summary"></a>Zusammenfassung fehlender Updates

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

#### <a name="missing-updates-list"></a>Liste fehlender Updates

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl

```

### <a name="multi-vm-assessment-queries"></a>Bewertungsabfragen für mehrere virtuelle Computer

#### <a name="computers-summary"></a>Computerzusammenfassung

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Approved, Optional, Classification) by SourceComputerId, UpdateID
    | distinct SourceComputerId, Classification, UpdateState, Approved, Optional
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed" and (Optional==false or Classification has "Critical" or Classification has "Security") and Approved!=false, iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity
| union (Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by SourceComputerId, Product, ProductArch
    | distinct SourceComputerId, Classification, UpdateState
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed", iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity)
| summarize assessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity>-1), notAssessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==-1), computersNeedCriticalUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==4), computersNeedSecurityUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==2), computersNeedOtherUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==1), upToDateComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==0)
| summarize assessedComputersCount=sum(assessedComputersCount), computersNeedCriticalUpdatesCount=sum(computersNeedCriticalUpdatesCount),  computersNeedSecurityUpdatesCount=sum(computersNeedSecurityUpdatesCount), computersNeedOtherUpdatesCount=sum(computersNeedOtherUpdatesCount), upToDateComputersCount=sum(upToDateComputersCount), notAssessedComputersCount=sum(notAssessedComputersCount)
| extend allComputersCount=assessedComputersCount+notAssessedComputersCount


```

#### <a name="missing-updates-summary"></a>Zusammenfassung fehlender Updates

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| union (Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification )
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

#### <a name="computers-list"></a>Computerliste

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Product, Computer, ComputerEnvironment) by SourceComputerId, Product, ProductArch
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed"), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed"), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed"), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2)
| union(Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, Optional, Approved, Computer, ComputerEnvironment) by Computer, SourceComputerId, UpdateID
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed" and Approved!=false), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed" and Approved!=false), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed" and Optional==false and Approved!=false), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2) )
| order by ComplianceOrder asc, missingCriticalUpdatesCount desc, missingSecurityUpdatesCount desc, missingOtherUpdatesCount desc, displayName asc
| project-away ComplianceOrder
```

#### <a name="missing-updates-list"></a>Liste fehlender Updates

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| union(Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2)
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

## <a name="next-steps"></a>Nächste Schritte

* Verwenden Sie Protokollsuchen in [Azure Monitor-Protokollen](../log-analytics/log-analytics-log-searches.md), um ausführliche Aktualisierungsdaten anzuzeigen.
* [Erstellen Sie Warnungen](automation-tutorial-update-management.md#configure-alerts) für den Status der Updatebereitstellung.
