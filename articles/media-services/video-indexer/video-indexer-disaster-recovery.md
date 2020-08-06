---
title: Video Indexer-Failover und -Notfallwiederherstellung
titleSuffix: Azure Media Services
description: Erfahren Sie, wie Sie im Falle eines Ausfalls oder Notfalls des regionalen Rechenzentrums ein Failover auf ein sekundäres Video Indexer-Konto ausführen.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 07/29/2019
ms.author: juliako
ms.openlocfilehash: eab376c44065979de86e5c70b796be952fccffaa
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87065401"
---
# <a name="video-indexer-failover-and-disaster-recovery"></a>Video Indexer-Failover und -Notfallwiederherstellung

Azure Media Services Video Indexer bietet kein sofortiges Failover des Diensts, wenn es zu einem Ausfall oder Fehler des regionalen Rechenzentrums gekommen ist. In diesem Artikel wird erläutert, wie Sie Ihre Umgebung für ein Failover konfigurieren, um eine optimale Verfügbarkeit für Apps und eine minimale Wiederherstellungszeit bei einem Notfall sicherzustellen.

Wir empfehlen das Konfigurieren von Business Continuity Disaster Recovery (BCDR) zwischen Regionalpaaren, um von Richtlinien für Isolierung und Verfügbarkeit von Azure zu profitieren. Weitere Informationen finden Sie unter [Azure-Regionspaare](../../best-practices-availability-paired-regions.md).

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure-Abonnement. Wenn Sie noch nicht über ein Azure-Abonnement verfügen, können Sie sich für ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) registrieren.

## <a name="failover-to-a-secondary-account"></a>Failover auf ein sekundäres Konto

Um BCDR zu implementieren, müssen Sie über zwei Video Indexer-Konten verfügen, um Redundanz sicherzustellen.

1. Erstellen Sie zwei Video Indexer-Konten, die mit Azure verbunden sind (siehe [Erstellen eines Video Indexer-Kontos](connect-to-azure.md)). Erstellen Sie ein Konto für Ihre primäre Region und das andere für die gekoppelte Azure-Region.
1. Wenn in Ihrer primären Region ein Fehler auftritt, wechseln Sie zur Indizierung mit dem sekundären Konto.

> [!TIP]
> Sie können BCDR automatisieren, indem Sie Aktivitätsprotokollwarnungen für Dienstintegritätsbenachrichtigungen gemäß den Ausführungen unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](../../service-health/alerts-activity-log-service-notifications-portal.md) einrichten.

Informationen zum Verwenden mehrerer Mandanten finden Sie unter [Verwalten mehrerer Mandanten](manage-multiple-tenants.md). Wählen Sie zum Implementieren von BCDR eine der folgenden beiden Optionen aus: [Video Indexer-Konto pro Mandant](manage-multiple-tenants.md#video-indexer-account-per-tenant) oder [Azure-Abonnement pro Mandant](manage-multiple-tenants.md#azure-subscription-per-tenant).

## <a name="next-steps"></a>Nächste Schritte

[Verwalten eines Video Indexer-Kontos mit Azure-Verbindung](manage-account-connected-to-azure.md)
