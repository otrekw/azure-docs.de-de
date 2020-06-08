---
title: Verwenden von GitOps für Azure Arc-fähiger Clusterkonfiguration (Vorschauversion)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Verwenden von GitOps für Azure Arc-fähiger Clusterkonfiguration (Vorschauversion)
keywords: GitOps, Kubernetes, K8s, Azure, Arc, Azure Kubernetes Service, Container
ms.openlocfilehash: 954c77503e8adacc4cd27b25b68b50cac1f80458
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83779712"
---
# <a name="use-gitops-for-an-azure-arc-enabled--configuration-preview"></a>Verwenden von GitOps für Azure Arc-fähige Konfiguration (Vorschauversion)

Diese Architektur verwendet einen GitOps-Workflow, um den Cluster zu konfigurieren und Anwendungen bereitzustellen. Die Konfiguration wird deklarativ in YAML-Dateien beschrieben und in Git gespeichert. Ein Agent überwacht das Git-Repository auf Änderungen und wendet diese an.  Derselbe Agent stellt außerdem regelmäßig sicher, dass der Clusterstatus mit dem im Git-Repository deklarierten Status übereinstimmt, und er stellt den gewünschten Zustand für den Cluster her, wenn nicht verwaltete Änderungen aufgetreten sind.

Die Verbindung zwischen Ihrem Cluster und mindestens einem Git-Repository wird in Azure Resource Manager als `sourceControlConfiguration`-Erweiterungsressource nachverfolgt. Die `sourceControlConfiguration`-Ressourceneigenschaften stellen dar, wohin und wie Kubernetes-Ressourcen von Git an Ihren Cluster weitergeleitet werden sollen. Die `sourceControlConfiguration`-Daten werden im Ruhezustand verschlüsselt in einer Azure Cosmos DB-Datenbank gespeichert, um ihre Vertraulichkeit zu gewährleisten.

Der Azure Arc-fähige `config-agent` von Kubernetes, der in Ihrem Cluster ausgeführt wird, ist für die Überwachung neuer oder aktualisierter `sourceControlConfiguration`-Ressourcen zuständig und orchestriert das Hinzufügen, Aktualisieren oder Entfernen von Git-Repositorylinks automatisch.

Die gleichen Muster können verwendet werden, um eine größere Sammlung von Clustern zu verwalten, die in heterogenen Umgebungen bereitgestellt werden können. Sie können beispielsweise mit einem Repository die Baselinekonfiguration für Ihre Organisation definieren und diese auf zehn Kubernetes-Cluster gleichzeitig anwenden.

Das Git-Repository kann gültige Kubernetes-Ressourcen einschließlich Namespaces, ConfigMaps, Bereitstellungen, DaemonSets usw. enthalten.  Es kann auch Helm-Charts für die Bereitstellung von Anwendungen enthalten. Mehrere allgemeine Szenarios umfassen das Definieren einer Baselinekonfiguration für Ihre Organisation, die allgemeine RBAC-Rollen und -Bindungen, Überwachungs- oder Protokollierungs-Agents oder clusterweite Dienste beinhalten kann.

Dieser Leitfaden für die ersten Schritte erläutert das Anwenden mehrerer Konfigurationen für Cluster auf Administratorebene.

## <a name="create-a-configuration"></a>Erstellen einer Konfiguration

- Beispielrepository: <https://github.com/Azure/arc-k8s-demo>

Das Beispielrepository ist um die Persona eines Clusteroperators strukturiert, der einige Namespaces, eine gemeinsame Workload und teamspezifische Konfigurationen bereitstellen möchte. Durch Verwendung dieses Repositorys werden die folgenden Ressourcen in Ihrem Cluster erstellt:

**Namespaces:** `cluster-config`, `team-a`, `team-b`
**Bereitstellung:** `cluster-config/azure-vote`
**ConfigMap:** `team-a/endpoints`

Der `config-agent` fragt Azure alle 30 Sekunden nach einer neuen oder aktualisierten `sourceControlConfiguration`-Ressource.  Dies ist die maximale Zeitspanne, die der `config-agent` benötigt, um eine neue oder aktualisierte Konfiguration zu übernehmen.
Stellen Sie sicher, dass Sie auch die Schritte im Artikel [Anwenden der Konfiguration über ein privates Git-Repository](#apply-configuration-from-a-private-git-repository) ausführen, wenn Sie ein privates Repository zuordnen.

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Mit der Azure CLI-Erweiterung für `k8sconfiguration` verknüpfen wir den verbundenen Cluster mit einem [Git-Beispielrepository](https://github.com/Azure/arc-k8s-demo). Wir geben dieser Konfiguration den Namen `cluster-config`, weisen den Agent an, den Operator im `cluster-config`-Namespace bereitzustellen, und erteilen ihm `cluster-admin`-Berechtigungen.

```console
az k8sconfiguration create \
    --name cluster-config \
    --cluster-name AzureArcTest1 --resource-group AzureArcTest \
    --operator-instance-name cluster-config --operator-namespace cluster-config \
    --repository-url https://github.com/Azure/arc-k8s-demo \
    --scope cluster --cluster-type connectedClusters
```

**Ausgabe:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "ComplianceStatus": 1,
    "clientAppliedTime": null,
    "level": 3,
    "message": "{\"OperatorMessage\":null,\"ClusterState\":null}"
  },
  "configKind": "Git",
  "configName": "cluster-config",
  "configOperator": {
    "operatorInstanceName": "cluster-config",
    "operatorNamespace": "cluster-config",
    "operatorParams": "--git-readonly",
    "operatorScope": "cluster",
    "operatorType": "flux"
  },
  "configType": "",
  "id": null,
  "name": null,
  "operatorInstanceName": null,
  "operatorNamespace": null,
  "operatorParams": null,
  "operatorScope": null,
  "operatorType": null,
  "providerName": "ConnectedClusters",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": null,
  "repositoryUrl": null,
  "sourceControlConfiguration": {
    "repositoryPublicKey": "",
    "repositoryUrl": "git://github.com/Azure/arc-k8s-demo.git"
  },
  "type": null
}
```

#### <a name="repository-url-parameter"></a>Parameter „repository-url“

Im Folgenden finden Sie die unterstützten Szenarios für den Wert des Parameters „--repository-url“.

| Szenario | Format | BESCHREIBUNG |
| ------------- | ------------- | ------------- |
| Privates GitHub-Repository (SSH) | git@github.com:username/repo | Das SSH-Schlüsselpaar wird von Flux generiert.  Der Benutzer muss dem GitHub-Konto den öffentlichen Schlüssel als Bereitstellungsschlüssel hinzufügen. |
| Öffentliches GitHub-Repository | `http://github.com/username/repo` oder git://github.com/username/repo   | Öffentliches Git-Repository  |

Diese Szenarios werden von Flux, jedoch noch nicht von der „sourceControlConfiguration“-Ressource unterstützt. 

| Szenario | Format | BESCHREIBUNG |
| ------------- | ------------- | ------------- |
| Privates GitHub-Repository (HTTPS) | `https://github.com/username/repo` | Flux generiert kein SSH-Schlüsselpaar.  [Anweisungen](https://docs.fluxcd.io/en/1.17.0/guides/use-git-https.html) |
| Privater Git-Host | user@githost:path/to/repo | [Anweisungen](https://docs.fluxcd.io/en/1.18.0/guides/use-private-git-host.html) |
| Privates GitHub-Repository (SSH, Bring Your Own Key) | git@github.com:username/repo | [Verwenden eines eigenen SSH-Schlüsselpaars](https://docs.fluxcd.io/en/1.17.0/guides/provide-own-ssh-key.html) |


#### <a name="additional-parameters"></a>Zusätzliche Parameter

Hier finden Sie einige zusätzliche Parameter, um die Erstellung der Konfiguration anzupassen.

`--enable-helm-operator` : *Optionaler* Switch zum Aktivieren der Unterstützung für Helm-Chartbereitstellungen. Diese Einstellung ist standardmäßig deaktiviert.

`--helm-operator-chart-values` : *Optionale* Chartwerte für den Helm-Operator (falls aktiviert).  Beispiel: „--set helm.versions=v3“

`--helm-operator-chart-version` : *Optionale* Chartversion für den Helm-Operator (falls aktiviert). Standardwert: „0.6.0“

`--operator-namespace` : *Optionaler* Name für den Operatornamespace. Standardwert: „default“

`--operator-params` : *Optionale* Parameter für den Operator. Diese müssen in einfachen Anführungszeichen angegeben werden. Zum Beispiel, ```--operator-params='--git-readonly --git-path=releases/prod' ```

Unterstützte Optionen in „--operator-params“

| Option | BESCHREIBUNG |
| ------------- | ------------- |
| --git-branch  | Dies ist die Verzweigung des Git-Repositorys für Kubernetes-Manifeste. Der Standardwert ist „master“. |
| --git-path  | Dies ist der relative Pfad im Git-Repository für Flux zum Suchen von Kubernetes-Manifesten. |
| --git-readonly | Das Git-Repository wird als schreibgeschützt betrachtet. Flux versucht nicht, in die Datei zu schreiben. |
| --manifest-generation  | Wenn diese Option aktiviert ist, sucht Flux nach „.flux.yaml“ und führt Kustomize oder andere Manifest-Generatoren aus. |
| --git-poll-interval  | Dies ist die Zeitspanne, in der das Git-Repository für neue Commits abgerufen wird. Der Standardwert ist „5m“ (fünf Minuten). |
| --sync-garbage-collection  | Wenn diese Option aktiviert ist, werden die von Flux erstellten Ressourcen gelöscht und sind nicht mehr in Git verfügbar. |
| --git-label  | Dies ist die Bezeichnung zum Nachverfolgen des Synchronisierungsprozesses, der verwendet wird, um die Git-Verzweigung zu markieren.  Der Standardwert ist „flux-sync“. |
| --git-user  | Dies ist der Benutzername für Git-Commits. |
| --git-email  | Dies ist die für Git-Commits zu verwendende E-Mail-Adresse. |

* Wenn „--git-user“ oder „--git-email“ nicht festgelegt ist, da Sie nicht möchten, dass Flux in das Repository schreibt, wird „--git-readonly“ (falls nicht bereits festgelegt) automatisch festgelegt.

* Wenn „enableHelmOperator“ den Wert „True“ aufweist, können die Zeichenfolgen „operatorInstanceName“ und „operatorNamespace“ zusammen nicht mehr als 47 Zeichen enthalten.  Wenn Sie diesen Grenzwert nicht einhalten, erhalten Sie den folgenden Fehler:

   ```console
   {"OperatorMessage":"Error: {failed to install chart from path [helm-operator] for release [<operatorInstanceName>-helm-<operatorNamespace>]: err [release name \"<operatorInstanceName>-helm-<operatorNamespace>\" exceeds max length of 53]} occurred while doing the operation : {Installing the operator} on the config","ClusterState":"Installing the operator"}
   ```

Weitere Informationen finden Sie in der [Flux-Dokumentation](https://aka.ms/FluxcdReadme).

## <a name="validate-the-sourcecontrolconfiguration"></a>Überprüfen der „sourceControlConfiguration“-Ressource

Überprüfen Sie mithilfe der Azure CLI, ob die `sourceControlConfiguration`-Ressource erfolgreich erstellt wurde.

```console
az k8sconfiguration show --resource-group AzureArcTest --name cluster-config --cluster-name AzureArcTest1 --cluster-type connectedClusters
```

Beachten Sie, dass die `sourceControlConfiguration`-Ressource mit einem Compliancestatus, Nachrichten und Debuginformationen aktualisiert wird.

**Ausgabe:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Compliant",
    "lastConfigApplied": "2019-12-05T05:34:41.481000",
    "message": "...",
    "messageLevel": "3"
  },
  "id": "/subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
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
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

Wenn die `sourceControlConfiguration`-Ressource erstellt wird, geschieht im Hintergrund Folgendes:

1. Der `config-agent` von Azure Arc überwacht den Azure Resource Manager auf neue oder aktualisierte Konfigurationen (`Microsoft.KubernetesConfiguration/sourceControlConfiguration`).
1. Der `config-agent` bemerkt die neue `Pending`-Konfiguration.
1. Der `config-agent` liest die Konfigurationseigenschaften und bereitet die Bereitstellung einer verwalteten Instanz von `flux` vor.
    * Der `config-agent` erstellt den Zielnamespace.
    * Der `config-agent` bereitet ein Kubernetes-Dienstkonto mit der entsprechenden Berechtigung vor (`cluster`- oder `namespace`-Bereich).
    * Der `config-agent` stellt eine Instanz von `flux` bereit.
    * `flux` generiert einen SSH-Schlüssel und protokolliert den öffentlichen Schlüssel.
1. Der `config-agent` meldet den Status zurück an die `sourceControlConfiguration`-Ressource.

Während des Bereitstellungsprozesses wird der Status der `sourceControlConfiguration`-Ressource mehrfach geändert. Überwachen Sie den Fortschritt mit dem obigen `az k8sconfiguration show ...`-Befehl:

1. `complianceStatus` -> `Pending`: Hiermit werden der anfängliche Status und der Status während der Bearbeitung dargestellt.
1. `complianceStatus` -> `Compliant`: Der `config-agent` konnte den Cluster erfolgreich konfigurieren und `flux` ohne Fehler bereitstellen.
1. `complianceStatus` -> `Noncompliant`: Der `config-agent` hat einen Fehler beim Bereitstellen von `flux` gefunden, die Details sind im Antworttext `complianceStatus.message` verfügbar.

## <a name="apply-configuration-from-a-private-git-repository"></a>Anwenden der Konfiguration über ein privates Git-Repository

Wenn Sie ein privates Git-Repository verwenden, müssen Sie eine weitere Aufgabe ausführen: Sie müssen den von `flux` generierten Schlüssel als **Bereitstellungsschlüssel** im Repository hinzufügen.

**Abrufen des öffentlichen Schlüssels mithilfe der Azure CLI**

```console
$ az k8sconfiguration show --resource-group <resource group name> --cluster-name <connected cluster name> --name <configuration name> --query 'repositoryPublicKey'
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAREDACTED"
```

**Abrufen des öffentlichen Schlüssels über das Azure-Portal**

1. Navigieren Sie im Azure-Portal zur verbundenen Clusterressource.
2. Klicken Sie auf der Ressourcenseite auf „Konfigurationen“, um die Liste der Konfigurationen für diesen Cluster anzuzeigen.
3. Klicken Sie auf die Konfiguration, die das private Git-Repository verwendet.
4. Kopieren Sie den **öffentlichen Repositoryschlüssel** im geöffneten Kontextfenster unten auf der Seite.

**Hinzufügen des öffentlichen Schlüssels als Bereitstellungsschlüssel zum Git-Repository**

1. Öffnen Sie GitHub, navigieren Sie zunächst zu Ihrem Fork, dann zu **Einstellungen** und anschließend zu **Deploy keys** (Bereitstellungsschlüssel).
2. Klicken Sie auf **Add deploy key** (Bereitstellungsschlüssel hinzufügen).
3. Geben Sie einen Titel an.
4. Aktivieren Sie die Option **Allow write access** (Schreibzugriff gewähren).
5. Fügen Sie den öffentlichen Schlüssel ohne die diesen umgebenden Anführungszeichen ein.
6. Klicken Sie auf **Schlüssel hinzufügen**.

Weitere Informationen zur Verwaltung von Bereitstellungsschlüsseln finden Sie in den GitHub-Dokumentationen.

**Hinzufügen des Schlüssels zu den SSH-Schlüsseln bei Verwendung eines Azure DevOps-Repositorys**

1. Klicken Sie unter **Benutzereinstellungen** oben rechts (neben dem Profilbild) auf **Öffentliche SSH-Schlüssel**.
1. Klicken Sie auf **Neuer Schlüssel**.
1. Geben Sie einen Namen an.
1. Fügen Sie den öffentlichen Schlüssel ohne die diesen umgebenden Anführungszeichen ein.
1. Klicken Sie auf **Hinzufügen**.

## <a name="validate-the-kubernetes-configuration"></a>Überprüfen der Kubernetes-Konfiguration

Nachdem der `config-agent` die `flux`-Instanz installiert hat, sollten die im Git-Repository gespeicherten Ressourcen an den Cluster weitergeleitet werden. Überprüfen Sie, ob die Namcespaces, Bereitstellungen und Ressourcen erstellt wurden:

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

Sie können die anderen Ressourcen untersuchen, die als Teil des Konfigurationsrepositorys bereitgestellt werden:

```console
kubectl -n team-a get cm -o yaml
kubectl -n itops get all
```

## <a name="delete-a-configuration"></a>Löschen einer Konfiguration

Sie können eine `sourceControlConfiguration`-Ressource mithilfe der Azure CLI oder über das Azure-Portal löschen.  Nachdem Sie den Löschbefehl initiiert haben, wird die `sourceControlConfiguration`-Ressource sofort in Azure gelöscht. Es kann jedoch bis zu einer Stunde dauern, bis die zugeordneten Objekte aus dem Cluster gelöscht werden (Dauer kann mit Backlog Item verkürzt werden). Wenn die `sourceControlConfiguration`-Ressource mit dem Namespacebereich erstellt wurde, wird der Namespace nicht aus dem Cluster gelöscht. Auf diese Weise wird verhindert, dass andere Ressourcen beeinträchtigt werden, die in diesem Namespace erstellt wurden.

Beachten Sie, dass alle Änderungen am Cluster, die das Ergebnis von Bereitstellungen aus dem nachverfolgten Git-Repository sind, beim Löschen der `sourceControlConfiguration`-Ressource nicht gelöscht werden.

```console
az k8sconfiguration delete --name '<config name>' -g '<resource group name>' --cluster-name '<cluster name>' --cluster-type connectedClusters
```

**Ausgabe:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von GitOps mit Helm für die Clusterkonfiguration](./use-gitops-with-helm.md)
- [Verwenden von Azure Policy zum Steuern der Clusterkonfiguration](./use-azure-policy.md)
