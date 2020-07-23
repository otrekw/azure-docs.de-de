---
title: Unterstützung des Verschiebevorgangs nach Ressourcentyp
description: Eine Liste von Azure-Ressourcentypen, die in eine neue Ressourcengruppe oder ein neues Abonnement verschoben werden können.
ms.topic: conceptual
ms.date: 07/13/2020
ms.openlocfilehash: 7b7f6f05c9cae80cffb64245a3fc8a6b0890d577
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86539226"
---
# <a name="move-operation-support-for-resources"></a>Unterstützung des Verschiebevorgangs für Ressourcen

In diesem Artikel wird aufgeführt, für welche Azure-Ressourcentypen der Verschiebevorgang unterstützt wird. Außerdem finden Sie hier Informationen zu speziellen Bedingungen, die beim Verschieben einer Ressource berücksichtigt werden müssen.

Navigieren Sie direkt zu einem Ressourcenanbieter-Namespace:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.Addons](#microsoftaddons)
> - [Microsoft.ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Attestation](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AVS](#microsoftavs)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.AzureStackHCI](#microsoftazurestackhci)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Billing](#microsoftbilling)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Capacity](#microsoftcapacity)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ChangeAnalysis](#microsoftchangeanalysis)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.ClassicSubscription](#microsoftclassicsubscription)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CostManagementExports](#microsoftcostmanagementexports)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [Microsoft.CustomerLockbox](#microsoftcustomerlockbox)
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
> - [Microsoft.DataProtection](#microsoftdataprotection)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DigitalTwins](#microsoftdigitaltwins)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Experimentation](#microsoftexperimentation)
> - [Microsoft.Falcon](#microsoftfalcon)
> - [Microsoft.Features](#microsoftfeatures)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.HybridNetwork](#microsofthybridnetwork)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kubernetes](#microsoftkubernetes)
> - [Microsoft.KubernetesConfiguration](#microsoftkubernetesconfiguration)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft.MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft.MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.ManagedNetwork](#microsoftmanagednetwork)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.Marketplace](#microsoftmarketplace)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.MixedReality](#microsoftmixedreality)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.PowerPlatform](#microsoftpowerplatform)
> - [Microsoft.ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.ProviderHub](#microsoftproviderhub)
> - [Microsoft.Quantum](#microsoftquantum)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.RedHatOpenShift](#microsoftredhatopenshift)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.SerialConsole](#microsoftserialconsole)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft.Subscription](#microsoftsubscription)
> - [microsoft.support](#microsoftsupport)
> - [Microsoft.Synapse](#microsoftsynapse)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft.VMware](#microsoftvmware)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.VnfManager](#microsoftvnfmanager)
> - [Microsoft.VSOnline](#microsoftvsonline)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsESU](#microsoftwindowsesu)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WorkloadBuilder](#microsoftworkloadbuilder)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | domainservices | Nein | Nein |
> | domainservices/oucontainer | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | diagnosticsettings | Nein | Nein |
> | diagnosticsettingscategories | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | privatelinkforazuread | Ja | Ja |
> | tenants | Ja | Ja |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | supportproviders | Nein | Nein |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | aadsupportcases | Nein | Nein |
> | addsservices | Nein | Nein |
> | agents | Nein | Nein |
> | anonymousapiusers | Nein | Nein |
> | Konfiguration | Nein | Nein |
> | logs | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | reports | Nein | Nein |
> | servicehealthmetrics | Nein | Nein |
> | services | Nein | Nein |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Konfigurationen | Nein | Nein |
> | generaterecommendations | Nein | Nein |
> | metadata | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | empfehlungen | Nein | Nein |
> | suppressions | Nein | Nein |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | actionRules | Ja | Ja |
> | alerts | Nein | Nein |
> | alertslist | Nein | Nein |
> | alertsmetadata | Nein | Nein |
> | alertssummary | Nein | Nein |
> | alertssummarylist | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | smartdetectoralertrules | Ja | Ja |
> | smartgroups | Nein | Nein |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | servers | Ja | Ja |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checkfeedbackrequired | Nein | Nein |
> | checknameavailability | Nein | Nein |
> | checkservicenameavailability | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | reportfeedback | Nein | Nein |
> | Dienst | Ja | Ja |
> | validateservicename | Nein | Nein |

> [!IMPORTANT]
> Ein auf die Verbrauchs-SKU festgelegter API Management-Dienst kann nicht verschoben werden.

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | configurationstores | Ja | Ja |
> | configurationstores/eventgridfilters | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operationsstatus | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operationstatus | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | spring | Ja | Ja |
> | spring/apps | Nein | Nein |
> | spring/apps/deployments | Nein | Nein |

## <a name="microsoftappservice"></a>Microsoft.AppService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | apiapps | Nein | Nein |
> | appidentities | Nein | Nein |
> | gateways | Nein | Nein |

> [!IMPORTANT]
> Weitere Informationen finden Sie unter [Move guidance for App Service resources](./move-limitations/app-service-move-limitations.md) (Anleitung zum Verschieben von App Service-Ressourcen).

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | attestationproviders | Ja | Ja |
> | Vorgänge | Nein | Nein |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checkaccess | Nein | Nein |
> | classicadministrators | Nein | Nein |
> | dataaliases | Nein | Nein |
> | denyassignments | Nein | Nein |
> | elevateaccess | Nein | Nein |
> | findorphanroleassignments | Nein | Nein |
> | locks | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | operationstatus | Nein | Nein |
> | Berechtigungen | Nein | Nein |
> | policyassignments | Nein | Nein |
> | policydefinitions | Nein | Nein |
> | policysetdefinitions | Nein | Nein |
> | privatelinkassociations | Nein | Nein |
> | provideroperations | Nein | Nein |
> | resourcemanagementprivatelinks | Nein | Nein |
> | roleassignments | Nein | Nein |
> | roleassignmentsusagemetrics | Nein | Nein |
> | roledefinitions | Nein | Nein |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | automationaccounts | Ja | Ja |
> | automationaccounts/configurations | Ja | Ja |
> | automationaccounts/jobs | Nein | Nein |
> | automationaccounts/privateendpointconnectionproxies | Nein | Nein |
> | automationaccounts/privateendpointconnections | Nein | Nein |
> | automationaccounts/privatelinkresources | Nein | Nein |
> | automationaccounts/runbooks | Ja | Ja |
> | automationaccounts/softwareupdateconfigurations | Nein | Nein |
> | automationaccounts/webhooks | Nein | Nein |
> | Vorgänge | Nein | Nein |

> [!IMPORTANT]
> Runbooks müssen sich in der gleichen Ressourcengruppe befinden wie das Automation-Konto.
>
> Weitere Informationen finden Sie unter [Verschieben Ihres Azure Automation-Kontos in ein anderes Abonnement](../../automation/how-to/move-account.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftavs"></a>Microsoft.AVS

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/checkquotaavailability | Nein | Nein |
> | locations/checktrialavailability | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | privateclouds | Ja | Ja |
> | privateclouds/clusters | Nein | Nein |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | b2cdirectories | Ja | Ja |
> | b2ctenants | Nein | Nein |
> | checknameavailability | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | datacontrollers | Nein | Nein |
> | hybriddatamanagers | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | postgresinstances | Nein | Nein |
> | sqlinstances | Nein | Nein |
> | sqlmanagedinstances | Nein | Nein |
> | sqlserverinstances | Nein | Nein |
> | sqlserverregistrations | Ja | Ja |
> | sqlserverregistrations/sqlservers | Nein | Nein |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | cloudmanifestfiles | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | registrations | Ja | Ja |
> | registrations/customersubscriptions | Nein | Nein |
> | registrations/products | Nein | Nein |

## <a name="microsoftazurestackhci"></a>Microsoft.AzureStackHCI

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | batchaccounts | Ja | Ja |
> | locations | Nein | Nein |
> | locations/accountoperationresults | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/quotas | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | billingaccounts | Nein | Nein |
> | billingaccounts/agreements | Nein | Nein |
> | billingaccounts/billingpermissions | Nein | Nein |
> | billingaccounts/billingprofiles | Nein | Nein |
> | billingaccounts/billingprofiles/availablebalance | Nein | Nein |
> | billingaccounts/billingprofiles/billingpermissions | Nein | Nein |
> | billingaccounts/billingprofiles/billingroleassignments | Nein | Nein |
> | billingaccounts/billingprofiles/billingroledefinitions | Nein | Nein |
> | billingaccounts/billingprofiles/billingsubscriptions | Nein | Nein |
> | billingaccounts/billingprofiles/createbillingroleassignment | Nein | Nein |
> | billingaccounts/billingprofiles/customers | Nein | Nein |
> | billingaccounts/billingprofiles/instructions | Nein | Nein |
> | billingaccounts/billingprofiles/invoices | Nein | Nein |
> | billingaccounts/billingprofiles/invoices/pricesheet | Nein | Nein |
> | billingaccounts/billingprofiles/invoices/transactions | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections/billingpermissions | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections/billingroleassignments | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections/billingroledefinitions | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections/billingsubscriptions | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections/createbillingroleassignment | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections/initiatetransfer | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections/products | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections/products/transfer | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections/products/updateautorenew | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections/transactions | Nein | Nein |
> | billingaccounts/billingprofiles/invoicesections/transfers | Nein | Nein |
> | billingaccounts/billingprofiles/patchoperations | Nein | Nein |
> | billingaccounts/billingprofiles/paymentmethods | Nein | Nein |
> | billingaccounts/billingprofiles/policies | Nein | Nein |
> | billingaccounts/billingprofiles/pricesheet | Nein | Nein |
> | billingaccounts/billingprofiles/pricesheetdownloadoperations | Nein | Nein |
> | billingaccounts/billingprofiles/products | Nein | Nein |
> | billingaccounts/billingprofiles/transactions | Nein | Nein |
> | billingaccounts/billingroleassignments | Nein | Nein |
> | billingaccounts/billingroledefinitions | Nein | Nein |
> | billingaccounts/billingsubscriptions | Nein | Nein |
> | billingaccounts/billingsubscriptions/invoices | Nein | Nein |
> | billingaccounts/createbillingroleassignment | Nein | Nein |
> | billingaccounts/createinvoicesectionoperations | Nein | Nein |
> | billingaccounts/customers | Nein | Nein |
> | billingaccounts/customers/billingpermissions | Nein | Nein |
> | billingaccounts/customers/billingsubscriptions | Nein | Nein |
> | billingaccounts/customers/initiatetransfer | Nein | Nein |
> | billingaccounts/customers/policies | Nein | Nein |
> | billingaccounts/customers/products | Nein | Nein |
> | billingaccounts/customers/transactions | Nein | Nein |
> | billingaccounts/customers/transfers | Nein | Nein |
> | billingaccounts/departments | Nein | Nein |
> | billingaccounts/enrollmentaccounts | Nein | Nein |
> | billingaccounts/invoices | Nein | Nein |
> | billingaccounts/invoicesections | Nein | Nein |
> | billingaccounts/invoicesections/billingsubscriptionmoveoperations | Nein | Nein |
> | billingaccounts/invoicesections/billingsubscriptions | Nein | Nein |
> | billingaccounts/invoicesections/billingsubscriptions/transfer | Nein | Nein |
> | billingaccounts/invoicesections/elevate | Nein | Nein |
> | billingaccounts/invoicesections/initiatetransfer | Nein | Nein |
> | billingaccounts/invoicesections/patchoperations | Nein | Nein |
> | billingaccounts/invoicesections/productmoveoperations | Nein | Nein |
> | billingaccounts/invoicesections/products | Nein | Nein |
> | billingaccounts/invoicesections/products/transfer | Nein | Nein |
> | billingaccounts/invoicesections/products/updateautorenew | Nein | Nein |
> | billingaccounts/invoicesections/producttransfersresults | Nein | Nein |
> | billingaccounts/invoicesections/transactions | Nein | Nein |
> | billingaccounts/invoicesections/transfers | Nein | Nein |
> | billingaccounts/lineofcredit | Nein | Nein |
> | billingaccounts/listinvoicesectionswithcreatesubscriptionpermission | Nein | Nein |
> | billingaccounts/operationresults | Nein | Nein |
> | billingaccounts/patchoperations | Nein | Nein |
> | billingaccounts/paymentmethods | Nein | Nein |
> | billingaccounts/products | Nein | Nein |
> | billingaccounts/transactions | Nein | Nein |
> | billingperiods | Nein | Nein |
> | billingpermissions | Nein | Nein |
> | billingproperty | Nein | Nein |
> | billingroleassignments | Nein | Nein |
> | billingroledefinitions | Nein | Nein |
> | createbillingroleassignment | Nein | Nein |
> | departments | Nein | Nein |
> | enrollmentaccounts | Nein | Nein |
> | invoices | Nein | Nein |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | operationstatus | Nein | Nein |
> | transfers | Nein | Nein |
> | transfers/accepttransfer | Nein | Nein |
> | transfers/declinetransfer | Nein | Nein |
> | transfers/operationstatus | Nein | Nein |
> | transfers/validatetransfer | Nein | Nein |
> | validateaddress | Nein | Nein |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | listcommunicationpreference | Nein | Nein |
> | mapapis | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | updatecommunicationpreference | Nein | Nein |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | biztalk | Nein | Nein |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | blockchainmembers | Nein | Nein |
> | cordamembers | Nein | Nein |
> | locations | Nein | Nein |
> | locations/blockchainmemberoperationresults | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/listconsortiums | Nein | Nein |
> | locations/watcheroperationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | watchers | Nein | Nein |

## <a name="microsoftblockchaintokens"></a>Microsoft.BlockchainTokens

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Vorgänge | Nein | Nein |
> | tokenservices | Nein | Nein |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | blueprintassignments | Nein | Nein |
> | blueprintassignments/assignmentoperations | Nein | Nein |
> | blueprintassignments/operations | Nein | Nein |
> | blueprints | Nein | Nein |
> | blueprints/artifacts | Nein | Nein |
> | blueprints/versions | Nein | Nein |
> | blueprints/versions/artifacts | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | botservices | Ja | Ja |
> | botservices/channels | Nein | Nein |
> | botservices/connections | Nein | Nein |
> | checknameavailability | Nein | Nein |
> | listauthserviceproviders | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operationsstatus | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | redis | Ja | Ja |
> | redis/eventgridfilters | Nein | Nein |
> | redis/privatelinkresources | Nein | Nein |
> | redisenterprise | Nein | Nein |

> [!IMPORTANT]
> Wenn die Azure Cache for Redis-Instanz mit einem virtuellen Netzwerk konfiguriert ist, kann die Instanz nicht in ein anderes Abonnement verschoben werden. Weitere Informationen finden Sie unter [Einschränkungen beim Verschieben von Netzwerkressourcen](./move-limitations/networking-move-limitations.md).

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | appliedreservations | Nein | Nein |
> | calculateexchange | Nein | Nein |
> | calculateprice | Nein | Nein |
> | calculatepurchaseprice | Nein | Nein |
> | catalogs | Nein | Nein |
> | checkoffers | Nein | Nein |
> | checkpurchasestatus | Nein | Nein |
> | checkscopes | Nein | Nein |
> | commercialreservationorders | Nein | Nein |
> | Börse | Nein | Nein |
> | listbenefits | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | placepurchaseorder | Nein | Nein |
> | reservationorders | Nein | Nein |
> | reservationorders/availablescopes | Nein | Nein |
> | reservationorders/calculaterefund | Nein | Nein |
> | reservationorders/merge | Nein | Nein |
> | reservationorders/reservations | Nein | Nein |
> | reservationorders/reservations/availablescopes | Nein | Nein |
> | reservationorders/reservations/revisions | Nein | Nein |
> | reservationorders/return | Nein | Nein |
> | reservationorders/split | Nein | Nein |
> | reservationorders/swap | Nein | Nein |
> | reservations | Nein | Nein |
> | ressourcen | Nein | Nein |
> | validatereservationorder | Nein | Nein |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | cdnwebapplicationfirewallmanagedrulesets | Nein | Nein |
> | cdnwebapplicationfirewallpolicies | Ja | Ja |
> | checknameavailability | Nein | Nein |
> | checkresourceusage | Nein | Nein |
> | edgenodes | Nein | Nein |
> | operationresults | Nein | Nein |
> | operationresults/profileresults | Nein | Nein |
> | operationresults/profileresults/endpointresults | Nein | Nein |
> | operationresults/profileresults/endpointresults/customdomainresults | Nein | Nein |
> | operationresults/profileresults/endpointresults/origingroupresults | Nein | Nein |
> | operationresults/profileresults/endpointresults/originresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | profiles | Ja | Ja |
> | profiles/endpoints | Ja | Ja |
> | profiles/endpoints/customdomains | Nein | Nein |
> | profiles/endpoints/origingroups | Nein | Nein |
> | profiles/endpoints/origins | Nein | Nein |
> | validateprobe | Nein | Nein |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | certificateorders | Ja | Ja |
> | certificateorders/certificates | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | validatecertificateregistrationinformation | Nein | Nein |

> [!IMPORTANT]
> Weitere Informationen finden Sie unter [Move guidance for App Service resources](./move-limitations/app-service-move-limitations.md) (Anleitung zum Verschieben von App Service-Ressourcen).

## <a name="microsoftchangeanalysis"></a>Microsoft.ChangeAnalysis

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Vorgänge | Nein | Nein |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | capabilities | Nein | Nein |
> | checkdomainnameavailability | Nein | Nein |
> | domainnames | Ja | Nein |
> | domainnames/capabilities | Nein | Nein |
> | domainnames/internalloadbalancers | Nein | Nein |
> | domainnames/servicecertificates | Nein | Nein |
> | domainnames/slots | Nein | Nein |
> | domainnames/slots/roles | Nein | Nein |
> | domainnames/slots/roles/metricdefinitions | Nein | Nein |
> | domainnames/slots/roles/metrics | Nein | Nein |
> | movesubscriptionresources | Nein | Nein |
> | operatingsystemfamilies | Nein | Nein |
> | operatingsystems | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | operationstatuses | Nein | Nein |
> | quotas | Nein | Nein |
> | resourcetypes | Nein | Nein |
> | validatesubscriptionmoveavailability | Nein | Nein |
> | virtualmachines | Ja | Nein |
> | virtualmachines/diagnosticsettings | Nein | Nein |
> | virtualmachines/metricdefinitions | Nein | Nein |
> | virtualmachines/metrics | Nein | Nein |

> [!IMPORTANT]
> Weitere Informationen finden Sie unter [Move guidance for Classic deployment model resources](./move-limitations/classic-model-move-limitations.md) (Anleitung zum Verschieben von Ressourcen des klassischen Bereitstellungsmodells). Klassische Bereitstellungsressourcen können mithilfe eines szenariospezifischen Vorgangs zwischen Abonnements verschoben werden.

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | classicinfrastructureresources | Nein | Nein |

> [!IMPORTANT]
> Weitere Informationen finden Sie unter [Move guidance for Classic deployment model resources](./move-limitations/classic-model-move-limitations.md) (Anleitung zum Verschieben von Ressourcen des klassischen Bereitstellungsmodells). Klassische Bereitstellungsressourcen können mithilfe eines szenariospezifischen Vorgangs zwischen Abonnements verschoben werden.

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | capabilities | Nein | Nein |
> | expressroutecrossconnections | Nein | Nein |
> | expressroutecrossconnections/peerings | Nein | Nein |
> | gatewaysupporteddevices | Nein | Nein |
> | networksecuritygroups | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | quotas | Nein | Nein |
> | reservedips | Nein | Nein |
> | virtualnetworks | Nein | Nein |
> | virtualnetworks/remotevirtualnetworkpeeringproxies | Nein | Nein |
> | virtualnetworks/virtualnetworkpeerings | Nein | Nein |

> [!IMPORTANT]
> Weitere Informationen finden Sie unter [Move guidance for Classic deployment model resources](./move-limitations/classic-model-move-limitations.md) (Anleitung zum Verschieben von Ressourcen des klassischen Bereitstellungsmodells). Klassische Bereitstellungsressourcen können mithilfe eines szenariospezifischen Vorgangs zwischen Abonnements verschoben werden.

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | capabilities | Nein | Nein |
> | checkstorageaccountavailability | Nein | Nein |
> | disks | Nein | Nein |
> | images | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | osimages | Nein | Nein |
> | osplatformimages | Nein | Nein |
> | publicimages | Nein | Nein |
> | quotas | Nein | Nein |
> | storageaccounts | Ja | Nein |
> | storageaccounts/blobservices | Nein | Nein |
> | storageaccounts/fileservices | Nein | Nein |
> | storageaccounts/metricdefinitions | Nein | Nein |
> | storageaccounts/metrics | Nein | Nein |
> | storageaccounts/queueservices | Nein | Nein |
> | storageaccounts/services | Nein | Nein |
> | storageaccounts/services/diagnosticsettings | Nein | Nein |
> | storageaccounts/services/metricdefinitions | Nein | Nein |
> | storageaccounts/services/metrics | Nein | Nein |
> | storageaccounts/tableservices | Nein | Nein |
> | storageaccounts/vmimages | Nein | Nein |
> | vmimages | Nein | Nein |

> [!IMPORTANT]
> Weitere Informationen finden Sie unter [Move guidance for Classic deployment model resources](./move-limitations/classic-model-move-limitations.md) (Anleitung zum Verschieben von Ressourcen des klassischen Bereitstellungsmodells). Klassische Bereitstellungsressourcen können mithilfe eines szenariospezifischen Vorgangs zwischen Abonnements verschoben werden.

## <a name="microsoftclassicsubscription"></a>Microsoft.ClassicSubscription

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Vorgänge | Nein | Nein |

> [!IMPORTANT]
> Weitere Informationen finden Sie unter [Move guidance for Classic deployment model resources](./move-limitations/classic-model-move-limitations.md) (Anleitung zum Verschieben von Ressourcen des klassischen Bereitstellungsmodells). Klassische Bereitstellungsressourcen können mithilfe eines szenariospezifischen Vorgangs zwischen Abonnements verschoben werden.

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Ja | Ja |
> | checkdomainavailability | Nein | Nein |
> | locations | Nein | Nein |
> | locations/checkskuavailability | Nein | Nein |
> | locations/deletevirtualnetworkorsubnets | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Vorgänge | Nein | Nein |
> | ratecard | Nein | Nein |
> | usageaggregates | Nein | Nein |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availabilitysets | Ja | Ja |
> | diskaccesses | Nein | Nein |
> | diskencryptionsets | Nein | Nein |
> | disks | Ja | Ja |
> | galleries | Nein | Nein |
> | galleries/images | Nein | Nein |
> | galleries/images/versions | Nein | Nein |
> | hostgroups | Nein | Nein |
> | hostgroups/hosts | Nein | Nein |
> | images | Ja | Ja |
> | locations | Nein | Nein |
> | locations/artifactpublishers | Nein | Nein |
> | locations/capsoperations | Nein | Nein |
> | locations/diskoperations | Nein | Nein |
> | locations/loganalytics | Nein | Nein |
> | locations/operations | Nein | Nein |
> | locations/publishers | Nein | Nein |
> | locations/runcommands | Nein | Nein |
> | locations/usages | Nein | Nein |
> | locations/virtualmachines | Nein | Nein |
> | locations/vmsizes | Nein | Nein |
> | locations/vsmoperations | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | proximityplacementgroups | Ja | Ja |
> | restorepointcollections | Nein | Nein |
> | restorepointcollections/restorepoints | Nein | Nein |
> | sharedvmextensions | Nein | Nein |
> | sharedvmimages | Nein | Nein |
> | sharedvmimages/versions | Nein | Nein |
> | snapshots | Ja | Ja |
> | sshpublickeys | Nein | Nein |
> | virtualmachines | Ja | Ja |
> | virtualmachines/extensions | Ja | Ja |
> | virtualmachines/metricdefinitions | Nein | Nein |
> | virtualmachines/runcommands | Nein | Nein |
> | virtualmachinescalesets | Ja | Ja |
> | virtualmachinescalesets/extensions | Nein | Nein |
> | virtualmachinescalesets/networkinterfaces | Nein | Nein |
> | virtualmachinescalesets/publicipaddresses | Nein | Nein |
> | virtualmachinescalesets/virtualmachines | Nein | Nein |
> | virtualmachinescalesets/virtualmachines/networkinterfaces | Nein | Nein |

> [!IMPORTANT]
> Weitere Informationen finden Sie unter [Move guidance for virtual machines](./move-limitations/virtual-machines-move-limitations.md) (Anleitung zum Verschieben virtueller Computer).

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | aggregatedcost | Nein | Nein |
> | balances | Nein | Nein |
> | budgets | Nein | Nein |
> | charges | Nein | Nein |
> | costtags | Nein | Nein |
> | credits | Nein | Nein |
> | events | Nein | Nein |
> | forecasts | Nein | Nein |
> | lots | Nein | Nein |
> | marketplaces | Nein | Nein |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | operationstatus | Nein | Nein |
> | pricesheets | Nein | Nein |
> | products | Nein | Nein |
> | reservationdetails | Nein | Nein |
> | reservationrecommendationdetails | Nein | Nein |
> | reservationrecommendations | Nein | Nein |
> | reservationsummaries | Nein | Nein |
> | reservationtransactions | Nein | Nein |
> | tags | Nein | Nein |
> | tenants | Nein | Nein |
> | terms | Nein | Nein |
> | usagedetails | Nein | Nein |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | containergroups | Nein | Nein |
> | locations | Nein | Nein |
> | locations/cachedimages | Nein | Nein |
> | locations/capabilities | Nein | Nein |
> | locations/deletevirtualnetworkorsubnets | Nein | Nein |
> | locations/operations | Nein | Nein |
> | locations/usages | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | serviceassociationlinks | Nein | Nein |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | locations | Nein | Nein |
> | locations/authorize | Nein | Nein |
> | locations/deletevirtualnetworkorsubnets | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/setupauth | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | registries | Ja | Ja |
> | registries/agentpools | Ja | Ja |
> | registries/agentpools/listqueuestatus | Nein | Nein |
> | registries/builds | Nein | Nein |
> | registries/builds/cancel | Nein | Nein |
> | registries/builds/getloglink | Nein | Nein |
> | registries/buildtasks | Ja | Ja |
> | registries/buildtasks/listsourcerepositoryproperties | Nein | Nein |
> | registries/buildtasks/steps | Nein | Nein |
> | registries/buildtasks/steps/listbuildarguments | Nein | Nein |
> | registries/eventgridfilters | Nein | Nein |
> | registries/exportpipelines | Nein | Nein |
> | registries/generatecredentials | Nein | Nein |
> | registries/getbuildsourceuploadurl | Nein | Nein |
> | registries/getcredentials | Nein | Nein |
> | registries/importimage | Nein | Nein |
> | registries/importpipelines | Nein | Nein |
> | registries/listbuildsourceuploadurl | Nein | Nein |
> | registries/listcredentials | Nein | Nein |
> | registries/listpolicies | Nein | Nein |
> | registries/listusages | Nein | Nein |
> | registries/pipelineruns | Nein | Nein |
> | registries/privateendpointconnectionproxies | Nein | Nein |
> | registries/privateendpointconnectionproxies/validate | Nein | Nein |
> | registries/privateendpointconnections | Nein | Nein |
> | registries/privatelinkresources | Nein | Nein |
> | registries/queuebuild | Nein | Nein |
> | registries/regeneratecredential | Nein | Nein |
> | registries/regeneratecredentials | Nein | Nein |
> | registries/replications | Ja | Ja |
> | registries/runs | Nein | Nein |
> | registries/runs/cancel | Nein | Nein |
> | registries/runs/listlogsasurl | Nein | Nein |
> | registries/schedulerun | Nein | Nein |
> | registries/scopemaps | Nein | Nein |
> | registries/taskruns | Nein | Nein |
> | registries/taskruns/listdetails | Nein | Nein |
> | registries/tasks | Ja | Ja |
> | registries/tasks/listdetails | Nein | Nein |
> | registries/tokens | Nein | Nein |
> | registries/updatepolicies | Nein | Nein |
> | registries/webhooks | Ja | Ja |
> | registries/webhooks/getcallbackconfig | Nein | Nein |
> | registries/webhooks/listevents | Nein | Nein |
> | registries/webhooks/ping | Nein | Nein |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | containerservices | Nein | Nein |
> | locations | Nein | Nein |
> | locations/openshiftclusters | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operations | Nein | Nein |
> | locations/orchestrators | Nein | Nein |
> | managedclusters | Nein | Nein |
> | openshiftmanagedclusters | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applications | Nein | Nein |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Nein | Nein |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | alerts | Nein | Nein |
> | billingaccounts | Nein | Nein |
> | budgets | Nein | Nein |
> | cloudconnectors | Nein | Nein |
> | connectors | Ja | Ja |
> | departments | Nein | Nein |
> | dimensions | Nein | Nein |
> | enrollmentaccounts | Nein | Nein |
> | exports | Nein | Nein |
> | externalbillingaccounts | Nein | Nein |
> | externalbillingaccounts/alerts | Nein | Nein |
> | externalbillingaccounts/dimensions | Nein | Nein |
> | externalbillingaccounts/forecast | Nein | Nein |
> | externalbillingaccounts/query | Nein | Nein |
> | externalsubscriptions | Nein | Nein |
> | externalsubscriptions/alerts | Nein | Nein |
> | externalsubscriptions/dimensions | Nein | Nein |
> | externalsubscriptions/forecast | Nein | Nein |
> | externalsubscriptions/query | Nein | Nein |
> | forecast | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | Abfrage | Nein | Nein |
> | Registrieren | Nein | Nein |
> | reportconfigs | Nein | Nein |
> | reports | Nein | Nein |
> | settings | Nein | Nein |
> | showbackrules | Nein | Nein |
> | views | Nein | Nein |

## <a name="microsoftcostmanagementexports"></a>Microsoft.CostManagementExports

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Vorgänge | Nein | Nein |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hubs | Nein | Nein |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Vorgänge | Nein | Nein |
> | requests | Nein | Nein |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | associations | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | resourceproviders | Ja | Ja |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | jobs | Nein | Nein |
> | locations | Nein | Nein |
> | locations/availableskus | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/regionconfiguration | Nein | Nein |
> | locations/validateaddress | Nein | Nein |
> | locations/validateinputs | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availableskus | Nein | Nein |
> | databoxedgedevices | Ja | Ja |
> | databoxedgedevices/checknameavailability | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/getnetworkpolicies | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | workspaces | Nein | Nein |
> | workspaces/dbworkspaces | Nein | Nein |
> | workspaces/virtualnetworkpeerings | Nein | Nein |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | catalogs | Ja | Ja |
> | checknameavailability | Nein | Nein |
> | datacatalogs | Nein | Nein |
> | locations | Nein | Nein |
> | locations/jobs | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | connectionmanagers | Nein | Nein |

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | packages | Nein | Nein |
> | plans | Nein | Nein |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checkazuredatafactorynameavailability | Nein | Nein |
> | checkdatafactorynameavailability | Nein | Nein |
> | datafactories | Ja | Ja |
> | datafactories/diagnosticsettings | Nein | Nein |
> | datafactories/metricdefinitions | Nein | Nein |
> | datafactoryschema | Nein | Nein |
> | factories | Ja | Ja |
> | factories/integrationruntimes | Nein | Nein |
> | locations | Nein | Nein |
> | locations/configurefactoryrepo | Nein | Nein |
> | locations/getfeaturevalue | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftdatalake"></a>Microsoft.DataLake

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | datalakeaccounts | Nein | Nein |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Ja | Ja |
> | accounts/datalakestoreaccounts | Nein | Nein |
> | accounts/storageaccounts | Nein | Nein |
> | accounts/storageaccounts/containers | Nein | Nein |
> | accounts/storageaccounts/containers/listsastokens | Nein | Nein |
> | locations | Nein | Nein |
> | locations/capability | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/usages | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Ja | Ja |
> | accounts/eventgridfilters | Nein | Nein |
> | accounts/firewallrules | Nein | Nein |
> | locations | Nein | Nein |
> | locations/capability | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/deletevirtualnetworkorsubnets | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/usages | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | services | Nein | Nein |
> | services/projects | Nein | Nein |
> | slots | Nein | Nein |

## <a name="microsoftdataprotection"></a>Microsoft.DataProtection

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | backupvaults | Nein | Nein |
> | locations | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Ja | Ja |
> | accounts/shares | Nein | Nein |
> | accounts/shares/datasets | Nein | Nein |
> | accounts/shares/invitations | Nein | Nein |
> | accounts/shares/providersharesubscriptions | Nein | Nein |
> | accounts/shares/synchronizationsettings | Nein | Nein |
> | accounts/sharesubscriptions | Nein | Nein |
> | accounts/sharesubscriptions/consumersourcedatasets | Nein | Nein |
> | accounts/sharesubscriptions/datasetmappings | Nein | Nein |
> | accounts/sharesubscriptions/triggers | Nein | Nein |
> | listinvitations | Nein | Nein |
> | locations | Nein | Nein |
> | locations/consumerinvitations | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/rejectinvitation | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | locations | Nein | Nein |
> | locations/azureasyncoperation | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/performancetiers | Nein | Nein |
> | locations/privateendpointconnectionazureasyncoperation | Nein | Nein |
> | locations/privateendpointconnectionoperationresults | Nein | Nein |
> | locations/privateendpointconnectionproxyazureasyncoperation | Nein | Nein |
> | locations/privateendpointconnectionproxyoperationresults | Nein | Nein |
> | locations/recommendedactionsessionsazureasyncoperation | Nein | Nein |
> | locations/recommendedactionsessionsoperationresults | Nein | Nein |
> | locations/securityalertpoliciesazureasyncoperation | Nein | Nein |
> | locations/securityalertpoliciesoperationresults | Nein | Nein |
> | locations/serverkeyazureasyncoperation | Nein | Nein |
> | locations/serverkeyoperationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | servers | Ja | Ja |
> | servers/advisors | Nein | Nein |
> | servers/privateendpointconnectionproxies | Nein | Nein |
> | servers/privateendpointconnections | Nein | Nein |
> | servers/privatelinkresources | Nein | Nein |
> | servers/querytexts | Nein | Nein |
> | servers/recoverableservers | Nein | Nein |
> | servers/topquerystatistics | Nein | Nein |
> | servers/virtualnetworkrules | Nein | Nein |
> | servers/waitstatistics | Nein | Nein |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | locations | Nein | Nein |
> | locations/administratorazureasyncoperation | Nein | Nein |
> | locations/administratoroperationresults | Nein | Nein |
> | locations/azureasyncoperation | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/performancetiers | Nein | Nein |
> | locations/privateendpointconnectionazureasyncoperation | Nein | Nein |
> | locations/privateendpointconnectionoperationresults | Nein | Nein |
> | locations/privateendpointconnectionproxyazureasyncoperation | Nein | Nein |
> | locations/privateendpointconnectionproxyoperationresults | Nein | Nein |
> | locations/recommendedactionsessionsazureasyncoperation | Nein | Nein |
> | locations/recommendedactionsessionsoperationresults | Nein | Nein |
> | locations/securityalertpoliciesazureasyncoperation | Nein | Nein |
> | locations/securityalertpoliciesoperationresults | Nein | Nein |
> | locations/serverkeyazureasyncoperation | Nein | Nein |
> | locations/serverkeyoperationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | servers | Ja | Ja |
> | servers/advisors | Nein | Nein |
> | servers/keys | Nein | Nein |
> | servers/privateendpointconnectionproxies | Nein | Nein |
> | servers/privateendpointconnections | Nein | Nein |
> | servers/privatelinkresources | Nein | Nein |
> | servers/querytexts | Nein | Nein |
> | servers/recoverableservers | Nein | Nein |
> | servers/topquerystatistics | Nein | Nein |
> | servers/virtualnetworkrules | Nein | Nein |
> | servers/waitstatistics | Nein | Nein |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | locations | Nein | Nein |
> | locations/administratorazureasyncoperation | Nein | Nein |
> | locations/administratoroperationresults | Nein | Nein |
> | locations/azureasyncoperation | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/performancetiers | Nein | Nein |
> | locations/privateendpointconnectionazureasyncoperation | Nein | Nein |
> | locations/privateendpointconnectionoperationresults | Nein | Nein |
> | locations/privateendpointconnectionproxyazureasyncoperation | Nein | Nein |
> | locations/privateendpointconnectionproxyoperationresults | Nein | Nein |
> | locations/recommendedactionsessionsazureasyncoperation | Nein | Nein |
> | locations/recommendedactionsessionsoperationresults | Nein | Nein |
> | locations/securityalertpoliciesazureasyncoperation | Nein | Nein |
> | locations/securityalertpoliciesoperationresults | Nein | Nein |
> | locations/serverkeyazureasyncoperation | Nein | Nein |
> | locations/serverkeyoperationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | servergroups | Nein | Nein |
> | servers | Ja | Ja |
> | servers/advisors | Nein | Nein |
> | servers/keys | Nein | Nein |
> | servers/privateendpointconnectionproxies | Nein | Nein |
> | servers/privateendpointconnections | Nein | Nein |
> | servers/privatelinkresources | Nein | Nein |
> | servers/querytexts | Nein | Nein |
> | servers/recoverableservers | Nein | Nein |
> | servers/topquerystatistics | Nein | Nein |
> | servers/virtualnetworkrules | Nein | Nein |
> | servers/waitstatistics | Nein | Nein |
> | serversv2 | Ja | Ja |
> | singleservers | Ja | Ja |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | artifactsources | Ja | Ja |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | rollouts | Ja | Ja |
> | servicetopologies | Ja | Ja |
> | servicetopologies/services | Ja | Ja |
> | servicetopologies/services/serviceunits | Ja | Ja |
> | steps | Ja | Ja |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applicationgroups | Ja | Ja |
> | applicationgroups/applications | Nein | Nein |
> | applicationgroups/desktops | Nein | Nein |
> | applicationgroups/startmenuitems | Nein | Nein |
> | hostpools | Ja | Ja |
> | hostpools/sessionhosts | Nein | Nein |
> | hostpools/sessionhosts/usersessions | Nein | Nein |
> | hostpools/usersessions | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | workspaces | Ja | Ja |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | checkprovisioningservicenameavailability | Nein | Nein |
> | elasticpools | Nein | Nein |
> | elasticpools/iothubtenants | Nein | Nein |
> | iothubs | Ja | Ja |
> | iothubs/eventgridfilters | Nein | Nein |
> | iothubs/securitysettings | Nein | Nein |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | provisioningservices | Ja | Ja |
> | usages | Nein | Nein |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | pipelines | Ja | Ja |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Controller | Ja | Ja |
> | controllers/listconnectiondetails | Nein | Nein |
> | locations | Nein | Nein |
> | locations/checkcontainerhostmapping | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | labcenters | Nein | Nein |
> | labs | Ja | Nein |
> | labs/environments | Ja | Ja |
> | labs/servicerunners | Ja | Ja |
> | labs/virtualmachines | Ja | Nein |
> | locations | Nein | Nein |
> | locations/operations | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | schedules | Ja | Ja |

## <a name="microsoftdigitaltwins"></a>Microsoft.DigitalTwins

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | digitaltwinsinstances | Nein | Nein |
> | digitaltwinsinstances/operationresults | Nein | Nein |
> | locations | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | databaseaccountnames | Nein | Nein |
> | databaseaccounts | Ja | Ja |
> | locations | Nein | Nein |
> | locations/deletevirtualnetworkorsubnets | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operationsstatus | Nein | Nein |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checkdomainavailability | Nein | Nein |
> | domains | Ja | Ja |
> | domains/domainownershipidentifiers | Nein | Nein |
> | generatessorequest | Nein | Nein |
> | listdomainrecommendations | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | topleveldomains | Nein | Nein |
> | validatedomainregistrationinformation | Nein | Nein |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | services | Ja | Ja |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | domains | Ja | Ja |
> | domains/topics | Nein | Nein |
> | eventsubscriptions | Nein, kann nicht unabhängig verschoben, aber automatisch mit abonnierter Ressource verschoben werden. | Nein, kann nicht unabhängig verschoben, aber automatisch mit abonnierter Ressource verschoben werden. |
> | extensiontopics | Nein | Nein |
> | locations | Nein | Nein |
> | locations/eventsubscriptions | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operationsstatus | Nein | Nein |
> | locations/topictypes | Nein | Nein |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | operationsstatus | Nein | Nein |
> | partnernamespaces | Ja | Ja |
> | partnernamespaces/eventchannels | Nein | Nein |
> | partnerregistrations | Nein | Nein |
> | partnertopics | Ja | Ja |
> | partnertopics/eventsubscriptions | Nein | Nein |
> | systemtopics | Ja | Ja |
> | systemtopics/eventsubscriptions | Nein | Nein |
> | topics | Ja | Ja |
> | topictypes | Nein | Nein |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availableclusterregions | Nein | Nein |
> | checknameavailability | Nein | Nein |
> | checknamespaceavailability | Nein | Nein |
> | clusters | Ja | Ja |
> | locations | Nein | Nein |
> | locations/deletevirtualnetworkorsubnets | Nein | Nein |
> | Namespaces | Ja | Ja |
> | namespaces/authorizationrules | Nein | Nein |
> | namespaces/disasterrecoveryconfigs | Nein | Nein |
> | namespaces/disasterrecoveryconfigs/checknameavailability | Nein | Nein |
> | namespaces/eventhubs | Nein | Nein |
> | namespaces/eventhubs/authorizationrules | Nein | Nein |
> | namespaces/eventhubs/consumergroups | Nein | Nein |
> | namespaces/networkrulesets | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | sku | Nein | Nein |

## <a name="microsoftexperimentation"></a>Microsoft.Experimentation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | experimentworkspaces | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operations | Nein | Nein |

## <a name="microsoftfalcon"></a>Microsoft.Falcon

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Namespaces | Ja | Ja |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | featureproviders | Nein | Nein |
> | Features | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | providers | Nein | Nein |
> | subscriptionfeatureregistrations | Nein | Nein |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Nein | Nein |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | automanagedaccounts | Nein | Nein |
> | automanagedvmconfigurationprofiles | Nein | Nein |
> | guestconfigurationassignments | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | software | Nein | Nein |
> | softwareupdateprofile | Nein | Nein |
> | softwareupdates | Nein | Nein |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hanainstances | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operations | Nein | Nein |
> | locations/operationsstatus | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | sapmonitors | Ja | Ja |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | dedicatedhsms | Nein | Nein |
> | locations | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | Ja | Ja |
> | clusters/applications | Nein | Nein |
> | clusters/operationresults | Nein | Nein |
> | locations | Nein | Nein |
> | locations/azureasyncoperations | Nein | Nein |
> | locations/billingspecs | Nein | Nein |
> | locations/capabilities | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/usages | Nein | Nein |
> | locations/validatecreaterequest | Nein | Nein |
> | Vorgänge | Nein | Nein |

> [!IMPORTANT]
> Sie können HDInsight-Cluster in ein neues Abonnement oder eine neue Ressourcengruppe verschieben. Dagegen können die mit dem HDInsight-Cluster verknüpften Netzwerkressourcen (z.B. virtuelles Netzwerk, NIC oder Load Balancer) nicht zwischen Abonnements verschoben werden. Darüber hinaus kann eine NIC, die an einen virtuellen Computer für den Cluster angefügt ist, nicht in eine neue Ressourcengruppe verschoben werden.
>
> Beim Verschieben eines HDInsight-Clusters in ein neues Abonnement sollten Sie zunächst andere Ressourcen (z.B. das Speicherkonto) verschieben. Verschieben Sie erst anschließend den HDInsight-Cluster.

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | services | Ja | Ja |
> | services/privateendpointconnections | Nein | Nein |
> | services/privatelinkresources | Nein | Nein |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operationstatus | Nein | Nein |
> | machines | Ja | Ja |
> | machines/extensions | Ja | Ja |
> | Vorgänge | Nein | Nein |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | datamanagers | Ja | Ja |
> | Vorgänge | Nein | Nein |

## <a name="microsofthybridnetwork"></a>Microsoft.HybridNetwork

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | devices | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | vnfs | Nein | Nein |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | components | Nein | Nein |
> | locations | Nein | Nein |
> | networkscopes | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | jobs | Ja | Ja |
> | locations | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | actiongroups | Ja | Ja |
> | activitylogalerts | Nein | Nein |
> | alertrules | Ja | Ja |
> | autoscalesettings | Ja | Ja |
> | baseline | Nein | Nein |
> | calculatebaseline | Nein | Nein |
> | components | Ja | Ja |
> | components/events | Nein | Nein |
> | components/linkedstorageaccounts | Nein | Nein |
> | components/metadata | Nein | Nein |
> | components/metrics | Nein | Nein |
> | components/pricingplans | Nein | Nein |
> | components/query | Nein | Nein |
> | datacollectionrules | Nein | Nein |
> | diagnosticsettings | Nein | Nein |
> | diagnosticsettingscategories | Nein | Nein |
> | eventcategories | Nein | Nein |
> | eventtypes | Nein | Nein |
> | extendeddiagnosticsettings | Nein | Nein |
> | guestdiagnosticsettings | Nein | Nein |
> | listmigrationdate | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | logdefinitions | Nein | Nein |
> | logprofiles | Nein | Nein |
> | logs | Nein | Nein |
> | metricalerts | Nein | Nein |
> | metricbaselines | Nein | Nein |
> | metricbatch | Nein | Nein |
> | metricdefinitions | Nein | Nein |
> | metricnamespaces | Nein | Nein |
> | metrics | Nein | Nein |
> | migratealertrules | Nein | Nein |
> | migratetonewpricingmodel | Nein | Nein |
> | myworkbooks | Nein | Nein |
> | notificationgroups | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | privatelinkscopeoperationstatuses | Nein | Nein |
> | privatelinkscopes | Nein | Nein |
> | privatelinkscopes/privateendpointconnectionproxies | Nein | Nein |
> | privatelinkscopes/privateendpointconnections | Nein | Nein |
> | privatelinkscopes/scopedresources | Nein | Nein |
> | rollbacktolegacypricingmodel | Nein | Nein |
> | scheduledqueryrules | Ja | Ja |
> | Topologie | Nein | Nein |
> | Transaktionen | Nein | Nein |
> | vminsightsonboardingstatuses | Nein | Nein |
> | webtests | Ja | Ja |
> | webtests/gettestresultfile | Nein | Nein |
> | workbooks | Ja | Ja |
> | workbooktemplates | Ja | Ja |

> [!IMPORTANT]
> Berücksichtigen Sie beim Verschieben in ein neues Abonnement die [Abonnementkontingente](azure-subscription-service-limits.md#azure-monitor-limits).

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | apptemplates | Nein | Nein |
> | checknameavailability | Nein | Nein |
> | checksubdomainavailability | Nein | Nein |
> | iotapps | Ja | Ja |
> | Vorgänge | Nein | Nein |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Ja | Ja |
> | graph | Ja | Ja |
> | Vorgänge | Nein | Nein |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | deletedvaults | Nein | Nein |
> | hsmpools | Nein | Nein |
> | locations | Nein | Nein |
> | locations/deletedvaults | Nein | Nein |
> | locations/deletevirtualnetworkorsubnets | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | managedhsms | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | vaults | Ja | Ja |
> | vaults/accesspolicies | Nein | Nein |
> | vaults/eventgridfilters | Nein | Nein |
> | vaults/secrets | Nein | Nein |

> [!IMPORTANT]
> Für die Datenträgerverschlüsselung verwendete Schlüsseltresore können nicht in einer Ressourcengruppe im gleichen Abonnement oder zwischen Abonnements verschoben werden.

## <a name="microsoftkubernetes"></a>Microsoft.Kubernetes

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | connectedclusters | Ja | Ja |
> | locations | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | registeredsubscriptions | Nein | Nein |

## <a name="microsoftkubernetesconfiguration"></a>Microsoft.KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | sourcecontrolconfigurations | Nein | Nein |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | Ja | Ja |
> | clusters/attacheddatabaseconfigurations | Nein | Nein |
> | clusters/databases | Nein | Nein |
> | clusters/databases/dataconnections | Nein | Nein |
> | clusters/databases/eventhubconnections | Nein | Nein |
> | clusters/databases/principalassignments | Nein | Nein |
> | clusters/principalassignments | Nein | Nein |
> | locations | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | labaccounts | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operations | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | users | Nein | Nein |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Nein | Nein |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Nein | Nein |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hostingenvironments | Nein | Nein |
> | integrationaccounts | Ja | Ja |
> | integrationserviceenvironments | Ja | Nein |
> | integrationserviceenvironments/managedapis | Ja | Nein |
> | isolatedenvironments | Nein | Nein |
> | locations | Nein | Nein |
> | locations/workflows | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | workflows | Ja | Ja |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | commitmentplans | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operations | Nein | Nein |
> | locations/operationsstatus | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | webservices | Ja | Nein |
> | workspaces | Ja | Ja |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | operationalizationclusters | Nein | Nein |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Nein | Nein |
> | accounts/workspaces | Nein | Nein |
> | accounts/workspaces/projects | Nein | Nein |
> | teamaccounts | Nein | Nein |
> | teamaccounts/workspaces | Nein | Nein |
> | teamaccounts/workspaces/projects | Nein | Nein |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Nein | Nein |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/computeoperationsstatus | Nein | Nein |
> | locations/quotas | Nein | Nein |
> | locations/updatequotas | Nein | Nein |
> | locations/usages | Nein | Nein |
> | locations/vmsizes | Nein | Nein |
> | locations/workspaceoperationsstatus | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | workspaces | Nein | Nein |
> | workspaces/computes | Nein | Nein |
> | workspaces/eventgridfilters | Nein | Nein |

## <a name="microsoftmaintenance"></a>Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applyupdates | Nein | Nein |
> | configurationassignments | Nein | Nein |
> | maintenanceconfigurations | Ja | Ja |
> | updates | Nein | Nein |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Identitäten | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | userassignedidentities | Nein | Nein |

## <a name="microsoftmanagednetwork"></a>Microsoft.ManagedNetwork

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | managednetworks | Nein | Nein |
> | managednetworks/managednetworkgroups | Nein | Nein |
> | managednetworks/managednetworkpeeringpolicies | Nein | Nein |
> | Benachrichtigung | Nein | Nein |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | marketplaceregistrationdefinitions | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | operationstatuses | Nein | Nein |
> | registrationassignments | Nein | Nein |
> | registrationdefinitions | Nein | Nein |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | getentities | Nein | Nein |
> | managementgroups | Nein | Nein |
> | managementgroups/settings | Nein | Nein |
> | operationresults | Nein | Nein |
> | operationresults/asyncoperation | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | ressourcen | Nein | Nein |
> | starttenantbackfill | Nein | Nein |
> | tenantbackfillstatus | Nein | Nein |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Ja | Ja |
> | accounts/eventgridfilters | Nein | Nein |
> | accounts/privateatlases | Ja | Ja |
> | Vorgänge | Nein | Nein |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | listavailableoffers | Nein | Nein |
> | offers | Nein | Nein |
> | offertypes | Nein | Nein |
> | offertypes/publishers | Nein | Nein |
> | offertypes/publishers/offers | Nein | Nein |
> | offertypes/publishers/offers/plans | Nein | Nein |
> | offertypes/publishers/offers/plans/agreements | Nein | Nein |
> | offertypes/publishers/offers/plans/configs | Nein | Nein |
> | offertypes/publishers/offers/plans/configs/importimage | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | privategalleryitems | Nein | Nein |
> | privatestoreclient | Nein | Nein |
> | privatestores | Nein | Nein |
> | privatestores/offers | Nein | Nein |
> | products | Nein | Nein |
> | publishers | Nein | Nein |
> | publishers/offers | Nein | Nein |
> | publishers/offers/amendments | Nein | Nein |
> | Registrieren | Nein | Nein |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | classicdevservices | Nein | Nein |
> | listcommunicationpreference | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | updatecommunicationpreference | Nein | Nein |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | agreements | Nein | Nein |
> | offertypes | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | locations | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | mediaservices | Ja | Ja |
> | mediaservices/accountfilters | Nein | Nein |
> | mediaservices/assets | Nein | Nein |
> | mediaservices/assets/assetfilters | Nein | Nein |
> | mediaservices/contentkeypolicies | Nein | Nein |
> | mediaservices/eventgridfilters | Nein | Nein |
> | mediaservices/liveeventoperations | Nein | Nein |
> | mediaservices/liveevents | Ja | Ja |
> | mediaservices/liveevents/liveoutputs | Nein | Nein |
> | mediaservices/liveoutputoperations | Nein | Nein |
> | mediaservices/streamingendpointoperations | Nein | Nein |
> | mediaservices/streamingendpoints | Ja | Ja |
> | mediaservices/streaminglocators | Nein | Nein |
> | mediaservices/streamingpolicies | Nein | Nein |
> | mediaservices/transforms | Nein | Nein |
> | mediaservices/transforms/jobs | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | appclusters | Nein | Nein |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | assessmentprojects | Nein | Nein |
> | locations | Nein | Nein |
> | locations/assessmentoptions | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | migrateprojects | Nein | Nein |
> | movecollections | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | projects | Nein | Nein |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | holographicsbroadcastaccounts | Nein | Nein |
> | locations | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | objectunderstandingaccounts | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | remoterenderingaccounts | Ja | Ja |
> | spatialanchorsaccounts | Ja | Ja |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | netappaccounts | Nein | Nein |
> | netappaccounts/backuppolicies | Nein | Nein |
> | netappaccounts/capacitypools | Nein | Nein |
> | netappaccounts/capacitypools/volumes | Nein | Nein |
> | netappaccounts/capacitypools/volumes/mounttargets | Nein | Nein |
> | netappaccounts/capacitypools/volumes/snapshots | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applicationgatewayavailablerequestheaders | Nein | Nein |
> | applicationgatewayavailableresponseheaders | Nein | Nein |
> | applicationgatewayavailableservervariables | Nein | Nein |
> | applicationgatewayavailablessloptions | Nein | Nein |
> | applicationgatewayavailablewafrulesets | Nein | Nein |
> | applicationgateways | Nein | Nein |
> | applicationgatewaywebapplicationfirewallpolicies | Nein | Nein |
> | applicationsecuritygroups | Ja | Ja |
> | azurefirewallfqdntags | Nein | Nein |
> | azurefirewalls | Nein | Nein |
> | bastionhosts | Nein | Nein |
> | bgpservicecommunities | Nein | Nein |
> | checkfrontdoornameavailability | Nein | Nein |
> | checktrafficmanagernameavailability | Nein | Nein |
> | connections | Ja | Ja |
> | ddoscustompolicies | Ja | Ja |
> | ddosprotectionplans | Nein | Nein |
> | dnsoperationresults | Nein | Nein |
> | dnsoperationstatuses | Nein | Nein |
> | dnszones | Ja | Ja |
> | dnszones/a | Nein | Nein |
> | dnszones/aaaa | Nein | Nein |
> | dnszones/all | Nein | Nein |
> | dnszones/caa | Nein | Nein |
> | dnszones/cname | Nein | Nein |
> | dnszones/mx | Nein | Nein |
> | dnszones/ns | Nein | Nein |
> | dnszones/ptr | Nein | Nein |
> | dnszones/recordsets | Nein | Nein |
> | dnszones/soa | Nein | Nein |
> | dnszones/srv | Nein | Nein |
> | dnszones/txt | Nein | Nein |
> | expressroutecircuits | Nein | Nein |
> | expressroutegateways | Nein | Nein |
> | expressrouteserviceproviders | Nein | Nein |
> | firewallpolicies | Ja | Ja |
> | frontdooroperationresults | Nein | Nein |
> | frontdoors | Nein | Nein |
> | frontdoors/frontendendpoints | Nein | Nein |
> | frontdoorwebapplicationfirewallmanagedrulesets | Nein | Nein |
> | frontdoorwebapplicationfirewallpolicies | Nein | Nein |
> | getdnsresourcereference | Nein | Nein |
> | internalnotify | Nein | Nein |
> | ipallocations | Ja | Ja |
> | ipgroups | Ja | Ja |
> | loadbalancers | Ja: Basic-SKU<br>Nein: Standard-SKU | Ja: Basic-SKU<br>Nein: Standard-SKU |
> | localnetworkgateways | Ja | Ja |
> | locations | Nein | Nein |
> | locations/autoapprovedprivatelinkservices | Nein | Nein |
> | locations/availabledelegations | Nein | Nein |
> | locations/availableprivateendpointtypes | Nein | Nein |
> | locations/availableservicealiases | Nein | Nein |
> | locations/baremetaltenants | Nein | Nein |
> | locations/batchnotifyprivateendpointsforresourcemove | Nein | Nein |
> | locations/batchvalidateprivateendpointsforresourcemove | Nein | Nein |
> | locations/checkacceleratednetworkingsupport | Nein | Nein |
> | locations/checkdnsnameavailability | Nein | Nein |
> | locations/checkprivatelinkservicevisibility | Nein | Nein |
> | locations/commitinternalazurenetworkmanagerconfiguration | Nein | Nein |
> | locations/effectiveresourceownership | Nein | Nein |
> | locations/nfvoperationresults | Nein | Nein |
> | locations/nfvoperations | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operations | Nein | Nein |
> | locations/servicetags | Nein | Nein |
> | locations/setresourceownership | Nein | Nein |
> | locations/supportedvirtualmachinesizes | Nein | Nein |
> | locations/usages | Nein | Nein |
> | locations/validateresourceownership | Nein | Nein |
> | locations/virtualnetworkavailableendpointservices | Nein | Nein |
> | natgateways | Nein | Nein |
> | networkexperimentprofiles | Nein | Nein |
> | networkintentpolicies | Ja | Ja |
> | networkinterfaces | Ja | Ja |
> | networkprofiles | Nein | Nein |
> | networksecuritygroups | Ja | Ja |
> | networkwatchers | Ja | Nein |
> | networkwatchers/connectionmonitors | Ja | Nein |
> | networkwatchers/flowlogs | Ja | Nein |
> | networkwatchers/pingmeshes | Ja | Nein |
> | Vorgänge | Nein | Nein |
> | p2svpngateways | Nein | Nein |
> | privatednsoperationresults | Nein | Nein |
> | privatednsoperationstatuses | Nein | Nein |
> | privatednszones | Ja | Ja |
> | privatednszones/a | Nein | Nein |
> | privatednszones/aaaa | Nein | Nein |
> | privatednszones/all | Nein | Nein |
> | privatednszones/cname | Nein | Nein |
> | privatednszones/mx | Nein | Nein |
> | privatednszones/ptr | Nein | Nein |
> | privatednszones/soa | Nein | Nein |
> | privatednszones/srv | Nein | Nein |
> | privatednszones/txt | Nein | Nein |
> | privatednszones/virtualnetworklinks | Ja | Ja |
> | privatednszonesinternal | Nein | Nein |
> | privateendpointredirectmaps | Nein | Nein |
> | privateendpoints | Ja | Ja |
> | privatelinkservices | Nein | Nein |
> | publicipaddresses | Ja: Basic-SKU<br>Nein: Standard-SKU | Ja: Basic-SKU<br>Nein: Standard-SKU |
> | publicipprefixes | Ja | Ja |
> | routefilters | Nein | Nein |
> | routetables | Ja | Ja |
> | securitypartnerproviders | Ja | Ja |
> | serviceendpointpolicies | Ja | Ja |
> | trafficmanagergeographichierarchies | Nein | Nein |
> | trafficmanagerprofiles | Ja | Ja |
> | trafficmanagerprofiles/heatmaps | Nein | Nein |
> | trafficmanagerusermetricskeys | Nein | Nein |
> | virtualhubs | Nein | Nein |
> | virtualnetworkgateways | Ja | Ja |
> | virtualnetworks | Ja | Ja |
> | virtualnetworktaps | Nein | Nein |
> | virtualrouters | Ja | Ja |
> | virtualwans | Nein | Nein |
> | vpngateways (Virtual WAN) | Nein | Nein |
> | vpnserverconfigurations | Nein | Nein |
> | vpnsites (Virtual WAN) | Nein | Nein |

> [!IMPORTANT]
> Weitere Informationen finden Sie unter [Move guidance for networking resources](./move-limitations/networking-move-limitations.md) (Anleitung zum Verschieben von Netzwerkressourcen).

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | checknamespaceavailability | Nein | Nein |
> | Namespaces | Ja | Ja |
> | namespaces/notificationhubs | Ja | Ja |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | osnamespaces | Ja | Ja |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hypervsites | Nein | Nein |
> | importsites | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | serversites | Nein | Nein |
> | vmwaresites | Nein | Nein |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | Nein | Nein |
> | deletedworkspaces | Nein | Nein |
> | linktargets | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | storageinsightconfigs | Nein | Nein |
> | workspaces | Ja | Ja |
> | workspaces/datasources | Nein | Nein |
> | workspaces/linkedservices | Nein | Nein |
> | workspaces/linkedstorageaccounts | Nein | Nein |
> | workspaces/metadata | Nein | Nein |
> | workspaces/query | Nein | Nein |
> | workspaces/scopedprivatelinkproxies | Nein | Nein |

> [!IMPORTANT]
> Berücksichtigen Sie beim Verschieben in ein neues Abonnement die [Abonnementkontingente](azure-subscription-service-limits.md#azure-monitor-limits).
>
> Arbeitsbereiche, die ein verknüpftes Automation-Konto besitzen, können nicht verschoben werden. Bevor Sie mit einem Verschiebevorgang beginnen, stellen Sie sicher, dass Sie die Verknüpfung jeglicher Automation-Kontos aufheben.

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | managementassociations | Nein | Nein |
> | managementconfigurations | Ja | Ja |
> | Vorgänge | Nein | Nein |
> | solutions | Ja | Ja |
> | views | Ja | Ja |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checkserviceprovideravailability | Nein | Nein |
> | legacypeerings | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | peerasns | Nein | Nein |
> | peeringlocations | Nein | Nein |
> | peerings | Nein | Nein |
> | peeringservicecountries | Nein | Nein |
> | peeringservicelocations | Nein | Nein |
> | peeringserviceproviders | Nein | Nein |
> | peeringservices | Nein | Nein |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | asyncoperationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | policyevents | Nein | Nein |
> | policystates | Nein | Nein |
> | policytrackedresources | Nein | Nein |
> | remediations | Nein | Nein |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | consoles | Nein | Nein |
> | dashboards | Ja | Ja |
> | locations | Nein | Nein |
> | locations/consoles | Nein | Nein |
> | locations/usersettings | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | usersettings | Nein | Nein |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | workspacecollections | Ja | Ja |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | capacities | Ja | Ja |
> | locations | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftpowerplatform"></a>Microsoft.PowerPlatform

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Vorgänge | Nein | Nein |

## <a name="microsoftprojectbabylon"></a>Microsoft.ProjectBabylon

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Nein | Nein |
> | checknameavailability | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftproviderhub"></a>Microsoft.ProviderHub

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availableaccounts | Nein | Nein |
> | providerregistrations | Nein | Nein |
> | providerregistrations/resourcetyperegistrations | Nein | Nein |
> | rollouts | Nein | Nein |

## <a name="microsoftquantum"></a>Microsoft.Quantum

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | workspaces | Nein | Nein |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | backupprotecteditems | Nein | Nein |
> | locations | Nein | Nein |
> | locations/allocatedstamp | Nein | Nein |
> | locations/allocatestamp | Nein | Nein |
> | locations/backupaadproperties | Nein | Nein |
> | locations/backupcrossregionrestore | Nein | Nein |
> | locations/backupcrrjob | Nein | Nein |
> | locations/backupcrrjobs | Nein | Nein |
> | locations/backupcrroperationresults | Nein | Nein |
> | locations/backupcrroperationsstatus | Nein | Nein |
> | locations/backupprevalidateprotection | Nein | Nein |
> | locations/backupstatus | Nein | Nein |
> | locations/backupvalidatefeatures | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | replicationeligibilityresults | Nein | Nein |
> | vaults | Ja | Ja |

> [!IMPORTANT]
> Weitere Informationen finden Sie unter [Verschieben eines Recovery Services-Tresors zwischen Azure-Abonnements und Ressourcengruppen](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftredhatopenshift"></a>Microsoft.RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operationsstatus | Nein | Nein |
> | openshiftclusters | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | Namespaces | Ja | Ja |
> | namespaces/authorizationrules | Nein | Nein |
> | namespaces/hybridconnections | Nein | Nein |
> | namespaces/hybridconnections/authorizationrules | Nein | Nein |
> | namespaces/privateendpointconnections | Nein | Nein |
> | namespaces/wcfrelays | Nein | Nein |
> | namespaces/wcfrelays/authorizationrules | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | Vorgänge | Nein | Nein |
> | Abfragen | Ja | Ja |
> | resourcechangedetails | Nein | Nein |
> | resourcechanges | Nein | Nein |
> | ressourcen | Nein | Nein |
> | resourceshistory | Nein | Nein |
> | subscriptionsstatus | Nein | Nein |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availabilitystatuses | Nein | Nein |
> | childavailabilitystatuses | Nein | Nein |
> | childresources | Nein | Nein |
> | emergingissues | Nein | Nein |
> | events | Nein | Nein |
> | metadata | Nein | Nein |
> | Benachrichtigungen | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | calculatetemplatehash | Nein | Nein |
> | checkpolicycompliance | Nein | Nein |
> | checkresourcename | Nein | Nein |
> | deployments | Nein | Nein |
> | deployments/operations | Nein | Nein |
> | deploymentscripts | Nein | Nein |
> | deploymentscripts/logs | Nein | Nein |
> | Verknüpfungen | Nein | Nein |
> | locations | Nein | Nein |
> | locations/deploymentscriptoperationresults | Nein | Nein |
> | notifyresourcejobs | Nein | Nein |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | providers | Nein | Nein |
> | resourcegroups | Nein | Nein |
> | ressourcen | Nein | Nein |
> | subscriptions | Nein | Nein |
> | subscriptions/locations | Nein | Nein |
> | subscriptions/operationresults | Nein | Nein |
> | subscriptions/providers | Nein | Nein |
> | subscriptions/resourcegroups | Nein | Nein |
> | subscriptions/resourcegroups/resources | Nein | Nein |
> | subscriptions/resources | Nein | Nein |
> | subscriptions/tagnames | Nein | Nein |
> | subscriptions/tagnames/tagvalues | Nein | Nein |
> | tags | Nein | Nein |
> | templatespecs | Nein | Nein |
> | templatespecs/versions | Nein | Nein |
> | tenants | Nein | Nein |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applications | Ja | Nein |
> | checkmoderneligibility | Nein | Nein |
> | checknameavailability | Nein | Nein |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | saasresources | Nein | Nein |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | checkservicenameavailability | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | resourcehealthmetadata | Nein | Nein |
> | searchservices | Ja | Ja |

> [!IMPORTANT]
> Es ist nicht möglich, mehrere Search-Ressourcen in verschiedenen Regionen gleichzeitig zu verschieben. Verschieben Sie diese stattdessen in mehreren Vorgängen.

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | adaptivenetworkhardenings | Nein | Nein |
> | advancedthreatprotectionsettings | Nein | Nein |
> | alerts | Nein | Nein |
> | allowedconnections | Nein | Nein |
> | applicationwhitelistings | Nein | Nein |
> | assessmentmetadata | Nein | Nein |
> | assessments | Nein | Nein |
> | autodismissalertsrules | Nein | Nein |
> | automations | Ja | Ja |
> | autoprovisioningsettings | Nein | Nein |
> | complianceresults | Nein | Nein |
> | compliances | Nein | Nein |
> | datacollectionagents | Nein | Nein |
> | devicesecuritygroups | Nein | Nein |
> | discoveredsecuritysolutions | Nein | Nein |
> | externalsecuritysolutions | Nein | Nein |
> | informationprotectionpolicies | Nein | Nein |
> | iotsecuritysolutions | Ja | Ja |
> | iotsecuritysolutions/analyticsmodels | Nein | Nein |
> | iotsecuritysolutions/analyticsmodels/aggregatedalerts | Nein | Nein |
> | iotsecuritysolutions/analyticsmodels/aggregatedrecommendations | Nein | Nein |
> | jitnetworkaccesspolicies | Nein | Nein |
> | locations | Nein | Nein |
> | locations/alerts | Nein | Nein |
> | locations/allowedconnections | Nein | Nein |
> | locations/applicationwhitelistings | Nein | Nein |
> | locations/discoveredsecuritysolutions | Nein | Nein |
> | locations/externalsecuritysolutions | Nein | Nein |
> | locations/jitnetworkaccesspolicies | Nein | Nein |
> | locations/securitysolutions | Nein | Nein |
> | locations/securitysolutionsreferencedata | Nein | Nein |
> | locations/tasks | Nein | Nein |
> | locations/topologies | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | Richtlinien | Nein | Nein |
> | pricings | Nein | Nein |
> | regulatorycompliancestandards | Nein | Nein |
> | regulatorycompliancestandards/regulatorycompliancecontrols | Nein | Nein |
> | regulatorycompliancestandards/regulatorycompliancecontrols/regulatorycomplianceassessments | Nein | Nein |
> | securitycontacts | Nein | Nein |
> | securitysolutions | Nein | Nein |
> | securitysolutionsreferencedata | Nein | Nein |
> | securitystatuses | Nein | Nein |
> | securitystatusessummaries | Nein | Nein |
> | servervulnerabilityassessments | Nein | Nein |
> | settings | Nein | Nein |
> | subassessments | Nein | Nein |
> | Tasks | Nein | Nein |
> | topologies | Nein | Nein |
> | workspacesettings | Nein | Nein |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | aggregations | Nein | Nein |
> | alertrules | Nein | Nein |
> | alertruletemplates | Nein | Nein |
> | automationrules | Nein | Nein |
> | bookmarks | Nein | Nein |
> | cases | Nein | Nein |
> | dataconnectors | Nein | Nein |
> | dataconnectorscheckrequirements | Nein | Nein |
> | entities | Nein | Nein |
> | entityqueries | Nein | Nein |
> | incidents | Nein | Nein |
> | officeconsents | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | settings | Nein | Nein |
> | threatintelligence | Nein | Nein |

## <a name="microsoftserialconsole"></a>Microsoft.SerialConsole

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | consoleservices | Nein | Nein |
> | locations | Nein | Nein |
> | locations/consoleservices | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | gateways | Nein | Nein |
> | nodes | Nein | Nein |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | checknamespaceavailability | Nein | Nein |
> | locations | Nein | Nein |
> | locations/deletevirtualnetworkorsubnets | Nein | Nein |
> | Namespaces | Ja | Ja |
> | namespaces/authorizationrules | Nein | Nein |
> | namespaces/disasterrecoveryconfigs | Nein | Nein |
> | namespaces/disasterrecoveryconfigs/checknameavailability | Nein | Nein |
> | namespaces/eventgridfilters | Nein | Nein |
> | namespaces/networkrulesets | Nein | Nein |
> | namespaces/queues | Nein | Nein |
> | namespaces/queues/authorizationrules | Nein | Nein |
> | namespaces/topics | Nein | Nein |
> | namespaces/topics/authorizationrules | Nein | Nein |
> | namespaces/topics/subscriptions | Nein | Nein |
> | namespaces/topics/subscriptions/rules | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | premiummessagingregions | Nein | Nein |
> | sku | Nein | Nein |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applications | Nein | Nein |
> | clusters | Ja | Ja |
> | clusters/applications | Nein | Nein |
> | containergroups | Nein | Nein |
> | containergroupsets | Nein | Nein |
> | edgeclusters | Nein | Nein |
> | locations | Nein | Nein |
> | locations/clusterversions | Nein | Nein |
> | locations/environments | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operations | Nein | Nein |
> | managedclusters | Nein | Nein |
> | networks | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | secretstores | Nein | Nein |
> | volumes | Nein | Nein |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applications | Ja | Ja |
> | containergroups | Nein | Nein |
> | gateways | Ja | Ja |
> | locations | Nein | Nein |
> | locations/applicationoperations | Nein | Nein |
> | locations/gatewayoperations | Nein | Nein |
> | locations/networkoperations | Nein | Nein |
> | locations/secretoperations | Nein | Nein |
> | locations/volumeoperations | Nein | Nein |
> | networks | Ja | Ja |
> | Vorgänge | Nein | Nein |
> | secrets | Ja | Ja |
> | volumes | Ja | Ja |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | rollouts | Nein | Nein |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | locations/usages | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | signalr | Ja | Ja |
> | signalr/eventgridfilters | Nein | Nein |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hybridusebenefits | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applicationdefinitions | Nein | Nein |
> | applications | Nein | Nein |
> | jitrequests | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | instancepools | Nein | Nein |
> | locations | Ja | Ja |
> | locations/administratorazureasyncoperation | Nein | Nein |
> | locations/administratoroperationresults | Nein | Nein |
> | locations/auditingsettingsazureasyncoperation | Nein | Nein |
> | locations/auditingsettingsoperationresults | Nein | Nein |
> | locations/capabilities | Nein | Nein |
> | locations/databaseazureasyncoperation | Nein | Nein |
> | locations/databaseoperationresults | Nein | Nein |
> | locations/databaserestoreazureasyncoperation | Nein | Nein |
> | locations/deletevirtualnetworkorsubnets | Nein | Nein |
> | locations/deletevirtualnetworkorsubnetsazureasyncoperation | Nein | Nein |
> | locations/deletevirtualnetworkorsubnetsoperationresults | Nein | Nein |
> | locations/dnsaliasasyncoperation | Nein | Nein |
> | locations/dnsaliasoperationresults | Nein | Nein |
> | locations/elasticpoolazureasyncoperation | Nein | Nein |
> | locations/elasticpooloperationresults | Nein | Nein |
> | locations/encryptionprotectorazureasyncoperation | Nein | Nein |
> | locations/encryptionprotectoroperationresults | Nein | Nein |
> | locations/extendedauditingsettingsazureasyncoperation | Nein | Nein |
> | locations/extendedauditingsettingsoperationresults | Nein | Nein |
> | locations/failovergroupazureasyncoperation | Nein | Nein |
> | locations/failovergroupoperationresults | Nein | Nein |
> | locations/firewallrulesazureasyncoperation | Nein | Nein |
> | locations/firewallrulesoperationresults | Nein | Nein |
> | locations/instancefailovergroupazureasyncoperation | Nein | Nein |
> | locations/instancefailovergroupoperationresults | Nein | Nein |
> | locations/instancefailovergroups | Nein | Nein |
> | locations/instancepoolazureasyncoperation | Nein | Nein |
> | locations/instancepooloperationresults | Nein | Nein |
> | locations/jobagentazureasyncoperation | Nein | Nein |
> | locations/jobagentoperationresults | Nein | Nein |
> | locations/longtermretentionbackupazureasyncoperation | Nein | Nein |
> | locations/longtermretentionbackupoperationresults | Nein | Nein |
> | locations/longtermretentionbackups | Nein | Nein |
> | locations/longtermretentionmanagedinstancebackupazureasyncoperation | Nein | Nein |
> | locations/longtermretentionmanagedinstancebackupoperationresults | Nein | Nein |
> | locations/longtermretentionmanagedinstancebackups | Nein | Nein |
> | locations/longtermretentionmanagedinstances | Nein | Nein |
> | locations/longtermretentionpolicyazureasyncoperation | Nein | Nein |
> | locations/longtermretentionpolicyoperationresults | Nein | Nein |
> | locations/longtermretentionservers | Nein | Nein |
> | locations/manageddatabaseazureasyncoperation | Nein | Nein |
> | locations/manageddatabasecompleterestoreazureasyncoperation | Nein | Nein |
> | locations/manageddatabasecompleterestoreoperationresults | Nein | Nein |
> | locations/manageddatabaseoperationresults | Nein | Nein |
> | locations/manageddatabaserestoreazureasyncoperation | Nein | Nein |
> | locations/manageddatabaserestoreoperationresults | Nein | Nein |
> | locations/managedinstanceazureasyncoperation | Nein | Nein |
> | locations/managedinstanceencryptionprotectorazureasyncoperation | Nein | Nein |
> | locations/managedinstanceencryptionprotectoroperationresults | Nein | Nein |
> | locations/managedinstancekeyazureasyncoperation | Nein | Nein |
> | locations/managedinstancekeyoperationresults | Nein | Nein |
> | locations/managedinstancelongtermretentionpolicyazureasyncoperation | Nein | Nein |
> | locations/managedinstancelongtermretentionpolicyoperationresults | Nein | Nein |
> | locations/managedinstanceoperationresults | Nein | Nein |
> | locations/managedinstancetdecertazureasyncoperation | Nein | Nein |
> | locations/managedinstancetdecertoperationresults | Nein | Nein |
> | locations/managedserversecurityalertpoliciesazureasyncoperation | Nein | Nein |
> | locations/managedserversecurityalertpoliciesoperationresults | Nein | Nein |
> | locations/managedshorttermretentionpolicyazureasyncoperation | Nein | Nein |
> | locations/managedshorttermretentionpolicyoperationresults | Nein | Nein |
> | locations/notifyazureasyncoperation | Nein | Nein |
> | locations/privateendpointconnectionazureasyncoperation | Nein | Nein |
> | locations/privateendpointconnectionoperationresults | Nein | Nein |
> | locations/privateendpointconnectionproxyazureasyncoperation | Nein | Nein |
> | locations/privateendpointconnectionproxyoperationresults | Nein | Nein |
> | locations/securityalertpoliciesazureasyncoperation | Nein | Nein |
> | locations/securityalertpoliciesoperationresults | Nein | Nein |
> | locations/serveradministratorazureasyncoperation | Nein | Nein |
> | locations/serveradministratoroperationresults | Nein | Nein |
> | locations/serverazureasyncoperation | Nein | Nein |
> | locations/serverkeyazureasyncoperation | Nein | Nein |
> | locations/serverkeyoperationresults | Nein | Nein |
> | locations/serveroperationresults | Nein | Nein |
> | locations/shorttermretentionpolicyazureasyncoperation | Nein | Nein |
> | locations/shorttermretentionpolicyoperationresults | Nein | Nein |
> | locations/syncagentoperationresults | Nein | Nein |
> | locations/syncdatabaseids | Nein | Nein |
> | locations/syncgroupoperationresults | Nein | Nein |
> | locations/syncmemberoperationresults | Nein | Nein |
> | locations/tdecertazureasyncoperation | Nein | Nein |
> | locations/tdecertoperationresults | Nein | Nein |
> | locations/usages | Nein | Nein |
> | locations/virtualclusterazureasyncoperation | Nein | Nein |
> | locations/virtualclusteroperationresults | Nein | Nein |
> | locations/virtualnetworkrulesazureasyncoperation | Nein | Nein |
> | locations/virtualnetworkrulesoperationresults | Nein | Nein |
> | locations/vulnerabilityassessmentscanazureasyncoperation | Nein | Nein |
> | locations/vulnerabilityassessmentscanoperationresults | Nein | Nein |
> | managedinstances | Nein | Nein |
> | managedinstances/administrators | Nein | Nein |
> | managedinstances/databases | Nein | Nein |
> | managedinstances/databases/backuplongtermretentionpolicies | Nein | Nein |
> | managedinstances/databases/vulnerabilityassessments | Nein | Nein |
> | managedinstances/metricdefinitions | Nein | Nein |
> | managedinstances/metrics | Nein | Nein |
> | managedinstances/recoverabledatabases | Nein | Nein |
> | managedinstances/tdecertificates | Nein | Nein |
> | managedinstances/vulnerabilityassessments | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | servers | Ja | Ja |
> | servers/administratoroperationresults | Nein | Nein |
> | servers/administrators | Nein | Nein |
> | servers/advisors | Nein | Nein |
> | servers/aggregateddatabasemetrics | Nein | Nein |
> | servers/auditingpolicies | Nein | Nein |
> | servers/auditingsettings | Nein | Nein |
> | servers/automatictuning | Nein | Nein |
> | servers/communicationlinks | Nein | Nein |
> | servers/connectionpolicies | Nein | Nein |
> | servers/databases | Ja | Ja |
> | servers/databases/advisors | Nein | Nein |
> | servers/databases/auditingpolicies | Nein | Nein |
> | servers/databases/auditingsettings | Nein | Nein |
> | servers/databases/auditrecords | Nein | Nein |
> | servers/databases/automatictuning | Nein | Nein |
> | servers/databases/backuplongtermretentionpolicies | Nein | Nein |
> | servers/databases/backupshorttermretentionpolicies | Nein | Nein |
> | servers/databases/connectionpolicies | Nein | Nein |
> | servers/databases/datamaskingpolicies | Nein | Nein |
> | servers/databases/datamaskingpolicies/rules | Nein | Nein |
> | servers/databases/extensions | Nein | Nein |
> | servers/databases/geobackuppolicies | Nein | Nein |
> | servers/databases/metricdefinitions | Nein | Nein |
> | servers/databases/metrics | Nein | Nein |
> | servers/databases/recommendedsensitivitylabels | Nein | Nein |
> | servers/databases/securityalertpolicies | Nein | Nein |
> | servers/databases/syncgroups | Nein | Nein |
> | servers/databases/syncgroups/syncmembers | Nein | Nein |
> | servers/databases/topqueries | Nein | Nein |
> | servers/databases/topqueries/querytext | Nein | Nein |
> | servers/databases/transparentdataencryption | Nein | Nein |
> | servers/databases/vulnerabilityassessment | Nein | Nein |
> | servers/databases/vulnerabilityassessments | Nein | Nein |
> | servers/databases/vulnerabilityassessmentscans | Nein | Nein |
> | servers/databases/vulnerabilityassessmentsettings | Nein | Nein |
> | servers/databases/workloadgroups | Nein | Nein |
> | servers/databasesecuritypolicies | Nein | Nein |
> | servers/disasterrecoveryconfiguration | Nein | Nein |
> | servers/dnsaliases | Nein | Nein |
> | servers/elasticpoolestimates | Nein | Nein |
> | servers/elasticpools | Ja | Ja |
> | servers/elasticpools/advisors | Nein | Nein |
> | servers/elasticpools/metricdefinitions | Nein | Nein |
> | servers/elasticpools/metrics | Nein | Nein |
> | servers/encryptionprotector | Nein | Nein |
> | servers/extendedauditingsettings | Nein | Nein |
> | servers/failovergroups | Nein | Nein |
> | servers/import | Nein | Nein |
> | servers/importexportoperationresults | Nein | Nein |
> | servers/jobaccounts | Ja | Ja |
> | servers/jobagents | Ja | Ja |
> | servers/jobagents/jobs | Nein | Nein |
> | servers/jobagents/jobs/executions | Nein | Nein |
> | servers/jobagents/jobs/steps | Nein | Nein |
> | servers/keys | Nein | Nein |
> | servers/operationresults | Nein | Nein |
> | servers/recommendedelasticpools | Nein | Nein |
> | servers/recoverabledatabases | Nein | Nein |
> | servers/restorabledroppeddatabases | Nein | Nein |
> | servers/securityalertpolicies | Nein | Nein |
> | servers/serviceobjectives | Nein | Nein |
> | servers/syncagents | Nein | Nein |
> | servers/tdecertificates | Nein | Nein |
> | servers/usages | Nein | Nein |
> | servers/virtualnetworkrules | Nein | Nein |
> | servers/vulnerabilityassessments | Nein | Nein |
> | virtualclusters | Ja | Ja |

> [!IMPORTANT]
> Datenbank und Server müssen sich in der gleichen Ressourcengruppe befinden. Wenn Sie eine SQL Server-Instanz verschieben, werden auch alle ihre Datenbanken verschoben. Dieses Verhalten gilt für Azure SQL-Datenbank- und Azure Synapse Analytics-Datenbanken.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/availabilitygrouplisteneroperationresults | Nein | Nein |
> | locations/operationtypes | Nein | Nein |
> | locations/sqlvirtualmachinegroupoperationresults | Nein | Nein |
> | locations/sqlvirtualmachineoperationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | sqlvirtualmachinegroups | Ja | Ja |
> | sqlvirtualmachinegroups/availabilitygrouplisteners | Nein | Nein |
> | sqlvirtualmachines | Ja | Ja |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | locations | Nein | Nein |
> | locations/asyncoperations | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/deletevirtualnetworkorsubnets | Nein | Nein |
> | locations/usages | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | storageaccounts | Ja | Ja |
> | storageaccounts/blobservices | Nein | Nein |
> | storageaccounts/fileservices | Nein | Nein |
> | storageaccounts/listaccountsas | Nein | Nein |
> | storageaccounts/listservicesas | Nein | Nein |
> | storageaccounts/queueservices | Nein | Nein |
> | storageaccounts/services | Nein | Nein |
> | storageaccounts/services/metricdefinitions | Nein | Nein |
> | storageaccounts/tableservices | Nein | Nein |
> | usages | Nein | Nein |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | caches | Nein | Nein |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/checknameavailability | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operations | Nein | Nein |
> | locations/workflows | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | storagesyncservices | Ja | Ja |
> | storagesyncservices/registeredservers | Nein | Nein |
> | storagesyncservices/syncgroups | Nein | Nein |
> | storagesyncservices/syncgroups/cloudendpoints | Nein | Nein |
> | storagesyncservices/syncgroups/serverendpoints | Nein | Nein |
> | storagesyncservices/workflows | Nein | Nein |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Nein | Nein |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Nein | Nein |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | managers | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | Nein | Nein |
> | locations | Nein | Nein |
> | locations/quotas | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | streamingjobs | Ja | Ja |

> [!IMPORTANT]
> Stream Analytics-Aufträge können nicht verschoben werden, während sie ausgeführt werden.

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | environments | Nein | Nein |
> | environments/eventsources | Nein | Nein |
> | instances | Nein | Nein |
> | instances/environments | Nein | Nein |
> | instances/environments/eventsources | Nein | Nein |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | cancel | Nein | Nein |
> | createsubscription | Nein | Nein |
> | enable | Nein | Nein |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | rename | Nein | Nein |
> | subscriptiondefinitions | Nein | Nein |
> | subscriptionoperations | Nein | Nein |
> | subscriptions | Nein | Nein |

## <a name="microsoftsupport"></a>microsoft.support

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | operationresults | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | operationsstatus | Nein | Nein |
> | services | Nein | Nein |
> | services/problemclassifications | Nein | Nein |
> | supporttickets | Nein | Nein |

## <a name="microsoftsynapse"></a>Microsoft.Synapse

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | workspaces | Ja | Ja |
> | workspaces/bigdatapools | Ja | Ja |
> | workspaces/operationresults | Nein | Nein |
> | workspaces/operationstatuses | Nein | Nein |
> | workspaces/sqlpools | Ja | Ja |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | environments | Ja | Ja |
> | environments/accesspolicies | Nein | Nein |
> | environments/eventsources | Ja | Ja |
> | environments/referencedatasets | Ja | Ja |
> | Vorgänge | Nein | Nein |

## <a name="microsofttoken"></a>Microsoft.Token

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | stores | Ja | Ja |
> | stores/accesspolicies | Nein | Nein |
> | stores/services | Nein | Nein |
> | stores/services/tokens | Nein | Nein |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | imagetemplates | Nein | Nein |
> | imagetemplates/runoutputs | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operations | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | account | Nein | Nein |
> | account/extension | Nein | Nein |
> | account/project | Nein | Nein |
> | checknameavailability | Nein | Nein |
> | Vorgänge | Nein | Nein |

> [!IMPORTANT]
> Informationen zum Ändern des Abonnements für Azure DevOps finden Sie unter [Change or remove the Azure subscription that your organization uses for billing](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json) (Ändern oder Entfernen des Azure-Abonnements, das von Ihrer Organisation für die Abrechnung verwendet wird).

## <a name="microsoftvmware"></a>Microsoft.VMware

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | arczones | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | resourcepools | Nein | Nein |
> | vcenters | Nein | Nein |
> | virtualmachines | Nein | Nein |
> | virtualmachinetemplates | Nein | Nein |
> | virtualnetworks | Nein | Nein |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | dedicatedcloudnodes | Nein | Nein |
> | dedicatedcloudservices | Nein | Nein |
> | locations | Nein | Nein |
> | locations/availabilities | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/privateclouds | Nein | Nein |
> | locations/privateclouds/resourcepools | Nein | Nein |
> | locations/privateclouds/virtualmachinetemplates | Nein | Nein |
> | locations/privateclouds/virtualnetworks | Nein | Nein |
> | locations/usages | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | virtualmachines | Nein | Nein |

## <a name="microsoftvnfmanager"></a>Microsoft.VnfManager

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | devices | Nein | Nein |
> | locations | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | vnfs | Nein | Nein |

## <a name="microsoftvsonline"></a>Microsoft.VSOnline

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | accounts | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | plans | Nein | Nein |
> | registeredsubscriptions | Nein | Nein |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availablestacks | Nein | Nein |
> | billingmeters | Nein | Nein |
> | certificates | Nein | Ja |
> | checknameavailability | Nein | Nein |
> | connectiongateways | Ja | Ja |
> | connections | Ja | Ja |
> | customapis | Ja | Ja |
> | deletedsites | Nein | Nein |
> | deploymentlocations | Nein | Nein |
> | georegions | Nein | Nein |
> | hostingenvironments | Nein | Nein |
> | hostingenvironments/eventgridfilters | Nein | Nein |
> | hostingenvironments/multirolepools | Nein | Nein |
> | hostingenvironments/workerpools | Nein | Nein |
> | ishostingenvironmentnameavailable | Nein | Nein |
> | ishostnameavailable | Nein | Nein |
> | isusernameavailable | Nein | Nein |
> | kubeenvironments | Ja | Ja |
> | listsitesassignedtohostname | Nein | Nein |
> | locations | Nein | Nein |
> | locations/apioperations | Nein | Nein |
> | locations/connectiongatewayinstallations | Nein | Nein |
> | locations/deletedsites | Nein | Nein |
> | locations/deletevirtualnetworkorsubnets | Nein | Nein |
> | locations/extractapidefinitionfromwsdl | Nein | Nein |
> | locations/getnetworkpolicies | Nein | Nein |
> | locations/listwsdlinterfaces | Nein | Nein |
> | locations/managedapis | Nein | Nein |
> | locations/operationresults | Nein | Nein |
> | locations/operations | Nein | Nein |
> | locations/runtimes | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | publishingusers | Nein | Nein |
> | empfehlungen | Nein | Nein |
> | resourcehealthmetadata | Nein | Nein |
> | runtimes | Nein | Nein |
> | serverfarms | Ja | Ja |
> | serverfarms/eventgridfilters | Nein | Nein |
> | sites | Ja | Ja |
> | sites/eventgridfilters | Nein | Nein |
> | sites/hostnamebindings | Nein | Nein |
> | sites/networkconfig | Nein | Nein |
> | sites/premieraddons | Ja | Ja |
> | sites/slots | Ja | Ja |
> | sites/slots/eventgridfilters | Nein | Nein |
> | sites/slots/hostnamebindings | Nein | Nein |
> | sites/slots/networkconfig | Nein | Nein |
> | sourcecontrols | Nein | Nein |
> | staticsites | Nein | Nein |
> | validate | Nein | Nein |
> | verifyhostingenvironmentvnet | Nein | Nein |

> [!IMPORTANT]
> Weitere Informationen finden Sie unter [Move guidance for App Service resources](./move-limitations/app-service-move-limitations.md) (Anleitung zum Verschieben von App Service-Ressourcen).

## <a name="microsoftwindowsesu"></a>Microsoft.WindowsESU

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | multipleactivationkeys | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | deviceservices | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="microsoftworkloadbuilder"></a>Microsoft.WorkloadBuilder

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | locations | Nein | Nein |
> | locations/operationstatuses | Nein | Nein |
> | Vorgänge | Nein | Nein |
> | workloads | Nein | Nein |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | components | Nein | Nein |
> | componentssummary | Nein | Nein |
> | monitorinstances | Nein | Nein |
> | monitorinstancessummary | Nein | Nein |
> | monitors | Nein | Nein |
> | notificationsettings | Nein | Nein |
> | Vorgänge | Nein | Nein |

## <a name="third-party-services"></a>Dienste von Drittanbietern

Derzeit wird der Verschiebevorgang nicht für Drittanbieterdienste unterstützt.

## <a name="next-steps"></a>Nächste Schritte
Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](move-resource-group-and-subscription.md).

Um die Daten als Datei mit durch Trennzeichen getrennten Werten abzurufen, laden Sie [move-support-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv) herunter.
