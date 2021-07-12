---
title: Interoperabilität von Teams-Besprechungen
titleSuffix: An Azure Communication Services concept document
description: Beitreten zu Teams-Besprechungen
author: chpalm
manager: chpalm
services: azure-communication-services
ms.author: chpalm
ms.date: 06/30/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 142b4635bed6361987c21173245bb4d4e7557f90
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113108097"
---
# <a name="teams-interoperability"></a>Teams-Interoperabilität

[!INCLUDE [Public Preview](../includes/public-preview-include-document.md)]

> [!IMPORTANT]
> Um die [Interoperabilität von Teams-Mandanten](../concepts/teams-interop.md) zu aktivieren/deaktivieren, füllen Sie [dieses Formular](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR21ouQM6BHtHiripswZoZsdURDQ5SUNQTElKR0VZU0VUU1hMOTBBMVhESS4u) aus.

> [!NOTE]
> Die Interoperabilität zwischen Azure Communication Services und Microsoft Teams ermöglicht Ihren Anwendungen und Benutzern, an Teams-Anrufen, -Besprechungen und -Chats teilzunehmen. Sie sind dafür verantwortlich, dass die Benutzer Ihrer Anwendung benachrichtigt werden, wenn die Aufzeichnung oder Transkription in einem Teams-Anruf oder einer Teams-Besprechung aktiviert ist. Microsoft gibt Ihnen über die Azure Communication Services-API einen Hinweis, dass die Aufzeichnung oder Transkription begonnen hat. Sie müssen dies Ihren Benutzern auf der Benutzeroberfläche Ihrer Anwendung in Echtzeit mitteilen. Sie erklären sich damit einverstanden, Microsoft für alle Kosten und Schäden freizustellen, die aufgrund der Nichteinhaltung dieser Verpflichtung durch Sie entstehen.

> [!NOTE]
> Die VoIP- und Chat-Nutzung wird nur dann über Ihre Azure-Ressource abgerechnet, wenn Sie Azure-APIs und -SDKs verwenden. Teams-Clients, die mit Azure Communication Services-Anwendungen interagieren, sind kostenlos.

Azure Communication Services kann dazu verwendet werden, benutzerdefinierte Besprechungsumgebungen zu erstellen, die mit Microsoft Teams interagieren. Benutzer Ihrer Communication Services-Lösung(en) können mit den Teams-Teilnehmern über Sprache, Video, Chat und Bildschirmfreigabe interagieren.

Die Teams-Interoperabilität ermöglicht es Ihnen, benutzerdefinierte Anwendungen zu erstellen, die Verbindungen zwischen Benutzern und Teams-Besprechungen herstellen. Benutzer Ihrer benutzerdefinierten Anwendungen müssen keine Azure Active Directory-Identitäten oder Teams-Lizenzen besitzen, um diese Funktion nutzen zu können. Dies ist ideal, um Mitarbeiter (die möglicherweise mit Teams vertraut sind) und externe Benutzer (mithilfe einer benutzerdefinierten Anwendungsumgebung) in einer reibungslosen Besprechungsumgebung zusammenzubringen. Beispiel:

1. Mitarbeiter verwenden Teams zur Planung einer Besprechung 
1. Besprechungsdetails werden über Ihre benutzerdefinierte Anwendung für externe Benutzer freigegeben.
   * **Verwenden der Graph-API:** Ihre benutzerdefinierte Communication Services-Anwendung verwendet die Microsoft Graph-APIs für den Zugriff auf freizugebende Besprechungsdetails. 
   * **Verwenden anderer Optionen:** Beispielsweise kann der Besprechungslink aus Ihrem Kalender in Microsoft Teams kopiert werden.
1. Externe Benutzer verwenden Ihre benutzerdefinierte Anwendung, um an der Teams-Besprechung teilzunehmen (über die Communication Services-SDKs Calling und Chat)

Die allgemeine Architektur für diesen Anwendungsfall sieht folgendermaßen aus: 

![Architektur für die Teams-Interoperabilität](./media/call-flows/teams-interop.png)

Communication Services-Benutzer können an geplanten Teams-Besprechungen teilnehmen, solange in den [Besprechungseinstellungen](/microsoftteams/meeting-settings-in-teams) die anonyme Teilnahme aktiviert ist. 

Obwohl bestimmte Features für Teams-Besprechungen, z. B. Heben der Hand, Together-Modus und Breakout Rooms nur für Benutzer von Teams verfügbar sind, hat Ihre benutzerdefinierte Anwendung Zugriff auf die wichtigsten Funktionen für Audio, Video, Chat und Bildschirmfreigabe der Besprechung. Der Besprechungschat ist für die Benutzer Ihrer benutzerdefinierten Anwendung zugänglich, während sich diese im Anruf befinden. Sie können weder vor dem Beitritt noch nach dem Verlassen des Anrufs Nachrichten senden oder empfangen. Wenn die Besprechung für einen Kanal geplant ist, können die Benutzer von Communication Services dem Chat nicht beitreten und Nachrichten weder senden noch empfangen.

Wenn ein Communication Services-Benutzer an der Teams-Besprechung teilnimmt, wird der Anzeigename, der über das Calling SDK bereitgestellt wird, den Teams-Benutzern angezeigt. Der Communication Services-Benutzer wird ansonsten wie ein anonymer Benutzer in Teams behandelt.  Ihre benutzerdefinierte Anwendung sollte die Benutzerauthentifizierung und andere Sicherheitsmaßnahmen zum Schutz von Teams-Besprechungen berücksichtigen. Seien Sie sich der Auswirkungen auf die Sicherheit bewusst, wenn Sie anonymen Benutzern die Teilnahme an Besprechungen ermöglichen, und verwenden Sie den [Teams-Sicherheitsleitfaden](/microsoftteams/teams-security-guide#addressing-threats-to-teams-meetings), um die für anonyme Benutzer verfügbaren Funktionen zu konfigurieren.

## <a name="teams-in-government-clouds-gcc"></a>Teams in Government-Clouds (GCC)
Azure Communication Services-Interoperabilität ist derzeit nicht mit Teams-Bereitstellungen mit [Microsoft 365-Government-Clouds (GCC)](/MicrosoftTeams/plan-for-government-gcc) kompatibel. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Beitreten mit einer Telefonie-App zu einer Teams-Besprechung](../quickstarts/voice-video-calling/get-started-teams-interop.md)

Weitere Informationen finden Sie in den folgenden Artikeln:

- Informationen zur [UI-Bibliothek](./ui-library/ui-library-overview.md)
- Erfahren Sie mehr über [die Möglichkeiten der UI-Bibliothek](./ui-library/ui-library-use-cases.md)
