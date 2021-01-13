---
title: 'Tutorial zu Kubernetes in Azure: Vorbereiten einer Anwendung'
description: In diesem Azure Kubernetes Service-Tutorial (AKS) erfahren Sie, wie mit Docker Compose eine App mit mehreren Containern vorbereitet und erstellt wird, die Sie dann in AKS bereitstellen können.
services: container-service
ms.topic: tutorial
ms.date: 09/30/2020
ms.custom: mvc
ms.openlocfilehash: 15bf29c676c4ca41fc2d005f3500a89ed6b9c380
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91576335"
---
# <a name="tutorial-prepare-an-application-for-azure-kubernetes-service-aks"></a>Tutorial: Vorbereiten einer Anwendung für Azure Kubernetes Service (AKS)

In diesem Tutorial – Teil 1 von 7 – wird eine Anwendung mit mehreren Containern für die Verwendung in Kubernetes vorbereitet. Vorhandene Entwicklungstools wie Docker Compose dienen zum lokalen Erstellen und Testen einer Anwendung. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Klonen einer Beispielanwendungsquelle von GitHub
> * Erstellen eines Containerimages aus der Beispielanwendungsquelle
> * Testen der Anwendung mit mehreren Containern in einer lokalen Docker-Umgebung

Nach Abschluss wird die Anwendung in Ihrer lokalen Entwicklungsumgebung ausgeführt:

![Abbildung: Kubernetes-Cluster in Azure](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

In weiteren Tutorials wird das Containerimage in eine Azure Container Registry-Instanz hochgeladen und anschließend in einem AKS-Cluster bereitgestellt.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass zentrale Docker-Konzepte wie Container und Containerimages sowie `docker`-Befehle bekannt sind. Eine Einführung in Container finden Sie bei Bedarf unter [Get started with Docker][docker-get-started] (Erste Schritte mit Docker).

Für dieses Tutorial ist eine lokale Docker-Entwicklungsumgebung mit Linux-Containern erforderlich. Für Docker sind Pakete erhältlich, mit denen Docker in einem [Mac][docker-for-mac]-, [Windows][docker-for-windows]- oder [Linux][docker-for-linux]-System konfiguriert werden kann.

Azure Cloud Shell umfasst keine Docker-Komponenten, die zum Abschließen der einzelnen Schritte dieser Tutorials erforderlich sind. Aus diesem Grund wird empfohlen, eine vollständige Docker-Entwicklungsumgebung zu verwenden.

## <a name="get-application-code"></a>Abrufen von Anwendungscode

Die in diesem Tutorial verwendete Beispielanwendung ist eine einfache Abstimmungs-App. Die Anwendung besteht aus einer Front-End-Webkomponente und einer Back-End-Redis-Instanz. Die Webkomponente wird in ein benutzerdefiniertes Containerimage gepackt. Die Redis-Instanz verwendet ein nicht modifiziertes Image aus Docker Hub.

Verwenden Sie [Git][], um die Beispielanwendung in Ihrer Entwicklungsumgebung zu klonen:

```console
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Wechseln Sie in das geklonte Verzeichnis.

```console
cd azure-voting-app-redis
```

Im Verzeichnis befinden sich der Anwendungsquellcode, eine vorab erstellte Docker Compose-Datei und eine Kubernetes-Manifestdatei. Diese Dateien werden während des Tutorials verwendet.

## <a name="create-container-images"></a>Erstellen von Containerimages

[Docker Compose][docker-compose] kann verwendet werden, um die Erstellung von Containerimages und die Bereitstellung von Anwendungen mit mehreren Containern zu automatisieren.

Verwenden Sie die Beispieldatei `docker-compose.yaml`, um das Containerimage zu erstellen, das Redis-Image herunterzuladen und die Anwendung zu starten:

```console
docker-compose up -d
```

Verwenden Sie anschließend den Befehl [docker-images][docker-images], um die erstellten Images anzuzeigen. Drei Images wurden heruntergeladen oder erstellt. Das Image *azure-vote-front* enthält die Front-End-Anwendung und verwendet das *nginx-flask*-Image als Grundlage. Das *redis*-Image wird zum Starten einer Redis-Instanz verwendet.

```
$ docker images

REPOSITORY                                     TAG                 IMAGE ID            CREATED             SIZE
mcr.microsoft.com/azuredocs/azure-vote-front   v1                  84b41c268ad9        9 seconds ago       944MB
mcr.microsoft.com/oss/bitnami/redis            6.0.8               3a54a920bb6c        2 days ago          103MB
tiangolo/uwsgi-nginx-flask                     python3.6           a16ce562e863        6 weeks ago         944MB
```

Führen Sie den Befehl [docker ps][docker-ps] aus, um die ausgeführten Container anzuzeigen:

```
$ docker ps

CONTAINER ID        IMAGE                                             COMMAND                  CREATED             STATUS              PORTS                           NAMES
d10e5244f237        mcr.microsoft.com/azuredocs/azure-vote-front:v1   "/entrypoint.sh /sta…"   3 minutes ago       Up 3 minutes        443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
21574cb38c1f        mcr.microsoft.com/oss/bitnami/redis:6.0.8         "/opt/bitnami/script…"   3 minutes ago       Up 3 minutes        0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Lokales Testen der Anwendung

Zum Anzeigen der ausgeführten Anwendungen geben Sie `http://localhost:8080` in einem lokalen Webbrowser ein. Die Beispielanwendung wird wie im folgenden Beispiel geladen:

![Abbildung: Kubernetes-Cluster in Azure](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nach der Überprüfung der Funktionsfähigkeit der Anwendung können die ausgeführten Container beendet und entfernt werden. Löschen Sie die Containerimages nicht. Im nächsten Tutorial wird das Image *azure-vote-front* in eine Azure Container Registry-Instanz hochgeladen.

Beenden und entfernen Sie die Containerinstanzen und -ressourcen mit dem Befehl [docker-compose down][docker-compose-down]:

```console
docker-compose down
```

Wenn die lokale Anwendung entfernt wurde, verfügen Sie über ein Docker-Image, das die Azure Vote-Anwendung *azure-vote-front* für die Verwendung im nächsten Tutorial enthält.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde eine Anwendung getestet und es wurden Containerimages für die Anwendung erstellt. Sie haben Folgendes gelernt:

> [!div class="checklist"]
> * Klonen einer Beispielanwendungsquelle von GitHub
> * Erstellen eines Containerimages aus der Beispielanwendungsquelle
> * Testen der Anwendung mit mehreren Containern in einer lokalen Docker-Umgebung

Fahren Sie mit dem nächsten Tutorial fort, und erfahren Sie, wie Containerimages in Azure Container Registry gespeichert werden.

> [!div class="nextstepaction"]
> [Übertragen von Images zu Azure Container Registry mithilfe von Push][aks-tutorial-prepare-acr]

<!-- LINKS - external -->
[docker-compose]: https://docs.docker.com/compose/
[docker-for-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-for-mac]: https://docs.docker.com/docker-for-mac/
[docker-for-windows]: https://docs.docker.com/docker-for-windows/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-ps]: https://docs.docker.com/engine/reference/commandline/ps/
[docker-compose-down]: https://docs.docker.com/compose/reference/down
[git]: https://git-scm.com/downloads

<!-- LINKS - internal -->
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
