---
title: 'Analysieren von Livevideos mithilfe eines eigenen gRPC-Modells: Azure'
description: In diesem Schnellstart wenden Sie maschinelles Sehen an, um den Livevideofeed von einer (simulierten) IP-Kamera zu analysieren.
ms.topic: quickstart
ms.date: 08/14/2020
zone_pivot_groups: ams-lva-edge-programming-languages
ms.openlocfilehash: 907d402a4b0ef5d5e437322d02431a0ccd2d8da3
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89421522"
---
# <a name="quickstart-analyze-live-video-by-using-your-own-grpc-model"></a>Schnellstart: Analysieren von Livevideos mithilfe eines eigenen gRPC-Modells

In dieser Schnellstartanleitung wird beschrieben, wie Sie mithilfe von Live Video Analytics in IoT Edge einen Livevideofeed einer (simulierten) IP-Kamera analysieren. Sie erfahren, wie Sie ein Modell für maschinelles Sehen anwenden, um Objekte zu erkennen. Eine Teilmenge der Einzelbilder des Livevideofeeds wird an einen Rückschlussdienst gesendet. Die Ergebnisse werden an IoT Edge Hub gesendet.

::: zone pivot="programming-language-csharp"
[!INCLUDE [header](includes/analyze-live-video-your-grpc-model-quickstart/csharp/header.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [header](includes/analyze-live-video-your-grpc-model-quickstart/python/header.md)]
::: zone-end

## <a name="prerequisites"></a>Voraussetzungen

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/analyze-live-video-your-grpc-model-quickstart/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/analyze-live-video-your-grpc-model-quickstart/python/prerequisites.md)]
::: zone-end

## <a name="review-the-sample-video"></a>Überprüfen des Beispielvideos

::: zone pivot="programming-language-csharp"
[!INCLUDE [review sample video](includes/analyze-live-video-your-grpc-model-quickstart/csharp/review-sample-video.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [review sample video](includes/analyze-live-video-your-grpc-model-quickstart/python/review-sample-video.md)]
::: zone-end

## <a name="overview"></a>Übersicht

::: zone pivot="programming-language-csharp"
[!INCLUDE [overview](includes/analyze-live-video-your-grpc-model-quickstart/csharp/overview.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [overview](includes/analyze-live-video-your-grpc-model-quickstart/python/overview.md)]
::: zone-end

## <a name="create-and-deploy-the-media-graph"></a>Erstellen und Bereitstellen des Mediengraphen

::: zone pivot="programming-language-csharp"
[!INCLUDE [create and deploy the media graph](includes/analyze-live-video-your-grpc-model-quickstart/csharp/create-deploy-media-graph.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [create and deploy the media graph](includes/analyze-live-video-your-grpc-model-quickstart/python/create-deploy-media-graph.md)]
::: zone-end

## <a name="interpret-results"></a>Interpretieren von Ergebnissen

::: zone pivot="programming-language-csharp"
[!INCLUDE [interpret results](includes/analyze-live-video-your-grpc-model-quickstart/csharp/interpret-results.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [interpret results](includes/analyze-live-video-your-grpc-model-quickstart/python/interpret-results.md)]
::: zone-end

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie weitere Schnellstartanleitungen durcharbeiten möchten, sollten Sie die von Ihnen erstellten Ressourcen beibehalten. Navigieren Sie andernfalls zum Azure-Portal, dort zu Ihren Ressourcengruppen, wählen Sie die Ressourcengruppe aus, unter der Sie diese Schnellstartanleitung ausgeführt haben, und löschen Sie alle Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

* Versuchen Sie, mithilfe des gRPC-Protokolls verschiedene Mediendiagrammtopologien auszuführen.
* **Erstellen und Ausführen von Live Video Analytics-Beispielerweiterungen**
<br/>Testen Sie unsere Jupyter-Beispielnotebooks, mit denen Sie [ONNX](http://onnx.ai/)-basierte YOLO-Modelle als LVA-Erweiterung (Live Video Analytics) erstellen und ausführen können.
    * [YOLOv3-Beispielmodell](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/notebooks/Yolo/yolov3/yolov3-grpc-icpu-onnx/readme.md)
    * [YOLOv4-Beispielmodell](https://github.com/Azure/live-video-analytics/blob/master/utilities/video-analysis/notebooks/Yolo/yolov4/yolov4-grpc-icpu-onnx/readme.md)

