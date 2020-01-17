---
title: Einführung in Containergruppen
description: Informationen zu Containergruppen in Azure Container Instances, einer Sammlung von Instanzen mit dem gleichen Lebenszyklus, die Ressourcen gemeinsam nutzen, z. B. Speicher und Netzwerk
ms.topic: article
ms.date: 11/01/2019
ms.custom: mvc
ms.openlocfilehash: ca160c62160bc5233139dccc650474811c4cd784
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75442294"
---
# <a name="container-groups-in-azure-container-instances"></a>Containergruppen in Azure Container Instances

Die oberste Ressource in Azure Container Instances ist die *Containergruppe*. In diesem Artikel wird beschrieben, was Containergruppen sind und welche Arten von Szenarien damit möglich sind.

## <a name="what-is-a-container-group"></a>Was ist eine Containergruppe?

Eine Containergruppe ist eine Sammlung mit Containern, die auf demselben Hostcomputer geplant werden. Für die Container einer Containergruppe werden der Lebenszyklus, die Ressourcen, das lokale Netzwerk und die Speichervolumes gemeinsam genutzt. Dies ähnelt dem *Pod*-Konzept in [Kubernetes][kubernetes-pod].

Das folgende Diagramm zeigt ein Beispiel für eine Containergruppe, die mehrere Container enthält:

![Diagramm zu Containergruppen][container-groups-example]

Für diese Beispielcontainergruppe gilt Folgendes:

* Sie wird auf einem einzelnen Hostcomputer geplant.
* Ihr ist eine DNS-Namensbezeichnung zugewiesen.
* Sie macht eine einzelne öffentliche IP-Adresse mit einem Port verfügbar.
* Sie besteht aus zwei Containern. Ein Container lauscht über Port 80 und der andere über Port 5000.
* Sie enthält zwei Azure-Dateifreigaben als Volumebereitstellungen, und jeder Container stellt eine der Freigaben lokal bereit.

> [!NOTE]
> Gruppen mit mehreren Containern unterstützen aktuell nur Linux-Container. Für Windows-Container unterstützt Azure Container Instances nur die Bereitstellung einer einzelnen Instanz. Bis alle Features auch für Windows-Container verfügbar sind, finden Sie die aktuellen Plattformunterschiede in der [Dienstübersicht](container-instances-overview.md#linux-and-windows-containers).

## <a name="deployment"></a>Bereitstellung

Zwei gebräuchliche Möglichkeiten zum Bereitstellen einer Gruppe mit mehreren Containern sind eine [Resource Manager-Vorlage][resource-manager template] oder eine [YAML-Datei][yaml-file]. Verwenden Sie eine Resource Manager-Vorlage, wenn Sie bei der Bereitstellung von Containerinstanzen zusätzliche Azure-Dienstressourcen (z.B. eine [Azure Files-Freigabe][azure-files]) bereitstellen müssen. Das YAML-Format ist präziser, daher wird eine YAML-Datei empfohlen, wenn Ihre Bereitstellung nur Containerinstanzen enthält. Details zu den Eigenschaften, die Sie festlegen können, finden Sie in der Referenzdokumentation zu [Resource Manager-Vorlagen](/azure/templates/microsoft.containerinstance/containergroups) oder [YAML](container-instances-reference-yaml.md).

Mit dem Azure CLI-Befehl [az container export][az-container-export] können Sie die Konfiguration einer Containergruppe in eine YAML-Datei exportieren, um diese beizubehalten. Durch den Export können Sie die Konfiguration einer Containergruppe in der Versionskontrolle für „Konfiguration als Code“ speichern. Oder Sie können die exportierte Datei als Ausgangspunkt bei der Entwicklung einer neuen Konfiguration in YAML verwenden.



## <a name="resource-allocation"></a>Ressourcenzuteilung

Azure Container Instances weist einer Gruppe mit mehreren Containern Ressourcen wie CPUs, Arbeitsspeicher und optional [GPUs][gpus] (Vorschau) zu, indem die [Ressourcenanforderungen][resource-requests] der Instanzen in der Gruppe hinzugefügt werden. Wenn Sie zum Beispiel bei CPU-Ressourcen eine Containergruppe mit zwei Instanzen erstellen, von denen jede 1 CPU erfordert, werden der Containergruppe 2 CPUs zugeordnet.

### <a name="resource-usage-by-instances"></a>Ressourcenverwendung durch Instanzen

Jeder Containerinstanz in einer Gruppe sind die Ressourcen zugeordnet, die in der jeweiligen Ressourcenanforderung angegeben sind. Die maximalen Ressourcen, die von einer Instanz in einer Gruppe verwendet werden, können sich jedoch unterscheiden, wenn Sie die optionale Eigenschaft [Ressourcenlimit][resource-limits] konfigurieren. Das Ressourcenlimit einer Instanz muss größer als oder gleich der obligatorischen Eigenschaft [Ressourcenanforderung][resource-requests] sein.

* Wenn Sie kein Ressourcenlimit angeben, ist die maximale Ressourcenverwendung der Instanz identisch mit der Ressourcenanforderung.

* Wenn Sie ein Limit für eine Instanz angeben, kann die maximale Auslastung der Instanz bis zu dem von Ihnen festgelegten Limit größer sein als die Anforderung. Dementsprechend kann die Ressourcenverwendung durch andere Instanzen in der Gruppe abnehmen. Als maximales Ressourcenlimit für eine Instanz können Sie die Gesamtmenge der Ressourcen festlegen, die der Gruppe zugeordnet sind.
    
In einer Gruppe mit zwei Instanzen, die jeweils 1 CPU anfordern, kann für einen Container beispielsweise eine Workload ausgeführt werden, die mehr CPUs für die Ausführung erfordert, als der andere Container aufweist.

In diesem Szenario könnten Sie einen Ressourcengrenzwert von 2 CPUs für die Instanz festlegen. Diese Konfiguration erlaubt es dem Container, bis zu 2 CPUs voll auszunutzen (falls vorhanden).

### <a name="minimum-and-maximum-allocation"></a>Minimale und maximale Zuordnung

* Ordnen Sie einer Containergruppe **mindestens** 1 CPU und 1 GB Arbeitsspeicher zu. Einzelne Containerinstanzen in einer Gruppe können mit weniger als 1 CPU und 1 GB Arbeitsspeicher bereitgestellt werden. 

* Informationen zur **maximalen** Zuordnung von Ressourcen in einer Containergruppe finden Sie unter [Ressourcenverfügbarkeit][region-availability] für Azure Container Instances in der Bereitstellungsregion.

## <a name="networking"></a>Netzwerk

Containergruppen können eine extern ansprechbare IP-Adresse und einen Portnamespace dieser IP-Adresse gemeinsam nutzen. Sie müssen den Port unter der IP-Adresse und für den Container verfügbar machen, um es externen Clients zu ermöglichen, einen Container in der Gruppe zu erreichen. Da Container in der Gruppe einen Portnamespace gemeinsam nutzen, wird die Portzuordnung nicht unterstützt. 

Innerhalb einer Containergruppe können sich Containerinstanzen über localhost auf jedem Port gegenseitig erreichen, auch wenn diese Ports nicht extern über die IP-Adresse der Gruppe oder aus dem Container verfügbar gemacht werden.

Durch die optionale Bereitstellung von Containergruppen in einem [virtuellen Azure-Netzwerk][virtual-network] (Vorschau) können Container sicher mit anderen Ressourcen im virtuellen Netzwerk kommunizieren.

## <a name="storage"></a>Storage

Sie können externe Volumes für die Bereitstellung in einer Containergruppe angeben. Sie können diese Volumes bestimmten Pfaden in den einzelnen Containern einer Gruppe zuordnen.

## <a name="common-scenarios"></a>Häufige Szenarios

Gruppen mit mehreren Containern sind nützlich, wenn Sie eine einzelne funktionale Aufgabe auf eine kleine Anzahl von Containerimages aufteilen wollen. Diese Images können dann von verschiedenen Teams bereitgestellt werden und separate Ressourcenanforderungen aufweisen.

Beispiel für Verwendung:

* Ein Container, über den eine Webanwendung bereitgestellt wird, und ein Container, der den aktuellen Inhalt aus der Quellcodeverwaltung abruft.
* Ein Anwendungscontainer und ein Protokollierungscontainer. Mit dem Protokollierungscontainer werden die Protokolle und Metriken gesammelt, die von der Hauptanwendung ausgegeben werden, und in den Langzeitspeicher geschrieben.
* Ein Anwendungscontainer und ein Überwachungscontainer. Der Überwachungscontainer sendet regelmäßig eine Anforderung an die Anwendung, um sicherzustellen, dass sie ausgeführt wird und richtig reagiert. Andernfalls wird eine Warnung ausgelöst.
* Ein Front-End-Container und ein Back-End-Container. Das Front-End kann als Webanwendung dienen und das Back-End einen Dienst zum Abrufen von Daten ausführen. 

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das Bereitstellen einer Gruppe mit mehreren Containern per Azure Resource Manager-Vorlage.

> [!div class="nextstepaction"]
> [Bereitstellen einer Containergruppe][resource-manager template]

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/

<!-- LINKS - Internal -->
[resource-manager template]: container-instances-multi-container-group.md
[yaml-file]: container-instances-multi-container-yaml.md
[region-availability]: container-instances-region-availability.md
[resource-requests]: /rest/api/container-instances/containergroups/createorupdate#resourcerequests
[resource-limits]: /rest/api/container-instances/containergroups/createorupdate#resourcelimits
[resource-requirements]: /rest/api/container-instances/containergroups/createorupdate#resourcerequirements
[azure-files]: container-instances-volume-azure-files.md
[virtual-network]: container-instances-vnet.md
[gpus]: container-instances-gpu.md
[az-container-export]: /cli/azure/container#az-container-export
