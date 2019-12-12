---
title: Azure Managed Applications mit Benachrichtigungen
description: Konfigurieren Sie Managed Applications mit Webhookendpunkten, um Benachrichtigungen über Erstellungs-, Aktualisierungs- und Löschungsereignisse und Fehler in den Instanzen verwalteter Anwendungen zu erhalten.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.reviewer: ''
ms.author: ilahat
author: ilahat
ms.date: 11/01/2019
ms.openlocfilehash: 8cf9fc0b3d9c13ebc5309be6d27c7be0f2e60878
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74805687"
---
# <a name="azure-managed-applications-with-notifications"></a>Azure Managed Applications mit Benachrichtigungen

Azure Managed Applications-Benachrichtigungen ermöglichen es Herausgebern, Aktionen auf Grundlage von Lebenszyklusereignissen der Instanzen verwalteter Anwendungen zu automatisieren. Herausgeber können benutzerdefinierte Benachrichtigungswebhookendpunkte angeben, um Ereignisbenachrichtigungen über neue und vorhandene Instanzen verwalteter Anwendungen zu empfangen. Dies ermöglicht dem Herausgeber das Einrichten von benutzerdefinierten Workflows zum Zeitpunkt der Bereitstellung, Aktualisierung und Löschung von Anwendungen.

## <a name="getting-started"></a>Erste Schritte
Um Benachrichtigungen für verwaltete Anwendungen zu erhalten, richten Sie einen öffentlichen HTTPS-Endpunkt ein, und geben Sie ihn an, wenn Sie die Definition der Dienstkataloganwendung oder das Marketplace-Angebot veröffentlichen.

Dies sind die empfohlenen Schritte für die schnelle Durchführung:
1. Richten Sie einen öffentlichen HTTPS-Endpunkt ein, der die eingehenden POST-Anforderungen protokolliert und `200 OK` zurückgibt.
2. Fügen Sie der Definition der Dienstkataloganwendung oder dem Marketplace-Angebot den Endpunkt hinzu, wie unten erläutert.
3. Erstellen Sie eine Instanz einer verwalteten Anwendung, die auf die Anwendungsdefinition oder das Marketplace-Angebot verweist.
4. Überprüfen Sie, ob die Benachrichtigungen erfolgreich empfangen wurden.
5. Aktivieren Sie die Autorisierung, wie im Abschnitt **Endpunktauthentifizierung** weiter unten erläutert.
6. Befolgen Sie die nachstehende Dokumentation zum **Benachrichtigungsschema**, um die Benachrichtigungsanforderungen zu analysieren und die Geschäftslogik basierend auf der Benachrichtigung zu implementieren.

## <a name="adding-service-catalog-application-definition-notifications"></a>Hinzufügen von Benachrichtigungen über Definitionen von Dienstkataloganwendungen
#### <a name="azure-portal"></a>Azure-Portal
Lesen Sie zunächst [Veröffentlichen einer Dienstkataloganwendung über das Azure-Portal](./publish-portal.md).

![Benachrichtigungen über Definitionen von Dienstkataloganwendungen im Portal](./media/publish-notifications/service-catalog-notifications.png)
#### <a name="rest-api"></a>REST-API

> [!NOTE]
> Zurzeit wird nur ein Endpunkt als Teil der **notificationEndpoints** in den Anwendungsdefinitionseigenschaften unterstützt.

``` JSON
    {
      "properties": {
        "isEnabled": true,
        "lockLevel": "ReadOnly",
        "displayName": "Sample Application Definition",
        "description": "Notification-enabled application definition.",
        "notificationPolicy": {
            "notificationEndpoints": [
                {
                    "uri": "https://isv.azurewebsites.net:1214?sig=unique_token"
                }
            ]
        },
        "authorizations": [
          {
            "principalId": "d6b7fbd3-4d99-43fe-8a7a-f13aef11dc18",
            "roleDefinitionId": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635"
          },
        ...

```
## <a name="adding-marketplace-managed-application-notifications"></a>Hinzufügen von Benachrichtigungen für verwaltete Marketplace-Anwendungen
Informationen hierzu finden Sie unter [Erstellen eines Azure-Anwendungsangebots](../marketplace/cloud-partner-portal/azure-applications/cpp-create-offer.md).

![Benachrichtigungen über Definitionen von Dienstkataloganwendungen im Portal](./media/publish-notifications/marketplace-notifications.png)
## <a name="event-triggers"></a>Ereignistrigger
In der folgenden Tabelle werden alle möglichen Kombinationen von „EventType“ und „ProvisioningState“ sowie ihre Trigger beschrieben:

EventType | ProvisioningState | Trigger für Benachrichtigung
---|---|---
PUT | Zulässig | Die verwaltete Ressourcengruppe wurde nach Ausführung von „PUT“ für die Anwendung erfolgreich erstellt und projiziert. (Bevor die Bereitstellung in der verwalteten Ressourcengruppe gestartet wird.)
PUT | Succeeded | Die vollständige Bereitstellung der verwalteten Anwendung war nach Ausführung von „PUT“ erfolgreich.
PUT | Fehler | Fehler beim Ausführen von „PUT“ der Anwendungsinstanzbereitstellung an einem beliebigen Punkt.
PATCH | Succeeded | Nach erfolgreichem „PATCH“ einer Instanz einer verwalteten Anwendung zum Aktualisieren von Tags, der JIT-Zugriffsrichtlinie oder der verwalteten Identität.
DELETE | Wird gelöscht | Sobald der Benutzer einen DELETE-Vorgang einer Instanz einer verwalteten Anwendung initiiert.
DELETE | Deleted | Nach der vollständigen und erfolgreichen Löschung der verwalteten Anwendung.
DELETE | Fehler | Nach jedem Fehler während der Aufhebung der Bereitstellung, der den Löschvorgang blockiert.
## <a name="notification-schema"></a>Benachrichtigungsschema
Wenn Sie den Webhookendpunkt für die Behandlung von Benachrichtigungen einrichten, müssen Sie die Nutzlast analysieren, um wichtige Eigenschaften zu ermitteln, damit auf die Benachrichtigung reagiert werden kann. Viele der Eigenschaften von Benachrichtigungen zu verwalteten Anwendungen des Dienstkatalogs und des Marketplace sind identisch. Die geringen Unterschiede werden nachfolgend beschrieben.

#### <a name="service-catalog-application-notification-schema"></a>Benachrichtigungsschema für Dienstkataloganwendungen
Im Folgenden finden Sie ein Beispiel für eine Dienstkatalogbenachrichtigung nach der erfolgreichen Bereitstellung einer Instanz einer verwalteten Anwendung.
``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Succeeded",
    "applicationDefinitionId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applicationDefinitions/<appDefName>"    
}

```

Wenn die Bereitstellung fehlschlägt, wird eine Benachrichtigung mit den Fehlerdetails an den angegebenen Endpunkt gesendet.

``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Failed",
    "applicationDefinitionId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applicationDefinitions/<appDefName>",
    "error": {
        "code": "ErrorCode",
        "message": "error message",
        "details": [
            {
                "code": "DetailedErrorCode",
                "message": "error message"
            }
        ]
    }
}

```

#### <a name="marketplace-application-notification-schema"></a>Benachrichtigungsschema für Marketplace-Anwendungen

Im Folgenden finden Sie ein Beispiel für eine Dienstkatalogbenachrichtigung nach der erfolgreichen Bereitstellung einer Instanz einer verwalteten Anwendung.
``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Succeeded",
    "billingDetails": {
        "resourceUsageId":"<resourceUsageId>"
    },
    "plan": {
        "publisher": "publisherId",
        "product": "offer",
        "name": "skuName",
        "version": "1.0.1"
    }
}

```

Wenn die Bereitstellung fehlschlägt, wird eine Benachrichtigung mit den Fehlerdetails an den angegebenen Endpunkt gesendet.

``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Failed",
    "billingDetails": {
        "resourceUsageId":"<resourceUsageId>"
    },
    "plan": {
        "publisher": "publisherId",
        "product": "offer",
        "name": "skuName",
        "version": "1.0.1"
    },
    "error": {
        "code": "ErrorCode",
        "message": "error message",
        "details": [
            {
                "code": "DetailedErrorCode",
                "message": "error message"
            }
        ]
    }
}

```

Parameter | BESCHREIBUNG
---|---
eventType | Der Typ des Ereignisses, das die Benachrichtigung ausgelöst hat (Beispiele: „PUT“, „PATCH“, „DELETE“).
applicationId | Der vollqualifizierte Ressourcenbezeichner der verwalteten Anwendung, für die die Benachrichtigung ausgelöst wurde. 
eventTime | Der Zeitstempel des Ereignisses, das die Benachrichtigung ausgelöst hat (Datum und Uhrzeit im UTC ISO 8601-Format).
provisioningState | Der Bereitstellungsstatus der Instanz der verwalteten Anwendung. (Beispiele: „Succeeded“, „Failed“, „Deleting“, „Deleted“)
billingDetails | Hierbei handelt es sich um die Abrechnungsdetails der verwalteten Anwendungsinstanz. Mithilfe von „resourceUsageId“ kann der Marketplace nach Nutzungsdetails abgefragt werden.
error | *Wird nur angegeben, wenn „provisioningState“ den Wert „Failed“ aufweist.* . Enthält den Fehlercode, die Meldung und Details zu dem Problem, das den Fehler verursacht hat.
applicationDefinitionId | *Wird nur für verwaltete Dienstkataloganwendungen angegeben*. Stellt den vollqualifizierten Ressourcenbezeichner der Anwendungsdefinition dar, für die die Instanz der verwalteten Anwendung bereitgestellt wurde.
Tarif | *Wird nur für verwaltete Marketplace-Anwendungen angegeben*. Stellt den Herausgeber, das Angebot, die SKU und die Version der Instanz der verwalteten Anwendung dar.

## <a name="endpoint-authentication"></a>Endpunktauthentifizierung
So sichern Sie den Webhookendpunkt und stellen die Echtheit der Benachrichtigung sicher:
- Geben Sie im Webhook-URI einen Abfrageparameter an, z. B. https://your-endpoint.com?sig=Guid. Führen Sie bei jeder Benachrichtigung eine schnelle Überprüfung durch, ob der Abfrageparameter `sig` den erwarteten Wert `Guid` aufweist.
- Rufen Sie für die Instanz der verwalteten Anwendung „GET“ mit „applicationId“ auf. Überprüfen Sie, ob der „provisioningState“ dem „provisioningState“ der Benachrichtigung entspricht, um die Konsistenz sicherzustellen.

## <a name="notification-retries"></a>Benachrichtigungswiederholungen

Der Managed Applications-Benachrichtigungsdienst erwartet vom Webhookendpunkt die Antwort `200 OK` auf die Benachrichtigung. Der Benachrichtigungsdienst versucht erneut, die Benachrichtigung zu senden, wenn der Webhookendpunkt einen HTTP-Fehlercode > = 500 oder gleich 429 zurückgibt oder vorübergehend nicht erreichbar ist. Wenn der Webhookendpunkt nicht innerhalb von 10 Stunden verfügbar wird, wird die Benachrichtigung gelöscht, und die Versuche zum Senden der Benachrichtigung werden eingestellt.

