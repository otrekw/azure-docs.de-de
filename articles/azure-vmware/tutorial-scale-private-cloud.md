---
title: 'Tutorial: Skalieren einer privaten Cloud'
description: In diesem Tutorial wird das Azure-Portal verwendet, um eine Vorschauversion einer privaten AVS-Cloud (Azure VMware Solution, Azure-VMware-Lösung) zu skalieren.
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: 70d8c1c555badd6102f4b2547492bdea54e55783
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82739725"
---
# <a name="tutorial-scale-an-azure-vmware-solution-avs-preview-private-cloud"></a>Tutorial: Skalieren einer privaten AVS-Cloud (Vorschauversion)

Skalieren Sie die Cluster und Hosts so, dass sie Ihren Bedarf für geplante Workloads optimal erfüllen, um den größtmöglichen Nutzen aus der Vorschauversion Ihrer privaten AVS-Cloud (Azure VMware Solution, Azure-VMware-Lösung) zu ziehen. Da Ihre lokale vCenter-Instanz während der Vorschauphase nicht von AVS unterstützt wird, müssen Sie die Elemente verwenden, die Sie bereits über das Azure-Portal erstellt haben.

Sie können die Anzahl von Clustern und Hosts in einer privaten Cloud gemäß dem Bedarf Ihrer Anwendungsworkload skalieren. Leistungs- und Verfügbarkeitseinschränkungen für bestimmte Dienste müssen von Fall zu Fall in Ihrer AVS-Cloudumgebung (Vorschauversion) behandelt werden. Informationen zu den Cluster- und Hostgrenzwerten in einer privaten Cloud finden Sie im [Konzeptartikel zur privaten Cloud](concepts-private-clouds-clusters.md).

In diesem Tutorial werden über das Azure-Portal folgende Aktionen ausgeführt:

> [!div class="checklist"]
> * Hinzufügen eines Clusters zu einer vorhandenen privaten Cloud
> * Hinzufügen von Hosts zu einem vorhandenen Cluster

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird eine private Cloud benötigt. Sollten Sie noch keine private Cloud erstellt haben, verwenden Sie das [Tutorial zum Erstellen einer privaten Cloud](tutorial-create-private-cloud.md), um eine private Cloud zu erstellen, und konfigurieren Sie das Netzwerk für Ihre virtuelle VMware-Cloud in Azure, um das erforderliche virtuelle Netzwerk einzurichten.

## <a name="add-a-new-cluster"></a>Hinzufügen eines neuen Clusters

Wählen Sie in der Übersicht einer vorhandenen privaten Cloud unter **Verwalten** die Option **Scale private cloud** (Private Cloud skalieren) aus. Wählen Sie als Nächstes **+ Add a cluster** (+ Cluster hinzufügen) aus.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss2-select-add-cluster.png" alt-text="Auswählen von „Add a cluster“ (Cluster hinzufügen)" border="true":::

Wählen Sie auf der Seite **Cluster hinzufügen** mithilfe des Schiebereglers die Anzahl von Hosts aus. Wählen Sie **Speichern** aus.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss3-configure-new-cluster.png" alt-text="Konfigurieren eines neuen privaten Cloudclusters" border="true":::

Die Bereitstellung des neuen Clusters beginnt.

## <a name="scale-a-cluster"></a>Skalieren eines Clusters 

Wählen Sie in der Übersicht einer vorhandenen privaten Cloud die Option **Scale private cloud** (Private Cloud skalieren) und anschließend das Stiftsymbol aus, um den Cluster zu bearbeiten.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss4-select-scale-private-cloud-2.png" alt-text="Auswählen von „Scale private cloud“ (Private Cloud skalieren) in der Übersicht" border="true":::

Wählen Sie auf der Seite **Cluster bearbeiten** mithilfe des Schiebereglers die Anzahl von Hosts aus. Wählen Sie **Speichern** aus.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss5-scale-cluster.png" alt-text="Konfigurieren eines neuen privaten Cloudclusters" border="true":::

Die Hosts werden dem Cluster hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

Sollten Sie eine weitere private AVS-Cloud benötigen, [erstellen Sie eine weitere private Cloud](tutorial-create-private-cloud.md), und verwenden Sie dabei die gleichen Netzwerkvoraussetzungen, Cluster-/Hostgrenzwerte usw.

<!-- LINKS - external-->

<!-- LINKS - internal -->
