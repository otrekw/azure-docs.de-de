---
author: Juliako
ms.service: azure-video-analyzer
ms.topic: include
ms.date: 04/07/2021
ms.author: juliako
ms.openlocfilehash: c21a052dd25fae2d74762726a520fd109da11c9e
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385838"
---
Als Teil der Voraussetzungen haben Sie den Beispielcode in einen Ordner heruntergeladen. Führen Sie die unten angegebenen Schritte aus, um die Beispieldateien zu überprüfen und zu bearbeiten.

1. Navigieren Sie in Visual Studio Code zu **src/edge**. Ihre *ENV*-Datei und einige Bereitstellungsvorlagendateien werden angezeigt.

    Die Bereitstellungsvorlage verweist auf das Bereitstellungsmanifest für das Edge-Gerät. Es enthält einige Platzhalterwerte. Die ENV-Datei enthält die Werte für diese Variablen.
1. Navigieren Sie zum Ordner **src/cloud-to-device-console-app**. Darin finden Sie Ihre Datei *appsettings.json* und einige andere Dateien:

    * **c2d-console-app.csproj**: Die Projektdatei für Visual Studio Code.
    * **operations.json**: Eine Liste mit den Vorgängen, die vom Programm ausgeführt werden sollen.
    * **Program.cs**: Das Beispiel für den Programmcode. Mit diesem Code wird Folgendes durchgeführt:

        * Laden der App-Einstellungen
        * Aufrufen [direkter Methoden](../../../direct-methods.md),<!--add a link--> die vom Video Analyzer-Edge-Modul verfügbar gemacht werden
        * Anhalten der Ausführung, sodass Sie die Ausgabe des Programms im **Terminalfenster** und die vom Modul generierten Ereignisse im **Ausgabefenster** untersuchen können
        * Aufrufen direkter Methoden zur Bereinigung der Ressourcen
1. Bearbeiten Sie die Datei **operations.json**:
    
    * Ändern Sie den Link zur Pipeline: <br/>`"topologyUrl" : "https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/evr-motion-files/topology.json" `
    * Bearbeiten Sie unter „livePipelineSet“ den Namen der Pipelinetopologie, sodass er mit dem Wert im vorherigen Link übereinstimmt: <br/>`"topologyName" : "EVRToFilesOnMotionDetection" `
    * Bearbeiten Sie unter „PipelineTopologyDelete“ den Namen: <br/>`"name": "EVRToFilesOnMotionDetection" `
