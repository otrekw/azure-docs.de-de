---
title: Überwachen des Auftragsfortschritts mit .NET
description: Erfahren Sie, wie Sie den Fortschritt von Aufträgen mit Ereignishandlern abrufen und Statusupdates senden können. Das Codebeispiel ist in C# geschrieben und verwendet das Media Services SDK für .NET.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ee720ed6-8ce5-4434-b6d6-4df71fca224e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.custom: devx-track-csharp
ms.openlocfilehash: 5a32744f7a2385bf9da92a0f9cae5f664b1a18fc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103016966"
---
# <a name="monitor-job-progress-using-net"></a>Überwachen des Auftragsfortschritts mit .NET

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!NOTE]
> Media Services v2 werden derzeit keine neuen Features oder Funktionen hinzugefügt. <br/>Sehen Sie sich die neuste Version – [Media Services v3](../latest/index.yml) – an. Lesen Sie außerdem die [Hinweise zur Migration von v2 zu v3](../latest/migrate-v-2-v-3-migration-introduction.md).

Beim Ausführen von Aufträgen ist es nützlich, deren Fortschritt verfolgen zu können. Sie können den Fortschritt überprüfen, indem Sie einen StateChanged-Ereignishandler definieren (wie im vorliegenden Thema beschrieben) oder Azure-Warteschlangenspeicher zur Überwachung von Media Services-Auftragsbenachrichtigungen verwenden (wie in [diesem](media-services-dotnet-check-job-progress-with-queues.md) Thema beschrieben).

## <a name="define-statechanged-event-handler-to-monitor-job-progress"></a>Definieren eines StateChanged-Ereignishandlers zum Überwachen des Auftragsfortschritts

Das folgende Codebeispiel definiert den StateChanged-Ereignishandler. Dieser Ereignishandler verfolgt den Auftragsfortschritt und liefert Statusupdates je nach Auftragsstatus. Der Code definiert außerdem die LogJobStop-Methode. Diese Hilfsmethode protokolliert Fehlerdetails.

```csharp
    private static void StateChanged(object sender, JobStateChangedEventArgs e)
    {
        Console.WriteLine("Job state changed event:");
        Console.WriteLine("  Previous state: " + e.PreviousState);
        Console.WriteLine("  Current state: " + e.CurrentState);

        switch (e.CurrentState)
        {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("********************");
                Console.WriteLine("Job is finished.");
                Console.WriteLine("Please wait while local tasks or downloads complete...");
                Console.WriteLine("********************");
                Console.WriteLine();
                Console.WriteLine();
                break;
            case JobState.Canceling:
            case JobState.Queued:
            case JobState.Scheduled:
            case JobState.Processing:
                Console.WriteLine("Please wait...\n");
                break;
            case JobState.Canceled:
            case JobState.Error:
                // Cast sender as a job.
                IJob job = (IJob)sender;
                // Display or log error details as needed.
                LogJobStop(job.Id);
                break;
            default:
                break;
        }
    }

    private static void LogJobStop(string jobId)
    {
        StringBuilder builder = new StringBuilder();
        IJob job = GetJob(jobId);

        builder.AppendLine("\nThe job stopped due to cancellation or an error.");
        builder.AppendLine("***************************");
        builder.AppendLine("Job ID: " + job.Id);
        builder.AppendLine("Job Name: " + job.Name);
        builder.AppendLine("Job State: " + job.State.ToString());
        builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
        builder.AppendLine("Media Services account name: " + _accountName);
        builder.AppendLine("Media Services account location: " + _accountLocation);
        // Log job errors if they exist.  
        if (job.State == JobState.Error)
        {
            builder.Append("Error Details: \n");
            foreach (ITask task in job.Tasks)
            {
                foreach (ErrorDetail detail in task.ErrorDetails)
                {
                    builder.AppendLine("  Task Id: " + task.Id);
                    builder.AppendLine("    Error Code: " + detail.Code);
                    builder.AppendLine("    Error Message: " + detail.Message + "\n");
                }
            }
        }
        builder.AppendLine("***************************\n");
        // Write the output to a local file and to the console. The template 
        // for an error output file is:  JobStop-{JobId}.txt
        string outputFile = _outputFilesFolder + @"\JobStop-" + JobIdAsFileName(job.Id) + ".txt";
        WriteToFile(outputFile, builder.ToString());
        Console.Write(builder.ToString());
    }

    private static string JobIdAsFileName(string jobID)
    {
        return jobID.Replace(":", "_");
    }
```


## <a name="next-step"></a>Nächster Schritt
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
