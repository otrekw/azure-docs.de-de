---
title: Einrichten eines Clusters für Azure Dev Spaces
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: In diesem Artikel wird das Einrichten eines Azure Kubernetes Service-Clusters für Azure Dev Spaces beschrieben.
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: 841e67b96e95aa251fa5bf1ef469b68de30f54d9
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91972678"
---
# <a name="how-setting-up-a-cluster-for-azure-dev-spaces-works"></a>Einrichten eines Clusters für Azure Dev Spaces

[!INCLUDE [Azure Dev Spaces deprecation](../../includes/dev-spaces-deprecation.md)]

Azure Dev Spaces bietet Ihnen mehrere Möglichkeiten, Kubernetes-Anwendungen schnell zu durchlaufen und zu debuggen und mit Ihrem Team in einem Azure Kubernetes Service-Cluster (AKS) zusammenzuarbeiten. Eine Möglichkeit besteht darin, Azure Dev Spaces in Ihrem AKS-Cluster zu aktivieren, damit Sie [Dienste direkt im Cluster ausführen][how-it-works-up] und [weitere Netzwerk- und Routingfunktionen][how-it-works-routing] verwenden können. In diesem Artikel wird beschrieben, wie Sie Ihren Cluster vorbereiten und Azure Dev Spaces aktivieren.

## <a name="prepare-your-aks-cluster"></a>Vorbereiten Ihres AKS-Clusters

Vergewissern Sie sich bei der Vorbereitung des AKS-Clusters für Dev Spaces, dass sich der Cluster in einer Region befindet, die [in Azure Dev Spaces unterstützt wird][supported-regions], und dass Kubernetes 1.10.3 oder höher ausgeführt wird. Sie können Azure Dev Spaces über die Azure-Befehlszeilenschnittstelle aktivieren, indem Sie `az aks use-dev-spaces` ausführen.

Wenn Sie Azure Dev Spaces in Ihrem AKS-Cluster aktivieren, wird der Controller für den Cluster installiert. Der Controller befindet sich außerhalb des AKS-Clusters. Er steuert das Verhalten und die Kommunikation zwischen den clientseitigen Tools und dem AKS-Cluster. Nachdem der Controller aktiviert ist, können Sie über die clientseitigen Tools mit ihm interagieren.

Der Controller führt folgende Aktionen aus:

* Er verwaltet das Erstellen und Auswählen des Entwicklungsbereichs (Dev Space).
* Er installiert das Helm-Chart Ihrer Anwendung und erstellt Kubernetes-Objekte.
* Er erstellt das Containerimage Ihrer Anwendung.
* Er stellt Ihre Anwendung in AKS bereit.
* Er erstellt inkrementelle Builds und führt einen Neustart aus, wenn sich Ihr Quellcode geändert hat.
* Er verwaltet Protokolle und HTTP-Überwachungen.
* Er sendet Standardausgabe (stdout) und Standardfehlerausgabe (stderr) an die clientseitigen Tools.
* Konfigurieren des Routings für Anwendungen in einem Entwicklungsbereich als auch zwischen übergeordneten und untergeordneten Bereichen

Der Controller ist eine separate Azure-Ressource außerhalb Ihres Clusters und führt für Ressourcen in Ihrem Cluster die folgenden Aktionen aus:

* Erstellen oder Bestimmen eines Kubernetes-Namespace, der als Entwicklungsbereich verwendet werden soll
* Entfernen aller Kubernetes-Namespaces namens *azds* (sofern vorhanden) und Erstellen eines neuen Namespace
* Stellt eine Kubernetes-Webhook-Konfiguration bereit.
* Stellt einen Webhook-Zulassungsserver bereit.

Es wird derselbe Dienstprinzipal wie in Ihrem AKS-Cluster verwendet, um Dienstaufrufe an andere Azure Dev Spaces-Komponenten durchzuführen.

![Clustervorbereitung mit Azure Dev Spaces](media/how-dev-spaces-works/prepare-cluster.svg)

Damit Azure Dev Spaces verwendet werden kann, muss mindestens ein Entwicklungsbereich (Dev Space) vorhanden sein. Azure Dev Spaces verwendet Kubernetes-Namespaces in Ihrem AKS-Cluster für Entwicklungsbereiche. Wenn ein Controller installiert wird, werden Sie aufgefordert, einen neuen Kubernetes-Namespace zu erstellen oder einen vorhandenen Namespace auszuwählen, der als erster Entwicklungsbereich verwendet werden soll. Standardmäßig bietet der Controller die Möglichkeit, für den vorhandenen Kubernetes-*Standardnamespace* ein Upgrade zum ersten Entwicklungsbereich durchzuführen.

Wird ein Namespace als Entwicklungsbereich bestimmt, fügt der Controller die Bezeichnung *azds.io/space=true* zu diesem Namespace hinzu, um ihn als Entwicklungsbereich zu kennzeichnen. Der Erstentwicklungsbereich, den Sie erstellen oder bestimmen, wird standardmäßig ausgewählt, nachdem Sie Ihren Cluster vorbereitet haben. Wenn ein Bereich ausgewählt ist, wird er von Azure Dev Spaces für das Erstellen neuer Workloads verwendet.

Sie können die clientseitigen Tools verwenden, um neue Entwicklungsbereiche zu erstellen und vorhandene Entwicklungsbereiche zu entfernen. Aufgrund einer Einschränkung in Kubernetes kann der *default*-Entwicklungsbereich nicht entfernt werden. Der Controller entfernt auch alle vorhandenen Kubernetes-Namespaces namens *azds*, um Konflikte mit dem `azds`-Befehl zu vermeiden, der von den clientseitigen Tools verwendet wird.

Das Kubernetes-Webhook-Zulassungsserver wird verwendet, um Pods mit drei Containern während der Bereitstellung für die Instrumentierung einzufügen: ein devspaces-proxy-init-Container, ein devspaces-proxy-init-Container und ein devspaces-build-Container. **Diese drei Container werden alle mit Root-Zugriff in Ihrem AKS-Cluster ausgeführt.** Sie verwenden außerdem derselben Dienstprinzipal wie in Ihrem AKS-Cluster, um Dienstaufrufe an andere Azure Dev Spaces-Komponenten durchzuführen.

![Azure Dev Spaces Kubernetes-Webhook-Zulassungsserver](media/how-dev-spaces-works/kubernetes-webhook-admission-server.svg)

Der devspaces-proxy-Container ist ein unterstützender Container, in dem der gesamte ein- und ausgehende TCP-Datenverkehr des Anwendungscontainers verarbeitet und das Routing unterstützt wird. Der devspaces-proxy-Container leitet HTTP-Nachrichten um, wenn bestimmte Bereiche verwendet werden. Beispielsweise kann er dabei unterstützen, HTTP-Nachrichten zwischen Anwendungen in über- und untergeordneten Bereichen weiterzuleiten. Jeglicher Nicht-HTTP-Datenverkehr durchläuft den devspaces-proxy-Container ungeändert. Im devspaces-proxy-Container werden auch alle eingehenden und ausgehenden HTTP-Nachrichten protokolliert und als Überwachungen an die clientseitigen Tools gesendet. Der Entwickler kann diese Überwachungen dann anzeigen, um das Verhalten der Anwendung zu untersuchen.

Der devspaces-proxy-init-Container ist ein [Init-Container](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/), für den zusätzliche Routingregeln entsprechend der Bereichshierarchie zum Container Ihrer Anwendung hinzugefügt werden. Die Routingregeln werden vor dem Start des Containers hinzugefügt, indem die */etc/resolv.conf*-Datei des Anwendungcontainers und die iptables-Konfiguration aktualisiert werden. Die Aktualisierungen an */etc/resolv.conf* ermöglichen die DNS-Auflösung von Diensten in übergeordneten Bereichen. Die Aktualisierungen an der iptables-Konfiguration stellen sicher, dass der gesamte ein- und ausgehende TCP-Datenverkehr des Containers der Anwendung über den devspaces-proxy-Container weitergeleitet wird. Alle Aktualisierungen aus dem devspaces-proxy-init-Container erfolgen zusätzlich zu den Regeln, die von Kubernetes hinzugefügt werden.

Der devspaces-build-Container ist ein Init-Container, und in ihm sind der Projektquellcode und der Docker-Socket eingebunden. Der Projektquellcode und der Zugriff auf Docker ermöglichen, dass der Anwendungscontainer direkt durch den Pod erstellt werden kann.

> [!NOTE]
> Azure Dev Spaces verwendet denselben Knoten, um den Container Ihrer Anwendung zu erstellen und auszuführen. Daher ist für Azure Dev Spaces keine externe Containerregistrierung erforderlich, um Ihre Anwendung zu erstellen und auszuführen.

Der Kubernetes-Webhook-Zulassungsserver lauscht auf jeden neuen Pod, der im AKS-Cluster erstellt wird. Wird dieser Pod in irgendeinem Namespace mit der Bezeichnung *azds.io/space=true* bereitgestellt, fügt das Objekt diesen Pod mit den zusätzlichen Containern ein. Der devspaces-build-Container wird nur eingefügt, wenn der Container der Anwendung über die clientseitigen Tools ausgeführt wird.

Nachdem Sie Ihren AKS-Cluster vorbereitet haben, können Sie die clientseitigen Tools verwenden, um Ihren Code in Ihrem Entwicklungsbereich (Dev Space) vorzubereiten und auszuführen.

## <a name="client-side-tooling"></a>Clientseitige Tools

Die clientseitigen Tools eröffnen dem Benutzer folgende Möglichkeiten:
* Generieren einer Dockerfile-Datei, eines Helm-Charts und einer Azure Dev Spaces-Konfigurationsdatei für die Anwendung
* Erstellen von übergeordneten und untergeordneten Entwicklungsbereichen
* Anweisen des Controllers, dass er Ihre Anwendung erstellen zu starten soll

Während Ihre Anwendung ausgeführt wird, führen die clientseitigen Tools außerdem folgende Aktionen aus:
* Empfangen und Anzeigen von Standardausgabe (stdout) und Standardfehlerausgabe (stderr) aus Ihrer Anwendung, die in AKS ausgeführt wird
* Verwenden von [port-forward](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/), um Webzugriff auf Ihre Anwendung über „http:\//localhost“ zu ermöglichen
* Einbinden eines Debuggers in Ihre aktive Anwendung in AKS
* Synchronisieren des Quellcodes mit Ihrem Entwicklungsbereich, wenn eine Änderung für inkrementelle Builds erkannt wurde, wodurch eine schnelle Iteration unterstützt wird
* Verbinden des Entwicklungscomputers direkt mit dem AKS-Cluster

Sie können die clientseitigen Tools über die Befehlszeile als Bestandteil des `azds`-Befehls verwenden. Sie können die clientseitigen Tools auch verwenden mit:

* Visual Studio Code über die [Azure Dev Spaces-Erweiterung](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds)
* Visual Studio mit der Workload „Azure-Entwicklung“

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung der clientseitigen Tools zum Vorbereiten und Ausführen des Codes im Entwicklungsbereich finden Sie unter [Funktionsweise der Vorbereitung eines Projekts für Azure Dev Spaces][how-it-works-prep].


[how-it-works-prep]: how-dev-spaces-works-prep.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[how-it-works-up]: how-dev-spaces-works-up.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service