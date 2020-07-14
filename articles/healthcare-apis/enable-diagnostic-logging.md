---
title: Aktivieren der Diagnoseprotokollierung in Azure API for FHIR®
description: In diesem Artikel wird das Aktivieren der Diagnoseprotokollierung in Azure API for FHIR® erläutert.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.reviewer: dseven
ms.author: cavoeg
author: CaitlinV39
ms.date: 11/01/2019
ms.openlocfilehash: 948ca03b5bf503c884df5df56c61951b381874a9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84870870"
---
# <a name="enable-diagnostic-logging-in-azure-api-for-fhir"></a>Aktivieren der Diagnoseprotokollierung in Azure API for FHIR®

In diesem Artikel erfahren Sie, wie Sie die Diagnoseprotokollierung in Azure API for FHIR® aktivieren und einige Beispiele von Abfragen dieser Protokolle einsehen können. Der Zugriff auf Diagnoseprotokolle ist für jeden Dienst im Gesundheitswesen unerlässlich, bei dem die Einhaltung gesetzlicher Vorschriften (wie z. B. HIPAA) zwingend erforderlich ist. Eine Option in Azure API for FHIR®, die Diagnoseprotokolle aktiviert, ist [**Diagnoseeinstellungen**](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings) im Azure-Portal. 

## <a name="enable-audit-logs"></a>Aktivieren von Überwachungsprotokollen
1. Um die Diagnoseprotokollierung in der Azure API for FHIR® zu aktivieren, wählen Sie Ihren Dienst „Azure API for FHIR®“ im Azure-Portal aus. 
2. Navigieren Sie zu **Diagnoseeinstellungen** 
![Diagnoseeinstellungen](media/diagnostic-logging/diagnostic-settings-screen.png). 

3. Wählen Sie **+ Diagnoseeinstellung hinzufügen**  aus.

4. Geben Sie einen Namen für die Einstellung ein.

5. Wählen Sie die Methode aus, die Sie für den Zugriff auf Ihre Diagnoseprotokolle verwenden möchten:

    1. **Archivieren Sie Protokolle zur (manuellen) Überprüfung in einem Speicherkonto**. Das Speicherkonto, das Sie verwenden möchten, muss bereits erstellt worden sein.
    2. **Streamen Sie die Protokolle zu Event Hub**, damit sie von einem Dienst eines Drittanbieters oder einer benutzerdefinierten Analyselösung erfasst werden können. Sie müssen einen Event Hub-Namespace und eine Event Hub-Richtlinie erstellen, ehe Sie diesen Schritt konfigurieren können.
    3. **Streamen Sie die Protokolle zum Log Analytics-Arbeitsbereich** in Azure Monitor. Sie müssen zuvor Ihren Log Analytics-Arbeitsbereich erstellen, bevor Sie diese Option auswählen können.

6. Wählen Sie **AuditLogs** und alle Metriken aus, die Sie erfassen möchten.

7. Klicken Sie auf „Speichern“.

> [!Note] 
> Es kann bis zu 15 Minuten dauern, bis die ersten Protokolle in Log Analytics angezeigt werden.  
 
Weitere Informationen zum Arbeiten mit Diagnoseprotokollen finden Sie in der Dokumentation zu [Azure-Ressourcenprotokollen](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-overview).

## <a name="audit-log-details"></a>Überwachungsprotokolldetails
Derzeit gibt der Dienst „Azure API for FHIR®“ im Überwachungsprotokoll die folgenden Felder zurück: 

|Feldname  |type  |Notizen  |
|---------|---------|---------|
|CallerIdentity|Dynamisch|Ein generischer Eigenschaftenbehälter mit Identitätsinformationen
|CallerIdentityIssuer|String|Issuer (Aussteller) 
|CallerIdentityObjectId|String|Object_Id 
|CallerIPAddress|String|Die IP-Adresse des Aufrufers 
|CorrelationId|String| Korrelations-ID
|FhirResourceType|String|Der Ressourcentyp, für den der Vorgang ausgeführt wurde
|LogCategory|String|Die Protokollkategorie (wir geben derzeit AuditLogs als LogCategory zurück)
|Standort|String|Der Standort des Servers, der die Anforderung verarbeitet hat (z. B. USA, Süden-Mitte)
|OperationDuration|Int|Der Zeitaufwand für die Bearbeitung dieser Anforderung in Sekunden
|Vorgangsname|String| Beschreibt den Typ des Vorgangs (z. b. Aktualisierung, Suchtyp)
|RequestUri|String|Der Anforderungs-URI 
|ResultType|String|Die derzeit verfügbaren Werte sind **Started**, **Succeeded** oder **Failed**.
|StatusCode|Int|Der HTTP-Statuscode. (Beispiel: 200) 
|TimeGenerated|Datetime|Datum und Uhrzeit des Ereignisses|
|Eigenschaften|String| Beschreibt die Eigenschaften von fhirResourceType
|SourceSystem|String| Quellsystem (in diesem Fall immer Azure)
|TenantId|String|Mandanten-ID
|type|String|Typ des Protokolls (in diesem Fall immer MicrosoftHealthcareApisAuditLog)
|_ResourceId|String|Details zur Ressource

## <a name="sample-queries"></a>Beispielabfragen

Es folgen einige einfache Application Insights-Abfragen, mit denen Sie Ihre Protokolldaten erkunden können.

Führen Sie diese Abfrage aus, um die **letzten 100 Protokolle** anzuzeigen:

```Application Insights
MicrosoftHealthcareApisAuditLogs
| limit 100
```

Führen Sie diese Abfrage aus, um die Vorgänge nach **FHIR-Ressourcentyp** zu gruppieren:

```Application Insights
MicrosoftHealthcareApisAuditLogs 
| summarize count() by FhirResourceType
```

Führen Sie diese Abfrage aus, um alle **fehlgeschlagenen Ergebnisse** anzuzeigen.

```Application Insights
MicrosoftHealthcareApisAuditLogs 
| where ResultType == "Failed" 
```

## <a name="conclusion"></a>Zusammenfassung 
Der Zugriff auf Diagnoseprotokolle ist für die Überwachung eines Diensts und die Bereitstellung von Berichten zur Compliance unerlässlich. Azure API for FHIR® ermöglicht Ihnen diese Aufgaben mithilfe von Diagnoseprotokollen. 
 
FHIR® ist ein eingetragenes Markenzeichen von HL7 und wird mit Erlaubnis von HL7 verwendet.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Überwachungsprotokolle für Azure API for FHIR® aktiviert werden können. Im nächsten Artikel lernen Sie weitere Einstellungen kennen, die Sie in Azure API for FHIR konfigurieren können.
 
>[!div class="nextstepaction"]
>[Zusätzliche Einstellungen](azure-api-for-fhir-additional-settings.md)