---
title: Verwalten der Authentifizierung | Microsoft Azure Maps
description: Verwalten der Authentifizierung in Microsoft Azure Maps über das Azure-Portal.
author: walsehgal
ms.author: v-musehg
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 30eb637e9117818758ed4ab0e3adef9db29cc698
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77057310"
---
# <a name="manage-authentication-in-azure-maps"></a>Verwalten der Authentifizierung in Azure Maps

Nachdem Sie ein Azure Maps-Konto erstellt haben, werden eine Client-ID und Schlüssel für die Authentifizierung über Azure Active Directory (Azure AD) und über gemeinsam verwendete Schlüssel erstellt.

## <a name="view-authentication-details"></a>Anzeigen von Authentifizierungsdetails

Nach dem Erstellen eines Azure Maps-Kontos werden der Primärschlüssel und sekundäre Schlüssel generiert. Es wird empfohlen, einen Primärschlüssel als Abonnementschlüssel zu verwenden, wenn Sie [Azure Maps mithilfe der Authentifizierung mit gemeinsam verwendetem Schlüssel aufrufen](https://docs.microsoft.com/azure/azure-maps/azure-maps-authentication#shared-key-authentication). Sie können einen sekundären Schlüssel in Szenarien wie Änderungen beim Schlüsselrollover verwenden. Weitere Informationen finden Sie unter [Authentifizierung in Azure Maps](https://aka.ms/amauth).

Sie können Ihre Authentifizierungsdetails im Azure-Portal anzeigen. Wählen Sie dort in Ihrem Konto im Menü **Einstellungen** die Option **Authentifizierung** aus.

![Authentifizierungsdetails](./media/how-to-manage-authentication/how-to-view-auth.png)


## <a name="register-and-configure-an-azure-ad-app"></a>Registrieren und Konfigurieren einer Azure AD-App

1. Wählen Sie im Azure-Portal in der Liste der Azure-Dienste die Optionen **Azure Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus.  

    ![App-Registrierung](./media/how-to-manage-authentication/app-registration.png)

1. Wenn Sie die App bereits registriert haben, fahren Sie mit dem nächsten Schritt fort. Wenn Sie die App noch nicht registriert haben, geben Sie einen **Namen** ein, und wählen Sie einen **unterstützten Kontotyp** und dann **Registrieren** aus.  

    ![App-Registrierungsdetails](./media/how-to-manage-authentication/app-create.png)

1. Navigieren Sie zu der Anwendung, um Azure Maps die delegierten API-Berechtigungen zuzuweisen. Wählen Sie dann unter **App-Registrierungen** die Optionen **API-Berechtigungen** > **Berechtigung hinzufügen** aus. Suchen Sie unter **Von meiner Organisation verwendete APIs** die Option **Azure Maps**, und wählen Sie sie aus.

    ![Hinzufügen der App-API-Berechtigungen](./media/how-to-manage-authentication/app-permissions.png)

1. Aktivieren Sie das Kontrollkästchen neben **Auf Azure Maps zugreifen**, und wählen Sie dann **Berechtigungen hinzufügen** aus.

    ![Auswählen von App-API-Berechtigungen](./media/how-to-manage-authentication/select-app-permissions.png)

1. Führen Sie je nach verwendeter Authentifizierungsmethode einen der folgenden Schritte aus. 

    * Wenn in Ihrer Anwendung die Benutzertokenauthentifizierung mit dem Azure Maps Web SDK verwendet wird, aktivieren Sie `oauth2AllowImplicitFlow`. Um diese Einstellung zu aktivieren, legen Sie `oauth2AllowImplicitFlow` im Abschnitt **Manifest** der App-Registrierung auf TRUE fest. 
    
       ![App-Manifest](./media/how-to-manage-authentication/app-manifest.png)

    * Wenn in der Anwendung die Server- oder Anwendungsauthentifizierung verwendet wird, navigieren Sie auf der Registrierungsseite für die App zu **Zertifikate & Geheimnisse**. Laden Sie dann entweder ein öffentliches Schlüsselzertifikat hoch, oder erstellen Sie ein Kennwort, indem Sie **Neuer geheimer Clientschlüssel** auswählen. 
    
       ![Erstellen eines Clientgeheimnisses](./media/how-to-manage-authentication/app-keys.png)

        Wenn Sie ein Kennwort erstellen, kopieren Sie es nach dem Auswählen von **Hinzufügen**, und bewahren Sie es sicher auf. Dieses Kennwort benötigen Sie zum Abrufen von Token von Azure AD.

       ![Geheimen Clientschlüssel hinzufügen](./media/how-to-manage-authentication/add-key.png)


## <a name="grant-role-based-access-control-to-azure-maps"></a>Gewähren der rollenbasierten Zugriffssteuerung in Azure Maps

Nachdem Sie Ihrem Azure AD-Mandanten ein Azure Maps-Konto zugeordnet haben, können Sie die Zugriffssteuerung gewähren. Sie gewähren die *rollenbasierte Zugriffssteuerung* (RBAC), indem Sie einen Benutzer, eine Gruppe oder eine Anwendung zu einer oder mehreren Azure Maps-Zugriffssteuerungsrollen zuweisen. 

1. Navigieren Sie zu Ihrem **Azure Maps-Konto**. Wählen Sie **Zugriffssteuerung (IAM)**  > **Rollenzuweisungen** aus.

    ![Gewähren von RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

1. Wählen Sie auf der Registerkarte **Rollenzuweisungen** unter **Rolle** die Rolle **Azure Maps Date Reader (Vorschau)** aus. Wählen Sie unter **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal** aus. Wählen Sie den Benutzer oder die Anwendung aus. Klicken Sie dann auf **Speichern**.

    ![Rollenzuweisung hinzufügen](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Anzeigen verfügbarer Azure Maps-RBAC-Rollen

Navigieren Sie zum Anzeigen der für Azure Maps verfügbaren RBAC-Rollen zu **Zugriffssteuerung (IAM)** . Wählen Sie **Rollen** aus, und suchen Sie dann nach Rollen, die mit *Azure Maps* beginnen. Hierbei handelt es sich um die Azure Maps-Rollen, denen Sie den Zugriff gewähren können.

![Anzeigen verfügbarer Rollen](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-rbac"></a>Anzeigen der rollenbasierten Zugriffssteuerung von Azure Maps

Mit der rollenbasierten Zugriffssteuerung kann die Zugriffssteuerung genau abgestimmt werden.

Um Benutzer und Apps anzuzeigen, denen die rollenbasierte Zugriffssteuerung für Azure Maps gewährt wurde, navigieren Sie zu **Zugriffssteuerung (IAM)** . Wählen Sie dort die Option **Rollenzuweisungen** aus, und filtern Sie dann nach **Azure Maps**.

![Anzeigen von Benutzern und Apps, denen die rollenbasierte Zugriffssteuerung gewährt wurde](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Anfordern von Token für Azure Maps

Nachdem Sie Ihre App registriert und mit Azure Maps verknüpft haben, können Sie Zugriffstoken anfordern.

Wenn in der Anwendung die Benutzertokenauthentifizierung mit dem Azure Maps Web SDK verwendet wird, konfigurieren Sie die HTML-Seite mit der Azure Maps-Client-ID und der Azure AD-App-ID.

Wenn in der Anwendung die Server- oder Anwendungsauthentifizierung verwendet wird, fordern Sie ein Token vom Azure AD-Tokenendpunkt `https://login.microsoftonline.com` an. Verwenden Sie in der Anforderung die folgenden Details: 

* Azure AD-Ressourcen-ID `https://atlas.microsoft.com/`
* Azure Maps-Client-ID
* Azure AD-App-ID
* Kennwort oder Zertifikat für die Azure AD-App-Registrierung

| Azure-Umgebung   | Azure AD-Tokenendpunkt | Azure-Ressourcen-ID |
| --------------------|-------------------------|-------------------|
| Öffentliche Azure-Cloud        | `https://login.microsoftonline.com` | `https://atlas.microsoft.com/` |
| Azure Government-Cloud   | `https://login.microsoftonline.us`  | `https://atlas.microsoft.com/` | 

Weitere Informationen zum Anfordern von Zugriffstoken aus Azure AD für Benutzer und Dienstprinzipale finden Sie unter [Was ist Authentifizierung?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Azure AD und Azure Maps Web SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).

Suchen der API-Nutzungsmetriken für Ihr Azure Maps-Konto:
> [!div class="nextstepaction"] 
> [Anzeigen von Nutzungsmetriken](how-to-view-api-usage.md)

Beispiele für die Integration von Azure AD in Azure Maps:

> [!div class="nextstepaction"]
> [Beispiele für die Azure AD-Authentifizierung](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)
