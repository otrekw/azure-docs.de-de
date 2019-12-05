---
title: 'Beispiel: Richtlinieninitiative zur Abrechnung von Tags'
description: Für diese Beispielrichtlinieninitiative (Richtliniensatz) müssen Tagwerte, die in Parametern definiert sind, für die Kostenstelle und den Produktnamen angegeben werden.
ms.date: 01/23/2019
ms.topic: sample
ms.openlocfilehash: 449909b160c2b811c62cd8c6592e74dd0f714e41
ms.sourcegitcommit: 95931aa19a9a2f208dedc9733b22c4cdff38addc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74463391"
---
# <a name="sample---billing-tags-policy-initiative"></a>Beispiel: Richtlinieninitiative zur Abrechnung von Tags

Diese festgelegte Richtlinie erfordert angegebene Tagwerte für die Kostenstelle und den Produktnamen. Verwendet integrierte Richtlinien, um erforderliche Tags anzuwenden und zu erzwingen. Sie geben die erforderlichen Werte für die Tags an.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Vorlagenbeispiel

[!code-json[main](../../../../policy-templates/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.json "Billing Tags Policy Initiative")]

Sie können diese Vorlage mit [PowerShell](#deploy-with-powershell) bereitstellen.

## <a name="deploy-with-powershell"></a>Bereitstellen mit PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```azurepowershell-interactive
$policydefinitions = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.definitions.json"
$policysetparameters = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.parameters.json"

$policyset= New-AzPolicySetDefinition -Name "multiple-billing-tags" -DisplayName "Billing Tags Policy Initiative" -Description "Specify cost Center tag and product name tag" -PolicyDefinition $policydefinitions -Parameter $policysetparameters

New-AzPolicyAssignment -PolicySetDefinition $policyset -Name <assignmentname> -Scope <scope>  -costCenterValue <required value for Cost Center tag> -productNameValue <required value for product Name tag>
```

### <a name="clean-up-powershell-deployment"></a>Bereinigen der PowerShell-Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="apply-tags-to-existing-resources"></a>Anwenden von Tags auf vorhandene Ressourcen

Nach dem Zuweisen der Richtlinien können Sie ein Update für alle vorhandenen Ressourcen auslösen, um die hinzugefügten Tagrichtlinien zu erzwingen. Das folgende Skript behält alle anderen Tags bei, die für die Ressourcen vorhanden waren:

```azurepowershell-interactive
$resources = Get-AzResource -ResourceGroupName 'ExampleGroup'

foreach ($r in $resources) {
    try {
        Set-AzResource -Tags ($a = if ($r.Tags -eq $NULL) { @{} } else {$r.Tags}) -ResourceId $r.ResourceId -Force -UsePatchSemantics
    }
    catch {
        Write-Host $r.ResourceId + "can't be updated"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Unter [Azure Policy-Beispiele](index.md) finden Sie weitere Beispiele.