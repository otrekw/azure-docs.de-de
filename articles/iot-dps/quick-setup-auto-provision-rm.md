---
title: 'Schnellstart: Einrichten von Azure IoT Hub Device Provisioning mithilfe einer Azure Resource Manager-Vorlage'
description: 'Azure-Schnellstart: Einrichten von Azure IoT Hub Device Provisioning Service (DPS) mit einer Vorlage'
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 59b68730710de189d690c367e2c04e3a433c9af2
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2020
ms.locfileid: "94964744"
---
# <a name="quickstart-set-up-the-iot-hub-device-provisioning-service-with-an-azure-resource-manager-template"></a>Schnellstart: Einrichten des IoT Hub Device Provisioning-Diensts mit einer Azure Resource Manager-Vorlage

Sie können [Azure Resource Manager](../azure-resource-manager/management/overview.md) verwenden, um die Azure-Cloudressourcen programmgesteuert einzurichten, die zum Bereitstellen Ihrer Geräte erforderlich sind. In diesen Schritten wird gezeigt, wie Sie einen IoT-Hub und eine neue IoT Hub Device Provisioning Service-Instanz erstellen und diese beiden Komponenten mit einer Azure Resource Manager-Vorlage verknüpfen. In diesem Schnellstart wird die [Azure-Befehlszeilenschnittstelle](../azure-resource-manager/templates/deploy-cli.md) verwendet, um die erforderlichen programmgesteuerten Schritte zum Erstellen einer Ressourcengruppe und Bereitstellen der Vorlage auszuführen. Sie können aber auch problemlos das [Azure-Portal](../azure-resource-manager/templates/deploy-portal.md), [PowerShell](../azure-resource-manager/templates/deploy-powershell.md), .NET, Ruby oder andere Programmiersprachen verwenden, um diese Schritte auszuführen und Ihre Vorlage bereitzustellen. 


## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
- Für diese Schnellstartanleitung ist es erforderlich, dass Sie die Azure CLI lokal ausführen. Hierfür muss Azure CLI Version 2.0 oder höher installiert sein. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Installieren und Aktualisieren der Befehlszeilenschnittstelle finden Sie bei Bedarf unter [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).


## <a name="sign-in-to-azure-and-create-a-resource-group"></a>Anmelden an Azure und Erstellen einer Ressourcengruppe

Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.

1. Führen Sie an der Eingabeaufforderung den [Anmeldebefehl][lnk-login-command] aus:
    
    ```azurecli
    az login
    ```

    Befolgen Sie die Anweisungen zur Authentifizierung mit dem Code, und melden Sie sich in einem Webbrowser bei Ihrem Azure-Konto an.

2. Wenn Sie über mehrere Azure-Abonnements verfügen, erhalten Sie durch die Anmeldung bei Azure Zugriff auf alle Azure-Konten, die mit Ihren Anmeldeinformationen verknüpft sind. Führen Sie den folgenden [Befehl aus, um eine Liste der Azure-Konten anzuzeigen][lnk-az-account-command], die Sie verwenden können:
    
    ```azurecli
    az account list 
    ```

    Führen Sie den folgenden Befehl aus, um das Abonnement auszuwählen, das Sie zum Ausführen der Befehle zum Erstellen Ihres IoT Hubs verwenden möchten. Sie können entweder den Abonnementnamen oder die ID aus der Ausgabe des vorherigen Befehls verwenden:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

3. Wenn Sie Azure-Cloudressourcen wie IoT Hubs und Bereitstellungsdienste erstellen, führen Sie dies in einer Ressourcengruppe durch. Verwenden Sie entweder eine vorhandene Ressourcengruppe, oder führen Sie den folgenden [Befehl zum Erstellen einer Ressourcengruppe][lnk-az-resource-command] aus:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > Im vorherigen Beispiel wird die Ressourcengruppe am Standort „USA, Westen“ erstellt. Mit dem Befehl `az account list-locations -o table` können Sie eine Liste der verfügbaren Standorte anzeigen.
    >
    >

## <a name="create-a-resource-manager-template"></a>Erstellen einer Resource Manager-Vorlage

Verwenden Sie eine JSON-Vorlage, um in Ihrer Ressourcengruppe einen Bereitstellungsdienst und einen verknüpften IoT Hub zu erstellen. Sie können auch eine Azure Resource Manager-Vorlage verwenden, um Änderungen an einem vorhandenen Bereitstellungsdienst oder IoT Hub vorzunehmen.

1. Nutzen Sie einen Text-Editor, um eine Azure Resource Manager-Vorlage mit dem Namen **template.json** und dem folgenden Inhaltsgerüst zu erstellen. 

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {},
       "variables": {},
       "resources": []
   }
   ```

2. Ersetzen Sie den Abschnitt **parameters** durch den folgenden Inhalt. Im Abschnitt „parameters“ werden Parameter definiert, deren Werte aus einer anderen Datei übergeben werden können. In diesem Abschnitt werden die Namen des zu erstellenden IoT-Hubs und des Bereitstellungsdiensts definiert. Außerdem wird der Standort für den IoT-Hub und den Bereitstellungsdienst definiert. Die Werte sind auf Azure-Regionen beschränkt, in denen IoT-Hubs und Bereitstellungsdienste unterstützt werden. Eine Liste mit unterstützten Standorten für den Device Provisioning-Dienst erhalten Sie, indem Sie den Befehl `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` ausführen oder auf der Seite [Azure-Status](https://azure.microsoft.com/status/) nach „Device Provisioning-Dienst“ suchen.

   ```json
    "parameters": {
        "iotHubName": {
            "type": "string"
        },
        "provisioningServiceName": {
            "type": "string"
        },
        "hubLocation": {
            "type": "string",
            "allowedValues": [
                "eastus",
                "westus",
                "westeurope",
                "northeurope",
                "southeastasia",
                "eastasia"
            ]
        }
    },

   ```

3. Ersetzen Sie den Abschnitt **variables** durch den folgenden Inhalt. In diesem Abschnitt werden die Werte definiert, die später verwendet werden, um die IoT Hub-Verbindungszeichenfolge zu erstellen. Sie wird zum Verknüpfen des Bereitstellungsdiensts und des IoT Hub benötigt. 
 
   ```json
    "variables": {        
        "iotHubResourceId": "[resourceId('Microsoft.Devices/Iothubs', parameters('iotHubName'))]",
        "iotHubKeyName": "iothubowner",
        "iotHubKeyResource": "[resourceId('Microsoft.Devices/Iothubs/Iothubkeys', parameters('iotHubName'), variables('iotHubKeyName'))]"
    },

   ```

4. Fügen Sie der Sammlung **resources** die folgenden Zeilen hinzu, um einen IoT Hub zu erstellen. Im JSON-Code werden die Mindesteigenschaften angegeben, die für die Erstellung eines IoT-Hubs erforderlich sind. Die Werte für den **Namen** und den **Standort** werden als Parameter aus einer anderen Datei übergeben. Weitere Informationen zu den Eigenschaften, die Sie in einer Vorlage für einen IoT-Hub angeben können, finden Sie unter [Microsoft.Devices/IotHubs template reference](/azure/templates/microsoft.devices/iothubs) (Microsoft.Devices/IotHubs-Vorlagenreferenz).

   ```json
        {
            "apiVersion": "2017-07-01",
            "type": "Microsoft.Devices/IotHubs",
            "name": "[parameters('iotHubName')]",
            "location": "[parameters('hubLocation')]",
            "sku": {
                "name": "S1",
                "capacity": 1
            },
            "tags": {
            },
            "properties": {
            }            
        },

   ``` 

5. Fügen Sie nach der IoT Hub-Angabe in der Sammlung **resources** die folgenden Zeilen hinzu, um den Bereitstellungsdienst zu erstellen. **Name** und **Standort** des Bereitstellungsdiensts werden als Parameter übergeben. In der Sammlung **iotHubs** sind die IoT-Hubs angegeben, die mit dem Bereitstellungsdienst verknüpft werden sollen. Sie müssen für jeden verknüpften IoT Hub mindestens die Eigenschaften **connectionString** und **location** angeben. Sie können für jeden IoT Hub auch Eigenschaften wie **allocationWeight** und **applyAllocationPolicy** sowie Eigenschaften wie **allocationPolicy** und **authorizationPolicies** für den Bereitstellungsdienst selbst festlegen. Weitere Informationen finden Sie unter [Microsoft.Devices/provisioningServices template reference](/azure/templates/microsoft.devices/provisioningservices) (Microsoft.Devices/provisioningServices-Vorlagenreferenz).

   Mit der **dependsOn**-Eigenschaft wird sichergestellt, dass mit Resource Manager der IoT Hub vor dem Bereitstellungsdienst erstellt wird. Für die Vorlage ist es erforderlich, dass die Verbindungszeichenfolge des IoT Hub die Verknüpfung mit dem Bereitstellungsdienst angibt. Der Hub und die dazugehörigen Schlüssel müssen also zuerst erstellt werden. Für die Vorlage werden Funktionen wie **concat** und **listKeys** zum Erstellen der Verbindungszeichenfolge aus parametrisierten Variablen verwendet. Weitere Informationen finden Sie unter [Vorlagenfunktionen im Azure Resource Manager](../azure-resource-manager/templates/template-functions.md).

   ```json
        {
            "type": "Microsoft.Devices/provisioningServices",
            "sku": {
                "name": "S1",
                "capacity": 1
            },
            "name": "[parameters('provisioningServiceName')]",
            "apiVersion": "2017-11-15",
            "location": "[parameters('hubLocation')]",
            "tags": {},
            "properties": {
                "iotHubs": [
                    {
                        "connectionString": "[concat('HostName=', reference(variables('iotHubResourceId')).hostName, ';SharedAccessKeyName=', variables('iotHubKeyName'), ';SharedAccessKey=', listkeys(variables('iotHubKeyResource'), '2017-07-01').primaryKey)]",
                        "location": "[parameters('hubLocation')]",
                        "name": "[concat(parameters('iotHubName'),'.azure-devices.net')]"
                    }
                ]
            },
            "dependsOn": ["[parameters('iotHubName')]"]
        }

   ```

6. Speichern Sie die Vorlagendatei. Die fertige Vorlage sollte wie folgt aussehen:

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
           "iotHubName": {
               "type": "string"
           },
           "provisioningServiceName": {
               "type": "string"
           },
           "hubLocation": {
               "type": "string",
               "allowedValues": [
                   "eastus",
                   "westus",
                   "westeurope",
                   "northeurope",
                   "southeastasia",
                   "eastasia"
               ]
           }
       },
       "variables": {        
           "iotHubResourceId": "[resourceId('Microsoft.Devices/Iothubs', parameters('iotHubName'))]",
           "iotHubKeyName": "iothubowner",
           "iotHubKeyResource": "[resourceId('Microsoft.Devices/Iothubs/Iothubkeys', parameters('iotHubName'), variables('iotHubKeyName'))]"
       },
       "resources": [
           {
               "apiVersion": "2017-07-01",
               "type": "Microsoft.Devices/IotHubs",
               "name": "[parameters('iotHubName')]",
               "location": "[parameters('hubLocation')]",
               "sku": {
                   "name": "S1",
                   "capacity": 1
               },
               "tags": {
               },
               "properties": {
               }            
           },
           {
               "type": "Microsoft.Devices/provisioningServices",
               "sku": {
                   "name": "S1",
                   "capacity": 1
               },
               "name": "[parameters('provisioningServiceName')]",
               "apiVersion": "2017-11-15",
               "location": "[parameters('hubLocation')]",
               "tags": {},
               "properties": {
                   "iotHubs": [
                       {
                           "connectionString": "[concat('HostName=', reference(variables('iotHubResourceId')).hostName, ';SharedAccessKeyName=', variables('iotHubKeyName'), ';SharedAccessKey=', listkeys(variables('iotHubKeyResource'), '2017-07-01').primaryKey)]",
                           "location": "[parameters('hubLocation')]",
                           "name": "[concat(parameters('iotHubName'),'.azure-devices.net')]"
                       }
                   ]
               },
               "dependsOn": ["[parameters('iotHubName')]"]
           }
       ]
   }
   ```

## <a name="create-a-resource-manager-parameter-file"></a>Erstellen einer Resource Manager-Parameterdatei

Für die Vorlage, die Sie im letzten Schritt definiert haben, werden Parameter zum Angeben des Namens des IoT-Hubs, des Namens des Bereitstellungsdiensts und des Standorts (Azure-Region) für die Erstellung verwendet. Sie übergeben diese Parameter in einer separaten Datei an die Vorlage. Auf diese Weise können Sie dieselbe Vorlage für mehrere Bereitstellungen wiederverwenden. Führen Sie diese Schritte aus, um die Parameterdatei zu erstellen:

1. Nutzen Sie einen Text-Editor, um eine Azure Resource Manager-Parameterdatei mit dem Namen **parameters.json** und dem folgenden Inhaltsgerüst zu erstellen: 

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {}
       }
   }
   ```

2. Fügen Sie dem Abschnitt „parameter“ den Wert **iotHubName** hinzu.  Ein IoT-Hub-Name muss in Azure global eindeutig sein, daher können Sie dem Beispielnamen ein eindeutiges Präfix oder Suffix hinzufügen oder einen ganz neuen Namen auswählen. Achten Sie darauf, dass der Name den Benennungskonventionen für IoT-Hubs entspricht: Er muss 3 bis 50 Zeichen lang sein und darf nur alphanumerische Zeichen (Klein- und Großbuchstaben) oder Bindestriche („-“) enthalten. 

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
    }
   
   ```

3. Fügen Sie dem Abschnitt „parameter“ den Wert **provisioningServiceName** hinzu. Außerdem müssen Sie einen global eindeutigen Namen für Ihren Bereitstellungsdienst auswählen. Achten Sie darauf, dass er den Namenskonventionen für IoT Hub Device Provisioning Service entspricht: Er muss 3 bis 64 Zeichen lang sein und darf nur alphanumerische Zeichen (Klein- und Großbuchstaben) oder Bindestriche („-“) enthalten.

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
        "provisioningServiceName": {
            "value": "my-sample-provisioning-service"
        },
    }

   ```

4. Fügen Sie dem Abschnitt „parameter“ den Wert **hubLocation** hinzu. Mit diesem Wert wird außerdem der Standort für den IoT Hub und den Bereitstellungsdienst angegeben. Der Wert muss einem der Standorte entsprechen, die in der Sammlung **allowedValues** in der Parameterdefinition der Vorlagendatei angegeben sind. Mit dieser Sammlung werden die Werte auf Azure-Standorte beschränkt, für die sowohl IoT Hubs als auch Bereitstellungsdienste unterstützt werden. Eine Liste mit unterstützten Standorten für Device Provisioning Service erhalten Sie, indem Sie den Befehl `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` ausführen oder auf der Seite [Azure-Status](https://azure.microsoft.com/status/) nach „Device Provisioning Service“ suchen.

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
        "provisioningServiceName": {
            "value": "my-sample-provisioning-service"
        },
        "hubLocation": {
            "value": "westus"
        }
    }

   ```

5. Speichern Sie die Datei . 


> [!IMPORTANT]
> Da sowohl der IoT Hub als auch der Bereitstellungsdienst öffentlich als DNS-Endpunkte ermittelbar sind, sollten Sie sicherstellen, dass Sie beim Vergeben der Namen keine sensiblen Informationen verwenden.
>

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Verwenden Sie die folgenden Azure CLI-Befehle, um Ihre Vorlagen bereitzustellen und die Bereitstellung zu überprüfen.

1. Navigieren Sie zum Bereitstellen der Vorlage zu dem Ordner, der die Vorlage und die Parameterdateien enthält, und führen Sie den folgenden [Befehl aus, um eine Bereitstellung zu starten](/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create&preserve-view=true):
    
    ```azurecli
     az group deployment create -g {your resource group name} --template-file template.json --parameters @parameters.json
    ```

   Dieser Vorgang kann einige Minuten in Anspruch nehmen. Suchen Sie anschließend in der Ausgabe nach dem Wert „Succeeded“ (Erfolgreich) für die **provisioningState**-Eigenschaft. 

   ![Bereitstellen der Ausgabe](./media/quick-setup-auto-provision-rm/output.png) 


2. Führen Sie zum Überprüfen Ihrer Bereitstellung den folgenden [Befehl zum Auflisten von Ressourcen](/cli/azure/resource?view=azure-cli-latest#az-resource-list&preserve-view=true) aus, und suchen Sie in der Ausgabe nach dem neuen Bereitstellungsdienst und dem IoT Hub:

    ```azurecli
     az resource list -g {your resource group name}
    ```


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstarts in dieser Sammlung bauen auf diesem Schnellstart auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts oder Tutorials fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie über die Azure-Befehlszeilenschnittstelle [eine einzelne Ressource löschen][lnk-az-resource-command], z. B. einen IoT-Hub oder einen Bereitstellungsdienst, oder eine Ressourcengruppe und alle zugehörigen Ressourcen löschen.

Führen Sie den folgenden Befehl aus, um den Bereitstellungsdienst zu löschen:

```azurecli
az iot hub delete --name {your provisioning service name} --resource-group {your resource group name}
```
Führen Sie den folgenden Befehl aus, um einen IoT Hub zu löschen:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

Führen Sie den folgenden Befehl aus, um eine Ressourcengruppe und alle zugehörigen Ressourcen zu löschen:

```azurecli
az group delete --name {your resource group name}
```

Sie können auch Ressourcengruppen und einzelne Ressourcen löschen, indem Sie das Azure-Portal, PowerShell oder REST-APIs oder auch unterstützte Plattform-SDKs verwenden, die für Azure Resource Manager oder IoT Hub Device Provisioning Service veröffentlicht werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen IoT-Hub und einen Gerätebereitstellungsdienst bereitgestellt und die beiden Ressourcen verknüpft. Sie können mit dem Schnellstart zur Erstellung eines simulierten Geräts fortfahren, um zu erfahren, wie Sie diese Einrichtung zum Bereitstellen eines simulierten Geräts verwenden.

> [!div class="nextstepaction"]
> [Schnellstart zum Erstellen eines simulierten Geräts](./quick-create-simulated-device.md)


<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: /cli/azure/install-az-cli2
[lnk-login-command]: /cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: /cli/azure/account
[lnk-az-register-command]: /cli/azure/provider
[lnk-az-addcomponent-command]: /cli/azure/component
[lnk-az-resource-command]: /cli/azure/resource
[lnk-az-iot-command]: /cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md