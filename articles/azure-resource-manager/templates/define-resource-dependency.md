---
title: Festlegen der Bereitstellungsreihenfolge für Ressourcen
description: Beschreibt, wie während der Bereitstellung festlegt wird, dass eine Ressource von einer anderen Ressource abhängt, um sicherzustellen, dass die Ressourcen in der richtigen Reihenfolge bereitgestellt werden.
ms.topic: conceptual
ms.date: 12/03/2019
ms.openlocfilehash: 44cf793859d2817695a58bd1159e2f4465c1f9c2
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76121963"
---
# <a name="define-the-order-for-deploying-resources-in-azure-resource-manager-templates"></a>Definieren der Reihenfolge für die Bereitstellung von Ressourcen in Azure Resource Manager-Vorlagen

Beim Bereitstellen einer Ressource müssen Sie möglicherweise sicherstellen, dass andere Ressourcen bereits vorhanden sind. So benötigen Sie beispielsweise eine SQL Server-Instanz, bevor Sie eine SQL-Datenbank bereitstellen. Diese Beziehung definieren Sie, indem Sie eine Ressource als von einer anderen Ressource abhängig kennzeichnen. Sie definieren eine Abhängigkeit mit dem **dependsOn**-Element oder mit der **reference**-Funktion.

Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der Reihenfolge ihrer Abhängigkeiten bereit. Wenn Ressourcen nicht voneinander abhängig sind, stellt Resource Manager sie parallel bereit. Sie müssen nur Abhängigkeiten für Ressourcen definieren, die in der gleichen Vorlage bereitgestellt werden.

## <a name="dependson"></a>dependsOn

Innerhalb Ihrer Vorlage bietet das „dependsOn“-Element die Möglichkeit, eine Ressource als von einer oder mehreren Ressourcen abhängig zu definieren. Der Wert ist eine durch Trennzeichen getrennte Liste mit Ressourcennamen. Die Liste kann Ressourcen mit [bedingter Bereitstellung](conditional-resource-deployment.md) enthalten. Wenn eine bedingte Ressource nicht bereitgestellt wurde, entfernt Azure Resource Manager sie automatisch aus den erforderlichen Abhängigkeiten.

Das folgende Beispiel zeigt eine VM-Skalierungsgruppe, die abhängig von einem Load Balancer, einem virtuellen Netzwerk und einer Schleife ist, die mehrere Speicherkonten erstellt. Diese anderen Ressourcen werden im folgenden Beispiel nicht gezeigt, müssen jedoch anderswo in der Vorlage vorhanden sein.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "apiVersion": "2016-03-30",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

Im vorherigen Beispiel ist eine Abhängigkeit für die Ressourcen enthalten, die durch eine Kopierschleife mit dem Namen **storageLoop** erstellt werden. Ein Beispiel finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](create-multiple-instances.md).

Wenn Sie Abhängigkeiten definieren, können Sie Ressourcenanbieter-Namespace und Ressourcentyp einbeziehen, um Mehrdeutigkeiten zu vermeiden. Um z.B. einen Lastenausgleich und ein virtuelles Netzwerk eindeutig darzustellen, die möglicherweise die gleichen Namen wie andere Ressourcen haben, verwenden Sie folgendes Format:

```json
"dependsOn": [
  "[resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName'))]",
  "[resourceId('Microsoft.Network/virtualNetworks', variables('virtualNetworkName'))]"
]
```

Sie können Beziehungen zwischen Ihren Ressourcen mithilfe von „dependsOn“ zuordnen, dabei sollte Ihnen jedoch bewusst sein, welchen Zweck diese Zuordnung erfüllt. Um beispielsweise zu dokumentieren, wie Ressourcen miteinander verbunden sind, ist „dependsOn“ nicht der richtige Ansatz. Nach der Bereitstellung können Sie nicht mehr abfragen, welche Ressourcen im dependsOn-Element definiert waren. Durch Verwendung von „dependsOn“ beeinträchtigen Sie möglicherweise die Bereitstellungszeit, da Resource Manager zwei Ressourcen, die eine Abhängigkeit aufweisen, nicht parallel bereitstellt.

## <a name="child-resources"></a>Untergeordnete Ressourcen

Mit der resources-Eigenschaft können Sie untergeordnete Ressourcen angeben, die mit der definierten Ressource verknüpft sind. Untergeordnete Ressourcen können nur mit fünf Ebenen definiert werden. Es ist wichtig zu beachten, dass keine implizite Bereitstellungsabhängigkeit zwischen einer untergeordneten Ressource und der übergeordneten Ressource erstellt wird. Wenn die untergeordnete Ressource nach der übergeordneten Ressource bereitgestellt werden muss, müssen Sie diese Abhängigkeit explizit mit der dependsOn-Eigenschaft angeben.

Jede übergeordnete Ressource akzeptiert nur bestimmte Ressourcentypen als untergeordnete Ressourcen. Die akzeptierten Ressourcentypen werden im [Vorlagenschema](https://github.com/Azure/azure-resource-manager-schemas) der übergeordneten Ressource angegeben. Der Name des untergeordneten Ressourcentyps enthält den Namen des übergeordneten Ressourcentyps. So sind z.B. **Microsoft.Web/sites/config** und **Microsoft.Web/sites/extensions** untergeordnete Ressourcen von **Microsoft.Web/sites**.

Das folgende Beispiel zeigt einen SQL Server und eine SQL-Datenbank. Beachten Sie, dass eine explizite Abhängigkeit zwischen der SQL-Datenbank und SQL Server definiert wird, obwohl die Datenbank ein untergeordnetes Element des Servers ist.

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "apiVersion": "2014-04-01-preview",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "apiVersion": "2014-04-01-preview",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "tags": {
          "displayName": "Database"
        },
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-and-list-functions"></a>Funktionen „reference“ und „list“

Mit der [reference](template-functions-resource.md#reference) -Funktion kann ein Ausdruck seinen Wert von anderen Paaren aus JSON-Name und -Wert oder Laufzeitressourcen ableiten. Die [list*](template-functions-resource.md#list)-Funktionen geben Werte für eine Ressource aus einem list-Vorgang zurück.  reference- und list-Ausdrücke deklarieren implizit, dass eine Ressource von der anderen abhängig ist, wenn die referenzierte Ressource in der gleichen Vorlage bereitgestellt wird und anhand ihres Namens (nicht ihrer Ressourcen-ID) auf sie verwiesen wird. Wenn Sie die Ressourcen-ID an die reference- oder list-Funktionen übergeben, wird kein impliziter Verweis erstellt.

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

Sie können dieses Element oder das dependsOn-Element verwenden, um Abhängigkeiten anzugeben, aber Sie müssen nicht beide für dieselbe abhängige Ressource verwenden. Verwenden Sie nach Möglichkeit einen impliziten Verweis, um das Hinzufügen einer unnötigen Abhängigkeit zu vermeiden.

Weitere Informationen finden Sie unter [reference-Funktion](template-functions-resource.md#reference).

## <a name="circular-dependencies"></a>Ringabhängigkeiten

Resource Manager kennzeichnet Ringabhängigkeiten während der Überprüfung der Vorlage. Falls Sie einen Fehler erhalten, der besagt, dass eine Ringabhängigkeit vorhanden ist, überprüfen Sie Ihre Vorlage, um festzustellen, ob bestimmte Abhängigkeiten nicht erforderlich sind und entfernt werden können. Wenn das Entfernen der Abhängigkeiten nicht funktioniert, können Sie Ringabhängigkeiten vermeiden, indem Sie einige Bereitstellungsvorgänge in untergeordnete Ressourcen verschieben, die nach den Ressourcen mit der Ringabhängigkeit bereitgestellt werden. Nehmen wir beispielsweise an, Sie stellen zwei virtuelle Computer bereit, müssen aber Eigenschaften festlegen, die auf den jeweils anderen verweisen. Sie können diese in der folgenden Reihenfolge bereitstellen:

1. VM1
2. VM2
3. Die Erweiterung auf VM1 hängt von VM1 und VM2 ab. Die Erweiterung legt Werte auf VM1 fest, die sie von VM2 abruft.
4. Die Erweiterung auf VM2 hängt von VM1 und VM2 ab. Die Erweiterung legt Werte auf VM2 fest, die sie von VM1 abruft.

Informationen über das Bewerten der Bereitstellungsreihenfolge und das Beheben von Abhängigkeitsfehlern finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](common-deployment-errors.md).

## <a name="next-steps"></a>Nächste Schritte

* Ein Tutorial, das Sie durcharbeiten können, finden Sie unter [Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen](template-tutorial-create-templates-with-dependent-resources.md).
* Empfehlungen zum Festlegen von Abhängigkeiten finden Sie unter [Bewährte Methoden für Azure Resource Manager-Vorlagen](template-best-practices.md).
* Informationen zur Behebung von Abhängigkeiten während der Bereitstellung finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](common-deployment-errors.md).
* Weitere Informationen zum Erstellen von Azure-Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Vorlagen](template-syntax.md).
* Eine Liste der verfügbaren Funktionen in einer Vorlage finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](template-functions.md).

