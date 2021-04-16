---
title: Überschreiben von SKU-Informationen über CSCFG/CSDEF für Azure Cloud Services (erweiterter Support)
description: Überschreiben von SKU-Informationen über CSCFG/CSDEF für Azure Cloud Services (erweiterter Support)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: surbhijain
ms.author: surbhijain
ms.reviewer: gachandw
ms.date: 04/05/2021
ms.custom: ''
ms.openlocfilehash: 17e47b562c52ffce631a01cf03004d77053ea647
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106387153"
---
# <a name="override-sku-information-over-cscfgcsdef-in-cloud-services-extended-support"></a>Überschreiben von SKU-Informationen über CSCFG/CSDEF in Cloud Services (erweiterter Support) 

Dieses Feature ermöglicht es Benutzern, die Rollengröße und die Anzahl der Instanzen in ihrem Clouddienst mithilfe der Eigenschaft **allowModelOverride** zu aktualisieren, ohne die Dienstkonfigurations- und Dienstdefinitionsdateien zu aktualisieren. Dadurch kann der Clouddienst hoch-, herunter-, ab- oder aufskaliert werden, ohne neu paketiert und neu bereitgestellt werden zu müssen.

## <a name="set-allowmodeloverride-property"></a>Festlegen der Eigenschaft „allowModelOverride“
Die Eigenschaft „allowModelOverride“ kann wie folgt festgelegt werden:
* Bei „allowModelOverride = true“ werden durch den API-Aufruf die Rollengröße und die Anzahl der Instanzen für den Clouddienst aktualisiert, ohne die Werte anhand der CSDEF- und CSCFG-Dateien zu überprüfen. 
> [!Note]
> CSCFG wird aktualisiert, um die Anzahl von Rolleninstanzen widerzuspiegeln, aber in CSDEF (innerhalb von CSPKG) bleiben die alten Werte erhalten.
* Bei „allowModelOverride = false“ wird durch den API-Aufruf ein Fehler ausgelöst, wenn die Werte für Rollengröße und Anzahl der Instanzen nicht mit den CSDEF- und CSCFG-Dateien übereinstimmen.

Standardmäßig ist der Wert auf „false“ festgelegt. Wenn die Eigenschaft von „true“ wieder auf „false“ zurückgesetzt wird, werden die CSDEF-und CSCFG-Dateien zur Validierung erneut überprüft.

Gehen Sie die folgenden Beispiele durch, um die Eigenschaft in PowerShell, in der Vorlage und im SDK anzuwenden.

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage
Wenn die Eigenschaft „allowModelOverride“ hier auf „true“ festgelegt wird, wird der Clouddienst mit den im Abschnitt „roleProfile“ definierten Rolleneigenschaften aktualisiert.
```json
"properties": {
        "packageUrl": "[parameters('packageSasUri')]",
        "configurationUrl": "[parameters('configurationSasUri')]",
        "upgradeMode": "[parameters('upgradeMode')]",
        “**allowModelOverride**” : true,
        "roleProfile": {
          "roles": [
            {
              "name": "WebRole1",
              "sku": {
                "name": "Standard_D1_v2",
                "capacity": "1"
              }
            },
            {
              "name": "WorkerRole1",
              "sku": {
                "name": "Standard_D1_v2",
                "capacity": "1"
              }
            }
          ]
        },

```
### <a name="powershell"></a>PowerShell
Wenn Sie den Switch „AllowModelOverride“ für das neue Cmdlet „New-AzCloudService“ festlegen, wird der Clouddienst mit den im Rollenprofil definierten SKU-Eigenschaften aktualisiert.
```powershell
New-AzCloudService ` 
-Name “ContosoCS” ` 
-ResourceGroupName “ContosOrg” ` 
-Location “East US” `
-AllowModelOverride  ` 
-PackageUrl $cspkgUrl ` 
-ConfigurationUrl $cscfgUrl ` 
-UpgradeMode 'Auto' ` 
-RoleProfile $roleProfile ` 
-NetworkProfile $networkProfile  ` 
-ExtensionProfile $extensionProfile ` 
-OSProfile $osProfile `
-Tag $tag
```
### <a name="sdk"></a>SDK
Wenn Sie die Variable „AllowModelOverride= true“ festlegen, wird der Clouddienst mit den im Rollenprofil definierten SKU-Eigenschaften aktualisiert.

```csharp
CloudService cloudService = new CloudService
    {
        Properties = new CloudServiceProperties
            {
                RoleProfile = cloudServiceRoleProfile
                Configuration = < Add Cscfg xml content here>
                PackageUrl = <Add cspkg SAS url here>,
                ExtensionProfile = cloudServiceExtensionProfile,
                OsProfile= cloudServiceOsProfile,
                NetworkProfile = cloudServiceNetworkProfile,
                UpgradeMode = 'Auto',
                AllowModelOverride = true
            },
                Location = m_location
            };
CloudService createOrUpdateResponse = m_CrpClient.CloudServices.CreateOrUpdate(“ContosOrg”, “ContosoCS”, cloudService);
```
### <a name="azure-portal"></a>Azure-Portal
Das Portal lässt die Überschreibung der Rollengröße und der Anzahl der Instanzen in CSDEF und CSCFG durch die oben genannte Eigenschaft nicht zu. 


## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
