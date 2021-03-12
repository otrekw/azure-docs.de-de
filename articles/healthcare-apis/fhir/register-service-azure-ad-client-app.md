---
title: 'Registrieren einer Dienst-App in Azure AD: Azure API for FHIR'
description: Erfahren Sie, wie Sie eine Dienstclientanwendung in Azure Active Directory registrieren.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 6f7bf122b292ca144eac406957f19a13c7ba6662
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103018974"
---
# <a name="register-a-service-client-application-in-azure-active-directory"></a>Registrieren einer Dienstclientanwendun in Azure Active Directory

In diesem Artikel erfahren Sie, wie Sie eine Dienstclientanwendung in Azure Active Directory registrieren. Clientanwendungsregistrierungen sind Azure Active Directory-Darstellungen von Anwendungen, mit denen Token authentifiziert und abgerufen werden können. Ein Dienstclient ist zur Verwendung durch eine Anwendung vorgesehen, um ein Zugriffstoken ohne interaktive Authentifizierung eines Benutzers abzurufen. Der Client hat bestimmte Anwendungsberechtigungen und verwendet ein Anwendungsgeheimnis (Kennwort), wenn er Zugriffstoken abruft.

Führen Sie die folgenden Schritte aus, um einen neuen Dienstclient zu erstellen.

## <a name="app-registrations-in-azure-portal"></a>App-Registrierungen im Azure-Portal

1. Navigieren Sie im **Azure-Portal** zu [Azure Active Directory](https://portal.azure.com).

2. Wählen Sie **App-Registrierungen** aus.

    ![Azure-Portal. Neue App-Registrierung.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Wählen Sie **Neue Registrierung** aus.

4. Geben Sie dem Dienstclient einen Anzeigenamen. Dienstclientanwendungen verwenden in der Regel keine Antwort-URL.

    :::image type="content" source="media/service-client-app/service-client-registration.png" alt-text="Azure-Portal. Registrierung einer neuen Dienstclient-App.":::

5. Wählen Sie **Registrieren**.

## <a name="api-permissions"></a>API-Berechtigungen

Da Sie Ihre Anwendung nun registriert haben, müssen Sie festlegen, welche API-Berechtigungen im Auftrag der Benutzer von dieser Anwendung angefordert werden können:

1. Wählen Sie **API-Berechtigungen anfordern** aus.
1. Wählen Sie **Berechtigung hinzufügen** aus.

    Wenn Sie die Azure API for FHIR verwenden, müssen Sie den Azure Healthcare APIs eine Berechtigung hinzufügen, indem Sie unter **Von meiner Organisation verwendete APIs** nach **Azure Healthcare APIs** suchen. 

    Wenn Sie auf eine andere Ressourcenanwendung verweisen, wählen Sie unter **Meine APIs** die zuvor erstellte [Ressourcenanwendungsregistrierung für die FHIR-API](register-resource-azure-ad-client-app.md) aus.

    :::image type="content" source="media/service-client-app/service-client-org-api.png" alt-text="Vertraulicher Client. Meine Organisations-APIs" lightbox="media/service-client-app/service-client-org-api-expanded.png":::

1. Wählen Sie Bereiche (Berechtigungen) aus, nach denen die vertrauliche Anwendung im Auftrag eines Benutzers fragen können soll:

    :::image type="content" source="media/service-client-app/service-client-add-permission.png" alt-text="Dienstclient. Delegierte Berechtigungen":::

1. Gewähren Sie der Anwendung die Einwilligung. Sollten Sie nicht die erforderlichen Berechtigungen haben, wenden Sie sich an Ihren Azure Active Directory-Administrator:

    :::image type="content" source="media/service-client-app/service-client-grant-permission.png" alt-text="Dienstclient. Einwilligung erteilen":::

## <a name="application-secret"></a>Anwendungsgeheimnis

Der Dienstclient benötigt ein Geheimnis (Kennwort), um ein Token abrufen zu können.

1. Wählen Sie **Zertifikate & Geheimnisse** aus.
2. Wählen Sie **Neuer geheimer Clientschlüssel**.

    ![Azure-Portal. Geheimer Schlüssel für Dienstclient](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-SECRET.png)

3. Geben Sie eine Beschreibung und die Dauer des Geheimnisses an (1 Jahr, 2 Jahre oder nie).

4. Sobald das Geheimnis generiert wurde, wird es nur einmal im Portal angezeigt. Notieren Sie sich das Geheimnis, und bewahren Sie es sicher auf.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie eine Dienstclientanwendung in Azure Active Directory registrieren. Als Nächstes testen Sie den Zugriff auf Ihren FHIR-Server über Postman.
 
>[!div class="nextstepaction"]
>[Zugreifen auf Azure API for FHIR mit Postman](access-fhir-postman-tutorial.md)
