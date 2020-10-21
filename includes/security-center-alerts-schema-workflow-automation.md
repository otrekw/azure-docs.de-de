---
title: include file
description: include file
services: data-factory
author: memildin
ms.service: data-factory
ms.topic: include
ms.date: 03/10/2020
ms.author: memildin
ms.custom: include file
ms.openlocfilehash: 55390a3eb2a074729b4a0868416a95e208325b76
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91401111"
---
```json
{
  "VendorName": "Microsoft",
  "AlertType": "SUSPECT_SVCHOST",
  "StartTimeUtc": "2016-12-20T13:38:00.000Z",
  "EndTimeUtc": "2019-12-20T13:40:01.733Z",
  "ProcessingEndTime": "2019-09-16T12:10:19.5673533Z",
  "TimeGenerated": "2016-12-20T13:38:03.000Z",
  "IsIncident": false,
  "Severity": "High",
  "Status": "New",
  "ProductName": "Azure Security Center",
  "SystemAlertId": "2342409243234234_F2BFED55-5997-4FEA-95BD-BB7C6DDCD061",
  "CompromisedEntity": "WebSrv1",
  "Intent": "Execution",
  "AlertDisplayName": "Suspicious process detected",
  "Description": "Suspicious process named 'SVCHOST.EXE' was running from path: %{Process Path}",
  "RemediationSteps": ["contact your security information team"],
  "ExtendedProperties": {
    "Process Path": "c:\\temp\\svchost.exe",
    "Account": "Contoso\\administrator",
    "PID": 944,
    "ActionTaken": "Detected"
  },
  "Entities": [],
  "ResourceIdentifiers": [
        {
            Type: "AzureResource",
            AzureResourceId: "/subscriptions/86057C9F-3CDD-484E-83B1-7BF1C17A9FF8/resourceGroups/backend-srv/providers/Microsoft.Compute/WebSrv1"
        },
        {
            Type: "LogAnalytics",
            WorkspaceId: "077BA6B7-8759-4F41-9F97-017EB7D3E0A8",
            WorkspaceSubscriptionId: "86057C9F-3CDD-484E-83B1-7BF1C17A9FF8",
            WorkspaceResourceGroup: "omsrg",
            AgentId: "5A651129-98E6-4E6C-B2CE-AB89BD815616",
        }
  ]
}
```

### <a name="the-data-model-of-the-schema"></a>Das Datenmodell des Schemas

|Feld|Datentyp|BESCHREIBUNG|
|----|----|----|
|**AlertDisplayName**|String|Der Anzeigename der Warnung.|
|**AlertType**|String|Der Warnungstyp. Warnungen desselben Typs sollten denselben Wert aufweisen. Dieses Feld ist eine Schlüsselzeichenfolge, die den Typ der Warnung und nicht einer Warnungsinstanz darstellt. Alle Warnungsinstanzen derselben Erkennungslogik/Analyse sollten denselben Wert für den Warnungstyp aufweisen.|
|**CompromisedEntity**|String|Der Anzeigename der Ressource, die in der Regel mit dieser Warnung im Zusammenhang steht.|
|**Description**|String|Beschreibung der Warnung.|
|**EndTimeUtc**|DateTime|Der Zeitpunkt des letzten Ereignisses oder der letzten Aktivität in der Warnung.  Das Feld muss eine Zeichenfolge sein, die dem ISO8601-Format entspricht, einschließlich UTC-Zeitzoneninformationen.|
|**Entities**|IEnumerable (IEntity)|Eine Liste der Entitäten im Zusammenhang mit der Warnung. Diese Liste kann eine Mischung aus Entitäten unterschiedlicher Typen enthalten. Der Entitätentyp kann einer der im Abschnitt „Entities“ definierten Typen sein. Entitäten, die nicht in der folgenden Liste enthalten sind, können ebenfalls gesendet werden. Es ist jedoch nicht sichergestellt, dass Sie verarbeitet werden (bei neuen Entitätstypen schlägt eine Überprüfung der Warnung nicht fehl).|
|**ExtendedProperties**|Dictionary (String,String)|Anbieter können hier (optional) benutzerdefinierte Felder einschließen.|
|**Intent**|Enum|Die mit der Kill Chain im Zusammenhang stehende Absicht hinter der Warnung. Eine Liste der unterstützten Werte und Erläuterungen der vom Azure Security Center unterstützten Kill Chain-Absichten finden Sie unter [Absichten](../articles/security-center/alerts-reference.md#intentions).<br/>Dieses Feld kann mehrere Werte enthalten (durch Kommas getrennt).|
|**IsIncident**|Bool|Dieses Feld bestimmt, ob es sich bei der Warnung um einen Incident (eine zusammengesetzte Gruppierung mehrerer Warnungen) oder eine einzelne Warnung handelt. Der Standardwert für das Feld lautet „false“ (das bedeutet, dass es sich um eine einzelne Warnung handelt).|
|**ProcessingEndTime**|Datetime|Der Zeitpunkt, zu dem die Warnung für den Endbenutzer im ursprünglichen Produkt mit der Warnung zugänglich war.|
|**ProductName**|String|Der Name des Produkts, das diese Warnung veröffentlicht hat (Azure Security Center, Azure ATP, Microsoft Defender ATP, MCAS usw.).|
|**RemediationSteps**|List<String>|Manuelle Aktionselemente zum Beheben der Warnung.|
|**ResourceIdentifiers**|List (Resource Identifiers)|Die Ressourcenbezeichner für diese Warnung, mit deren Hilfe die Warnung an die richtige Produktverfügbarkeitsgruppe (Mandant, Arbeitsbereich, Abonnement usw.) weitergeleitet werden kann. Es können mehrere Bezeichner unterschiedlichen Typs pro Warnung vorhanden sein.|
|**Severity**|Enum|Der vom Anbieter gemeldete Schweregrad der Warnung. Mögliche Werte: Information, Niedrig, Mittel und Hoch.|
|**StartTimeUtc**|DateTime|Der Zeitpunkt des ersten Ereignisses oder der ersten Aktivität in der Warnung. Das Feld muss eine Zeichenfolge sein, die dem ISO8601-Format entspricht, einschließlich UTC-Zeitzoneninformationen.|
|**Status**|Enum|Der Lebenszyklusstatus der Warnung.<br/>Folgende Status werden unterstützt: „New“ (Neu), „Resolved“ (Aufgelöst), „Dismissed“ (Verworfen), „Unknown“ (Unbekannt).<br/>Einer Warnung, die einen anderen Wert als die unterstützten Optionen angibt, wird der Status „Unknown“ zugewiesen.<br/>Einer Warnung, die keinen Wert angibt, wird der Status „New“ zugewiesen.|
|**SystemAlertId**|String|Der Warnungsbezeichner.|
|**TimeGenerated**|DateTime|Der Zeitpunkt, zu dem die Warnung vom Warnungsanbieter generiert wurde. Wenn die Warnung nicht von einem internen Warnungsanbieter gemeldet wird, kann ein Produkt den Zeitpunkt zuweisen, zu dem sie zur Verarbeitung durch das Produkt empfangen wurde.  Das Feld muss eine Zeichenfolge sein, die dem ISO8601-Format entspricht, einschließlich UTC-Zeitzoneninformationen.|
|**VendorName**|String|Der Name des Anbieters, der die Warnung auslöst.|
|||
