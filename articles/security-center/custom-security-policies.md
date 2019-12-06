---
title: Erstellen benutzerdefinierter Sicherheitsrichtlinien in Azure Security Center | Microsoft-Dokumentation
description: Von Azure Security Center überwachte benutzerdefinierte Azure Policy-Definitionen.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 3e60c0e165b8f9ec79aac2048011d9e315b0ce18
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483958"
---
# <a name="using-custom-security-policies-preview"></a>Verwenden von benutzerdefinierten Sicherheitsrichtlinien (Vorschauversion)

Um Ihre Systeme und Ihre Umgebung zu sichern, generiert Azure Security Center Sicherheitsempfehlungen. Diese Empfehlungen basieren auf bewährten Methoden der Branche, die in die generische Standardsicherheitsrichtlinie integriert sind, die für alle Kunden bereitgestellt wird. Sie können auch aus Branchenwissen von Security Center und regulatorische Normen stammen.

Mit dieser Previewfunktion können Sie eigene *benutzerdefinierte* Initiativen hinzufügen. Sie erhalten dann Empfehlungen, wenn Ihre Umgebung die von Ihnen erstellten Richtlinien nicht einhält.

Wie [unter diesem Link](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#definition-location) in der Azure Policy-Dokumentation erläutert, müsse Sie als Speicherort für Ihre benutzerdefinierte Initiative eine Verwaltungsgruppe oder ein Abonnement angeben. 

## <a name="to-add-a-custom-initiative-to-your-subscription"></a>So fügen Sie eine benutzerdefinierte Initiative zu Ihrem Abonnement hinzu 

1. Öffnen Sie auf der Security Center-Randleiste die Seite **Sicherheitsrichtlinie**.

1. Wählen Sie ein Abonnement oder eine Verwaltungsgruppe aus, dem bzw. der Sie eine benutzerdefinierte Initiative hinzufügen möchten.

    [![Auswählen eines Abonnements, für das Sie die benutzerdefinierte Richtlinie erstellen](media/custom-security-policies/custom-policy-selecting-a-subscription.png)](media/custom-security-policies/custom-policy-selecting-a-subscription.png#lightbox)

    > [!NOTE]
    > Sie müssen benutzerdefinierte Standards auf Abonnementebene (oder höher) hinzufügen, damit diese ausgewertet und in Security Center angezeigt werden. 
    >
    > Wenn Sie einen benutzerdefinierten Standard hinzufügen, wird diesem Bereich eine *Initiative* zugewiesen. Daher empfiehlt es sich, den größtmöglichen Bereich auszuwählen, der für diese Zuweisung erforderlich.

1. Klicken Sie auf der Seite „Sicherheitsrichtlinie“ unter „Ihre benutzerdefinierten Initiativen (Vorschau)“ auf **Benutzerdefinierte Initiative hinzufügen**.

    [![Klicken Sie auf **Benutzerdefinierte Initiative hinzufügen**](media/custom-security-policies/custom-policy-add-initiative.png)](media/custom-security-policies/custom-policy-add-initiative.png#lightbox)

    Die folgende Seite wird angezeigt:

    ![Erstellen oder Hinzufügen einer Richtlinie](media/custom-security-policies/create-or-add-custom-policy.png)

1. Überprüfen Sie auf der Seite „Benutzerdefinierte Initiativen hinzufügen“ die Liste der benutzerdefinierten Richtlinien aus, die in Ihrer Organisation bereits erstellt wurden. Sehen Sie eine, die Sie Ihrem Abonnement zuweisen möchten, klicken Sie auf **Hinzufügen**. Wenn in der Liste keine Initiative enthalten ist, die Ihren Anforderungen entspricht, überspringen Sie diesen Schritt.

1. So erstellen Sie eine neue benutzerdefinierte Initiative

    1. Klicken Sie auf **Neu erstellen**.
    1. Geben Sie den Speicherort und den Namen der Definition ein.
    1. Wählen Sie die einzuschließenden Richtlinien aus, und klicken Sie auf **Hinzufügen**.
    1. Geben Sie die gewünschten Parameter ein.
    1. Klicken Sie auf **Speichern**.
    1. Klicken Sie auf der Seite „Benutzerdefinierte Initiativen hinzufügen“ auf „Aktualisieren“, und die neue Initiative wird als verfügbar angezeigt.
    1. Klicken Sie auf **Hinzufügen**, und weisen Sie sie Ihrem Abonnement zu.

    > [!NOTE]
    > Zum Erstellen neuer Initiativen sind Abonnementbesitzer-Anmeldeinformationen erforderlich. Weitere Informationen zu Azure-Rollen finden Sie unter [Berechtigungen in Azure Security Center](security-center-permissions.md).

1. Um die resultierenden Empfehlungen für Ihre Richtlinie anzuzeigen, klicken Sie in der Randleiste auf **Empfehlungen**, um die Seite „Empfehlungen“ zu öffnen. Die Empfehlungen werden mit der Bezeichnung „Benutzerdefiniert“ angezeigt und sind innerhalb einer Stunde verfügbar.

    [![Benutzerdefinierte Empfehlungen](media/custom-security-policies/custom-policy-recommendations.png)](media/custom-security-policies/custom-policy-recommendations-in-context.png#lightbox)


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie benutzerdefinierte Sicherheitsrichtlinien erstellt werden. 

Weitere verwandte Informationen finden Sie in den folgenden Artikeln: 

- [Übersicht über Sicherheitsrichtlinien](tutorial-security-policy.md)
- [Liste der integrierten Sicherheitsrichtlinien](security-center-policy-definitions.md)