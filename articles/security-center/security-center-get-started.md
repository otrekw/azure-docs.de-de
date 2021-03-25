---
title: 'Upgraden auf Azure Defender: Azure Security Center'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Upgrade auf Azure Defender von Security Center durchführen, um die Sicherheit zu verbessern.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: quickstart
ms.date: 02/17/2021
ms.author: memildin
ms.openlocfilehash: 2f37a59d5db3883754b602b2b2525e07b57206b7
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102099351"
---
# <a name="quickstart-set-up-azure-security-center"></a>Schnellstart: Einrichten von Azure Security Center

Azure Security Center bietet einheitliche Funktionen für die Sicherheitsverwaltung und den Bedrohungsschutz für Ihre Hybrid Cloud- und Multi-Cloud-Workloads. Die kostenlosen Features bieten lediglich eingeschränkte Sicherheit für Ihre Azure-Ressourcen. Durch Aktivieren von Azure Defender können Sie diese Funktionen auf lokale Umgebungen und andere Clouds ausweiten. Azure Defender hilft Ihnen dabei, Sicherheitsrisiken zu finden und zu beseitigen, Zugriffs- und Anwendungssteuerungen anzuwenden, um böswillige Aktivitäten zu blockieren, Bedrohungen mithilfe von Analysen und intelligenten Funktionen zu erkennen und bei Angriffen schnell zu reagieren. Sie können Azure Defender kostenlos testen. Weitere Informationen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/security-center/).

In diesem Abschnitt der Schnellstartanleitung werden die empfohlenen Schritte zum Aktivieren von Azure Security Center und Azure Defender erläutert. Wenn Sie alle Schnellstartschritte ausgeführt haben, gilt Folgendes:

> [!div class="checklist"]
> * Security Center ist für Ihre Azure-Abonnements aktiviert.
> * Azure Defender ist für Ihre Azure-Abonnements aktiviert.
> * Die automatische Datensammlung ist eingerichtet.
> * E-Mail-Benachrichtigungen für Sicherheitswarnungen wurden eingerichtet.
> * Ihre Hybrid Cloud- und Multi-Cloud-Computer sind mit Azure verbunden.

## <a name="prerequisites"></a>Voraussetzungen
Für den Einstieg in Security Center benötigen Sie ein Microsoft Azure-Abonnement. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

Wenn Sie Azure Defender für ein Abonnement aktivieren möchten, muss Ihnen die Rolle „Abonnementbesitzer“, „Mitwirkender des Abonnements“ oder „Sicherheitsadministrator“ zugewiesen sein.

## <a name="enable-security-center-on-your-azure-subscription"></a>Aktivieren von Security Center für Ihr Azure-Abonnement

> [!TIP]
> Informationen zur Aktivierung von Security Center in allen Abonnements einer Verwaltungsgruppe finden Sie unter [Aktivieren von Security Center in allen Abonnements einer Verwaltungsgruppe](onboard-management-group.md).

1. Melden Sie sich beim [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) an.

1. Wählen Sie im Menü des Portals die Option **Security Center** aus. 

    Daraufhin wird die Übersichtsseite von Security Center geöffnet.

    :::image type="content" source="./media/security-center-get-started/overview.png" alt-text="Übersichtsdashboard von Security Center" lightbox="./media/security-center-get-started/overview.png":::

    In der **Security Center – Übersicht** erhalten Sie einen vereinheitlichten Überblick über den Sicherheitsstatus Ihrer Hybrid Cloud-Workloads und können so die Sicherheit Ihrer Workloads ermitteln und bewerten sowie Risiken identifizieren und mindern. Security Center aktiviert automatisch und kostenlos alle Ihre Azure-Abonnements, für die zuvor kein Onboarding von Ihnen oder von einem anderen Abonnementbenutzer durchgeführt wurde.

Sie können die Liste der Abonnements anzeigen und filtern, indem Sie das Menüelement **Abonnements** auswählen. Die Anzeige wird angepasst, um den Sicherheitsstatus der ausgewählten Abonnements widerzuspiegeln. 

Innerhalb weniger Minuten nach dem ersten Start von Security Center wird ggf. Folgendes angezeigt:

- **Empfehlungen** zur Verbesserung der Sicherheit Ihrer verbundenen Ressourcen.
- Die Ressourcen, die nun von Security Center bewertet werden, sowie der jeweilige Sicherheitsstatus.

Damit Sie Security Center in vollem Umfang nutzen können, fahren Sie mit den nächsten Schritten des Schnellstartabschnitts fort.



## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie Azure Security Center aktiviert. Im nächsten Schritt aktivieren Sie Azure Defender für die einheitliche Sicherheitsverwaltung und den Bedrohungsschutz in Ihren Hybrid Cloud Workloads.

> [!div class="nextstepaction"]
> [Schnellstart: Aktivieren von Azure Defender](enable-azure-defender.md)