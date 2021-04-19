---
title: 'Benutzerdefinierte Modelle: Formularerkennung'
titleSuffix: Azure Cognitive Services
description: Konzepte in Verbindung mit benutzerdefinierten Modellen der Formularerkennungs-API – Verwendung und Einschränkungen.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 03/25/2021
ms.author: lajanuar
ms.openlocfilehash: 9d339d02310fddc3e5fd463f7d37e253614dba17
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106067587"
---
# <a name="form-recognizer-custom-models"></a>Benutzerdefinierte Formularerkennungsmodelle

Bei der Formularerkennung werden Daten unter Verwendung erweiterter Technologien für maschinelles Lernen aus Formularen und Dokumenten extrahiert und analysiert. Ein Formularerkennungsmodell ist eine Darstellung der extrahierten Daten, die als Referenz für die Analyse Ihrer spezifischen Inhalte verwendet werden. Zwei Typen von Formularerkennungsmodellen stehen zur Verfügung:

* **Benutzerdefinierte Modelle:** Benutzerdefinierte Formularerkennungsmodelle stellen extrahierte Daten aus spezifischen _Formularen_ Ihres Unternehmens dar. Benutzerdefinierte Modelle müssen für die Analyse Ihrer spezifischen Formulardaten trainiert werden.

* **Vordefinierte Modelle:** Bei der Formularerkennung werden aktuell vordefinierte Modelle für _Belege, Visitenkarten, Ausweise_ und _Rechnungen_ unterstützt. Mit vordefinierten Modellen werden Informationen aus Dokumentbildern erkannt und extrahiert und die extrahierten Daten in einer strukturierten JSON-Ausgabe zurückgegeben.

## <a name="what-does-a-custom-model-do"></a>Funktionsweise eines benutzerdefinierten Modells

Mit der Formularerkennung können Sie ein Modell trainieren, mit dem Informationen aus Formularen extrahiert werden, die für den jeweiligen Anwendungsfall relevant sind. Für den Anfang benötigen Sie lediglich fünf Beispiele desselben Formulartyps. Das benutzerdefinierte Modell kann mit oder ohne bezeichnete Datasets trainiert werden.

## <a name="create-use-and-manage-your-custom-model"></a>Erstellen, Verwenden und Verwalten eines benutzerdefinierten Modells

Die Schritte zum Erstellen, Trainieren und Verwenden eines benutzerdefinierten Modells sehen im Großen und Ganzen wie folgt aus:

> [!div class="nextstepaction"]
>[&#120783;. Zusammenstellen des Trainingsdatasets](build-training-data-set.md#custom-model-input-requirements)

Die Erstellung eines benutzerdefinierten Modells beginnt mit dem Einrichten des Trainingsdatasets. Für das Beispieldataset benötigen Sie mindestens fünf ausgefüllte Formulare desselben Typs. Diese können unterschiedliche Dateitypen aufweisen und Text sowie Handschrift enthalten. Die Formulare müssen denselben Dokumenttyp aufweisen und die [Eingabeanforderungen](build-training-data-set.md#custom-model-input-requirements) für die Formularerkennung erfüllen.  

> [!div class="nextstepaction"]
> [&#120784;. Hochladen des Trainingsdatasets](build-training-data-set.md#upload-your-training-data)

Sie müssen die Trainingsdaten in einen Azure Blob Storage-Container hochladen. Informationen zum Erstellen eines Azure Storage-Kontos mit einem Container finden *Sie im* [Azure Storage-Schnellstart für das Azure-Portal](../../storage/blobs/storage-quickstart-blobs-portal.md). Verwenden Sie den Tarif „Free“ (F0), um den Dienst zu testen, und führen Sie später für die Produktion ein Upgrade auf einen kostenpflichtigen Tarif durch.  

> [!div class="nextstepaction"]
>[&#120785;. Trainieren des benutzerdefinierten Modells](quickstarts/client-library.md#train-a-custom-model)

Sie können Ihr Modell [ohne](quickstarts/client-library.md#train-a-model-without-labels) oder [mit](quickstarts/client-library.md#train-a-model-with-labels) bezeichneten Datasets trainieren. Nicht bezeichnete Datasets basieren zur Erkennung und Identifizierung der wesentlichen Informationen ohne weitere Benutzereingabe ausschließlich auf der Layout-API. Auch bezeichnete Datasets basieren auf der Layout-API, enthalten jedoch zusätzliche Benutzereingaben, z. B. Ihre spezifischen Bezeichnungen und Feldpositionen. Wenn Sie sowohl bezeichnete als auch nicht bezeichnete Daten verwenden möchten, beginnen Sie mit mindestens fünf ausgefüllten Formularen desselben Typs für die bezeichneten Trainingsdaten, und fügen Sie dann dem gewünschten Dataset nicht bezeichnete Daten hinzu.  

>[!div class="nextstepaction"]
>[&#120786;. Analysieren von Dokumenten mit dem benutzerdefinierten Modell](quickstarts/client-library.md#analyze-forms-with-a-custom-model)

Testen Sie das neu trainierte Modell unter Verwendung eines Formulars, das im Trainingsdataset nicht enthalten war. Mit zusätzlichen Trainingsdurchläufen können Sie die Leistung des benutzerdefinierten Modells optimieren.  

> [!div class="nextstepaction"]
>[&#120787;. Verwalten von benutzerdefinierten Modellen](quickstarts/client-library.md#manage-custom-models)

Sie können jederzeit eine Liste aller benutzerdefinierten Modelle in Ihrem Abonnement anzeigen, Informationen zu einem bestimmten benutzerdefinierten Modell abrufen oder ein benutzerdefiniertes Modell aus Ihrem Konto löschen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Formularerkennungs Clientbibliothek finden Sie in unserer API-Referenzdokumentation.

> [!div class="nextstepaction"]
> [Referenz zur Formularerkennungs-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-3/operations/AnalyzeWithCustomForm)
>
