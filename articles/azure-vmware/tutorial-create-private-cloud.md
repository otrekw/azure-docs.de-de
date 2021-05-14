---
title: 'Tutorial: Bereitstellen einer privaten Azure VMware Solution-Cloud'
description: Hier erfahren Sie, wie Sie eine private Azure VMware Solution-Cloud erstellen und bereitstellen.
ms.topic: tutorial
ms.date: 04/23/2021
ms.openlocfilehash: cdbd00473890e22c08ebf57f7c6f54f6eef188bb
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107945801"
---
# <a name="tutorial-deploy-an-azure-vmware-solution-private-cloud"></a>Tutorial: Bereitstellen einer privaten Azure VMware Solution-Cloud

Die private Azure VMware Solution-Cloud ermöglicht das Bereitstellen eines vSphere-Clusters in Azure. Für jede erstellte private Cloud gibt es standardmäßig ein vSAN-Cluster. Sie können Cluster hinzufügen, löschen und skalieren.  Pro Cluster sind mindestens drei Hosts erforderlich. Später können nacheinander weitere Host hinzugefügt werden. Pro Cluster sind bis zu 16 Hosts möglich. Pro privater Cloud sind maximal vier Cluster möglich.  Die erste Bereitstellung von Azure VMware Solution verfügt über drei Hosts. 

Testcluster stehen zu Bewertungszwecken zur Verfügung und sind auf drei Hosts begrenzt. Pro privater Cloud gibt es einen einzelnen Testcluster. Sie können einen Testcluster während des Evaluierungszeitraums unter Verwendung eines einzelnen Hosts skalieren.

Sie verwenden vSphere und NSX-T Manager, um die meisten weiteren Aspekte der Clusterkonfiguration oder des Clusterbetriebs zu verwalten. Der gesamte lokale Speicher jedes Hosts in einem Cluster befindet sich unter der Kontrolle von vSAN.

>[!TIP]
>Sie können den Cluster später jederzeit erweitern und zusätzliche Cluster hinzufügen, wenn Sie die anfängliche Bereitstellungsanzahl erhöhen möchten.

Da Sie Ihre private Cloud zunächst nicht mit Ihrer lokalen vCenter-Instanz verwalten können, sind zusätzliche Konfigurationsschritte erforderlich. Diese Verfahren und die damit zusammenhängenden Voraussetzungen werden in diesem Tutorial behandelt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer privaten Cloud von Azure VMware Solution
> * Vergewissern, dass die private Cloud bereitgestellt wurde

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Geeignete Administratorrechte und Berechtigung zum Erstellen einer privaten Cloud. Sie müssen mindestens die Rolle „Mitwirkender“ im Abonnement haben.
- Verwenden Sie die Informationen, die Sie im Artikel [Planen der Azure VMware Solution-Bereitstellung](production-ready-deployment-steps.md) gesammelt haben:
- Vergewissern Sie sich, dass das Netzwerk ordnungsgemäß konfiguriert ist, wie unter [Checkliste für die Netzwerkplanung für Azure VMware Solution](tutorial-network-checklist.md) beschrieben.
- Hosts wurden bereitgestellt, und der Ressourcenanbieter „Microsoft.AVS“ wurde registriert wie unter [Anfordern des Hostkontingents und Aktivieren von Azure VMware Solution](enable-azure-vmware-solution.md) beschrieben.

## <a name="create-a-private-cloud"></a>Erstellen einer privaten Cloud

[!INCLUDE [create-avs-private-cloud-azure-portal](includes/create-private-cloud-azure-portal-steps.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer privaten Cloud von Azure VMware Solution
> * Vergewissern, dass die private Cloud bereitgestellt wurde
> * Löschen einer privaten Azure VMware Solution-Cloud

Im nächsten Tutorial erfahren Sie, wie Sie eine Jumpbox erstellen. Die Jumpbox wird verwendet, um eine Verbindung mit Ihrer Umgebung herzustellen, damit Sie Ihre private Cloud lokal verwalten können.


> [!div class="nextstepaction"]
> [Tutorial: Hier erfahren Sie, wie Sie auf eine private Azure VMware Solution-Cloud zugreifen.](tutorial-access-private-cloud.md)
