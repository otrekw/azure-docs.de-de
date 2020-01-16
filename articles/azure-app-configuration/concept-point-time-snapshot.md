---
title: 'Azure App Configuration: Point-in-Time-Momentaufnahme'
description: Ein Überblick über die Funktionsweise von Point-in-Time-Momentaufnahmen in Azure App Configuration
services: azure-app-configuration
author: yegu-ms
ms.author: yegu
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.openlocfilehash: 4db52ce1897aa5a2b809cb7044b9764baffd0767
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/26/2019
ms.locfileid: "75495264"
---
# <a name="point-in-time-snapshot"></a>Point-in-Time-Momentaufnahme

Azure App Configuration dokumentiert die genauen Zeiten, zu denen ein neues Schlüssel-Wert-Paar erstellt und dann geändert wird. So entsteht eine lückenlose Chronik aller Schlüssel-Wert-Änderungen. Ein App Configuration-Speicher kann den Verlauf jedes beliebigen Schlüssel-Wert-Paars rekonstruieren und dessen Wert zu einem beliebigen Zeitpunkt (bis hin zum aktuellen Zeitpunkt) wiedergeben. Mithilfe dieses Features können Sie ein altes Schlüssel-Wert-Paar aus der „Vergangenheit“ abrufen. Beispielsweise können Sie die gestrigen (unmittelbar von vor der neuesten Bereitstellung stammenden) Konfigurationseinstellungen abrufen, um eine vorherige Konfiguration wiederherzustellen und ein Rollback der Anwendung durchzuführen.

## <a name="key-value-retrieval"></a>Abrufen von Schlüssel-Wert-Paaren

Wenn Sie Schlüssel-Wert-Paare aus der Vergangenheit abrufen möchten, geben Sie im HTTP-Header eines REST-API-Aufrufs einen Zeitpunkt für die Erstellung einer Momentaufnahme der Schlüssel-Wert-Paare an. Beispiel:

```rest
GET /kv HTTP/1.1
Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT
```

Der von App Configuration gespeicherte Änderungsverlauf umfasst derzeit sieben Tage.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer ASP.NET Core-Web-App](./quickstart-aspnet-core-app.md)  
