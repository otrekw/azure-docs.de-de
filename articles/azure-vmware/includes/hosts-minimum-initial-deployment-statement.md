---
title: Minimale anfängliche Hostbereitstellung
description: Die Bereitstellung muss mindestens drei Hosts umfassen.
ms.topic: include
ms.date: 04/27/2021
ms.openlocfilehash: 72f0ad6c02c7ef24301276f1c6f68b300c86c0a4
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108126307"
---
<!-- Used in production-ready-deployment-steps.md and concepts-private-clouds-clusters.md -->


Für jede erstellte private Cloud gibt es standardmäßig ein vSAN-Cluster. Sie können Cluster hinzufügen, löschen und skalieren.  Pro Cluster sind mindestens drei Hosts erforderlich. Später können nacheinander weitere Host hinzugefügt werden. Pro Cluster sind bis zu 16 Hosts möglich. Pro privater Cloud sind maximal zwölf Cluster möglich.  Die erste Bereitstellung von Azure VMware Solution verfügt über drei Hosts. 

Testcluster stehen zu Bewertungszwecken zur Verfügung und sind auf drei Hosts begrenzt. Pro privater Cloud gibt es einen einzelnen Testcluster. Sie können einen Testcluster während des Evaluierungszeitraums unter Verwendung eines einzelnen Hosts skalieren.

Sie verwenden vSphere und NSX-T Manager, um die meisten weiteren Aspekte der Clusterkonfiguration oder des Clusterbetriebs zu verwalten. Der gesamte lokale Speicher jedes Hosts in einem Cluster befindet sich unter der Kontrolle von vSAN.

>[!TIP]
>Sie können den Cluster später jederzeit erweitern und zusätzliche Cluster hinzufügen, wenn Sie die anfängliche Bereitstellungsanzahl erhöhen möchten.