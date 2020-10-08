---
title: Bereitstellen auf einem dedizierten Host
description: Verwenden eines dedizierten Hosts, um eine echte Isolation auf Hostebene für Ihre Azure Container Instances-Workloads zu erreichen
ms.topic: article
ms.date: 01/17/2020
author: macolso
ms.author: macolso
ms.openlocfilehash: 967d2da61ffdfa9d1723bcab589deb2277d4041e
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91825603"
---
# <a name="deploy-on-dedicated-hosts"></a>Bereitstellen auf dedizierten Hosts

„Dediziert“ (Dedicated) ist eine Azure Container Instances-SKU (ACI), die eine isolierte und dedizierte Computeumgebung für die sichere Ausführung von Containern bereitstellt. Die Verwendung der dedizierten SKU führt dazu, dass jede Containergruppe über einen dedizierten physischen Server in einem Azure-Rechenzentrum verfügt, was eine vollständige Workloadisolation sicherstellt, um die Sicherheits- und Complianceanforderungen Ihrer Organisation zu erfüllen. 

Die dedizierte SKU eignet sich für Containerworkloads, die Workloadisolation aus Perspektive eines physischen Servers erfordern.

## <a name="prerequisites"></a>Voraussetzungen

* Das Standardlimit für die Verwendung der dedizierten SKU für jedes Abonnement ist 0. Wenn Sie diese SKU für Ihre Produktionscontainerbereitstellungen verwenden möchten, erstellen Sie eine [Azure-Supportanfrage][azure-support], um das Limit zu erhöhen.

## <a name="use-the-dedicated-sku"></a>Verwenden der dedizierten SKU

> [!IMPORTANT]
> Das Verwenden der dedizierten SKU ist nur in der neuesten API-Version (2019-12-01) verfügbar, deren Rollout momentan durchgeführt wird. Geben Sie diese API-Version in Ihrer Bereitstellungsvorlage an.
>

Ab API-Version 2019-12-01 gibt es eine `sku`-Eigenschaft im Abschnitt mit den Containergruppeneigenschaften einer Bereitstellungsvorlage, die für eine ACI-Bereitstellung erforderlich ist. Derzeit können Sie diese Eigenschaft als Teil einer Azure Resource Manager-Bereitstellungsvorlage für ACI verwenden. Weitere Informationen zum Bereitstellen von ACI-Ressourcen mit einer Vorlage finden Sie im [Tutorial: Bereitstellen einer Gruppe mit mehreren Containern über eine Resource Manager-Vorlage](./container-instances-multi-container-group.md). 

Die `sku`-Eigenschaft kann einen der folgenden Werte haben:
* `Standard`: Die standardmäßige ACI-Bereitstellungsauswahl, die immer noch Sicherheit auf Hypervisor-Ebene gewährleistet. 
* `Dedicated`: Wird für die Isolation auf Workloadebene mit dedizierten physischen Hosts für die Containergruppe verwendet.

## <a name="modify-your-json-deployment-template"></a>Ändern Ihrer JSON-Bereitstellungsvorlage

Ändern Sie in Ihrer Bereitstellungsvorlage die folgenden Eigenschaften, oder fügen Sie sie hinzu:
* Legen Sie unter `resources` `apiVersion` auf `2019-12-01` fest.
* Fügen Sie unter den Eigenschaften der Containergruppe eine `sku`-Eigenschaft mit dem Wert `Dedicated` hinzu.

Im Folgenden finden Sie einen Beispielcodeausschnitt für den Ressourcenabschnitt einer Containergruppen-Bereitstellungsvorlage, die die dedizierte SKU verwendet:

```json
[...]
"resources": [
    {
        "name": "[parameters('containerGroupName')]",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2019-12-01",
        "location": "[resourceGroup().location]",    
        "properties": {
            "sku": "Dedicated",
            "containers": {
                [...]
            }
        }
    }
]
```

Im Folgenden finden Sie eine vollständige Vorlage, die eine Beispielcontainergruppe bereitstellt, die eine einzelne Containerinstanz ausführt:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "containerGroupName": {
        "type": "string",
        "defaultValue": "myContainerGroup",
        "metadata": {
          "description": "Container Group name."
        }
      }
    },
    "resources": [
        {
            "name": "[parameters('containerGroupName')]",
            "type": "Microsoft.ContainerInstance/containerGroups",
            "apiVersion": "2019-12-01",
            "location": "[resourceGroup().location]",
            "properties": {
                "sku": "Dedicated",
                "containers": [
                    {
                        "name": "container1",
                        "properties": {
                            "image": "nginx",
                            "command": [
                                "/bin/sh",
                                "-c",
                                "while true; do echo `date`; sleep 1000000; done"
                            ],
                            "ports": [
                                {
                                    "protocol": "TCP",
                                    "port": 80
                                }
                            ],
                            "environmentVariables": [],
                            "resources": {
                                "requests": {
                                    "memoryInGB": 1.0,
                                    "cpu": 1
                                }
                            }
                        }
                    }
                ],
                "restartPolicy": "Always",
                "ipAddress": {
                    "ports": [
                        {
                            "protocol": "TCP",
                            "port": 80
                        }
                    ],
                    "type": "Public"
                },
                "osType": "Linux"
            },
            "tags": {}
        }
    ]
}
```

## <a name="deploy-your-container-group"></a>Bereitstellen Ihrer Containergruppe

Wenn Sie die Bereitstellungsvorlagendatei auf Ihrem Desktop erstellt und bearbeitet haben, können Sie sie in Ihr Cloud Shell-Verzeichnis hochladen, indem Sie sie dorthin ziehen. 

Erstellen Sie mithilfe des Befehls [az group create][az-group-create] eine Ressourcengruppe.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Stellen Sie die Vorlage mit dem Befehl [az deployment group create][az-deployment-group-create] bereit.

```azurecli-interactive
az deployment group create --resource-group myResourceGroup --template-file deployment-template.json
```

Innerhalb weniger Sekunden sollten Sie eine erste Antwort von Azure erhalten. Eine erfolgreiche Bereitstellung findet auf einem dedizierten Host statt.

<!-- LINKS - Internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-deployment-group-create]: /cli/azure/deployment/group#az-deployment-group-create

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
