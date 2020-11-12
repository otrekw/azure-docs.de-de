---
title: Registrieren einer Ressourcen-App in Azure AD – Azure API for FHIR
description: Registrieren Sie eine Ressourcen-App (oder API-App) in Azure Active Directory, damit Clientanwendungen bei der Authentifizierung Zugriff auf die Ressource anfordern können.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: e22eaacd73bb15ddf43f416831ff5ff42923b6e0
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2020
ms.locfileid: "93393386"
---
# <a name="register-a-resource-application-in-azure-active-directory"></a>Registrieren einer Ressourcenanwendung in Azure Active Directory

In diesem Artikel erfahren Sie, wie Sie eine Ressourcenanwendung (oder API-Anwendung) in Azure Active Directory registrieren. Eine Ressourcenanwendung ist eine Azure Active Directory-Darstellung der FHIR-Server-API selbst, und Clientanwendungen können bei der Authentifizierung den Zugriff auf die Ressource anfordern. Die Ressourcenanwendung wird im OAuth-Kontext auch als *Zielgruppe* bezeichnet.

## <a name="azure-api-for-fhir"></a>Azure-API für FHIR

Wenn Sie Azure API for FHIR verwenden, wird automatisch eine Ressourcenanwendung erstellt, wenn Sie den Dienst bereitstellen. Verwenden Sie Azure API for FHIR in dem Azure Active Directory-Mandanten, in dem Sie auch Ihre Anwendung bereitstellen, so können Sie diese Anleitung überspringen und stattdessen mit der Bereitstellung von Azure API for FHIR beginnen.

Wenn Sie einen anderen Azure Active Directory-Mandanten verwenden (der nicht Ihrem Abonnement zugeordnet ist), können Sie die Azure API for FHIR-Ressourcenanwendung mit PowerShell in Ihren Mandanten importieren:

```azurepowershell-interactive
New-AzADServicePrincipal -ApplicationId 4f6778d8-5aef-43dc-a1ff-b073724b9495
```

Alternativ können Sie die Azure-Befehlszeilenschnittstelle verwenden:

```azurecli-interactive
az ad sp create --id 4f6778d8-5aef-43dc-a1ff-b073724b9495
```

## <a name="fhir-server-for-azure"></a>FHIR-Server für Azure

Wenn Sie den Open-Source-FHIR-Server für Azure verwenden, führen Sie die folgenden Schritte aus, um eine Ressourcenanwendung zu registrieren.

### <a name="app-registrations-in-azure-portal"></a>App-Registrierungen im Azure-Portal

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**.

2. Klicken Sie auf dem Blatt **Azure Active Directory** auf **App-Registrierungen** :

    ![Azure-Portal. Neue App-Registrierung.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Klicken Sie auf **Neue Registrierung**.

### <a name="add-a-new-application-registration"></a>Hinzufügen einer neuen Anwendungsregistrierung

Füllen Sie die Details für die neue Anwendung aus. Es gibt keine bestimmten Anforderungen an den Anzeigenamen, aber wenn er auf den URI des FHIR-Servers festgelegt wird, ist er leicht zu finden:

![Registrierung einer neuen Anwendung](media/how-to-aad/portal-aad-register-new-app-registration-NAME.png)

### <a name="set-identifier-uri-and-define-scopes"></a>Festlegen des Bezeichner-URIs und Definieren von Bereichen

Eine Ressourcenanwendung verfügt über einen Bezeichner-URI (Anwendungs-ID-URI), mit dem Clients den Zugriff auf die Ressource anfordern können. Dieser Wert füllt den `aud`-Anspruch des Zugriffstokens. Es wird empfohlen, dass Sie diesen URI so festlegen, dass er dem URI Ihres FHIR-Servers entspricht. Bei SMART für FHIR-Apps wird davon ausgegangen, dass die *Zielgruppe* der URI des FHIR-Servers ist.

1. Klicken Sie auf **Eine API verfügbar machen**.

2. Klicken Sie neben *Anwendungs-ID-URI* auf **Festlegen**.

3. Geben Sie den Bezeichner-URI ein, und klicken Sie auf **Speichern**. Ein geeigneter Bezeichner-URI wäre der URI Ihres FHIR-Servers.

4. Klicken Sie auf **Bereich hinzufügen** , und fügen Sie alle Bereiche hinzu, die Sie für Ihre API definieren möchten. Sie müssen mindestens einen Bereich hinzufügen, um Ihrer Ressourcenanwendung in Zukunft Berechtigungen erteilen zu können. Wenn Sie keine bestimmten Bereiche hinzufügen möchten, können Sie „user_impersonation“ hinzufügen.

![Zielgruppe und Bereich](media/how-to-aad/portal-aad-register-new-app-registration-AUD-SCOPE.png)

### <a name="define-application-roles"></a>Definieren der Anwendungsrollen

Azure API for FHIR und der OSS FHIR-Server für Azure verwenden [Azure Active Directory-Anwendungsrollen](/azure/architecture/multitenant-identity/app-roles) für die rollenbasierte Zugriffssteuerung. Um festzulegen, welche Rollen für Ihre FHIR-Server-API verfügbar sein sollen, öffnen Sie das [Manifest](/azure/active-directory/active-directory-application-manifest/) der Ressourcenanwendung:

1. Klicken Sie auf **Manifest** :

    ![Anwendungsrollen](media/how-to-aad/portal-aad-register-new-app-registration-APP-ROLES.png)

2. Fügen Sie in der `appRoles`-Eigenschaft die Rollen hinzu, die Benutzer oder Anwendungen erhalten sollen:

    ```json
    "appRoles": [
      {
        "allowedMemberTypes": [
          "User",
          "Application"
        ],
        "description": "FHIR Server Administrators",
        "displayName": "admin",
        "id": "1b4f816e-5eaf-48b9-8613-7923830595ad",
        "isEnabled": true,
        "value": "admin"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "description": "Users who can read",
        "displayName": "reader",
        "id": "c20e145e-5459-4a6c-a074-b942bbd4cfe1",
        "isEnabled": true,
        "value": "reader"
      }
    ],
    ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie eine Ressourcenanwendung in Azure Active Directory registrieren. Als Nächstes registrieren Sie Ihre vertrauliche Clientanwendung.
 
>[!div class="nextstepaction"]
>[Registrieren einer vertraulichen Clientanwendung](register-confidential-azure-ad-client-app.md)