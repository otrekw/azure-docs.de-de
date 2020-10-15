---
title: 'Speech-Dienst: Audio Content Creation'
titleSuffix: Azure Cognitive Services
description: Audio Content Creation ist ein Onlinetool, mit dem Sie die Sprachsynthese von Microsoft an Ihre Apps und Produkte anpassen und optimieren können.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: trbye
ms.openlocfilehash: 72fecbdc173a6174e54a28e48f983965f397ba6a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86224586"
---
# <a name="improve-synthesis-with-the-audio-content-creation-tool"></a>Verbessern der Synthese mit dem Audio Content Creation-Tool

[Audio Content Creation](https://aka.ms/audiocontentcreation) ist ein Onlinetool, mit dem Sie die Sprachsynthese von Microsoft an Ihre Apps und Produkte anpassen und optimieren können. Sie können mit diesem Tool öffentliche und benutzerdefinierte Stimmen optimieren, um noch natürlichere Sprachausdrücke zu erzielen und Ihre Ausgabe in der Cloud zu verwalten.

Das Tool Audio Content Creation basiert auf der [Speech Synthesis Markup Language (SSML)](speech-synthesis-markup.md). Um die Anpassung und Optimierung zu vereinfachen, ermöglicht Audio Content Creation das visuelle Untersuchen von Sprachsyntheseausgaben in Echtzeit.

Informationen dazu finden Sie im [Videotutorial](https://www.youtube.com/watch?v=O1wIJ7mts_w) zu Audio Content Creation.

## <a name="how-does-it-work"></a>Wie funktioniert dies?

Das folgende Diagramm zeigt die erforderlichen Schritte zum Optimieren der Sprachsyntheseausgaben. Verwenden Sie die unten angegebenen Links, um mehr über die einzelnen Schritte zu erfahren.

![Ein Diagramm der erforderlichen Schritte zum Optimieren der Sprachsyntheseausgaben.](media/audio-content-creation/audio-content-creation-diagram.jpg)

1. [Richten Sie Ihr Azure-Konto und die Speech-Ressource ein](#set-up-your-azure-account-and-speech-resource), um loszulegen.
2. [Erstellen Sie eine Datei zur Audiooptimierung](#create-an-audio-tuning-file) als Nur-Text- oder SSML-Skript.
3. Wählen Sie die Stimme und die Sprache für Ihren Skriptinhalt aus. Audio Content Creation umfasst alle [Stimmen der Microsoft-Sprachsynthese](language-support.md#text-to-speech). Sie können eine Standardstimme, eine neuronale Stimme oder Ihre eigene benutzerdefinierte Stimme verwenden.
   >[!NOTE]
   > Der Zugriff auf benutzerdefinierte neuronale Stimmen ist nur eingeschränkt möglich. Hiermit können Sie High-Definition-Stimmen erstellen, die wie natürliche Stimmen klingen. Weitere Einzelheiten finden Sie unter [Zulassung](https://aka.ms/ignite2019/speech/ethics).

4. Überprüfen Sie die Standardsyntheseausgabe. Verbessern Sie dann die Ausgabe, indem Sie Aussprache, Pausen, Tonhöhe, Geschwindigkeit, Intonation, Sprachstil usw. anpassen. Eine umfassende Liste der Optionen finden Sie unter [Markupsprache für Sprachsynthese](speech-synthesis-markup.md). In [diesem Video](https://youtu.be/mUvf2NbfuYU) wird gezeigt, wie Sie die Sprachausgabe mit Audio Content Creation optimieren. 
5. Speichern und [exportieren Sie die optimierten Audiodaten](#export-tuned-audio). Nachdem Sie die Optimierungsspur im System gespeichert haben, können Sie die Ausgabe weiter bearbeiten. Wenn Sie mit der Ausgabe zufrieden sind, können Sie mit der Exportfunktion eine Aufgabe zur Audioerstellung erstellen. Sie können den Status der Exportaufgabe beobachten und die Ausgabe für die Verwendung in Ihren Apps und Produkten herunterladen.

## <a name="set-up-your-azure-account-and-speech-resource"></a>Einrichten Ihres Azure-Kontos und der Speech-Ressource

1. Sie benötigen ein Azure-Konto, um Audio Content Creation verwenden zu können. Ein Azure-Konto kann mithilfe Ihres Microsoft-Kontos erstellt werden. Befolgen Sie [diese Anweisungen](get-started.md#new-resource) zum Einrichten eines Azure-Kontos. 
2. [Erstellen Sie eine Speech-Ressource](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-the-resource) für Ihr Azure-Konto. Vergewissern Sie sich, dass Ihr Tarif **S0** ist. Wenn Sie eine der neuronalen Stimmen verwenden, müssen Sie die Ressource in einer [unterstützten Region](regions.md#standard-and-neural-voices) erstellen.
2. Wenn Sie das Azure-Konto und die Sprachressource erstellt haben, können Sie die Sprachdienste verwenden und auf [Audio Content Creation](https://aka.ms/audiocontentcreation) zugreifen.
3. Wählen Sie die Speech-Ressource aus, die Sie bearbeiten müssen. Sie können hier auch eine neue Speech-Ressource erstellen. 
4. Sie können Ihre Speech-Ressource jederzeit ändern, indem Sie im oberen Navigationsbereich die Option **Einstellungen** auswählen.

## <a name="create-an-audio-tuning-file"></a>Erstellen einer Audiooptimierungsdatei

Es gibt zwei Möglichkeiten, Ihre Inhalte in das Audio Content Creation-Tool zu übernehmen.

**Option 1:**

1. Klicken Sie auf **Neue Datei**, um eine neue Audiooptimierungsdatei zu erstellen.
2. Geben oder fügen Sie Ihren Inhalt in das Bearbeitungsfenster ein. Eine Datei kann jeweils bis zu 20.000 Zeichen enthalten. Ist Ihr Skript länger als 20.000 Zeichen, können Sie Option 2 verwenden, um den Inhalt automatisch in mehrere Dateien zu unterteilen. 
3. Vergessen Sie nicht zu speichern.

**Option 2:**

1. Klicken Sie auf **Hochladen**, um Textdateien zu importieren. Sowohl Nur-Text als auch SSML werden unterstützt.
2. Wenn die Skriptdatei mehr als 20.000 Zeichen umfasst, teilen Sie die Datei nach Absätzen, Zeichen oder regulären Ausdrücken. 
3. Stellen Sie beim Hochladen Ihrer Textdateien sicher, dass die Datei die folgenden Anforderungen erfüllt:

   | Eigenschaft | Wert/Hinweise |
   |----------|---------------|
   | Dateiformat | Nur-Text (.txt)<br/> SSML-Text (.txt)<br/> ZIP-Dateien werden nicht unterstützt. |
   | Codierungsformat | UTF-8 |
   | Dateiname | Jede Datei muss einen eindeutigen Namen haben. Duplikate werden nicht unterstützt. |
   | Textlänge | Textdateien dürfen maximal 20.000 Zeichen lang sein. |
   | SSML-Einschränkungen | Jede SSML-Datei darf nur ein einziges SSML-Element enthalten. |

### <a name="plain-text-example"></a>Beispiel für Nur-Text

```txt
Welcome to use Audio Content Creation to customize audio output for your products.
```

### <a name="ssml-text-example"></a>Beispiel für SSML-Text

```xml
<speak xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="http://www.w3.org/2001/mstts" version="1.0" xml:lang="en-US">
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, AriaNeural)">
    Welcome to use Audio Content Creation <break time="10ms" />to customize audio output for your products.
    </voice>
</speak>
```

## <a name="export-tuned-audio"></a>Exportieren der optimierten Audiodatei

Nachdem Sie Ihre Audioausgabe überprüft haben und mit der Optimierung und den Anpassungen zufrieden sind, können Sie die Audiodatei exportieren.

1. Klicken Sie auf **Exportieren**, um eine Aufgabe zur Audioerstellung zu erstellen. Der **Export in eine Audiobibliothek** wird empfohlen, da dadurch die lange Audioausgabe und die gesamten Audioausgabefunktionen unterstützt werden. Sie können die Audiodaten auch direkt auf den lokalen Datenträger herunterladen, allerdings sind nur die ersten zehn Minuten verfügbar. 
2. Wählen Sie das Ausgabeformat für die Audiooptimierung aus. Eine Liste der unterstützten Formate und Abtastraten finden Sie unten.
3. Sie können den Status der Aufgabe auf der Registerkarte der **Exportaufgabe** anzeigen. Wenn die Aufgabe zu einem Fehler führt, finden Sie auf der Seite mit den ausführlichen Informationen einen vollständigen Bericht.
4. Wenn die Aufgabe abgeschlossen wurde, steht Ihre Audiodatei auf der Registerkarte **Audio Library** (Audiobibliothek) zum Download zur Verfügung.
5. Klicken Sie auf **Download**. Sie können nun Ihre benutzerdefinierten und optimierten Audiodaten in Ihren Apps oder Produkten verwenden.

### <a name="supported-audio-formats"></a>Unterstützte Audioformate

| Format | 16-kHz-Abtastrate | 24-kHz-Abtastrate |
|--------|--------------------|--------------------|
| WAV | riff-16khz-16bit-mono-pcm | riff-24khz-16bit-mono-pcm |
| MP3 | audio-16khz-128kbitrate-mono-mp3 | audio-24khz-160kbitrate-mono-mp3 |

## <a name="see-also"></a>Weitere Informationen

* [API für lange Audioinhalte](https://aka.ms/long-audio-api)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Speech Studio](https://speech.microsoft.com)
