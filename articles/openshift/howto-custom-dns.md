---
title: Konfigurieren benutzerdefinierter DNS-Ressourcen in einem ARO-Cluster (Azure Red Hat OpenShift)
description: Hier erfahren Sie, wie Sie einen benutzerdefinierten DNS-Server für alle Knoten in Azure Red Hat OpenShift (ARO) hinzufügen.
author: bryanro92
ms.author: suvetriv
ms.service: azure-redhat-openshift
ms.topic: article
ms.date: 06/02/2021
ms.openlocfilehash: 842e0a4d57254c4ec27bdadf2ff168fe2f4c0424
ms.sourcegitcommit: 070122ad3aba7c602bf004fbcf1c70419b48f29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111442079"
---
# <a name="configure-custom-dns-for-your-azure-red-hat-openshift-aro-cluster"></a>Konfigurieren eines benutzerdefinierten DNS in einem ARO-Cluster (Azure Red Hat OpenShift)

Dieser Artikel enthält die erforderlichen Informationen, die Sie zum Konfigurieren Ihres ARO-Clusters (Azure Red Hat OpenShift) für die Verwendung eines benutzerdefinierten DNS-Servers benötigen. Er enthält die Clusteranforderungen für eine grundlegende ARO-Bereitstellung.

## <a name="before-you-begin"></a>Vorbereitung

In diesem Artikel wird davon ausgegangen, dass Sie einen neuen Cluster erstellen oder über einen vorhandenen Cluster mit den neuesten Updates verfügen. Wenn Sie einen ARO-Cluster benötigen, finden Sie unter [Tutorial: Erstellen eines Azure Red Hat OpenShift 4-Clusters](./tutorial-create-cluster.md) Informationen zu einem öffentlichen Cluster bzw. unter [Erstellen eines privaten Azure Red Hat OpenShift 4-Clusters](./howto-create-private-cluster-4x.md) Informationen zu einem privaten Cluster. Die Schritte zum Konfigurieren Ihres Clusters für die Verwendung eines benutzerdefinierten DNS-Servers sind für private und öffentliche Cluster identisch.

### <a name="confirm-cluster-compatibility-with-custom-dns"></a>Überprüfen der Clusterkompatibilität mit benutzerdefiniertem DNS

Vergewissern Sie sich, dass Ihr Cluster dieses Feature unterstützen kann, indem Sie überprüfen, ob `99-master-aro-dns` und `99-worker-aro-dns` `machineconfigs` vorhanden sind.

```
oc get machineconfig
```

Wenn die Ergebnisse des obigen Befehls die folgenden Computerkonfigurationen (machineconfig) enthalten, ist Ihr Cluster für die Unterstützung von benutzerdefiniertem DNS berechtigt.

```
NAME                 GENERATEDBYCONTROLLER                      IGNITIONVERSION   AGE
...
99-master-aro-dns                                               2.2.0             54d
99-worker-aro-dns                                               2.2.0             54d
...
```

## <a name="dns-architecture-overview"></a>Übersicht über die DNS-Architektur

Wenn die einzelnen Knoten im Azure Red Hat OpenShift-Cluster aktiviert und dem Netzwerk hinzugefügt werden, konfiguriert DHCP den virtuellen Computer mit Informationen wie der IP-Adresse und dem zu verwendenden DNS-Server.

Nachfolgend sehen Sie eine Übersicht über den Prozessablauf für die Konfiguration:

![DNS](media/concepts-networking/custom-dns-pfd.jpg)

Ein wichtiger Nachteil bei der Verwendung Ihres eigenen DNS-Servers anstelle des DNS-Standardservers im virtuellen Netzwerk besteht darin, dass die vom DNS-Server bereitgestellte Konfiguration verloren geht. Die Namen der virtuellen Computer werden nicht mehr über DNS im Netzwerk aufgelöst.

### <a name="update-process-overview"></a>Übersicht über den Aktualisierungsvorgang

Das Konfigurieren eines benutzerdefinierten DNS-Servers für den Cluster ist in zwei Schritte unterteilt:

1. Ändern der Konfigurationseinstellung für die VNet-DNS-Server
2. Neustarten von Knoten im Cluster zum Übernehmen von Änderungen


## <a name="configure-a-custom-dns-server"></a>Konfigurieren eines benutzerdefinierten DNS-Servers

Die folgenden Schritte können auch über die Befehlszeile ausgeführt werden. In dieser Dokumentation wird jedoch die Weboberfläche des Portals verwendet.

### <a name="update-dns-configuration-in-virtual-network"></a>Aktualisieren der DNS-Konfiguration im virtuellen Netzwerk

Melden Sie sich beim Azure-Portal an, und navigieren Sie zu dem virtuellen Netzwerk, das Sie aktualisieren möchten. Wählen Sie in der Einstellungsliste für virtuelle Netzwerke die Option **DNS-Server** aus.

![Auswählen von DNS](media/concepts-networking/vnet-dns-config.png)

Aktivieren Sie auf dem angezeigt DNS-Konfigurationsbildschirm das Optionsfeld **Benutzerdefiniert**. Geben Sie die IP-Adressen für Ihre DNS-Server ein.

>[!IMPORTANT]
> Wenn Sie einen benutzerdefinierten DNS-Server angeben möchten, können Sie Knotennamen im virtuellen Netzwerk nicht mehr über DNS auflösen. Knoten sind nur über die IP-Adresse erreichbar.

![Angeben benutzerdefinierter DNS-Server](media/concepts-networking/vnet-custom-dns.png)

Wählen Sie **Speichern** aus.

>[!NOTE]
> Wie auf der Portaloberfläche gezeigt, müssen Sie alle virtuellen Computer neu starten, damit die Änderungen wirksam werden.

Sie sollten eine Benachrichtigung erhalten, dass Ihr Update erfolgreich war.

![Bestätigung der DNS-Änderungen](media/concepts-networking/vnet-dns-confirm-saved.png)

### <a name="gracefully-reboot-your-cluster"></a>Ordnungsgemäßes Neustarten des Clusters

Für diese Schritte ist eine gültige kubeconfig-Datei für Ihren Cluster erforderlich. Ausführliche Informationen zum Abrufen einer kubeconfig-Datei finden Sie in [diesem Tutorial](./tutorial-connect-cluster.md).

Mit den folgenden Codeausschnitten werden Noop-Computerkonfigurationen (`machineconfig`) für Master- und Workerknoten erstellt. Dadurch können Sie parallele Neustarts für die Worker- oder Masterknoten initiieren. Weitere Informationen zu Machine Config Operator (MCO) finden Sie im [Quellcode](https://github.com/openshift/machine-config-operator) oder in der [OpenShift-Dokumentation für MCO](https://docs.openshift.com/container-platform/4.6/architecture/control-plane.html).
#### <a name="machineconfig-definitions"></a>MachineConfig-Definitionen

Workerneustarts:

```
apiVersion: machineconfiguration.openshift.io/v1
kind: MachineConfig
metadata:
  labels:
    machineconfiguration.openshift.io/role: worker
  name: 25-machineconfig-worker-reboot
spec:
  config:
    ignition:
      version: 2.2.0
    storage:
      files:
      - contents:
          source: data:text/plain;charset=utf-8;base64,cmVzdGFydAo=
        filesystem: root
        mode: 0644
        path: /etc/mco-noop-worker-restart.txt
```

Masterneustarts:

```
apiVersion: machineconfiguration.openshift.io/v1
kind: MachineConfig
metadata:
  labels:
    machineconfiguration.openshift.io/role: master
  name: 25-machineconfig-master-reboot
spec:
  config:
    ignition:
      version: 2.2.0
    storage:
      files:
      - contents:
          source: data:text/plain;charset=utf-8;base64,cmVzdGFydAo=
        filesystem: root
        mode: 0644
        path: /etc/mco-master-noop-restart.txt
```

#### <a name="reboot-worker-nodes"></a>Neustarten von Workerknoten

Erstellen Sie die Workerneustartdatei. In diesem Beispiel wird die Datei `worker-restarts.yml` aufgerufen und angewendet.

```
[user@bastion ~]$ vim worker-restarts.yml
[user@bastion ~]$ oc apply -f worker-restarts.yml
machineconfig.machineconfiguration.openshift.io/25-machineconfig-worker-reboot created
```

MCO verschiebt Workloads und startet dann nacheinander die einzelnen Knoten neu. Sobald die Worker wieder online sind, führen Sie das gleiche Verfahren aus, um die Masterknoten neu zu starten. Sie können den Status der Worker überprüfen, indem Sie die Knoten abfragen und überprüfen, ob sie sich alle im Zustand `Ready` befinden.

>[!NOTE]
> Abhängig von der Größe der Clusterworkload kann es einige Minuten dauern, bis jeder Knoten neu gestartet wurde.


Beispielworkerknoten, die nicht vollständig bereit sind:

```
NAME                                  STATUS                     ROLES    AGE     VERSION
dns-docs-tm45t-master-0               Ready                      master   5h40m   v1.19.0+a5a0987
dns-docs-tm45t-master-1               Ready                      master   5h40m   v1.19.0+a5a0987
dns-docs-tm45t-master-2               Ready                      master   5h40m   v1.19.0+a5a0987
dns-docs-tm45t-worker-eastus1-8t6q8   Ready                      worker   5h35m   v1.19.0+a5a0987
dns-docs-tm45t-worker-eastus2-ln2kq   Ready,SchedulingDisabled   worker   5h34m   v1.19.0+a5a0987
dns-docs-tm45t-worker-eastus3-gg75h   Ready                      worker   5h35m   v1.19.0+a5a0987
```

Wenn der Knoten neu gestartet wird, wird sein Zustand in „NotReady“ geändert:

```
dns-docs-tm45t-worker-eastus2-ln2kq   NotReady,SchedulingDisabled   worker   5h38m   v1.19.0+a5a0987
```

Vollständig bereit:

```
[user@bastion ~]$ oc get nodes
NAME                                  STATUS   ROLES    AGE     VERSION
dns-docs-tm45t-master-0               Ready    master   5h45m   v1.19.0+a5a0987
dns-docs-tm45t-master-1               Ready    master   5h46m   v1.19.0+a5a0987
dns-docs-tm45t-master-2               Ready    master   5h46m   v1.19.0+a5a0987
dns-docs-tm45t-worker-eastus1-8t6q8   Ready    worker   5h41m   v1.19.0+a5a0987
dns-docs-tm45t-worker-eastus2-ln2kq   Ready    worker   5h40m   v1.19.0+a5a0987
dns-docs-tm45t-worker-eastus3-gg75h   Ready    worker   5h41m   v1.19.0+a5a0987
```

#### <a name="reboot-master-nodes"></a>Neustarten von Masterknoten

Wiederholen Sie nun den gleichen Prozess für die Masterknoten:

```
[user@bastion ~]$ vim master-restarts.yml
[user@bastion ~]$ oc apply -f master-restarts.yml
machineconfig.machineconfiguration.openshift.io/25-machineconfig-master-reboot created
```

Vergewissern Sie sich, dass alle Knoten wieder den Zustand `Ready` aufweisen:

```
[user@bastion ~]$ oc get nodes
NAME                                  STATUS   ROLES    AGE    VERSION
dns-docs-tm45t-master-0               Ready    master   6h8m   v1.19.0+a5a0987
dns-docs-tm45t-master-1               Ready    master   6h8m   v1.19.0+a5a0987
dns-docs-tm45t-master-2               Ready    master   6h8m   v1.19.0+a5a0987
dns-docs-tm45t-worker-eastus1-8t6q8   Ready    worker   6h3m   v1.19.0+a5a0987
dns-docs-tm45t-worker-eastus2-ln2kq   Ready    worker   6h2m   v1.19.0+a5a0987
dns-docs-tm45t-worker-eastus3-gg75h   Ready    worker   6h3m   v1.19.0+a5a0987
```

#### <a name="confirm-changes-on-a-node-optional"></a>Überprüfen der Änderungen für einen Knoten (optional)

Zum Überprüfen des neuen DNS-Servers auf einem Knoten verwenden Sie den Pod `oc debug`.

```
[user@bastion ~]$ oc debug node/dns-docs-tm45t-worker-eastus2-ln2kq
Starting pod/dns-docs-tm45t-worker-eastus2-ln2kq-debug ...
To use host binaries, run `chroot /host`
chroot Pod IP: 10.0.2.6
If you don't see a command prompt, try pressing enter.
sh-4.4# chroot /host
sh-4.4# uptime
 18:40:16 up 1 min,  0 users,  load average: 0.82, 0.32, 0.12
sh-4.4# cat /etc/resolv.conf.dnsmasq
# Generated by NetworkManager
search reddog.microsoft.com
nameserver 192.168.0.1
```
## <a name="modifying-the-custom-dns-server"></a>Ändern des benutzerdefinierten DNS-Servers

Zum Ändern des benutzerdefinierten DNS in einem Cluster, der bereits über benutzerdefiniertes DNS verfügt, wird das gleiche [Verfahren](#update-process-overview) genutzt.

### <a name="modify-dns"></a>Ändern des DNS

Führen Sie das [hier](#update-dns-configuration-in-virtual-network) beschriebene Verfahren aus, um die DNS-Konfiguration im virtuellen Netzwerk zu aktualisieren.

### <a name="reboot-nodes"></a>Neustarten von Knoten

Löschen Sie die `machineconfig`-Elemente, die Sie beim ersten Mal erstellt haben, anstatt `machineconfig` zu erstellen. Sie beginnen mit den Workerknoten:

```
oc delete machineconfig 25-machineconfig-worker-reboot
```

Die Ausgabe ist:
```
machineconfig.machineconfiguration.openshift.io "25-machineconfig-worker-reboot" deleted
```

Warten Sie, bis alle Workerknoten neu gestartet wurden. Dies ähnelt dem [Neustarten von Workerknoten](#reboot-worker-nodes) weiter oben.

Jetzt starten Sie die Masterknoten neu.

```
oc delete machineconfig 25-machineconfig-master-reboot
```

Die Ausgabe ist:

```
machineconfig.machineconfiguration.openshift.io "25-machineconfig-master-reboot" deleted
```

Warten Sie, bis alle Masterknoten neu gestartet wurden und wieder den Zustand „Bereit“ aufweisen.