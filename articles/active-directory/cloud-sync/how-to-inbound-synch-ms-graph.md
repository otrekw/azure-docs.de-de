---
title: Eingehende Synchronisierung für die Cloudsynchronisierung mit der MS Graph-API
description: In diesem Thema wird beschrieben, wie Sie die eingehende Synchronisierung nur mit der Graph-API aktivieren.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/04/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3796b3d86f647e38cf2ff018e8c0c903d9a64e41
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98682037"
---
# <a name="inbound-synchronization-for-cloud-sync-using-ms-graph-api"></a>Eingehende Synchronisierung für die Cloudsynchronisierung mit der MS Graph-API

Im folgenden Dokument wird beschrieben, wie Sie ein Synchronisierungsprofil mithilfe von MS Graph-APIs von Grund auf neu replizieren.  
Die Vorgehensweise umfasst folgende Schritte.  Sie lauten wie folgt:

- [Grundlegende Einrichtung](#basic-setup)
- [Erstellen von Dienstprinzipalen](#create-service-principals)
- [Erstellen des Synchronisierungsauftrags](#create-sync-job)
- [Aktualisieren der Zieldomäne](#update-targeted-domain)
- [Aktivieren der Synchronisierung von Kennworthashes](#enable-sync-password-hashes-on-configuration-blade)
- [Starten des Synchronisierungsauftrags](#start-sync-job)
- [Überprüfen des Status](#review-status)

Verwenden Sie diese Befehle des [Microsoft Azure Active Directory-Moduls für Windows PowerShell](/powershell/module/msonline/), um die Synchronisierung für einen Produktionsmandanten zu aktivieren. Dies ist eine Voraussetzung dafür, dass der Verwaltungswebdienst für diesen Mandanten aufgerufen werden kann.

## <a name="basic-setup"></a>Grundlegende Einrichtung

### <a name="enable-tenant-flags"></a>Aktivieren von Mandantenflags

 ```PowerShell
 Connect-MsolService ('-AzureEnvironment <AzureEnvironmnet>')
 Set-MsolDirSyncEnabled -EnableDirSync $true
 ```
Für den ersten dieser beiden Befehle sind Azure Active Directory-Anmeldeinformationen erforderlich. Diese Cmdlets identifizieren implizit den Mandanten und aktivieren ihn für die Synchronisierung.

## <a name="create-service-principals"></a>Erstellen von Dienstprinzipalen
Als Nächstes müssen Sie die [AD2AAD-Anwendung mit Dienstprinzipal](/graph/api/applicationtemplate-instantiate?view=graph-rest-beta&tabs=http) erstellen.

Sie müssen diese Anwendungs-ID verwenden: 1a4721b3-e57f-4451-ae87-ef078703ec94. Der Anzeigename (displayName) ist bei Verwendung im Portal die URL der AD-Domäne (z. B. contoso.com), er kann aber umbenannt werden.

 ```
 POST https://graph.microsoft.com/beta/applicationTemplates/1a4721b3-e57f-4451-ae87-ef078703ec94/instantiate
 Content-type: application/json
 {
    displayName: [your app name here]
 }
 ```


## <a name="create-sync-job"></a>Erstellen des Synchronisierungsauftrags
In der Ausgabe des obigen Befehls wird die objectId des erstellten Dienstprinzipals zurückgegeben. In diesem Beispiel lautet die objectId 614ac0e9-a59b-481f-bd8f-79a73d167e1c.  Fügen Sie dem Dienstprinzipal mit Microsoft Graph einen Synchronisierungsauftrag hinzu.  

Eine Dokumentation zum Erstellen eines Synchronisierungsauftrags finden Sie [hier](/graph/api/synchronization-synchronizationjob-post?tabs=http&view=graph-rest-beta).

Falls Sie sich die oben genannte ID nicht notiert haben, können Sie den Dienstprinzipal mit dem folgenden MS Graph-Aufruf ermitteln. Sie benötigen für diesen Aufruf die Berechtigung „Directory.Read.All“:
 
 `GET https://graph.microsoft.com/beta/servicePrincipals `

Suchen Sie dann in der Ausgabe nach dem Namen Ihrer App.

Führen Sie die folgenden beiden Befehle aus, um zwei Aufträge zu erstellen: einen für die Benutzer-/Gruppenbereitstellung und einen für die Kennworthashsynchronisierung. Dabei handelt es sich zweimal um dieselbe Anforderung, aber mit unterschiedlichen Vorlagen-IDs.


Rufen Sie die beiden folgenden Anforderungen auf:

 ```
 POST https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs
 Content-type: application/json
 {
 "templateId":"AD2AADProvisioning"
 } 
 ```

 ```
 POST https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs
 Content-type: application/json
 {
 "templateId":"AD2AADPasswordHash"
 }
 ```

Wenn Sie beides erstellen möchten, benötigen Sie zwei Aufrufe.

Beispielrückgabewert (für die Bereitstellung):

 ```
HTTP 201/Created
{
    "@odata.context": "https://graph.microsoft.com/beta/$metadata#servicePrincipals('614ac0e9-a59b-481f-bd8f-79a73d167e1c')/synchronization/jobs/$entity",
    "id": "AD2AADProvisioning.fc96887f36da47508c935c28a0c0b6da",
    "templateId": "ADDCInPassthrough",
    "schedule": {
        "expiration": null,
        "interval": "PT40M",
        "state": "Disabled"
    },
    "status": {
        "countSuccessiveCompleteFailures": 0,
        "escrowsPruned": false,
        "code": "Paused",
        "lastExecution": null,
        "lastSuccessfulExecution": null,
        "lastSuccessfulExecutionWithExports": null,
        "quarantine": null,
        "steadyStateFirstAchievedTime": "0001-01-01T00:00:00Z",
        "steadyStateLastAchievedTime": "0001-01-01T00:00:00Z",
        "troubleshootingUrl": null,
        "progress": [],
        "synchronizedEntryCountByType": []
    }
}
```

## <a name="update-targeted-domain"></a>Aktualisieren der Zieldomäne
Für diesen Mandanten lauten der Objektbezeichner und der Anwendungsbezeichner des Dienstprinzipals wie folgt:

Objekt-ID: 8895955e-2e6c-4d79-8943-4d72ca36878f App-ID: 00000014-0000-0000-c000-000000000000, Anzeigename: testApp

Sie müssen die Domäne aktualisieren, auf die diese Konfiguration abzielt, indem Sie die Geheimnisse für diese Domäne aktualisieren.

Achten Sie darauf, denselben Domänennamen und dieselbe URL zu verwenden wie für Ihren lokalen Domänencontroller.

 ```
 PUT – https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/secrets
 ```
 Fügen Sie das folgende Schlüssel-Wert-Paar in das unten angegebene Wertarray ein, je nachdem, was Sie ausführen möchten:
 - Aktivieren von PHS-Flag und Flag des Synchronisierungsmandanten: { key: "AppKey", value: "{"appKeyScenario":"AD2AADPasswordHash"}" }
 
 - Aktivieren nur des Flags des Synchronisierungsmandanten (nicht PHS) { key: "AppKey", value: "{"appKeyScenario":"AD2AADProvisioning"}" }
 ```
 Request body –
 {
    "value": [
               {
                 "key": "Domain",
                 "value": "{\"domain\":\"ad2aadTest.com\"}"
               }
             ]
  }
```

Die erwartete Antwort lautet: HTTP 204/No Content.

Dabei ist der hervorgehobene Wert „Domain“ der Name der lokalen Active Directory-Domäne, von der Einträge in Azure Active Directory bereitgestellt werden sollen.

## <a name="enable-sync-password-hashes-on-configuration-blade"></a>Aktivieren der Synchronisierung von Kennworthashes auf dem Blatt „Konfiguration“

 In diesem Abschnitt wird das Aktivieren der Synchronisierung von Kennworthashes für eine bestimmte Konfiguration behandelt. Dies unterscheidet sich vom AppKey-Geheimnis für die Aktivierung des Featureflags auf Mandantenebene, da es nur für eine einzelne Domäne/Konfiguration gilt. Sie müssen den Anwendungsschlüssel auf das PHS-Flag festlegen, damit alles vollständig funktioniert.

1. Rufen Sie das Schema ab (Warnung: sehr groß): 
 ```
 GET –https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs/ [AD2AADProvisioningJobId]/schema
 ```
2. Verwenden Sie diese Attributzuordnung von CredentialData:
 ``` 
 {
 "defaultValue": null,
 "exportMissingReferences": false,
 "flowBehavior": "FlowWhenChanged",
 "flowType": "Always",
 "matchingPriority": 0,
 "targetAttributeName": "CredentialData",
 "source": {
 "expression": "[PasswordHash]",
 "name": "PasswordHash",
 "type": "Attribute",
 "parameters": []
 }
 ```
3. Suchen Sie die Objektzuordnungen mit den folgenden Namen im Schema:
 - Provision Active Directory Users
 - Provision Active Directory inetOrgPersons

 Sie finden die Objektzuordnungen in „schema.synchronizationRules[0].objectMappings“. (Sie können hier davon ausgehen, dass es nur eine Synchronisierungsregel gibt.)

4. Fügen Sie die CredentialData-Zuordnung aus Schritt 2 in die Objektzuordnungen in Schritt 3 ein.

 Ihre Objektzuordnung sollte der folgenden ähneln:
 ```
 {
 "enabled": true,
 "flowTypes": "Add,Update,Delete",
 "name": "Provision Active Directory users",
 "sourceObjectName": "user",
 "targetObjectName": "User",
 "attributeMappings": [
 ...
 } 
 ```
 Kopieren Sie die Zuordnung aus dem Schritt zum **Erstellen der Aufträge AD2AADProvisioning und AD2AADPasswordHash** oben in das attributeMappings-Array. 

 Die Reihenfolge der Elemente in diesem Array spielt keine Rolle, da sie vom Back-End sortiert werden. Gehen Sie beim Hinzufügen dieser Attributzuordnung vorsichtig vor: Wenn der Name bereits im Array vorhanden ist (z. B. da es in attributeMappings bereits ein Element mit dem targetAttributeName-Wert „CredentialData“ gibt), treten möglicherweise Konflikte auf, oder die vorhandene und die neue Zuordnung werden miteinander kombiniert (das Ergebnis ist normalerweise nicht wünschenswert). Das Back-End löscht Duplikate nicht für Sie. 

 Beachten Sie dies sowohl für Users als auch für inetOrgpersons.

5. Speichern Sie das erstellte Schema: 
 ```
 PUT –
 https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs/ [AD2AADProvisioningJobId]/schema
```

 Fügen Sie das Schema in den Anforderungstext ein. 

## <a name="start-sync-job"></a>Starten des Synchronisierungsauftrags
Der Auftrag kann mit dem folgenden Befehl erneut abgerufen werden:

 `GET https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs/ ` 

Die Dokumentation zum Abrufen von Aufträgen finden Sie [hier](/graph/api/synchronization-synchronizationjob-list?tabs=http&view=graph-rest-beta). 
 
Um den Auftrag zu starten, geben Sie diese Anforderung mithilfe der Objekt-ID des im ersten Schritt erstellten Dienstprinzipals und des von der Anforderung für die Auftragserstellung zurückgegebenen Auftragsbezeichners an.

Die Dokumentation zum Starten eines Auftrags finden Sie [hier](/graph/api/synchronization-synchronizationjob-start?tabs=http&view=graph-rest-beta). 

 ```
 POST  https://graph.microsoft.com/beta/servicePrincipals/8895955e-2e6c-4d79-8943-4d72ca36878f/synchronization/jobs/AD2AADProvisioning.fc96887f36da47508c935c28a0c0b6da/start
 ```

Die erwartete Antwort lautet: HTTP 204/No Content.

Weitere Befehle zum Steuern von Aufträgen sind [hier](/graph/api/resources/synchronization-synchronizationjob?view=graph-rest-beta) dokumentiert.
 
Für das erneute Starten eines Auftrags verwenden Sie:

 ```
 POST  https://graph.microsoft.com/beta/servicePrincipals/8895955e-2e6c-4d79-8943-4d72ca36878f/synchronization/jobs/AD2AADProvisioning.fc96887f36da47508c935c28a0c0b6da/restart
 {
   "criteria": {
       "resetScope": "Full"
   }
 }
 ```

## <a name="review-status"></a>Überprüfen des Status
Rufen Sie den Auftragsstatus ab:

 ```
 GET https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs/ 
 ```

Suchen Sie im Abschnitt „status“ des Rückgabeobjekts nach relevanten Details.

## <a name="next-steps"></a>Nächste Schritte 

- [Worum handelt es sich bei der Azure AD Connect-Cloudsynchronisierung?](what-is-cloud-sync.md)
- [Transformationen](how-to-transformation.md)
- [Azure AD-Synchronisierungs-API](/graph/api/resources/synchronization-overview?view=graph-rest-beta)