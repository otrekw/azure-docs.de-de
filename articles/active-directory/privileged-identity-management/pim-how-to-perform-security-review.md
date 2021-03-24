---
title: Überprüfen des Zugriffs auf Azure AD-Rollen in PIM – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Zugriff auf Azure Active Directory-Rollen in Azure AD Privileged Identity Management (PIM) überprüfen.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 04/24/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4dd0b63e08f44167b89b6e2d491722f3708b4036
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90055434"
---
# <a name="review-access-to-azure-ad-roles-in-privileged-identity-management"></a>Überprüfen des Zugriffs auf Azure AD-Rollen in Privileged Identity Management

Privileged Identity Management (PIM) vereinfacht die Art und Weise, in der Unternehmen den privilegierten Zugriff auf Ressourcen in Azure Active Directory (Azure AD) und anderen Microsoft-Onlinediensten wie Microsoft 365 oder Microsoft Intune verwalten. Führen Sie die in diesem Artikel beschriebenen Schritte aus, um die Ihnen zugewiesenen Rollen erfolgreich selbst zu überprüfen.

Wenn Ihnen eine Administratorrolle zugewiesen wurde, werden Sie vom Administrator für privilegierte Rollen Ihrer Organisation möglicherweise gebeten, regelmäßig zu bestätigen, dass Sie diese Rolle für Ihre Aufgaben benötigen. Sie erhalten möglicherweise eine E-Mail mit einem Link, oder Sie können direkt zum [Azure-Portal](https://portal.azure.com) navigieren und mit der Arbeit beginnen.

Wenn Sie Administrator für privilegierte Rollen oder globaler Administrator sind und sich für Zugriffsüberprüfungen interessieren, erhalten Sie weitere Informationen unter [Starten einer Zugriffsüberprüfung](pim-how-to-start-security-review.md).

## <a name="add-a-pim-dashboard-tile"></a>Hinzufügen einer PIM-Dashboardkachel

Wenn Sie den Privileged Identity Management-Dienst nicht im Azure-Portal an Ihr Dashboard angeheftet haben, führen Sie die folgenden Schritte aus, um zu beginnen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie in der rechten oberen Ecke des Azure-Portals Ihren Benutzernamen und die Azure AD-Organisation aus, in der Sie arbeiten möchten.
3. Wählen Sie **Alle Dienste** aus, und verwenden Sie das Textfeld „Filter“, um nach **Azure AD Privileged Identity Management** zu suchen.
4. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie dann auf **Erstellen**. Die Anwendung Privileged Identity Management wird geöffnet.

## <a name="approve-or-deny-access"></a>Genehmigen oder Verweigern des Zugriffs

Wenn Sie Zugriff genehmigen oder verweigern, teilen Sie dem Prüfer lediglich mit, ob Sie diese Rolle weiterhin verwenden. Wählen Sie **Genehmigen** aus, wenn Sie in der Rolle bleiben möchten, oder **Verweigern**, wenn Sie den Zugriff nicht mehr benötigen. Ihr Status ändert sich nicht sofort, sondern erst, wenn der Prüfer die Ergebnisse angewendet hat.
Gehen Sie wie folgt vor, um die Zugriffsüberprüfung zu finden und abzuschließen:

1. Wählen Sie im Privileged Identity Management-Dienst die Option **Privilegierten Zugriff überprüfen** aus. Wenn Zugriffsüberprüfungen ausstehen, werden diese in Azure AD auf der Seite **Zugriffsüberprüfungen** angezeigt.
2. Wählen Sie die Überprüfung aus, die Sie abschließen möchten.
3. Wenn Sie die Überprüfung nicht erstellt haben, werden Sie als der einzige Benutzer in der Überprüfung angezeigt. Wählen Sie das Kontrollkästchen neben Ihrem Namen.
4. Wählen Sie entweder **Genehmigen** oder **Verweigern** aus. Möglicherweise müssen Sie einen Grund für Ihre Entscheidung im Textfeld **Grund angeben** eingeben.  
5. Schließen Sie das Blatt **Azure AD-Rollen überprüfen** .

## <a name="next-steps"></a>Nächste Schritte

- [Ausführen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM](pim-resource-roles-perform-access-review.md)
