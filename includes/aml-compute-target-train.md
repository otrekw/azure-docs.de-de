---
title: include file
description: include file
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 08/19/2020
ms.openlocfilehash: 54328c7ae4fe2112a718bd82b9f7330c9e95ec69
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2020
ms.locfileid: "89146731"
---
**Computeziele können für mehrere Trainingsaufträge wiederverwendet werden.** Beispielsweise können Sie einen virtuellen Remotecomputer, den Sie an Ihren Arbeitsbereich anfügen, für mehrere Aufträge wiederverwenden.  Verwenden Sie für Machine Learning-Pipelines den entsprechenden [Pipelineschritt](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py) für jedes Computeziel.

|Trainings&nbsp;ziele|[Automatisiertes Machine Learning](../articles/machine-learning/concept-automated-ml.md) | [ML-Pipelines](../articles/machine-learning/concept-ml-pipelines.md) | [Azure Machine Learning-Designer](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Lokaler Computer](../articles/machine-learning/how-to-create-attach-compute-sdk.md#local)| ja | &nbsp; | &nbsp; |
|[Azure Machine Learning Compute-Cluster](../articles/machine-learning/how-to-create-attach-compute-sdk.md#amlcompute)| Ja & <br/>Hyperparameteroptimierung&nbsp; | ja | ja |
|[Azure Machine Learning-Computeinstanz](../articles/machine-learning/how-to-create-attach-compute-sdk.md#instance) | Ja & <br/>Hyperparameteroptimierung | ja |  |
|[Remote-VM](../articles/machine-learning/how-to-create-attach-compute-sdk.md#vm) | Ja & <br/>Hyperparameteroptimierung | ja | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/how-to-create-attach-compute-sdk.md#databricks)| Ja (nur lokaler SDK-Modus) | ja | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/how-to-create-attach-compute-sdk.md#adla) | &nbsp; | ja | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/how-to-create-attach-compute-sdk.md#hdinsight) | &nbsp; | ja | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-create-attach-compute-sdk.md#azbatch) | &nbsp; | ja | &nbsp; |
