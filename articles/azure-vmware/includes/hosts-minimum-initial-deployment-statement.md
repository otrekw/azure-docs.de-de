---
title: Minimale anfängliche Hostbereitstellung
description: Die Bereitstellung muss mindestens drei Hosts umfassen.
ms.topic: include
ms.date: 04/28/2021
author: shortpatti
ms.author: v-patsho
ms.service: azure-vmware
ms.openlocfilehash: d0efae1a8f3bf30360d2649b9d638f5cc800179f
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "121747612"
---
<!-- Used in plan-private-cloud-deployment.md and concepts-private-clouds-clusters.md -->

Für jede erstellte private Cloud gibt es standardmäßig ein vSAN-Cluster. Sie können Cluster hinzufügen, löschen und skalieren.  Die Mindestanzahl von Hosts pro Cluster und bei der anfänglichen Bereitstellung beträgt drei. 

Testcluster stehen zu Bewertungszwecken zur Verfügung und sind auf drei Hosts je privater Cloud begrenzt.

Sie verwenden vSphere und NSX-T Manager, um die meisten weiteren Aspekte der Clusterkonfiguration oder des Clusterbetriebs zu verwalten. Der gesamte lokale Speicher jedes Hosts in einem Cluster befindet sich unter der Kontrolle von vSAN.

>[!TIP]
>Sie können den Cluster später jederzeit erweitern und zusätzliche Cluster hinzufügen, wenn Sie die anfängliche Bereitstellungsanzahl erhöhen möchten.
