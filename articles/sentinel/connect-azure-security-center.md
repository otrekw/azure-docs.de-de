---
title: Azure Security Center-Daten mit Azure Sentinel
description: Erfahren Sie, wie Sie Azure Security Center-Daten mit Azure Sentinel verknüpfen.
author: rkarlin
manager: rkarlin
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 4c75d1870ca3b966acdd658c91c0af8fd7465bfb
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190882"
---
# <a name="connect-data-from-azure-security-center"></a>Verknüpfen von Daten aus Azure Security Center





Mit Azure Sentinel können Sie Warnungen aus [Azure Security Center](../security-center/security-center-intro.md) verknüpfen und in Azure Sentinel streamen. 

## <a name="prerequisites"></a>Voraussetzungen

- Um Warnungen aus dem Azure Security Center zu exportieren, muss in dem Abonnement, dessen Protokolle Sie streamen, die Rolle „Sicherheitsleseberechtigter“ vorhanden sein.

- Sie müssen das Abonnement im [Azure Security Center-Standard-Tarif](../security-center/security-center-pricing.md) ausführen. Wenn dies nicht der Fall ist, [aktualisieren Sie Ihr Abonnement auf den Standard-Tarif](https://azure.microsoft.com/pricing/details/security-center/).



## <a name="connect-to-azure-security-center"></a>Herstellen einer Verbindung mit Azure Security Center

1. Klicken Sie in Azure Sentinel auf **Data connectors** (Datenconnectors) und anschließend auf die Kachel **Azure Security Center**.

1. Klicken Sie in der rechten Spalte neben jedem Abonnement, dessen Warnungen Sie in Azure Sentinel streamen möchten, auf **Verbinden**. Stellen Sie sicher, dass Sie jedes Abonnement auf den Azure Security Center-Standard-Tarif aktualisieren, um Warnungen an Azure Sentinel zu streamen.

1. Sie können auswählen, ob die Warnungen von Azure Security Center automatisch Incidents in Azure Sentinel generieren sollen. Wählen Sie unter **Incidents erstellen** die Option **Aktivieren** aus, um die standardmäßige Analyseregel zu aktivieren, die automatisch Incidents aus im verbundenen Sicherheitsdienst generierten Warnungen erstellt. Anschließend können Sie diese Regel unter **Analytics** und dann unter **Aktive Regeln** bearbeiten.

3. Klicken Sie auf **Verbinden**.

4. Um das relevante Schema für die Azure Security Center-Warnungen in Log Analytics zu verwenden, suchen Sie nach **SecurityAlert**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde beschrieben, wie Sie Azure Security Center mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
