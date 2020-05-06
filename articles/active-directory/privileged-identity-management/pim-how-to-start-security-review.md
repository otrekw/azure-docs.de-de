---
title: Erstellen einer Zugriffsüberprüfung für Azure AD-Rollen in PIM – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Zugriffsüberprüfung für Azure AD-Rollen in Azure AD Privileged Identity Management (PIM) erstellen.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 10/22/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b4c8c1a1f846d8d04d7758c8d7bdcd932ac3c90
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82185716"
---
# <a name="create-an-access-review-of-azure-ad-roles-in-privileged-identity-management"></a>Erstellen einer Zugriffsüberprüfung für Azure AD-Rollen in Privileged Identity Management

Daher sollten Sie den Zugriff in regelmäßigen Abständen überprüfen, um das mit veralteten Rollenzuweisungen verbundene Risiko zu verringern. Mit Azure AD Privileged Identity Management (PIM) können Sie Zugriffsüberprüfungen für privilegierte Azure AD-Rollen erstellen. Sie können auch wiederholte Zugriffsüberprüfungen konfigurieren, die automatisch ausgeführt werden.

In diesem Artikel wird beschrieben, wie Sie eine oder mehrere Zugriffsüberprüfungen für privilegierte Azure AD-Rollen erstellen.

## <a name="prerequisites"></a>Voraussetzungen

[Administrator für privilegierte Rollen](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Öffnen von Zugriffsüberprüfungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als Benutzer an, der Mitglied der Rolle „Administrator für privilegierte Rollen“ ist.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure AD-Rollen** aus.

1. Wählen Sie unter „Verwalten“ die Option **Zugriffsüberprüfungen** und anschließend **Neu** aus.

    ![Azure AD-Rollen: Liste der Zugriffsüberprüfungen mit dem Status aller Überprüfungen](./media/pim-how-to-start-security-review/access-reviews.png)

[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]

## <a name="start-the-access-review"></a>Starten der Zugriffsüberprüfung

Wählen Sie nach dem Festlegen der Einstellungen für eine Zugriffsüberprüfung die Option **Starten** aus. Die Zugriffsüberprüfung wird in der Liste mit einer Angabe des Status angezeigt.

![Liste der Zugriffsüberprüfungen mit dem Status gestarteter Überprüfungen](./media/pim-how-to-start-security-review/access-reviews-list.png)

Standardmäßig sendet Azure AD kurz nach dem Start der Überprüfung eine E-Mail an die Prüfer. Wenn Sie nicht möchten, dass Azure AD die E-Mail sendet, stellen Sie sicher, dass die Prüfer darüber in Kenntnis gesetzt werden, dass sie eine ausstehende Zugriffsüberprüfung abschließen müssen. Sie können ihnen die Anweisungen zum [Durchführen einer Zugriffsüberprüfung für Azure AD-Rollen in PIM](pim-how-to-perform-security-review.md) zeigen.

## <a name="manage-the-access-review"></a>Verwalten der Zugriffsüberprüfung

Sie können den Fortschritt der Überprüfungen durch die Prüfer auf der Seite **Übersicht** der Zugriffsüberprüfung nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn die [Überprüfung abgeschlossen](pim-how-to-complete-review.md) ist.

![Übersichtsseite der Zugriffsüberprüfung mit den Details der Überprüfung](./media/pim-how-to-start-security-review/access-review-overview.png)

Führen Sie bei einer einmaligen Überprüfung nach Ablauf des Zeitraums für die Zugriffsüberprüfung oder nach Beenden der Zugriffsüberprüfung durch den Administrator die Schritte unter [Abschließen einer Zugriffsüberprüfung für Azure AD-Rollen in PIM](pim-how-to-complete-review.md) aus, um die Ergebnisse anzuzeigen und anzuwenden.  

Um eine Serie von Zugriffsüberprüfungen zu verwalten, navigieren Sie zur Zugriffsüberprüfung. Dort finden Sie unter den geplanten Überprüfungen die anstehenden Überprüfungen, und Sie können das Enddatum bearbeiten oder Prüfer entsprechend hinzufügen/entfernen.

Basierend auf Ihrer Auswahl unter **Einstellungen nach Abschluss** wird nach dem Enddatum der Überprüfung oder bei manueller Beendigung der Überprüfung die automatische Anwendung ausgeführt. Der Status der Überprüfung ändert sich von **Abgeschlossen** über Zwischenzustände wie **Wird angewandt** schließlich in den Status **Angewandt**. Erwartungsgemäß sollten abgelehnte Benutzer (sofern vorhanden) innerhalb weniger Minuten aus den Rollen entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen des Zugriffs auf Azure AD-Rollen](pim-how-to-perform-security-review.md)
- [Abschließen einer Zugriffsüberprüfung für Azure AD-Rollen](pim-how-to-complete-review.md)
- [Erstellen einer Zugriffsüberprüfung für Azure AD-Rollen](pim-resource-roles-start-access-review.md)
