---
title: Unterschiede bei den Funktionen zwischen Azure Media Services V2 und V3
description: In diesem Artikel werden die Unterschiede bei den Funktionen zwischen Azure Media Services V2 und V3 beschrieben.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.devlang: multiple
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: 2fa827bc2841a0bae4c9646c8a70e42dc2b500e3
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98898408"
---
# <a name="feature-gaps-between-azure-media-services-v2-and-v3"></a>Unterschiede bei den Funktionen zwischen Azure Media Services V2 und V3

![Logo des Migrationsleitfadens](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![Schritte bei der Migration 2](./media/migration-guide/steps-2.svg)

Dieser Teil des Migrationsleitfadens enthält detaillierte Informationen zu den Unterschieden zwischen den V2- und V3-APIs.

## <a name="feature-gaps-between-v2-and-v3-apis"></a>Funktionslücken zwischen den V2- und V3-APIs

Die V3-API weist in Bezug auf die V2-API die folgenden Funktionslücken auf. Einige der erweiterten Funktionen von Media Encoder Standard in V2-APIs sind derzeit in V3 nicht verfügbar:

- Einfügen einer stillen Audiospur bei einer Eingabe ohne Audio, da dies für Azure Media Player nicht mehr erforderlich ist

- Einfügen einer Videospur bei einer Eingabe ohne Video

- Liveereignisse mit Transcodierung unterstützen derzeit keine Slate-Einfügungen beim Streamen und Werbemarkereinfügungen über API-Aufrufe.

- Der Azure Media Encoder Premium wird in V2 nicht mehr unterstützt. Wenn Sie ihn für die 8-Bit-HEVC-Codierung verwenden, sollten Sie die neue HEVC-Unterstützung im Media Encoder Standard verwenden. 
    - Im Media Encoder Standard wurde Unterstützung für die Audiokanalzuordnung hinzugefügt.  Weitere Informationen finden Sie in der Dokumentation zu [Audiodaten bei der Media Services-Codierung mit Swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2020-05-01/Encoding.json).
    - Wenn Sie erweiterte Funktionen oder Ausgabeformate des lizenzierten Produkts eines Drittanbieters wie MXF oder ProRes verwenden, setzen Sie die Azure-Partnerlösung von Telestream ein, die nach der Einstellung von V2 transaktional ist. Alternativ können Sie auch Imagine Communications oder [Bitmovin](http://bitmovin.com) verwenden.

- Die Eigenschaft „Verfügbarkeitsgruppe“ für den Streamingendpunkt wird in V2 wird nicht mehr unterstützt. Weitere Informationen finden Sie im Beispielprojekt und im Leitfaden zur Übertragung von [VoD mit Hochverfügbarkeit](https://docs.microsoft.com/azure/media-services/latest/media-services-high-availability-encoding) in der V3-API.

- In Media Services V3 kann FairPlay IV nicht angegeben werden. Obwohl es keine Auswirkungen auf Kunden hat, die Media Services sowohl für das Packen als auch für die Lizenzbereitstellung nutzen, kann es ein Problem darstellen, wenn ein DRM-System eines Drittanbieters zur Bereitstellung der FairPlay-Lizenzen verwendet wird (Hybridmodus).

- Die clientseitige Speicherverschlüsselung für den Schutz von ruhenden Medienobjekten wurde in der V3-API entfernt und durch die Verschlüsselung des Storage-Diensts für ruhende Daten ersetzt. Die V3-APIs funktionieren auch weiterhin mit vorhandenen speicherverschlüsselten Medienobjekten, lassen jedoch nicht die Erstellung neuer Objekte zu.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
