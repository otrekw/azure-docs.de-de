---
title: Angebote für verwaltete Dienste im Azure Marketplace
description: Angebote für verwaltete Dienste gestatten es Dienstanbietern, Ressourcenverwaltungsangebote an Kunden in Azure Marketplace zu verkaufen.
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: a0670bd74971132dcf243736bdf882a00154a942
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80672422"
---
# <a name="managed-service-offers-in-azure-marketplace"></a>Angebote für verwaltete Dienste im Azure Marketplace

In diesem Artikel wird der neue Angebotstyp **Verwaltete Dienste** im [Azure Marketplace](https://azuremarketplace.microsoft.com) beschrieben. Angebote für verwaltete Dienste gestatten Ihnen, Kunden Ressourcenverwaltungsdienste mit [delegierter Azure-Ressourcenverwaltung](azure-delegated-resource-management.md) anzubieten. Sie können diese Angebote für alle potenziellen Kunden oder nur für einen oder mehrere bestimmte Kunden verfügbar machen. Da Sie Kunden die Gebühren, die im Zusammenhang mit diesen verwalteten Diensten stehen, direkt in Rechnung stellen, werden von Microsoft keine Gebühren erhoben.

## <a name="understand-managed-service-offers"></a>Grundlegendes zu Angeboten für verwaltete Dienste

Angebote für verwaltete Dienste optimieren den Prozess des Onboardings von Kunden für die delegierte Azure-Ressourcenverwaltung. Wenn ein Kunde ein Angebot im Azure Marketplace kauft, kann er angeben, welche Abonnements und/oder Ressourcengruppen integriert werden sollen.

Danach können Benutzer in Ihrer Organisation entsprechend dem Zugriff, den Sie beim Erstellen des Angebots im [Cloud-Partnerportal](https://cloudpartner.azure.com/) festgelegt haben, mit diesen Ressourcen im Mandanten Ihrer Organisation arbeiten. Dazu wird ein Manifest verwendet, das die Azure AD-Benutzer, -Gruppen und -Dienstprinzipale, die mithilfe der delegierten Azure-Ressourcenverwaltung Zugriff auf Kundenressourcen erhalten, sowie die Rollen für ihre Zugriffsebene angibt. Dadurch, dass Sie einer Azure AD-Gruppe Berechtigungen anstelle einer Reihe einzelner Benutzer- oder Anwendungskonten zuweisen, können Sie einzelne Benutzer hinzufügen oder entfernen, wenn sich Ihre Zugriffsanforderungen ändern.

## <a name="public-and-private-offers"></a>Öffentliche und private Angebote

Jedes Angebot verwalteter Dienste umfasst mindestens einen Plan. Pläne können entweder privat oder öffentlich sein.

Wenn Sie Ihr Angebot auf bestimmte Kunden beschränken möchten, können Sie einen privaten Plan veröffentlichen. Wenn Sie dies tun, kann der Plan nur für die speziellen, von Ihnen angegebenen Abonnement-IDs erworben werden. Weitere Informationen finden Sie unter [Private Angebote](../../marketplace/private-offers.md).

Mit öffentlichen Plänen können Sie Ihre Dienste bei neuen Kunden bewerben. Diese sind normalerweise besser geeignet, wenn Sie nur eingeschränkten Zugriff auf den Mandanten des Kunden benötigen. Nachdem Sie eine Beziehung zu einem Kunden aufgebaut haben, können Sie, wenn dieser sich entschließt, Ihrer Organisation zusätzlichen Zugriff zu gewähren, dies entweder durch Veröffentlichen eines neuen privaten Plans ausschließlich für diesen Kunden oder durch [Onboarding des Kunden für den weiteren Zugriff mithilfe von Azure Resource Manager-Vorlagen](../how-to/onboard-customer.md) erreichen.

Gegebenenfalls können Sie sowohl öffentliche als auch private Pläne in dasselbe Angebot aufnehmen.

> [!IMPORTANT]
> Nachdem ein Plan als öffentlich veröffentlicht wurde, kann er nicht mehr in privat geändert werden. Um zu steuern, welche Kunden Ihr Angebot annehmen und Ressourcen delegieren können, verwenden Sie einen privaten Plan. Mit einem öffentlichen Plan können Sie die Verfügbarkeit nicht auf bestimmte Kunden oder auf eine bestimmte Anzahl von Kunden beschränken, obgleich Sie den Verkauf des Plans vollständig beenden können, wenn Sie sich dafür entscheiden. Sie können den [Zugriff auf eine Delegierung nur entfernen](../how-to/onboard-customer.md#remove-access-to-a-delegation), nachdem ein Kunde ein Angebot akzeptiert hat, wenn Sie beim Veröffentlichen des Angebots eine **Autorisierung** mit der auf [Rolle zum Löschen der Registrierungszuweisung für verwaltete Dienste](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) festgelegten **Rollendefinition** eingeschlossen haben. Sie können sich auch an den Kunden wenden und diesen bitten, [Ihren Zugriff zu entfernen](../how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers).

## <a name="publish-managed-service-offers"></a>Veröffentlichen von Angeboten für verwaltete Dienste

Informationen zum Veröffentlichen eines Angebots für verwaltete Dienste finden Sie unter [Veröffentlichen eines Angebots für verwaltete Dienste im Azure Marketplace](../how-to/publish-managed-services-offers.md). Allgemeine Informationen zum Veröffentlichen im Azure Marketplace mithilfe des Cloudpartnerportals finden Sie im [Veröffentlichungsleitfaden für Azure Marketplace und AppSource](../../marketplace/marketplace-publishers-guide.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Delegierte Azure-Ressourcenverwaltung](azure-delegated-resource-management.md) und [Mandantenübergreifende Verwaltungserfahrungen](cross-tenant-management-experience.md).
- [Veröffentlichen von Angeboten für verwaltete Dienste](../how-to/publish-managed-services-offers.md) im Azure Marketplace.
