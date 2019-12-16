---
title: Auflisten Ihrer App im Azure AD-Anwendungskatalog | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine Anwendung, die einmaliges Anmelden unterstützt, im Azure Active Directory-App-Katalog listen.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 12/06/2019
ms.author: ryanwi
ms.reviewer: jeedes
ms.custom: aaddev, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3bfdeaba26e98f600b81b3a473326ff4086f1aa2
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74967149"
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Auflisten Ihrer Anwendung zum Azure Active Directory-Anwendungskatalog

In diesem Artikel wird erläutert, wie eine Anwendung im Anwendungskatalog von Azure Active Directory (Azure AD) aufgelistet, das einmalige Anmelden (Single Sign-On, SSO) implementiert und die Auflistung verwaltet wird.

## <a name="what-is-the-azure-ad-application-gallery"></a>Was ist der Azure AD-Anwendungskatalog?

- Kunden finden so die bestmögliche Umgebung für einmaliges Anmelden.
- Die Konfiguration der Anwendung ist einfach und in wenigen Schritten erledigt.
- Eine schnelle Suche findet Ihre Anwendung im Katalog.
- Azure AD-Kunden mit den Tarifen Free, Basic und Premium können diese Integration nutzen.
- Gemeinsame Kunden erhalten ausführliche Tutorials zur Konfiguration.
- Kunden, die das System für die domänenübergreifende Identitätsverwaltung (System for Cross-domain Identity Management, [SCIM](https://techcommunity.microsoft.com/t5/Identity-Standards-Blog/Provisioning-with-SCIM-getting-started/ba-p/880010)) verwenden, können die Bereitstellung für dieselbe App nutzen.

## <a name="prerequisites"></a>Voraussetzungen

- Bei Verbundanwendungen (OpenID und SAML/WS-Fed) muss die Anwendung das Software-as-a-Service-Modell (SaaS-Modell) für die Aufnahme in den Azure AD-App-Katalog unterstützen. Die Unternehmenskataloganwendungen müssen mehrere Kundenkonfigurationen und nicht einen bestimmten Kunden unterstützen.
- Für OpenID Connect muss die Anwendung mehrinstanzenfähig sein, und das [Azure AD-Zustimmungsframework](consent-framework.md) muss für die Anwendung ordnungsgemäß implementiert werden. Der Benutzer kann die Anmeldeanforderung an einen gemeinsamen Endpunkt senden, damit jeder Kunde der Anwendung zustimmen kann. Sie können den Benutzerzugriff anhand der Mandanten-ID und des im Token erhaltenen UPN des Benutzers steuern.
- Für SAML 2.0/WS-Fed muss Ihre Anwendung über eine Funktion zur SAML-/WS-Fed-SSO-Integration im SP- oder IDP-Modus verfügen. Stellen Sie sicher, dass diese Funktion ordnungsgemäß funktioniert, bevor Sie die Anforderung senden.
- Stellen Sie beim Kennwort-SSO sicher, dass Ihre Anwendung die Formularauthentifizierung unterstützt, sodass Kennworttresore verwendet werden können, damit einmaliges Anmelden wie erwartet funktioniert.
- Für Tests benötigen Sie ein dauerhaftes Konto mit mindestens zwei registrierten Benutzern.

**Wie erhalte ich Azure AD für Entwickler?**

Sie können ein kostenloses Testkonto mit allen Premium-Features von Azure AD erhalten. Dieses ist 90 Tage lang kostenlos mit einer Verlängerungsmöglichkeit, solange Sie Entwicklungsaufgaben ausführen: https://docs.microsoft.com/office/developer-program/office-365-developer-program

## <a name="submit-the-request-in-the-portal"></a>Übermitteln der Anforderung im Portal

Nachdem Sie mit einem Test sichergestellt haben, dass die Integration Ihrer Anwendung in Azure AD funktioniert, senden Sie Ihre Anforderung für den Zugriff auf das [Anwendungsnetzwerkportal](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Wenn Sie über ein Office 365-Konto verfügen, verwenden Sie dieses für die Anmeldung bei diesem Portal. Verwenden Sie andernfalls Ihr Microsoft-Konto (z. B. Outlook oder Hotmail) für die Anmeldung.

Wenn nach der Anmeldung die folgende Seite angezeigt wird, wenden Sie sich an das [Azure AD-SSO-Integrationsteam](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). Geben Sie das E-Mail-Konto an, das Sie zum Senden der Anforderung verwenden möchten. Das Azure AD-Team fügt das Konto dann im Microsoft-Anwendungsnetzwerkportal hinzu.

![Zugriffsanforderungsmeldung im SharePoint-Portal](./media/howto-app-gallery-listing/errorimage.png)

Nachdem das Konto hinzugefügt wurde, können Sie sich beim Microsoft-Anwendungsnetzwerkportal anmelden.

Wenn nach der Anmeldung die folgende Seite angezeigt wird, geben Sie eine geschäftliche Begründung für den Zugriff in das Textfeld ein. Wählen Sie dann **Zugriff anfordern** aus.

  ![Feld für die geschäftliche Begründung im SharePoint-Portal](./media/howto-app-gallery-listing/accessrequest.png)

Unser Team überprüft die Details und gewährt Ihnen entsprechend Zugriff. Nachdem Ihre Anforderung genehmigt wurde, können Sie sich beim Portal anmelden und die Anforderung senden, indem Sie auf der Startseite auf die Kachel **Anforderung senden (ISV)** klicken.

![Kachel „Anforderung senden (ISV)“ auf der Startseite](./media/howto-app-gallery-listing/homepage.png)

## <a name="issues-on-logging-into-portal"></a>Probleme bei der Anmeldung am Portal

Wenn dieser Fehler angezeigt wird, während Sie sich anmelden, finden Sie hier die Details zu diesem Problem sowie zum Beheben des Fehlers.

* Wenn Ihre Anmeldung blockiert wurde, wie unten gezeigt:

  ![Fehler beim Auflösen einer Anwendung im Katalog](./media/howto-app-gallery-listing/blocked.png)

**Hintergrund:**

Der Gastbenutzer befindet sich in einem Verbund mit einem Basismandanten, der ebenfalls in Azure AD enthalten ist. Der Gastbenutzer wird als hohes Risiko eingestuft. Microsoft gestattet Benutzern mit hohem Risiko keinen Zugriff auf Ressourcen. Alle Benutzer mit hohem Risiko (Mitarbeiter oder Gäste/Lieferanten) müssen Ihr Risiko für den Zugriff auf Microsoft-Ressourcen korrigieren bzw. eliminieren. Für Gastbenutzer stammt dieses Benutzerrisiko vom Basismandanten, und die Richtlinie stammt vom Ressourcenmandanten (in diesem Fall Microsoft).
 
**Sichere Lösungen:**

* Über MFA registrierte Gastbenutzer korrigieren ihre eigenen Benutzerrisiken. Dies kann durch den Gastbenutzer erfolgen, der eine gesicherte Kennwortänderung oder -zurücksetzung durchführt (https://aka.ms/sspr) in seinem Basismandanten (dazu sind MFA und SSPR im Basismandanten erforderlich)). Die gesicherte Kennwortänderung oder -zurücksetzung muss in Azure AD und nicht lokal initiiert werden.

* Gastbenutzer bitten ihre Administratoren, das Risiko zu korrigieren. In diesem Fall führt der Administrator eine Kennwortzurücksetzung aus (temporäre Kennwortgenerierung). Dies erfordert keinen Identitätsschutz. Der Administrator des Gastbenutzers kann zu https://aka.ms/RiskyUsers navigieren und auf „Kennwort zurücksetzen“ klicken.

* Gastbenutzer bitten ihre Administratoren, das Risiko zu schließen bzw. zu verwerfen. Auch dies erfordert keinen Identitätsschutz. Der Administrator kann zu https://aka.ms/RiskyUsers navigieren und dann auf „Benutzerrisiko verwerfen“ klicken. Administratoren müssen jedoch die erforderliche Sorgfalt walten lassen, um sicherzustellen, dass es sich um eine falsch positive Risikobewertung handelt, bevor sie das Benutzerrisiko schließen. Andernfalls setzen sie ihre und die Ressourcen von Microsoft aufs Spiel, indem sie eine Risikobewertung ohne Untersuchung unterdrücken.

> [!NOTE]
> Wenn Sie Probleme beim Zugriff haben, wenden Sie sich an das [Azure AD-SSO-Integrationsteam](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implement-sso-by-using-the-federation-protocol"></a>Implementieren des einmaligen Anmeldens mithilfe des Verbundprotokolls

Damit eine Anwendung im Azure AD-App-Katalog aufgelistet werden kann, müssen Sie zunächst eines der folgenden von Azure AD unterstützten Verbundprotokolle implementieren. Außerdem müssen Sie den Nutzungsbedingungen des Azure AD-Anwendungskatalogs zustimmen. Lesen Sie die Nutzungsbedingungen des Azure AD-Anwendungskatalogs auf [dieser Website](https://azure.microsoft.com/support/legal/active-directory-app-gallery-terms/).

- **OpenID Connect:** Wenn Sie Ihre Anwendung mithilfe des OpenID Connect-Protokolls in Azure AD integrieren möchten, folgen Sie den [Anweisungen für Entwickler](v1-authentication-scenarios.md).

    ![Auflisten einer OpenID Connect-Anwendung im Katalog](./media/howto-app-gallery-listing/openid.png)

    * Wenn Sie Ihre im Katalog aufzulistende Anwendung mithilfe von OpenID Connect hinzufügen möchten, wählen Sie **OpenID Connect und OAuth 2.0** (wie hier gezeigt) aus.
    * Wenn Sie Probleme beim Zugriff haben, wenden Sie sich an das [Azure AD-SSO-Integrationsteam](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

- **SAML 2.0** oder **WS-Fed**: Wenn Ihre App SAML 2.0 unterstützt, können Sie die App direkt in einen Azure AD-Mandanten integrieren, indem Sie den [Anweisungen zum Hinzufügen einer benutzerdefinierten Anwendung](../active-directory-saas-custom-apps.md) folgen.

  ![Auflisten einer SAML 2.0- oder WS-Fed-Anwendung im Katalog](./media/howto-app-gallery-listing/saml.png)

  * Wenn Sie Ihre im Katalog aufzulistende Anwendung mithilfe von **SAML 2.0** oder **WS-Fed** hinzufügen möchten, wählen Sie **SAML 2.0/WS-Fed** (wie hier gezeigt) aus.

  * Wenn Sie Probleme beim Zugriff haben, wenden Sie sich an das [Azure AD-SSO-Integrationsteam](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implement-sso-by-using-the-password-sso"></a>Implementieren des einmaligen Anmeldens mithilfe von Kennwort-SSO

Erstellen Sie eine Webanwendung mit einer HTML-Anmeldeseite, um das [kennwortbasierte einmalige Anmelden](../manage-apps/what-is-single-sign-on.md) zu konfigurieren. Das kennwortbasierte einmalige Anmelden (auch als „Password Vaulting“ oder „Kennworttresor“ bezeichnet) ermöglicht es Ihnen, den Benutzerzugriff und die Kennwörter für Webanwendungen zu verwalten, die keinen Identitätsverbund unterstützen. Es ist auch in Szenarien nützlich, in denen mehrere Benutzer ein Konto gemeinsam verwenden müssen, wie z. B. bei den App-Konten für soziale Medien Ihrer Organisation.

![Auflisten einer Kennwort-SSO-Anwendung im Katalog](./media/howto-app-gallery-listing/passwordsso.png)

* Wenn Sie Ihre im Katalog aufzulistende Anwendung mithilfe von Kennwort-SSO hinzufügen möchten, wählen Sie **Kennwort-SSO** (wie hier gezeigt) aus.
* Wenn Sie Probleme beim Zugriff haben, wenden Sie sich an das [Azure AD-SSO-Integrationsteam](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="request-for-user-provisioning"></a>Anfordern der Benutzerbereitstellung

Gehen Sie wie in der folgenden Abbildung gezeigt vor, um die Benutzerbereitstellung anzufordern.

   ![Anfordern der Benutzerbereitstellung](./media/howto-app-gallery-listing/user-provisioning.png)

## <a name="update-or-remove-an-existing-listing"></a>Aktualisieren oder Entfernen einer vorhandenen Auflistung

Um eine im Azure AD-App-Katalog vorhandene Anwendung zu aktualisieren oder zu entfernen, müssen Sie zuerst im [Anwendungsnetzwerkportal](https://microsoft.sharepoint.com/teams/apponboarding/Apps) die Anforderung senden. Wenn Sie über ein Office 365-Konto verfügen, verwenden Sie dieses für die Anmeldung bei diesem Portal. Verwenden Sie andernfalls Ihr Microsoft-Konto (z. B. Outlook oder Hotmail) für die Anmeldung.

- Wählen Sie die entsprechende Option wie in der folgenden Abbildung gezeigt aus.

    ![Auflisten einer SAML-Anwendung im Katalog](./media/howto-app-gallery-listing/updateorremove.png)

    * Wenn Sie eine vorhandene Anwendung aktualisieren möchten, wählen Sie die entsprechende Option gemäß Ihrer Anforderung aus.
    * Wenn Sie eine vorhandene Anwendung aus dem Azure AD-App-Katalog entfernen möchten, wählen Sie **Meine Anwendungsauflistung aus dem Katalog entfernen** aus.
    * Wenn Sie Probleme beim Zugriff haben, wenden Sie sich an das [Azure AD-SSO-Integrationsteam](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="list-requests-by-customers"></a>Auflisten von Anforderungen nach Kunden

Kunden können eine Anforderung zum Auflisten einer Anwendung senden, indem sie **App-Anforderungen nach Kunden** > **Neue Anforderung übermitteln** auswählen.

![Zeigt die Kachel für die von Kunden angeforderten Apps](./media/howto-app-gallery-listing/customer-submit-request.png)

Nachstehend ist der Flow für die vom Kunden angeforderten Anwendungen dargestellt.

![Zeigt den Flow für die vom Kunden angeforderten Apps](./media/howto-app-gallery-listing/customer-request.png)

## <a name="timelines"></a>Zeitpläne

Das Auflisten einer SAML 2.0- oder WS-Fed-Anwendung im Katalog dauert etwa 7 bis 10 Werktage.

  ![Zeitskala für das Auflisten einer SAML-Anwendung im Katalog](./media/howto-app-gallery-listing/timeline.png)

Das Auflisten einer OpenID Connect-Anwendung im Katalog dauert etwa 2 bis 5 Werktage.

  ![Zeitskala für das Auflisten einer OpenID Connect-Anwendung im Katalog](./media/howto-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Eskalationen

Für Eskalationen senden Sie eine E-Mail an das [Azure AD-SSO-Integrationsteam](mailto:SaaSApplicationIntegrations@service.microsoft.com) (SaaSApplicationIntegrations@service.microsoft.com). Wir melden uns so schnell wie möglich bei Ihnen.
