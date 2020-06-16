---
title: Versionshinweise – Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Hier finden Sie eine fortlaufende Liste der veröffentlichten Features, Verbesserungen, Fehlerbehebungen und bekannten Problemen des Speech-Diensts.
services: cognitive-services
author: oliversc
manager: jhakulin
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/08/2020
ms.author: oliversc
ms.custom: seodec18
ms.openlocfilehash: 13a7250bc89b1c9f81996dfa4e15d7d4469779ab
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84607872"
---
# <a name="release-notes"></a>Versionshinweise
## <a name="speech-sdk-1121-2020-june-release"></a>Speech SDK 1.12.1: Release von Juni 2020
**Speech-Befehlszeilenschnittstelle (auch als SPX bezeichnet)**
-   Hinzugefügte Suchfeatures für die Hilfe in der Befehlszeilenschnittstelle:
    -   `spx help find --text TEXT`
    -   `spx help find --topic NAME`
-   Für die neu bereitgestellten APIs der Version 3.0 für Batch und Custom Speech aktualisiert:
    -   `spx help batch examples`
    -   `spx help csr examples`

**Neue Features**
-   **C\#, C++:** Sprechererkennung (Vorschauversion): Dieses Feature ermöglicht die Sprecheridentifikation („Wer spricht?“) und Sprecherüberprüfung („Ist dies die angegebene Person?“). Beginnen Sie mit der [Übersicht](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/speaker-recognition-overview), und lesen Sie den Artikel [Grundlagen zur Sprechererkennung](https://docs.microsoft.com/azure/cognitive-services/speech-service/speaker-recognition-basics) oder die [API-Referenzdokumentation](https://docs.microsoft.com/rest/api/speakerrecognition/).

**Fehlerbehebungen**
-   **C\#, C++:** Die Mikrofonaufzeichnung funktionierte in 1.12 bei der Sprechererkennung nicht. Dies wurde behoben.
-   **JavaScript:** Die Sprachsynthese in Firefox und Safari unter macOS und iOS wurde korrigiert.
-   Es wurde ein Fehler behoben, bei dem es durch eine Zugriffsverletzung der Windows-Anwendungsüberprüfung bei der Unterhaltungstranskription von 8-Kanal-Datenströmen zu einem Absturz kam.
-   Es wurde ein Fehler behoben, bei dem es durch eine Zugriffsverletzung der Windows-Anwendungsüberprüfung bei der Konversationsübersetzung von mehreren Geräten zu einem Absturz kam.

**Beispiele**
-   **C#** : [Codebeispiel](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/dotnet/speaker-recognition) für die Sprechererkennung.
-   **C++:** [Codebeispiel](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp/windows/speaker-recognition) für die Sprechererkennung.
-   **Java**: [Codebeispiel](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/intent-recognition) für die Absichtserkennung unter Android. 

**Abgekürzte Tests aufgrund von COVID-19:** Da wir in den letzten Wochen remote gearbeitet haben, konnten wir die manuellen Tests zur Überprüfung nicht im gewohnten Umfang durchführen. Wir haben keine Änderungen vorgenommen, die möglicherweise zu Kompatibilitätsproblemen geführt hätten, und alle unsere automatisierten Tests wurden bestanden. Falls wir doch entgegen aller Wahrscheinlichkeit etwas übersehen haben sollten, informieren Sie uns bitte auf [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br>
Bleiben Sie gesund!


## <a name="speech-sdk-1120-2020-may-release"></a>Speech SDK 1.12.0: Release von Mai 2020
**Speech-Befehlszeilenschnittstelle (auch als SPX bezeichnet)**
- **SPX** ist ein neues Befehlszeilentool, mit dem Sie Aktionen wie Erkennung, Synthese, Übersetzung, Batch-Transkription und benutzerdefinierte Sprachverwaltung über die Befehlszeile ausführen können. Verwenden Sie es zum Testen des Speech-Diensts oder zum Erstellen von Skripts für die auszuführenden Aufgaben des Speech-Diensts. Das Tool steht [hier](https://docs.microsoft.com/azure/cognitive-services/speech-service/spx-overview) zum Download zur Verfügung. Dort finden Sie auch die Dokumentation.

**Neue Features**
- **Go**: Neue Unterstützung der Sprache Go für [Spracherkennung](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/speech-to-text-from-microphone?pivots=programming-language-go) und [benutzerdefinierten Sprachassistenten](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/voice-assistants?pivots=programming-language-go). Ihre Entwicklungsumgebung können Sie [hier](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?pivots=programming-language-go) einrichten. Beispielcode finden Sie weiter unten im Abschnitt „Beispiele“. 
- **JavaScript:** Browserunterstützung für Sprachsynthese hinzugefügt. Die zugehörige Dokumentation finden Sie [hier](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/text-to-speech-audio-file?pivots=programming-language-JavaScript).
- **C++, C#, Java:** Unterstützung des neuen `KeywordRecognizer`-Objekts sowie neuer APIs unter Windows, Android, Linux und iOS. Lesen Sie die [Dokumentation](https://docs.microsoft.com/azure/cognitive-services/speech-service/custom-keyword-overview). Beispielcode finden Sie weiter unten im Abschnitt „Beispiele“. 
- **Java**: Konversation mit mehreren Geräten mit Übersetzungsunterstützung hinzugefügt. Die zugehörige Referenzdokumentation finden Sie [hier](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.transcription).

**Verbesserungen und Optimierungen**
- **JavaScript:** Mikrofonimplementierung für Browser optimiert, um die Genauigkeit bei der Spracherkennung zu verbessern.
- **Java**: Bindungen mit direkter JNI-Implementierung ohne SWIG wurden umgestaltet. Dadurch wird die Bindungsgröße aller für Windows, Android, Linux und Mac verwendeten Java-Pakete um das Zehnfache verringert und die weitere Entwicklung der Speech SDK-Java-Implementierung vereinfacht.
- **Linux:** Die unterstützende [Dokumentation](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=linux) wurde mit den neuesten RHEL 7-spezifischen Anmerkungen aktualisiert.
- Die Verbindungslogik wurde verbessert, um im Falle von Dienst- oder Netzwerkfehlern mehrere Verbindungsversuche zu unternehmen.
- Die Speech-Schnellstartseite [portal.azure.com](https://portal.azure.com) wurde aktualisiert, um Entwickler beim nächsten Schritt für die Azure Speech-Journey zu unterstützen.

**Fehlerbehebungen**
- **C#, Java:** Es wurde ein [Problem](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/587) beim Laden von SDK-Bibliotheken in Linux ARM behoben (sowohl für die 32-Bit- als auch für die 64-Bit-Version).
- **C#** : Das explizite Löschen nativer Handles für die TranslationRecognizer-, IntentRecognizer- und Connection-Objekte wurde korrigiert.
- **C#** : Für das ConversationTranscriber-Objekt wurde die Lebensdauerverwaltung für Audioeingaben korrigiert.
- Es wurde ein Problem behoben, bei dem der Grund für das `IntentRecognizer`-Ergebnis nicht ordnungsgemäß festgelegt wurde, wenn Absichten aus einfachen Ausdrücken erkannt wurden.
- Es wurde ein Problem behoben, bei dem das `SpeechRecognitionEventArgs`-Ergebnisoffset nicht ordnungsgemäß festgelegt wurde.
- Es wurde eine Racebedingung behoben, bei der vom SDK versucht wurde, eine Netzwerknachricht zu senden, bevor die WebSocket-Verbindung hergestellt wurde. Dies war für `TranslationRecognizer` beim Hinzufügen von Teilnehmern reproduzierbar.
- Es wurden Arbeitsspeicherverluste in der Schlüsselworterkennungs-Engine korrigiert.

**Beispiele**
- **Go**: Schnellstartanleitungen für [Spracherkennung](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/speech-to-text-from-microphone?pivots=programming-language-go) und [benutzerdefinierten Sprachassistenten](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/voice-assistants?pivots=programming-language-go) hinzugefügt. Beispielcode finden Sie [hier](https://github.com/microsoft/cognitive-services-speech-sdk-go/tree/master/samples). 
- **JavaScript:** Schnellstartanleitungen für [Sprachsynthese](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/text-to-speech?pivots=programming-language-javascript), [Übersetzung](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/translate-speech-to-text?pivots=programming-language-javascript) und [Absichtserkennung](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/intent-recognition?pivots=programming-language-javascript) hinzugefügt.
- Schlüsselworterkennungsbeispiele für [C\#](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/uwp/keyword-recognizer) und [Java](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/keyword-recognizer) (Android).  

**Abgekürzte Tests aufgrund von COVID-19:** Da wir in den letzten Wochen remote gearbeitet haben, konnten wir die manuellen Tests zur Überprüfung nicht im gewohnten Umfang durchführen. Wir haben keine Änderungen vorgenommen, die möglicherweise zu Kompatibilitätsproblemen geführt hätten, und alle unsere automatisierten Tests wurden bestanden. Falls wir doch entgegen aller Wahrscheinlichkeit etwas übersehen haben sollten, informieren Sie uns bitte auf [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br>
Bleiben Sie gesund!

## <a name="speech-sdk-1110-2020-march-release"></a>Speech SDK 1.11.0: Release von März 2020
**Neue Features**
- Linux: Unterstützung für Red Hat Enterprise Linux (RHEL)/CentOS 7 x64 mit [Anweisungen](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-rhel-centos-7) zum Konfigurieren des Systems für Speech SDK hinzugefügt.
- Linux: Unterstützung für .NET Core C# unter Linux ARM32 und ARM64 hinzugefügt. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=linux). 
- C#, C++: `UtteranceId` in `ConversationTranscriptionResult` hinzugefügt. Dies ist eine konsistente ID für alle Spracherkennungs-Zwischenergebnisse und -Endergebnisse. Ausführlichere Informationen für [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.transcription.conversationtranscriptionresult?view=azure-dotnet) und [C++](https://docs.microsoft.com/cpp/cognitive-services/speech/transcription-conversationtranscriptionresult).
- Python: Unterstützung für `Language ID` wurde hinzugefügt. Siehe „speech_sample.py“ im [GitHub-Repository](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/python/console).
- Windows: Unterstützung für komprimierte Audioeingabeformate auf der Windows-Plattform für alle Win32-Konsolenanwendungen hinzugefügt. Ausführlichere Informationen finden Sie [hier](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-use-codec-compressed-audio-input-streams). 
- JavaScript: Unterstützung von Sprachsynthese (Text-to-Speech) in NodeJS. [Hier](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/text-to-speech)erhalten Sie weitere Informationen. 
- JavaScript: Fügen Sie neue APIs hinzu, um die Überprüfung aller gesendeten und empfangenen Nachrichten zu ermöglichen. [Hier](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript)erhalten Sie weitere Informationen. 
        
**Fehlerbehebungen**
- C#, C++: Es wurde ein Problem behoben, sodass `SendMessageAsync` jetzt binäre Nachrichten als binären Typ sendet. Ausführlichere Informationen für [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.connection.sendmessageasync?view=azure-dotnet#Microsoft_CognitiveServices_Speech_Connection_SendMessageAsync_System_String_System_Byte___System_UInt32_) und [C++](https://docs.microsoft.com/cpp/cognitive-services/speech/connection).
- C#, C++: Es wurde das Problem behoben, dass die Verwendung des `Connection MessageReceived`-Ereignisses einen Absturz verursachen kann, wenn `Recognizer` vor dem `Connection`-Objekt verworfen wird. Ausführlichere Informationen für [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.connection.messagereceived?view=azure-dotnet) und [C++](https://docs.microsoft.com/cpp/cognitive-services/speech/connection#messagereceived).
- Android: Die Audiopuffergröße des Mikrofons wurde von 800 ms auf 100 ms verringert, um die Wartezeit zu reduzieren.
- Android: Es wurde ein [Problem](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/563) beim x86-Android-Emulator in Android Studio behoben.
- JavaScript: Unterstützung für Regionen in China mit der `fromSubscription`-API hinzugefügt. Ausführlichere Informationen finden Sie [hier](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#fromsubscription-string--string-). 
- JavaScript: Fügen Sie weitere Fehlerinformationen zu Verbindungsfehlern aus NodeJS hinzu.
        
**Beispiele**
- Unity: Problem bei öffentlichem Absichtserkennungsbeispiel ist behoben, bei dem der LUIS-JSON-Import fehlgeschlagen ist. Ausführlichere Informationen finden Sie [hier](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/369).
- Python: Beispiel für `Language ID` hinzugefügt. Ausführlichere Informationen finden Sie [hier](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/python/console/speech_sample.py).
    
**Abgekürzte Tests aufgrund von COVID-19:** Da wir in den letzten Wochen remote gearbeitet haben, konnten wir die manuellen Tests zur Geräteüberprüfung nicht im gewohnten Umfang durchführen. Beispiele hierfür sind Tests der Mikrofoneingabe und Lautsprecherausgabe unter Linux, iOS und macOS. Wir haben keine Änderungen vorgenommen, die möglicherweise zu Beschädigungen auf diesen Plattformen geführt haben, und alle unsere automatisierten Tests wurden bestanden. Falls wir doch entgegen aller Wahrscheinlichkeit etwas übersehen haben sollten, informieren Sie uns bitte auf [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br> Vielen Dank für Ihre Unterstützung. Fragen können Sie wie immer auf [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen) oder in [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/731) stellen. Auf diesen Plattformen können Sie auch Feedback geben.<br>
Bleiben Sie gesund!

## <a name="speech-sdk-1100-2020-february-release"></a>Speech SDK 1.10.0: Release von Februar 2020

**Neue Features**

 - Python-Pakete zur Unterstützung des neuen Python-Release 3.8 hinzugefügt
 - x64-Unterstützung für Red Hat Enterprise Linux (RHEL)/CentOS 8 (C++, C#, Java, Python)
   > [!NOTE] 
   > Kunden müssen OpenSSL wie [hier beschrieben](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-openssl-linux) konfigurieren.
 - Linux ARM32-Unterstützung für Debian und Ubuntu
 - Von „DialogServiceConnector“ wird jetzt der optionale Parameter „bot ID“ für „BotFrameworkConfig“ unterstützt. Dieser Parameter ermöglicht die Verwendung mehrerer Direct Line Speech-Bots mit einer einzelnen Azure Speech-Ressource. Ohne Angabe des Parameters wird der (auf der Direct Line Speech-Kanalkonfigurationsseite festgelegte) Standardbot verwendet.
 - „DialogServiceConnector“ verfügt nun über die Eigenschaft „SpeechActivityTemplate“. Der Inhalt dieser JSON-Zeichenfolge wird von Direct Line Speech verwendet, um ein breites Spektrum an unterstützten Feldern in allen Aktivitäten vorab aufzufüllen, die einen Direct Line Speech-Bot erreichen. Hierzu zählen auch Aktivitäten, die als Reaktion auf Ereignisse automatisch generiert werden (beispielsweise Spracherkennung).
 - Von der Sprachsynthese wird nun der Abonnementschlüssel für die Authentifizierung verwendet. Dadurch verringert sich die Wartezeit für das erste Byte des ersten Syntheseergebnisses nach der Erstellung eines Synthesizers.
 - Verringerung der durchschnittlichen Wortfehlerrate um 18,6 Prozent dank aktualisierter Spracherkennungsmodelle für 19 Gebietsschemas (es-ES, es-MX, fr-CA, fr-FR, it-IT, ja-JP, ko-KR, pt-BR, zh-CN, zh-HK, nb-NO, fi-FL, ru-RU, pl-PL, ca-ES, zh-TW, th-TH, pt-PT, tr-TR). Die neuen Modelle führen zu erheblichen Verbesserungen in verschiedenen Bereichen. Hierzu zählen unter anderem Diktat, Callcentertranskription und Videoindizierung.

**Fehlerbehebungen**

 - Fehler behoben, der dazu führte, dass von der Unterhaltungstranskription in Java-APIs nicht ordnungsgemäß gewartet wurde 
 - Xamarin-bezogenes [GitHub-Problem](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/363) mit dem Android-x86-Emulator behoben
 - Fehlende (Get|Set)Property-Methoden zu „AudioConfig“ hinzugefügt
 - Fehler bei der Sprachsynthese behoben, der dazu führte, dass der Audiodatenstrom (audioDataStream) im Falle eines Verbindungsfehlers nicht beendet werden konnte
 - Die Verwendung eines Endpunkts ohne Region hatte USP-Fehler für die Konversationsübersetzung zur Folge.
 - Für die ID-Generierung in universellen Windows-Anwendungen wird nun ein Algorithmus für eine angemessen eindeutige GUID verwendet. Zuvor wurde ungewollt standardmäßig eine Stubimplementierung verwendet, die bei umfangreichen Interaktionen häufig zu Konflikten führte.
 
 **Beispiele**
 
 - Unity-Beispiel für die Verwendung des Speech SDK mit [Unity-Mikrofon und Pushmodusstreaming](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/unity/from-unitymicrophone)

**Weitere Änderungen**

 - [OpenSSL-Konfigurationsdokumentation für Linux aktualisiert](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-openssl-linux)

## <a name="speech-sdk-190-2020-january-release"></a>Speech SDK 1.9.0: Release 2020-January

**Neue Features**

- Mehrgerätekonversation: Verbinden Sie mehrere Geräte mit derselben sprach- oder textbasierten Konversation, und übersetzen Sie optional die zwischen ihnen gesendeten Nachrichten. Weitere Informationen finden Sie in [diesem Artikel](multi-device-conversation.md). 
- Unterstützung für die Schlüsselworterkennung wurde für das AAR-Paket für Android und für x86- und x64-Versionen hinzugefügt. 
- Objective-C: Methoden `SendMessage` und `SetMessageProperty` wurden dem `Connection`-Objekt hinzugefügt. Die zugehörige Dokumentation finden Sie [hier](https://docs.microsoft.com/objectivec/cognitive-services/speech/spxconnection).
- Die TTS-API in C++ unterstützt jetzt `std::wstring` als Texteingabe für die Synthese. Dadurch ist es nicht mehr erforderlich, den Typ wstring vor der Übergabe an das SDK in string zu konvertieren. Ausführlichere Informationen finden Sie [hier](https://docs.microsoft.com/cpp/cognitive-services/speech/speechsynthesizer#speaktextasync). 
- C#: [Sprach-ID](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-automatic-language-detection?pivots=programming-language-csharp) und [Ausgangssprachenkonfiguration](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-specify-source-language?pivots=programming-language-csharp) sind jetzt verfügbar.
- JavaScript: Dem `Connection`-Objekt wurde eine Funktion für die Weiterleitung benutzerdefinierter Nachrichten vom Speech-Dienst als Rückruf von `receivedServiceMessage` zu hinzugefügt.
- JavaScript: Unterstützung für `FromHost API` wurde hinzugefügt, um die Verwendung mit lokalen Containern und Sovereign Clouds zu vereinfachen Die zugehörige Dokumentation finden Sie [hier](speech-container-howto.md).
- JavaScript: `NODE_TLS_REJECT_UNAUTHORIZED` wird nun dank eines Beitrags von [orgads](https://github.com/orgads) berücksichtigt. Ausführlichere Informationen finden Sie [hier](https://github.com/microsoft/cognitive-services-speech-sdk-js/pull/75).

**Wichtige Änderungen**

- `OpenSSL` wurde auf Version 1.1.1b aktualisiert und ist statisch mit der Kernbibliothek des Speech SDK für Linux verknüpft. Dies kann zu einer Unterbrechung führen, wenn `OpenSSL` für Ihren Posteingang nicht im Verzeichnis `/usr/lib/ssl` im System installiert wurde. In [unserer Dokumentation](how-to-configure-openssl-linux.md) in den Dokumenten zum Speech SDK finden Sie Möglichkeiten, wie Sie das Problem umgehen können.
- Wir haben den in C# für `WordLevelTimingResult.Offset` zurückgegebenen Datentyp von `int` in `long` geändert, um den Zugriff auf `WordLevelTimingResults` zu ermöglichen, wenn Sprachdaten länger als 2 Minuten sind.
- `PushAudioInputStream` und `PullAudioInputStream` senden nun WAV-Headerinformationen an den Speech-Dienst basierend auf dem `AudioStreamFormat`, das bei der Erstellung optional angegeben werden kann. Kunden müssen nun das [unterstützte Audioeingabeformat](how-to-use-audio-input-streams.md) verwenden. Alle anderen Formate führen zu weniger guten Erkennungsergebnissen oder anderen Problemen. 

**Fehlerbehebungen**

- Weitere Informationen finden Sie im obigen `OpenSSL`-Update unter „Wichtige Änderungen“. Wir haben sowohl einen zeitweiligen Absturz als auch ein Leistungsproblem (Sperrkonflikte bei hoher Auslastung) in Linux und Java korrigiert. 
- Java: Es wurden Verbesserungen am Objektabschluss in Szenarien mit hoher Parallelität vorgenommen.
- Das NuGet-Paket wurde umstrukturiert. Wir haben die drei Kopien von `Microsoft.CognitiveServices.Speech.core.dll` und `Microsoft.CognitiveServices.Speech.extension.kws.dll` im Ordner „lib“ entfernt, sodass das NuGet-Paket nun kleiner ist und schneller heruntergeladen werden kann. Außerdem haben wir Header hinzugefügt, die zum Kompilieren einiger nativer C++-Apps benötigt werden.
- Die korrigierten Schnellstartbeispiele finden Sie [hier](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp). Diese wurden ohne Anzeige der Ausnahme „Mikrofon wurde nicht gefunden“ unter Linux, macOS und Windows beendet.
- Ein SDK-Absturz bei langen Spracherkennungsergebnissen für bestimmte Codepfade wie in [diesem Beispiel](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/csharp/uwp/speechtotext-uwp) wurde korrigiert.
- Ein Fehler bei der SDK-Bereitstellung in Azure-Web-App-Umgebungen wurde behoben, um [dieses Kundenproblem](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/396) zu beseitigen.
- Ein TTS-Fehler bei der Verwendung mehrerer `<voice>`- oder `<audio>`-Tags wurde behoben, um [dieses Kundenproblems](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/433) zu beseitigen. 
- Ein TTS 401-Fehler beim Wiederherstellen des SDK nach dem Anhalten wurde behoben.
- JavaScript: Ein zirkulärer Import von Audiodaten wurde dank eines Beitrags von [euirim](https://github.com/euirim) korrigiert. 
- JavaScript: Unterstützung für das Festlegen von Diensteigenschaften wurde wie in 1.7 hinzugefügt.
- JavaScript: Ein Problem wurde behoben, bei dem ein Verbindungsfehler zu kontinuierlichen erfolglosen WebSocket-Verbindungsversuchen führen konnte.

**Beispiele**

- Es wurde ein [Beispiel für die Schlüsselworterkennung für Android](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/java/android/sdkdemo) hinzugefügt.
- Es wurde ein [TTS-Beispiel für das Serverszenario](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/speech_synthesis_server_scenario_sample.cs) hinzugefügt.
- Es wurden [Schnellstarts für die Mehrgerätekonversation in C# und C++](quickstarts/multi-device-conversation.md) hinzugefügt.

**Weitere Änderungen**

- Die Größe der SDK-Kernbibliothek unter Android wurde optimiert.
- Das SDK ab Version 1.9.0 unterstützt sowohl `int`- als auch `string`-Typen im Feld für die Stimmensignaturversion für die Unterhaltungstranskription.

## <a name="speech-sdk-180-2019-november-release"></a>Speech SDK 1.8.0: Release von November 2019

**Neue Features**

- `FromHost()`-API hinzugefügt, um die Verwendung mit lokalen Containern und Sovereign Clouds zu vereinfachen
- Automatische Erkennung der Ausgangssprache für die Spracherkennung hinzugefügt (in Java und C++)
- `SourceLanguageConfig`-Objekt zur Angabe erwarteter Ausgangssprachen für die Spracherkennung hinzugefügt (in Java und C++)
- `KeywordRecognizer`-Unterstützung unter Windows (UWP), Android und iOS über die NuGet- und Unity-Pakete hinzugefügt
- Java-Remoteunterhaltungs-API für die Unterhaltungstranskription in asynchronen Batches hinzugefügt

**Wichtige Änderungen**

- Die Funktionen für die Unterhaltungstranskription wurden unter den Namespace `Microsoft.CognitiveServices.Speech.Transcription` verschoben.
- Ein Teil der Unterhaltungstranskriptionsmethoden wurde in die neue `Conversation`-Klasse verschoben.
- Die Unterstützung für 32-Bit-iOS (ARMv7 und x86) wurde eingestellt.

**Fehlerbehebungen**

- Ein Absturz wurde behoben, der auftrat, wenn die lokale `KeywordRecognizer`-Instanz ohne gültigen Abonnementschlüssel für den Speech-Dienst verwendet wurde.

**Beispiele**

- Xamarin-Beispiel für `KeywordRecognizer`
- Unity-Beispiel für `KeywordRecognizer`
- C++- und Java-Beispiele für die automatische Erkennung der Ausgangssprache

## <a name="speech-sdk-170-2019-september-release"></a>Speech SDK 1.7.0: Release von September 2019

**Neue Features**

- Unterstützung der Betaversion für Xamarin unter der universellen Windows-Plattform (UWP), Android und iOS wurde hinzugefügt
- iOS-Unterstützung für Unity wurde hinzugefügt
- Unterstützung von `Compressed`-Eingaben für ALaw, Mulaw, FLAC unter Android, iOS und Linux hinzugefügt
- `SendMessageAsync` in der Klasse `Connection` zum Senden einer Nachricht an einen Dienst hinzugefügt
- `SetMessageProperty` in der Klasse `Connection` zum Festlegen der Eigenschaft einer Nachricht hinzugefügt
- TTS hat Bindungen für Java (JRE und Android), Python, Swift und Objective-C hinzugefügt
- TTS hat die Unterstützung der Wiedergabe für macOS, iOS und Android hinzugefügt
- Es wurden Informationen zur „Wortgrenze“ für TTS hinzugefügt

**Fehlerbehebungen**

- IL2CPP-Buildproblem in Unity 2019 für Android wurde behoben
- Es wurde ein Problem behoben, bei dem falsch formatierte Header in der Eingabe von WAV-Dateien falsch verarbeitet wurden
- Es wurde ein Problem behoben, bei dem UUIDs in einigen Verbindungseigenschaften nicht eindeutig waren
- Es wurden einige Warnungen bezüglich Spezifizierer für die NULL-Zulässigkeit in den Swift-Bindungen behoben (möglicherweise sind kleine Codeänderungen erforderlich)
- Es wurde ein Fehler behoben, der dazu führte, dass WebSocket-Verbindungen unter Netzwerklast nicht ordnungsgemäß geschlossen wurden
- Problem unter Android behoben, das gelegentlich dazu führt, dass `DialogServiceConnector` doppelte Eindruck-IDs verwendet
- Es wurden Verbesserungen an der Stabilität von Verbindungen über Interaktionen mit Mehrfachdurchläufen und an der Berichterstellung bei Fehlern vorgenommen (über Ereignisse vom Typ `Canceled`), wenn sie mit `DialogServiceConnector` auftreten.
- `DialogServiceConnector`-Sitzungsstarts stellen jetzt ordnungsgemäß Ereignisse bereit, einschließlich des Aufrufs von `ListenOnceAsync()`, während `StartKeywordRecognitionAsync()` aktiv ist.
- Es wurde ein Absturzproblem behoben, das mit dem Empfangen von `DialogServiceConnector`-Aktivitäten verbunden war.

**Beispiele**

- Schnellstart für Xamarin
- Aktualisierter CPP-Schnellstart mit Linux ARM64-Informationen
- Aktualisierter Unity-Schnellstart mit iOS-Informationen

## <a name="speech-sdk-160-2019-june-release"></a>Speech SDK 1.6.0: Release von Juni 2019

**Beispiele**

- Schnellstartbeispiele für Sprachsynthese auf UWP und Unity
- Schnellstartbeispiel für Swift unter iOS
- Unity-Beispiele für Sprach- und Absichtserkennung sowie Übersetzung
- Schnellstartbeispiele für `DialogServiceConnector` aktualisiert

**Verbesserungen/Änderungen**

- Dialog „Namespace“:
  - `SpeechBotConnector` wurde in `DialogServiceConnector` umbenannt.
  - `BotConfig` wurde in `DialogServiceConfig` umbenannt.
  - `BotConfig::FromChannelSecret()` wurde `DialogServiceConfig::FromBotSecret()` neu zugeordnet.
  - Alle vorhandenen Direct Line Speech-Clients werden nach der Umbenennung weiterhin unterstützt.
- Aktualisierung des TTS-REST-Adapter zur Unterstützung von Proxys, dauerhafte Verbindung
- Verbesserung von Fehlermeldungen, wenn eine ungültige Region übergeben wird.
- Swift/Objective-C:
  - Verbesserte Fehlerberichterstellung: Methoden, die zu einem Fehler führen können, sind jetzt in zwei Versionen vorhanden: Eine, die ein `NSError`-Objekt für die Fehlerbehandlung bereitstellt, und eine, das eine Ausnahme auslöst. Das erste wird für Swift verfügbar gemacht. Diese Änderung erfordert Anpassungen an vorhandenem Swift-Code.
  - Verbesserte Behandlung von Ereignissen

**Fehlerbehebungen**

- Korrektur für TTS: Hierbei führte `SpeakTextAsync` die Rückgabe aus, ohne zu warten, bis das Audiorendering abgeschlossen war.
- Korrektur für das Marshalling von Zeichenfolgen in C#, um vollständige Sprachunterstützung zu ermöglichen.
- Korrektur für ein .NET Core-App-Problem beim Laden der Core-Bibliothek mit dem Zielframework net461 in Beispielen.
- Korrektur für gelegentlich Probleme beim Bereitstellen nativer Bibliotheken im Ausgabeordner in Beispielen.
- Korrektur für das zuverlässige Schließen von WebSockets.
- Korrektur für mögliche Abstürze beim Öffnen einer Verbindung bei sehr hoher Auslastung unter Linux.
- Korrektur für fehlende Metadaten im Frameworkbündel für macOS.
- Korrektur für Probleme mit `pip install --user` unter Windows.

## <a name="speech-sdk-151"></a>Speech SDK 1.5.1

Dies ist ein Fehlerbehebungsrelease und betrifft nur das native/verwaltete SDK. Es betrifft nicht die JavaScript-Version des SDK.

**Fehlerbehebungen**

- Fehlerbehebung bei FromSubscription bei Verwendung mit Unterhaltungstranskription.
- Fehlerbehebung bei der Schlüsselworterkennung für Sprachassistenten.

## <a name="speech-sdk-150-2019-may-release"></a>Speech SDK 1.5.0: Release von Mai 2019

**Neue Features**

- Die Schlüsselworterkennung (Keyword Spotting Functionality, KWS) ist für Windows und Linux verfügbar. Die KWS-Funktionalität kann u. U. mit jedem Mikrofontyp verwendet werden, offiziell wird KWS derzeit jedoch nur für die Mikrofonarrays in der Azure Kinect DK-Hardware oder im Speech Devices SDK unterstützt.
- Begriffshinweisfunktionalität ist über das SDK verfügbar. Weitere Informationen finden Sie [hier](how-to-phrase-lists.md).
- Unterhaltungstranskriptionsfunktionalität ist über das SDK verfügbar. Klicken Sie [hier](conversation-transcription-service.md).
- Unterstützung für Sprachassistenten über den Direct Line Speech-Kanal wurde hinzugefügt.

**Beispiele**

- Beispiele für neue Funktionen oder neue Dienste, die vom SDK unterstützt werden, wurden hinzugefügt.

**Verbesserungen/Änderungen**

- Verschiedene Erkennungseigenschaften wurden hinzugefügt, um das Dienstverhalten oder Dienstergebnisse anzupassen (z. B. Maskieren von Obszönitäten).
- Sie können die Erkennung jetzt über die Standardkonfigurationseigenschaften konfigurieren, auch wenn Sie den Erkenner `FromEndpoint` erstellt haben.
- Objective-C: Die Eigenschaft `OutputFormat` wurde zu `SPXSpeechConfiguration` hinzugefügt.
- Das SDK unterstützt jetzt Debian 9 als Linux-Distribution.

**Fehlerbehebungen**

- Es wurde ein Problem behoben, bei dem die Sprecherressource in der Sprachsynthese zu früh zerstört wurde.

## <a name="speech-sdk-142"></a>Speech SDK 1.4.2

Dies ist ein Fehlerbehebungsrelease und betrifft nur das native/verwaltete SDK. Es betrifft nicht die JavaScript-Version des SDK.

## <a name="speech-sdk-141"></a>Speech SDK 1.4.1

Dieses Release gilt nur für JavaScript. Es wurden keine Features hinzugefügt. Die folgenden Fehler wurden behoben:

- Verhindern Sie das Laden von „https-proxy-agent“ durch Webpack.

## <a name="speech-sdk-140-2019-april-release"></a>Speech SDK 1.4.0: Release von April 2019

**Neue Features**

- Das SDK unterstützt jetzt den Text-Sprach-Dienst als Betaversion. Dies wird unter Windows- und Linux-Desktops für C++ und C# unterstützt. Weitere Informationen finden Sie in der [Übersicht über die Sprachsynthese](text-to-speech.md#get-started).
- Das SDK unterstützt jetzt MP3- und Opus/OGG-Audiodateien als Streameingabedateien. Dieses Feature steht nur unter Linux mit C++ und C# zur Verfügung und befindet sich derzeit in der Betaversion (weitere Details finden Sie [hier](how-to-use-codec-compressed-audio-input-streams.md)).
- Das Speech SDK für Java, .NET Core, C++ und Objective-C unterstützt nun auch macOS. Die Objective-C-Unterstützung für macOS befindet sich derzeit in der Betaphase.
- iOS: Das Speech SDK für iOS (Objective-C) wird jetzt auch als ein CocoaPod veröffentlicht.
- JavaScript: Unterstützung von nicht standardisierten Mikrofonen als Eingabegeräte.
- JavaScript: Proxyunterstützung für Node.js.

**Beispiele**

- Beispiele für die Verwendung des Speech SDK mit C++ und Objective-C unter macOS wurden hinzugefügt.
- Beispiele zur Veranschaulichung der Verwendung des Text-zu-Sprache-Diensts wurden hinzugefügt.

**Verbesserungen/Änderungen**

- Python: Zusätzliche Eigenschaften der Erkennungsergebnisse werden jetzt über die `properties`-Eigenschaft verfügbar gemacht.
- Zur weiteren Unterstützung beim Entwickeln und Debuggen können Sie die Informationen aus SDK-Protokollierung und Diagnose in eine Protokolldatei umleiten (weitere Details finden Sie [hier](how-to-use-logging.md)).
- JavaScript: Verbesserte Prozessleistung bei Audiodaten.

**Fehlerbehebungen**

- Mac/iOS: Ein Fehler, der zu einer langen Wartezeit geführt hat, wenn keine Verbindung mit Speech Services hergestellt werden konnte, wurde behoben.
- Python: verbesserte Fehlerbehandlung für Argumente in Python-Rückrufen.
- JavaScript: Ein Fehler bei Statusmeldungen nach dem Ende der Spracheingabe mit RequestSession wurde behoben.

## <a name="speech-sdk-131-2019-february-refresh"></a>Sprach-SDK 1.3.1: Aktualisierung von Februar 2019

Dies ist ein Fehlerbehebungsrelease und betrifft nur das native/verwaltete SDK. Es betrifft nicht die JavaScript-Version des SDK.

**Fehlerbehebung**

- Korrigiert einen Speicherverlust bei der Verwendung von Mikrofoneingabe. Streambasierte oder Dateieingaben sind nicht betroffen.

## <a name="speech-sdk-130-2019-february-release"></a>Speech SDK 1.3.0: Version von Februar 2019

**Neue Features**

- Das Speech SDK unterstützt die Auswahl des Eingangsmikrofons über die `AudioConfig`-Klasse. Dadurch können Sie Audiodaten über ein anderes als das Standardmikrofon an den Spracherkennungsdienst streamen. Weitere Informationen finden Sie in der Dokumentation, in der die [Auswahl eines Audioeingabegeräts](how-to-select-audio-input-devices.md) beschrieben wird. Für JavaScript ist diese Funktion noch nicht verfügbar.
- Das Speech SDK unterstützt jetzt Unity in einer Betaversion. Senden Sie uns Feedback über den Abschnitt „Issue“ im [GitHub-Beispielrepository](https://aka.ms/csspeech/samples). Dieses Release unterstützt Unity unter Windows x86 und x64 (Desktopanwendungen oder Anwendungen der universellen Windows-Plattform) und unter Android (ARM32/64, x86). Weitere Informationen finden Sie in unserem [Unity-Schnellstart](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=unity).
- Die Datei `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (im Lieferumfang von früheren Releases enthalten) ist nicht mehr erforderlich. Die Funktion ist jetzt in das Core-SDK integriert.

**Beispiele**

Die folgenden neuen Inhalte stehen in unserem [Beispielrepository](https://aka.ms/csspeech/samples) zur Verfügung:

- Weitere Beispiele für `AudioConfig.FromMicrophoneInput`
- Weitere Python-Beispiele für Absichtserkennung und Übersetzung.
- Weitere Beispiele für die Verwendung des Objekts `Connection` in iOS
- Weitere Java-Beispiele für die Übersetzung mit Audioausgabe.
- Neues Beispiel für die Verwendung der [REST-API zur Batchtranskription](batch-transcription.md).

**Verbesserungen/Änderungen**

- Python
  - Verbesserte Parameterüberprüfung und Fehlermeldungen in `SpeechConfig`
  - Unterstützung für das Objekt `Connection` hinzugefügt
  - Unterstützung für 32-Bit-Python (x86) unter Windows.
  - Das Speech SDK für Python befindet sich nicht mehr in der Betaversion.
- iOS
  - Das SDK wird jetzt für das iOS SDK, Version 12.1, erstellt.
  - Das SDK unterstützt jetzt die iOS-Versionen 9.2 und höher.
  - Verbesserte Referenzdokumentation und Korrektur mehrerer Eigenschaftsnamen.
- JavaScript
  - Unterstützung für das Objekt `Connection` hinzugefügt
  - Hinzugefügte Typdefinitionsdateien für JavaScript-Pakete
  - Anfangsunterstützung und Implementierung für Phrasenhinweise.
  - Rückgabe der Eigenschaftensammlung mit Dienst-JSON für die Erkennung.
- Windows-DLLs enthalten jetzt eine Versionsressource.
- Wenn Sie eine `FromEndpoint`-Erkennung erstellen, können Sie der Endpunkt-URL direkt Parameter hinzufügen. Mithilfe von `FromEndpoint` können Sie die Erkennung nicht über die Standardkonfigurationseigenschaften konfigurieren.

**Fehlerbehebungen**

- Leere Angaben für Proxybenutzername und Proxykennwort wurden nicht ordnungsgemäß behandelt. Wenn Sie in diesem Release den Proxybenutzernamen und das Proxykennwort auf eine leere Zeichenfolge festlegen, werden diese bei der Verbindungsherstellung mit dem Proxy nicht übermittelt.
- Vom SDK erstellte SessionId-Angaben wurden&nbsp;für einige Sprachen/Umgebungen nicht immer wirklich zufällig gewählt. Es wurde eine Initialisierung des Zufallsgenerators hinzugefügt, um dieses Problem zu beheben.
- Verbesserte Verarbeitung des Autorisierungstokens. Wenn Sie ein Autorisierungstoken verwenden möchten, geben Sie es in `SpeechConfig` an, und lassen Sie den Abonnementschlüssel leer. Erstellen Sie die Erkennung dann wie gewohnt.
- In einigen Fällen wurde das Objekt `Connection` nicht ordnungsgemäß freigegeben. Dieses Problem wurde behoben.
- Das JavaScript-Beispiel wurde korrigiert, um die Audioausgabe für die Übersetzungssynthese auch in Safari zu unterstützen.

## <a name="speech-sdk-121"></a>Speech SDK 1.2.1

Dieses Release gilt nur für JavaScript. Es wurden keine Features hinzugefügt. Die folgenden Fehler wurden behoben:

- Ende des Datenstroms wird bei turn.end und nicht bei speech.end ausgelöst.
- In Audiopump wurde der Fehler behoben, dass der nächste Sendevorgang nicht geplant wurde, wenn beim aktuellen Sendevorgang ein Fehler auftrat.
- Die kontinuierliche Erkennung mit Authentifizierungstoken wurde korrigiert.
- Programmfehlerbehebung für verschiedene Erkennungen/Endpunkte.
- Verbesserungen bei der Dokumentation.

## <a name="speech-sdk-120-2018-december-release"></a>Speech SDK 1.2.0: Release von Dezember 2018

**Neue Features**

- Python
  - Die Betaversion der Python-Unterstützung (ab 3.5) ist mit diesem Release verfügbar. Weitere Informationen finden Sie hier (quickstart-python.md).
- JavaScript
  - Das Speech SDK für JavaScript wird jetzt als Open-Source-Code bereitgestellt. Der Quellcode steht auf [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js)zur Verfügung.
  - Node.js wird jetzt unterstützt. Weitere Informationen finden Sie [hier](quickstart-js-node.md).
  - Die Längenbeschränkung für Audiositzungen wurde entfernt. Die Verbindungswiederherstellung erfolgt automatisch im Hintergrund.
- `Connection`-Objekt
  - Über `Recognizer` kann auf ein Objekt vom Typ `Connection` zugegriffen werden. Mit diesem Objekt können Sie die Dienstverbindung explizit initiieren und Verbindungsherstellungs- und Verbindungstrennungsereignisse abonnieren.
    (Für JavaScript und Python ist diese Funktion noch nicht verfügbar.)
- Unterstützung von Ubuntu 18.04
- Android
  - ProGuard-Unterstützung während der APK-Generierung aktiviert

**Verbesserungen**

- Verbesserungen bei der internen Threadverwendung (weniger Threads, Sperren, Mutexe)
- Verbesserte Fehlerberichterstellung/-informationen. In einigen Fällen wurden Fehlermeldungen nicht ordnungsgemäß verteilt.
- Entwicklungsabhängigkeiten in JavaScript wurden für die Verwendung aktueller Module aktualisiert.

**Fehlerbehebungen**

- Arbeitsspeicherverluste aufgrund eines Typenkonflikts in `RecognizeAsync` behoben
- In einigen Fällen sind Ausnahmen verloren gegangen.
- Behebung des Arbeitsspeicherverlusts in Übersetzungsereignisargumenten
- Sperrproblem bei der Verbindungswiederherstellung in langen Sitzungen behoben
- Problem behoben, dass dazu führen konnte, dass das Endergebnis für fehlerhafte Übersetzungen verpasst wird.
- C#: Wenn im Hauptthread nicht auf einen Vorgang vom Typ `async` gewartet wurde, konnte es vorkommen, dass die Erkennung vor Abschluss der asynchronen Aufgabe entfernt wurde.
- Java: Problem behoben, das zum Absturz des virtuellen Java-Computers geführt hat
- Objective-C: Enumerationszuordnung korrigiert. Anstelle von `RecognizingIntent` wurde „RecognizedIntent“ zurückgegeben.
- JavaScript: Standardausgabeformat in `SpeechConfig` auf „einfach“ festgelegt
- JavaScript: Beseitigung der Inkonsistenz zwischen Eigenschaften des Konfigurationsobjekts in JavaScript und anderen Sprachen

**Beispiele**

- Mehrere Beispiele aktualisiert und korrigiert (z.B. die Ausgabestimmen für die Übersetzung).
- Node.js-Beispiele zum [Beispielrepository](https://aka.ms/csspeech/samples) hinzugefügt

## <a name="speech-sdk-110"></a>Speech SDK 1.1.0

**Neue Features**

- Unterstützung für Android x86/x64.
- Proxyunterstützung: Im `SpeechConfig`-Objekt können Sie jetzt eine Funktion aufrufen, um die Proxyinformationen (Hostname, Port, Benutzername und Kennwort) festzulegen. Dieses Feature ist in iOS noch nicht verfügbar.
- Verbesserte Fehlercodes und Meldungen. Wenn eine Erkennung einen Fehler zurückgab, wurde dadurch bereits `Reason` (im abgebrochenen Ereignis) oder `CancellationDetails` (im Erkennungsergebnis) auf `Error` festgelegt. Das abgebrochene Ereignis enthält jetzt zwei zusätzliche Member: `ErrorCode` und `ErrorDetails`. Wenn der Server zusätzliche Fehlerinformationen mit dem Fehler zurückgibt, sind diese jetzt in den neuen Membern verfügbar.

**Verbesserungen**

- In der Konfiguration der Erkennung wurde eine zusätzliche Überprüfung hinzugefügt, und es wurde eine zusätzliche Fehlermeldung hinzugefügt.
- Die Verarbeitung von langen Pausen mitten in einer Audiodatei wurde verbessert.
- NuGet-Paket: Für .NET Framework-Projekte wird die Erstellung mit AnyCPU-Konfiguration verhindert.

**Fehlerbehebungen**

- In Erkennungen wurden verschiedene Ausnahmen behoben. Darüber hinaus werden Ausnahmen abgefangen und in Ereignisse vom Typ `Canceled` konvertiert.
- Ein Arbeitsspeicherverlust in der Eigenschaftenverwaltung wurde behoben.
- Es wurde ein Fehler behoben, bei dem eine Audioeingabedatei zum Absturz der Erkennung führen konnte.
- Es wurde ein Fehler behoben, bei dem nach dem Ereignis zum Beenden einer Sitzung weiter Ereignisse empfangen werden konnten.
- Einige Racebedingungen im Threading wurden korrigiert.
- Ein iOS-Kompatibilitätsproblem wurde behoben, das zu einem Absturz führen konnte.
- Verbesserungen bei der Stabilität für die Android-Mikrofonunterstützung.
- Es wurde ein Fehler behoben, bei dem eine Erkennung in JavaScript die Erkennungssprache ignorierte.
- Es wurde ein Fehler behoben, der (in einigen Fällen) das Festlegen von `EndpointId` in JavaScript verhinderte.
- Die Parameterreihenfolge in AddIntent in JavaScript wurde geändert, und es wurde eine fehlende JavaScript-Signatur für `AddIntent` hinzugefügt.

**Beispiele**

- Dem [Beispielrepository](https://aka.ms/csspeech/samples) wurden C++- und C#-Beispiele für die Verwendung von Pull- und Pushstreams hinzugefügt.

## <a name="speech-sdk-101"></a>Speech SDK 1.0.1

Verbesserte Zuverlässigkeit und Fehlerbehebungen:

- Ein potenziell schwerwiegender Fehler aufgrund einer Racebedingung bei der Löscherkennung wurde behoben.
- Ein potenziell schwerwiegender Fehler bei einer nicht festgelegten Eigenschaft wurde behoben.
- Zusätzliche Fehler- und Parameterüberprüfungen wurden hinzugefügt.
- Objective-C: Ein potenziell schwerwiegender Fehler durch Namensüberschreibungen in NSString wurde behoben.
- Objective-C: Sichtbarkeit der API wurde angepasst
- JavaScript: Korrektur in Bezug auf Ereignisse und deren Nutzlasten.
- Verbesserungen bei der Dokumentation.

Im [Beispielrepository](https://aka.ms/csspeech/samples) wurde ein neues Beispiel für JavaScript hinzugefügt.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>Cognitive Services Speech SDK 1.0.0: Release von September 2018

**Neue Features**

- Unterstützung für Objective-C unter iOS. Sehen Sie sich unseren [Objective-C-Schnellstart für iOS](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone-langs/objectivec-ios.md) an.
- Unterstützung für JavaScript im Browser. Sehen Sich unseren [JavaScript-Schnellstart](quickstart-js-browser.md) an.

**Wichtige Änderungen**

- Mit diesem Release werden einige Breaking Changes eingeführt.
  Ausführliche Informationen finden Sie auf [dieser Seite](https://aka.ms/csspeech/breakingchanges_1_0_0).

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Cognitive Services Speech SDK 0.6.0: Release von August 2018

**Neue Features**

- Mit dem Speech SDK erstellte UWP-Apps erfüllen nun die Anforderungen des Windows App Certification Kit (WACK).
  Sehen Sie sich den [UWP-Schnellstart](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp&tabs=uwp) an.
- Unterstützung für .NET Standard 2.0 unter Linux (Ubuntu 16.04 x 64)
- Experimentell: Unterstützung für Java 8 unter Windows (64 Bit) und Linux (Ubuntu 16.04 x64).
  Sehen Sie sich den [Schnellstart zur Java Runtime Environment](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=jre) an.

**Funktionale Änderung**

- Es werden weitere Detailinformationen zu Verbindungsfehlern verfügbar gemacht.

**Wichtige Änderungen**

- In Java (Android) erfordert die `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate`-Funktion keinen Path-Parameter mehr. Der Pfad wird nun auf allen unterstützten Plattformen automatisch erkannt.
- Der get-Accessor der `EndpointUrl`-Eigenschaft in Java und C# wurde entfernt.

**Fehlerbehebungen**

- In Java werden die Ergebnisse der Audiosynthese in der Übersetzungserkennung jetzt implementiert.
- Ein Problem wurde behoben, das inaktive Threads und eine erhöhte Anzahl von offenen und nicht verwendeten Sockets verursachen konnte.
- Ein Problem wurde behoben, das dazu führen konnte, dass lange ausgeführte Erkennungen während der Übertragung beendet wurden.
- Eine Racebedingung beim Herunterfahren der Erkennung wurde behoben.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Cognitive Services Speech SDK 0.5.0: Release von Juli 2018

**Neue Features**

- Unterstützung für Android-Plattform (API 23: Android 6.0 Marshmallow oder höher). Sehen Sie sich den [Android-Schnellstart](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=android) an.
- Unterstützung für .NET Standard 2.0 unter Windows. Sehen Sie sich den [.NET Core-Schnellstart](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore) an.
- Experimentell: Unterstützung für UWP unter Windows (Version 1709 oder höher).
  - Sehen Sie sich den [UWP-Schnellstart](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp) an.
  - Hinweis: Mit dem Speech SDK erstellte UWP-Apps erfüllen die Anforderungen des Windows App Certification Kit (WACK) noch nicht.
- Unterstützung einer lang andauernden Erkennung mit automatischer erneuter Verbindungsherstellung.

**Funktionale Änderungen**

- `StartContinuousRecognitionAsync()` unterstützt eine lang andauernde Erkennung.
- Das Erkennungsergebnis enthält mehr Felder. Versatz vom Audiobeginn und Dauer (beides in Takten) des erkannten Texts und weitere Werte, die den Erkennungsstatus darstellen, z.B. `InitialSilenceTimeout` und `InitialBabbleTimeout`.
- Unterstützung für AuthorizationToken zum Erstellen von Factoryinstanzen.

**Wichtige Änderungen**

- Erkennungsereignisse: Der `NoMatch`-Ereignistyp wurde mit dem `Error`-Ereignis zusammengeführt.
- SpeechOutputFormat in C# wurde in `OutputFormat` umbenannt, um mit C++ konsistent zu bleiben.
- Der Rückgabetyp einiger Methoden der `AudioInputStream`-Schnittstelle wurde geringfügig geändert:
  - In Java gibt die `read`-Methode jetzt `long` anstelle von `int` zurück.
  - In C# gibt die `Read`-Methode jetzt `uint` anstelle von `int` zurück.
  - In C++ geben die `Read`- und die `GetFormat`-Methoden jetzt `size_t` anstelle von `int` zurück.
- C++: Instanzen von Audioeingabestreams können jetzt nur als `shared_ptr` übergeben werden.

**Fehlerbehebungen**

- Korrektur falscher Rückgabewerte im Ergebnis, wenn bei `RecognizeAsync()` ein Timeout auftritt.
- Die Abhängigkeit von Media Foundation-Bibliotheken für Windows wurde entfernt. Das SDK verwendet jetzt die Core Audio-APIs.
- Korrektur der Dokumentation: Eine Seite [Regionen](regions.md) wurde hinzugefügt, um die unterstützten Regionen zu beschreiben.

**Bekanntes Problem**

- Das Speech SDK für Android meldet die Ergebnisse der Sprachsynthese für Übersetzungen nicht. Dieses Problem wird im nächsten Release behoben.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Cognitive Services Speech SDK 0.4.0: Release von Juni 2018

**Funktionale Änderungen**

- AudioInputStream

  Eine Erkennung kann jetzt einen Stream als Audioquelle nutzen. Weitere Informationen finden Sie in der zugehörigen [Schrittanleitung](how-to-use-audio-input-streams.md).

- Detailliertes Ausgabeformat

  Beim Erstellen von `SpeechRecognizer` können Sie das Ausgabeformat `Detailed` oder `Simple` anfordern. `DetailedSpeechRecognitionResult` enthält eine Zuverlässigkeitsbewertung, erkannten Text, eine lexikalische Rohform, eine normalisierte Form und eine normalisierte Form mit maskierten anstößigen Ausdrücken.

**Wichtige Änderung**

- Änderung von `SpeechRecognitionResult.RecognizedText` in `SpeechRecognitionResult.Text` in C#.

**Fehlerbehebungen**

- Ein mögliches Rückrufproblem auf USP-Ebene beim Herunterfahren wurde behoben.
- Wenn eine Audioeingabedatei von einer Erkennung genutzt wurde, wurde das Dateihandle länger als erforderlich gespeichert.
- Mehrere Deadlocks zwischen dem Nachrichtensystem und der Erkennung wurden entfernt.
- Ein `NoMatch`-Ergebnis wird ausgelöst, wenn bei der Antwort vom Dienst ein Timeout auftritt.
- Die Media Foundation-Bibliotheken unter Windows werden verzögert geladen. Diese Bibliothek ist nur für die Mikrofoneingabe erforderlich.
- Die Uploadgeschwindigkeit für Audiodaten ist auf das Doppelte der ursprünglichen Audiogeschwindigkeit beschränkt.
- C# .NET-Assemblys haben unter Windows nun einen starken Namen.
- Korrektur der Dokumentation: `Region` ist eine erforderliche Information zum Erstellen einer Erkennung.

Weitere Beispiele wurden hinzugefügt und werden regelmäßig aktualisiert. Die Sammlung der aktuellsten Beispiele finden Sie im [GitHub-Repository mit Beispielen für das Speech SDK](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Cognitive Services Speech SDK 0.2.12733: Release von Mai 2018

Dieses Release ist erste öffentliche Vorschauversion des Cognitive Services Speech SDK.
