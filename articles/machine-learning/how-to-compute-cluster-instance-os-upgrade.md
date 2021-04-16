---
title: Durchführen eines Upgrades des Hostbetriebssystems für Computecluster und -instanzen
titleSuffix: Azure Machine Learning
description: Führen Sie ein Upgrade des Hostbetriebssystems für Computecluster und Computeinstanzen von Ubuntu 16.04 LTS auf 18.04 LTS durch.
services: machine-learning
author: nishankgu
ms.author: nigup
ms.reviewer: larryfr
ms.service: machine-learning
ms.subservice: core
ms.date: 03/03/2021
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 710a860b1ed87f176b6f42b4963dad17acb323b1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104954053"
---
# <a name="upgrade-compute-instance-and-compute-cluster-host-os"></a>Durchführen eines Upgrades des Hostbetriebssystems für Computeinstanzen und Computecluster

__Computecluster__ und __Computeinstanzen__ von Azure Machine Learning gehören zur verwalteten Computeinfrastruktur. Als verwalteter Dienst verwaltet Microsoft das Hostbetriebssystem sowie die installierten Pakete und Softwareversionen.

Das Hostbetriebssystem für Computecluster und Computeinstanzen war Ubuntu 16.04 LTS. Am **30. April 2021** endet die Unterstützung von Ubuntu 16.04. Microsoft aktualisiert ab dem __15. März 2021__ das Hostbetriebssystem automatisch auf Ubuntu 18.04 LTS. Das Update auf 18.04 stellt sicher, dass weiterhin Sicherheitsupdates und Support durch die Ubuntu-Community erfolgen können. Dieses Update wird in allen Azure-Regionen durchgeführt und ist bis zum __9. April 2021__ in allen Regionen verfügbar. Weitere Informationen zur Einstellung der Unterstützung für Ubuntu 16.04 finden Sie in der [Liste der Ubuntu-Releases](https://wiki.ubuntu.com/Releases) (in englischer Sprache).

> [!TIP]
> * Das Hostbetriebssystem ist nicht die Betriebssystemversion, die Sie für eine [Umgebung](how-to-use-environments.md) angeben, wenn Sie ein Modell trainieren oder bereitstellen. Umgebungen werden in Docker ausgeführt. Docker wird unter dem Hostbetriebssystem ausgeführt.
> * Wenn Sie derzeit für das Training oder die Bereitstellung auf Ubuntu 16.04 basierende Umgebungen verwenden, empfiehlt Microsoft, auf Ubuntu 18.04 basierende Images zu verwenden. Weitere Informationen finden Sie unter [Verwenden von Umgebungen](how-to-use-environments.md) und im [Repository für Azure Machine Learning-Container](https://github.com/Azure/AzureML-Containers/tree/master/base) (in englischer Sprache).
> * Wenn Sie eine auf Ubuntu 18.04 basierende Azure Machine Learning-Computeinstanz verwenden, lautet die Python-Standardversion _Python 3.8_.
## <a name="creating-new-resources"></a>Erstellen neuer Ressourcen

Für nach dem __9. April 2021__ erstellte Computecluster und Computeinstanzen wird als Hostbetriebssystem standardmäßig Ubuntu 18.04 LTS verwendet. Sie keine kein anderes Hostbetriebssystem auswählen.

## <a name="upgrade-existing-resources"></a>Durchführen eines Upgrades vorhandener Ressourcen

Wenn Sie über Computecluster oder Computeinstanzen verfügen, die vor dem __15. März 2021__ erstellt wurden, müssen Sie ein Upgrade des Hostbetriebssystems auf Ubuntu 18.04 durchführen. Abhängig von der Region, in Sie auf Azure Machine Learning zugreifen, wird empfohlen, dies nach dem __9. April 2021__ durchzuführen, um sicherzustellen, dass unsere Änderungen in allen Regionen eingeführt wurden.

* __Azure Machine Learning-Computecluster__:

    * Wenn der Cluster mit __min nodes = 0__ konfiguriert ist, wird das Upgrade des Clusters automatisch durchgeführt, sobald alle Aufträge abgeschlossen sind und die Anzahl der Knoten null beträgt.
    * Wenn __min nodes > 0__ konfiguriert ist, legen Sie vorübergehend „min nodes = 0“ fest, und warten Sie, bis die Clustergröße auf null Knoten reduziert wurde.

    Weitere Informationen zum Ändern der minimalen Knotenanzahl finden Sie in der Dokumentation zum Azure CLI Befehl [az ml computetarget update amlcompute](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/update#ext_azure_cli_ml_az_ml_computetarget_update_amlcompute) und in der SDK-Referenz zu [AmlCompute.update()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#update-min-nodes-none--max-nodes-none--idle-seconds-before-scaledown-none-).

* __Azure Machine Learning-Computeinstanz__: Erstellen Sie eine neue Computeinstanz (die Ubuntu 18.04 verwendet), und löschen Sie die alte Instanz.

    * Über die neue Computeinstanz kann auf alle in der Dateifreigabe des Arbeitsbereichs gespeicherten Notebooks sowie auf alle Datenspeicher und Datasets zugegriffen werden.
    * Wenn Sie benutzerdefinierte Conda-Umgebungen erstellt haben, können Sie diese Umgebungen aus der vorhandenen Instanz exportieren und in der neuen Instanz importieren. Weitere Informationen zum Conda-Export und -Import finden Sie in der [Conda-Dokumentation](https://docs.conda.io/).

    Weitere Informationen finden Sie in den Artikeln [Was ist eine Azure Machine Learning-Compute-Instanz?](concept-compute-instance.md) und [Erstellen und Verwalten einer Azure Machine Learning Computeinstanz](how-to-create-manage-compute-instance.md).

## <a name="check-host-os-version"></a>Überprüfen der Version des Hostbetriebssystems

Informationen zum Überprüfen der Version des Hostbetriebssystems finden Sie auf der Wiki-Seite der Ubuntu-Community unter [Checking your Ubuntu version](https://help.ubuntu.com/community/CheckingYourUbuntuVersion) (Überprüfen Ihrer Ubuntu-Version).

> [!TIP]
> Für den Befehl `lsb_release -a` aus dem Wiki-Artikel können Sie [eine Terminalsitzung in einer Computeinstanz verwenden](how-to-access-terminal.md).
## <a name="next-steps"></a>Nächste Schritte

Bei weiteren Fragen oder Bedenken können Sie über [ubuntu18azureml@service.microsoft.com](mailto:ubuntu18azureml@service.microsoft.com) Kontakt mit uns aufnehmen.
