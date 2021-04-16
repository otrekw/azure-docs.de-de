---
title: Funktionsweise der Vorbereitung eines Projekts für Azure Dev Spaces
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Beschreibt die Funktionsweise der Vorbereitung eines Projekts für Azure Dev Spaces.
keywords: azds.yaml, Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: d2da69dd8a8c2683ff584dfd0ffc61cb023f2ece
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91968156"
---
# <a name="how-preparing-a-project-for-azure-dev-spaces-works"></a>Funktionsweise der Vorbereitung eines Projekts für Azure Dev Spaces

[!INCLUDE [Azure Dev Spaces deprecation](../../includes/dev-spaces-deprecation.md)]

Azure Dev Spaces bietet Ihnen mehrere Möglichkeiten, Kubernetes-Anwendungen schnell zu durchlaufen und zu debuggen und mit Ihrem Team in einem Azure Kubernetes Service-Cluster (AKS) zusammenzuarbeiten. Dev Spaces kann Dockerfile-Dateien und Helm-Diagramme für Ihr Projekt generieren. Dev Spaces erstellt und verwendet auch eine Konfigurationsdatei, um Kubernetes-Anwendungen in AKS bereitzustellen, auszuführen und zu debuggen. Alle diese Dateien werden zusammen mit dem Code Ihrer Anwendung gespeichert und können Ihrem Versionskontrollsystem hinzugefügt werden.

In diesem Artikel wird beschrieben, was geschieht, wenn Sie Ihr Projekt für die Ausführung in AKS mit Dev Spaces vorbereiten.

## <a name="prepare-your-code"></a>Vorbereiten Ihres Codes

Um Ihre Anwendung in einem Entwicklungsbereich ausführen zu können, muss sie containerisiert sein, und Sie müssen definieren, wie sie in Kubernetes bereitgestellt werden soll. Um Ihre Anwendung zu containerisieren, benötigen Sie eine Dockerfile-Datei. Um zu definieren, wie Ihre Anwendung in Kubernetes bereitgestellt wird, benötigen Sie ein [Helm-Chart](https://docs.helm.sh/). Als Unterstützung für ein Erstellen der Dockerfile-Datei und des Helm-Charts für Ihre Anwendung stellen die clientseitigen Tools den `prep`-Befehl bereit:

```cmd
azds prep --enable-ingress
```

Der `prep`-Befehl analysiert die Dateien in Ihrem Projekt und versucht, die Dockerfile-Datei und das Helm-Chart zu erstellen, um Ihre Anwendung in Kubernetes auszuführen. Derzeit generiert der `prep`-Befehl eine Dockerfile-Datei und ein Helm-Chart mit den folgenden Sprachen:

* Java
* Node.js
* .NET Core

Sie *müssen* den `prep`-Befehl aus einem Verzeichnis ausführen, das Quellcode enthält. Wird der `prep`-Befehl aus dem richtigen Verzeichnis ausgeführt, können die clientseitigen Tools die Sprache erkennen und eine entsprechende Dockerfile-Datei erstellen, um Ihre Anwendung zu containerisieren. Sie können den `prep`-Befehl auch aus einem Verzeichnis ausführen, das eine *pom.xml*-Datei für Java-Projekte enthält.

Wenn Sie den `prep`-Befehl aus einem Verzeichnis ausführen, das keinen Quellcode enthält, erstellen die clientseitigen Tools keine Dockerfile-Datei. Außerdem wird der folgende Fehler angezeigt: *Aufgrund einer nicht unterstützten Sprache konnte kein Dockerfile generiert werden.* Dieser Fehler tritt auch auf, wenn die clientseitigen Tools den Projekttyp nicht erkennen können.

Wenn Sie den `prep`-Befehl ausführen, können Sie das Flag `--enable-ingress` angeben. Dieses Flag weist den Controller an, einen über das Internet zugänglichen Endpunkt für diesen Dienst zu erstellen. Wenn Sie dieses Flag nicht angeben, kann nur aus dem Cluster oder über den localhost-Tunnel, der von den clientseitigen Tools erstellt wurde, auf den Dienst zugegriffen werden. Sie können dieses Verhalten nach dem Ausführen des `prep`-Befehls aktivieren oder deaktivieren, indem Sie das generierte Helm-Chart aktualisieren.

Der `prep`-Befehl ersetzt keine vorhandenen Dockerfile-Dateien oder Helm-Charts, die es in Ihrem Projekt gibt. Wurde für eine vorhandene Dockerfile-Datei oder ein vorhandenes Helm-Chart dieselbe Namenskonvention verwendet wie für die Dateien, die vom `prep`-Befehl generiert werden, überspringt der `prep`-Befehl diese Dateien. Andernfalls generiert der `prep`-Befehl seine eigene Dockerfile-Datei oder sein eigenes Helm-Chart neben den vorhandenen Dateien.

> [!IMPORTANT]
> Azure Dev Spaces verwendet die Dockerfile-Datei und das Helm-Diagramm für Ihr Projekt zum Erstellen und Ausführen Ihres Codes. Sie können diese Dateien jedoch ändern, wenn Sie anpassen möchten, wie das Projekt erstellt und ausgeführt wird.

Außerdem generiert der `prep`-Befehl eine `azds.yaml`-Datei im Stammverzeichnis Ihres Projekts. In Azure Dev Spaces wird diese Datei verwendet, um Ihre Anwendung zu erstellen, zu konfigurieren und auszuführen. In dieser Konfigurationsdatei werden der Speicherort Ihrer Dockerfile-Datei und Ihres Helm-Charts aufgelistet sowie aufsetzend auf diesen Artefakten weitere Konfigurationsoptionen bereitgestellt.

Es folgt eine „azds.yaml“-Beispieldatei, die mit einer [.NET Core-Beispielanwendung](https://github.com/Azure/dev-spaces/tree/master/samples/dotnetcore/getting-started/webfrontend) erstellt wurde:

```yaml
kind: helm-release
apiVersion: 1.1
build:
  context: .
  dockerfile: Dockerfile
install:
  chart: charts/webfrontend
  values:
  - values.dev.yaml?
  - secrets.dev.yaml?
  set:
    replicaCount: 1
    image:
      repository: webfrontend
      tag: $(tag)
      pullPolicy: Never
    ingress:
      annotations:
        kubernetes.io/ingress.class: traefik-azds
      hosts:
        # This expands to [space.s.][rootSpace.]webfrontend.<random suffix>.<region>.azds.io
        # Customize the public URL by changing the 'webfrontend' text between the $(rootSpacePrefix) and $(hostSuffix) tokens
        # For more information see https://aka.ms/devspaces/routing
        - $(spacePrefix)$(rootSpacePrefix)webfrontend$(hostSuffix)
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
      useGitIgnore: true
      args:
        BUILD_CONFIGURATION: ${BUILD_CONFIGURATION:-Debug}
    container:
      sync:
      - "**/Pages/**"
      - "**/Views/**"
      - "**/wwwroot/**"
      - "!**/*.{sln,csproj}"
      command: [dotnet, run, --no-restore, --no-build, --no-launch-profile, -c, "${BUILD_CONFIGURATION:-Debug}"]
      iterate:
        processesToKill: [dotnet, vsdbg]
        buildCommands:
        - [dotnet, build, --no-restore, -c, "${BUILD_CONFIGURATION:-Debug}"]
```

Die `azds.yaml`-Datei, die durch den `prep`-Befehl generiert wurde, sollte für ein einfaches Einzelprojekt-Entwicklungsszenario funktionieren. Ist Ihr spezielles Projekt deutlich komplexer, müssen Sie diese Datei möglicherweise aktualisieren, nachdem Sie den `prep`-Befehl ausgeführt haben. Beispielsweise kann es sein, dass für Ihr Projekt entsprechend Ihren Entwicklungs- oder Debuganforderungen einige Änderungen an Ihrem Build- oder Startprozess erforderlich sind. Außerdem haben Sie möglicherweise mehrere Anwendungen in Ihrem Projekt, die mehrere Buildprozesse oder unterschiedliche Buildinhalte erfordern.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Ausführen von Code in Ihrem Entwicklungsbereich finden Sie unter [Funktionsweise des Ausführens von Code mit Azure Dev Spaces][how-it-works-up].

[how-it-works-up]: how-dev-spaces-works-up.md