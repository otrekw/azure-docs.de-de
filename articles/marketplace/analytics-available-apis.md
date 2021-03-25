---
title: APIs für den Zugriff auf kommerzielle Marketplace-Analysedaten
description: Greifen Sie mit diesen APIs programmgesteuert auf Analysedaten im Partner Center zu.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: ed6d658155267ab21fd4cdd28dd50fcbb258ee90
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102583651"
---
# <a name="apis-for-accessing-commercial-marketplace-analytics-data"></a>APIs für den Zugriff auf kommerzielle Marketplace-Analysedaten

Im Folgenden finden Sie die Liste der APIs für den Zugriff auf kommerzielle Marketplace-Analysedaten und die zugehörigen Funktionen.

- [Dataset-Pull-APIs](#dataset-pull-apis)
- [Abfrageverwaltung-APIs](#query-management-apis)
- [Berichtverwaltung-APIs](#report-management-apis)
- [Berichtsausführung-Pull-APIs](#report-execution-pull-apis)

## <a name="dataset-pull-apis"></a>Dataset-Pull-APIs

***Tabelle 1: Dataset-Pull-APIs***

| **API** | **Funktion** |
| --- | --- |
| [Abrufen aller Datasets](analytics-api-get-all-datasets.md) | Ruft alle verfügbaren Datasets ab. Zu den Datasets gehören Tabellen, Spalten, Metriken und Zeitbereiche. |
|||

## <a name="query-management-apis"></a>Abfrageverwaltung-APIs

***Tabelle 2: Abfrageverwaltung-APIs***

| **API** | **Funktion** |
| --- | --- |
| [Erstellen einer Berichtsabfrage](analytics-programmatic-access.md#create-report-query-api) | Erstellt benutzerdefinierte Abfragen, mit denen das Dataset definiert wird, aus dem Spalten und Metriken exportiert werden müssen. |
| [ABRUFEN von Berichtsabfragen](analytics-api-get-report-queries.md) | Ruft alle Abfragen ab, die für die Verwendung in Berichten verfügbar sind. Es werden standardmäßig alle System- und benutzerdefinierten Abfragen abgerufen. |
| [LÖSCHEN von Berichtsabfragen](analytics-api-delete-report-queries.md) | Löscht benutzerdefinierte Abfragen. |
|||

## <a name="report-management-apis"></a>Berichtverwaltung-APIs

***Tabelle 3: Berichtverwaltung-APIs***

| **API** | **Funktion** |
| --- | --- |
| [Erstellen von Berichten](analytics-programmatic-access.md#create-report-api) | Plant die Ausführung einer Abfrage in regelmäßigen Abständen. |
| [VERSUCHEN von Berichtsabfragen](analytics-api-try-report-queries.md) | Führt eine Berichtsabfrageanweisung aus. Gibt nur 10 Datensätze zurück, die ein Partner verwenden kann, um zu überprüfen, ob die Daten den Erwartungen entsprechen. |
| [Bericht abrufen](analytics-api-get-report.md) | Alle geplanten Berichte abrufen. |
| [Bericht aktualisieren](analytics-api-update-report.md) | Einen Berichtsparameter ändern. |
| [Bericht löschen](analytics-api-delete-report.md) | Alle Berichts- und Berichtsausführungs-Datensätze löschen. |
| [Berichtsausführungen anhalten](analytics-api-pause-report-executions.md) | Die geplante Ausführung von Berichten anhalten. |
| [Berichtsausführungen fortsetzen](analytics-api-resume-report-executions.md) | Die geplante Ausführung eines angehaltenen Berichts fortsetzen. |
|||

## <a name="report-execution-pull-apis"></a>Berichtsausführung-Pull-APIs

***Tabelle 4: Berichtsausführung-Pull-APIs***

| **API** | **Funktion** |
| --- | --- |
| [Berichtsausführungen abrufen](analytics-programmatic-access.md#get-report-executions-api) | Alle Ausführungen für einen bestimmten Bericht abrufen. |
|||

## <a name="next-steps"></a>Nächste Schritte

- Sie können die APIs über die [Swagger-API-URL](https://api.partnercenter.microsoft.com/insights/v1/cmp/swagger/index.html) testen.
