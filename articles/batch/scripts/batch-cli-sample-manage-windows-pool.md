---
title: 'Azure CLI-Skriptbeispiel: Windows-Pool in Batch'
description: In diesem Skript werden einige der verfügbaren Azure CLI-Tools zum Erstellen und Verwalten eines Pools mit Windows-Computeknoten in Azure Batch präsentiert.
ms.topic: sample
ms.date: 12/12/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: 773699dde9342a4b230a08471a289a56fca7e308
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107768195"
---
# <a name="cli-example-create-and-manage-a-windows-pool-in-azure-batch"></a>CLI-Beispiel: Erstellen und Verwalten eines Windows-Pools in Azure Batch

In diesem Skript werden einige der verfügbaren Azure CLI-Tools zum Erstellen und Verwalten eines Pools mit Windows-Computeknoten in Azure Batch präsentiert. Ein Windows-Pool kann auf zwei Arten konfiguriert werden: als Cloud Services-Konfiguration oder als VM-Konfiguration. Dieses Beispiel zeigt die Erstellung eines Windows-Pools mit der Cloud Services-Konfiguration.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Für dieses Tutorial ist mindestens Version 2.0.20 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert. 

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
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az batch account create](/cli/azure/batch/account#az_batch_account_create) | Erstellt das Batch-Konto. |
| [az batch account login](/cli/azure/batch/account#az_batch_account_login) | Authentifiziert das angegebene Batch-Konto zur weiteren CLI-Interaktion. |
| [az batch pool create](/cli/azure/batch/pool#az_batch_pool_create) | Erstellt einen Pool mit Computeknoten.  |
| [az batch pool set](/cli/azure/batch/pool#az_batch_pool_set) | Aktualisiert die Eigenschaften eines Pools.  |
| [az batch pool autoscale enable](/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_enable) | Aktiviert die automatische Skalierung für einen Pool, und wendet eine Formel an.  |
| [az batch pool show](/cli/azure/batch/pool#az_batch_pool_show) | Zeigt die Eigenschaften eines Pools an.  |
| [az batch pool autoscale disable](/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_disable) | Deaktiviert die automatische Skalierung für einen Pool. |
| [az group delete](/cli/azure/group#az_group_delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).
