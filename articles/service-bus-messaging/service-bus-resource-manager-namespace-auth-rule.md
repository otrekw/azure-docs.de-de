---
title: Erstellen einer Service Bus-Autorisierungsregel anhand einer Azure-Vorlage
description: Erstellen einer Service Bus-Autorisierungsregel für Namespaces und Warteschlangen mithilfe einer Azure Resource Manager-Vorlage
author: spelluru
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.date: 06/23/2020
ms.author: spelluru
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 6e26192b0a6473c40b05e2009be4b5593b6dcd9a
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112294227"
---
# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Erstellen einer Service Bus-Autorisierungsregel für Namespaces und Warteschlangen mithilfe einer Azure Resource Manager-Vorlage

In diesem Artikel wird beschrieben, wie Sie eine Azure Resource Manager-Vorlage verwenden, die eine [Autorisierungsregel](service-bus-authentication-and-authorization.md#shared-access-signature) für einen Service Bus-Namespace und eine Service Bus-Warteschlange erstellt. Dieser Artikel zeigt Ihnen, wie Sie angeben, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen][Authoring Azure Resource Manager templates].

Die vollständige Vorlage finden Sie unter [Service Bus authorization rule template][Service Bus auth rule template] (Service Bus-Autorisierungsregelvorlage) auf GitHub.

> [!NOTE]
> Die folgenden Azure Resource Manager-Vorlagen sind zum Download und zur Bereitstellung verfügbar.
>
> * [Erstellen eines Service Bus-Namespaces](service-bus-resource-manager-namespace.md)
> * [Create a Service Bus namespace and a queue using an Azure Resource Manager template](service-bus-resource-manager-namespace-queue.md)
> * [Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template (Erstellen eines Service Bus-Namespace mit Thema und Abonnement mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-topic.md)
> * [Create a Service Bus namespace with topic, subscription, and rule (Erstellen eines Service Bus-Namespace mit Thema, Abonnement und Regel)](service-bus-resource-manager-namespace-topic-with-rule.md)
>
> Die neuesten Vorlagen finden Sie, indem Sie im Katalog [Azure-Schnellstartvorlagen][Azure Quickstart Templates] nach **Service Bus** suchen.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?

Mit dieser Vorlage stellen Sie eine Service Bus-Autorisierungsregel für einen Namespace und eine Messagingentität (in diesem Fall eine Warteschlange) bereit.

Diese Vorlage verwendet [Shared Access Signature (SAS)](service-bus-sas.md) für die Authentifizierung. SAS ermöglicht Anwendungen die Authentifizierung bei Service Bus mithilfe eines Zugriffsschlüssels, der für den Namespace oder für die Messagingentität (Warteschlange oder Thema) konfiguriert wird, welcher bestimmte Rechte zugeordnet sind. Sie können diesen Schlüssel zum Generieren eines SAS-Tokens verwenden, das Clients wiederum für die Authentifizierung bei Service Bus verwenden können.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![In Azure bereitstellen](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.servicebus%2Fservicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure Resource Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters` , der alle Parameterwerte enthält. Definieren Sie einen Parameter für die Werte, die basierend auf dem bereitgestellten Projekt oder der bereitgestellten Umgebung variieren. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert wird in der Vorlage verwendet, um die bereitgestellten Ressourcen zu definieren.

Die Vorlage definiert die folgenden Parameter:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Der Name des zu erstellenden Service Bus-Namespace.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName

Der Name der Autorisierungsregel für den Namespace.

```json
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Der Name der Warteschlange im Service Bus-Namespace.

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Die Service Bus-API-Version der Vorlage.

```json
"serviceBusApiVersion": {
       "type": "string",
       "defaultValue": "2017-04-01",
       "metadata": {
           "description": "Service Bus ApiVersion used by the template"
       }
```

## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen

Erstellt einen standardmäßigen Service Bus-Namespace des Typs **Messaging** und eine Service Bus-Autorisierungsregel für Namespace und Entität.

```json
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "Standard",
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

JSON-Syntax und Eigenschaften finden Sie unter [namespaces](/azure/templates/microsoft.servicebus/namespaces), [queues](/azure/templates/microsoft.servicebus/namespaces/queues) und [AuthorizationRules](/azure/templates/microsoft.servicebus/namespaces/authorizationrules).

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```powershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/quickstarts/microsoft.servicebus/servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/quickstarts/microsoft.servicebus/servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun Ressourcen mit Azure Resource Manager erstellt und bereitgestellt haben, lesen Sie die folgenden Artikel, und erfahren Sie, wie Sie diese Ressourcen verwalten können:

* [Verwalten von Service Bus mit PowerShell](./service-bus-manage-with-ps.md)
* [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)
* [Service Bus-Authentifizierung und -Autorisierung](service-bus-authentication-and-authorization.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/templates/syntax.md
[Azure Quickstart Templates]: https://azure.microsoft.com/resources/templates/?term=service+bus
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/management/manage-resources-powershell.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/management/manage-resources-cli.md
[Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/quickstarts/microsoft.servicebus/servicebus-create-authrule-namespace-and-queue/