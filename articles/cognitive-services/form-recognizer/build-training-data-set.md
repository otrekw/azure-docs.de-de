---
title: Erstellen eines Trainingsdatasets für ein benutzerdefiniertes Modell – Formularerkennung
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie sicherstellen, dass Ihr Trainingsdataset für das Trainieren eines Formularerkennungsmodells optimiert ist.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: pafarley
ms.openlocfilehash: ffa09293ad2ff02e104ce285b6b0aaca7d4744a2
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85212665"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Erstellen eines Trainingsdatasets für ein benutzerdefiniertes Modell

Wenn Sie das benutzerdefinierte Modell zur Formularerkennung verwenden, stellen Sie Ihre eigenen Trainingsdaten bereit, damit das Modell mit Ihren branchenspezifischen Formularen trainiert werden kann. 

Wenn Sie das Modell ohne manuelle Bezeichnungen trainieren, können Sie fünf ausgefüllte Formulare oder ein leeres Formular (der Dateiname muss das Wort „empty“ enthalten) und zwei ausgefüllte Formulare verwenden. Selbst wenn Sie über genügend ausgefüllte Formulare verfügen, kann das Hinzufügen eines leeren Formulars zu Ihrem Trainingsdataset die Genauigkeit des Modells verbessern.

Wenn Sie manuell bezeichnete Trainingsdaten verwenden möchten, müssen Sie mit mindestens fünf ausgefüllten Formularen desselben Typs beginnen. Sie können trotzdem nicht bezeichnete Formulare und ein leeres Formular zusätzlich zum erforderlichen Dataset verwenden.

## <a name="training-data-tips"></a>Tipps zu Trainingsdaten

Es ist wichtig, ein Dataset zu verwenden, das für das Trainieren optimiert ist. Verwenden Sie die folgenden Tipps, um sicherzustellen, dass Sie die besten Ergebnisse aus dem Vorgang [Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/TrainCustomModelAsync) (Benutzerdefiniertes Modell trainieren)-erzielen:

* Verwenden Sie nach Möglichkeit textbasierte PDF-Dokumente anstelle von bildbasierten Dokumenten. Gescannte PDF-Dateien werden als Bilder behandelt.
* Verwenden Sie als ausgefüllte Formulare Exemplare, in denen alle Felder ausgefüllt sind.
* Verwenden Sie Formulare mit verschiedenen Werten in jedem Feld.
* Wenn Ihre Formularbilder eine mäßige Qualität aufweisen, verwenden Sie ein größeres Dataset (beispielsweise 10–15 Bilder).
* Die Gesamtgröße des Trainingsdataset kann bis zu 500 Seiten betragen.

## <a name="general-input-requirements"></a>Allgemeine Eingabeanforderungen

Achten Sie darauf, dass Ihr Trainingsdataset auch den Eingabeanforderungen für alle Inhalte der Formularerkennung genügt. 

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="upload-your-training-data"></a>Hochladen Ihrer Trainingsdaten

Wenn Sie den Satz von Formulardokumenten, die Sie für das Training verwenden möchten, zusammengestellt haben, müssen Sie ihn in einen Azure Blob Storage-Container hochladen. Wenn Sie nicht wissen, wie Sie ein Azure Storage-Konto mit einem Container erstellen, folgen Sie den Anweisungen im [Azure Storage-Schnellstart für das Azure-Portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

Wenn Sie manuell bezeichnete Daten verwenden möchten, müssen Sie auch die Dateien *.labels.json* und *.ocr.json* hochladen, die den Trainingsdokumenten entsprechen. Sie können das [Tool für die Beschriftung von Beispielen](./quickstarts/label-tool.md) (oder Ihre eigene Benutzeroberfläche) verwenden, um diese Dateien zu generieren.

### <a name="organize-your-data-in-subfolders-optional"></a>Organisieren Ihrer Daten in Unterordnern (optional)

Standardmäßig werden von der [Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/TrainCustomModelAsync)-API nur Formulardokumente verwendet, die sich im Stammverzeichnis Ihres Speichercontainers befinden. Sie können jedoch mit Daten in Unterordnern trainieren, wenn Sie dies im API-Aufruf angeben. Normalerweise hat der Text des [Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/TrainCustomModelAsync)-Aufrufs das folgende Format, wobei `<SAS URL>` die SAS-URL (Shared Access Signature) des Containers ist:

```json
{
  "source":"<SAS URL>"
}
```

Wenn Sie dem Anforderungstext den folgenden Inhalt hinzufügen, wird die API mit Dokumenten trainiert, die sich in Unterordnern befinden. Das Feld `"prefix"` ist optional und schränkt das Trainingsdataset auf Dateien ein, deren Pfad mit der angegebenen Zeichenfolge beginnt. So bewirkt beispielsweise der Wert `"Test"`, dass die API nur Dateien oder Ordner berücksichtigt, die mit dem Wort „Test“ beginnen.

```json
{
  "source": "<SAS URL>",
  "sourceFilter": {
    "prefix": "<prefix string>",
    "includeSubFolders": true
  },
  "useLabelFile": false
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun gelernt haben, wie Sie ein Trainingsdataset erstellen, folgen Sie den Anweisungen in einem Schnellstart, um ein benutzerdefiniertes Formularerkennungsmodell zu trainieren und es mit Ihren Formularen zu verwenden.

* [Schnellstart: Trainieren eines Modells zur Formularerkennung und Extrahieren von Formulardaten unter Verwendung der REST-API mit cURL](./quickstarts/curl-train-extract.md)
* [Schnellstart: Trainieren eines Modells zur Formularerkennung und Extrahieren von Formulardaten unter Verwendung der REST-API mit Python](./quickstarts/python-train-extract.md)
* [Trainieren eines Formularerkennungsmodells mit Beschriftungen mithilfe des Tools für die Beschriftung von Beispielen](./quickstarts/label-tool.md)
* [Trainieren mit Bezeichnungen mit der REST-API und Python](./quickstarts/python-labeled-data.md)
