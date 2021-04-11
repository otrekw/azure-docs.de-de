---
title: 'Registrieren einer vertraulichen Client-App in Azure AD: Azure API for FHIR'
description: Registrieren einer vertraulichen Clientanwendung in Azure Active Directory, die sich im Namen eines Benutzers authentifiziert und den Zugriff auf Ressourcenanwendungen anfordert
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 04/08/2021
ms.author: matjazl
ms.openlocfilehash: c10b27d375e2bfb8c64130eceb416a633241cf68
ms.sourcegitcommit: c6a2d9a44a5a2c13abddab932d16c295a7207d6a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107284431"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Registrieren einer vertraulichen Clientanwendung in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie eine vertrauliche Client Anwendung in Azure Active Directory registrieren (Azure AD).  

Eine Client Anwendungs Registrierung ist eine Azure AD Darstellung einer Anwendung, die verwendet werden kann, um im Namen eines Benutzers zu authentifizieren und Zugriff auf [Ressourcen Anwendungen](register-resource-azure-ad-client-app.md)anzufordern. Eine vertrauliche Clientanwendung ist eine Anwendung, die sich beim Speichern eines Geheimnisses und dem Anzeigen dieses Geheimnisses beim Anfordern von Zugriffstoken als vertrauenswürdig erweist. Beispiele für vertrauliche Anwendungen sind serverseitige Anwendungen. 

Weitere Informationen zum Registrieren einer neuen vertraulichen Client Anwendung finden Sie in den folgenden Schritten. 

## <a name="register-a-new-application"></a>Registrieren einer neuen Anwendung

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Azure Active Directory** aus.

1. Wählen Sie **App-Registrierungen** aus. 

    :::image type="content" source="media/how-to-aad/portal-aad-new-app-registration.png" alt-text="Azure-Portal. Neue APP-Registrierung.":::

1. Wählen Sie **Neue Registrierung** aus.

1. Weisen Sie der Anwendung einen Benutzer seitigen anzeigen Amen zu.

1. Wählen Sie für **unterstützte Konto Typen** aus, wer die Anwendung verwenden oder auf die API zugreifen kann.

1. Optionale Gibt einen **Umleitungs-URI** an. Diese Details können später geändert werden, aber wenn Sie die Antwort-URL Ihrer Anwendung kennen, geben Sie sie jetzt ein.

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png" alt-text="Registrierung einer neuen vertraulichen Client-App.":::

1. Wählen Sie **Registrieren**.

## <a name="api-permissions"></a>API-Berechtigungen

Nachdem Sie Ihre Anwendung registriert haben, müssen Sie auswählen, welche API-Berechtigungen diese Anwendung im Auftrag von Benutzern anfordern soll.

1. Wählen Sie **API-Berechtigungen anfordern** aus.

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png" alt-text="Vertraulicher Client. API-Berechtigungen.":::

1. Wählen Sie **Berechtigung hinzufügen** aus.

    Wenn Sie die Azure-API für fhir verwenden, fügen Sie den Azure-Healthcare-APIs eine Berechtigung hinzu, indem Sie in den von **meiner Organisation verwendeten APIs** nach **Azure Healthcare-API** suchen. Das Suchergebnis für die Azure-Healthcare-API gibt nur zurück, wenn Sie [die Azure-API für den Einsatz von](fhir-paas-powershell-quickstart.md)Azure bereits bereitgestellt haben.

    Wenn Sie auf eine andere Ressourcen Anwendung verweisen, wählen Sie Ihre [fhir-API-Ressourcen Anwendungs Registrierung](register-resource-azure-ad-client-app.md) aus, die Sie zuvor unter **meine APIs** erstellt haben.


    :::image type="content" source="media/conf-client-app/confidential-client-org-api.png" alt-text="Vertraulicher Client. Meine Organisations-APIs" lightbox="media/conf-client-app/confidential-app-org-api-expanded.png":::
    

1. Wählen Sie Bereiche (Berechtigungen) aus, die von der vertraulichen Client Anwendung im Auftrag eines Benutzers angefordert werden. Wählen Sie **user_impersonation** aus, und wählen Sie dann **Berechtigungen hinzufügen** aus.

    :::image type="content" source="media/conf-client-app/confidential-client-add-permission.png" alt-text="Vertraulicher Client. Delegierte Berechtigungen":::


## <a name="application-secret"></a>Anwendungsgeheimnis

1. Wählen Sie **Zertifikate & Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus. 

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png" alt-text="Vertraulicher Client. Geheimer Anwendungs Schlüssel.":::

1. Geben Sie unter **Beschreibung** eine Beschreibung für Ihren geheimen Clientschlüssel ein. Wählen Sie das Dropdown Menü **ablaufen** aus, um einen Ablaufzeit Rahmen auszuwählen, und klicken Sie dann auf **Hinzufügen**.

   :::image type="content" source="media/how-to-aad/add-a-client-secret.png" alt-text="Fügen Sie einen geheimen Client Schlüssel hinzu.":::

1. Kopieren Sie nach der Erstellung der geheimen Client Schlüssel Zeichenfolge den **Wert** und die **ID**, und speichern Sie Sie an einem sicheren Ort Ihrer Wahl.

   :::image type="content" source="media/how-to-aad/client-secret-string-password.png" alt-text="Geheime Client Schlüssel Zeichenfolge."::: 

> [!NOTE]
>Die geheime Client Schlüssel Zeichenfolge ist im Azure-Portal nur einmal sichtbar. Wenn Sie von der Webseite mit den Zertifikaten & Geheimnissen navigieren und dann zurückkehren, wird die Wert Zeichenfolge maskiert. Es ist wichtig, dass Sie die Zeichenfolge des geheimen Client Schlüssels direkt nach der Generierung kopieren. Wenn Sie nicht über eine Sicherungskopie Ihres geheimen Client Schlüssels verfügen, müssen Sie die oben genannten Schritte wiederholen, um Sie neu zu generieren.
 
## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden die Schritte zum Registrieren einer vertraulichen Client Anwendung im Azure AD erläutert. Außerdem wurden Sie durch die Schritte zum Hinzufügen von API-Berechtigungen zur Azure-Healthcare-API geführt. Schließlich haben Sie erfahren, wie Sie einen geheimen Anwendungs Schlüssel erstellen. Darüber hinaus erfahren Sie, wie Sie mithilfe von Postman auf Ihren fhir-Server zugreifen können.
 
>[!div class="nextstepaction"]
>[Zugreifen auf Azure API for FHIR mit Postman](access-fhir-postman-tutorial.md)
