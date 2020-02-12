---
title: Verkleinern einer privaten AVS-Cloud (Azure VMware Solutions)
description: Beschreibt, wie Sie eine private AVS-Cloud verkleinern
author: sharaths-cs
ms.author: b-shsury
ms.date: 07/01/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0ea764081cd0b4d5c6d44cd7364d1e9a89a3cec3
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014265"
---
# <a name="shrink-an-avs-private-cloud"></a>Verkleinern einer privaten AVS-Cloud

AVS bietet die Flexibilität, eine private AVS-Cloud dynamisch zu verkleinern. Eine private AVS-Cloud besteht aus einem oder mehreren vSphere-Clustern. Jeder Cluster kann 3 bis 16 Knoten enthalten. Wenn Sie eine private AVS-Cloud verkleinern, entfernen Sie einen Knoten aus dem bestehenden Cluster oder löschen einen ganzen Cluster. 

## <a name="before-you-begin"></a>Voraussetzungen

Die folgenden Bedingungen müssen vor dem Verkleinern einer privaten AVS-Cloud erfüllt sein. Der Verwaltungscluster (erster Cluster) wird beim Erstellen einer privaten AVS-Cloud erstellt. Er kann nicht gelöscht werden.

* Ein vSphere-Cluster muss aus drei Knoten bestehen. Ein Cluster mit nur drei Knoten kann nicht verkleinert werden.
* Der gesamte Speicherverbrauch darf die Gesamtkapazität nach dem Verkleinern des Clusters nicht überschreiten.
* Überprüfen Sie, ob die DRS-Regeln (Distributed Resource Scheduler) die vMotion-Ausführung eines virtuellen Computers verhindern. Deaktivieren oder löschen Sie die Regeln, wenn Regeln vorhanden sind. DRS-Regeln schließen virtuelle Computer zum Hosten von Affinitätsregeln ein.


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="shrinking-an-avs-private-cloud"></a>Verkleinern einer privaten AVS-Cloud

1. [Greifen Sie auf das AVS-Portal zu](access-cloudsimple-portal.md).

2. Öffnen Sie die Seite **Ressourcen**.

3. Klicken Sie auf die private AVS-Cloud, die Sie verkleinern möchten.

4. Klicken Sie auf der Übersichtsseite auf **Verkleinern**.

    ![Verkleinern der privaten AVS-Cloud](media/shrink-private-cloud.png)

5. Wählen Sie den Cluster aus, den Sie verkleinern oder löschen möchten. 

    ![Verkleinern der privaten AVS-Cloud – Auswählen des Clusters](media/shrink-private-cloud-select-cluster.png)

6. Wählen Sie **Einen Knoten entfernen** oder **Gesamten Cluster löschen** aus. 

7. Überprüfen der Clusterkapazität

8. Klicken Sie auf **Senden**, um die private AVS-Cloud zu verkleinern.

Die Verkleinerung der privaten AVS-Cloud wird gestartet. Sie können den Status unter „Aufgaben“ verfolgen. Der Verkleinerungsprozess kann je nach den Daten, die mit vSAN erneut synchronisiert werden müssen, einige Stunden dauern.

> [!NOTE]
> 1. Wenn Sie eine private Cloud verkleinern, indem Sie den letzten bzw. den einzigen Cluster im Rechenzentrum löschen, wird das Rechenzentrum nicht gelöscht.
> 2. Wenn eine DRS-Regelverletzung auftritt, wird der Knoten nicht aus dem Cluster entfernt und in der Aufgabenbeschreibung wird angezeigt, dass durch das Entfernen eines Knotens die DRS-Regeln im Cluster verletzt werden.    


## <a name="next-steps"></a>Nächste Schritte

* [Nutzen von virtuellen VMware-Computern in Azure](quickstart-create-vmware-virtual-machine.md)
* Informieren Sie sich über [private AVS-Clouds](cloudsimple-private-cloud.md).
