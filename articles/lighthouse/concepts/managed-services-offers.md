---
title: Angebote für verwaltete Dienste im Azure Marketplace
description: Angebote für verwaltete Dienste gestatten es Dienstanbietern, Ressourcenverwaltungsangebote an Kunden in Azure Marketplace zu verkaufen.
ms.date: 07/28/2020
ms.topic: conceptual
ms.openlocfilehash: 4f3813b7e2adabe5caaf0c7a5cbe8b15ed084730
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87406859"
---
# <a name="managed-service-offers-in-azure-marketplace"></a>Angebote für verwaltete Dienste im Azure Marketplace

In diesem Artikel wird der neue Angebotstyp **Verwaltete Dienste** im [Azure Marketplace](https://azuremarketplace.microsoft.com) beschrieben. Angebote für verwaltete Dienste gestatten es Ihnen, Kunden Ressourcenverwaltungsdienste über [Azure Lighthouse](../overview.md) anzubieten. Sie können diese Angebote für alle potenziellen Kunden oder nur für einen oder mehrere bestimmte Kunden verfügbar machen. Da Sie Kunden die Gebühren, die im Zusammenhang mit diesen verwalteten Diensten stehen, direkt in Rechnung stellen, werden von Microsoft keine Gebühren erhoben.

## <a name="understand-managed-service-offers"></a>Grundlegendes zu Angeboten für verwaltete Dienste

Angebote für verwaltete Dienste optimieren den Prozess des Onboardings von Kunden für Azure Lighthouse. Wenn ein Kunde ein Angebot im Azure Marketplace kauft, kann er angeben, welche Abonnements und/oder Ressourcengruppen integriert werden sollen.

Danach können Benutzer in Ihrer Organisation entsprechend dem Zugriff, den Sie beim Erstellen des Angebots festgelegt haben, mithilfe der [delegierten Azure-Ressourcenverwaltung](azure-delegated-resource-management.md) mit diesen Ressourcen im Mandanten Ihrer Organisation arbeiten. Dazu wird ein Manifest verwendet, das die Azure Active Directory-Benutzer (Azure AD), -Gruppen und -Dienstprinzipale, die Zugriff auf Kundenressourcen erhalten, sowie die Rollen für ihre Zugriffsebene angibt. Dadurch, dass Sie einer Azure AD-Gruppe Berechtigungen anstelle einer Reihe einzelner Benutzer- oder Anwendungskonten zuweisen, können Sie einzelne Benutzer hinzufügen oder entfernen, wenn sich Ihre Zugriffsanforderungen ändern.

## <a name="public-and-private-offers"></a>Öffentliche und private Angebote

Jedes Angebot für verwaltete Dienste umfasst mindestens einen Plan. Pläne können entweder privat oder öffentlich sein.

Wenn Sie Ihr Angebot auf bestimmte Kunden beschränken möchten, können Sie einen privaten Plan veröffentlichen. In diesem Fall kann der Plan nur für die angegebenen Abonnement-IDs erworben werden. Weitere Informationen finden Sie unter [Private Angebote](../../marketplace/private-offers.md).

> [!NOTE]
> Private Angebote werden bei Abonnements, die über einen Handelspartner des Cloud Solution Provider-Programms (CSP) eingerichtet wurden, nicht unterstützt.

Mit öffentlichen Plänen können Sie Ihre Dienste bei neuen Kunden bewerben. Diese sind normalerweise besser geeignet, wenn Sie nur eingeschränkten Zugriff auf den Mandanten des Kunden benötigen. Nachdem Sie eine Beziehung zu einem Kunden aufgebaut haben, können Sie, wenn dieser sich entschließt, Ihrer Organisation zusätzlichen Zugriff zu gewähren, dies entweder durch Veröffentlichen eines neuen privaten Plans ausschließlich für diesen Kunden oder durch [Onboarding des Kunden für den weiteren Zugriff mithilfe von Azure Resource Manager-Vorlagen](../how-to/onboard-customer.md) erreichen.

Gegebenenfalls können Sie sowohl öffentliche als auch private Pläne in dasselbe Angebot aufnehmen.

> [!IMPORTANT]
> Nachdem ein Plan als öffentlich veröffentlicht wurde, kann er nicht mehr in privat geändert werden. Um zu steuern, welche Kunden Ihr Angebot annehmen und Ressourcen delegieren können, verwenden Sie einen privaten Plan. Mit einem öffentlichen Plan können Sie die Verfügbarkeit nicht auf bestimmte Kunden oder auf eine bestimmte Anzahl von Kunden beschränken, obgleich Sie den Verkauf des Plans vollständig beenden können, wenn Sie sich dafür entscheiden. Sie können den [Zugriff auf eine Delegierung nur entfernen](../how-to/remove-delegation.md), nachdem ein Kunde ein Angebot akzeptiert hat, wenn Sie beim Veröffentlichen des Angebots eine **Autorisierung** mit der auf [Rolle zum Löschen der Registrierungszuweisung für verwaltete Dienste](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) festgelegten **Rollendefinition** eingeschlossen haben. Sie können sich auch an den Kunden wenden und diesen bitten, [Ihren Zugriff zu entfernen](../how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers).

## <a name="publish-managed-service-offers"></a>Veröffentlichen von Angeboten für verwaltete Dienste

Informationen zum Veröffentlichen eines Angebots für verwaltete Dienste finden Sie unter [Veröffentlichen eines Angebots für verwaltete Dienste im Azure Marketplace](../how-to/publish-managed-services-offers.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Delegierte Azure-Ressourcenverwaltung](azure-delegated-resource-management.md) und [Mandantenübergreifende Verwaltungserfahrungen](cross-tenant-management-experience.md).
- [Veröffentlichen von Angeboten für verwaltete Dienste](../how-to/publish-managed-services-offers.md) in Azure Marketplace.
