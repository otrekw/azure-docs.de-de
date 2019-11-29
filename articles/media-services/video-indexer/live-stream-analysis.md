---
title: Livestreamanalyse mittels Video Indexer
titleSuffix: Azure Media Services
description: In diesem Artikel wird gezeigt, wie Sie eine Livestreamanalyse mittels Video Indexer durchführen.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 11/13/2019
ms.author: juliako
ms.openlocfilehash: 0f34aad4a8590c71f926d12d201f9a614afaa127
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74114701"
---
# <a name="live-stream-analysis-with-video-indexer"></a>Livestreamanalyse mit Video Indexer

Azure Media Services Video Indexer ist ein Azure-Dienst, der entwickelt wurde, um tiefgehende Erkenntnisse aus Video- und Audiodateien offline zu extrahieren. Das heißt, dass eine bestimmte Mediendatei analysiert wird, die bereits im Voraus erstellt wurde. Bei manchen Anwendungsfällen ist es jedoch wichtig, dass die Medienerkenntnisse aus einem Livefeed so schnell wie möglich zu erhalten, um Betriebs- und andere Anwendungsfälle freizugeben, die unter Zeitdruck stehen. Beispielsweise könnten solche umfangreichen Metadaten in einem Livestream von Inhaltsproduzenten verwendet werden, um die TV-Produktion zu automatisieren. Ein Beispiel hierfür wäre die [Endemol Shine-Gruppe](https://customers.microsoft.com/story/esg-media-telecommunications-azure), bei der Journalisten des Newsrooms die Livefeeds durchsucht haben, um auf Grundlage des Inhalts Benachrichtigungsdienste zu erstellen.

Mithilfe der in diesem Artikel beschriebenen Lösung können Kunden Video Indexer in nahezu Echtzeitauflösungen für Livefeeds verwenden. Die Verzögerung bei der Indizierung kann bei Verwendung dieser Lösung so niedrig wie bis zu vier Minuten sein, abhängig davon, welche Datenblöcke indiziert werden, sowie von der Eingangsauflösung, dem Inhaltstyp und der für diesen Prozess verwendeten Computeleistung.

![Die Video Indexer-Metadaten im Livestream](./media/live-stream-analysis/live-stream-analysis01.png)

*Abbildung 1 – Beispiel-Player, der die Video Indexer-Metadaten im Livestream anzeigt*

Die vorliegende [Streamanalyselösung](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/blob/master/media-functions-for-logic-app/LiveStreamAnalysis.md) verwendet Azure Functions und zwei Logik-Apps, um ein Liveprogramm aus einem Livekanal in Azure Media Services mit Video Indexer zu verarbeiten, und zeigt das Ergebnis mit Azure Media Player an, wobei der Ergebnisstream nahezu in Echtzeit dargestellt wird.

Grob besehen, besteht dies aus zwei wesentlichen Schritten. Der erste Schritt wird alle 60 Sekunden ausgeführt und erfasst einen Teilclip der letzten wiedergegebenen 60 Sekunden, erstellt ein Objekt daraus und indiziert dieses mittels Video Indexer. Anschließend wird der zweite Schritt aufgerufen, sobald die Indizierung abgeschlossen ist. Die erfassten Erkenntnisse werden verarbeitet, an Azure Cosmos DB gesendet, und der indizierte Teilclip wird gelöscht.

Der Beispiel-Player gibt den Livestream wieder und erhält die Erkenntnisse von Azure Cosmos DB unter Verwendung einer dedizierten Azure-Funktion. Er zeigt die Metadaten und Miniaturansichten synchron mit dem Livevideo an.

![Die beiden Logik-Apps, die den Livestream jede Minute in der Cloud verarbeiten](./media/live-stream-analysis/live-stream-analysis02.png)

*Abbildung 2 – Die beiden Logik-Apps, die den Livestream jede Minute in der Cloud verarbeiten.*

## <a name="step-by-step-guide"></a>Leitfaden mit Schrittanleitungen 

Den vollständigen Code und einen Leitfaden mit Schrittanleitungen zum Bereitstellen der Ergebnisse finden Sie im [GitHub-Projekt für Livemedienanalysen mit Video Indexer](https://aka.ms/livestreamanalysis). 

## <a name="next-steps"></a>Nächste Schritte

[What is Video Indexer? (preview)](video-indexer-overview.md) (Was ist Video Indexer? (Vorschauversion))
