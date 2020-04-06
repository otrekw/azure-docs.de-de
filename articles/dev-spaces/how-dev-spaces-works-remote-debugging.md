---
title: So funktioniert das Remotedebuggen Ihres Codes mit Azure Dev Spaces
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Beschreibt die Prozesse für das Remotedebuggen in Azure Kubernetes Service mit Azure Dev Spaces
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: a9f303ea6f954285a319be137a594f7ce127f740
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241244"
---
# <a name="how-remote-debugging-your-code-with-azure-dev-spaces-works"></a>So funktioniert das Remotedebuggen Ihres Codes mit Azure Dev Spaces

Azure Dev Spaces bietet Ihnen mehrere Möglichkeiten, Kubernetes-Anwendungen schnell zu durchlaufen und zu debuggen und mit Ihrem Team in einem Azure Kubernetes Service-Cluster (AKS) zusammenzuarbeiten. Sobald das Projekt in einem Entwicklungsbereich ausgeführt wird, bietet Azure Dev Spaces eine Möglichkeit, eine laufende Anwendung in AKS anzufügen und zu debuggen.

Dieser Artikel beschreibt, wie das Remotedebuggen mit Dev Spaces funktioniert.

## <a name="debug-your-code"></a>Debuggen Ihres Codes

Bei einer Java-, .NET Core- oder Node.js-Anwendung können Sie diese, wenn sie direkt in Ihrem Entwicklungsbereich ausgeführt wird, mit Visual Studio Code oder Visual Studio debuggen. Visual Studio Code und Visual Studio stellen Tools bereit, mit denen eine Verbindung mit Ihrem Entwicklungsbereich hergestellt, Ihre Anwendung gestartet und ein Debugger eingebunden wird. Nachdem Sie `azds prep` ausgeführt haben, können Sie Ihr Projekt in Visual Studio Code oder Visual Studio öffnen. Visual Studio Code oder Visual Studio erstellt eigene Konfigurationsdateien für die Verbindungsherstellung, die sich von einem Ausführen von `azds prep` unterscheidet. Aus Visual Studio Code oder Visual Studio können Sie Breakpoints festlegen und Ihre Anwendung im Entwicklungsbereich starten.

![Debuggen von Code](media/get-started-node/debug-configuration-nodejs2.png)

Wenn Sie Ihre Anwendung über Visual Studio Code oder Visual Studio für ein Debuggen starten, erfolgen das Starten sowie das Verbinden mit Ihrem Entwicklungsbereich auf die gleiche Weise wie bei einem Ausführen von `azds up`. Die clientseitigen Tools in Visual Studio Code und Visual Studio stellen auch einen zusätzlichen Parameter mit speziellen Informationen für das Debuggen bereit. Der Parameter enthält den Namen des Debugger-Image, den Speicherort des Debuggers im Image des Debuggers und den Zielspeicherort im Container der Anwendung, um den Debugger-Ordner bereitzustellen.

Das Debugger-Image wird automatisch durch die clientseitigen Tools bestimmt. Dazu wird eine Methode verwendet, die derjenigen ähnelt, die bei der Generierung der Dockerfile-Datei und des Helm-Charts beim Ausführen von `azds prep` verwendet wird. Sobald der Debugger im Image der Anwendung bereitgestellt ist, wird er mit `azds exec` ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Einstieg in Azure Dev Spaces für das Remotedebuggen Ihres Projekts finden Sie in den folgenden Schnellstarts:

* [Debuggen und Iterieren mit Visual Studio Code und Java in Kubernetes mithilfe von Azure Dev Spaces][quickstart-java]
* [Entwickeln mit .NET Core unter Kubernetes mit Azure Dev Spaces (Visual Studio Code)][quickstart-netcore]
* [Debuggen und Iterieren mit Visual Studio Code und Node.js in Kubernetes mithilfe von Azure Dev Spaces][quickstart-node]
* [Schnelles Durchlaufen und Debuggen mit Visual Studio und .NET Core][quickstart-vs]


[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
[quickstart-vs]: quickstart-netcore-visualstudio.md
