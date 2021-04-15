---
title: Anwenden der Windows Azure Diagnoseerweiterung in Cloud Services (erweiterter Support)
description: Anwenden der Windows Azure Diagnoseerweiterung für Cloud Services (erweiterter Support)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 14b1661792ca5276bd6ebfa4cee1c4b46f94764d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104780445"
---
# <a name="apply-the-windows-azure-diagnostics-extension-in-cloud-services-extended-support"></a>Anwenden der Windows Azure Diagnoseerweiterung in Cloud Services (erweiterter Support) 
Sie können wichtige Leistungsmetriken für beliebige Clouddienste überwachen. Für jede Clouddienstrolle werden minimale Daten zur CPU-Nutzung, Netzwerkverwendung und Datenträgerverwendung erfasst. Wenn auf eine Rolle des Clouddiensts die Erweiterung Microsoft.Azure.Diagnostics angewendet wurde, können für diese Rolle zusätzliche Datenpunkte erfasst werden. Weitere Informationen finden Sie unter [Erweiterungen: Übersicht](extensions.md).

Die Windows Azure-Diagnoseerweiterung kann für Cloud Services (erweiterter Support) über [PowerShell](deploy-powershell.md) oder eine [ARM-Vorlage](deploy-template.md) aktiviert werden.

## <a name="apply-windows-azure-diagnostics-extension-using-powershell"></a>Anwenden der Windows Azure-Diagnoseerweiterung mit PowerShell

```powershell
# Create WAD extension object
$storageAccountKey = Get-AzStorageAccountKey -ResourceGroupName "ContosOrg" -Name "contosostorageaccount"
$configFilePath = "<Insert WAD public configuration file path>"
$wadExtension = New-AzCloudServiceDiagnosticsExtension -Name "WADExtension" -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -StorageAccountName "contosostorageaccount" -StorageAccountKey $storageAccountKey[0].Value -DiagnosticsConfigurationPath $configFilePath -TypeHandlerVersion "1.5" -AutoUpgradeMinorVersion $true 

# Add <privateConfig> settings
$wadExtension.ProtectedSetting = "<Insert WAD Private Configuration as raw string here>"

# Get existing Cloud Service
$cloudService = Get-AzCloudService -ResourceGroup "ContosOrg" -CloudServiceName "ContosoCS"

# Add WAD extension to existing Cloud Service extension object
$cloudService.ExtensionProfile.Extension = $cloudService.ExtensionProfile.Extension + $wadExtension

# Update Cloud Service
$cloudService | Update-AzCloudService
```
Laden Sie die Schemadefinition für die öffentliche Konfigurationsdatei mit dem folgenden PowerShell-Befehl herunter:

```powershell
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'PublicWadConfig.xsd'
```
Hier sehen Sie ein Beispiel für die öffentliche XML-Konfigurationsdatei:
```
<?xml version="1.0" encoding="utf-8"?>
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <WadCfg>
    <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
      <PerformanceCounters scheduledTransferPeriod="PT1M">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
        <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
      </PerformanceCounters>
      <EtwProviders>
        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
          <Event id="1" eventDestination="EnumsTable"/>
          <DefaultEvents eventDestination="DefaultTable" />
        </EtwEventSourceProviderConfiguration>
      </EtwProviders>
    </DiagnosticMonitorConfiguration>
  </WadCfg>
</PublicConfig>
```
Laden Sie die Schemadefinition für die private Konfigurationsdatei mit dem folgenden PowerShell-Befehl herunter:

```powershell
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PrivateConfigurationSchema | Out-File -Encoding utf8 -FilePath 'PrivateWadConfig.xsd'
```
Hier sehen Sie ein Beispiel für die private XML-Konfigurationsdatei:

```
<?xml version="1.0" encoding="utf-8"?>
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="string" key="string" />
  <AzureMonitorAccount>
    <ServicePrincipalMeta>
      <PrincipalId>string</PrincipalId>
      <Secret>string</Secret>
    </ServicePrincipalMeta>
  </AzureMonitorAccount>
  <SecondaryStorageAccounts>
    <StorageAccount name="string" />
  </SecondaryStorageAccounts>
  <SecondaryEventHubs>
    <EventHub Url="string" SharedAccessKeyName="string" SharedAccessKey="string" />
  </SecondaryEventHubs>
</PrivateConfig>
```

## <a name="apply-windows-azure-diagnostics-extension-using-arm-template"></a>Anwenden der Windows Azure-Diagnoseerweiterung mit einer ARM-Vorlage
```json
"extensionProfile": { 
          "extensions": [ 
            { 
              "name": "Microsoft.Insights.VMDiagnosticsSettings_WebRole1", 
              "properties": { 
                "autoUpgradeMinorVersion": true, 
                "publisher": "Microsoft.Azure.Diagnostics", 
                "type": "PaaSDiagnostics", 
                "typeHandlerVersion": "1.5", 
                "settings": "Include PublicConfig XML as a raw string", 
                "protectedSettings": "Include PrivateConfig XML as a raw string”", 
                "rolesAppliedTo": [ 
                  "WebRole1" 
                ] 
              } 
            }
          ]
        },

```


## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
- Stellen Sie eine Cloud Service-Instanz (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.
