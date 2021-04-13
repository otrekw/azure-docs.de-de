---
Titel: Media Services-Konto: ARM-Vorlage: Azure Media Services description: In diesem Artikel wird beschrieben, wie Sie mithilfe einer ARM-Vorlage ein Media Services-Konto erstellen.
services: media-services documentationcenter: '' author: IngridAtMicrosoft manager: femila editor: ''

ms.service: media-services ms.workload: ms.topic: quickstart ms.date: 03/23/2021 ms.author: inhenkel ms.custom: subject-armqs

---

# <a name="quickstart-media-services-account-arm-template"></a>Schnellstart: Media Services-Konto: ARM-Vorlage

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

In diesem Artikel wird beschrieben, wie Sie mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) ein Media Services-Konto erstellen.

## <a name="introduction"></a>Einführung

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Leser, die mit ARM-Vorlagen vertraut sind, können mit dem Abschnitt [Bereitstellung](#deploy-the-template) fortfahren.

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-media-services-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Wenn Sie noch nie eine ARM-Vorlage bereitgestellt haben, ist es hilfreich, sich über [Azure ARM-Vorlagen](../../azure-resource-manager/templates/index.yml) zu informieren und das [Tutorial](../../azure-resource-manager/templates/template-tutorial-create-first-template.md?tabs=azure-powershell) durchzuarbeiten.

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-media-services-create/).

Die Syntax für den JSON-Codebereich lautet wie folgt:

:::code language="json" source="~/quickstart-templates/101-media-services-create/azuredeploy.json":::

In der Vorlage sind drei Azure-Ressourcentypen definiert:

- [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts): Dient zum Erstellen eines Speicherkontos.
- [Microsoft.Media/mediaservices](/azure/templates/microsoft.media/mediaservices): Dient zum Erstellen eines Media Services-Kontos.

## <a name="set-the-account"></a>Festlegen des Kontos

```azurecli-interactive

az account set --subscription {your subscription name or id}

```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

```azurecli-interactive

az group create --name {the name you want to give your resource group} --location "{pick a location}"

```

## <a name="assign-a-variable-to-your-deployment-file"></a>Zuweisen einer Variablen zur Bereitstellungsdatei

Erstellen Sie zur einfacheren Handhabung eine Variable, in der der Pfad zu der Vorlagendatei gespeichert wird. Diese Variable erleichtert es Ihnen, die Bereitstellungsbefehle auszuführen, da Sie den Pfad nicht bei jeder Bereitstellung erneut eingeben müssen.

```azurecli-interactive

templateFile="{provide the path to the template file}"

```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Sie werden zum Eingeben Ihres Media Services-Kontonamens aufgefordert.

```azurecli-interactive

az deployment group create --name {the name you want to give to your deployment} --resource-group {the name of resource group you created} --template-file $templateFile

```

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Daraufhin sollte eine JSON-Antwort angezeigt werden, die etwa wie folgt aussieht:

```json
{
  "id": "/subscriptions/{subscriptionid}/resourceGroups/amsarmquickstartrg/providers/Microsoft.Resources/deployments/amsarmquickstartdeploy",
  "location": null,
  "name": "amsarmquickstartdeploy",
  "properties": {
    "correlationId": "{correlationid}",
    "debugSetting": null,
    "dependencies": [
      {
        "dependsOn": [
          {
            "id": "/subscriptions/{subscriptionid}/resourceGroups/amsarmquickstartrg/providers/Microsoft.Storage/storageAccounts/storagey44cfdmliwatk",
            "resourceGroup": "amsarmquickstartrg",
            "resourceName": "storagey44cfdmliwatk",
            "resourceType": "Microsoft.Storage/storageAccounts"
          }
        ],
        "id": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/resourceGroups/amsarmquickstartrg/providers/Microsoft.Media/mediaServices/{accountname}",
        "resourceGroup": "amsarmquickstartrg",
        "resourceName": "{accountname}",
        "resourceType": "Microsoft.Media/mediaServices"
      }
    ],
    "duration": "PT1M10.8615001S",
    "error": null,
    "mode": "Incremental",
    "onErrorDeployment": null,
    "outputResources": [
      {
        "id": "/subscriptions/{subscriptionid}/resourceGroups/amsarmquickstartrg/providers/Microsoft.Media/mediaServices/{accountname}",
        "resourceGroup": "amsarmquickstartrg"
      },
      {
        "id": "/subscriptions/{subscriptionid}/resourceGroups/amsarmquickstartrg/providers/Microsoft.Storage/storageAccounts/storagey44cfdmliwatk",
        "resourceGroup": "amsarmquickstartrg"
      }
    ],
    "outputs": null,
    "parameters": {
      "mediaServiceName": {
        "type": "String",
        "value": "{accountname}"
      }
    },
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "Microsoft.Media",
        "registrationPolicy": null,
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "capabilities": null,
            "locations": [
              "eastus"
            ],
            "properties": null,
            "resourceType": "mediaServices"
          }
        ]
      },
      {
        "id": null,
        "namespace": "Microsoft.Storage",
        "registrationPolicy": null,
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "capabilities": null,
            "locations": [
              "eastus"
            ],
            "properties": null,
            "resourceType": "storageAccounts"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "templateHash": "{templatehash}",
    "templateLink": null,
    "timestamp": "2020-11-24T23:25:52.598184+00:00",
    "validatedResources": null
  },
  "resourceGroup": "amsarmquickstartrg",
  "tags": null,
  "type": "Microsoft.Resources/deployments"
}

```

Vergewissern Sie sich im Azure-Portal, dass Ihre Ressourcen erstellt wurden.

![Schnellstart: Ressourcen erstellt](./media/quickstart-arm-template-account/quickstart-arm-template-resources.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie nicht vorhaben, die eben erstellten Ressourcen zu verwenden, können Sie die Ressourcengruppe löschen.

```azurecli-interactive

az group delete --name {name of the resource group}

```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung einer ARM-Vorlage durch Befolgen des Prozesses zum Erstellen einer Vorlage mit Parametern, Variablen usw. finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Tutorial: Erstellen und Bereitstellen Ihrer ersten ARM-Vorlage](../../azure-resource-manager/templates/template-tutorial-create-first-template.md)