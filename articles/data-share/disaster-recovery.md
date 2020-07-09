---
title: Notfallwiederherstellung für Azure Data Share
description: Notfallwiederherstellung für Azure Data Share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: how-to
ms.date: 12/18/2019
ms.openlocfilehash: 8c53450094801825b86b74505fbe34dc8defe4f6
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86110963"
---
# <a name="disaster-recovery-for-azure-data-share"></a>Notfallwiederherstellung für Azure Data Share

In diesem Artikel werden die Schritte zum Konfigurieren einer Notfallwiederherstellungsumgebung für Azure Data Share beschrieben. Ausfälle von Azure-Rechenzentren sind selten, können aber von wenigen Minuten bis zu mehreren Stunden dauern. Rechenzentrumsausfälle können zu Störungen bei Umgebungen führen, die davon abhängen, dass Daten vom Datenanbieter freigegeben werden. Anhand der in diesem Artikel beschriebenen Schritte können Datenanbieter weiterhin Daten für Ihre Datenconsumer freigeben, wenn ein Rechenzentrum in der primären Region, die die Datenfreigabe hostet, ausgefallen ist. 

## <a name="achieving-business-continuity-for-azure-data-share"></a>Erreichen der Geschäftskontinuität für Azure Data Share

Für Fall, dass ein Rechenzentrum ausfällt, kann der Datenanbieter eine Datenfreigabeumgebung in einer sekundären Region bereitstellen. Es gibt Maßnahmen, mit denen ein reibungsloses Failover sichergestellt werden kann, falls ein Rechenzentrum ausfällt. 

Datenanbieter können sekundäre Azure Data Share-Ressourcen in einer zusätzlichen Region bereitstellen. Diese Azure Data Share-Ressourcen können so konfiguriert werden, dass sie Datasets umfassen, die in der primären Datenfreigabeumgebung vorhanden sind. Datenconsumer können der Datenfreigabe beim Konfigurieren der Notfallwiederherstellungsumgebung oder zu einem späteren Zeitpunkt hinzugefügt werden (d. h. im Rahmen manueller Failoverschritte).

Wenn die Datenconsumer in einer sekundären Umgebung, die für Notfallwiederherstellungszwecke bereitgestellt wurde, über ein aktives Freigabeabonnement verfügen, können sie den Momentaufnahmezeitplan im Rahmen eines Failovers aktivieren. Wenn die Datenconsumer keine sekundäre Region für Notfallwiederherstellungszwecke abonnieren möchten, können sie zu einem späteren Zeitpunkt in die sekundäre Datenfreigabe eingeladen werden. 

Datenconsumer können entweder über ein aktives Freigabeabonnement verfügen, das für Notfallwiederherstellungszwecke inaktiv ist. Datenanbieter können Datenconsumer auch zu einem späteren Zeitpunkt im Rahmen manueller Failoverprozeduren hinzufügen. 

## <a name="related-information"></a>Verwandte Informationen

- [Business Continuity & Disaster Recovery](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
- [Hochverfügbarkeit – ein Teil Ihrer BCDR-Strategie](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/build-high-availability-into-your-bcdr-strategy)

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md).




