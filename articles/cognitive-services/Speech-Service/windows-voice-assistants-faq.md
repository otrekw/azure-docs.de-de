---
title: 'Sprachassistenten unter Windows: Häufig gestellte Fragen (FAQ)'
titleSuffix: Azure Cognitive Services
description: Häufig gestellte Fragen bei der Entwicklung von Windows-Sprach-Agents.
services: cognitive-services
author: cfogg6
manager: trrwilson
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: travisw
ms.openlocfilehash: 3cb4dd16592f3af962b32fe3005d2ad4b241d831
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "84457008"
---
# <a name="samples-and-faqs"></a>Beispiele und FAQs

## <a name="the-uwp-voice-assistant-sample"></a>Das UWP Voice Assistant-Beispiel

Das UWP Voice Assistant-Beispiel ist ein Sprachassistent unter Windows, der folgenden Zwecken dient:

- Veranschaulichen der Verwendung der Windows-ConversationalAgent-APIs
- Aufzeigen bewährter Methoden für einen Sprach-Agent unter Windows
- Bereitstellen einer anpassbaren App und wiederverwendbarer Komponenten für Ihre MVP-Agent-Anwendung
- Darstellen, wie Direct Line Speech in Verbindung mit Bot Framework oder Custom Commands zusammen mit den APIs des Windows ConversationalAgent für eine End-to-End-Benutzererfahrung des Sprach-Agents verwendet werden kann

Die Dokumentation zur Beispiel-App führt schrittweise durch den Codepfad von Sprachaktivierung und Agent-Verwaltung, von den Voraussetzungen für den Start bis zur ordnungsgemäßen Bereinigung.

> [!div class="nextstepaction"]
> [Das GitHub-Repository für das UWP-Beispiel besuchen](https://aka.ms/MVA/sample)

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="how-do-i-contact-microsoft-for-resources-like-limited-access-feature-tokens-and-keyword-model-files"></a>Wie kann ich Ressourcen wie Token für das Limited Access Feature und Schlüsselwort-Modelldateien von Microsoft erhalten?

Wenden Sie sich an winvoiceassistants@microsoft.com, um diese Ressourcen anzufordern.

### <a name="my-app-is-showing-in-a-small-window-when-i-activate-it-by-voice-how-can-i-transition-from-the-compact-view-to-a-full-application-window"></a>Meine App wird in einem kleinen Fenster angezeigt, wenn ich sie per Sprache aktiviere. Wie gelange ich von der Kompaktansicht zu einem vollständigen Anwendungsfenster?

Bei der ersten Sprachaktivierung wird Ihre Anwendung in einer Kompaktansicht gestartet. Informationen zu den verschiedenen Ansichten und dem Übergang zwischen ihnen für Sprachassistenten unter Windows finden Sie unter [Design guidance for voice activation preview](windows-voice-assistants-best-practices.md#design-guidance-for-voice-activation-preview) (Entwurfsleitfaden für die Sprachaktivierung (Vorschau)).

Verwenden Sie für den Übergang von der Kompaktansicht zur vollständigen App-Darstellung die appView-API `TryEnterViewModeAsync`:

`var appView = ApplicationView.GetForCurrentView();
 await appView.TryEnterViewModeAsync(ApplicationViewMode.Default);`

### <a name="why-are-voice-assistant-features-on-windows-only-enabled-for-uwp-applications"></a>Warum sind die Sprachassistentenfunktionen unter Windows nur für UWP-Anwendungen aktiviert?

Angesichts der Gefahren für den Datenschutz, die mit Funktionen wie der Sprachaktivierung einhergehen, sind die Features der UWP-Plattform erforderlich, um eine ausreichende Sicherheit der Sprachassistentenfunktionen unter Windows zu ermöglichen.

### <a name="the-uwp-voice-assistant-sample-uses-direct-line-speech-do-i-have-to-use-direct-line-speech-for-my-voice-assistant-on-windows"></a>Das UWP Voice Assistant-Beispiel verwendet Direct Line Speech. Muss ich für meinen Sprachassistenten unter Windows Direct Line Speech verwenden?

Die UWP-Beispielanwendung wurde mithilfe von Direct Line Speech und des Speech Services SDK entwickelt, um die Nutzung eines Dialogdiensts mit der Windows Conversational Agent-Funktionalität zu veranschaulichen. Sie können jedoch jeden lokalen oder Clouddienst für Schlüsselwortprüfung, Spracherkennung, Bot-Dialoge und Sprachsynthese verwenden.