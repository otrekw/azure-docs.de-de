---
title: 'Azure CLI: Bereitstellen des Open-Source-FHIR-Servers für Azure – Azure API for Azure'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie den Microsoft Open-Source-FHIR-Server für Azure bereitstellen.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: f8047ebeb8e47f609db79e3ac1235b5cd65a4fd4
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2020
ms.locfileid: "84819975"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-azure-cli"></a>Schnellstart: Bereitstellen des Open-Source-FHIR-Servers mithilfe der Azure CLI

In dieser Schnellstartanleitung erfahren Sie, wie Sie einen Open-Source-FHIR&reg;-Server in Azure mithilfe der Azure CLI bereitstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-resource-group"></a>Erstellen der Ressourcengruppe

Wählen Sie einen Namen für die Ressourcengruppe aus, in der die bereitgestellten Ressourcen enthalten sein sollen, und erstellen Sie sie:

```azurecli-interactive
servicename="myfhirservice"
az group create --name $servicename --location westus2
```

## <a name="deploy-template"></a>Bereitstellen der Vorlage

Das [GitHub-Repository](https://github.com/Microsoft/fhir-server) für den Microsoft FHIR-Server für Azure enthält eine Vorlage, mit der alle erforderlichen Ressourcen bereitgestellt werden. Führen Sie die Bereitstellung wie folgt aus:

```azurecli-interactive
az group deployment create -g $servicename --template-uri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json --parameters serviceName=$servicename
```

## <a name="verify-fhir-server-is-running"></a>Überprüfen, ob der FHIR-Server ausgeführt wird

Rufen Sie eine Funktionsbestätigung vom FHIR-Server wie folgt ab:

```console
metadataurl="https://${servicename}.azurewebsites.net/metadata"
curl --url $metadataurl
```

Es dauert etwa eine Minute, bis der Server zum ersten Mal antwortet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht mehr benötigen, löschen Sie die Ressourcengruppe wie folgt:

```azurecli-interactive
az group delete --name $servicename
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie den Microsoft Open-Source-FHIR-Server für Azure in Ihrem Abonnement bereitgestellt. Informationen zum Zugriff auf die FHIR-API mit Postman finden Sie im Tutorial zu Postman.
 
>[!div class="nextstepaction"]
>[Zugriff auf die FHIR-API mit Postman](access-fhir-postman-tutorial.md)