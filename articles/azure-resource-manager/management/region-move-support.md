---
title: Unterstützung für das regionenübergreifende Verschieben von Azure-Ressourcen
description: Hier werden die Azure-Ressourcentypen aufgelistet, die in Azure-Regionen verschoben werden können.
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 08/25/2020
ms.author: raynew
ms.openlocfilehash: 83cd36683a0487f13ab5707e4b1534cc7f20a88a
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91948526"
---
# <a name="support-for-moving-azure-resources-across-regions"></a>Unterstützung für das regionenübergreifende Verschieben von Azure-Ressourcen

In diesem Artikel wird erläutert, ob ein Azure-Ressourcentyp für das Verschieben in eine andere Azure-Region unterstützt wird. 

Navigieren Sie direkt zu einem Ressourcenanbieter-Namespace:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.BatchAI](#microsoftbatchai)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Container](#microsoftcontainer)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataConnect](#microsoftdataconnect)
> - [Microsoft.DataExchange](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLake](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft.MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft.MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningOperationalization](#microsoftmachinelearningoperationalization)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PortalSdk](#microsoftportalsdk)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProjectOxford](#microsoftprojectoxford)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Scheduler](#microsoftscheduler)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.SqlVM](#microsoftsqlvm)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft.TerraformOSS](#microsoftterraformoss)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WindowsVirtualDesktop](#microsoftwindowsvirtualdesktop)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- | 
> | domainservices | Nein | 
> | domainservices/replicasets | Nein | 

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | tenants | Nein |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | actionRules | Nein | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | servers | Nein |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | Dienst |  Ja (Vorlage verwenden) <br/><br/> [Regionsübergreifendes Verschieben von API Management](../../api-management/api-management-howto-migrate.md). | 

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | configurationstores | Nein | 

## <a name="microsoftappservice"></a>Microsoft.AppService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | apiapps | Ja (Vorlage verwenden)<br/><br/> [Verschieben einer App Service-Apps in eine andere Region](../../app-service/manage-move-across-regions.md) | 
> | appidentities | Nein | 
> | gateways | Nein | 


## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | policyassignments | Nein |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | automationaccounts | Ja (Vorlage verwenden) <br/><br/> [Verwenden von Georeplikation](../../automation/automation-managing-data.md#geo-replication-in-azure-automation) |  
> | automationaccounts/configurations | Nein | 
> | automationaccounts/runbooks | Nein | 



## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | b2cdirectories | Nein | 

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | sqlserverregistrations | Nein |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | registrations | Nein | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | batchaccounts |  Batch-Konten können nicht direkt aus einer Region in eine andere verschoben werden, Sie können jedoch eine Vorlage verwenden, um eine Vorlage zu exportieren, sie zu ändern und dann die Vorlage in der neuen Region bereitzustellen. <br/><br/> Informationen zum [Verschieben eines Batch-Kontos zwischen Regionen](../../batch/best-practices.md#moving-batch-accounts-across-regions) |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | clusters | Nein <br/><br/> Der Azure Batch KI-Dienst wurde [eingestellt](/previous-versions/azure/batch-ai/overview-what-happened-batch-ai).
> | fileservers | Nein | 
> | jobs | Nein | 
> | workspaces | Nein | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | mapapis | Nein | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | biztalk | Nein | 

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | blockchainmembers | Nein <br/><br/> Das Blockchain-Netzwerk kann keine Knoten in unterschiedlichen Regionen haben. 
> | watchers | Nein | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | blueprintassignments | Nein | 

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | botservices | Nein | 

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | redis | Nein | 


## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | cdnwebapplicationfirewallpolicies | Nein |
> | profiles | Nein | 
> | profiles/endpoints | Nein | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | certificateorders | Nein | 


## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | domainnames | Für klassische Dienste ist keine Arbeit geplant.
> | virtualmachines | Nein | 



## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | networksecuritygroups | Für klassische Dienste ist keine Arbeit geplant.
> | reservedips | Nein | 
> | virtualnetworks | Nein | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | storageaccounts | Ja |  


## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein | 
> | Cognitive Search | Unterstützt mit manuellen Schritten.<br/><br/> Informationen zum [Verschieben Ihres Azure Cognitive Search-Diensts in eine andere Region](../../search/search-howto-move-across-regions.md)

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | availabilitysets | Ja <br/><br/> Verschieben Sie mit [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md) Verfügbarkeitsgruppen. | 
> | diskencryptionsets | Nein | 
> | disks | Ja <br/><br/> Verschieben Sie mit [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md) Azure-VMs und zugehörige Datenträger. | 
> | galleries | Nein | 
> | galleries/images | Nein | 
> | galleries/images/versions | Nein | 
> | hostgroups | Nein | 
> | hostgroups/hosts | Nein | 
> | images | Nein | 
> | proximityplacementgroups | Nein | 
> | restorepointcollections | Nein | 
> | sharedvmimages | Nein | 
> | sharedvmimages/versions | Nein | 
> | snapshots | Nein | 
> | virtualmachines | Ja <br/><br/> Verschieben Sie mit [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md) Azure-VMs. | 
> | virtualmachines/extensions | Nein | 
> | virtualmachinescalesets | Nein | 

## <a name="microsoftcontainer"></a>Microsoft.Container

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | containergroups | Nein | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | containergroups | Nein | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | registries | Nein |  
> | registries/buildtasks | Nein |  
> | registries/replications | Nein | 
> | registries/tasks | Nein |  
> | registries/webhooks | Nein | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | containerservices | Nein.<br/><br/> Der Dienst wurde [eingestellt](https://azure.microsoft.com/updates/azure-container-service-will-retire-on-january-31-2020/).
> | managedclusters | Nein | 
> | openshiftmanagedclusters | Nein | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | applications | Nein | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | connectors | Nein |  

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | hubs | Nein |  

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | resourceproviders | Nein | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | jobs | Nein | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | databoxedgedevices | Nein | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | workspaces | Nein | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | catalogs | Nein | 
> | datacatalogs | Nein | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | connectionmanagers | Nein | 

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | packages | Nein | 
> | plans | Nein | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | datafactories | Nein | 
> | factories | Nein |  

## <a name="microsoftdatalake"></a>Microsoft.DataLake

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | datalakeaccounts | Nein | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | services | Nein | 
> | services/projects | Nein | 
> | slots | Nein | 

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | servers | Zum Verschieben eines vorhandenen Servers können Sie ein regionsübergreifendes Lesereplikat verwenden. [Weitere Informationen](../../postgresql/howto-move-regions-portal.md)<br/><br/> Wenn der Dienst mit georedundantem Sicherungsspeicher bereitgestellt wird, können Sie die Geowiederherstellung für die Wiederherstellung in anderen Regionen verwenden. [Weitere Informationen](../../mariadb/concepts-business-continuity.md#recover-from-an-azure-regional-data-center-outage)

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | servers | Zum Verschieben eines vorhandenen Servers können Sie ein regionsübergreifendes Lesereplikat verwenden. [Weitere Informationen](../../mysql/howto-move-regions-portal.md)

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | servergroups | Nein | 
> | servers | Zum Verschieben eines vorhandenen Servers können Sie ein regionsübergreifendes Lesereplikat verwenden. [Weitere Informationen](../../postgresql/howto-move-regions-portal.md)
> | serversv2 | Nein | 

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | artifactsources | Nein | 
> | rollouts | Nein |  
> | servicetopologies | Nein | 
> | servicetopologies/services | Nein |  
> | servicetopologies/services/serviceunits | Nein | 
> | steps | Nein | 

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | elasticpools | Nein. Die Ressource wird nicht verfügbar gemacht.
> | elasticpools/iothubtenants | Nein. Die Ressource wird nicht verfügbar gemacht.
> | iothubs | Ja. [Weitere Informationen](../../iot-hub/iot-hub-how-to-clone.md)
> | provisioningservices | Nein | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | Controller | Nein | 
> | AKS-Cluster | Nein<br/><br/> [Weitere Informationen](../../dev-spaces/faq.md#can-i-migrate-my-aks-cluster-with-azure-dev-spaces-to-another-region) zum Verschieben in eine andere Region.

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | labcenters | Nein | 
> | labs | Nein | 
> | labs/environments | Nein |  
> | labs/servicerunners | Nein | 
> | labs/virtualmachines | Nein |  
> | schedules | Nein |  

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | databaseaccounts | Nein | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | domains | Nein | 

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | services | Nein |  

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | domains | Nein |  
> | topics | Nein | 

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | clusters | Nein |  
> | Namespaces | Ja (mit Vorlage)<br/><br/> [Verschieben eines Event Hub-Namespace in eine andere Region](../../event-hubs/move-across-regions.md) | 

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | hanainstances | Nein | 
> | sapmonitors | Nein |  

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | clusters | Nein | 

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | services | Nein |  

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | machines | Nein | 

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | datamanagers |  Nein | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | jobs |  Nein | 

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein. [Weitere Informationen](../../azure-monitor/faq.md#how-do-i-move-an-application-insights-resource-to-a-new-region).
> | actiongroups |  Nein | 
> | activitylogalerts | Nein | 
> | alertrules |  Nein | 
> | autoscalesettings |  Nein | 
> | components |  Nein |  
> | guestdiagnosticsettings | Nein | 
> | metricalerts | Nein | 
> | notificationgroups | Nein | 
> | notificationrules | Nein | 
> | scheduledqueryrules |  Nein | 
> | webtests |  Nein | 
> | workbooks |  Nein |  


## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | checknameavailability |  Nein.<br/><br/> IoT Central arbeitet mit geografischen Regionen, nicht mit Regionen.
> | graph | Nein

## <a name="microsoftiothub"></a>Microsoft.IoTHub

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> |  iothub |  Ja (Hub klonen) <br/><br/> [Klonen eines IoT-Hubs in eine andere Region](../../iot-hub/iot-hub-how-to-clone.md)

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | checknameavailability |  Nein |  
> | graph |  Nein | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | hsmpools | Nein | 
> | vaults |  Nein | 


## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | clusters |  Nein |  

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | labaccounts | Nein | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein, dies ist ein globaler Dienst.

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | hostingenvironments | Nein | 
> | integrationaccounts |  Nein |  
> | integrationserviceenvironments | Nein | 
> | isolatedenvironments | Nein | 
> | workflows |  Nein |  

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | commitmentplans |  Nein | 
> | webservices |  Nein | 
> | workspaces |  Nein | 

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | operationalizationclusters |  Nein | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein | 
> | accounts/workspaces | Nein | 
> | accounts/workspaces/projects | Nein | 
> | teamaccounts | Nein | 
> | teamaccounts/workspaces | Nein | 
> | teamaccounts/workspaces/projects | Nein | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein | 

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperationalization

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | hostingaccounts | Nein | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | workspaces | Nein | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | userassignedidentities | Nein | 

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts |  Nein, Azure Maps ist ein räumlicher Dienst. 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | classicdevservices | Für klassische Dienste ist keine Arbeit geplant. 

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | mediaservices |  Nein | 
> | mediaservices/liveevents |  Nein | 
> | mediaservices/streamingendpoints |  Nein | 

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | appclusters | Nein | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | assessmentprojects | Nein | 
> | migrateprojects | Nein | 
> | projects | Nein | 

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | netappaccounts | Nein | 
> | netappaccounts/capacitypools | Nein | 
> | netappaccounts/capacitypools/volumes | Nein | 
> | netappaccounts/capacitypools/volumes/mounttargets | Nein | 
> | netappaccounts/capacitypools/volumes/snapshots | Nein | 

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | applicationgateways | Nein | 
> | applicationgatewaywebapplicationfirewallpolicies | Nein | 
> | applicationsecuritygroups |  Nein |  
> | azurefirewalls |  Nein |  
> | bastionhosts | Nein | 
> | connections |  Nein | 
> | ddoscustompolicies |  Nein | 
> | ddosprotectionplans | Nein | 
> | dnszones |  Nein | 
> | expressroutecircuits | Nein | 
> | expressroutecrossconnections | Nein | 
> | expressroutegateways | Nein | 
> | expressrouteports | Nein | 
> | frontdoors | Nein | 
> | frontdoorwebapplicationfirewallpolicies | Nein | 
> | loadbalancers | Ja <br/><br/> Verschieben Sie in- und externe Lastenausgleichsmodule mit [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md). |
> | localnetworkgateways |  Nein | 
> | natgateways |  Nein | 
> | networkintentpolicies |  Nein | 
> | networkinterfaces | Ja <br/><br/> Verschieben Sie NICs mit [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md). | 
> | networkprofiles | Nein | 
> | networksecuritygroups | Ja <br/><br/> Verschieben Sie Netzwerksicherheitsgruppen (NSGs) mit [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md). | 
> | networkwatchers |  Nein |  
> | networkwatchers/connectionmonitors |  Nein | 
> | networkwatchers/lenses |  Nein | 
> | networkwatchers/pingmeshes |  Nein | 
> | p2svpngateways | Nein | 
> | privatednszones |  Nein |  
> | privatednszones/virtualnetworklinks |  Nein |  
> | privateendpoints | Nein | 
> | privatelinkservices | Nein | 
> | publicipaddresses | Ja<br/><br/> Verschieben Sie öffentliche IP-Adressen mit [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md). |
> | publicipprefixes | Nein | 
> | routefilters | Nein | 
> | routetables |  Nein | 
> | serviceendpointpolicies |  Nein | 
> | trafficmanagerprofiles |  Nein | 
> | virtualhubs | Nein | 
> | virtualnetworkgateways |  Nein |  
> | virtualnetworks |  Nein | 
> | virtualnetworktaps | Nein | 
> | virtualwans | Nein | 
> | vpngateways (Virtual WAN) | Nein | 
> | vpnsites (Virtual WAN) | Nein | 
> | webapplicationfirewallpolicies |  Nein | 


## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | Namespaces |  Nein | 
> | namespaces/notificationhubs |  Nein |  

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | workspaces |  Nein | 



## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | managementconfigurations |  Nein | 
> | views |  Nein | 

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | peerings | Nein | 

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | dashboards | Nein | 

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | rootresources | Nein | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | workspacecollections |  Nein | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | capacities |  Nein | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | accounts | Nein | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | vaults | Nein.<br/><br/> Das Verschieben von Recovery Services-Tresoren für Azure Backup zwischen Azure-Regionen wird nicht unterstützt.<br/><br/> In Recovery Services-Tresoren für Azure Site Recovery können Sie [den Tresor deaktivieren und in der Zielregion neu erstellen](../../site-recovery/move-vaults-across-regions.md). | 


## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | Namespaces |  Nein | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | Abfragen |  Nein |  

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung |
> | ------------- | ----------- |
> | deploymentScripts |  Ja<br/><br/>[Verschieben von Ressourcen unter Microsoft.Resources in eine neue Region](microsoft-resources-move-regions.md) |
> | templateSpecs |  Ja<br/><br/>[Verschieben von Ressourcen unter Microsoft.Resources in eine neue Region](microsoft-resources-move-regions.md) |  

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | applications |  Nein | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | flows |  Nein |  
> | jobcollections |  Nein | 

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | searchservices |  Nein | 


## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | iotsecuritysolutions |  Nein | 
> | playbookconfigurations | Nein | 

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | gateways | Nein | 
> | nodes | Nein | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | Namespaces |  Nein | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | applications | Nein | 
> | clusters |  Nein | 
> | clusters/applications | Nein | 
> | containergroups | Nein | 
> | containergroupsets | Nein | 
> | edgeclusters | Nein | 
> | networks | Nein | 
> | secretstores | Nein | 
> | volumes | Nein | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | applications |  Nein | 
> | containergroups | Nein | 
> | gateways |  Nein | 
> | networks |  Nein | 
> | secrets |  Nein | 
> | volumes |  Nein |  

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | signalr |  Nein |  

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | appliancedefinitions | Nein | 
> | appliances | Nein | 
> | applicationdefinitions | Nein | 
> | applications | Nein | 
> | jitrequests | Nein | 

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | instancepools | Nein | 
> | managedinstances | Ja <br/><br/> [Erfahren Sie mehr](../../azure-sql/database/move-resources-across-regions.md) über das regionsübergreifende Verschieben verwalteter Instanzen. | 
> | managedinstances/databases | Ja | 
> | servers | Ja | 
> | servers/databases | Ja <br/><br/> [Erfahren Sie mehr](../../azure-sql/database/move-resources-across-regions.md) über das regionsübergreifende Verschieben von Datenbanken.<br/><br/> [Erfahren Sie mehr](../../resource-mover/tutorial-move-region-sql.md) über das Verschieben von Azure SQL-Datenbank-Instanzen mit Azure Resource Mover.  | 
> | servers/elasticpools | Ja <br/><br/> [Erfahren Sie mehr](../../azure-sql/database/move-resources-across-regions.md) über das regionsübergreifende Verschieben von Pools für elastische Datenbanken.<br/><br/> [Erfahren Sie mehr](../../resource-mover/tutorial-move-region-sql.md) über das Verschieben von Pools für elastische Azure SQL-Datenbank-Instanzen mit Azure Resource Mover.  | 
> | virtualclusters | Ja | 

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | sqlvirtualmachinegroups |  Nein |  
> | sqlvirtualmachines |  Nein |  

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | dwvm | Nein | 

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | storageaccounts | Ja<br/><br/> [Verschieben eines Azure Storage-Kontos in eine andere Region](../../storage/common/storage-account-move.md) | 

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | caches | Nein | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | storagesyncservices |  Nein | 

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | storagesyncservices | Nein | 

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | storagesyncservices | Nein | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | managers | Nein | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | streamingjobs |  Nein |  


## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | environments | Nein | 
> | environments/eventsources | Nein | 
> | instances | Nein | 
> | instances/environments | Nein | 
> | instances/environments/eventsources | Nein | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | providerregistrations | Nein | 
> | ressourcen | Nein | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | environments |  Nein | 
> | environments/eventsources |  Nein |  
> | environments/referencedatasets |  Nein | 

## <a name="microsofttoken"></a>Microsoft.Token

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | stores | Nein | 

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | imagetemplates | Nein | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | account |  Nein | 
> | account/extension |  Nein | 
> | account/project |  Nein | 



## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | dedicatedcloudnodes | Nein | 
> | dedicatedcloudservices | Nein | 
> | virtualmachines | Nein | 

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | certificates | Nein | 
> | connectiongateways |  Nein |  
> | connections |  Nein |  
> | customapis |  Nein | 
> | hostingenvironments | Nein | 
> | serverfarms |  Nein |  
> | sites |  Nein | 
> | sites/premieraddons |  Nein |  
> | sites/slots |  Nein |  


## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | deviceservices | Nein | 

## <a name="microsoftwindowsvirtualdesktop"></a>Microsoft.WindowsVirtualDesktop

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Regionenverschiebung | 
> | ------------- | ----------- |
> | applicationgroups | Nein | 
> | hostpools | Nein | 
> | workspaces | Nein | 

## <a name="third-party-services"></a>Dienste von Drittanbietern

Derzeit wird der Verschiebevorgang nicht für Drittanbieterdienste unterstützt.