---
title: 'Gewusst wie: Erstellen einer Überlagerung mit Media Encoder Standard'
description: Erfahren Sie, wie Sie eine Überlagerung mit Media Encoder Standard erstellen.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 07/27/2020
ms.openlocfilehash: 85b1b6c56221deaa03057a7ccb658b4bf67124eb
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87830579"
---
# <a name="how-to-create-an-overlay-with-media-encoder-standard"></a>Gewusst wie: Erstellen einer Überlagerung mit Media Encoder Standard

Media Encoder Standard ermöglicht die Überlagerung eines Bildes mit einem vorhandenen Video. Derzeit werden die folgenden Formate unterstützt: png, jpg, gif und bmp.

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
    "Region": "",
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

* [Erstellen von Subclips für ein Video beim Codieren mit Media Services: .NET](subclip-video-dotnet-howto.md)