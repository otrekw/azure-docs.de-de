---
title: Referenzarchitekturen für das Rendering in Azure
description: Architekturen für die Verwendung von Azure Batch und anderen Azure-Diensten zum Erweitern einer lokalen Render-Farm mittels Cloud Bursting in die Cloud
ms.date: 02/07/2019
ms.topic: how-to
ms.custom: seodec18
ms.openlocfilehash: 784fd89c3dea88e25a2058713897c7a655c8a3af
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "83726518"
---
# <a name="reference-architectures-for-azure-rendering"></a>Referenzarchitekturen für das Rendering in Azure

In diesem Artikel werden Architekturdiagramme auf hoher Ebene für Szenarien zum Erweitern (oder „Bursting“) einer lokalen Render-Farm zu Azure gezeigt. Die Beispiele zeigen verschiedene Optionen für Compute-, Netzwerk- und Speicherdienste in Azure.

## <a name="hybrid-with-nfs-or-cfs"></a>Hybrid mit NFS oder CFS

Das folgende Diagramm zeigt ein Hybridszenario, das die folgenden Azure-Dienste umfasst:

* **Compute**: Azure Batch-Pool oder VM-Skalierungsgruppe.

* **Netzwerk**: Lokal: Azure Expressroute oder VPN. Azure: Azure VNET.

* **Speicher**: Eingabe- und Ausgabedateien: NFS oder CFS mithilfe von Azure-VMs, synchronisiert mit lokalem Speicher über Azure-Dateisynchronisierung oder RSync. Alternative: Avere vFXT für die Ein- oder Ausgabe von Dateien von lokalen NAS-Geräten mithilfe von NFS.

  ![Cloudbursting – hybrid mit NFS oder CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs-avere.png)

## <a name="hybrid-with-blobfuse"></a>Hybrid mit Blobfuse

Das folgende Diagramm zeigt ein Hybridszenario, das die folgenden Azure-Dienste umfasst:

* **Compute**: Azure Batch-Pool oder VM-Skalierungsgruppe.

* **Netzwerk**: Lokal: Azure Expressroute oder VPN. Azure: Azure VNET.

* **Speicher**: Eingabe- und Ausgabedateien: Blobspeicher, eingebunden in Computeressourcen über Azure Blobfuse.

  ![Cloudbursting – hybrid mit Blobfuse](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Hybrides Compute und Speichern

Das folgende Diagramm zeigt ein vollständig verbundenes Hybridszenario für Compute und Speichern und umfasst die folgenden Azure-Dienste:

* **Compute**: Azure Batch-Pool oder VM-Skalierungsgruppe.

* **Netzwerk**: Lokal: Azure Expressroute oder VPN. Azure: Azure VNET.

* **Speicher**: Standortübergreifend: Avere vFXT. Optionale Archivierung von lokalen Dateien mithilfe von Azure Data Box in Blob Storage oder lokal mit Avere FXT für NAS-Beschleunigung.

  ![Cloudbursting – hybrides Compute und Speichern](./media/batch-rendering-architectures/hybrid-compute-storage-avere.png)


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Verwendung von [Render-Managern](batch-rendering-render-managers.md) mit Azure Batch.

* Weitere Informationen zu Optionen für das [Rendering in Azure](batch-rendering-service.md).