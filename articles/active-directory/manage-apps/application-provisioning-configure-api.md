---
title: Verwenden von MS Graph-APIs zum Konfigurieren der Bereitstellung – Azure Active Directory | Microsoft-Dokumentation
description: Sie müssen die Bereitstellung für mehrere Instanzen einer Anwendung einrichten? Erfahren Sie, wie Sie Zeit sparen, indem Sie mit MS Graph-APIs die Konfiguration der automatischen Bereitstellung automatisieren.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ca26c7277f69a817a48884acce116e3a66608ff
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74275717"
---
# <a name="configure-provisioning-using-microsoft-graph-apis"></a>Konfigurieren der Bereitstellung mithilfe von Microsoft Graph-APIs

Das Azure-Portal bietet eine bequeme Möglichkeit, die Bereitstellung für einzelne Apps einzeln zu konfigurieren. Wenn Sie jedoch mehrere – oder sogar Hunderte – Instanzen einer Anwendung erstellen, kann es einfacher sein, die App-Erstellung und-Konfiguration mit Microsoft Graph-APIs zu automatisieren. In diesem Artikel wird beschrieben, wie Sie die Bereitstellungskonfiguration über APIs automatisieren. Dieses Verfahren wird häufig für Anwendungen wie [Amazon Web Services](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial#configure-azure-ad-sso) verwendet.

**Übersicht über die Schritte zum Automatisieren der Bereitstellungskonfiguration mithilfe von Microsoft Graph-APIs**


|Schritt  |Details  |
|---------|---------|
|[Schritt 1. Erstellen der Kataloganwendung](#step-1-create-the-gallery-application)     |Anmelden beim API-Client <br> Abrufen der Kataloganwendungsvorlage <br> Erstellen der Kataloganwendung         |
|[Schritt 2. Erstellen eines Bereitstellungsauftrags basierend auf der Vorlage](#step-2-create-the-provisioning-job-based-on-the-template)     |Abrufen der Vorlage für den Bereitstellungsconnector <br> Erstellen des Bereitstellungsauftrags         |
|[Schritt 3. Autorisieren des Zugriffs](#step-3-authorize-access)     |Testen der Verbindung mit der Anwendung <br> Speichern der Anmeldeinformationen         |
|[Schritt 4. Starten des Bereitstellungsauftrags](#step-4-start-the-provisioning-job)     |Starten des Auftrags         |
|[Schritt 5. Überwachen der Bereitstellung](#step-5-monitor-provisioning)     |Überprüfen des Status des Bereitstellungsauftrags <br> Abrufen der Bereitstellungsprotokolle         |

> [!NOTE]
> Die in diesem Artikel gezeigten Antwortobjekte werden möglicherweise zur besseren Lesbarkeit verkürzt dargestellt. Von einem tatsächlichen Aufruf werden alle Eigenschaften zurückgegeben.

## <a name="step-1-create-the-gallery-application"></a>Schritt 1: Erstellen der Kataloganwendung

### <a name="sign-in-to-microsoft-graph-explorer-recommended-postman-or-any-other-api-client-you-use"></a>Melden Sie sich bei Microsoft Graph-Tester (empfohlen), Postman oder einem beliebigen anderen API-Client an, den Sie verwenden.

1. Öffnen Sie den [Microsoft Graph-Tester](https://developer.microsoft.com/graph/graph-explorer).
1. Wählen Sie die Schaltfläche „Mit Microsoft anmelden“ aus, und melden Sie sich mit den globalen Azure AD-Anmeldeinformationen oder mit den Anmeldeinformationen für den App-Administrator an.

    ![Graph-Anmeldung](./media/export-import-provisioning-mappings/wd_export_02.png)

1. Nach der erfolgreichen Anmeldung werden im linken Bereich die Details des Benutzerkontos angezeigt.

### <a name="retrieve-the-gallery-application-template-identifier"></a>Abrufen des Bezeichners für die Kataloganwendungsvorlage
Anwendungen im Azure AD Anwendungskatalog verfügen jeweils über eine [Anwendungsvorlage](https://docs.microsoft.com/graph/api/applicationtemplate-list?view=graph-rest-beta&tabs=http), in der die Metadaten für die Anwendung beschrieben werden. Mithilfe dieser Vorlage können Sie im Mandanten für die Verwaltung eine Instanz der Anwendung und des Dienstprinzipals erstellen.

#### <a name="request"></a>*Anforderung*

<!-- {
  "blockType": "request",
  "name": "get_applicationtemplates"
}-->

```msgraph-interactive
GET https://graph.microsoft.com/beta/applicationTemplates
```

#### <a name="response"></a>*Antwort*

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

#### <a name="request"></a>*Anforderung*

<!-- {
  "blockType": "request",
  "name": "applicationtemplate_instantiate"
}-->

```msgraph-interactive
POST https://graph.microsoft.com/beta/applicationTemplates/{id}/instantiate
Content-type: application/json

{
  "displayName": "AWS Contoso"
}
```

#### <a name="response"></a>*Antwort*


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

## <a name="step-2-create-the-provisioning-job-based-on-the-template"></a>Schritt 2: Erstellen des Bereitstellungsauftrags basierend auf der Vorlage

### <a name="retrieve-the-template-for-the-provisioning-connector"></a>Abrufen der Vorlage für den Bereitstellungsconnector

Anwendungen im Katalog, die für die Bereitstellung aktiviert sind, verfügen über Vorlagen zum Optimieren der Konfiguration. Verwenden Sie die nachstehende Anforderung, um [die Vorlage für die Bereitstellungskonfiguration abzurufen](https://docs.microsoft.com/graph/api/synchronization-synchronizationtemplate-list?view=graph-rest-beta&tabs=http).

#### <a name="request"></a>*Anforderung*

<!-- {
  "blockType": "request",
  "name": "get_synchronizationtemplate"
}-->
```msgraph-interactive
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/templates
```


#### <a name="response"></a>*Antwort*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.synchronizationTemplate",
  "isCollection": true
} -->
```http
HTTP/1.1 200 OK

{
    "value": [
        {
            "id": "aws",
            "factoryTag": "aws",
            "schema": {
                    "directories": [],
                    "synchronizationRules": []
                    }
        }
    ]
}
```

### <a name="create-the-provisioning-job"></a>Erstellen des Bereitstellungsauftrags
Um die Bereitstellung zu ermöglichen, müssen Sie zunächst [einen Auftrag erstellen](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-post?view=graph-rest-beta&tabs=http). Verwenden Sie die nachstehende Anforderung, um einen Bereitstellungsauftrag zu erstellen. Verwenden Sie die „templateId“ aus dem vorherigen Schritt, wenn Sie die Vorlage angeben, die für den Auftrag verwendet werden soll.

#### <a name="request"></a>*Anforderung*
<!-- {
  "blockType": "request",
  "name": "create_synchronizationjob_from_synchronization"
}-->
```msgraph-interactive
POST https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs
Content-type: application/json

{ 
    "templateId": "aws"
}
```

#### <a name="response"></a>*Antwort*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.synchronizationJob"
} -->
```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "id": "{jobId}",
    "templateId": "aws",
    "schedule": {
        "expiration": null,
        "interval": "P10675199DT2H48M5.4775807S",
        "state": "Disabled"
    },
    "status": {
        "countSuccessiveCompleteFailures": 0,
        "escrowsPruned": false,
        "synchronizedEntryCountByType": [],
        "code": "NotConfigured",
        "lastExecution": null,
        "lastSuccessfulExecution": null,
        "lastSuccessfulExecutionWithExports": null,
        "steadyStateFirstAchievedTime": "0001-01-01T00:00:00Z",
        "steadyStateLastAchievedTime": "0001-01-01T00:00:00Z",
        "quarantine": null,
        "troubleshootingUrl": null
    }
}
```

## <a name="step-3-authorize-access"></a>Schritt 3: Autorisieren des Zugriffs

### <a name="test-the-connection-to-the-application"></a>Testen der Verbindung mit der Anwendung

Testen Sie die Verbindung mit der Drittanbieteranwendung. Das folgende Beispiel gilt für eine Anwendung, die „clientSecret“ und „secretToken“ erfordert. Jede Anwendung verfügt über eigene Anforderungen. Die verfügbaren Optionen finden Sie in der [API-Dokumentation](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-validatecredentials?view=graph-rest-beta&tabs=http). 

#### <a name="request"></a>*Anforderung*
```http
POST https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{id}/validateCredentials
{ 
    credentials: [ 
        { key: "ClientSecret", value: "xxxxxxxxxxxxxxxxxxxxx" },
        { key: "SecretToken", value: "xxxxxxxxxxxxxxxxxxxxx" }
    ]
}
```
#### <a name="response"></a>*Antwort*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.None"
} -->
```http
HTTP/1.1 204 No Content
```

### <a name="save-your-credentials"></a>Speichern Ihrer Anmeldeinformationen

Das Konfigurieren der Bereitstellung erfordert das Einrichten einer Vertrauensstellung zwischen Azure AD und der Anwendung. Autorisieren Sie den Zugriff auf die Drittanbieteranwendung. Das folgende Beispiel gilt für eine Anwendung, die „clientSecret“ und „secretToken“ erfordert. Jede Anwendung verfügt über eigene Anforderungen. Die verfügbaren Optionen finden Sie in der [API-Dokumentation](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-validatecredentials?view=graph-rest-beta&tabs=http). 

#### <a name="request"></a>*Anforderung*
```json
PUT https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/secrets 
 
{ 
    value: [ 
        { key: "ClientSecret", value: "xxxxxxxxxxxxxxxxxxxxx" },
        { key: "SecretToken", value: "xxxxxxxxxxxxxxxxxxxxx" }
    ]
}
```

#### <a name="response"></a>*Antwort*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.None"
} -->
```http
HTTP/1.1 204 No Content
```

## <a name="step-4-start-the-provisioning-job"></a>Schritt 4: Starten des Bereitstellungsauftrags
Nachdem der Bereitstellungsauftrag konfiguriert wurde, verwenden Sie den folgenden Befehl, um [den Auftrag zu starten](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-start?view=graph-rest-beta&tabs=http). 


#### <a name="request"></a>*Anforderung*
<!-- {
  "blockType": "request",
  "name": "synchronizationjob_start"
}-->
```http
POST https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/start
```

#### <a name="response"></a>*Antwort*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.None"
} -->
```http
HTTP/1.1 204 No Content
```


## <a name="step-5-monitor-provisioning"></a>Schritt 5: Überwachen der Bereitstellung

### <a name="monitor-the-provisioning-job-status"></a>Überwachen des Status des Bereitstellungsauftrags

Da der Bereitstellungsauftrag jetzt ausgeführt wird, verwenden Sie den folgenden Befehl, um den Fortschritt des aktuellen Bereitstellungszyklus sowie die bisherigen Statistiken, z. B. die Anzahl der Benutzer und Gruppen, die im Zielsystem erstellt wurden, nachzuverfolgen. 

#### <a name="request"></a>*Anforderung*
<!-- {
  "blockType": "request",
  "name": "get_synchronizationjob"
}-->
```msgraph-interactive
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/
```

#### <a name="response"></a>*Antwort*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.synchronizationJob"
} -->
```http
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 2577

{
    "id": "{jobId}",
    "templateId": "aws",
    "schedule": {
        "expiration": null,
        "interval": "P10675199DT2H48M5.4775807S",
        "state": "Disabled"
    },
    "status": {
        "countSuccessiveCompleteFailures": 0,
        "escrowsPruned": false,
        "synchronizedEntryCountByType": [],
        "code": "Paused",
        "lastExecution": null,
        "lastSuccessfulExecution": null,
        "progress": [],
        "lastSuccessfulExecutionWithExports": null,
        "steadyStateFirstAchievedTime": "0001-01-01T00:00:00Z",
        "steadyStateLastAchievedTime": "0001-01-01T00:00:00Z",
        "quarantine": null,
        "troubleshootingUrl": null
    },
    "synchronizationJobSettings": [
      {
          "name": "QuarantineTooManyDeletesThreshold",
          "value": "500"
      }
    ]
}
```


### <a name="monitor-provisioning-events-using-the-provisioning-logs"></a>Überwachen von Bereitstellungsereignissen mithilfe der Bereitstellungsprotokolle
Zusätzlich zur Überwachung des Status des Bereitstellungsauftrags können Sie die [Bereitstellungsprotokolle](https://docs.microsoft.com/graph/api/provisioningobjectsummary-list?view=graph-rest-beta&tabs=http) verwenden, um alle eintretenden Ereignisse abzufragen (z. B. Abfragen eines bestimmten Benutzers und Ermitteln, ob er erfolgreich bereitgestellt wurde).

#### <a name="request"></a>*Anforderung*
```msgraph-interactive
GET https://graph.microsoft.com/beta/auditLogs/provisioning
```
#### <a name="response"></a>*Antwort*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.provisioningObjectSummary",
  "name": "list_provisioningobjectsummary_error"
} -->

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "@odata.context": "https://graph.microsoft.com/beta/$metadata#auditLogs/provisioning",
    "value": [
        {
            "id": "gc532ff9-r265-ec76-861e-42e2970a8218",
            "activityDateTime": "2019-06-24T20:53:08Z",
            "tenantId": "7928d5b5-7442-4a97-ne2d-66f9j9972ecn",
            "jobId": "BoxOutDelta.7928d5b574424a97ne2d66f9j9972ecn",
            "cycleId": "44576n58-v14b-70fj-8404-3d22tt46ed93",
            "changeId": "eaad2f8b-e6e3-409b-83bd-e4e2e57177d5",
            "action": "Create",
            "durationInMilliseconds": 2785,
            "sourceSystem": {
                "id": "0404601d-a9c0-4ec7-bbcd-02660120d8c9",
                "displayName": "Azure Active Directory",
                "details": {}
            },
            "targetSystem": {
                "id": "cd22f60b-5f2d-1adg-adb4-76ef31db996b",
                "displayName": "Box",
                "details": {
                    "ApplicationId": "f2764360-e0ec-5676-711e-cd6fc0d4dd61",
                    "ServicePrincipalId": "chc46a42-966b-47d7-9774-576b1c8bd0b8",
                    "ServicePrincipalDisplayName": "Box"
                }
            },
            "initiatedBy": {
                "id": "",
                "displayName": "Azure AD Provisioning Service",
                "initiatorType": "system"
            },
            "sourceIdentity": {
                "id": "5e6c9rae-ab4d-5239-8ad0-174391d110eb",
                "displayName": "Self-service Pilot",
                "identityType": "Group",
                "details": {}
            },
            "targetIdentity": {
                "id": "",
                "displayName": "",
                "identityType": "Group",
                "details": {}
            },
            "statusInfo": {
                "@odata.type": "#microsoft.graph.statusDetails",
                "status": "failure",
                "errorCode": "BoxEntryConflict",
                "reason": "Message: Box returned an error response with the HTTP status code 409.  This response indicates that a user or a group already exisits with the same name. This can be avoided by identifying and removing the conflicting user from Box via the Box administrative user interface, or removing the current user from the scope of provisioning either by removing their assignment to the Box application in Azure Active Directory or adding a scoping filter to exclude the user.",
                "additionalDetails": null,
                "errorCategory": "NonServiceFailure",
                "recommendedAction": null
            },
            "provisioningSteps": [
                {
                    "name": "EntryImportAdd",
                    "provisioningStepType": "import",
                    "status": "success",
                    "description": "Received Group 'Self-service Pilot' change of type (Add) from Azure Active Directory",
                    "details": {}
                },
                {
                    "name": "EntrySynchronizationAdd",
                    "provisioningStepType": "matching",
                    "status": "success",
                    "description": "Group 'Self-service Pilot' will be created in Box (Group is active and assigned in Azure Active Directory, but no matching Group was found in Box)",
                    "details": {}
                },
                {
                    "name": "EntryExportAdd",
                    "provisioningStepType": "export",
                    "status": "failure",
                    "description": "Failed to create Group 'Self-service Pilot' in Box",
                    "details": {
                        "ReportableIdentifier": "Self-service Pilot"
                    }
                }
            ],
            "modifiedProperties": [
                {
                    "displayName": "objectId",
                    "oldValue": null,
                    "newValue": "5e0c9eae-ad3d-4139-5ad0-174391d110eb"
                },
                {
                    "displayName": "displayName",
                    "oldValue": null,
                    "newValue": "Self-service Pilot"
                },
                {
                    "displayName": "mailEnabled",
                    "oldValue": null,
                    "newValue": "False"
                },
                {
                    "displayName": "mailNickname",
                    "oldValue": null,
                    "newValue": "5ce25n9a-4c5f-45c9-8362-ef3da29c66c5"
                },
                {
                    "displayName": "securityEnabled",
                    "oldValue": null,
                    "newValue": "True"
                },
                {
                    "displayName": "Name",
                    "oldValue": null,
                    "newValue": "Self-service Pilot"
                }
            ]
       }
    ]
}

```
## <a name="related-articles"></a>Verwandte Artikel

- [Lesen Sie die Dokumentation zur Synchronisierung für Microsoft Graph](https://docs.microsoft.com/graph/api/resources/synchronization-overview?view=graph-rest-beta)
- [Integrieren einer benutzerdefinierten SCIM-App in Azure AD](use-scim-to-provision-users-and-groups.md)
