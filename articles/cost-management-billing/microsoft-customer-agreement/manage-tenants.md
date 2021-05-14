---
title: 'Verwalten von Mandanten in Ihrem Abrechnungskonto der Microsoft-Kundenvereinbarung: Azure'
description: Dieser Artikel hilft Ihnen, Mandanten zu verstehen und zu verwalten, die Ihrem Abrechnungskonto der Microsoft-Kundenvereinbarung (Microsoft Customer Agreement, MCA) zugeordnet sind.
author: bandersmsft
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 04/06/2021
ms.author: banders
ms.reviewer: baolcsva
ms.openlocfilehash: dc34d0f12430838be29897ccc5cbeee382ecaa2b
ms.sourcegitcommit: afb79a35e687a91270973990ff111ef90634f142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107485588"
---
# <a name="manage-tenants-in-your-microsoft-customer-agreement-billing-account"></a>Verwalten von Mandanten in Ihrem Abrechnungskonto der Microsoft-Kundenvereinbarung

Dieser Artikel hilft Ihnen, Mandanten zu verstehen und zu verwalten, die Ihrem Abrechnungskonto der Microsoft-Kundenvereinbarung (Microsoft Customer Agreement, MCA) zugeordnet sind. Verwenden Sie die Informationen, um Mandanten zu verwalten, Abonnements zu übertragen und den Abrechnungsbesitz zu verwalten, während Sie den sicheren Zugriff auf Ihre Abrechnungsumgebung sicherstellen.

## <a name="whats-a-tenant"></a>Was ist ein Mandant?

Ein Mandant ist eine digitale Darstellung Ihrer Organisation und in erster Linie einer Domäne, z. B. Microsoft.com, zugeordnet. Es handelt sich dabei um eine Umgebung, die über Azure Active Directory verwaltet wird und es Ihnen ermöglicht, Benutzern Berechtigungen zum Verwalten von Azure-Ressourcen und -Abrechnung zuzuweisen.

Jeder Mandant unterscheidet sich von anderen Mandanten, aber Sie können Gastbenutzern aus anderen Mandanten den Zugriff auf Ihren Mandanten gestatten, um Ihre Kosten nachzuverfolgen und die Abrechnung zu verwalten.

## <a name="how-tenants-and-subscriptions-relate-to-billing-account"></a>Beziehung zwischen Mandanten/Abonnements und Abrechnungskonto

Sie verwenden Ihre Microsoft-Kundenvereinbarung (Abrechnungskonto), um Kosten nachzuverfolgen und die Abrechnung zu verwalten. Jedes Abrechnungskonto verfügt über mindestens ein Abrechnungsprofil. Mit dem Abrechnungsprofil können Sie Ihre Rechnungen und Zahlungsmethoden verwalten. Jedes Abrechnungsprofil enthält standardmäßig einen Rechnungsabschnitt. Sie können bei Bedarf weitere Rechnungsabschnitte erstellen, um Kosten auf einer präziseren Ebene zu gruppieren, nachzuverfolgen und zu verwalten.

- Ihr Abrechnungskonto ist einem einzelnen Mandanten zugeordnet. Dies bedeutet, dass nur Benutzer, die Teil des Mandanten sind, auf Ihr Abrechnungskonto zugreifen können.
- Wenn Sie ein neues Azure-Abonnement für Ihr Abrechnungskonto erstellen, wird es immer in Ihrem Abrechnungskontomandanten erstellt. Sie können Abonnements jedoch in andere Mandanten verschieben. Sie können auch vorhandene Abonnements anderer Mandanten mit Ihrem Abrechnungskonto verknüpfen. Dies ermöglicht die zentrale Verwaltung der Abrechnung über einen Mandanten, während Ressourcen und Abonnements bei Bedarf in anderen Mandanten verbleiben.

Das folgende Diagramm zeigt, wie das Abrechnungskonto und Abonnements mit Mandanten verknüpft sind. Das MCA-Abrechnungskonto von Contoso ist Mandant 1 zugeordnet, während das Contoso-Konto mit nutzungsbasierter Zahlung Mandant 2 zugeordnet ist. Angenommen, Contoso möchte für sein Abonnement mit nutzungsbasierter Zahlung über sein MCA-Abrechnungskonto bezahlen. In diesem Fall kann Contoso eine Übertragung des Abrechnungsbesitzes verwenden, um das Abonnement mit dem MCA-Abrechnungskonto zu verknüpfen. Das Abonnement und seine Ressourcen sind weiterhin Mandant 2 zugeordnet, aber sie werden über das MCA-Abrechnungskonto bezahlt:

:::image type="content" source="./media/manage-tenants/billing-hierarchy-example.png" alt-text="Diagramm: Beispielabrechnungshierarchie" border="false" lightbox="./media/manage-tenants/billing-hierarchy-example.png":::

## <a name="manage-subscriptions-under-multiple-tenants-in-a-single-microsoft-customer-agreement"></a>Verwalten von Abonnements unter mehreren Mandanten in einer Microsoft-Kundenvereinbarung

Abrechnungsbesitzer können Abonnements erstellen, wenn sie über die [entsprechenden Berechtigungen](../manage/understand-mca-roles.md#subscription-billing-roles-and-tasks) für das Abrechnungskonto verfügen. Standardmäßig befinden sich alle neuen Abonnements, die unter der Microsoft-Kundenvereinbarung erstellt werden, im Mandanten der Microsoft-Kundenvereinbarung.

- Sie können Abonnements von anderen Mandanten mit Ihrem Abrechnungskonto der Microsoft-Kundenvereinbarung verknüpfen. Wenn Sie den Abrechnungsbesitz eines Abonnements übernehmen, ändert sich nur die Rechnungsstellung. Dies hat keine Auswirkungen auf den Dienstmandanten oder die Azure RBAC-Rollen.
- Um den Abonnementbesitzer im Dienstmandanten zu ändern, müssen Sie das [Abonnement in ein anderes Azure Active Directory-Verzeichnis übertragen](../../role-based-access-control/transfer-subscription.md).

## <a name="add-guest-users-to-your-microsoft-customer-agreement-tenant"></a>Hinzufügen von Gastbenutzern zum Mandanten Ihrer Microsoft-Kundenvereinbarung

Benutzer, die dem Abrechnungsmandanten Ihrer Microsoft-Kundenvereinbarung hinzugefügt werden, müssen als Gast eingeladen werden, um Abrechnungsaufgaben von einem anderen Mandanten aus zu verwalten.

Der Benutzer muss über eine vorhandene E-Mail-Adresse mit einer Domäne verfügen, die sich von Ihrer Azure Active Directory-Domäne (AD) unterscheidet, um eine Person als Gast einzuladen. Azure AD sendet dem Gastbenutzer eine E-Mail mit einem Link zur Authentifizierung:

:::image type="content" source="./media/manage-tenants/guest-invitation-email.png" alt-text="Screenshot: Beispiel für eine Einladung per E-Mail" lightbox="./media/manage-tenants/guest-invitation-email.png" :::

Wenn ein Benutzer dem Mandanten der Microsoft-Kundenvereinbarung hinzugefügt wird, muss dieser die [Einladung annehmen](../../active-directory/external-identities/b2b-quickstart-add-guest-users-portal.md#accept-the-invitation).

Wenn Benutzer den Link **Einladung annehmen** auswählen, werden sie aufgefordert, sich bei Azure zu authentifizieren:

:::image type="content" source="./media/manage-tenants/authenticate.png" alt-text="Screenshot: Eingabeaufforderung zur Authentifizierung bei Azure" lightbox="./media/manage-tenants/authenticate.png" :::

Anschließend wählen sie **Annehmen** aus.

:::image type="content" source="./media/manage-tenants/accept-invitation.png" alt-text="Screenshot: Eingabeaufforderung zum Annehmen der Einladung" lightbox="./media/manage-tenants/accept-invitation.png" :::

Nach der Annahme können sie [das Abrechnungskonto der Microsoft-Kundenvereinbarung unter „Cost Management + Billing“ anzeigen](../understand/mca-overview.md#check-access-to-a-microsoft-customer-agreement):

:::image type="content" source="./media/manage-tenants/billing-microsoft-customer-agreement-in-list.png" alt-text="Screenshot: Microsoft-Kundenvereinbarung in der Liste der Abrechnungskonten" lightbox="./media/manage-tenants/billing-microsoft-customer-agreement-in-list.png" :::

Die Autorisierung zum Einladen von Gastbenutzern wird durch Ihre Azure AD-Einstellungen gesteuert. Der Wert der Einstellungen wird auf der Seite **Organisationsbeziehungen** unter **Einstellungen** angezeigt. Stellen Sie sicher, dass die Einstellung ausgewählt ist, andernfalls schlägt die Einladung fehl. Weitere Informationen finden Sie unter [Einschränken von Gastzugriffsberechtigungen (Vorschau) in Azure Active Directory](../../active-directory/enterprise-users/users-restrict-guest-permissions.md).

:::image type="content" source="./media/manage-tenants/external-collaboration-settings.png" alt-text="Screenshot: Einstellungen für externe Zusammenarbeit" lightbox="./media/manage-tenants/external-collaboration-settings.png" :::

> [!IMPORTANT]
> Gastbenutzer erhalten Zugriff auf den Mandanten der Microsoft-Kundenvereinbarung, was möglicherweise ein Sicherheitsproblem darstellen kann. Weitere Informationen finden Sie unter [Standardberechtigungen von Mitgliedsbenutzern einschränken](../../active-directory/fundamentals/users-default-permissions.md#restrict-member-users-default-permissions).

## <a name="manage-multiple-microsoft-cloud-services-under-an-azure-ad-tenant"></a>Verwalten mehrerer Microsoft-Clouddienste unter einem Azure AD-Mandanten

Sie können mehrere Clouddienste für Ihre Organisation unter einem einzelnen Azure AD-Mandanten verwalten. Benutzerkonten für alle Cloudangebote von Microsoft werden im Azure AD-Mandanten gespeichert, der Benutzerkonten und Gruppen enthält. Das folgende Diagramm zeigt ein Beispiel für eine Organisation mit mehreren Diensten, die einen gemeinsamen Azure AD-Mandanten mit Konten verwendet. Jeder Dienst verfügt über ein eigenes Portal (in blauer Textform), in dem Benutzer ihre Dienste verwalten:

:::image type="content" source="./media/manage-tenants/diagram-multiple-services-common-azure-ad-tenant-accounts.png" alt-text="Diagramm: Beispiel für eine Organisation mit mehreren Diensten, die einen gemeinsamen Azure AD-Mandanten mit Konten verwendet" border="false" lightbox="./media/manage-tenants/diagram-multiple-services-common-azure-ad-tenant-accounts.png":::

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um zu erfahren, wie Sie den flexiblen Abrechnungsbesitz verwalten und den sicheren Zugriff auf Ihre Microsoft-Kundenvereinbarung sicherstellen:

- [Schnellstart: Einrichten eines Mandanten](../../active-directory/develop/quickstart-create-new-tenant.md)
- [Integrierte Azure-Rollen](../../role-based-access-control/built-in-roles.md)
- [Übertragen eines Azure-Abonnements in ein anderes Azure AD-Verzeichnis](../../role-based-access-control/transfer-subscription.md)
- [Einschränken von Gastzugriffsberechtigungen (Vorschau) in Azure Active Directory](../../active-directory/enterprise-users/users-restrict-guest-permissions.md)
- [Hinzufügen von Gastbenutzern zu Ihrem Verzeichnis im Azure-Portal](../../active-directory/external-identities/b2b-quickstart-add-guest-users-portal.md#accept-the-invitation)
- [Welche Standardbenutzerberechtigungen gibt es in Azure Active Directory?](../../active-directory/external-identities/b2b-quickstart-add-guest-users-portal.md#accept-the-invitation)
- [Was ist Azure Active Directory?](../../active-directory/fundamentals/active-directory-whatis.md)