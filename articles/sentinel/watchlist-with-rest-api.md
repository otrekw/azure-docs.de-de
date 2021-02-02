---
title: Verwalten von Watchlists in Azure Sentinel mithilfe der REST-API | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Azure Sentinel-Watchlists mithilfe der REST-API von Log Analytics verwalten können, um Watchlists und deren Elemente zu erstellen, zu ändern und zu löschen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: reference
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2021
ms.author: yelevin
ms.openlocfilehash: ea571f9b033ba82709a13c6d32649f3228ee04b1
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98798024"
---
# <a name="manage-watchlists-in-azure-sentinel-using-rest-api"></a>Verwalten von Watchlists in Azure Sentinel mithilfe der REST-API

> [!IMPORTANT]
> Das Feature Watchlists befindet sich derzeit in der **VORSCHAU**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

Mit Azure Sentinel, das teilweise auf Azure Monitor Log Analytics basiert, können Sie mit der REST-API von Log Analytics Watchlists verwalten. In diesem Dokument erfahren Sie, wie Sie Watchlists und deren Elemente mithilfe der REST-API erstellen, ändern und löschen.  Watchlists, die auf diese Weise erstellt werden, werden in der Azure Sentinel-Benutzeroberfläche angezeigt.

## <a name="common-uri-parameters"></a>Allgemeine URI-Parameter

Im Folgenden finden Sie die allgemeinen URI-Parameter für alle Watchlist-API-Befehle:

| Name | Geben Sie in | Erforderlich | type | Beschreibung |
|-|-|-|-|-|
| **{subscriptionId}** | path | ja | GUID | die Azure-Abonnement-ID |
| **{resourceGroupName}** | path | ja | Zeichenfolge | der Name der Ressourcengruppe innerhalb des Abonnements |
| **{workspaceName}** | path | ja | Zeichenfolge | der Name des Log Analytics-Arbeitsbereichs |
| **{watchlistAlias}** | path | Ja* | Zeichenfolge | der Name einer bestimmten Watchlist<br>\* Nicht erforderlich, wenn alle Watchlists abgerufen werden |
| **{watchlistItemId}** | path | ja** | GUID | die ID des Elements, das in der Watchlist erstellt, hinzugefügt oder aus dieser entfernt werden soll<br>\*\* Nur für Watchlist-Elementbefehle erforderlich |
| **{api-version}** | Abfrage | ja | Zeichenfolge | die Version des für die Anforderung verwendeten Protokolls. Ab dem 29. Oktober 2020 ist die Watchlist-API-Version *2019-01-01-Preview* |
| **{bearerToken}** | authorization | ja | token | das Bearerautorisierungstoken |
|  

## <a name="retrieve-all-watchlists"></a>Abrufen aller Watchlists

Mit diesem Befehl werden alle einem Arbeitsbereich zugeordneten Watchlists ohne deren Elemente abgerufen.

### <a name="request-uri"></a>Anforderungs-URI
(Der URI ist eine einzelne, zur leichteren Lesbarkeit umbrochene Zeile.)

| Methode | Anforderungs-URI |
|-|-|
| GET | `https://management.azure.com/subscriptions/{{subscriptionId}}/`<br>`resourceGroups/{{resourceGroupName}}/`<br>`providers/Microsoft.OperationalInsights/`<br>`workspaces/{{workspaceName}}/`<br>`providers/Microsoft.SecurityInsights/`<br>`watchlists?api-version={{api-version}}` |
|

### <a name="responses"></a>Antworten

| Statuscode | Antworttext | Beschreibung |
|-|-|-|
| 200 / OK | Liste vorhandener Watchlists oder leer, wenn keine Watchlist gefunden wird |  |
| 400 / Ungültige Anforderung |  | Falsch formatierte Anforderungssyntax, ungültiger Anforderungsparameter... |
|

## <a name="look-up-a-watchlist-by-name"></a>Suchen einer Watchlist nach Namen

Mit diesem Befehl wird eine bestimmte, einem Arbeitsbereich zugeordnete Watchlist ohne deren Elemente abgerufen.

### <a name="request-uri"></a>Anforderungs-URI
(Der URI ist eine einzelne, zur leichteren Lesbarkeit umbrochene Zeile.)

| Methode | Anforderungs-URI |
|-|-|
| GET | `https://management.azure.com/subscriptions/{{subscriptionId}}/`<br>`resourceGroups/{{resourceGroupName}}/`<br>`providers/Microsoft.OperationalInsights/`<br>`workspaces/{{workspaceName}}/`<br>`providers/Microsoft.SecurityInsights/`<br>`watchlists/{{watchlistAlias}}?api-version={{api-version}}` |
|

### <a name="responses"></a>Antworten

| Statuscode | Antworttext | Beschreibung |
|-|-|-|
| 200 / OK | Die angeforderte Watchlist |  |
| 400 / Ungültige Anforderung |  | Falsch formatierte Anforderungssyntax, ungültiger Anforderungsparameter... |
| 404 / Nicht gefunden |  | Keine Watchlist mit dem angeforderten Namen gefunden |
|

## <a name="create-a-watchlist"></a>Erstellen einer Watchlist

Mit diesem Befehl werden eine Watchlist erstellt und ihr Elemente hinzugefügt. Zwei Aufrufe dieses Endpunkts sind erforderlich, um die Watchlist und ihre Elemente zu erstellen: Der erste erstellt die Watchlist (übergeordnet), und der zweite fügt die Elemente hinzu.

### <a name="request-uri"></a>Anforderungs-URI
(Der URI ist eine einzelne, zur leichteren Lesbarkeit umbrochene Zeile.)

| Methode | Anforderungs-URI |
|-|-|
| PUT | `https://management.azure.com/subscriptions/{{subscriptionId}}/`<br>`resourceGroups/{{resourceGroupName}}/`<br>`providers/Microsoft.OperationalInsights/`<br>`workspaces/{{workspaceName}}/`<br>`providers/Microsoft.SecurityInsights/`<br>`watchlists/{{watchlistAlias}}?api-version={{api-version}}` |
|

### <a name="request-body"></a>Anforderungstext

Im Folgenden finden Sie ein Beispiel für den Anforderungstext einer Anforderung zum Erstellen einer Watchlist:

```json
{
    "properties": {
        "displayName": "High Value Assets Watchlist",
        "source": "Local file",
        "provider": "Microsoft",
        "numberOfLinesToSkip":"1",
        "rawContent": "metadata line\nheader1, header2\nval1, val2",
        "contentType": "text/csv"
    }
}
```

### <a name="responses"></a>Antworten

| Statuscode | Antworttext | Beschreibung |
|-|-|-|
| 200 / OK | Die von der Anforderung erstellte Watchlist ohne Elemente |  |
| 400 / Ungültige Anforderung |  | Falsch formatierte Anforderungssyntax, ungültiger Anforderungsparameter... |
| 409 / Konflikt |  | Fehler beim Vorgang, Watchlist mit demselben Alias ist vorhanden |
|

## <a name="delete-a-watchlist"></a>Löschen einer Watchlist

Dieser Befehl löscht eine Watchlist und deren Elemente.

### <a name="request-uri"></a>Anforderungs-URI
(Der URI ist eine einzelne, zur leichteren Lesbarkeit umbrochene Zeile.)

| Methode | Anforderungs-URI |
|-|-|
| DELETE | `https://management.azure.com/subscriptions/{{subscriptionId}}/`<br>`resourceGroups/{{resourceGroupName}}/`<br>`providers/Microsoft.OperationalInsights/`<br>`workspaces/{{workspaceName}}/`<br>`providers/Microsoft.SecurityInsights/`<br>`watchlists/{{watchlistAlias}}?api-version={{api-version}}` |
|

### <a name="responses"></a>Antworten

| Statuscode | Antworttext | Beschreibung |
|-|-|-|
| 200 / OK | Leerer Antworttext |  |
| 204 / Kein Inhalt | Leerer Antworttext | Nichts gelöscht |
| 400 / Ungültige Anforderung |  | Falsch formatierte Anforderungssyntax, ungültiger Anforderungsparameter... |
|

## <a name="add-or-update-a-watchlist-item"></a>Hinzufügen oder Aktualisieren eines Watchlist-Elements

Wenn dieser Befehl für eine vorhandene *watchlisItemId* (eine GUID) ausgeführt wird, aktualisiert er das Element mit den Daten, die im Anforderungstext bereitgestellt werden. Andernfalls wird ein neues Element erstellt.

### <a name="request-uri"></a>Anforderungs-URI
(Der URI ist eine einzelne, zur leichteren Lesbarkeit umbrochene Zeile.)

| Methode | Anforderungs-URI |
|-|-|
| PUT | `https://management.azure.com/subscriptions/{{subscriptionId}}/`<br>`resourceGroups/{{resourceGroupName}}/`<br>`providers/Microsoft.OperationalInsights/`<br>`workspaces/{{workspaceName}}/`<br>`providers/Microsoft.SecurityInsights/`<br>`watchlists/{{watchlistAlias}}/`<br>`watchlistitems/{{watchlistItemId}}?api-version={{api-version}}` |
|

### <a name="request-body"></a>Anforderungstext

Im Folgenden finden Sie ein Beispiel für den Anforderungstext einer Anforderung zum Hinzufügen/Aktualisieren eines Watchlist-Elements:

```json
{
    "properties": {
      "itemsKeyValue": {
           "Gateway subnet": "10.0.255.224/27",
           "Web Tier": "10.0.1.0/24",
           "Business tier": "10.0.2.0/24",
           "Private DMZ in": "10.0.0.0/27",
           "Public DMZ out": "10.0.0.96/27"
      }
}
}
```

### <a name="responses"></a>Antworten

| Statuscode | Antworttext | Beschreibung |
|-|-|-|
| 200 / OK | Das aufgrund der Anforderung erstellte oder aktualisierte Watchlist-Element |  |
| 400 / Ungültige Anforderung |  | Falsch formatierte Anforderungssyntax, ungültiger Anforderungsparameter... |
| 409 / Konflikt |  | Fehler beim Vorgang, Watchlist mit demselben Alias ist vorhanden |
|

## <a name="delete-a-watchlist-item"></a>Löschen eines Watchlist-Elements

Mit diesem Befehl wird ein vorhandenes Watchlist-Element gelöscht.

### <a name="request-uri"></a>Anforderungs-URI
(Der URI ist eine einzelne, zur leichteren Lesbarkeit umbrochene Zeile.)

| Methode | Anforderungs-URI |
|-|-|
| DELETE | `https://management.azure.com/subscriptions/{{subscriptionId}}/`<br>`resourceGroups/{{resourceGroupName}}/`<br>`providers/Microsoft.OperationalInsights/`<br>`workspaces/{{workspaceName}}/`<br>`providers/Microsoft.SecurityInsights/`<br>`watchlists/{{watchlistAlias}}/`<br>`watchlistitems/{{watchlistItemId}}?api-version={{api-version}}` |
|

### <a name="responses"></a>Antworten

| Statuscode | Antworttext | Beschreibung |
|-|-|-|
| 200 / OK | Leerer Antworttext |  |
| 204 / Kein Inhalt | Leerer Antworttext | Nichts gelöscht |
| 400 / Ungültige Anforderung |  | Falsch formatierte Anforderungssyntax, ungültiger Anforderungsparameter... |
|

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Watchlists und ihre Elemente in Azure Sentinel mithilfe der Log Analytics-API verwalten. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Weitere Informationen zu [Watchlists](watchlists.md)
- Erkunden anderer Verwendungsmöglichkeiten der [Azure Sentinel-API](/rest/api/securityinsights/)