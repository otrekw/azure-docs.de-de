---
title: Konfigurieren des Azure Dev Spaces-Clusters zur Verwendung von Helm 3 (Vorschau)
services: azure-dev-spaces
ms.date: 02/28/2020
ms.topic: conceptual
description: Informationen zum Konfigurieren eines Dev Spaces-Clusters für die Verwendung von Helm 3
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: 9e3f3ff90d36215c386bf1d8b8ec1edd54ebfb6a
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78202023"
---
# <a name="configure-your-azure-dev-spaces-cluster-to-use-helm-3-preview"></a>Konfigurieren des Azure Dev Spaces-Clusters zur Verwendung von Helm 3 (Vorschau)

Azure Dev Spaces verwendet standardmäßig Helm 2 zum Installieren von Benutzerdiensten in Dev Spaces in Ihrem AKS-Cluster. Sie können Azure Dev Spaces für die Verwendung von Helm 3 anstelle von Helm 2 aktivieren, um Benutzerdienste in Dev Spaces zu installieren. Unabhängig von der Version von Helm, die Azure Dev Spaces zum Installieren von Benutzerdiensten verwendet, können Sie weiterhin den Helm 2- oder Helm 3-Client verwenden, um Ihre eigenen Releases auf demselben Cluster zu verwalten.

Wenn Sie Helm 3 aktivieren, verhält sich Azure Dev Spaces auf folgende Weise anders, wenn Benutzerdienste in Dev Spaces installiert werden:

* Tiller wird nicht mehr in Ihrem Cluster im Namespace *azds* bereitgestellt.
* Helm speichert Releaseinformationen im Namespace, in dem ein Dienst installiert wird, nicht im *azds*-Namespace.
* Helm 3-Releaseinformationen verbleiben im Namespace, in dem ein Dienst installiert wird, nachdem ein Controller gelöscht wurde.
* Sie können direkt mit allen Releases interagieren, die von Azure Dev Spaces in Ihrem Cluster mithilfe des Helm 3-Clients verwaltet werden.

In diesem Leitfaden erfahren Sie, wie Sie Helm 3 für Azure Dev Spaces aktivieren, um Benutzerdienste in Dev Spaces zu installieren.

> [!IMPORTANT]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="before-you-begin"></a>Voraussetzungen

### <a name="prerequisites"></a>Voraussetzungen

* ein Azure-Abonnement. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Die [Azure CLI][azure-cli] muss installiert sein.

### <a name="register-the-helm3preview-preview-feature"></a>Registrieren der Previewfunktion Helm3Preview

Um Azure Dev Spaces für die Verwendung von Helm 3 für die Installation von Benutzerdiensten in Dev Spaces zu aktivieren, aktivieren Sie zunächst mit dem Befehl *az feature register* das Featureflag *Helm3Preview* für Ihr Abonnement:

> [!WARNING]
> Alle AKS-Cluster, für die Sie Azure Dev Spaces mit dem Featureflag *Helm3Preview* aktivieren, verwenden diese Vorschauversion. Um weiterhin vollständig unterstützte Azure Dev Spaces in AKS-Clustern zu erstellen, sollten Sie keine Previewfunktionen für Produktionsabonnements aktivieren. Verwenden Sie ein separates Test- oder Entwickungsabonnement von Azure, um Vorschaufeatures zu testen.

```azure-cli
az feature register --namespace Microsoft.DevSpaces --name Helm3Preview
```

Es dauert einige Minuten, bis die Registrierung abgeschlossen ist. Überprüfen Sie den Registrierungsstatus mit dem Befehl *az feature show*:

```azure-cli
az feature show --namespace Microsoft.DevSpaces --name Helm3Preview
```

Wenn der *Status* *Registriert* ist, aktualisieren Sie die Registrierung von *Microsoft.DevSpaces* mithilfe von *az provider register*:

```azure-cli
az provider register --namespace Microsoft.DevSpaces
```

### <a name="limitations"></a>Einschränkungen

Die folgenden Einschränkungen gelten, während sich dieses Feature in der Vorschauphase befindet:

* Dieses Feature kann nicht in AKS-Clustern mit vorhandenen Workloads verwendet werden. Sie müssen einen neuen AKS-Cluster erstellen.

## <a name="create-your-cluster"></a>Erstellen Ihres Clusters

Erstellen Sie einen neuen AKS-Cluster in einer Region, in der diese Previewfunktion verfügbar ist. Die folgenden Befehle erstellen eine Ressourcengruppe namens *MyResourceGroup* und einen neuen AKS-Cluster namens *MyAKS* in der Region *USA, Süden-Mitte*:

```azure-cli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces"></a>Aktivieren von Azure Dev Spaces

Verwenden Sie den Befehl *use-dev-spaces*, um Dev Spaces in Ihrem AKS-Cluster zu aktivieren, und befolgen Sie die angezeigten Anweisungen. Mit dem unten angegebenen Befehl wird Dev Spaces im Cluster *MyAKS* in der Gruppe *MyResourceGroup* aktiviert und ein Standardentwicklerbereich erstellt.

```cmd
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

## <a name="verify-dev-spaces-is-running-helm-3"></a>Überprüfen, ob Dev Spaces Helm 3 ausführt

Vergewissern Sie sich, dass Tiller nicht ausgeführt wird, indem Sie die Bereitstellungen im *azds*-Namespace auflisten:

```cmd
kubectl get deployment -n azds
```

Vergewissern Sie sich, dass *tiller-desploy* nicht im azds-Namespace ausgeführt wird. Beispiel:

```console
$ kubectl get deployments -n azds
NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
azds-webhook-deployment   2/2     2            2           39m
traefik                   1/1     1            1           39m
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Azure Dev Spaces Sie bei der Entwicklung komplexerer containerübergreifender Anwendungen unterstützt und wie Sie die gemeinsame Entwicklung vereinfachen können, indem Sie in verschiedenen Bereichen mit verschiedenen Versionen oder Branches Ihres Codes arbeiten.

> [!div class="nextstepaction"]
> [Schnellstart: Entwicklung im Team unter Kubernetes: Azure Dev Spaces][team-quickstart]


[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[team-quickstart]: ../quickstart-team-development.md