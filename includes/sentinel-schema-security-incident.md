---
title: include file
description: include file
services: azure-sentinel
author: yelevin
ms.service: azure-sentinel
ms.topic: include
ms.date: 06/28/2020
ms.author: yelevin
ms.custom: include file
ms.openlocfilehash: 76020b3c1f28e5b5f6363aef181b76bc93a9613e
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87293776"
---
### <a name="the-data-model-of-the-schema"></a>Das Datenmodell des Schemas

| Feld | Datentyp | BESCHREIBUNG |
| ---- | ---- | ---- |
| **AdditionalData** | dynamisch | Anzahl von Warnungen, Lesezeichen und Kommentaren sowie Namen und Taktiken von Warnungsprodukten |
| **AlertIds** | dynamisch | Warnungen, aus denen der Incident erstellt wurde |
| **BookmarkIds** | dynamisch | Mit Lesezeichen markierte Entitäten |
| **Klassifizierung** | Zeichenfolge | Abschlussklassifizierung des Incidents |
| **ClassificationComment** | Zeichenfolge | Kommentar zur Abschlussklassifizierung des Incidents |
| **ClassificationReason** | Zeichenfolge | Grund für Abschlussklassifizierung des Incidents |
| **ClosureTime** | datetime | Zeitstempel (UTC) für letzten Abschluss des Incidents |
| **Kommentare** | dynamisch | Kommentare zum Incident |
| **CreatedTime** | datetime | Zeitstempel (UTC) für Erstellung des Incidents |
| **Beschreibung** | string | Beschreibung des Vorfalls |
| **FirstActivityTime** | datetime | Erste Ereigniszeit |
| **FirstModifiedTime** | datetime | Zeitstempel (UTC) für erste Änderung des Incidents |
| **IncidentName** | Zeichenfolge | Interne GUID |
| **IncidentNumber** | INT |  |
| **IncidentUrl** | Zeichenfolge | Link zum Incident |
| **Bezeichnungen** | dynamisch | `Tags` |
| **LastActivityTime** | datetime | Letzte Ereigniszeit |
| **LastModifiedTime** | datetime | Zeitstempel (UTC) für letzte Änderung des Incidents <br>(die durch den aktuellen Datensatz beschriebene Änderung) |
| **ModifiedBy** | Zeichenfolge | Benutzer oder System, der/das den Incident geändert hat |
| **Besitzer** | dynamisch |  |
| **RelatedAnalyticRuleIds** | dynamisch | Regeln, durch die Warnungen für den Incident ausgelöst wurden |
| **Severity** | Zeichenfolge | Schweregrad des Incidents (hoch/mittel/niedrig/Information) |
| **SourceSystem** | Zeichenfolge | Konstante ('Azure') |
| **Status** | Zeichenfolge |  |
| **TenantId** | Zeichenfolge |  |
| **TimeGenerated** | datetime | Zeitstempel (UTC) für Erstellung des aktuellen Datensatzes <br>(bei Änderung des Incidents) |
| **Titel** | Zeichenfolge | 
| **Type** | Zeichenfolge | Konstante ('SecurityIncident') |
|
