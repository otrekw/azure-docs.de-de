---
title: Informationen zum Modellparser „Digital Twins“ | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen, wie Sie als Entwickler den DTDL-Parser zur Überprüfung von Modellen verwenden.
author: rido-min
ms.author: rmpablos
ms.date: 04/29/2020
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: peterpr
ms.openlocfilehash: 20c4452a32c791f33e08c883d8cec89a345ab188
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352001"
---
# <a name="understand-the-digital-twins-model-parser"></a>Informationen zum Modellparser „Digital Twins“

Die Digital Twins Definition Language (DTDL) wird unter [Digital Twins Definition Language](https://github.com/Azure/opendigitaltwins-dtdl) beschrieben. Benutzer können das _Digital Twins Model Parser_-NuGet-Paket verwenden, um ein Modell zu überprüfen und abzufragen, das in mehreren Dateien definiert ist.

## <a name="install-the-dtdl-model-parser"></a>Installieren des DTDL-Modellparsers

Der Parser steht unter NuGet.org mit folgender ID zur Verfügung: [Microsoft.Azure.DigitalTwins.Parser](https://www.nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser). Verwenden Sie zur Installation des Parsers einen kompatiblen NuGet-Paket-Manager, z. B. den in Visual Studio oder in der `dotnet`-CLI.

```bash
dotnet add package Microsoft.Azure.DigitalTwins.Parser
```

## <a name="use-the-parser-to-validate-a-model"></a>Verwenden des Parsers zur Überprüfung eines Modells

Das Modell, das Sie überprüfen möchten, kann aus mehreren in JSON-Dateien beschriebenen Schnittstellen bestehen. Sie können den Parser verwenden, um alle Dateien in einem bestimmten Ordner zu laden, und Sie können den Parser verwenden, um alle Dateien als Ganzes zu überprüfen, einschließlich zwischen den Dateien enthaltenen Verweisen:

1. Erstellen Sie eine `IEnumerable<string>`-Schnittstelle mit einer Liste aller Modellinhalte:

    ```csharp
    using System.IO;

    string folder = @"c:\myModels\";
    string filespec = "*.json";

    List<string> modelJson = new List<string>();
    foreach (string filename in Directory.GetFiles(folder, filespec))
    {
        using StreamReader modelReader = new StreamReader(filename);
        modelJson.Add(modelReader.ReadToEnd());
    }
    ```

1. Instanziieren Sie `ModelParser`, und rufen Sie `ParseAsync` auf:

    ```csharp
    using Microsoft.Azure.DigitalTwins.Parser;

    ModelParser modelParser = new ModelParser();
    IReadOnlyDictionary<Dtmi, DTEntityInfo> parseResult = await modelParser.ParseAsync(modelJson);
    ```

1. Überprüfen Sie, ob Überprüfungsfehler auftreten. Wenn der Parser keine Fehler entdeckt, wird `AggregateException` mit einer Liste detaillierter Fehlermeldungen zurückgegeben:

    ```csharp
    try
    {
        IReadOnlyDictionary<Dtmi, DTEntityInfo> parseResult = await modelParser.ParseAsync(modelJson);
    }
    catch (AggregateException ae)
    {
        foreach (var e in ae.InnerExceptions)
        {
            Console.WriteLine(e.Message);
        }
    }
    ```

1. Untersuchen Sie `Model`. Wenn die Überprüfung erfolgreich war, können Sie die Modellparser-API verwenden, um das Modell zu untersuchen. Der folgende Codeausschnitt zeigt, wie Sie alle geparsten Modelle durchlaufen, und die vorhandenen Eigenschaften anzeigen:

    ```csharp
    foreach (var m in parseResult)
    {
        Console.WriteLine(m.Key);
        foreach (var item in m.Value.AsEnumerable<DTEntityInfo>())
        {
            var p = item as DTInterfaceInfo;
            if (p!=null)
            {
                Console.WriteLine($"\t{p.Id}");
                Console.WriteLine($"\t{p.Description.FirstOrDefault()}");
            }
            Console.WriteLine("--------------");
        }
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

Die in diesem Artikel erläuterte Modellparser-API ermöglicht es in vielen Szenarios, Aufgaben zu automatisieren oder zu überprüfen, die von DTDL-Modellen abhängig sind. Sie können beispielsweise dynamisch eine Benutzeroberfläche aus den Informationen im Modell erstellen.
