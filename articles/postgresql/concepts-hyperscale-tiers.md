---
title: Basic-Tarif (Vorschauversion) – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Einzelknotentarif „Basic“ für Azure Database for PostgreSQL – Hyperscale (Citus)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: c8cad5eb0983715a7cc7c18249243e93a2c498d7
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107023897"
---
# <a name="basic-tier-preview"></a>Basic-Tarif (Vorschau)

> [!IMPORTANT]
> Der Basic-Tarif von Hyperscale (Citus) ist zurzeit als Vorschauversion verfügbar.  Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
>
> Eine vollständige Liste der anderen neuen Features finden Sie unter [Vorschaufeatures für Hyperscale (Citus)](hyperscale-preview-features.md).

Der Basic-Tarif in Azure Database for PostgreSQL – Hyperscale (Citus) ist eine einfache Möglichkeit zum Erstellen einer kleinen Servergruppe, die Sie später skalieren können. Während Servergruppen im Standard-Tarif einen Koordinatorknoten und mindestens zwei Workerknoten haben,wird beim Basic-Tarif alles auf einem einzelnen Datenbankknoten ausgeführt.

Außer der Verwendung von weniger Knoten enthält der Basic-Tarif sämtliche Features des Standard-Tarifs. Wie der Standard-Tarif unterstützt er unter anderem Hochverfügbarkeit, Lesereplikate und spaltenbasierten Tabellenspeicher.

## <a name="choosing-basic-vs-standard-tier"></a>Auswählen des Basic-Tarifs im Vergleich zum Standard-Tarif

Der Basic-Tarif kann eine kostengünstige und praktische Bereitstellungsoption für anfängliche Entwicklung, Tests und Continuous Integration sein. Es wird ein einzelner Datenbankknoten verwendet und dieselbe SQL-API wie beim Standard-Tarif bereitgestellt. Sie können Anwendungen mit dem Basic-Tarif testen und später mit dem Vertrauen, dass die Schnittstelle unverändert bleibt, [zum Standard-Tarif wechseln](howto-hyperscale-scale-grow.md#add-worker-nodes).

Der Basic-Tarif ist auch für kleinere Workloads in der Produktion geeignet (sobald sie aus der Vorschau in allgemeine Verfügbarkeit übergeht). Es gibt Platz zum vertikalen Skalieren *im* Basic-Tarif, indem die Anzahl der virtuellen Serverkerne erhöht wird.

Wenn eine größere Skalierung sofort erforderlich ist, verwenden Sie den Standard-Tarif. Die kleinste zulässige Servergruppe hat einen Koordinatorknoten und zwei Workers. Sie können je nach Ihrem Anwendungsfall auswählen, dass Sie weitere Knoten verwenden möchten, wie in unserer Vorgehensweise für [anfängliche Dimensionierung](howto-hyperscale-scale-initial.md) beschrieben wird.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich, wie Sie [den Basic-Tarif bereitstellen](quickstart-create-hyperscale-basic-tier.md) können.
* Sehen Sie sich abschließend an, wie Sie vom Basic-Tarif zum Standard-Tarif [wechseln](howto-hyperscale-scale-grow.md#add-worker-nodes) können.
* Die Option [spaltenbasierter Speicher](concepts-hyperscale-columnar.md) steht sowohl im Basic- als auch im Standard-Tarif zur Verfügung.
