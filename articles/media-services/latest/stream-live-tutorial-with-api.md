---
Titel: Livestreaming mit Media Services v3 : Azure Media Services description: Informationen zum Livestreaming mit Azure Media Services v3
services: media-services documentationcenter: '' author: IngridAtMicrosoft manager: femila editor: ''

ms.service: media-services ms.workload: media ms.tgt_pltfrm: na ms.devlang: na ms.topic: tutorial ms.custom: "mvc, devx-track-csharp" ms.date: 06/13/2019 ms.author: inhenkel

---

# <a name="tutorial-stream-live-with-media-services"></a>Tutorial: Livestreaming mit Media Services

> [!NOTE]
> Auch wenn in diesem Tutorial die [.NET SDK](/dotnet/api/microsoft.azure.management.media.models.liveevent)-Beispiele verwendet werden, sind die allgemeinen Schritte für die [REST-API](/rest/api/media/liveevents), die [CLI](/cli/azure/ams/live-event) oder für andere unterstützte [SDKs](media-services-apis-overview.md#sdks) identisch. 

In Azure Media Services sind [Liveereignisse](/rest/api/media/liveevents) für die Verarbeitung von Livestreaminginhalten zuständig. Ein Liveereignis stellt einen Eingabeendpunkt (Erfassungs-URL) bereit, den Sie dann für einen Liveencoder bereitstellen. Das Liveereignis empfängt Live-Eingabestreams aus dem Liveencoder und stellt diese zum Streamen durch einen oder mehrere [Streamingendpunkte](/rest/api/media/streamingendpoints) zur Verfügung. Zudem stellen Liveereignisse einen Vorschauendpunkt (Vorschau-URL) bereit, mit dem Sie eine Vorschau des Streams anzeigen und überprüfen können, bevor Sie ihn weiter verarbeiten und übermitteln. In diesem Tutorial erfahren Sie, wie Sie unter Verwendung von .NET Core ein Liveereignis vom Typ **Pass-Through** erstellen.

Das Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Herunterladen der in diesem Thema beschriebenen Beispiel-App
> * Untersuchen des Codes für das Livestreaming
> * Wiedergabe des Ereignisses mit [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) unter [https://ampdemo.azureedge.net](https://ampdemo.azureedge.net).
> * Bereinigen der Ressourcen

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Installieren Sie Visual Studio Code oder Visual Studio.
- [Erstellen Sie ein Media Services-Konto.](./account-create-how-to.md)<br/>Wichtig: Kopieren Sie die API-Zugriffsdetails im JSON-Format, oder speichern Sie die Werte, die zum Herstellen einer Verbindung mit dem Media Services-Konto benötigt werden, im ENV-Dateiformat, das in diesem Beispiel verwendet wird.
- Führen Sie die Schritte unter [Zugreifen auf die Azure Media Services-API mit der Azure CLI](./access-api-howto.md) aus, und speichern Sie die Anmeldeinformationen. Sie müssen für den Zugriff auf die API in diesem Beispiel verwendet oder im ENV-Dateiformat eingegeben werden. 
- Eine Kamera oder ein Gerät (beispielsweise ein Laptop) zum Übertragen eines Ereignisses.
- Ein lokaler Softwareencoder, der Ihren Kameradatenstrom codiert und unter Verwendung des RTM-Protokolls an den Media Services-Livestreamingdienst sendet (siehe [Überprüfte lokale Livestreamingencoder](encode-recommended-on-premises-live-encoders.md)). Der Datenstrom muss das Format **RTMP** oder **Smooth Streaming** haben.  
- Für dieses Beispiel empfiehlt es sich, mit einem Softwareencoder wie [Open Broadcast Software OBS Studio](https://obsproject.com/download) (kostenlos) zu beginnen, um den Einstieg zu erleichtern. 

> [!TIP]
> Lesen Sie [Live streaming with Azure Media Services v3](stream-live-streaming-concept.md) (Livestreaming mit Azure Media Services v3), bevor Sie mit diesem Tutorial fortfahren. 

## <a name="download-and-configure-the-sample"></a>Herunterladen und Konfigurieren des Beispiels

Klonen Sie auf Ihrem Computer das folgende GitHub-Repository mit dem .NET-Streamingbeispiel. Verwenden Sie dazu den folgenden Befehl:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet.git
 ```

Das Livestreamingbeispiel befindet sich im Ordner [Live](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/Live).

Öffnen Sie in Ihrem heruntergeladenen Projekt die Datei [appsettings.json](https://github.com/Azure-Samples/media-services-v3-dotnet/blob/main/Live/LiveEventWithDVR/appsettings.json). Ersetzen Sie die Werte durch die Anmeldeinformationen, die Sie per [API-Zugriff](./access-api-howto.md) abgerufen haben.

Beachten Sie, dass Sie auch das ENV-Dateiformat im Stammverzeichnis des Projekts verwenden können, um Ihre Umgebungsvariablen nur einmal für alle Projekte im .NET-Beispielrepository festzulegen. Kopieren Sie einfach die ENV-Beispieldatei, und geben Sie die über die Media Services-API-Zugriffsseite im Azure-Portal oder über die Azure CLI ermittelten Informationen an.  Benennen Sie die Datei „sample.env“ in „.env“ um, um sie in allen Projekten zu verwenden.
Die GITIGNORE-Datei ist bereits so konfiguriert, dass die Veröffentlichung der Inhalte dieser Datei in Ihrem geforkten Repository vermieden wird. 

> [!IMPORTANT]
> In diesem Beispiel wird für jede Ressourcen ein eindeutiges Suffix verwendet. Wenn Sie das Debuggen abbrechen oder die App beenden, ohne das Beispiel vollständig zu durchlaufen, werden in Ihrem Konto mehrere Liveereignisse generiert. <br/>Die aktiven Liveereignisse müssen unbedingt beendet werden. Andernfalls **fallen für die Ereignisse Kosten an**!

## <a name="examine-the-code-that-performs-live-streaming"></a>Untersuchen des Codes für Livestreaming

In diesem Abschnitt werden die Funktionen untersucht, die in der Datei [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet/blob/main/Live/LiveEventWithDVR/Program.cs) des *LiveEventWithDVR*-Projekts definiert werden.

Das Beispiel erstellt für jede Ressource ein eindeutiges Suffix, damit keine Namenskonflikte auftreten, wenn Sie das Beispiel ohne Bereinigung der Ressourcen mehrmals ausführen.


### <a name="start-using-media-services-apis-with-net-sdk"></a>Starten der Verwendung von Media Services-APIs mit dem .NET SDK

Um mit der Verwendung von Media Services-APIs in .NET zu beginnen, müssen Sie ein **AzureMediaServicesClient**-Objekt erstellen. Zum Erstellen des Objekts müssen Sie Anmeldeinformationen bereitstellen, die für den Client zum Herstellen einer Verbindung mit Azure mithilfe von Azure AD erforderlich sind. In dem Code, den Sie am Anfang des Artikels geklont haben, wird durch die Funktion **GetCredentialsAsync** das Objekt „ServiceClientCredentials“ auf der Grundlage der Anmeldeinformationen erstellt, die in der lokalen Konfigurationsdatei („appsettings.json“) oder über die ENV-Umgebungsvariablendatei im Stammverzeichnis des Repositorys angegeben wurden.

[!code-csharp[Main](../../../media-services-v3-dotnet/Live/LiveEventWithDVR/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Erstellen eines Liveereignisses

In diesem Abschnitt erfahren Sie, wie Sie ein Liveereignis vom Typ **Pass-Through** erstellen. („LiveEventEncodingType“ ist in diesem Fall auf „None“ festgelegt.) Weitere Informationen zu den anderen verfügbaren Arten von Liveereignissen finden Sie unter [Liveereignistypen](live-event-outputs-concept.md#live-event-types). Neben Passthrough können Sie ein Liveereignis mit Livetranscodierung für eine Cloudcodierung mit adaptiver Bitrate (720p oder 1080p) verwenden. 
 
Beim Erstellen des Liveereignisses können Sie folgende Punkte angeben:

* Erfassungsprotokoll für das Liveereignis (aktuell unterstützte Protokolle: RTMP(S) und Smooth Streaming).<br/>Die Protokolloption kann nicht geändert werden, während das Liveereignis oder die zugehörigen Liveausgaben aktiv sind. Sollten Sie verschiedene Protokolle benötigen, erstellen Sie für jedes Streamingprotokoll ein separates Liveereignis.  
* IP-Einschränkungen für Erfassung und Vorschau. Sie können die IP-Adressen definieren, die ein Video für dieses Liveereignis erfassen dürfen. Zulässige IP-Adressen können als einzelne IP-Adresse (Beispiel: 10.0.0.1), als IP-Adressbereiche mit einer IP-Adresse und einer CIDR-Subnetzmaske (Beispiel: 10.0.0.1/22) oder als IP-Adressbereiche mit einer IP-Adresse und einer Subnetzmaske in Punkt-Dezimalschreibweise (Beispiel: 10.0.0.1(255.255.252.0)) angegeben werden.<br/>Wenn keine IP-Adressen angegeben sind und es keine Regeldefinition gibt, sind keine IP-Adressen zulässig. Um alle IP-Adressen zuzulassen, erstellen Sie eine Regel und legen 0.0.0.0/0 fest.<br/>Die IP-Adressen müssen in einem der folgenden Formate vorliegen: IPv4-Adresse mit vier Ziffern oder CIDR-Adressbereich
* Bei der Ereigniserstellung können Sie angeben, dass das Ereignis automatisch gestartet werden soll. <br/>Wenn für den automatischen Start „true“ festgelegt ist, wird das Liveereignis nach der Erstellung gestartet. Dies bedeutet, dass die Abrechnung beginnt, sobald das Liveereignis startet. Sie müssen für die Liveereignisressource explizit „Beenden“ auswählen, damit keine Gebühren mehr anfallen. Weitere Informationen finden Sie im Abschnitt [LiveEvent-Zustandswerte und Abrechnung](live-event-states-billing-concept.md).
Es stehen auch Standbymodi zur Verfügung, um das Liveereignis in einem preisgünstigeren „zugeordneten“ Zustand zu starten, der einen schnelleren Wechsel zu einem Ausführungszustand ermöglicht. Dies ist etwa im Falle von Pools der heißen Ebene hilfreich, die schnell Kanäle für Streamer bereitstellen müssen.
* Legen Sie die Eigenschaft „useStaticHostname“ auf „true“ fest, um eine Erfassungs-URL zu erhalten, die vorhersagbar und in einem hardwarebasierten Liveencoder einfacher zu verwalten ist. Ausführliche Informationen finden Sie unter [Erfassungs-URLs für Liveereignisse](live-event-outputs-concept.md#live-event-ingest-urls).

[!code-csharp[Main](../../../media-services-v3-dotnet/Live/LiveEventWithDVR/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>Abrufen von Erfassungs-URLs

Sobald das Liveereignis erstellt wurde, können Sie Erfassungs-URLs abrufen, die Sie dem Liveencoder bereitstellen. Diese URLs werden vom Encoder zur Eingabe eines Livedatenstroms verwendet.

[!code-csharp[Main](../../../media-services-v3-dotnet/Live/LiveEventWithDVR/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>Abrufen der Vorschau-URL

Verwenden Sie den Vorschauendpunkt (previewEndpoint), um eine Vorschau anzuzeigen und sich zu vergewissern, dass die Eingabe des Encoders auch tatsächlich empfangen wird.

> [!IMPORTANT]
> Vergewissern Sie sich, dass das Video an die Vorschau-URL übertragen wird, bevor Sie fortfahren.

[!code-csharp[Main](../../../media-services-v3-dotnet/Live/LiveEventWithDVR/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-live-events-and-live-outputs"></a>Erstellen und Verwalten von Liveereignissen und Liveausgaben

Wenn der Stream an das Liveereignis übertragen wird, können Sie das Streamingereignis starten, indem Sie ein Medienobjekt, eine Liveausgabe und einen Streaminglocator erstellen. Dadurch wird der Datenstrom archiviert und über den Streamingendpunkt für die Zuschauer verfügbar gemacht.

Das Medienobjekt können Sie sich als Videokassette vorstellen, wie sie früher einmal in einen Videorekorder eingelegt wurde. Die Liveausgabe ist der Videorekorder selbst. Das Liveereignis ist das vom Videorekorder ausgegebene Videosignal.

Als Erstes erstellen Sie das Signal, indem Sie das Liveereignis erstellen.  Das Signal wird erst übertragen, wenn Sie dieses Liveereignis starten und Ihren Encoder mit der Eingabe verbinden.

Die Videokassette kann zu einem beliebigen Zeitpunkt erstellt werden. Sie ist lediglich ein leeres Medienobjekt, das an das Objekt für die Liveausgabe (in dieser Analogie: der Videorekorder) übergeben wird.

Der Videorekorder kann zu einem beliebigen Zeitpunkt erstellt werden. Sie können also eine Liveausgabe vor oder nach dem Starten der Signalübertragung erstellen. Wenn es schnell gehen muss, empfiehlt es sich manchmal, sie vor dem Starten der Signalübertragung zu erstellen.

Zum Anhalten des Videorekorders muss „delete“ für die Liveausgabe aufgerufen werden. Der Inhalt des Medienobjekts (also der Videokassette) werden dadurch nicht gelöscht.  Das Medienobjekt bleibt mit dem archivierten Videoinhalt erhalten, bis Sie „delete“ explizit für das Medienobjekt aufrufen.

Im nächsten Abschnitt wird die Erstellung des Medienobjekts (Videokassette) und der Liveausgabe (Videorekorder) durchlaufen.

#### <a name="create-an-asset"></a>Erstellen eines Medienobjekts

Erstellen Sie ein Medienobjekt, das von der Liveausgabe verwendet werden kann. In der oben verwendeten Analogie ist dies die Videokassette zum Aufzeichnen des Livevideosignals. Benutzer können den Inhalt live oder bei Bedarf von dieser virtuellen Videokassette ansehen.

[!code-csharp[Main](../../../media-services-v3-dotnet/Live/LiveEventWithDVR/Program.cs#CreateAsset)]

#### <a name="create-a-live-output"></a>Erstellen einer Liveausgabe

Liveausgaben werden bei der Erstellung gestartet und beim Löschen beendet. Hierbei handelt es sich um den Videorekorder für unser Ereignis. Wenn Sie die Liveausgabe löschen, bleiben das zugrunde liegende Medienobjekt und dessen Inhalt erhalten. Der Vorgang ist mit dem Auswerfen der Videokassette vergleichbar. Das Medienobjekt mit der Aufzeichnung kann beliebig lange erhalten bleiben, und nach dem Auswerfen (also nach dem Löschen der Liveausgabe) steht es umgehend für die On-Demand-Wiedergabe zur Verfügung. 

[!code-csharp[Main](../../../media-services-v3-dotnet/Live/LiveEventWithDVR/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaming-locator"></a>Erstellen eines Streaminglocators

> [!NOTE]
> Beim Erstellen Ihres Media Services-Kontos wird dem Konto ein **Standard**-Streamingendpunkt im Zustand **Beendet** hinzugefügt. Um mit dem Streamen Ihrer Inhalte zu beginnen und die [dynamische Paketerstellung](encode-dynamic-packaging-concept.md) und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Zustand **Wird ausgeführt** aufweisen.

Wenn Sie das Medienobjekt mit einem Streaminglocator veröffentlicht haben, ist das Liveereignis (bis zur DVR-Fensterlänge) weiterhin bis zum Ablauf oder zur Löschung des Streaminglocators sichtbar (je nachdem, was zuerst eintritt). Auf diese Weise können Sie die virtuelle Aufzeichnung so verfügbar machen, dass sie von Ihrer Zielgruppe live und nach Bedarf angesehen werden kann. Die gleiche URL kann verwendet werden, um das Liveereignis, das DVR-Fenster oder das On-Demand-Medienobjekt anzusehen, wenn die Aufzeichnung abgeschlossen ist (nach dem Löschen der Liveausgabe).

[!code-csharp[Main](../../../media-services-v3-dotnet/Live/LiveEventWithDVR/Program.cs#CreateStreamingLocator)]

```csharp

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Bereinigen der Ressourcen in Ihrem Media Services-Konto

Wenn die Streamingereignisse abgeschlossen sind und Sie die zuvor bereitgestellten Ressourcen bereinigen möchten, führen Sie die folgenden Schritte aus:

* Beenden Sie die Datenstromeingabe vom Encoder.
* Beenden Sie das Liveereignis. Nach Beendigung des Liveereignisses fallen keine weiteren Kosten an. Wenn Sie den Kanal erneut starten, weist er die gleiche Erfassungs-URL auf, damit Sie den Encoder nicht erneut konfigurieren müssen.
* Sie können Ihren Streamingendpunkt beenden, sofern Sie das Archiv Ihres Liveereignisses nicht als bedarfsgesteuerten Datenstrom bereitstellen möchten. Wenn sich das Liveereignis im Zustand „Beendet“ befindet, fallen keine weiteren Kosten an.

[!code-csharp[Main](../../../media-services-v3-dotnet/Live/LiveEventWithDVR/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet/Live/LiveEventWithDVR/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

## <a name="watch-the-event"></a>Ansehen des Ereignisses

Kopieren Sie zur Wiedergabe des Ereignisses die Streaming-URL, die Sie beim Ausführen des Codes in „Erstellen eines Streaminglocators“ erhalten haben. Sie können einen Medienplayer Ihrer Wahl verwenden. Um Ihren Stream zu testen, können Sie den [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) unter https://ampdemo.azureedge.net verwenden.

Das Liveereignis konvertiert Ereignisse automatisch in On-Demand-Inhalt, wenn es beendet wird. Auch nach dem Beenden und Löschen des Ereignisses können die Benutzer archivierte Inhalte als Video auf Abruf streamen, solange Sie das Medienobjekt nicht löschen. Ein Medienobjekt kann nicht gelöscht werden, wenn es von einem Ereignis verwendet wird. Zuerst muss das betreffende Ereignis gelöscht werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie keine Ressourcen in Ihrer Ressourcengruppe mehr benötigen, einschließlich der Media Services und Speicherkonten, die Sie für dieses Tutorial erstellt haben, löschen Sie die zuvor erstellte Ressourcengruppe.

Führen Sie den folgenden CLI-Befehl aus:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Wenn Sie das Liveereignis nicht beenden, fallen weiter Kosten dafür an. Falls das Projekt/Programm abstürzt oder aus einem anderen Grund geschlossen wird, bleibt das Liveereignis unter Umständen aktiv und verursacht weitere Kosten.

## <a name="ask-questions-give-feedback-get-updates"></a>Fragen stellen, Feedback geben, Updates abrufen

Im Artikel [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services zu bekommen.

## <a name="next-steps"></a>Nächste Schritte

[Streamen von Dateien](stream-files-tutorial-with-api.md)
 
