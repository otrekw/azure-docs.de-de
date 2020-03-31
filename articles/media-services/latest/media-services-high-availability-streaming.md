---
title: Hochverfügbarkeitsstreaming für Azure Media Services
description: Erfahren Sie, wie Sie im Falle eines Ausfalls oder Fehlers des regionalen Rechenzentrums ein Failover auf ein sekundäres Media Services-Konto ausführen.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: ''
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 02/24/2020
ms.author: juliako
ms.openlocfilehash: 1492dd392eabc4331f8e3d4604fb245a89dedff5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898988"
---
# <a name="media-services-high-availability-streaming"></a>Hochverfügbarkeitsstreaming für Media Services

Azure Media Services bietet derzeit keinen sofortigen Failover für den Dienst, wenn es einen Ausfall eines regionalen Rechenzentrums oder einen Ausfall der zugrunde liegenden Komponente oder abhängiger Dienste gibt. In diesem Artikel finden Sie einen Leitfaden für die Erstellung von regionsübergreifendem Video-on-Demand-Streaming.

## <a name="prerequisites"></a>Voraussetzungen

Lesen Sie [Erstellen eines überregionalen Codierungssystems](media-services-high-availability-encoding.md).

## <a name="how-to-build-video-on-demand-cross-region-streaming"></a>Erstellen von regionsübergreifendem Video on Demand-Streaming 

* Beim regionsübergreifenden Video on Demand-Streaming werden [Medienobjekte](assets-concept.md), [Inhaltsschlüsselrichtlinien](content-key-policy-concept.md) (falls verwendet), [Streamingrichtlinien](streaming-policy-concept.md) und [Streaminglocators](streaming-locators-concept.md) dupliziert. 
* Sie müssen die Richtlinien in beiden Regionen erstellen und sie auf dem neuesten Stand halten. 
* Wenn Sie die Streaminglocators erstellen, sollten Sie dieselben Werte für „Locator ID“, „ContentKey ID“ und „ContentKey“ verwenden.  
* Wenn Sie den Inhalt codieren, empfiehlt es sich, den Inhalt in Region A zu codieren und zu veröffentlichen, dann den codierten Inhalt in Region B zu kopieren und ihn mit den gleichen Werten wie von Region A zu veröffentlichen.
* Sie können den Traffic Manager für die Hostnamen für den Ursprung und den Schlüsselübermittlungsdienst verwenden (in der Media-Services-Konfiguration sieht dies wie eine benutzerdefinierte Schlüsselserver-URL aus).

## <a name="next-steps"></a>Nächste Schritte

Auschecken:

* [Tutorial: Codieren einer Remotedatei anhand einer URL und Streamen des Videos](stream-files-dotnet-quickstart.md)
* [Codebeispiele](https://docs.microsoft.com/samples/browse/?products=azure-media-services)
