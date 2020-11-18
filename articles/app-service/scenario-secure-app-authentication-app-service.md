---
title: 'Tutorial: Hinzufügen der Authentifizierung zu einer Web-App in Azure App Service | Azure'
description: In diesem Tutorial wird beschrieben, wie Sie die Authentifizierung und Autorisierung für eine Web-App aktivieren, die in Azure App Service ausgeführt wird.  Beschränken Sie den Zugriff auf die Web-App auf die Benutzer Ihrer Organisation.
services: active-directory, app-service-web
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 11/09/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.openlocfilehash: a1b2809371a01b1c6fd822e5c4aaa197d0de3c1b
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2020
ms.locfileid: "94428293"
---
# <a name="tutorial-add-authentication-to-your-web-app-running-on-azure-app-service"></a>Tutorial: Hinzufügen der Authentifizierung zu Ihrer Web-App in Azure App Service

Es wird beschrieben, wie Sie die Authentifizierung für Ihre Web-App aktivieren, die in Azure App Service ausgeführt wird, und den Zugriff auf die Benutzer Ihrer Organisation beschränken.

:::image type="content" source="./media/scenario-secure-app-authentication-app-service/web-app-sign-in.svg" alt-text="Anmeldung für Benutzer" border="false":::

Azure App Service verfügt über integrierte Authentifizierungs- und Autorisierungsunterstützung, sodass Sie für das Anmelden von Benutzern und den Zugriff auf Daten in Ihrer Web-App nur wenig oder keinen Code schreiben müssen.  Die Verwendung des App Service-Moduls für die Authentifizierung/Autorisierung ist nicht erforderlich, aber sie vereinfacht die Authentifizierung und Autorisierung für Ihre App. In diesem Artikel wird veranschaulicht, wie Sie Ihre Web-App mit dem App Service-Modul für die Authentifizierung/Autorisierung schützen, indem Sie Azure Active Directory als Identitätsanbieter verwenden.

Das Modul für die Authentifizierung/Autorisierung wird über das Azure-Portal und die App-Einstellungen aktiviert und konfiguriert. Weder SDKs noch bestimmte Sprachen oder Änderungen am Anwendungscode sind erforderlich. Es werden verschiedene Identitätsanbieter unterstützt, z. B.: Azure AD, Microsoft-Konto, Facebook, Google und Twitter. Wenn das Modul für die Authentifizierung/Autorisierung aktiviert ist, wird es von allen eingehenden HTTP-Anforderungen durchlaufen, bevor diese vom App-Code verarbeitet werden.  Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung in Azure App Service](overview-authentication-authorization.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Konfigurieren der Authentifizierung für die Web-App
> * Beschränken des Zugriffs auf die Web-App auf die Benutzer Ihrer Organisation

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-publish-a-web-app-on-app-service"></a>Erstellen und Veröffentlichen einer Web-App in App Service

Für dieses Tutorial benötigen Sie eine in Azure App Service bereitgestellte Web-App.  Sie können eine vorhandene Web-App verwenden oder die [ASP.NET Core-Schnellstartanleitung](quickstart-dotnetcore.md) befolgen, um eine neue Web-App für App Service zu erstellen und zu veröffentlichen.

Unabhängig davon, ob Sie eine vorhandene Web-App verwenden oder eine neue erstellen, sollten Sie sich den Namen der Web-App und den Namen der Ressourcengruppe notieren, in der die Web-App bereitgestellt wurde. Sie benötigen diese Namen in diesem Tutorial. Im gesamten Tutorial enthalten die Beispielnamen in Prozeduren und Screenshots die Zeichenfolge *SecureWebApp*.

## <a name="configure-authentication-and-authorization"></a>Konfigurieren der Authentifizierung und Autorisierung

Sie verfügen nun über eine Web-App, die in App Service ausgeführt wird.  Als Nächstes aktivieren Sie die Authentifizierung und Autorisierung für die Web-App. Sie verwenden Azure Active Directory als Identitätsanbieter. Weitere Informationen finden Sie unter [Konfigurieren der Azure Active Directory-Authentifizierung für die App Services-Anwendung](configure-authentication-provider-aad.md).

Wählen Sie im Menü [Azure-Portal](https://portal.azure.com) den Eintrag **Ressourcengruppen** aus, oder suchen und wählen Sie auf einer beliebigen Seite *Ressourcengruppen* aus.

Navigieren Sie in **Ressourcengruppen** zu Ihrer Ressourcengruppe, und wählen Sie sie aus. Wählen Sie unter **Übersicht** die Verwaltungsseite Ihrer App aus.

:::image type="content" alt-text="Auswählen von App Service" source="./media/scenario-secure-app-authentication-app-service/select-app-service.png":::

Klicken Sie in Ihrer App im Menü auf der linken Seite auf **Authentifizierung/Autorisierung**, und aktivieren Sie anschließend „App Service-Authentifizierung“, indem Sie **Ein** auswählen.

Wählen Sie unter **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** die Option **Mit Azure Active Directory anmelden**.

Wählen Sie unter **Authentifizierungsanbieter** die Option **Azure Active Directory** aus. Wählen Sie **Express** aus, und übernehmen Sie die Standardeinstellungen, um eine neue AD-App zu erstellen. Klicken Sie anschließend auf **OK**.

:::image type="content" alt-text="Express-Authentifizierung" source="./media/scenario-secure-app-authentication-app-service/configure-authentication.png":::

Klicken Sie auf der Seite **Authentifizierung/Autorisierung** auf **Speichern**.

Wenn die Benachrichtigung mit der Meldung `Successfully saved the Auth Settings for <app-name> App` angezeigt wird, aktualisieren Sie die Portalseite.

Sie verfügen nun über eine App, die per App Service-Authentifizierung und -Autorisierung geschützt ist.

## <a name="verify-limited-access-to-the-web-app"></a>Überprüfen des beschränkten Zugriffs auf die Web-App

Als Sie das App Service-Modul für die Authentifizierung und Autorisierung aktiviert haben, wurde auf Ihrem Azure AD-Mandanten eine App-Registrierung erstellt.  Die App-Registrierung hat den gleichen Anzeigenamen wie Ihre Web-App. Wählen Sie zum Überprüfen der Einstellungen im Portalmenü die Option **Azure Active Directory** und dann **App-Registrierungen** aus.  Wählen Sie die App-Registrierung aus, die erstellt wurde.  Überprüfen Sie in der Übersicht, ob **Unterstützte Kontotypen** auf **Nur meine Organisation** festgelegt ist.

:::image type="content" alt-text="Überprüfen des Zugriffs" source="./media/scenario-secure-app-authentication-app-service/verify-access.png":::

Starten Sie einen Browser im Inkognito- oder InPrivate-Modus, und navigieren Sie zu `https://<app-name>.azurewebsites.net`, um zu überprüfen, ob der Zugriff auf Ihre App auf die Benutzer Ihrer Organisation beschränkt ist.  Sie sollten auf eine geschützte Anmeldeseite weitergeleitet werden. Dies ist der Beweis, dass nicht authentifizierte Benutzer keinen Zugriff auf die Website haben.  Melden Sie sich als Benutzer Ihrer Organisation an, um Zugriff auf die Website zu erhalten.  Sie können auch einen neuen Browser starten und versuchen, sich mit einem persönlichen Konto anzumelden, um sich zu vergewissern, dass externe Benutzer keinen Zugriff haben.  

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie dieses Tutorial abgeschlossen haben und die Web-App und die zugehörigen Ressourcen nicht mehr benötigen, sollten Sie [die von Ihnen erstellten Ressourcen bereinigen](scenario-secure-app-clean-up-resources.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
>
> * Konfigurieren der Authentifizierung für die Web-App
> * Beschränken des Zugriffs auf die Web-App auf die Benutzer Ihrer Organisation

> [!div class="nextstepaction"]
> [App Service: Zugreifen auf den Speicher](scenario-secure-app-access-storage.md)
