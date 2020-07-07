---
title: 'Azure CLI-Skriptbeispiel: Windows-Pool in Batch'
description: In diesem Skript werden einige der verfügbaren Azure CLI-Tools zum Erstellen und Verwalten eines Pools mit Windows-Computeknoten in Azure Batch präsentiert.
ms.topic: sample
ms.date: 12/12/2019
ms.openlocfilehash: 6e7d53ea4ca0a320cd70563ed982c690fa30542c
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85963987"
---
# <a name="cli-example-create-and-manage-a-windows-pool-in-azure-batch"></a>CLI-Beispiel: Erstellen und Verwalten eines Windows-Pools in Azure Batch

In diesem Skript werden einige der verfügbaren Azure CLI-Tools zum Erstellen und Verwalten eines Pools mit Windows-Computeknoten in Azure Batch präsentiert. Ein Windows-Pool kann auf zwei Arten konfiguriert werden: als Cloud Services-Konfiguration oder als VM-Konfiguration. Dieses Beispiel zeigt die Erstellung eines Windows-Pools mit der Cloud Services-Konfiguration.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.20 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Windows Cloud Services Pool")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Erstellt das Batch-Konto. |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Authentifiziert das angegebene Batch-Konto zur weiteren CLI-Interaktion. |
| [az batch pool create](/cli/azure/batch/pool#az-batch-pool-create) | Erstellt einen Pool mit Computeknoten.  |
| [az batch pool set](/cli/azure/batch/pool#az-batch-pool-set) | Aktualisiert die Eigenschaften eines Pools.  |
| [az batch pool autoscale enable](/cli/azure/batch/pool/autoscale#az-batch-pool-autoscale-enable) | Aktiviert die automatische Skalierung für einen Pool, und wendet eine Formel an.  |
| [az batch pool show](/cli/azure/batch/pool#az-batch-pool-show) | Zeigt die Eigenschaften eines Pools an.  |
| [az batch pool autoscale disable](/cli/azure/batch/pool/autoscale#az-batch-pool-autoscale-disable) | Deaktiviert die automatische Skalierung für einen Pool. |
| [az group delete](/cli/azure/group#az-group-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).
