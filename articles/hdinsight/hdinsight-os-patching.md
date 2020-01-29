---
title: Konfigurieren des Zeitplans für das Patchen des Betriebssystems für Azure HDInsight-Cluster
description: Erfahren Sie, wie der Zeitplan für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster konfiguriert wird.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/21/2020
ms.openlocfilehash: 102ae56bb9dce2898c14bdc710420759a527a9e9
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514696"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Konfigurieren des Zeitplans für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster

> [!IMPORTANT]
> Ubuntu-Images stehen für die Erstellung neuer Azure HDInsight-Cluster innerhalb von drei Monaten nach der Veröffentlichung zur Verfügung. Seit Januar 2019 werden laufende Cluster nicht automatisch gepatcht. Kunden müssen Skriptaktionen oder andere Mechanismen verwenden, um einen laufenden Cluster zu patchen. Neu erstellte Cluster verfügen immer über die aktuellen verfügbaren Updates, einschließlich der neuesten Sicherheitspatches.

HDInsight bietet Unterstützung für die Durchführung allgemeiner Aufgaben in Ihrem Cluster, z. B. Installieren von Betriebssystempatches, Sicherheitsupdates und Neustarts von Knoten. Diese Aufgaben werden mithilfe der folgenden beiden Skripts ausgeführt, die als [Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md) ausgeführt werden können und mit Parametern konfiguriert werden:

- `schedule-reboots.sh` – Ausführen eines sofortigen Neustarts oder Planen eines Neustarts auf den Clusterknoten.
- `install-updates-schedule-reboots.sh` – Installieren aller Updates, nur Installieren von Kernel- und Sicherheitsupdates, oder nur von Kernelupdates.

> [!NOTE]  
> Skriptaktionen wenden nicht automatisch Updates für alle zukünftigen Updatezyklen an. Führen Sie die Skripts immer dann aus, wenn neue Updates angewendet werden müssen, damit die Updates installiert werden, und starten Sie den virtuellen Computer dann neu.

## <a name="preparation"></a>Vorbereitung

Führen Sie vor der Bereitstellung in der Produktionsumgebung einen Patch für eine repräsentative nicht produktive Umgebung aus. Entwickeln Sie einen Plan, um Ihr System vor dem eigentlichen Patchen angemessen zu testen.

Von Zeit zu Zeit können Sie in einer SSH-Sitzung auf Ihrem Cluster eine Nachricht erhalten, dass ein Upgrade verfügbar ist. Die Nachricht kann etwa wie folgt aussehen:

```
New release '18.04.3 LTS' available.
Run 'do-release-upgrade' to upgrade it
```

Das Patchen erfolgt optional und nach eigenem Ermessen.

## <a name="restart-nodes"></a>Neustart von Knoten
  
Mit dem Skript [schedule-reboots](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh) wird der Typ des Neustarts festgelegt, der auf den Computern im Cluster ausgeführt wird. Wenn Sie die Skriptaktion übermitteln, legen Sie sie auf allen drei Knotentypen fest: Hauptknoten, Workerknoten und Zookeeper. Wenn das Skript nicht auf einen Knotentyp angewendet wird, werden die VMs für diesen Knotentyp nicht aktualisiert oder neu gestartet.

`schedule-reboots script` akzeptiert einen numerischen Parameter:

| Parameter | Zulässige Werte | Definition |
| --- | --- | --- |
| Typ des auszuführenden Neustarts | 1 oder 2 | Der Wert 1 aktiviert den Neustart des Zeitplans (geplant in 12 bis 24 Stunden). Der Wert 2 ermöglicht einen sofortigen Neustart (in 5 Minuten). Wenn kein Parameter angegeben wird, ist der Standardwert 1. |  

## <a name="install-updates-and-restart-nodes"></a>Installieren von Updates und Neustarten von Knoten

Das Skript [install-updates-schedule-reboots.sh](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh) bietet Optionen zum Installieren verschiedener Updatetypen und Neustarten des virtuellen Computers.

Das `install-updates-schedule-reboots`-Skript akzeptiert zwei numerische Parameter, wie in der folgenden Tabelle beschrieben:

| Parameter | Zulässige Werte | Definition |
| --- | --- | --- |
| Typ der zu installierenden Updates | 0, 1 oder 2 | Bei einem Wert von 0 werden nur Kernelupdates installiert. Bei einem Wert von 1 werden alle Updates installiert, und 2 installiert nur die Kernel- und Sicherheitsupdates. Wenn kein Parameter angegeben wird, ist der Standardwert 0. |
| Typ des auszuführenden Neustarts | 0, 1 oder 2 | Der Wert 0 deaktiviert den Neustart. Der Wert 1 aktiviert den Neustart des Zeitplans, und 2 aktiviert einen sofortigen Neustart. Wenn kein Parameter angegeben wird, ist der Standardwert 0. Der Benutzer muss den Eingabeparameter 1 in den Eingabeparameter 2 ändern. |

> [!NOTE]
> Sie müssen ein Skript als permanent gespeichert kennzeichnen, nachdem Sie es auf einen vorhandenen Cluster angewendet haben. Andernfalls verwenden durch Skalierungsvorgänge erstellte neue Knoten den Standardpatchzeitplan. Wenn Sie das Skript im Rahmen der Clustererstellung anwenden, wird es automatisch persistent gespeichert.

## <a name="next-steps"></a>Nächste Schritte

Spezielle Schritte zur Verwendung der Skriptaktionen finden Sie in den folgenden Abschnitten unter [Anpassen von Linux-basierten HDInsight-Clustern mithilfe einer Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md):

- [Verwenden einer Skriptaktion während der Clustererstellung](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
- [Anwenden einer Skriptaktion auf einen ausgeführten Cluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
