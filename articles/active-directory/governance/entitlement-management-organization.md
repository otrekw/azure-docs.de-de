---
title: Hinzufügen einer verbundenen Organisation in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Erfahren Sie, wie Sie Personen außerhalb Ihres Unternehmens das Anfordern von Zugriffspaketen gestatten, um eine gemeinsame Arbeit an Projekten zu ermöglichen.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee370bc9c381eb11ae7cae53b31d0c987c52733c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128607"
---
# <a name="add-a-connected-organization-in-azure-ad-entitlement-management"></a>Hinzufügen einer verbundenen Organisation in der Azure AD-Berechtigungsverwaltung

Dank der Berechtigungsverwaltung von Azure Active Directory (Azure AD) ist es möglich, mit Personen außerhalb Ihres Unternehmens zusammenzuarbeiten. Wenn Sie häufig mit Benutzern in einem externen Azure AD-Verzeichnis oder einer externen Domäne zusammenarbeiten, können Sie dieses Verzeichnis oder diese Domäne als verbundene Organisation hinzufügen. Dieser Artikel beschreibt, wie Sie eine verbundene Organisation hinzufügen, sodass Sie Benutzern außerhalb Ihrer Organisation das Anfordern von Ressourcen in Ihrem Verzeichnis gestatten können.

## <a name="what-is-a-connected-organization"></a>Was ist eine verbundene Organisation?

Eine verbundene Organisation ist ein externes Azure AD-Verzeichnis bzw. eine externe Domäne, mit der eine Beziehung besteht.

Angenommen, Sie arbeiten bei der Woodgrove Bank und möchten mit zwei externen Organisationen zusammenarbeiten. Diese beiden Organisationen verfügen über unterschiedliche Konfigurationen:

- Graphic Design Institute verwendet Azure AD, und die Benutzer haben einen Benutzerprinzipalnamen, der mit *graphicdesigninstitute.com* endet.
- Contoso verwendet Azure AD noch nicht. Contoso-Benutzer haben ein Benutzerprinzipalnamen, der mit *contoso.com* endet.

In diesem Fall können Sie zwei verbundene Organisationen konfigurieren. Sie erstellen eine verbundene Organisation für Graphic Design Institute und eine für Contoso. Wenn Sie diese beiden verbundenen Unternehmen anschließend zu einer Richtlinie hinzufügen, können Benutzer beider Organisationen, deren Benutzername der Richtlinie entspricht, Zugriffspakete anfordern. Benutzer mit einem Benutzerprinzipalnamen mit der Domäne *graphicdesigninstitute.com* entsprächen der mit Graphic Design Institute verbundenen Organisation und dürften Anfragen übermitteln. Benutzer mit einem Benutzerprinzipalnamen mit der Domäne *contoso.com* entsprächen der mit Contoso verbundenen Organisation und dürften ebenfalls Pakete anfordern. Da Graphic Design Institute Azure AD verwendet, könnten außerdem alle Benutzer mit einem Prinzipalnamen, der einer [verifizierten Domäne](../fundamentals/add-custom-domain.md#verify-your-custom-domain-name) entspricht, die zu ihrem Mandanten hinzugefügt wurde (z. B. *graphicdesigninstitute.example*), über dieselbe Richtlinie Zugriffspakete anfordern.

![Beispiel für eine verbundene Organisation](./media/entitlement-management-organization/connected-organization-example.png)

Wie sich Benutzer aus dem Azure AD-Verzeichnis oder der Domäne authentifizieren, hängt vom Authentifizierungstyp ab. Für verbundene Organisationen gibt es folgende Authentifizierungstypen :

- Azure AD
- [Direkter Verbund](../b2b/direct-federation.md)
- [Einmalkennung](../b2b/one-time-passcode.md) (Domäne)

Eine Demonstration, wie Sie eine verbundene Organisation hinzufügen können, finden Sie im folgenden Video:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4dskS]

## <a name="add-a-connected-organization"></a>Hinzufügen einer verbundenen Organisation

Führen Sie die Anweisungen in diesem Abschnitt aus, um ein externes Azure AD-Verzeichnis oder eine externe Domäne als verbundene Organisation hinzuzufügen.

**Erforderliche Rolle:** *Globaler Administrator*, *Benutzeradministrator* oder *Gasteinladender*

1. Wählen Sie im Azure-Portal die Option **Azure Active Directory** und dann **Identity Governance** aus.

1. Wählen Sie im linken Bereich **Verbundene Organisationen** und anschließend **Verbundene Organisation hinzufügen** aus.

    ![Schaltfläche „Verbundene Organisation hinzufügen“](./media/entitlement-management-organization/connected-organization.png)

1. Wählen Sie die Registerkarte **Grundlagen** aus, und geben Sie einen Anzeigenamen und eine Beschreibung für die Organisation ein.

    ![„Verbundene Organisation hinzufügen“, Bereich „Grundlagen“](./media/entitlement-management-organization/organization-basics.png)

1. Wählen Sie die Registerkarte **Verzeichnis + Domäne** und dann **Verzeichnis und Domäne hinzufügen** aus.

    Der Bereich **Verzeichnisse und Domänen auswählen** wird geöffnet.

1. Geben Sie im Suchfeld einen Domänennamen ein, um nach dem Azure AD-Verzeichnis oder der Domäne zu suchen. Stellen Sie sicher, dass Sie den vollständigen Domänennamen eingeben.

1. Vergewissern Sie sich, dass Organisationsname und Authentifizierungstyp richtig sind. Die Art der Benutzeranmeldung hängt vom Authentifizierungstyp ab.

    ![Bereich „Verzeichnisse und Domänen auswählen“](./media/entitlement-management-organization/organization-select-directories-domains.png)

1. Wählen Sie **Hinzufügen** aus, um das Azure AD-Verzeichnis oder die Domäne hinzuzufügen. Aktuell kann pro verbundener Organisation nur ein Azure AD-Verzeichnis oder eine Domäne hinzugefügt werden.

    > [!NOTE]
    > Alle Benutzer aus dem Azure AD-Verzeichnis oder der Domäne können Zugriffspakete anfordern. Dies schließt in Azure AD auch Benutzer aller Unterdomänen ein, die dem Verzeichnis zugeordnet sind, sofern diese Domänen nicht über die Zulassungs- oder Verweigerungsliste von Azure AD B2B (Business-to-Business) blockiert werden. Weitere Informationen finden Sie unter [Zulassen oder Blockieren von Einladungen für B2B-Benutzer von bestimmten Organisationen](../b2b/allow-deny-list.md).

1. Nachdem Sie das Azure AD-Verzeichnis oder die Domäne hinzugefügt haben, wählen Sie **Auswählen** aus.

    Die Organisation wird in der Liste angezeigt.

    ![Bereich „Verzeichnis und Domäne“](./media/entitlement-management-organization/organization-directory-domain.png)

1. Wählen Sie die Registerkarte **Sponsoren** aus, und fügen Sie optional Sponsoren für diese verbundene Organisation hinzu.

    Sponsoren sind interne oder externe Benutzer, die sich bereits in Ihrem Verzeichnis befinden und als Ansprechpartner für die Beziehung mit dieser verbundenen Organisation fungieren. Interne Sponsoren sind Mitgliederbenutzer in Ihrem Verzeichnis. Externe Sponsoren sind Gastbenutzer aus der verbundenen Organisation, die zuvor eingeladen wurden und sich bereits in Ihrem Verzeichnis befinden. Sponsoren können als genehmigende Personen fungieren, wenn Benutzer in dieser verbundenen Organisation Zugriff auf ein Zugriffspaket anfordern. Informationen zum Einladen eines Gastbenutzers zu Ihrem Verzeichnis finden Sie unter [Hinzufügen von Azure Active Directory B2B-Zusammenarbeitsbenutzern über das Azure-Portal](../b2b/add-users-administrator.md).

    Wenn Sie **Hinzufügen/Entfernen** auswählen, wird ein Bereich geöffnet, in dem Sie interne oder externe Sponsoren auswählen können. Der Bereich zeigt eine ungefilterte Liste der Benutzer und Gruppen in Ihrem Verzeichnis an.

    ![Bereich „Sponsoren“](./media/entitlement-management-organization/organization-sponsors.png)

1. Wählen Sie die Registerkarte **Überprüfen und erstellen** aus, überprüfen Sie Ihre Organisationseinstellungen, und wählen Sie dann **Erstellen** aus.

    ![Bereich „Überprüfen + erstellen“](./media/entitlement-management-organization/organization-review-create.png)

## <a name="update-a-connected-organization"></a>Aktualisieren einer verbundenen Organisation 

Wenn die verbundene Organisation in eine andere Domäne wechselt, sich der Name der Organisation ändert, oder wenn Sie die Sponsoren ändern möchten, können Sie die verbundene Organisation aktualisieren. Führen Sie dazu die Anweisungen in diesem Abschnitt aus.

**Erforderliche Rolle:** *Globaler Administrator*, *Benutzeradministrator* oder *Gasteinladender*

1. Wählen Sie im Azure-Portal die Option **Azure Active Directory** und dann **Identity Governance** aus.

1. Wählen Sie im linken Bereich **Verbundene Organisationen** aus, und wählen Sie dann die verbundene Organisation aus, um diese zu öffnen.

1. Wählen Sie im Übersichtsbereich der verbundenen Organisation **Bearbeiten** aus, um den Organisationsnamen oder die Beschreibung zu ändern.  

1. Wählen Sie im Bereich **Verzeichnis und Domäne** die Option **Verzeichnis und Domäne aktualisieren** aus, um zu einem anderen Verzeichnis oder eine anderen Domäne zu wechseln.

1. Wählen Sie im Bereich **Sponsoren** die Option **Interne Sponsoren hinzufügen** oder **Externe Sponsoren hinzufügen** aus, um einen Benutzer als Sponsor hinzuzufügen. Wenn Sie einen Sponsor entfernen möchten, wählen Sie zuerst den Sponsor und dann im rechten Bereich **Löschen** aus.


## <a name="delete-a-connected-organization"></a>Löschen einer verbundenen Organisation

Wenn die Beziehung mit einem externen Azure AD-Verzeichnis bzw. einer externe Domäne nicht länger besteht, können Sie die verbundene Organisation löschen.

**Erforderliche Rolle:** *Globaler Administrator*, *Benutzeradministrator* oder *Gasteinladender*

1. Wählen Sie im Azure-Portal die Option **Azure Active Directory** und dann **Identity Governance** aus.

1. Wählen Sie im linken Bereich **Verbundene Organisationen** aus, und wählen Sie dann die verbundene Organisation aus, um diese zu öffnen.

1. Klicken Sie auf der Übersichtsseite der verbundenen Organisation auf **Löschen**, um die Organisation zu löschen.

    Derzeit kann eine verbundene Organisation nur gelöscht werden, wenn keine verbundenen Benutzer vorhanden sind.

    ![Schaltfläche „Löschen“ für die verbundene Organisation](./media/entitlement-management-organization/organization-delete.png)

## <a name="next-steps"></a>Nächste Schritte

- [Steuern des Zugriffs für externe Benutzer in der Azure AD-Berechtigungsverwaltung (Vorschau)](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users)
- [Steuern des Zugriffs für nicht in Ihrem Verzeichnis befindliche Benutzer](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
