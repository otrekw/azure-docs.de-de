---
title: Azure Security Center-Daten mit Azure Sentinel
description: Erfahren Sie, wie Sie Warnungen aus Azure Security Center (ASC, Standard-Tarif) verknüpfen und in Azure Sentinel streamen.
author: yelevin
manager: rkarlin
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 2fc7744600a9652ad43fd0aae8d886dc94acd58f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559156"
---
# <a name="connect-data-from-azure-security-center-asc"></a>Verknüpfen von Daten aus Azure Security Center (ASC)

Mit Azure Sentinel können Sie Warnungen aus [Azure Security Center](../security-center/security-center-intro.md) verknüpfen und in Azure Sentinel streamen. 

## <a name="prerequisites"></a>Voraussetzungen

- Um Warnungen aus dem Azure Security Center zu exportieren, muss in dem Abonnement, dessen Protokolle Sie streamen, die Rolle „Sicherheitsleseberechtigter“ vorhanden sein.

- Sie müssen das Abonnement im [Azure Security Center-Standard-Tarif](../security-center/security-center-pricing.md) ausführen. Wenn dies nicht der Fall ist, [aktualisieren Sie Ihr Abonnement auf den Standard-Tarif](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="connect-to-azure-security-center"></a>Herstellen einer Verbindung mit Azure Security Center

1. Wählen Sie im Navigationsmenü von Azure Sentinel **Datenconnectors** aus.

1. Wählen Sie im Katalog für Datenconnectors **Azure Security Center** aus, und klicken Sie auf die Schaltfläche **Connectorseite öffnen**.

1. Klicken Sie unter **Konfiguration** neben jedem Abonnement, dessen Warnungen Sie in Azure Sentinel streamen möchten, auf **Verbinden**. Die Schaltfläche „Verbinden“ ist nur verfügbar, wenn Sie über die erforderlichen Berechtigungen und das ASC-Abonnement mit dem Standard-Tarif verfügen.

1. Sie können auswählen, ob durch die Warnungen von Azure Security Center automatisch Incidents in Azure Sentinel generiert werden sollen. Wählen Sie unter **Incidents erstellen** die Option **Aktiviert** aus, um die Standardanalyseregel zu aktivieren, durch die aus Warnungen automatisch Incidents erstellt werden. Anschließend können Sie diese Regel unter **Analytics** auf der Registerkarte **Aktive Regeln** bearbeiten.

1. Um das relevante Schema für die Azure Security Center-Warnungen in Log Analytics zu verwenden, suchen Sie nach **SecurityAlert**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde beschrieben, wie Sie Azure Security Center mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
