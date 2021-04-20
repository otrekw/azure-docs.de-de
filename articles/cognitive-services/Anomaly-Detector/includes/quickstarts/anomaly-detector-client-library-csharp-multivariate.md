---
title: 'Schnellstart: Multivariate .NET-Clientbibliothek für die Anomalieerkennung'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/06/2021
ms.author: mbullwin
ms.openlocfilehash: 1318a8c410f14f4a1dc91072d66f18e39f7ca7e7
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107318769"
---
Hier finden Sie Informationen zu den ersten Schritten mit der multivariaten Clientbibliothek für die Anomalieerkennung für .NET. Führen Sie die hier angegebenen Schritte aus, um das Paket zu installieren und mit der Verwendung der bereitgestellten Algorithmen zu beginnen. Die neuen APIs für die multivariate Anomalieerkennung ermöglichen Entwicklern die einfache Integration fortschrittlicher KI zur Erkennung von Anomalien in Metrikgruppen ganz ohne Machine Learning-Kenntnisse oder gekennzeichnete Daten. Abhängigkeiten und Interkorrelationen zwischen verschiedenen Signalen werden automatisch als Schlüsselfaktoren gewertet. Dadurch können Sie Ihre komplexen Systeme leichter proaktiv vor Fehlern schützen.

Die Clientbibliothek für die multivariate Anomalieerkennung für .NET kann für Folgendes verwendet werden:

* Erkennen von Anomalien auf Systemebene in einer Gruppe von Zeitreihen
* Betrachten sämtlicher Signale, um ein Problem zu erkennen, wenn die Betrachtung einzelner Zeitreihen nicht ausreicht
* Prädikative Wartung teurer physischer Ressourcen mit dutzenden bis hunderten verschiedenen Arten von Sensoren zur Messung diverser Aspekte der Systemintegrität

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* Aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title="Erstellen einer Anomalieerkennungsressource"  target="_blank"> eine Anomalieerkennungsressource </a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Warten Sie die Bereitstellung ab, und wählen Sie dann die Schaltfläche **Zu Ressource wechseln** aus.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Anomalieerkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-net-core-application"></a>Erstellen einer neuen .NET Core-Anwendung

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl `dotnet new` zum Erstellen einer neuen Konsolen-App mit dem Namen `anomaly-detector-quickstart-multivariate`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-Projekt mit einer einzigen C#-Quelldatei: *Program.cs*.

```dotnetcli
dotnet new console -n anomaly-detector-quickstart-multivariate
```

Wechseln Sie zum Ordner der neu erstellten App. Sie können die Anwendung mit folgendem Befehl erstellen:

```dotnetcli
dotnet build
```

Die Buildausgabe sollte keine Warnungen oder Fehler enthalten.

```output
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Installieren Sie im Anwendungsverzeichnis mit dem folgenden Befehl die Anomalieerkennungs-Clientbibliothek für .NET:

```dotnetcli
dotnet add package Azure.AI.AnomalyDetector --version 3.0.0-preview.3
```

Öffnen Sie über das Projektverzeichnis die Datei *program.cs*, und fügen Sie mithilfe von `directives` Folgendes hinzu:

```csharp
using System;
using System.Collections.Generic;
using System.Drawing.Text;
using System.IO;
using System.Linq;
using System.Linq.Expressions;
using System.Net.NetworkInformation;
using System.Reflection;
using System.Text;
using System.Threading.Tasks;
using Azure.AI.AnomalyDetector.Models;
using Azure.Core.TestFramework;
using Microsoft.Identity.Client;
using NUnit.Framework;
```

Erstellen Sie in der Methode `main()` der Anwendung Variablen für den Azure-Endpunkt Ihrer Ressource sowie für Ihren API-Schlüssel und für eine benutzerdefinierte Datenquelle.

```csharp
string endpoint = "YOUR_API_KEY";
string apiKey =  "YOUR_ENDPOINT";
string datasource = "YOUR_SAMPLE_ZIP_FILE_LOCATED_IN_AZURE_BLOB_STORAGE_WITH_SAS";
```

 Um die multivariaten APIs für die Anomalieerkennung verwenden zu können, müssen wir unser eigenes Modell vor der Erkennung trainieren. Für das Training wird ein Batch von Zeitreihendaten verwendet. Die einzelnen Zeitreihen müssen jeweils im CSV-Format mit zwei Spalten (Zeitstempel und Wert) vorliegen. Alle Zeitreihen müssen in einer ZIP-Datei zusammengefasst und in [Azure Blob Storage](../../../../storage/blobs/storage-blobs-introduction.md#blobs) hochgeladen werden. Standardmäßig wird der Dateiname verwendet, um die Variable für die Zeitreihe darzustellen. Alternativ kann eine zusätzliche Datei vom Typ „meta.json“ in die ZIP-Datei eingeschlossen werden, wenn sich der Name der Variablen vom Namen der ZIP-Datei unterscheiden soll. Nach dem Generieren einer [BLOB-SAS-URL (Shared Access Signatures)](../../../../storage/common/storage-sas-overview.md) können wir die URL der ZIP-Datei für das Training verwenden.

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie folgende Vorgänge mit der multivariaten Clientbibliothek für die Anomalieerkennung für .NET durchgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Trainieren des Modells](#train-the-model)
* [Erkennen von Anomalien](#detect-anomalies)
* [Exportieren des Modells](#export-model)
* [Löschen des Modells](#delete-model)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Instanziieren Sie einen Anomalieerkennungsclient mit Ihrem Endpunkt und Schlüssel.

```csharp
var endpointUri = new Uri(endpoint);
var credential = new AzureKeyCredential(apiKey)

AnomalyDetectorClient client = new AnomalyDetectorClient(endpointUri, credential);
```

## <a name="train-the-model"></a>Trainieren des Modells

Erstellen Sie wie im Anschluss beschrieben eine neue private asynchrone Aufgabe zur Behandlung des Modelltrainings. Sie verwenden `TrainMultivariateModel`, um das Modell zu trainieren, und `GetMultivariateModelAysnc`, um zu überprüfen, wann das Training abgeschlossen ist.

```csharp
private async Task trainAsync(AnomalyDetectorClient client, string datasource, DateTimeOffset start_time, DateTimeOffset end_time, int max_tryout = 500)
{
    try
    {
        Console.WriteLine("Training new model...");

        int model_number = await getModelNumberAsync(client, false).ConfigureAwait(false);
        Console.WriteLine(String.Format("{0} available models before training.", model_number));

        ModelInfo data_feed = new ModelInfo(datasource, start_time, end_time);
        Response response_header = client.TrainMultivariateModel(data_feed);
        response_header.Headers.TryGetValue("Location", out string trained_model_id_path);
        Guid trained_model_id = Guid.Parse(trained_model_id_path.Split('/').LastOrDefault());
        Console.WriteLine(trained_model_id);

        // Wait until the model is ready. It usually takes several minutes
        Response<Model> get_response = await client.GetMultivariateModelAsync(trained_model_id).ConfigureAwait(false);
        ModelStatus? model_status = null;
        int tryout_count = 0;
        TimeSpan create_limit = new TimeSpan(0, 3, 0);
        while (tryout_count < max_tryout & model_status != ModelStatus.Ready)
        {
            System.Threading.Thread.Sleep(10000);
            get_response = await client.GetMultivariateModelAsync(trained_model_id).ConfigureAwait(false);
            ModelInfo model_info = get_response.Value.ModelInfo;
            Console.WriteLine(String.Format("model_id: {0}, createdTime: {1}, lastUpdateTime: {2}, status: {3}.", get_response.Value.ModelId, get_response.Value.CreatedTime, get_response.Value.LastUpdatedTime, model_info.Status));

            if (model_info != null)
            {
                model_status = model_info.Status;
            }
            tryout_count += 1;
        };
        get_response = await client.GetMultivariateModelAsync(trained_model_id).ConfigureAwait(false);

        if (model_status != ModelStatus.Ready)
        {
            Console.WriteLine(String.Format("Request timeout after {0} tryouts", max_tryout));
        }

        model_number = await getModelNumberAsync(client).ConfigureAwait(false);
        Console.WriteLine(String.Format("{0} available models after training.", model_number));
        return trained_model_id;
    }
    catch (Exception e)
    {
        Console.WriteLine(String.Format("Train error. {0}", e.Message));
        throw new Exception(e.Message);
    }
}
```

## <a name="detect-anomalies"></a>Erkennen von Anomalien

Um Anomalien mit dem neu trainierten Modell zu erkennen, erstellen Sie eine private asynchrone Aufgabe (`private async Task`) namens `detectAsync`. Sie erstellen eine neue Erkennungsanforderung (`DetectionRequest`) und übergeben sie als Parameter an `DetectAnomalyAsync`.

```csharp
private async Task<DetectionResult> detectAsync(AnomalyDetectorClient client, string datasource, Guid model_id, DateTimeOffset start_time, DateTimeOffset end_time, int max_tryout = 500)
{
    try
    {
        Console.WriteLine("Start detect...");
        Response<Model> get_response = await client.GetMultivariateModelAsync(model_id).ConfigureAwait(false);

        DetectionRequest detectionRequest = new DetectionRequest(datasource, start_time, end_time);
        Response result_response = await client.DetectAnomalyAsync(model_id, detectionRequest).ConfigureAwait(false);
        var ok = result_response.Headers.TryGetValue("Location", out string result_id_path);
        Guid result_id = Guid.Parse(result_id_path.Split('/').LastOrDefault());
        // get detection result
        Response<DetectionResult> result = await client.GetDetectionResultAsync(result_id).ConfigureAwait(false);
        int tryout_count = 0;
        while (result.Value.Summary.Status != DetectionStatus.Ready & tryout_count < max_tryout)
        {
            System.Threading.Thread.Sleep(2000);
            result = await client.GetDetectionResultAsync(result_id).ConfigureAwait(false);
            tryout_count += 1;
        }

        if (result.Value.Summary.Status != DetectionStatus.Ready)
        {
            Console.WriteLine(String.Format("Request timeout after {0} tryouts", max_tryout));
            return null;
        }

        return result.Value;
    }
    catch (Exception e)
    {
        Console.WriteLine(String.Format("Detection error. {0}", e.Message));
        throw new Exception(e.Message);
    }
}
```

## <a name="export-model"></a>Exportieren des Modells

Erstellen Sie zum Exportieren des zuvor trainierten Modells eine private asynchrone Aufgabe (`private async Task`) namens `exportAysnc`. Sie verwenden `ExportModelAsync` und übergeben die Modell-ID des Modells, das Sie exportieren möchten.

```csharp
private async Task exportAsync(AnomalyDetectorClient client, Guid model_id, string model_path = "model.zip")
{
    try
    {
        Response model_response = await client.ExportModelAsync(model_id).ConfigureAwait(false);
        Stream model;
        if (model_response.ContentStream != null)
        {
            model = model_response.ContentStream;
            var fileStream = File.Create(model_path);
            model.Seek(0, SeekOrigin.Begin);
            model.CopyTo(fileStream);
            fileStream.Close();
        }
    }
    catch (Exception e)
    {
        Console.WriteLine(String.Format("Export error. {0}", e.Message));
        throw new Exception(e.Message);
    }
}
```

## <a name="delete-model"></a>Löschen des Modells

Verwenden Sie zum Löschen eines zuvor erstellten Modells `DeleteMultivariateModelAsync`, und übergeben Sie die Modell-ID des zu löschenden Modells. Zum Abrufen einer Modell-ID können Sie `getModelNumberAsync` verwenden:

```csharp
private async Task deleteAsync(AnomalyDetectorClient client, Guid model_id)
{
    await client.DeleteMultivariateModelAsync(model_id).ConfigureAwait(false);
    int model_number = await getModelNumberAsync(client).ConfigureAwait(false);
    Console.WriteLine(String.Format("{0} available models after deletion.", model_number));
}
private async Task<int> getModelNumberAsync(AnomalyDetectorClient client, bool delete = false)
{
    int count = 0;
    AsyncPageable<ModelSnapshot> model_list = client.ListMultivariateModelAsync(0, 10000);
    await foreach (ModelSnapshot x in model_list)
    {
        count += 1;
        Console.WriteLine(String.Format("model_id: {0}, createdTime: {1}, lastUpdateTime: {2}.", x.ModelId, x.CreatedTime, x.LastUpdatedTime));
        if (delete & count < 4)
        {
            await client.DeleteMultivariateModelAsync(x.ModelId).ConfigureAwait(false);
        }
    }
    return count;
}
```

## <a name="main-method"></a>Main-Methode

Nachdem Sie nun über alle Komponententeile verfügen, müssen Sie Ihrer main-Methode zusätzlichen Code hinzufügen, um Ihre neu erstellten Aufgaben aufzurufen.

```csharp

{
    //read endpoint and apiKey
     string endpoint = "YOUR_API_KEY";
    string apiKey =  "YOUR_ENDPOINT";
    string datasource = "YOUR_SAMPLE_ZIP_FILE_LOCATED_IN_AZURE_BLOB_STORAGE_WITH_SAS";
    Console.WriteLine(endpoint);
    var endpointUri = new Uri(endpoint);
    var credential = new AzureKeyCredential(apiKey);

    //create client
    AnomalyDetectorClient client = new AnomalyDetectorClient(endpointUri, credential);

    // train
    TimeSpan offset = new TimeSpan(0);
    DateTimeOffset start_time = new DateTimeOffset(2021, 1, 1, 0, 0, 0, offset);
    DateTimeOffset end_time = new DateTimeOffset(2021, 1, 2, 12, 0, 0, offset);
    Guid? model_id_raw = null;
    try
    {
        model_id_raw = await trainAsync(client, datasource, start_time, end_time).ConfigureAwait(false);
        Console.WriteLine(model_id_raw);
        Guid model_id = model_id_raw.GetValueOrDefault();

        // detect
        start_time = end_time;
        end_time = new DateTimeOffset(2021, 1, 3, 0, 0, 0, offset);
        DetectionResult result = await detectAsync(client, datasource, model_id, start_time, end_time).ConfigureAwait(false);
        if (result != null)
        {
            Console.WriteLine(String.Format("Result ID: {0}", result.ResultId));
            Console.WriteLine(String.Format("Result summary: {0}", result.Summary));
            Console.WriteLine(String.Format("Result length: {0}", result.Results.Count));
        }

        // export model
        await exportAsync(client, model_id).ConfigureAwait(false);

        // delete
        await deleteAsync(client, model_id).ConfigureAwait(false);
    }
    catch (Exception e)
    {
        String msg = String.Format("Multivariate error. {0}", e.Message);
        if (model_id_raw != null)
        {
            await deleteAsync(client, model_id_raw.GetValueOrDefault()).ConfigureAwait(false);
        }
        Console.WriteLine(msg);
        throw new Exception(msg);
    }
}

```

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `dotnet run` aus dem Anwendungsverzeichnis aus.

```dotnetcli
dotnet run
```

## <a name="next-steps"></a>Nächste Schritte

* [Bewährte Methoden für die multivariate Anomalieerkennung mit Zeitreihen](../../concepts/best-practices-multivariate.md)
