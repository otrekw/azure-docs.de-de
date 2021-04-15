---
title: Anpassen eines Sprachmodells mit der Video Indexer-API
titlesuffix: Azure Media Services
description: Erfahren Sie, wie Sie ein Sprachmodell mit der Video Indexer-API anpassen können.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 02/04/2020
ms.author: kumud
ms.openlocfilehash: 890e02812fb06cf0b0ebe990b0175311d5c85ab5
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107312925"
---
# <a name="customize-a-language-model-with-the-video-indexer-api"></a>Anpassen eines Sprachmodells mit der Video Indexer-API

Mit Video Indexer können Sie benutzerdefinierte Sprachmodelle erstellen, um die Spracherkennung durch Hochladen von Anpassungstexten anzupassen, insbesondere von Texten aus dem Bereich, an dessen Vokabular sich die Engine anpassen soll. Sobald Sie Ihr Modell trainiert haben, werden neue Wörter, die im Anpassungstext vorkommen, erkannt.

Eine detaillierte Übersicht und bewährte Methoden für benutzerdefinierte Sprachmodelle finden Sie unter [Anpassen eines Sprachmodells mit Video Indexer](customize-language-model-overview.md).

Sie können die Video Indexer-APIs verwenden, um benutzerdefinierte Sprachmodelle in Ihrem Konto zu erstellen und zu bearbeiten, wie in diesem Thema beschrieben. Sie können auch die Website verwenden, wie unter [Anpassen des Sprachmodells mit der Video Indexer-Website](customize-language-model-with-api.md) beschrieben wird.

## <a name="create-a-language-model"></a>Erstellen eines Sprachmodells

Die API zum [Erstellen eines Sprachmodells](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Create-Language-Model) erstellt ein neues benutzerdefiniertes Sprachmodell im angegebenen Konto. Sie können Dateien für das Sprachmodell in diesem Aufruf hochladen. Alternativ können Sie das Sprachmodell hier erstellen und Dateien für das Modell später durch Aktualisieren des Sprachmodells hochladen.

> [!NOTE]
> Sie müssen das Modell mit seinen aktivierten Dateien weiterhin trainieren, damit das Modell den Inhalt seiner Dateien erlernen kann. Anleitungen zum Trainieren einer Sprache finden Sie im nächsten Abschnitt.

Um Dateien hochzuladen, die dem Sprachmodell hinzugefügt werden sollen, müssen Sie Dateien im Textkörper mithilfe von FormData hochladen und zusätzlich Werte für die oben genannten erforderlichen Parameter angeben. Hierfür gibt es zwei Möglichkeiten:

* Der Schlüssel ist der Dateiname und der Wert die TXT-Datei.
* Der Schlüssel ist der Dateiname und der Wert eine URL zur TXT-Datei.

### <a name="response"></a>Antwort

Die Antwort stellt Metadaten zum neu erstellten Sprachmodell zusammen mit Metadaten zu jeder der Dateien des Modells im Format der JSON-Beispielausgabe bereit:

```json
{
    "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
    "name": "TestModel",
    "language": "En-US",
    "state": "None",
    "languageModelId": "00000000-0000-0000-0000-000000000000",
    "files": [
    {
        "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.6733333"
    },
    {
        "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
        "name": "worldfile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.86"
    }
    ]
}

```

## <a name="train-a-language-model"></a>Trainieren eines Sprachmodells

Die API zum [Trainieren eines Sprachmodells](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Train-Language-Model) trainiert ein benutzerdefiniertes Sprachmodell im angegebenen Konto mit dem Inhalt der Dateien, die in das Sprachmodell hochgeladen und in ihm aktiviert wurden.

> [!NOTE]
> Zunächst müssen Sie das Sprachmodell erstellen und seine Dateien hochladen. Sie können Dateien beim Erstellen des Sprachmodells oder durch Aktualisieren des Sprachmodells hochladen.

### <a name="response"></a>Antwort

Die Antwort stellt Metadaten zum neu trainierten Sprachmodell zusammen mit Metadaten zu jeder der Dateien des Modells im Format der JSON-Beispielausgabe bereit:

```json
{
    "id": "41464adf-e432-42b1-8e09-f52905d7e29d",
    "name": "TestModel",
    "language": "En-US",
    "state": "Waiting",
    "languageModelId": "531e5745-681d-4e1d-b124-12e5ab57a891",
    "files": [
    {
        "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
        "name": "RenamedFile",
        "enable": false,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.5233333"
    },
    {
        "id": "9ac35b4b-1381-49c4-9fe4-8234bfdd0f50",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.68"
    }
    ]
}
```

Die zurückgegebene `id` ist eine eindeutige ID, die zur Unterscheidung von Sprachmodellen genutzt wird. `languageModelId` wird dagegen sowohl für APIs zum [Hochladen eines Videos in den Index](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Upload-Video) als auch [zum erneuten Indizieren eines Videos](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Re-Index-Video) (in Video Indexer-APIs für Upload bzw. Neuindizierung auch als `linguisticModelId` bezeichnet) verwendet.

## <a name="delete-a-language-model"></a>Löschen eines Sprachmodells

Die API zum [Löschen eines Sprachmodells](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Delete-Language-Model) löscht ein benutzerdefiniertes Sprachmodell aus dem angegebenen Konto. Jedes Video, das das gelöschte Sprachmodell verwendet hat, behält den gleichen Index bei, bis Sie das Video erneut indizieren. Wenn Sie das Video erneut indizieren, können Sie dem Video ein neues Sprachmodell zuweisen. Andernfalls verwendet Video Indexer das Standardmodell, um das Video erneut zu indizieren.

### <a name="response"></a>Antwort

Es wird kein Inhalt zurückgegeben, wenn das Sprachmodell erfolgreich gelöscht wurde.

## <a name="update-a-language-model"></a>Aktualisieren eines Sprachmodells

Die API zum [Aktualisieren eines Sprachmodells](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Update-Language-Model) aktualisiert ein benutzerdefiniertes Sprachmodell im angegebenen Konto.

> [!NOTE]
> Sie müssen das Sprachmodell bereits erstellt haben. Mit diesem Aufruf können Sie alle Dateien im Modell aktivieren oder deaktivieren, den Namen des Sprachmodells aktualisieren und Dateien hochladen, die dem Sprachmodell hinzugefügt werden sollen.

Um Dateien hochzuladen, die dem Sprachmodell hinzugefügt werden sollen, müssen Sie Dateien im Textkörper mithilfe von FormData hochladen und zusätzlich Werte für die oben genannten erforderlichen Parameter angeben. Hierfür gibt es zwei Möglichkeiten:

* Der Schlüssel ist der Dateiname und der Wert die TXT-Datei.
* Der Schlüssel ist der Dateiname und der Wert eine URL zur TXT-Datei.

### <a name="response"></a>Antwort

Die Antwort stellt Metadaten zum neu trainierten Sprachmodell zusammen mit Metadaten zu jeder der Dateien des Modells im Format der JSON-Beispielausgabe bereit:

```json
{
    "id": "41464adf-e432-42b1-8e09-f52905d7e29d",
    "name": "TestModel",
    "language": "En-US",
    "state": "Waiting",
    "languageModelId": "531e5745-681d-4e1d-b124-12e5ab57a891",
    "files": [
    {
        "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
        "name": "RenamedFile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.5233333"
    },
    {
        "id": "9ac35b4b-1381-49c4-9fe4-8234bfdd0f50",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.68"
    }
    ]
}
```

Verwenden Sie die `id` der in der Antwort zurückgegebenen Dateien, um den Inhalt der Datei herunterzuladen.

## <a name="update-a-file-from-a-language-model"></a>Aktualisieren einer Datei aus einem Sprachmodell

Die API zum [Aktualisieren einer Datei](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Update-Language-Model-file) ermöglicht Ihnen, den Namen und den `enable`-Zustand einer Datei in einem benutzerdefinierten Sprachmodell im angegebenen Konto zu aktualisieren.

### <a name="response"></a>Antwort

Die Antwort stellt Metadaten im Format der unten gezeigten JSON-Beispielausgabe für die Datei bereit, die Sie aktualisiert haben.

```json
{
  "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
  "name": "RenamedFile",
  "enable": false,
  "creator": "John Doe",
  "creationTime": "2018-04-27T20:10:10.5233333"
}
```

Verwenden Sie die `id` der in der Antwort zurückgegebenen Datei, um den Inhalt der Datei herunterzuladen.

## <a name="get-a-specific-language-model"></a>Abrufen eines bestimmten Sprachmodells

Die API [get](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Get-Language-Model) gibt Informationen zum angegebenen Sprachmodell im angegebenen Konto zurück, z. B. die Sprache und die Dateien, die sich im Sprachmodell befinden.

### <a name="response"></a>Antwort

Die Antwort stellt Metadaten zum angegebenen Sprachmodell zusammen mit Metadaten zu jeder der Dateien des Modells im Format der gezeigten JSON-Beispielausgabe bereit:

```json
{
    "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
    "name": "TestModel",
    "language": "En-US",
    "state": "None",
    "languageModelId": "00000000-0000-0000-0000-000000000000",
    "files": [
    {
        "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.6733333"
    },
    {
        "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
        "name": "worldfile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.86"
    }
    ]
}
```

Verwenden Sie die `id` der in der Antwort zurückgegebenen Datei, um den Inhalt der Datei herunterzuladen.

## <a name="get-all-the-language-models"></a>Abrufen aller Sprachmodelle

Die API [get all](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Get-Language-Models) gibt alle benutzerdefinierten Sprachmodelle im angegebenen Konto in einer Liste zurück.

### <a name="response"></a>Antwort

Die Antwort enthält eine Liste mit allen Sprachmodellen in Ihrem Konto und deren Metadaten und Dateien im Format der gezeigten JSON-Beispielausgabe:

```json
[
    {
        "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
        "name": "TestModel",
        "language": "En-US",
        "state": "None",
        "languageModelId": "00000000-0000-0000-0000-000000000000",
        "files": [
        {
            "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
            "name": "hellofile",
            "enable": true,
            "creator": "John Doe",
            "creationTime": "2018-04-28T11:55:34.6733333"
        },
        {
            "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
            "name": "worldfile",
            "enable": true,
            "creator": "John Doe",
            "creationTime": "2018-04-28T11:55:34.86"
        }
        ]
    },
    {
        "id": "dfae5745-6f1d-4edd-b224-42e1ab57a892",
        "name": "AnotherTestModel",
        "language": "En-US",
        "state": "None",
        "languageModelId": "00000000-0000-0000-0000-000000000001",
        "files": []
    }
]
```

## <a name="delete-a-file-from-a-language-model"></a>Löschen einer Datei aus einem Sprachmodell

Die API [delete](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Delete-Language-Model-File) löscht die angegebene Datei aus dem angegebenen Sprachmodell im angegebenen Konto.

### <a name="response"></a>Antwort

Es wird kein Inhalt zurückgegeben, wenn die Datei erfolgreich aus dem Sprachmodell gelöscht wurde.

## <a name="get-metadata-on-a-file-from-a-language-model"></a>Abrufen von Metadaten für eine Datei aus einem Sprachmodell

Die API zum [Abrufen der Metadaten einer Datei](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Get-Language-Model-File-Data) gibt die Inhalte und Metadaten für die angegebene Datei aus dem ausgewählten Sprachmodell in Ihrem Konto zurück.

### <a name="response"></a>Antwort

Die Antwort enthält die Inhalte und Metadaten der Datei im JSON-Format, ähnlich wie in diesem Beispiel:

```json
{
    "content": "hello\r\nworld",
    "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
    "name": "Hello",
    "enable": true,
    "creator": "John Doe",
    "creationTime": "2018-04-27T20:10:10.5233333"
}
```

> [!NOTE]
> Der Inhalt dieser Beispieldatei sind die Wörter „hello“ und „world“ in zwei separaten Zeilen.

## <a name="download-a-file-from-a-language-model"></a>Herunterladen einer Datei aus einem Sprachmodell

Die API zum [Herunterladen einer Datei](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Download-Language-Model-File-Content) lädt eine Textdatei mit dem Inhalt der angegebenen Datei aus dem angegebenen Sprachmodell im angegebenen Konto herunter. Diese Textdatei sollte mit dem Inhalt der Textdatei übereinstimmen, die ursprünglich hochgeladen wurde.

### <a name="response"></a>Antwort

Die Antwort ist der Download einer Textdatei mit dem Inhalt der Datei im JSON-Format.

## <a name="next-steps"></a>Nächste Schritte

[Anpassen des Sprachmodells mit der Website](customize-language-model-with-website.md)
