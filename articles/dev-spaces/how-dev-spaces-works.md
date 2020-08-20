---
title: Funktionsweise von Azure Dev Spaces
services: azure-dev-spaces
ms.date: 06/02/2020
ms.topic: conceptual
description: Beschreibt die Prozesse, auf denen Azure Dev Spaces basiert.
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: 48bde5f3cc6f397d51a31f80f41ab299ba8866ee
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88212530"
---
# <a name="how-azure-dev-spaces-works"></a>Funktionsweise von Azure Dev Spaces

Das Entwickeln einer Kubernetes-Anwendung kann eine Herausforderung sein. Sie benötigen Docker und Kubernetes-Konfigurationsdateien. Sie müssen feststellen, wie Sie Ihre Anwendung lokal testen können und wie sie mit anderen abhängigen Diensten zusammenarbeitet. Möglicherweise müssen Sie das Entwickeln und Testen für mehrere Dienste gleichzeitig und mit einem Team von Entwicklern durchführen.

Azure Dev Spaces bietet Ihnen mehrere Möglichkeiten, Kubernetes-Anwendungen schnell zu durchlaufen und zu debuggen und mit Ihrem Team zusammenzuarbeiten. In diesem Artikel werden Zweck und Funktionsweise von Azure Dev Spaces beschrieben.

## <a name="rapidly-iterate-and-debug-your-kubernetes-application"></a>Schnelles Durchlaufen und Debuggen Ihrer Kubernetes-Anwendung

Azure Dev Spaces reduziert den Aufwand für das Entwickeln, Testen und Durchlaufen Ihrer Kubernetes-Anwendung im Kontext Ihres AKS-Clusters. Diese Verringerung des Aufwands ermöglicht es Entwicklern, sich auf die Geschäftslogik ihrer Anwendungen zu konzentrieren und ihre Dienste nicht für die Ausführung in Kubernetes zu konfigurieren.

### <a name="local-process-with-kubernetes"></a>Lokaler Prozess mit Kubernetes

Beim lokalen Prozess mit Kubernetes können Sie Ihren Entwicklungscomputer mit Ihrem Kubernetes-Cluster verbinden, sodass Sie Code auf dem Entwicklungscomputer so ausführen und debuggen können, als würde er im Cluster ausgeführt. Azure Dev Spaces leitet den Datenverkehr zwischen dem verbundenen Cluster um, indem ein Pod in Ihrem Cluster ausgeführt wird, der als Remote-Agent zum Umleiten von Datenverkehr zwischen dem Entwicklungscomputer und dem Cluster dient. Durch diese Umleitung des Datenverkehrs können Code auf dem Entwicklungscomputer und die im Cluster ausgeführten Dienste so kommunizieren, als befänden sie sich im selben Cluster. Weitere Informationen zum Herstellen einer Verbindung zwischen Ihrem Entwicklungscomputer und einem Kubernetes-Cluster finden Sie unter [Funktionsweise des lokalen Prozesses mit Kubernetes][how-it-works-local-process-kubernetes].

### <a name="run-your-code-in-aks"></a>Ausführen Ihres Codes in AKS

Zusätzlich zum Umleiten von Datenverkehr zwischen dem Entwicklungscomputer und Ihrem AKS-Cluster können Sie mit Azure Dev Spaces Code direkt in AKS konfigurieren und schnell ausführen. Mit Visual Studio, Visual Studio Code oder der Azure Dev Spaces-CLI lädt Azure Dev Spaces Ihren Code in den Cluster hoch, erstellt ihn und führt ihn dann aus. Azure Dev Spaces kann auch Codeänderungen intelligent synchronisieren und Ihren Dienst bei Bedarf neu starten, um Änderungen widerzuspiegeln. Beim Ausführen des Codes werden Buildprotokolle und HTTP-Ablaufverfolgungen zurück an den Client gestreamt, damit Sie den Fortschritt überwachen und Probleme diagnostizieren können. Sie können Azure Dev Spaces auch verwenden, um den Debugger in Visual Studio und Visual Studio Code an Java-, Node.js- und .NET Core-Dienste anzufügen. Weitere Informationen finden Sie unter [Funktionsweise beim Vorbereiten eines Projekts für Azure Dev Spaces][how-it-works-prep], [Funktionsweise der Ausführung von Code mit Azure Dev Spaces][how-it-works-up] und [Funktionsweise des Remotedebuggens von Code mit Azure Dev Spaces][how-it-works-remote-debugging].

## <a name="team-development"></a>Teamentwicklung

Azure Dev Spaces unterstützt Teams bei der produktiven Arbeit an einer gemeinsamen Anwendung im selben AKS-Cluster, ohne dass es zu Unterbrechungen kommt.

### <a name="intelligent-routing-between-dev-spaces"></a>Intelligentes Routing zwischen Dev Spaces

Mit Azure Dev Spaces kann ein Team einen einzelnen AKS-Cluster mit einer cloudbasierten nativen Anwendung gemeinsam nutzen und isolierte Entwicklungsbereiche erstellen, in denen das Team die Anwendung entwickeln, testen und debuggen kann, ohne die anderen Entwicklungsbereiche zu stören. Eine Baselineversion der Anwendung wird in einem Stammentwicklungsbereich ausgeführt. Teammitglieder erstellen dann unabhängige untergeordnete Entwicklungsbereiche basierend auf dem Stammbereich für die Entwicklung, das Testen und das Debuggen von Änderungen an der Anwendung. Mithilfe der Routingfunktionen in Dev Spaces können untergeordnete Entwicklungsbereiche Anforderungen zwischen Diensten weiterleiten, die im untergeordneten Entwicklungsbereich und im übergeordneten Entwicklungsbereich ausgeführt werden. Mithilfe dieses Routings können Entwickler ihre eigene Version eines Diensts ausführen und dabei abhängige Dienste aus dem übergeordneten Bereich wiederverwenden. Jeder untergeordnete Bereich verfügt über eine eigene eindeutige URL, die für andere Benutzer für die Zusammenarbeit freigegeben und von ihnen für den Zugriff genutzt werden kann. Weitere Informationen zur Funktionsweise des Routings in Azure Dev Spaces finden Sie unter [Funktionsweise von Routing mit Azure Dev Spaces][how-it-works-routing].

### <a name="live-testing-an-open-pull-request"></a>Livetest eines offenen Pull Requests

Sie können auch GitHub Actions mit Azure Dev Spaces verwenden, um Änderungen an ihrer Anwendung in einem Pull Request direkt in Ihrem Cluster vor dem Mergen testen. Azure Dev Spaces kann automatisch eine Überprüfungsversion der Anwendung in Ihrem Cluster bereitstellen, sodass sowohl der Autor als auch andere Teammitglieder die Änderungen im Kontext der gesamten Anwendung überprüfen können. Mithilfe der Routingfunktionen von Azure Dev Spaces wird diese Überprüfungsversion der Anwendung auch in Ihrem Cluster bereitgestellt, ohne dass sich dies auf andere Entwicklungsbereiche auswirkt. Diese Funktionen ermöglichen es Ihnen, Pull Requests zuverlässig zu genehmigen und zu mergen. Ein Beispiel für GitHub Actions und Azure Dev Spaces finden Sie unter [GitHub Actions und Azure Kubernetes Service][pr-flow].

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten beim Verbinden Ihres lokalen Entwicklungscomputers mit Ihrem AKS-Cluster finden Sie unter [Verbinden Ihres Entwicklungscomputers mit einem AKS-Cluster][connect].


[connect]: https://code.visualstudio.com/docs/containers/local-process-kubernetes
[how-it-works-local-process-kubernetes]: /visualstudio/containers/overview-local-process-kubernetes
[how-it-works-prep]: how-dev-spaces-works-prep.md
[how-it-works-remote-debugging]: how-dev-spaces-works-remote-debugging.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[how-it-works-up]: how-dev-spaces-works-up.md
[pr-flow]: how-to/github-actions.md
[routing]: #team-development