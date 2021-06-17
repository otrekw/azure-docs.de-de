---
title: Einrichten von Data Science-Umgebungen in Azure – Team Data Science-Prozess
description: Einrichten von Data Science-Umgebungen in Azure für die Verwendung im Team Data Science-Prozess.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 8d8244384abe42819fed61329409e150d334c8d8
ms.sourcegitcommit: a434cfeee5f4ed01d6df897d01e569e213ad1e6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111814305"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Einrichten von Data Science-Umgebungen für die Verwendung im Team Data Science-Prozess
Der Team Data Science-Prozess verwendet verschiedene Data Science-Umgebungen für die Speicherung, Verarbeitung und Analyse von Daten. Dazu zählen Azure Blob Storage, verschiedene Typen von virtuellen Azure-Computern, HDInsight (Hadoop)-Cluster und Azure Machine Learning-Arbeitsbereiche. Die Entscheidung, welche Art von Umgebung Sie verwenden, hängt von der Art und der Menge der Daten ab, die modelliert werden sollen, sowie vom Ziel für die Daten in der Cloud. 

* Hilfe bei dieser Entscheidung finden Sie unter [Planen der Azure Machine Learning Data Science-Umgebung](plan-your-environment.md). 
* Einen Katalog mit einigen Szenarien für die erweiterte Analyse finden Sie unter [Szenarien für den Team Data Science-Prozess](plan-sample-scenarios.md)

In den folgenden Artikeln wird das Einrichten der verschiedenen Data Science-Umgebungen beschrieben, die vom Team Data Science-Prozess verwendet werden.

* [Azure Storage-Konto](../../storage/common/storage-account-create.md)
* R und Python in HDInsight-Clustern
  * [Einführung in Spark in HDInsight](../../hdinsight/spark/apache-spark-overview.md)
  * [HDInsight (Hadoop)-Cluster](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md)
  * [PySpark-Kernel für Jupyter Notebook](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md)
  * [R Server auf HDInsight](../../hdinsight/r-server/r-server-overview.md)
  * [Erste Schritte mit R Server in HDInsight](../../hdinsight/r-server/r-server-overview.md)
* [Azure Machine Learning Studio-Arbeitsbereich (klassisch)](../classic/create-workspace.md)

Die **Microsoft Data Science Virtual Machine (DSVM)** ist auch als Image des virtuellen Azure-Computers (VM) verfügbar. Diese VM ist vorinstalliert und mit einigen gängigen Tools konfiguriert, die häufig für Datenanalysen und Machine Learning verwendet werden. Die DSVM steht unter Windows und Linux zur Verfügung. Weitere Informationen finden Sie unter [Einführung in den cloudbasierten virtuellen Computer für Data Science für Linux und Windows](../data-science-virtual-machine/overview.md).

Informieren Sie sich darüber, wie Sie Folgendes erstellen:

- [DSVM unter Windows](../data-science-virtual-machine/provision-vm.md)
- [DSVM unter Ubuntu](../data-science-virtual-machine/dsvm-ubuntu-intro.md)
- [DSVM unter CentOS](../data-science-virtual-machine/release-notes.md)