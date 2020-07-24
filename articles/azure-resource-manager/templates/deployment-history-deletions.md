---
title: Löschung des Bereitstellungsverlaufs
description: Hier erfahren Sie, wie Azure Resource Manager Bereitstellungen automatisch aus dem Bereitstellungsverlauf löscht. Bereitstellungen werden gelöscht, wenn der Verlauf den Grenzwert von 800 Einträgen überschreitet.
ms.topic: conceptual
ms.date: 07/10/2020
ms.openlocfilehash: 8ec3291dc5e35689d4e2c614949e0328057fbfd3
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86248979"
---
# <a name="automatic-deletions-from-deployment-history"></a>Automatische Löschungen aus dem Bereitstellungsverlauf

Jedes Mal, wenn Sie eine Vorlage bereitstellen, werden Informationen über die Bereitstellung in den Bereitstellungsverlauf geschrieben. Jede Ressourcengruppe ist in ihrem Bereitstellungsverlauf auf 800 Bereitstellungen beschränkt.

Azure Resource Manager beginnt bald mit dem automatischen Löschen von Bereitstellungen aus Ihrem Verlauf, wenn Sie sich dem Grenzwert nähern. Die automatische Löschung unterscheidet sich vom bisherigen Verhalten. Zuvor mussten Sie Bereitstellungen manuell aus dem Bereitstellungsverlauf löschen, um einen Fehler zu vermeiden. **Diese Funktion wurde noch nicht zu Azure hinzugefügt. Wir benachrichtigen Sie über diese bevorstehende Änderung, falls Sie sich gegen eine Teilnahme entscheiden möchten.**

> [!NOTE]
> Das Löschen einer Bereitstellung aus dem Verlauf hat keinerlei Auswirkungen auf die bereitgestellten Ressourcen.
>
> Wenn Sie eine [CanNotDelete-Sperre](../management/lock-resources.md) für eine Ressourcengruppe eingerichtet haben, können die Bereitstellungen für diese Ressourcengruppe nicht gelöscht werden. Sie müssen die Sperre entfernen, um die Vorteile automatischer Löschungen aus dem Bereitstellungsverlauf nutzen zu können.

## <a name="when-deployments-are-deleted"></a>Wann Bereitstellungen gelöscht werden

Bereitstellungen werden aus Ihrem Verlauf gelöscht, wenn Sie den Grenzwert von 775 Bereitstellungen erreicht haben. Azure Resource Manager löscht Bereitstellungen, bis der Verlauf wieder 750 Bereitstellungen enthält. Die ältesten Bereitstellungen werden immer zuerst gelöscht.

:::image type="content" border="false" source="./media/deployment-history-deletions/deployment-history.svg" alt-text="Löschungen aus dem Bereitstellungsverlauf":::

> [!NOTE]
> Die Anfangszahl (775) und die Endzahl (750) können geändert werden.
>
> Wenn Ihre Ressourcengruppe bereits den Grenzwert von 800 erreicht hat, schlägt Ihre nächste Bereitstellung fehl. Der automatische Löschvorgang wird sofort gestartet. Nach einer kurzen Wartezeit können Sie die Bereitstellung noch mal versuchen.

Neben Bereitstellungen können Sie Löschungen auch auslösen, wenn Sie den [what-if-Vorgang](template-deploy-what-if.md) ausführen oder eine Bereitstellung überprüfen.

Wenn Sie einer Bereitstellung einen Namen geben, der bereits im Verlauf vorhanden ist, setzen Sie dessen Position im Verlauf zurück. Die Bereitstellung wechselt dann zur aktuellsten Position im Verlauf. Die Position einer Bereitstellung wird auch zurückgesetzt, wenn Sie nach einem Fehler ein [Rollback auf diese Bereitstellung ausführen](rollback-on-error.md).

## <a name="opt-out-of-automatic-deletions"></a>Deaktivieren der automatischen Löschungen

Sie können die automatischen Löschungen aus dem Verlauf deaktivieren. **Verwenden Sie diese Option nur, wenn Sie den Bereitstellungsverlauf eigenhändig verwalten möchten.** Der Grenzwert von 800 Bereitstellungen im Verlauf gilt weiterhin. Wenn Sie 800 Bereitstellungen überschreiten, wird ein Fehler angezeigt und Ihre Bereitstellung schlägt fehl.

Registrieren Sie das Featureflag `Microsoft.Resources/DisableDeploymentGrooming`, um automatische Löschungen zu deaktivieren. Wenn Sie das Featureflag registrieren, deaktivieren Sie die automatischen Löschungen für das gesamte Azure-Abonnement. Sie können dies nicht nur für eine bestimmte Ressourcengruppe deaktivieren. Um automatisches Löschen wieder zu aktivieren, heben Sie die Registrierung des Featureflags auf.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie für PowerShell den Befehl [Register-AzProviderFeature](/powershell/module/az.resources/Register-AzProviderFeature).

```azurepowershell-interactive
Register-AzProviderFeature -ProviderNamespace Microsoft.Resources -FeatureName DisableDeploymentGrooming
```

Verwenden Sie den folgenden Befehl, um den aktuellen Status Ihres Abonnements anzuzeigen:

```azurepowershell-interactive
Get-AzProviderFeature -ProviderNamespace Microsoft.Resources -FeatureName DisableDeploymentGrooming
```

Um automatisches Löschen wieder zu aktivieren, verwenden Sie die Azure-REST-API oder die Azure CLI.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie für die Azure CLI den Befehl [az feature register](/cli/azure/feature#az-feature-register).

```azurecli-interactive
az feature register --namespace Microsoft.Resources --name DisableDeploymentGrooming
```

Verwenden Sie den folgenden Befehl, um den aktuellen Status Ihres Abonnements anzuzeigen:

```azurecli-interactive
az feature show --namespace Microsoft.Resources --name DisableDeploymentGrooming
```

Um automatisches Löschen wieder zu aktivieren, verwenden Sie [az feature unregister](/cli/azure/feature#az-feature-unregister).

```azurecli-interactive
az feature unregister --namespace Microsoft.Resources --name DisableDeploymentGrooming
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

Um automatisches Löschen wieder zu aktivieren, verwenden Sie [Features: Registrierung aufheben](/rest/api/resources/features/unregister).

```rest
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Resources/features/DisableDeploymentGrooming/unregister?api-version=2015-12-01
```

---

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Anzeigen des Bereitstellungsverlaufs finden Sie unter [Anzeigen des Bereitstellungsverlaufs mit Azure Resource Manager](deployment-history.md).
