---
title: Importieren der Postman-Umgebung für Azure Media Services-REST-Aufrufe
description: Dieses Thema enthält eine Definition der Postman-Umgebung für Azure Media Services- REST-Aufrufe.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 2ccc765a3fb0063d92382edcec260808183f27a2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89259250"
---
# <a name="import-the-postman-environment"></a>Importieren der Postman-Umgebung

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)] 

Dieser Artikel enthält eine Definition der **Postman**-Umgebungsvariablen, die in der [Postman-Sammlung](postman-collection.md) verwendet werden, die gruppierte HTTP-Anforderungen enthält, die Media Services-REST-APIs aufrufen. Die Umgebungs- und Sammlungsdateien werden vom Tutorial [Konfigurieren von Postman für Media Services-REST-API-Aufrufe](media-rest-apis-with-postman.md) verwendet.

> [!NOTE]
> Der Wert von `AzureADSTSEndpoint ` = `https://login.microsoftonline.com/{{TenantId}}/oauth2/token`. Um Ihre Mandanten-ID zu erhalten, können Sie mit der Maus im Portal auf Ihren Benutzernamen zeigen (in der rechten oberen Ecke). Sie befindet sich dann unter „Verzeichnis: Microsoft ({{MANDANTEN-ID}})“.

```
{
  "id": "2dbce3ce-74c2-2ceb-0461-c4c2323f5b09",
  "name": "AzureMedia",
  "values": [
    {
      "enabled": true,
      "key": "TenantID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AzureADSTSEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "RESTAPIEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientSecret",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AccessToken",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAssetId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAccessPolicyId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "UploadURL",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "MediaFileName",
      "value": "BigBuckBunny.mp4",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastChannelId",
      "value": "",
      "type": "text"
    }
  ],
  "_postman_variable_scope": "environment",
  "_postman_exported_using": "Postman/5.5.0"
}
```
