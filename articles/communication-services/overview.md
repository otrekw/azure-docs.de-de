---
title: Was ist Azure Communication Services?
description: Erfahren Sie, wie Azure Communication Services Sie bei der Entwicklung umfassender Benutzererfahrungen mit Echtzeitkommunikation unterstützt.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 8c2559315e3bfffc41c138be6826adae95dd7b07
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107588104"
---
# <a name="what-is-azure-communication-services"></a>Was ist Azure Communication Services?

Mit Azure Communication Services können Sie Ihren Anwendungen mühelos Funktionen für die Sprach-, Video- und Telefonkommunikation in Echtzeit hinzufügen. Communication Services-SDKs ermöglichen zudem das Hinzufügen von SMS-Funktionen zu Ihren Kommunikationslösungen. Azure Communication Services ist identitätsunabhängig, und Sie haben uneingeschränkte Kontrolle über die Identifizierung und Authentifizierung von Endbenutzern. Sie können Menschen mit der Kommunikationsdatenebene oder mit entsprechenden Diensten (Bots) verbinden.

Dies können diese Arten von Anwendungen sein:

- **B2C (Business-to-Consumer):** Die Mitarbeiter und Dienste eines Unternehmens können mit Kunden über Sprach-, Video- und Rich-Text-Chats in einem benutzerdefinierten Browser oder einer mobilen Anwendung interagieren. Eine Organisation kann SMS-Nachrichten senden und empfangen oder ein IVR-System (Interactive Voice Response, interaktive Sprachantwort) mit einer über Azure bezogenen Telefonnummer betreiben. Dank [Microsoft Teams-Integration](./quickstarts/voice-video-calling/get-started-teams-interop.md) können Kunden an Teams-Besprechungen teilnehmen, die von Mitarbeitern gehostet werden – perfekt für Remoteszenarien im Gesundheitsbereich, im Bankwesen und im Produktsupport, wo Mitarbeiter ggf. bereits mit Teams vertraut sind.
- **C2C (Consumer-to-Consumer):** Erstellen Sie ansprechende soziale Bereiche für die Interaktion zwischen Verbrauchern mit Sprach-, Video- und Rich-Text-Chats. Auf der Grundlage von Azure Communication Services SDKs kann jede beliebige Art von Benutzeroberfläche erstellt werden. Für einen schnellen Einstieg stehen jedoch vollständige Anwendungsbeispiele und Benutzeroberflächenressourcen zur Verfügung.

Weitere Informationen finden Sie in unserem [Microsoft Mechanics-Video](https://www.youtube.com/watch?v=apBX7ASurgM) sowie in den unten verlinkten Ressourcen.

## <a name="common-scenarios"></a>Häufige Szenarien

<br>

| Resource                               |Beschreibung                           |
|---                                    |---                                   |
|**[Erstellen einer Communication Services-Ressource](./quickstarts/create-communication-resource.md)**|Beginnen Sie mit der Verwendung von Azure Communication Services, indem Sie das Azure-Portal oder das Communication Services SDK verwenden, um Ihre erste Communication Services-Ressource bereitzustellen. Sobald Sie über die Communication Services-Verbindungszeichenfolge verfügen, können Sie Ihre ersten Benutzerzugriffstoken bereitstellen.|
|**[Beschaffen einer Telefonnummer](./quickstarts/telephony-sms/get-phone-number.md)**|Verwenden Sie Azure Communication Services zum Bereitstellen und Freigeben von Telefonnummern. Diese Telefonnummern können zum Initiieren oder Entgegennehmen von Anrufen sowie zum Erstellen von SMS-Lösungen verwendet werden.|
|**[Versenden einer SMS von Ihrer App](./quickstarts/telephony-sms/send.md)**|Das Azure Communication Services SMS SDK dient zum Senden und Empfangen von SMS-Nachrichten von Dienstanwendungen.|

Nach dem Erstellen einer Communication Services-Ressource können Sie mit dem Erstellen von Clientszenarien beginnen, etwa für Sprach- und Videoanrufe oder Textchats:

| Resource                               |BESCHREIBUNG                           |
|---                                    |---                                   |
|**[Erstellen Ihrer ersten Benutzerzugriffstoken](./quickstarts/access-tokens.md)**|Benutzerzugriffstoken werden verwendet, um Clients gegenüber Ihrer Azure Communication Services-Ressource zu authentifizieren. Diese Token werden mithilfe des Communication Services SDK bereitgestellt und erneut ausgestellt.|
|**[Erste Schritte mit Sprach- und Videoanrufen](./quickstarts/voice-video-calling/getting-started-with-calling.md)**| Mit Azure Communication Services können Sie Ihren browserbasierten oder nativen Apps unter Verwendung des Calling SDK Sprach- und Videoanrufe hinzufügen. |
|**[Beitreten mit einer Telefonie-App zu einer Teams-Besprechung](./quickstarts/voice-video-calling/get-started-teams-interop.md)**|Azure Communication Services kann dazu verwendet werden, benutzerdefinierte Besprechungsumgebungen zu erstellen, die mit Microsoft Teams interagieren. Benutzer Ihrer Communication Services-Lösung(en) können mit den Teams-Teilnehmern über Sprache, Video, Chat und Bildschirmfreigabe interagieren.|
|**[Erste Schritte mit dem Chat](./quickstarts/chat/get-started.md)**|Mit dem Azure Communication Services Chat SDK können Sie Ihre Anwendungen mit umfangreichen Funktionen für Echtzeittextchats ausstatten.|

## <a name="samples"></a>Beispiele

In den folgenden End-to-End-Beispielen wird die Verwendung von Azure Communication Services veranschaulicht. Verwenden Sie diese Beispiele zum Bootstrapping Ihrer Communication Services-Lösungen.
<br>

| Name des Beispiels                               | Beschreibung                           |
|---                                    |---                                   |
|**[Hero-Beispiel für Gruppenanrufe](./samples/calling-hero-sample.md)**| Laden Sie ein Anwendungsbeispiel herunter, das für Gruppenanrufe über Browser sowie über iOS- und Android-Geräte entworfen wurde. |
|**[Hero-Beispiel für Gruppenchats](./samples/chat-hero-sample.md)**| Laden Sie ein Anwendungsbeispiel herunter, das für browserbasierte Gruppentextchats entworfen wurde. |


## <a name="platforms-and-sdk-libraries"></a>Plattformen und SDK-Bibliotheken

Die folgenden Ressourcen enthalten weitere Informationen zu den Azure Communication Services SDKs. Für die meisten Funktionen sind REST-APIs verfügbar, falls Sie Ihre eigenen Clients erstellen oder anderweitig über das Internet auf den Dienst zugreifen möchten.

| Resource                               | BESCHREIBUNG                           |
|---                                    |---                                   |
|**[SDKs und REST-APIs](./concepts/sdk-options.md)**|Azure Communication Services-Funktionen sind vom Prinzip her in sechs Bereiche unterteilt, die durch jeweils ein SDK dargestellt sind. Basierend auf Ihren Anforderungen an die Echtzeitkommunikation können Sie entscheiden, welche SDK-Bibliotheken verwendet werden sollen.|
|**[Übersicht über das Calling SDK](./concepts/voice-video-calling/calling-sdk-features.md)**|Lesen Sie die Übersicht über das Calling SDK von Communication Services.|
|**[Übersicht über das Chat SDK](./concepts/chat/sdk-features.md)**|Lesen Sie die Übersicht über das Communication Services Chat SDK.|
|**[Übersicht über das SMS SDK](./concepts/telephony-sms/sdk-features.md)**|Lesen Sie die Übersicht über das Communication Services SMS SDK.|

## <a name="other-microsoft-communication-services"></a>Andere Microsoft-Kommunikationsdienste

Es gibt noch zwei andere Microsoft-Kommunikationsprodukte, die Sie ggf. verwenden können. Diese sind jedoch aktuell nicht direkt mit Communication Services interoperabel:

 - [Microsoft Graph Cloud Communication-APIs](/graph/cloud-communications-concept-overview) ermöglichen es Organisationen, Kommunikationsfunktionen zu erstellen, die mit Azure Active Directory-Benutzern mit Microsoft 365-Lizenzen verknüpft sind. Dies ist ideal für Anwendungen, die an Azure Active Directory gebunden sind, oder wenn Sie Produktivitätsumgebungen in Microsoft Teams erweitern möchten. Es gibt auch APIs zum Erstellen von Anwendungen und Anpassungen innerhalb der [Teams-Funktionen.](/microsoftteams/platform/?preserve-view=true&view=msteams-client-js-latest)

 - [Azure PlayFab Party](/gaming/playfab/features/multiplayer/networking/) vereinfacht das Hinzufügen von Chat- und Datenkommunikation mit geringer Latenz zu Spielen. Spielechats und Netzwerksysteme können zwar mit Communication Services unterstützt werden, PlayFab ist jedoch eine maßgeschneiderte Option und auf Xbox kostenlos.


## <a name="next-steps"></a>Nächste Schritte

 - [Erstellen einer Communication Services-Ressource](./quickstarts/create-communication-resource.md)
