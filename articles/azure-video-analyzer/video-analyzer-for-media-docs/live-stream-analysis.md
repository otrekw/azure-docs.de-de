---
title: Livestreamanalyse mit Azure Video Analyzer for Media (ehemals Video Indexer)
titleSuffix: Azure Video Analyzer for Media
description: In diesem Artikel wird gezeigt, wie Sie eine Livestreamanalyse mit Azure Video Analyzer for Media (ehemals Video Indexer) durchführen.
services: azure-video-analyzer
author: Juliako
manager: femila
ms.topic: article
ms.subservice: azure-video-analyzer-media
ms.date: 11/13/2019
ms.author: juliako
ms.openlocfilehash: d5c45a708a1a878a00a93ce30dcbad37105bd5ea
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2021
ms.locfileid: "112119495"
---
# <a name="live-stream-analysis-with-video-analyzer-for-media"></a>Livestreamanalyse mit Video Analyzer for Media

Azure Video Analyzer for Media (ehemals Video Indexer) ist ein Azure-Dienst, der entwickelt wurde, um eingehende Erkenntnisse aus Video- und Audiodateien offline zu extrahieren. Das heißt, dass eine bestimmte Mediendatei analysiert wird, die bereits im Voraus erstellt wurde. Bei manchen Anwendungsfällen ist es jedoch wichtig, dass die Medienerkenntnisse aus einem Livefeed so schnell wie möglich zu erhalten, um Betriebs- und andere Anwendungsfälle freizugeben, die unter Zeitdruck stehen. Beispielsweise könnten solche umfangreichen Metadaten in einem Livestream von Inhaltsproduzenten verwendet werden, um die TV-Produktion zu automatisieren.

Mithilfe der in diesem Artikel beschriebenen Lösung können Kunden Video Analyzer for Media in nahezu Echtzeitauflösungen für Livefeeds verwenden. Die Verzögerung bei der Indizierung kann bei Verwendung dieser Lösung so niedrig wie bis zu vier Minuten sein, abhängig davon, welche Datenblöcke indiziert werden, sowie von der Eingangsauflösung, dem Inhaltstyp und der für diesen Prozess verwendeten Computeleistung.

![Metadaten von Video Analyzer for Media im Livestream](./media/live-stream-analysis/live-stream-analysis01.png)

*Abbildung 1 – Beispielplayer, der die Metadaten von Video Analyzer for Media im Livestream anzeigt*

Die vorliegende [Streamanalyselösung](https://aka.ms/livestreamanalysis) verwendet Azure Functions und zwei Logik-Apps, um ein Liveprogramm aus einem Livekanal in Azure Media Services mit Video Analyzer for Media zu verarbeiten, und zeigt das Ergebnis mit Azure Media Player an, wobei der Ergebnisstream nahezu in Echtzeit dargestellt wird.

Grob besehen, besteht dies aus zwei wesentlichen Schritten. Der erste Schritt wird alle 60 Sekunden ausgeführt und erfasst einen Teilclip der letzten wiedergegebenen 60 Sekunden, erstellt ein Objekt daraus und indiziert dieses mittels Video Analyzer for Media. Anschließend wird der zweite Schritt aufgerufen, sobald die Indizierung abgeschlossen ist. Die erfassten Erkenntnisse werden verarbeitet, an Azure Cosmos DB gesendet, und der indizierte Teilclip wird gelöscht.

Der Beispiel-Player gibt den Livestream wieder und erhält die Erkenntnisse von Azure Cosmos DB unter Verwendung einer dedizierten Azure-Funktion. Er zeigt die Metadaten und Miniaturansichten synchron mit dem Livevideo an.

![Die beiden Logik-Apps, die den Livestream jede Minute in der Cloud verarbeiten](./media/live-stream-analysis/live-stream-analysis02.png)

*Abbildung 2 – Die beiden Logik-Apps, die den Livestream jede Minute in der Cloud verarbeiten.*

## <a name="step-by-step-guide"></a>Leitfaden mit Schrittanleitungen 

Den vollständigen Code und einen Leitfaden mit Schrittanleitungen zum Bereitstellen der Ergebnisse finden Sie im [GitHub-Projekt für Livemedienanalysen mit Video Analyzer for Media](https://aka.ms/livestreamanalysis). 

## <a name="next-steps"></a>Nächste Schritte

[Video Analyzer for Media: Übersicht](video-indexer-overview.md)
