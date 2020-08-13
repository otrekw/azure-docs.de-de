---
title: 'Schnellstart: Asynchrone Synthese für lange Audioinhalte (Vorschau) – Spracherkennungsdienst'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie die API für lange Audioinhalte, um Text asynchron in Sprache zu konvertieren und die Audioausgabe von einem vom Dienst bereitgestellten URI abzurufen. Diese REST-API eignet sich ideal für Inhaltsanbieter, die Textdateien mit mehr als 10.000 Zeichen oder 50 Absätzen in synthetisierte Sprache konvertieren müssen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.custom: devx-track-python
ms.openlocfilehash: 330bd3593ea10e4a4a03e430025b8acf1f7ab21a
ms.sourcegitcommit: dea88d5e28bd4bbd55f5303d7d58785fad5a341d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87875386"
---
# <a name="quickstart-asynchronous-synthesis-for-long-form-audio-in-python-preview"></a>Schnellstart: Asynchrone Synthese für lange Audioinhalte in Python (Vorschau)

In dieser Schnellstartanleitung verwenden Sie die API für lange Audioinhalte, um Text asynchron in Sprache zu konvertieren und die Audioausgabe von einem vom Dienst bereitgestellten URI abzurufen. Diese REST-API eignet sich ideal für Inhaltsanbieter, die Audiodaten aus Texten mit mehr als 5.000 Zeichen (oder mit einer Länge von mehr als 10 Minuten) synthetisieren müssen. Weitere Informationen finden Sie unter [API für lange Audioinhalte](../../long-audio-api.md).

Asynchrone Synthese für lange Audioinhalte kann mit [öffentlichen neuronalen Stimmen](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices) und [benutzerdefinierten neuronalen Stimmen](../../how-to-custom-voice.md#custom-neural-voices) verwendet werden. Beide unterstützen eine bestimmte Sprache und einen bestimmten Dialekt. 

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* Python 2.7.x oder 3.x
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) oder ein anderer Editor
* Ein Azure-Abonnement und ein Abonnementschlüssel für den Spracherkennungsdienst. [Erstellen Sie ein Azure-Konto](../../get-started.md#new-resource) und [eine Speech-Ressource](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#new-resource), um den Schlüssel zu erhalten. Stellen Sie beim Erstellen der Speech-Ressource sicher, dass als Tarif **S0** und als Standort eine [unterstützte Region](../../regions.md#standard-and-neural-voices) festgelegt ist.

## <a name="create-a-project-and-import-required-modules"></a>Erstellen eines Projekts und Importieren der erforderlichen Module

Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Python-Projekt. Kopieren Sie anschließend den folgenden Codeausschnitt in eine Datei namens `voice_synthesis_client.py`.

```python
import argparse
import json
import ntpath
import urllib3
import requests
import time
from json import dumps, loads, JSONEncoder, JSONDecoder
import pickle

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
```

> [!NOTE]
> Wenn Sie diese Module bisher nicht verwendet haben, müssen Sie sie vor der Ausführung Ihres Programms installieren. Führen Sie zum Installieren dieser Pakete `pip install requests urllib3` aus.

Diese Module werden verwendet, um Argumente zu analysieren, die HTTP-Anforderung zu erstellen und die REST-API für lange Audioinhalte (Sprachsynthese) aufzurufen.

## <a name="get-a-list-of-supported-voices"></a>Abrufen einer Liste der unterstützten Stimmen

Dieser Code ermöglicht es Ihnen, eine vollständige Liste der Stimmen für eine bestimmte Region bzw. für einen Endpunkt abzurufen, die bzw. den Sie verwenden können. Sehen Sie sich die Informationen zu [unterstützten Regionen/Endpunkten](../../long-audio-api.md) an. Fügen Sie den Code zu `voice_synthesis_client.py` hinzu:

```python
parser = argparse.ArgumentParser(description='Text-to-speech client tool to submit voice synthesis requests.')
parser.add_argument('--voices', action="store_true", default=False, help='print voice list')
parser.add_argument('-key', action="store", dest="key", required=True, help='the speech subscription key, like fg1f763i01d94768bda32u7a******** ')
parser.add_argument('-region', action="store", dest="region", required=True, help='the region information, could be centralindia, canadacentral or uksouth')
args = parser.parse_args()
baseAddress = 'https://%s.customvoice.api.speech.microsoft.com/api/texttospeech/v3.0-beta1/' % args.region

def getVoices():
    response=requests.get(baseAddress+"voicesynthesis/voices", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    voices = json.loads(response.text)
    return voices

if args.voices:
    voices = getVoices()
    print("There are %d voices available:" % len(voices))
    for voice in voices:
        print ("Name: %s, Description: %s, Id: %s, Locale: %s, Gender: %s, PublicVoice: %s, Created: %s" % (voice['name'], voice['description'], voice['id'], voice['locale'], voice['gender'], voice['isPublicVoice'], voice['created']))
```

### <a name="test-your-code"></a>Testen Ihres Codes

Lassen Sie uns testen, was Sie bisher geleistet haben. Sie müssen einige Punkte in der folgenden Anforderung aktualisieren:

* Ersetzen Sie `<your_key>` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst. Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.
* Ersetzen Sie `<region>` durch die Region, in der Ihre Speech-Ressource erstellt wurde (Beispiel: `eastus` oder `westus`). Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.

Führen Sie den folgenden Befehl aus:

```console
python voice_synthesis_client.py --voices -key <your_key> -region <Region>
```

Sie erhalten eine Ausgabe, die wie folgt aussieht:

```console
There are xx voices available:

Name: Microsoft Server Speech Text to Speech Voice (en-US, xxx), Description: xxx , Id: xxx, Locale: en-US, Gender: Male, PublicVoice: xxx, Created: 2019-07-22T09:38:14Z
Name: Microsoft Server Speech Text to Speech Voice (zh-CN, xxx), Description: xxx , Id: xxx, Locale: zh-CN, Gender: Female, PublicVoice: xxx, Created: 2019-08-26T04:55:39Z
```

Wenn der Parameter **PublicVoice** auf **True** festgelegt ist, wird eine öffentliche neuronale Stimme verwendet. Andernfalls wird eine benutzerdefinierte neuronale Stimme verwendet. 

## <a name="prepare-input-files"></a>Vorbereiten der Eingabedateien

Bereiten Sie eine Datei mit dem Eingabetext vor. Es kann sich um Nur-Text oder SSML-Text handeln. Informationen zu den Anforderungen an die Eingabedatei finden Sie unter [Vorbereiten der Inhalte für die Synthese](https://docs.microsoft.com/azure/cognitive-services/speech-service/long-audio-api#prepare-content-for-synthesis).

## <a name="convert-text-to-speech"></a>Konvertieren von Text in Sprache

Nach dem Vorbereiten der Eingabetextdatei fügen Sie diesen Code für die Sprachsynthese zu `voice_synthesis_client.py` hinzu:

> [!NOTE]
> 'concatenateResult' ist ein optionaler Parameter. Wenn dieser Parameter nicht festgelegt ist, wird für jeden Absatz eine Audioausgabe generiert. Indem Sie den Parameter festlegen, können Sie die Audiodateien in eine Ausgabe verketten. Standardmäßig ist die Audioausgabe auf riff-16khz-16bit-16bit-mono-pcm eingestellt. Weitere Informationen zu unterstützten Audioausgaben finden Sie unter [Audioausgabeformate](https://docs.microsoft.com/azure/cognitive-services/speech-service/long-audio-api#audio-output-formats).

```python
parser.add_argument('--submit', action="store_true", default=False, help='submit a synthesis request')
parser.add_argument('--concatenateResult', action="store_true", default=False, help='If concatenate result in a single wave file')
parser.add_argument('-file', action="store", dest="file", help='the input text script file path')
parser.add_argument('-voiceId', action="store", nargs='+', dest="voiceId", help='the id of the voice which used to synthesis')
parser.add_argument('-locale', action="store", dest="locale", help='the locale information like zh-CN/en-US')
parser.add_argument('-format', action="store", dest="format", default='riff-16khz-16bit-mono-pcm', help='the output audio format')

def submitSynthesis():
    modelList = args.voiceId
    data={'name': 'simple test', 'description': 'desc...', 'models': json.dumps(modelList), 'locale': args.locale, 'outputformat': args.format}
    if args.concatenateResult:
        properties={'ConcatenateResult': 'true'}
        data['properties'] = json.dumps(properties)
    if args.file is not None:
        scriptfilename=ntpath.basename(args.file)
        files = {'script': (scriptfilename, open(args.file, 'rb'), 'text/plain')}
    response = requests.post(baseAddress+"voicesynthesis", data, headers={"Ocp-Apim-Subscription-Key":args.key}, files=files, verify=False)
    if response.status_code == 202:
        location = response.headers['Location']
        id = location.split("/")[-1]
        print("Submit synthesis request successful")
        return id
    else:
        print("Submit synthesis request failed")
        print("response.status_code: %d" % response.status_code)
        print("response.text: %s" % response.text)
        return 0

def getSubmittedSynthesis(id):
    response=requests.get(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    synthesis = json.loads(response.text)
    return synthesis

if args.submit:
    id = submitSynthesis()
    if (id == 0):
        exit(1)

    while(1):
        print("\r\nChecking status")
        synthesis=getSubmittedSynthesis(id)
        if synthesis['status'] == "Succeeded":
            r = requests.get(synthesis['resultsUrl'])
            filename=id + ".zip"
            with open(filename, 'wb') as f:  
                f.write(r.content)
                print("Succeeded... Result file downloaded : " + filename)
            break
        elif synthesis['status'] == "Failed":
            print("Failed...")
            break
        elif synthesis['status'] == "Running":
            print("Running...")
        elif synthesis['status'] == "NotStarted":
            print("NotStarted...")
        time.sleep(10)
```

### <a name="test-your-code"></a>Testen Ihres Codes

Nun erstellen Sie eine Anforderung zur Textsynthese, bei der Ihre Eingabedatei als Quelle verwendet wird. Sie müssen einige Punkte in der folgenden Anforderung aktualisieren:

* Ersetzen Sie `<your_key>` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst. Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.
* Ersetzen Sie `<region>` durch die Region, in der Ihre Speech-Ressource erstellt wurde (Beispiel: `eastus` oder `westus`). Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.
* Ersetzen Sie `<input>` durch den Pfad zu der Textdatei, die Sie für die Sprachsynthese vorbereitet haben.
* Ersetzen Sie `<locale>` durch das gewünschte Ausgabegebietsschema. Weitere Informationen finden Sie unter [Sprachunterstützung](../../language-support.md#neural-voices).
* Ersetzen Sie `<voice_guid>` durch die gewünschte Ausgabestimme. Verwenden Sie eine der Stimmen, die durch [Abrufen einer Liste der unterstützten Stimmen](#get-a-list-of-supported-voices) zurückgegeben werden.

Konvertieren Sie mit dem folgenden Befehl den Text in Sprache:

```console
python voice_synthesis_client.py --submit -key <your_key> -region <Region> -file <input> -locale <locale> -voiceId <voice_guid>
```

> [!NOTE]
> Wenn Sie über mehr als eine Eingabedatei verfügen, müssen Sie mehrere Anforderungen übermitteln. Es gelten jedoch einige Einschränkungen, die Sie beachten müssen. 
> * Der Client darf für jedes Azure-Abonnementkonto bis zu **5** Anforderungen pro Sekunde an den Server übermitteln. Wenn dieses Limit überschritten wird, wird der Fehlercode 429 (zu viele Anforderungen) an den Client gesendet. Reduzieren Sie die Menge an Anforderungen pro Sekunde.
> * Der Server darf bis zu **120** Anforderungen für jedes Azure-Abonnementkonto ausführen und in die Warteschlange einreihen. Wenn dieses Limit überschritten wird, gibt der Server den Fehlercode 429 (zu viele Anforderungen) zurück. Warten Sie, und senden Sie erst dann wieder neue Anforderungen, wenn einige Anforderungen abgeschlossen sind.

Sie erhalten eine Ausgabe, die wie folgt aussieht:

```console
Submit synthesis request successful

Checking status
NotStarted...

Checking status
Running...

Checking status
Running...

Checking status
Succeeded... Result file downloaded : xxxx.zip
```

Das Ergebnis enthält den Eingabetext und die vom Dienst erzeugten Audioausgabedateien. Sie können diese Dateien in einem ZIP-Archiv herunterladen.

## <a name="remove-previous-requests"></a>Entfernen vorheriger Anforderungen

Der Server speichert bis zu **20.000** Anforderungen für jedes Azure-Abonnementkonto. Wenn die Anzahl Ihrer Anforderungen dieses Limit überschreitet, entfernen Sie vorherige Anforderungen, bevor Sie neue übermitteln. Wenn Sie vorhandene Anforderungen nicht entfernen, erhalten Sie eine Fehlermeldung.

Fügen Sie den Code zu `voice_synthesis_client.py` hinzu:

```python
parser.add_argument('--syntheses', action="store_true", default=False, help='print synthesis list')
parser.add_argument('--delete', action="store_true", default=False, help='delete a synthesis request')
parser.add_argument('-synthesisId', action="store", nargs='+', dest="synthesisId", help='the id of the voice synthesis which need to be deleted')

def getSubmittedSyntheses():
    response=requests.get(baseAddress+"voicesynthesis", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    syntheses = json.loads(response.text)
    return syntheses

def deleteSynthesis(ids):
    for id in ids:
        print("delete voice synthesis %s " % id)
        response = requests.delete(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
        if (response.status_code == 204):
            print("delete successful")
        else:
            print("delete failed, response.status_code: %d, response.text: %s " % (response.status_code, response.text))

if args.syntheses:
    synthese = getSubmittedSyntheses()
    print("There are %d synthesis requests submitted:" % len(synthese))
    for synthesis in synthese:
        print ("ID : %s , Name : %s, Status : %s " % (synthesis['id'], synthesis['name'], synthesis['status']))

if args.delete:
    deleteSynthesis(args.synthesisId)
```

### <a name="test-your-code"></a>Testen Ihres Codes

Überprüfen Sie jetzt, welche Anforderungen Sie zuvor bereits übermittelt haben. Bevor Sie fortfahren, müssen Sie einige Punkte in dieser Anforderung aktualisieren:

* Ersetzen Sie `<your_key>` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst. Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.
* Ersetzen Sie `<region>` durch die Region, in der Ihre Speech-Ressource erstellt wurde (Beispiel: `eastus` oder `westus`). Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.

Führen Sie den folgenden Befehl aus:

```console
python voice_synthesis_client.py --syntheses -key <your_key> -region <Region>
```

Dieser Befehl gibt eine Liste Ihrer Syntheseanforderungen zurück. Eine Ausgabe wie die folgende sollte angezeigt werden:

```console
There are <number> synthesis requests submitted:
ID : xxx , Name : xxx, Status : Succeeded
ID : xxx , Name : xxx, Status : Running
ID : xxx , Name : xxx : Succeeded
```

Entfernen Sie jetzt eine zuvor übermittelte Anforderung. Sie müssen einige Punkte im folgenden Code aktualisieren:

* Ersetzen Sie `<your_key>` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst. Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.
* Ersetzen Sie `<region>` durch die Region, in der Ihre Speech-Ressource erstellt wurde (Beispiel: `eastus` oder `westus`). Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.
* Ersetzen Sie `<synthesis_id>` durch den Wert, der in der vorherigen Anforderung zurückgegeben wurde.

> [!NOTE]
> Anforderungen mit dem Status „Wird ausgeführt“/„Wartend“ können nicht entfernt oder gelöscht werden.

Führen Sie den folgenden Befehl aus:

```console
python voice_synthesis_client.py --delete -key <your_key> -region <Region> -synthesisId <synthesis_id>
```

Eine Ausgabe wie die folgende sollte angezeigt werden:

```console
delete voice synthesis xxx
delete successful
```

## <a name="get-the-full-client"></a>Abrufen des vollständigen Clients

Die vollständige `voice_synthesis_client.py`-Datei steht auf [GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Python/voiceclient.py) zum Download zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zur API für lange Audioinhalte](../../long-audio-api.md)
