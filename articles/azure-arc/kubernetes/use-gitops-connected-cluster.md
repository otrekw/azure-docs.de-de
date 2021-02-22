---
title: Bereitstellen von Konfigurationen mithilfe von GitOps in Arc-fähigen Kubernetes-Clustern (Vorschauversion)
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Verwenden von GitOps zum Konfigurieren eines Azure Arc-fähigen Kubernetes-Clusters (Vorschau)
keywords: GitOps, Kubernetes, K8s, Azure, Arc, Azure Kubernetes Service, AKS, Container
ms.openlocfilehash: 3cadcdf80abd997ec10aeb9521680944d455898f
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100560171"
---
# <a name="deploy-configurations-using-gitops-on-an-arc-enabled-kubernetes-cluster-preview"></a>Bereitstellen von Konfigurationen mithilfe von GitOps in einem Arc-fähigen Kubernetes-Cluster (Vorschauversion)

In diesem Artikel wird das Anwenden von Konfigurationen auf einen Azure Arc-fähigen Kubernetes-Cluster veranschaulicht. Eine konzeptionelle Übersicht dazu finden Sie [hier](./conceptual-configurations.md).

## <a name="before-you-begin"></a>Vorbereitung

* Überprüfen Sie, ob Sie über einen verbundenen, Azure Arc-fähigen Kubernetes-Cluster verfügen. Wenn Sie einen verbundenen Cluster benötigen, sehen Sie sich den [Schnellstart zum Verbinden eines Azure Arc-fähigen Kubernetes-Clusters](./connect-cluster.md) an.

* Informationen zu den Vorteilen und der Architektur dieses Features finden Sie im [Artikel „Konfigurationen und GitOps mit Arc für Kubernetes“](./conceptual-configurations.md).

## <a name="create-a-configuration"></a>Erstellen einer Konfiguration

Das in diesem Artikel verwendete [Beispielrepository](https://github.com/Azure/arc-k8s-demo) ist um die Persona eines Clusteroperators strukturiert, der einige Namespaces, eine gemeinsame Workload und teamspezifische Konfigurationen bereitstellen möchte. Durch Verwendung dieses Repositorys werden die folgenden Ressourcen in Ihrem Cluster erstellt:

* **Namespaces:** `cluster-config`, `team-a`, `team-b`
* **Bereitstellung:** `cluster-config/azure-vote`
* **ConfigMap:** `team-a/endpoints`

Der `config-agent` fragt Azure nach einer neuen oder aktualisierten `sourceControlConfiguration` ab. Diese Aufgabe kann bis zu 30 Sekunden dauern.

Führen Sie auch die Schritte unter [Anwenden der Konfiguration über ein privates Git-Repository](#apply-configuration-from-a-private-git-repository) aus, wenn Sie ein privates Repository mit `sourceControlConfiguration` zuordnen.

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Verwenden Sie die Azure CLI-Erweiterung für `k8sconfiguration`, um einen verbundenen Cluster mit dem [Git-Beispielrepository](https://github.com/Azure/arc-k8s-demo) zu verknüpfen. 
1. Benennen Sie diese Konfiguration `cluster-config`.
1. Weisen Sie den Agent an, den Operator im `cluster-config`-Namespace bereitzustellen.
1. Erteilen Sie dem Operator `cluster-admin`-Berechtigungen.

```azurecli
az k8sconfiguration create --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --operator-instance-name cluster-config --operator-namespace cluster-config --repository-url https://github.com/Azure/arc-k8s-demo --scope cluster --cluster-type connectedClusters
```

**Ausgabe:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Pending",
    "lastConfigApplied": "0001-01-01T00:00:00",
    "message": "{\"OperatorMessage\":null,\"ClusterState\":null}",
    "messageLevel": "3"
  },
  "configurationProtectedSettings": {},
  "enableHelmOperator": false,
  "helmOperatorProperties": null,
  "id": "/subscriptions/<sub id>/resourceGroups/<group name>/providers/Microsoft.Kubernetes/connectedClusters/<cluster name>/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "cluster-config",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "cluster-config",
  "operatorParams": "--git-readonly",
  "operatorScope": "cluster",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "",
  "repositoryUrl": "https://github.com/Azure/arc-k8s-demo",
  "resourceGroup": "MyRG",
  "sshKnownHostsContents": "",
  "systemData": {
    "createdAt": "2020-11-24T21:22:01.542801+00:00",
    "createdBy": null,
    "createdByType": null,
    "lastModifiedAt": "2020-11-24T21:22:01.542801+00:00",
    "lastModifiedBy": null,
    "lastModifiedByType": null
  },
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
  ```

#### <a name="use-a-public-git-repo"></a>Verwenden eines öffentlichen Git-Repositorys

| Parameter | Format |
| ------------- | ------------- |
| `--repository-url` | http[s]://server/repo[.git] oder git://server/repo[.git]

#### <a name="use-a-private-git-repo-with-ssh-and-flux-created-keys"></a>Verwenden eines privaten Git-Repositorys mit SSH und mit Flux erstellten Schlüsseln

| Parameter | Format | Notizen
| ------------- | ------------- | ------------- |
| `--repository-url` | ssh://user@server/repo[.git] oder user@server:repo[.git] | `git@` kann `user@` ersetzen

> [!NOTE]
> Der mit Flux generierte öffentliche Schlüssel muss dem Benutzerkonto in Ihrem Git-Dienstanbieter hinzugefügt werden. Wenn der Schlüssel dem Repository statt dem Benutzerkonto hinzugefügt wird, verwenden Sie in der URL `git@` anstelle von `user@`. Weitere Informationen finden Sie im Abschnitt [Anwenden einer Konfiguration aus einem privaten Git-Repository](#apply-configuration-from-a-private-git-repository).

#### <a name="use-a-private-git-repo-with-ssh-and-user-provided-keys"></a>Verwenden eines privaten Git-Repositorys mit SSH und benutzerseitig bereitgestellten Schlüsseln

| Parameter | Format | Notizen |
| ------------- | ------------- | ------------- |
| `--repository-url`  | ssh://user@server/repo[.git] oder user@server:repo[.git] | `git@` kann `user@` ersetzen |
| `--ssh-private-key` | base64-codierter Schlüssel im [PEM-Format](https://aka.ms/PEMformat) | Direktes Bereitstellen des Schlüssels |
| `--ssh-private-key-file` | Vollständiger Pfad zur lokalen Datei | Bereitstellen des vollständigen Pfads zur lokalen Datei, die den Schlüssel im PEM-Format enthält

> [!NOTE]
> Stellen Sie Ihren eigenen privaten Schlüssel direkt oder in einer Datei bereit. Der Schlüssel muss im [PEM-Format](https://aka.ms/PEMformat) sein und mit einem Zeilenumbruch (\n) enden.  Der zugeordnete öffentliche Schlüssel muss dem Benutzerkonto in Ihrem Git-Dienstanbieter hinzugefügt werden. Wenn der Schlüssel dem Repository statt dem Benutzerkonto hinzugefügt wird, verwenden Sie `git@` anstelle von `user@`. Weitere Informationen finden Sie im Abschnitt [Anwenden einer Konfiguration aus einem privaten Git-Repository](#apply-configuration-from-a-private-git-repository).

#### <a name="use-a-private-git-host-with-ssh-and-user-provided-known-hosts"></a>Verwenden eines privaten Git-Hosts mit SSH und benutzerseitig bereitgestellten bekannten Hosts

| Parameter | Format | Notizen |
| ------------- | ------------- | ------------- |
| `--repository-url`  | ssh://user@server/repo[.git] oder user@server:repo[.git] | `git@` kann `user@` ersetzen |
| `--ssh-known-hosts` | base64-codiert | Inhalte bekannter Hosts direkt bereitstellen |
| `--ssh-known-hosts-file` | Vollständiger Pfad zur lokalen Datei | Inhalte bekannter Hosts in einer lokalen Datei bereitstellen |

> [!NOTE]
> Um das Git-Repository vor dem Einrichten der SSH-Verbindung zu authentifizieren, verwaltet der Flux-Operator eine Liste gängiger Git-Hosts in seiner Datei bekannter Hosts. Wenn Sie ein nicht gängiges Git-Repository oder Ihren eigenen Git-Host verwenden, müssen Sie möglicherweise den Hostschlüssel bereitstellen, um sicherzustellen, dass Flux Ihr Repository identifizieren kann. Sie können Ihren known_hosts-Inhalt (bekannte Hosts) direkt oder in einer Datei bereitstellen. Verwenden Sie die [Formatspezifikationen für known_hosts-Inhalt](https://aka.ms/KnownHostsFormat) zusammen mit einem der oben beschriebenen SSH-Schlüsselszenarien, wenn Sie Ihren eigenen Inhalt bereitstellen.

#### <a name="use-a-private-git-repo-with-https"></a>Verwenden eines privaten Git-Repository mit HTTPS

| Parameter | Format | Notizen |
| ------------- | ------------- | ------------- |
| `--repository-url` | https://server/repo [.git] | HTTPS mit Standardauthentifizierung |
| `--https-user` | Unformatiert (raw) base64-codiert | HTTPS-Benutzername |
| `--https-key` | Unformatiert (raw) base64-codiert | Persönliches HTTPS-Zugriffstoken oder -Kennwort

> [!NOTE]
> Die private Authentifizierung von HTTPS Helm Release wird nur mit der Chartversion 1.2.0 und höher (Standard) des Helm-Operators unterstützt.
> Die private Authentifizierung von HTTPS Helm Release wird für verwaltete Azure Kubernetes Services-Cluster derzeit nicht unterstützt.
> Wenn Sie Flux benötigen, um über Ihren Proxy auf das Git-Repository zuzugreifen, müssen Sie die Azure Arc-Agents mit den Proxyeinstellungen aktualisieren. Informationen finden Sie unter [Herstellen einer Verbindung mithilfe eines ausgehenden Proxyservers](./connect-cluster.md#connect-using-an-outbound-proxy-server).

#### <a name="additional-parameters"></a>Zusätzliche Parameter

Passen Sie die Konfiguration mit den folgenden optionalen Parametern an:

| Parameter | BESCHREIBUNG |
| ------------- | ------------- |
| `--enable-helm-operator`| Switch zum Aktivieren der Unterstützung für Helm-Chartbereitstellungen. |
| `--helm-operator-params` | Chartwerte für den Helm-Operator (falls aktiviert). Beispiel: `--set helm.versions=v3`. |
| `--helm-operator-version` | Chartversion für den Helm-Operator (falls aktiviert). Verwenden Sie Version 1.2.0 und höher. Standardwert: ‚1.2.0‘. |
| `--operator-namespace` | Name für den Operatornamespace. Standardwert: „default“. Max: 23 Zeichen. |
| `--operator-params` | Parameter für den Operator. Diese müssen in einfachen Anführungszeichen angegeben werden. Zum Beispiel, ```--operator-params='--git-readonly --sync-garbage-collection --git-branch=main' ``` 

##### <a name="options-supported-in----operator-params"></a>In `--operator-params` unterstützte Optionen:

| Option | BESCHREIBUNG |
| ------------- | ------------- |
| `--git-branch`  | Die Verzweigung des Git-Repositorys für Kubernetes-Manifeste. Der Standardwert ist „master“. Neuere Repositorys haben einen Stammzweig namens `main`, in diesem Fall müssen Sie `--git-branch=main` festlegen. |
| `--git-path`  | Dies ist der relative Pfad im Git-Repository für Flux zum Suchen von Kubernetes-Manifesten. |
| `--git-readonly` | Das Git-Repository wird als schreibgeschützt betrachtet. Flux versucht nicht, in die Datei zu schreiben. |
| `--manifest-generation`  | Wenn diese Option aktiviert ist, sucht Flux nach „.flux.yaml“ und führt Kustomize oder andere Manifest-Generatoren aus. |
| `--git-poll-interval`  | Dies ist die Zeitspanne, in der das Git-Repository für neue Commits abgerufen wird. Der Standardwert ist `5m` (fünf Minuten). |
| `--sync-garbage-collection`  | Wenn diese Option aktiviert ist, werden die von Flux erstellten Ressourcen gelöscht und sind nicht mehr in Git verfügbar. |
| `--git-label`  | Bezeichnung zum Nachverfolgen des Synchronisierungsprozesses. Wird verwendet, um die Git-Verzweigung zu markieren.  Der Standardwert ist `flux-sync`. |
| `--git-user`  | Benutzername für Git-Commits. |
| `--git-email`  | Für Git-Commits zu verwendende E-Mail-Adresse. 

Wenn Flux nicht in das Repository schreiben soll und `--git-user` oder `--git-email` nicht festgelegt ist, wird `--git-readonly` automatisch festgelegt.

Weitere Informationen finden Sie in der [Flux-Dokumentation](https://aka.ms/FluxcdReadme).

> [!TIP]
> Sie können eine `sourceControlConfiguration` im Azure-Portal auf der Registerkarte **GitOps** der Azure Arc-fähigen Kubernetes-Ressource erstellen.

## <a name="validate-the-sourcecontrolconfiguration"></a>Überprüfen der „sourceControlConfiguration“-Ressource

Überprüfen Sie mithilfe der Azure CLI, ob die `sourceControlConfiguration`-Ressource erfolgreich erstellt wurde.

```azurecli
az k8sconfiguration show --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

Die `sourceControlConfiguration`-Ressource wird mit Compliancestatus, mit Nachrichten und Debuginformationen aktualisiert.

**Ausgabe:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Installed",
    "lastConfigApplied": "2020-12-10T18:26:52.801000+00:00",
    "message": "...",
    "messageLevel": "Information"
  },
  "configurationProtectedSettings": {},
  "enableHelmOperator": false,
  "helmOperatorProperties": {
    "chartValues": "",
    "chartVersion": ""
  },
  "id": "/subscriptions/<sub id>/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "cluster-config",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "cluster-config",
  "operatorParams": "--git-readonly",
  "operatorScope": "cluster",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "...",
  "repositoryUrl": "git://github.com/Azure/arc-k8s-demo.git",
  "resourceGroup": "AzureArcTest",
  "sshKnownHostsContents": null,
  "systemData": {
    "createdAt": "2020-12-01T03:58:56.175674+00:00",
    "createdBy": null,
    "createdByType": null,
    "lastModifiedAt": "2020-12-10T18:30:56.881219+00:00",
    "lastModifiedBy": null,
    "lastModifiedByType": null
},
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

Wenn die Ressource `sourceControlConfiguration` erstellt oder aktualisiert wird, geschieht im Hintergrund Folgendes:

1. Der `config-agent` von Azure Arc überwacht Azure Resource Manager auf neue oder aktualisierte Konfigurationen (`Microsoft.KubernetesConfiguration/sourceControlConfigurations`) und erkennt die neue Konfiguration `Pending`.
1. Der `config-agent` liest die Konfigurationseigenschaften und erstellt den Zielnamespace.
1. Der `controller-manager` von Azure Arc bereitet ein Kubernetes-Dienstkonto mit der entsprechenden Berechtigung vor (Bereich `cluster` oder `namespace`) und stellt dann eine Instanz von `flux` bereit.
1. Bei Verwendung der Option von SSH mit von Flux generierten Schlüsseln generiert `flux` einen SSH-Schlüssel und protokolliert den öffentlichen Schlüssel.
1. Der `config-agent` meldet den Status zurück an die Ressource `sourceControlConfiguration` in Azure.

Während des Bereitstellungsprozesses wird der Status der `sourceControlConfiguration`-Ressource mehrfach geändert. Überwachen Sie den Fortschritt mit dem obigen `az k8sconfiguration show ...`-Befehl:

| Phasenänderung | BESCHREIBUNG |
| ------------- | ------------- |
| `complianceStatus`-> `Pending` | Hiermit werden der anfängliche Status und der Status während der Bearbeitung dargestellt. |
| `complianceStatus` -> `Installed`  | `config-agent` konnte den Cluster erfolgreich konfigurieren und `flux` ohne Fehler bereitstellen. |
| `complianceStatus` -> `Failed` | Bei `config-agent` ist ein Fehler beim Bereitstellen von `flux` aufgetreten. Details sollten im `complianceStatus.message`-Antworttext zu finden sein. |

## <a name="apply-configuration-from-a-private-git-repository"></a>Anwenden der Konfiguration aus einem privaten Git-Repository

Wenn Sie ein privates Git-Repository verwenden, müssen Sie den öffentlichen SSH-Schlüssel in Ihrem Repository konfigurieren. Der öffentliche SSH-Schlüssel ist entweder der von Flux generierte oder der von Ihnen bereitgestellte. Sie können den öffentlichen Schlüssel entweder für das spezifische Git-Repository oder den Git-Benutzer konfigurieren, der Zugriff auf das Repository hat. 

### <a name="get-your-own-public-key"></a>Abrufen Ihres eigenen öffentlichen Schlüssels

Wenn Sie Ihre eigenen SSH-Schlüssel generiert haben, verfügen Sie bereits über die privaten und öffentlichen Schlüssel.

#### <a name="get-the-public-key-using-azure-cli"></a>Abrufen des öffentlichen Schlüssels mithilfe der Azure CLI 

Folgendes ist nützlich, wenn Flux die Schlüssel generiert.

```console
$ az k8sconfiguration show --resource-group <resource group name> --cluster-name <connected cluster name> --name <configuration name> --cluster-type connectedClusters --query 'repositoryPublicKey' 
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAREDACTED"
```

#### <a name="get-the-public-key-from-the-azure-portal"></a>Abrufen des öffentlichen Schlüssels über das Azure-Portal

Folgendes ist nützlich, wenn Flux die Schlüssel generiert.

1. Navigieren Sie im Azure-Portal zur verbundenen Clusterressource.
2. Wählen Sie auf der Ressourcenseite „GitOps“ aus, um die Liste der Konfigurationen für diesen Cluster anzuzeigen.
3. Klicken Sie auf die Konfiguration, die das private Git-Repository verwendet.
4. Kopieren Sie den **öffentlichen Repositoryschlüssel** im geöffneten Kontextfenster unten auf der Seite.

#### <a name="add-public-key-using-github"></a>Hinzufügen eines öffentlichen Schlüssels mithilfe von GitHub

Nutzen Sie eine der folgenden Optionen:

* Option 1: Hinzufügen des öffentlichen Schlüssels zu Ihrem Benutzerkonto (gilt für alle Repositorys in Ihrem Konto):  
    1. Öffnen Sie GitHub, und klicken Sie in der oberen rechten Ecke der Seite auf Ihr Profilsymbol.
    2. Klicken Sie auf **Einstellungen**.
    3. Klicken Sie auf **SSH- und GPG-Schlüssel**.
    4. Klicken Sie auf **Neuer SSH-Schlüssel**.
    5. Geben Sie einen Titel an.
    6. Fügen Sie den öffentlichen Schlüssel ohne die diesen umgebenden Anführungszeichen ein.
    7. Klicken Sie auf **SSH-Schlüssel hinzufügen**.

* Option 2: Hinzufügen des öffentlichen Schlüssels als Bereitstellungsschlüssel zum Git-Repository (gilt nur für dieses Repository):  
    1. Öffnen Sie GitHub, und navigieren Sie zu Ihrem Repository.
    1. Klicken Sie auf **Einstellungen**.
    1. Klicken Sie auf **Bereitstellungsschlüssel**.
    1. Klicken Sie auf **Bereitstellungsschlüssel hinzufügen**.
    1. Geben Sie einen Titel an.
    1. Aktivieren Sie **Schreibzugriff gewähren**.
    1. Fügen Sie den öffentlichen Schlüssel ohne die diesen umgebenden Anführungszeichen ein.
    1. Klicken Sie auf **Schlüssel hinzufügen**.

#### <a name="add-public-key-using-an-azure-devops-repository"></a>Hinzufügen eines öffentlichen Schlüssels mit einem Azure DevOps-Repository

Führen Sie die folgenden Schritte aus, um den Schlüssel zu Ihren SSH-Schlüsseln hinzuzufügen:

1. Klicken Sie unter **Benutzereinstellungen** oben rechts (neben dem Profilbild) auf **Öffentliche SSH-Schlüssel**.
1. Klicken Sie auf **+ Neuer Schlüssel**.
1. Geben Sie einen Namen an.
1. Fügen Sie den öffentlichen Schlüssel ohne die diesen umgebenden Anführungszeichen ein.
1. Klicken Sie auf **Hinzufügen**.

## <a name="validate-the-kubernetes-configuration"></a>Überprüfen der Kubernetes-Konfiguration

Nachdem der `config-agent` die `flux`-Instanz installiert hat, sollten die im Git-Repository gespeicherten Ressourcen an den Cluster weitergeleitet werden. Überprüfen Sie, ob die Namespaces, Bereitstellungen und Ressourcen erstellt wurden, mit dem folgenden Befehl:

```console
kubectl get ns --show-labels
```

**Ausgabe:**

```console
NAME              STATUS   AGE    LABELS
azure-arc         Active   24h    <none>
cluster-config    Active   177m   <none>
default           Active   29h    <none>
itops             Active   177m   fluxcd.io/sync-gc-mark=sha256.9oYk8yEsRwWkR09n8eJCRNafckASgghAsUWgXWEQ9es,name=itops
kube-node-lease   Active   29h    <none>
kube-public       Active   29h    <none>
kube-system       Active   29h    <none>
team-a            Active   177m   fluxcd.io/sync-gc-mark=sha256.CS5boSi8kg_vyxfAeu7Das5harSy1i0gc2fodD7YDqA,name=team-a
team-b            Active   177m   fluxcd.io/sync-gc-mark=sha256.vF36thDIFnDDI2VEttBp5jgdxvEuaLmm7yT_cuA2UEw,name=team-b
```

Wir sehen, dass die Namespaces `team-a`, `team-b`, `itops` und `cluster-config` erstellt wurden.

Der `flux`-Operator wurde wie von unserer `sourceControlConfig`-Ressource im `cluster-config`-Namespace bereitgestellt:

```console
kubectl -n cluster-config get deploy  -o wide
```

**Ausgabe:**

```console
NAME             READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES                         SELECTOR
cluster-config   1/1     1            1           3h    flux         docker.io/fluxcd/flux:1.16.0   instanceName=cluster-config,name=flux
memcached        1/1     1            1           3h    memcached    memcached:1.5.15               name=memcached
```

## <a name="further-exploration"></a>Weitere Untersuchungen

Sie können die anderen Ressourcen, die als Teil des Konfigurationsrepositorys bereitgestellt werden, mit Folgendem untersuchen:

```console
kubectl -n team-a get cm -o yaml
kubectl -n itops get all
```

## <a name="delete-a-configuration"></a>Löschen einer Konfiguration

Löschen Sie eine `sourceControlConfiguration`-Ressource mithilfe der Azure CLI oder über das Azure-Portal.  Nachdem Sie den Löschenbefehl initiiert haben, wird die `sourceControlConfiguration`-Ressource sofort in Azure gelöscht. Das vollständige Löschen der zugeordneten Objekte aus dem Cluster sollte innerhalb von 10 Minuten erfolgen. Wenn sich die `sourceControlConfiguration` beim Entfernen in einem fehlerhaften Zustand befindet, kann das vollständige Löschen der zugeordneten Objekte bis zu einer Stunde dauern.

> [!NOTE]
> Nachdem eine `sourceControlConfiguration` mit `namespace`-Umfang erstellt wurde, können Benutzer mit der `edit`-Rollenbindung an den Namespace Workloads in diesem Namespace bereitstellen. Wenn diese `sourceControlConfiguration`-Ressource mit Namespacebereich gelöscht wird, bleibt der Namespace intakt und wird nicht gelöscht, um zu vermeiden, dass diese anderen Workloads unterbrochen werden. Bei Bedarf können Sie diesen Namespace manuell mit `kubectl` löschen.  
> Alle Änderungen am Cluster, die das Ergebnis von Bereitstellungen aus dem nachverfolgten Git-Repository sind, werden beim Löschen der `sourceControlConfiguration`-Ressource nicht gelöscht.

```azurecli
az k8sconfiguration delete --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

**Ausgabe:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von GitOps mit Helm für Azure Arc-fähige Clusterkonfiguration (Vorschauversion)](./use-gitops-with-helm.md)
- [Verwenden von Azure Policy zum Anwenden von Clusterkonfigurationen im großen Stil (Vorschau)](./use-azure-policy.md)
