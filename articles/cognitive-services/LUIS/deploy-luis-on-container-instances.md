---
title: Bereitstellen eines LUIS-Containers in Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Stellen Sie den LUIS-Container für die Gesichtserkennung in einer Azure Container-Instanz bereit, und testen Sie diesen in einem Webbrowser.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: aahi
ms.openlocfilehash: 08af17106846a0f5f7a0ccc2b01da1b2e15c1143
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879203"
---
# <a name="deploy-the-language-understanding-luis-container-to-azure-container-instances"></a>Bereitstellen des Language Understanding-Containers (LUIS) in Azure Container-Instanzen

Erfahren Sie, wie Sie die [LUIS](luis-container-howto.md)-Container von Cognitive Services für [Azure Container-Instanzen](https://docs.microsoft.com/azure/container-instances/) bereitstellen. Dieses Verfahren veranschaulicht die Erstellung der Ressource für die Anomalieerkennung. Anschließend wird das Abrufen des dazugehörigen Containerimages erläutert. Abschließend erklären wir, wie Sie die Orchestrierung von Ressource und Image über einen Browser ausführen können. Die Verwendung von Containern kann die Aufmerksamkeit der Entwickler von der Verwaltung der Infrastruktur auf die Anwendungsentwicklung lenken.

[!INCLUDE [Prerequisites](../containers/includes/container-prerequisites.md)]

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe

Der LUIS-Container erfordert eine `.gz`-Modelldatei, die zur Laufzeit abgerufen wird. Der Container muss auf diese Modelldatei über eine Volumeneinbindung aus der Containerinstanz zugreifen können. Weitere Informationen zum Erstellen einer Azure-Dateifreigabe finden Sie unter [Erstellen einer Dateifreigabe](../../storage/files/storage-how-to-create-file-share.md). Notieren Sie sich den Namen des Azure Storage-Kontos, den Schlüssel und den Namen der Dateifreigabe, da Sie diese Angaben später benötigen.

### <a name="export-and-upload-packaged-luis-app"></a>Exportieren und Hochladen des LUIS-App-Pakets

Um das LUIS-Modell (App-Paket) auf die Azure-Dateifreigabe hochzuladen, müssen Sie es <a href="luis-container-howto.md#export-packaged-app-from-luis" target="_blank" rel="noopener">zuerst aus dem LUIS-Portal exportieren <span class="docon docon-navigate-external x-hidden-focus"></span></a>. Navigieren Sie im Azure-Portal zur Seite **Übersicht** der Speicherkontoressource, und wählen Sie **Dateifreigaben** aus. Wählen Sie den Namen der Dateifreigabe aus, die Sie kürzlich erstellt haben, und wählen Sie dann die Schaltfläche **Hochladen** aus.

> [!div class="mx-imgBorder"]
> ![Hochladen auf die Dateifreigabe](media/luis-how-to-deploy-to-aci/upload-file-share.png)

Laden Sie die LUIS-Modelldatei hoch.

[!INCLUDE [Create LUIS Container instance resource](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Next steps](../containers/includes/containers-next-steps.md)]
