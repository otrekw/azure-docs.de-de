---
title: Erstellen und Verwalten einer Compute-Instanz
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie eine Azure Machine Learning-Compute-Instanz erstellen und verwalten. Verwenden Sie es als Ihre Entwicklungsumgebung oder als Computeziel für Entwicklungs-/Testzwecke.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, devx-track-azurecli
ms.author: sgilley
author: sdgilley
ms.reviewer: sgilley
ms.date: 10/02/2020
ms.openlocfilehash: 2778f52b312e5d2fda7879b834fcd204285b7144
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105628950"
---
# <a name="create-and-manage-an-azure-machine-learning-compute-instance"></a>Erstellen und Verwalten einer Azure Machine Learning-Compute-Instanz

Hier erfahren Sie, wie Sie eine [Compute-Instanz](concept-compute-instance.md) in Ihrem Azure Machine Learning-Arbeitsbereich erstellen und verwalten.

Nutzen Sie eine Compute-Instanz als Ihre vollständig konfigurierte und verwaltete Entwicklungsumgebung in der Cloud. Zu Entwicklungs- und Testzwecken können Sie die Instanz auch als [Trainingscomputeziel](concept-compute-target.md#train) oder [Rückschlussziel](concept-compute-target.md#deploy) verwenden.   Computeinstanzen können mehrere Aufträge parallel ausführen und verfügen über eine Auftragswarteschlange. Da es sich bei einer Compute-Instanz um eine Entwicklungsumgebung handelt, kann diese nicht für andere in Ihrem Arbeitsbereich freigegeben werden.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

* Erstellen einer Compute-Instanz 
* Verwalten einer Compute-Instanz (Starten, Beenden, Neu starten und Löschen)
* Zugreifen auf das Terminalfenster 
* Installieren von R- oder Python-Paketen
* Erstellen neuer Umgebungen oder Jupyter-Kernels

Sie können Aufträge sicher in einer [virtuellen Netzwerkumgebung](how-to-secure-training-vnet.md) ausführen, ohne dass Unternehmen hierfür SSH-Ports öffnen müssen. Der Auftrag wird in einer Containerumgebung ausgeführt und packt die Abhängigkeiten Ihres Modells in einen Docker-Container. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

* Die [Azure CLI-Erweiterung für Machine Learning Service](reference-azure-machine-learning-cli.md), das [Azure Machine Learning Python SDK](/python/api/overview/azure/ml/intro) oder die [Visual Studio Code-Erweiterung für Azure Machine Learning](tutorial-setup-vscode-extension.md).

## <a name="create"></a>Erstellen

**Geschätzter Zeitaufwand**: Ca. fünf Minuten.

Das Erstellen einer Compute-Instanz ist ein für Ihren Arbeitsbereich einmaliger Prozess. Sie können die Compute-Instanz als Entwicklungsarbeitsstation oder als Computeziel für das Training wiederverwenden. Sie können mehrere Compute-Instanzen an Ihren Arbeitsbereich anfügen.

Das Kontingent dedizierter Kerne pro Region pro VM-Familie und gesamte regionale Kontingent, das für die Erstellung von Compute-Instanzen gilt, ist einheitlich und wird mit dem Kontingent für Azure Machine Learning-Trainingcomputecluster gemeinsam genutzt. Das Beenden der Compute-Instanz gibt keine Kontingente frei, um sicherzustellen, dass Sie die Compute-Instanz erneut starten können. Beachten Sie, dass die Größe des virtuellen Computers nach Erstellung der Compute-Instanz nicht mehr geändert werden kann.

Das folgende Beispiel veranschaulicht das Erstellen einer Compute-Instanz:

# <a name="python"></a>[Python](#tab/python)

```python
import datetime
import time

from azureml.core.compute import ComputeTarget, ComputeInstance
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your instance
# Compute instance name should be unique across the azure region
compute_name = "ci{}".format(ws._workspace_id)[:10]

# Verify that instance does not exist already
try:
    instance = ComputeInstance(workspace=ws, name=compute_name)
    print('Found existing instance, use it.')
except ComputeTargetException:
    compute_config = ComputeInstance.provisioning_configuration(
        vm_size='STANDARD_D3_V2',
        ssh_public_access=False,
        # vnet_resourcegroup_name='<my-resource-group>',
        # vnet_name='<my-vnet-name>',
        # subnet_name='default',
        # admin_user_ssh_public_key='<my-sshkey>'
    )
    instance = ComputeInstance.create(ws, compute_name, compute_config)
    instance.wait_for_completion(show_output=True)
```

Weitere Informationen zu den in diesem Beispiel verwendeten Klassen, Methoden und Parametern finden Sie in den folgenden Referenzdokumenten:

* [„ComputeInstance“-Klasse](/python/api/azureml-core/azureml.core.compute.computeinstance.computeinstance)
* [ComputeTarget.create](/python/api/azureml-core/azureml.core.compute.computetarget#create-workspace--name--provisioning-configuration-)
* [ComputeInstance.wait_for_completion](/python/api/azureml-core/azureml.core.compute.computeinstance(class)#wait-for-completion-show-output-false--is-delete-operation-false-)


# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az ml computetarget create computeinstance  -n instance -s "STANDARD_D3_V2" -v
```

Weitere Informationen finden Sie in der Referenz zu [az ml computetarget create computeinstance](/cli/azure/ext/azure-cli-ml/ml/computetarget/create#ext_azure_cli_ml_az_ml_computetarget_create_computeinstance).

# <a name="studio"></a>[Studio](#tab/azure-studio)

Erstellen Sie in Ihrem Arbeitsbereich im Azure Machine Learning Studio eine neue Compute-Instanz entweder im Abschnitt **Compute** oder im Abschnitt **Notebooks**, wenn Sie bereit sind, eines Ihrer Notebooks auszuführen.

Weitere Informationen zum Erstellen einer Compute-Instanz in Studio finden Sie unter [Erstellen von Computezielen für Modelltraining und -bereitstellung in Azure Machine Learning Studio](how-to-create-attach-compute-studio.md#compute-instance).

---

Sie können eine Compute-Instanz auch mit einer [Azure Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-computeinstance) erstellen. 

### <a name="create-on-behalf-of-preview"></a>Erstellen im Namen von (Vorschau)

Als Administrator können Sie im Namen einer wissenschaftlichen Fachkraft für Daten eine Compute-Instanz erstellen und ihr die Instanz mit der folgenden Methode zuweisen:
* [Azure Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-computeinstance).  Ausführliche Informationen zum Suchen der in dieser Vorlage benötigten „TenantID“ und „ObjectID“ finden Sie unter [Ermitteln von Identitätsobjekt-IDs für die Authentifizierungskonfiguration](../healthcare-apis/fhir/find-identity-object-ids.md).  Sie können diese Werte auch im Azure Active Directory-Portal abrufen.
* REST-API

Die wissenschaftliche Fachkraft für Daten, für die Sie die Compute-Instanz erstellen, benötigt die folgenden [Azure RBAC-Berechtigungen](../role-based-access-control/overview.md) (rollenbasierte Zugriffssteuerung in Azure): 
* *Microsoft.MachineLearningServices/workspaces/computes/start/action*
* *Microsoft.MachineLearningServices/workspaces/computes/stop/action*
* *Microsoft.MachineLearningServices/workspaces/computes/restart/action*
* *Microsoft.MachineLearningServices/workspaces/computes/applicationaccess/action*

Die wissenschaftliche Fachkraft für Daten kann die Compute-Instanz starten, beenden und neu starten. Sie kann die Compute-Instanz für folgende Zwecke verwenden:
* Jupyter
* JupyterLab
* RStudio
* Integrierte Notebooks

## <a name="manage"></a>Verwalten

Starten, Beenden, Neustarten und Löschen einer Compute-Instanz. Eine Compute-Instanz wird nicht automatisch herunterskaliert. Stellen Sie daher sicher, dass die Ressource beendet wird, um laufende Gebühren zu vermeiden.

> [!TIP]
> Die Compute-Instanz verfügt über einen 120 GB Betriebssystemdatenträger. Wenn Ihnen der Speicherplatz ausgeht, [verwenden Sie das Terminal](how-to-access-terminal.md), um mindestens 1–2 GB zu löschen, bevor Sie die Compute-Instanz beenden oder neu starten.

# <a name="python"></a>[Python](#tab/python)

In den unten stehenden Beispielen lautet der Name der Compute-Instanz **instance**.

* Abrufen des Status

    ```python
    # get_status() gets the latest status of the ComputeInstance target
    instance.get_status()
    ```

* Beenden

    ```python
    # stop() is used to stop the ComputeInstance
    # Stopping ComputeInstance will stop the billing meter and persist the state on the disk.
    # Available Quota will not be changed with this operation.
    instance.stop(wait_for_completion=True, show_output=True)
    ```

* Start

    ```python
    # start() is used to start the ComputeInstance if it is in stopped state
    instance.start(wait_for_completion=True, show_output=True)
    ```

* Neu starten

    ```python
    # restart() is used to restart the ComputeInstance
    instance.restart(wait_for_completion=True, show_output=True)
    ```

* Löschen

    ```python
    # delete() is used to delete the ComputeInstance target. Useful if you want to re-use the compute name 
    instance.delete(wait_for_completion=True, show_output=True)
    ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

In den unten stehenden Beispielen lautet der Name der Compute-Instanz **instance**.

* Beenden

    ```azurecli-interactive
    az ml computetarget stop computeinstance -n instance -v
    ```

    Weitere Informationen finden Sie unter [az ml computetarget stop computeinstance](/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance#ext-azure-cli-ml-az-ml-computetarget-computeinstance-stop).

* Start 

    ```azurecli-interactive
    az ml computetarget start computeinstance -n instance -v
    ```

    Weitere Informationen finden Sie unter [az ml computetarget start computeinstance](/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance#ext-azure-cli-ml-az-ml-computetarget-computeinstance-start).

* Neu starten 

    ```azurecli-interactive
    az ml computetarget restart computeinstance -n instance -v
    ```

    Weitere Informationen finden Sie unter [az ml computetarget restart computeinstance](/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance#ext-azure-cli-ml-az-ml-computetarget-computeinstance-restart).

* Löschen

    ```azurecli-interactive
    az ml computetarget delete -n instance -v
    ```

    Weitere Informationen finden Sie unter [az ml computetarget delete computeinstance](/cli/azure/ext/azure-cli-ml/ml/computetarget#ext-azure-cli-ml-az-ml-computetarget-delete).

# <a name="studio"></a>[Studio](#tab/azure-studio)

Wählen Sie Ihrem Arbeitsbereich in Azure Machine Learning Studio **Compute** aus, und wählen sie dann oben **Compute-Instanz** aus.

![Verwalten einer Compute-Instanz](./media/concept-compute-instance/manage-compute-instance.png)

Sie können folgende Aktionen ausführen:

* Erstellen einer neuen Compute-Instanz 
* Aktualisieren der Registerkarte „Compute-Instanzen“.
* Starten, Beenden und erneutes Starten einer Compute-Instanz.  Sie zahlen für die Instanz, wann immer sie ausgeführt wird. Beenden Sie die Compute-Instanz, wenn Sie sie nicht verwenden, um Kosten zu sparen. Das Beenden einer Compute-Instanz gibt diese frei. Starten Sie sie dann erneut, wenn Sie sie benötigen.
* Löschen einer Compute-Instanz.
* Filtern Sie die Liste der Compute-Instanzen, um nur diejenigen anzuzeigen, die Sie erstellt haben.

Für jede Compute-Instanz in Ihrem Arbeitsbereich, die Sie erstellt haben (oder die für Sie erstellt wurde), können Sie folgende Aktionen ausführen:

* Zugreifen auf Jupyter, JupyterLab, RStudio auf der Computeinstanz
* SSH-Verbindung mit Compute-Instanz. Der SSH-Zugriff ist standardmäßig deaktiviert, kann aber zum Zeitpunkt der Erstellung der Compute-Instanz aktiviert werden. Der SSH-Zugriff erfolgt über einen Mechanismus mit öffentlichem/privatem Schlüssel. Auf der Registerkarte finden Sie Details zur SSH-Verbindung wie IP-Adresse, Benutzername und Portnummer.
* Informieren Sie sich über Details einer bestimmten Compute-Instanz wie IP-Adresse und Region.

---


Mithilfe von [Azure RBAC](../role-based-access-control/overview.md) können Sie steuern, welche Benutzer im Arbeitsbereich eine Compute-Instanz erstellen, löschen, starten, beenden und neu starten können. Alle Benutzer mit der Rolle „Mitwirkender“ und „Besitzer“ des Arbeitsbereichs können Compute-Instanzen im gesamten Arbeitsbereich erstellen, löschen, starten, beenden und neu starten. Allerdings darf nur der Ersteller einer bestimmten Compute-Instanz oder der zugewiesene Benutzer (falls sie in seinem Namen erstellt wurde) auf dieser Compute-Instanz auf Jupyter, JupyterLab und RStudio zugreifen. Eine Compute-Instanz ist für einen einzelnen Benutzer vorgesehen, der über Root-Zugriff und Terminalzugriff über Jupyter/JupyterLab/RStudio verfügt. Die Compute-Instanz verfügt über eine Einzelbenutzeranmeldung, und bei allen Aktionen wird die Identität dieses Benutzers für Azure RBAC und die Zuordnung von Experimentausführungen verwendet. Der SSH-Zugriff wird über einen Mechanismus mit öffentlichem/privatem Schlüssel gesteuert.

Diese Aktionen können von Azure RBAC gesteuert werden:
* *Microsoft.MachineLearningServices/workspaces/computes/read*
* *Microsoft.MachineLearningServices/workspaces/computes/write*
* *Microsoft.MachineLearningServices/workspaces/computes/delete*
* *Microsoft.MachineLearningServices/workspaces/computes/start/action*
* *Microsoft.MachineLearningServices/workspaces/computes/stop/action*
* *Microsoft.MachineLearningServices/workspaces/computes/restart/action*

## <a name="next-steps"></a>Nächste Schritte

* [Zugreifen auf ein Compute-Instanzterminal in Ihrem Arbeitsbereich](how-to-access-terminal.md)
* [Erstellen und Verwalten von Dateien](how-to-manage-files.md)
* [Übermitteln einer Trainingsausführung](how-to-set-up-training-targets.md)
