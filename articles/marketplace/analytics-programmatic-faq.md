---
title: Häufig gestellte Fragen zum programmgesteuerten Zugriff auf Analysedaten
description: Hier finden Sie häufig gestellte Fragen zum programmgesteuerten Zugriff auf Analysedaten in Partner Center für Ihre Angebote im kommerziellen Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 393a718632138f4ffcf26e4875eea9ba3d886897
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102583491"
---
# <a name="programmatic-access-of-analytics-data-common-questions"></a>Häufig gestellte Fragen zum programmgesteuerten Zugriff auf Analysedaten

In diesem Artikel werden häufig gestellte Fragen zum programmgesteuerten Zugriff auf Analysedaten in Partner Center für Ihre Angebote im kommerziellen Marketplace behandelt.

## <a name="api-responses"></a>API-Antworten

In welchen verschiedenen Szenarien kann ich eine andere API-Antwort als 200 (Erfolg) erhalten?

In dieser Tabelle werden die API-Antworten und die Vorgehensweisen beim Empfang beschrieben.

| Fehlerbeschreibung | Fehlercode | Problembehandlung |
| ------------ | ------------- | ------------- |
| Nicht autorisiert | 401 | Dies ist eine Authentifizierungsausnahme. Prüfen Sie die Richtigkeit des Azure AD-Tokens (Azure Active Directory). Das Azure AD-Token ist 60 Minuten lang gültig. Danach müssen Sie es neu generieren. |
| Ungültiger Tabellenname | 400 | Der Name des Datasets ist falsch. Überprüfen Sie den Datasetnamen, indem Sie die API zum Abrufen aller Datasets aufrufen. |
| Falscher Spaltenname | 400| Der Name der Spalte in der Abfrage ist falsch. Überprüfen Sie den Spaltennamen, indem Sie die API zum Abrufen aller Datasets aufrufen, oder sehen Sie sich die Spaltennamen in den folgenden Tabellen an:<br><ul><li>[Tabelle „Auftragsdetails“](orders-dashboard.md#orders-details-table)</li><li>[Tabelle „Nutzungsdetails“](usage-dashboard.md#usage-details-table)</li><li>[Tabelle der Kundendetails](customer-dashboard.md#customer-details-table)</li><li>[Marketplace-Erkenntnisse: Tabelle „Details“](insights-dashboard.md#marketplace-insights-details-table)</li></UL> |
| Nullwert oder fehlender Wert | 400 | Es fehlen möglicherweise erforderliche Parameter als Teil der Anforderungsnutzlast der API. |
| Ungültige Berichtsparameter | 400 | Stellen Sie sicher, dass die Berichtsparameter korrekt sind. Für den Parameter `RecurrenceInterval` könnte beispielsweise ein kleinerer Wert als 4 angeben sein. |
| Zeitintervall muss zwischen 4 und 90 liegen | 400 | Stellen Sie sicher, dass der Wert des Anforderungsparameters `RecurrenceInterval` zwischen 4 und 90 liegt. |
| Ungültige Abfrage-ID (QueryId) | 400 | Prüfen Sie die generierte `QueryId`. |
| Ungültige Berichtsparameter für Erstellung: Startzeit des Berichts sollte sich mindestens vier Stunden von der aktuellen UTC-Zeit unterscheiden | 400 | Der Startzeitparameter als Teil der Anforderungsnutzlast sollte nicht in der Vergangenheit liegen. Die Startzeit des Berichts sollte sich mindestens vier Stunden von der aktuellen UTC-Zeit unterscheiden. |
| Angeforderter Wert „string“ wurde nicht gefunden | 400 | Prüfen Sie, ob Sie die Anforderungsparameter `callbackurl` oder `format` aktualisiert haben. |
| Es wurde kein Element mit den angegebenen Filtern gefunden. | 404 | Überprüfen Sie den in der API zum Abrufen von Berichtsausführungen verwendeten Parameter `reportID`. |
| Es haben keine Ausführungen für die angegebenen Filterbedingungen stattgefunden. Überprüfen Sie die Berichts-ID (reportId) oder Ausführungs-ID (executionId), und wiederholen Sie die API nach der geplanten Ausführungszeit des Berichts. | 404 | Stellen Sie sicher, dass die `reportId` korrekt ist. Wiederholen Sie die API nach der geplanten Ausführungszeit des Berichts, wie in der Anforderungsnutzlast angegeben. |
| Interner Fehler beim Erstellen des Berichts. Korrelations-ID <> | 500 | Stellen Sie sicher, dass das Datumsformat für die Felder „StartTime“, „QueryStartTime“ und „QueryEndTime“ korrekt ist. |
| Dienst nicht verfügbar | 500 | Wenn Sie ständig die Fehlermeldung erhalten, dass der Dienst nicht verfügbar ist (5xx-Fehler), erstellen Sie ein [Supportticket](support.md). |
||||

## <a name="no-records"></a>Keine Datensätze

Ich erhalte die API-Antwort 200, wenn ich den Bericht vom sicheren Speicherort herunterladen. Warum erhalte ich keine Datensätze?

Überprüfen Sie, ob die Zeichenfolge in der Abfrage einen der zulässigen Werte für den Spaltenheader aufweist. Die folgende Abfrage gibt beispielsweise keine Ergebnisse zurück:

`"SELECT UsageDate, NormalizedUsage, EstimatedExtendedChargePC FROM ISVUsage WHERE SKUBillingType = 'Paided' ORDER BY UsageDate DESC TIMESPAN LAST_MONTH"`

Die zulässigen Werte für „SKUBillingType“ sind in diesem Beispiel „Paid“ oder „Free“. In den folgenden Tabellen sind alle möglichen Werte für die verschiedenen Spalten aufgeführt:

- [Tabelle „Auftragsdetails“](orders-dashboard.md#orders-details-table)
- [Tabelle „Nutzungsdetails“](usage-dashboard.md#usage-details-table)
- [Tabelle der Kundendetails](customer-dashboard.md#customer-details-table)
- [Marketplace-Erkenntnisse: Tabelle „Details“](insights-dashboard.md#marketplace-insights-details-table)

## <a name="next-steps"></a>Nächste Schritte

- [APIs für den Zugriff auf kommerzielle Marketplace-Analysedaten](analytics-available-apis.md)
