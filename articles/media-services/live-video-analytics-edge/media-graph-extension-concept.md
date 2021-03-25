---
title: 'Informationen zur Mediengrapherweiterung: Azure'
description: Mit Live Video Analytics in IoT Edge können Sie die Mediengraph-Verarbeitungsfunktionen über einen Grapherweiterungsknoten erweitern.
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: 66656690e4e53508253091134a67adeb899b3dbf
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102455838"
---
# <a name="media-graph-extension"></a>Mediengrapherweiterung

Mit Live Video Analytics in IoT Edge können Sie die Mediengraph-Verarbeitungsfunktionen über einen Grapherweiterungsknoten erweitern. Ihr Analyseerweiterungs-Plug-In kann herkömmliche Bildverarbeitungstechniken oder KI-Modelle des maschinellen Sehens nutzen. Zur Aktivierung von Grapherweiterungen wird ein Erweiterungsprozessorknoten in einen Mediengraphen eingeschlossen. Der Erweiterungsprozessorknoten leitet Videoframes an den konfigurierten Endpunkt weiter und fungiert als Schnittstelle für Ihre Erweiterung. Die Verbindung kann zu einem lokalen oder entfernten Endpunkt hergestellt und bei Bedarf durch Authentifizierung und TLS-Verschlüsselung gesichert werden. Außerdem ermöglicht der Graph-Erweiterungsprozessorknoten optional die Skalierung und Codierung der Videoframes, bevor sie an Ihre benutzerdefinierte Erweiterung übermittelt werden. 

Live Video Analytics unterstützt zwei Arten von Mediengraph-Erweiterungsprozessoren:

* [Verarbeitungsknoten für die HTTP-Erweiterung](media-graph-concept.md#http-extension-processor)
* [gRPC-Erweiterungsprozessor](media-graph-concept.md#grpc-extension-processor)

Der Graph-Erweiterungsknoten erwartet, dass das Analyse-Erweiterungs-Plug-In die Ergebnisse im JSON-Format zurückgibt. Idealerweise sollten die Ergebnisse dem [Schemaobjektmodell für Rückschlussmetadaten](/azure/media-services/live-video-analytics-edge/inference-metadata-schema) entsprechen.

## <a name="http-extension-processor"></a>Verarbeitungsknoten für die HTTP-Erweiterung

Der HTTP-Erweiterungsprozessor ermöglicht Erweiterungsszenarien mit dem [HTTP-Protokoll](/azure/media-services/live-video-analytics-edge/http-extension-protocol), bei denen es nicht in erster Linie auf Leistung und/oder optimale Ressourcenverwendung ankommt. Sie können Ihre eigene KI für einen Mediengraphen über einen HTTP-REST-Endpunkt verfügbar machen. 

Der HTTP-Erweiterungsprozessorknoten kann in folgenden Fällen verwendet werden:

* Sie möchten die Interoperabilität mit vorhandenen HTTP-Rückschlusssystemen verbessern.
* Datenübertragung mit geringer Leistung ist akzeptabel.
* Sie möchten eine einfache Anforderung/Antwort-Schnittstelle für Live Video Analytics verwenden.

## <a name="grpc-extension-processor"></a>gRPC-Erweiterungsprozessor

Der gRPC-Erweiterungsprozessor ermöglicht Erweiterungsszenarien mit einem gRPC-basierten, hochleistungsfähigen [strukturierten Protokoll](/azure/media-services/live-video-analytics-edge/grpc-extension-protocol). Er eignet sich perfekt für Szenarien, in denen Leistung und/oder optimale Ressourcennutzung Priorität haben bzw. hat. Mit dem gRPC-Erweiterungsprozessor können Sie in vollem Umfang von den strukturierten Datendefinitionen profitieren. gRPC bietet eine hohe Inhaltsübertragungsleistung. Hierzu wird Folgendes genutzt:

* [Integrierter, gemeinsam genutzter Arbeitsspeicher](https://en.wikipedia.org/wiki/Shared_memory) oder 
* Direktes Einbetten des Inhalts in den Text von gRPC-Nachrichten 

Der gRPC-Erweiterungsprozessor kann zum Senden von Medieneigenschaften sowie zum Austauschen von Rückschlussnachrichten verwendet werden.
Verwenden Sie einen gRPC-Erweiterungsprozessorknoten daher in folgenden Fällen:

* Sie möchten einen strukturierten Vertrag verwenden (beispielsweise strukturierte Nachrichten für Anforderungen und Antworten).
* Sie möchten für die Kommunikation Protokollpuffer ([Protobuf](https://developers.google.com/protocol-buffers)) als zugrunde liegendes Nachrichtenaustauschformat verwenden.
* Sie möchten mit einem gRPC-Server in einer einzelnen Streamsitzung kommunizieren, anstatt das herkömmlichen Anforderung/Antwort-Modell zu verwenden, das einen benutzerdefinierten Anforderungshandler erfordert, um eingehende Anforderungen zu analysieren und die richtigen Implementierungsfunktionen aufzurufen. 
* Sie möchten bei der Kommunikation zwischen Live Video Analytics und Ihrem Modul eine geringe Wartezeit und einen hohen Durchsatz erzielen.

## <a name="use-your-inferencing-model-with-live-video-analytics"></a>Verwenden Ihres Rückschlussmodells mit Live Video Analytics

Mithilfe von Mediengrapherweiterungen können Sie Rückschlussmodelle Ihrer Wahl für jede verfügbare Rückschlussruntime ausführen. Hierzu zählen etwa ONNX, TensorFlow, PyTorch und andere in Ihrem eigenen Docker-Container. Die benutzerdefinierte Rückschlusserweiterung sollte zusammen mit dem Live Video Analytics-Edgemodul bereitgestellt werden, um eine optimale Leistung zu erzielen, und wird dann über den in Ihrer Graphtopologie enthaltenen HTTP- oder gRPC-Erweiterungsprozessor aufgerufen. Darüber hinaus kann die Häufigkeit der Aufrufe für Ihre benutzerdefinierte Erweiterung gedrosselt werden, indem optional dem Medienerweiterungsprozessor ein [Bewegungsmelderprozessor](media-graph-concept.md#motion-detection-processor) vorgeschaltet wird.

Das folgende Diagramm zeigt den Datenfluss im Überblick:

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/media-graph-extension/analyze-live-video-with-AI-inference-service.svg" alt-text="KI-Rückschlussdienst":::

## <a name="samples"></a>Proben

Sie können mithilfe eines unserer Schnellstarts einsteigen, der Live Video Analytics mit dem vordefinierten Erweiterungsdienst mit [HTTP-Erweiterungsprozessor](/azure/media-services/live-video-analytics-edge/use-your-model-quickstart?pivots=programming-language-csharp) oder bei hohen Bildfrequenzen mit dem [gRPC-Erweiterungsprozessor](/azure/media-services/live-video-analytics-edge/analyze-live-video-use-your-grpc-model-quickstart?pivots=programming-language-csharp) veranschaulicht.

Fortgeschrittene Benutzer können sich einige unserer [Jupyter Notebook-Beispiele](https://github.com/Azure/live-video-analytics/blob/master/utilities/video-analysis/notebooks/readme.md) für Live Video Analytics ansehen. In diesen Notebooks finden Sie schrittweise Anleitungen für **die Mediengrapherweiterungen**:

* Erstellen eines Docker-Containerimages eines Erweiterungsdiensts
* Bereitstellen des Erweiterungsdiensts als Container zusammen mit dem Live Video Analytics-Container
* Verwenden eines Live Video Analytics-Mediengraphen mit einem Erweiterungsclient und Verweisen auf den Endpunkt der Erweiterung (HTTP/gRPC)
