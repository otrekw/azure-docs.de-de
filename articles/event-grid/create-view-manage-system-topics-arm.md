---
title: Verwenden von Azure Resource Manager-Vorlagen zum Erstellen von Systemthemen in Azure Event Grid
description: In diesem Artikel wird beschrieben, wie Sie mit Azure Resource Manager-Vorlagen Systemthemen in Azure Event Grid erstellen können.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 1c8881a2d9dfca43084cc537b106e84b050a18d5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "86115161"
---
# <a name="create-system-topics-in-azure-event-grid-using-resource-manager-templates"></a>Erstellen von Systemthemen in Azure Event Grid mithilfe von Resource Manager-Vorlagen
In diesem Artikel wird beschrieben, wie Sie Systemthemen mithilfe von Resource Manager-Vorlagen erstellen und verwalten. Eine Übersicht über Systemthemen finden Sie unter [Systemthemen](system-topics.md).

## <a name="create-system-topic-first-and-then-create-event-subscription"></a>Erstellen eines Systemthemas und anschließendes Erstellen eines Ereignisabonnements
Um zunächst ein Systemthema in einer Azure-Quelle und anschließend ein Ereignisabonnement für das Thema zu erstellen, können Sie eine Vorlage wie die folgende verwenden: 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string",
            "defaultValue": "[concat('storage', uniqueString(resourceGroup().id))]",
            "metadata": {
                "description": "Provide a unique name for the Blob Storage account."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Provide a location for the Blob Storage account that supports Event Grid."
            }
        },
        "eventSubName": {
            "type": "string",
            "defaultValue": "subToStorage",
            "metadata": {
                "description": "Provide a name for the Event Grid subscription."
            }
        },
        "endpoint": {
            "type": "string",
            "metadata": {
                "description": "Provide the URL for the WebHook to receive events. Create your own endpoint for events."
            }
        },
        "systemTopicName": {
            "type": "String",
            "defaultValue": "mystoragesystemtopic",
            "metadata": {
                "description": "Provide a name for the system topic."
            }
        }
    },
    "resources": [
        {
            "name": "[parameters('storageName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2017-10-01",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "location": "[parameters('location')]",
            "tags": {},
            "properties": {
                "accessTier": "Hot"
            }
        },
        {
            "name": "[parameters('systemTopicName')]",
            "type": "Microsoft.EventGrid/systemTopics",
            "apiVersion": "2020-04-01-preview",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[parameters('storageName')]"
            ],
            "properties": {
                "source": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageName'))]",
                "topicType": "Microsoft.Storage.StorageAccounts"
            }
        },
        {
            "type": "Microsoft.EventGrid/systemTopics/eventSubscriptions",
            "apiVersion": "2020-04-01-preview",
            "name": "[concat(parameters('systemTopicName'), '/', parameters('eventSubName'))]",
            "dependsOn": [
                "[resourceId('Microsoft.EventGrid/systemTopics', parameters('systemTopicName'))]"
            ],
            "properties": {
                "destination": {
                    "properties": {
                        "endpointUrl": "[parameters('endpoint')]"
                    },
                    "endpointType": "WebHook"
                },
                "filter": {
                    "includedEventTypes": [
                        "Microsoft.Storage.BlobCreated",
                        "Microsoft.Storage.BlobDeleted"
                    ]
                }
            }
        }
    ]
}
```

Unter [Weiterleiten von Blob Storage-Ereignissen an einen Webendpunkt mithilfe einer Azure Resource Manager-Vorlage](blob-event-quickstart-template.md) finden Sie Anweisungen zum Erstellen von Systemthemen und zugehörigen Abonnements mithilfe von Resource Manager-Vorlagen. 

## <a name="create-system-topic-while-creating-an-event-subscription"></a>Erstellen eines Systemthemas bei der Erstellung eines Ereignisabonnements 
Wenn Sie bei der Erstellung eines Ereignisabonnements in einer Azure-Quelle implizit ein Systemthema erstellen möchten, können Sie die folgende Vorlage verwenden:

``` json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string",
            "defaultValue": "[concat('storage', uniqueString(resourceGroup().id))]",
            "metadata": {
                "description": "Provide a unique name for the Blob Storage account."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Provide a location for the Blob Storage account that supports Event Grid."
            }
        },
        "eventSubName": {
            "type": "string",
            "defaultValue": "subToStorage",
            "metadata": {
                "description": "Provide a name for the Event Grid subscription."
            }
        },
        "endpoint": {
            "type": "string",
            "metadata": {
                "description": "Provide the URL for the WebHook to receive events. Create your own endpoint for events."
            }
        }
    },
    "resources": [
        {
            "name": "[parameters('storageName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2017-10-01",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "location": "[parameters('location')]",
            "tags": {},
            "properties": {
                "accessTier": "Hot"
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
            "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/', parameters('eventSubName'))]",
            "apiVersion": "2018-01-01",
            "dependsOn": [
                "[parameters('storageName')]"
            ],
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "[parameters('endpoint')]"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false,
                    "includedEventTypes": [
                        "All"
                    ]
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte
Im Abschnitt [Systemthemen in Azure Event Grid](system-topics.md) erfahren Sie mehr über Systemthemen und Thementypen, die von Azure Event Grid unterstützt werden. 
