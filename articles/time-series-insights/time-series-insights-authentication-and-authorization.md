---
title: API-Authentifizierung und -Autorisierung – Azure Time Series Insights | Microsoft-Dokumentation
description: In diesem Artikel wird die Konfiguration der Authentifizierung und Autorisierung für eine benutzerdefinierte Anwendung erläutert, die die Azure Time Series Insights-API aufruft.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: shresha
manager: dpalled
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/18/2020
ms.custom: seodec18, has-adal-ref
ms.openlocfilehash: 94fef951bf1c5c9d69a9b49cd9465d7d248c74a7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85099220"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Authentifizierung und Autorisierung für die Azure Time Series Insights-API

Dieses Dokument beschreibt die Registrierung einer App in Azure Active Directory mit dem neuen Azure Active Directory-Blatt. Apps, die in Azure Active Directory registriert sind, ermöglichen es Benutzern, sich für die Azure Time Series Insights-API in einer Time Series Insights-Umgebung authentifizieren und autorisieren zu lassen.

## <a name="service-principal"></a>Dienstprinzipal

In den folgenden Abschnitten wird erläutert, wie Sie eine Anwendung für den Zugriff auf die Azure Time Series Insights-API für die Anwendung konfigurieren. Die Anwendung kann dann anhand der eigenen Anwendungsanmeldeinformationen über Azure Active Directory in der Time Series Insights-Umgebung Referenzdaten abfragen oder veröffentlichen.

## <a name="summary-and-best-practices"></a>Zusammenfassung und bewährte Methoden

Der Ablauf der Azure Active Directory-App-Registrierung besteht aus drei wichtigen Schritten.

1. [Registrieren einer Anwendung](#azure-active-directory-app-registration) in Azure Active Directory.
1. Autorisieren dieser Anwendung zum [Zugriff auf Daten der Time Series Insights-Umgebung](#granting-data-access).
1. Verwenden der **Anwendungs-ID** und des **Clientgeheimnisses**, um ein Token von `https://api.timeseries.azure.com/` in Ihrer [Client-App](#client-app-initialization) abzurufen. Mit dem Token kann dann die Time Series Insights-API aufgerufen werden.

Wenn Sie in **Schritt 3** die Anmeldeinformationen Ihrer Anwendung und Ihrer Benutzer trennen, ist Folgendes möglich:

* Zuweisen von Berechtigungen zur App-Identität, die sich von Ihren eigenen Berechtigungen unterscheiden. In der Regel sind diese Berechtigungen auf diejenigen beschränkt, die für die App erforderlich sind. Beispielsweise können Sie festlegen, dass die App nur in einer bestimmten Time Series Insights-Umgebung Daten lesen darf.
* Isolieren der Sicherheit der App von den Authentifizierungsinformationen des erstellenden Benutzers, indem Sie ein **Clientgeheimnis** oder ein Sicherheitszertifikat verwenden. Danach sind die Anmeldeinformationen der Anwendung nicht mehr von den Anmeldeinformationen eines bestimmten Benutzers abhängig. Wenn sich die Rolle des Benutzers ändert, erfordert die Anwendung nicht unbedingt neue Anmeldeinformationen oder weitere Konfiguration. Wenn der Benutzer sein Kennwort ändert, müssen für Zugriffe auf die Anwendung keine neuen Anmeldeinformationen oder Schlüssel erstellt werden.
* Ausführen eines unbeaufsichtigten Skripts mit einem **Clientgeheimnis** oder einen Sicherheitszertifikat anstatt mit bestimmten Benutzeranmeldeinformationen (für die der Benutzer anwesend sein muss).
* Verwenden eines Sicherheitszertifikats anstelle eines Kennworts, um den Zugriff auf Ihre Azure Time Series Insights-API zu sichern.

> [!IMPORTANT]
> Halten Sie sich an das Konzept der **Trennung der Belange** (wie für dieses Szenario oben beschrieben), wenn Sie ihre Azure Time Series Insights-Sicherheitsrichtlinien konfigurieren.

> [!NOTE]
> * Der Schwerpunkt des Artikels liegt dabei auf einer Anwendung mit nur einem Mandanten, die zur Ausführung in nur einer einzigen Organisation vorgesehen ist.
> * Sie setzen Anwendungen mit nur einem Mandanten in der Regel für Branchenanwendungen ein, die innerhalb Ihrer Organisation ausgeführt werden.

## <a name="detailed-setup"></a>Setup – detailliert

### <a name="azure-active-directory-app-registration"></a>Azure Active Directory-App-Registrierung

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

### <a name="granting-data-access"></a>Gewähren von Datenzugriff

1. Wählen Sie für die Time Series Insights-Umgebung **Datenzugriffsrichtlinien** und dann **Hinzufügen** aus.

   [![Hinzufügen einer neuen Datenzugriffsrichtlinie zur Time Series Insights-Umgebung](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. Fügen Sie im Dialogfeld **Benutzer auswählen** entweder den **Anwendungsnamen** oder die **Anwendungs-ID** aus dem Abschnitt „Azure Active Directory-App-Registrierung“ ein.

   [![Suchen einer Anwendung im Dialogfeld „Benutzer auswählen“](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Wählen Sie die Rolle aus. Wählen Sie **Leser** aus, um Daten abzufragen oder **Mitwirkender**, um Daten abzufragen und Referenzdaten zu ändern. Klicken Sie auf **OK**.

   [![Auswählen von „Leser“ oder „Mitwirkender“ im Dialogfeld „Benutzerrolle auswählen“](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Speichern Sie die Richtlinie mit **OK**.

   > [!TIP]
   > Informationen zu erweiterten Datenzugriffsoptionen finden Sie unter [Gewähren von Datenzugriff](./time-series-insights-data-access.md).

### <a name="client-app-initialization"></a>Initialisierung der Client-App

* Entwickler können die Microsoft Authentication Library (MSAL) für die Authentifizierung bei Azure Time Series Insights verwenden.

* So authentifizieren Sie sich über die ADAL:

   1. Verwenden Sie die **Anwendungs-ID** und das **Clientgeheimnis** (Anwendungsschlüssel) aus der Azure Active Directory-App-Registrierung, um das Token für die Anwendung abzurufen.

   1. In C# können Sie das Token für die Anwendung mit dem folgenden Code abrufen. Ein vollständiges Beispiel finden Sie unter [Abfragen von Daten aus der Azure Time Series Insights GA-Umgebung mit C#](time-series-insights-query-data-csharp.md).

        [!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs?range=170-199)]

   1. Das Token kann dann im `Authorization`-Header übergeben werden, wenn die Anwendung die Time Series Insights-API aufruft.

> [!IMPORTANT]
> Wenn Sie die [Active Directory-Authentifizierungsbibliothek (ADAL)](https://docs.microsoft.com/azure/active-directory/azuread-dev/active-directory-authentication-libraries) verwenden, informieren Sie sich über die [Migration zu MSAL](https://docs.microsoft.com/azure/active-directory/develop/msal-net-migration).

    See our [Manage GA reference data for an Azure Time Series Insights environment using C#](time-series-insights-manage-reference-data-csharp.md) article to learn more.

## <a name="common-headers-and-parameters"></a>Allgemeine Parameter und Header

In diesem Abschnitt werden allgemeine HTTP-Anforderungsheader und Parameter beschrieben, mit denen Abfragen der Time Series Insights GA- und Vorschau-APIs durchgeführt werden. API-spezifische Anforderungen werden ausführlicher in der [Referenzdokumentation zur Time Series Insights-REST-API](https://docs.microsoft.com/rest/api/time-series-insights/) behandelt.

> [!TIP]
> Weitere Informationen zum Nutzen von REST-APIs, Ausführen von HTTP-Anforderungen und Verarbeiten von HTTP-Antworten finden Sie in der [Azure-Rest-API-Referenz](https://docs.microsoft.com/rest/api/azure/).

### <a name="authentication"></a>Authentifizierung

Um authentifizierte Abfragen der [Time Series Insights-REST-APIs](https://docs.microsoft.com/rest/api/time-series-insights/) durchzuführen, muss ein gültiges OAuth 2.0-Bearertoken im [Autorisierungsheader](/rest/api/apimanagement/2019-12-01/authorizationserver/createorupdate) mit einem REST-Client Ihrer Wahl (Postman, JavaScript, C#) übergeben werden.

> [!TIP]
> Weitere Informationen zur programmgesteuerten Authentifizierung mit den APIs von Time Series Insights unter Verwendung des [JavaScript Client SDK](https://github.com/microsoft/tsiclient/blob/master/docs/API.md) zusammen mit Diagrammen und Grafiken finden Sie in der gehosteten [Beispielvisualisierung des Azure Time Series Insights-Client-SDK](https://tsiclientsample.azurewebsites.net/).

### <a name="http-headers"></a>HTTP-Header

Erforderliche Anforderungsheader werden nachfolgend beschrieben.

| Erforderlicher Anforderungsheader | BESCHREIBUNG |
| --- | --- |
| Authorization | Für die Authentifizierung bei Time Series Insights muss im **Autorisierungsheader** ein gültiges OAuth 2.0-Bearertoken übergeben werden. |

> [!IMPORTANT]
> Das Token muss spezifisch für die `https://api.timeseries.azure.com/`-Ressource (auch als „Zielgruppe“ des Tokens bezeichnet) ausgestellt werden.
> * Ihre [Postman](https://www.getpostman.com/)-**AuthURL** lautet darum: `https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?scope=https://api.timeseries.azure.com/.default`.
> * `https://api.timeseries.azure.com/` ist gültig, `https://api.timeseries.azure.com` jedoch nicht.

Optionale Anforderungsheader werden nachfolgend beschrieben.

| Optionaler Anforderungsheader. | BESCHREIBUNG |
| --- | --- |
| Inhaltstyp | Nur `application/json` wird unterstützt. |
| x-ms-client-request-id | Eine Clientanforderungs-ID. Der Dienst zeichnet diesen Wert auf. Ermöglicht es dem Dienst, den Vorgang dienstübergreifend nachzuverfolgen. |
| x-ms-client-session-id | Eine Clientsitzungs-ID. Der Dienst zeichnet diesen Wert auf. Ermöglicht es dem Dienst, eine Gruppe verwandter Vorgänge dienstübergreifend nachzuverfolgen. |
| x-ms-client-application-name | Der Name der Anwendung, die diese Anforderung generiert hat. Der Dienst zeichnet diesen Wert auf. |

Optionale, aber empfohlene Antwortheader werden unten beschrieben.

| Antwortheader | BESCHREIBUNG |
| --- | --- |
| Inhaltstyp | Nur `application/json` wird unterstützt. |
| x-ms-request-id | Vom Server generierte Anforderungs-ID. Kann zum Kontaktieren von Microsoft verwendet werden, um eine Anforderung zu untersuchen. |
| x-ms-property-not-found-behavior | Optionaler GA-API-Antwortheader. Mögliche Werte sind: `ThrowError` (Standard) oder `UseNull`. |

### <a name="http-parameters"></a>HTTP-Parameter

> [!TIP]
> Weitere Informationen zu erforderlichen und optionalen Abfrageinformationen finden Sie in der [Referenzdokumentation](https://docs.microsoft.com/rest/api/time-series-insights/).

Die erforderlichen URL-Abfragezeichenfolgenparameter hängen von der API-Version ab.

| Release | Mögliche API-Versionswerte |
| --- |  --- |
| Allgemeine Verfügbarkeit | `api-version=2016-12-12`|
| Vorschau | `api-version=2018-11-01-preview` |
| Vorschau | `api-version=2018-08-15-preview` |

Optionale URL-Abfragezeichenfolgen-Parameter umfassen das Festlegen eines Timeouts für die HTTP-Anforderungsausführungszeiten.

| Optionaler Abfrageparameter | BESCHREIBUNG | Version |
| --- |  --- | --- |
| `timeout=<timeout>` | Das serverseitige Timeout für die Ausführung der HTTP-Anforderung. Gilt nur für die APIs zum [Abrufen von Umgebungsereignissen](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-events-api) und [Abrufen von Umgebungsaggregaten](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-aggregates-api). Der Timeoutwert muss das ISO 8601-Format für die Dauer aufweisen (z.B. `"PT20S"`) und sollte im Bereich `1-30 s` liegen. Der Standardwert ist `30 s`. | Allgemein verfügbar |
| `storeType=<storeType>` | In Vorschauumgebungen, in denen der Warmspeicher aktiviert ist, kann die Abfrage entweder im `WarmStore` oder `ColdStore` ausgeführt werden. Dieser Parameter in der Abfrage definiert, in welchem Speicher die Abfrage ausgeführt werden soll. Wenn nicht definiert, wird die Abfrage im kalten Speicher ausgeführt. Um den warmen Speicher abzufragen, muss **storeType** auf `WarmStore` festgelegt werden. Wenn nicht definiert, wird die Abfrage im kalten Speicher ausgeführt. | Vorschau |

## <a name="next-steps"></a>Nächste Schritte

* Beispielcode, in dem die GA Time Series Insights-API aufgerufen wird, finden Sie unter [Abfragen von Daten aus der Azure Time Series Insights GA-Umgebung mit C#](./time-series-insights-query-data-csharp.md).

* Codebeispiele für die Vorschauversion der Time Series Insights-API finden Sie unter [Abfragen von Daten aus der Azure Time Series Insights Preview-Umgebung mit C#](./time-series-insights-update-query-data-csharp.md).

* API-Referenzinformationen finden Sie in der [Abfrage-API-Referenzdokumentation](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api).

* Informieren Sie sich, wie Sie [einen Dienstprinzipal erstellen](../active-directory/develop/howto-create-service-principal-portal.md).
