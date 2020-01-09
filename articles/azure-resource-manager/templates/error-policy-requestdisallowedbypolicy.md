---
title: RequestDisallowedByPolicy-Fehler
description: Hier wird die Ursache des Fehlers „RequestDisallowedByPolicy“ beim Bereitstellen von Ressourcen mit Azure Resource Manager beschrieben.
author: genlin
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 41581ba48da2f2e717c5abf2a749f8fd2b86ac06
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75474272"
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Fehler „RequestDisallowedByPolicy“ bei Azure-Ressourcenrichtlinien

Dieser Artikel beschreibt die Ursache des Fehlers „RequestDisallowedByPolicy“ und bietet eine Lösung für diesen Fehler.

## <a name="symptom"></a>Symptom

Während der Bereitstellung erhalten Sie ggf. den Fehler **RequestDisallowedByPolicy**, der die Erstellung der Ressourcen verhindert. Das folgende Beispiel zeigt den Fehler:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Problembehandlung

Verwenden Sie zum Abrufen von Details zur Richtlinie, die die Bereitstellung blockiert hat, eine der folgenden Methoden:

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Geben Sie in PowerShell diese Richtlinienkennung als `Id`-Parameter an, um Details zur Richtlinie abzurufen, die Ihre Bereitstellung blockiert.

```powershell
(Get-AzPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Geben Sie in der Azure CLI den Namen der Richtliniendefinition an:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Lösung

Ihre Abonnementadministratoren können zu Sicherheits- oder Compliancezwecken Richtlinien zuweisen, mit denen eingeschränkt wird, wie Ressourcen bereitgestellt werden. Beispielsweise kann Ihr Abonnement über eine Richtlinie verfügen, mit der die Erstellung von öffentlichen IP-Adressen, Netzwerksicherheitsgruppen, benutzerdefinierten Routen oder Routingtabellen verhindert wird. Die Fehlermeldung im Abschnitt **Symptome** enthält den Namen der Richtlinie.
Sehen Sie sich zum Beheben dieses Problems die Ressourcenrichtlinien an, und ermitteln Sie, wie Sie Ressourcen bereitstellen, die diese Richtlinien einhalten.

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Was ist Azure Policy?](../../governance/policy/overview.md)
- [Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../../governance/policy/tutorials/create-and-manage.md)
