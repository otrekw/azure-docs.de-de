---
title: Grundlegendes zum Azure AD-Schema und zu benutzerdefinierten Ausdrücken
description: In diesem Thema werden das Azure AD-Schema, die Attribute, die der Bereitstellungs-Agent weiterleitet, und benutzerdefinierte Ausdrücke beschrieben.
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
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: eae594bcc20e3c4ed1c6fbd0333699de8c9f4452
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795529"
---
# <a name="understanding-the-azure-ad-schema"></a>Grundlegendes zum Azure AD-Schema
Ein Objekt in Azure AD ist wie jedes Verzeichnis ein programmgesteuertes allgemeines Datenkonstrukt, das Elemente wie Benutzer, Gruppen und Kontakte darstellt.  Wenn Sie einen neuen Benutzer oder Kontakt in Azure AD erstellen, erstellen Sie eine neue Instanz dieses Objekts.  Diese Instanzen können anhand ihrer Eigenschaften unterschieden werden.

Eigenschaften in Azure AD sind die Elemente, die zum Speichern von Informationen zu einer Instanz eines Objekts in Azure AD dienen.  

Das Azure AD-Schema definiert die Regeln, nach denen Eigenschaften in einem Eintrag verwendet werden können, die Arten von Werten, die diese Eigenschaften haben können, und wie Benutzer mit diesen Werten interagieren können. 

Azure AD umfasst zwei Typen von Eigenschaften.  Die Eigenschaften sind:
- **Integrierte Eigenschaften:** Eigenschaften, die durch das Azure AD-Schema vordefiniert sind.  Diese Eigenschaften ermöglichen unterschiedliche Verwendungen und können verfügbar oder nicht verfügbar sein.
- **Verzeichniserweiterungen:** Eigenschaften, die bereitgestellt werden, damit Sie Azure AD für die eigene Verwendung anpassen können.  Wenn Sie beispielsweise Ihr lokales Active Directory um ein bestimmtes Attribut erweitert haben und dieses Attribut weiterleiten möchten, können Sie eine der bereitgestellten benutzerdefinierten Eigenschaften verwenden. 

## <a name="attributes-and-expressions"></a>Attribute und Ausdrücke
Wenn ein Objekt, z. B. ein Benutzer, in Azure AD bereitgestellt wird, wird eine neue Instanz des Benutzerobjekts erstellt.  Diese Erstellung beinhaltet die Eigenschaften des Objekts, die auch als Attribute bezeichnet werden.  Die Attribute des neu erstellten Objekts sind zunächst auf Werte festgelegt, die durch die Synchronisierungsregeln bestimmt werden.  Diese Attribute werden dann über den Agent für die Cloudbereitstellung auf dem neuesten Stand gehalten.

![](media/concept-attributes/attribute1.png)

Wenn ein Benutzer z. B. der Marketingabteilung angehört, wird das zugehörige „department“-Attribut in Azure AD zunächst bei der Bereitstellung erstellt. Anschließend wird der Wert auf „Marketing“ festgelegt.  Sechs Monate später wechselt der Benutzer in die Vertriebsabteilung.  Das „department“-Attribut des Benutzers wird in der lokalen AD-Instanz in „Sales“ (Vertrieb) geändert.  Diese Änderung wird dann mit Azure AD synchronisiert und für das zugehörige Azure AD-Benutzerobjekt übernommen.

Die Attributsynchronisierung kann entweder erfolgen: Der Wert in Azure AD wird direkt auf den Wert des lokalen Attributs festgelegt.  Diese Synchronisierung kann aber auch über einen programmgesteuerten Ausdruck erfolgen.  Ein programmgesteuerter Ausdruck wird in den Fällen benötigt, bei denen Logik oder eine Festlegung erforderlich ist, um den Wert zu füllen.

Wenn Sie beispielsweise das „mail“-Attribut („john.smith@contoso.com“) verwenden und den Teil „@contoso.com“ entfernen und nur den Wert „john.smith“ übergeben möchten, können Sie folgenden Code verwenden:

`Replace([mail], "@contoso.com", , ,"", ,)`  

**Beispieleingabe/-ausgabe:** <br>

* **EINGABE** (mail): "john.smith@contoso.com"
* **Ausgabe:** „john.smith“

Weitere Informationen zum Schreiben von benutzerdefinierten Ausdrücken und zur Syntax finden Sie unter [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data).

In der folgenden Tabelle sind allgemeine Attribute und ihre Synchronisierung mit Azure AD aufgeführt.


|Lokales Active Directory|Zuordnungstyp|Azure AD|
|-----|-----|-----|
|cn|Direkt|commonName
|countryCode|Direkt|countryCode|
|displayName|Direkt|displayName|
|givenName|Ausdruck|givenName|
|objectGUID|Direkt|sourceAnchorBinary|  
|userprincipalName|Direkt|userPrincipalName|
|ProxyAdress|Direkt|ProxyAddress|

## <a name="viewing-the-schema"></a>Anzeigen des Schemas
Führen Sie die folgenden Schritte aus, um das Schema anzuzeigen und zu überprüfen:

1.  Navigieren Sie zu [Graph-Tester](https://developer.microsoft.com/graph/graph-explorer).
2.  Melden Sie sich mit Ihrem globalen Administratorkonto an.
3.  Klicken Sie links auf **Berechtigungen ändern**, und stellen Sie sicher, dass die Einwilligung für **Directory.ReadWrite.All** erteilt wurde.
4.  Führen Sie die folgende Abfrage aus: https://graph.microsoft.com/beta/serviceprincipals/.  Mit dieser Abfrage wird eine Liste von Dienstprinzipalen zurückgegeben.
5.  Suchen Sie „appDisplayName“: „Active Directory to Azure Active Directory Provisioning“, und notieren Sie sich den Wert von „id:“.
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
6. Ersetzen Sie {Service Principal id} durch Ihren Wert, und führen Sie die folgende Abfrage aus: `https://graph.microsoft.com/beta/serviceprincipals/{Service Principal id}/synchronization/jobs/`
7. Suchen Sie den Abschnitt „id“: „AD2AADProvisioning.fd1c9b9e8077402c8bc03a7186c8f976“, und notieren Sie sich den Wert von „id:“.
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
8. Führen Sie dann die folgende Abfrage aus: `https://graph.microsoft.com/beta/serviceprincipals/{Service Principal Id}/synchronization/jobs/{AD2AAD Provisioning id}/schema`
 
    Beispiel: https://graph.microsoft.com/beta/serviceprincipals/653c0018-51f4-4736-a3a3-94da5dcb6862/synchronization/jobs/AD2AADProvisioning.e9287a7367e444c88dc67a531c36d8ec/schema

 Ersetzen Sie {Service Principal Id} und {AD2ADD Provisioning Id} durch Ihre Werte.

9. Mit dieser Abfrage wird das Schema zurückgegeben.
  ![](media/concept-attributes/schema1.png)
 
## <a name="next-steps"></a>Nächste Schritte 

- [Was ist eine Bereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
