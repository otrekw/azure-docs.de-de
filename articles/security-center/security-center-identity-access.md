---
title: Überwachen von Identität und Zugriff in Azure Security Center | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe der Identitäts- und Zugriffsfunktion in Azure Security Center die Zugriffsaktivitäten der Benutzer sowie identitätsbezogene Probleme überwachen können.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2019
ms.author: memildin
ms.openlocfilehash: 6b262baddd10c9d0dff4b196b733972b97d99872
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552983"
---
# <a name="monitor-identity-and-access-preview"></a>Überwachen der Identität und des Zugriffs (Vorschau)
Werden potenzielle Sicherheitslücken erkannt, erstellt Security Center Empfehlungen, die Sie beim Konfigurieren der erforderlichen Steuerelemente zum Sichern und Schützen Ihrer Ressourcen unterstützen.

In diesem Artikel wird die Seite **Identität und Zugriff** des Abschnitts „Ressourcensicherheit“ im Azure Security Center erläutert.

Eine vollständige Liste der Empfehlungen, die auf dieser Seite angezeigt werden können, finden Sie unter [Empfehlungen zu Identität und Zugriff](recommendations-reference.md#recs-identity).

> [!NOTE]
> Die Überwachung der Identität und des Zugriffs ist als Vorschauversion und im Standard-Tarif vom Security Center verfügbar. Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md).
>

Die Identität sollte die Kontrollebene für Ihr Unternehmen darstellen, und der Schutz von Identitäten sollte höchste Priorität haben. Der Fokus bei der Entwicklung des Sicherheitsbereichs wurde von der Netzwerkorientierung auf die Identitätsorientierung verlagert. Bei der Sicherheit geht es immer weniger um die Verteidigung Ihres Netzwerks und immer mehr um die Verteidigung Ihrer Daten sowie um die Verwaltung der Sicherheit Ihrer Apps und Benutzer. Da heutzutage jedoch immer mehr Daten und Apps in die Cloud verlagert werden, ist die Identität zur neuen Grenze geworden.

Durch die Überwachung von Identitätsaktivitäten können Sie proaktive Maßnahmen ergreifen, bevor es zu einem Vorfall kommt, oder einen Angriffsversuch abwehren. Das Dashboard für Identität und Zugriff bietet Empfehlungen wie etwa zu Folgendem:

- MFA für privilegierte Konten in Ihrem Abonnement aktivieren
- Externe Konten mit Schreibberechtigungen aus Ihrem Abonnement entfernen
- Privilegierte externe Konten aus Ihrem Abonnement entfernen

> [!NOTE]
> Wenn Ihr Abonnement mehr als 600 Konten umfasst, kann das Security Center nicht die Identitätsempfehlungen für Ihr Abonnement ausführen. Empfehlungen, die nicht ausgeführt werden, werden unter „Nicht verfügbare Bewertungen“ aufgeführt.
Im Security Center können keine Identitätsempfehlungen für Administrator-Agents eines Cloud Solution Provider-Partners (CSP) ausgeführt werden.
>

## <a name="monitor-identity-and-access"></a>Überwachen der Identität und des Zugriffs

Öffnen Sie die Liste der identifizierten Identitäts- und Zugriffsprobleme, indem Sie **Identität und Zugriff** in der Security Center-Seitenleiste (unter **Ressourcen**) oder auf der Übersichtsseite auswählen. 

Unter **Identität und Zugriff** befinden sich zwei Registerkarten:

- **Übersicht**: Vom Security Center identifizierte Empfehlungen.
- **Abonnements**: Liste Ihrer Abonnements und der jeweils aktuelle Sicherheitsstatus.

[![Identität und Zugriff](./media/security-center-identity-access/identity-dashboard.png)](./media/security-center-identity-access/identity-dashboard.png#lightbox)

### <a name="overview-section"></a>Abschnitt „Übersicht“
Unter **Übersicht** finden Sie eine Liste von Empfehlungen. Die erste Spalte enthält die Empfehlung. Die zweite Spalte enthält die Gesamtanzahl der Abonnements, die diese Empfehlung betrifft. Die dritte Spalte gibt den Schweregrad des Problems an.

1. Wählen Sie eine Empfehlung aus. Das Fenster für Empfehlungen wird geöffnet und zeigt Folgendes an:

   - Beschreibung der Empfehlung
   - Liste der fehlerhaften und fehlerfreien Abonnements
   - Liste der Ressourcen, die aufgrund einer fehlerhaften Bewertung nicht überprüft wurden oder aufgrund der Ausführung in einem Abonnement mit dem Free-Tarif nicht bewertet wurden

    [![Fenster „Empfehlungen“](./media/security-center-identity-access/select-subscription.png)](./media/security-center-identity-access/select-subscription.png#lightbox)

1. Um weitere Einzelheiten zu erhalten, wählen Sie ein Abonnement in der Liste aus.

### <a name="subscriptions-section"></a>Abonnementabschnitt
Unter **Abonnements** finden Sie eine Liste von Abonnements. Die erste Spalte enthält die Abonnements. Die zweite Spalte zeigt die Gesamtanzahl der Empfehlungen für die einzelnen Abonnements. Die dritte Spalte gibt die Schweregrade der Probleme an.

[![Registerkarte „Abonnements“](./media/security-center-identity-access/subscriptions.png)](./media/security-center-identity-access/subscriptions.png#lightbox)

1. Wählen Sie ein Abonnement aus. Eine zusammenfassende Darstellung mit drei Registerkarten wird geöffnet:

   - **Empfehlungen**: Basiert auf vom Security Center ausgeführten Bewertungen, bei denen ein Fehler aufgetreten ist.
   - **Bestandene Bewertungen**: Liste der vom Security Center ausgeführten Bewertungen, die bestanden wurden.
   - **Nicht verfügbare Bewertungen**: Liste von Bewertungen, bei denen ein Fehler aufgetreten ist oder ein Abonnement mit mehr als 600 Konten vorkommt.

   Unter **Empfehlungen** finden Sie eine Liste der Empfehlungen für das ausgewählte Abonnement und den Schweregrad der einzelnen Empfehlungen.

   [![Empfehlungen für das ausgewählte Abonnement](./media/security-center-identity-access/recommendations.png)](./media/security-center-identity-access/recommendations.png#lightbox)

1. Wählen Sie eine Empfehlung aus, um eine Beschreibung der Empfehlung, eine Liste von fehlerhaften und fehlerfreien Abonnements und eine Liste von nicht überprüften Ressourcen zu erhalten.

   [![Beschreibung der Empfehlung](./media/security-center-identity-access/designate.png)](./media/security-center-identity-access/designate.png#lightbox)

   Unter **Bestandene Bewertungen** finden Sie eine Liste der bestandenen Bewertungen.  Der Schweregrad dieser Bewertungen wird immer in Grün angezeigt.

   [![Bestandene Bewertungen](./media/security-center-identity-access/passed-assessments.png)](./media/security-center-identity-access/passed-assessments.png#lightbox)

1. Wählen Sie eine bestandene Bewertung aus der Liste aus, um eine Beschreibung der Bewertung und eine Liste von fehlerfreien Abonnements zu erhalten. Auf einer Registerkarte für fehlerhafte Abonnements werden alle Abonnements aufgeführt, bei denen ein Fehler aufgetreten ist.

   [![Bestandene Bewertungen](./media/security-center-identity-access/remove.png)](./media/security-center-identity-access/remove.png#lightbox)

> [!NOTE]
> Wenn Sie eine Richtlinie für bedingten Zugriff erstellt haben, die MFA erfordert, für die jedoch Ausschlüsse festgelegt sind, wird die Richtlinie beim Assessment der MFA-Empfehlung für Security Center als nicht kompatibel eingestuft, da sie zulässt, dass sich einige Benutzer in Azure ohne MFA anmelden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Artikeln:

- [Schützen von Computern und Anwendungen im Azure Security Center](security-center-virtual-machine-protection.md)
- [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md)
- [Schützen des Azure SQL-Diensts und der Daten im Azure Security Center](security-center-sql-service-recommendations.md)