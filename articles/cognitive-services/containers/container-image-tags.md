---
title: Cognitive Services-Containerimagetags
titleSuffix: Azure Cognitive Services
description: Eine umfassende Liste aller Cognitive Services-Containerimagetags.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 11/06/2020
ms.author: aahi
ms.openlocfilehash: 0efb278c217ede94f67b47250633622501a24414
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2020
ms.locfileid: "94412543"
---
# <a name="azure-cognitive-services-container-image-tags-and-release-notes"></a>Azure Cognitive Services-Containerimagetags und -Versionshinweise

Azure Cognitive Services bietet viele Containerimages. Die Containerregistrierungen und die entsprechenden Repositorys unterscheiden sich je nach Containerimage. Jeder Containerimagename bietet mehrere Tags. Ein Containerimagetag ist ein Mechanismus zur Versionsverwaltung für Containerimages. Dieser Artikel ist als umfassende Referenz für die Auflistung aller Cognitive Services-Containerimages und ihrer verfügbaren Tags gedacht.

> [!TIP]
> Achten Sie bei der Verwendung von [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) besonders auf die Groß-/Kleinschreibung der Containerregistrierung, des Repositorys, des Containerimagenamens und des entsprechenden Tags, da bei ihnen die **Groß-/Kleinschreibung berücksichtigt** wird.

## <a name="anomaly-detector"></a>Anomalieerkennung

Das Containerimage für die [Anomalieerkennung][ad-containers] finden Sie im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/decision` und trägt den Namen `anomaly-detector`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/decision/anomaly-detector/tags/list).

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013560003-amd64-preview` |      |

# <a name="previous-versions"></a>[Vorherige Versionen](#tab/previous)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `1.1.012300001-amd64-preview` |       |

---

## <a name="read-ocr-optical-character-recognition"></a>Read OCR (Optical Character Recognition; optische Zeichenerkennung)

Das Containerimage [Maschinelles Sehen][cv-containers] für das Lesen von OCR befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services` und trägt den Namen `read`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/vision/read`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/vision/read/tags/list).

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `v2.0.013250001-amd64-preview`:

* Verringern Sie die Speicherauslastung für den Container weiter.
* Für das Einrichten von mehreren Pods ist externer Cache erforderlich. Richten Sie z. B. Redis als Cache ein.
* Fehlender Ergebnisse, wenn Redis-Cache eingerichtet und `ResultExpirationPeriod` auf 0 festgelegt ist, wurden behoben.
* Heben Sie die Größenbeschränkung für den Anforderungstext von 26 MB auf. Der Container kann jetzt Dateien akzeptieren, die größer als 26 MB sind.
* Fügen Sie der Konsolenprotokollierung Zeitstempel und Buildversion hinzu.

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `2.0.013250001-amd64-preview` |  |

# <a name="previous-versions"></a>[Vorherige Versionen](#tab/previous)

Versionshinweise für `1.1.013050001-amd64-preview`

* Die Konfiguration `ReadEngineConfig:ResultExpirationPeriod` für die Containerinitialisierung wurde hinzugefügt, um anzugeben, wann das System Erkennungsergebnisse bereinigen soll. 
    * Die Einstellung wird in Stunden angegeben, und der Standardwert ist 48 Stunden.
    * Durch die Einstellung kann der Speicherverbrauch für die Speicherung der Ergebnisse reduziert werden. Dies gilt insbesondere dann, wenn für den Container ein In-Memory-Speicher verwendet wird.
    * Beispiel 1: ReadEngineConfig:ResultExpirationPeriod=1: Das System löscht das Erkennungsergebnis 1 Stunde nach dem Prozess.
    * Wird diese Konfiguration auf 0 festgelegt, löscht das System das Erkennungsergebnis nach dem Abrufen des Ergebnisses.

* Es wurde ein interner Serverfehler mit dem Code 500 korrigiert, wenn ein ungültiges Imageformat an das System übermittelt wurde. Nun wird ein 400-Fehler zurückgegeben:

    ```json
    {
        "error": {
        "code": "InvalidImageSize",
        "message": "Image must be between 1024 and 209715200 bytes."
        }
    }
    ```

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `1.1.013050001-amd64-preview` |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

---


## <a name="form-recognizer"></a>Formularerkennung

Das Containerimage [Formularerkennung][fr-containers] befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/custom-form` und trägt den Namen `labeltool`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/custom-form/labeltool/tags/list).

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |


# <a name="previous-versions"></a>[Vorherige Versionen](#tab/previous)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

---

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

Das Containerimage [LUIS][lu-containers] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services/language` und trägt den Namen `luis`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/language/luis`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/language/luis/tags/list).

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012280003-amd64-preview` |       |


# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `1.1.012130003-amd64-preview` |       |

---

## <a name="custom-speech-to-text"></a>Benutzerdefinierte Spracherkennung

Das Containerimage [Benutzerdefinierte Spracherkennung][sp-cstt] befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `custom-speech-to-text`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-speech-to-text`. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-speech-to-text/tags/list).


# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `2.6.0-amd64`:

**Funktionen**
* Unterstützung für PhraseList v2 
* Ausdruckslisten werden in folgenden Gebietsschemas unterstützt:
    * en-au
    * en-ca
    * en-gb
    * en-in
    * en-nz
    * de-de
    * zh-cn
* Unterstützung für das Herunterladen benutzerdefinierter Basismodelle. 
    * Verwenden Sie `BaseModelLocale=<locale>` mit dem Befehl `docker run`.
* Vollständig migriert zu .NET 3.1

**Fehlerbehebungen**
* Ein Problem wurde behoben, bei dem der Konfidenzscore im Diarisierungsmodus immer 1 war.
* Migriert zur TextAnalytics 3.0-API

Beachten Sie, dass dieses Containerimage aufgrund der enthaltenen Ausdruckslisten größer geworden ist.

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `2.6.0-amd64`                 |       |


# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

Versionshinweise für `2.5.0-amd64`:

**Funktionen**
* Unterstützung für benutzerdefinierte Aussprache bei benutzerdefinierten Modellen
* Unterstützung für Azure und Azure US Government Cloud

**Fehlerbehebungen**
* Behebung des Problems mit „Ausführen als Nicht-Root-Benutzer“ im Diarisierungsmodus

| Imagetags                    | Notizen               |
|-------------------------------|:--------------------|
| `2.5.0-amd64`                 |   1\. allgemein verfügbare Version    |

---

## <a name="custom-text-to-speech"></a>Benutzerdefinierte Sprachsynthese

Das Containerimage [Benutzerdefinierte Sprachsynthese][sp-ctts] befindet sich im Containerregistrierungssyndikat der `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `custom-text-to-speech`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-text-to-speech`. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-text-to-speech/tags/list).


# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `1.8.0-amd64`:

**Funktionen**
* Vollständig migriert zu .NET 3.1

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.8.0-amd64`                 |       |


# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

Versionshinweise für `1.7.0-amd64`:

**Feature**
* Teilweise migriert zu .NET 3.1

| Imagetags                    | Notizen               |
|-------------------------------|:--------------------|
| `1.7.0-amd64`                 |   1\. allgemein verfügbare Version    |

---

## <a name="speech-to-text"></a>Spracherkennung

Das Containerimage [Spracherkennung][sp-stt] befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `speech-to-text`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text`. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/speech-to-text/tags/list).

Seit Spracherkennung v2.5.0 werden Images in der Region *US Government Virginia* unterstützt. Verwenden Sie den Abrechnungsendpunkt sowie die API-Schlüssel der Region *US Government Virginia* , wenn Sie diese Region verwenden.

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `2.6.0-amd64-<locale>`:

**Funktionen**
* Upgrade auf die neuesten Modelle erfolgt und vollständig migriert zu .NET 3.1
* Unterstützung für PhraseList v2
* Ausdruckslisten werden in folgenden Gebietsschemas unterstützt:
    * en-au
    * en-ca
    * en-gb
    * en-in
    * en-nz
    * de-de
    * zh-cn
* Unterstützung für das neue Gebietsschema `cs-CZ` 
    * Groß-/Kleinschreibung und Satzzeichen werden zurzeit nicht unterstützt.

**Fehlerbehebungen**
* Ein Problem wurde behoben, bei dem der Konfidenzscore im Diarisierungsmodus immer 1 war.
* Migriert zur TextAnalytics 3.0-API

Beachten Sie, dass dieses Containerimage aufgrund der enthaltenen Ausdruckslisten größer geworden ist. 

| Imagetags                    | Notizen                                                                                                | 
|-------------------------------|:-----------------------------------------------------------------------------------------------------|
| `latest`                      | Containerimage mit dem Gebietsschema `en-US`.                                                             |
| `2.6.0-amd64-<locale>`        | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `2.6.0-amd64-en-us`. |

Für diesen Container sind die folgenden Gebietsschemas verfügbar.

| Gebietsschema für v2.6.0           | Notizen                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Containerimage mit dem Gebietsschema `ar-AE`. |
| `ar-eg`                     | Containerimage mit dem Gebietsschema `ar-EG`. |
| `ar-kw`                     | Containerimage mit dem Gebietsschema `ar-KW`. |
| `ar-qa`                     | Containerimage mit dem Gebietsschema `ar-QA`. |
| `ar-sa`                     | Containerimage mit dem Gebietsschema `ar-SA`. |
| `ca-es`                     | Containerimage mit dem Gebietsschema `ca-ES`. |
| `cs-cz`                     | Containerimage mit dem Gebietsschema `cs-CZ`. |
| `da-dk`                     | Containerimage mit dem Gebietsschema `da-DK`. |
| `de-de`                     | Containerimage mit dem Gebietsschema `de-DE`. |
| `en-au`                     | Containerimage mit dem Gebietsschema `en-AU`. |
| `en-ca`                     | Containerimage mit dem Gebietsschema `en-CA`. |
| `en-gb`                     | Containerimage mit dem Gebietsschema `en-GB`. |
| `en-in`                     | Containerimage mit dem Gebietsschema `en-IN`. |
| `en-nz`                     | Containerimage mit dem Gebietsschema `en-NZ`. |
| `en-us`                     | Containerimage mit dem Gebietsschema `en-US`. |
| `es-es`                     | Containerimage mit dem Gebietsschema `es-ES`. |
| `es-mx`                     | Containerimage mit dem Gebietsschema `es-MX`. |
| `fi-fi`                     | Containerimage mit dem Gebietsschema `fi-FI`. |
| `fr-ca`                     | Containerimage mit dem Gebietsschema `fr-CA`. |
| `fr-fr`                     | Containerimage mit dem Gebietsschema `fr-FR`. |
| `gu-in`                     | Containerimage mit dem Gebietsschema `gu-IN`. |
| `hi-in`                     | Containerimage mit dem Gebietsschema `hi-IN`. |
| `it-it`                     | Containerimage mit dem Gebietsschema `it-IT`. |
| `ja-jp`                     | Containerimage mit dem Gebietsschema `ja-JP`. |
| `ko-kr`                     | Containerimage mit dem Gebietsschema `ko-KR`. |
| `mr-in`                     | Containerimage mit dem Gebietsschema `mr-IN`. |
| `nb-no`                     | Containerimage mit dem Gebietsschema `nb-NO`. |
| `nl-nl`                     | Containerimage mit dem Gebietsschema `nl-NL`. |
| `pl-pl`                     | Containerimage mit dem Gebietsschema `pl-PL`. |
| `pt-br`                     | Containerimage mit dem Gebietsschema `pt-BR`. |
| `pt-pt`                     | Containerimage mit dem Gebietsschema `pt-PT`. |
| `ru-ru`                     | Containerimage mit dem Gebietsschema `ru-RU`. |
| `sv-se`                     | Containerimage mit dem Gebietsschema `sv-SE`. |
| `ta-in`                     | Containerimage mit dem Gebietsschema `ta-IN`. |
| `te-in`                     | Containerimage mit dem Gebietsschema `te-IN`. |
| `th-th`                     | Containerimage mit dem Gebietsschema `th-TH`. |
| `tr-tr`                     | Containerimage mit dem Gebietsschema `tr-TR`. |
| `zh-cn`                     | Containerimage mit dem Gebietsschema `zh-CN`. |
| `zh-hk`                     | Containerimage mit dem Gebietsschema `zh-HK`. |
| `zh-tw`                     | Containerimage mit dem Gebietsschema `zh-TW`. |


# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

Versionshinweise für `2.5.0-amd64-<locale>`:

**Funktionen**
* Unterstützung für Azure US Government Cloud

**Fehlerbehebungen**
* Behebt ein Problem mit „Ausführen als Nicht-Root-Benutzer“ im Diarisierungsmodus

| Imagetags                  | Notizen                                    |
|-----------------------------|:-----------------------------------------|
| `2.5.0-amd64-<locale>`      | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `2.5.0-amd64-en-us`.  |

Für diesen Container sind die folgenden Gebietsschemas verfügbar.

| Gebietsschema für v2.5.0           | Notizen                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Containerimage mit dem Gebietsschema `ar-AE`. |
| `ar-eg`                     | Containerimage mit dem Gebietsschema `ar-EG`. |
| `ar-kw`                     | Containerimage mit dem Gebietsschema `ar-KW`. |
| `ar-qa`                     | Containerimage mit dem Gebietsschema `ar-QA`. |
| `ar-sa`                     | Containerimage mit dem Gebietsschema `ar-SA`. |
| `ca-es`                     | Containerimage mit dem Gebietsschema `ca-ES`. |
| `da-dk`                     | Containerimage mit dem Gebietsschema `da-DK`. |
| `de-de`                     | Containerimage mit dem Gebietsschema `de-DE`. |
| `en-au`                     | Containerimage mit dem Gebietsschema `en-AU`. |
| `en-ca`                     | Containerimage mit dem Gebietsschema `en-CA`. |
| `en-gb`                     | Containerimage mit dem Gebietsschema `en-GB`. |
| `en-in`                     | Containerimage mit dem Gebietsschema `en-IN`. |
| `en-nz`                     | Containerimage mit dem Gebietsschema `en-NZ`. |
| `en-us`                     | Containerimage mit dem Gebietsschema `en-US`. |
| `es-es`                     | Containerimage mit dem Gebietsschema `es-ES`. |
| `es-mx`                     | Containerimage mit dem Gebietsschema `es-MX`. |
| `fi-fi`                     | Containerimage mit dem Gebietsschema `fi-FI`. |
| `fr-ca`                     | Containerimage mit dem Gebietsschema `fr-CA`. |
| `fr-fr`                     | Containerimage mit dem Gebietsschema `fr-FR`. |
| `gu-in`                     | Containerimage mit dem Gebietsschema `gu-IN`. |
| `hi-in`                     | Containerimage mit dem Gebietsschema `hi-IN`. |
| `it-it`                     | Containerimage mit dem Gebietsschema `it-IT`. |
| `ja-jp`                     | Containerimage mit dem Gebietsschema `ja-JP`. |
| `ko-kr`                     | Containerimage mit dem Gebietsschema `ko-KR`. |
| `mr-in`                     | Containerimage mit dem Gebietsschema `mr-IN`. |
| `nb-no`                     | Containerimage mit dem Gebietsschema `nb-NO`. |
| `nl-nl`                     | Containerimage mit dem Gebietsschema `nl-NL`. |
| `pl-pl`                     | Containerimage mit dem Gebietsschema `pl-PL`. |
| `pt-br`                     | Containerimage mit dem Gebietsschema `pt-BR`. |
| `pt-pt`                     | Containerimage mit dem Gebietsschema `pt-PT`. |
| `ru-ru`                     | Containerimage mit dem Gebietsschema `ru-RU`. |
| `sv-se`                     | Containerimage mit dem Gebietsschema `sv-SE`. |
| `ta-in`                     | Containerimage mit dem Gebietsschema `ta-IN`. |
| `te-in`                     | Containerimage mit dem Gebietsschema `te-IN`. |
| `th-th`                     | Containerimage mit dem Gebietsschema `th-TH`. |
| `tr-tr`                     | Containerimage mit dem Gebietsschema `tr-TR`. |
| `zh-cn`                     | Containerimage mit dem Gebietsschema `zh-CN`. |
| `zh-hk`                     | Containerimage mit dem Gebietsschema `zh-HK`. |
| `zh-tw`                     | Containerimage mit dem Gebietsschema `zh-TW`. |

---

## <a name="text-to-speech"></a>Text-zu-Sprache

Das Containerimage [Sprachsynthese][sp-tts] befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `text-to-speech`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/text-to-speech`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/text-to-speech/tags/list).


# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `1.8.0-amd64-<locale-and-voice>`:

**Feature**
* Vollständig migriert zu .NET 3.1

| Imagetags                                  | Notizen                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `latest`                                    | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.                                            | 
| `1.8.0-amd64-<locale-and-voice>`            | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.8.0-amd64-en-us-ariarus`.  |


| Gebietsschemas für v.1.8.0                          | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            |
| `ar-sa-naayf`                               | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           |
| `bg-bg-ivan`                                | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            |
| `ca-es-herenarus`                           | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       |
| `cs-cz-jakub`                               | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           |
| `da-dk-hellerus`                            | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        |
| `de-at-michael`                             | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         |
| `de-ch-karsten`                             | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         |
| `de-de-hedda`                               | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `de-de-heddarus`                            | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `de-de-stefan-apollo`                       | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   |
| `el-gr-stefanos`                            | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        |
| `en-au-catherine`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       |
| `en-au-hayleyrus`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       |
| `en-ca-heatherrus`                          | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      |
| `en-ca-linda`                               | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           |
| `en-gb-george-apollo`                       | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   |
| `en-gb-hazelrus`                            | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        |
| `en-gb-susan-apollo`                        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    |
| `en-ie-sean`                                | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            |
| `en-in-heera-apollo`                        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    |
| `en-in-priyarus`                            | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        |
| `en-in-ravi-apollo`                         | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     |
| `en-us-benjaminrus`                         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `en-us-guy24krus`                           | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `en-us-aria24krus`                          | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.      |
| `en-us-ariarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         |
| `en-us-zirarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `es-es-helenarus`                           | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       |
| `es-es-laura-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    |
| `es-es-pablo-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    |
| `es-mx-hildarus`                            | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        |
| `es-mx-raul-apollo`                         | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     |
| `fi-fi-heidirus`                            | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        |
| `fr-ca-caroline`                            | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        |
| `fr-ca-harmonierus`                         | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     |
| `fr-ch-guillaume`                           | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       |
| `fr-fr-hortenserus`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     |
| `fr-fr-julie-apollo`                        | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    |
| `fr-fr-paul-apollo`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     |
| `he-il-asaf`                                | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            |
| `hi-in-hemant`                              | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          |
| `hi-in-kalpana-apollo`                      | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  |
| `hi-in-kalpana`                             | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         |
| `hr-hr-matej`                               | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           |
| `hu-hu-szabolcs`                            | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        |
| `id-id-andika`                              | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          |
| `it-it-cosimo-apollo`                       | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   |
| `it-it-luciarus`                            | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        |
| `ja-jp-ayumi-apollo`                        | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    |
| `ja-jp-harukarus`                           | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       |
| `ja-jp-ichiro-apollo`                       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   |
| `ko-kr-heamirus`                            | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        |
| `ms-my-rizwan`                              | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          |
| `nb-no-huldarus`                            | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        |
| `nl-nl-hannarus`                            | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        |
| `pl-pl-paulinarus`                          | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      |
| `pt-br-daniel-apollo`                       | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   |
| `pt-br-heloisarus`                          | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      |
| `pt-pt-heliarus`                            | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        |
| `ro-ro-andrei`                              | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          |
| `ru-ru-ekaterinarus`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    |
| `ru-ru-irina-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    |
| `ru-ru-pavel-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    |
| `sk-sk-filip`                               | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           |
| `sl-si-lado`                                | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            |
| `sv-se-hedvigrus`                           | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       |
| `ta-in-valluvar`                            | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        |
| `te-in-chitra`                              | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          |
| `th-th-pattara`                             | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         |
| `tr-tr-sedarus`                             | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         |
| `vi-vn-an`                                  | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              |
| `zh-cn-huihuirus`                           | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `zh-cn-kangkang-apollo`                     | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `zh-cn-yaoyao-apollo`                       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |
| `zh-hk-danny-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    |
| `zh-hk-tracy-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    |
| `zh-hk-tracyrus`                            | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        |
| `zh-tw-hanhanrus`                           | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       |
| `zh-tw-yating-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   |
| `zh-tw-zhiwei-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   |


# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

Versionshinweise für `1.7.0-amd64-<locale-and-voice>`:

**Feature**
* Upgrade von Komponenten auf .NET 3.1

| Imagetags                                  | Notizen                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `1.7.0-amd64-<locale-and-voice>`            | 1\. allgemein verfügbare Version. Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.7.0-amd64-en-us-ariarus`.  |


| Gebietsschemas für v.1.7.0                          | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            |
| `ar-sa-naayf`                               | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           |
| `bg-bg-ivan`                                | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            |
| `ca-es-herenarus`                           | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       |
| `cs-cz-jakub`                               | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           |
| `da-dk-hellerus`                            | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        |
| `de-at-michael`                             | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         |
| `de-ch-karsten`                             | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         |
| `de-de-hedda`                               | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `de-de-heddarus`                            | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `de-de-stefan-apollo`                       | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   |
| `el-gr-stefanos`                            | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        |
| `en-au-catherine`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       |
| `en-au-hayleyrus`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       |
| `en-ca-heatherrus`                          | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      |
| `en-ca-linda`                               | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           |
| `en-gb-george-apollo`                       | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   |
| `en-gb-hazelrus`                            | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        |
| `en-gb-susan-apollo`                        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    |
| `en-ie-sean`                                | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            |
| `en-in-heera-apollo`                        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    |
| `en-in-priyarus`                            | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        |
| `en-in-ravi-apollo`                         | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     |
| `en-us-benjaminrus`                         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `en-us-guy24krus`                           | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `en-us-aria24krus`                          | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.      |
| `en-us-ariarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         |
| `en-us-zirarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `es-es-helenarus`                           | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       |
| `es-es-laura-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    |
| `es-es-pablo-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    |
| `es-mx-hildarus`                            | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        |
| `es-mx-raul-apollo`                         | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     |
| `fi-fi-heidirus`                            | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        |
| `fr-ca-caroline`                            | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        |
| `fr-ca-harmonierus`                         | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     |
| `fr-ch-guillaume`                           | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       |
| `fr-fr-hortenserus`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     |
| `fr-fr-julie-apollo`                        | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    |
| `fr-fr-paul-apollo`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     |
| `he-il-asaf`                                | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            |
| `hi-in-hemant`                              | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          |
| `hi-in-kalpana-apollo`                      | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  |
| `hi-in-kalpana`                             | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         |
| `hr-hr-matej`                               | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           |
| `hu-hu-szabolcs`                            | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        |
| `id-id-andika`                              | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          |
| `it-it-cosimo-apollo`                       | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   |
| `it-it-luciarus`                            | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        |
| `ja-jp-ayumi-apollo`                        | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    |
| `ja-jp-harukarus`                           | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       |
| `ja-jp-ichiro-apollo`                       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   |
| `ko-kr-heamirus`                            | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        |
| `ms-my-rizwan`                              | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          |
| `nb-no-huldarus`                            | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        |
| `nl-nl-hannarus`                            | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        |
| `pl-pl-paulinarus`                          | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      |
| `pt-br-daniel-apollo`                       | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   |
| `pt-br-heloisarus`                          | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      |
| `pt-pt-heliarus`                            | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        |
| `ro-ro-andrei`                              | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          |
| `ru-ru-ekaterinarus`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    |
| `ru-ru-irina-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    |
| `ru-ru-pavel-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    |
| `sk-sk-filip`                               | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           |
| `sl-si-lado`                                | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            |
| `sv-se-hedvigrus`                           | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       |
| `ta-in-valluvar`                            | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        |
| `te-in-chitra`                              | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          |
| `th-th-pattara`                             | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         |
| `tr-tr-sedarus`                             | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         |
| `vi-vn-an`                                  | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              |
| `zh-cn-huihuirus`                           | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `zh-cn-kangkang-apollo`                     | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `zh-cn-yaoyao-apollo`                       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |
| `zh-hk-danny-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    |
| `zh-hk-tracy-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    |
| `zh-hk-tracyrus`                            | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        |
| `zh-tw-hanhanrus`                           | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       |
| `zh-tw-yating-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   |
| `zh-tw-zhiwei-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   |

---

## <a name="neural-text-to-speech"></a>Text-zu-Sprache (neuronal)

Das Containerimage [neuronale Sprachsynthese][sp-ntts] befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `neural-text-to-speech`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/neural-text-to-speech`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/neural-text-to-speech/tags/list).

| Imagetags                                  | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `1.2.0-amd64-<locale-and-voice>-preview`    | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.2.0-amd64-en-us-arianeural-preview`. |


| v1.2.0 Vorschau Gebietsschemas und Stimmen           | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `de-de-katjaneural-preview`                 | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-KatjaNeural`.     |
| `en-au-natashaneural-preview`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-NatashaNeural`.   |
| `en-ca-claraneural-preview`                 | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-ClaraNeural`.     |
| `en-gb-libbyneural-preview`                 | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-LibbyNeural`.     |
| `en-gb-mianeural-preview`                   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-MiaNeural`.       |
| `en-us-arianeural-preview`                  | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `en-us-guyneural-preview`                   | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-GuyNeural`.       |
| `es-es-elviraneural-preview`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-ElviraNeural`.    |
| `es-mx-dalianeural-preview`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-DaliaNeural`.     |
| `fr-ca-sylvieneural-preview`                | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-SylvieNeural`.    |
| `fr-fr-deniseneural-preview`                | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-DeniseNeural`.    |
| `it-it-elsaneural-preview`                  | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-ElsaNeural`.      |
| `ja-jp-nanamineural-preview`                | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-NanamiNeural`.    |
| `ko-kr-sunhineural-preview`                 | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-SunHiNeural`.     |
| `pt-br-franciscaneural-preview`             | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-FranciscaNeural`. |
| `zh-cn-xiaoxiaoneural-preview`              | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoxiaoNeural`.  |

## <a name="speech-language-detection"></a>Speech-Sprachenerkennung

Das Containerimage [Speech-Sprachenerkennung][sp-lid] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `language-detection`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/language-detection`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/language-detection/tags/list).

| Imagetags                                  | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                       |      |
| `1.1.0-amd64-preview`                       |      |

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Das Containerimage befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/textanalytics/` und trägt den Namen `keyphrase`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/keyphrase/tags/list).

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)


| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` |       |

# <a name="previous-versions"></a>[Vorherige Versionen](#tab/previous)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `1.1.012840001-amd64` |       |
| `1.1.012830001-amd64`    |       |

---

## <a name="text-language-detection"></a>Textsprachenerkennung

Das Containerimage [Sprachenerkennung][ta-la] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services/textanalytics/` und trägt den Namen `language`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/textanalytics/language`.


Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/language/tags/list).

# <a name="latest-versions"></a>[Neueste Versionen](#tab/current)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` | |
   

# <a name="previous-versions"></a>[Vorherige Versionen](#tab/previous)


| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012840001-amd64` |   |
| `1.1.012830001-amd64` |   |

---

## <a name="sentiment-analysis"></a>Standpunktanalyse

Das Containerimage [Standpunktanalyse][ta-se] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services/textanalytics/` und trägt den Namen `sentiment`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/sentiment/tags/list).

| Imagetags | Notizen                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | Standpunktanalyse v3 (Englisch)               |
| `3.0-es`   | Standpunktanalyse v3 (Spanisch)               |
| `3.0-fr`   | Standpunktanalyse v3 (Französisch)                |
| `3.0-it`   | Standpunktanalyse v3 (Italienisch)               |
| `3.0-de`   | Standpunktanalyse v3 (Deutsch)                |
| `3.0-zh`   | Standpunktanalyse v3 (Chinesisch, vereinfacht)  |
| `3.0-zht`  | Standpunktanalyse v3 (Chinesisch, traditionell) |
| `3.0-ja`   | Standpunktanalyse v3 (Japanisch)              |
| `3.0-pt`   | Standpunktanalyse v3 (Portugiesisch)            |
| `3.0-nl`   | Standpunktanalyse v3 (Niederländisch)                 |
| `2.1`    | Standpunktanalyse v2      |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[sp-ntts]: ../speech-service/speech-container-howto.md?tabs=ntts
[sp-lid]: ../speech-service/speech-container-howto.md?tabs=lid
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
