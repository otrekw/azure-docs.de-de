---
title: Notfallwiederherstellung für Azure Data Share
description: Notfallwiederherstellung für Azure Data Share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 07/30/2020
ms.openlocfilehash: 1d7c9935d7e0d6bb2d457aa4c08f9b2b3e5fa910
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92218696"
---
# <a name="disaster-recovery-for-azure-data-share"></a>Notfallwiederherstellung für Azure Data Share

In diesem Artikel wird erläutert, wie Sie eine Notfallwiederherstellungsumgebung für Azure Data Share konfigurieren. Ausfälle von Azure-Rechenzentren sind selten, können aber von wenigen Minuten bis zu mehreren Stunden dauern. Rechenzentrumsausfälle können zu Störungen bei Umgebungen führen, die davon abhängen, dass Daten vom Datenanbieter freigegeben werden. Anhand der in diesem Artikel beschriebenen Schritte können Datenanbieter Daten für ihre Datenconsumer weiterhin freigeben, wenn ein Rechenzentrum in der primären Region, in der Ihre Datenfreigabe gehostet wird, ausgefallen ist. 

## <a name="achieving-business-continuity-for-azure-data-share"></a>Erreichen der Geschäftskontinuität für Azure Data Share

Für Fall, dass ein Rechenzentrum ausfällt, kann der Datenanbieter eine Datenfreigabeumgebung in einer sekundären Region bereitstellen. Maßnahmen können ergriffen werden, um beim Ausfall eines Rechenzentrums ein reibungsloses Failover sicherzustellen. 

Datenanbieter können sekundäre Azure Data Share-Ressourcen in einer zusätzlichen Region bereitstellen. Diese Data Share-Ressourcen können so konfiguriert werden, dass sie Freigaben und Datasets enthalten, die es in der primären Azure Data Share-Ressource gibt. Sie können Datenconsumer zu den sekundären Freigaben einladen, und zwar beim Konfigurieren der Notfallwiederherstellungsumgebung oder zu einem späteren Zeitpunkt (d. h. im Rahmen manueller Failoverschritte).

Wenn die Datenconsumer in einer sekundären Umgebung, die für Notfallwiederherstellungszwecke bereitgestellt wurde, aktive Freigabeabonnements haben, können sie einen Momentaufnahmezeitplan im Rahmen eines Failovers aktivieren. Wenn die Datenconsumer keine sekundäre Region für Notfallwiederherstellungszwecke abonnieren möchten, können sie zu einem späteren Zeitpunkt in die sekundäre Freigabe eingeladen werden. 

Datenconsumer können entweder ein aktives Freigabeabonnement haben, das für Notfallwiederherstellungszwecke inaktiv ist, oder Datenanbieter können sie im Rahmen manueller Failoverprozeduren zu einem späteren Zeitpunkt einladen. 

## <a name="related-information"></a>Verwandte Informationen

- [Business Continuity & Disaster Recovery](../best-practices-availability-paired-regions.md)
- [Hochverfügbarkeit – ein Teil Ihrer BCDR-Strategie](/azure/architecture/solution-ideas/articles/build-high-availability-into-your-bcdr-strategy)

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md).