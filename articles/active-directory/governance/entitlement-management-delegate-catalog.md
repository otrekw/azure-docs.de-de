---
title: Delegieren der Zugriffssteuerung an Katalogersteller in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Erfahren Sie, wie Sie die Zugriffssteuerung von IT-Administratoren an Katalogersteller und Projektmanager delegieren, damit sie den Zugriff selbst verwalten können.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/18/2020
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02f44543f78789284e7b8d39471d1346ca7b8e74
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85078375"
---
# <a name="delegate-access-governance-to-catalog-creators-in-azure-ad-entitlement-management"></a>Delegieren der Zugriffssteuerung an Katalogersteller in der Azure AD-Berechtigungsverwaltung

Ein Katalog ist ein Container für Ressourcen und Zugriffspakete. Sie erstellen einen Katalog, wenn Sie zugehörige Ressourcen und Zugriffspakete gruppieren möchten. Standardmäßig kann ein globaler Administrator oder ein Benutzeradministrator [einen Katalog erstellen](entitlement-management-catalog-create.md) und zusätzliche Benutzer als Katalogbesitzer hinzufügen.

Wenn Sie an Benutzer delegieren möchten, die keine Administratoren sind, damit sie ihre eigenen Kataloge erstellen können, fügen Sie diese Benutzer der in der Azure AD-Berechtigungsverwaltung definierten Rolle Katalogersteller hinzu. Sie können einzelne Benutzer oder eine ganze Gruppe hinzufügen, deren Mitglieder dann Kataloge erstellen können.  Nach dem Erstellen eines Katalogs können sie ihrem Katalog dann die Ressourcen hinzufügen, die sie besitzen.

## <a name="as-an-it-administrator-delegate-to-a-catalog-creator"></a>Delegieren an einen Katalogersteller als IT-Administrator

Führen Sie diese Schritte aus, um einen Benutzer der Katalogerstellerrolle zuzuordnen.

**Erforderliche Rolle:** Globaler Administrator oder Benutzeradministrator

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im linken Menü im Abschnitt **Berechtigungsverwaltung** auf **Einstellungen**.

1. Klicken Sie auf **Bearbeiten**.

    ![Einstellungen zum Hinzufügen von Katalogerstellern](./media/entitlement-management-delegate-catalog/settings-delegate.png)

1. Klicken Sie im Abschnitt **Berechtigungsverwaltung delegieren** auf **Katalogersteller hinzufügen**, um die Benutzer oder Gruppen auszuwählen, an die Sie die Berechtigungsverwaltungsrolle delegieren möchten.

1. Klicken Sie auf **Auswählen**.

1. Klicken Sie auf **Speichern**.

## <a name="allow-delegated-roles-to-access-the-azure-portal"></a>Gewähren des Zugriffs delegierter Rollen auf das Azure-Portal

Wenn Sie delegierten Rollen wie (z. B. Katalogerstellern und Zugriffspaket-Managern), den Zugriff auf das Azure-Portal zum Verwalten von Zugriffspaketen gestatten möchten, sollten Sie die Einstellung im Verwaltungsportal prüfen.

**Erforderliche Rolle:** Globaler Administrator oder Benutzeradministrator

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Benutzer**.

1. Klicken Sie im linken Menü auf **Benutzereinstellungen**.

1. Vergewissern Sie sich, dass **Zugriff auf Azure AD-Verwaltungsportal einschränken** auf **Nein** festgelegt ist.

    ![Azure AD-Benutzereinstellungen – Verwaltungsportal](./media/entitlement-management-delegate-catalog/user-settings.png)

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen und Verwalten eines Katalogs von Ressourcen](entitlement-management-catalog-create.md)
- [Delegieren der Zugriffssteuerung an Zugriffspaket-Manager](entitlement-management-delegate-managers.md)

