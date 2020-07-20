---
title: Verwenden von Podsicherheitsrichtlinien in Azure Kubernetes Service (AKS)
description: Hier erfahren Sie, wie Sie den Podzugang mittels Podsicherheitsrichtlinie in Azure Kubernetes Service (AKS) steuern.
services: container-service
ms.topic: article
ms.date: 06/30/2020
ms.openlocfilehash: eb2e7fca3a808a1e2c4f7d1f81b8dc1d64deeee7
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86077625"
---
# <a name="preview---secure-your-cluster-using-pod-security-policies-in-azure-kubernetes-service-aks"></a>Vorschauversion: Schützen Ihres Clusters mithilfe von Podsicherheitsrichtlinien in Azure Kubernetes Service (AKS)

<!--
> [!WARNING]
> **The pod security policy feature on AKS is set for deprecation** in favor of [Azure Policy for AKS](use-pod-security-on-azure-policy.md). The feature described in this document is not moving to general availability and is set for removal in September 2020.
> It is highly recommended to begin testing with the Azure Policy Add-on which offers unique policies which support scenarios captured by pod security policy.

**This document and feature are set for deprecation.**
-->

Zur Verbesserung der Sicherheit Ihres AKS-Clusters können Sie einschränken, welche Pods geplant werden können. Pods, die von Ihnen nicht zugelassene Ressourcen anfordern, können im AKS-Cluster nicht ausgeführt werden. Dieser Zugriff wird mithilfe von Podsicherheitsrichtlinien definiert. In diesem Artikel erfahren Sie, wie Sie Podsicherheitsrichtlinien verwenden, um die Bereitstellung von Pods in AKS einzuschränken.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauen werden teilweise vom Kundensupport auf der Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen hierzu finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service][aks-support-policies]
> * [Häufig gestellte Fragen zum Azure-Support][aks-faq]

## <a name="before-you-begin"></a>Voraussetzungen

Es wird vorausgesetzt, dass Sie über ein AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

Azure CLI-Version 2.0.61 oder höher muss installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter  [Installieren der Azure CLI][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Installieren der CLI-Erweiterung „aks-preview“

Um Podsicherheitsrichtlinien verwenden zu können, benötigen Sie die *aks-preview* CLI-Erweiterung Version 0.4.1 oder höher. Installieren Sie die Azure CLI-Erweiterung *aks-preview* mit dem Befehl [az extension add][az-extension-add], und suchen Sie dann mit dem Befehl [az extension update][az-extension-update] nach verfügbaren Updates:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-pod-security-policy-feature-provider"></a>Registrieren des Anbieters des Podsicherheitsrichtlinien-Features

Um einen AKS-Cluster für die Verwendung von Podsicherheitsrichtlinien zu erstellen oder zu aktualisieren, müssen Sie zunächst ein Featureflag für Ihr Abonnement aktivieren. Verwenden Sie den Befehl [az feature register][az-feature-register] wie im folgenden Beispiel gezeigt, um das Featureflag *PodSecurityPolicyPreview* zu registrieren:

```azurecli-interactive
az feature register --name PodSecurityPolicyPreview --namespace Microsoft.ContainerService
```

Es dauert einige Minuten, bis der Status *Registered (Registriert)* angezeigt wird. Sie können den Registrierungsstatus mithilfe des Befehls [az feature list][az-feature-list] überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/PodSecurityPolicyPreview')].{Name:name,State:properties.state}"
```

Wenn Sie so weit sind, aktualisieren Sie mithilfe des Befehls [az provider register][az-provider-register] die Registrierung des Ressourcenanbieters *Microsoft.ContainerService*:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="overview-of-pod-security-policies"></a>Übersicht über Podsicherheitsrichtlinien

In einem Kubernetes-Cluster wird eine Zugangssteuerung verwendet, um an den API-Server gerichtete Anforderungen abzufangen, wenn eine Ressource erstellt werden soll. Die Zugangssteuerung kann dann die Ressourcenanforderung anhand einer Reihe von Regeln *überprüfen* oder die Ressource *mutieren*, um die Bereitstellungsparameter zu ändern.

*PodSecurityPolicy* ist eine Zugangssteuerung, die überprüft, ob eine Podspezifikation die von Ihnen definierten Anforderungen erfüllt. Diese Anforderungen schränken möglicherweise die Verwendung privilegierter Container, den Zugriff auf bestimmte Arten von Speicher oder die Benutzer/Gruppen ein, als die der Container ausgeführt werden kann. Wenn Sie versuchen, eine Ressource bereitzustellen, bei der die Podspezifikationen nicht die in der Podsicherheitsrichtlinie beschriebenen Anforderungen erfüllen, wird die Anforderung abgelehnt. Die Möglichkeit zu steuern, welche Pods im AKS-Cluster geplant werden können, beugt einigen möglichen Sicherheitsrisiken und Rechteausweitungen vor.

Wenn Sie Podsicherheitsrichtlinien in einem AKS-Cluster aktivieren, werden einige Standardrichtlinien angewendet. Diese Standardrichtlinien sind sofort einsatzbereit und definieren, welche Pods geplant werden können. Clusterbenutzer haben jedoch möglicherweise Probleme bei der Podbereitstellung, bis Sie Ihre eigenen Richtlinien definieren. Empfohlene Vorgehensweise:

* Erstellen eines AKS-Clusters
* Definieren Ihrer eigenen Podsicherheitsrichtlinien
* Aktivieren des Podsicherheitsrichtlinien-Features

Um zu zeigen, wie Podbereitstellungen durch die Standardrichtlinien eingeschränkt werden, aktivieren wir in diesem Artikel zunächst das Podsicherheitsrichtlinien-Feature und erstellen dann eine benutzerdefinierte Richtlinie.

## <a name="enable-pod-security-policy-on-an-aks-cluster"></a>Aktivieren von Podsicherheitsrichtlinien für einen AKS-Cluster

Podsicherheitsrichtlinien können mithilfe des Befehls [az aks update][az-aks-update] aktiviert oder deaktiviert werden. Im folgenden Beispiel werden Podsicherheitsrichtlinien für den Clusternamen *myAKSCluster* in der Ressourcengruppe *myResourceGroup* aktiviert.

> [!NOTE]
> In der Praxis sollten Podsicherheitsrichtlinien erst aktiviert werden, nachdem Sie Ihre eigenen benutzerdefinierten Richtlinien definiert haben. In diesem Artikel werden Podsicherheitsrichtlinien im ersten Schritt aktiviert, um zu zeigen, wie Podbereitstellungen durch die Standardrichtlinien eingeschränkt werden.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-pod-security-policy
```

## <a name="default-aks-policies"></a>AKS-Standardrichtlinien

Wenn Sie Podsicherheitsrichtlinien aktivieren, erstellt AKS eine Standardrichtlinie mit dem Namen *privileged* (Privilegiert). Die Standardrichtlinie darf nicht bearbeitet oder entfernt werden. Erstellen Sie stattdessen Ihre eigenen Richtlinien, um die Einstellungen zu definieren, die Sie steuern möchten. Befassen wir uns zunächst mit den Standardrichtlinien und ihren Auswirkungen auf Podbereitstellungen.

Verwenden Sie zum Anzeigen der verfügbaren Richtlinien den Befehl [kubectl get psp][kubectl-get], wie im folgenden Beispiel gezeigt.

```console
$ kubectl get psp

NAME         PRIV    CAPS   SELINUX    RUNASUSER          FSGROUP     SUPGROUP    READONLYROOTFS   VOLUMES
privileged   true    *      RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
```

Die Podsicherheitsrichtlinie *privileged* gilt für alle authentifizierten Benutzer im AKS-Cluster. Diese Zuweisung wird durch Clusterrollen und Clusterrollenbindungen gesteuert. Verwenden Sie den Befehl [kubectl get clusterrolebindings][kubectl-get], und suchen Sie im Namespace *kube-system* nach der Bindung *default:privileged:* :

```console
kubectl get rolebindings default:privileged -n kube-system -o yaml
```

Wie in der folgenden gekürzten Ausgabe zu sehen, ist die Clusterrolle *psp:privileged* allen Benutzern vom Typ *system:authenticated* zugewiesen. Diese Funktionalität bietet eine einfache Berechtigungsebene, ohne dass Sie eigene Richtlinien definiert haben.

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  [...]
  name: default:privileged
  [...]
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: psp:privileged
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:masters
```

Es ist wichtig zu verstehen, wie diese Standardrichtlinien mit den Benutzeranforderungen für die Podplanung interagieren, bevor Sie mit der Erstellung Ihrer eigenen Podsicherheitsrichtlinien beginnen. In den nächsten Abschnitten werden einige Pods geplant, um die Auswirkungen der Standardrichtlinien zu veranschaulichen.

## <a name="create-a-test-user-in-an-aks-cluster"></a>Erstellen eines Testbenutzers in einem AKS-Cluster

Bei Verwendung des Befehls [az aks get-credentials][az-aks-get-credentials] werden Ihrer `kubectl`-Konfiguration standardmäßig die Administratoranmeldeinformationen (*admin*) für den AKS-Cluster hinzugefügt. Der Administratorbenutzer umgeht die Erzwingung von Podsicherheitsrichtlinien. Wenn Sie für Ihre AKS-Cluster die Azure Active Directory-Integration verwenden, können Sie sich mit den Anmeldeinformationen eines Benutzers ohne Administratorrechte anmelden, um die Erzwingung von Richtlinien in Aktion zu sehen. In diesem Artikel erstellen wir ein Testbenutzerkonto im AKS-Cluster, das Sie verwenden können.

Erstellen Sie mithilfe des Befehls [kubectl create namespace][kubectl-create] einen Beispielnamespace namens *psp-aks* für Testressourcen. Erstellen Sie anschließend mithilfe des Befehls [kubectl create serviceaccount][kubectl-create] ein Dienstkonto namens *nonadmin-user*:

```console
kubectl create namespace psp-aks
kubectl create serviceaccount --namespace psp-aks nonadmin-user
```

Erstellen Sie als Nächstes mithilfe des Befehls [kubectl create rolebinding][kubectl-create] eine Rollenbindung für *nonadmin-user*, um grundlegende Aktionen im Namespace ausführen zu können:

```console
kubectl create rolebinding \
    --namespace psp-aks \
    psp-aks-editor \
    --clusterrole=edit \
    --serviceaccount=psp-aks:nonadmin-user
```

### <a name="create-alias-commands-for-admin-and-non-admin-user"></a>Erstellen von Aliasbefehlen für Administratorbenutzer und Benutzer ohne Administratorrechte

Zur Verdeutlichung des Unterschieds bei der Verwendung von `kubectl` zwischen dem regulären Administratorbenutzer und dem Benutzer ohne Administratorrechte, der in den vorherigen Schritten erstellt wurde, erstellen wir zwei Befehlszeilenaliase:

* Der Alias **kubectl-admin** ist für den regulären Administratorbenutzer vorgesehen und auf den Namespace *psp-aks* ausgerichtet.
* Der Alias **kubectl-nonadminuser** ist für den im vorherigen Schritt erstellten Benutzer ohne Administratorrechte (*nonadmin-user*) vorgesehen und auf den Namespace *psp-aks* ausgerichtet.

Erstellen Sie diese beiden Aliase wie in den folgenden Befehlen gezeigt:

```console
alias kubectl-admin='kubectl --namespace psp-aks'
alias kubectl-nonadminuser='kubectl --as=system:serviceaccount:psp-aks:nonadmin-user --namespace psp-aks'
```

## <a name="test-the-creation-of-a-privileged-pod"></a>Testen der Erstellung eines privilegierten Pods

Testen Sie als Erstes, was passiert, wenn Sie einen Pod mit dem Sicherheitskontext `privileged: true` planen. Dieser Sicherheitskontext weitet die Podberechtigungen aus. Im vorherigen Abschnitt mit den standardmäßigen AKS-Podsicherheitsrichtlinien sollte diese Anforderung durch die Richtlinie *privilege* abgelehnt werden.

Erstellen Sie eine Datei mit dem Namen `nginx-privileged.yaml`, und fügen Sie das folgende YAML-Manifest ein:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged
spec:
  containers:
    - name: nginx-privileged
      image: nginx:1.14.2
      securityContext:
        privileged: true
```

Erstellen Sie den Pod mithilfe des Befehls [kubectl apply][kubectl-apply], und geben Sie den Namen Ihres YAML-Manifests an:

```console
kubectl-nonadminuser apply -f nginx-privileged.yaml
```

Der Pod kann nicht geplant werden, wie in der folgenden Beispielausgabe zu sehen:

```console
$ kubectl-nonadminuser apply -f nginx-privileged.yaml

Error from server (Forbidden): error when creating "nginx-privileged.yaml": pods "nginx-privileged" is forbidden: unable to validate against any pod security policy: []
```

Da der Pod die Planungsphase nicht erreicht, müssen keine Ressourcen gelöscht werden, und Sie können direkt mit dem nächsten Schritt fortfahren.

## <a name="test-creation-of-an-unprivileged-pod"></a>Testen der Erstellung eines nicht privilegierten Pods

Im vorherigen Beispiel wurde durch die Podspezifikation eine Rechteausweitung angefordert. Diese Anforderung wird durch die standardmäßige Podsicherheitsrichtlinie *privilege* abgelehnt, sodass der Pod nicht geplant werden kann. Versuchen Sie als Nächstes, den gleichen NGINX-Pod ohne Anforderung der Rechteausweitung auszuführen.

Erstellen Sie eine Datei mit dem Namen `nginx-unprivileged.yaml`, und fügen Sie das folgende YAML-Manifest ein:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx:1.14.2
```

Erstellen Sie den Pod mithilfe des Befehls [kubectl apply][kubectl-apply], und geben Sie den Namen Ihres YAML-Manifests an:

```console
kubectl-nonadminuser apply -f nginx-unprivileged.yaml
```

Der Pod kann nicht geplant werden, wie in der folgenden Beispielausgabe zu sehen:

```console
$ kubectl-nonadminuser apply -f nginx-unprivileged.yaml

Error from server (Forbidden): error when creating "nginx-unprivileged.yaml": pods "nginx-unprivileged" is forbidden: unable to validate against any pod security policy: []
```

Da der Pod die Planungsphase nicht erreicht, müssen keine Ressourcen gelöscht werden, und Sie können direkt mit dem nächsten Schritt fortfahren.

## <a name="test-creation-of-a-pod-with-a-specific-user-context"></a>Testen der Erstellung eines Pods mit einem spezifischen Benutzerkontext

Im vorherigen Beispiel hat das Containerimage automatisch versucht, Stammberechtigungen zu verwenden, um NGINX an den Port 80 zu binden. Diese Anforderung wurde durch die standardmäßige Podsicherheitsrichtlinie *privilege* abgelehnt, sodass der Pod nicht gestartet werden kann. Als Nächstes versuchen wir, den gleichen NGINX-Pod mit einem spezifischen Benutzerkontext (etwa `runAsUser: 2000`) auszuführen.

Erstellen Sie eine Datei mit dem Namen `nginx-unprivileged-nonroot.yaml`, und fügen Sie das folgende YAML-Manifest ein:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged-nonroot
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx:1.14.2
      securityContext:
        runAsUser: 2000
```

Erstellen Sie den Pod mithilfe des Befehls [kubectl apply][kubectl-apply], und geben Sie den Namen Ihres YAML-Manifests an:

```console
kubectl-nonadminuser apply -f nginx-unprivileged-nonroot.yaml
```

Der Pod kann nicht geplant werden, wie in der folgenden Beispielausgabe zu sehen:

```console
$ kubectl-nonadminuser apply -f nginx-unprivileged-nonroot.yaml

Error from server (Forbidden): error when creating "nginx-unprivileged-nonroot.yaml": pods "nginx-unprivileged-nonroot" is forbidden: unable to validate against any pod security policy: []
```

Da der Pod die Planungsphase nicht erreicht, müssen keine Ressourcen gelöscht werden, und Sie können direkt mit dem nächsten Schritt fortfahren.

## <a name="create-a-custom-pod-security-policy"></a>Erstellen einer benutzerdefinierten Podsicherheitsrichtlinie

Nachdem Sie nun mit dem Verhalten der Standard-Podsicherheitsrichtlinien vertraut sind, erfahren Sie als Nächstes, wie Sie dem Benutzer ohne Administratorrechte (*nonadmin-user*) die erfolgreiche Planung von Pods ermöglichen.

Hierzu erstellen wir eine Richtlinie zur Ablehnung von Pods, die privilegierten Zugriff anfordern. Andere Optionen wie *RunAsUser* oder zulässige *Volumes* werden explizit nicht eingeschränkt. Diese Art von Richtlinie lehnt die Anforderung von privilegiertem Zugriff ab, lässt aber ansonsten die Ausführung der angeforderten Pods durch den Cluster zu.

Erstellen Sie eine Datei mit dem Namen `psp-deny-privileged.yaml`, und fügen Sie das folgende YAML-Manifest ein:

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: psp-deny-privileged
spec:
  privileged: false
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  runAsUser:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
  volumes:
  - '*'
```

Erstellen Sie die Richtlinie mithilfe des Befehls [kubectl apply][kubectl-apply], und geben Sie den Namen Ihres YAML-Manifests an:

```console
kubectl apply -f psp-deny-privileged.yaml
```

Verwenden Sie zum Anzeigen der verfügbaren Richtlinien den Befehl [kubectl get psp][kubectl-get], wie im folgenden Beispiel gezeigt. Vergleichen Sie die Richtlinie *psp-deny-privileged* mit der Standardrichtlinie *privilege*, die in den vorherigen Beispielen für die Poderstellung erzwungen wurde. Durch Ihre Richtlinie wird lediglich die Verwendung von *PRIV* für die Rechteausweitung abgelehnt. Bei der Richtlinie *psp-deny-privileged* gibt es keinerlei Benutzer- oder Gruppeneinschränkungen.

```console
$ kubectl get psp

NAME                  PRIV    CAPS   SELINUX    RUNASUSER          FSGROUP     SUPGROUP    READONLYROOTFS   VOLUMES
privileged            true    *      RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *
psp-deny-privileged   false          RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *          
```

## <a name="allow-user-account-to-use-the-custom-pod-security-policy"></a>Zulassen der Verwendung der benutzerdefinierten Podsicherheitsrichtlinie durch das Benutzerkonto

Im vorherigen Schritt haben Sie eine Podsicherheitsrichtlinie erstellt, um Pods abzulehnen, die privilegierten Zugriff anfordern. Damit die Richtlinie verwendet werden kann, müssen Sie eine Rolle (*Role*) oder Clusterrolle (*ClusterRole*) erstellen. Anschließend müssen Sie eine dieser Rollen mithilfe einer Rollenbindung (*RoleBinding*) oder Clusterrollenbindung (*ClusterRoleBinding*) zuordnen.

In diesem Beispiel wird eine Clusterrolle erstellt, die es Ihnen ermöglicht, die im vorherigen Schritt erstellte Richtlinie *psp-deny-privileged* zu verwenden (*use*). Erstellen Sie eine Datei mit dem Namen `psp-deny-privileged-clusterrole.yaml`, und fügen Sie das folgende YAML-Manifest ein:

```yaml
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: psp-deny-privileged-clusterrole
rules:
- apiGroups:
  - extensions
  resources:
  - podsecuritypolicies
  resourceNames:
  - psp-deny-privileged
  verbs:
  - use
```

Erstellen Sie die Clusterrolle mithilfe des Befehls [kubectl apply][kubectl-apply], und geben Sie den Namen Ihres YAML-Manifests an:

```console
kubectl apply -f psp-deny-privileged-clusterrole.yaml
```

Erstellen Sie nun eine Clusterrollenbindung für die Verwendung der im vorherigen Schritt erstellten Clusterrolle. Erstellen Sie eine Datei mit dem Namen `psp-deny-privileged-clusterrolebinding.yaml`, und fügen Sie das folgende YAML-Manifest ein:

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: psp-deny-privileged-clusterrolebinding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: psp-deny-privileged-clusterrole
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:serviceaccounts
```

Erstellen Sie eine Clusterrollenbindung mithilfe des Befehls [kubectl apply][kubectl-apply], und geben Sie den Namen Ihres YAML-Manifests an:

```console
kubectl apply -f psp-deny-privileged-clusterrolebinding.yaml
```

> [!NOTE]
> Im ersten Schritt dieses Artikels wurde das Podsicherheitsrichtlinien-Feature für den AKS-Cluster aktiviert. Laut empfohlener Vorgehensweise sollte das Podsicherheitsrichtlinien-Feature allerdings erst aktiviert werden, nachdem Sie Ihre eigenen Richtlinien definiert haben. Das ist nun die Phase, in der Sie das Podsicherheitsrichtlinien-Feature normalerweise aktivieren würden. Es wurde mindestens eine benutzerdefinierte Richtlinie definiert, und den Richtlinien wurden Benutzerkonten zugeordnet. Nun können Sie problemlos das Podsicherheitsrichtlinien-Feature aktivieren und Probleme minimieren, die durch die Standardrichtlinien verursacht werden.

## <a name="test-the-creation-of-an-unprivileged-pod-again"></a>Erneutes Testen der Erstellung eines nicht privilegierten Pods

Versuchen Sie erneut, einen nicht privilegierten Pod zu erstellen, nachdem Sie Ihre benutzerdefinierte Podsicherheitsrichtlinie angewendet haben und über eine Bindung verfügen, damit das Benutzerkonto die Richtlinie verwenden kann. Verwenden Sie zum Erstellen des Pods das gleiche Manifest (`nginx-privileged.yaml`) und den Befehl [kubectl apply][kubectl-apply]:

```console
kubectl-nonadminuser apply -f nginx-unprivileged.yaml
```

Der Pod wird erfolgreich geplant. Wenn Sie mithilfe des Befehls [kubectl get pods][kubectl-get] den Status des Pods überprüfen, sehen Sie, dass der Pod ausgeführt wird (*Running*):

```
$ kubectl-nonadminuser get pods

NAME                 READY   STATUS    RESTARTS   AGE
nginx-unprivileged   1/1     Running   0          7m14s
```

Dieses Beispiel zeigt, wie Sie benutzerdefinierte Podsicherheitsrichtlinien erstellen, um den Zugriff auf den AKS-Cluster für verschiedene Benutzer oder Gruppen zu definieren. Mit den AKS-Standardrichtlinien wird sehr streng kontrolliert, welche Pods ausgeführt werden können. Erstellen Sie daher Ihre eigenen benutzerdefinierten Richtlinien, um die korrekten Einschränkungen für Ihre Anforderungen zu definieren.

Löschen Sie den nicht privilegierten NGINX-Pod mithilfe des Befehls [kubectl delete][kubectl-delete], und geben Sie den Namen Ihres YAML-Manifests an:

```console
kubectl-nonadminuser delete -f nginx-unprivileged.yaml
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Podsicherheitsrichtlinien können durch erneutes Ausführen des Befehls [az aks update][az-aks-update] deaktiviert werden. Im folgenden Beispiel werden Podsicherheitsrichtlinien für den Clusternamen *myAKSCluster* in der Ressourcengruppe *myResourceGroup* deaktiviert:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --disable-pod-security-policy
```

Löschen Sie als Nächstes die Clusterrolle und die Clusterrollenbindung:

```console
kubectl delete -f psp-deny-privileged-clusterrolebinding.yaml
kubectl delete -f psp-deny-privileged-clusterrole.yaml
```

Löschen Sie die Sicherheitsrichtlinie mithilfe des Befehls [kubectl delete][kubectl-delete], und geben Sie den Namen Ihres YAML-Manifests an:

```console
kubectl delete -f psp-deny-privileged.yaml
```

Löschen Sie abschließend den Namespace *psp-aks*:

```console
kubectl delete namespace psp-aks
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie eine Podsicherheitsrichtlinie erstellen, um die Verwendung von privilegiertem Zugriff zu verhindern. Mit einer Richtlinie lassen sich noch viele andere Features erzwingen – etwa die Art des Volumes oder der RunAs-Benutzer. Weitere Informationen zu den verfügbaren Optionen finden Sie in der [Referenzdokumentation zu Kubernetes-Podsicherheitsrichtlinien][kubernetes-policy-reference].

Weitere Informationen zum Einschränken des Netzwerkdatenverkehrs von Pods finden Sie unter [Vorschauversion: Sicherer Datenverkehr zwischen Pods durch Netzwerkrichtlinien in Azure Kubernetes Service (AKS)][network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[kubernetes-policy-reference]: https://kubernetes.io/docs/concepts/policy/pod-security-policy/#policy-reference

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policies]: use-network-policies.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
