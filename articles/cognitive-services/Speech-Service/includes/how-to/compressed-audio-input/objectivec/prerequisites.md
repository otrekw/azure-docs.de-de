---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: 28f6ef7248fada8286bbe90c868cd9f947f0435d
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80409628"
---
Die Verarbeitung komprimierter Audiodaten wird mit [GStreamer](https://gstreamer.freedesktop.org) implementiert. Aus Lizenzierungsgründen werden die GStreamer-Binärdateien nicht kompiliert und mit dem Speech SDK verknüpft. Stattdessen muss eine Wrapperbibliothek, die diese Funktionen enthält, erstellt und mit den Apps, die das SDK verwenden, ausgeliefert werden.

Um diese Wrapperbibliothek zu erstellen, laden Sie zunächst das [GStreamer SDK](https://gstreamer.freedesktop.org/data/pkg/ios/1.16.0/gstreamer-1.0-devel-1.16.0-ios-universal.pkg) herunter und installieren Sie es. Laden Sie dann das **Xcode**-Projekt für die [Wrapperbibliothek](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/objective-c/ios/compressed-streams/GStreamerWrapper) herunter.

Öffnen Sie das Projekt in **Xcode**, und erstellen Sie es für das Ziel **Allgemeines iOS-Gerät**. Die Erstellung für ein bestimmtes Ziel wird *nicht* funktionieren.

Der Buildschritt generiert ein dynamisches Frameworkpaket mit einer dynamischen Bibliothek für alle erforderlichen Architekturen mit dem Namen `GStreamerWrapper.framework`.

Dieses Framework muss in allen Anwendungen enthalten sein, die komprimierte Audiostreams mit dem SDK des Speech-Diensts verwenden.

Wenden Sie dazu die folgenden Einstellungen in Ihrem **Xcode**-Projekt an:

1. Kopieren Sie sowohl das soeben erstellte `GStreamerWrapper.framework` als auch das Framework des Cognitive Services Speech SDK, das Sie [hier](https://aka.ms/csspeech/iosbinary) herunterladen können, in das Verzeichnis mit Ihrem Beispielprojekt.
1. Passen Sie die Pfade an die Frameworks in den *Projekteinstellungen* an.
   1. Fügen Sie auf der Registerkarte **Allgemein** unter der Überschrift **Eingebettete Binärdateien** die SDK-Bibliothek als Framework hinzu: **Eingebettete Binärdateien hinzufügen** > **Weitere hinzufügen...** , und navigieren Sie zu dem von Ihnen gewählten Verzeichnis, und wählen Sie dann beide Frameworks aus.
   1. Navigieren Sie zur Registerkarte **Buildeinstellungen**, und aktivieren Sie **Alle** Einstellungen.
1. Fügen Sie das Verzeichnis `$(SRCROOT)/..` den _Frameworksuchpfaden_ unter der Überschrift **Suchpfade** hinzu.
