---
title: 'Azure CLI-Skriptbeispiel: Integrieren von App Service in Application Gateway | Microsoft-Dokumentation'
description: 'Azure CLI-Skriptbeispiel: Integrieren von App Service in Application Gateway'
services: appservice
documentationcenter: appservice
author: madsd
manager: ccompy
editor: ''
tags: azure-service-management
ms.assetid: 6c16d6f8-3c08-4a59-858e-684a2ceccb5f
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/09/2019
ms.author: madsd
ms.custom: seodec18, devx-track-azurecli
ms.openlocfilehash: 8e99d4f1d6de9aa9845ca83dfa68099aad25f9f2
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88960631"
---
# <a name="integrate-app-service-with-application-gateway-using-cli"></a>Integrieren von App Service in Application Gateway mithilfe der Befehlszeilenschnittstelle

Mit diesem Beispielskript werden eine Azure App Service-Web-App, eine Azure Virtual Network-Instanz und eine Application Gateway-Instanz erstellt. Anschließend wird der Datenverkehr für die Web-App ausschließlich auf Datenverkehr aus dem Application Gateway-Subnetz beschränkt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, benötigen Sie die Azure CLI-Version 2.0.74 oder höher. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/integrate-with-app-gateway/integrate-with-app-gateway.sh "Integrate with Application Gateway")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine App Service-App, die Cosmos DB-Instanz und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [`az network vnet create`](/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-create) | Erstellen Sie ein virtuelles Netzwerk. |
| [`az network public-ip create`](/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) | Erstellt eine öffentliche IP-Adresse. |
| [`az network public-ip show`](/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-show) | Zeigt Details einer öffentlichen IP-Adresse an. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | Erstellt einen App Service-Plan. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Erstellt eine App Service-Web-App. |
| [`az webapp show`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-show) | Zeigt Details einer App Service-Web-App an. |
| [`az webapp config access-restriction add`](/cli/azure/webapp/config/access-restriction?view=azure-cli-latest#az-webapp-config-access-restriction-add) | Fügt der App Service-Web-App eine Zugriffsbeschränkung hinzu. |
| [`az network application-gateway create`](/cli/azure/network/application-gateway?view=azure-cli-latest#az-network-application-gateway-create) | Erstellt eine Application Gateway-Instanz. |
| [`az network application-gateway http-settings update`](/cli/azure/network/application-gateway/http-settings?view=azure-cli-latest#az-network-application-gateway-http-settings-update) | Aktualisiert Application Gateway-HTTP-Einstellungen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../samples-cli.md).