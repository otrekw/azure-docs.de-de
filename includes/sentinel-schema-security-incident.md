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
ms.openlocfilehash: 63cb53dc60a718892d4bf86140e7fd51303bd61c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88761717"
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
| **ClosedTime** | datetime | Zeitstempel (UTC) für letzten Abschluss des Incidents |
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
