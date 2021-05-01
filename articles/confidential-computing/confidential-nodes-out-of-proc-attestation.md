---
title: Nachweisunterstützung mit DaemonSet für Intel SGX-Angebotshilfe in Azure (Vorschau)
description: DaemonSet zum Erstellen des Angebots außerhalb des Intel SGX-Anwendungsprozesses. In diesem Artikel erfahren Sie, wie das Feature für Out-of-Process-Nachweise für vertrauliche Workloads bereitgestellt wird, die in einem Container ausgeführt werden.
ms.service: container-service
ms.subservice: confidential-computing
author: agowdamsft
ms.topic: overview
ms.date: 2/12/2021
ms.author: amgowda
ms.openlocfilehash: 849fd7afa3f9365f31ee8e03d9f9cc2174d64304
ms.sourcegitcommit: afb79a35e687a91270973990ff111ef90634f142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107484403"
---
# <a name="platform-software-management-with-intel-sgx-quote-helper-daemonset-preview"></a>Plattformsoftwareverwaltung mit DaemonSet für Intel SGX-Angebotshilfe (Vorschau)

Für [Enklavenanwendungen](confidential-computing-enclaves.md) mit Remotenachweis wird ein erstelltes Angebot benötigt. Dieses Angebot liefert den kryptografischen Nachweis der Identität und des Zustands der Anwendung und gibt die Umgebung an, in der die Enklave ausgeführt wird. Für die Erstellung des Angebots müssen vertrauenswürdige Softwarekomponenten verwendet werden, die Teil der Plattformsoftwarekomponenten (PSW) von Intel sind.

## <a name="overview"></a>Übersicht
 
Intel unterstützt zwei Nachweismodi zum Ausführen der Angebotserstellung:

- *In-Process:* Die vertrauenswürdigen Softwarekomponenten werden innerhalb des Enklavenanwendungsprozesses gehostet.

- *Out-of-Process:* Die vertrauenswürdigen Softwarekomponenten werden außerhalb der Enklavenanwendung gehostet.
 
Von Intel SGX-Anwendungen (Intel Software Guard Extension), die mit dem Open Enclave SDK erstellt werden, wird standardmäßig der In-Process-Nachweismodus verwendet. Intel SGX-basierte Anwendungen lassen die Verwendung des Out-of-Process-Nachweismodus zu. Wenn Sie diesen Modus verwenden möchten, benötigen Sie zusätzliches Hosting und müssen die erforderlichen Komponenten wie etwa Architectural Enclave Service Manager (AESM) außerhalb der Anwendung verfügbar machen.

Dieses Feature verbessert die Betriebszeit für Ihre Enklaven-Apps bei Updates der Intel-Plattform oder der DCAP-Treiber. Wir empfehlen daher, es zu verwenden.

Wenn Sie dieses Feature in einem AKS-Cluster (Azure Kubernetes Services) aktivieren möchten, fügen Sie der Azure CLI beim Aktivieren des Confidential Computing-Add-Ons den Befehl `--enable-sgxquotehelper` hinzu. 

```azurecli-interactive
# Create a new AKS cluster with system node pool with Confidential Computing addon enabled and SGX Quote Helper
az aks create -g myResourceGroup --name myAKSCluster --generate-ssh-keys --enable-addon confcom --enable-sgxquotehelper
```

Weitere Informationen finden Sie unter [Schnellstart: Bereitstellen eines AKS-Clusters mit Confidential Computing-Knoten über die Azure CLI](confidential-nodes-aks-get-started.md).

## <a name="benefits-of-the-out-of-process-mode"></a>Vorteile des Out-of-Process-Modus

In der folgenden Liste werden einige der Hauptvorteile dieses Nachweismodus beschrieben:

-   Für die Komponenten zur Angebotserstellung von PSW für die einzelnen Containeranwendungen sind keine Updates erforderlich. Containerbesitzer müssen keine Updates in ihrem Container verwalten. Stattdessen nutzen Containerbesitzer die vom Anbieter bereitgestellte Schnittstelle, mit der der zentralisierte Dienst außerhalb des Containers aufgerufen wird. Der Container wird durch den Anbieter aktualisiert und verwaltet.

-   Sie müssen sich keine Gedanken über Nachweisfehler aufgrund von veralteten PSW-Komponenten machen. Der Anbieter verwaltet die Updates für diese Komponenten.

-   Der Out-of-Process-Modus bietet im Vergleich zum In-Process-Modus eine bessere Auslastung des EPC-Arbeitsspeichers. Im In-Process-Modus muss jede Enklavenanwendung die Kopie von QE und PCE für den Remotenachweis instanziieren. Im Out-of-Process-Modus müssen diese Enklaven nicht durch den Container gehostet werden. Somit wird auch kein Enklavenarbeitsspeicher aus dem Containerkontingent beansprucht.

-   Wenn Sie den Intel SGX-Treiber mittels Upstream in einen Linux-Kernel integrieren, wird für eine Enklave eine höhere Berechtigung erzwungen. Mit dieser Berechtigung kann die Enklave PCE aufrufen, was dazu führt, dass die im In-Process-Modus ausgeführte Enklavenanwendung nicht mehr funktioniert. Standardmäßig wird Enklaven diese Berechtigung nicht gewährt. Zum Gewähren dieser Berechtigung für eine Enklavenanwendung muss der Prozess für die Anwendungsinstallation geändert werden. Im Out-of-Process-Modus stellt dagegen der Anbieter des Diensts, von dem Out-of-Process-Anforderungen behandelt werden, sicher, dass der Dienst mit dieser Berechtigung installiert wird.

-   Es ist keine Überprüfung auf Abwärtskompatibilität mit PSW und DCAP erforderlich. Die Updates der Komponenten für die Angebotserstellung von PSW werden vom Anbieter vor der Aktualisierung auf Abwärtskompatibilität überprüft. Dadurch können Sie Kompatibilitätsprobleme behandeln, bevor Sie Updates für vertrauliche Workloads bereitstellen.

## <a name="confidential-workloads"></a>Vertrauliche Workloads

Der Angebotsanforderer und die Angebotserstellung werden separat, aber auf dem gleichen physischen Computer ausgeführt. Die Angebotserstellung ist zentralisiert und verarbeitet Angebotsanforderungen von allen Entitäten. Die Schnittstelle muss ordnungsgemäß definiert und ermittelbar sein, damit Entitäten Angebote anfordern können.

![Diagramm: Beziehungen zwischen Angebotsanforderer, Angebotserstellung und Schnittstelle](./media/confidential-nodes-out-of-proc-attestation/aesmmanager.png)

Dieses abstrakte Modell gilt für das Szenario mit vertraulichen Workloads und nutzt den bereits verfügbaren AESM-Dienst. AESM befindet sich in einem Container und wird im gesamten Kubernetes-Cluster als DaemonSet bereitgestellt. Durch Kubernetes wird garantiert, dass auf jedem Agent-Knoten eine einzelne Instanz eines AESM-Dienstcontainers (umschlossen von einem Pod) bereitgestellt wird. Das neue DaemonSet für Intel SGX-Angebote ist vom DaemonSet „sgx-device-plugin“ abhängig, da der AESM-Dienstcontainer zum Starten von QE- und PCE-Enklaven EPC-Arbeitsspeicher von „sgx-device-plugin“ anfordert.

Für jeden Container muss die Nutzung der Out-of-Process-Angebotserstellung aktiviert werden. Hierzu muss bei der Erstellung die Umgebungsvariable `SGX_AESM_ADDR=1` festgelegt werden. Darüber hinaus sollte der Container auch das Paket „libsgx-quote-ex“ enthalten, über das die Anforderung an den standardmäßigen UNIX-Domänensocket weitergeleitet wird.

Für eine Anwendung kann trotzdem wie zuvor der In-Process-Nachweis genutzt werden, aber „In-Process“ und „Out-of-Process“ können nicht gleichzeitig in einer Anwendung verwendet werden. Die Out-of-Process-Infrastruktur ist standardmäßig verfügbar und verbraucht Ressourcen.

## <a name="sample-implementation"></a>Beispielimplementierung

Die folgende Docker-Datei ist ein Beispiel für eine Open Enclave-basierte Anwendung. Legen Sie die Umgebungsvariable `SGX_AESM_ADDR=1` in der Docker-Datei oder in der Bereitstellungsdatei fest. Das folgende Beispiel enthält Details zur Docker-Datei und zur Bereitstellung. 

  > [!Note] 
  > „libsgx-quote-ex“ von Intel muss im Anwendungscontainer verpackt sein, damit der Out-of-Process-Nachweis ordnungsgemäß funktioniert.
    
```yaml
# Refer to Intel_SGX_Installation_Guide_Linux for details
FROM ubuntu:18.04 as sgx_base
RUN apt-get update && apt-get install -y \
    wget \
    gnupg

# Add the repository to sources, and add the key to the list of
# trusted keys used by the apt to authenticate packages
RUN echo "deb [arch=amd64] https://download.01.org/intel-sgx/sgx_repo/ubuntu bionic main" | tee /etc/apt/sources.list.d/intel-sgx.list \
    && wget -qO - https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key | apt-key add -
# Add Microsoft repo for az-dcap-client
RUN echo "deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/prod bionic main" | tee /etc/apt/sources.list.d/msprod.list \
    && wget -qO - https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

FROM sgx_base as sgx_sample
RUN apt-get update && apt-get install -y \
    clang-7 \
    libssl-dev \
    gdb \
    libprotobuf10 \
    libsgx-dcap-ql \
    libsgx-quote-ex \
    az-dcap-client \
    open-enclave
WORKDIR /opt/openenclave/share/openenclave/samples/remote_attestation
RUN . /opt/openenclave/share/openenclave/openenclaverc \
    && make build
# This sets the flag for out-of-process attestation mode. Alternatively you can set this flag on the deployment files.
ENV SGX_AESM_ADDR=1 

CMD make run
```
Alternativ können Sie den Out-of-Process-Nachweismodus in der YAML-Bereitstellungsdatei festlegen. Gehen Sie dabei folgendermaßen vor:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: sgx-test
spec:
  template:
    spec:
      containers:
      - name: sgxtest
        image: <registry>/<repository>:<version>
        env:
        - name: SGX_AESM_ADDR
          value: 1
        resources:
          limits:
            kubernetes.azure.com/sgx_epc_mem_in_MiB: 10
        volumeMounts:
        - name: var-run-aesmd
          mountPath: /var/run/aesmd
      restartPolicy: "Never"
      volumes:
      - name: var-run-aesmd
        hostPath:
          path: /var/run/aesmd
```

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Bereitstellen eines AKS-Clusters mit Confidential Computing-Knoten über die Azure CLI](./confidential-nodes-aks-get-started.md)

[Schnellstart: Beispiele für vertrauliche Container](https://github.com/Azure-Samples/confidential-container-samples)

[DCsv2-Serie](../virtual-machines/dcv2-series.md)
