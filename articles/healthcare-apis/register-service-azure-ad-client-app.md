---
title: 'Registrieren einer Dienst-App in Azure AD: Azure API for FHIR'
description: Erfahren Sie, wie Sie eine Dienstclientanwendung in Azure Active Directory registrieren, die zum Authentifizieren und Abrufen von Token verwendet werden kann.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 34eec3ad0d2fc193744898b6f08cbe50c261c945
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87853023"
---
# <a name="register-a-service-client-application-in-azure-active-directory"></a>Registrieren einer Dienstclientanwendun in Azure Active Directory

In diesem Artikel erfahren Sie, wie Sie eine Dienstclientanwendung in Azure Active Directory registrieren. Clientanwendungsregistrierungen sind Azure Active Directory-Darstellungen von Anwendungen, mit denen Token authentifiziert und abgerufen werden können. Ein Dienstclient ist zur Verwendung durch eine Anwendung vorgesehen, um ein Zugriffstoken ohne interaktive Authentifizierung eines Benutzers abzurufen. Der Client hat bestimmte Anwendungsberechtigungen und verwendet ein Anwendungsgeheimnis (Kennwort), wenn er Zugriffstoken abruft.

Führen Sie die folgenden Schritte aus, um einen neuen Dienstclient zu erstellen.

## <a name="app-registrations-in-azure-portal"></a>App-Registrierungen im Azure-Portal

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**.

2. Klicken Sie auf dem Blatt **Azure Active Directory** auf **App-Registrierungen**:

    ![Azure-Portal. Neue App-Registrierung.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Klicken Sie auf **Neue Registrierung**.

## <a name="service-client-application-details"></a>Details einer Dienstclientanwendung

* Der Dienstclient benötigt einen Anzeigenamen, und Sie können auch eine Antwort-URL angeben, aber diese wird in der Regel nicht verwendet.

    ![Azure-Portal. Registrierung einer neuen Dienstclientanwendung.](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-NAME.png)

## <a name="api-permissions"></a>API-Berechtigungen

Sie müssen der Dienstclientanwendungs Rollen gewähren. 

1. Öffnen Sie die **API-Berechtigungen**, und wählen Sie Ihre [FHIR-API-Ressourcenanwendungsregistrierung](register-resource-azure-ad-client-app.md) aus. Wenn Sie die Azure API for FHIR verwenden, müssen Sie den Azure Healthcare APIs eine Berechtigung hinzufügen, indem Sie unter **Von meiner Organisation verwendete APIs** nach diesen APIs suchen.

    ![Azure-Portal. API-Berechtigungen eines Dienstclients.](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-API-PERMISSIONS.png)

2. Wählen Sie die Anwendungsrollen aus denjenigen aus, die für die Ressourcenanwendung definiert sind:

    ![Azure-Portal. Berechtigungen einer Dienstclientanwendung.](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-APPLICATION-PERMISSIONS.png)

3. Gewähren Sie der Anwendung die Einwilligung. Sollten Sie nicht die erforderlichen Berechtigungen haben, wenden Sie sich an Ihren Azure Active Directory-Administrator:

    ![Azure-Portal. Administratoreinwilligung für Dienstclient](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-ADMIN-CONSENT.png)

## <a name="application-secret"></a>Anwendungsgeheimnis

Der Dienstclient benötigt ein Geheimnis (Kennwort), das Sie beim Abrufen von Token verwenden.

1. Klicken Sie auf **Zertifikate &amp; Geheimnisse**.

2. Klicken Sie auf **Neuer geheimer Clientschlüssel**.

    ![Azure-Portal. Geheimer Schlüssel für Dienstclient](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-SECRET.png)

3. Geben Sie eine Dauer für das Geheimnis an.

4. Sobald es generiert wurde, wird es nur einmal im Portal angezeigt. Notieren Sie sich das Geheimnis, und bewahren Sie es sicher auf.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie eine Dienstclientanwendung in Azure Active Directory registrieren. Als Nächstes stellen Sie eine FHIR-API in Azure bereit.
 
>[!div class="nextstepaction"]
>[Bereitstellen von Open Source-FHIR-Servern](fhir-oss-powershell-quickstart.md)