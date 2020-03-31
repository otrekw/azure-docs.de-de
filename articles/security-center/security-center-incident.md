---
title: Verwalten von Sicherheitsvorfällen in Azure Security Center | Microsoft-Dokumentation
description: Dieses Dokument enthält hilfreiche Informationen zur Verwendung des Azure Security Centers zum Verwalten von Sicherheitsvorfällen.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: memildin
ms.openlocfilehash: 98fc339e473ffb2bf54e7119634e93046cca1ef3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79415663"
---
# <a name="manage-security-incidents-in-azure-security-center"></a>Verwalten von Sicherheitsvorfällen in Azure Security Center

Das Selektieren und Untersuchen von Sicherheitswarnungen kann auch für sehr erfahrene Sicherheitsanalysten zeitaufwändig sein, und häufig ist nur schwer zu erkennen, wo überhaupt begonnen werden soll. Mithilfe von [Analysen](security-center-detection-capabilities.md) zum Verknüpfen der Informationen der einzelnen [Sicherheitswarnungen](security-center-managing-and-responding-alerts.md) kann Security Center Ihnen eine zentrale Übersicht über einen Angriffsversuch und alle zugehörigen Warnungen bieten. So können Sie schnell überblicken, welche Aktionen der Angreifer unternommen hat und welche Ressourcen betroffen sind.

In diesem Thema werden Vorfälle in Security Center erläutert und wie Sie deren Warnungen korrigieren können.

## <a name="what-is-a-security-incident"></a>Was ist ein Sicherheitsvorfall?

In Security Center ist ein Sicherheitsvorfall eine Aggregation aller Warnungen für eine Ressource, die [Kill Chain](alerts-reference.md#intentions)-Mustern entsprechen. Vorfälle werden in der Liste [Sicherheitswarnungen](security-center-managing-and-responding-alerts.md) angezeigt. Klicken Sie auf einen Vorfall, um die zugehörigen Warnungen anzuzeigen, wodurch Sie weitere Informationen zu den einzelnen Fällen erhalten können.

## <a name="managing-security-incidents"></a>Verwalten von Sicherheitsvorfällen

1. Im Security Center-Dashboard klicken Sie auf die Kachel **Sicherheitswarnungen**. Der Vorfall und die Warnungen werden aufgeführt. Beachten Sie, dass die Beschreibung des Sicherheitsvorfalls im Gegensatz zu anderen Warnungen ein anderes Symbol enthält.

    ![Sicherheitsvorfälle anzeigen](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Um Details anzuzeigen, klicken Sie auf einen Vorfall. Auf dem Blatt **Sicherheitsvorfall erkannt** werden weitere Details angezeigt. Der Abschnitt **Allgemeine Informationen** bietet Erkenntnisse dazu, was die Sicherheitswarnung ausgelöst hat. Er enthält Informationen wie die Zielressource, die Quell-IP-Adresse (sofern zutreffend), ob die Warnung noch aktiv ist sowie Empfehlungen zur Behebung.  

    ![Reagieren auf Sicherheitsvorfälle in Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-alert-incident.png)

1. Um weitere Informationen zu den einzelnen Warnungen anzuzeigen, klicken Sie auf die jeweilige. Die von Security Center vorgeschlagene Wiederherstellung variiert je nach Sicherheitshinweis.

   > [!NOTE]
   > Dieselbe Warnung kann als Teil eines Vorfalls vorhanden sein und als eigenständige Warnung angezeigt werden.

    ![Warnungsdetails](./media/security-center-incident/security-center-incident-alert.png)

1. Führen Sie die für jede Warnung angegebenen Korrekturschritte durch.


## <a name="see-also"></a>Weitere Informationen
In diesem Dokument haben Sie erfahren, wie Sie die Sicherheitsvorfallfunktion in Security Center verwenden. Zugehörige Informationen finden Sie in den folgenden Artikeln:

* [Bedrohungsschutz in Azure Security Center](threat-protection.md)
* [Sicherheitswarnungen in Azure Security Center](security-center-alerts-overview.md)
* [Verwalten von Sicherheitswarnungen](security-center-managing-and-responding-alerts.md)