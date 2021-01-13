---
title: Verwalten von Sicherheitsvorfällen in Azure Security Center | Microsoft-Dokumentation
description: Dieses Dokument enthält hilfreiche Informationen zur Verwendung des Azure Security Centers zum Verwalten von Sicherheitsvorfällen.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 06/15/2020
ms.author: memildin
ms.openlocfilehash: 2b5dc30de19704b5e8950515cfa6224b4bbdbaf0
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92341345"
---
# <a name="manage-security-incidents-in-azure-security-center"></a>Verwalten von Sicherheitsvorfällen in Azure Security Center

Das Selektieren und Untersuchen von Sicherheitswarnungen kann auch für sehr erfahrene Sicherheitsanalysten zeitaufwändig sein. Häufig ist nur schwer zu erkennen, wo überhaupt begonnen werden soll. 

Security Center verwendet [Analysen](./security-center-alerts-overview.md), um die Informationen zwischen verschiedenen [Sicherheitswarnungen](security-center-managing-and-responding-alerts.md) zu verbinden. Mithilfe dieser Verbindungen kann Security Center eine Einzelansicht einer Angriffskampagne und der zugehörigen Warnungen bereitstellen, damit Sie die Aktionen des Angreifers und die betroffenen Ressourcen besser verstehen können.

Diese Seite enthält eine Übersicht über die Vorfälle in Security Center.

## <a name="what-is-a-security-incident"></a>Was ist ein Sicherheitsvorfall?

In Security Center ist ein Sicherheitsvorfall eine Aggregation aller Warnungen für eine Ressource, die [Kill Chain](alerts-reference.md#intentions)-Mustern entsprechen. Vorfälle werden auf der Seite [Sicherheitswarnungen](security-center-managing-and-responding-alerts.md) angezeigt. Wählen Sie einen Vorfall aus, um die zugehörigen Warnungen anzuzeigen und weitere Informationen zu erhalten.

## <a name="managing-security-incidents"></a>Verwalten von Sicherheitsvorfällen

1. Wählen Sie auf der Security Center-Übersichtsseite die Kachel **Sicherheitswarnungen** aus. Der Vorfall und die Warnungen werden aufgeführt. Beachten Sie, dass Sicherheitsvorfälle ein anderes Symbol als Sicherheitswarnungen aufweisen.

    ![Sicherheitsvorfälle anzeigen](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Um Details anzuzeigen, wählen Sie einen Vorfall aus. Weitere Details finden Sie auf der Seite **Sicherheitsvorfall**. 

    [![Reagieren auf Sicherheitsvorfälle in Azure Security Center](media/security-center-incident/incident-details.png)](media/security-center-incident/incident-details.png#lightbox)

    Im linken Bereich der Seite mit dem Sicherheitsvorfall werden allgemeine Informationen über den Sicherheitsvorfall angezeigt: Titel, Schweregrad, Status, Zeitpunkt der Aktivität, Beschreibung und die betroffene Ressource. Neben der betroffenen Ressource sehen Sie die relevanten Azure-Tags. Verwenden Sie diese Tags, um den Organisationskontext der Ressource abzuleiten, wenn Sie die Warnung untersuchen.

    Der rechte Bereich enthält die Registerkarte **Warnungen** mit den Sicherheitswarnungen, die mit diesem Vorfall korreliert wurden. 

    >[!TIP]
    > Um weitere Informationen zu einer bestimmten Warnung zu erhalten, wählen Sie sie aus. 

    [![Registerkarte „Maßnahmen ergreifen“ des Vorfalls](media/security-center-incident/incident-take-action-tab.png)](media/security-center-incident/incident-take-action-tab.png#lightbox)

    Um zur Registerkarte **Maßnahmen ergreifen** zu wechseln, wählen Sie die Registerkarte oder die Schaltfläche am unteren Rand des rechten Bereichs aus. Verwenden Sie diese Registerkarte, um weitere Aktionen durchzuführen, wie z. B:
    - *Auswirkungen der Bedrohung minimieren*: Stellt manuelle Schritte zur Behebung dieses Sicherheitsvorfalls bereit.
    - *Künftige Angriffe verhindern*: Gibt Sicherheitsempfehlungen, um die Angriffsfläche zu verringern, den Sicherheitsstatus zu erhöhen und künftige Angriffe zu verhindern.
    - *Automatische Reaktion auslösen*: Bietet die Möglichkeit, eine Logik-App als Reaktion auf diesen Sicherheitsvorfall auszulösen.
    - *Ähnliche Warnungen unterdrücken*: Bietet die Möglichkeit, zukünftige Warnungen mit ähnlichen Merkmalen zu unterdrücken, wenn die Warnung für Ihre Organisation nicht relevant ist. 

   > [!NOTE]
   > Dieselbe Warnung kann als Teil eines Vorfalls vorhanden sein und als eigenständige Warnung angezeigt werden.

1. Führen Sie die für jede Warnung bereitgestellten Problembehandlungsschritte aus, um die Bedrohungen durch den Vorfall zu beseitigen.


## <a name="next-steps"></a>Nächste Schritte

Auf dieser Seite wurden die Sicherheitsvorfallfunktionen von Security Center erläutert. Zugehörige Informationen finden Sie auf den folgenden Seiten:

- [Sicherheitswarnungen in Security Center](security-center-alerts-overview.md)
- [Verwalten von und Reagieren auf Sicherheitswarnungen](security-center-managing-and-responding-alerts.md)