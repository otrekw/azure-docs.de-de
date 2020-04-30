---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: f0baac79bfbfc06b24d692caef87a20013c4654c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81422191"
---
Erstellen Sie `SPXPullAudioInputStream` oder `SPXPushAudioInputStream` zum Streamen von komprimierten Audioformaten an den Speech-Dienst.

Der folgende Codeausschnitt zeigt, wie Sie eine `SPXAudioConfiguration` aus einer Instanz eines `SPXPushAudioInputStream` erstellen, indem Sie MP3 als Komprimierungsformat des Datenstroms angeben.

[!code-objectivec[Set up the input stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=67-76&highlight=2-11)]

Der nächste Codeausschnitt zeigt, wie komprimierte Audiodaten aus einer Datei gelesen und in das `SPXPushAudioInputStream` eingefügt werden können.

[!code-objectivec[Push compressed audio data into the stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=106-150&highlight=19-44)]
