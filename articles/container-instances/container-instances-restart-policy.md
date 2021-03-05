---
title: Neustartrichtlinie für Aufgaben mit einmaliger Ausführung
description: Hier erfahren Sie, wie Sie mit Azure Container Instances Aufgaben ausführen, die bis zum Abschluss ausgeführt werden, z.B. bei Build-, Test- oder Image-Rendering-Aufträgen.
ms.topic: article
ms.date: 08/11/2020
ms.openlocfilehash: 49280549fa834b82574f81494f1cf44817d8be5d
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102203826"
---
# <a name="run-containerized-tasks-with-restart-policies"></a>Ausführen von Aufgaben in Containern mit Neustartrichtlinien

Da Container in Azure Container Instances sehr schnell und bequem bereitgestellt werden können, ist dies eine ideale Plattform zum Ausführen von einmaligen Aufgaben, wie Erstellen, Testen und Image-Rendering in einer Containerinstanz.

Mit einer konfigurierbaren Neustartrichtlinie können Sie angeben, dass Container nach dem Abschluss ihrer Prozesse beendet werden. Da Containerinstanzen nach Sekunden abgerechnet werden, fallen nur Gebühren für die Computeressourcen an, die beim Ausführen des Containers verwendet werden, der Ihre Aufgabe ausführt.

In den Beispielen in diesem Artikel wird die Azure-Befehlszeilenschnittstelle (Azure CLI) verwendet. Sie benötigen Azure CLI-Version 2.0.21 oder höher mit einer [lokalen Installation][azure-cli-install], oder Sie verwenden die CLI in der [Azure Cloud Shell](../cloud-shell/overview.md).

## <a name="container-restart-policy"></a>Container-Neustartrichtlinie

Wenn Sie eine [Containergruppe](container-instances-container-groups.md) in Azure Container Instances erstellen, können Sie eine von drei Einstellungen für Neustartrichtlinien festlegen.

| Neustartrichtlinie   | Beschreibung |
| ---------------- | :---------- |
| `Always` | Container in der Containergruppe werden immer neu gestartet. Dies ist die **Standardeinstellung**, wenn beim Erstellen des Containers keine Neustartrichtlinie angegeben wird. |
| `Never` | Container in der Containergruppe werden nie neu gestartet. Die Container werden höchstens einmal ausgeführt. |
| `OnFailure` | Container in der Containergruppe werden nur dann neu gestartet, wenn bei dem im Container ausgeführten Prozess ein Fehler auftritt (wenn er also mit einem Exitcode ungleich 0 beendet wird). Die Container werden mindestens einmal ausgeführt. |

[!INCLUDE [container-instances-restart-ip](../../includes/container-instances-restart-ip.md)]

## <a name="specify-a-restart-policy"></a>Angeben einer Neustartrichtlinie

Wie Sie eine Richtlinie für den Neustart angeben, hängt davon ab, wie Sie die Containerinstanzen erstellen, z.B. mit der Azure CLI, Azure PowerShell-Cmdlets oder im Azure-Portal. Geben Sie in der Azure CLI den `--restart-policy`-Parameter an, wenn Sie [az container create][az-container-create] aufrufen.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

## <a name="run-to-completion-example"></a>Beispiel für das Ausführen bis zum Abschluss

Erstellen Sie eine Containerinstanz auf der Grundlage des Microsoft-Images [aci-wordcount][aci-wordcount-image], und geben Sie die Neustartrichtlinie `OnFailure` an, um die Neustartrichtlinie in Aktion zu sehen. Bei diesem Beispielcontainer wird ein Python-Skript ausgeführt, das standardmäßig den Text von Shakespeares [Hamlet](http://shakespeare.mit.edu/hamlet/full.html) analysiert, die 10 häufigsten Wörter an STDOUT schreibt und dann beendet wird.

Führen Sie den Beispielcontainer mit dem folgenden [az container create][az-container-create]-Befehl aus:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure Container Instances startet den Container und beendet ihn dann, wenn die Anwendung (oder wie in diesem Fall das Skript) beendet wird. Wenn Azure Container Instances einen Container beendet, dessen Neustartrichtlinie `Never` oder `OnFailure` lautet, wird der Status des Containers auf **Beendet** festgelegt. Sie können den Status des Containers mit dem Befehl [az container show][az-container-show] überprüfen:

```azurecli-interactive
az container show \
    --resource-group myResourceGroup \
    --name mycontainer \
    --query containers[0].instanceView.currentState.state
```

Beispielausgabe:

```bash
"Terminated"
```

Sobald der Status des Beispielcontainers *Beendet* lautet, können Sie die Taskausgabe in den Containerprotokollen anzeigen. Führen Sie den Befehl [az container logs][az-container-logs] aus, um die Ausgabe des Skripts anzuzeigen:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Ausgabe:

```bash
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```

Dieses Beispiel zeigt die Ausgabe, die das Skript an STDOUT gesendet hat. Die in Containern ausgeführten Aufgaben schreiben ihre Ausgabe allerdings möglicherweise in permanenten Speicher, um sie später abrufen zu können. Dies kann beispielsweise eine [Azure-Dateifreigabe](./container-instances-volume-azure-files.md) sein.

## <a name="next-steps"></a>Nächste Schritte

Aufgabenbasierte Szenarien, z. B. Batchverarbeitung eines großen Datasets mit mehreren Containern, können von benutzerdefinierten [Umgebungsvariablen](container-instances-environment-variables.md) oder [Befehlszeilen](container-instances-start-command.md) zur Laufzeit profitieren.

Details zum dauerhaften Speichern der Ausgabe von Containern, die bis zum Abschluss ausgeführt werden, finden Sie unter [Einbinden einer Azure-Dateifreigabe in Azure Container Instances](./container-instances-volume-azure-files.md).

<!-- LINKS - External -->
[aci-wordcount-image]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/install-azure-cli
