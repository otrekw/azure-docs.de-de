---
title: 'Registrieren einer vertraulichen Client-App in Azure AD: Azure API for FHIR'
description: Registrieren einer vertraulichen Clientanwendung in Azure Active Directory, die sich im Namen eines Benutzers authentifiziert und den Zugriff auf Ressourcenanwendungen anfordert
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 8021fb3fa9f11ef895569f48a2ae21b3f7adcd36
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103019024"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Registrieren einer vertraulichen Clientanwendung in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie eine vertrauliche Clientanwendung in Azure Active Directory registrieren. 

Die Registrierung einer Clientanwendung ist eine Azure Active Directory-Darstellung einer Anwendung, die zur Authentifizierung im Auftrag eines Benutzers und zur Anforderung des Zugriffs auf [Ressourcenanwendungen](register-resource-azure-ad-client-app.md) verwendet werden kann. Eine vertrauliche Clientanwendung ist eine Anwendung, die sich beim Speichern eines Geheimnisses und dem Anzeigen dieses Geheimnisses beim Anfordern von Zugriffstoken als vertrauenswürdig erweist. Beispiele für vertrauliche Anwendungen sind serverseitige Anwendungen.

Führen Sie die folgenden Schritte aus, um eine neue vertrauliche Anwendung im Portal zu registrieren.

## <a name="register-a-new-application"></a>Registrieren einer neuen Anwendung

1. Navigieren Sie im **Azure-Portal** zu [Azure Active Directory](https://portal.azure.com).

1. Wählen Sie **App-Registrierungen** aus.

    ![Azure-Portal. Neue App-Registrierung.](media/how-to-aad/portal-aad-new-app-registration.png)

1. Wählen Sie **Neue Registrierung** aus.

1. Weisen Sie der Anwendung einen Anzeigenamen zu.

1. Stellen Sie eine Antwort-URL bereit. Diese Details können später geändert werden, aber wenn Sie die Antwort-URL Ihrer Anwendung kennen, geben Sie sie jetzt ein.

    ![Registrierung einer neuen vertraulichen Client-App.](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png)
1. Wählen Sie **Registrieren**.

## <a name="api-permissions"></a>API-Berechtigungen

Da Sie Ihre Anwendung nun registriert haben, müssen Sie festlegen, welche API-Berechtigungen im Auftrag der Benutzer von dieser Anwendung angefordert werden können:

1. Wählen Sie **API-Berechtigungen anfordern** aus.

    ![Vertraulicher Client. API-Berechtigungen](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png)

1. Wählen Sie **Berechtigung hinzufügen** aus.

    Wenn Sie die Azure API for FHIR verwenden, müssen Sie den Azure Healthcare APIs eine Berechtigung hinzufügen, indem Sie unter **Von meiner Organisation verwendete APIs** nach **Azure Healthcare APIs** suchen. Diese APIs werden nur gefunden, wenn Sie die [Azure API for FHIR](fhir-paas-powershell-quickstart.md) bereits bereitgestellt haben.

    Wenn Sie auf eine andere Ressourcenanwendung verweisen, wählen Sie unter **Meine APIs** die zuvor erstellte [Ressourcenanwendungsregistrierung für die FHIR-API](register-resource-azure-ad-client-app.md) aus.


    :::image type="content" source="media/conf-client-app/confidential-client-org-api.png" alt-text="Vertraulicher Client. Meine Organisations-APIs" lightbox="media/conf-client-app/confidential-app-org-api-expanded.png":::
    

3. Wählen Sie Bereiche (Berechtigungen) aus, nach denen die vertrauliche Anwendung im Auftrag eines Benutzers fragen können soll:

    :::image type="content" source="media/conf-client-app/confidential-client-add-permission.png" alt-text="Vertraulicher Client. Delegierte Berechtigungen":::

## <a name="application-secret"></a>Anwendungsgeheimnis

1. Wählen Sie **Zertifikate & Geheimnisse** aus.
1. Wählen Sie **Neuer geheimer Clientschlüssel**. 

    ![Vertraulicher Client. Anwendungsgeheimnis](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png)

2. Geben Sie eine Beschreibung und die Dauer des Geheimnisses an (1 Jahr, 2 Jahre oder nie).

3. Nach der Generierung wird es im Portal nur einmal angezeigt. Notieren Sie es sich, und bewahren Sie es sicher auf.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie eine vertrauliche Clientanwendung in Azure Active Directory registrieren. Nun können Sie mithilfe von Postman auf Ihren FHIR-Server zugreifen.
 
>[!div class="nextstepaction"]
>[Zugreifen auf Azure API for FHIR mit Postman](access-fhir-postman-tutorial.md)
