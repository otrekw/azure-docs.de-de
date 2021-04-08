---
title: Exemplarische Vorgehensweise für Peering Service-Partner
titleSuffix: Azure
description: Exemplarische Vorgehensweise für Peering Service-Partner
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 12/15/2020
ms.author: prmitiki
ms.openlocfilehash: 292c850f5352c80112e3f0bcc5c868f8128819be
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97592312"
---
# <a name="peering-service-partner-walkthrough"></a>Exemplarische Vorgehensweise für Peering Service-Partner

In diesem Abschnitt werden die Schritte erläutert, die ein Anbieter befolgen muss, um ein Direct Peering für den Peering Service zu aktivieren.

## <a name="create-direct-peering-connection-for-peering-service"></a>Erstellen einer Direct Peering-Verbindung für Peering Service
Dienstanbieter können ihre geografische Reichweite erweitern, indem sie ein neues Direct Peering erstellen, das Peering Service unterstützt. Dies erreichen Sie folgendermaßen:
1. Werden Sie Peering Service-Partner, wenn Sie dies noch nicht sind
1. Befolgen Sie die Anweisungen zum [Erstellen oder Ändern einer Instanz für Direct Peering über das Portal](howto-direct-portal.md). Achten Sie darauf, dass sie die Hochverfügbarkeitsanforderung erfüllt.
1. Befolgen Sie anschließend die Schritte zum [Aktivieren von Peering Service für Direct Peering über das Portal](howto-peering-service-portal.md).

## <a name="use-legacy-direct-peering-connection-for-peering-service"></a>Verwenden einer Legacy-Direct Peering-Verbindung für Peering Service
Wenn Sie über ein Legacy-Direct Peering verfügen, das Sie zur Unterstützung von Peering Service verwenden möchten, gehen Sie wie folgt vor:
1. Werden Sie Peering Service-Partner, wenn Sie dies noch nicht sind.
1. Befolgen Sie die Anweisungen in [Konvertieren einer älteren Instanz für Direct Peering in eine Azure-Ressource über das Portal](howto-legacy-direct-portal.md). Falls erforderlich, bestellen Sie zusätzliche Leitungen, um den Anforderungen an Hochverfügbarkeit zu genügen.
1. Befolgen Sie anschließend die Schritte zum [Aktivieren von Peering Service für Direct Peering über das Portal](howto-peering-service-portal.md).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Peeringrichtlinie](https://peering.azurewebsites.net/peering).
* Informationen zum Einrichten von Direct Peering mit Microsoft finden Sie in der [exemplarischen Vorgehensweise für Direct Peering](walkthrough-direct-all.md).
* Informationen zum Einrichten von Exchange-Peering mit Microsoft finden Sie in der [exemplarischen Vorgehensweise für Exchange Peering](walkthrough-exchange-all.md).