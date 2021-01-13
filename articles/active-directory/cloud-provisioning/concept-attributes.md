---
title: Grundlegendes zum Azure AD-Schema und zu benutzerdefinierten Ausdrücken
description: In diesem Artikel werden das Azure AD-Schema, die Attribute, die der Bereitstellungs-Agent weiterleitet, und benutzerdefinierte Ausdrücke beschrieben.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/18/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1fff90777c63820b54a1cb37156021c894de19c8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89226929"
---
# <a name="understand-the-azure-ad-schema"></a>Grundlegendes zum Azure AD-Schema
Ein Objekt in Azure Active Directory (Azure AD) ist wie jedes Verzeichnis ein programmgesteuertes allgemeines Datenkonstrukt, das Elemente wie Benutzer, Gruppen und Kontakte darstellt. Wenn Sie einen neuen Benutzer oder Kontakt in Azure AD erstellen, erstellen Sie eine neue Instanz dieses Objekts. Diese Instanzen können anhand ihrer Eigenschaften unterschieden werden.

Eigenschaften in Azure AD sind die Elemente, die zum Speichern von Informationen zu einer Instanz eines Objekts in Azure AD dienen.

Das Azure AD-Schema definiert die Regeln, nach denen Eigenschaften in einem Eintrag verwendet werden können, die Art der Werte, die diese Eigenschaften haben können, und wie Benutzer mit diesen Werten interagieren können. 

Azure AD verfügt über zwei Arten von Eigenschaften:
- **Integrierte Eigenschaften**: Eigenschaften, die durch das Azure AD-Schema vordefiniert sind. Diese Eigenschaften (mit oder ohne Zugriffsmöglichkeit) dienen unterschiedlichen Verwendungszwecken.
- **Verzeichniserweiterungen**: Bereitgestellte Eigenschaften, mit denen Sie Azure AD für die eigene Verwendung anpassen können. Wenn Sie beispielsweise Ihr lokales Azure Active Directory um ein bestimmtes Attribut erweitert haben und dieses Attribut weiterleiten möchten, können Sie eine der bereitgestellten benutzerdefinierten Eigenschaften verwenden. 

## <a name="attributes-and-expressions"></a>Attribute und Ausdrücke
Wenn ein Objekt (z. B. ein Benutzer) in Azure AD bereitgestellt wird, wird eine neue Instanz des Benutzerobjekts erstellt. Diese Erstellung beinhaltet die Eigenschaften des Objekts, die auch als Attribute bezeichnet werden. Die Attribute des neu erstellten Objekts sind zunächst auf Werte festgelegt, die durch die Synchronisierungsregeln bestimmt werden. Diese Attribute werden dann über den Agent für die Cloudbereitstellung auf dem neuesten Stand gehalten.

![Objektbereitstellung](media/concept-attributes/attribute1.png)

Ein Benutzer kann beispielsweise zu einer Marketingabteilung gehören. Das Azure AD-Abteilungsattribut wird erstmalig bei der Bereitstellung des Benutzers erstellt, und der Wert wird auf „Marketing“ festgelegt. Wenn dieser Benutzer sechs Monate später zum Vertrieb wechselt, wird das lokale Active Directory-Abteilungsattribut in „Vertrieb“ geändert. Diese Änderung wird mit Azure AD synchronisiert und für das zugehörige Azure AD-Benutzerobjekt übernommen.

Die Attributsynchronisierung kann direkt erfolgen, wobei der Wert in Azure AD direkt auf den Wert des lokalen Attributs festgelegt wird. Alternativ kann aber auch ein programmgesteuerter Ausdruck die Synchronisierung übernehmen. Ein programmgesteuerter Ausdruck wird in den Fällen benötigt, bei denen zum Auffüllen des Werts eine bestimmte Logik oder eine Festlegung erforderlich ist.

Wenn Sie beispielsweise das E-Mail-Attribut „john.smith@contoso.com“ haben und den Teil „@contoso.com“ entfernen und nur den Wert „john.smith“ übergeben müssten, würden Sie etwa Folgendes verwenden:

`Replace([mail], "@contoso.com", , ,"", ,)`  

**Beispieleingabe/-ausgabe:** <br>

* **EINGABE** (mail): "john.smith@contoso.com"
* **AUSGABE**: "john.smith"

Weitere Informationen zum Schreiben benutzerdefinierter Ausdrücke und zur Syntax finden Sie unter [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](../app-provisioning/functions-for-customizing-application-data.md).

In der folgenden Tabelle sind einige allgemeine Attribute und ihre Synchronisierung mit Azure AD aufgeführt.


|Lokales Active Directory|Zuordnungstyp|Azure AD|
|-----|-----|-----|
|cn|Direkt|commonName
|countryCode|Direkt|countryCode|
|displayName|Direkt|displayName|
|givenName|Ausdruck|givenName|
|objectGUID|Direkt|sourceAnchorBinary|  
|userprincipalName|Direkt|userPrincipalName|
|ProxyAdress|Direkt|ProxyAddress|

## <a name="view-the-schema"></a>Anzeigen des Schemas
> [!WARNING]
> Bei der Konfiguration der Cloudbereitstellung wird ein Dienstprinzipal erstellt. Der Dienstprinzipal ist im Azure-Portal sichtbar. Die Attributzuordnungen dürfen nicht mithilfe des Dienstprinzipals im Azure-Portal geändert werden.  Dieser Vorgang wird nicht unterstützt.

Führen Sie die folgenden Schritte aus, um das Schema anzuzeigen und zu überprüfen.

1.  Wechseln Sie zum [Graph-Explorer](https://developer.microsoft.com/graph/graph-explorer).
1.  Melden Sie sich mit Ihrem globalen Administratorkonto an.
1.  Wählen Sie links **Berechtigungen ändern** aus, und stellen Sie sicher, dass für **Directory.ReadWrite.All** die *Einwilligung erteilt* wurde.
1.  Führen Sie die Abfrage `https://graph.microsoft.com/beta/serviceprincipals/?$filter=startswith(Displayname,'Active')` aus. Mit dieser Abfrage wird eine gefilterte Liste von Dienstprinzipalen zurückgegeben.
1.  Suchen Sie nach `"appDisplayName": "Active Directory to Azure Active Directory Provisioning"`, und notieren Sie sich den Wert für `"id"`.
    ```
    "value": [
            {
                "id": "00d41b14-7958-45ad-9d75-d52fa29e02a1",
                "deletedDateTime": null,
                "accountEnabled": true,
                "appDisplayName": "Active Directory to Azure Active Directory Provisioning",
                "appId": "1a4721b3-e57f-4451-ae87-ef078703ec94",
                "applicationTemplateId": null,
                "appOwnerOrganizationId": "47df5bb7-e6bc-4256-afb0-dd8c8e3c1ce8",
                "appRoleAssignmentRequired": false,
                "displayName": "Active Directory to Azure Active Directory Provisioning",
                "errorUrl": null,
                "homepage": "https://account.activedirectory.windowsazure.com:444/applications/default.aspx?metadata=AD2AADProvisioning|ISV9.1|primary|z",
                "loginUrl": null,
                "logoutUrl": null,
                "notificationEmailAddresses": [],
                "preferredSingleSignOnMode": null,
                "preferredTokenSigningKeyEndDateTime": null,
                "preferredTokenSigningKeyThumbprint": null,
                "publisherName": "Active Directory Application Registry",
                "replyUrls": [],
                "samlMetadataUrl": null,
                "samlSingleSignOnSettings": null,
                "servicePrincipalNames": [
                    "http://adapplicationregistry.onmicrosoft.com/adprovisioningtoaad/primary",
                    "1a4721b3-e57f-4451-ae87-ef078703ec94"
                ],
                "signInAudience": "AzureADMultipleOrgs",
                "tags": [
                    "WindowsAzureActiveDirectoryIntegratedApp"
                ],
                "addIns": [],
                "api": {
                    "resourceSpecificApplicationPermissions": []
                },
                "appRoles": [
                    {
                        "allowedMemberTypes": [
                            "User"
                        ],
                        "description": "msiam_access",
                        "displayName": "msiam_access",
                        "id": "a0326856-1f51-4311-8ae7-a034d168eedf",
                        "isEnabled": true,
                        "origin": "Application",
                        "value": null
                    }
                ],
                "info": {
                    "termsOfServiceUrl": null,
                    "supportUrl": null,
                    "privacyStatementUrl": null,
                    "marketingUrl": null,
                    "logoUrl": null
                },
                "keyCredentials": [],
                "publishedPermissionScopes": [
                    {
                        "adminConsentDescription": "Allow the application to access Active Directory to Azure Active Directory Provisioning on behalf of the signed-in user.",
                        "adminConsentDisplayName": "Access Active Directory to Azure Active Directory Provisioning",
                        "id": "d40ed463-646c-4efe-bb3e-3fa7d0006688",
                        "isEnabled": true,
                        "type": "User",
                        "userConsentDescription": "Allow the application to access Active Directory to Azure Active Directory Provisioning on your behalf.",
                        "userConsentDisplayName": "Access Active Directory to Azure Active Directory Provisioning",
                        "value": "user_impersonation"
                    }
                ],
                "passwordCredentials": []
            },
    ```
1. Ersetzen Sie `{Service Principal id}` durch Ihren eigenen Wert, und führen Sie die Abfrage `https://graph.microsoft.com/beta/serviceprincipals/{Service Principal id}/synchronization/jobs/` aus.
1. Suchen Sie nach `"id": "AD2AADProvisioning.fd1c9b9e8077402c8bc03a7186c8f976"`, und notieren Sie sich den Wert für `"id"`.
    ```
    {
                "id": "AD2AADProvisioning.fd1c9b9e8077402c8bc03a7186c8f976",
                "templateId": "AD2AADProvisioning",
                "schedule": {
                    "expiration": null,
                    "interval": "PT2M",
                    "state": "Active"
                },
                "status": {
                    "countSuccessiveCompleteFailures": 0,
                    "escrowsPruned": false,
                    "code": "Active",
                    "lastSuccessfulExecutionWithExports": null,
                    "quarantine": null,
                    "steadyStateFirstAchievedTime": "2019-11-08T15:48:05.7360238Z",
                    "steadyStateLastAchievedTime": "2019-11-20T16:17:24.7957721Z",
                    "troubleshootingUrl": "",
                    "lastExecution": {
                        "activityIdentifier": "2dea06a7-2960-420d-931e-f6c807ebda24",
                        "countEntitled": 0,
                        "countEntitledForProvisioning": 0,
                        "countEscrowed": 15,
                        "countEscrowedRaw": 15,
                        "countExported": 0,
                        "countExports": 0,
                        "countImported": 0,
                        "countImportedDeltas": 0,
                        "countImportedReferenceDeltas": 0,
                        "state": "Succeeded",
                        "error": null,
                        "timeBegan": "2019-11-20T16:15:21.116098Z",
                        "timeEnded": "2019-11-20T16:17:24.7488681Z"
                    },
                    "lastSuccessfulExecution": {
                        "activityIdentifier": null,
                        "countEntitled": 0,
                        "countEntitledForProvisioning": 0,
                        "countEscrowed": 0,
                        "countEscrowedRaw": 0,
                        "countExported": 5,
                        "countExports": 0,
                        "countImported": 0,
                        "countImportedDeltas": 0,
                        "countImportedReferenceDeltas": 0,
                        "state": "Succeeded",
                        "error": null,
                        "timeBegan": "0001-01-01T00:00:00Z",
                        "timeEnded": "2019-11-20T14:09:46.8855027Z"
                    },
                    "progress": [],
                    "synchronizedEntryCountByType": [
                        {
                            "key": "group to Group",
                            "value": 33
                        },
                        {
                            "key": "user to User",
                            "value": 3
                        }
                    ]
                },
                "synchronizationJobSettings": [
                    {
                        "name": "Domain",
                        "value": "{\"DomainFQDN\":\"contoso.com\",\"DomainNetBios\":\"CONTOSO\",\"ForestFQDN\":\"contoso.com\",\"ForestNetBios\":\"CONTOSO\"}"
                    },
                    {
                        "name": "DomainFQDN",
                        "value": "contoso.com"
                    },
                    {
                        "name": "DomainNetBios",
                        "value": "CONTOSO"
                    },
                    {
                        "name": "ForestFQDN",
                        "value": "contoso.com"
                    },
                    {
                        "name": "ForestNetBios",
                        "value": "CONTOSO"
                    },
                    {
                        "name": "QuarantineTooManyDeletesThreshold",
                        "value": "500"
                    }
                ]
            }
    ```
1. Führen Sie jetzt die Abfrage `https://graph.microsoft.com/beta/serviceprincipals/{Service Principal Id}/synchronization/jobs/{AD2AAD Provisioning id}/schema` aus.
 
    Beispiel: https://graph.microsoft.com/beta/serviceprincipals/653c0018-51f4-4736-a3a3-94da5dcb6862/synchronization/jobs/AD2AADProvisioning.e9287a7367e444c88dc67a531c36d8ec/schema

   Ersetzen Sie `{Service Principal Id}` und `{AD2ADD Provisioning Id}` durch Ihre eigenen Werte.

1. Mit dieser Abfrage wird das Schema zurückgegeben.

   ![Zurückgegebenes Schema](media/concept-attributes/schema1.png)
 
## <a name="next-steps"></a>Nächste Schritte

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)