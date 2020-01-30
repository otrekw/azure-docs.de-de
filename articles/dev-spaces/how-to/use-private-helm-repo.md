---
title: Verwenden eines privaten Helm-Repositorys in Azure Dev Spaces
services: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 08/22/2019
ms.topic: conceptual
description: Verwenden eines privaten Helm-Repositorys in einem Azure Dev Space.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, Container, Helm
manager: gwallace
ms.openlocfilehash: b1579adc00540a429170027b66c5d3e508bcb5d4
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76718735"
---
# <a name="use-a-private-helm-repository-in-azure-dev-spaces"></a>Verwenden eines privaten Helm-Repositorys in Azure Dev Spaces

[Helm][helm] ist ein Paket-Manager für Kuberentes. Helm verwendet ein [Diagramm][helm-chart]format, um Abhängigkeiten zu packen. Helm-Diagramme werden in einem Repository gespeichert, das öffentlich oder privat sein kann. Azure Dev Spaces ruft Helm-Diagramme nur bei der Ausführung Ihrer Anwendung aus öffentlichen Repositorys ab. In Fällen, in denen das Helm-Repository privat ist oder Azure Dev Spaces nicht darauf zugreifen kann, können Sie ein Diagramm aus diesem Repository direkt zu Ihrer Anwendung hinzufügen. Wenn Sie das Diagramm direkt hinzufügen, kann Azure Dev Spaces Ihre Anwendung ausführen, ohne auf das private Helm-Repository zugreifen zu müssen.

## <a name="add-the-private-helm-repository-to-your-local-machine"></a>Hinzufügen des privaten Helm-Repositorys zu Ihrem lokalen Computer

Verwenden Sie [helm repo add][helm-repo-add] und [helm repo update][helm-repo-update], um von Ihrem lokalen Computer aus auf das private Helm-Repository zuzugreifen.

```cmd
helm repo add privateRepoName http://example.com/helm/v1/repo --username user --password 5tr0ng_P@ssw0rd!
helm repo update
```

## <a name="add-the-chart-to-your-application"></a>Hinzufügen des Diagramms zu Ihrer Anwendung

Navigieren Sie zum Verzeichnis Ihres Projekts, und führen Sie `azds prep` aus.

```cmd
azds prep --public
```

> [!TIP]
> Der Befehl `prep` versucht, [ein Dockerfile und Helm-Chart](../how-dev-spaces-works.md#prepare-your-code) für Ihr Projekt zu generieren. Azure Dev Spaces nutzt diese Dateien zum Erstellen und Ausführen Ihres Codes. Sie können sie jedoch ändern, wenn Sie anpassen möchten, wie das Projekt erstellt und ausgeführt wird.

Erstellen Sie eine Datei [requirements.yaml][helm-requirements] mit Ihrem Diagramm im Diagrammverzeichnis Ihrer Anwendung. Wenn Ihre Anwendung beispielsweise *app1* heißt, würden Sie *charts/app1/requirements.yaml* erstellen.

```yaml
dependencies:
    - name: mychart
      version: 0.1.0
      repository:  http://example.com/helm/v1/repo
```

Navigieren Sie zum Diagrammverzeichnis Ihrer Anwendung, und verwenden Sie [helm dependency update][helm-dependency-update], um die Helm-Abhängigkeiten für Ihre Anwendung zu aktualisieren und das Diagramm aus dem privaten Repository herunterzuladen.

```cmd
helm dependency update
```

Überprüfen Sie, ob dem Diagrammverzeichnis Ihrer Anwendung ein Unterverzeichnis *charts* (Diagramme) mit einer *tgz*-Datei hinzugefügt wurde. Beispiel: *charts/app1/charts/mychart-0.1.0.tgz*.

Das Diagramm aus Ihrem privaten Helm-Repository wurde heruntergeladen und Ihrem Projekt hinzugefügt. Entfernen Sie die Datei *requirements.yaml*, damit Dev Spaces nicht versucht, diese Abhängigkeit zu aktualisieren.

## <a name="run-your-application"></a>Ausführen der Anwendung

Navigieren Sie zum Stammverzeichnis Ihres Projekts, und führen Sie `azds up` aus, um zu überprüfen, ob Ihre Anwendung erfolgreich in Ihrem Dev Space ausgeführt wird.

```cmd
$ azds up
Using dev space 'default' with target 'MyAKS'
Synchronizing files...2s
Installing Helm chart...2s
Waiting for container image build...2m 25s
Building container image...
...
Service 'app1' port 'http' is available at http://app1.1234567890abcdef1234.eus.azds.io/
Service 'app1' port 80 (http) is available at http://localhost:54256
...
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Helm und seine Funktionsweise][helm].

[helm]: https://docs.helm.sh
[helm-chart]: https://helm.sh/docs/topics/charts/
[helm-dependency-update]: https://helm.sh/docs/topics/charts/#managing-dependencies-with-the-dependencies-field
[helm-repo-add]: https://helm.sh/docs/intro/using_helm/#helm-repo-working-with-repositories
[helm-repo-update]: https://helm.sh/docs/intro/using_helm/#helm-repo-working-with-repositories
[helm-requirements]: https://helm.sh/docs/topics/charts/#chart-dependencies