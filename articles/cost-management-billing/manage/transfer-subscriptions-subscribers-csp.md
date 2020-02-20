---
title: Übertragen von Azure-Abonnements zwischen Abonnenten und CSPs
description: Erfahren Sie, wie Azure-Abonnements zwischen Abonnenten und CSPs übertragen werden können.
author: bandersmsft
ms.reviewer: dhgandhi
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: banders
ms.openlocfilehash: 4e7e9ea61d74eb38f3c225d66a39906bc18fe6d2
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77200519"
---
# <a name="transfer-azure-subscriptions-between-subscribers-and-csps"></a>Übertragen von Azure-Abonnements zwischen Abonnenten und CSPs

Dieser Artikel enthält allgemeine Schritte zum Übertragen von Azure-Abonnements zwischen CSP-Partnern (Cloud Solution Provider) und ihren Kunden.

## <a name="transfer-ea-subscriptions"></a>Übertragen von EA-Abonnements

CSP-Partner mit direkter Abrechnung, die als [Azure Expert Managed Services Provider (MSP)](https://partner.microsoft.com/membership/azure-expert-msp) zertifiziert sind, können die Übertragung von Azure-Abonnements für ihre Kunden anfordern, die über ein direktes Enterprise Agreement (EA) verfügen. Abonnementübertragungen sind nur für Kunden zulässig, die eine Microsoft-Kundenvereinbarung (Microsoft Customer Agreement, MCA) akzeptiert und einen Azure-Plan erworben haben.

Wenn die Anforderung genehmigt wird, kann der CSP seinen Kunden eine kombinierte Rechnung stellen. Weitere Informationen zum Übertragen von Abonnements durch CSPs finden Sie unter [Übernehmen des Abrechnungsbesitzes für Azure-Abonnements für Ihr MPA-Konto](mpa-request-ownership.md).

## <a name="other-subscription-transfers-to-a-csp-partner"></a>Übertragungen anderer Abonnements an einen CSP-Partner

Um andere Azure-Abonnements an einen CSP-Partner zu übertragen, muss der Abonnent Ressourcen aus Quellabonnements in CSP-Abonnements verschieben. Verwenden Sie die folgenden Anleitungen, um Ressourcen zwischen Abonnements zu verschieben.

1. Arbeiten Sie mit Ihrem CSP-Partner zusammen, um Azure CSP-Zielabonnements zu erstellen.
1. Stellen Sie sicher, dass sich Quell- und Zielabonnement des CSP in demselben Azure AD-Mandanten (Azure Active Directory) befinden.  
    Der Azure AD-Mandant für ein Azure CSP-Abonnement kann nicht geändert werden. Stattdessen müssen Sie das Quellabonnement dem Azure AD-Mandanten des CSP hinzufügen oder zuordnen. Weitere Informationen finden Sie unter [Zuordnen oder Hinzufügen eines Azure-Abonnements zu Ihrem Azure Active Directory-Mandanten](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).
    > [!IMPORTANT]
    > - Wenn Sie ein Abonnement einem anderen Azure AD-Verzeichnis zuordnen, verlieren Benutzer, denen Rollen mit der [rollenbasierten Zugriffssteuerung (RBAC)](../../role-based-access-control/role-assignments-portal.md) zugewiesen wurden, ihren Zugriff. Klassische Abonnementadministratoren (Dienstadministrator und Co-Admin) verlieren ebenfalls ihren Zugriff.
    > - Richtlinienzuweisungen werden auch aus einem Abonnement entfernt, wenn das Abonnement einem anderen Verzeichnis zugeordnet ist.
1. Das Benutzerkonto, das Sie für die Übertragung verwenden, muss für beide Abonnements über [RBAC](add-change-subscription-administrator.md)-Besitzerzugriff verfügen.
1. [Überprüfen](/rest/api/resources/resources/validatemoveresources) Sie zu Beginn, ob alle Azure-Ressourcen aus dem Quellabonnement in das Zielabonnement verschoben werden können.  
    Einige Azure-Ressourcen können nicht zwischen Abonnements verschoben werden. Die komplette Liste der Azure-Ressourcen, die verschoben werden können, finden Sie unter [Unterstützung des Verschiebevorgangs für Ressourcen](../../azure-resource-manager/management/move-support-resources.md).
    > [!IMPORTANT]
    >  - Azure CSP unterstützt nur Azure Resource Manager-Ressourcen. Wenn Azure-Ressourcen im Quellabonnement mit dem klassischen Azure-Bereitstellungsmodell erstellt wurden, müssen diese zu [Azure Resource Manager](https://docs.microsoft.com/azure/cloud-solution-provider/migration/ea-payg-to-azure-csp/ea-open-direct-asm-to-arm) migriert werden, damit sie verschoben werden können. Sie müssen ein Partner sein, um die Webseite anzeigen zu können.

1. Stellen Sie sicher, dass alle Quellabonnementdienste das Azure Resource Manager-Modell verwenden. Übertragen Sie dann Ressourcen aus dem Quellabonnement durch [Azure-Ressourcenverschiebung](../../azure-resource-manager/management/move-resource-group-and-subscription.md) in das Zielabonnement.
    > [!IMPORTANT]
    >  - Das Verschieben von Azure-Ressourcen zwischen Abonnements kann je nach den in den Abonnements enthaltenen Ressourcen zu Dienstdowntime führen.

## <a name="all-subscription-transfers-from-a-csp-partner"></a>Alle Abonnementübertragungen von einem CSP-Partner

Um andere Abonnements von einem CSP-Partner auf ein anderes Azure-Angebot zu übertragen, muss der Abonnent Ressourcen zwischen CSP-Quellabonnements und Zielabonnements verschieben.

1. Erstellen Sie Azure-Zielabonnements.
1. Stellen Sie sicher, dass sich Quell- und Zielabonnement im gleichen Azure Active Directory-Mandanten (Azure AD) befinden. Weitere Informationen zum Ändern eines Azure AD-Mandanten finden Sie unter [Zuordnen oder Hinzufügen eines Azure-Abonnements zu Ihrem Azure Active Directory-Mandanten](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

    > [!IMPORTANT]
    >  - Wenn Sie ein Abonnement einem anderen Verzeichnis zuordnen, verlieren Benutzer, denen Rollen über die [RBAC](../../role-based-access-control/role-assignments-portal.md) zugewiesen wurden, ihren Zugriff. Klassische Abonnementadministratoren (Dienstadministrator und Co-Admin) verlieren ebenfalls ihren Zugriff.
    >  - Richtlinienzuweisungen werden auch aus einem Abonnement entfernt, wenn das Abonnement einem anderen Verzeichnis zugeordnet ist.

1. Das Benutzerkonto, das Sie für die Übertragung verwenden, muss für beide Abonnements über [RBAC](add-change-subscription-administrator.md)-Besitzerzugriff verfügen.
1. [Überprüfen](/rest/api/resources/resources/validatemoveresources) Sie zu Beginn, ob alle Azure-Ressourcen aus dem Quellabonnement in das Zielabonnement verschoben werden können.
    > [!IMPORTANT]
    >  - Einige Azure-Ressourcen können nicht zwischen Abonnements verschoben werden. Die komplette Liste der Azure-Ressourcen, die verschoben werden können, finden Sie unter [Unterstützung des Verschiebevorgangs für Ressourcen](../../azure-resource-manager/management/move-support-resources.md).

1. Übertragen Sie Ressourcen aus dem Quellabonnement durch [Azure-Ressourcenverschiebung](../../azure-resource-manager/management/move-resource-group-and-subscription.md) in das Zielabonnement.
    > [!IMPORTANT]
    >  - Das Verschieben von Azure-Ressourcen zwischen Abonnements kann je nach den in den Abonnements enthaltenen Ressourcen zu Dienstdowntime führen.

## <a name="next-steps"></a>Nächste Schritte
- [Übernehmen des Abrechnungsbesitzes für Azure-Abonnements für Ihr MPA-Konto](mpa-request-ownership.md)
- [Verwalten von Konten und Abonnements mit der Azure-Abrechnung](index.yml)
