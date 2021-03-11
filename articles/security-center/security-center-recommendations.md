---
title: Sicherheitsempfehlungen in Azure Security Center
description: In diesem Dokument erfahren Sie, wie Sicherheitsempfehlungen in Azure Security Center Ihnen helfen, Ihre Azure-Ressourcen zu schützen und Ihre Sicherheitsrichtlinien einzuhalten.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 01/24/2021
ms.author: memildin
ms.openlocfilehash: 57760443746e111750e74ef55fc18729f6ba32c4
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102100337"
---
# <a name="review-your-security-recommendations"></a>Überprüfen Ihrer Sicherheitsempfehlungen

In diesem Thema wird erläutert, wie Sie die Empfehlungen in Azure Security Center anzeigen und nutzen können, mit denen Sie Ihre Azure-Ressourcen schützen.

## <a name="monitor-recommendations"></a>Überwachen von Empfehlungen<a name="monitor-recommendations"></a>

Security Center analysiert den Sicherheitsstatus Ihrer Ressourcen, um mögliche Sicherheitsrisiken zu identifizieren. 

1. Öffnen Sie im Menü von Security Center die Seite **Empfehlungen**, um die für Ihre Umgebung geltenden Empfehlungen anzuzeigen. Die Empfehlungen sind nach Sicherheitssteuerungen gruppiert.

    :::image type="content" source="./media/security-center-recommendations/view-recommendations.png" alt-text="Nach Sicherheitssteuerung gruppierte Empfehlungen" lightbox="./media/security-center-recommendations/view-recommendations.png":::

1. Verwenden Sie die optionalen Filter oberhalb der Liste der Empfehlungen, um Empfehlungen zu Ressourcentyp, Schweregrad, Umgebung oder anderen Kriterien zu finden, die für Sie wichtig sind.

    :::image type="content" source="media/security-center-recommendations/recommendation-list-filters.png" alt-text="Filter zum Verfeinern der Liste der Azure Security Center-Empfehlungen":::

1. Erweitern Sie eine Steuerung, und wählen Sie eine bestimmte Empfehlung aus, um die Seite mit den Empfehlungsdetails anzuzeigen.

    :::image type="content" source="./media/security-center-recommendations/recommendation-details-page.png" alt-text="Seite mit den Empfehlungsdetails" lightbox="./media/security-center-recommendations/recommendation-details-page.png":::

    Inhalt der Seite:

    1. Für unterstützte Empfehlungen zeigt die obere Symbolleiste eine oder alle der folgenden Schaltflächen an:
        - **Erzwingen** und **Ablehnen** (Weitere Informationen finden Sie unter [Verhindern von Fehlkonfigurationen mit den Optionen zum Erzwingen/Ablehnen für Empfehlungen](prevent-misconfigurations.md).)
        - **Richtliniendefinitionen anzeigen**, um direkt zum Azure Policy-Eintrag der zugrunde liegenden Richtlinie zu gelangen
    1. **Angabe des Schweregrads**
    1. **Aktualisierungsintervall** (sofern relevant)
    1. **Anzahl ausgenommener Ressourcen**: Wenn für diese Empfehlung Ausnahmen vorhanden sind, wird die Anzahl der ausgenommenen Ressourcen angezeigt.
    1. **Beschreibung**: Eine kurze Beschreibung des Problems
    1. **Schritte zur Bereinigung**: Eine Beschreibung der manuellen Schritte, die erforderlich sind, um das Sicherheitsproblem für die betroffenen Ressourcen zu beheben. Für Empfehlungen unter „Schnellkorrektur“ können Sie **Korrekturlogik anzeigen** auswählen, bevor Sie die vorgeschlagene Lösung auf die Ressourcen anwenden. 
    1. **Betroffene Ressourcen**: Die Ressourcen sind in Registerkarten unterteilt:
        - **Fehlerfreie Ressourcen**: Relevante Ressourcen, die entweder nicht beeinträchtigt sind oder bei denen das Problem bereits behoben wurde.
        - **Fehlerhafte Ressourcen**: Ressourcen, die weiterhin vom identifizierten Problem betroffen sind.
        - **Nicht anwendbare Ressourcen**: Ressourcen, für die die Empfehlung keine definitive Antwort geben kann. Auf dieser Registerkarte werden auch Gründe für die jeweilige Ressource angegeben. 

            :::image type="content" source="./media/security-center-recommendations/recommendations-not-applicable-reasons.png" alt-text="Nicht anwendbare Ressourcen mit Gründen":::
    1. Aktionsschaltflächen zum Beheben über die Empfehlung oder Auslösen einer Logik-App

## <a name="preview-recommendations"></a>Vorschau der Empfehlungen

Empfehlungen, die als **Vorschau** gekennzeichnet sind, werden nicht in die Berechnungen Ihrer Sicherheitsbewertung einbezogen.

Sie sollten nach Möglichkeit weiterhin korrigiert werden, damit sie nach Ablauf des Vorschauzeitraums zu Ihrer Bewertung beitragen.

Beispiel für eine Vorschauempfehlung:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Empfehlung mit dem Flag „Vorschau“":::
 
## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurden Ihnen die Sicherheitsempfehlungen in Security Center vorgestellt. In den folgenden Artikeln finden Sie verwandte Informationen:

- [Umsetzen von Empfehlungen](security-center-remediate-recommendations.md) – In diesem Artikel erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verhindern von Fehlkonfigurationen mit den Optionen zum Erzwingen/Ablehnen für Empfehlungen](prevent-misconfigurations.md).
- [Automatisieren von Reaktionen auf Security Center-Trigger](workflow-automation.md) – In diesem Artikel erfahren Sie, wie Sie Reaktionen auf Empfehlungen automatisieren.
- [Ausschließen einer Ressource aus einer Empfehlung](exempt-resource.md)
- [Sicherheitsempfehlungen: Referenzhandbuch](recommendations-reference.md)