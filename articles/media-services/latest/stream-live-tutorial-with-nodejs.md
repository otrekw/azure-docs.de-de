---
title: 'Livestreaming mit Media Services v3: Node.js'
titleSuffix: Azure Media Services
description: Hier finden Sie eine Anleitung zum Livestreaming mit Node.js.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc, devx-track-nodejs
ms.date: 04/15/2021
ms.author: inhenkel
ms.openlocfilehash: 749d2fc845f036a2802c80c161b3fc8c171c2555
ms.sourcegitcommit: 6f1aa680588f5db41ed7fc78c934452d468ddb84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107730208"
---
# <a name="tutorial-stream-live-with-media-services-using-nodejs-and-typescript"></a>Tutorial: Livestreaming mit Media Services, Node.js und TypeScript

> [!NOTE]
> In diesem Tutorial werden Node.js-Beispiele verwendet. Die allgemeinen Schritte sind jedoch auch bei Verwendung der [REST-API](/rest/api/media/liveevents), der [CLI](/cli/azure/ams/live-event) oder anderer unterstützter [SDKs](media-services-apis-overview.md#sdks) gleich. 

In Azure Media Services sind [Liveereignisse](/rest/api/media/liveevents) für die Verarbeitung von Livestreaminginhalten zuständig. Ein Liveereignis stellt einen Eingabeendpunkt (Erfassungs-URL) bereit, den Sie dann für einen Liveencoder bereitstellen. Das Liveereignis empfängt Live-Eingabestreams aus dem Liveencoder und stellt diese zum Streamen durch einen oder mehrere [Streamingendpunkte](/rest/api/media/streamingendpoints) zur Verfügung. Zudem stellen Liveereignisse einen Vorschauendpunkt (Vorschau-URL) bereit, mit dem Sie eine Vorschau des Streams anzeigen und überprüfen können, bevor Sie ihn weiter verarbeiten und übermitteln. In diesem Tutorial erfahren Sie, wie Sie unter Verwendung von Node.js ein Liveereignis vom Typ **Passthrough** erstellen und mit [OBS Studio](https://obsproject.com/download) einen Livestream dafür übertragen.

Das Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Herunterladen des in diesem Thema beschriebenen Beispiel-Codes
> * Untersuchen des Codes zum Konfigurieren und Durchführen des Livestreamings
> * Wiedergabe des Ereignisses mit [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) unter [https://ampdemo.azureedge.net](https://ampdemo.azureedge.net).
> * Bereinigen der Ressourcen


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Installieren Sie [Node.js](https://nodejs.org/en/download/).
- Installieren Sie [TypeScript](https://www.typescriptlang.org/).
- [Erstellen Sie ein Media Services-Konto.](./create-account-howto.md)<br/>Merken Sie sich die Werte, die Sie für den Namen der Ressourcengruppe und des Media Services-Kontos verwendet haben.
- Führen Sie die Schritte unter [Zugreifen auf die Azure Media Services-API mit der Azure CLI](./access-api-howto.md) aus, und speichern Sie die Anmeldeinformationen. Sie werden für den Zugriff auf die API sowie zum Konfigurieren Ihrer Umgebungsvariablendatei benötigt.
- Lesen Sie sich zunächst die Anleitung zum [Konfigurieren und Herstellen einer Verbindung mit Node.js](./configure-connect-nodejs-howto.md) durch, um sich mit der Verwendung des Node.js-Client-SDK vertraut zu machen.
- Installieren Sie Visual Studio Code oder Visual Studio.
- [Richten Sie Ihre Visual Studio Code-Umgebung ein](https://code.visualstudio.com/Docs/languages/typescript), um die TypeScript-Sprache zu unterstützen.

## <a name="additional-settings-for-live-streaming-software"></a>Zusätzliche Einstellungen für Livestreamingsoftware

- Eine Kamera oder ein Gerät (beispielsweise ein Laptop) zum Übertragen eines Ereignisses.
- Ein lokaler Softwareencoder, der Ihren Kameradatenstrom codiert und unter Verwendung des RTM-Protokolls an den Media Services-Livestreamingdienst sendet (siehe [Überprüfte lokale Livestreamingencoder](encode-recommended-on-premises-live-encoders.md)). Der Datenstrom muss das Format **RTMP** oder **Smooth Streaming** haben.  
- Für dieses Beispiel empfiehlt es sich, mit einem Softwareencoder wie [Open Broadcast Software OBS Studio](https://obsproject.com/download) (kostenlos) zu beginnen, um den Einstieg zu erleichtern.

In diesem Beispiel wird davon ausgegangen, dass Sie OBS Studio verwenden, um RTMP an den Erfassungsendpunkt zu übertragen. Installieren Sie zunächst OBS Studio.
Verwenden Sie in OBS Studio die folgenden Codierungseinstellungen:

- Encoder: NVIDIA NVENC (sofern verfügbar) oder x264
- Ratensteuerung: CBR
- Bitrate: 2.500 KBit/s (oder eine andere sinnvolle Bitrate für Ihren Laptop)
- Keyframe-Intervall: zwei Sekunden (oder eine Sekunde für geringe Wartezeit)  
- Voreinstellung: „Low-Latency Quality“ (Geringe Wartezeit: Qualität) oder „Low-Latency Performance“ (Geringe Wartezeit: Leistung) bei Verwendung von NVENC oder „veryfast“ (sehr schnell) bei Verwendung von x264
- Profil: „high“ (hoch)
- GPU: 0 (Auto)
- Max. B-Frames: 2

> [!TIP]
> Lesen Sie [Live streaming with Azure Media Services v3](stream-live-streaming-concept.md) (Livestreaming mit Azure Media Services v3), bevor Sie mit diesem Tutorial fortfahren.

## <a name="download-and-configure-the-sample"></a>Herunterladen und Konfigurieren des Beispiels

Klonen Sie auf Ihrem Computer das folgende GitHub-Repository mit dem Livestreamingbeispiel für Node.js. Verwenden Sie dazu den folgenden Befehl:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-node-tutorials.git
 ```

Das Livestreamingbeispiel befindet sich im Ordner [Live](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/main/AMSv3Samples/Live).

Kopieren Sie im Ordner [AMSv3Samples](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/main/AMSv3Samples) die Datei „sample.env“ in eine neue Datei namens „.env“. Diese Datei dient zum Speichern Ihrer Umgebungsvariableneinstellungen, die Sie im Artikel [Abrufen von Anmeldeinformationen für den Zugriff auf die Media Services-API](./access-api-howto.md) erfasst haben.
Achten Sie darauf, dass der Dateiname den Punkt (.) vor „env“ enthält, damit das Codebeispiel ordnungsgemäß funktioniert.

Die [ENV-Datei](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/main/AMSv3Samples/sample.env) enthält Ihren AAD-Anwendungsschlüssel und Ihr Geheimnis sowie den Kontonamen und Abonnementinformationen, die zum Authentifizieren des SDK-Zugriffs auf Ihr Media Services-Konto erforderlich sind. Die GITIGNORE-Datei ist bereits so konfiguriert, dass diese Datei nicht in Ihrem geforkten Repository veröffentlicht wird. Achten Sie darauf, dass diese Anmeldeinformationen nicht kompromittiert werden, da es sich hierbei um wichtige Geheimnisse für Ihr Konto handelt.

> [!IMPORTANT]
> In diesem Beispiel wird für jede Ressourcen ein eindeutiges Suffix verwendet. Wenn Sie das Debuggen abbrechen oder die App beenden, ohne das Beispiel vollständig zu durchlaufen, werden in Ihrem Konto mehrere Liveereignisse generiert. <br/>Die aktiven Liveereignisse müssen unbedingt beendet werden. Andernfalls **fallen für die Ereignisse Kosten an**! Am Ende der Programmausführung werden die Ressourcen automatisch bereinigt. Falls das Programm abstürzt oder Sie versehentlich den Debugger beenden und dadurch die Programmausführung abgebrochen wird, vergewissern Sie sich im Portal, dass keine Liveereignisse im Ausführungs- oder Standby-Zustand vorhanden sind, um unerwünschte Abrechnungsgebühren zu vermeiden.

## <a name="examine-the-typescript-code-for-live-streaming"></a>Untersuchen des TypeScript-Codes für Livestreaming

In diesem Abschnitt werden die in der Datei [index.ts](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/main/AMSv3Samples/Live/index.ts) des Projekts *Live* definierten Funktionen untersucht.

Das Beispiel erstellt für jede Ressource ein eindeutiges Suffix, damit keine Namenskonflikte auftreten, wenn Sie das Beispiel ohne Bereinigung der Ressourcen mehrmals ausführen.

### <a name="start-using-media-services-sdk-for-nodejs-with-typescript"></a>Erste Schritte mit dem Media Services SDK für Node.js mit TypeScript

Um Media Services-APIs mit Node.js verwenden zu können, muss zunächst das SDK-Modul [@azure/arm-mediaservices](https://www.npmjs.com/package/@azure/arm-mediaservices) mithilfe des Paket-Managers npm hinzugefügt werden.

```bash
npm install @azure/arm-mediaservices
```

In „package.json“ ist dies bereits für Sie konfiguriert. Sie müssen daher nur noch *npm install* ausführen, um die Module und Abhängigkeiten zu laden.

1. Öffnen Sie eine **Eingabeaufforderung**, und navigieren Sie zum Verzeichnis für das Beispiel.
1. Wechseln Sie zum Ordner „AMSv3Samples“.

    ```bash
    cd AMSv3Samples
    ```

1. Installieren Sie die Pakete, die in der Datei *packages.json* verwendet werden.

    ```bash
    npm install 
    ```

1. Starten Sie Visual Studio Code aus dem Ordner *AMSv3Samples*. (Dies ist erforderlich, damit das Starten aus dem Ordner erfolgt, in dem sich der Ordner *.vscode* und die *tsconfig.json*-Dateien befinden.)

    ```bash
    cd ..
    code .
    ```

Öffnen Sie den Ordner für *Live*, und öffnen Sie die Datei *index.ts* im Visual Studio Code-Editor.
Drücken Sie bei geöffneter Datei *index.ts* die Taste F5, um den Debugger zu starten.

### <a name="create-the-media-services-client"></a>Erstellen des Media Services-Clients

Im folgenden Codeausschnitt wird die Erstellung des Media Services-Clients in Node.js gezeigt.
Wie Sie sehen, wird in diesem Code zuerst die Eigenschaft **longRunningOperationRetryTimeout** von „AzureMediaServicesOptions“ auf zwei Sekunden festgelegt, um die Zeit zu verringern, die benötigt wird, um den Status eines zeitintensiven Vorgangs am Azure-Ressourcenverwaltungsendpunkt abzurufen.  Da die meisten Vorgänge für Liveereignisse asynchron sind und einige Zeit dauern können, empfiehlt es sich, dieses standardmäßig auf 30 Sekunden festgelegte Abrufintervall für das SDK zu verringern, um wichtige Vorgänge wie das Erstellen von Liveereignissen oder das Starten und Beenden (jeweils asynchrone Aufrufe) zu beschleunigen. Für die meisten Anwendungsfallszenarien wird ein Wert von zwei Sekunden empfohlen.

[!code-typescript[Main](../../../media-services-v3-node-tutorials/AMSv3Samples/Live/index.ts#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Erstellen eines Liveereignisses

In diesem Abschnitt erfahren Sie, wie Sie ein Liveereignis vom Typ **Pass-Through** erstellen. („LiveEventEncodingType“ ist in diesem Fall auf „None“ festgelegt.) Weitere Informationen zu den anderen verfügbaren Arten von Liveereignissen finden Sie unter [Liveereignistypen](live-event-outputs-concept.md#live-event-types). Neben Passthrough können Sie ein Liveereignis mit Livetranscodierung für eine Cloudcodierung mit adaptiver Bitrate (720p oder 1080p) verwenden.
 
Beim Erstellen des Liveereignisses können Sie folgende Punkte angeben:

* Erfassungsprotokoll für das Liveereignis (aktuell unterstützte Protokolle: RTMP(S) und Smooth Streaming).<br/>Die Protokolloption kann nicht geändert werden, während das Liveereignis oder die zugehörigen Liveausgaben aktiv sind. Sollten Sie verschiedene Protokolle benötigen, erstellen Sie für jedes Streamingprotokoll ein separates Liveereignis.  
* IP-Einschränkungen für Erfassung und Vorschau. Sie können die IP-Adressen definieren, die ein Video für dieses Liveereignis erfassen dürfen. Zulässige IP-Adressen können als einzelne IP-Adresse (Beispiel: 10.0.0.1), als IP-Adressbereiche mit einer IP-Adresse und einer CIDR-Subnetzmaske (Beispiel: 10.0.0.1/22) oder als IP-Adressbereiche mit einer IP-Adresse und einer Subnetzmaske in Punkt-Dezimalschreibweise (Beispiel: 10.0.0.1(255.255.252.0)) angegeben werden.<br/>Wenn keine IP-Adressen angegeben sind und es keine Regeldefinition gibt, sind keine IP-Adressen zulässig. Um alle IP-Adressen zuzulassen, erstellen Sie eine Regel und legen 0.0.0.0/0 fest.<br/>Die IP-Adressen müssen in einem der folgenden Formate vorliegen: IPv4-Adresse mit vier Ziffern oder CIDR-Adressbereich
* Bei der Ereigniserstellung können Sie angeben, dass das Ereignis automatisch gestartet werden soll. <br/>Wenn für den automatischen Start „true“ festgelegt ist, wird das Liveereignis nach der Erstellung gestartet. Dies bedeutet, dass die Abrechnung beginnt, sobald das Liveereignis startet. Sie müssen für die Liveereignisressource explizit „Beenden“ auswählen, damit keine Gebühren mehr anfallen. Weitere Informationen finden Sie im Abschnitt [LiveEvent-Zustandswerte und Abrechnung](live-event-states-billing-concept.md).
Es stehen auch Standbymodi zur Verfügung, um das Liveereignis in einem preisgünstigeren „zugeordneten“ Zustand zu starten, der einen schnelleren Wechsel zu einem Ausführungszustand ermöglicht. Dies ist etwa im Falle von Pools der heißen Ebene hilfreich, die schnell Kanäle für Streamer bereitstellen müssen.
* Legen Sie die Eigenschaft „useStaticHostname“ auf „true“ fest, und verwenden Sie eine benutzerdefinierte eindeutige GUID in „accessToken“, um eine Erfassungs-URL zu erhalten, die vorhersagbar und in einem hardwarebasierten Liveencoder einfacher zu verwalten ist. Ausführliche Informationen finden Sie unter [Erfassungs-URLs für Liveereignisse](live-event-outputs-concept.md#live-event-ingest-urls).

[!code-typescript[Main](../../../media-services-v3-node-tutorials/AMSv3Samples/Live/index.ts#CreateLiveEvent)]

### <a name="create-an-asset-to-record-and-archive-the-live-event"></a>Erstellen eines Medienobjekts zum Aufzeichnen und Archivieren des Liveereignisses

In diesem Codeblock wird ein leeres Medienobjekt als „Videokassette“ zum Aufzeichnen Ihres Liveereignisarchivs erstellt.
Das Medienobjekt können Sie sich als Videokassette vorstellen, wie sie früher einmal in einen Videorekorder eingelegt wurde. Die Liveausgabe ist der Videorekorder selbst. Das Liveereignis ist das vom Videorekorder ausgegebene Videosignal.

Das Medienobjekt (die „Videokassette“) kann zu einem beliebigen Zeitpunkt erstellt werden. Sie ist lediglich ein leeres Medienobjekt, das an das Objekt für die Liveausgabe (in dieser Analogie: der Videorekorder) übergeben wird.

[!code-typescript[Main](../../../media-services-v3-node-tutorials/AMSv3Samples/Live/index.ts#CreateAsset)]

### <a name="create-the-live-output"></a>Erstellen der Liveausgabe

In diesem Abschnitt wird eine Liveausgabe erstellt, die den Medienobjektnamen als Eingabe verwendet, um anzugeben, wo das Liveereignis aufgezeichnet werden soll. Darüber hinaus wird das Timeshift-Fenster (DVR) für die Aufzeichnung eingerichtet.
Im Beispielcode wird die Einrichtung eines Timeshift-Fensters von einer Stunde gezeigt. Dadurch können Clients die letzte Stunde des Ereignisses ab einer beliebigen Stelle wiedergeben.  Darüber hinaus wird nur die letzte Stunde des Liveereignisses im Archiv gespeichert. Dieser Wert kann auf bis zu 25 Stunden erhöht werden.  Beachten Sie außerdem, dass Sie die Benennung des Ausgabemanifests steuern können, die nach der Veröffentlichung für die HLS- und DASH-Manifeste in Ihren URL-Pfaden verwendet wird.

Die Liveausgabe (in dieser Analogie: der Videorekorder) kann ebenfalls zu einem beliebigen Zeitpunkt erstellt werden. Sie können also eine Liveausgabe vor oder nach dem Starten der Signalübertragung erstellen. Wenn es schnell gehen muss, empfiehlt es sich häufig, sie vor dem Starten der Signalübertragung zu erstellen.

Liveausgaben werden bei der Erstellung gestartet und beim Löschen beendet.  Wenn Sie die Liveausgabe löschen, bleiben das zugrunde liegende Medienobjekt und dessen Inhalt erhalten. Der Vorgang ist mit dem Auswerfen der Videokassette vergleichbar. Das Medienobjekt mit der Aufzeichnung kann beliebig lange erhalten bleiben, und nach dem Auswerfen (also nach dem Löschen der Liveausgabe) steht es umgehend für die On-Demand-Wiedergabe zur Verfügung.

[!code-typescript[Main](../../../media-services-v3-node-tutorials/AMSv3Samples/Live/index.ts#CreateLiveOutput)]


### <a name="get-ingest-urls"></a>Abrufen von Erfassungs-URLs

Sobald das Liveereignis erstellt wurde, können Sie Erfassungs-URLs abrufen, die Sie dem Liveencoder bereitstellen. Diese URLs werden vom Encoder zur Eingabe eines Livestreams mit dem RTMP-Protokoll verwendet.

[!code-typescript[Main](../../../media-services-v3-node-tutorials/AMSv3Samples/Live/index.ts#GetIngestURL)]

### <a name="get-the-preview-url"></a>Abrufen der Vorschau-URL

Verwenden Sie den Vorschauendpunkt (previewEndpoint), um eine Vorschau anzuzeigen und sich zu vergewissern, dass die Eingabe des Encoders auch tatsächlich empfangen wird.

> [!IMPORTANT]
> Vergewissern Sie sich, dass das Video an die Vorschau-URL übertragen wird, bevor Sie fortfahren.

[!code-typescript[Main](../../../media-services-v3-node-tutorials/AMSv3Samples/Live/index.ts#GetPreviewURL)]

### <a name="create-and-manage-live-events-and-live-outputs"></a>Erstellen und Verwalten von Liveereignissen und Liveausgaben

Sobald der Datenstrom bei Ihrem Liveereignis eingeht, können Sie das Streamingereignis starten, indem Sie einen Streaminglocator für Ihre Clientplayer veröffentlichen. Dadurch wird der Datenstrom über den Streamingendpunkt für die Zuschauer verfügbar gemacht.

Als Erstes erstellen Sie das Signal, indem Sie das Liveereignis erstellen.  Das Signal wird erst übertragen, wenn Sie dieses Liveereignis starten und Ihren Encoder mit der Eingabe verbinden.

Zum Anhalten des „Videorekorders“ muss „delete“ für die Liveausgabe (LiveOutput) aufgerufen werden. Dadurch wird lediglich der „Videorekorder“ gelöscht und die Archivierung beendet. Der **Inhalt** Ihres Archivs auf der „Videokassette“ (Medienobjekt) wird dagegen nicht gelöscht. Das Medienobjekt bleibt mit dem archivierten Videoinhalt erhalten, bis Sie „delete“ explizit für das Medienobjekt aufrufen. Nach dem Löschen der Liveausgabe (LiveOutput) kann der aufgezeichnete Inhalt des Medienobjekts weiterhin über alle bereits veröffentlichten Streaminglocator-URLs wiedergegeben werden. Sollen Kunden den archivierten Inhalt nicht mehr wiedergeben können, müssen Sie zunächst alle Locators aus dem Medienobjekt entfernen und auch den CDN-Cache für den URL-Pfad leeren, falls Sie ein CDN für die Übermittlung verwenden. Andernfalls bleibt der Inhalt für den Zeitraum, der im CDN als Standardgültigkeitsdauer festgelegt ist (bis zu 72 Stunden), im CDN-Cache erhalten.

#### <a name="create-a-streaming-locator-to-publish-hls-and-dash-manifests"></a>Erstellen eines Streaminglocators zum Veröffentlichen von HLS- und DASH-Manifesten

> [!NOTE]
> Beim Erstellen Ihres Media Services-Kontos wird dem Konto ein **Standard**-Streamingendpunkt im Zustand **Beendet** hinzugefügt. Um mit dem Streamen Ihrer Inhalte zu beginnen und die [dynamische Paketerstellung](encode-dynamic-packaging-concept.md) und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Zustand **Wird ausgeführt** aufweisen.

Wenn Sie das Medienobjekt mit einem Streaminglocator veröffentlicht haben, ist das Liveereignis (bis zur DVR-Fensterlänge) weiterhin bis zum Ablauf oder zur Löschung des Streaminglocators sichtbar (je nachdem, was zuerst eintritt). Auf diese Weise können Sie die virtuelle Aufzeichnung so verfügbar machen, dass sie von Ihrer Zielgruppe live und nach Bedarf angesehen werden kann. Die gleiche URL kann verwendet werden, um das Liveereignis, das DVR-Fenster oder das On-Demand-Medienobjekt anzusehen, wenn die Aufzeichnung abgeschlossen ist (nach dem Löschen der Liveausgabe).

[!code-typescript[Main](../../../media-services-v3-node-tutorials/AMSv3Samples/Live/index.ts#CreateStreamingLocator)]

#### <a name="build-the-paths-to-the-hls-and-dash-manifests"></a>Erstellen der Pfade zu den HLS- und DASH-Manifesten

Die Methode „BuildManifestPaths“ im Beispiel zeigt die deterministische Erstellung der Streamingpfade für die DASH- oder HLS-Übermittlung an verschiedene Clients und Playerframeworks.

[!code-typescript[Main](../../../media-services-v3-node-tutorials/AMSv3Samples/Live/index.ts#BuildManifestPaths)]

## <a name="watch-the-event"></a>Ansehen des Ereignisses

Kopieren Sie zur Wiedergabe des Ereignisses die Streaming-URL, die Sie beim Ausführen des Codes in „Erstellen eines Streaminglocators“ erhalten haben. Sie können einen Medienplayer Ihrer Wahl verwenden. Um Ihren Stream zu testen, können Sie den [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) unter https://ampdemo.azureedge.net verwenden.

Das Liveereignis konvertiert Ereignisse automatisch in On-Demand-Inhalt, wenn es beendet wird. Auch nach dem Beenden und Löschen des Ereignisses können die Benutzer archivierte Inhalte als Video auf Abruf streamen, solange Sie das Medienobjekt nicht löschen. Ein Medienobjekt kann nicht gelöscht werden, wenn es von einem Ereignis verwendet wird. Zuerst muss das betreffende Ereignis gelöscht werden.

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Bereinigen der Ressourcen in Ihrem Media Services-Konto

Wenn Sie die Anwendung bis zum Ende ausführen, werden in der Funktion „cleanUpResources“ automatisch alle verwendeten Ressourcen bereinigt. Stellen Sie sicher, dass die Anwendung bzw. der Debugger bis zum Ende ausgeführt wird. Andernfalls kommt es womöglich zu einem Ressourcenleck sowie zu aktiven Liveereignissen in Ihrem Konto. Vergewissern Sie sich im Azure-Portal, dass alle Ressourcen in Ihrem Media Services-Konto bereinigt wurden.  

Sehen Sie sich im Beispielcode die Methode **cleanUpResources** an, um weitere Details zu erhalten.

> [!IMPORTANT]
> Wenn Sie das Liveereignis nicht beenden, fallen weiter Kosten dafür an. Falls das Projekt/Programm abstürzt oder aus einem anderen Grund geschlossen wird, bleibt das Liveereignis unter Umständen aktiv und verursacht weitere Kosten.

## <a name="ask-questions-give-feedback-get-updates"></a>Fragen stellen, Feedback geben, Updates abrufen

Im Artikel [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services zu bekommen.

## <a name="more-developer-documentation-for-nodejs-on-azure"></a>Weitere Entwicklerdokumentation für Node.js in Azure

- [Azure für JavaScript- und Node.js-Entwickler](/azure/developer/javascript/)
- [Media Services-Quellcode im GitHub-Repository @azure/azure-sdk-for-js](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/mediaservices/arm-mediaservices)
- [Dokumentation zum Azure-Paket für Node.js-Entwickler](/javascript/api/overview/azure/)


## <a name="next-steps"></a>Nächste Schritte

[Streamen von Dateien](stream-files-tutorial-with-api.md)