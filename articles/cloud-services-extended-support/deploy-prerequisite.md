---
title: Voraussetzungen für die Bereitstellung von Azure Cloud Services (erweiterter Support)
description: Voraussetzungen für die Bereitstellung von Azure Cloud Services (erweiterter Support)
ms.topic: article
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 1473305d7da57d1216ef05c0b88a0f69d586784b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101728109"
---
# <a name="prerequisites-for-deploying-azure-cloud-services-extended-support"></a>Voraussetzungen für die Bereitstellung von Azure Cloud Services (erweiterter Support)

> [!IMPORTANT]
> Cloud Services (erweiterter Support) befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Um eine erfolgreiche Bereitstellung von Cloud Services (erweiterter Support) zu gewährleisten, führen Sie die folgenden Schritte durch und schließen jeden Punkt ab, bevor Sie mit der Bereitstellung beginnen. 

## <a name="register-the-cloudservices-feature"></a>Registrieren des CloudServices-Features
Registrieren Sie das Features für Ihr Abonnement. Die Registrierung kann mehrere Minuten dauern. 

```powershell
Register-AzProviderFeature -FeatureName CloudServices -ProviderNamespace Microsoft.Compute
```

Überprüfen Sie den Status der Registrierung wie folgt:  
```powershell
Get-AzProviderFeature 

#Sample output
FeatureName               ProviderName      RegistrationState
CloudServices           Microsoft.Compute    Registered
```

## <a name="required-service-configuration-cscfg-file-updates"></a>Erforderliche Aktualisierungen der Dienstkonfigurationsdatei (CSCFG-Datei)

### <a name="1-virtual-network"></a>1) Virtuelles Netzwerk
Bereitstellungen von Cloud Services (erweiterter Support) müssen sich in einem virtuellen Netzwerk befinden. Das virtuelle Netzwerk kann über das [Azure-Portal](../virtual-network/quick-create-portal.md), mit [PowerShell](../virtual-network/quick-create-powershell.md), der [Azure CLI](../virtual-network/quick-create-cli.md) oder einer [ARM-Vorlage](../virtual-network/quick-create-template.md) erstellt werden. Auf das virtuelle Netzwerk und die Subnetze muss außerdem in der Dienstkonfiguration (CSCFG-Datei) im Abschnitt [NetworkConfiguration](schema-cscfg-networkconfiguration.md) verwiesen werden. 

Bei virtuellen Netzwerken, die derselben Ressourcengruppe wie der Clouddienst angehören, genügt das Verweisen auf den Namen des virtuellen Netzwerks in der Dienstkonfigurationsdatei (CSCFG-Datei). Wenn sich das virtuelle Netzwerk und der Clouddienst in zwei unterschiedlichen Ressourcengruppen befinden, muss die vollständige Azure Resource Manager-ID des virtuellen Netzwerks in der Dienstkonfigurationsdatei (CSCFG-Datei) angegeben werden.
 
#### <a name="virtual-network-located-in-same-resource-group"></a>Virtuelles Netzwerk in derselben Ressourcengruppe
```xml
<VirtualNetworkSite name="<vnet-name>"/> 
  <AddressAssignments> 
    <InstanceAddress roleName="<role-name>"> 
     <Subnets> 
       <Subnet name="<subnet-name>"/> 
     </Subnets> 
    </InstanceAddress> 
```

#### <a name="virtual-network-located-in-different-resource-group"></a>Virtuelles Netzwerk in einer anderen Ressourcengruppe
```xml
<VirtualNetworkSite name="/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>"/> 
   <AddressAssignments> 
     <InstanceAddress roleName="<role-name>"> 
       <Subnets> 
        <Subnet name="<subnet-name>"/> 
       </Subnets> 
     </InstanceAddress> 
```
### <a name="2-remove-the-old-plugins"></a>2) Entfernen der alten Plug-Ins

Entfernen Sie alte Remotedesktopeinstellungen aus der Dienstkonfigurationsdatei (CSCFG-Datei).  

```xml
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="gachandw" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="XXXX" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="2021-12-17T23:59:59.0000000+05:30" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" /> 
```

## <a name="required-service-definition-file-csdef-updates"></a>Erforderliche Aktualisierungen der Dienstdefinitionsdatei (CSDEF-Datei)

### <a name="1-virtual-machine-sizes"></a>1) VM-Größen
Die folgenden Größen sind in Azure Resource Manager veraltet. Wenn Sie diese jedoch weiterhin verwenden möchten, aktualisieren Sie den `vmsize`-Namen mit der zugeordneten Azure Resource Manager-Benennungskonvention.  

| Vorheriger Name der Größe | Aktualisierter Name der Größe | 
|---|---|
| Sehr klein | Standard_A0 | 
| Small | Standard_A1 |
| Medium | Standard_A2 | 
| Large | Standard_A3 | 
| Extragroß | Standard_A4 | 
| A5 | Standard_A5 | 
| A6 | Standard_A6 | 
| A7 | Standard_A7 |  
| A8 | Standard_A8 | 
| A9 | Standard_A9 |
| A10 | Standard_A10 | 
| A11 | Standard_A11 | 
| MSODSG5 | Standard_MSODSG5 | 

 Beispielsweise wird `<WorkerRole name="WorkerRole1" vmsize="Medium"` zu `<WorkerRole name="WorkerRole1" vmsize="Standard_A2"`.
 
> [!NOTE]
> Informationen zum Abrufen einer Liste der verfügbaren Größen finden Sie unter [Ressourcen-SKUs: Liste](/rest/api/compute/resourceskus/list). Wenden Sie die folgenden Filter an: <br>
`ResourceType = virtualMachines ` <br>
`VMDeploymentTypes = PaaS `


### <a name="2-remove-old-remote-desktop-plugins"></a>2) Entfernen alter Remotedesktop-Plug-Ins
In Bereitstellungen, die die alten Remotedesktop-Plug-Ins genutzt haben, müssen die Module aus der Dienstdefinitionsdatei (CSDEF-Datei) und allen zugehörigen Zertifikaten entfernt werden. 

```xml
<Imports> 
<Import moduleName="RemoteAccess" /> 
<Import moduleName="RemoteForwarder" /> 
</Imports> 
```

## <a name="key-vault-creation"></a>Erstellen einer Key Vault-Instanz 

Key Vault wird zum Speichern von Zertifikaten verwendet, die Cloud Services (erweiterter Support) zugeordnet sind. Fügen Sie Key Vault die Zertifikate hinzu, und verweisen Sie dann in der Dienstkonfigurationsdatei auf die Zertifikatfingerabdrücke. Außerdem müssen Sie Key Vault für entsprechende Berechtigungen aktivieren, damit Cloud Services (erweiterter Support) als Geheimnisse gespeicherte Zertifikate aus Key Vault abrufen kann. Key Vault kann über das [Azure-Portal](../key-vault/general/quick-create-portal.md) und mit [PowerShell](../key-vault/general/quick-create-powershell.md) erstellt werden. Key Vault muss in derselben Region und im selben Abonnement wie der Clouddienst erstellt werden. Weitere Informationen finden Sie unter [Verwenden von Zertifikaten mit Azure Cloud Services (erweiterter Support)](certificates-and-key-vault.md).

## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Stellen Sie eine Cloud Service-Instanz (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
- Besuchen Sie das [Beispielrepository zu Cloud Services (erweiterter Support)](https://github.com/Azure-Samples/cloud-services-extended-support).