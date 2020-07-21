---
title: Konvertieren in ein Bildverzeichnis
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie das Modul „Convert to Image Directory“ (In Bildverzeichnis konvertieren) verwenden, um ein Dataset in das Bildverzeichnisformat zu konvertieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: b29b5fa1beb19bc055f94c56b064ae2c0ae175b5
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171141"
---
# <a name="convert-to-image-directory"></a>Konvertieren in ein Bildverzeichnis

In diesem Artikel erfahren Sie, wie Sie das Modul „Convert to Image Directory“ (In Bildverzeichnis konvertieren) verwenden, um ein Bilddataset in den Datentyp „Bildverzeichnis“ zu konvertieren. Hierbei handelt es sich um ein standardisiertes Datenformat für bildbezogene Aufgaben wie etwa die Bildklassifizierung im Azure Machine Learning-Designer (Vorschauversion).

## <a name="how-to-use-convert-to-image-directory"></a>Verwenden von „Convert to Image Directory“ (In Bildverzeichnis konvertieren)  

1.  Fügen Sie Ihrem Experiment das Modul **Convert to Image Directory** (In Bildverzeichnis konvertieren) hinzu. Dieses Modul befindet sich in der Modulliste in der Kategorie „Maschinelles Sehen/Transformation von Bilddaten“. 

2.  [Registrieren Sie ein Bilddataset](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets), und verbinden Sie es mit dem Moduleingabeport. Achten Sie darauf, dass das Eingabedataset Bilder enthält. 
    Folgende Datasetformate werden unterstützt:

    - Komprimierte Datei mit folgenden Erweiterungen: „.zip“, „.tar“, „.gz“, „.bz2“.
    - Ordner mit Bildern. **Es empfiehlt sich, einen solchen Ordner zu komprimieren und dann die komprimierte Datei als Dataset zu verwenden.**

    > [!WARNING]
    > Bilddatasets können **nicht** mithilfe des Moduls **Daten importieren** importiert werden, da das Modul **Daten importieren** Daten als DataFrame-Verzeichnis ausgibt und die Daten dann nur die Dateipfadzeichenfolge enthalten.
    

    > [!NOTE]
    > Wenn das Bilddataset im Rahmen von beaufsichtigtem Lernen verwendet wird, ist eine Bezeichnung erforderlich.
    > Für die Bildklassifizierungsaufgabe kann die Bezeichnung in der Modulausgabe als Bildkategorie generiert werden, wenn dieses Bilddataset als ImageFolder-Format von Torchvision strukturiert ist. Andernfalls werden nur Bilder ohne Bezeichnung gespeichert. Das folgende Beispiel zeigt, wie Sie das Bilddataset strukturieren können, um die Bezeichnung zu erhalten und die Bildkategorie als Unterordnername verwenden. Weitere Informationen finden Sie unter [Torchvision-Datasets](https://pytorch.org/docs/stable/torchvision/datasets.html#imagefolder).
    >
    > ```
    > root/dog/xxx.png
    > root/dog/xxy.png
    > root/dog/xxz.png
    >
    > root/cat/123.png
    > root/cat/nsdf3.png
    > root/cat/asd932_.png
    > ```

3.  Übermitteln Sie die Pipeline.

## <a name="results"></a>Ergebnisse

Das Modul **Convert to Image Directory** (In Bildverzeichnis konvertieren) gibt Daten im Bildverzeichnisformat aus, und die Daten können mit anderen bildbezogenen Modulen verbunden werden, die ebenfalls das Bildverzeichnisformat als Eingabeportformat haben.

## <a name="technical-notes"></a>Technische Hinweise 

###  <a name="expected-inputs"></a>Erwartete Eingaben  

| Name          | type                  | Beschreibung   |
| ------------- | --------------------- | ------------- |
| Eingabedataset | AnyDirectory, ZipFile | Eingabedataset |

###  <a name="output"></a>Output  

| Name                   | type           | Beschreibung            |
| ---------------------- | -------------- | ---------------------- |
| Ausgabebildverzeichnis | ImageDirectory | Ausgabebildverzeichnis |

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
