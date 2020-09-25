---
title: Sicherheitsempfehlungen in Azure Security Center
description: In diesem Dokument erfahren Sie, wie Sicherheitsempfehlungen in Azure Security Center Ihnen helfen, Ihre Azure-Ressourcen zu schützen und Ihre Sicherheitsrichtlinien einzuhalten.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/29/2019
ms.author: memildin
ms.openlocfilehash: 11043d6686bd762b1c0a9827c7edb2230487cc72
ms.sourcegitcommit: 1b320bc7863707a07e98644fbaed9faa0108da97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2020
ms.locfileid: "89595443"
---
# <a name="security-recommendations-in-azure-security-center"></a>Sicherheitsempfehlungen in Azure Security Center 
In diesem Thema wird erläutert, wie Sie die Empfehlungen in Azure Security Center anzeigen und nutzen können, mit denen Sie Ihre Azure-Ressourcen schützen.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Dieses Dokument ist keine Schritt-für-Schritt-Anleitung.
>

## <a name="what-are-security-recommendations"></a>Was sind Sicherheitsempfehlungen?

Empfehlungen sind Maßnahmen, die Sie ergreifen sollten, um Ihre Ressourcen zu schützen.

Azure Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen, um mögliche Sicherheitsrisiken zu erkennen. Anschließend erhalten Sie Empfehlungen dazu, wie Sie diese Sicherheitsrisiken beheben können.

Jede Empfehlung beinhaltet Folgendes:

- Eine kurze Beschreibung des Problems
- Die Schritte zur Bereinigung, die zum Implementieren der Empfehlung ausgeführt werden müssen
- Die betroffenen Ressourcen

## <a name="monitor-recommendations"></a>Überwachen von Empfehlungen<a name="monitor-recommendations"></a>

Security Center analysiert den Sicherheitsstatus Ihrer Ressourcen, um mögliche Sicherheitsrisiken zu identifizieren. Auf der Kachel **Empfehlungen** unter **Übersicht** wird die Gesamtzahl der von Security Center identifizierten Empfehlungen angezeigt.

![Übersicht über Security Center](./media/security-center-recommendations/asc-overview.png)

1. Wählen Sie die Kachel **Empfehlungen** unter **Übersicht** aus. Die Liste **Empfehlungen** wird geöffnet.

1. Die Empfehlungen sind nach Sicherheitssteuerungen gruppiert.

      ![Nach Sicherheitssteuerung gruppierte Empfehlungen](./media/security-center-recommendations/view-recommendations.png)

1. Erweitern Sie eine Steuerung, und wählen Sie eine bestimmte Empfehlung aus, um die Empfehlungsseite anzuzeigen.

    :::image type="content" source="./media/security-center-recommendations/recommendation-details-page.png" alt-text="Seite mit den Empfehlungsdetails" lightbox="./media/security-center-recommendations/recommendation-details-page.png":::

    Inhalt der Seite:

    - Die Schaltflächen **Erzwingen** und **Ablehnen** für unterstützte Empfehlungen (siehe [Verhindern von Fehlkonfigurationen mit den Optionen zum Erzwingen/Ablehnen für Empfehlungen](prevent-misconfigurations.md))
    - **Angabe des Schweregrads**
    - **Aktualisierungsintervall** (sofern relevant) 
    - **Beschreibung**: Eine kurze Beschreibung des Problems
    - **Schritte zur Bereinigung**: Eine Beschreibung der manuellen Schritte, die erforderlich sind, um das Sicherheitsproblem für die betroffenen Ressourcen zu beheben. Für Empfehlungen unter „Schnellkorrektur“ können Sie **Korrekturlogik anzeigen** auswählen, bevor Sie die vorgeschlagene Lösung auf die Ressourcen anwenden. 
    - **Betroffene Ressourcen**: Die Ressourcen sind in Registerkarten unterteilt:
        - **Fehlerfreie Ressourcen**: Relevante Ressourcen, die entweder nicht beeinträchtigt sind oder bei denen das Problem bereits behoben wurde.
        - **Fehlerhafte Ressourcen**: Ressourcen, die weiterhin vom identifizierten Problem betroffen sind.
        - **Nicht anwendbare Ressourcen**: Ressourcen, für die die Empfehlung keine definitive Antwort geben kann. Auf dieser Registerkarte werden auch Gründe für die jeweilige Ressource angegeben. 

            :::image type="content" source="./media/security-center-recommendations/recommendations-not-applicable-reasons.png" alt-text="Nicht anwendbare Ressourcen mit Gründen":::

## <a name="preview-recommendations"></a>Vorschau der Empfehlungen

Empfehlungen, die als **Vorschau** gekennzeichnet sind, werden nicht in die Berechnungen Ihrer Sicherheitsbewertung einbezogen.

Sie sollten nach Möglichkeit weiterhin korrigiert werden, damit sie nach Ablauf des Vorschauzeitraums zu Ihrer Bewertung beitragen.

Beispiel für eine Vorschauempfehlung:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Empfehlung mit dem Flag „Vorschau“":::
 
## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurden Ihnen die Sicherheitsempfehlungen in Security Center vorgestellt. So erfahren Sie, wie Sie die Empfehlungen umsetzen

- [Umsetzen von Empfehlungen:](security-center-remediate-recommendations.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verhindern von Fehlkonfigurationen mit den Optionen zum Erzwingen/Ablehnen für Empfehlungen](prevent-misconfigurations.md).
