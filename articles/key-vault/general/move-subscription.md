---
title: 'Azure Key Vault: Verschieben eines Tresors in ein anderes Abonnement | Microsoft-Dokumentation'
description: Anleitung zum Verschieben eines Schlüsseltresors in ein anderes Abonnement.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 05/05/2020
ms.author: mbaldwin
Customer intent: As a key vault administrator, I want to move my vault to another subscription.
ms.openlocfilehash: a84627b2b426385d21ad72b85780db86e7dd5ee3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101096066"
---
# <a name="moving-an-azure-key-vault-to-another-subscription"></a>Verschieben einer Azure Key Vault-Instanz in ein anderes Abonnement

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Übersicht

> [!IMPORTANT]
> **Das Verschieben eines Schlüsseltresors in ein anderes Abonnement stellt einen Breaking Change Ihrer Umgebung dar.**
> Stellen Sie sicher, dass Ihnen die Auswirkungen dieser Änderung bekannt sind, und befolgen Sie die Anleitungen in diesem Artikel sorgfältig, bevor Sie sich entscheiden, einen Schlüsseltresor in ein neues Abonnement zu verschieben.
> Wenn Sie verwaltete Dienstidentitäten (Managed Service Identities, MSI) verwenden, lesen Sie die Anweisungen für Aktionen im Anschluss an das Verschieben am Ende des Dokuments. 

[Azure Key Vault](overview.md) wird automatisch an die standardmäßige [Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md)-Mandanten-ID für das Abonnement gebunden, in dem die Instanz erstellt wurde. Die Mandanten-ID, die Ihrem Abonnement zugeordnet ist, finden Sie in diesem [Leitfaden](../../active-directory/fundamentals/active-directory-how-to-find-tenant.md). Außerdem werden auch alle Zugriffsrichtlinieneinträge und Rollenzuweisung an diese Mandanten-ID gebunden.  Wenn Sie Ihr Azure-Abonnement aus Mandant A in Mandant B verschieben, können die Dienstprinzipale (Benutzer und Anwendungen) in Mandant B nicht auf Ihre vorhandenen Schlüsseltresore zugreifen. Gehen Sie wie folgt vor, um dies zu beheben:

* Ändern Sie die Mandanten-ID, die allen vorhandenen Schlüsseltresoren im Abonnement zugeordnet ist, in den Mandanten B.
* Entfernen Sie alle vorhandenen Zugriffsrichtlinieneinträge.
* Fügen Sie neue Zugriffsrichtlinieneinträge hinzu, die Mandant B zugeordnet sind.

Weitere Informationen zu Azure Key Vault und Azure Active Directory finden Sie in folgenden Artikeln:
- [Informationen zu Azure Key Vault](overview.md)
- [Was ist Azure Active Directory?](../../active-directory/fundamentals/active-directory-whatis.md)
- [Ermitteln der Mandanten-ID](../../active-directory/fundamentals/active-directory-how-to-find-tenant.md)

## <a name="limitations"></a>Einschränkungen

> [!IMPORTANT]
> **Für die Datenträgerverschlüsselung verwendete Schlüsseltresore können nicht verschoben werden** Wenn Sie Key Vault mit Datenträgerverschlüsselung für einen virtuellen Computer verwenden, kann der Schlüsseltresor nicht in eine andere Ressourcengruppe oder ein anderes Abonnement verschoben werden, solange die Datenträgerverschlüsselung aktiviert ist. Sie müssen die Datenträgerverschlüsselung deaktivieren, bevor Sie den Schlüsseltresor in eine neue Ressourcengruppe oder ein neues Abonnement verschieben. 

Einige Dienstprinzipale (Benutzer und Anwendungen) sind an einen bestimmten Mandanten gebunden. Wenn Sie Ihren Schlüsseltresor in ein Abonnement in einem anderen Mandanten verschieben, besteht die Möglichkeit, dass Sie den Zugriff auf einen bestimmten Dienstprinzipal nicht wiederherstellen können. Stellen Sie sicher, dass alle wichtigen Dienstprinzipale in dem Mandanten vorhanden sind, in den Sie Ihren Schlüsseltresor verschieben.

## <a name="prerequisites"></a>Voraussetzungen

* Zugriffsebene [Mitwirkender](../../role-based-access-control/built-in-roles.md#contributor) oder höher für das aktuelle Abonnement mit dem Schlüsseltresor Sie können eine Rolle über das [Azure-Portal](../../role-based-access-control/role-assignments-portal.md), die [Azure CLI](../../role-based-access-control/role-assignments-cli.md) oder [PowerShell](../../role-based-access-control/role-assignments-powershell.md) zuweisen.
* Zugriffsebene [Mitwirkender](../../role-based-access-control/built-in-roles.md#contributor) oder höher für das Abonnement, in das Sie Ihren Schlüsseltresor verschieben möchten. Sie können eine Rolle über das [Azure-Portal](../../role-based-access-control/role-assignments-portal.md), die [Azure CLI](../../role-based-access-control/role-assignments-cli.md) oder [PowerShell](../../role-based-access-control/role-assignments-powershell.md) zuweisen.
* Eine Ressourcengruppe im neuen Abonnement Sie können eine über das [Azure-Portal](../../azure-resource-manager/management/manage-resource-groups-portal.md) oder per [PowerShell](../../azure-resource-manager/management/manage-resource-groups-powershell.md) bzw. die [Azure CLI](../../azure-resource-manager/management/manage-resource-groups-cli.md) erstellen.

Benutzerdefinierte Rollen können mit dem [Azure-Portal](../../role-based-access-control/role-assignments-list-portal.md), [PowerShell](../../role-based-access-control/role-assignments-list-powershell.md), der [Azure CLI](../../role-based-access-control/role-assignments-list-cli.md) oder der [REST-API](../../role-based-access-control/role-assignments-list-rest.md) erstellt werden.


## <a name="moving-a-key-vault-to-a-new-subscription"></a>Verschieben eines Schlüsseltresors in ein neues Abonnement.

1. Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.
2. Navigieren Sie zu Ihrem [Schlüsseltresor](overview.md).
3. Klicken auf die Registerkarte „Übersicht“
4. Auswählen der Schaltfläche „Verschieben“
5. Auswählen der Option „In ein anderes Abonnement verschieben“ im Dropdownmenü
6. Auswählen der Ressourcengruppe, in die der Schlüsseltresor verschoben werden soll
7. Bestätigen der Warnung zum Verschieben von Ressourcen
8. Auswählen von „OK“

## <a name="additional-steps-when-subscription-is-in-a-new-tenant"></a>Weitere Schritte, wenn sich das Abonnement in einem neuen Mandanten befindet

Wenn Sie Ihren Schlüsseltresor in ein Abonnement in einem neuen Mandanten verschoben haben, müssen Sie die Mandanten-ID manuell aktualisieren und alte Zugriffsrichtlinien und Rollenzuweisungen entfernen. Hier finden Sie Tutorials für diese Schritte mit PowerShell und der Azure-Befehlszeilenschnittstelle. Wenn Sie PowerShell verwenden, müssen Sie möglicherweise den unten dokumentierten Befehl „Clear-AzContext“ ausführen, damit Sie Ressourcen außerhalb des aktuell ausgewählten Bereichs anzeigen können. 

### <a name="update-tenant-id-in-a-key-vault"></a>Aktualisieren der Mandanten-ID in einem Schlüsseltresor

```azurepowershell
Select-AzSubscription -SubscriptionId <your-subscriptionId>                # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your key vault's Resource ID 
$vault = Get-AzResource -ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your key vault
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
### <a name="update-access-policies-and-role-assignments"></a>Aktualisieren von Zugriffsrichtlinien und Rollenzuweisungen

> [!NOTE]
> Wenn Key Vault das Berechtigungsmodell [Azure RBAC](../../role-based-access-control/overview.md) verwendet, müssen Sie zudem Rollenzuweisungen für den Schlüsseltresor entfernen. Sie können Rollenzuweisungen über das [Azure-Portal](../../role-based-access-control/role-assignments-portal.md), die [Azure CLI](../../role-based-access-control/role-assignments-cli.md) oder [PowerShell](../../role-based-access-control/role-assignments-powershell.md) entfernen. 

Nachdem Sie Ihren Tresor nun der richtigen Mandanten-ID zugeordnet haben und alte Zugriffsrichtlinieneinträge und Rollenzuweisungen entfernt wurden, können Sie neue Zugriffsrichtlinieneinträge bzw. Rollenzuweisungen festlegen.

Informationen zum Zuweisen von Richtlinien finden Sie unter:
- [Zuweisen einer Key Vault-Zugriffsrichtlinie über das Azure-Portal](assign-access-policy-portal.md)
- [Zuweisen einer Key Vault-Zugriffsrichtlinie mit der Azure CLI](assign-access-policy-cli.md)
- [Zuweisen einer Key Vault-Zugriffsrichtlinie mit Azure PowerShell](assign-access-policy-powershell.md)

Informationen zum Hinzufügen von Rollenzuweisungen finden Sie unter:
- [Zuweisen von Azure-Rollen über das Azure-Portal](../../role-based-access-control/role-assignments-portal.md)
- [Zuweisen von Azure-Rollen mithilfe der Azure-Befehlszeilenschnittstelle](../../role-based-access-control/role-assignments-cli.md)
- [Zuweisen von Azure-Rollen mithilfe von Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)


### <a name="update-managed-identities"></a>Aktualisieren von verwalteten Identitäten

Wenn Sie ein ganzes Abonnement übertragen und eine verwaltete Identität für Azure-Ressourcen verwenden, müssen Sie sie ebenfalls auf den neuen Azure Active Directory-Mandanten aktualisieren. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Verwaltete Identitäten: Übersicht](../../active-directory/managed-identities-azure-resources/overview.md).

Wenn Sie verwaltete Identitäten verwenden, müssen Sie auch die Identität aktualisieren, weil sich die alte Identität nicht mehr im richtigen Azure Active Directory-Mandanten befindet. Informationen zum Beheben dieses Problems finden Sie in den folgenden Dokumenten. 

* [Aktualisieren von MSI](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories)
* [Übertragen eines Abonnements in ein neues Verzeichnis](../../role-based-access-control/transfer-subscription.md)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Schlüssel, Geheimnisse und Zertifikate](about-keys-secrets-certificates.md).
- Konzeptionelle Informationen einschließlich dazu, wie Protokolle in Key Vault interpretiert werden, finden Sie unter [Key Vault-Protokollierung](logging.md).
- [Entwicklerhandbuch für Key Vault](../general/developers-guide.md)
- [Schützen Ihrer Key Vault-Instanz](secure-your-key-vault.md)
- [Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken](network-security.md)