---
title: Verwenden einer Azure-Ressourcen-Manager-Vorlage zum Bereitstellen eines privaten Endpunkts für eine Web-App
description: Hier erfahren Sie, wie Sie mithilfe einer ARM-Vorlage einen privaten Endpunkt für Ihre Web-App bereitstellen.
author: ericgre
ms.assetid: 49e460d0-7759-4ceb-b5a4-f1357e4fde56
ms.topic: sample
ms.date: 07/08/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: acf7198477ce0c55f9321fd3f8e57294a60c0961
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89652017"
---
# <a name="create-an-app-service-app-and-deploy-a-private-endpoint-by-using-an-azure-resource-manager-template"></a>Erstellen einer App Service-App und Bereitstellen eines privaten Endpunkts mithilfe einer Azure Resource Manager-Vorlage

In dieser Schnellstartanleitung erstellen Sie mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) eine Web-App und machen sie mit einem privaten Endpunkt verfügbar.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisite"></a>Voraussetzung

Sie benötigen ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts

Mit dieser Vorlage wird ein privater Endpunkt für eine Azure-Web-App erstellt.

### <a name="review-the-template"></a>Überprüfen der Vorlage

:::code language="json" source="~/quickstart-templates/101-private-endpoint-webapp/azuredeploy.json" :::

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

So stellen Sie die Azure Resource Manager-Vorlage in Azure bereit:

1. Wählen Sie den folgenden Link aus, um sich bei Azure anzumelden und die Vorlage zu öffnen:  [Bereitstellung in Microsoft Azure:](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-private-endpoint-webapp%2Fazuredeploy.json) Die Vorlage erstellt das virtuelle Netzwerk, die Web-App, den privaten Endpunkt und die private DNS-Zone.
2. Wählen Sie Ihre Ressourcengruppe aus, oder erstellen Sie sie.
3. Geben Sie den Namen Ihrer Web-App, den Azure App Service-Plan und den privaten Endpunkt ein.
5. Lesen Sie die Geschäftsbedingungen. Wenn Sie einverstanden sind, wählen Sie **Ich stimme den oben genannten Geschäftsbedingungen zu** > **Kaufen** aus. Die Bereitstellung kann einige Minuten dauern.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem privaten Endpunkt erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden der private Endpunkt und alle zugehörigen Ressourcen entfernt.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Azure Resource Manager-Vorlagen für Azure App Service-Web-Apps finden Sie unter [Azure Resource Manager-Vorlagen für App Service](../samples-resource-manager-templates.md).
