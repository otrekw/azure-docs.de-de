---
author: baanders
description: Includedatei für die erforderlichen Berechtigungen für das Setup von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 10/14/2020
ms.author: baanders
ms.openlocfilehash: 919cc934920d922a82ceb9e30630006627af44e9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "92205483"
---
## <a name="prerequisites-permission-requirements"></a>Voraussetzungen: Berechtigungsanforderungen

Damit Sie alle in diesem Artikel beschriebenen Schritte ausführen können, benötigen [Sie eine Rolle](../articles/role-based-access-control/rbac-and-directory-admin-roles.md) mit den folgenden Berechtigungen für Ihr Abonnement:
* Erstellen und Verwalten von Azure-Ressourcen
* Verwalten des Benutzerzugriffs auf Azure-Ressourcen (einschließlich erteilen und delegieren von Berechtigungen)

Allgemeine Rollen, die diese Anforderung erfüllen, heißen *Besitzer* oder *Kontoadministrator*. Ebenso können die beiden Rollen *Benutzerzugriffsadministrator* und *Mitwirkender* kombiniert werden. Ausführliche Informationen zu den Rollen und Berechtigungen, einschließlich der Berechtigungen, die für andere Rollen freigeschaltet sind, finden Sie in der Dokumentation zu Azure RBAC unter [*Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Rollen*](../articles/role-based-access-control/rbac-and-directory-admin-roles.md).

Wenn Sie herausfinden möchten, welche Rolle Ihnen für Ihr Abonnement zugewiesen ist, erhalten Sie weitere Informationen auf der Seite [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) im Azure-Portal. Sie können entweder diesen Link verwenden oder über die Suchleiste des Portals nach *Abonnements* suchen. Suchen Sie nach dem Namen des Abonnements, das Sie verwenden, und prüfen Sie Ihre Rolle in der Spalte *Meine Rolle*:

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/portal/subscriptions-role.png" alt-text="Ansicht der Seite „Abonnements“ im Azure-Portal, die den Benutzer als Besitzer anzeigt" lightbox="../articles/digital-twins/media/how-to-set-up-instance/portal/subscriptions-role.png":::

Wenn Sie feststellen, dass Ihnen die Rolle *Mitwirkender* oder eine andere Rolle zugewiesen ist, die nicht die erforderlichen Berechtigungen beinhaltet, können Sie einen Benutzer Ihres Abonnements kontaktieren, der über diese Berechtigungen *verfügt* (z. B. der Besitzer oder der Kontoadministrator des Abonnements), und dann eine der folgenden Möglichkeiten auswählen, um fortzufahren:
* Bitten Sie den Benutzer darum, die in diesem Artikel beschriebenen Schritte in Ihrem Namen auszuführen.
* Bitten Sie darum, dass der Benutzer Ihnen eine höhere Rolle zuweist, damit Sie über die benötigten Berechtigungen verfügen, um selbst fortzufahren. Ob diese Vorgehensweise angemessen ist, hängt von Ihrer Organisation und Ihrer Rolle darin ab.