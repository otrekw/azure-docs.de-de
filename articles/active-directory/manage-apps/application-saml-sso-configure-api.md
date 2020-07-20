---
title: Verwenden von Microsoft Graph-APIs zum Konfigurieren von SAML-basiertem einmaligem Anmelden
titleSuffix: Azure Active Directory
description: Sie müssen SAML-basiertes einmaliges Anmelden für mehrere Instanzen einer Anwendung einrichten? Erfahren Sie, wie Sie Zeit sparen können, indem Sie die Konfiguration von SAML-basiertem einmaligem Anmelden mithilfe von Microsoft Graph-APIs automatisieren.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.date: 05/19/2020
ms.author: kenwith
ms.reviewer: luleon
ms.openlocfilehash: 50ee9e3c22c885931e2586f65ba2fa3353fccfeb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85355844"
---
# <a name="automate-saml-based-sso-app-configuration-with-microsoft-graph-api"></a>Automatisieren der Konfiguration der SAML-basierten SSO-App mit der Microsoft Graph-API

In diesem Artikel erfahren Sie, wie Sie eine Anwendung aus dem Azure Active Directory-Katalog (Azure AD) erstellen und konfigurieren. In diesem Artikel wird AWS als Beispiel verwendet, aber Sie können die Schritte in diesem Artikel für alle SAML-basierten Apps im Azure AD-Katalog verwenden.

**Schritte zum Verwenden der Microsoft Graph-APIs zum Automatisieren der Konfiguration von SAML-basiertem einmaligem Anmelden**

| Schritt  | Details  |
|---------|---------|
| [1. Erstellen der Kataloganwendung](#step-1-create-the-gallery-application) | Anmelden beim API-Client <br> Abrufen der Kataloganwendung <br> Erstellen der Kataloganwendung|
| [2. Konfigurieren von einmaligem Anmelden](#step-2-configure-single-sign-on) | Abrufen der App-Objekt-ID und Dienstprinzipalobjekt-ID <br> Festlegen des SSO-Modus <br> Festlegen grundlegender SAML-URLs, etwa Bezeichner, Antwort-URL, Anmelde-URL <br> Hinzufügen von App-Rollen (optional)|
| [3. Konfigurieren der Anspruchszuordnung](#step-3-configure-claims-mapping) | Erstellen einer Anspruchszuordnungsrichtlinie <br> Zuweisen einer Anspruchszuordnungs richtlinie zum Dienstprinzipal|
| [4. Konfigurieren des Signaturzertifikats](#step-4-configure-signing-certificate) | Erstellen eines Zertifikats <BR> Hinzufügen eines benutzerdefinierten Signaturschlüssels <br> Aktivieren des benutzerdefinierten Signaturschlüssels|
| [5. Zuweisen von Benutzern](#step-5-assign-users) | Zuweisen von Benutzern und Gruppen zur Anwendung
| [6. Konfigurieren der Anwendungsseite](#step-6-configure-the-application-side)| Abrufen von Azure AD-SAML-Metadaten

**Liste aller in der Dokumentation verwendeten APIs**

Stellen Sie sicher, dass Sie über die entsprechenden Berechtigungen verfügen, um die folgenden APIs aufzurufen.

|Ressourcentyp |Methode |
|---------|---------|
|[applicationTemplate](https://docs.microsoft.com/graph/api/resources/applicationtemplate?view=graph-rest-beta)|[Auflisten von applicationTemplate](https://docs.microsoft.com/graph/api/applicationtemplate-list?view=graph-rest-beta&tabs=http) <br>[Instanziieren von applicationTemplate](https://docs.microsoft.com/graph/api/applicationtemplate-instantiate?view=graph-rest-beta&tabs=http)|
|[servicePrincipals](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-1.0)|[Aktualisieren von servicePrincipal](https://docs.microsoft.com/graph/api/serviceprincipal-update?view=graph-rest-1.0&tabs=http) <br> [Erstellen von appRoleAssignments](https://docs.microsoft.com/graph/api/serviceprincipal-post-approleassignments?view=graph-rest-1.0&tabs=http) <br> [Zuweisen von claimsMappingPolicies](https://docs.microsoft.com/graph/api/serviceprincipal-post-claimsmappingpolicies?view=graph-rest-beta&tabs=http)|
|[applications](https://docs.microsoft.com/graph/api/resources/application?view=graph-rest-1.0)|[Aktualisieren einer Anwendung](https://docs.microsoft.com/graph/api/application-update?view=graph-rest-1.0&tabs=http)|
|[claimsMappingPolicy](https://docs.microsoft.com/graph/api/resources/claimsmappingpolicy?view=graph-rest-beta)| [Erstellen von claimsMappingPolicy](https://docs.microsoft.com/graph/api/claimsmappingpolicy-post-claimsmappingpolicies?view=graph-rest-beta&tabs=http)

>[!NOTE]
>Die in diesem Artikel gezeigten Antwortobjekte werden möglicherweise zur besseren Lesbarkeit verkürzt dargestellt. Von einem tatsächlichen Aufruf werden alle Eigenschaften zurückgegeben.

## <a name="step-1-create-the-gallery-application"></a>Schritt 1: Erstellen der Kataloganwendung

### <a name="sign-in-to-microsoft-graph-explorer-recommended-postman-or-any-other-api-client-you-use"></a>Melden Sie sich bei Microsoft Graph-Tester (empfohlen), Postman oder einem beliebigen anderen API-Client an, den Sie verwenden.

1. Öffnen Sie den [Microsoft Graph-Tester](https://developer.microsoft.com/graph/graph-explorer).
2. Wählen Sie die Schaltfläche **Mit Microsoft anmelden** aus, und melden Sie sich mit globalen Azure AD-Anmeldeinformationen oder mit Anmeldeinformationen für den App-Administrator an.
3. Nach der erfolgreichen Anmeldung werden im linken Bereich die Details des Benutzerkontos angezeigt.

### <a name="retrieve-the-gallery-application-template-identifier"></a>Abrufen des Bezeichners für die Kataloganwendungsvorlage

Anwendungen im Azure AD Anwendungskatalog verfügen jeweils über eine [Anwendungsvorlage](https://docs.microsoft.com/graph/api/applicationtemplate-list?view=graph-rest-beta&tabs=http), in der die Metadaten für die Anwendung beschrieben werden. Mithilfe dieser Vorlage können Sie im Mandanten für die Verwaltung eine Instanz der Anwendung und des Dienstprinzipals erstellen.

#### <a name="request"></a>Anforderung

<!-- {
  "blockType": "request",
  "name": "get_applicationtemplates"
}-->

```msgraph-interactive
GET https://graph.microsoft.com/beta/applicationTemplates
```

#### <a name="response"></a>Antwort

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.applicationTemplate",
  "isCollection": true
} -->

```http
HTTP/1.1 200 OK
Content-type: application/json

{
  "value": [
  {
    "id": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "displayName": "Amazon Web Services (AWS)",
        "homePageUrl": "http://aws.amazon.com/",
        "supportedSingleSignOnModes": [
             "password",
             "saml",
             "external"
         ],
         "supportedProvisioningTypes": [
             "sync"
         ],
         "logoUrl": "https://az495088.vo.msecnd.net/app-logo/aws_215.png",
         "categories": [
             "developerServices"
         ],
         "publisher": "Amazon",
         "description": null    
  
}
```

### <a name="create-the-gallery-application"></a>Erstellen der Kataloganwendung

Verwenden Sie die Vorlagen-ID, die Sie im letzten Schritt für die Anwendung abgerufen haben, um im Mandanten [eine Instanz](https://docs.microsoft.com/graph/api/applicationtemplate-instantiate?view=graph-rest-beta&tabs=http) der Anwendung und des Dienstprinzipals zu erstellen.

> [!NOTE] 
> Sie können die applicationTemplate-API verwenden, um [katalogfremde Apps](add-non-gallery-app.md) zu instanziieren. Verwenden Sie „applicationTemplateId `8adf8e6e-67b2-4cf2-a259-e3dc5476c621`“.
#### <a name="request"></a>Anforderung

<!-- {
  "blockType": "request",
  "name": "applicationtemplate_instantiate"
}-->

```msgraph-interactive
POST https://graph.microsoft.com/beta/applicationTemplates/8b1025e4-1dd2-430b-a150-2ef79cd700f5/instantiate
Content-type: application/json

{
  "displayName": "AWS Contoso"
}
```

#### <a name="response"></a>Antwort


<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.applicationServicePrincipal"
} -->

```http
HTTP/1.1 201 OK
Content-type: application/json


{
    "application": {
        "objectId": "cbc071a6-0fa5-4859-8g55-e983ef63df63",
        "appId": "92653dd4-aa3a-3323-80cf-e8cfefcc8d5d",
        "applicationTemplateId": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "displayName": "AWS Contoso",
        "homepage": "https://signin.aws.amazon.com/saml?metadata=aws|ISV9.1|primary|z",
        "replyUrls": [
            "https://signin.aws.amazon.com/saml"
        ],
        "logoutUrl": null,
        "samlMetadataUrl": null,
    },
    "servicePrincipal": {
        "objectId": "f47a6776-bca7-4f2e-bc6c-eec59d058e3e",
        "appDisplayName": "AWS Contoso",
        "applicationTemplateId": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "appRoleAssignmentRequired": true,
        "displayName": "My custom name",
        "homepage": "https://signin.aws.amazon.com/saml?metadata=aws|ISV9.1|primary|z",
        "replyUrls": [
            "https://signin.aws.amazon.com/saml"
        ],
        "servicePrincipalNames": [
            "93653dd4-aa3a-4323-80cf-e8cfefcc8d7d"
        ],
        "tags": [
            "WindowsAzureActiveDirectoryIntegratedApp"
        ],
    }
}
```

## <a name="step-2-configure-single-sign-on"></a>Schritt 2: Einmaliges Anmelden konfigurieren

### <a name="retrieve-app-object-id-and-service-principal-object-id"></a>Abrufen der App-Objekt-ID und Dienstprinzipalobjekt-ID

Verwenden Sie die Antwort aus dem vorherigen-Befehl, um die Anwendungsobjekt-ID und die Dienstprinzipalobjekt-ID abzurufen und zu speichern.

```
"application": {
        "objectId": "cbc071a6-0fa5-4859-8g55-e983ef63df63"
}
"servicePrincipal": {
        "objectId": "f47a6776-bca7-4f2e-bc6c-eec59d058e3e"
}
```
### <a name="set-single-sign-on-mode"></a>Festlegen des SSO-Modus

In diesem Beispiel legen Sie `saml` im [servicePrincipal-Ressourcentyp](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-1.0) als SSO-Modus fest. Andere SAML-SSO-Eigenschaften, die Sie konfigurieren können, sind `notificationEmailAddresses`, `loginUrl` und `samlSingleSignOnSettings.relayState`.

#### <a name="request"></a>Anforderung

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
PATCH https://graph.microsoft.com/v1.0/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e
Content-type: servicePrincipal/json

{
    "preferredSingleSignOnMode": "saml"
}
```

#### <a name="response"></a>Antwort

<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 204
```

### <a name="set-basic-saml-urls-such-as-identifier-reply-url-sign-on-url"></a>Festlegen grundlegender SAML-URLs, etwa Bezeichner, Antwort-URL, Anmelde-URL

Legen Sie die Bezeichner- und Antwort-URLs für AWS im Anwendungsobjekt fest.

#### <a name="request"></a>Anforderung

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
PATCH https://graph.microsoft.com/v1.0/applications/cbc071a6-0fa5-4859-8g55-e983ef63df63
Content-type: applications/json

{
    "web": {
        "redirectUris": [
            "https://signin.aws.amazon.com/saml"
        ] 
    },
    "identifierUris": [
        "https://signin.aws.amazon.com/saml"
    ]    
}
```
#### <a name="response"></a>Antwort

<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 204
```
### <a name="add-app-roles-optional"></a>Hinzufügen von App-Rollen (optional)

Wenn die Anwendung die Rolleninformationen im Token benötigt, fügen Sie die Definition der Rollen im Anwendungsobjekt hinzu. Für AWS können Sie [Benutzerbereitstellung aktivieren](../app-provisioning/application-provisioning-configure-api.md), um alle Rollen aus diesem AWS-Konto abzurufen. 

Weitere Informationen finden Sie unter [Konfigurieren von im SAML-Token ausgestellten Rollenansprüchen](../develop/active-directory-enterprise-app-role-management.md).

> [!NOTE] 
> Wenn Sie App-Rollen hinzufügen, ändern Sie msiam_access der Standard-App-Rollen nicht. 

#### <a name="request"></a>Anforderung

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
PATCH https://graph.microsoft.com/v1.0/serviceprincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e
Content-type: serviceprincipals/json

{
"appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "454dc4c2-8176-498e-99df-8c4efcde41ef",
            "isEnabled": true,
            "value": "arn:aws:iam::212743507312:role/accountname-aws-admin,arn:aws:iam::212743507312:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Finance,WAAD",
            "displayName": "Finance,WAAD",
            "id": "8642d5fa-18a3-4245-ab8c-a96000c1a217",
            "isEnabled": true,
            "value": "arn:aws:iam::212743507312:role/accountname-aws-finance,arn:aws:iam::212743507312:saml-provider/WAAD"
        }
    ]

}
```

#### <a name="response"></a>Antwort

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```

## <a name="step-3-configure-claims-mapping"></a>Schritt 3: Konfigurieren der Anspruchszuordnung

### <a name="create-claims-mapping-policy"></a>Erstellen einer Anspruchszuordnungsrichtlinie

Zusätzlich zu den grundlegenden Ansprüchen konfigurieren Sie die folgenden Ansprüche für Azure AD, die im SAML-Token ausgegeben werden:

| Anspruchsname | `Source`  |
|---------|---------|
| `https://aws.amazon.com/SAML/Attributes/Role` | assignetzdroles| 
| `https://aws.amazon.com/SAML/Attributes/RoleSessionName` | userprincipalname |
| `https://aws.amazon.com/SAML/Attributes/SessionDuration` | „900“ |
| roles | assignetzdroles |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` | userprincipalname |

Weitere Informationen finden Sie unter [Gewusst wie: Anpassen von in Token ausgegebenen Ansprüchen für eine bestimmte App in einem Mandanten (Vorschau)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping).

#### <a name="request"></a>Anforderung

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
POST https://graph.microsoft.com/v1.0/policies/claimsMappingPolicies
Content-type: claimsMappingPolicies/json

{
    "definition":[
            "{\"ClaimsMappingPolicy\": {
                \"Version\": 1,
                \"IncludeBasicClaimSet\": \"true\",
                \"ClaimsSchema\": [
                    {
                        \"Source\": \"user\",
                        \"ID\": \"assignedroles\",
                        \"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/Role\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"userprincipalname\",
                        \"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/RoleSessionName\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"900\",
                        \"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/SessionDuration\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"assignedroles\",
                        \"SamlClaimType\": \"appRoles\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"userprincipalname\",
                        \"SamlClaimType\": \"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier\"
                    }
                ]
            }
        }"

    ],
    "displayName": "AWS Claims policy",
    "isOrganizationDefault": false
}
```

#### <a name="response"></a>Antwort

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.claimsMappingPolicies",
  "isCollection": true
} -->

```http
HTTP/1.1 200 OK
Content-type: claimsMappingPolicies/json

{
    "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#policies/claimsMappingPolicies/$entity",
    "id": "6b33aa8e-51f3-41a6-a0fd-d660d276197a",
    "definition": [
        "{\"ClaimsMappingPolicy\": {\"Version\": 1,\"IncludeBasicClaimSet\": \"true\",\"ClaimsSchema\": [{\"Source\": \"user\",\"ID\": \"assignedroles\",\"SamlClaimType\":\"https://aws.amazon.com/SAML/Attributes/Role\"\r\n                    },{\"Source\": \"user\",\"ID\": \"userprincipalname\",\"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/RoleSessionName\"},{\"Source\": \"user\",\"ID\": \"900\",\"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/SessionDuration\"},{\"Source\": \"user\",\"ID\": \"assignedroles\",\"SamlClaimType\":\"appRoles\"},{\"Source\": \"user\",\"ID\": \"userprincipalname\",\"SamlClaimType\": \"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier\"}]}}"
    ],
    "displayName": "AWS Claims policy",
    "isOrganizationDefault": false
}
```

### <a name="assign-claims-mapping-policy-to-service-principal"></a>Zuweisen einer Anspruchszuordnungs richtlinie zum Dienstprinzipal

#### <a name="request"></a>Anforderung

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
POST https://graph.microsoft.com/beta/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e/claimsMappingPolicies/$ref

Content-type: claimsMappingPolicies/json

{
  "@odata.id":"https://graph.microsoft.com/v1.0/policies/claimsMappingPolicies/6b33aa8e-51f3-41a6-a0fd-d660d276197a"
}
```

#### <a name="response"></a>Antwort

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```

## <a name="step-4-configure-signing-certificate"></a>Schritt 4: Konfigurieren des Signaturzertifikats

Wenn Sie die [applicationTemplate](https://docs.microsoft.com/graph/api/resources/applicationtemplate?view=graph-rest-beta)-API verwenden, wird standardmäßig kein Signaturzertifikat erstellt. Erstellen Sie Ihr benutzerdefiniertes Signaturzertifikat, und weisen Sie es der Anwendung zu. 

### <a name="create-a-custom-signing-certificate"></a>Erstellen eines benutzerdefinierten Signaturzertifikats

Zum Testen können Sie den folgenden PowerShell-Befehl verwenden, um ein selbstsigniertes Zertifikat abzurufen. Verwenden Sie die bewährte Sicherheitsmethode Ihres Unternehmens, um ein Signaturzertifikat für die Produktion zu erstellen.

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [string]$fqdn,
    [Parameter(Mandatory=$true)]
    [string]$pwd,
    [Parameter(Mandatory=$true)]
    [string]$location
) 

if (!$PSBoundParameters.ContainsKey('location'))
{
    $location = "."
} 

$cert = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -DnsName $fqdn
$pwdSecure = ConvertTo-SecureString -String $pwd -Force -AsPlainText
$path = 'cert:\currentuser\my\' + $cert.Thumbprint
$cerFile = $location + "\\" + $fqdn + ".cer"
$pfxFile = $location + "\\" + $fqdn + ".pfx" 

Export-PfxCertificate -cert $path -FilePath $pfxFile -Password $pwdSecure
Export-Certificate -cert $path -FilePath $cerFile
```

### <a name="add-a-custom-signing-key"></a>Hinzufügen eines benutzerdefinierten Signaturschlüssels

Fügen Sie dem Dienstprinzipal die folgenden Informationen hinzu:

* Privater Schlüssel
* Kennwort
* Öffentlicher Schlüssel 

Extrahieren Sie den privaten und öffentlichen Schlüssel Base64-codiert aus der PFX-Datei. Weitere Informationen zu den Eigenschaften finden Sie unter [Ressourcentyp keyCredential](https://docs.microsoft.com/graph/api/resources/keycredential?view=graph-rest-1.0).

Stellen Sie sicher, dass die für „Sign“ verwendete keyId mit der keyId von passwordCredential übereinstimmt.

Sie können `customkeyIdentifier` generieren, indem Sie den Hash des Zertifikatfingerabdrucks abrufen.

```csharp
  public string GetSha256FromThumbprint(string thumbprint)
  {
      var message = Encoding.ASCII.GetBytes(thumbprint);
      SHA256Managed hashString = new SHA256Managed();
      return Convert.ToBase64String(hashString.ComputeHash(message));
  }
```

#### <a name="request"></a>Anforderung

> [!NOTE]
> Der Wert „key“ in der keyCredentials-Eigenschaft wird zur besseren Lesbarkeit gekürzt. Der Wert ist Base64-codiert. Für den privaten Schlüssel hat die Eigenschaft `usage` den Wert „Sign“. Für den öffentlichen Schlüssel hat die Eigenschaft `usage` den Wert „Verify“.

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
PATCH https://graph.microsoft.com/v1.0/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e

Content-type: servicePrincipals/json

{
    "keyCredentials":[
        {
            "customKeyIdentifier": "lY85bR8r6yWTW6jnciNEONwlVhDyiQjdVLgPDnkI5mA=",
            "endDateTime": "2021-04-22T22:10:13Z",
            "keyId": "4c266507-3e74-4b91-aeba-18a25b450f6e",
            "startDateTime": "2020-04-22T21:50:13Z",
            "type": "AsymmetricX509Cert",
            "usage": "Sign",
            "key":"MIIKIAIBAz.....HBgUrDgMCERE20nuTptI9MEFCh2Ih2jaaLZBZGeZBRFVNXeZmAAgIH0A==",
            "displayName": "CN=awsAPI"
        },
        {
            "customKeyIdentifier": "lY85bR8r6yWTW6jnciNEONwlVhDyiQjdVLgPDnkI5mA=",
            "endDateTime": "2021-04-22T22:10:13Z",
            "keyId": "e35a7d11-fef0-49ad-9f3e-aacbe0a42c42",
            "startDateTime": "2020-04-22T21:50:13Z",
            "type": "AsymmetricX509Cert",
            "usage": "Verify",
            "key": "MIIDJzCCAg+gAw......CTxQvJ/zN3bafeesMSueR83hlCSyg==",
            "displayName": "CN=awsAPI"
        }

    ],
    "passwordCredentials": [
        {
            "customKeyIdentifier": "lY85bR8r6yWTW6jnciNEONwlVhDyiQjdVLgPDnkI5mA=",
            "keyId": "4c266507-3e74-4b91-aeba-18a25b450f6e",
            "endDateTime": "2022-01-27T19:40:33Z",
            "startDateTime": "2020-04-20T19:40:33Z",
            "secretText": "61891f4ee44d"
        }
    ]
}
```

#### <a name="response"></a>Antwort

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```

### <a name="activate-the-custom-signing-key"></a>Aktivieren des benutzerdefinierten Signaturschlüssels

Sie müssen die `preferredTokenSigningKeyThumbprint`-Eigenschaft auf den Fingerabdruck des Zertifikats festlegen, das Azure AD zum Signieren der SAML-Antwort verwenden soll. 

#### <a name="request"></a>Anforderung

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
PATCH https://graph.microsoft.com/v1.0/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e

Content-type: servicePrincipals/json

{
    "preferredTokenSigningKeyThumbprint": "AC09FEF18DDE6983EE2A164FBA3C4DD7518BD787"
}
```

#### <a name="response"></a>Antwort

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```
## <a name="step-5-assign-users"></a>Schritt 5: Benutzer zuweisen

### <a name="assign-users-and-groups-to-the-application"></a>Zuweisen von Benutzern und Gruppen zur Anwendung

Weisen Sie den folgenden Benutzer dem Dienstprinzipal zu, und weisen Sie die AWS_Role1 zu. 

| Name  | id  |
|---------|---------|
| Benutzer-ID (principalId) | 6cad4079-4e79-4a3f-9efb-ea30a14bdb26 |
| Typ (principalType) | Benutzer |
| App-Rollen-ID (appRoleId) | 454dc4c2-8176-498e-99df-8c4efcde41ef |
| servicePrincipalID (resourceId) | 515f62cb-d18a-4dca-bec3-bb0bf31deeea |

#### <a name="request"></a>Anforderung

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
POST https://graph.microsoft.com/v1.0/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e/appRoleAssignments

Content-type: appRoleAssignments/json

{
  "principalId": "6cad4079-4e79-4a3f-9efb-ea30a14bdb26",
  "principalType": "User",
  "appRoleId":"454dc4c2-8176-498e-99df-8c4efcde41ef",
  "resourceId":"515f62cb-d18a-4dca-bec3-bb0bf31deeea"
}
```
#### <a name="response"></a>Antwort

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 201 
Content-type: appRoleAssignments/json

{
    "id": "rq7hyzl4yECaNZleMrTpDV-OCe5TEl5Ao_o76XMrRFU",
    "creationTimestamp": "2020-04-23T17:38:13.2508567Z",
    "appRoleId": "454dc4c2-8176-498e-99df-8c4efcde41ef",
    "principalDisplayName": "User 1",
    "principalId": "6cad4079-4e79-4a3f-9efb-ea30a14bdb26",
    "principalType": "User",
    "resourceDisplayName": "AWS API Created",
    "resourceId": "515f62cb-d18a-4dca-bec3-bb0bf31deeea"
}
```

Weitere Informationen finden Sie unter Ressourcentyp [appRoleAssignment](https://docs.microsoft.com/graph/api/resources/approleassignment?view=graph-rest-1.0).

## <a name="step-6-configure-the-application-side"></a>Schritt 6: Konfigurieren der Anwendungsseite

### <a name="get-azure-ad-saml-metadata"></a>Abrufen von Azure AD-SAML-Metadaten

Verwenden Sie die folgende URL, um die Azure AD-SAML-Metadaten für die jeweilige konfigurierte Anwendung abzurufen. Die Metadaten enthalten Informationen wie das Signaturzertifikat, Azure AD-entityID, Azure AD-SingleSignOnService usw.

https://login.microsoftonline.com/{tenant-id}/federationmetadata/2007-06/federationmetadata.xml?appid={app-id}

## <a name="next-steps"></a>Nächste Schritte
- [Verwenden von Microsoft Graph-APIs zum Konfigurieren der Benutzerbereitstellung](../app-provisioning/application-provisioning-configure-api.md)
- [Verwenden des AD FS-Anwendungsaktivitätsberichts, um Anwendungen zu Azure AD zu migrieren](migrate-adfs-application-activity.md)
