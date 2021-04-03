---
title: MAU-Abrechnungsmodell für externe Identitäten in Azure AD
description: Erfahren Sie mehr über das Abrechnungsmodell anhand monatlich aktiver Benutzer (Monthly Active Users, MAU) bei externe Identitäten für die Kollaboration von Gastbenutzern (B2B) in Azure AD. Erfahren Sie, wie Sie Ihren Azure AD-Mandanten mit einem Azure-Abonnement verknüpfen.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 09/21/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.workload: identity
ms.collection: M365-identity-device-management
ms.openlocfilehash: 983a803245467145a0b1161a4495e8045759e7ab
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92442064"
---
# <a name="billing-model-for-azure-ad-external-identities"></a>Abrechnungsmodell für externe Identitäten in Azure AD

Die Preise für externe Identitäten in Azure Active Directory (Azure AD) basieren auf den monatlich aktiven Benutzern (MAU). Dies ist die Anzahl eindeutiger Benutzer mit Authentifizierungsaktivität innerhalb eines Kalendermonats. Dieses Abrechnungsmodell gilt sowohl für die Azure AD-Kollaboration von Gastbenutzern (B2B) als auch für [Azure AD B2C-Mandanten](../../active-directory-b2c/billing.md). Die MAU-Abrechnung hilft Ihnen, Ihre Kosten zu senken, da sie einen kostenlosen Tarif und einen flexiblen, vorhersehbaren Preis bietet. In diesem Artikel erhalten Sie Informationen zur MAU-Abrechnung und zum Verknüpfen Ihrer Azure AD-Mandanten mit einem Abonnement.

> [!IMPORTANT]
> Dieser Artikel enthält keine Preisinformationen. Die aktuellen Informationen zur Verbrauchsabrechnung und zu den Preisen finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-i-need-to-do"></a>Wie muss ich vorgehen?

Um die MAU-Abrechnung zu nutzen, muss Ihr Azure AD-Mandanten mit einem Azure-Abonnement verknüpft werden.

|Wenn für Ihren Mandanten Folgendes gilt:  |Sie müssen folgende Schritte durchführen:  |
|---------|---------|
| Ein Azure AD-Mandant, der bereits mit einem Abonnement verknüpft ist     | Sie unternehmen nichts. Wenn Sie Features externer Identitäten für die Zusammenarbeit mit Gastbenutzern verwenden, erfolgt Ihre Abrechnung automatisch nach dem MAU-Modell.        |
| Ein Azure AD-Mandant, der noch nicht mit einem Abonnement verknüpft ist     | [Verknüpfen Sie Ihren Azure AD-Mandanten mit einem Abonnement](#link-your-azure-ad-tenant-to-a-subscription), um die MAU-Abrechnung zu aktivieren.        |
|  |  |

## <a name="about-monthly-active-users-mau-billing"></a>Informationen zur Abrechnung basierend auf den monatlich aktiven Benutzern (MAU)

In Ihrem Azure AD-Mandanten wird der Verbrauch für die Kollaboration mit Gastbenutzern basierend auf der Anzahl eindeutiger Gastbenutzer mit Authentifizierungsaktivitäten innerhalb eines Kalendermonats abgerechnet. Dieses Modell ersetzt das Abrechnungsmodell im Verhältnis 1:5, das bis zu fünf Gastbenutzer pro Azure AD Premium-Lizenz in Ihrem Mandanten zuließ. Wenn Ihr Mandant mit einem Abonnement verknüpft ist und Sie Features externer Identitäten für die Zusammenarbeit mit Gastbenutzern verwenden, erfolgt Ihre Abrechnung automatisch nach dem MAU-basierten Abrechnungsmodell.
  
Der Tarif für Ihre Gastbenutzer basiert auf dem höchsten Tarif, der Ihrem Azure AD-Mandanten zugewiesen ist. Wenn der höchste Tarif in Ihrem Mandanten z. B. Azure AD Premium P1 ist, gilt der Tarif Premium P1 auch für Ihre Gastbenutzer. Ist der höchste Tarif Azure AD Free, werden Sie aufgefordert, ein Upgrade auf einen Premium-Tarif durchzuführen, wenn Sie versuchen, Premium-Features für Gastbenutzer zu verwenden.

## <a name="link-your-azure-ad-tenant-to-a-subscription"></a>Verknüpfen Ihres Azure AD-Mandanten mit einem Abonnement

Ein Azure AD-Mandant muss mit einem Azure-Abonnement verknüpft werden, um eine ordnungsgemäße Abrechnung zu ermöglichen und Zugriff auf Features zu erhalten. Wenn das Verzeichnis nicht bereits über ein Abonnement verfügt, mit dem Sie eine Verknüpfung herstellen können, haben Sie die Möglichkeit, während dieses Vorgangs eine hinzuzufügen.

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com/) mit einem Azure-Konto an, dem mindestens die Rolle [Mitwirkender](../../role-based-access-control/built-in-roles.md) innerhalb des Abonnements oder einer Ressourcengruppe im Abonnement zugewiesen wurde.

2. Wählen Sie das Verzeichnis aus, zu dem Sie eine Verknüpfung herstellen möchten: Wählen Sie auf der Symbolleiste im Azure-Portal das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus.

    ![Auswählen des Symbols „Verzeichnis + Abonnement“](media/external-identities-pricing/portal-mau-pick-directory.png)

3. Wählen Sie unter **Azure-Dienste** die Option **Azure Active Directory** aus.

4. Wählen Sie im Menü auf der linken Seite **Externe Identitäten** aus.

5. Wählen Sie unter **Abonnements** die Option **Verknüpfte Abonnements** aus.

6. Aktivieren Sie in der Mandantenliste das Kontrollkästchen neben dem Mandanten, und wählen Sie dann **Abonnement verknüpfen** aus.

    ![Auswählen des Mandanten und Verknüpfen eines Abonnements](media/external-identities-pricing/linked-subscriptions.png)

7. Wählen Sie im Bereich „Abonnement verknüpfen“ ein **Abonnement** und eine **Ressourcengruppe** aus. Wählen Sie dann **Anwenden** aus.

   > [!NOTE]
   > Wenn keine Abonnements aufgeführt sind, können Sie [Ihrem Mandanten ein Abonnement zuordnen](../fundamentals/active-directory-how-subscriptions-associated-directory.md). Sie können aber auch ein neues Abonnement hinzufügen, indem Sie den Link **Wenn Sie noch kein Abonnement haben, können Sie hier ein Abonnement erstellen** auswählen.

    ![Auswählen eines Abonnements und einer Ressourcengruppe](media/external-identities-pricing/link-subscription-resource.png)

Nachdem Sie diese Schritte ausgeführt haben, erfolgt die Abrechnung für Ihr Azure-Abonnement gemäß den Azure Direct- bzw. Enterprise Agreement-Details (sofern zutreffend).

## <a name="next-steps"></a>Nächste Schritte

Die aktuellsten Preisinformationen finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).