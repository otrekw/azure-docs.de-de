---
title: 'Tutorial: Skalieren einer privaten Cloud'
description: In diesem Tutorial wird das Azure-Portal verwendet, um eine private Azure VMware Solution-Cloud zu skalieren.
ms.topic: tutorial
ms.date: 03/13/2021
ms.openlocfilehash: 2129a3f5d04311883369b7b708689a13f07ec118
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2021
ms.locfileid: "103463601"
---
# <a name="tutorial-scale-an-azure-vmware-solution-private-cloud"></a>Tutorial: Skalieren einer privaten Azure VMware Solution-Cloud

Skalieren Sie die Cluster und Hosts so, dass sie Ihren Bedarf für geplante Workloads optimal erfüllen, um den größtmöglichen Nutzen aus der privaten Azure VMware Solution-Cloud zu ziehen. Sie können die Cluster und Hosts in einer privaten Cloud gemäß dem Bedarf Ihrer Anwendungsworkload skalieren. Leistungs- und Verfügbarkeitseinschränkungen für bestimmte Dienste sollten von Fall zu Fall behandelt werden. Informationen zu den Cluster- und Hostgrenzwerten finden Sie im [Konzeptartikel zur privaten Cloud](concepts-private-clouds-clusters.md).

In diesem Tutorial verwenden Sie das Azure-Portal zu folgenden Zwecken:

> [!div class="checklist"]
> * Hinzufügen eines Clusters zu einer vorhandenen privaten Cloud
> * Hinzufügen von Hosts zu einem vorhandenen Cluster

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen eine vorhandene private Cloud für dieses Tutorial. Wenn Sie noch keine private Cloud erstellt haben, verwenden Sie die Anleitung [Erstellen einer privaten Cloud](tutorial-create-private-cloud.md), um eine private Cloud zu erstellen. 

## <a name="add-a-new-cluster"></a>Hinzufügen eines neuen Clusters

1. Wählen Sie in der Übersicht einer vorhandenen privaten Cloud unter **Verwalten** die Option **Scale private cloud** (Private Cloud skalieren) aus. Wählen Sie als Nächstes **+ Add a cluster** (+ Cluster hinzufügen) aus.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss2-select-add-cluster.png" alt-text="Auswählen von „Add a cluster“ (Cluster hinzufügen)" border="true":::

1. Wählen Sie auf der Seite **Cluster hinzufügen** mithilfe des Schiebereglers die Anzahl von Hosts aus. Wählen Sie **Speichern** aus.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss3-configure-new-cluster.png" alt-text="Wählen Sie auf der Seite „Cluster hinzufügen“ mithilfe des Schiebereglers die Anzahl von Hosts aus. Wählen Sie „Speichern“ aus." border="true":::

   Die Bereitstellung des neuen Clusters beginnt.

## <a name="scale-a-cluster"></a>Skalieren eines Clusters 

1. Wählen Sie in der Übersicht einer vorhandenen privaten Cloud die Option **Scale private cloud** (Private Cloud skalieren) und anschließend das Stiftsymbol aus, um den Cluster zu bearbeiten.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss4-select-scale-private-cloud-2.png" alt-text="Auswählen von „Scale private cloud“ (Private Cloud skalieren) in der Übersicht" border="true":::

1. Wählen Sie auf der Seite **Cluster bearbeiten** mithilfe des Schiebereglers die Anzahl von Hosts aus. Wählen Sie **Speichern** aus.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss5-scale-cluster.png" alt-text="Wählen Sie auf der Seite „Cluster bearbeiten“ mithilfe des Schiebereglers die Anzahl von Hosts aus. Wählen Sie „Speichern“ aus." border="true":::

   Die Hosts werden dem Cluster hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

Sollten Sie eine weitere private Azure VMware Solution-Cloud benötigen, [erstellen Sie eine weitere private Cloud](tutorial-create-private-cloud.md), und verwenden Sie dabei die gleichen Netzwerkvoraussetzungen, Cluster-/Hostgrenzwerte usw.

<!-- LINKS - external-->

<!-- LINKS - internal -->
