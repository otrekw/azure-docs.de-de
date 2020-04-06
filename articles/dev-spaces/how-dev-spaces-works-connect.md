---
title: 'Funktionsweise: Verbinden Ihres Entwicklungscomputers mit einem AKS-Cluster'
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Beschreibt die Prozesse, um Azure Dev Spaces zum Verbinden Ihres Entwicklungscomputers mit Ihrem Azure Kubernetes Service-Cluster zu verwenden.
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: a74a5a623006ccd64441023c2c4bc9ad3dcb517e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241340"
---
# <a name="how-connecting-your-development-computer-to-your-aks-cluster-works"></a>Funktionsweise: Verbinden Ihres Entwicklungscomputers mit einem AKS-Cluster

Mit Azure Dev Spaces können Sie Ihren Entwicklungscomputer mit Ihrem AKS-Cluster verbinden, sodass Sie Code auf dem Entwicklungscomputer so ausführen und debuggen können, als würde er auf dem Cluster ausgeführt. Azure Dev Spaces leitet den Datenverkehr zwischen dem verbundenen AKS-Cluster um, indem ein Pod in Ihrem Cluster ausgeführt wird, der als Remote-Agent zum Umleiten von Datenverkehr zwischen dem Entwicklungscomputer und dem Cluster fungiert. Diese Umleitung des Datenverkehrs ermöglicht es, dass Code auf dem Entwicklungscomputer und die im AKS-Cluster ausgeführten Dienste so kommunizieren, als befänden sie sich im gleichen AKS-Cluster. Mit dieser Verbindung können Sie auch Code mit oder ohne Container auf dem Entwicklungscomputer ausführen und debuggen. Durch Verbinden des Entwicklungscomputers mit dem Cluster können Sie Ihre Anwendung schnell entwickeln und End-to-End-Tests durchführen.

## <a name="connecting-to-your-cluster"></a>Herstellen einer Verbindung mit Ihrem Cluster

Mit [Visual Studio Code][vs-code] mit installierter [Azure Dev Spaces-Erweiterung][azds-vs-code] unter macOS oder Windows 10 stellen Sie eine Verbindung mit Ihrem vorhandenen AKS-Cluster her. Wenn Sie eine Verbindung herstellen, können Sie den gesamten Datenverkehr zu oder von einem neuen oder vorhandenen Pod im Cluster an Ihren Entwicklungscomputer umleiten.

> [!NOTE]
> Wenn Sie Visual Studio Code verwenden, um eine Verbindung mit Ihrem Cluster herzustellen, bietet die Azure Dev Spaces Erweiterung Ihnen die Möglichkeit, einen Dienst auf den Entwicklungscomputer umzuleiten. Diese Option ist eine bequeme Möglichkeit, einen Pod für die Umleitung zu identifizieren. Alle Umleitungen zwischen Ihrem AKS-Cluster und dem Entwicklungscomputer sind für einen Pod vorgesehen.

Zum Herstellen einer Verbindung mit Ihrem Cluster ist es nicht erforderlich, dass Azure Dev Spaces auf Ihrem Cluster aktiviert ist. Wenn die Azure Dev Spaces-Erweiterung eine Verbindung mit Ihrem Cluster herstellt, geschieht stattdessen Folgendes:

* Der Container im Pod im AKS-Cluster wird durch einen Remote-Agent-Container ersetzt, der Datenverkehr an Ihren Entwicklungscomputer umleitet. Beim Umleiten eines neuen Pods erstellt Azure Dev Spaces mit dem Remote-Agent einen neuen Pod in Ihrem AKS-Cluster.
* [kubectl port-forward][kubectl-port-forward] wird auf dem Entwicklungscomputer ausgeführt, um Datenverkehr vom Entwicklungscomputer an den Remote-Agent weiterzuleiten, der im Cluster ausgeführt wird.
* Es werden Umgebungsinformationen von Ihrem Cluster mithilfe des Remote-Agents erfasst. Zu diesen Umgebungsinformationen zählen Umgebungsvariablen, sichtbare Dienste, Volumeeinbindungen und Geheimniseinbindungen.
* Die Umgebung im Visual Studio Code-Terminal wird eingerichtet, sodass der Dienst auf dem Entwicklungscomputer auf die gleichen Variablen zugreifen kann, als würde er auf dem Cluster ausgeführt.  
* Die Hostdatei wird aktualisiert, um Dienste auf Ihrem AKS-Cluster lokalen IP-Adressen auf dem Entwicklungscomputer zuzuordnen. Diese Hostdateieinträge ermöglichen, dass Code, der auf dem Entwicklungscomputer ausgeführt wird, Anforderungen an andere Dienste senden kann, die in Ihrem Cluster ausgeführt werden. Um die Hostdatei zu aktualisieren, fordert Azure Dev Spaces beim Herstellen einer Verbindung mit Ihrem Cluster Administratorzugriff auf dem Entwicklungscomputer an.

Wenn Sie Azure Dev Spaces in Ihrem Cluster aktiviert haben, besteht auch die Möglichkeit, die [von Azure Dev Spaces angebotene Umleitung des Datenverkehrs][how-it-works-routing] zu verwenden. Die von Azure Dev Spaces angebotene Umleitung des Datenverkehrs ermöglicht Ihnen das Herstellen einer Verbindung mit einer Kopie Ihres Diensts, der in einem untergeordneten Entwicklungsbereich ausgeführt wird. Wenn Sie einen untergeordneten Entwicklungsbereich verwenden, vermeiden Sie Unterbrechungen für andere Personen, die im übergeordneten Entwicklungsbereich arbeiten, da Sie nur den Datenverkehr umleiten, der sich auf die Instanz des untergeordneten Bereichs des Diensts bezieht. Die Instanz des übergeordneten Bereichs des Diensts bleibt unverändert.

Nachdem Sie eine Verbindung mit Ihrem Cluster hergestellt haben, wird der Datenverkehr an ihren Entwicklungscomputer unabhängig davon weitergeleitet, ob der Dienst auf dem Entwicklungscomputer ausgeführt wird.

## <a name="running-code-on-your-development-computer"></a>Ausführen von Code auf dem Entwicklungscomputer

Nachdem Sie eine Verbindung mit Ihrem AKS-Cluster hergestellt haben, können Sie beliebigen Code ohne Containerisierung nativ auf dem Computer ausführen. Sämtlicher Netzwerkverkehr, den der Remote-Agent empfängt, wird an den während der Verbindung angegebenen lokalen Port umgeleitet, sodass der nativ ausgeführte Code diesen Datenverkehr akzeptieren und verarbeiten kann. Die Umgebungsvariablen, Volumes und Geheimnisse aus dem Cluster werden für Code zur Verfügung gestellt, der auf dem Entwicklungscomputer ausgeführt wird. Aufgrund der Hostdateieinträge und der Portweiterleitung, die auf dem Entwicklercomputer durch Azure Dev Spaces hinzugefügt wurden, kann Ihr Code außerdem Netzwerkdatenverkehr an Dienste senden, die in Ihrem Cluster ausgeführt werden. Dabei werden die Dienstnamen aus dem Cluster verwendet, und dieser Datenverkehr wird an die Dienste weitergeleitet, die in Ihrem Cluster ausgeführt werden.

Da Ihr Code auf Ihrem Entwicklungscomputer ausgeführt wird, besteht die Flexibilität, jedes Tool verwenden zu können, das Sie normalerweise für die Entwicklung verwenden, um Ihren Code auszuführen und zu debuggen. Der Datenverkehr wird zwischen dem Entwicklungscomputer und Ihrem Cluster weitergeleitet, solange die Verbindung besteht. Diese permanente Verbindung ermöglicht Ihnen das erforderliche Starten, Beenden und Neustarten Ihres Codes, ohne dass eine Verbindung erneut hergestellt werden muss.

Azure Dev Spaces bietet außerdem die Möglichkeit, die für Pods im AKS-Cluster verfügbaren Umgebungsvariablen und eingebundenen Dateien über die Datei *azds-local.env* auf dem Entwicklungscomputer zu replizieren. Sie können diese Datei auch verwenden, um neue Umgebungsvariablen und Volumeeinbindungen zu erstellen.

## <a name="additional-configuration-with-azds-localenv"></a>Zusätzliche Konfiguration mit „azds-local.env“

Mit der Datei *azds-local.env* können Sie Umgebungsvariablen und eingebundene Dateien replizieren und für Pods in Ihrem AKS-Cluster bereitstellen. Sie können die folgenden Aktionen in einer Datei *azds-local.env* angeben:

* Laden Sie ein Volume herunter, und legen Sie den Pfad zu diesem Volume als Umgebungsvariable fest.
* Laden Sie eine einzelne Datei oder einen Satz von Dateien von einem Volume herunter, und binden Sie die Dateien auf dem Entwicklungscomputer ein.
* Stellen Sie einen Dienst unabhängig vom Cluster bereit, mit dem Sie verbunden sind.

Dies ist ein Beispiel für eine Datei *azds-local.env*:

```
# This downloads the "whitelist" volume from the container,
# saves it to a temporary directory on your development computer,
# and sets the full path to an environment variable called WHITELIST_PATH.

WHITELIST_PATH=${volumes.whitelist}/whitelist

# This downloads a file from the container's 'default-token-<any>' mount directory 
# to /var/run/secrets/kubernetes.io/serviceaccount on your development computer.

KUBERNETES_IN_CLUSTER_CONFIG_OVERWRITE=${volumes.default-token-*|/var/run/secrets/kubernetes.io/serviceaccount}

# This makes the myapp1 service available to your development computer
# regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP1_SERVICE_HOST.

# If the myapp1 service is made available in this way, 
# you can also access it using "myapp1" and "myapp1.svc.cluster.local"
# in addition to the IP in the MYAPP1_SERVICE_HOST environment variable.

MYAPP1_SERVICE_HOST=${services.myapp1}

# This makes the service myapp2 in namespace mynamespace available to your 
# development computer regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP2_SERVICE_HOST.

MYAPP2_SERVICE_HOST=${services.mynamespace.myapp2}
```

Eine Datei *azds-local.env* wird nicht standardmäßig automatisch erstellt, sodass Sie die Datei im Stammverzeichnis des Projekts manuell erstellen müssen.

## <a name="diagnostics-and-logging"></a>Diagnose und Protokollierung

Wenn eine Verbindung mit dem AKS-Cluster besteht, werden Diagnoseprotokolle vom Cluster im [temporären Verzeichnis][azds-tmp-dir] Ihres Entwicklungscomputers protokolliert. Mit Visual Studio Code können Sie auch den Befehl *Diagnoseinformationen anzeigen* verwenden, um die aktuellen Umgebungsvariablen und DNS-Einträge von Ihrem AKS-Cluster auszugeben.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten beim Verbinden Ihres lokalen Entwicklungscomputers mit Ihrem AKS-Cluster finden Sie unter [Verbinden Ihres Entwicklungscomputers mit einem AKS-Cluster][connect].

[azds-tmp-dir]: troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[connect]: how-to/connect.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward
[vs-code]: https://code.visualstudio.com/download
