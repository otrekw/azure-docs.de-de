---
title: Konzepte – Speicher in Azure Kubernetes Service (AKS)
description: Informationen zu Speicher in Azure Kubernetes Service (AKS) einschließlich Volumes, persistente Volumes, Speicherklassen und Ansprüche
services: container-service
ms.topic: conceptual
ms.date: 03/11/2021
ms.openlocfilehash: a1f68b06c31597a1d2d044686274e86a79e6e9a3
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107105781"
---
# <a name="storage-options-for-applications-in-azure-kubernetes-service-aks"></a>Speicheroptionen für Anwendungen in Azure Kubernetes Service (AKS)

Anwendungen, die in Azure Kubernetes Service (AKS) ausgeführt werden, müssen möglicherweise Daten speichern und abrufen. Während einige Anwendungsworkloads lokale, schnelle Speicherung auf nicht benötigten, geleerten Knoten verwenden können, erfordern andere Speicher, der auf reguläreren Datenvolumes innerhalb der Azure-Plattform beibehalten wird. 

Mehrere Pods erfordern möglicherweise Folgendes:
* Gemeinsame Nutzung der gleichen Datenvolumes 
* Erneutes Anfügen von Datenvolumes, wenn der Pod auf einem anderen Knoten neu geplant wird 

Abschließend müssen Sie möglicherweise vertrauliche Daten oder Informationen zur Anwendungskonfiguration in Pods einfügen.

In diesem Artikel werden die wichtigsten Konzepte vorgestellt, mit denen Sie für Ihre Anwendungen in AKS Speicher bereitstellen:

- [Volumes](#volumes)
- [Persistente Volumes](#persistent-volumes)
- [Speicherklassen](#storage-classes)
- [Ansprüche auf persistente Volumes](#persistent-volume-claims)

![Speicheroptionen für Anwendungen in einem Azure Kubernetes Service-Cluster (AKS)](media/concepts-storage/aks-storage-options.png)

## <a name="volumes"></a>Volumes

Kubernetes behandelt einzelne Pods in der Regel als kurzlebige, verwerfbare Ressourcen. Anwendungen stehen verschiedene Methoden zur Verwendung und Beibehaltung von Daten zur Verfügung. Ein *Volume* ist eine Möglichkeit, Daten während des Lebenszyklus der Anwendung Pods übergreifend zu speichern, abzurufen und beizubehalten.

Herkömmliche Volumes werden als Kubernetes-Ressourcen erstellt, die von Azure Storage unterstützt werden. Sie können Datenvolumes manuell erstellen, damit sie Pods direkt zugewiesen werden, oder durch Kubernetes automatisch erstellen lassen. Datenvolumes können Azure-Datenträger oder Azure Files verwenden.

### <a name="azure-disks"></a>Azure Disks

Verwenden Sie *Azure-Datenträger* zum Erstellen einer Kubernetes-*DataDisk*-Ressource. Azure Disks kann Folgendes verwenden:
* Azure Storage Premium, von Hochleistungs-SSDs unterstützt, oder 
* Azure Storage Standard, von regulären HDDs unterstützt 

> [!TIP]
>Verwenden Sie für die meisten Produktionsumgebungen und Entwicklungsworkloads Storage Premium. 

Da Azure-Datenträger als *ReadWriteOnce* eingebunden werden, sind sie nur für einen einzelnen Pod verfügbar. Verwenden Sie für die Speichervolumes, auf die mehrere Pods gleichzeitig zugreifen können, Azure Files.

### <a name="azure-files"></a>Azure Files
Verwenden Sie *Azure Files*, um eine SMB 3.0-Dateifreigabe bereitzustellen, die von einem Azure Storage-Konto auf Pods unterstützt wird. Mit Azure Files können Sie Daten für mehrere Knoten und Pods übergreifend freigeben und Folgendes verwenden:
* Azure Storage Premium, von Hochleistungs-SSDs unterstützt, oder 
* Azure Storage Standard, von regulären HDDs unterstützt

### <a name="volume-types"></a>Volumetypen
Kubernetes-Volumes stellen mehr als nur einen herkömmlichen Datenträger zum Speichern und Abrufen von Informationen dar. Kubernetes-Volumes können auch als Möglichkeit zum Einfügen von Daten in einen Pod zur Verwendung von den Containern genutzt werden. 

Zu den üblichen Volumetypen in Kubernetes gehören:

#### <a name="emptydir"></a>emptyDir

Dieses Volume wird häufig als temporärer Speicher für einen Pod verwendet. Alle Container in einem Pod können auf die Daten des Volumes zugreifen. Auf diesen Volumetyp geschriebene Daten werden nur für die Lebensdauer des Pods beibehalten. Sobald Sie den Pod löschen, wird das Volume gelöscht. Dieses Volume verwendet in der Regel den zugrunde liegenden lokalen Knotendatenträger-Speicher, obwohl es auch nur im Arbeitsspeicher des Knotens vorhanden sein kann.

#### <a name="secret"></a>secret

*secret*-Volumes werden verwendet, um sensible Daten, wie z. B. Kennwörter, in Pods einzufügen. 
1. Erstellen Sie ein Geheimnis mit der Kubernetes-API. 
1. Definieren Sie Ihren Pod oder Ihre Bereitstellung, und fordern Sie ein bestimmtes Geheimnis an. 
    * Geheimnisse werden nur für Knoten mit einem geplanten Pod bereitgestellt, der diese erfordert.
    * Das Geheimnis wird in *tmpfs* gespeichert und nicht auf den Datenträger geschrieben. 
1. Wenn Sie den letzten Pod auf einem Knoten löschen, der ein Geheimnis erfordert, wird das Geheimnis aus „tmpfs“ des Knotens gelöscht. 
   * Geheimnisse werden in einem bestimmten Namespace gespeichert, und nur Pods im gleichen Namespace können darauf zugreifen.

#### <a name="configmap"></a>configMap

Mit *ConfigMap* werden Schlüssel-Wert-Paar-Eigenschaften in Pods eingefügt, z. B. Informationen zur Anwendungskonfiguration. Definieren Sie Informationen zur Anwendungskonfiguration als eine Kubernetes-Ressource, die problemlos aktualisiert und auf neue Instanzen von Pods bei deren Bereitstellung angewendet werden kann. 

Gehen Sie wie bei der Verwendung eines Geheimnisses vor:
1. Erstellen Sie eine ConfigMap mithilfe der Kubernetes-API. 
1. Fordern Sie die ConfigMap an, wenn Sie einen Pod oder eine Bereitstellung definieren. 
   * ConfigMaps werden in einem bestimmten Namespace gespeichert, und nur Pods im gleichen Namespace können darauf zugreifen.

## <a name="persistent-volumes"></a>Persistente Volumes

Volumes, die als Teil des Podlebenszyklus definiert und erstellt werden, sind nur solange vorhanden, bis Sie den Pod löschen. Wenn Pods während eines Wartungsereignisses auf einem anderen Host neu eingeplant werden, erwarten sie häufig, dass ihr Speicher erhalten bleibt, insbesondere in StatefulSets. Ein *persistentes Volume* (PV) ist eine von der Kubernetes-API erstellte und verwaltete Speicherressource, die unabhängig von der Lebensdauer eines einzelnen Pods vorhanden sein kann.

Sie können Azure-Datenträger oder Azure Files zum Bereitstellen des persistenten Volumes verwenden. Wie im Abschnitt [Volumes](#volumes) erwähnt, wird die Auswahl von Azure Disks oder Azure Files häufig von der Notwendigkeit des gleichzeitigen Zugriffs auf die Daten oder die Leistungsstufe bestimmt.

![Persistente Volumes in einem Azure Kubernetes Service-Cluster (AKS)](media/concepts-storage/persistent-volumes.png)

Ein PersistentVolume kann *statisch* von einem Clusteradministrator oder *dynamisch* vom Kubernetes-API-Server erstellt werden. Wenn ein Pod geplant ist und derzeit nicht verfügbaren Speicher anfordert, kann Kubernetes den zugrunde liegenden Speicher auf dem Azure-Datenträger oder in Azure Files erstellen und dem Pod anfügen. Dynamische Bereitstellung verwendet eine *StorageClass* zum Identifizieren des Azure-Speichertyps, der erstellt werden muss.

## <a name="storage-classes"></a>Speicherklassen

Sie können zum Definieren verschiedener Speicherstufen, z.B. „Premium“ und „Standard“, eine *StorageClass* erstellen. 

Die StorageClass definiert auch die *reclaimPolicy*. Wenn Sie den Pod löschen und das persistente Volume nicht mehr benötigt wird, steuert die reclaimPolicy das Verhalten der zugrunde liegenden Azure-Speicherressource. Die zugrunde liegende Speicherressource kann entweder gelöscht oder für die Verwendung mit einem zukünftigen Pod beibehalten werden.

In AKS werden vier anfängliche `StorageClasses` für den Cluster erstellt, der die Plug-Ins zur strukturinternen Speicherung verwendet:

| Berechtigung | `Reason` |
|---|---|
| `default` | Verwendet Azure SSD Standard-Speicher zum Erstellen eines verwalteten Datenträgers. Mit der Freigaberichtlinie wird sichergestellt, dass der zugrunde liegende Azure Disk-Datenträger gelöscht wird, wenn das persistente Volume gelöscht wird, das ihn verwendet hat. |
| `managed-premium` | Verwendet Azure Storage Premium zum Erstellen verwalteter Datenträger. Mit der Freigaberichtlinie wird wieder sichergestellt, dass der zugrunde liegende Azure Disk-Datenträger gelöscht wird, wenn das persistente Volume gelöscht wird, das ihn verwendet hat. |
| `azurefile` | Verwendet Azure Storage Standard zum Erstellen einer Azure-Dateifreigabe. Mit der Freigaberichtlinie wird sichergestellt, dass die zugrunde liegende Azure-Dateifreigabe gelöscht wird, wenn das persistente Volume gelöscht wird, das sie verwendet hat. |
| `azurefile-premium` | Verwendet Azure Storage Premium zum Erstellen einer Azure-Dateifreigabe. Mit der Freigaberichtlinie wird sichergestellt, dass die zugrunde liegende Azure-Dateifreigabe gelöscht wird, wenn das persistente Volume gelöscht wird, das sie verwendet hat.|

Für Cluster, die die neuen externen CSI-Plug-Ins (Container Storage Interface) (Vorschau) verwenden, werden die folgenden zusätzlichen `StorageClasses` erstellt:

| Berechtigung | `Reason` |
|---|---|
| `managed-csi` | Verwendet lokal redundanten Azure SSD Standard-Speicher (LRS) zum Erstellen eines verwalteten Datenträgers. Mit der Freigaberichtlinie wird sichergestellt, dass der zugrunde liegende Azure Disk-Datenträger gelöscht wird, wenn das persistente Volume gelöscht wird, das ihn verwendet hat. Mit der Speicherklasse werden auch die persistenten Volumes so konfiguriert, dass sie erweiterbar sind. Sie müssen lediglich den Anspruch der persistenten Volumes entsprechend der neuen Größe anpassen. |
| `managed-csi-premium` | Verwendet lokal redundanten Azure Premium-Speicher (LRS) zum Erstellen eines verwalteten Datenträgers. Mit der Freigaberichtlinie wird wieder sichergestellt, dass der zugrunde liegende Azure Disk-Datenträger gelöscht wird, wenn das persistente Volume gelöscht wird, das ihn verwendet hat. Zudem ermöglicht diese Speicherklasse auch eine Erweiterung von persistenten Volumes. |
| `azurefile-csi` | Verwendet Azure Storage Standard zum Erstellen einer Azure-Dateifreigabe. Mit der Freigaberichtlinie wird sichergestellt, dass die zugrunde liegende Azure-Dateifreigabe gelöscht wird, wenn das persistente Volume gelöscht wird, das sie verwendet hat. |
| `azurefile-csi-premium` | Verwendet Azure Storage Premium zum Erstellen einer Azure-Dateifreigabe. Mit der Freigaberichtlinie wird sichergestellt, dass die zugrunde liegende Azure-Dateifreigabe gelöscht wird, wenn das persistente Volume gelöscht wird, das sie verwendet hat.|

Sofern Sie keine StorageClass für ein persistentes Volume angeben, wird die standardmäßige StorageClass verwendet. Stellen Sie sicher, dass Sie beim Anfordern persistenter Volumes den entsprechenden Speicher verwenden. 

Sie können eine StorageClass für zusätzliche Anforderungen mit `kubectl` erstellen. Im folgenden Beispiel wird Managed Disks Premium verwendet und festgelegt, dass der zugrunde liegende Azure-Datenträger beim Löschen des Pods *beibehalten* werden soll:

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: managed-premium-retain
provisioner: kubernetes.io/azure-disk
reclaimPolicy: Retain
parameters:
  storageaccounttype: Premium_LRS
  kind: Managed
```

> [!NOTE]
> AKS stimmt die Standardspeicherklassen ab und überschreibt alle Änderungen, die Sie an diesen Speicherklassen vornehmen.

## <a name="persistent-volume-claims"></a>Ansprüche auf persistente Volumes

Ein PersistentVolumeClaim fordert entweder Disk- oder Files-Speicher von bestimmter StorageClass, bestimmtem Zugriffsmodus und bestimmter Größe an. Der Kubernetes-API-Server kann die zugrunde liegende Azure-Speicherressource dynamisch bereitstellen, wenn keine vorhandene Ressource den Anspruch basierend auf der definierten StorageClass erfüllen kann. 

Sobald die Verbindung des Volumes mit dem Pod hergestellt ist, enthält die Poddefinition die Volumebereitstellung.

![Ansprüche auf persistente Volumes in einem Azure Kubernetes Service-Cluster (AKS)](media/concepts-storage/persistent-volume-claims.png)

Sobald dem anfordernden Pod eine verfügbare Speicherressource zugewiesen wurde, ist ein PersistentVolume an einen PersistentVolumeClaim *gebunden*. Persistente Volumes werden Ansprüchen 1:1 zugeordnet.

Das folgende Beispiel eines YAML-Manifests zeigt einen Anspruch eines persistenten Volumes, der die StorageClass *managed-premium* verwendet und einen Datenträger der Größe *5Gi* anfordert:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 5Gi
```

Wenn Sie eine Poddefinition erstellen, geben Sie auch Folgendes an:
* Den Anspruch auf ein persistentes Volume zum Anfordern des gewünschten Speichers 
* *volumeMount* zum Lesen und Schreiben von Daten für Ihre Anwendungen 

Das folgende Beispiel-YAML-Manifest zeigt, wie der vorherige Anspruch auf ein persistentes Volume verwendet werden kann, um ein Volume unter */mnt/azure* einzubinden:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx
spec:
  containers:
    - name: myfrontend
      image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

Geben Sie zum Einbinden eines Volumes in einen Windows-Container den Laufwerkbuchstaben und Pfad an. Beispiel:

```yaml
...      
       volumeMounts:
        - mountPath: "d:"
          name: volume
        - mountPath: "c:\k"
          name: k-dir
...
```

## <a name="next-steps"></a>Nächste Schritte

Entsprechenden bewährte Methoden finden Sie unter [Bewährte Methoden für die Speicherung und Sicherungen in AKS][operator-best-practices-storage].

Informationen zum Erstellen von dynamischen und statischen Volumes, die Azure Disk oder Azure Files verwenden, finden Sie in den folgenden „Gewusst-wie“-Artikeln:

- [Erstellen eines statischen Volumes mit Azure Disks][aks-static-disks]
- [Erstellen eines statischen Volumes mit Azure Files][aks-static-files]
- [Erstellen eines dynamischen Volumes mit Azure Disks][aks-dynamic-disks]
- [Erstellen eines dynamischen Volumes mit Azure Files][aks-dynamic-files]

Weitere Informationen zu den wesentlichen Konzepten von Kubernetes und AKS finden Sie in den folgenden Artikeln:

- [Kubernetes/AKS: Cluster und Workloads][aks-concepts-clusters-workloads]
- [Kubernetes/AKS: Identität][aks-concepts-identity]
- [Kubernetes/AKS: Sicherheit][aks-concepts-security]
- [Kubernetes/AKS: virtuelle Netzwerke][aks-concepts-network]
- [Kubernetes/AKS: Skalierung][aks-concepts-scale]

<!-- EXTERNAL LINKS -->

<!-- INTERNAL LINKS -->
[aks-static-disks]: azure-disk-volume.md
[aks-static-files]: azure-files-volume.md
[aks-dynamic-disks]: azure-disks-dynamic-pv.md
[aks-dynamic-files]: azure-files-dynamic-pv.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-network]: concepts-network.md
[operator-best-practices-storage]: operator-best-practices-storage.md
