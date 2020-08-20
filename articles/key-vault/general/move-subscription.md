---
title: 'Azure Key Vault: Verschieben eines Tresors in ein anderes Abonnement | Microsoft-Dokumentation'
description: Anleitung zum Verschieben eines Schlüsseltresors in ein anderes Abonnement.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 05/05/2020
ms.author: sudbalas
Customer intent: As a key vault administrator, I want to move my vault to another subscription.
ms.openlocfilehash: d37fae18cd2f3e3bfad647cc176253dc6bb101ab
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88585762"
---
# <a name="moving-an-azure-key-vault-to-another-subscription"></a>Verschieben einer Azure Key Vault-Instanz in ein anderes Abonnement

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Übersicht

> [!IMPORTANT]
> **Das Verschieben eines Schlüsseltresors in ein anderes Abonnement stellt einen Breaking Change Ihrer Umgebung dar.**
> Stellen Sie sicher, dass Ihnen die Auswirkungen dieser Änderung bekannt sind, und befolgen Sie die Anleitungen in diesem Artikel sorgfältig, bevor Sie sich entscheiden, einen Schlüsseltresor in ein neues Abonnement zu verschieben.
> Wenn Sie verwaltete Dienstidentitäten (Managed Service Identities, MSI) verwenden, lesen Sie die Anweisungen für Aktionen im Anschluss an das Verschieben am Ende des Dokuments. 

Wenn Sie einen Schlüsseltresor erstellen, wird er automatisch an die standardmäßige Azure Active Directory-Mandanten-ID für das Abonnement gebunden, in dem er erstellt wurde. Außerdem werden auch alle Zugriffsrichtlinieneinträge an diese Mandanten-ID gebunden. Wenn Sie Ihr Azure-Abonnement aus Mandant A in Mandant B verschieben, können die Dienstprinzipale (Benutzer und Anwendungen) in Mandant B nicht auf Ihre vorhandenen Schlüsseltresore zugreifen. Gehen Sie wie folgt vor, um dies zu beheben:

* Ändern Sie die Mandanten-ID, die allen vorhandenen Schlüsseltresoren im Abonnement zugeordnet ist, in den Mandanten B.
* Entfernen Sie alle vorhandenen Zugriffsrichtlinieneinträge.
* Fügen Sie neue Zugriffsrichtlinieneinträge hinzu, die Mandant B zugeordnet sind.

## <a name="limitations"></a>Einschränkungen

Einige Dienstprinzipale (Benutzer und Anwendungen) sind an einen bestimmten Mandanten gebunden. Wenn Sie Ihren Schlüsseltresor in ein Abonnement in einem anderen Mandanten verschieben, besteht die Möglichkeit, dass Sie den Zugriff auf einen bestimmten Dienstprinzipal nicht wiederherstellen können. Stellen Sie sicher, dass alle wichtigen Dienstprinzipale in dem Mandanten vorhanden sind, in den Sie Ihren Schlüsseltresor verschieben.

## <a name="design-considerations"></a>Überlegungen zum Entwurf

Ihre Organisation hat möglicherweise Azure Policy mit Erzwingung oder Ausschlüssen auf Abonnementebene implementiert. In dem Abonnement, in dem der Schlüsseltresor derzeit enthalten ist, und in dem Abonnement, in das Sie den Schlüsseltresor verschieben, gilt möglicherweise ein unterschiedlicher Satz von Richtlinienzuweisungen. Ein Konflikt in den Richtlinienanforderungen kann Ihre Anwendungen potenziell unterbrechen.

### <a name="example"></a>Beispiel

Sie verfügen über eine Anwendung, die mit dem Schlüsseltresor, der Zertifikate erstellt, verbunden ist. Die Zertifikate sind zwei Jahre gültig. Das Abonnement, in das Sie den Schlüsseltresor verschieben möchten, verfügt über eine Richtlinienzuweisung, die die Erstellung von Zertifikaten blockiert, die länger als ein Jahr gültig sind. Nachdem Sie Ihren Schlüsseltresor in das neue Abonnement verschoben haben, wird der Vorgang zum Erstellen eines Zertifikats, das zwei Jahre gültig ist, durch eine Azure Policy-Zuweisung blockiert.

### <a name="solution"></a>Lösung

Überprüfen Sie auf der Seite „Azure Policy“ im Azure-Portal die Richtlinienzuweisungen für Ihr aktuelles Abonnement und das Zielabonnement, und stellen Sie sicher, dass keine Konflikte bestehen.

## <a name="prerequisites"></a>Voraussetzungen

* Zugriffsebene „Mitwirkender“ oder höher für das aktuelle Abonnement mit dem Schlüsseltresor
* Zugriffsebene „Mitwirkende“ oder höher für das Abonnement, in das Sie Ihren Schlüsseltresor verschieben möchten
* Eine Ressourcengruppe im neuen Abonnement

## <a name="procedure"></a>Verfahren

### <a name="initial-steps-moving-key-vault"></a>Erste Schritte (Verschieben des Schlüsseltresors)

1. Anmelden beim Azure-Portal
2. Navigieren zum Schlüsseltresor
3. Klicken auf die Registerkarte „Übersicht“
4. Auswählen der Schaltfläche „Verschieben“
5. Auswählen der Option „In ein anderes Abonnement verschieben“ im Dropdownmenü
6. Auswählen der Ressourcengruppe, in die der Schlüsseltresor verschoben werden soll
7. Bestätigen der Warnung zum Verschieben von Ressourcen
8. Auswählen von „OK“

### <a name="additional-steps-post-move"></a>Zusätzliche Schritte (nach dem Verschieben)

Nachdem Sie Ihren Schlüsseltresor in das neue Abonnement verschoben haben, müssen Sie die Mandanten-ID aktualisieren und alte Zugriffsrichtlinien entfernen. Hier finden Sie Tutorials für diese Schritte mit PowerShell und der Azure-Befehlszeilenschnittstelle.

```azurepowershell
Select-AzSubscription -SubscriptionId <your-subscriptionId>                # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your key vault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your key vault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your key vault resides in
$vault.Properties.AccessPolicies = @()                                     # Access policies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                             # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the key vault's properties.

Clear-AzContext                                                            #Clear the context from PowerShell
Connect-AzAccount                                                          #Log in again to confirm you have the correct tenant id
````

```azurecli
az account set -s <your-subscriptionId>                                    # Select your Azure Subscription
tenantId=$(az account show --query tenantId)                               # Get your tenantId
az keyvault update -n myvault --remove Properties.accessPolicies           # Remove the access policies
az keyvault update -n myvault --set Properties.tenantId=$tenantId          # Update the key vault tenantId
```

Nachdem Sie Ihren Tresor nun der richtigen Mandanten-ID zugeordnet haben und alte Zugriffsrichtlinieneinträge entfernt wurden, können Sie neue Zugriffsrichtlinieneinträge mit dem Azure PowerShell-Cmdlet [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy) oder dem Azure CLI-Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) festlegen.

Wenn Sie eine verwaltete Identität für Azure-Ressourcen verwenden, müssen Sie sie ebenfalls auf den neuen Azure AD-Mandanten aktualisieren. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Bereitstellen der Key Vault-Authentifizierung mit einer verwalteten Identität](managed-identity.md).

Wenn Sie MSI verwenden, müssen Sie auch die MSI-Identität aktualisieren, da sich die alte Identität nicht mehr im richtigen AAD-Mandanten befindet. Informationen zum Beheben dieses Problems finden Sie in den folgenden Dokumenten. 

* [Aktualisieren von MSI](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/known-issues#transferring-a-subscription-between-azure-ad-directories)
* [Übertragen eines Abonnements in ein neues Verzeichnis](https://docs.microsoft.com/azure/role-based-access-control/transfer-subscription)


