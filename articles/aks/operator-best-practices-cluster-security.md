---
title: Bewährte Methoden für die Clustersicherheit
titleSuffix: Azure Kubernetes Service
description: Lernen Sie die Best Practices für den Clusteroperator zum Verwalten der Clustersicherheit und von Upgrades in Azure Kubernetes Service (AKS) kennen.
services: container-service
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: ea63db2d8868a1333ae264c9cfdf31d0b7397a83
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107105152"
---
# <a name="best-practices-for-cluster-security-and-upgrades-in-azure-kubernetes-service-aks"></a>Best Practices für Clustersicherheit und Upgrades in Azure Kubernetes Service (AKS)

Beim Verwalten von Clustern in Azure Kubernetes Service (AKS) ist die Sicherheit Ihrer Workloads und Daten ein wichtiger Faktor. Wenn Sie Cluster mit mehreren Mandanten und logischer Isolierung verwenden, muss der Zugriff auf Ressourcen und Workloads besonders geschützt werden. Wenden Sie die neuesten Sicherheitsupdates für Kubernetes und das Knotenbetriebssystem an, um das Angriffsrisiko zu minimieren.

In diesem Artikel wird erläutert, wie AKS-Cluster gesichert werden. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Verwenden von Azure Active Directory und rollenbasierter Kubernetes-Zugriffssteuerung (Role-Based Access Control, RBAC) zum Schutz des API-Serverzugriffs
> * Schützen des Containerzugriffs auf Knotenressourcen
> * Upgraden eines AKS-Clusters auf die neueste Kubernetes-Version
> * Aktualisieren von Knoten und automatisches Anwenden von Sicherheitspatches

Weitere Informationen finden Sie unter [Best Practices für Containerimageverwaltung und Sicherheit in Azure Kubernetes Service (AKS)][best-practices-container-image-management] und [Best Practices für Podsicherheit in Azure Kubernetes Service (AKS)][best-practices-pod-security].

Sie können auch die [Integration von Security Center in Azure Kubernetes Service (Vorschau) ][security-center-aks] verwenden, um Bedrohungen zu erkennen und Empfehlungen zum Sichern Ihrer AKS-Cluster anzuzeigen.

## <a name="secure-access-to-the-api-server-and-cluster-nodes"></a>Sicherer Zugriff auf API-Server und Clusterknoten

> **Best Practices-Leitfaden** 
>
> Eine der wichtigsten Maßnahmen zum Schutz Ihres Clusters ist der Schutz des Zugriffs auf den Kubernetes-API-Server. Integrieren Sie Kubernetes RBAC in Azure Active Directory (Azure AD), um den Zugriff auf den API-Server zu steuern. Dadurch können Sie AKS auf die gleiche Weise schützen wie den Zugriff auf Ihre Azure-Abonnements.

Der Kubernetes API-Server stellt einen einzelnen Verbindungspunkt zur Verfügung, der für Aktionsanforderungen innerhalb eines Clusters zuständig ist. Beschränken Sie den Zugriff, und gewähren Sie so wenige Berechtigungen wie möglich, um den Zugriff auf den API-Server zu schützen und zu überwachen. Diese Vorgehensweise ist nicht Kubernetes-spezifisch, aber besonders wichtig, wenn Ihr AKS-Cluster für die Verwendung mit mehreren Mandanten logisch isoliert ist.

Azure AD bietet eine Lösung zur Identitätsverwaltung für Unternehmen, die in AKS-Cluster integriert werden kann. Da von Kubernetes keine Identitätsverwaltungslösung bereitgestellt wird, ist es möglicherweise schwierig, den Zugriff auf den API-Server präzise einzuschränken. Wenn Sie in AKS Cluster verwenden, die in Azure AD integriert sind, authentifizieren Sie Benutzer im API-Server anhand von bestehenden Benutzer- und Gruppenkonten.

![Azure Active Directory-Integration mit AKS-Clustern](media/operator-best-practices-cluster-security/aad-integration.png)

Mit Kubernetes RBAC und Azure AD-Integration können Sie den API-Server schützen und die erforderlichen Mindestberechtigungen für einen bereichsbezogenen Ressourcensatz (beispielsweise einen einzelnen Namespace) gewähren. Sie können verschiedenen Azure AD-Benutzern oder -Gruppen unterschiedliche Kubernetes-Rollen zuweisen. Mit präzisen Berechtigungen können Sie den Zugriff auf den API-Server beschränken und ein eindeutiges Überwachungsprotokoll mit den durchgeführten Aktionen bereitstellen.

Es empfiehlt sich, Zugriff auf Dateien und Ordner nicht einzelnen Identitäten, sondern *Gruppen* zu gewähren. Verwenden Sie also beispielsweise anstelle einzelner *Benutzer* eine Azure AD-*Gruppen* mitgliedschaft, um Benutzer an Kubernetes-Rollen zu binden. In diesem Fall ändern sich die Zugriffsberechtigungen eines Benutzers im AKS-Cluster automatisch, wenn sich die Gruppenmitgliedschaft des Benutzers ändert. 

Nehmen wir nun an, Sie binden den einzelnen Benutzer direkt an eine Rolle, und sein Tätigkeitsbereich ändert sich. Die Azure AD-Gruppenmitgliedschaften werden zwar aktualisiert, die Berechtigungen für den AKS-Cluster bleiben jedoch unverändert. In diesem Szenario hätte der Benutzer letzten Endes mehr Berechtigungen als er benötigt.

Weitere Informationen zu Azure AD-Integration, Kubernetes RBAC und Azure RBAC finden Sie unter [Zugriffs- und Identitätsoptionen für Azure Kubernetes Service (AKS)][aks-best-practices-identity].

## <a name="secure-container-access-to-resources"></a>Sicherer Containerzugriff auf Ressourcen

> **Best Practices-Leitfaden** 
> 
> Begrenzen Sie den Zugriff auf Aktionen, die von Containern ausgeführt werden können. Gewähren Sie möglichst wenige Berechtigungen, und vermeiden Sie die Verwendung von Root-Zugriff oder Rechteausweitung.

Nicht nur Benutzern und Gruppen sollten lediglich die erforderlichen Mindestberechtigungen gewährt werden, auch Container sollten auf die Aktionen und Prozesse beschränkt werden, die unbedingt erforderlich sind. Konfigurieren Sie zur Minimierung des Angriffsrisikos nach Möglichkeit keine Anwendungen und Container, die ausgeweitete Berechtigungen oder Root-Zugriff benötigen. 

Legen Sie im Podmanifest beispielsweise `allowPrivilegeEscalation: false` fest. Diese in Kubernetes integrierten *Podsicherheitskontexte* ermöglichen es Ihnen, zusätzliche Berechtigungen festzulegen – etwa, unter welchem Benutzer oder welcher Gruppe die Ausführung erfolgen soll oder welche Linux-Funktionen verfügbar gemacht werden sollen. Weitere Best Practices finden Sie unter [Absichern des Podzugriffs auf Ressourcen][pod-security-contexts].

Wenn Sie die Steuerungsmöglichkeiten für Containeraktionen noch feiner abstimmen möchten, können Sie dazu auch integrierte Linux-Sicherheitsfeatures wie *AppArmor* und *seccomp* verwenden. 
1. Definieren Sie Linux-Sicherheitsfeatures auf Knotenebene.
1. Implementieren Sie Features über ein Podmanifest. 

In Linux integrierte Sicherheitsfunktionen sind nur auf Linux-Knoten und -Pods verfügbar.

> [!NOTE]
> Derzeit sind Kubernetes-Umgebungen für die Verwendung feindlicher Workloads mit mehreren Mandanten nicht vollständig sicher. Mithilfe zusätzlicher Sicherheitsfeatures wie *AppArmor*, *seccomp*, *Podsicherheitsrichtlinien* oder Kubernetes RBAC für Knoten können Exploits effizient blockiert werden. 
>
>Echte Sicherheit bei der Ausführung feindlicher Workloads mit mehreren Mandanten erzielen Sie, indem Sie nur einem Hypervisor vertrauen. Die Sicherheitsdomäne für Kubernetes wird zum gesamten Cluster und nicht zu einem einzelnen Knoten. 
>
> Für diese Art von feindlichen Workloads mit mehreren Mandanten sollten Sie physisch isolierte Cluster verwenden.

### <a name="app-armor"></a>AppArmor

Mit dem Kernelsicherheitsmodul [AppArmor][k8s-apparmor] von Linux können Sie Containeraktionen einschränken. AppArmor ist als Teil des zugrunde liegenden AKS Knotens des Betriebssystems verfügbar und standardmäßig aktiviert. Sie erstellen AppArmor-Profile, die Aktionen wie „Lesen“, „Schreiben“ oder „Ausführen“ oder Systemfunktionen wie das Einbinden von Dateisystemen beschränken. Standardprofile von AppArmor beschränken den Zugriff auf verschiedene `/proc`- und `/sys`-Speicherorte und ermöglichen es, Container logisch vom zugrunde liegenden Knoten zu isolieren. AppArmor funktioniert nicht nur mit Kubernetes-Pods, sondern zusammen mit jeder Anwendung, die auf Linux ausgeführt werden kann.

![Verwendete AppArmor-Profile in einem AKS-Cluster, um Containeraktionen zu beschränken](media/operator-best-practices-container-security/apparmor.png)

In der folgenden Beispielverwendung von AppArmor wird ein Profil erstellt, das den Schreibzugriff auf Dateien verhindert. 
1. Stellen Sie eine [SSH][aks-ssh]-Verbindung mit einem AKS-Knoten her.
1. Erstellen Sie eine Datei mit dem Namen *deny-write.profile*.
1. Fügen Sie Folgendes ein:

    ```
    #include <tunables/global>
    profile k8s-apparmor-example-deny-write flags=(attach_disconnected) {
      #include <abstractions/base>
  
      file,
      # Deny all file writes.
      deny /** w,
    }
    ```

AppArmor-Profile werden mithilfe des `apparmor_parser`-Befehls hinzugefügt. 
1. Fügen Sie das Profil zu AppArmor hinzu.
1. Geben Sie den Namen des im vorherigen Schritt erstellten Profils an:

    ```console
    sudo apparmor_parser deny-write.profile
    ```

    Wenn das Profil ordnungsgemäß analysiert und auf AppArmor angewendet wurde, wird keine Ausgabe angezeigt, und Sie befinden sich wieder an der Eingabeaufforderung.

1. Erstellen Sie auf Ihrem lokalen Computer ein Podmanifest namens *aks-apparmor.yaml*. Für dieses Manifest gilt Folgendes:
    * Es definiert eine Anmerkung für `container.apparmor.security.beta.kubernetes`.
    * Es verweist auf das Profil *deny-write*, das in den vorherigen Schritten erstellt wurde.

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: hello-apparmor
      annotations:
        container.apparmor.security.beta.kubernetes.io/hello: localhost/k8s-apparmor-example-deny-write
    spec:
      containers:
      - name: hello
        image: mcr.microsoft.com/aks/fundamental/base-ubuntu:v0.0.11
        command: [ "sh", "-c", "echo 'Hello AppArmor!' && sleep 1h" ]
    ```

1. Überprüfen Sie nach der Bereitstellung, ob der Pod *hello-apparmor* als *Blocked* (Blockiert) angezeigt wird:

    ```
    $ kubectl get pods

    NAME             READY   STATUS    RESTARTS   AGE
    aks-ssh          1/1     Running   0          4m2s
    hello-apparmor   0/1     Blocked   0          50s
    ```

Weitere Informationen zu AppArmor finden Sie im Kubernetes-Artikel [AppArmor][k8s-apparmor].

### <a name="secure-computing"></a>Sicheres Computing

AppArmor ist für jede Linux-Anwendung geeignet. [Seccomp (*Sec* ure *Comp* uting, sicheres Computing)][seccomp] arbeitet dagegen auf Prozessebene. Seccomp ist ebenfalls ein Kernelsicherheitsmodul von Linux und wird nativ von der Docker-Runtime unterstützt, die für AKS-Knoten verwendet wird. Mit seccomp können Sie Containerprozessaufrufe einschränken. Es empfiehlt sich, dem Container nur minimale Berechtigungen für die Ausführung zu erteilen. Gehen Sie dazu wie folgt vor:
* Definieren Sie mithilfe von Filtern, welche Aktionen zugelassen oder verweigert werden sollen.
* Verwenden Sie Anmerkungen innerhalb eines YAML-Podmanifests für die Zuordnung zum entsprechenden seccomp-Filter. 

In der folgenden Beispielverwendung von Seccomp erstellen Sie einen Filter, der verhindert, dass Berechtigungen für eine Datei geändert werden können. 
1. Stellen Sie eine [SSH][aks-ssh]-Verbindung mit einem AKS-Knoten her.
1. Erstellen Sie einen seccomp-Filter mit dem Namen */var/lib/kubelet/seccomp/prevent-chmod*.
1. Fügen Sie Folgendes ein:

    ```json
    {
      "defaultAction": "SCMP_ACT_ALLOW",
      "syscalls": [
        {
          "name": "chmod",
          "action": "SCMP_ACT_ERRNO"
        },
        {
          "name": "fchmodat",
          "action": "SCMP_ACT_ERRNO"
        },
        {
          "name": "chmodat",
          "action": "SCMP_ACT_ERRNO"
        }
      ]
    }
    ```

    Ab Version 1.19 muss Folgendes konfiguriert werden:

    ```json
    {
      "defaultAction": "SCMP_ACT_ALLOW",
      "syscalls": [
        {
          "names": ["chmod","fchmodat","chmodat"],
          "action": "SCMP_ACT_ERRNO"
        }
      ]
    }
    ```

1. Erstellen Sie auf Ihrem lokalen Computer ein Podmanifest namens *aks-seccomp.yaml*, und fügen Sie den folgenden Inhalt ein. Für dieses Manifest gilt Folgendes:
    * Es definiert eine Anmerkung für `seccomp.security.alpha.kubernetes.io`.
    * Es verweist auf den Filter *prevent-chmod*, der im vorherigen Schritt erstellt wurde.

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: chmod-prevented
      annotations:
        seccomp.security.alpha.kubernetes.io/pod: localhost/prevent-chmod
    spec:
      containers:
      - name: chmod
        image: mcr.microsoft.com/aks/fundamental/base-ubuntu:v0.0.11
        command:
          - "chmod"
        args:
         - "777"
         - /etc/hostname
      restartPolicy: Never
    ```

    Ab Version 1.19 muss Folgendes konfiguriert werden:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: chmod-prevented
    spec:
      securityContext:
        seccompProfile:
          type: Localhost
          localhostProfile: prevent-chmod
      containers:
      - name: chmod
        image: mcr.microsoft.com/aks/fundamental/base-ubuntu:v0.0.11
        command:
          - "chmod"
        args:
         - "777"
         - /etc/hostname
      restartPolicy: Never
    ```

1. Stellen Sie den Beispielpod mithilfe des Befehls [kubectl apply][kubectl-apply] bereit:

    ```console
    kubectl apply -f ./aks-seccomp.yaml
    ```

1. Sehen Sie sich mithilfe des Befehls [kubectl get pods][kubectl-get] den Podstatus an. 
    * Der Pod gibt eine Fehlermeldung zurück. 
    * Wie in folgender Beispielausgabe ersichtlich wird, wird das Ausführen des `chmod`-Befehls vom Seccomp-Filter verhindert:    

    ```
    $ kubectl get pods

    NAME                      READY     STATUS    RESTARTS   AGE
    chmod-prevented           0/1       Error     0          7s
    ```

Weitere Informationen zu verfügbaren Filtern finden Sie unter [Seccomp][seccomp].

## <a name="regularly-update-to-the-latest-version-of-kubernetes"></a>Regelmäßiges Update auf die neueste Version von Kubernetes

> **Best Practices-Leitfaden** 
> 
> Upgraden Sie regelmäßig die Kubernetes-Version in Ihrem AKS-Cluster, um alle neuen Features und Fehlerbehebungen zu erhalten.

Kubernetes veröffentlicht neue Features viel schneller als herkömmliche Infrastrukturplattformen. Kubernetes-Updates umfassen Folgendes:
* Neue Funktionen
* Fehler- oder Sicherheitskorrekturen 

Neue Features durchlaufen normalerweise eine *Alpha-* und eine *Beta-Phase*, bevor sie als *stabil* eingestuft werden. Wenn sie stabil sind, werden sie allgemein verfügbar gemacht und für den Einsatz in der Produktion empfohlen. Der Releasezyklus für neue Kubernetes-Features ermöglicht es Ihnen, Kubernetes zu aktualisieren, ohne ständige auf Breaking Changes zu treffen oder Ihre Bereitstellungen und Vorlagen anpassen zu müssen.

AKS unterstützt drei Nebenversionen von Kubernetes. Wenn eine neue Patchnebenversion veröffentlicht wird, laufen die älteste Nebenversion und die unterstützten Patchreleases aus. Kleinere Kubernetes-Updates werden regelmäßig durchgeführt. Sorgen Sie für einen Governance-Prozess mit regelmäßiger Überprüfung auf erforderliche Upgrades, um weiterhin Support zu erhalten. Weitere Informationen finden Sie unter [Unterstützte Kubernetes-Versionen in Azure Kubernetes Service (AKS)][aks-supported-versions].

Wenn Sie überprüfen möchten, welche Versionen für Ihren Cluster verfügbar sind, verwenden Sie den Befehl [az aks get-upgrades][az-aks-get-upgrades] wie im folgenden Beispiel zu sehen:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster
```

Anschließend können Sie Ihren AKS-Cluster mithilfe des Befehls [az aks upgrade][az-aks-upgrade] upgraden. Der Upgradeprozess umfasst die folgenden sicheren Schritte:
* Er sperrt die Knoten sicher nacheinander ab und gleicht sie aus.
* Er plant Pods für die verbleibenden Knoten.
* Er stellt einen neuen Knoten mit der neuesten Betriebssystem- und Kubernetes-Version bereit.

>[!IMPORTANT]
> Testen Sie neue Nebenversionen in einer Entwicklertestumgebung, und vergewissern Sie sich, dass Ihre Workload mit der neuen Kubernetes-Version weiterhin fehlerfrei funktioniert. 
>
> Es kann vorkommen, dass von Ihren Workloads genutzte APIs von Kubernetes eingestellt werden (wie in Version 1.16). Wenn Sie neue Versionen in der Produktion einsetzen, sollten Sie in Erwägung ziehen, [mehrere Knotenpools für separate Versionen](use-multiple-node-pools.md) zu verwenden und einzelne Pools nacheinander zu aktualisieren, um das Rollout des Updates progressiv auf einem Cluster auszuführen. Wenn Sie mehrere Cluster ausführen, aktualisieren Sie jeweils einen Cluster, um Auswirkung oder Änderungen progressiv zu überwachen.
>
>```azurecli-interactive
>az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version KUBERNETES_VERSION
>```

Weitere Informationen zu Upgrades in AKS finden Sie unter [Unterstützte Kubernetes-Versionen in Azure Kubernetes Service (AKS)][aks-supported-versions] und [Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)][aks-upgrade].

## <a name="process-linux-node-updates-and-reboots-using-kured"></a>Verarbeiten von Updates und Neustarts von Linux-Knoten mithilfe von kured

> **Best Practices-Leitfaden** 
> 
> AKS lädt zwar automatisch Behebungen von Sicherheitsproblemen für jeden einzelnen Linux-Knoten herunter und installiert sie, führt aber keinen automatischen Neustart durch. 
> 1. Verwenden Sie `kured`, um auf ausstehende Neustarts zu achten.
> 1. Sperren Sie den Knoten sicher ab, und gleichen Sie ihn aus, damit er neu gestartet werden kann.
> 1. Wenden Sie die Updates an.
> 1. Gehen Sie so umsichtig wie möglich mit Ihrem Betriebssystem um. 

Führen Sie für Windows Server-Knoten regelmäßig ein AKS-Upgrade durch, um die Pods sicher abzusperren und zu leeren und aktualisierte Knoten bereitzustellen.

Jeden Abend werden über die Updateverteilungskanäle Sicherheitspatches für Linux-Knoten in AKS zur Verfügung gestellt. Dieses Verhalten wird im Rahmen der Bereitstellung von Knoten in einem AKS-Cluster automatisch konfiguriert. Neustarts werden für Knoten nicht automatisch ausgeführt, wenn ein Sicherheitspatch oder Kernelupdate es erfordern würde, um Störungen und eventuelle negative Einflüsse auf ausgeführte Workloads zu minimieren.

Das Open-Source-Projekt [kured (KUbernetes REboot Daemon)][kured] von Weaveworks überwacht ausstehende Neustarts für Knoten. Wenn ein Linux-Knoten Updates implementiert, die einen Neustart erfordern, wird der Knoten sicher abgesperrt und geleert, um Pods in andere Knoten des Clusters zu verschieben und einen Zeitplan für sie festzulegen. Nach dem Neustart des Knotens wird er wieder dem Cluster hinzugefügt, und die Podplanung von Kubernetes wird fortgesetzt. Immer nur ein Knoten auf einmal kann durch `kured` neu gestartet werden, um Störungen so gering wie möglich zu halten.

![Der Neustartvorgang eines AKS-Knotens mithilfe von kured.](media/operator-best-practices-cluster-security/node-reboot-process.png)

Falls Sie noch mehr Kontrolle über Neustarts benötigen, kann `kured` in Prometheus integriert werden, um Neustarts zu verhindern, wenn gleichzeitig andere Wartungsereignisse stattfinden oder Clusterprobleme vorliegen. Diese Integration minimiert durch Neustarts von Knoten hervorgerufene Komplikationen, wenn Sie gerade andere Probleme behandeln.

Weitere Informationen zum Umgang mit Knotenneustarts finden Sie im Artikel [Anwenden von Sicherheits- und Kernelupdates auf Linux-Knoten in Azure Kubernetes Service (AKS)][aks-kured].

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde erläutert, wie AKS-Cluster gesichert werden. Wenn Sie einige dieser Best Practices implementieren möchten, lesen Sie folgende Artikel:

* [Integrieren von Azure Active Directory in AKS][aks-aad]
* [Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)][aks-upgrade]
* [Anwenden von Sicherheits- und Kernelupdates auf Knoten in Azure Kubernetes Service (AKS)][aks-kured]

<!-- EXTERNAL LINKS -->
[kured]: https://github.com/weaveworks/kured
[k8s-apparmor]: https://kubernetes.io/docs/tutorials/clusters/apparmor/
[seccomp]: https://kubernetes.io/docs/concepts/policy/pod-security-policy/#seccomp
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-exec]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[aks-supported-versions]: supported-kubernetes-versions.md
[aks-upgrade]: upgrade-cluster.md
[aks-best-practices-identity]: concepts-identity.md
[aks-kured]: node-updates-kured.md
[aks-aad]: ./azure-ad-integration-cli.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[pod-security-contexts]: developer-best-practices-pod-security.md#secure-pod-access-to-resources
[aks-ssh]: ssh.md
[security-center-aks]: ../security-center/defender-for-kubernetes-introduction.md
