---
title: Minimale anfängliche Hostbereitstellung
description: Die Bereitstellung muss mindestens drei Hosts umfassen.
ms.topic: include
ms.date: 04/28/2021
ms.openlocfilehash: 32ed19a8ac3535f407549bea87acb6b433004b02
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111430470"
---
<!-- Used in production-ready-deployment-steps.md and concepts-private-clouds-clusters.md -->

Für jede erstellte private Cloud gibt es standardmäßig ein vSAN-Cluster. Sie können Cluster hinzufügen, löschen und skalieren.  Die Mindestanzahl von Hosts pro Cluster und bei der anfänglichen Bereitstellung beträgt drei. 

Testcluster stehen zu Bewertungszwecken zur Verfügung und sind auf drei Hosts je privater Cloud begrenzt.

Sie verwenden vSphere und NSX-T Manager, um die meisten weiteren Aspekte der Clusterkonfiguration oder des Clusterbetriebs zu verwalten. Der gesamte lokale Speicher jedes Hosts in einem Cluster befindet sich unter der Kontrolle von vSAN.

>[!TIP]
>Sie können den Cluster später jederzeit erweitern und zusätzliche Cluster hinzufügen, wenn Sie die anfängliche Bereitstellungsanzahl erhöhen möchten.
