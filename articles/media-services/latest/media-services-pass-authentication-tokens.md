---
title: Übergeben von Authentifizierungstoken an Media Services v3 | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Authentifizierungstoken vom Client an den Schlüsselbereitstellungsdienst von Media Services v3 senden.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: how-to
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: 590309ad392876ba3c5cb6d21abe777ab5a93efb
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105572299"
---
# <a name="pass-tokens-to-the-azure-media-services-v3-key-delivery-service"></a>Übergeben von Token an den Schlüsselbereitstellungsdienst von Media Services v3

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Kunden fragen häufig, wie ein Player Token für die Überprüfung an den Schlüsselübermittlungsdienst von Azure Media Services übergeben kann, damit der Player den Schlüssel abrufen kann. Media Services unterstützt die Formate „Simple Web Token“ (SWT) und „JSON Web Token“ (JWT). Die Tokenauthentifizierung kann auf jede Art von Schlüssel angewendet werden, unabhängig davon, ob Sie die Common Encryption oder die AES-Umschlagverschlüsselung (Advanced Encryption Standard) im System verwenden.

 Je nach Player und Zielplattform haben Sie folgende Möglichkeiten, das Token mit Ihrem Player zu übergeben:

## <a name="pass-a-token-through-the-http-authorization-header"></a>Übergeben eines Tokens über den HTTP-Autorisierungsheader

> [!NOTE]
> Das Präfix „Bearer“ wird gemäß den OAuth 2.0-Spezifikationen erwartet. Wählen Sie **AES (JWT-Token)** oder **AES (SWT-Token)** aus, um die Videoquelle festzulegen. Das Token wird über den Autorisierungsheader übergeben.

## <a name="pass-a-token-via-the-addition-of-a-url-query-parameter-with-tokentokenvalue"></a>Übergeben eines Tokens durch Hinzufügen eines URL-Abfrageparameters mit „token=tokenvalue“.

> [!NOTE]
> Das Präfix „Bearer“ wird nicht erwartet. Da das Token über eine URL gesendet wird, müssen Sie die Tokenzeichenfolge schützen. Hier folgt ein C#-Beispielcode zur Veranschaulichung der Vorgehensweise:

```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
```

## <a name="pass-a-token-through-the-customdata-field"></a>Übergeben eines Tokens über das CustomData-Feld

Diese Option wird nur für den Erwerb von PlayReady-Lizenzen über das Feld „CustomData“ der PlayReady License Acquisition Challenge verwendet. In diesem Fall muss sich das Token innerhalb des hier beschriebenen XML-Dokuments befinden:

```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
```

Platzieren Sie Ihr Authentifizierungstoken im Token-Element.
