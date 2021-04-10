---
title: Migrationsleitfaden für reservierte Einheiten für Medien (Media Reserved Units, MRUs)
description: In diesem Artikel finden Sie einen szenariobasierten Migrationsleitfaden für reservierte Einheiten für Medien (MRUs), der Sie bei der Migration von Azure Media Services V2 zu V3 unterstützt.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 03/25/2021
ms.author: inhenkel
ms.openlocfilehash: 00e1fa623fc625a568f6580998443758c3916be8
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105563071"
---
# <a name="media-reserved-units-mrus-scenario-based-migration-guidance"></a>Szenariobasierter Migrationsleitfaden für reservierte Einheiten für Medien (Media Reserved Units, MRUs)

![Logo des Migrationsleitfadens](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![Schritte bei der Migration 2](./media/migration-guide/steps-4.svg)

In diesem Artikel finden Sie einen szenariobasierten Migrationsleitfaden für reservierte Einheiten für Medien (MRUs), der Sie bei der Migration von Azure Media Services V2 zu V3 unterstützt.

- Bei neuen V3-Konten, die im Azure-Portal oder mit der Version 2020-05-01 der V3-API erstellt wurden, müssen Sie keine reservierten Einheiten für Medien (MRUs) mehr festlegen. Das System skaliert jetzt basierend auf der Last automatisch hoch oder herunter.
- Wenn Sie über ein V3- oder V2-Konto verfügen, das vor der API-Version 2020-05-01 erstellt wurde, können Sie weiterhin die Parallelität und Leistung Ihrer Aufträge mithilfe von reservierten Einheiten für Medien steuern. Weitere Informationen finden Sie unter Übersicht über das Skalieren der Medienverarbeitung. Die reservierten Einheiten für Medien können mit der Befehlszeilenschnittstelle 2.0 für Media Services V3 oder über das Azure-Portal verwaltet werden.  
- Wenn die Option zum Verwalten von reservierten Einheiten für Medien im Azure-Portal nicht angezeigt wird, nutzen Sie ein Konto, das mit einer API ab Version 2020-05-01 erstellt wurde.
- Wenn Sie bereits wissen, wie Sie Ihren MRU-Typ auf „S3“ festlegen, können Sie damit die Leistung verbessern oder unverändert halten.
- Wenn Sie V2-Kunde sind, müssen Sie vor dem Abschluss der Migration ein neues V2-Konto erstellen, damit Ihre vorhandene Anwendung weiterhin unterstützt wird. 
- Indexer V1 oder andere Medienprozessoren, die noch nicht vollständig veraltet sind, müssen jedoch möglicherweise erneut aktiviert werden. 

Weitere Informationen zu reservierten Einheiten für Medien finden Sie unter [Reservierte Einheiten für Medien](concept-media-reserved-units.md) und [Skalieren reservierter Einheiten für Medien](media-reserved-units-cli-how-to.md).

## <a name="mru-concepts-tutorials-and-how-to-guides"></a>Konzepte, Tutorials und Schrittanleitungen für reservierte Einheiten für Medien

### <a name="concepts"></a>Konzepte

[Reservierte Einheiten für Medien](concept-media-reserved-units.md)

### <a name="how-to-guides"></a>Anleitungen

[Skalieren reservierter Einheiten für Medien](media-reserved-units-cli-how-to.md)

## <a name="samples"></a>Beispiele

Sehen Sie sich auch den [Vergleich von V2- und V3-Code in den Codebeispielen](migrate-v-2-v-3-migration-samples.md) an.
