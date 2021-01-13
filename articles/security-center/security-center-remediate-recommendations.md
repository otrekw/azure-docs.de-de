---
title: Umsetzen von Empfehlungen in Azure Security Center | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie auf Empfehlungen in Azure Security Center reagieren, um Ihre Ressourcen zu schützen und Sicherheitsrichtlinien einzuhalten.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 8be947cc-cc86-421d-87a6-b1e23077fd50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/08/2020
ms.author: memildin
ms.openlocfilehash: dabd7e9e2c3c74225efc4611c7ad3523a6c76ba5
ms.sourcegitcommit: 02ed9acd4390b86c8432cad29075e2204f6b1bc3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/29/2020
ms.locfileid: "97807989"
---
# <a name="remediate-recommendations-in-azure-security-center"></a>Umsetzen von Empfehlungen in Azure Security Center

Empfehlungen enthalten Vorschläge dazu, wie Sie Ihre Ressourcen besser schützen können. Sie implementieren eine Empfehlung, indem Sie die in der Empfehlung beschriebenen Schritte zur Bereinigung ausführen.

## <a name="remediation-steps"></a>Schritte zur Bereinigung<a name="remediation-steps"></a>

Nach der Überprüfung aller Empfehlungen müssen Sie entscheiden, welche zuerst umgesetzt werden soll. Sie sollten die Sicherheitskontrollen mit dem höchsten Potenzial für die Erhöhung Ihrer Sicherheitsbewertung priorisieren.

1. Wählen Sie in der Liste eine Empfehlung aus.

1. Befolgen Sie die Anweisungen im Abschnitt **Schritte zur Bereinigung**. Jede Empfehlung beinhaltet spezifische Anweisungen. Im folgenden Screenshot werden die Schritte angezeigt, die ausgeführt werden müssen, um Anwendungen so zu konfigurieren, dass der Datenverkehr nur über HTTPS zulässig ist.

    :::image type="content" source="./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png" alt-text="Manuelle Schritte zur Problembehebung für eine Empfehlung" lightbox="./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png":::

1. Nach Abschluss des Vorgangs wird eine Benachrichtigung angezeigt, in der Sie darüber informiert werden, ob das Problem behoben wurde.

## <a name="quick-fix-remediation"></a>Schnelle Problembehebung

Um die Problembehebung zu vereinfachen und die Sicherheit Ihrer Umgebung zu verbessern (und Ihre Sicherheitsbewertung zu erhöhen), beinhalten viele Empfehlungen eine Option zur schnellen Problembehebung.

Mithilfe der schnellen Problembehebung können Sie eine Empfehlung schnell für mehrere Ressourcen umsetzen.

> [!TIP]
> Lösungen zur schnellen Problembehebung sind nur für bestimmte Empfehlungen verfügbar. Um die Empfehlungen zu finden, für die eine schnelle Problembehebung verfügbar ist, verwenden Sie den Filter **Antwortaktionen** für die Liste der Empfehlungen:
> 
> :::image type="content" source="media/security-center-remediate-recommendations/quick-fix-filter.png" alt-text="Verwenden Sie die Filter über der Empfehlungsliste, um Empfehlungen mit der Option zur schnellen Problembehebung zu finden":::

So implementieren Sie eine Lösung zur schnellen Problembehebung

1. Klicken Sie in der Liste der Empfehlungen, die mit **Schnelle Problembehebung** bezeichnet sind, auf eine Empfehlung.

    [![Wählen Sie „Schnelle Problembehebung“ aus.](media/security-center-remediate-recommendations/security-center-quick-fix-select.png)](media/security-center-remediate-recommendations/security-center-quick-fix-select.png#lightbox)

1. Wählen Sie auf der Registerkarte **Fehlerhafte Ressourcen** die Ressourcen aus, für die Sie die Empfehlung implementieren möchten, und wählen Sie dann **Wartung ausführen** aus.

    > [!NOTE]
    > Einige der aufgelisteten Ressourcen sind möglicherweise deaktiviert, da Sie nicht über die entsprechenden Berechtigungen zum Ändern verfügen.

1. Lesen Sie sich im Bestätigungsdialogfeld die Details und Auswirkungen der Korrektur durch.

    ![Schnelle Problembehebung](./media/security-center-remediate-recommendations/security-center-quick-fix-view.png)

    > [!NOTE]
    > Die Auswirkungen sind in dem grauen Feld im Fenster **Ressourcen warten** aufgelistet, das nach dem Klicken auf **Wartung ausführen** geöffnet wird. Hier ist angegeben, welche Änderungen auftreten, wenn Sie mit der schnellen Problembehebung fortfahren.

1. Fügen Sie ggf. die relevanten Parameter ein, und genehmigen Sie die Korrektur.

    > [!NOTE]
    > Es kann einige Minuten dauern, bis nach der Korrektur die Ressourcen auf der Registerkarte **Fehlerfreie Ressourcen** angezeigt werden. Zum Anzeigen der Korrekturaktionen sehen Sie sich das [Aktivitätsprotokoll](#activity-log) an.

1. Nach Abschluss des Vorgangs wird eine Benachrichtigung angezeigt, in der Sie darüber informiert werden, ob die Umstellung erfolgreich durchgeführt wurde.

## <a name="quick-fix-remediation-logging-in-the-activity-log"></a>Protokollierung der schnellen Problembehebung im Aktivitätsprotokoll <a name="activity-log"></a>

Beim Korrekturvorgang wird ein Vorlagenbereitstellungs- oder REST PATCH-API-Aufruf verwendet, um die Konfiguration auf die Ressource anzuwenden. Diese Vorgänge werden im [Azure-Aktivitätsprotokoll](../azure-resource-manager/management/view-activity-logs.md) protokolliert.


## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde beschrieben, wie Sie Empfehlungen in Security Center umsetzen. Weitere Informationen zu Security Center finden Sie auf den folgenden Seiten:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
