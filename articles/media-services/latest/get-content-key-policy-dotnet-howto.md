---
title: Abrufen eines Signaturschlüssels aus einer Richtlinie mit Azure Media Services v3 .NET
description: In diesem Thema wird gezeigt, wie Sie mit Media Services V3 .NET SDK einen Signaturschlüssel aus der vorhandenen Richtlinie abrufen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18
ms.openlocfilehash: 7f46c77d463873a5cdd5d8c4ac1b28f1b7d0b9eb
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89298996"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Abrufen eines Signaturschlüssels aus der vorhandenen Richtlinie

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Eines der wichtigsten Entwurfsprinzipien der v3-API ist es, die API sicherer zu machen. v3-APIs geben bei einem **Get**- oder **List**-Vorgang keine geheimen Schlüssel oder Anmeldeinformationen zurück. Die ausführliche Erläuterung finden Sie hier: Weitere Informationen finden Sie unter [RBAC and Media Services accounts (RBAC und Media Services-Konten)](rbac-overview.md).

Anhand des Beispiels in diesem Artikel wird veranschaulicht, wie .NET verwendet wird, um einen Signaturschlüssel aus der vorhandenen Richtlinie abzurufen. 
 
## <a name="download"></a>Download 

Klonen Sie ein GitHub-Repository auf Ihren Computer, das das vollständige .NET-Beispiel enthält, indem Sie den folgenden Befehl verwenden:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Die Richtlinie „ContentKeyPolicy“ mit Beispiel für Geheimnisse befindet sich im Ordner [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM).

## <a name="get-contentkeypolicy-with-secrets"></a>Abrufen von „ContentKeyPolicy“ mit Geheimnissen 

Um den Schlüssel abzurufen, verwenden Sie **GetPolicyPropertiesWithSecretsAsync**, wie im folgenden Beispiel gezeigt.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Nächste Schritte

[Entwurf eines Multi-DRM-Inhaltsschutzsystems mit Zugriffssteuerung](design-multi-drm-system-with-access-control.md) 
