---
title: Dokumentation zum Speech-Geräte-SDK
titleSuffix: Azure Cognitive Services
description: Die Versionshinweise enthalten ein Protokoll über Updates, Verbesserungen, Fehlerbehebungen und Änderungen am Speech-Geräte-SDK. Dieser Artikel wird mit jeder Version des Speech-Geräte-SDK aktualisiert.
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/13/2019
ms.author: wellsi
ms.openlocfilehash: 3a74f3eb9aece3535f5505d69833a074c2dd0ed2
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74091195"
---
# <a name="release-notes-speech-devices-sdk"></a>Versionshinweise: Speech-Geräte-SDK

In den folgenden Abschnitten sind Änderungen in den neuesten Versionen aufgeführt.

## <a name="speech-devices-sdk-170"></a>Speech Devices SDK 1.7.0:

*   Linux ARM wird jetzt unterstützt.
*   Anfängliche Binärdateien für Roobov2 werden bereitgestellt (Linux ARM64).
*   Windows-Benutzer können mit „AudioConfig.fromDefaultMicrophoneInput()“ oder „AudioConfig.fromMicrophoneInput(deviceName)“ das zu verwendende Mikrofon angeben.
*   Die Größe der Bibliothek wurde optimiert.
*   Unterstützung für die Erkennung mit mehreren Durchläufen unter Verwendung desselben Sprach-/Absichterkennungsobjekts wurde hinzugefügt.
*   Es wurde ein vereinzelt aufgetretenes Problem behoben, das beim Beenden der Erkennung auftreten konnte.
*   Beispiel-Apps enthalten jetzt eine „participants.properties“-Beispieldatei, um das Format der Datei zu veranschaulichen.
*   Die [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 1.7.0 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-160"></a>Speech Devices SDK 1.6.0:

- Unterstützung von [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/) unter Windows und Linux mit gemeinsamer [Beispielanwendung](https://aka.ms/sdsdk-download)
- Die [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 1.6.0 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-151"></a>Speech Devices SDK 1.5.1:

- [Unterhaltungstranskription](conversation-transcription-service.md) wurde in die Beispiel-App aufgenommen.
- Die [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 1.5.1 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-150-2019-may-release"></a>Speech Devices SDK 1.5.0: Release von Mai 2019

- Das Speech Devices SDK befindet sich nicht mehr in der geschlossenen Vorschau und ist nun allgemein verfügbar.
- Die [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 1.5.0 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](https://aka.ms/csspeech/whatsnew).
- Eine neue Schlüsselworttechnologie bringt erhebliche Qualitätsverbesserungen mit sich. Informationen finden Sie unter „Wichtige Änderungen“.
- Neue Audioverarbeitungspipeline für Fernfelderkennung

**Wichtige Änderungen**

- Aufgrund der neuen Schlüsselworttechnologie müssen alle Schlüsselwörter in unserem optimierten Schlüsselwortportal neu erstellt werden. Deinstallieren Sie die alte App, um alte Schlüsselwörter vollständig vom Gerät zu entfernen.
  - Deinstallieren Sie „com.microsoft.coginitiveservices.speech.samples.sdsdkstarterapp“ mit adb.

## <a name="speech-devices-sdk-140-2019-apr-release"></a>Speech Devices SDK 1.4.0: Release von April 2019

- Die [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 1.4.0 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-131-2019-mar-release"></a>Speech Devices SDK 1.3.1: Release März 2019

- Die [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 1.3.1 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](https://aka.ms/csspeech/whatsnew).
- Informationen zur aktualisierten Schlüsselwortverarbeitung finden Sie unter „Wichtige Änderungen“.
- Beispielanwendung fügt die Auswahl der Sprache für die Spracherkennung und Übersetzung hinzu.

**Wichtige Änderungen**

- Das [Installieren eines Schlüsselworts](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws) wurde vereinfacht und ist jetzt Teil der App. Es ist keine separate Installation auf dem Gerät erforderlich.
- Die Schlüsselworterkennung wurde geändert, und es werden zwei Ereignisse unterstützt.
  - RecognizingKeyword: Gibt an, dass das Ergebnis der Spracherkennung (nicht überprüften) Schlüsselworttext enthält.
  - RecognizedKeyword: Gibt an, dass die Schlüsselworterkennung abgeschlossen und das angegebene Schlüsselwort erkannt wurde.

## <a name="speech-devices-sdk-110-2018-nov-release"></a>Speech Devices SDK 1.1.0: Version 2018-Nov

- Die [Speech-SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 1.1.0 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](https://aka.ms/csspeech/whatsnew).
- Die Spracherkennungsgenauigkeit im Fernfeld wird mit unserem erweiterten Audioverarbeitungsalgorithmus verbessert.
- Der Beispielanwendung wurde Unterstützung für die Erkennung von Chinesisch hinzugefügt.

## <a name="speech-devices-sdk-101-2018-oct-release"></a>Speech Devices SDK 1.0.1: Version 2018-Oct

- [Speech-SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 1.0.1 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](https://aka.ms/csspeech/whatsnew).
- Die Spracherkennungsgenauigkeit wird mit unserem verbesserten Audioverarbeitungsalgorithmus verbessert.
- Ein Fehler bei der kontinuierlichen Erkennung von Audiositzungen wurde behoben.

**Wichtige Änderungen**

- Mit diesem Release werden einige Breaking Changes eingeführt. Ausführliche Informationen zu den APIs finden Sie auf [dieser Seite](https://aka.ms/csspeech/breakingchanges_1_0_0).
- Die KWS-Modelldateien sind nicht mit dem Speech-Geräte-SDK 1.0.1 kompatibel. Vorhandene Schlüsselwortdateien werden nach dem Schreiben der neuen Schlüsselwortdateien auf dem Gerät gelöscht.

## <a name="speech-devices-sdk-050-2018-aug-release"></a>Speech Devices SDK 0.5.0: Version 2018-Aug

- Die Genauigkeit der Spracherkennung wurde verbessert, indem ein Fehler im Code für die Audiodatenverarbeitung behoben wurde.
- [Speech-SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 0.5.0 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](releasenotes.md#cognitive-services-speech-sdk-050-2018-july-release).

## <a name="speech-devices-sdk-0212733-2018-may-release"></a>Speech Devices SDK 0.2.12733: Release von Mai 2018

Die erste öffentliche Vorschauversion des Cognitive Services Speech-Geräte-SDK.
