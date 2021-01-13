---
title: 'Azure Monitor-Ressourcenprotokolle: unterstützte Dienste und Kategorien'
description: 'Referenz zu Azure Monitor: Erläuterung der unterstützten Dienste und Ereignisschemas für Azure-Ressourcenprotokolle.'
ms.subservice: logs
ms.topic: reference
ms.date: 12/09/2020
ms.openlocfilehash: c7b2d48b40843930bba78f54d2294769d952daf6
ms.sourcegitcommit: 1756a8a1485c290c46cc40bc869702b8c8454016
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/09/2020
ms.locfileid: "96931227"
---
# <a name="supported-categories-for-azure-resource-logs"></a>Unterstützte Kategorien für Azure-Ressourcenprotokolle

> [!NOTE]
> Ressourcenprotokolle wurden zuvor als Diagnoseprotokolle bezeichnet. Der Name wurde im Oktober 2019 geändert, da die Typen der von Azure Monitor gesammelten Protokolle nicht mehr nur die Azure-Ressource umfassen.

[Azure Monitor-Ressourcenprotokolle](./platform-logs-overview.md) sind von Azure-Diensten ausgegebene Protokolle, die die Vorgänge der jeweiligen Dienste oder Ressourcen beschreiben. Für alle Ressourcenprotokolle, die über Azure Monitor verfügbar sind, wird ein Schema der obersten Ebene gemeinsam genutzt. Auf diese Weise kann jeder Dienst für seine eigenen Ereignisse flexibel eindeutige Eigenschaften ausgeben.

Ein Schema wird mit einer Kombination aus dem Ressourcentyp (in der `resourceId`-Eigenschaft verfügbar) und dem `category`-Element eindeutig identifiziert. Es gibt ein allgemeines Schema für alle Ressourcenprotokolle mit dienstspezifischen Feldern, die dann für verschiedene Protokollkategorien hinzugefügt werden. Weitere Informationen finden Sie unter [Allgemeines und dienstspezifisches Schema für Azure-Ressourcenprotokolle]().


## <a name="costs"></a>Kosten

Das Senden und Speichern von Daten an und in Log Analytics, Azure Storage und/oder Event Hub ist mit Kosten verbunden. Sie können die Kosten bezahlen, um die Daten an diese Speicherorte zu senden und dort zu speichern.  Ressourcenprotokolle sind ein Datentyp, den Sie an diese Speicherorte senden können. Es entstehen zusätzliche [Kosten, um einige Kategorien von Ressourcenprotokollen](https://azure.microsoft.com/pricing/details/monitor/) nach diesen Speicherorten zu exportieren, während bei anderen keine Exportkosten entstehen. Spezifische Exportkosten sind in der folgenden Tabelle aufgeführt.

## <a name="supported-log-categories-per-resource-type"></a>Unterstützte Protokollkategorien pro Ressourcentyp

Es folgt eine Liste der Arten von Protokollen, die für jeden Ressourcentyp verfügbar sind. 

Einige Kategorien werden möglicherweise nur für bestimmte Ressourcentypen unterstützt. Wenn Sie der Meinung sind, dass eine Ressource fehlt, lesen Sie die ressourcenspezifische Dokumentation. Beispielsweise sind Kategorien des Typs „Microsoft.Sql/servers/databases“ nicht für alle Datenbanktypen verfügbar. Weitere Informationen finden Sie unter den [Informationen zur SQL-Datenbank-Diagnoseprotokollierung](../../azure-sql/database/metrics-diagnostic-telemetry-logging-streaming-export-configure.md). 

Wenn Sie noch etwas vermissen, können Sie unten in diesem Artikel einen GitHub-Kommentar öffnen.
## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|Engine|Engine|
|Dienst|Dienst|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|GatewayLogs|Protokolle im Zusammenhang mit dem ApiManagement-Gateway|


## <a name="microsoftappplatformspring"></a>Microsoft.AppPlatform/Spring

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|ApplicationConsole|Anwendungskonsole|
|SystemLogs|Systemprotokolle|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|DscNodeStatus|DSC-Knotenstatus|
|JobLogs|Auftragsprotokolle|
|JobStreams|Auftragsdatenströme|


## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|ServiceLog|Dienstprotokolle|


## <a name="microsoftbatchaiworkspaces"></a>Microsoft.BatchAI/workspaces

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|BaiClusterEvent|BaiClusterEvent|
|BaiClusterNodeEvent|BaiClusterNodeEvent|
|BaiJobEvent|BaiJobEvent|


## <a name="microsoftblockchainblockchainmembers"></a>Microsoft.Blockchain/blockchainMembers

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|BlockchainApplication|Blockchainanwendung|
|FabricOrderer|Fabric-Orderer|
|FabricPeer|Fabric-Peer|
|Proxy|Proxy|


## <a name="microsoftblockchaincordamembers"></a>Microsoft.Blockchain/cordaMembers

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|BlockchainApplication|Blockchainanwendung|


## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Microsoft.Cdn/cdnwebapplicationfirewallpolicies

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|WebApplicationFirewallLogs|Web Application Firewall-Protokolle|


## <a name="microsoftcdnprofiles"></a>Microsoft.Cdn/profiles

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AzureCdnAccessLog|Azure CDN-Zugriffsprotokoll|


## <a name="microsoftcdnprofilesendpoints"></a>Microsoft.Cdn/profiles/endpoints

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|CoreAnalytics|Ruft die Metriken des Endpunkts ab, z.B. Bandbreite, ausgehenden Datenverkehr usw.|


## <a name="microsoftclassicnetworknetworksecuritygroups"></a>Microsoft.ClassicNetwork/networksecuritygroups

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|Regelflussereignis der Netzwerksicherheitsgruppe|Regelflussereignis der Netzwerksicherheitsgruppe|


## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|Audit|Überwachungsprotokolle|
|RequestResponse|Anforderungs- und Antwortprotokolle|
|Trace|Ablaufverfolgungsprotokolle|


## <a name="microsoftcontainerregistryregistries"></a>Microsoft.ContainerRegistry/registries

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|ContainerRegistryLoginEvents|Anmeldeereignisse|
|ContainerRegistryRepositoryEvents|RepositoryEvent-Protokolle|


## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|cluster-autoscaler|Automatische Kubernetes-Clusterskalierung|
|kube-apiserver|Kubernetes-API-Server|
|kube-audit|Kubernetes-Überwachung|
|kube-controller-manager|Kubernetes-Controller-Manager|
|kube-scheduler|Kubernetes-Scheduler|


## <a name="microsoftcustomprovidersresourceproviders"></a>Microsoft.CustomProviders/resourceproviders

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AuditLogs|Überwachungsprotokolle für MiniRP-Aufrufe|


## <a name="microsoftdatabricksworkspaces"></a>Microsoft.Databricks/workspaces

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|accounts|Databricks-Konten|
|clusters|Databricks-Cluster|
|dbfs|Databricks-Dateisystem|
|instancePools|Instanzenpools|
|jobs|Databricks-Aufträge|
|Notebook|Databricks-Notebook|
|secrets|Databricks-Geheimnisse|
|sqlPermissions|Databricks-SQL-Berechtigungen|
|ssh|Databricks-SSH|
|Arbeitsbereich|Databricks-Arbeitsbereich|


## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|ActivityRuns|Pipeline-Aktivitätsausführungsprotokoll|
|PipelineRuns|Pipelineausführungsprotokoll|
|TriggerRuns|Triggerausführungsprotokoll|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|Audit|Überwachungsprotokolle|
|Requests|Anforderungsprotokolle|


## <a name="microsoftdatashareaccounts"></a>Microsoft.DataShare/accounts

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|ReceivedShareSnapshots|Empfangene Freigabemomentaufnahmen|
|SentShareSnapshots|Gesendete Freigabemomentaufnahmen|
|Freigaben|Freigaben|
|ShareSubscriptions|Freigabeabonnements|


## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|MySqlAuditLogs|MariaDB-Überwachungsprotokolle|
|MySqlSlowLogs|MariaDB-Serverprotokolle|


## <a name="microsoftdbformysqlflexibleservers"></a>Microsoft.DBforMySQL/flexibleServers

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|MySqlAuditLogs|MySQL-Überwachungsprotokolle|
|MySqlSlowLogs|Protokolle zu langsamen MySQL-Abfragen|


## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|MySqlAuditLogs|MySQL-Überwachungsprotokolle|
|MySqlSlowLogs|MySQL Server-Protokolle|


## <a name="microsoftdbforpostgresqlflexibleservers"></a>Microsoft.DBforPostgreSQL/flexibleServers

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|PostgreSQLLogs|PostgreSQL-Serverprotokolle|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|PostgreSQLLogs|PostgreSQL-Serverprotokolle|
|QueryStoreRuntimeStatistics|Laufzeitstatistik des PostgreSQL-Abfragespeichers|
|QueryStoreWaitStatistics|Wartestatistik des PostgreSQL-Abfragespeichers|


## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|PostgreSQLLogs|PostgreSQL-Serverprotokolle|


## <a name="microsoftdesktopvirtualizationapplicationgroups"></a>Microsoft.DesktopVirtualization/applicationgroups

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|Prüfpunkt|Prüfpunkt|
|Fehler|Fehler|
|Verwaltung|Verwaltung|


## <a name="microsoftdesktopvirtualizationhostpools"></a>Microsoft.DesktopVirtualization/hostpools

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|Prüfpunkt|Prüfpunkt|
|Verbindung|Verbindung|
|Fehler|Fehler|
|HostRegistration|HostRegistration|
|Verwaltung|Verwaltung|


## <a name="microsoftdesktopvirtualizationworkspaces"></a>Microsoft.DesktopVirtualization/workspaces

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|Prüfpunkt|Prüfpunkt|
|Fehler|Fehler|
|Feed|Feed|
|Verwaltung|Verwaltung|


## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|C2DCommands|C2D-Befehle|
|C2DTwinOperations|C2D-Zwillingsvorgänge|
|Configurations|Configurations|
|Verbindungen|Verbindungen|
|D2CTwinOperations|D2CTwinOperations|
|DeviceIdentityOperations|Geräteidentitätsvorgänge|
|DeviceStreams|Gerätestreams (Vorschau)|
|DeviceTelemetry|Gerätetelemetrie|
|DirectMethods|Direkte Methoden|
|DistributedTracing|Verteilte Ablaufverfolgung (Vorschauversion)|
|FileUploadOperations|Dateiuploadvorgänge|
|JobsOperations|Auftragsvorgänge|
|Routen|Routen|
|TwinQueries|Zwillingsabfragen|


## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|DeviceOperations|Gerätevorgänge|
|ServiceOperations|Dienstoperationen|


## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|CassandraRequests|CassandraRequests|
|ControlPlaneRequests|ControlPlaneRequests|
|DataPlaneRequests|DataPlaneRequests|
|GremlinRequests|GremlinRequests|
|MongoRequests|MongoRequests|
|PartitionKeyRUConsumption|PartitionKeyRUConsumption|
|PartitionKeyStatistics|PartitionKeyStatistics|
|QueryRuntimeStatistics|QueryRuntimeStatistics|


## <a name="microsofteventgriddomains"></a>Microsoft.EventGrid/domains

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|DeliveryFailures|Protokolle für Zustellungsfehler|
|PublishFailures|Protokolle für Veröffentlichungsfehler|


## <a name="microsofteventgridsystemtopics"></a>Microsoft.EventGrid/systemTopics

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|DeliveryFailures|Protokolle für Zustellungsfehler|


## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|DeliveryFailures|Protokolle für Zustellungsfehler|
|PublishFailures|Protokolle für Veröffentlichungsfehler|


## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|ArchiveLogs|Archivprotokolle|
|AutoScaleLogs|Protokolle zur automatischen Skalierung|
|CustomerManagedKeyUserLogs|Protokolle für kundenseitig verwalteten Schlüssel|
|EventHubVNetConnectionEvent|VNet/IP-Filterung-Verbindungsprotokolle|
|KafkaCoordinatorLogs|Kafka-Koordinatorprotokolle|
|KafkaUserErrorLogs|Kafka-Benutzerfehlerprotokolle|
|OperationalLogs|Betriebsprotokolle|


## <a name="microsofthealthcareapisservices"></a>Microsoft.HealthcareApis/services

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AuditLogs|Überwachungsprotokolle|


## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AutoscaleEvaluations|Bewertungen der Autoskalierung|
|AutoscaleScaleActions|Skalierungsaktionen der Autoskalierung|


## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AppAvailabilityResults|Verfügbarkeitsergebnisse|
|AppBrowserTimings|Browserzeiten|
|AppDependencies|Abhängigkeiten|
|AppEvents|Events|
|AppExceptions|Ausnahmen|
|AppMetrics|Metriken|
|AppPageViews|Seitenaufrufe|
|AppPerformanceCounters|Leistungsindikatoren|
|AppRequests|Requests|
|AppSystemEvents|Systemereignisse|
|AppTraces|Traces|


## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AuditEvent|Überwachungsprotokolle|


## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|Befehl|Befehl|
|FailedIngestion|Fehlgeschlagene Erfassungsvorgänge|
|IngestionBatching|Batchverarbeitung der Datenerfassung|
|Abfrage|Abfrage|
|SucceededIngestion|Erfolgreiche Erfassungsvorgänge|
|TableDetails|Tabellendetails|
|TableUsageStatistics|Tabellenverwendungsstatistik|


## <a name="microsoftlogicintegrationaccounts"></a>Microsoft.Logic/integrationAccounts

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|IntegrationAccountTrackingEvents|Integrationskonto –Nachverfolgen von Ereignissen|


## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|WorkflowRuntime|Diagnoseereignisse zur Workflowlaufzeit|


## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/workspaces

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AmlComputeClusterEvent|AmlComputeClusterEvent|
|AmlComputeClusterNodeEvent|AmlComputeClusterNodeEvent|
|AmlComputeCpuGpuUtilization|AmlComputeCpuGpuUtilization|
|AmlComputeJobEvent|AmlComputeJobEvent|
|AmlRunStatusChangedEvent|AmlRunStatusChangedEvent|


## <a name="microsoftmediamediaservices"></a>Microsoft.Media/mediaservices

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|KeyDeliveryRequests|Schlüsselübermittlungsanforderungen|


## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|ApplicationGatewayAccessLog|Application Gateway-Zugriffsprotokoll|
|ApplicationGatewayFirewallLog|Application Gateway-Firewallprotokoll|
|ApplicationGatewayPerformanceLog|Application Gateway-Leistungsprotokoll|


## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AzureFirewallApplicationRule|Azure Firewall-Anwendungsregel|
|AzureFirewallNetworkRule|Azure Firewall-Netzwerkregel|


## <a name="microsoftnetworkbastionhosts"></a>Microsoft.Network/bastionHosts

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|BastionAuditLogs|Bastion-Überwachungsprotokolle|


## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|PeeringRouteLog|Routentabellenprotokolle zum Peering|


## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|FrontdoorAccessLog|Frontdoor-Zugriffsprotokoll|
|FrontdoorWebApplicationFirewallLog|Frontdoor-Web Application Firewall-Protokoll|


## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|LoadBalancerAlertEvent|Load Balancer-Warnereignisse|
|LoadBalancerProbeHealthStatus|Integritätsstatus der Load Balancer-Stichprobe|


## <a name="microsoftnetworknetworksecuritygroups"></a>Microsoft.Network/networksecuritygroups

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|NetworkSecurityGroupEvent|Ereignis der Netzwerksicherheitsgruppe|
|NetworkSecurityGroupFlowEvent|Regelflussereignis der Netzwerksicherheitsgruppe|
|NetworkSecurityGroupRuleCounter|Regelzähler der Netzwerksicherheitsgruppe|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|DDoSMitigationFlowLogs|Datenflussprotokolle von Entscheidungen zur DDoS-Risikominderung|
|DDoSMitigationReports|Berichte zur DDoS-Risikominderung|
|DDoSProtectionNotifications|DDoS-Schutz-Benachrichtigungen|


## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|ProbeHealthStatusEvents|Traffic Manager-Testintegritätsergebnisse (Ereignis)|


## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|GatewayDiagnosticLog|Gatewaydiagnoseprotokolle|
|IKEDiagnosticLog|IKE-Diagnoseprotokolle|
|P2SDiagnosticLog|P2S-Diagnoseprotokolle|
|RouteDiagnosticLog|Routendiagnoseprotokolle|
|TunnelDiagnosticLog|Tunneldiagnoseprotokolle|


## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft.Network/virtualNetworks

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|VMProtectionAlerts|VM-Schutz-Warnungen|


## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|Engine|Engine|


## <a name="microsoftrecoveryservicesvaults"></a>Microsoft.RecoveryServices/Vaults

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AddonAzureBackupAlerts|Add-On: Azure Backup-Warnungsdaten|
|AddonAzureBackupJobs|Add-On: Azure Backup-Auftragsdaten|
|AddonAzureBackupPolicy|Add-On: Azure Backup-Richtliniendaten|
|AddonAzureBackupProtectedInstance|Add-On: Azure Backup-Daten für geschützte Instanz|
|AddonAzureBackupStorage|Add-On: Azure Backup-Speicherdaten|
|AzureBackupReport|Azure Backup-Berichtsdaten|
|AzureSiteRecoveryEvents|Azure Site Recovery-Ereignisse|
|AzureSiteRecoveryJobs|Azure Site Recovery-Aufträge|
|AzureSiteRecoveryProtectedDiskDataChurn|Datenänderungen auf mit Azure Site Recovery geschützten Datenträgern|
|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery-Wiederherstellungspunkte|
|AzureSiteRecoveryReplicatedItems|Replizierte Azure Site Recovery-Elemente|
|AzureSiteRecoveryReplicationDataUploadRate|Uploadrate für Azure Site Recovery-Replikationsdaten|
|AzureSiteRecoveryReplicationStats|Azure Site Recovery-Replikationsstatistiken|
|CoreAzureBackup|Kern: Azure Backup-Daten|


## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|HybridConnectionsEvent|HybridConnections Events|


## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|OperationLogs|Vorgangsprotokolle|


## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|OperationalLogs|Betriebsprotokolle|


## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AllLogs|Protokolle für Azure SignalR Service|


## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|DevOpsOperationsAudit|Überwachungsprotokolle für DevOps-Vorgänge|
|ResourceUsageStats|Nutzungsstatistiken zu Ressourcen|
|SQLSecurityAuditEvents|SQL-Sicherheitsüberwachungsereignis|


## <a name="microsoftsqlmanagedinstancesdatabases"></a>Microsoft.Sql/managedInstances/databases

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|Errors|Errors|
|QueryStoreRuntimeStatistics|Laufzeitstatistik des Abfragespeichers|
|QueryStoreWaitStatistics|Wartestatistik des Abfragespeichers|
|SQLInsights|SQL-Informationen|


## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AutomaticTuning|Automatische Optimierung|
|Blöcke|Blöcke|
|DatabaseWaitStatistics|Wartestatistik der Datenbank|
|Deadlocks|Deadlocks|
|DevOpsOperationsAudit|Überwachungsprotokolle für DevOps-Vorgänge|
|DmsWorkers|DMS-Worker|
|Errors|Errors|
|ExecRequests|Ausführungsanforderungen|
|QueryStoreRuntimeStatistics|Laufzeitstatistik des Abfragespeichers|
|QueryStoreWaitStatistics|Wartestatistik des Abfragespeichers|
|RequestSteps|Anforderungsschritte|
|SQLInsights|SQL-Informationen|
|SqlRequests|SQL-Anforderungen|
|SQLSecurityAuditEvents|SQL-Sicherheitsüberwachungsereignis|
|Zeitlimits|Zeitlimits|
|Wartevorgänge|Wartevorgänge|


## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

Kosten: Dies wird wie im Abschnitt „Plattformprotokolle“ der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/) beschrieben bezahlt. 

|Category |Anzeigename der Kategorie|
|---|---|
|StorageDelete|StorageDelete|
|StorageRead|StorageRead|
|StorageWrite|StorageWrite|


## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

Kosten: Dies wird wie im Abschnitt „Plattformprotokolle“ der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/) beschrieben bezahlt. 

|Category |Anzeigename der Kategorie|
|---|---|
|StorageDelete|StorageDelete|
|StorageRead|StorageRead|
|StorageWrite|StorageWrite|


## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

Kosten: Dies wird wie im Abschnitt „Plattformprotokolle“ der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/) beschrieben bezahlt. 
 
|Category |Anzeigename der Kategorie|
|---|---|
|StorageDelete|StorageDelete|
|StorageRead|StorageRead|
|StorageWrite|StorageWrite|


## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

Kosten: Dies wird wie im Abschnitt „Plattformprotokolle“ der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/) beschrieben bezahlt. 
 
|Category |Anzeigename der Kategorie|
|---|---|
|StorageDelete|StorageDelete|
|StorageRead|StorageRead|
|StorageWrite|StorageWrite|


## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|Erstellen|Erstellen|
|Ausführung|Ausführung|


## <a name="microsoftsynapseworkspaces"></a>Microsoft.Synapse/workspaces

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|BuiltinSqlReqsEnded|Integrierter SQL-Pool, beendete Anforderungen|
|GatewayApiRequests|Synapse-Gateway-API-Anforderungen|
|SQLSecurityAuditEvents|SQL-Sicherheitsüberwachungsereignis|
|SynapseRbacOperations|Synapse-RBAC-Vorgänge|


## <a name="microsoftsynapseworkspacesbigdatapools"></a>Microsoft.Synapse/workspaces/bigDataPools

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|BigDataPoolAppsEnded|Big Data-Pool, beendete Anwendungen|


## <a name="microsoftsynapseworkspacessqlpools"></a>Microsoft.Synapse/workspaces/sqlPools

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|DmsWorkers|DMS-Worker|
|ExecRequests|Ausführungsanforderungen|
|RequestSteps|Anforderungsschritte|
|SqlRequests|SQL-Anforderungen|
|SQLSecurityAuditEvents|SQL-Sicherheitsüberwachungsereignis|
|Wartevorgänge|Wartevorgänge|


## <a name="microsoftwebhostingenvironments"></a>microsoft.web/hostingenvironments

Kosten: Kostenlos 

|Category |Anzeigename der Kategorie|
|---|---|
|AppServiceEnvironmentPlatformLogs|Plattformprotokolle der App Service-Umgebung|


## <a name="microsoftwebsites"></a>microsoft.web/sites

Kosten: Kostenlos 


|Category |Anzeigename der Kategorie|
|---|---|
|AppServiceAppLogs|App Service-Anwendungsprotokolle|
|AppServiceAuditLogs|Zugriffsüberwachungsprotokolle|
|AppServiceConsoleLogs|App Service-Konsolenprotokolle|
|AppServiceFileAuditLogs|Überwachungsprotokolle für Website-Inhaltsänderungen|
|AppServiceHTTPLogs|HTTP-Protokolle|
|FunctionAppLogs|Funktionsanwendungsprotokolle|


## <a name="microsoftwebsitesslots"></a>microsoft.web/sites/slots

Kosten: Kostenlos 


|Category |Anzeigename der Kategorie|
|---|---|
|AppServiceAppLogs|App Service-Anwendungsprotokolle|
|AppServiceAuditLogs|Zugriffsüberwachungsprotokolle|
|AppServiceConsoleLogs|App Service-Konsolenprotokolle|
|AppServiceFileAuditLogs|Überwachungsprotokolle für Website-Inhaltsänderungen|
|AppServiceHTTPLogs|HTTP-Protokolle|
|FunctionAppLogs|Funktionsanwendungsprotokolle|


## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Ressourcenprotokollen](./platform-logs-overview.md)
* [Streamen von Ressourcenprotokollen an **Event Hubs**](./resource-logs.md#send-to-azure-event-hubs)
* [Ändern der Diagnoseeinstellungen für Ressourcenprotokolle mithilfe der Azure Monitor-REST-API](/rest/api/monitor/diagnosticsettings)
* [Analysieren von Protokollen aus Azure Storage mit Log Analytics](./resource-logs.md#send-to-log-analytics-workspace)

