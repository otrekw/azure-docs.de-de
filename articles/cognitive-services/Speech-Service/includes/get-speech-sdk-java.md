---
author: laujan
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: lajanuar
ms.openlocfilehash: dabaffe4e37bf3d818cd1478eba174c3b75ee4e0
ms.sourcegitcommit: e7d500f8cef40ab3409736acd0893cad02e24fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340505"
---
:::row:::
    :::column span="3":::
        Das Java SDK für Android ist als <a href="https://developer.android.com/studio/projects/android-library" target="_blank">AAR (Android-Bibliothek) </a> gepackt und enthält die erforderlichen Bibliotheken sowie die erforderlichen Android-Berechtigungen. Es wird in einem Maven-Repository unter `https://csspeechstorage.blob.core.windows.net/maven/` als Paket `com.microsoft.cognitiveservices.speech:client-sdk:1.17.0` gehostet. (Durchsuchen Sie [unser GitHub-Repository](https://github.com/Azure-Samples/cognitive-services-speech-sdk/search?q=com.microsoft.cognitiveservices.speech%3Aclient-sdk), um sich zu vergewissern, dass 1.17.0 die neueste Version ist.)
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Java" src="/media/logos/logo_java.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

Um das Paket im Android Studio-Projekt zu nutzen, nehmen Sie die folgenden Änderungen vor:

1. Fügen Sie der Datei *build.gradle* auf Projektebene Folgendes im Abschnitt `repositories` hinzu:
  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

2. Fügen Sie der Datei *build.gradle* auf Modulebene Folgendes im Abschnitt `dependencies` hinzu:
  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.17.0'
  ```

Das Java SDK ist auch Teil des [Speech-Geräte-SDK](../speech-devices-sdk.md).

#### <a name="additional-resources"></a>Zusätzliche Ressourcen

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android" target="_blank">Quellcode zum Android-spezifischen Java-Schnellstart </a>
- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/jre" target="_blank">Quellcode zum Schnellstart für die Java-Runtime (JRE) </a>
