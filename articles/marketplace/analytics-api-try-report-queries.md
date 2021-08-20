---
title: API zum Testen von Berichtsabfragen
description: Verwenden Sie diese API, um eine Berichtsabfrage für Analyseberichte im kommerzielle Marketplace auszuführen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: smannepalle
ms.author: smannepalle
ms.reviewer: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 74dfed4697942ba921cda11dfba8698ad822c8c4
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355889"
---
# <a name="try-report-queries-api"></a>API zum Testen von Berichtsabfragen

Diese API führt eine Berichtsabfrageanweisung aus. Die API gibt nur 10 Datensätze zurück, die Sie als Partner verwenden können, um zu überprüfen, ob die Daten den Erwartungen entsprechen.

> [!IMPORTANT]
> Das Timeout für die Abfrageausführung beträgt bei dieser API 100 Sekunden. Wenn Sie bemerken, dass die API-Ausführung länger als 100 Sekunden dauert, ist die Abfrage sehr wahrscheinlich syntaktisch korrekt, da Sie andernfalls einen anderen Fehlercode als 200 erhalten. Die tatsächliche Berichterstellung gilt als bestanden, wenn die Abfragesyntax korrekt ist.

**Anforderungssyntax**

| **Methode** | **Anforderungs-URI** |
| --- | --- |
| GET | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledQueries/testQueryResult?exportQuery={query text}` |
|||

**Anforderungsheader**

| **Header** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken (Azure Active Directory) in der Form `Bearer <token>`. |
| Content-Type | Zeichenfolge | `Application/JSON` |
|||

**QueryParameter**

| **Parametername** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| `exportQuery` | string | Berichtsabfragezeichenfolge, die ausgeführt werden soll |
| `queryId` | Zeichenfolge | Eine gültige vorhandene Abfrage-ID |
|||

**Pfadparameter**

Keine

**Anforderungsnutzdaten**

Keine

**Glossar**

Keine

**Antwort**

Die Antwortnutzdaten sind wie folgt strukturiert:

Antwortcode: 200, 400, 401, 403, 404, 500

Antwortnutzdaten: erste 10 Zeilen der Abfrageausführung
