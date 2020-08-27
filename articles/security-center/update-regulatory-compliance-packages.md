---
title: Aktualisieren auf die dynamische Überwachung der Einhaltung gesetzlicher Bestimmungen in Ihrem Azure Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen | Microsoft-Dokumentation
description: Aktualisieren der Compliancepakete für die Einhaltung gesetzlicher Vorschriften
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 28b6a70297efb8d8237f085e1f5b08dbb6a09072
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88522605"
---
# <a name="customizing-the-set-of-standards-in-your-regulatory-compliance-dashboard"></a>Anpassen der Standards in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen

Azure Security Center vergleicht die Konfiguration Ihrer Ressourcen kontinuierlich mit den Anforderungen von Branchenstandards, Vorschriften und Benchmarks. Das **Dashboard für die Einhaltung gesetzlicher Bestimmungen** bietet Erkenntnisse zu Ihrem Compliancestatus basierend auf der Erfüllung bestimmter Compliancevorgaben und -anforderungen.


## <a name="overview-of-compliance-packages"></a>Überblick über Compliancepakete

Industriestandards, gesetzliche Standards und Benchmarks werden im Security Center als *Compliancepakete* dargestellt.  Jedes Paket kann als eine in Azure Policy definierte Initiative verstanden werden. Fügen Sie der Verwaltungsgruppe oder dem Abonnement auf der Seite **Sicherheitsrichtlinie** ein Compliancepaket hinzu, um Compliancedaten als zugeordnete Bewertungen in Ihrem Dashboard anzuzeigen. Weitere Informationen zu Azure Policy und Initiativen erhalten Sie unter [Arbeiten mit Sicherheitsrichtlinien](tutorial-security-policy.md).

Nachdem Sie das Onboarding für einen Standard oder einen Benchmarkwert für einen ausgewählten Bereich durchgeführt haben, wird die Initiative dem Bereich zugeordnet und in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen mit allen zugeordneten Compliancedaten in Form von Bewertungen angezeigt. Ein zusammenfassender Bericht für alle Standards, für die das Onboarding durchgeführt wurde, wird als Download bereitgestellt.

Microsoft überwacht die gesetzlichen Standards auch selbst und passt die Pakete im Laufe der Zeit entsprechend an. Wenn Microsoft neue Inhalte für die Initiative veröffentlicht (neue Richtlinien, die mehr Regeln im Standard entsprechen), werden die zusätzlichen Inhalte automatisch in Ihrem Dashboard hinzugefügt.

> [!TIP]
> Ein Standard, der im Laufe der Zeit immer weiterentwickelt wird und von Microsoft in Form von Releases veröffentlicht wird, ist **Azure CIS 1.1.0 (neu)** . Dieser Standard wird formell auch als [CIS Microsoft Azure Foundations Benchmark v1.1.0](https://www.cisecurity.org/benchmark/azure/) bezeichnet. Fügen Sie neue Inhalte zusammen mit „Azure CIS 1.1.0“, also die Azure CIS-Repräsentation, die standardmäßig in jeder Security Center-Umgebung konfiguriert ist, Ihrem Dashboard hinzu. Dieses Paket basiert auf statischen Regeln. Das neuere Paket beinhaltet mehr Richtlinien und führt im Laufe der Zeit automatisch Updates durch. Aktualisieren Sie wie unten beschrieben auf das neue dynamische Paket.


## <a name="available-packages"></a>Verfügbare Pakete

Sie können Standards wie NIST SP 800-53 R4, SWIFT CSP CSCF-v2020, UK Official & UK NHS, Canada Federal PBMM sowie Azure CIS 1.1.0 (neu) hinzufügen. Dabei handelt es sich um eine vollständigere Repräsentation für Azure CIS 1.1.0. 

Zusätzlich können Sie den **Azure-Sicherheitsvergleichstest** hinzufügen. Dabei handelt es sich um von Microsoft erstellte Azure-spezifische Richtlinien zu den Best Practices für Sicherheit und Compliance, die auf allgemeinen Complianceframeworks basieren. Unter [Einführung zum Azure Security-Vergleichstest](https://docs.microsoft.com/azure/security/benchmarks/introduction) erhalten Sie weitere Informationen.

Weitere Standards werden im Dashboard unterstützt, sobald sie verfügbar sind. 


## <a name="adding-a-regulatory-standard-to-your-dashboard"></a>Hinzufügen eines gesetzlichen Standards in Ihrem Dashboard

In den folgenden Schritten wird erklärt, wie Sie ein Paket hinzufügen, um Ihre Compliance mit einem der unterstützten gesetzlichen Standards zu überwachen.

> [!NOTE]
> Nur Benutzer, die Besitzer oder Mitwirkende an Richtlinien sind, haben die erforderlichen Berechtigungen zum Hinzufügen von Compliancestandards. 

1. Wählen Sie auf der Seitenleiste in Security Center **Einhaltung gesetzlicher Bestimmungen** aus, um das Dashboard für die Einhaltung gesetzlicher Bestimmungen zu öffnen. Hier sehen Sie die Compliancestandards, die den aktuell ausgewählten Abonnements zugeordnet sind.   

1. Wählen Sie oben auf der Seite **Konformitätsrichtlinien verwalten** aus. Dadurch wird die Seite für die Richtlinienverwaltung geöffnet.

1. Wählen Sie das Abonnement oder die Verwaltungsgruppe aus, für das bzw. die Sie den Status der Einhaltung gesetzlicher Bestimmungen verwalten möchten. 

    > [!TIP]
    > Es wird empfohlen, den höchsten Bereich auszuwählen, für den der Standard gilt, damit Compliancedaten für alle geschachtelten Ressourcen aggregiert und nachverfolgt werden. 

1. Wenn Sie die für Ihre Organisation relevanten Standards hinzufügen möchten, klicken Sie auf **Weitere Standards hinzufügen**. 

1. Auf der Seite **Standards zur Einhaltung gesetzlicher Bestimmungen hinzufügen** können Sie nach Paketen für alle verfügbaren Standards suchen. Unten finden Sie Beispiele für verfügbare Standards:

    - **Azure-Sicherheitsvergleichstest**
    - **NIST SP 800-53 R4**
    - **SWIFT CSP CSCF-v2020**
    - **UKO und UK NHS**
    - **Canada PBMM**
    
    ![Hinzufügen von Compliancepaketen zum Dashboard für die Einhaltung gesetzlicher Bestimmungen in Azure Security Center](./media/update-regulatory-compliance-packages/dynamic-regulatory-compliance-additional-standards.png)

1. Klicken Sie auf **Hinzufügen**, und geben Sie alle notwendigen Details für die jeweilige Initiative ein, z. B. Bereich, Parameter und Wartung.

1. Wählen Sie auf der Seitenleiste in Security Center erneut **Einhaltung gesetzlicher Bestimmungen** aus, um zum Dashboard für die Einhaltung gesetzlicher Bestimmungen zurückzukehren.
    * Ihr neuer Standard wird jetzt in der Liste der Branchen- und gesetzlichen Standards aufgeführt. 
    * Wenn Sie **Azure CIS 1.1.0 (neu)** hinzugefügt haben, bleibt die ursprüngliche *statische* Ansicht Ihrer Azure CIS 1.1.0-Compliance ebenfalls erhalten. Sie wird möglicherweise zukünftig automatisch entfernt.

    > [!NOTE]
    > Es kann einige Stunden dauern, bis ein neu hinzugefügter Standard im Compliance-Dashboard angezeigt wird.

    [![Dashboard für die Einhaltung gesetzlicher Bestimmungen mit neuem und altem Azure CIS](media/update-regulatory-compliance-packages/regulatory-compliance-dashboard-with-benchmark-small.png)](media/update-regulatory-compliance-packages/regulatory-compliance-dashboard-with-benchmark.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zum **Hinzufügen von Compliancepaketen** zum Überwachen Ihrer Compliance mit zusätzlichen Standards erhalten. 

Weitere verwandte Informationen finden Sie in den folgenden Artikeln: 

- [Einführung zum Azure Security-Vergleichstest](https://docs.microsoft.com/azure/security/benchmarks/introduction)
- [Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen](security-center-compliance-dashboard.md)
- [Arbeiten mit Sicherheitsrichtlinien](tutorial-security-policy.md)