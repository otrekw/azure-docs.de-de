---
title: Rollover für ein Azure Service Fabric-Clusterzertifikat
description: Erfahren Sie, wie Sie ein Rollover für ein Service Fabric-Clusterzertifikat ausführen, das anhand seines allgemeinen Zertifikatsnamens identifiziert wird.
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: 7a5fe2a7f2a05295605ef0e1d5db321a83b96712
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611907"
---
# <a name="manually-roll-over-a-service-fabric-cluster-certificate"></a>Manuelles Rollover für ein Service Fabric-Clusterzertifikat
Wenn ein Service Fabric-Clusterzertifikat in Kürze abläuft, müssen Sie das Zertifikat aktualisieren.  Ein Zertifikatrollover ist einfach, wenn der Cluster [für die Verwendung von Zertifikaten auf Basis des allgemeinen Namens konfiguriert wurde](service-fabric-cluster-change-cert-thumbprint-to-cn.md) (anstelle eines Fingerabdrucks).  Fordern Sie ein neues Zertifikat von einer Zertifizierungsstelle mit einem neuen Ablaufdatum an.  Selbstsignierte Zertifikate werden für Service Fabric-Produktionscluster nicht unterstützt, damit Zertifikate berücksichtigt werden, die während des Clustererstellungsworkflows im Azure-Portal generiert werden. Das neue Zertifikat muss den gleichen allgemeinen Name wie das ältere Zertifikat haben. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Der Service Fabric-Cluster verwendet das deklarierte Zertifikat automatisch mit einem weiter in der Zukunft liegenden Ablaufdatum, wenn mehrere Überprüfungszertifikate auf dem Host installiert sind. Eine bewährte Methode ist die Verwendung einer Resource Manager-Vorlage zum Bereitstellen von Azure-Ressourcen. Für eine Nicht-Produktionsumgebung kann das folgende Skript verwendet werden, um ein neues Zertifikat in einen Schlüsseltresor hochzuladen und das Zertifikat dann für die VM-Skalierungsgruppe zu installieren: 

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  <subscription ID>

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "keyvaultgroup"
$VaultName = "cntestvault2"
$certFilename = "C:\users\sfuser\sftutorialcluster20180419110824.pfx"
$certname = "cntestcert"
$Password  = "!P@ssw0rd321"
$VmssResourceGroupName     = "sfclustertutorialgroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Get the key vault.  The key vault must be enabled for deployment.
$keyVault = Get-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName 
$resourceId = $keyVault.ResourceId  

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzKeyVaultCertificate -VaultName $vaultName -Name $certName  -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    

Set-StrictMode -Version 3
$ErrorActionPreference = "Stop"

$certConfig = New-AzVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)

# Update the VM scale set 
Update-AzVmss -ResourceGroupName $VmssResourceGroupName -Name $VmssName -VirtualMachineScaleSet $vmss  -Verbose
```

>[!NOTE]
> Computegeheimnisse von VM-Skalierungsgruppen unterstützen nicht die gleiche Ressourcen-ID für zwei separate Geheimnisse, da jedes Geheimnis eine eindeutige Ressource mit Versionsverwaltung ist. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Clustersicherheit](service-fabric-cluster-security.md).
* [Aktualisieren und Verwalten von Clusterzertifikaten](service-fabric-cluster-security-update-certs-azure.md)
