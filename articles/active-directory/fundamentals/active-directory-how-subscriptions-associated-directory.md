---
title: Hinzufügen eines vorhandenen Azure-Abonnements zu Ihrem Mandanten – Azure AD
description: Anleitungen zum Hinzufügen eines vorhandenen Azure-Abonnements zu Ihrem Azure Active Directory-Mandanten.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 104bf51fb03d88ab0e5efd25ebebb0e3060bc264
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457925"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Zuordnen oder Hinzufügen eines Azure-Abonnements zu Ihrem Azure Active Directory-Mandanten

Ein Azure-Abonnement weist eine Vertrauensstellung mit Azure Active Directory (Azure AD) auf. Ein Abonnement vertraut Azure AD bei der Authentifizierung von Benutzern, Diensten und Geräten.

Mehrere Abonnements können demselben Azure AD-Verzeichnis vertrauen. Jedes Abonnement kann nur einem einzelnen Verzeichnis vertrauen.

Wenn Ihr Abonnement abläuft, verlieren Sie auch den Zugriff auf alle Ressourcen, die dem Abonnement zugwiesen sind. Das Azure AD-Verzeichnis bleibt jedoch in Azure. Sie können das Verzeichnis mit einem anderen Azure-Abonnement verknüpfen und verwalten.

Alle Ihre Benutzer verfügen über ein einzelnes *Basisverzeichnis* für die Authentifizierung. Ihre Benutzer können jedoch auch Gäste in anderen Verzeichnissen sein. Ihnen werden sowohl das Basis- als auch das Gastverzeichnis für jeden Benutzer in Azure AD angezeigt.

> [!Important]
> Wenn Sie ein Abonnement einem anderen Verzeichnis zuordnen, verlieren Benutzer, denen Rollen mit der [rollenbasierten Zugriffssteuerung (RBAC)](../../role-based-access-control/role-assignments-portal.md) zugewiesen wurden, ihren Zugriff. Klassische Abonnementadministratoren (Dienstadministrator und Co-Admin) verlieren ebenfalls ihren Zugriff.
>
> Richtlinienzuweisungen werden auch aus einem Abonnement entfernt, wenn das Abonnement einem anderen Verzeichnis zugeordnet ist.
>
> Das Verschieben Ihres Azure Kubernetes Service-Clusters (AKS-Clusters) in ein anderes Abonnement oder das Verschieben des Abonnements, das den Cluster besitzt, in einen neuen Mandanten führt dazu, dass der Cluster seine Funktionalität aufgrund des Verlusts von Rollenzuweisungen und Dienstprinzipalrechten verliert. Weitere Informationen zu AKS finden Sie unter [Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/).


## <a name="before-you-begin"></a>Voraussetzungen

Bevor Sie Ihr Abonnement zuweisen oder hinzufügen können, müssen Sie die folgenden Aufgaben ausführen:

- Überprüfen Sie die folgende Liste von Änderungen und deren möglichen Folgen für Sie:

  - Benutzer, deren Rollen mithilfe von RBAC zugewiesen wurden, verlieren den Zugriff.
  - Dienstadministrator und Co-Admins verlieren den Zugriff.
  - Wenn Sie über Schlüsseltresore verfügen, kann darauf nicht mehr zugegriffen werden, und Sie müssen sie nach der Zuweisung korrigieren.
  - Wenn Sie über verwaltete Identitäten für Ressourcen wie Virtual Machines oder Logic Apps verfügen, müssen Sie diese nach der Zuordnung nochmal aktivieren oder erstellen.
  - Wenn Sie eine Azure Stack-Instanz konfiguriert haben, müssen Sie diese nach der Zuweisung erneut registrieren.

- Melden Sie sich mit einem Konto an, das:

  - Über die Rollenzuweisung [Besitzer](../../role-based-access-control/built-in-roles.md#owner) für das Abonnement verfügt. Weitere Informationen zum Zuweisen der Rolle „Besitzer“ finden Sie unter [Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und dem Azure-Portal](../../role-based-access-control/role-assignments-portal.md).
  - Ist im aktuellen Verzeichnis und im neuen Verzeichnis vorhanden. Das aktuelle Verzeichnis ist dem Abonnement zugeordnet. Verknüpfen Sie das neue Verzeichnis mit dem Abonnement. Weitere Informationen zum Zugriff auf ein anderes Verzeichnis finden Sie unter [Hinzufügen von Azure Active Directory B2B-Zusammenarbeitsbenutzern über das Azure-Portal](../b2b/add-users-administrator.md).

- Achten Sie darauf, dass Sie kein Azure-Clouddienstanbieter-Abonnement (MS-AZR-0145P, MS-AZR-0146P, MS-AZR-0146P, MS-AZR-159P), kein internes Microsoft-Abonnement (MS-AZR-0015P) und kein Microsoft Imagine-Abonnement (MS-AZR-0144P) verwenden.

## <a name="associate-a-subscription-to-a-directory"></a>Zuordnen eines Abonnements zu einem Verzeichnis<a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>

Führen Sie die folgenden Schritte aus, um Ihrem Azure AD-Verzeichnis ein vorhandenes Abonnement zuzuweisen:

1. Melden Sie sich an, und wählen Sie im [Azure-Portal auf der Seite „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ein Abonnement aus.

1. Wählen Sie **Verzeichnis ändern** aus.

    ![Seite „Abonnements“ mit hervorgehobener Option „Verzeichnis ändern“](media/active-directory-how-subscriptions-associated-directory/change-directory-in-azure-subscriptions.png)

1. Überprüfen Sie alle angezeigten Warnungen, und wählen Sie dann **Ändern** aus.

    ![Ändern der Verzeichnisseite und Anzeigen des zu ändernden Verzeichnisses](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    Das Verzeichnis wird für das Abonnement geändert, und eine Erfolgsmeldung wird angezeigt.

    ![Nachricht über die erfolgreiche Änderung des Verzeichnisses](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)

Rufen Sie mithilfe der Option **Verzeichnis wechseln** Ihr neues Verzeichnis auf. Es kann mehrere Stunden dauern, bis alles ordnungsgemäß angezeigt wird. Wenn es zu lange dauert, überprüfen Sie den **globalen Abonnementfilter**. Stellen Sie sicher, dass das verschobene Abonnement nicht ausgeblendet ist. Möglicherweise müssen Sie sich beim Azure-Portal abmelden und wieder anmelden, damit das neue Verzeichnis angezeigt wird.

![Seite mit dem Verzeichnisumschalter mit Beispielinformationen](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Das Ändern des Abonnementverzeichnisses erfolgt auf Dienstebene, sodass sich der Vorgang nicht auf die Abonnementabrechnung auswirkt. Der Kontoadministrator kann den Dienstadministrator weiterhin über das [Kontocenter](https://account.azure.com/subscriptions) ändern. Um das ursprüngliche Verzeichnis zu löschen, müssen Sie die Abonnementabrechnung einem neuen Kontoadministrator übertragen. Weitere Informationen zum Übertragen der Abrechnung finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](../../cost-management-billing/manage/billing-subscription-transfer.md).

## <a name="post-association-steps"></a>Schritte nach der Zuordnung

Nachdem Sie ein Abonnement einem anderen Verzeichnis zugeordnet haben, müssen Sie möglicherweise die folgenden Aufgaben ausführen, um den Vorgang fortzusetzen:

- Wenn Sie über Schlüsseltresore verfügen, müssen Sie die Mandanten-ID des Schlüsseltresors ändern. Weitere Informationen finden Sie unter [Ändern der Mandanten-ID des Schlüsseltresors nach einer Abonnementverschiebung](../../key-vault/general/subscription-move-fix.md).

- Wenn Sie vom System zugewiesene verwaltete Identitäten für Ressourcen verwendet haben, müssen Sie diese nochmal aktivieren. Wenn Sie vom Benutzer zugewiesene verwaltete Identitäten verwendet haben, müssen Sie diese nochmal erstellen. Nach dem erneuten Aktivieren oder Erstellen der verwalteten Identitäten müssen Sie die Berechtigungen, die diesen Identitäten zugewiesen sind, erneut einrichten. Weitere Informationen finden Sie im unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../managed-identities-azure-resources/overview.md).

- Wenn Sie mit diesem Abonnement eine Azure Stack-Instanz registriert haben, müssen Sie diese nochmal registrieren. Weitere Informationen finden Sie unter [Registrieren von Azure Stack in Azure](/azure-stack/operator/azure-stack-registration).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen eines neuen Azure AD-Mandanten finden Sie unter [Schnellstart: Erstellen eines neuen Mandanten in Azure Active Directory](active-directory-access-create-new-tenant.md).

- Weitere Informationen dazu, wie Microsoft Azure den Zugriff auf Ressourcen steuert, finden Sie unter [Administratorrollen für klassische Abonnements, Azure RBAC-Rollen und Azure AD-Administratorrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md).

- Weitere Informationen zum Zuweisen von Rollen in Azure AD finden Sie unter [Zuweisen von Administrator- und anderen Rollen zu Benutzern mithilfe von Azure Active Directory](active-directory-users-assign-role-azure-portal.md).
