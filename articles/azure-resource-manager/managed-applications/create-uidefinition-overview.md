---
title: Datei „CreateUiDefinition.json“ für den Portalbereich
description: Hier wird das Erstellen von Benutzeroberflächendefinitionen für die Azure-Portal beschrieben. Wird zum Definieren von Azure Managed Applications-Instanzen verwendet.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: tomfitz
ms.openlocfilehash: 4ee489e8b596adf0767856e3358c9bdcb17fbb6a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87004363"
---
# <a name="createuidefinitionjson-for-azure-managed-applications-create-experience"></a>Die Datei „CreateUiDefinition.json“ für die Benutzeroberfläche zum Erstellen verwalteter Azure-Anwendungen

In diesem Dokument werden die grundlegenden Konzepte der Datei **createUiDefinition.json** vorgestellt. Das Azure-Portal verwendet diese Datei zum Definieren der Benutzeroberfläche beim Erstellen einer verwalteten Anwendung.

Die Vorlage sieht wie folgt aus:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Azure.CreateUIDef",
    "version": "0.1.2-preview",
    "parameters": {
        "config": {
            "basics": { }
        },
        "basics": [ ],
        "steps": [ ],
        "outputs": { },
        "resourceTypes": [ ]
    }
}
```

Ein CreateUiDefinition-Element enthält immer drei Eigenschaften: 

* handler
* version
* parameters

Der Handler muss immer `Microsoft.Azure.CreateUIDef` lauten, und die neueste unterstützte Version ist `0.1.2-preview`.

Das Schema der parameters-Eigenschaft hängt von der Kombination aus den angegebenen Werten für „handler“ und „version“ ab. Für verwaltete Anwendungen lauten die unterstützten Eigenschaften `basics`, `steps`, `outputs` und `config`. Die Eigenschaften „basics“ und „steps“ enthalten die [Elemente ](create-uidefinition-elements.md) (wie Textfelder und Dropdownfelder), die im Azure-Portal angezeigt werden sollen. Mit der outputs-Eigenschaft werden die Ausgabewerte der angegebenen Elemente den Parametern der Azure Resource Manager-Vorlage zugeordnet. Sie verwenden `config` nur, wenn Sie das Standardverhalten des `basics`-Schritts außer Kraft setzen müssen.

Die Aufnahme von `$schema` wird empfohlen, ist aber optional. Wenn ein Wert angegeben wird, muss der Wert für `version` der Version im `$schema`-URI entsprechen.

Sie können einen JSON-Editor zum Erstellen von „createUiDefinition“ verwenden, und sie dann in der [Sandbox für „createUiDefinition“](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade) testen, um eine Vorschau davon anzuzeigen. Weitere Informationen zur Sandbox finden Sie unter [Testen Ihrer Portaloberfläche für Azure Managed Applications](test-createuidefinition.md).

## <a name="basics"></a>Grundlagen

Der **Basics**-Schritt ist der erste Schritt, der generiert wird, wenn das Azure-Portal die Datei analysiert. Standardmäßig kann der Benutzer im Basics-Schritt das Abonnement, die Ressourcengruppe und den Speicherort für die Bereitstellungsvorlage auswählen.

:::image type="content" source="./media/create-uidefinition-overview/basics.png" alt-text="Basics-Standard":::

In diesem Abschnitt können Sie weitere Elemente hinzufügen. Fügen Sie nach Möglichkeit Elemente hinzu, die bereitstellungsweite Parameter abfragen, z. B. den Namen eines Clusters oder Administratoranmeldeinformationen.

Das folgende Beispiel zeigt ein Textfeld, das den Standardelementen hinzugefügt wurde.

```json
"basics": [
    {
        "name": "textBox1",
        "type": "Microsoft.Common.TextBox",
        "label": "Textbox on basics",
        "defaultValue": "my text value",
        "toolTip": "",
        "visible": true
    }
]
```

## <a name="config"></a>Config

Sie geben das Config-Element an, wenn Sie das Standardverhalten für die Basics-Schritte überschreiben müssen. Im folgenden Beispiel wird die verfügbare Eigenschaft gezeigt.

```json
"config": {  
    "basics": {  
        "description": "Customized description with **markdown**, see [more](https://www.microsoft.com).",
        "subscription": {
            "constraints": {
                "validations": [
                    {
                        "isValid": "[expression for checking]",
                        "message": "Please select a valid subscription."
                    },
                    {
                        "permission": "<Resource Provider>/<Action>",
                        "message": "Must have correct permission to complete this step."
                    }
                ]
            },
            "resourceProviders": [ "<Resource Provider>" ]
        },
        "resourceGroup": {
            "constraints": {
                "validations": [
                    {
                        "isValid": "[expression for checking]",
                        "message": "Please select a valid resource group."
                    }
                ]
            },
            "allowExisting": true
        },
        "location": {  
            "label": "Custom label for location",  
            "toolTip": "provide a useful tooltip",  
            "resourceTypes": [ "Microsoft.Compute/virtualMachines" ],
            "allowedValues": [ "eastus", "westus2" ],  
            "visible": true  
        }  
    }  
},  
```

Geben Sie für `description` eine markdownaktivierte Zeichenfolge an, in der Ihre Ressource beschrieben wird. Mehrzeilige Formate und Verknüpfungen werden unterstützt.

Geben Sie für `location` die Eigenschaften für das Standort-Steuerelement an, das Sie überschreiben möchten. Alle nicht überschriebenen Eigenschaften werden auf ihre Standardwerte festgelegt. `resourceTypes` akzeptiert ein Array von Zeichenfolgen, das vollqualifizierte Ressourcentypnamen enthält. Die Standortoptionen sind ausschließlich auf Regionen beschränkt, die die Ressourcentypen unterstützen.  `allowedValues`  akzeptiert ein Array von Regionszeichenfolgen. Nur diese Regionen werden in der Dropdownliste angezeigt. Sie können sowohl `allowedValues` als auch   `resourceTypes` festlegen. Das Ergebnis ist die Schnittmenge beider Listen. Schließlich kann die `visible`-Eigenschaft verwendet werden, um das Standortdropdown bedingt oder vollständig zu deaktivieren.  

Die Elemente `subscription` und `resourceGroup` ermöglichen es Ihnen, zusätzliche Validierungen anzugeben. Die Syntax zum Angeben von Validierungen ist identisch mit der benutzerdefinierten Validierung für ein [Textfeld](microsoft-common-textbox.md). Sie können auch `permission`-Validierungen für das Abonnement oder die Ressourcengruppe angeben.  

Das Abonnement-Steuerelement (subscription) akzeptiert eine Liste mit Ressourcenanbieter-Namespaces. Beispielsweise können Sie **Microsoft.Compute** angeben. Es zeigt eine Fehlermeldung an, wenn der Benutzer ein Abonnement auswählt, das den Ressourcenanbieter nicht unterstützt. Der Fehler tritt auf, wenn der Ressourcenanbieter für dieses Abonnement nicht registriert ist und der Benutzer nicht über die Berechtigung zum Registrieren des Ressourcenanbieters verfügt.  

Das Ressourcengruppen-Steuerelement verfügt über eine Option für `allowExisting`. Wenn diese `true` ist, können die Benutzer Ressourcengruppen auswählen, die bereits über Ressourcen verfügen. Dieses Flag lässt sich am besten auf Projektmappenvorlagen anwenden, bei denen das Standardverhalten erfordert, dass Benutzer eine neue oder leere Ressourcengruppe auswählen müssen. In den meisten anderen Szenarien ist die Angabe dieser Eigenschaft nicht erforderlich.  

## <a name="steps"></a>Schritte

Die steps-Eigenschaft (Schritte) enthält null oder mehr zusätzliche Schritte, die nach dem „basics“-Schritt angezeigt werden sollen. Jeder Schritt enthält mindestens ein Element. Ziehen Sie das Hinzufügen von steps-Elementen pro Rolle oder Ebene der bereitgestellten Anwendung in Betracht. Fügen Sie beispielsweise ein „steps“-Element für Masterknoteneingaben und ein „steps“-Element für die Workerknoten in einem Cluster hinzu.

```json
"steps": [
    {
        "name": "demoConfig",
        "label": "Configuration settings",
        "elements": [
          ui-elements-needed-to-create-the-instance
        ]
    }
]
```

## <a name="outputs"></a>Ausgaben

Mit der `outputs`-Eigenschaft ordnet das Azure-Portal Elemente aus `basics` und `steps` den Parametern der Azure Resource Manager-Bereitstellungsvorlage zu. Die Schlüssel dieses Wörterbuchs stellen die Namen der Vorlagenparameter dar, und die Werte sind Eigenschaften der Ausgabeobjekte von den referenzierten Elementen.

Um den Ressourcennamen der verwalteten Anwendung festzulegen, müssen Sie einen Wert mit dem Namen `applicationResourceName` in die Ausgabeeigenschaft eingeben. Wenn Sie diesen Wert nicht festlegen, weist die Anwendung eine GUID für den Namen zu. Sie können ein Textfeld in die Benutzeroberfläche aufnehmen, das einen Namen vom Benutzer anfordert.

```json
"outputs": {
    "vmName": "[steps('appSettings').vmName]",
    "trialOrProduction": "[steps('appSettings').trialOrProd]",
    "userName": "[steps('vmCredentials').adminUsername]",
    "pwd": "[steps('vmCredentials').vmPwd.password]",
    "applicationResourceName": "[steps('appSettings').vmName]"
}
```

## <a name="resource-types"></a>Ressourcentypen

Um die verfügbaren Standorte nach denen zu filtern, die die bereitzustellenden Ressourcentypen unterstützen, geben Sie ein Array der Ressourcentypen an. Wenn Sie mehrere Ressourcentypen angeben, werden nur die Standorte zurückgegeben, die alle Ressourcentypen unterstützen. Diese Eigenschaft ist optional.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Azure.CreateUIDef",
    "version": "0.1.2-preview",
    "parameters": {
        "resourceTypes": ["Microsoft.Compute/disks"],
        "basics": [
          ...
```  

## <a name="functions"></a>Functions

„CreateUiDefinition“ stellt [Funktionen](create-uidefinition-functions.md) zum Arbeiten mit den Eingaben und Ausgaben von Elementen sowie Features wie konditionelle Abschnitte bereit. Diese Funktionen sind sowohl im Hinblick auf die Syntax als auch auf die Funktionalität den Vorlagenfunktionen von Azure Resource Manager ähnlich.

## <a name="next-steps"></a>Nächste Schritte

Die Datei „createUiDefinition.json“ besitzt selbst ein einfaches Schema. Die tatsächliche Tiefe ergibt sich aus allen unterstützten Elementen und Funktionen. Diese Elemente werden in den folgenden Dokumenten ausführlicher beschrieben:

- [CreateUiDefinition-Elemente](create-uidefinition-elements.md)
- [Funktionen](create-uidefinition-functions.md)

Ein aktuelles JSON-Schema für createUiDefinition ist unter folgendem Link verfügbar: `https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json`.

Ein Beispiel für eine Benutzeroberflächendatei finden Sie unter [createUiDefinition.json](https://github.com/Azure/azure-managedapp-samples/blob/master/Managed%20Application%20Sample%20Packages/201-managed-app-using-existing-vnet/createUiDefinition.json).
