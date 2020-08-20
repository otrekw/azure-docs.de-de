---
author: baanders
description: Includedatei für eine Schrittübersicht und die erforderlichen Berechtigungen für das Setup von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 8/7/2020
ms.author: baanders
ms.openlocfilehash: b87d45a8be68a77df7b06ebd6e80562ccbe0e444
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009648"
---
>[!NOTE]
>Diese Vorgänge sollen von einem Benutzer durchgeführt werden, der die Berechtigung hat, beide Ressourcen und den Benutzerzugriff auf das Azure-Abonnement zu verwalten. Obwohl einige Schritte mit niedrigeren Berechtigungen ausgeführt werden können, ist die Zusammenarbeit mit einem Benutzer mit diesen Berechtigungen erforderlich, um eine verwendbare Instanz vollständig einzurichten. Weitere Informationen hierzu finden Sie im Abschnitt [*Voraussetzungen: Erforderliche Berechtigungen*](#prerequisites-permission-requirements) weiter unten.

Das vollständige Setup für eine neue Azure Digital Twins-Instanz umfasst drei Teile:
1. **Erstellen der Instanz**
2. **Einrichten von Benutzerzugriffsberechtigungen:** Azure-Benutzer müssen über die Rolle *Azure Digital Twins-Besitzer (Vorschau)* auf der Azure Digital Twins-Instanz verfügen, um sie und ihre Daten verwalten zu können. In diesem Schritt weisen Sie als Besitzer/Administrator des Azure-Abonnements diese Rolle der Person zu, die Ihre Azure Digital Twins-Instanz verwalten soll. Dies können Sie selbst sein oder eine andere Person in Ihrer Organisation.
3. **Einrichten von Zugriffsberechtigungen für Clientanwendungen:** Es ist üblich, eine Clientanwendung zu schreiben, die für die Interaktion mit Ihrer Instanz verwendet werden soll. Damit diese Client-App auf Ihre Azure Digital Twins-Instanz zugreifen kann, müssen Sie eine *App-Registrierung* in [Azure Active Directory](../articles/active-directory/fundamentals/active-directory-whatis.md) einrichten, mit der sich die Clientanwendung bei der Instanz authentifiziert.

Zum Fortfahren benötigen Sie ein Azure-Abonnement. Sie können [hier](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) ein kostenloses einrichten.

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