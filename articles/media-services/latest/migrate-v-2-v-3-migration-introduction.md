---
title: Einführung in die Migration von Azure Media Services V2 zu V3 | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Einführung in die Migration von Media Services V2 zu V3.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: fb9abd8f3186405edc31b4af48ee98482e080c68
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98689474"
---
# <a name="migrate-from-media-services-v2-to-v3-introduction"></a>Einführung in die Migration von Media Services V2 zu V3

![Logo des Migrationsleitfadens](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

Der Migrationsleitfaden für Media Services unterstützt Sie bei der Migration von den Media Services V2-APIs zu den V3-APIs. Durch diese Migration können Sie die neuen Features und Funktionen nutzen, die jetzt verfügbar sind. Entscheiden Sie selbst, was für Ihr Szenario am besten geeignet ist.

## <a name="how-to-use-this-guide"></a>So verwenden Sie diesen Leitfaden

### <a name="navigating"></a>Navigation

Im gesamten Leitfaden wird die folgende Grafik angezeigt.

![Schritte bei der Migration](./media/migration-guide/steps.svg)<br/>

Der aktuelle Schritt wird durch eine Farbänderung in der Schrittnummer angezeigt, wie im folgenden Beispiel gezeigt:

![Schritte bei der Migration 2](./media/migration-guide/steps-2.svg)<br/>

Am Ende jeder Seite finden Sie unter der Überschrift **Nächste Schritte** Links zu den restlichen Migrationsdokumenten.

### <a name="guidance"></a>Anleitungen

Der hier bereitgestellte Leitfaden ist *allgemein* gehalten. Er enthält Informationen zu Funktionen, die in V3 verfügbar sind, und zu Änderungen bei den Media Services-Workflows.

Ausführlichere Leitfäden mit Screenshots und Konzeptgrafiken finden Sie unter den Links zu Konzepten, Tutorials, Schnellstarts, Beispielen und API-Referenzen der einzelnen szenariobasierten Themen. Sie finden hier auch Beispiele für Vergleiche zwischen der V2-API und der V3-API.

## <a name="migration-guidance-overview"></a>Übersicht über den Migrationsleitfaden

Es gibt vier allgemeine Schritte, die während der Migration ausgeführt werden müssen:

## <a name="step-1-benefits"></a>Schritt 1: Vorteile

<hr color="#5ea0ef" size="10">

[Informieren Sie sich über die Vorteile](migrate-v-2-v-3-migration-benefits.md) der Migration zur Media Services-API V3.

## <a name="step-2-differences"></a>Schritt 2: Unterschiede

<hr color="#5ea0ef" size="10">

Informieren Sie sich über die Unterschiede zwischen der Media Services-API V2 und der V3-API.

- [API-Zugriff](migrate-v-2-v-3-differences-api-access.md)
- [Funktionslücken](migrate-v-2-v-3-differences-feature-gaps.md)
- [Terminologie- und Entitätsänderungen](migrate-v-2-v-3-differences-terminology.md)

## <a name="step-3-sdk-setup"></a>Schritt 3: SDK-Setup

<hr color="#5ea0ef" size="10">

Informieren Sie sich über die SDK-Unterschiede und das [Setup für die Migration zur V3-REST-API oder zum Client-SDK](migrate-v-2-v-3-migration-setup.md).

## <a name="step-4-scenario-based-guidance"></a>Schritt 4: Szenariobasierter Leitfaden

<hr color="#5ea0ef" size="10">

Ihre Anwendung von Media Services V2 ist möglicherweise sehr speziell. Daher stehen Ihnen szenariobasierte Leitfäden zur Verfügung, die basierend darauf, wie Sie Media Services *eventuell* in der Vergangenheit verwendet haben, Schritte für die einzelnen Dienstfeatures bereitstellen, z. B.:

- [Codieren](migrate-v-2-v-3-migration-scenario-based-encoding.md)
- [Livestreaming](migrate-v-2-v-3-migration-scenario-based-live-streaming.md)
- [Paketerstellung und Bereitstellung](migrate-v-2-v-3-migration-scenario-based-publishing.md)
- [Inhaltsschutz](migrate-v-2-v-3-migration-scenario-based-content-protection.md)
- [Reservierte Einheiten für Medien (MRU)](migrate-v-2-v-3-migration-scenario-based-media-reserved-units.md)

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
