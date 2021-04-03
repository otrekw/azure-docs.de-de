---
title: Beispiele von ACR Tasks
description: Beispiel von ACR Tasks (Azure Container Registry Tasks) zum Erstellen, Ausführen und Patchen von Containerimages
ms.topic: article
ms.date: 11/14/2019
ms.openlocfilehash: 49df3bf565052a729ac3c587bd2ba11a299d05f1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "74456080"
---
# <a name="acr-tasks-samples"></a>Beispiele von ACR Tasks

Dieser Artikel verweist auf `task.yaml`-Beispieldateien und zugehörige Dockerfiles für mehrere Szenarios von [Azure Container Registry Tasks](container-registry-tasks-overview.md) (ACR Tasks). 

Zusätzliche Beispiele hierfür finden Sie im [Repository mit den Codebeispielen][task-examples] für Azure.

## <a name="scenarios"></a>Szenarien

* **Erstellen von Images** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Ausführen eines Containers** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml)

* **Erstellen und Übertragen eines Images** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Erstellen und Ausführen eines Images** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Erstellen und Übertragen mehrerer Images** -  [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world-multi.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Paralleles Erstellen und Testen von Images** -  [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Erstellen und Übertragen von Images in mehrere Registrierungen** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/multipleRegistries/testtask.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/multipleRegistries/hello-world.dockerfile)


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über ACR-Tasks:

* [Tasks mit mehreren Schritten](container-registry-tasks-multi-step.md): ACR-Task-basierte Workflows zum Erstellen, Testen und Patchen von Containerimages in der Cloud.
* [Taskreferenz](container-registry-tasks-reference-yaml.md): Arten von Taskschritten, zugehörige Eigenschaften und Verwendung
* [Cmd-Repository](https://github.com/AzureCR/cmd): Eine Sammlung von Containern als Befehle für ACR-Tasks.


<!-- LINKS - External -->
[task-examples]: https://github.com/Azure-Samples/acr-tasks
