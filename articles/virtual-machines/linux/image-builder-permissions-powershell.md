---
title: Konfigurieren von Berechtigungen für den Azure VM Image Builder-Dienst mithilfe von PowerShell
description: Konfigurieren von Anforderungen für den Azure VM Image Builder-Dienst einschließlich Berechtigungen und Rechten mithilfe von PowerShell
author: danielsollondon
ms.author: danis
ms.date: 03/02/2021
ms.topic: article
ms.service: virtual-machines
ms.subservice: image-builder
ms.collection: linux
ms.openlocfilehash: 4b9cf3ffdb1fc6db9604098e8e5782317a8eb431
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101695395"
---
# <a name="configure-azure-image-builder-service-permissions-using-powershell"></a>Konfigurieren von Berechtigungen für den Azure VM Image Builder-Dienst mithilfe von PowerShell

Für den Azure VM Image Builder-Dienst müssen Berechtigungen und Rechte konfiguriert werden, bevor ein Image erstellt wird. In den folgenden Abschnitten erfahren Sie, wie mögliche Szenarios mithilfe von PowerShell konfiguriert werden.

> [!IMPORTANT]
> Azure Image Builder ist derzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="register-the-features"></a>Registrieren des Features

Sie müssen sich zunächst für den Azure VM Image Builder-Dienst registrieren. Die Registrierung gewährt Ihnen die Dienstberechtigungen zum Erstellen, Verwalten und Löschen einer Ressourcengruppe für das Staging. Der Dienst verfügt auch über die Berechtigung, der Gruppe Ressourcen hinzuzufügen, die für das Erstellen des Images erforderlich sind.

```powershell-interactive
Register-AzProviderFeature -FeatureName VirtualMachineTemplatePreview -ProviderNamespace Microsoft.VirtualMachineImages
```

## <a name="create-an-azure-user-assigned-managed-identity"></a>Erstellen einer in Azure benutzerseitig zugewiesenen verwalteten Identität

Im Azure VM Image Builder-Dienst müssen Sie eine [in Azure benutzerseitig zugewiesene verwaltete Identität](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) erstellen. Der Azure VM Image Builder-Dienst verwendet die benutzerseitig zugewiesene verwaltete Identität, um Images zu lesen, Images zu schreiben und auf Speicherkonten in Azure zuzugreifen. Sie gewähren der Identität die Berechtigung, bestimmte Aktionen für Ihr Abonnement ausführen zu dürfen.

> [!NOTE]
> Bisher hat der Azure VM Image Builder-Dienst die Dienstprinzipalnamen (Service Principal Name, SPN) des Azure VM Image Builder-Diensts verwendet, um Imageressourcengruppen Berechtigungen zu gewähren. Die Verwendung des SPN wird eingestellt. Verwenden Sie stattdessen eine benutzerseitig zugewiesene verwaltete Identität.

Im folgenden Beispiel sehen Sie, wie Sie eine in Azure benutzerseitig zugewiesene verwaltete Identität erstellen. Ersetzen Sie die Platzhaltereinstellungen, um Ihre Variablen festzulegen.

| Einstellung | BESCHREIBUNG |
|---------|-------------|
| \<Resource group\> | Hierbei handelt es sich um die Ressourcengruppe, in der die benutzerseitig verwaltete Identität erstellt werden soll. |

```powershell-interactive
## Add AZ PS module to support AzUserAssignedIdentity
Install-Module -Name Az.ManagedServiceIdentity

$parameters = @{
    Name = 'aibIdentity'
    ResourceGroupName = '<Resource group>'
}
# create identity
New-AzUserAssignedIdentity @parameters
```

Weitere Informationen zu in Azure benutzerseitig zugewiesenen Identitäten finden Sie in der Dokumentation [Erstellen, Auflisten oder Löschen einer vom Benutzer zugewiesenen verwalteten Identität mithilfe der Azure-Befehlszeilenschnittstelle](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) im Abschnitt zum Erstellen einer Identität.

## <a name="allow-image-builder-to-distribute-images"></a>Berechtigungen für den Azure VM Image Builder-Dienst zum Verteilen von Images

Damit der Azure VM Image Builder-Dienst Images verteilen kann (Verwaltete Images/Shared Image Gallery), muss der Azure VM Image Builder-Dienst die Berechtigung haben, die Images in diese Ressourcengruppen einzufügen. Zum Gewähren der erforderlichen Berechtigungen müssen Sie eine benutzerseitig zugewiesene verwaltete Identität erstellen und ihr Rechte für die Ressourcengruppe erteilen, in der das Image erstellt wird. Der Azure VM Image Builder-Dienst hat **keine** Berechtigung, auf Ressourcen in anderen Ressourcengruppen des Abonnements zuzugreifen. Sie müssen explizite Aktionen ausführen, um den Zugriff zu gestatten, damit für Ihre Builds keine Fehler auftreten.

Sie müssen der benutzerseitig zugewiesenen verwalteten Identität keine Berechtigungen eines Mitwirkenden für die Ressourcengruppe zuweisen, damit Images verteilt werden können. Die benutzerseitig zugewiesene verwaltete Identität benötigt jedoch die folgenden `Actions`-Berechtigungen in Azure in der Verteilungsressourcengruppe:

```Actions
Microsoft.Compute/images/write
Microsoft.Compute/images/read
Microsoft.Compute/images/delete
```

Für die Verteilung an eine Shared Image Gallery-Instanz benötigen Sie außerdem Folgendes:

```Actions
Microsoft.Compute/galleries/read
Microsoft.Compute/galleries/images/read
Microsoft.Compute/galleries/images/versions/read
Microsoft.Compute/galleries/images/versions/write
```

## <a name="permission-to-customize-existing-images"></a>Berechtigung zum Anpassen vorhandener Images

Damit der Azure VM Image Builder-Dienst Images aus benutzerdefinierten Quellimages erstellen kann (Verwaltete Images/Image Gallery), muss der Azure VM Image Builder-Dienst die Berechtigung haben, die Images in diesen Ressourcengruppen zu lesen. Zum Gewähren der erforderlichen Berechtigungen müssen Sie eine benutzerseitig zugewiesene verwaltete Identität erstellen und ihr Rechte für die Ressourcengruppe erteilen, in der sich das Image befindet.

So erstellen Sie ein Image aus einem vorhandenen benutzerdefinierten Image:

```Actions
Microsoft.Compute/galleries/read
```

So erstellen Sie ein Image aus einer vorhandenen Shared Image Gallery-Version:

```Actions
Microsoft.Compute/galleries/read
Microsoft.Compute/galleries/images/read
Microsoft.Compute/galleries/images/versions/read
```

## <a name="permission-to-customize-images-on-your-vnets"></a>Berechtigung zum Anpassen von Images in Ihren VNETs

Der Azure VM Image Builder-Dienst hat die Funktion, ein vorhandenes VNET in Ihrem Abonnement bereitzustellen und zu verwenden. Insofern ist der Zugriff für Anpassungen für verknüpfte Ressourcen möglich.

Sie müssen der benutzerseitig zugewiesenen verwalteten Identität keine Berechtigungen eines Mitwirkenden für die Ressourcengruppe zuweisen, damit eine VM für ein vorhandenes VNET bereitgestellt werden kann. Die benutzerseitig zugewiesene verwaltete Identität benötigt jedoch die folgenden `Actions`-Berechtigungen in Azure für die VNET-Ressourcengruppe:

```Actions
Microsoft.Network/virtualNetworks/read
Microsoft.Network/virtualNetworks/subnets/join/action
```

## <a name="create-an-azure-role-definition"></a>Erstellen einer Rollendefinition in Azure

Die folgenden Beispiele erstellen eine Rollendefinition in Azure auf Grundlage der Aktionen, die in den vorherigen Abschnitten beschrieben wurden. Diese Beispiele werden auf Ebene der Ressourcengruppe angewendet. Bewerten und testen Sie, ob die Granularität der Beispiele ausreichend für Ihre Anforderungen ist. Für Ihr Szenario ist möglicherweise eine genauere Abstimmung für eine bestimmte Shared Image Gallery-Instanz nötig.

Die Aktionen für Images ermöglichen Lese- und Schreibvorgänge. Entscheiden Sie, was für Ihre Umgebung geeignet ist. Erstellen Sie beispielsweise eine Rolle, die es dem Azure VM Image Builder-Dienst ermöglicht, Images aus der Ressourcengruppe *example-rg-1* zu lesen und Images in die Ressourcengruppe *example-rg-2* zu schreiben.

### <a name="custom-image-azure-role-example"></a>Beispiel für eine benutzerdefinierte Rolle für ein Image in Azure

Im folgenden Beispiel wird eine Azure-Rolle erstellt, die für Verwendung und Verteilung eines benutzerdefinierten Quellimages verwendet wird. Dann weisen Sie die benutzerdefinierte Rolle der benutzerseitig zugewiesenen verwalteten Identität für den Azure VM Image Builder-Dienst zu.

Zur Vereinfachung der Ersetzung der Werte im Beispiel legen Sie die folgenden Variablen zuerst fest. Ersetzen Sie die Platzhaltereinstellungen, um Ihre Variablen festzulegen.

| Einstellung | BESCHREIBUNG |
|---------|-------------|
| \<Subscription ID\> | ID Ihres Azure-Abonnements |
| \<Resource group\> | Ressourcengruppe für benutzerdefiniertes Image |

```powershell-interactive
$sub_id = "<Subscription ID>"
# Resource group - For Preview, image builder will only support creating custom images in the same Resource Group as the source managed image.
$imageResourceGroup = "<Resource group>"
$identityName = "aibIdentity"

# Use a web request to download the sample JSON description
$sample_uri="https://raw.githubusercontent.com/azure/azvmimagebuilder/master/solutions/12_Creating_AIB_Security_Roles/aibRoleImageCreation.json"
$role_definition="aibRoleImageCreation.json"

Invoke-WebRequest -Uri $sample_uri -Outfile $role_definition -UseBasicParsing

# Create a unique role name to avoid clashes in the same Azure Active Directory domain
$timeInt=$(get-date -UFormat "%s")
$imageRoleDefName="Azure Image Builder Image Def"+$timeInt

# Update the JSON definition placeholders with variable values
((Get-Content -path $role_definition -Raw) -replace '<subscriptionID>',$sub_id) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace '<rgName>', $imageResourceGroup) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace 'Azure Image Builder Service Image Creation Role', $imageRoleDefName) | Set-Content -Path $role_definition

# Create a custom role from the aibRoleImageCreation.json description file. 
New-AzRoleDefinition -InputFile $role_definition

# Get the user-identity properties
$identityNameResourceId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).Id
$identityNamePrincipalId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).PrincipalId

# Grant the custom role to the user-assigned managed identity for Azure Image Builder.
$parameters = @{
    ObjectId = $identityNamePrincipalId
    RoleDefinitionName = $imageRoleDefName
    Scope = '/subscriptions/' + $sub_id + '/resourceGroups/' + $imageResourceGroup
}

New-AzRoleAssignment @parameters
```

### <a name="existing-vnet-azure-role-example"></a>Beispiel für eine vorhandene VNET-Rolle in Azure

Im folgenden Beispiel wird eine Azure-Rolle erstellt, die für Verwendung und Verteilung eines vorhandenen VNET-Images verwendet wird. Dann weisen Sie die benutzerdefinierte Rolle der benutzerseitig zugewiesenen verwalteten Identität für den Azure VM Image Builder-Dienst zu.

Zur Vereinfachung der Ersetzung der Werte im Beispiel legen Sie die folgenden Variablen zuerst fest. Ersetzen Sie die Platzhaltereinstellungen, um Ihre Variablen festzulegen.

| Einstellung | BESCHREIBUNG |
|---------|-------------|
| \<Subscription ID\> | ID Ihres Azure-Abonnements |
| \<Resource group\> | VNET-Ressourcengruppe |

```powershell-interactive
$sub_id = "<Subscription ID>"
$res_group = "<Resource group>"
$identityName = "aibIdentity"

# Use a web request to download the sample JSON description
$sample_uri="https://raw.githubusercontent.com/azure/azvmimagebuilder/master/solutions/12_Creating_AIB_Security_Roles/aibRoleNetworking.json"
$role_definition="aibRoleNetworking.json"

Invoke-WebRequest -Uri $sample_uri -Outfile $role_definition -UseBasicParsing

# Create a unique role name to avoid clashes in the same AAD domain
$timeInt=$(get-date -UFormat "%s")
$networkRoleDefName="Azure Image Builder Network Def"+$timeInt

# Update the JSON definition placeholders with variable values
((Get-Content -path $role_definition -Raw) -replace '<subscriptionID>',$sub_id) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace '<vnetRgName>', $res_group) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace 'Azure Image Builder Service Networking Role',$networkRoleDefName) | Set-Content -Path $role_definition

# Create a custom role from the aibRoleNetworking.json description file
New-AzRoleDefinition -InputFile $role_definition

# Get the user-identity properties
$identityNameResourceId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).Id
$identityNamePrincipalId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).PrincipalId

# Assign the custom role to the user-assigned managed identity for Azure Image Builder
$parameters = @{
    ObjectId = $identityNamePrincipalId
    RoleDefinitionName = $networkRoleDefName
    Scope = '/subscriptions/' + $sub_id + '/resourceGroups/' + $res_group
}

New-AzRoleAssignment @parameters
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Übersicht über Azure Image Builder](../image-builder-overview.md).
