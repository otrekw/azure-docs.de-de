---
title: 'Schnellstart: Neue Richtlinienzuweisung mit Vorlagen'
description: In dieser Schnellstartanleitung erstellen Sie mit einer Resource Manager-Vorlage eine Richtlinienzuweisung zum Identifizieren von nicht konformen Ressourcen.
ms.date: 03/16/2020
ms.topic: quickstart
ms.custom: subject-armqs
ms.openlocfilehash: ae6b2d151baaa904215639276c7fb74766810c6a
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606605"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-by-using-a-resource-manager-template"></a>Schnellstart: Erstellen einer Richtlinienzuweisung zum Identifizieren von nicht konformen Ressourcen mit einer Resource Manager-Vorlage

Zum Verständnis der Konformität in Azure müssen Sie zunächst wissen, wie Sie den Status Ihrer Ressourcen ermitteln.
Diese Schnellstartanleitung führt Sie schrittweise durch die Erstellung einer Richtlinienzuweisung zur Identifizierung von virtuellen Computern, die keine verwalteten Datenträger verwenden. Am Ende dieses Prozesses können Sie erfolgreich virtuelle Computer identifizieren, die keine verwalteten Datenträger verwenden. Sie sind mit der Richtlinienzuweisung _nicht konform_.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="create-a-policy-assignment"></a>Erstellen einer Richtlinienzuweisung

In dieser Schnellstartanleitung erstellen Sie eine Richtlinienzuweisung und weisen eine integrierte Richtliniendefinition mit dem Namen _Virtuelle Computer überwachen, die keine verwalteten Datenträger verwenden_ zu. Eine Liste mit einem Teil der verfügbaren integrierten Richtlinien finden Sie unter [Azure Policy-Beispiele](./samples/index.md).

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/).

:::code language="json" source="~/quickstart-templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/azuredeploy.json" range="1-36" highlight="26-34":::

In der Vorlage ist die folgende Ressource definiert:

- [Microsoft.Authorization/policyAssignments](/azure/templates/microsoft.authorization/policyassignments)

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

> [!NOTE]
> Der Azure Policy-Dienst ist kostenlos. Weitere Informationen finden Sie in der [Übersicht über Azure Policy](./overview.md).

1. Wählen Sie die folgende Abbildung aus, um sich am Azure-Portal anzumelden und die Vorlage zu öffnen:

   [![Bereitstellen der Policy-Vorlage in Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurepolicy-assign-builtinpolicy-resourcegroup%2Fazuredeploy.json)

1. Wählen Sie die folgenden Werte aus, bzw. geben Sie sie ein:

   | Name | Wert |
   |------|-------|
   | Subscription | Wählen Sie Ihr Azure-Abonnement. |
   | Resource group | Wählen Sie **Neu erstellen**, geben Sie einen Namen an, und wählen Sie dann **OK**. Im Screenshot lautet der Name der Ressourcengruppe _mypolicyquickstart\<Datum in MMTT\>rg_. |
   | Position | Wählen Sie eine Region aus. Beispiel: **USA, Mitte**. |
   | Name der Richtlinienzuweisung | Geben Sie einen Namen für die Richtlinienzuweisung an. Bei Bedarf können Sie die Anzeige für die Richtliniendefinitionen verwenden. Beispiel: **Virtuelle Computer überwachen, die keine verwalteten Datenträger verwenden**. |
   | Ressourcengruppenname | Geben Sie den Namen einer Ressourcengruppe an, der Sie die Richtlinie zuweisen möchten. In dieser Schnellstartanleitung verwenden Sie den Standardwert **[resourceGroup().name]** . **[resourceGroup()](../../azure-resource-manager/templates/template-functions-resource.md#resourcegroup)** ist eine Vorlagenfunktion, mit der die Ressourcengruppe abgerufen wird. |
   | Richtliniendefinitions-ID | Geben Sie **/providers/Microsoft.Authorization/policyDefinitions/0a914e76-4921-4c19-b460-a2d36003525a** an. |
   | Ich stimme den oben genannten Geschäftsbedingungen zu | (Auswählen) |

1. Wählen Sie die Option **Kaufen**.

Einige zusätzliche Ressourcen:

- Weitere Beispielvorlagen finden Sie unter [Azure-Schnellstartvorlage](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization&pageNumber=1&sort=Popular).
- Navigieren Sie zur [Azure-Vorlagenreferenz](/azure/templates/microsoft.authorization/allversions), um die Vorlagenreferenz anzuzeigen.
- Informationen zur Entwicklung von Resource Manager-Vorlagen finden Sie in der [Azure Resource Manager-Dokumentation](../../azure-resource-manager/management/overview.md).
- Informationen zur Bereitstellung auf Abonnementebene finden Sie unter [Erstellen von Ressourcengruppen und Ressourcen auf Abonnementebene](../../azure-resource-manager/templates/deploy-to-subscription.md).

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

Wählen Sie links auf der Seite die Option **Konformität** aus. Suchen Sie dann die von Ihnen erstellte Richtlinienzuweisung **Überwachen Sie die virtuellen Computer, die nicht verwaltete Datenträger verwenden**.

![Übersichtsseite zur Richtlinienkonformität](./media/assign-policy-template/policy-compliance.png)

Falls Ressourcen vorhanden sind, die mit dieser neuen Zuweisung nicht konform sind, werden diese unter **Nicht konforme Ressourcen** angezeigt.

Weitere Informationen finden Sie unter [Funktionsweise der Konformität](./how-to/get-compliance-data.md#how-compliance-works).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Um die erstellte Zuweisung zu entfernen, gehen Sie folgendermaßen vor:

1. Wählen Sie links auf der Seite „Azure Policy“ **Konformität** (oder **Zuweisungen**), und suchen Sie die von Ihnen erstellte Richtlinienzuweisung **Überwachen Sie die virtuellen Computer, die nicht verwaltete Datenträger verwenden**.

1. Klicken Sie mit der rechten Maustaste auf die Richtlinienzuweisung **Virtuelle Computer überwachen, die keine verwalteten Datenträger verwenden**, und wählen Sie **Zuweisung löschen**.

   ![Löschen einer Zuweisung auf der Übersichtsseite zur Konformität](./media/assign-policy-template/delete-assignment.png)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einem Bereich eine integrierte Richtliniendefinition zugewiesen und deren Konformitätsbericht ausgewertet. Die Richtliniendefinition überprüft, ob alle Ressourcen im Bereich konform sind, und identifiziert die Ressourcen, die nicht konform sind.

Weitere Informationen zum Zuweisen von Richtlinien, die die Konformität neuer Ressourcen überprüfen, finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von Richtlinien](./tutorials/create-and-manage.md)