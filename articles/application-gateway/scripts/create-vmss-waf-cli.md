---
title: Azure CLI-Skriptbeispiel – Einschränken von Webdatenverkehr | Microsoft-Dokumentation
description: Azure CLI-Skriptbeispiel – Erstellen Sie ein Anwendungsgateway mit einer Web Application Firewall und einer VM-Skalierungsgruppe, die OWASP-Regeln verwendet, um den Datenverkehr einzuschränken.
services: application-gateway
documentationcenter: networking
author: vhorne
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 6e20a4324f46925b8f83d7519c481a1d5bfc06a9
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107789301"
---
# <a name="restrict-web-traffic-using-the-azure-cli"></a>Einschränken von Webdatenverkehr mithilfe der Azure-Befehlszeilenschnittstelle

Dieses Skript erstellt ein Anwendungsgateway mit einer Web Application Firewall, das eine VM-Skalierungsgruppe als Back-End-Server verwendet. Die Web Application Firewall schränkt den Webdatenverkehr anhand von OWASP-Regeln ein. Nach dem Ausführen des Skripts können Sie das Anwendungsgateway mit dessen öffentlicher IP-Adresse testen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss-waf.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, das Anwendungsgateway und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Erstellen Sie ein virtuelles Netzwerk. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Erstellt ein Subnetz in einem virtuellen Netzwerk. |
| [az network public-ip create](/cli/azure/network/public-ip) | Erstellt die öffentliche IP-Adresse für das Anwendungsgateway. |
| [az network application-gateway create](/cli/azure/network/application-gateway) | Erstellen eines Anwendungsgateways |
| [az vmss create](/cli/azure/vmss#az_vmss_create) | Erstellt eine VM-Skalierungsgruppe. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Erstellt ein Speicherkonto. |
| [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create) | Erstellt ein Speicherkonto. |
| [az network public-ip show](/cli/azure/network/public-ip#az_network_public_ip_show) | Ruft die öffentliche IP-Adresse für das Anwendungsgateway ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure/overview).

Zusätzliche CLI-Skriptbeispiele zu Anwendungsgateways finden Sie in der [Dokumentation zu Azure Application Gateway](../cli-samples.md).
