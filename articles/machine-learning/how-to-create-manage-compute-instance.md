---
title: Erstellen und Verwalten einer Compute-Instanz
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie eine Azure Machine Learning-Compute-Instanz erstellen und verwalten. Verwenden Sie es als Ihre Entwicklungsumgebung oder als Computeziel für Entwicklungs-/Testzwecke.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.custom: devx-track-azurecli, references_regions
ms.author: sgilley
author: sdgilley
ms.reviewer: sgilley
ms.date: 10/02/2020
ms.openlocfilehash: db6414ecf4b1b5fcbdf52d59c0c79b72998e610a
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110375214"
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

* Die [Azure CLI-Erweiterung für Machine Learning Service](reference-azure-machine-learning-cli.md), das [Azure Machine Learning Python SDK](/python/api/overview/azure/ml/intro) oder die [Visual Studio Code-Erweiterung für Azure Machine Learning](how-to-setup-vs-code.md).

## <a name="create"></a>Erstellen

> [!IMPORTANT]
> Die unten markierten Elemente (Vorschau) sind aktuell als öffentliche Vorschau verfügbar.
> Die Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

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


# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az ml computetarget create computeinstance  -n instance -s "STANDARD_D3_V2" -v
```

Weitere Informationen finden Sie in der Referenz zu [az ml computetarget create computeinstance](/cli/azure/ml/computetarget/create#az_ml_computetarget_create_computeinstance).

# <a name="studio"></a>[Studio](#tab/azure-studio)

Erstellen Sie in Ihrem Arbeitsbereich im Azure Machine Learning Studio eine neue Compute-Instanz entweder im Abschnitt **Compute** oder im Abschnitt **Notebooks**, wenn Sie bereit sind, eines Ihrer Notebooks auszuführen.

Weitere Informationen zum Erstellen einer Compute-Instanz in Studio finden Sie unter [Erstellen von Computezielen für Modelltraining und -bereitstellung in Azure Machine Learning Studio](how-to-create-attach-compute-studio.md#compute-instance).

---

Sie können eine Compute-Instanz auch mit einer [Azure Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.machinelearningservices/machine-learning-compute-create-computeinstance) erstellen.



## <a name="create-on-behalf-of-preview"></a><a name="on-behalf"></a> Erstellen im Namen von (Vorschau)

Als Administrator können Sie im Namen einer wissenschaftlichen Fachkraft für Daten eine Compute-Instanz erstellen und ihr die Instanz mit der folgenden Methode zuweisen:

* [Azure Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.machinelearningservices/machine-learning-compute-create-computeinstance).  Ausführliche Informationen zum Suchen der in dieser Vorlage benötigten „TenantID“ und „ObjectID“ finden Sie unter [Ermitteln von Identitätsobjekt-IDs für die Authentifizierungskonfiguration](../healthcare-apis/fhir/find-identity-object-ids.md).  Sie können diese Werte auch im Azure Active Directory-Portal abrufen.

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

## <a name="customize-the-compute-instance-with-a-script-preview"></a><a name="setup-script"></a> Anpassen der Compute-Instanz mit einem Skript (Vorschau)

Verwenden Sie ein Setupskript, um die Compute-Instanz zum Zeitpunkt der Bereitstellung automatisch anzupassen und zu konfigurieren. Als Administrator können Sie ein Anpassungsskript schreiben, mit dem alle Compute-Instanzen im Arbeitsbereich gemäß Ihren Anforderungen bereitgestellt werden.

Einige Beispiele für die Verwendungsmöglichkeiten eines Setupskripts:

* Installieren von Paketen, Tools und Software
* Einbinden von Daten
* Erstellen einer benutzerdefinierten Conda-Umgebung und von Jupyter-Kernels
* Klonen von Git-Repositorys und Festlegen der Git-Konfiguration
* Festlegen von Netzwerkproxys
* Festlegen von Umgebungsvariablen
* Installieren von JupyterLab-Erweiterungen

### <a name="create-the-setup-script"></a>Erstellen des Setupskripts

Das Setup-Skript ist ein Shellskript, das als *rootuser* ausgeführt wird.  Erstellen Sie das Skript, oder laden Sie es in Ihre **Notebooks**-Dateien hoch:

1. Melden Sie sich bei [Studio](https://ml.azure.com) an, und wählen Sie Ihren Arbeitsbereich aus.
2. Wählen Sie links **Notebooks** aus.
3. Verwenden Sie das Tool **Dateien hinzufügen**, um das Setupshellskript zu erstellen oder hochzuladen.  Der Skriptdateiname muss unbedingt mit „.sh“ enden.  Wenn Sie eine neue Datei erstellen, ändern Sie auch den **Dateityp** in *bash(.sh)* .

:::image type="content" source="media/how-to-create-manage-compute-instance/create-or-upload-file.png" alt-text="Erstellen oder Hochladen Ihres Setupskripts in die Notebooks-Datei in Studio":::

Wenn das Skript ausgeführt wird, ist das aktuelle Arbeitsverzeichnis des Skriptes das Verzeichnis, in das es hochgeladen wurde. Wenn Sie z. B. das Skript in **Benutzer>admin** hochladen, ist der Speicherort des Skripts auf der Compute-Instanz und des aktuellen Arbeitsverzeichnisses, wenn das Skript ausgeführt wird, */home/azureuser/cloudfiles/code/Users/admin*. Dadurch können Sie relative Pfade im Skript verwenden.

Skriptargumente können im Skript als $1, $2 usw. bezeichnet werden.

Wenn Ihr Skript für azureuser bestimmt war, z. B. die Installation der Conda-Umgebung oder des Jupyter-Kernels, müssen Sie es wie folgt im *sudo -u azureuser*-Block speichern

```shell
sudo -u azureuser -i <<'EOF'

EOF
```
Beachten Sie, dass *sudo -u azureuser* das aktuelle Arbeitsverzeichnis in */home/azureuser* ändert. Sie können auch nicht auf die Skriptargumente in diesem Block zugreifen.

In Ihrem Skript können zudem die folgenden Umgebungsvariablen verwendet werden:

1. CI_RESOURCE_GROUP
2. CI_WORKSPACE
3. CI_NAME
4. CI_LOCAL_UBUNTU_USER. Dies verweist auf azureuser

### <a name="use-the-script-in-the-studio"></a>Verwenden des Skripts in Studio

Nachdem Sie das Skript gespeichert haben, geben Sie es während der Erstellung Ihrer Compute-Instanz an:

1. Melden Sie sich bei [Studio](https://ml.azure.com/) an, und wählen Sie Ihren Arbeitsbereich aus.
1. Wählen Sie links **Compute** aus.
1. Wählen Sie **+Neu** aus, um eine neue Compute-Instanz zu erstellen.
1. [Füllen Sie das Formular aus.](how-to-create-attach-compute-studio.md#compute-instance)
1. Öffnen Sie auf der zweiten Seite des Formulars **Erweiterte Einstellungen anzeigen**.
1. Aktivieren Sie die Option **Provision with setup script** (Mit Setupskript bereitstellen).
1. Navigieren Sie zu dem Shellskript, das Sie gespeichert haben.  Oder laden Sie ein Skript von Ihrem Computer hoch.
1. Fügen Sie nach Bedarf Befehlsargumente hinzu.

:::image type="content" source="media/how-to-create-manage-compute-instance/setup-script.png" alt-text="Bereitstellen einer Compute-Instanz mit einem Setupskript in Studio":::

Beachten Sie Folgendes: Wenn der Arbeitsbereichsspeicher an ein virtuelles Netzwerk angefügt ist, können Sie möglicherweise nicht auf die Setupskriptdatei zugreifen, es sei denn, Sie greifen innerhalb des virtuellen Netzwerks auf Studio zu.

### <a name="use-script-in-a-resource-manager-template"></a>Verwenden eines Skripts in einer Resource Manager-Vorlage

Fügen Sie `setupScripts` zu einer Resource Manager-[Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.machinelearningservices/machine-learning-compute-create-computeinstance) hinzu, um das Setupskript beim Bereitstellen der Compute-Instanz aufzurufen. Beispiel:

```json
"setupScripts":{
    "scripts":{
        "creationScript":{
        "scriptSource":"workspaceStorage",
        "scriptData":"[parameters('creationScript.location')]",
        "scriptArguments":"[parameters('creationScript.cmdArguments')]"
        }
    }
}
```

Sie können das Skript stattdessen inline für eine Resource Manager-Vorlage bereitstellen.  Der Shellbefehl kann auf alle Abhängigkeiten verweisen, die in die Notebooks-Dateifreigabe hochgeladen wurden.  Wenn Sie eine Inlinezeichenfolge verwenden, ist das Arbeitsverzeichnis für das Skript */mnt/batch/tasks/shared/LS_root/mounts/clusters/**ciname**/code/Users*.

Geben Sie beispielsweise eine Base64-codierte Befehlszeichenfolge für `scriptData` an:

```json
"setupScripts":{
    "scripts":{
        "creationScript":{
        "scriptSource":"inline",
        "scriptData":"[base64(parameters('inlineCommand'))]",
        "scriptArguments":"[parameters('creationScript.cmdArguments')]"
        }
    }
}
```

### <a name="setup-script-logs"></a>Setupskriptprotokolle

Protokolle der Setupskriptausführung werden im Protokollordner auf der Detailseite der Compute-Instanz angezeigt. Protokolle werden wieder in Ihrer Notebooks-Dateifreigabe im Ordner „Protokolle\<compute instance name>“ gespeichert. Skriptdatei- und Befehlsargumente für eine bestimmte Compute-Instanz werden auf der Detailseite angezeigt.

## <a name="manage"></a>Verwalten

Starten, Beenden, Neustarten und Löschen einer Compute-Instanz. Eine Compute-Instanz wird nicht automatisch herunterskaliert. Stellen Sie daher sicher, dass die Ressource beendet wird, um laufende Gebühren zu vermeiden. Das Beenden einer Compute-Instanz gibt diese frei. Starten Sie sie dann erneut, wenn Sie sie benötigen. Das Beenden der Compute-Instanz stoppt zwar die Abrechnung der Computestunden, es werden Ihnen aber weiterhin Datenträger, öffentliche IP-Adresse und Standardlastenausgleich in Rechnung gestellt.

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

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

In den unten stehenden Beispielen lautet der Name der Compute-Instanz **instance**.

* Beenden

    ```azurecli-interactive
    az ml computetarget stop computeinstance -n instance -v
    ```

    Weitere Informationen finden Sie unter [az ml computetarget stop computeinstance](/cli/azure/ml/computetarget/computeinstance#az_ml_computetarget_computeinstance_stop).

* Start

    ```azurecli-interactive
    az ml computetarget start computeinstance -n instance -v
    ```

    Weitere Informationen finden Sie unter [az ml computetarget start computeinstance](/cli/azure/ml/computetarget/computeinstance#az_ml_computetarget_computeinstance_start).

* Neu starten

    ```azurecli-interactive
    az ml computetarget restart computeinstance -n instance -v
    ```

    Weitere Informationen finden Sie unter [az ml computetarget restart computeinstance](/cli/azure/ml/computetarget/computeinstance#az_ml_computetarget_computeinstance_restart).

* Löschen

    ```azurecli-interactive
    az ml computetarget delete -n instance -v
    ```

    Weitere Informationen finden Sie unter [az ml computetarget delete computeinstance](/cli/azure/ml/computetarget#az_ml_computetarget_delete).

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

Mithilfe von [Azure RBAC](../role-based-access-control/overview.md) können Sie steuern, welche Benutzer im Arbeitsbereich eine Compute-Instanz erstellen, löschen, starten, beenden und neu starten können. Alle Benutzer mit der Rolle „Mitwirkender“ und „Besitzer“ des Arbeitsbereichs können Compute-Instanzen im gesamten Arbeitsbereich erstellen, löschen, starten, beenden und neu starten. Allerdings darf nur der Ersteller einer bestimmten Compute-Instanz oder der zugewiesene Benutzer (falls sie in seinem Namen erstellt wurde) auf dieser Compute-Instanz auf Jupyter, JupyterLab und RStudio zugreifen. Eine Compute-Instanz ist für einen einzelnen Benutzer vorgesehen, der über Root-Zugriff und Terminalzugriff über Jupyter/JupyterLab/RStudio verfügt. Die Compute-Instanz weist die Einzelbenutzeranmeldung auf, und alle Aktionen verwenden die Identität dieses Benutzers für Azure RBAC und die Zuordnung von Experimentausführungen. Der SSH-Zugriff wird über einen Mechanismus mit öffentlichem/privatem Schlüssel gesteuert.

Diese Aktionen können von Azure RBAC gesteuert werden:
* *Microsoft.MachineLearningServices/workspaces/computes/read*
* *Microsoft.MachineLearningServices/workspaces/computes/write*
* *Microsoft.MachineLearningServices/workspaces/computes/delete*
* *Microsoft.MachineLearningServices/workspaces/computes/start/action*
* *Microsoft.MachineLearningServices/workspaces/computes/stop/action*
* *Microsoft.MachineLearningServices/workspaces/computes/restart/action*

Zum Erstellen einer Compute-Instanz müssen Sie über Berechtigungen für die folgenden Aktionen verfügen:
* *Microsoft.MachineLearningServices/workspaces/computes/write*
* *Microsoft.MachineLearningServices/workspaces/checkComputeNameAvailability/action*


## <a name="next-steps"></a>Nächste Schritte

* [Zugreifen auf ein Compute-Instanzterminal in Ihrem Arbeitsbereich](how-to-access-terminal.md)
* [Erstellen und Verwalten von Dateien](how-to-manage-files.md)
* [Übermitteln einer Trainingsausführung](how-to-set-up-training-targets.md)
