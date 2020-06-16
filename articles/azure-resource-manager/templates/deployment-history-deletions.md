---
title: Löschung des Bereitstellungsverlaufs
description: Hier erfahren Sie, wie Azure Resource Manager Bereitstellungen automatisch aus dem Bereitstellungsverlauf löscht. Bereitstellungen werden gelöscht, wenn der Verlauf den Grenzwert von 800 Einträgen überschreitet.
ms.topic: conceptual
ms.date: 05/27/2020
ms.openlocfilehash: 3e48b2da00986da00f7597cf887aa74f84587710
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84122336"
---
# <a name="automatic-deletions-from-deployment-history"></a>Automatische Löschungen aus dem Bereitstellungsverlauf

Jedes Mal, wenn Sie eine Vorlage bereitstellen, werden Informationen über die Bereitstellung in den Bereitstellungsverlauf geschrieben. Jede Ressourcengruppe ist in ihrem Bereitstellungsverlauf auf 800 Bereitstellungen beschränkt.

Ab Juni 2020 löscht Azure Resource Manager Bereitstellungen automatisch aus dem Verlauf, wenn der Grenzwert fast erreicht ist. Die automatische Löschung unterscheidet sich vom bisherigen Verhalten. Zuvor mussten Sie Bereitstellungen manuell aus dem Bereitstellungsverlauf löschen, um einen Fehler zu vermeiden.

> [!NOTE]
> Das Löschen einer Bereitstellung aus dem Verlauf hat keinerlei Auswirkungen auf die bereitgestellten Ressourcen.

## <a name="when-deployments-are-deleted"></a>Wann Bereitstellungen gelöscht werden

Bereitstellungen werden nur aus Ihrem Bereitstellungsverlauf gelöscht, wenn Sie den Grenzwert von 800 fast erreicht haben. Azure Resource Manager löscht eine kleine Teilmenge der ältesten Bereitstellungen, um Platz für zukünftige Bereitstellungen zu schaffen. Der Großteil Ihres Verlaufs bleibt unverändert. Die ältesten Bereitstellungen werden immer zuerst gelöscht.

:::image type="content" border="false" source="./media/deployment-history-deletions/deployment-history.svg" alt-text="Löschungen aus dem Bereitstellungsverlauf":::

Neben Bereitstellungen können Sie Löschungen auch auslösen, wenn Sie den [what-if-Vorgang](template-deploy-what-if.md) ausführen oder eine Bereitstellung überprüfen.

Wenn Sie einer Bereitstellung einen Namen geben, der bereits im Verlauf vorhanden ist, setzen Sie dessen Position im Verlauf zurück. Die Bereitstellung wechselt dann zur aktuellsten Position im Verlauf. Die Position einer Bereitstellung wird auch zurückgesetzt, wenn Sie nach einem Fehler ein [Rollback auf diese Bereitstellung ausführen](rollback-on-error.md).

## <a name="opt-out-of-automatic-deletions"></a>Deaktivieren der automatischen Löschungen

Sie können die automatischen Löschungen aus dem Verlauf deaktivieren. **Verwenden Sie diese Option nur, wenn Sie den Bereitstellungsverlauf eigenhändig verwalten möchten.** Der Grenzwert von 800 Bereitstellungen im Verlauf gilt weiterhin. Wenn Sie 800 Bereitstellungen überschreiten, wird ein Fehler angezeigt und Ihre Bereitstellung schlägt fehl.

Registrieren Sie das Featureflag `Microsoft.Resources/DisableDeploymentGrooming`, um automatische Löschungen zu deaktivieren. Wenn Sie das Featureflag registrieren, deaktivieren Sie die automatischen Löschungen für das gesamte Azure-Abonnement. Sie können dies nicht nur für eine bestimmte Ressourcengruppe deaktivieren.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie für PowerShell den Befehl [Register-AzProviderFeature](/powershell/module/az.resources/Register-AzProviderFeature).

```azurepowershell-interactive
Register-AzProviderFeature -ProviderNamespace Microsoft.Resources -FeatureName DisableDeploymentGrooming
```

Verwenden Sie den folgenden Befehl, um den aktuellen Status Ihres Abonnements anzuzeigen:

```azurepowershell-interactive
Get-AzProviderFeature -ProviderNamespace Microsoft.Resources -FeatureName DisableDeploymentGrooming
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie für die Azure CLI den Befehl [az feature register](/cli/azure/feature#az-feature-register).

```azurecli-interactive
az feature register --namespace Microsoft.Resources --name DisableDeploymentGrooming
```

Verwenden Sie den folgenden Befehl, um den aktuellen Status Ihres Abonnements anzuzeigen:

```azurecli-interactive
az feature show --namespace Microsoft.Resources --name DisableDeploymentGrooming
```

# <a name="rest"></a>[REST](#tab/rest)

Verwenden Sie für die REST-API den Befehl [Features – Register](/rest/api/resources/features/register).

```rest
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Resources/features/DisableDeploymentGrooming/register?api-version=2015-12-01
```

Verwenden Sie den folgenden Befehl, um den aktuellen Status Ihres Abonnements anzuzeigen:

```rest
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Resources/features/DisableDeploymentGrooming/register?api-version=2015-12-01
```

---

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Anzeigen des Bereitstellungsverlaufs finden Sie unter [Anzeigen des Bereitstellungsverlaufs mit Azure Resource Manager](deployment-history.md).
