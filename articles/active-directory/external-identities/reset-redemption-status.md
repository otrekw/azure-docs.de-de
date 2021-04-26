---
title: Zurücksetzen des Einlösestatus eines Gastbenutzers – Azure AD
description: Hier erfahren Sie, wie Sie den Einlösestatus der Einladung für einen Azure Active Directory-B2B-Gastbenutzer in Azure AD External Identities zurücksetzen.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 04/06/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.collection: M365-identity-device-management
ms.openlocfilehash: d0396698fe63cb62fc1cfaf5d930b8a97a7b1bbc
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106552256"
---
# <a name="reset-redemption-status-for-a-guest-user-preview"></a>Zurücksetzen des Einlösestatus für einen Gastbenutzer (Vorschau)

Wenn ein Gastbenutzer Ihre Einladung zu B2B-Zusammenarbeit eingelöst hat, kann es vorkommen, dass Sie seine Anmeldeinformationen aktualisieren müssen, z. B. in den folgenden Fällen:

- Der Benutzer möchte sich mit einer anderen E-Mail-Adresse und einem anderen Identitätsanbieter anmelden.
- Das Konto des Benutzers im Privatmandanten wurde gelöscht und neu erstellt.
- Der Benutzer ist zu einem anderen Unternehmen gewechselt, benötigt aber trotzdem weiterhin denselben Zugriff auf Ihre Ressourcen.
- Die Zuständigkeiten des Benutzers wurden auf einen anderen Benutzer übertragen.

Früher mussten Sie zum Verwalten dieser Szenarios das Konto des Gastbenutzers manuell aus Ihrem Verzeichnis löschen und den Benutzer erneut einladen. Nun können Sie den Einlösestatus des Benutzers mit PowerShell oder der Einladungs-API von Microsoft Graph zurücksetzen und den Benutzer wieder einladen und gleichzeitig die Objekt-ID, die Gruppenmitgliedschaften und die App-Zuweisungen des Benutzers beibehalten. Wenn der Benutzer die neue Einladung einlöst, ändert sich der UPN des Benutzers nicht. Der Anmeldename des Benutzers wird jedoch in die neue E-Mail-Adresse geändert. Der Benutzer kann sich anschließend mithilfe der neuen E-Mail-Adresse oder einer E-Mail-Adresse anmelden, die Sie der Eigenschaft `otherMails` des Benutzerobjekts hinzugefügt haben.

## <a name="reset-the-email-address-used-for-sign-in"></a>Zurücksetzen der für die Anmeldung verwendeten E-Mail-Adresse

Wenn ein Benutzer sich mit einer anderen E-Mail-Adresse anmelden möchte:

1. Stellen Sie sicher, dass die neue E-Mail-Adresse der `mail`- oder `otherMails`-Eigenschaft des Benutzerobjekts hinzugefügt wird. 
2.  Ersetzen Sie die E-Mail-Adresse der `InvitedUserEmailAddress`-Eigenschaft durch die neue E-Mail-Adresse.
3. Verwenden Sie eine der folgenden Methoden, um den Einlösestatus des Benutzers zurückzusetzen.

> [!NOTE]
>Wenn Sie während der öffentlichen Vorschau die E-Mail-Adresse des Benutzers zurücksetzen, sollten Sie für die `mail`-Eigenschaft die neue E-Mail-Adresse festlegen. Auf diese Weise kann der Benutzer die Einladung einlösen, indem er sich zusätzlich zur Verwendung des Einlösungslinks in der Einladung bei Ihrem Verzeichnis anmeldet.
>
## <a name="use-powershell-to-reset-redemption-status"></a>Zurücksetzen des Einlösestatus mithilfe von PowerShell

Installieren Sie das aktuelle PowerShell-Modul „AzureADPreview“, und erstellen Sie eine neue Einladung, wobei `InvitedUserEmailAddress` auf die neue E-Mail-Adresse und `ResetRedemption` auf `true` festgelegt ist.

```powershell  
Uninstall-Module AzureADPreview 
Install-Module AzureADPreview 
Connect-AzureAD 
$ADGraphUser = Get-AzureADUser -objectID "UPN of User to Reset"  
$msGraphUser = New-Object Microsoft.Open.MSGraph.Model.User -ArgumentList $ADGraphUser.ObjectId 
New-AzureADMSInvitation -InvitedUserEmailAddress <<external email>> -SendInvitationMessage $True -InviteRedirectUrl "http://myapps.microsoft.com" -InvitedUser $msGraphUser -ResetRedemption $True 
```

## <a name="use-microsoft-graph-api-to-reset-redemption-status"></a>Zurücksetzen des Einlösestatus mithilfe der Microsoft Graph-API

Legen Sie mithilfe der [Microsoft Graph-Einladungs-API](/graph/api/resources/invitation?view=graph-rest-1.0) die Eigenschaft `resetRedemption` auf `true` fest, und geben Sie die neue E-Mail-Adresse in der Eigenschaft `invitedUserEmailAddress` an.

```json
POST https://graph.microsoft.com/beta/invitations  
Authorization: Bearer eyJ0eX...  
ContentType: application/json  
{  
   "invitedUserEmailAddress": "<<external email>>",  
   "sendInvitationMessage": true,  
   "invitedUserMessageInfo": {  
      "messageLanguage": "en-US",  
      "ccRecipients": [  
         {  
            "emailAddress": {  
               "name": null,  
               "address": "<<optional additional notification email>>"  
            }  
         } 
      ],  
      "customizedMessageBody": "<<custom message>>"  
},  
"inviteRedirectUrl": "https://myapps.microsoft.com?tenantId=",  
"invitedUser": {  
   "id": "<<ID for the user you want to reset>>"  
}, 
"resetRedemption": true 
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Active Directory B2B collaboration API and customization](customize-invitation-api.md#powershell) (Azure Active Directory B2B-Zusammenarbeit: API und Anpassung)
- [Eigenschaften eines Azure AD-B2B-Gastbenutzers](user-properties.md)
