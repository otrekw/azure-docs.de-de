---
title: 'Schnellstart: Einrichten des einmaligen Anmeldens (Single Sign-On, SSO) für eine Anwendung auf Ihrem Azure AD-Mandanten (Azure Active Directory)'
description: In dieser Schnellstartanleitung wird die Einrichtung des einmaligen Anmeldens (Single Sign-On, SSO) für eine Anwendung auf Ihrem Azure AD-Mandanten (Azure Active Directory) Schritt für Schritt beschrieben.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 07/01/2020
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: f90edf525a8deb35376d6637090e7b424b2fd658
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86038989"
---
# <a name="quickstart-set-up-single-sign-on-sso-for-an-application-in-your-azure-active-directory-azure-ad-tenant"></a>Schnellstart: Einrichten des einmaligen Anmeldens (Single Sign-On, SSO) für eine Anwendung auf Ihrem Azure AD-Mandanten (Azure Active Directory)

Beginnen Sie mit vereinfachten Benutzeranmeldungen, indem Sie SSO für eine Anwendung einrichten, die Sie Ihrem Azure AD-Mandanten hinzugefügt haben. Nachdem Sie SSO eingerichtet haben, können sich Ihre Benutzer mit ihren Azure AD-Anmeldeinformationen bei einer Anwendung anmelden. SSO ist in der kostenlosen Edition von Azure AD enthalten.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um SSO für eine Anwendung einzurichten, die Sie Ihrem Azure AD-Mandanten hinzugefügt haben:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine der folgenden Rollen: Globaler Administrator, Cloudanwendungsadministrator, Anwendungsadministrator oder Besitzer des Dienstprinzipals.
- Eine Anwendung, die einmaliges Anmelden unterstützt und bereits vorkonfiguriert und dem Azure AD-Katalog hinzugefügt wurde. In den meisten Apps kann Azure AD für einmaliges Anmelden verwendet werden. Die Apps im Azure AD-Katalog wurden vorkonfiguriert. Falls Ihre App nicht aufgelistet ist oder es sich um eine benutzerdefinierte App handelt, können Sie sie trotzdem mit Azure AD verwenden. Sehen Sie sich die Tutorials und die restliche Dokumentation im Inhaltsverzeichnis an. In dieser Schnellstartanleitung liegt der Schwerpunkt auf Apps, die von den App-Entwicklern für SSO vorkonfiguriert und dem Azure AD-Katalog hinzugefügt wurden.
- (Optional: Durcharbeitung von [Anzeigen Ihrer Apps](view-applications-portal.md).)
- (Optional: Durcharbeitung von [Hinzufügen einer App](add-application-portal.md).)
- (Optional: Durcharbeitung von [Konfigurieren einer App](add-application-portal-configure.md).)


>[!IMPORTANT]
>Wir empfehlen Ihnen, zum Testen der Schritte in dieser Schnellstartanleitung keine Produktionsumgebung zu verwenden.


## <a name="enable-single-sign-on-for-an-app"></a>Aktivieren des einmaligen Anmeldens für eine App

Nachdem Sie das Hinzufügen einer Anwendung zu Ihrem Azure AD-Mandanten abgeschlossen haben, wird sofort die entsprechende Übersichtsseite angezeigt. Falls Sie eine bereits hinzugefügte Anwendung konfigurieren, hilft Ihnen die erste Schnellstartanleitung weiter. Darin wird das Anzeigen der Anwendungen beschrieben, die Sie Ihrem Mandanten hinzugefügt haben. 

Richten Sie einmaliges Anmelden wie folgt für eine Anwendung ein:

1. Wählen Sie im Azure AD-Portal die Option **Unternehmensanwendungen** und dann die Anwendung aus, für die Sie einmaliges Anmelden einrichten möchten.
2. Wählen Sie im Abschnitt „Verwalten“ die Option **Einmaliges Anmelden** aus, um den Eigenschaftenbereich für die Bearbeitung zu öffnen.
    :::image type="content" source="media/add-application-portal-setup-sso/configure-sso.png" alt-text="Seite zum Konfigurieren des einmaligen Anmeldens in Azure AD":::
3. Wählen Sie „SAML“ aus, um die Seite für die SSO-Konfiguration zu öffnen. In diesem Beispiel ist GitHub die Anwendung, für die wir SSO konfigurieren. Nachdem Sie GitHub eingerichtet haben, können sich Ihre Benutzer mit ihren Anmeldeinformationen von unserem Azure AD-Mandanten bei GitHub anmelden.
    :::image type="content" source="media/add-application-portal-setup-sso/github-sso.png" alt-text="Seite zum Konfigurieren des einmaligen Anmeldens auf GitHub":::
4. Der Prozess zum Konfigurieren einer Anwendung für die Verwendung von Azure AD für SAML-basiertes SSO variiert je nach Anwendung. Es ist ein Link zur Anleitung für GitHub vorhanden. Anleitungen für andere Apps finden Sie unter https://docs.microsoft.com/azure/active-directory/saas-apps/.
5. Befolgen Sie die Anleitung zum Einrichten von SSO für die Anwendung. Viele Anwendungen verfügen über spezifische Abonnementanforderungen für die SSO-Funktionen. Beispielsweise ist für GitHub ein Enterprise-Abonnement erforderlich.
    :::image type="content" source="media/add-application-portal-setup-sso/github-pricing.png" alt-text="Option für einmaliges Anmelden im Enterprise-Abonnement auf der GitHub-Preisseite":::


## <a name="next-steps"></a>Nächste Schritte

- [Löschen einer App](delete-application-portal.md)
