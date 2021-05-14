---
title: Minimale anfängliche Hostbereitstellung
description: Die Bereitstellung muss mindestens drei Hosts umfassen.
ms.topic: include
ms.date: 04/28/2021
ms.openlocfilehash: 49eb2fdd105b02a52d0cba555a78e0d44d133bf6
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108179533"
---
<!-- Used in production-ready-deployment-steps.md and concepts-private-clouds-clusters.md -->

Für jede erstellte private Cloud gibt es standardmäßig ein vSAN-Cluster. Sie können Cluster hinzufügen, löschen und skalieren.  Die Mindestanzahl von Hosts pro Cluster und bei der anfänglichen Bereitstellung beträgt drei. 

Testcluster stehen zu Bewertungszwecken zur Verfügung und sind auf drei Hosts begrenzt. Es gibt einen einzelnen Testcluster pro Private Cloud, in der Sie ihn während des Evaluierungszeitraums um einen einzelnen Host skalieren können.

Sie verwenden vSphere und NSX-T Manager, um die meisten weiteren Aspekte der Clusterkonfiguration oder des Clusterbetriebs zu verwalten. Der gesamte lokale Speicher jedes Hosts in einem Cluster befindet sich unter der Kontrolle von vSAN.

>[!TIP]
>Sie können den Cluster später jederzeit erweitern und zusätzliche Cluster hinzufügen, wenn Sie die anfängliche Bereitstellungsanzahl erhöhen möchten.
