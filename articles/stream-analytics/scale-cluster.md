---
title: Ändern der Größe eines Azure Stream Analytics-Clusters
description: Hier erfahren Sie, wie Sie die Größe eines Azure Stream Analytics-Clusters hoch- oder herunterskalieren können.
author: sidramadoss
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: overview
ms.custom: mvc
ms.date: 09/22/2020
ms.openlocfilehash: 0763e56de6c72a36d39b17d153db6fc4d7dd821a
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90944921"
---
# <a name="resize-an-azure-stream-analytics-cluster"></a>Ändern der Größe eines Azure Stream Analytics-Clusters

Die Kapazität eines Stream Analytics-Clusters wird in Streamingeinheiten (SU) gemessen. Mehrere Aufträge können parallel im gleichen Cluster ausgeführt werden, solange die Summe von SUs, die allen laufenden Aufträgen zugewiesen sind, die Kapazität des Clusters nicht überschreitet.

Die Kapazität des Clusters kann zentral hoch- und herunterskaliert werden, um diese an die Größe Ihrer Streamingworkloads anzupassen. Das Skalieren eines Clusters nimmt viel Zeit in Anspruch, und die Skalierung ist nicht vorgesehen. Es wird empfohlen, einen Cluster mit der exakten Anzahl von SUs zu planen und bereitzustellen, die Sie nutzen möchten.

## <a name="change-the-scale-of-your-cluster"></a>Ändern der Skalierung des Clusters

1. Suchen Sie im Azure-Portal nach Ihrem Stream Analytics-Cluster, und klicken Sie auf diesen.

1. Klicken Sie im Abschnitt **Übersicht** auf **Skalieren**. Hier können Sie sehen, wie viele SUs Ihrem Cluster zugewiesen sind. Verwenden Sie den Selektor, um die Anzahl der SUs nach Bedarf zu erhöhen oder zu verringern.

   ![Skalieren von Clustern](./media/scale-cluster/scale-cluster.png)

Der Skalierungsvorgang hat keine Auswirkungen auf Aufträge, die zurzeit ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte

Sie wissen jetzt, wie Sie Ihren Stream Analytics-Cluster zentral hoch- und herunterskalieren können. Als Nächstes erfahren Sie mehr über die Verwaltung privater Endpunkte und die automatische Skalierung Ihrer Aufträge:

* [Verwalten privater Endpunkte in einem Azure Stream Analytics-Cluster](private-endpoints.md)
* [Verwalten von Aufträgen in einem Azure Stream Analytics-Cluster](manage-jobs-cluster.md)
