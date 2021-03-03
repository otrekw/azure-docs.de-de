---
title: 'Gewusst wie: Erstellen einer Überlagerung mit Media Encoder Standard'
description: Erfahren Sie, wie Sie eine Überlagerung mit Media Encoder Standard erstellen.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 08/31/2020
ms.openlocfilehash: 69930e06e2ce7f2679feec74ca8ccbc93bdb8d30
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2021
ms.locfileid: "101721122"
---
# <a name="how-to-create-an-overlay-with-media-encoder-standard"></a>Gewusst wie: Erstellen einer Überlagerung mit Media Encoder Standard

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Mit Media Encoder Standard können Sie Videos mit einem Bild, einer Audiodatei oder einem anderen Video überlagern. Als Eingabe muss genau eine Datei angegeben werden. Sie können eine Bilddatei im JPG-, PNG-, GIF- oder BMP-Format oder eine Audiodatei (z. B. eine WAV-, MP3-, WMA- oder M4A-Datei) oder eine Videodatei angeben.


## <a name="prerequisites"></a>Voraussetzungen

* Sammeln Sie die Kontoinformationen, die Sie zum Konfigurieren der Datei *appSettings.json* im Beispiel benötigen. Wenn Sie sich nicht sicher sind, wie dies zu tun ist, finden Sie Informationen hierzu im [Schnellstart: Registrieren einer Anwendung bei der Microsoft Identity Platform](../../active-directory/develop/quickstart-register-app.md). Die folgenden Werte werden in der Datei *appsettings.json* erwartet.

    ```json
    {
    "AadClientId": "",
    "AadEndpoint": "https://login.microsoftonline.com",
    "AadSecret": "",
    "AadTenantId": "",
    "AccountName": "",
    "ArmAadAudience": "https://management.core.windows.net/",
    "ArmEndpoint": "https://management.azure.com/",
    "Location": "",
    "ResourceGroup": "",
    "SubscriptionId": ""
    }
    ```

Wenn Sie sich mit Transformationen noch nicht auskennen, wird empfohlen, dass Sie die folgenden Aktivitäten ausführen:

* Lesen Sie [Codieren von Video- und Audiodaten mit Media Services](encoding-concept.md).
* Lesen Sie [Codieren mit einer benutzerdefinierten Transformation – .NET](customize-encoder-presets-how-to.md). Führen Sie die Schritte in diesem Artikel aus, um .NET einzurichten, das für die Arbeit mit Transformationen erforderlich ist, und kehren Sie dann hierher zurück, um ein vordefiniertes Beispiel für Überlagerungen auszuprobieren.
* Weitere Informationen finden Sie im [Referenzdokument zu Transformationen](/rest/api/media/transforms).

Sobald Sie sich mit Transformationen vertraut gemacht haben, laden Sie das Beispiel für Überlagerungen herunter.

## <a name="overlays-preset-sample"></a>Vordefiniertes Beispiel für Überlagerungen

Laden Sie das [media-services-overlay-sample](https://github.com/Azure-Samples/media-services-overlays) herunter, um mit Überlagerungen zu beginnen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [transforms next steps](./includes/transforms-next-steps.md)]
