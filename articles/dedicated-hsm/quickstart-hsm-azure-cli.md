---
title: 'Schnellstart: Erstellen einer Azure Dedicated HSM-Instanz mit der Azure CLI'
description: Es wird beschrieben, wie Sie Azure Dedicated HSM-Instanzen mit der Azure CLI erstellen, anzeigen, auflisten, aktualisieren und löschen.
services: dedicated-hsm
author: msmbaldwin
ms.author: mbaldwin
ms.topic: quickstart
ms.service: key-vault
ms.devlang: azurecli
ms.date: 01/06/2021
ms.custom: devx-track-azurecli
ms.openlocfilehash: e07bc758b1ef86b3d8c605cbce72f6db564a355f
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98020855"
---
# <a name="quickstart-create-an-azure-dedicated-hsm-by-using-the-azure-cli"></a>Schnellstart: Erstellen einer Azure Dedicated HSM-Instanz mit der Azure CLI

In diesem Artikel wird beschrieben, wie Sie eine Azure Dedicated HSM-Instanz erstellen und verwalten, indem Sie die Azure CLI-Erweiterung [az dedicated-hsm](/cli/azure/ext/hardware-security-modules/dedicated-hsm) verwenden.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Sie können ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), falls Sie noch kein Konto besitzen.
  
  Wenn Sie über mehr als ein Azure-Abonnement verfügen, können Sie mit dem Azure CLI-Befehl [az account set](/cli/azure/account#az_account_set) das Abonnement festlegen, das für die Abrechnung genutzt werden soll.
  
  ```azurecli-interactive
  az account set --subscription 00000000-0000-0000-0000-000000000000
  ```
[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]  
  
- Alle Anforderungen für ein dediziertes HSM müssen erfüllt sein, z. B. in Bezug auf die Registrierung und Genehmigung, und zusätzlich müssen ein virtuelles Netzwerk und ein virtueller Computer für die Bereitstellung vorhanden sein. Weitere Informationen zu den Anforderungen und Voraussetzungen für ein dediziertes HSM finden Sie unter [Tutorial: Bereitstellen von HSMs in einem vorhandenen virtuellen Netzwerk mithilfe der Azure CLI](tutorial-deploy-hsm-cli.md).
  

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine [Azure-Ressourcengruppe](../azure-resource-manager/management/overview.md) ist ein logischer Container für die Bereitstellung und Verwaltung von Azure-Ressourcen als Gruppe. Wenn Sie nicht bereits über eine Ressourcengruppe für das dedizierte HSM verfügen, sollten Sie mit dem Befehl [az group create](/cli/azure/group#az_group_create) eine erstellen. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myRG` in der Azure-Region `westus` erstellt:

```azurecli-interactive
az group create --name myRG --location westus
```

## <a name="create-a-dedicated-hsm"></a>Erstellen eines dedizierten HSM

Verwenden Sie zum Erstellen eines dedizierten HSM den Befehl [az dedicated-hsm create](/cli/azure/ext/hardware-security-modules/dedicated-hsm#ext_hardware_security_modules_az_dedicated_hsm_create). Im folgenden Beispiel wird ein dediziertes HSM mit dem Namen `hsm1` in der Region `westus`, in der Ressourcengruppe `myRG` und in einem angegebenen Abonnement, virtuellen Netzwerk und Subnetz bereitgestellt. Die erforderlichen Parameter sind `name`, `location` und `resource group`.

```azurecli-interactive
az dedicated-hsm create \
   --resource-group myRG \
   --name "hsm1" \
   --location "westus" \
   --network-profile-network-interfaces private-ip-address="1.0.0.1" \
   --subnet id="/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/hsm-group/providers/Microsoft.Network/virtualNetworks/MyHSM-vnet/subnets/MyHSM-vnet" \
   --stamp-id "stamp1" \
   --sku name="SafeNet Luna Network HSM A790" \
   --tags resourceType="hsm" Environment="test" \
   --zones "AZ1"
```

Die Durchführung dieser Bereitstellung dauert ca. 25 bis 30 Minuten.

## <a name="get-a-dedicated-hsm"></a>Abrufen eines dedizierten HSM

Führen Sie zum Abrufen eines dedizierten HSM den Befehl [az dedicated-hsm show](/cli/azure/ext/hardware-security-modules/dedicated-hsm#ext_hardware_security_modules_az_dedicated_hsm_show) aus. Im folgenden Beispiel wird das dedizierte HSM `hsm1` in der Ressourcengruppe `myRG` abgerufen.

```azurecli-interactive
az dedicated-hsm show --resource-group myRG --name hsm1
```

## <a name="update-a-dedicated-hsm"></a>Aktualisieren eines dedizierten HSM

Verwenden Sie den Befehl [az dedicated-hsm update](/cli/azure/ext/hardware-security-modules/dedicated-hsm#ext_hardware_security_modules_az_dedicated_hsm_update), um ein dediziertes HSM zu aktualisieren. Im folgenden Beispiel wird das dedizierte HSM `hsm1` in der Ressourcengruppe `myRG` mit den zugehörigen Tags aktualisiert:

```azurecli-interactive
az dedicated-hsm update --resource-group myRG –-name hsm1 --tags resourceType="hsm" Environment="prod" Slice="A"
```

## <a name="list-dedicated-hsms"></a>Liste mit dedizierten HSMs

Führen Sie den Befehl [az dedicated-hsm list](/cli/azure/ext/hardware-security-modules/dedicated-hsm#ext_hardware_security_modules_az_dedicated_hsm_list) aus, um Informationen zu den aktuellen dedizierten HSMs zu erhalten. Im folgenden Beispiel werden die dedizierten HSMs in der Ressourcengruppe `myRG` aufgeführt:

```azurecli-interactive
az dedicated-hsm list --resource-group myRG
```

## <a name="remove-a-dedicated-hsm"></a>Entfernen eines dedizierten HSM

Verwenden Sie zum Entfernen eines dedizierten HSM den Befehl [az dedicated-hsm delete](/cli/azure/ext/hardware-security-modules/dedicated-hsm#ext_hardware_security_modules_az_dedicated_hsm_delete). Im folgenden Beispiel wird das dedizierte HSM mit dem Namen `hsm1` aus der Ressourcengruppe `myRG` gelöscht:

```azurecli-interactive
az dedicated-hsm delete --resource-group myRG –-name hsm1
```

## <a name="delete-the-resource-group"></a>Löschen der Ressourcengruppe

Falls Sie die Ressourcengruppe, die Sie für das dedizierte HSM erstellt haben, nicht mehr benötigen, können Sie sie mit dem Befehl [az group delete](/cli/azure/group#az_group_delete) löschen. Mit diesem Befehl werden die Gruppe und alle darin enthaltenen Ressourcen gelöscht. Dies gilt auch für Ressourcen, die sich nicht auf das dedizierte HSM beziehen. Im folgenden Beispiel wird die Ressourcengruppe `myRG` und ihr gesamter Inhalt gelöscht:

```azurecli-interactive
az group delete --name myRG
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Dedicated HSM finden Sie in [diesem Artikel](overview.md).
