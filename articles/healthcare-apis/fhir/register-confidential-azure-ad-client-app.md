---
title: 'Registrieren einer vertraulichen Client-App in Azure AD: Azure API for FHIR'
description: Registrieren einer vertraulichen Clientanwendung in Azure Active Directory, die sich im Namen eines Benutzers authentifiziert und den Zugriff auf Ressourcenanwendungen anfordert
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 04/08/2021
ms.author: cavoeg
ms.openlocfilehash: 20d2e86786c3586cd4f45e98d670d4a937a77992
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112299193"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Registrieren einer vertraulichen Clientanwendung in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie eine vertrauliche Clientanwendung in Azure Active Directory (Azure AD.  

Eine Clientanwendungsregistrierung ist Azure AD Darstellung einer Anwendung, die verwendet werden kann, um sich im Namen eines Benutzers zu authentifizieren und Zugriff auf [Ressourcenanwendungen an fordern zu können.](register-resource-azure-ad-client-app.md) Eine vertrauliche Clientanwendung ist eine Anwendung, die sich beim Speichern eines Geheimnisses und dem Anzeigen dieses Geheimnisses beim Anfordern von Zugriffstoken als vertrauenswürdig erweist. Beispiele für vertrauliche Anwendungen sind serverseitige Anwendungen. 

Informationen zum Registrieren einer neuen vertraulichen Clientanwendung finden Sie in den folgenden Schritten. 

## <a name="register-a-new-application"></a>Registrieren einer neuen Anwendung

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Azure Active Directory** aus.

1. Wählen Sie **App-Registrierungen** aus. 

    :::image type="content" source="media/how-to-aad/portal-aad-new-app-registration.png" alt-text="Azure-Portal. Neue App-Registrierung.":::

1. Wählen Sie **Neue Registrierung** aus.

1. Geben Sie der Anwendung einen anzeigebasierten Namen.

1. Wählen **Sie unter Unterstützte Kontotypen** aus, wer die Anwendung verwenden oder auf die API zugreifen kann.

1. (Optional) Geben Sie einen **Umleitungs-URI an.** Diese Details können später geändert werden, aber wenn Sie die Antwort-URL Ihrer Anwendung kennen, geben Sie sie jetzt ein.

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png" alt-text="Registrierung einer neuen vertraulichen Client-App.":::

1. Wählen Sie **Registrieren**.

## <a name="api-permissions"></a>API-Berechtigungen

Nachdem Sie Ihre Anwendung registriert haben, müssen Sie auswählen, welche API-Berechtigungen diese Anwendung im Auftrag von Benutzern anfordern soll.

1. Wählen Sie **API-Berechtigungen anfordern** aus.

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png" alt-text="Vertraulicher Client. API-Berechtigungen.":::

1. Wählen Sie **Berechtigung hinzufügen** aus.

    Wenn Sie die Azure API for FHIR verwenden, fügen Sie den Azure Healthcare-APIs eine Berechtigung hinzu, indem Sie unter **APIs,** die meine Organisation verwendet, nach **der Azure Healthcare-API** suchen. Das Suchergebnis für die Azure Healthcare-API wird nur dann angezeigt, wenn Sie den Dienst [bereits Azure API for FHIR.](fhir-paas-powershell-quickstart.md)

    Wenn Sie auf eine andere Ressourcenanwendung verweisen, wählen Sie Ihre [FHIR-API-Ressourcenanwendungsregistrierung](register-resource-azure-ad-client-app.md) aus, die Sie zuvor unter **Meine APIs erstellt haben.**


    :::image type="content" source="media/conf-client-app/confidential-client-org-api.png" alt-text="Vertraulicher Client. Meine Organisations-APIs" lightbox="media/conf-client-app/confidential-app-org-api-expanded.png":::
    

1. Wählen Sie Bereiche (Berechtigungen) aus, die die vertrauliche Clientanwendung im Auftrag eines Benutzers anfragt. Wählen Sie **user_impersonation** aus, und wählen Sie dann **Berechtigungen hinzufügen** aus.

    :::image type="content" source="media/conf-client-app/confidential-client-add-permission.png" alt-text="Vertraulicher Client. Delegierte Berechtigungen":::


## <a name="application-secret"></a>Anwendungsgeheimnis

1. Wählen Sie **Zertifikate & Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus. 

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png" alt-text="Vertraulicher Client. Anwendungsgeheimnis.":::

1. Geben Sie unter **Beschreibung** eine Beschreibung für Ihren geheimen Clientschlüssel ein. Wählen Sie **das Dropdownmenü Läuft ab** aus, um einen Ablaufzeitrahmen auszuwählen, und klicken Sie dann auf **Hinzufügen.**

   :::image type="content" source="media/how-to-aad/add-a-client-secret.png" alt-text="Fügen Sie einen geheimen Clientgeheimnis hinzu.":::

1. Kopieren Sie nach dem Erstellen  der geheimen Clientzeichenfolge den Wert und die **ID,** und speichern Sie sie an einem sicheren Ort Ihrer Wahl.

   :::image type="content" source="media/how-to-aad/client-secret-string-password.png" alt-text="Geheime Clientzeichenfolge."::: 

> [!NOTE]
>Die geheime Clientzeichenfolge ist nur einmal in der Azure-Portal. Wenn Sie von der Webseite Zertifikate & Geheimnisse weg navigieren und dann zurück zur Seite zurückkehren, wird die Wertzeichenfolge maskiert. Es ist wichtig, die geheime Clientzeichenfolge sofort nach der Generung zu kopieren. Wenn Sie keine Sicherungskopie Ihres geheimen Clientgeheimnis haben, müssen Sie die oben genannten Schritte wiederholen, um sie erneut zu generieren.
 
## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie die Schritte zum Registrieren einer vertraulichen Clientanwendung im Azure AD. Außerdem wurden Sie durch die Schritte zum Hinzufügen von API-Berechtigungen zur Azure Healthcare-API geführt. Schließlich haben Sie erfahren, wie Sie ein Anwendungsgeheimnis erstellen. Darüber hinaus erfahren Sie, wie Sie mithilfe von Postman auf Ihren FHIR-Server zugreifen.
 
>[!div class="nextstepaction"]
>[Zugreifen auf Azure API for FHIR mit Postman](access-fhir-postman-tutorial.md)
