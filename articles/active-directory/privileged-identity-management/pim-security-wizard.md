---
title: Azure AD-Rollen „Ermittlung und Erkenntnisse (Vorschau)“ in Privileged Identity Management, früher Sicherheits-Assistent – Azure Active Directory
description: Mit „Ermittlung und Erkenntnisse“ (früher Sicherheits-Assistent) können Sie permanente Azure AD-Rollenzuweisungen mit Privileged Identity Management in Just-in-Time-Zuweisungen konvertieren.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 08/07/2020
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef1de7e98a9cb57f83b87589ceddedc3cdd80927
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "88005925"
---
# <a name="discovery-and-insights-preview-for-azure-ad-roles-formerly-security-wizard"></a>„Ermittlung und Erkenntnisse (Vorschau)“ für Azure AD-Rollen (früher Sicherheits-Assistent)

Wenn Sie mit Privileged Identity Management (PIM) in Ihrer Azure AD-Organisation (Azure Active Directory) beginnen, können Sie für die ersten Schritte die Seite **Ermittlung und Erkenntnisse (Vorschau)** nutzen. Diese Funktion zeigt Ihnen, wer in Ihrer Organisation privilegierten Rollen zugewiesen ist, und wie Sie mit PIM schnell permanente Rollenzuweisungen in Just-in-Time-Zuweisungen ändern können. Sie können die Änderungen ihrer permanenten privilegierten Rollenzuweisungen in **Ermittlung und Erkenntnisse (Vorschau)** anzeigen oder ändern. Dabei handelt es sich um ein Analyse- und Aktionstool.

## <a name="discovery-and-insights-preview"></a>Ermittlung und Erkenntnisse (Vorschau)

Bevor Ihre Organisation Privileged Identity Management einsetzte, waren alle Rollenzuweisungen permanent. Benutzer befinden sich immer in den zugewiesenen Rollen, auch wenn sie deren Berechtigungen nicht benötigen. „Ermittlung und Erkenntnisse (Vorschau)“ ersetzt den früheren Sicherheits-Assistenten und zeigt Ihnen eine Liste privilegierter Rollen an sowie die Anzahl der Benutzer, die derzeit über diese Rollen verfügen. Sie können die Zuweisungen für eine Rolle auflisten, um mehr über die zugewiesenen Benutzer zu erfahren, wenn einer oder mehrere davon nicht vertraut sind.

:heavy_check_mark: **Microsoft empfiehlt** Ihnen, zwei Konten für den Notfallzugriff zu verwalten, die dauerhaft der Rolle „globaler Administrator“ zugewiesen sind, für die bei der Anmeldung keine Multi-Factor Authentication erforderlich ist. Sie können diese in jedem Notfallzugriffsszenario oder in Fällen verwenden, in denen niemand die Rolle „globaler Administrator“ einnehmen kann.

Verwenden Sie außerdem permanente Rollenzuweisungen, wenn ein Benutzer über ein Microsoft-Konto verfügt (also ein Konto zum Anmelden bei Microsoft-Diensten wie Skype oder Outlook.com). Wenn Sie in einer solchen Rolle die Aktivierung der Multi-Factor Authentication fordern, wird der Benutzer gesperrt.

## <a name="open-discovery-and-insights-preview"></a>Öffnen von „Ermittlung und Erkenntnisse (Vorschau)“

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure AD-Rollen** und dann **Ermittlung und Erkenntnisse (Vorschau)** aus. Beim Öffnen der Seite beginnt der Ermittlungsvorgang, relevante Rollenzuweisungen zu suchen.

    ![Azure AD-Rollen – Seite „Ermittlung und Erkenntnisse“ mit den drei Optionen](./media/pim-security-wizard/new-preview-link.png)

1. Wählen Sie **Anzahl globaler Administratoren verringern**.

    ![Anzahl globaler Administratoren verringern – Rollenbereich mit allen Mitgliedern](./media/pim-security-wizard/new-preview-page.png)

1. Untersuchen Sie die Liste mit den Rollenzuweisungen „Globaler Administrator“.

    ![Anzahl globaler Administratoren verringern – Rollenbereich mit allen Mitgliedern](./media/pim-security-wizard/new-global-administrator-list.png)

1. Wählen Sie **Weiter** aus, um die Benutzer oder Gruppen auszuwählen, die Sie berechtigen möchten, und wählen Sie dann **Als berechtigt festlegen** oder **Zuweisung entfernen** aus.

    ![Seite „Mitglieder in berechtigte Mitglieder konvertieren“ mit Optionen zum Auswählen von Mitgliedern, die in berechtigte Mitglieder für Rollen umgewandelt werden sollen](./media/pim-security-wizard/new-global-administrator-buttons.png)

1. Sie können auch verlangen, dass alle globalen Administratoren ihren eigenen Zugriff überprüfen.

    ![Seite „Globale Administratoren“ mit Zugriffsüberprüfungsabschnitt](./media/pim-security-wizard/new-global-administrator-access-review.png)

1. Nachdem Sie eine dieser Änderungen ausgewählt haben, wird eine Azure-Benachrichtigung angezeigt.

1. Sie können dann **Dauerhaften Zugriff entfernen** oder **Dienstprinzipale überprüfen** auswählen, um die oben genannten Schritte für andere privilegierte Rollen und Dienstprinzipal-Rollenzuweisungen zu wiederholen. Für Dienstprinzipal-Rollenzuweisungen können Sie nur Rollenzuweisungen entfernen.

    ![Zusätzliche Erkenntnisoptionen, um den dauerhaften Zugriff zu entfernen und Dienstprinzipale zu überprüfen ](./media/pim-security-wizard/new-preview-page-service-principals.png)

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure AD-Rollen in PIM](pim-how-to-add-role-to-user.md)
- [Anderen Administratoren Zugriff für die Verwaltung von Privileged Identity Management gewähren](pim-how-to-give-access-to-pim.md).
