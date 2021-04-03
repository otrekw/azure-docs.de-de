---
title: 'Schnellstart: Erstellen einer Azure API Management-Instanz per ARM-Vorlage'
description: Es wird beschrieben, wie Sie eine Azure API Management-Instanz mit dem Developer-Tarif erstellen, indem Sie eine ARM-Vorlage (Azure Resource Manager) verwenden.
services: azure-resource-manager
author: dlepow
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: danlep
ms.date: 10/09/2020
ms.openlocfilehash: 1610c51b613712c06410247e2eb673a50a9988b3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92792241"
---
# <a name="quickstart-create-a-new-azure-api-management-service-instance-using-an-arm-template"></a>Schnellstart: Erstellen einer neuen Azure API Management-Dienstinstanz mit einer ARM-Vorlage

In dieser Schnellstartanleitung wird beschrieben, wie Sie mit einer ARM-Vorlage (Azure Resource Manager) eine APIM-Dienstinstanz (Azure API Management) erstellen. APIM unterstützt Organisationen beim Veröffentlichen von APIs für externe und interne Entwickler sowie für Partner, damit diese das volle Potenzial von Daten und Diensten nutzen können. API Management bietet die Kernkompetenzen zur Sicherstellung eines erfolgreichen API-Programms in Form von Entwicklerengagement, geschäftlichen Erkenntnissen, Analysen, Sicherheit und Schutz. Mit APIM können Sie moderne API-Gateways für vorhandene Back-End-Dienste, die an einem beliebigen Ort gehostet werden, erstellen und verwalten. Weitere Informationen finden Sie in der [Übersicht](api-management-key-concepts.md).

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azure-api-management-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-azure-api-management-create/).

:::code language="json" source="~/quickstart-templates/101-azure-api-management-create/azuredeploy.json":::

In der Vorlage ist die folgende Ressource definiert:

- **[Microsoft.ApiManagement/service](/azure/templates/microsoft.apimanagement/service)**

Weitere Beispiele für Azure API Management-Vorlagen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Apimanagement&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

1. Klicken Sie auf das folgende Bild, um sich bei Azure anzumelden und eine Vorlage zu öffnen. Von der Vorlage wird eine Instanz des API Management-Diensts mit einem automatisch generierten Namen erstellt.

    [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azure-api-management-create%2Fazuredeploy.json)

    In diesem Beispiel wird die Instanz mit dem Developer-Tarif konfiguriert. Hierbei handelt es sich um eine kostengünstige Option zum Evaluieren von Azure API Management. Dieser Tarif ist nicht für die Produktion bestimmt. Weitere Informationen zum Skalieren der API Management-Tarife finden Sie unter [Upgrade and scale an API Management instance](upgrade-and-scale.md) (Upgraden und Skalieren einer API Management-Instanz).

1. Wählen Sie die folgenden Werte aus, bzw. geben Sie sie ein.
    - **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
    - **Ressourcengruppe**: Wählen Sie die Option **Neu erstellen** aus, geben Sie einen eindeutigen Namen für die Ressourcengruppe ein, und wählen Sie dann **OK** aus.
    - **Region**: Wählen Sie einen Standort für die Ressourcengruppe aus. Beispiel: **USA, Mitte**
    - **Publisher Email** (E-Mail-Adresse des Herausgebers): Geben Sie eine E-Mail-Adresse für den Empfang von Benachrichtigungen ein.
    - **Publisher Name** (Name des Herausgebers):Geben Sie den gewünschten Namen für den API-Herausgeber ein.
    - **SKU**: Übernehmen Sie den Standardwert für **Developer**.
    - **SKU Count** (SKU-Anzahl): Übernehmen Sie den Standardwert.
    - **Location** (Speicherort): Übernehmen Sie den generierten Speicherort für den API Management-Dienst.

    :::image type="content" source="media/quickstart-arm-template/create-instance-template.png" alt-text="Eigenschaften von API Management-Vorlagen":::

1. Wählen Sie **Überprüfen und erstellen** aus, und lesen Sie sich die Geschäftsbedingungen durch. Wählen Sie **Erstellen** aus, wenn Sie die Bedingungen akzeptieren.

    > [!TIP]
    >  Es dauert in der Regel zwischen 30 und 40 Minuten, einen API Management-Dienst im Developer-Tarif zu erstellen und zu aktivieren.

1. Nach der erfolgreichen Erstellung der Instanz erhalten Sie eine Benachrichtigung:

    :::image type="content" source="media/quickstart-arm-template/deployment-notification.png" alt-text="Bereitstellungsbenachrichtigung":::

 Zum Bereitstellen der Vorlage wird das Azure-Portal verwendet. Neben dem Azure-Portal können Sie auch Azure PowerShell, die Azure-Befehlszeilenschnittstelle (Azure CLI) und die REST-API verwenden. Informationen zu anderen Bereitstellungsmethoden finden Sie unter [Bereitstellen von Vorlagen](../azure-resource-manager/templates/deploy-cli.md).

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Verwenden Sie das Azure-Portal, um die bereitgestellten Ressourcen zu überprüfen, oder nutzen Sie Tools wie die Azure CLI oder Azure PowerShell, um sie aufzulisten.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach **API Management-Dienste**, und wählen Sie diese Option aus. Wählen Sie anschließend die von Ihnen erstellte Dienstinstanz aus.
1. Sehen Sie sich auf der Seite **Übersicht** die Eigenschaften Ihres Diensts an.

:::image type="content" source="media/quickstart-arm-template/service-instance-created.png" alt-text="Seite mit der Dienstübersicht":::

Sobald Ihre API Management-Dienstinstanz online ist, können Sie sie verwenden. Beginnen Sie mit dem Tutorial [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie auch die nachfolgenden Tutorials durcharbeiten möchten, ist es ggf. ratsam, die API Management-Instanz beizubehalten. Wenn Sie die Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden die Ressourcen in der Ressourcengruppe gelöscht.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach **Ressourcengruppen**, und wählen Sie die entsprechende Option aus. Die Option **Ressourcengruppen** kann auch auf der **Startseite** von Azure ausgewählt werden.
1. Wählen Sie auf der Seite **Ressourcengruppen** Ihre Ressourcengruppe aus.
1. Wählen Sie auf der Ressourcengruppenseite die Option **Ressourcengruppe löschen** aus.

    :::image type="content" source="media/quickstart-arm-template/delete-resource-group.png" alt-text="Ressourcengruppe löschen":::
1. Geben Sie den Namen Ihrer Ressourcengruppe ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md)
