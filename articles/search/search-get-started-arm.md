---
title: Bereitstellen Ihres Diensts mithilfe einer Azure Resource Manager-Vorlage
titleSuffix: Azure Cognitive Search
description: Mithilfe der Azure Resource Manager-Vorlage lässt sich schnell eine Azure Cognitive Search-Dienstinstanz bereitstellen.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/20/2020
ms.openlocfilehash: 6da2c324872a86c2c0ce6f55801bc7b0ee8d713e
ms.sourcegitcommit: 515482c6348d5bef78bb5def9b71c01bb469ed80
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80611814"
---
# <a name="quickstart-deploy-cognitive-search-using-a-resource-manager-template"></a>Schnellstart: Bereitstellen von Cognitive Search mithilfe einer Resource Manager-Vorlage

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie mithilfe einer Resource Manager-Vorlage eine Azure Cognitive Search-Ressource im Azure-Portal bereitstellen.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-cognitive-search-service"></a>Erstellen eines Cognitive Search-Diensts

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt aus den [Azure-Vorlagen](https://docs.microsoft.com/azure/templates/Microsoft.Search/2015-08-19/searchservices).

:::code language="json"source="~/quickstart-templates/101-azure-search-create/azuredeploy.json" range="1-86" highlight="4-50":::

In dieser Vorlage definierte Azure-Ressource:

- [Microsoft.Search/searchServices](https://docs.microsoft.com/azure/templates/Microsoft.Search/2015-08-19/searchServices) zum Erstellen eines Azure Cognitive Search-Diensts

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Klicken Sie auf das folgende Bild, um sich bei Azure anzumelden und eine Vorlage zu öffnen. Die Vorlage dient zum Erstellen einer Azure Cognitive Search-Ressource.

[![In Azure bereitstellen](./media/search-get-started-arm/arm-deploybuttona.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2F101-azure-search-create%2Fazuredeploy.json)

Im Portal wird ein Formular angezeigt, über das Sie ganz einfach Parameterwerte angeben können. Einige Parameter wurden bereits vorab mit den Standardwerten aus der Vorlage ausgefüllt. Geben Sie Ihr Abonnement, die Ressourcengruppe, den Standort und den Dienstnamen an. Wenn Sie Cognitive Services in einer [KI-Anreicherungspipeline](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro) verwenden möchten, um beispielsweise binäre Bilddateien auf Text zu analysieren, wählen Sie einen Standort aus, an dem sowohl Cognitive Search als auch Cognitive Services zur Verfügung stehen. Für KI-Anreicherungsworkloads müssen sich beide Dienste in der gleichen Region befinden. Nachdem Sie das Formular ausgefüllt haben, müssen Sie den Nutzungsbedingungen zustimmen und anschließend die Schaltfläche „Kaufen“ auswählen, um die Bereitstellung abzuschließen.

> [!div class="mx-imgBorder"]
> ![Vorlage im Azure-Portal](./media/search-get-started-arm/arm-portalscrnsht.png)

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Nach Abschluss der Bereitstellung können Sie im Portal auf Ihre neue Ressourcengruppe und auf den neuen Suchdienst zugreifen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstartanleitungen und Tutorials für Cognitive Search bauen auf dieser Schnellstartanleitung auf. Falls Sie mit weiteren Schnellstartanleitungen und Tutorials fortfahren möchten, empfiehlt es sich gegebenenfalls, diese Ressource nicht zu bereinigen. Wenn Sie die Ressourcen nicht mehr benötigen, können Sie die Ressourcengruppe löschen. Dadurch werden der Cognitive Search-Dienst und die zugehörigen Ressourcen gelöscht.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie mithilfe einer Azure Resource Manager-Vorlage einen Cognitive Search-Dienst erstellt und die Bereitstellung überprüft. Weitere Informationen zu Cognitive Search und Azure Resource Manager finden Sie in den folgenden Artikeln:

 - [Was ist Azure Cognitive Search?](https://docs.microsoft.com/azure/search/search-what-is-azure-search)
 - [Schnellstart: Erstellen eines Index für Azure Cognitive Search im Azure-Portal](https://docs.microsoft.com/azure/search/search-get-started-portal)
 - [Schnellstart: Erstellen einer Such-App im Portal (Azure Cognitive Search)](https://docs.microsoft.com/azure/search/search-create-app-portal) (mithilfe des Portal-Assistenten)
 - [Schnellstart: Erstellen eines kognitiven Skillsets für Azure Cognitive Search über das Azure-Portal](https://docs.microsoft.com/azure/search/cognitive-search-quickstart-blob) (zum Extrahieren von Informationen aus Ihren Daten)


