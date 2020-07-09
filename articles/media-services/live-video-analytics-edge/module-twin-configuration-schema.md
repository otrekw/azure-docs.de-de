---
title: 'JSON-Schema des Modulzwillings: Azure'
description: In diesem Thema wird das JSON-Schema des Modulzwillings für Live Video Analytics in IoT Edge beschrieben.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: a342c59b35c7ebb4b6021163da76bdd3e0d449c3
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84266813"
---
# <a name="module-twin-json-schema"></a>JSON-Schema des Modulzwillings

Gerätezwillinge sind JSON-Dokumente, in denen Gerätestatusinformationen gespeichert werden, z. B. Metadaten, Konfigurationen und Bedingungen. Azure IoT Hub pflegt einen Gerätezwilling für jedes Gerät, das Sie mit IoT Hub verbinden. Eine ausführliche Beschreibung finden Sie unter [Verstehen und Verwenden von Modulzwillingen in IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins).

In diesem Thema wird das JSON-Schema des Modulzwillings für Live Video Analytics in IoT Edge beschrieben.

> [!NOTE]
> Sie müssen zuerst authentifiziert werden, um berechtigt zu sein, auf Media Services-Ressourcen und die Media Services-API zuzugreifen. Weitere Informationen finden Sie unter [Zugreifen auf die Azure Media Services-API](../latest/media-services-apis-overview.md#accessing-the-azure-media-services-api).

## <a name="module-twin-properties"></a>Eigenschaften von Modulzwillingen

Bei Live Video Analytics in IoT Edge werden die folgenden Eigenschaften von Modulzwillingen verfügbar gemacht. 

|Eigenschaft |Erforderlich |Dynamisch |BESCHREIBUNG |
|---|---|---|---|
|applicationDataDirectory |Ja |Nein |Pfad zu einem eingebundenen Volume für die dauerhafte Speicherung der Konfiguration. |
|azureMediaServicesArmId |Ja |Nein |Eindeutiger Azure Resource Manager-Bezeichner für das Media Services-Konto.|
|aadTenantId |Ja |Nein |Azure AD-Mandanten-ID für Kunden.|
|aadServicePrincipalAppId |Ja |Ja |Vom Kunden erstellte Azure AD-App-ID.|
|aadServicePrincipalCertificate |Ja<sup>*</sup>  |Ja |Vom Kunden erstelltes Azure AD-App-ID-Zertifikat.|
|aadServicePrincipalPassword |Ja<sup>*</sup>  |Ja |Vom Kunden erstelltes Azure AD-App-ID-Kennwort.|
|aadEndpoint |Nein |Nein |Cloudspezifischer Azure AD-Endpunkt. <br/>Standard: `https://login.microsoftonline.com` |
|aadResourceId |Nein |Nein |Cloudspezifische Azure AD-Zielgruppen-/Ressourcen-ID. <br/>Standard: `https://management.core.windows.net/` |
|armEndpoint |Nein |Nein |Cloudspezifischer Azure Resource Manager-Endpunkt. <br/>Standard: `https://management.azure.com/` |
|diagnosticsLevel |Nein |Ja |Ausführlichkeit von Ereignissen: <br/>Information &#x02758; Warnung &#x02758; Fehler &#x02758; Kritisch &#x02758; Keine |
|diagnosticsEventsOutputName |Nein |Ja |Hub-Ausgabe für Diagnoseereignisse. <br/>(Leere Ausgabe bedeutet, dass keine Diagnosedaten veröffentlicht wurden.)|
|operationalEventsOutputName|Nein|Ja|Hub-Ausgabe für Betriebsereignisse.<br/>(Leere Ausgabe bedeutet, dass keine Betriebsereignisse veröffentlicht wurden.)
|logLevel|Nein|Ja|Einer der folgenden: <br/>&#x000B7; Ausführlich<br/>&#x000B7; Information (Standard)<br/>&#x000B7; Warnung<br/>&#x000B7; Fehler<br/>&#x000B7; Keine|
|logCategories|Nein|Ja|Eine durch Trennzeichen getrennte Liste mit folgenden Einträgen: Application, MediaPipeline, Events <br/>Standardwert: Application, Events|
|debugLogsDirectory|Nein|Ja|Verzeichnis für Debugprotokolle. Wenn es vorhanden ist, werden Protokolle generiert, und wenn es nicht vorhanden ist, werden Debugprotokolle deaktiviert.

<sup>*</sup>Sie MÜSSEN entweder ein Dienstprinzipalzertifikat der ein Kennwort angeben. 

Dynamische Eigenschaften können aktualisiert werden, ohne das Modul neu zu starten. Sie können die Werte für mehrere dieser Eigenschaften abrufen, indem Sie die Anleitung im Artikel [Abrufen von Anmeldeinformationen für den Zugriff auf die Media Services-API](../latest/access-api-cli-how-to.md) befolgen. 

Weitere Informationen zur Rolle der optionalen Diagnoseeinstellungen finden Sie im Artikel zur [Überwachung und Protokollierung](monitoring-logging.md).

```
{ 
    "properties.desired": { 
        // Required 
        "applicationDataDirectory": "/var/lib/azuremediaservices", 
        "azureMediaServicesArmId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/<rgname>/providers/microsoft.media/mediaservices/<ams_account>", 
        "aadTenantId": "00000000-0000-0000-0000-000000000000", 
        "aadServicePrincipalAppId": "00000000-0000-0000-0000-000000000000", 
        "aadServicePrincipalPassword": "{Service principal password}", 

        // Optional API Access 
        "aadEndpoint": "https://<aad-endpoint>", 
        "aadResourceId": "https://management.core.windows.net/", 
        "armEndpoint": "https://management.azure.com/", 
        
        // Optional Diagnostics 
        "diagnosticsEventsOutputName": "lvaEdgeDiagnostics",
        "operationalEventsOutputName": "lvaEdgeOperational",
        "logLevel": "Information",
        "logCategories": "Application,Events"
    } 
} 
```

## <a name="next-steps"></a>Nächste Schritte

[Direkte Methoden](direct-methods.md)
