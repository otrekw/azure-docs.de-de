---
title: Failover und Notfallwiederherstellung von Azure Video Analyzer for Media (ehemals Video Indexer)
titleSuffix: Azure Video Analyzer for Media
description: Erfahren Sie, wie Sie im Falle eines Ausfalls oder Notfalls des regionalen Rechenzentrums ein Failover auf ein sekundäres Konto bei Azure Video Analyzer for Media (ehemals Video Indexer) ausführen.
services: azure-video-analyzer
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.workload: ''
ms.topic: article
ms.subservice: azure-video-analyzer-media
ms.custom: ''
ms.date: 07/29/2019
ms.author: juliako
ms.openlocfilehash: a7361f75a9327a6fbc88132e67e87f9be7ed5f84
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2021
ms.locfileid: "112118837"
---
# <a name="video-analyzer-for-media-failover-and-disaster-recovery"></a>Video Analyzer for Media-Failover und Notfallwiederherstellung

Azure Video Analyzer for Media (ehemals Video Indexer) bietet kein sofortiges Failover des Diensts, wenn es zu einem Ausfall oder Fehler des regionalen Rechenzentrums gekommen ist. In diesem Artikel wird erläutert, wie Sie Ihre Umgebung für ein Failover konfigurieren, um eine optimale Verfügbarkeit für Apps und eine minimale Wiederherstellungszeit bei einem Notfall sicherzustellen.

Wir empfehlen das Konfigurieren von Business Continuity Disaster Recovery (BCDR) zwischen Regionalpaaren, um von Richtlinien für Isolierung und Verfügbarkeit von Azure zu profitieren. Weitere Informationen finden Sie unter [Azure-Regionspaare](../../best-practices-availability-paired-regions.md).

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure-Abonnement. Wenn Sie noch nicht über ein Azure-Abonnement verfügen, können Sie sich für ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) registrieren.

## <a name="failover-to-a-secondary-account"></a>Failover auf ein sekundäres Konto

Um BCDR zu implementieren, müssen Sie über zwei Azure Video Analyzer for Media-Konten verfügen, um Redundanz sicherzustellen.

1. Erstellen Sie zwei Video Analyzer for Media-Konten, die mit Azure verbunden sind (siehe [Erstellen eines Video Analyzer for Media-Kontos).](connect-to-azure.md) Erstellen Sie ein Konto für Ihre primäre Region und das andere für die gekoppelte Azure-Region.
1. Wenn in Ihrer primären Region ein Fehler auftritt, wechseln Sie zur Indizierung mit dem sekundären Konto.

> [!TIP]
> Sie können BCDR automatisieren, indem Sie Aktivitätsprotokollwarnungen für Dienstintegritätsbenachrichtigungen gemäß den Ausführungen unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](../../service-health/alerts-activity-log-service-notifications-portal.md) einrichten.

Informationen zum Verwenden mehrerer Mandanten finden Sie unter [Verwalten mehrerer Mandanten](manage-multiple-tenants.md). Wählen Sie zum Implementieren von BCDR eine der beiden folgenden Optionen aus: [Video Analyzer for Media-Konto pro Mandant](./manage-multiple-tenants.md#video-analyzer-for-media-account-per-tenant) oder [Azure-Abonnement pro Mandant](./manage-multiple-tenants.md#azure-subscription-per-tenant).

## <a name="next-steps"></a>Nächste Schritte

[Verwalten sie ein Video Analyzer for Media-Konto, das mit Azure verbunden ist](manage-account-connected-to-azure.md).
