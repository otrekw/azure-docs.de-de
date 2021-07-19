---
title: Cognitive Services in Azure Synapse Analytics
description: Reichern Sie Ihre Daten in Azure Synapse Analytics mithilfe von vortrainierten Modellen aus Azure Cognitive Services mit künstlicher Intelligenz (KI) an.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: overview
ms.reviewer: jrasnick, garye, negust, ruxu
ms.date: 06/30/2021
author: garyericson
ms.author: garye
ms.openlocfilehash: c9db8c1cf508d05dfafae672e5f03ff55bb046c0
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113114495"
---
# <a name="cognitive-services-in-azure-synapse-analytics"></a>Cognitive Services in Azure Synapse Analytics

Mit vortrainierten Modellen aus Azure Cognitive Services können Sie Ihre Daten mit künstlicher Intelligenz (KI) in Azure Synapse Analytics anreichern.

[Azure Cognitive Services](/azure/cognitive-services/what-are-cognitive-services) sind cloudbasierte Dienste, die Ihre Daten um kognitive Intelligenz erweitern, auch wenn Sie nicht über KI- oder Data Science-Kenntnisse verfügen. Es gibt verschiedene Möglichkeiten, wie Sie diese Dienste mit Ihren Daten in Synapse Analytics verwenden können:

- Der Cognitive Services-Assistent in Synapse Analytics generiert PySpark-Code in einem Synapse-Notebook, das mithilfe von Daten in einer Spark-Tabelle eine Verbindung mit einem Cognitive Services-Dienst herstellt. Mithilfe vorab trainierter Machine Learning-Modelle fügt der Dienst Ihren Daten dann KI hinzu.

- Im Tutorial [Erstellen von Machine Learning-Anwendungen mithilfe von Microsoft Machine Learning für Apache Spark (Vorschau)](tutorial-build-applications-use-mmlspark.md) wird gezeigt, wie Sie eine Reihe von Diensten aus Cognitive Services mithilfe von Microsoft Machine Learning für Apache Spark (MMLSpark) aufrufen.

- Sie können ausgehend von dem vom Assistenten generierten PySpark-Code oder dem im Tutorial bereitgestellten MMLSpark-Beispielcode eigenen Code schreiben, um andere Dienste von Cognitive Services mit Ihren Daten zu verwenden. Weitere Informationen zu den verfügbaren Diensten finden Sie unter [Was sind Azure Cognitive Services?](/azure/cognitive-services/what-are-cognitive-services).

## <a name="get-started"></a>Erste Schritte

Das Tutorial [Voraussetzungen für die Verwendung von Cognitive Services in Azure Synapse Analytics](tutorial-configure-cognitive-services-synapse.md) führt Sie durch einige Schritte, die Sie ausführen müssen, bevor Sie Cognitive Services in Synapse Analytics verwenden können.

## <a name="tutorials"></a>Tutorials

Die folgenden Tutorials enthalten vollständige Beispiele für die Verwendung von Cognitive Services in Synapse Analytics.

- [Stimmungsanalyse mit Cognitive Services:](tutorial-cognitive-services-sentiment.md) Sie erstellen mithilfe eines Beispieldatasets von Kundenkommentaren eine Spark-Tabelle mit einer Spalte, die die Stimmung der Kommentare in jeder Zeile angibt.

- [Anomalieerkennung mit Cognitive Services:](tutorial-cognitive-services-anomaly.md) Sie erstellen mithilfe eines Beispieldatasets von Zeitreihendaten eine Spark-Tabelle mit einer Spalte, die angibt, ob die Daten in den einzelnen Zeilen Anomalien sind.

- [Erstellen von Machine Learning-Anwendungen mithilfe von Microsoft Machine Learning für Apache Spark (Vorschau):](tutorial-build-applications-use-mmlspark.md) In diesem Tutorial wird veranschaulicht, wie Sie MMLSpark verwenden, um über Cognitive Services auf verschiedene Modelle zuzugreifen.

## <a name="next-steps"></a>Nächste Schritte

- [Machine Learning-Funktionen in Azure Synapse Analytics](what-is-machine-learning.md)
- [Was ist Cognitive Services?](/azure/cognitive-services/what-are-cognitive-services)
- [Verwenden eines Beispielnotebooks aus dem Synapse Analytics-Katalog](quickstart-gallery-sample-notebook.md)