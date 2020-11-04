---
title: Aktivieren von VM-Erweiterungen mithilfe der Azure-Befehlszeilenschnittstelle
description: In diesem Artikel erfahren Sie, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle VM-Erweiterungen auf Servern mit Azure Arc-Unterstützung bereitstellen, die in Hybrid Cloud-Umgebungen ausgeführt werden.
ms.date: 10/19/2020
ms.topic: conceptual
ms.openlocfilehash: 8f09914f246635f07b3c51c682bd67591c706732
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92462825"
---
# <a name="enable-azure-vm-extensions-using-the-azure-cli"></a>Aktivieren von Azure-VM-Erweiterungen mithilfe der Azure-Befehlszeilenschnittstelle

In diesem Artikel erfahren Sie, wie Sie die Azure-Befehlszeilenschnittstelle verwenden, um Azure-VM-Erweiterungen, die von Servern mit Azure Arc-Unterstützung unterstützt werden, auf einem Linux- oder Windows-Hybridcomputer bereitzustellen oder zu deinstallieren.

[!INCLUDE [Azure CLI Prepare your environment](../../../includes/azure-cli-prepare-your-environment.md)]

## <a name="prerequisites"></a>Voraussetzungen

[Installieren Sie die Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).

Damit Sie die Azure-Befehlszeilenschnittstelle verwenden können, um VM-Erweiterungen auf Ihrem Hybridserver zu verwalten, der von Servern mit Azure Arc-Unterstützung verwaltet wird, müssen Sie zuerst die CLI-Erweiterung `ConnectedMachine` installieren. Führen Sie auf Ihrem Server mit Arc-Unterstützung den folgenden Befehl aus:

```azurecli
az extension add connectedmachine
```

Nach Abschluss der Installation wird die folgende Meldung zurückgegeben:

`The installed extension `connectedmachine` is experimental and not covered by customer support. Please use with discretion.`

## <a name="enable-extension"></a>Aktivieren der Erweiterung

Verwenden Sie [az connectedmachine machine-extension create](/cli/azure/ext/connectedmachine/connectedmachine/machine-extension#ext_connectedmachine_az_connectedmachine_machine_extension_create) mit den Parametern `--machine-name`, `--extension-name`, `--location`, `--type`, `settings` und `--publisher`, um eine VM-Erweiterung auf Ihrem Server mit Arc-Unterstützung zu aktivieren.

Im folgenden Beispiel wird die Log Analytics-VM-Erweiterung auf einem Linux-Server mit Arc-Unterstützung aktiviert:

```azurecli
az connectedmachine machine-extension create --machine-name "myMachineName" --name "OmsAgentforLinux" --location "eastus" --type "CustomScriptExtension" --publisher "Microsoft.EnterpriseCloud.Monitoring" --settings "{\"workspaceId\":\"workspaceId"}" --protected-settings "{\workspaceKey\":"\workspaceKey"} --type-handler-version "1.10" --resource-group "myResourceGroup"
```

Im folgenden Beispiel wird die Erweiterung für benutzerdefinierte Skripts auf einem Server mit Arc-Unterstützung aktiviert:

```azurecli
az connectedmachine machine-extension create --machine-name "myMachineName" --name "CustomScriptExtension" --location "eastus" --type "CustomScriptExtension" --publisher "Microsoft.Compute" --settings "{\"commandToExecute\":\"powershell.exe -c \\\"Get-Process | Where-Object { $_.CPU -gt 10000 }\\\"\"}" --type-handler-version "1.10" --resource-group "myResourceGroup"
```

## <a name="list-extensions-installed"></a>Auflisten der Installierten Erweiterungen

Verwenden Sie [az connectedmachine machine-extension list](/cli/azure/ext/connectedmachine/connectedmachine/machine-extension#ext_connectedmachine_az_connectedmachine_machine_extension_list) mit den Parametern `--machine-name` und `--resource-group`, um eine Liste der VM-Erweiterungen auf dem Server mit Arc-Unterstützung abzurufen.

Beispiel:

```azurecli
az connectedmachine machine-extension list --machine-name "myMachineName" --resource-group "myResourceGroup"
```

Standardmäßig erfolgt die Ausgabe von Azure CLI-Befehlen in JSON (JavaScript Object Notation). Um beispielsweise die Standardausgabe in eine Liste oder Tabelle zu ändern, verwenden Sie [az configure --output](/cli/azure/reference-index). Ferner können Sie `--output` an beliebige Befehle anfügen, um eine einmalige Änderung des Ausgabeformats zu erreichen.

Das folgende Beispiel zeigt die partielle JSON-Ausgabe des Befehls `az connectedmachine machine-extension -list`:

```json
[
  {
    "autoUpgradingMinorVersion": "false",
    "forceUpdateTag": null,
    "id": "/subscriptions/subscriptionId/resourceGroups/resourceGroupName/providers/Microsoft.HybridCompute/machines/SVR01/extensions/DependencyAgentWindows",
    "location": "eastus",
    "name": "DependencyAgentWindows",
    "namePropertiesInstanceViewName": "DependencyAgentWindows",
```

## <a name="remove-an-installed-extension"></a>Entfernen einer installierten Erweiterung

Verwenden Sie [az connectedmachine machine-extension delete](/cli/azure/ext/connectedmachine/connectedmachine/machine-extension#ext_connectedmachine_az_connectedmachine_machine_extension_delete) mit den Parametern `--extension-name`, `--machine-name` und `--resource-group`, um eine installierte VM-Erweiterung auf Ihrem Server mit Arc-Unterstützung zu entfernen.

Führen Sie also beispielsweise den folgenden Befehl aus, um die Log Analytics-VM-Erweiterung für Linux zu entfernen:

```azurecli
az connectedmachine machine-extension delete --machine-name "myMachineName" --name "OmsAgentforLinux" --resource-group "myResourceGroup"
```

## <a name="next-steps"></a>Nächste Schritte

- Sie können VM-Erweiterungen mithilfe von [PowerShell](manage-vm-extensions-powershell.md), über das [Azure-Portal](manage-vm-extensions-portal.md) oder mithilfe von [Azure Resource Manager-Vorlagen](manage-vm-extensions-template.md) bereitstellen, verwalten und entfernen.

- Informationen zur Problembehandlung finden Sie im [Problembehandlungs-Handbuch für VM-Erweiterungen](troubleshoot-vm-extensions.md).
