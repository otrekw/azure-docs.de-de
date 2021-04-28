---
title: Bewährte Methoden für Speicherung und Sicherung
titleSuffix: Azure Kubernetes Service
description: Lernen Sie die bewährten Methoden für Speicherung, Datenverschlüsselung und Sicherungen in Azure Kubernetes Service (AKS) für Clusteroperatoren kennen.
services: container-service
ms.topic: conceptual
ms.date: 03/10/2021
ms.openlocfilehash: 9b3ee6fd7eea958a573743b21bf8940458e2a965
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107104914"
---
# <a name="best-practices-for-storage-and-backups-in-azure-kubernetes-service-aks"></a>Best Practices für Speicherung und Sicherungen in Azure Kubernetes Service (AKS)

Beim Erstellen und Verwalten von Clustern in Azure Kubernetes Service (AKS) benötigen Ihre Anwendungen oft Speicher. Stellen Sie sicher, dass Sie die Leistungsanforderungen und Zugriffsmethoden für Pods verstehen, damit Sie den optimalen Speicher für Ihre Anwendung auswählen können. Die Größe des AKS-Knotens kann sich auf Ihre Speicheroptionen auswirken. Planen Sie Möglichkeiten zur Sicherung und zum Testen des Wiederherstellungsprozesses für angefügten Speicher.

Dieser Best Practices-Artikel konzentriert sich auf Überlegungen zur Speicherung für Clusteroperatoren. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Welche Arten von Speicher verfügbar sind
> * Wie man AKS-Knoten für die Speicherleistung richtig dimensioniert
> * Unterschiede zwischen dynamischer und statischer Bereitstellung von Volumes
> * Möglichkeiten zum Sichern und Schützen Ihrer Datenvolumes

## <a name="choose-the-appropriate-storage-type"></a>Auswählen des geeigneten Speichertyps

> **Best Practices-Leitfaden**
> 
> Zur Auswahl des richtigen Speichers sollten Sie die Anforderungen Ihrer Anwendung verstehen. Verwenden Sie SSD-basierten Hochleistungsspeicher für Produktionsworkloads. Planen Sie netzwerkbasierten Speicher, wenn Sie mehrere gleichzeitige Verbindungen benötigen.

Anwendungen erfordern häufig verschiedene Arten und von Speicher mit unterschiedlichen Geschwindigkeiten. Ermitteln Sie den am besten geeigneten Speichertyp, indem Sie die folgenden Fragen stellen: 
* Benötigen Ihre Anwendungen Speicher, der mit einzelnen Pods verbunden ist?
* Benötigen Ihre Anwendungen Speicher, der über mehrere Pods freigegeben wird? 
* Ist der Speicher für den schreibgeschützten Zugriff auf Daten vorgesehen?
* Wird der Speicher für das Schreiben großer Mengen strukturierter Daten verwendet? 

In der folgenden Tabelle sind die verfügbarer Speichertypen und ihre Fähigkeiten aufgeführt:

| Anwendungsfall | Volume-Plug-In | Lese-/Schreibzugriff, einmal | Schreibgeschützt, mehrfach | Lese-/Schreibzugriff, mehrfach | Unterstützung für Windows Server-Container |
|----------|---------------|-----------------|----------------|-----------------|--------------------|
| Freigegebene Konfiguration       | Azure Files   | Ja | Ja | Ja | Ja |
| Strukturierte App-Daten        | Azure Disks   | Ja | Nein  | Nein  | Ja |
| Unstrukturierte Daten, Dateisystemvorgänge | [BlobFuse][blobfuse] | Ja | Ja | Ja | Nein |

AKS bietet zwei primäre Typen von sicherem Speicher für Volumes, die von Azure-Datenträgern oder Azure Files unterstützt werden. Beide verwenden die standardmäßige Azure-Speicherdienstverschlüsselung (Storage Service Encryption, SSE), die ruhende Daten verschlüsselt. Datenträger können nicht mit Azure Disk Encryption auf AKS-Knotenebene verschlüsselt werden.

Sowohl Azure Files als auch Azure-Datenträger sind derzeit in der Leistungsstufe „Standard“ und „Premium“ verfügbar:

- *Premium*-Datenträger
    - Von Hochleistungs-SSDs unterstützt 
    - Für alle Produktionsworkloads empfohlen
- *Standard*-Datenträger
    - Von regulären rotierenden Festplatten (HDDs) unterstützt
    - Gut geeignet für die Archivierung oder selten genutzte Daten

Um die geeignete Speicherebene auszuwählen, sollten Sie die Anforderungen an Anwendungsleistung und die Zugriffsmuster kennen. Weitere Informationen zur Größe und den Leistungsstufen von Managed Disks finden Sie in der [Übersicht über Managed Disks][managed-disks].

### <a name="create-and-use-storage-classes-to-define-application-needs"></a>Erstellen und Verwenden von Speicherklassen zum Definieren von Anwendungsanforderungen

Definieren Sie den gewünschten Speichertyp mithilfe von Kubernetes-*Speicherklassen*. Die Speicherklasse wird dann in der Pod- oder Bereitstellungsspezifikation referenziert. Durch Kombination von Speicherklassendefinitionen wird der geeignete Speicher erstellt und mit Pods verbunden. 

Weitere Informationen finden Sie unter [Speicherklassen in AKS][aks-concepts-storage-classes].

## <a name="size-the-nodes-for-storage-needs"></a>Größe der Knoten für den Speicherbedarf

> **Best Practices-Leitfaden**
> 
> Jede Knotengröße unterstützt eine maximale Anzahl von Datenträgern. Unterschiedliche Knotengrößen stellen auch verschiedene Mengen an lokalem Speicher und Netzwerkbandbreite bereit. Planen Sie entsprechend Ihren Anwendungsanforderungen, um die richtige Knotengröße bereitzustellen.

AKS-Knoten werden als verschiedene Azure-VM-Typen und -Größen ausgeführt. Jede VM-Größe bietet Folgendes:
* Unterschiedliche Menge an Kernressourcen wie CPU und Arbeitsspeicher 
* Maximale Anzahl von Datenträgern, die angefügt werden können 

Die Speicherleistung der verschiedenen VM-Größen variiert auch bezüglich der maximalen IOPS (Eingabe-/Ausgabevorgänge pro Sekunde) auf lokalen und angeschlossenen Datenträgern.

Wenn Ihre Anwendungen Azure-Datenträger als Speicherlösung benötigen, planen Sie eine geeignete VM-Größe für Knoten. Die Speicherkapazität sowie die Menge an CPU und Arbeitsspeicher spielen bei der Entscheidung über eine VM-Größe eine wichtige Rolle. 

Beispielsweise beinhalten die VM-Größen *Standard_B2ms* und *Standard_DS2_v2* beide eine ähnliche Menge an CPU- und Arbeitsspeicherressourcen, unterscheiden sich jedoch hinsichtlich der potenziellen Speicherleistung:

| Knotentyp und -größe | vCPU | Arbeitsspeicher (GiB) | Max. Anzahl Datenträger | Maximale Anzahl nicht zwischengespeicherter Datenträger-IOPS | Maximaler nicht zwischengespeicherter Durchsatz (Mbit/s) |
|--------------------|------|--------------|----------------|------------------------|--------------------------------|
| Standard_B2ms      | 2    | 8            | 4              | 1\.920                  | 22,5                           |
| Standard_DS2_v2    | 2    | 7            | 8              | 6\.400                  | 96                             |

In diesem Beispiel bietet *Standard_DS2_v2* doppelt so viele angefügte Datenträger und das Drei- bis Vierfache an IOPS und Datenträgerdurchsatz. Wenn Sie nur die wichtigsten Computeressourcen und die Kosten vergleichen, wählen Sie möglicherweise die VM-Größe *Standard_B2ms* mit schlechter Speicherleistung und Einschränkungen aus. 

Arbeiten Sie mit Ihrem Anwendungsentwicklungsteam zusammen, um ihren Speicherkapazitätsbedarf und ihre Leistungsanforderungen zu verstehen. Wählen Sie die geeignete VM-Größe für die AKS-Knoten aus, um ihre Leistungsanforderungen zu erfüllen oder zu überschreiten. Ermitteln Sie regelmäßig die Baseline von Anwendungen, um die VM-Größe bei Bedarf anzupassen.

Weitere Informationen zu verfügbaren VM-Größen finden Sie unter [Größen für virtuelle Linux-Computer in Azure][vm-sizes].

## <a name="dynamically-provision-volumes"></a>Dynamisches Bereitstellen von Volumes

> **Best Practices-Leitfaden** 
>
> Um den Verwaltungsaufwand zu reduzieren und ein Skalieren zu ermöglichen, vermeiden Sie das statische Erstellen und Zuweisen persistenter Volumes. Verwenden Sie die dynamische Bereitstellung. Definieren Sie in Ihren Speicherklassen die entsprechende Freigaberichtlinie, um die Kosten für nicht benötigten Speicher nach dem Löschen von Pods zu minimieren.

Zum Anfügen von Speicher an Pods verwenden Sie persistente Volumes. Persistente Volumes können manuell oder dynamisch erstellt werden. Die manuelle Erstellung von persistenten Volumes erhöht den Verwaltungsaufwand und beschränkt Ihre Möglichkeiten der Skalierung. Stellen Sie persistente Volumes stattdessen dynamisch bereit, um die Speicherverwaltung zu vereinfachen und Ihre Anwendungen bei Bedarf erweitern und skalieren zu können.

![Ansprüche auf persistente Volumes in einem Azure Kubernetes Service-Cluster (AKS)](media/concepts-storage/persistent-volume-claims.png)

Mit einem Anspruch auf persistente Volumes (PVC) können Sie bei Bedarf dynamisch Speicher erstellen. Zugrunde liegende Azure-Datenträger werden erstellt, wenn sie von Pods angefordert werden. In der Poddefinition fordern Sie an, dass ein Volume erstellt und an den festgelegten Einbindungspfad angefügt wird.

Informationen zu den Konzepten zum dynamischen Erstellen und Verwenden von Volumes finden Sie unter [Ansprüche auf persistente Volumes][aks-concepts-storage-pvcs].

Um diese Volumes in Aktion zu sehen, schauen Sie sich an, wie Sie ein persistentes Volume mit [Azure-Datenträgern][dynamic-disks] oder [Azure Files][dynamic-files] dynamisch erstellen und verwenden.

Legen Sie als Teil Ihrer Speicherklassendefinitionen die entsprechende *reclaimPolicy* fest. Diese reclaimPolicy steuert das Verhalten der zugrunde liegenden Azure-Speicherressource, wenn der Pod gelöscht wird. Die zugrunde liegende Speicherressource kann entweder gelöscht oder für einen zukünftigen Pod beibehalten werden. Legen Sie die reclaimPolicy auf *Beibehalten* oder *Löschen* fest. 

Sie sollten die Anforderungen Ihrer Anwendung kennen und regelmäßige Prüfungen auf beibehaltenen Speicher implementieren, um die Menge an ungenutztem und in Rechnung gestelltem Speicher zu minimieren.

Weitere Informationen zu Speicherklassenoptionen finden Sie unter [Richtlinien zur Freigabe von Speicher][reclaim-policy].

## <a name="secure-and-back-up-your-data"></a>Schützen und Sichern Ihrer Daten

> **Best Practices-Leitfaden** 
> 
> Sichern Sie Ihre Daten mithilfe eines entsprechenden Tools für Ihren Speichertyp, z. B. Velero oder Azure Backup. Überprüfen Sie die Integrität und Sicherheit dieser Sicherungen.

Wenn Ihre Anwendungen Daten speichern und verbrauchen, die auf Festplatten oder in Dateien gespeichert sind, müssen Sie regelmäßig Sicherungen oder Momentaufnahmen dieser Daten erstellen. Azure-Datenträger können integrierte Momentaufnahmetechnologien verwenden. Möglicherweise müssen Ihre Anwendungen die Schreibzugriffe auf die Festplatte leeren, bevor Sie den Momentaufnahmevorgang ausführen. [Velero][velero] kann persistente Volumes zusammen mit zusätzlichen Clusterressourcen und -konfigurationen sichern. Wenn Sie [den Zustand nicht aus Ihren Anwendungen entfernen können][remove-state], sichern Sie die Daten von persistenten Volumes, und testen Sie die Wiederherstellungsvorgänge regelmäßig, um die Datenintegrität und die erforderlichen Prozesse zu überprüfen.

Sie sollten die Grenzen der verschiedenen Ansätze für Datensicherungen kennen und wissen, ob Sie Ihre Daten vor dem erstellen der Momentaufnahme stilllegen müssen. Datensicherungen ermöglichen es Ihnen nicht unbedingt, Ihre Anwendungsumgebung der Clusterbereitstellung wiederherzustellen. Weitere Informationen zu diesen Szenarien finden Sie unter [Best Practices für Geschäftskontinuität und Notfallwiederherstellung in Azure Kubernetes Service (AKS)][best-practices-multi-region].

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel konzentriert sich auf bewährte Speichermethoden in AKS. Weitere Informationen zu den Grundlagen der Speicherung in Kubernetes finden Sie unter [Speicherkonzepte für Anwendungen in AKS][aks-concepts-storage].

<!-- LINKS - External -->
[velero]: https://github.com/heptio/velero
[blobfuse]: https://github.com/Azure/azure-storage-fuse

<!-- LINKS - Internal -->
[aks-concepts-storage]: concepts-storage.md
[vm-sizes]: ../virtual-machines/sizes.md
[dynamic-disks]: azure-disks-dynamic-pv.md
[dynamic-files]: azure-files-dynamic-pv.md
[reclaim-policy]: concepts-storage.md#storage-classes
[aks-concepts-storage-pvcs]: concepts-storage.md#persistent-volume-claims
[aks-concepts-storage-classes]: concepts-storage.md#storage-classes
[managed-disks]: ../virtual-machines/managed-disks-overview.md
[best-practices-multi-region]: operator-best-practices-multi-region.md
[remove-state]: operator-best-practices-multi-region.md#remove-service-state-from-inside-containers
