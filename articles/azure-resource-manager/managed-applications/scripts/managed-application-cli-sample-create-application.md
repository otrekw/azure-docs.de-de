---
title: 'Azure CLI-Skriptbeispiel: Bereitstellen einer verwalteten Anwendung'
description: Hier finden Sie ein Azure CLI-Beispielskript, mit dem die Definition für eine verwaltete Azure-Anwendung im Abonnement bereitgestellt wird.
author: tfitzmac
ms.devlang: azurecli
ms.topic: sample
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: 346ea59209bc2f74970e708c947f5caa158a0338
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75648787"
---
# <a name="deploy-a-managed-application-for-service-catalog-with-azure-cli"></a>Bereitstellen einer verwalteten Anwendung für einen Dienstkatalog mit Azure CLI

Mit diesem Skript wird eine Definition für eine verwaltete Anwendung aus dem Dienstkatalog bereitgestellt. 


[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../../cli_scripts/managed-applications/create-application/create-application.sh "Create application")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl zum Bereitstellen der verwalteten Anwendung. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az managedapp create](https://docs.microsoft.com/cli/azure/managedapp#az-managedapp-create) | Erstellen Sie eine verwaltete Anwendung. Geben Sie die Definitions-ID und die Parameter für die Vorlage an. |


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
