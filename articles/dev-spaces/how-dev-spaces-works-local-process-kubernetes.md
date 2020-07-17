---
title: Funktionsweise des lokalen Prozesses mit Kubernetes
services: azure-dev-spaces
ms.date: 06/02/2020
ms.topic: conceptual
description: Beschreibt die Prozesse zum Verwenden des lokalen Prozesses mit Kubernetes zum Herstellen einer Verbindung zwischen Ihrem Entwicklungscomputer und Ihrem Kubernetes-Cluster
keywords: Lokaler Prozess mit Kubernetes, Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: dd126fc55a86b1de115239a31e5adb7b1d264846
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84974397"
---
# <a name="how-local-process-with-kubernetes-works"></a>Funktionsweise des lokalen Prozesses mit Kubernetes

Der lokale Prozess mit Kubernetes ermöglicht Ihnen das Ausführen und Debuggen von Code auf Ihrem Entwicklungscomputer, während der Kubernetes-Cluster weiterhin mit den übrigen Anwendungen oder Diensten verbunden ist. Wenn Sie z. B. über eine umfangreiche Microservicearchitektur mit einer Vielzahl von Diensten und Datenbanken verfügen, die voneinander abhängig sind, kann es schwierig sein, diese Abhängigkeiten auf Ihrem Entwicklungscomputer zu replizieren. Außerdem kann der Vorgang zum Erstellen und Bereitstellen von Code in Ihrem Kubernetes-Cluster für jede Codeänderung während der Inner Loop-Entwicklung langsam, zeitaufwendig und bei Verwendung mit einem Debugger wenig benutzerfreundlich sein.

Bei Verwendung des lokalen Prozesses mit Kubernetes muss Ihr Code nicht erstellt und im Cluster bereitgestellt werden. Stattdessen wird eine direkte Verbindung zwischen Ihrem Entwicklungscomputer und Ihrem Cluster hergestellt. Indem Sie Ihren Entwicklungscomputer während des Debuggens mit Ihrem Cluster verbinden, können Sie Ihren Dienst im Handumdrehen im Kontext der gesamten Anwendung testen und entwickeln, ohne dazu eine Docker- oder Kubernetes-Konfiguration erstellen zu müssen.

Wenn Sie den lokalen Prozess mit Kubernetes verwenden, wird Datenverkehr zwischen Ihrem verbundenen Kubernetes-Cluster und Ihrem Entwicklungscomputer umgeleitet. Diese Umleitung des Datenverkehrs ermöglicht es, dass Code auf dem Entwicklungscomputer und die im Kubernetes-Cluster ausgeführten Dienste so kommunizieren, als befänden sie sich im gleichen Kubernetes-Cluster. Der lokale Prozess bietet außerdem die Möglichkeit, die für Pods im Kubernetes-Cluster verfügbaren Umgebungsvariablen und bereitgestellten Volumes auf dem Entwicklungscomputer zu replizieren. Indem Sie auf Ihrem Entwicklungscomputer auf Umgebungsvariablen und bereitgestellte Volumes zugreifen können, können Sie Ihren Code rasch bearbeiten, ohne diese Abhängigkeiten manuell replizieren zu müssen.

## <a name="using-local-process-with-kubernetes"></a>Verwenden des lokalen Prozesses mit Kubernetes

Damit Sie den lokalen Prozess mit Kubernetes verwenden können, muss [Visual Studio Code][vs-code] mit den Erweiterungen [Azure Dev Spaces][azds-vs-code] und [Azure Kubernetes Service][az-aks-vs-code] installiert sein und unter macOS oder Windows 10 ausgeführt werden. Außerdem muss die [Azure Dev Spaces-Befehlszeilenschnittstelle installiert sein][azds-cli]. Sie können auch [Visual Studio 2019][visual-studio] unter Windows 10 mit den installierten Workloads *ASP.NET und Webentwicklung* und *Azure-Entwicklung* sowie mit aktiviertem Featureflag *AzureDevSpacesTools.LocalKubernetesDebugging* (Vorschau) verwenden. Zudem muss die [Azure Dev Spaces-Befehlszeilenschnittstelle installiert][azds-cli] sein. Wenn Sie den lokalen Prozess mit Kubernetes verwenden, um eine Verbindung zu Ihrem Kubernetes-Cluster herzustellen, haben Sie die Möglichkeit, den gesamten Datenverkehr zu und von einem vorhandenen Pod im Cluster auf Ihren Entwicklungscomputer umzuleiten.

> [!NOTE]
> Wenn Sie den lokalen Prozess mit Kubernetes verwenden, werden Sie aufgefordert, den Namen des Diensts einzugeben, der auf Ihren Entwicklungscomputer umgeleitet werden soll. Diese Option ist eine bequeme Möglichkeit, einen Pod für die Umleitung zu identifizieren. Alle Umleitungen zwischen Ihrem Kubernetes-Cluster und dem Entwicklungscomputer sind für einen Pod vorgesehen.

Wenn der lokale Prozess mit Kubernetes eine Verbindung mit Ihrem Cluster herstellt, geht er wie folgt vor:

* Fordert Sie auf, den auf Ihrem Cluster zu ersetzenden Dienst, den für Ihren Code zu verwendenden Port auf Ihrem Entwicklungscomputer und die Startaufgabe für Ihren Code als einmalige Aktion zu konfigurieren.
* Der Container im Pod im Cluster wird durch einen Remote-Agent-Container ersetzt, der Datenverkehr an Ihren Entwicklungscomputer umleitet.
* [kubectl port-forward][kubectl-port-forward] wird auf dem Entwicklungscomputer ausgeführt, um Datenverkehr vom Entwicklungscomputer an den Remote-Agent weiterzuleiten, der im Cluster ausgeführt wird.
* Es werden Umgebungsinformationen von Ihrem Cluster mithilfe des Remote-Agents erfasst. Zu diesen Umgebungsinformationen zählen Umgebungsvariablen, sichtbare Dienste, Volumeeinbindungen und Geheimniseinbindungen.
* Die Umgebung in Visual Studio oder Visual Studio Code wird eingerichtet, sodass der Dienst auf dem Entwicklungscomputer auf die gleichen Variablen zugreifen kann, als würde er auf dem Cluster ausgeführt.  
* Die Hostdatei wird aktualisiert, um Dienste auf Ihrem Cluster lokalen IP-Adressen auf dem Entwicklungscomputer zuzuordnen. Diese Hostdateieinträge ermöglichen, dass Code, der auf dem Entwicklungscomputer ausgeführt wird, Anforderungen an andere Dienste senden kann, die in Ihrem Cluster ausgeführt werden. Um die Hostdatei zu aktualisieren, fordert der lokale Prozess mit Kubernetes beim Herstellen einer Verbindung mit Ihrem Cluster Administratorzugriff auf dem Entwicklungscomputer an.
* Startet die Ausführung und das Debuggen Ihres Codes auf Ihrem Entwicklungscomputer. Falls erforderlich, gibt der lokale Prozess mit Kubernetes erforderliche Ports auf Ihrem Entwicklungscomputer frei, indem er Dienste oder Prozesse beendet, die diese Ports derzeit verwenden.

Nachdem Sie eine Verbindung mit Ihrem Cluster hergestellt haben, können Sie Code ohne Containerisierung nativ auf Ihrem Computer ausführen und debuggen, und der Code kann direkt mit dem Rest Ihres Clusters interagieren. Sämtlicher Netzwerkverkehr, den der Remote-Agent empfängt, wird an den während der Verbindung angegebenen lokalen Port umgeleitet, sodass der nativ ausgeführte Code diesen Datenverkehr akzeptieren und verarbeiten kann. Die Umgebungsvariablen, Volumes und Geheimnisse aus dem Cluster werden für Code zur Verfügung gestellt, der auf dem Entwicklungscomputer ausgeführt wird. Aufgrund der Hostdateieinträge und der Portweiterleitung, die auf dem Entwicklercomputer durch den lokalen Prozess mit Kubernetes hinzugefügt wurden, kann Ihr Code außerdem Netzwerkdatenverkehr an Dienste senden, die in Ihrem Cluster ausgeführt werden. Dabei werden die Dienstnamen aus dem Cluster verwendet, und dieser Datenverkehr wird an die Dienste weitergeleitet, die in Ihrem Cluster ausgeführt werden. Der Datenverkehr wird zwischen dem Entwicklungscomputer und Ihrem Cluster weitergeleitet, solange die Verbindung besteht.

## <a name="diagnostics-and-logging"></a>Diagnose und Protokollierung

Wenn Sie den lokalen Prozess mit Kubernetes zum Herstellen einer Verbindung mit Ihrem Cluster verwenden, werden Diagnoseprotokolle von Ihrem Cluster im [temporären Verzeichnis][azds-tmp-dir] Ihres Entwicklungscomputers protokolliert. Mit Visual Studio Code können Sie auch den Befehl *Diagnoseinformationen anzeigen* verwenden, um die aktuellen Umgebungsvariablen und DNS-Einträge von Ihrem Cluster auszugeben.

## <a name="limitations"></a>Einschränkungen

„Lokaler Prozess mit Kubernetes“ unterliegt folgenden Einschränkungen:

* „Lokaler Prozess mit Kubernetes“ leitet Datenverkehr für einen einzelnen Dienst an Ihren Entwicklungscomputer um. Sie können „Lokaler Prozess mit Kubernetes“ nicht verwenden, um mehrere Dienste gleichzeitig umzuleiten.
* Ein Dienst muss von einem einzelnen Pod unterstützt werden, um eine Verbindung mit diesem Dienst herzustellen. Sie können keine Verbindung mit einem Dienst mit mehreren Pods herstellen, z. B. einem Dienst mit Replikaten.
* In einem Pod darf nur ein einzelner Container ausgeführt werden, damit „Lokaler Prozess mit Kubernetes“ erfolgreich eine Verbindung herstellen kann. „Lokaler Prozess mit Kubernetes“ kann keine Verbindung mit Diensten mit Pods herstellen, die über zusätzliche Container verfügen, wie z. B. Sidecar-Container, die von Dienstgittermodellen eingefügt werden.
* „Lokaler Prozess mit Kubernetes“ benötigt erhöhte Rechte, damit er auf Ihrem Entwicklungscomputer ausgeführt werden kann, um Ihre Hostdatei zu bearbeiten.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten bei der Verwendung des lokalen Prozesses mit Kubernetes zum Herstellen einer Verbindung zwischen Ihrem lokalen Entwicklungscomputer und Ihrem Cluster finden Sie unter [Verwenden des lokalen Prozesses mit Kubernetes und Visual Studio Code][local-process-kubernetes-vs-code] und [Verwenden des lokalen Prozesses mit Kubernetes und Visual Studio][local-process-kubernetes-vs].

[azds-cli]: how-to/install-dev-spaces.md#install-the-client-side-tools
[azds-tmp-dir]: troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[az-aks-vs-code]: https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-aks-tools
[local-process-kubernetes-vs-code]: how-to/local-process-kubernetes-vs-code.md
[local-process-kubernetes-vs]: how-to/local-process-kubernetes-visual-studio.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward
[visual-studio]: https://www.visualstudio.com/vs/
[vs-code]: https://code.visualstudio.com/download