---
title: Verwalten von Mandanten im kommerziellen Marketplace – Azure Marketplace
description: Erfahren Sie, wie Sie Mandanten für das Programm „Kommerzieller Marketplace“ in Partner Center verwalten.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: varsha-sarah
ms.author: vavargh
ms.custom: contperf-fy21q2
ms.date: 04/07/2021
ms.openlocfilehash: 446792b26527126a4db99da14a2585c17cf8610c
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107107967"
---
# <a name="manage-tenants-in-the-commercial-marketplace"></a>Verwalten von Mandanten im kommerziellen Marketplace

**Geeignete Rollen**

- Manager

Ein Azure AD-Mandant (Active Directory), in dieser Dokumentation auch als *Geschäftskonto* bezeichnet, spiegelt Ihre Organisationseinrichtung im Azure-Portal wider und hilft Ihnen, eine bestimmte Instanz der Microsoft-Clouddienste für Ihre internen und externen Benutzer zu verwalten. Wenn Ihre Organisation einen Microsoft-Clouddienst wie Azure, Microsoft Intune oder Microsoft 365 abonniert hat, wurde ein Azure AD-Mandant für Sie erstellt.

Sie können mehrere Mandanten für die Verwendung mit Partner Center einrichten. Wenn Ihr Unternehmen über mehrere Mandanten verfügt (z. B. „contoso.com“, „contoso.uk“ usw.), können Sie die zusätzlichen Mandanten hier verknüpfen. Mit dieser Zuordnung können Sie Benutzer aus den zusätzlichen Mandanten in Ihrem Konto im kommerziellen Marketplace hinzufügen und verwalten.

Jeder Benutzer mit der Manager-Rolle im Partner Center-Konto hat die Möglichkeit, Azure AD-Mandanten im Konto hinzuzufügen und zu entfernen.

## <a name="add-an-existing-tenant"></a>Hinzufügen eines vorhandenen Mandanten

So ordnen Sie einen anderen Azure AD-Mandanten zu Ihrem Partner Center-Konto hinzu

1. Wählen Sie rechts oben in Partner Center **Einstellungen** > **Kontoeinstellungen** aus.
1. Wählen Sie unter **Organisationsprofil** die Option **Mandanten** aus. Die aktuellen Mandantenzuordnungen werden angezeigt.
1. Wählen Sie auf der Registerkarte **Entwickler** die Option **Zuordnen** aus.
1. Geben Sie Ihre Azure AD-Anmeldeinformationen für den Mandanten ein, den Sie zuordnen möchten.
1. Überprüfen Sie die Organisation und den Domänennamen für Ihren Azure AD-Mandanten. Wählen Sie zum Abschließen der Zuordnung **Bestätigen** aus.

Wenn die Zuordnung erfolgreich ist, können Sie anschließend in Partner Center im Abschnitt Benutzer Kontobenutzer hinzufügen und verwalten. Weitere Informationen zum Hinzufügen von Benutzern finden Sie unter [Verwalten von Benutzern](add-manage-users.md).

## <a name="create-a-new-tenant"></a>Erstellen eines neuen Mandanten

So erstellen Sie einen neuen Azure AD-Mandanten mit Ihrem Partner Center-Konto

1. Wählen Sie rechts oben in Partner Center **Einstellungen** > **Kontoeinstellungen** aus.
1. Wählen Sie unter **Organisationsprofil** die Option **Mandanten** aus. Die aktuellen Mandantenzuordnungen werden angezeigt.
1. Wählen Sie auf der Registerkarte „Entwickler“ die Option **Erstellen** aus.
1. Geben Sie die Verzeichnisinformationen für das neue Azure AD ein:
    - **Domänenname**: Der eindeutige Name, der für die Azure AD-Domäne verwendet wird, zusammen mit „. onmicrosoft.com“. Wenn Sie beispielsweise „Beispiel“ eingegeben haben, würde Ihre Azure AD-Domäne „Beispiel.onmicrosoft.com“ lauten.
    - **Kontakt-E-Mail**: Eine E-Mail-Adresse, über die wir Sie bei Bedarf bezüglich Ihres Konto kontaktieren können.
    - **Benutzerkontoinformationen für Globaler Administrator**: Der Vorname, Nachname, Benutzername und das Kennwort, die Sie für das neue globale Administratorkonto verwenden möchten.
1. Wählen Sie Erstellen aus, um die neuen Domänen- und Kontoinformationen zu bestätigen.
1. Melden Sie sich mit Ihrem neuen Benutzernamen und Kennwort als globaler Azure AD-Administrator an, um [Benutzer hinzufügen und zu verwalten](add-manage-users.md).

Weitere Informationen zum Erstellen neuer Mandanten im Azure-Portal statt im Partner Center-Portal finden Sie im Artikel [Erstellen eines neuen Mandanten in Azure Active Directory](/azure/active-directory/fundamentals/active-directory-access-create-new-tenant).

## <a name="remove-a-tenant"></a>Entfernen eines Mandanten

Um einen Mandanten aus Ihrem Partner Center-Konto zu entfernen, suchen Sie dessen Namen auf der Seite **Mandanten** (unter **Kontoeinstellungen**), und wählen Sie dann **Entfernen** aus. Sie werden gefragt, ob Sie den Mandanten wirklich entfernen möchten. Anschließend kann sich kein Benutzer in diesem Mandanten mehr beim Partner Center-Konto anmelden, und alle Berechtigungen, die Sie für diese Benutzer konfiguriert haben, werden entfernt.

> [!TIP]
> Ein Mandant kann nicht entfernt werden, wenn Sie derzeit über ein Konto im betreffenden Mandanten in Partner Center angemeldet sind. Um einen Mandanten zu entfernen, müssen Sie sich in Partner Center als **Manager** für einen anderen Mandanten anmelden, der dem Konto zugeordnet ist. Wenn dem Konto nur ein einziger Mandant zugeordnet, kann dieser Mandant erst nach der Anmeldung bei dem Microsoft-Konto entfernt werden, das das Konto eröffnet hat.
