---
title: Grundlagen der Speech-Befehlszeilenschnittstelle
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie das Speech-Befehlszeilenschnittstellen-Tool für das Arbeiten mit dem Speech-Dienst verwenden, mit nur minimaler Einrichtung und ohne zu programmieren.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: e859ac13c72ed07d3f57da6e61fd6d9f827f0fca
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88854897"
---
# <a name="learn-the-basics-of-the-speech-cli"></a>Erlernen der Grundlagen der Speech-Befehlszeilenschnittstelle

In diesem Artikel lernen Sie die grundlegenden Verwendungsmuster der Speech-Befehlszeilenschnittstelle kennen, eines Befehlszeilentools zur Nutzung des Speech-Diensts ohne Programmierung. Sie können die Hauptfunktionen des Speech-Diensts schnell ausprobieren, ohne Entwicklungsumgebungen zu erstellen oder zu programmieren, um festzustellen, ob Sie Ihren Anwendungsfällen angemessen gerecht werden können. Außerdem ist die Speech-Befehlszeilenschnittstelle für den Produktionsbetrieb bereit und kann verwendet werden, um einfache Arbeitsabläufe im Speech-Dienst mithilfe von `.bat`- oder Shellskripts zu automatisieren.

[!INCLUDE [](includes/spx-setup.md)]

## <a name="basic-usage"></a>Grundlegende Verwendung

In diesem Abschnitt werden einige grundlegende SPX-Befehle gezeigt, die häufig für erste Tests und Experimente nützlich sind. Führen Sie zunächst den unten angegebenen Befehl aus, um die integrierte Hilfe des Tools anzuzeigen.

```shell
spx
```

Achten Sie auf die Hilfethemen vom Typ **Siehe:** , die sich rechts von den Befehlsparametern befinden. Sie können diese Befehle eingeben, um ausführliche Hilfe zu Unterbefehlen zu erhalten.

Sie können Hilfethemen nach Schlüsselwort durchsuchen. Geben Sie beispielsweise den folgenden Befehl ein, um eine Liste der Beispiele für die Speech-CLI-Verwendung anzuzeigen:

```shell
spx help find --topics "examples"
```

Geben Sie den folgenden Befehl ein, um die Optionen für den Befehl „recognize“ anzuzeigen:

```shell
spx help recognize
```

Nutzen Sie nun den Speech-Dienst für eine erste Spracherkennung mit Ihrem Standardmikrofon, indem Sie den unten angegebenen Befehl ausführen.

```shell
spx recognize --microphone
```

Nachdem Sie den Befehl eingegeben haben, beginnt SPX mit dem aktiven Lauschen nach Audiodaten am aktuellen aktiven Eingabegerät und hält an, wenn Sie `ENTER` drücken. Die aufgezeichnete Sprache wird dann erkannt und in der Konsolenausgabe in Text konvertiert. Auch die Sprachsynthese lässt sich mithilfe der Speech-Befehlszeilenschnittstelle einfach verwenden. 

Beim Ausführen des folgenden Befehls wird der eingegebene Text als Eingabe angenommen und die synthetisierte Sprache auf dem aktuellen aktiven Ausgabegerät ausgegeben.

```shell
spx synthesize --text "Testing synthesis using the Speech CLI" --speakers
```

Über Spracherkennung und -synthese hinaus können Sie mit der Speech-Befehlszeilenschnittstelle auch Übersetzungen erstellen. Führen Sie ähnlich wie beim Spracherkennungsbefehl oben den folgenden Befehl aus, um Audio von Ihrem Standardmikrofon zu erfassen und die Übersetzung in Text in der Zielsprache durchzuführen.

```shell
spx translate --microphone --source en-US --target ru-RU --output file C:\some\file\path\russian_translation.txt
```

In diesem Befehl geben Sie sowohl die Quellsprache (Sprache, **aus** der übersetzt wird) als auch die Zielsprache (Sprache, **in** die übersetzt wird) ein. Wenn Sie das Argument `--microphone` angeben, wird am aktuellen Eingabegerät nach Audio gelauscht und angehalten, wenn Sie `ENTER` drücken. Die Ausgabe ist eine Textübersetzung in die Zielsprache, die in eine Textdatei geschrieben wird.

> [!NOTE]
> Eine Liste aller unterstützten Sprachen mit ihren entsprechenden Gebietsschemacodes finden Sie im [Artikel zu Sprachen und Gebietsschemas](language-support.md).

## <a name="batch-operations"></a>Batchvorgänge

Die Befehle im vorherigen Abschnitt eignen sich hervorragend, um schnell zu sehen, wie der Speech-Dienst funktioniert. Bei der Beurteilung, ob Ihre Anwendungsfälle sinnvoll bedient werden können, müssen Sie wahrscheinlich Batchvorgänge für eine Bandbreite von Eingaben ausführen, die Sie bereits besitzen, um zu sehen, wie der Dienst eine gewisse Bandbreite von Szenarien behandelt. Wie das geht, erfahren Sie in diesem Abschnitt:

* Ausführen der Spracherkennung für ein Verzeichnis mit Audiodateien
* Iterieren durch eine `.tsv`-Datei und Ausführen von Sprachsynthese

## <a name="batch-speech-recognition"></a>Batch-Spracherkennung

Wenn Sie über ein Verzeichnis mit Audiodateien verfügen, können Sie dafür mithilfe der Speech-Befehlszeilenschnittstelle schnell eine Batch-Spracherkennung ausführen. Führen Sie einfach den folgenden Befehl aus, wobei Sie mit dem Befehl `--files` auf Ihr Verzeichnis verweisen. In diesem Beispiel fügen Sie `\*.wav` an das Verzeichnis an, um alle im Verzeichnis vorhandenen `.wav`-Dateien zu erkennen. Geben Sie darüber hinaus das `--threads`-Argument an, um die Erkennung auf 10 parallelen Threads auszuführen.

> [!NOTE]
> Das `--threads`-Argument kann außerdem im nächsten Abschnitt für `spx synthesize`-Befehle verwendet werden, und die verfügbaren Threads hängen von der CPU und dem aktuellen Prozentsatz ihrer Auslastung ab.

```shell
spx recognize --files C:\your_wav_file_dir\*.wav --output file C:\output_dir\speech_output.tsv --threads 10
```

Die erkannte Sprachausgabe wird mit dem Argument `--output file` nach `speech_output.tsv` geschrieben. Es folgt ein Strukturbeispiel für die Ausgabedatei.

```output
audio.input.id    recognizer.session.started.sessionid    recognizer.recognized.result.text
sample_1    07baa2f8d9fd4fbcb9faea451ce05475    A sample wave file.
sample_2    8f9b378f6d0b42f99522f1173492f013    Sample text synthesized.
```

## <a name="batch-text-to-speech-synthesis"></a>Batch-Sprachsynthese

Die einfachste Möglichkeit zum Ausführen von Batch-Sprachsynthese besteht darin, eine neue `.tsv`-Datei (Text mit Tabstopps) zu erstellen und den `--foreach`-Befehl an der Speech-Befehlszeilenschnittstelle zu verwenden. Betrachten Sie die folgende Datei `text_synthesis.tsv`:

```output
audio.output    text
C:\batch_wav_output\wav_1.wav    Sample text to synthesize.
C:\batch_wav_output\wav_2.wav    Using the Speech CLI to run batch-synthesis.
C:\batch_wav_output\wav_3.wav    Some more text to test capabilities.
```

 Führen Sie als nächstes einen Befehl aus, um auf `text_synthesis.tsv` zu verweisen, Synthese für jedes `text`-Feld auszuführen und das Ergebnis unter dem entsprechenden `audio.output`-Pfad als `.wav`-Datei zu schreiben. 

```shell
spx synthesize --foreach in @C:\your\path\to\text_synthesis.tsv
```

Dieser Befehl ist das Äquivalent zur Ausführung von `spx synthesize --text Sample text to synthesize --audio output C:\batch_wav_output\wav_1.wav` **für jeden** Datensatz in der `.tsv`-Datei. Ein paar Dinge sind zu beachten:

* Die Spaltenüberschriften `audio.output` und `text` entsprechen den Befehlszeilenargumenten `--audio output` bzw. `--text`. Mehrteilige Befehlszeilenargumente wie `--audio output` sollten in der Datei ohne Leerzeichen, ohne führende Bindestriche und Punkte zur Trennung von Zeichenfolgen (Beispiel: `audio.output`) formatiert werden. Alle anderen vorhandenen Befehlszeilenargumente können der Datei mithilfe dieses Musters als zusätzliche Spalten hinzugefügt werden.
* Wenn die Datei auf diese Weise formatiert wird, müssen keine zusätzlichen Argumente an `--foreach` übergeben werden.
* Achten Sie darauf, jeden Wert in der `.tsv`-Datei mit einem **Tabstopp** zu trennen.

Im Fall einer `.tsv`-Datei wie im folgenden Beispiel jedoch, deren Spaltenüberschriften mit den Befehlszeilenargumenten **nicht übereinstimmen**:

```output
wav_path    str_text
C:\batch_wav_output\wav_1.wav    Sample text to synthesize.
C:\batch_wav_output\wav_2.wav    Using the Speech CLI to run batch-synthesis.
C:\batch_wav_output\wav_3.wav    Some more text to test capabilities.
```

Sie können diese Feldnamen mit den richtigen Argumenten überschreiben, sofern Sie dazu im `--foreach`-Aufruf die folgende Syntax verwenden. Dies ist derselbe Aufruf wie oben.

```shell
spx synthesize --foreach audio.output;text in @C:\your\path\to\text_synthesis.tsv
```

## <a name="next-steps"></a>Nächste Schritte

* Führen Sie die Schnellstarts zur [Spracherkennung](./quickstarts/speech-to-text-from-microphone.md) oder [Sprachsynthese](./quickstarts/text-to-speech.md) mithilfe des SDKs aus.
