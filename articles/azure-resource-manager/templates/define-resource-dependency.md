---
title: Festlegen der Bereitstellungsreihenfolge für Ressourcen
description: Erfahren Sie, wie Sie während der Bereitstellung festlegen, dass eine Ressource von einer anderen Ressource abhängig ist. Die Abhängigkeiten stellen sicher, dass die Ressourcen in der richtigen Reihenfolge bereitgestellt werden.
ms.topic: conceptual
ms.date: 12/21/2020
ms.openlocfilehash: a96dca0ab30d0baee2688427d78867ea128e673a
ms.sourcegitcommit: a4533b9d3d4cd6bb6faf92dd91c2c3e1f98ab86a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2020
ms.locfileid: "97722010"
---
# <a name="define-the-order-for-deploying-resources-in-arm-templates"></a>Definieren der Reihenfolge für die Bereitstellung von Ressourcen in ARM-Vorlagen

Beim Bereitstellen einer Ressource müssen Sie möglicherweise sicherstellen, dass andere Ressourcen bereits vorhanden sind. So benötigen Sie beispielsweise einen logischen SQL-Server, bevor Sie eine Datenbank bereitstellen. Diese Beziehung richten Sie ein, indem Sie eine Ressource als von einer anderen Ressource abhängig kennzeichnen. Verwenden Sie das **dependsOn**-Element, um eine explizite Abhängigkeit zu definieren. Verwenden Sie die Funktionen **reference** oder **list**, um eine implizite Abhängigkeit zu definieren.

Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der Reihenfolge ihrer Abhängigkeiten bereit. Wenn Ressourcen nicht voneinander abhängig sind, stellt Resource Manager sie parallel bereit. Sie müssen nur Abhängigkeiten für Ressourcen definieren, die in der gleichen Vorlage bereitgestellt werden.

## <a name="dependson"></a>dependsOn

Innerhalb Ihrer Vorlage bietet das „dependsOn“-Element die Möglichkeit, eine Ressource als von einer oder mehreren Ressourcen abhängig zu definieren. Sein Wert ist ein JSON-Array von Zeichenfolgen, die jeweils einen Ressourcennamen oder eine ID darstellen. Das Array kann Ressourcen mit [bedingter Bereitstellung](conditional-resource-deployment.md) enthalten. Wenn eine bedingte Ressource nicht bereitgestellt wurde, entfernt Azure Resource Manager sie automatisch aus den erforderlichen Abhängigkeiten.

Das folgende Beispiel zeigt eine Netzwerkschnittstelle, die von einem virtuellen Netzwerk, einer Netzwerksicherheitsgruppe und einer öffentlichen IP-Adresse abhängig ist. Die vollständige Vorlage finden Sie in der [Schnellstartvorlage für eine Linux-VM](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-simple-linux/azuredeploy.json).

```json
{
    "type": "Microsoft.Network/networkInterfaces",
    "apiVersion": "2020-06-01",
    "name": "[variables('networkInterfaceName')]",
    "location": "[parameters('location')]",
    "dependsOn": [
      "[resourceId('Microsoft.Network/networkSecurityGroups/', parameters('networkSecurityGroupName'))]",
      "[resourceId('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
      "[resourceId('Microsoft.Network/publicIpAddresses/', variables('publicIpAddressName'))]"
    ],
    ...
}
```

Sie können Beziehungen zwischen Ihren Ressourcen mithilfe von „dependsOn“ zuordnen, dabei sollte Ihnen jedoch bewusst sein, welchen Zweck diese Zuordnung erfüllt. Um beispielsweise zu dokumentieren, wie Ressourcen miteinander verbunden sind, ist „dependsOn“ nicht der richtige Ansatz. Nach der Bereitstellung können Sie nicht mehr abfragen, welche Ressourcen im dependsOn-Element definiert waren. Das Festlegen unnötiger Abhängigkeiten verlangsamt die Bereitstellung, da Resource Manager diese Ressourcen nicht gleichzeitig bereitstellen kann.

## <a name="child-resources"></a>Untergeordnete Ressourcen

Es wird nicht automatisch eine implizite Bereitstellungsabhängigkeit zwischen einer [untergeordneten Ressource](child-resource-name-type.md) und der übergeordneten Ressource erstellt. Wenn Sie die untergeordnete Ressource nach der übergeordneten Ressource bereitstellen müssen, legen Sie die dependsOn-Eigenschaft fest.

Das folgende Beispiel zeigt einen logischen SQL-Server und eine Datenbank. Beachten Sie, dass eine explizite Abhängigkeit zwischen der Datenbank und dem Server definiert wird, obwohl die Datenbank ein untergeordnetes Element des Servers ist.

```json
"resources": [
  {
    "type": "Microsoft.Sql/servers",
    "apiVersion": "2020-02-02-preview",
    "name": "[parameters('serverName')]",
    "location": "[parameters('location')]",
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "type": "databases",
        "apiVersion": "2020-08-01-preview",
        "name": "[parameters('sqlDBName')]",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
          },
        "dependsOn": [
          "[resourceId('Microsoft.Sql/servers', concat(parameters('serverName')))]"
        ]
      }
    ]
  }
]
```

Die vollständige Vorlage finden Sie in der [Schnellstartvorlage für Azure SQL-Datenbank](https://github.com/Azure/azure-quickstart-templates/blob/master/101-sql-database/azuredeploy.json).

## <a name="reference-and-list-functions"></a>Funktionen „reference“ und „list“

Mit der [reference](template-functions-resource.md#reference) -Funktion kann ein Ausdruck seinen Wert von anderen Paaren aus JSON-Name und -Wert oder Laufzeitressourcen ableiten. Die [list*](template-functions-resource.md#list)-Funktionen geben Werte für eine Ressource aus einem list-Vorgang zurück.

reference- und list-Ausdrücke deklarieren implizit, dass eine Ressource von einer anderen abhängt. Verwenden Sie nach Möglichkeit einen impliziten Verweis, um das Hinzufügen einer unnötigen Abhängigkeit zu vermeiden.

Um eine implizite Abhängigkeit zu erzwingen, verweisen Sie über den Namen auf die Ressource und nicht über die Ressourcen-ID. Wenn Sie die Ressourcen-ID an die reference- oder list-Funktionen übergeben, wird kein impliziter Verweis erstellt.

Das allgemeine Format der reference-Funktion sieht folgendermaßen aus:

```json
reference('resourceName').propertyPath
```

Das allgemeine Format der listKeys-Funktion sieht folgendermaßen aus:

```json
listKeys('resourceName', 'yyyy-mm-dd')
```

Im folgenden Beispiel hängt ein CDN-Endpunkt explizit vom CDN-Profil und implizit von einer Web-App ab.

```json
{
    "name": "[variables('endpointName')]",
    "apiVersion": "2016-04-02",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "dependsOn": [
      "[variables('profileName')]"
    ],
    "properties": {
      "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
      ...
    }
```

Weitere Informationen finden Sie unter [reference-Funktion](template-functions-resource.md#reference).

## <a name="depend-on-resources-in-a-loop"></a>Abhängigkeit von Ressourcen in einer Schleife

Für das Bereitstellen von Ressourcen, die von Ressourcen in einer [Kopierschleife](copy-resources.md) abhängig sind, haben Sie zwei Möglichkeiten. Sie können eine Abhängigkeit von einzelnen Ressourcen in der Schleife oder von der gesamten Schleife festlegen.

> [!NOTE]
> In den meisten Szenarien sollten Sie die Abhängigkeit von einzelnen Ressourcen innerhalb der Kopierschleife festlegen. Legen Sie eine Abhängigkeit von der gesamten-Schleife nur dann fest, wenn alle Ressourcen in der Schleife vorhanden sein müssen, bevor Sie die nächste Ressource erstellen. Das Festlegen der Abhängigkeit von der gesamten Schleife führt zu einer erheblichen Erweiterung des Abhängigkeitsgraphen. Dies gilt insbesondere, wenn diese Schleifenressourcen wiederum von anderen Ressourcen abhängig sind. Die erweiterten Abhängigkeiten erschweren die effiziente Ausführung der Bereitstellung.

Im folgenden Beispiel wird veranschaulicht, wie Sie mehrere VMs bereitstellen. Die Vorlage erstellt die gleiche Anzahl von Netzwerkschnittstellen. Jede VM ist von einer Netzwerkschnittstelle abhängig und nicht von der gesamten Schleife.

```json
{
  "type": "Microsoft.Network/networkInterfaces",
  "apiVersion": "2020-05-01",
  "name": "[concat(variables('nicPrefix'),'-',copyIndex())]",
  "location": "[parameters('location')]",
  "copy": {
    "name": "nicCopy",
    "count": "[parameters('vmCount')]"
  },
  ...
},
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2020-06-01",
  "name": "[concat(variables('vmPrefix'),copyIndex())]",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[resourceId('Microsoft.Network/networkInterfaces',concat(variables('nicPrefix'),'-',copyIndex()))]"
  ],
  "copy": {
    "name": "vmCopy",
    "count": "[parameters('vmCount')]"
  },
  "properties": {
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(variables('nicPrefix'),'-',copyIndex()))]",
          "properties": {
            "primary": "true"
          }
        }
      ]
    },
    ...
  }
}
```

Das folgende Beispiel zeigt, wie drei Speicherkonten vor dem Bereitstellen des virtuellen Computers bereitgestellt werden. Beachten Sie, dass „name“ für das „copy“-Element auf `storagecopy` und auch das „dependsOn“-Element für die virtuellen Computer auf `storagecopy` festgelegt ist.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "copy": {
        "name": "storagecopy",
        "count": 3
      },
      "properties": {}
    },
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2015-06-15",
      "name": "[concat('VM', uniqueString(resourceGroup().id))]",
      "dependsOn": ["storagecopy"],
      ...
    }
  ],
  "outputs": {}
}
```

## <a name="circular-dependencies"></a>Ringabhängigkeiten

Resource Manager kennzeichnet Ringabhängigkeiten während der Überprüfung der Vorlage. Falls ein Fehler aufgrund einer Ringabhängigkeit ausgegeben wird, überprüfen Sie Ihre Vorlage, um festzustellen, ob bestimmte Abhängigkeiten entfernt werden können. Wenn das Entfernen der Abhängigkeiten nicht funktioniert, können Sie Ringabhängigkeiten vermeiden, indem Sie einige Bereitstellungsvorgänge in untergeordnete Ressourcen verschieben. Stellen Sie die untergeordneten Ressourcen nach den Ressourcen mit der Ringabhängigkeit bereit. Nehmen wir beispielsweise an, Sie stellen zwei virtuelle Computer bereit, müssen aber Eigenschaften festlegen, die auf den jeweils anderen verweisen. Sie können diese in der folgenden Reihenfolge bereitstellen:

1. VM1
2. VM2
3. Die Erweiterung auf VM1 hängt von VM1 und VM2 ab. Die Erweiterung legt Werte auf VM1 fest, die sie von VM2 abruft.
4. Die Erweiterung auf VM2 hängt von VM1 und VM2 ab. Die Erweiterung legt Werte auf VM2 fest, die sie von VM1 abruft.

Informationen über das Bewerten der Bereitstellungsreihenfolge und das Beheben von Abhängigkeitsfehlern finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](common-deployment-errors.md).

## <a name="next-steps"></a>Nächste Schritte

* Ein Tutorial, das Sie durcharbeiten können, finden Sie unter [Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen](template-tutorial-create-templates-with-dependent-resources.md).
* Ein Microsoft Learn-Modul, das Ressourcenabhängigkeiten behandelt, finden Sie unter [Verwalten komplexer Cloudbereitstellungen mithilfe erweiterter ARM-Vorlagenfunktionen](/learn/modules/manage-deployments-advanced-arm-template-features/).
* Empfehlungen zum Festlegen von Abhängigkeiten finden Sie unter [Bewährte Methoden für Azure Resource Manager-Vorlagen](template-best-practices.md).
* Informationen zur Behebung von Abhängigkeiten während der Bereitstellung finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](common-deployment-errors.md).
* Weitere Informationen zum Erstellen von Azure-Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Vorlagen](template-syntax.md).
* Eine Liste der verfügbaren Funktionen in einer Vorlage finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](template-functions.md).

