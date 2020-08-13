---
title: 'Einladen von internen Benutzern zur B2B-Zusammenarbeit: Azure AD'
description: Wenn Sie interne Benutzerkonten für Partner, Distributoren, Lieferanten, Anbieter und andere Gäste verwenden, können Sie die Umstellung auf die Azure AD B2B-Zusammenarbeit durchführen, indem Sie die Gäste zur Anmeldung mit ihren eigenen externen Anmeldeinformationen einladen. Verwenden Sie entweder PowerShell oder die Einladungs-API von Microsoft Graph.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 04/12/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: c03c2c55988df04cc45ef4a1d66d959513c1626d
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87907451"
---
# <a name="invite-internal-users-to-b2b-collaboration"></a>Einladen von internen Benutzern zur B2B-Zusammenarbeit

> [!NOTE]
> Die Einladung interner Benutzer zur Verwendung der B2B-Zusammenarbeit ist ein Feature von Azure Active Directory, das sich derzeit in der öffentlichen Vorschauphase befindet. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bevor die Azure AD B2B-Zusammenarbeit verfügbar war, konnten Organisationen mit Distributoren, Lieferanten, Anbietern und anderen Gästen zusammenarbeiten, indem sie interne Anmeldeinformationen für sie eingerichtet haben. Falls Sie über interne Gastbenutzer dieser Art verfügen, können Sie diese zur B2B-Zusammenarbeit einladen, damit Sie in den Genuss der Vorteile von Azure AD B2B kommen. Ihre B2B-Gastbenutzer können ihre eigenen Identitäten und Anmeldeinformationen nutzen, um sich anzumelden, und Sie müssen keine Kennwörter verwalten oder Lebenszyklen von Konten pflegen.

Das Senden einer Einladung an ein vorhandenes internes Konto ermöglicht es Ihnen, die Objekt-ID, den UPN, Gruppenmitgliedschaften und App-Zuweisungen für den Benutzer beizubehalten. Sie müssen den Benutzer nicht manuell löschen und erneut einladen und auch die Ressourcen nicht neu zuweisen. Zum Einladen des Benutzers verwenden Sie die Einladungs-API, um zusammen mit der Einladung sowohl das interne Benutzerobjekt als auch die E-Mail-Adresse des Gastbenutzers zu übergeben. Wenn der Benutzer die Einladung annimmt, stellt der B2B-Dienst das vorhandene interne Benutzerobjekt auf einen B2B-Benutzer um. Danach muss sich der Benutzer bei Cloudressourcendiensten mit seinen B2B-Anmeldeinformationen anmelden. Er kann seine internen Anmeldeinformationen weiterhin verwenden, um auf lokale Ressourcen zuzugreifen. Sie können dies aber verhindern, indem Sie das Kennwort für das interne Konto zurücksetzen oder ändern.

> [!NOTE]
> Die Einladung ist unidirektional. Sie können interne Benutzer zur Verwendung der B2B-Zusammenarbeit einladen, aber Sie können die B2B-Anmeldeinformationen nach dem Hinzufügen nicht mehr entfernen. Um für den Benutzer wieder die Umstellung auf einen rein internen Benutzer durchzuführen, müssen Sie das Benutzerobjekt löschen und ein neues erstellen.

Während der öffentlichen Vorschauphase kann das Verfahren, das in diesem Artikel zum Einladen von internen Benutzern zur B2B-Zusammenarbeit beschrieben ist, in den folgenden Fällen nicht genutzt werden:

- Dem internen Benutzer ist bereits eine Exchange-Lizenz zugewiesen.
- Der Benutzer stammt aus einer Domäne, für die der direkte Verbund in Ihrem Verzeichnis eingerichtet ist.
- Der interne Benutzer verfügt über ein reines Cloudkonto, und sein Hauptkonto befindet sich nicht in Azure AD.

Falls der interne Benutzer auf einen B2B-Benutzer umgestellt werden muss, sollten Sie in diesen Fällen das interne Konto löschen und eine Einladung zur B2B-Zusammenarbeit an den Benutzer senden.

**Lokal synchronisierte Benutzer**: Für Benutzerkonten, die zwischen dem lokalen Standort und der Cloud synchronisiert werden, bleibt das lokale Verzeichnis die Autoritätsquelle, nachdem die Einladung zur B2B-Zusammenarbeit erfolgt ist. Alle Änderungen, die Sie am lokalen Konto vornehmen, werden mit dem Cloudkonto synchronisiert, z. B. das Deaktivieren oder Löschen des Kontos. Aus diesem Grund können Sie durch das einfache Löschen des lokalen Kontos nicht verhindern, dass sich der Benutzer bei seinem lokalen Konto anmeldet, während das Cloudkonto beibehalten wird. Sie können aber das Kennwort für das lokale Konto auf eine zufällige GUID oder einen anderen unbekannten Wert festlegen.

## <a name="how-to-invite-internal-users-to-b2b-collaboration"></a>Gewusst wie: Einladen von internen Benutzern zur B2B-Zusammenarbeit

Sie können PowerShell oder die Einladungs-API nutzen, um eine B2B-Einladung an einen internen Benutzer zu senden. Stellen Sie sicher, dass die E-Mail-Adresse, die Sie für die Einladung verwenden möchten, für das interne Benutzerobjekt als externe E-Mail-Adresse festgelegt ist.

- Verwenden Sie für einen reinen Cloudbenutzer die E-Mail-Adresse in der „User.OtherMails“-Eigenschaft für die Einladung.
- Für einen lokal synchronisierten Benutzer müssen Sie den Wert in der „User.Mail“-Eigenschaft für die Einladung verwenden.
- Die Domäne in der „Mail“-Eigenschaft des Benutzers muss mit dem Konto übereinstimmen, dass dieser für die Anmeldung verwendet. Andernfalls kann der Benutzer von einigen Diensten, z. B. Teams, nicht authentifiziert werden.

Standardmäßig wird dem Benutzer beim Einladevorgang eine E-Mail mit einem Hinweis zur Einladung gesendet, aber Sie können diese E-Mail auch unterdrücken und eine eigene Nachricht senden.

> [!NOTE]
> Zum Senden einer eigenen E-Mail oder anderen Nachricht können Sie „New-AzureADMSInvitation“ mit „-SendInvitationMessage:$false“ verwenden, um Benutzer im Hintergrund einzuladen. Anschließend können Sie dann Ihre eigene E-Mail an den Benutzer senden, für den die Umstellung durchgeführt wurde. Weitere Informationen finden Sie unter [Azure AD B2B-Zusammenarbeit: API und Anpassung](customize-invitation-api.md).

## <a name="use-powershell-to-send-a-b2b-invitation"></a>Verwenden von PowerShell zum Senden einer B2B-Einladung

Verwenden Sie den folgenden Befehl, um den Benutzer zur B2B-Zusammenarbeit einzuladen:

```powershell
Uninstall-Module AzureADPreview
Install-Module AzureADPreview
$ADGraphUser = Get-AzureADUser -objectID "UPN of Internal User"
$msGraphUser = New-Object Microsoft.Open.MSGraph.Model.User -ArgumentList $ADGraphUser.ObjectId
New-AzureADMSInvitation -InvitedUserEmailAddress <<external email>> -SendInvitationMessage $True -InviteRedirectUrl "http://myapps.microsoft.com" -InvitedUser $msGraphUser
```

## <a name="use-the-invitation-api-to-send-a-b2b-invitation"></a>Verwenden der Einladungs-API zum Senden einer B2B-Einladung

Das folgende Beispiel veranschaulicht, wie Sie die Einladungs-API aufrufen, um einen internen Benutzer als B2B-Benutzer einzuladen.

```json
POST https://graph.microsoft.com/v1.0/invitations
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
        "id": "<<ID for the user you want to convert>>"
    }
}
```

Die Antwort auf die API entspricht der Antwort, die Sie erhalten, wenn Sie einen neuen Gastbenutzer für das Verzeichnis einladen.

## <a name="next-steps"></a>Nächste Schritte

- [B2B-Zusammenarbeit: Einlösen von Einladungen](redemption-experience.md)
