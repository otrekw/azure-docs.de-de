---
title: Konzepte – Grundlagen zu Kubernetes für Azure Kubernetes Service (AKS)
description: Lernen Sie die grundlegenden Cluster- und Workloadkomponenten von Kubernetes kennen, und erfahren Sie, wie diese mit den Features in Azure Kubernetes Service (AKS) in Zusammenhang stehen.
services: container-service
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: 5e505ed44d221b20178ea5ffb1d9125fb2bddd4c
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107105934"
---
# <a name="kubernetes-core-concepts-for-azure-kubernetes-service-aks"></a>Grundlegende Kubernetes-Konzepte für Azure Kubernetes Service (AKS)

Die Anwendungsentwicklung setzt zunehmend auf eine containerbasierte Vorgehensweise, sodass es immer wichtiger wird, die Ressourcen zu orchestrieren und zu verwalten. Als führende Plattform bietet Kubernetes eine zuverlässige Planung fehlertoleranter Anwendungsworkloads. Azure Kubernetes Service (AKS), ein Managed Kubernetes-Angebot, vereinfacht die containerbasierte Anwendungsbereitstellung und -verwaltung noch weiter.

Dieser Artikel bietet eine Einführung zu Folgendem:
* Grundlegende Komponenten der Kubernetes-Infrastruktur:
    * *Steuerungsebene*
    * *Knoten*
    * *Knotenpools*
* Workloadressourcen: 
    * *Pods*
    * *Bereitstellungen*
    * *Sets* 
* Gruppieren von Ressourcen in *Namespaces*

## <a name="what-is-kubernetes"></a>Was ist Kubernetes?

Kubernetes ist eine schnell wachsende Plattform, die containerbasierte Anwendungen und die zugehörigen Netzwerk- und Speicherkomponenten verwaltet. Kubernetes konzentriert sich die Anwendungsworkloads, nicht auf die zugrunde liegenden Infrastrukturkomponenten. Kubernetes bietet einen deklarativen Ansatz für Bereitstellungen und wird durch einen stabilen Satz an APIs für Verwaltungsvorgänge unterstützt.

Sie können moderne, portierbare, auf Microservices basierende Anwendungen erstellen und ausführen, wobei Kubernetes zum Orchestrieren und Verwalten der Verfügbarkeit der Anwendungskomponenten verwendet wird. Kubernetes unterstützt sowohl zustandslose als auch zustandsbehaftete Anwendungen für Teams, die zunehmend auf Microservices basierende Anwendungen entwickeln.

Als offene Plattform ermöglicht Kubernetes Ihnen, Ihre Anwendungen mit Ihren bevorzugten Programmiersprachen, Betriebssystemen, Bibliotheken oder Messagingbussen zu erstellen. Vorhandene CI/CD-Tools (Continuous Integration/Continuous Delivery) können in Kubernetes integriert werden, um Releases zu planen und bereitzustellen.

AKS bietet einen Managed Kubernetes-Dienst, der die Komplexität von Bereitstellungs- und wichtigen Verwaltungsaufgaben reduziert, z. B. die Koordination von Upgrades. Die Azure-Plattform verwaltet die AKS-Steuerungsebene, und Sie zahlen nur für die AKS-Knoten, die Ihre Anwendungen ausführen. AKS baut auf der Open-Source-Engine von Azure Kubernetes Service ([aks-engine][aks-engine]) auf.

## <a name="kubernetes-cluster-architecture"></a>Kubernetes-Cluster – Architektur

Ein Kubernetes-Cluster ist in zwei Komponenten unterteilt:

- *Steuerungsebene*: Diese stellt die grundlegenden Kubernetes-Dienste und Orchestrierungsfunktionen für Anwendungsworkloads bereit.
- *Knoten*: Diese führen Ihre Anwendungsworkloads aus.

![Kubernetes-Steuerungsebene und Knoten – Komponenten](media/concepts-clusters-workloads/control-plane-and-nodes.png)

## <a name="control-plane"></a>Steuerungsebene

Wenn Sie einen AKS-Cluster erstellen, wird automatisch eine Steuerungsebene erstellt und konfiguriert. Diese Steuerungsebene wird kostenlos als verwaltete Azure-Ressource bereitgestellt, die für den Benutzer abstrahiert wird. Sie zahlen nur für die Knoten, die an den AKS-Cluster angefügt sind. Die Steuerungsebene und ihre Ressourcen befinden sich nur in der Region, in der Sie den Cluster erstellt haben.

Die Steuerungsebene umfasst die folgenden Kubernetes-Kernkomponenten:

| Komponente | BESCHREIBUNG |  
| ----------------- | ------------- |  
| *kube-apiserver*                                                                                 | Über den API-Server werden die zugrunde liegenden Kubernetes-APIs verfügbar gemacht. Diese Komponente ermöglicht die Interaktion für Verwaltungstools, z.B. `kubectl` oder das Kubernetes-Dashboard.                                                        |  
| *etcd* | Für die Verwaltung des Zustands Ihres Kubernetes-Clusters und Ihrer Kubernetes-Konfiguration ist *etcd* ein hoch verfügbarer Schlüssel-Wert-Speicher in Kubernetes.                                      |  
| *kube-scheduler*                                                                            | Wenn Sie Anwendungen erstellen oder skalieren, ermittelt der Scheduler, welche Knoten die Workload ausführen können, und startet diese.                                                                                    |  
| *kube-controller-manager*                                                                            | Der Controller Manager überwacht eine Reihe kleinerer Controller, die Aktionen wie beispielsweise das Replizieren von Pods und das Verarbeiten von Knotenvorgängen ausführen.                                                                  |  

AKS stellt eine Steuerungsebene mit Einzelmandant, einem dedizierten API-Server, einem Scheduler usw. bereit. Sie definieren die Anzahl und Größe der Knoten, und die Azure-Plattform konfiguriert die sichere Kommunikation zwischen Steuerungsebene und Knoten. Die Interaktion mit der Steuerungsebene erfolgt über Kubernetes-APIs, z.B. `kubectl`, oder das Kubernetes-Dashboard.

Bei dieser verwalteten Steuerungsebene müssen Sie zwar keine Komponenten konfigurieren (z. B. hoch verfügbaren *etcd*-Speicher), doch Sie können nicht direkt auf die Steuerungsebene zugreifen. Upgrades der Kubernetes-Steuerungsebene und -Knoten werden über die Azure CLI oder das Azure-Portal orchestriert. Zum Beheben möglicher Probleme können Sie über Azure Monitor-Protokolle die Steuerungsebenenprotokolle überprüfen.

Um eine Steuerungsebene zu konfigurieren oder direkt darauf zuzugreifen, stellen Sie Ihren eigenen Kubernetes-Cluster mithilfe von [aks-engine][aks-engine] bereit.

Entsprechende bewährte Methoden finden Sie unter [Best Practices für Clustersicherheit und Upgrades in Azure Kubernetes Service (AKS)][operator-best-practices-cluster-security].

## <a name="nodes-and-node-pools"></a>Knoten und Knotenpools

Zum Ausführen Ihrer Anwendungen und der unterstützenden Dienste benötigen Sie einen Kubernetes-*Knoten*. Ein AKS-Cluster besteht aus mindestens einem Knoten, bei dem es sich um einen virtuellen Azure-Computer (Azure-VM) handelt, der die Kubernetes-Knotenkomponenten und die Containerruntime ausführt.

| Komponente | BESCHREIBUNG |  
| ----------------- | ------------- |  
| `kubelet`                                                                                 | Der Kubernetes-Agent, der die Orchestrierungsanforderungen der Steuerungsebene und die Planung der Ausführung der angeforderten Container verarbeitet.                                                        |  
| *kube-proxy* | Verarbeitet virtuelle Netzwerkfunktionen auf jedem Knoten. Der Proxy leitet den Netzwerkdatenverkehr weiter und verwaltet die IP-Adressen für Dienste und Pods.                                      |  
| *Containerruntime*                                                                            | Ermöglicht containerisierten Anwendungen die Ausführung von und Interaktion mit zusätzlichen Ressourcen, z. B. dem virtuellen Netzwerk und Speicher. AKS-Cluster, die Knotenpools mit Kubernetes-Version 1.19 und höher verwenden, nutzen `containerd` als Containerruntime. AKS-Cluster, die Knotenpools mit älteren Kubernetes-Versionen als 1.19 verwenden, nutzen [Moby](https://mobyproject.org/) (Upstream-Docker) als Containerruntime.                                                                                    |  


![Virtuelle Azure-Computer und unterstützende Ressourcen für einen Kubernetes-Knoten](media/concepts-clusters-workloads/aks-node-resource-interactions.png)

Die Größe der Azure-VMs für Ihre Knoten definiert die CPUs, den Arbeitsspeicher, die Größe und den Typ des verfügbaren Speichers (z. B. hochleistungsfähige SSDs oder reguläre HDDs). Planen Sie die Knotengröße danach, ob Ihre Anwendungen möglicherweise große Mengen an CPUs und Arbeitsspeicher oder Hochleistungsspeicher erfordern. Skalieren Sie die Anzahl von Knoten in Ihrem AKS-Cluster auf, um Anforderungen zu erfüllen.

In AKS basiert das VM-Image für die Knoten in Ihrem Cluster auf Ubuntu Linux oder Windows Server 2019. Wenn Sie einen AKS-Cluster erstellen oder die Anzahl von Knoten aufskalieren, erstellt und konfiguriert die Azure-Plattform automatisch die angeforderte Anzahl von VMs. Agent-Knoten werden als Standard-VMs in Rechnung gestellt, sodass alle etwaigen Rabatte für VM-Größen (einschließlich [Azure-Reservierungen][reservation-discounts]) automatisch angewendet werden.

Stellen Sie Ihren eigenen Kubernetes-Cluster mit [aks-engine][aks-engine] bereit, wenn Sie ein anderes Hostbetriebssystem oder eine andere Containerruntime verwenden oder andere benutzerdefinierte Pakete einschließen. Die `aks-engine`-Upstreamreleases bieten Funktionen und Konfigurationsoptionen vor der Unterstützung in AKS-Clustern. Wenn Sie also eine andere Containerruntime als `containerd` oder [Moby](https://mobyproject.org/) verwenden möchten, können Sie `aks-engine` ausführen, um einen Kubernetes-Cluster zu konfigurieren und bereitzustellen, der Ihre aktuellen Anforderungen erfüllt.

### <a name="resource-reservations"></a>Ressourcenreservierungen

AKS verwendet Knotenressourcen, damit der Knoten als Teil Ihres Clusters fungieren kann. Diese Verwendung kann zu einer Abweichung zwischen den Gesamtressourcen des Knotens und den Ressourcen führen, die in AKS zugewiesen werden können. Beachten Sie diese Informationen beim Festlegen von Anforderungen und Grenzwerten für vom Benutzer bereitgestellte Pods.

Führen Sie Folgendes aus, um die Ressourcen zu ermitteln, die einem Knoten zugewiesen werden können:
```kubectl
kubectl describe node [NODE_NAME]
```

Um die Leistung und Funktionalität des Knotens zu gewährleisten, reserviert AKS Ressourcen auf jedem Knoten. Wenn ein Knoten mehr Ressourcen nutzt, steigt die Anzahl der reservierten Ressourcen aufgrund eines höheren Verwaltungsbedarfs für vom Benutzer bereitgestellte Pods.

>[!NOTE]
> Durch die Verwendung von AKS-Add-Ons wie Container Insights (OMS) werden zusätzliche Knotenressourcen beansprucht.

Zwei Arten von Ressourcen sind reserviert:

- **CPU**  
    Die reservierten CPU-Ressourcen hängen vom Knotentyp und der Clusterkonfiguration ab. Diese können dazu führen, dass weniger CPU-Ressourcen zugewiesen werden können, da zusätzliche Features ausgeführt werden.

   | CPU-Kerne auf dem Host | 1    | 2    | 4    | 8    | 16 | 32|64|
   |---|---|---|---|---|---|---|---|
   |Kube-reserviert (Millicore)|60|100|140|180|260|420|740|

- **Memory**  
    Der von AKS genutzte Arbeitsspeicher umfasst die Summe aus zwei Werten.

   1. **`kubelet`-Daemon**   
       Der `kubelet`-Daemon wird auf allen Kubernetes-Agent-Knoten installiert, um die Erstellung und Beendigung von Containern zu verwalten. 
   
        In AKS gilt für den `kubelet`-Daemon standardmäßig die Entfernungsregel *memory.available<750Mi*. Dadurch wird sichergestellt, dass der zuweisbare Arbeitsspeicher auf einem Knoten immer mindestens 750 Mi betragen muss. Wenn ein Host diesen Schwellenwert des verfügbaren Arbeitsspeichers unterschreitet, wird `kubelet` ausgelöst, um einen der ausgeführten Pods zu beenden und Arbeitsspeicher auf dem Hostcomputer freizugeben.

   2. **Eine regressive Rate von Arbeitsspeicherreservierungen** für den Kubelet-Daemon, damit er ordnungsgemäß funktioniert (*kube-reserved*)
      - 25 % der ersten 4 GB Arbeitsspeicher
      - 20 % der nächsten 4 GB Arbeitsspeicher (bis 8 GB)
      - 10 % der nächsten 8 GB Arbeitsspeicher (bis 16 GB)
      - 6 % der nächsten 112 GB Arbeitsspeicher (bis 128 GB)
      - 2 % des Arbeitsspeichers oberhalb von 128 GB

Regeln für die Arbeitsspeicher- und CPU-Zuweisung:
* Gewährleisten Sie die Integrität der Agent-Knoten, von denen einige Systempods hosten, die für die Clusterintegrität wichtig sind. 
* Sorgen Sie dafür, dass der Knoten weniger zuteilbaren Arbeitsspeicher und CPU meldet, als wenn er nicht Teil eines Kubernetes-Clusters wäre. 

Die obigen Ressourcenreservierungen können nicht geändert werden.

Wenn ein Knoten beispielsweise 7 GB bietet, werden 34 % des Arbeitsspeichers einschließlich des festen Entfernungsschwellenwerts von 750 Mi als nicht zuteilbar angegeben.

`0.75 + (0.25*4) + (0.20*3) = 0.75GB + 1GB + 0.6GB = 2.35GB / 7GB = 33.57% reserved`

Zusätzlich zu den Reservierungen für Kubernetes selbst reserviert das zugrunde liegende Knotenbetriebssystem ebenfalls CPU- und Arbeitsspeicherressourcen für die Aufrechterhaltung der Betriebssystemfunktionen.

Entsprechende bewährte Methoden finden Sie unter [Best Practices für grundlegende Schedulerfunktionen in Azure Kubernetes Service (AKS)][operator-best-practices-scheduler].

### <a name="node-pools"></a>Knotenpools

Knoten mit der gleichen Konfiguration werden in *Knotenpools* gruppiert. Ein Kubernetes-Cluster enthält mindestens einen Knotenpool. Die anfängliche Knotenanzahl und -größe wird beim Erstellen eines AKS-Clusters definiert, wobei ein *Standardknotenpool* erstellt wird. Dieser Standardknotenpool in AKS enthält die zugrunde liegenden VMs, die Ihre Agent-Knoten ausführen.

> [!NOTE]
> Um zu gewährleisten, dass Ihr Cluster zuverlässig funktioniert, sollten Sie mindestens zwei (2) Knoten im Standardknotenpool ausführen.

Sie führen eine Skalierung oder ein Upgrade eines AKS-Clusters im Standardknotenpool durch. Sie können eine Skalierung oder ein Upgrade eines bestimmten Knotenpools auswählen. Bei Upgradevorgängen wird die Ausführung von Containern in anderen Knoten im Knotenpool geplant, bis das Upgrade für alle Knoten erfolgreich durchgeführt wurde.

Weitere Informationen zur Verwendung mehrerer Knotenpools in AKS finden Sie unter [Erstellen und Verwalten mehrerer Knotenpools für einen Cluster in AKS][use-multiple-node-pools].

### <a name="node-selectors"></a>Knotenselektoren

In einem AKS-Cluster mit mehreren Knotenpools müssen Sie dem Kubernetes-Scheduler möglicherweise mitteilen, welcher Knotenpool für eine bestimmte Ressource verwendet werden soll. Beispielsweise sollten Eingangscontroller nicht auf Windows Server-Knoten ausgeführt werden. 

Mit Knotenselektoren können Sie verschiedene Parameter festlegen, z. B. das Betriebssystem des Knotens, um zu steuern, wo ein Pod geplant werden soll.

Das folgende einfache Beispiel plant eine NGINX-Instanz auf einem Linux-Knoten unter Verwendung des Knotenselektors *"beta.kubernetes.io/os": linux*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx
spec:
  containers:
    - name: myfrontend
      image: mcr.microsoft.com/oss/nginx/nginx:1.15.12-alpine
  nodeSelector:
    "beta.kubernetes.io/os": linux
```

Weitere Informationen zum Steuern, wo Pods geplant werden, finden Sie unter [Best Practices für erweiterte Schedulerfunktionen in Azure Kubernetes Service (AKS)][operator-best-practices-advanced-scheduler].

## <a name="pods"></a>Pods

Kubernetes verwendet *Pods*, um eine Instanz Ihrer Anwendung auszuführen. Ein Pod repräsentiert eine einzelne Instanz der Anwendung. 

Pods weisen in der Regel eine 1:1-Zuordnung mit einem Container auf. In erweiterten Szenarien kann ein Pod mehrere Container enthalten. Pods mit mehreren Containern werden zusammen auf dem gleichen Knoten geplant und ermöglichen es Containern, zugehörige Ressourcen gemeinsam zu nutzen.

Wenn Sie einen Pod erstellen, können Sie *Ressourcenanforderungen* definieren, um eine bestimmte Menge an CPU- oder Arbeitsspeicherressourcen anzufordern. Der Kubernetes-Scheduler versucht, die Anforderung zu erfüllen, indem er die Ausführung der Pods auf einem Knoten mit verfügbaren Ressourcen plant. Sie können auch maximale Ressourcenlimits angeben, um zu verhindern, dass ein Pod zu viele Computeressourcen des zugrunde liegenden Knotens verbraucht. Eine bewährte Methode ist es, Ressourcenlimits für alle Pods einzurichten, um den Kubernetes-Scheduler beim Identifizieren benötigter, zulässiger Ressourcen zu unterstützen.

Weitere Informationen finden Sie unter [Kubernetes Pods][kubernetes-pods] (Kubernetes-Pods) und [Kubernetes Pod Lifecycle][kubernetes-pod-lifecycle] (Lebenszyklus von Kubernetes-Pods).

Ein Pod ist eine logische Ressource, aber die Anwendungsworkloads werden in den Containern ausgeführt. Pods sind in der Regel kurzlebige Ressourcen, die gelöscht werden können. Einzeln geplante Pods bieten nicht alle Hochverfügbarkeits- und Redundanzfeatures von Kubernetes. Stattdessen werden Pods von Kubernetes-*Controllern* bereitgestellt und verwaltet, beispielsweise dem Bereitstellungscontroller.

## <a name="deployments-and-yaml-manifests"></a>Bereitstellungen und YAML-Manifeste

Eine *Bereitstellung* repräsentiert identische Pods, die vom Kubernetes-Bereitstellungscontroller verwaltet werden. Eine Bereitstellung definiert die Anzahl von Pod-*Replikaten*, die erstellt werden sollen. Der Kubernetes-Scheduler stellt sicher, dass zusätzliche Pods auf fehlerfreien Knoten geplant werden, wenn auf Pods oder Knoten Probleme auftreten.

Sie können Bereitstellungen aktualisieren, um die Konfiguration von Pods, das verwendete Containerimage oder den angeschlossenen Speicher zu ändern. Der Bereitstellungscontroller:
* Leert und beendet eine bestimmte Anzahl von Replikaten.
* Erstellt Replikate gemäß der neuen Bereitstellungsdefinition.
* Setzt den Prozess so lange fort, bis alle Replikate in der Bereitstellung aktualisiert wurden.

Die meisten zustandslosen Anwendungen in AKS sollten das Bereitstellungsmodell verwenden, anstatt einzelne Pods zu planen. Kubernetes kann die Integrität und den Status von Bereitstellungen überwachen, um sicherzustellen, dass die erforderliche Anzahl von Replikaten im Cluster ausgeführt wird. Beim Planen einzelner Pods werden diese nach dem Auftreten eines Problems nicht neu gestartet. Außerdem werden die Pods nicht auf fehlerfreien Knoten erneut geplant, wenn auf dem aktuellen Knoten ein Problem auftritt.

Sie möchten nicht, dass Verwaltungsentscheidungen durch einen Updateprozess unterbrochen werden, wenn Ihre Anwendung eine Mindestanzahl verfügbarer Instanzen erfordert. *Budgets für die Unterbrechung von Pods* definieren, wie viele Replikate in einer Bereitstellung während einer Aktualisierung oder eines Knotenupgrades heruntergefahren werden können. Ein Beispiel: Wenn Sie in Ihrer Bereitstellung über *fünf (5)* Replikate verfügen, können Sie eine Podunterbrechung für *vier (4)* Replikate definieren, damit jeweils nur ein Replikat gelöscht oder neu geplant werden darf. Ebenso wie bei Podressourcenlimits besteht eine bewährte Methode darin, Budgets für die Unterbrechung von Pods für Anwendungen zu definieren, für die immer eine bestimmte Mindestanzahl von Replikaten vorhanden sein muss.

Bereitstellungen werden in der Regel mit `kubectl create` oder `kubectl apply` erstellt und verwaltet. Erstellen Sie eine Bereitstellung, indem Sie eine Manifestdatei im YAML-Format definieren. 

Das folgende Beispiel erstellt eine grundlegende Bereitstellung eines NGINX-Webservers. Die Bereitstellung gibt an, dass *3* Replikate erstellt werden sollen und dass Port *80* auf dem Container geöffnet sein muss. Es werden auch Ressourcenanforderungen und -limits für CPU und Arbeitsspeicher definiert.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: mcr.microsoft.com/oss/nginx/nginx:1.15.2-alpine
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 250m
            memory: 64Mi
          limits:
            cpu: 500m
            memory: 256Mi
```

Indem Sie Dienste (z. B. Lastenausgleichsmodule) im YAML-Manifest angeben, können Sie auch komplexere Anwendungen erstellen.

Weitere Informationen finden Sie unter [Kubernetes-Bereitstellungen][kubernetes-deployments].

### <a name="package-management-with-helm"></a>Paketverwaltung mit Helm

[Helm][helm] wird häufig zum Verwalten von Anwendungen in Kubernetes verwendet. Sie können Ressourcen bereitstellen, indem Sie vorhandene öffentliche Helm-*Charts* erstellen und verwenden, die eine gepackte Version des Anwendungscodes und der Kubernetes-YAML-Manifeste enthalten. Sie können Helm-Charts entweder lokal oder in einem Remoterepository speichern, z. B. einem [Helm-Chart-Repository in Azure Container Registry][acr-helm].

Um Helm zu verwenden, installieren Sie den Helm-Client auf Ihrem Computer, oder verwenden Sie den Helm-Client in [Azure Cloud Shell][azure-cloud-shell]. Suchen oder erstellen Sie Helm-Charts, und installieren Sie diese dann in Ihrem Kubernetes-Cluster. Weitere Informationen finden Sie unter [Installieren vorhandener Anwendungen mit Helm in AKS][aks-helm].

## <a name="statefulsets-and-daemonsets"></a>StatefulSets und DaemonSets

Mithilfe des Kubernetes-Schedulers führt der Bereitstellungscontroller Replikate auf einem beliebigen verfügbaren Knoten mit verfügbaren Ressourcen aus. Dieser Ansatz mag zwar für zustandslose Anwendungen genügen, doch ist der Bereitstellungscontroller für Anwendungen, die Folgendes erfordern, nicht optimal geeignet:
* Eine permanente Namenskonvention oder einen permanenten Speicher 
* Ein vorhandenes Replikat auf jedem ausgewählten Knoten in einem Cluster

Mit zwei Kubernetes-Ressourcen können Sie jedoch diese Arten von Anwendungen verwalten:

- *StatefulSets* verwalten den Zustand von Anwendungen über den Lebenszyklus eines einzelnen Pods hinaus, z. B. Speicher.
- *DaemonSets* stellen zu einem frühen Zeitpunkt im Kubernetes-Bootstrapprozess eine ausgeführte Ressource auf jedem Knoten sicher.

### <a name="statefulsets"></a>StatefulSets

Die moderne Anwendungsentwicklung strebt häufig nach zustandslosen Anwendungen. Für zustandsbehafte Anwendungen, z. B. solche, die Datenbankkomponenten enthalten, können Sie *StatefulSets* verwenden. Wie bei Bereitstellungen erstellt und verwaltet ein StatefulSet mindestens einen identischen Pod. Replikate in einem StatefulSet folgen einem ordnungsgemäßen, sequenziellen Verfahren für Bereitstellung, Skalierung, Upgrade und Beendigung. Namenskonvention, Netzwerknamen und Speicher bleiben permanent erhalten, wenn Replikate mit einem StatefulSet neu geplant werden.

Definieren Sie die Anwendung im YAML-Format mit `kind: StatefulSet`. Ab diesem Punkt übernimmt der StatefulSet-Controller die Bereitstellung und Verwaltung der erforderlichen Replikate. Daten werden in permanenten Speicher geschrieben, der von Azure Managed Disks oder Azure Files bereitgestellt wird. Bei StatefulSets bleibt der zugrunde liegende permanente Speicher erhalten, auch wenn das StatefulSet gelöscht wird.

Weitere Informationen finden Sie unter [Kubernetes StatefulSets][kubernetes-statefulsets].

Replikate in einem StatefulSet werden auf einem beliebigen verfügbaren Knoten in einem AKS-Cluster geplant und ausgeführt. Um sicherzustellen, dass mindestens ein Pod in Ihrem Set auf einem Knoten ausgeführt wird, verwenden Sie stattdessen ein DaemonSet.

### <a name="daemonsets"></a>DaemonSets

Bei einer spezifischen Protokollsammlung oder Überwachung müssen Sie möglicherweise einen Pod auf allen bzw. auf ausgewählten Knoten ausführen. Sie können *DaemonSet* verwenden, um einen oder mehrere identische Pods bereitzustellen, doch gewährleistet der DaemonSet-Controller, dass jeder angegebene Knoten eine Instanz des Pods ausführt.

Der DaemonSet-Controller kann Pods auf Knoten zu einem frühen Zeitpunkt im Clusterstartprozess planen, bevor der Kubernetes Scheduler gestartet wird. Dies stellt sicher, dass die Pods in einem DaemonSet gestartet wurden, bevor herkömmliche Pods in einer Bereitstellung oder einem StatefulSet geplant werden.

Ähnlich wie StatefulSets wird auch ein DaemonSet mit `kind: DaemonSet` als Teil einer YAML-Definition definiert.

Weitere Informationen finden Sie unter [Kubernetes DaemonSets][kubernetes-daemonset].

> [!NOTE]
> Wenn Sie das [Add-On für virtuelle Knoten](virtual-nodes-cli.md#enable-virtual-nodes-addon) verwenden, erstellen DaemonSets keine Pods auf dem virtuellen Knoten.

## <a name="namespaces"></a>Namespaces

Kubernetes-Ressourcen wie Pods und Bereitstellungen werden logisch in einem *Namespace* gruppiert, um einen AKS-Cluster zu unterteilen und den Zugriff zum Erstellen, Anzeigen oder Verwalten von Ressourcen einzuschränken. Sie können z. B. Namespaces erstellen, um Unternehmensgruppen voneinander zu trennen. Benutzer können nur mit den Ressourcen innerhalb der ihnen zugewiesenen Namespaces interagieren.

![Kubernetes-Namespaces zur logischen Unterteilung von Ressourcen und Anwendungen](media/concepts-clusters-workloads/namespaces.png)

Wenn Sie einen AKS-Cluster erstellen, stehen Ihnen folgende Namespaces zur Verfügung:

| Namespace | BESCHREIBUNG |  
| ----------------- | ------------- |  
| *default*                                                                                 | Hier werden Pods und Bereitstellungen standardmäßig erstellt, wenn kein anderer Namespace angegeben wird. In kleineren Umgebungen können Sie Anwendungen direkt im Standardnamespace bereitstellen, ohne weitere logische Unterteilungen zu erstellen. Wenn Sie mit der Kubernetes-API interagieren, z.B. mit `kubectl get pods`, wird der Standardnamespace verwendet, wenn kein anderer Namespace angegeben wurde.                                                        |  
| *kube-system* | Hier befinden sich grundlegende Ressourcen, beispielsweise Netzwerkfeatures wie DNS und Proxy oder das Kubernetes-Dashboard. In diesem Namespace stellen Sie in der Regel keine eigenen Anwendungen bereit.                                      |  
| *kube-public*                                                                            | Dieser Namespace wird in der Regel nicht genutzt. Er kann jedoch für Ressourcen verwendet werden, die über den gesamten Cluster hinweg sichtbar sein sollen, und er kann von allen Benutzern angezeigt werden.                                                                                    |  


Weitere Informationen finden Sie unter [Kubernetes-Namespaces][kubernetes-namespaces].

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden einige der wichtigsten Kubernetes-Komponenten sowie deren Anwendung in AKS-Clustern beschrieben. Weitere Informationen zu den wesentlichen Konzepten von Kubernetes und AKS finden Sie in den folgenden Artikeln:

- [Kubernetes/AKS: Zugriff und Identität][aks-concepts-identity]
- [Kubernetes/AKS: Sicherheit][aks-concepts-security]
- [Kubernetes/AKS: virtuelle Netzwerke][aks-concepts-network]
- [Kubernetes/AKS: Speicher][aks-concepts-storage]
- [Kubernetes/AKS: Skalierung][aks-concepts-scale]

<!-- EXTERNAL LINKS -->
[aks-engine]: https://github.com/Azure/aks-engine
[kubernetes-pods]: https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/
[kubernetes-pod-lifecycle]: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/
[kubernetes-deployments]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[kubernetes-statefulsets]: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
[kubernetes-namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[helm]: https://helm.sh/
[azure-cloud-shell]: https://shell.azure.com

<!-- INTERNAL LINKS -->
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[acr-helm]: ../container-registry/container-registry-helm-repos.md
[aks-helm]: kubernetes-helm.md
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[operator-best-practices-scheduler]: operator-best-practices-scheduler.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[reservation-discounts]:../cost-management-billing/reservations/save-compute-costs-reservations.md
