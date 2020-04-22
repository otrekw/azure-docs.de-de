---
title: 'Schnellstart: Registrieren von Apps bei Microsoft Identity Platform | Azure'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Anwendung hinzufügen und bei Microsoft Identity Platform registrieren.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 03/12/2020
ms.author: ryanwi
ms.custom: aaddev, identityplatformtop40
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: 79983678d13b810a521a00ba2c1978de92a5029f
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309511"
---
# <a name="quickstart-register-an-application-with-the-microsoft-identity-platform"></a>Schnellstart: Registrieren einer Anwendung bei Microsoft Identity Platform

In dieser Schnellstartanleitung wird eine Anwendung mithilfe von **App-Registrierungen** im Azure-Portal registriert. 

Ihre App wird bei einem Azure Active Directory-Mandanten registriert, um sie in Microsoft Identity Platform zu integrieren. Unternehmensentwickler und SaaS-Anbieter (Software-as-a-Service) können kommerzielle Clouddienste oder Branchenanwendungen entwickeln, die in Microsoft Identity Platform integriert werden können. Die Integration ermöglicht eine sichere Anmeldung und Autorisierung für solche Dienste.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* Ein [Azure AD-Mandant](quickstart-create-new-tenant.md).

## <a name="register-a-new-application-using-the-azure-portal"></a>Registrieren einer neuen Anwendung mit dem Azure-Portal

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wenn Sie mit Ihrem Konto Zugriff auf mehrere Mandanten haben, können Sie Ihr Konto rechts oben auswählen. Legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Suchen Sie nach **Azure Active Directory**, und wählen Sie diese Option aus. Wählen Sie unter **Verwalten** die Option **App-Registrierungen** aus.
1. Wählen Sie **Neue Registrierung** aus.
1. Geben Sie unter **Anwendung registrieren** einen aussagekräftigen Anwendungsnamen ein, der Benutzern angezeigt werden soll.
1. Geben Sie an, von wem die Anwendung verwendet werden kann:

    | Unterstützte Kontotypen | BESCHREIBUNG |
    |-------------------------|-------------|
    | **Nur Konten in diesem Organisationsverzeichnis** | Wählen Sie diese Option aus, wenn Sie eine Branchenanwendung erstellen. Falls Sie die Anwendung nicht in einem Verzeichnis registrieren, ist diese Option nicht verfügbar.<br><br>Diese Option ist für reine Azure AD-Apps mit einem einzelnen Mandanten vorgesehen.<br><br>Sofern Sie die App nicht außerhalb eines Verzeichnisses registrieren, ist dies die Standardoption. Wird die App außerhalb eines Verzeichnisses registriert, werden standardmäßig mehrinstanzenfähige Azure AD-Konten und persönliche Microsoft-Konten verwendet. |
    | **Konten in einem beliebigen Organisationsverzeichnis** | Wählen Sie diese Option, wenn Sie alle Kunden aus dem Unternehmens- und Bildungsbereich ansprechen möchten.<br><br>Diese Option ist für reine Azure AD-Apps mit mehreren Mandanten vorgesehen.<br><br>Wenn Sie die App als reine Azure AD-App mit einem einzelnen Mandanten registriert haben, können Sie sie über die Seite **Authentifizierung** in eine Azure AD-App mit mehreren Mandanten umwandeln (und umgekehrt). |
    | **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** | Verwenden Sie diese Option, um die breiteste Kundengruppe anzusprechen.<br><br>Diese Option ist für Azure AD-Apps mit mehreren Mandanten und für persönliche Microsoft-Konten vorgesehen.<br><br>Falls Sie die App als Azure AD-App mit mehreren Mandanten und für persönliche Microsoft-Konten registriert haben, können Sie diese Einstellung nicht über die Benutzeroberfläche ändern. In diesem Fall müssen die unterstützten Kontotypen mithilfe des Anwendungsmanifest-Editors geändert werden. |

1. Wählen Sie unter **Umleitungs-URI (optional)** die Art der App aus, die Sie erstellen: **Web** oder **Öffentlicher Client/nativ (mobil und Desktop)** . Geben Sie anschließend den Umleitungs-URI oder die Antwort-URL für Ihre Anwendung ein.

    * Geben Sie für Webanwendungen die Basis-URL Ihrer App an. `https://localhost:31544` kann beispielsweise die URL für eine Web-App sein, die auf einem lokalen Computer ausgeführt wird. Benutzer können diese URL nutzen, um sich an einer Webclientanwendung anzumelden.
    * Geben Sie für öffentliche Clientanwendungen den URI an, der von Azure AD zum Zurückgeben von Tokenantworten verwendet wird. Geben Sie einen für Ihre Anwendung spezifischen Wert ein (beispielsweise `myapp://auth`).

    Beispiele für Webanwendungen oder native Anwendungen finden Sie in den Schnellstartanleitungen unter [Microsoft Identity Platform](https://docs.microsoft.com/azure/active-directory/develop).

1. Wenn Sie so weit sind, klicken Sie auf **Registrieren**.

    ![Bildschirm zum Registrieren einer neuen Anwendung im Azure-Portal](./media/quickstart-add-azure-ad-app-preview/new-app-registration.png)

Azure AD weist Ihrer App eine eindeutige Anwendungs-ID (oder Client-ID) zu. Im Portal wird die Seite **Übersicht** für Ihre Anwendung geöffnet. Wenn Sie Ihrer Anwendung Funktionen hinzufügen möchten, können Sie weitere Konfigurationsoptionen wie Branding, Zertifikate und Geheimnisse, API-Berechtigungen und Ähnliches auswählen.

![Beispiel der Übersichtsseite einer neu registrierten App](./media/quickstart-add-azure-ad-app-preview/new-app-overview-page-expanded.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Zugreifen auf Web-APIs finden Sie unter [Schnellstart: Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](quickstart-configure-app-access-web-apis.md)

* Informationen zu den Berechtigungen finden Sie unter [Berechtigungen und Zustimmung im Microsoft Identity Platform-Endpunkt](v2-permissions-and-consent.md).

* Informationen zum Verfügbarmachen von Web-APIs finden Sie unter [Schnellstart: Konfigurieren einer Anwendung für das Verfügbarmachen von Web-APIs](quickstart-configure-app-expose-web-apis.md).

* Informationen zum Verwalten unterstützter Konten finden Sie unter [Schnellstart: Ändern der von einer Anwendung unterstützten Konten](quickstart-modify-supported-accounts.md).

* Informationen zum Erstellen einer App und zum Hinzufügen von Funktionen finden Sie in den Schnellstartanleitungen unter [Microsoft Identity Platform](https://docs.microsoft.com/azure/active-directory/develop).

* Um mehr über die beiden Azure AD-Objekte, die eine registrierte Anwendung darstellen, und die Beziehung zwischen ihnen zu erfahren, lesen Sie [Anwendungsobjekte und Dienstprinzipalobjekte](app-objects-and-service-principals.md).

* Weitere Informationen zu Brandingrichtlinien für die App-Entwicklung finden Sie unter [Brandingrichtlinien für Anwendungen](howto-add-branding-in-azure-ad-apps.md).
