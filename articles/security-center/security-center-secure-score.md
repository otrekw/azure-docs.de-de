---
title: Sicherheitsbewertung in Azure Security Center | Microsoft-Dokumentation
description: " Priorisieren Sie Sicherheitsempfehlungen mithilfe von Secure Score in Azure Security Center. "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/15/2019
ms.author: memildin
ms.openlocfilehash: 15a94a9724fac29d36f6bb88ee4810b3bc7ca607
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77603481"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>Verbessern des Secure Score in Azure Security Center

> [!NOTE]
> Es ist ein erweiterter Secure Score als Vorschauversion verfügbar. Der erweiterte Secure Score wird den vorhandenen Secure Score ersetzen, beide stehen aber für eine gewisse Zeit gemeinsam zur Verfügung, um den Übergang zu vereinfachen.
>
> Ausführliche Informationen zu den Vorteilen des erweiterten Secure Score finden Sie [hier](secure-score-security-controls.md).
>
> Um an der Vorschau teilzunehmen, öffnen Sie das Azure-Portal, starten Sie Azure Security Center, und wählen Sie Secure Score aus. Dort sehen Sie ein Banner am oberen Rand der Seite, in dem der neue Secure Score angeboten wird. Alternativ können Sie [hier](https://aka.ms/ascnewscore) klicken.

Bei so vielen Diensten, die Sicherheitsvorteile bieten, ist es oft schwierig zu wissen, welche Schritte zum Sichern und Schützen Ihrer Workload zuerst ausgeführt werden sollten. Sicherheitsbewertung überprüft Ihre Sicherheitsempfehlungen und priorisiert sie für Sie, damit Sie wissen, welche Empfehlungen Sie zuerst durchführen sollten. Dies hilft Ihnen, die schwerwiegendsten Sicherheitsrisiken zu finden, damit Sie die Untersuchung priorisieren können. Sicherheitsbewertung ist ein Tool, mit dem Sie den Sicherheitsstatus bewerten können.

## <a name="secure-score-calculation"></a>Berechnung der Sicherheitsbewertung

Security Center ahmt die Arbeit des Sicherheitsanalysten nach, überprüft die Sicherheitsempfehlungen und wendet erweiterte Algorithmen an, um zu bestimmen, wie wichtig die jeweiligen Empfehlungen sind.
Azure Security Center überprüft ständig die aktiven Empfehlungen und berechnet auf dieser Grundlage Ihre Sicherheitsbewertung. Die jeweilige Bewertung einer Empfehlung ergibt sich aus deren Schweregrad und bewährten Methoden für die Sicherheit, die sich am stärksten auf die Sicherheit Ihrer Workload auswirken.

Security Center bietet Ihnen auch einen **Gesamtwert der Sicherheitsbewertung**. 

Beim **Gesamtwert der Sicherheitsbewertung** handelt es sich um eine Ansammlung aller Ihrer Empfehlungen. Sie können den Gesamtwert der Sicherheitsbewertung je nach Auswahl für alle Abonnements oder Verwaltungsgruppen anzeigen. Die Bewertung variiert je nach ausgewähltem Abonnement und aktiven Empfehlungen für diese Abonnements.

Um zu überprüfen, welche Empfehlungen die größte Auswirkung auf die Sicherheitsbewertung haben, können Sie die drei Empfehlungen mit den größten Auswirkungen im Security Center-Dashboard anzeigen, oder Sie können die Empfehlungen auf dem Listenblatt mithilfe der Spalte **Auswirkung auf die Sicherheitsbewertung** sortieren.

So zeigen Sie den Gesamtwert der Sicherheitsbewertung an:

1. Klicken Sie auf dem Azure-Dashboard auf **Security Center** und dann auf **Sicherheitsbewertung**.

2. Oben sehen Sie Hervorhebungen der Sicherheitsbewertung:
   - **Sicherheitsbewertung gesamt** stellt die Bewertung nach Richtlinien und ausgewähltem Abonnement dar.
   - **Sicherheitsbewertung nach Kategorie** zeigt Ihnen, welche Ressourcen die meiste Aufmerksamkeit benötigen.
   - Über **Top-Empfehlungen gemäß Auswirkung auf die Sicherheitsbewertung** wird eine Liste mit Empfehlungen bereitgestellt, deren Umsetzung Ihre Sicherheitsbewertung am stärksten verbessert.
 
   ![Sicherheitsbewertung](./media/security-center-secure-score/secure-score-dashboard.png)

3. In der folgenden Tabelle sehen Sie alle Ihre Abonnements und die jeweilige gesamte Sicherheitsbewertung.

   > [!NOTE]
   > Die Summe der Sicherheitsbewertungen der einzelnen Abonnements entspricht nicht dem Gesamtwert der Sicherheitsbewertung. Die Sicherheitsbewertung wird basierend auf dem Verhältnis zwischen Ihren fehlerfreien Ressourcen und der Gesamtzahl Ihrer Ressourcen gemäß Empfehlung berechnet und ist nicht die Summe der Sicherheitsbewertungen Ihrer Abonnements. 
   >
4. Klicken Sie auf **Empfehlungen anzeigen**, um die Empfehlungen für dieses Abonnement anzuzeigen, die Sie durchführen können, um Ihre Sicherheitsbewertung zu verbessern.
4. In der Liste der Empfehlungen können Sie sehen, dass für jede Empfehlung eine Spalte mit der **Auswirkung auf die Sicherheitsbewertung** enthalten ist. Dieser Wert gibt an, um wie viel sich der Gesamtwert der Sicherheitsbewertung verbessert, wenn Sie den Empfehlungen folgen. Wenn Sie im folgenden Bildschirm beispielsweise **Sicherheitsrisiken in Containersicherheitskonfigurationen beseitigen**, erhöht sich Ihre Sicherheitsbewertung um 35 Punkte.

   ![Sicherheitsbewertung](./media/security-center-secure-score/security-center-secure-score1.png)



## <a name="individual-secure-score"></a>Individuelle Sicherheitsbewertung

Sie können auch einzelne Sicherheitsbewertungen anzeigen. Diese sind auf dem Blatt mit einzelnen Empfehlungen enthalten.  

**Sicherheitsbewertung für einzelne Empfehlung** ist eine Berechnung, die auf dem Verhältnis zwischen fehlerfreien Ressourcen und der Gesamtzahl Ihrer Ressourcen basiert. Wenn die Anzahl der fehlerfreien Ressourcen der Gesamtzahl der Ressourcen entspricht, erhalten Sie die maximale Sicherheitsbewertung von 50 für die Empfehlung. Um Ihre Sicherheitsbewertung dem maximalen Wert anzunähern, beheben Sie die fehlerhaften Ressourcen, indem Sie den Empfehlungen folgen.

Anhand der **Auswirkungen der Empfehlung** können Sie sehen, um wie viel sich die Sicherheitsbewertung verbessert, wenn Sie die empfohlenen Schritte ausführen. Wenn Ihre Sicherheitsbewertung beispielsweise 42 beträgt und für **Auswirkungen der Empfehlung** der Wert +3 angegeben ist, verbessert sich die Sicherheitsbewertung bei Ausführung der in der Empfehlung angegebenen Schritte auf 45.

Die Empfehlung zeigt, welche Bedrohungen für Ihre Workload bestehen, falls die Schritte zur Behebung nicht ausgeführt werden.

![Sicherheitsbewertung für einzelne Empfehlung](./media/security-center-secure-score/indiv-recommendation-secure-score.png)




## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde gezeigt, wie Sie den Sicherheitsstatus mithilfe von **Sicherheitsbewertung** in Azure Security Center verbessern. Weitere Informationen zu Security Center finden Sie unter folgendem Link:

* [Azure Security Center – häufig gestellte Fragen](faq-general.md): Hier finden Sie Antworten auf häufig gestellte Fragen zum Dienst und zur Sicherheitsbewertung.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Erweiterte Sicherheitsbewertung](secure-score-security-controls.md): Erfahren Sie mehr über die Vorteile der erweiterten Sicherheitsbewertung (derzeit in der Vorschauphase).