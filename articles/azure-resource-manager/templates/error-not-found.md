---
title: Fehler des Typs „Ressource nicht gefunden“
description: Hier wird beschrieben, wie Sie Fehler beheben, wenn beim Bereitstellen mit einer Azure Resource Manager-Vorlage eine Ressource nicht gefunden wird.
ms.topic: troubleshooting
ms.date: 01/21/2020
ms.openlocfilehash: b6f433118092e46f734d4b65040dd97c2fcb58d9
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76773260"
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Beheben von Fehlern aufgrund nicht gefundener Azure-Ressourcen

In diesem Artikel werden mögliche Fehler beschrieben, wenn eine Ressource während der Bereitstellung nicht gefunden wird.

## <a name="symptom"></a>Symptom

Wenn Ihre Vorlage einen Ressourcennamen enthält, der nicht aufgelöst werden kann, erhalten Sie in etwa folgende Fehlermeldung:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Wenn die Funktionen [reference](template-functions-resource.md#reference) oder [listKeys](template-functions-resource.md#listkeys) mit einer Ressource verwenden, die nicht aufgelöst werden kann, erhalten Sie folgenden Fehler:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Ursache

Resource Manager muss die Eigenschaften für eine Ressource abrufen, kann die Ressource in Ihrem Abonnement aber nicht identifizieren.

## <a name="solution-1---set-dependencies"></a>Lösung 1: Festlegen von Abhängigkeiten

Wenn Sie versuchen, die fehlende Ressource in der Vorlage bereitzustellen, sollten Sie prüfen, ob Sie eine Abhängigkeit hinzufügen müssen. Resource Manager optimiert die Bereitstellung, indem, sofern möglich, gleichzeitig Ressourcen erstellt werden. Wenn eine Ressource nach einer anderen Ressource bereitgestellt werden muss, müssen Sie das **dependsOn**-Element in der Vorlage verwenden. Beim Bereitstellen einer Web-App muss z. B. der App Service-Plan vorhanden sein. Wenn Sie nicht angegeben haben, dass die Web-App vom App Service-Plan abhängig ist, erstellt Resource Manager beide Ressourcen zur gleichen Zeit. Beim Versuch, eine Eigenschaft für die Web-App festzulegen, erhalten Sie eine Fehlermeldung, dass die App Service-Planressource nicht gefunden werden kann, da sie noch nicht vorhanden ist. Sie verhindern diesen Fehler, indem Sie die Abhängigkeit in der Web-App festlegen.

```json
{
  "type": "Microsoft.Web/sites",
  "apiVersion": "2015-08-01",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Es ist jedoch ratsam, keine Abhängigkeiten einzurichten, die nicht benötigt werden. Wenn Sie nicht benötigte Abhängigkeiten einrichten, verlängern Sie die Dauer der Bereitstellung, indem Sie verhindern, dass nicht voneinander abhängige Ressourcen parallel bereitgestellt werden. Außerdem besteht die Gefahr, dass Sie Ringabhängigkeiten erstellen, die die Bereitstellung blockieren. Die Funktion [reference](template-functions-resource.md#reference) und die [list*](template-functions-resource.md#list)-Funktionen erstellen eine implizite Abhängigkeit von der angegebenen Ressource, wenn diese Ressource in derselben Vorlage bereitgestellt und mit ihrem eigenen Namen (nicht ihrer Ressourcen-ID) auf sie verwiesen wird. Aus diesem Grund verfügen Sie ggf. über eine höhere Zahl von Abhängigkeiten als in der **dependsOn**-Eigenschaft angegeben. Die Funktion [resourceId](template-functions-resource.md#resourceid) erstellt keine implizite Abhängigkeit und überprüft nicht, ob die Ressource vorhanden ist. Die Funktion [reference](template-functions-resource.md#reference) und die [list*](template-functions-resource.md#list)-Funktionen erstellen keine implizite Abhängigkeit, wenn mit der Ressourcen-ID auf die Ressource verwiesen wird. Um eine implizite Abhängigkeit zu erstellen, übergeben Sie den Namen der Ressource, die in der gleichen Vorlage bereitgestellt wird.

Wenn Abhängigkeitsprobleme auftreten, benötigen Sie Informationen zur Reihenfolge der Ressourcenbereitstellung. So zeigen Sie die Reihenfolge der Vorgänge bei der Bereitstellung an:

1. Wählen Sie den Bereitstellungsverlauf für die Ressourcengruppe aus.

   ![Bereitstellungsverlauf auswählen](./media/error-not-found/select-deployment.png)

2. Wählen Sie aus dem Verlauf eine Bereitstellung und **Ereignisse** aus.

   ![Bereitstellungsereignisse auswählen](./media/error-not-found/select-deployment-events.png)

3. Überprüfen Sie die Abfolge von Ereignissen für jede Ressource. Achten Sie auf den Status der einzelnen Vorgänge. Die folgende Abbildung zeigt beispielsweise drei parallel bereitgestellte Speicherkonten. Beachten Sie, dass die drei Speicherkonten zur selben Zeit gestartet werden.

   ![Parallele Bereitstellung](./media/error-not-found/deployment-events-parallel.png)

   Die nächste Abbildung zeigt drei Speicherkonten, die nicht parallel bereitgestellt werden. Das zweite Speicherkonto ist vom ersten Speicherkonto abhängig, und das dritte Speicherkonto ist vom zweiten Speicherkonto abhängig. Das erste Speicherkonto wird gestartet, akzeptiert und abgeschlossen, bevor das nächste gestartet wird.

   ![Sequenzielle Bereitstellung](./media/error-not-found/deployment-events-sequence.png)

## <a name="solution-2---get-resource-from-different-resource-group"></a>Lösung 2: Abrufen der Ressource aus einer anderen Ressourcengruppe

Wenn die Ressource in einer anderen Ressourcengruppe als der Ressourcengruppe enthalten ist, die gerade bereitgestellt wird, rufen Sie mit [resourceId function](template-functions-resource.md#resourceid) den vollqualifizierten Namen der Ressource ab.

```json
"properties": {
  "name": "[parameters('siteName')]",
  "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

## <a name="solution-3---check-reference-function"></a>Lösung 3: Überprüfen der reference-Funktion

Suchen Sie nach einem Ausdruck, der die [reference](template-functions-resource.md#reference)-Funktion enthält. Die von Ihnen bereitgestellten Werte variieren basierend darauf, ob sich die Ressource in der gleichen Vorlage und Ressourcengruppe und im gleichen Abonnement befindet. Vergewissern Sie sich, dass die erforderlichen Parameterwerte für Ihr Szenario angegeben wurden. Befindet sich die Ressource in einer anderen Ressourcengruppe, geben Sie die vollständige Ressourcen-ID an. Verwenden Sie zum Verweisen auf ein Speicherkonto in einer anderen Ressourcengruppe beispielsweise Folgendes:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```

## <a name="solution-4---get-managed-identity-from-resource"></a>Lösung 4: Abrufen der verwalteten Identität aus der Ressource

Wenn Sie eine Ressource bereitstellen, die implizit eine [verwaltete Identität](../../active-directory/managed-identities-azure-resources/overview.md) erstellt, müssen Sie warten, bis diese Ressource bereitgestellt wurde, bevor Sie Werte für die verwaltete Identität abrufen können. Wenn Sie den Namen der verwalteten Identität an die [Verweis](template-functions-resource.md#reference)funktion (reference) übergeben, versucht Resource Manager, den Verweis aufzulösen, bevor die Ressource und die Identität bereitgestellt sind. Übergeben Sie stattdessen den Namen der Ressource, auf die die Identität angewendet wird. Mit diesem Ansatz wird sichergestellt, dass die Ressource und die verwaltete Identität bereitgestellt sind, bevor Resource Manager die Verweisfunktion auflöst.

Verwenden Sie in der Verweisfunktion `Full`, um alle Eigenschaften, einschließlich der verwalteten Identität, abzurufen.

Um z. B. die Mandanten-ID für eine verwaltete Identität abzurufen, die auf eine VM-Skalierungsgruppe angewendet wird, verwenden Sie:

```json
"tenantId": "[reference(resourceId('Microsoft.Compute/virtualMachineScaleSets',  variables('vmNodeType0Name')), variables('vmssApiVersion'), 'Full').Identity.tenantId]"
```