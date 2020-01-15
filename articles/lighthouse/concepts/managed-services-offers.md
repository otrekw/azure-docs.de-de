---
title: Angebote für verwaltete Dienste in Azure Marketplace
description: Angebote für verwaltete Dienste gestatten Dienstanbietern, Ressourcenverwaltungsangebote an Kunden in Azure Marketplace zu verkaufen.
ms.date: 12/16/2019
ms.topic: conceptual
ms.openlocfilehash: 1b4f0d7457a74afe710a48f429cfe47535a9b122
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453585"
---
# <a name="managed-services-offers-in-azure-marketplace"></a>Angebote für verwaltete Dienste in Azure Marketplace

Dieser Artikel beschreibt die den neuen Angebotstyp **Verwaltete Dienste** im [Azure Marketplace](https://azuremarketplace.microsoft.com). Angebote für verwaltete Dienste gestatten Ihnen, Kunden Ressourcenverwaltungsdienste mit delegierter Azure-Ressourcenverwaltung anzubieten. Sie können diese Angebote für alle potenziellen Kunden oder nur für einen oder mehrere bestimmte Kunden verfügbar machen. Da Sie Kunden die Gebühren, die im Zusammenhang mit diesen verwalteten Diensten stehen, direkt in Rechnung stellen, werden von Microsoft keine Gebühren erhoben.

## <a name="understand-managed-services-offers"></a>Grundlegendes zur Angeboten für verwaltete Dienste

Angebote für verwaltete Dienste optimieren den Prozess des Onboardings von Kunden für die delegierte Azure-Ressourcenverwaltung. Wenn ein Kunde ein Angebot im Azure Marketplace kauft, kann er angeben, welche Abonnements und/oder Ressourcengruppen integriert werden sollen. Jedes Abonnement muss zunächst für das Onboarding autorisiert werden, indem der Ressourcenanbieter **Microsoft.ManagedServices** manuell registriert wird.

Danach können Benutzer in Ihrer Organisation entsprechend dem Zugriff, den Sie beim Erstellen des Angebots im [Cloud-Partnerportal](https://cloudpartner.azure.com/) festgelegt haben, Verwaltungsaufgaben für diese Ressourcen im Mandanten Ihrer Organisation ausführen. Dazu wird ein Manifest verwendet, das die Azure AD-Benutzer, -Gruppen und -Dienstprinzipale, die mithilfe der delegierten Azure-Ressourcenverwaltung Zugriff auf Kundenressourcen erhalten, sowie die Rollen für ihre Zugriffsebene angibt. Dadurch, dass Sie einer Azure AD-Gruppe Berechtigungen anstelle einer Reihe einzelner Benutzer- oder Anwendungskonten zuweisen, können Sie einzelne Benutzer hinzufügen oder entfernen, wenn sich Ihre Zugriffsanforderungen ändern.

## <a name="public-and-private-offers"></a>Öffentliche und private Angebote

Jedes Angebot verwalteter Dienste umfasst mindestens einen Plan. Pläne können entweder privat oder öffentlich sein. 

Wenn Sie Ihr Angebot auf bestimmte Kunden beschränken möchten, können Sie einen privaten Plan veröffentlichen. Wenn Sie dies tun, kann der Plan nur für die speziellen, von Ihnen angegebenen Abonnement-IDs erworben werden. Weitere Informationen finden Sie unter [Private Angebote](../../marketplace/private-offers.md).

Mit öffentlichen Plänen können Sie Ihre Dienste bei neuen Kunden bewerben. Diese sind normalerweise besser geeignet, wenn Sie nur eingeschränkten Zugriff auf den Mandanten des Kunden benötigen. Nachdem Sie eine Beziehung zu einem Kunden aufgebaut haben, können Sie, wenn dieser sich entschließt, Ihrer Organisation zusätzlichen Zugriff zu gewähren, dies entweder durch Veröffentlichen eines neuen privaten Plans ausschließlich für diesen Kunden oder durch [Onboarding des Kunden für den weiteren Zugriff mithilfe von Azure Resource Manager-Vorlagen](../how-to/onboard-customer.md) erreichen.

Gegebenenfalls können Sie sowohl öffentliche als auch private Pläne in dasselbe Angebot aufnehmen.

> [!IMPORTANT]
> Nachdem ein Plan als öffentlich veröffentlicht wurde, kann er nicht mehr in privat geändert werden. Um zu steuern, welche Kunden Ihr Angebot annehmen und Ressourcen delegieren können, verwenden Sie einen privaten Plan. Mit einem öffentlichen Plan können Sie die Verfügbarkeit nicht auf bestimmte Kunden oder auf eine bestimmte Anzahl von Kunden beschränken, obgleich Sie den Verkauf des Plans vollständig beenden können, wenn Sie sich dafür entscheiden. Es gibt zurzeit keinen Mechanismus zum Ablehnen oder Entfernen von Delegierungen, wenn ein Kunde ein Angebot annimmt. Sie können sich jedoch jederzeit an den jeweiligen Kunden wenden und ihn bitten, [Ihren Zugriff zu entfernen](../how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers).

## <a name="publish-managed-service-offers"></a>Veröffentlichen von Angeboten für verwaltete Dienste

Informationen zum Veröffentlichen eines Angebots für verwaltete Dienste finden Sie unter [Veröffentlichen eines Angebots für verwaltete Dienste im Azure Marketplace](../how-to/publish-managed-services-offers.md). Allgemeine Informationen zum Veröffentlichen im Azure Marketplace mithilfe des Cloud-Partnerportals finden Sie unter [Veröffentlichungsleitfaden für Azure Marketplace und AppSource](../../marketplace/marketplace-publishers-guide.md) und [Verwalten von Angeboten im Azure Marketplace und im AppSource-Marketplace](../../marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Delegierte Azure-Ressourcenverwaltung](azure-delegated-resource-management.md) und [Mandantenübergreifende Verwaltungserfahrungen](cross-tenant-management-experience.md).
- [Veröffentlichen von Angeboten für verwaltete Dienste](../how-to/publish-managed-services-offers.md) im Azure Marketplace.
