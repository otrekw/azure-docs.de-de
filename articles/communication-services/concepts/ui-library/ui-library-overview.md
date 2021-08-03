---
title: Übersicht über die UI-Bibliothek
titleSuffix: An Azure Communication Services concept document
description: Hier finden Sie Informationen zur UI-Bibliothek von Azure Communication Services.
author: ddematheu2
manager: chrispalm
services: azure-communication-services
ms.author: dademath
ms.date: 05/11/2021
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 4176bdf61cafdf50aef71e76b832a0e669f1535c
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110468802"
---
# <a name="ui-library-overview"></a>Übersicht über die UI-Bibliothek

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

> [!NOTE]
> Eine ausführliche Dokumentation zur UI-Bibliothek finden Sie im [Storybook der UI-Bibliothek](https://azure.github.io/communication-ui-library). Dort finden Sie zusätzliche konzeptionelle Dokumentation, Schnellstarts und Beispiele.

Mit der UI-Bibliothek von Azure Communication Services ist es für Sie einfach, moderne Benutzeroberflächen mit Azure Communication Services zu entwickeln. Sie können eine Bibliothek mit für die Produktion geeigneten Benutzeroberflächenkomponenten nutzen, die Sie in Ihre Anwendungen einfügen können:

- **Zusammensetzungen**: Bei diesen Komponenten handelt es sich um fertige Lösungen, mit denen gängige Kommunikationsszenarien implementiert werden.
  Sie können Ihren Anwendungen schnell Umgebungen für Videoanrufe oder Chats hinzufügen.
  Zusammensetzungen sind Open-Source-Komponenten höherer Ordnung, die mithilfe von UI-Komponenten erstellt wurden.

- **UI-Komponenten**: Bei diesen Komponenten handelt es sich um Open-Source-Bausteine, mit denen Sie benutzerdefinierte Kommunikationsumgebungen erstellen können.
  Es werden sowohl Komponenten für Anruf- als auch für Chatfunktionen angeboten, die für die Entwicklung von Benutzeroberflächen kombiniert werden können.

Für diese Clientbibliotheken für Benutzeroberflächen werden jeweils die [Fluent-Entwurfssprache von Microsoft](https://developer.microsoft.com/fluentui/) und die zugehörigen Objekte verwendet. Fluent-UI stellt eine grundlegende Ebene für die UI-Bibliothek zur Verfügung und wird aktiv in Microsoft-Produkten verwendet.

In Verbindung mit den UI-Komponenten stellt die UI-Bibliothek eine zustandsbehaftete Clientbibliothek für Anruf- und Chatfunktionen zur Verfügung.
Dieser Client ist unabhängig von einem bestimmten Framework für Zustandsverwaltung und kann in allgemeine Zustands-Manager wie Redux oder React Context integriert werden.
Diese zustandsbehaftete Clientbibliothek kann mit den UI-Komponenten verwendet werden, um Eigenschaften und Methoden für die UI-Komponenten zum Rendern von Daten zu übergeben. Weitere Informationen finden Sie unter [Zustandsbehafteter Client: Übersicht](https://azure.github.io/communication-ui-library/?path=/story/stateful-client-what-is-stateful--page).

## <a name="installing-ui-library"></a>Installieren der UI-Bibliothek

Zustandsbehaftete Clients sind Teil des `@azure/communication-react`-Pakets. 

```bash
npm i --save @azure/communication-react
```

## <a name="composites-overview"></a>Übersicht über Zusammensetzungen

Zusammensetzungen sind Komponenten auf höherer Ebene, die aus UI-Komponenten bestehen, die mithilfe von Azure Communication Services schlüsselfertige Lösungen für allgemeine Kommunikationsszenarien bereitstellen.
Entwickler können die Zusammensetzung ganz einfach mithilfe eines Azure Communication Services-Zugriffstokens und der erforderlichen Konfiguration instanziieren, die für Anruf- oder Chatfunktionen erfolgt.

| Composite    | Anwendungsfälle  | 
| ------------ | ---------- |
| [CallComposite](https://azure.github.io/communication-ui-library/?path=/docs/composites-callcomposite--basic-example) | Anrufbenutzeroberfläche, mit der Benutzer einen Anruf starten oder ihm beitreten können. Innerhalb der Benutzeroberfläche können Benutzer ihre Geräte konfigurieren, am Anruf mit Video teilnehmen und andere Teilnehmer sehen, einschließlich der Teilnehmer mit aktivierten Videofunktionen. Für Teams Interop ist eine Wartebereichfunktion enthalten, mit der Benutzer auf ihre Zulassung warten können. |
| [ChatComposite](https://azure.github.io/communication-ui-library/?path=/docs/composites-chatcomposite--basic-example)    | Chatbenutzeroberfläche, mit der Benutzer Nachrichten senden und empfangen können. Threadereignisse wie Eingaben, Lesevorgänge und Teilnehmer, die dem Chat beitreten oder ihn verlassen, werden dem Benutzer als Teil des Chatthreads angezeigt.                                                                                                                          |

## <a name="ui-component-overview"></a>Übersicht über die UI-Komponenten

Reine UI-Komponenten, die von Entwicklern verwendet werden können, um Kommunikationsbenutzeroberflächen zu erstellen, vom Zusammenfügen von Videokacheln in einem Raster bis hin zum Präsentieren von Remoteteilnehmern und Organisieren von Komponenten, damit diese Ihren Anwendungsspezifikationen entsprechen.
UI-Komponenten unterstützen Anpassung, um den Komponenten das richtige Erscheinungsbild und Aussehen zu verleihen, damit sie einem Anwendungsbranding bzw. -stil entsprechen.

| Bereich    | Komponente    | Beschreibung       |
| ------- | ------------ | ----------------- |
| Aufrufen | [Rasterlayout](https://azure.github.io/communication-ui-library/?path=/story/ui-components-gridlayout--grid-layout-component)                | Rasterkomponente zum Organisieren von Videokacheln in einem NxN-Raster                                            |
|         | [Videokachel](https://azure.github.io/communication-ui-library/?path=/story/ui-components-videotile--video-tile-component)                   | Komponente, die den Videodatenstrom anzeigt, wenn verfügbar, und eine statische Standardkomponente, wenn dies nicht möglich ist.        |
|         | [Steuerleiste](https://azure.github.io/communication-ui-library/?path=/story/ui-components-controlbar--control-bar-component)                | Container zum Organisieren von DefaultButtons, um bestimmte Anrufaktionen wie Stummschaltung oder Bildschirmfreigabe zu ermöglichen. |
|         | [VideoGallery](https://azure.github.io/communication-ui-library/?path=/story/ui-components-video-gallery--video-gallery)                                           | Schlüsselfertige Videokatalogkomponente, die sich dynamisch ändert, wenn Teilnehmer hinzugefügt werden.               |
| Chat    | [Nachrichtenthread](https://azure.github.io/communication-ui-library/?path=/story/ui-components-messagethread--message-thread-component)       | Container zum Rendern von Chatnachrichten, Systemnachrichten und benutzerdefinierten Nachrichten.                          |
|         | [Sendefeld](https://azure.github.io/communication-ui-library/?path=/story/ui-components-sendbox--send-box-component)                         | Texteingabekomponente mit eigenständigen Sendeschaltfläche.                                                   |
|         | [Nachrichtenstatusanzeige](https://azure.github.io/communication-ui-library/?path=/story/ui-components-message-status-indicator--message-status-indicator)        | Lesebestätigungskomponente mit mehreren Zuständen zum Anzeigen des Status der gesendeten Nachricht.                                   |
|         | [Eingabeindikator](https://azure.github.io/communication-ui-library/?path=/story/ui-components-typingindicator--typing-indicator-component) | Textkomponente zum Rendern der Teilnehmer, die aktiv Eingaben in einem Thread vornehmen.                      |
| Allgemein  | [Teilnehmerelement](https://azure.github.io/communication-ui-library/?path=/story/ui-components-participantitem--participant-item-component) | Allgemeine Komponente zum Rendern eines Anruf- oder Chatteilnehmers, einschließlich Avatar und Anzeigename.            |
|         | [Teilnehmerliste](https://azure.github.io/communication-ui-library/?path=/story/ui-components-participant-list--participant-list)                                 | Allgemeine Komponente zum Rendern einer Anrufer- oder Chatteilnehmerliste, einschließlich Avatar und Anzeigename.       |


## <a name="what-ui-artifact-is-best-for-my-project"></a>Welches UI-Artefakt ist für mein Projekt am besten geeignet?

Wenn Sie sich mit den folgenden Anforderungen vertraut machen, können Sie die richtige Clientbibliothek auswählen:

- **Welchen Anpassungsgrad streben Sie an?** Die zentralen Azure Communication Services-Clientbibliotheken weisen keine Benutzeroberfläche auf und sind so konzipiert, dass Sie die Benutzeroberfläche nach Ihren Vorstellungen erstellen können. Mit den Komponenten der UI-Bibliothek werden UI-Ressourcen auf Kosten verringerter Anpassungsmöglichkeiten bereitgestellt.
- **Welche Plattformen sollen verwendet werden?** Verschiedene Plattformen verfügen über unterschiedliche Funktionen.

Ausführliche Informationen zur Verfügbarkeit von Features in der [UI-Bibliothek finden Sie hier](https://azure.github.io/communication-ui-library/?path=/story/use-cases--page). Eine Zusammenfassung der wichtigsten Nachteile finden Sie unten.

| Clientbibliothek/SDK  | Implementierungskomplexität | Möglichkeit zur Anpassung | Aufrufen | Chat | [Teams-Interoperabilität](../teams-interop.md) |
| --------------------- | ------------------------- | --------------------- | ------- | ---- | ----------------------------------------------------------------------------------------------------- |
| Zusammengesetzte Komponenten  | Niedrig                       | Niedrig                   | ✔       | ✔    | ✔                                                                                                     |
| Basiskomponenten       | Medium                    | Medium                | ✔       | ✔    | ✔                                                                                                     |
| Zentrale Clientbibliotheken | High                      | High                  | ✔       | ✔    | ✔                                                                                                     |

> [!div class="nextstepaction"]
> [Storybook der UI-Bibliothek besuchen](https://azure.github.io/communication-ui-library)
