---
title: Fehler „Watchdog BUG soft lockup - CPU“ in einem Azure HDInsight-Cluster
description: Der Fehler „Watchdog BUG soft lockup CPU“ wird in Kernel-Syslog-Protokollen des Azure HDInsight-Clusters angezeigt
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/05/2019
ms.openlocfilehash: d2deb421dce440c1c8f949c858269fed038ac3d5
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112299751"
---
# <a name="scenario-watchdog-bug-soft-lockup---cpu-error-from-an-azure-hdinsight-cluster"></a>Szenario: Fehler „watchdog: BUG: soft lockup - CPU“ in einem Azure HDInsight-Cluster

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Die Kernel-Syslog-Protokolle enthalten die folgende Fehlermeldung: `watchdog: BUG: soft lockup - CPU`.

## <a name="cause"></a>Ursache

Ein [Fehler](https://bugzilla.kernel.org/show_bug.cgi?id=199437) im Linux-Kernel verursacht eine vorübergehende CPU-Blockierung.

## <a name="resolution"></a>Lösung

Wenden Sie einen Kernelpatch an. Das folgende Skript aktualisiert den Linux-Kernel und startet die Computer im Laufe von 24 Stunden zu verschiedenen Zeitpunkten neu. Führen Sie die Skriptaktion in zwei Batches aus. Der erste Batch wird für alle Knoten mit Ausnahme des Hauptknotens ausgeführt. Der zweite Batch wird für den Hauptknoten ausgeführt. Führen Sie die Batches nicht gleichzeitig für den Hauptknoten und die anderen Knoten aus.

1. Navigieren Sie im Azure-Portal zu Ihrem HDInsight-Cluster.

1. Rufen Sie die Skriptaktionen auf.

1. Wählen Sie **Neue übermitteln** aus, und geben Sie Folgendes ein:

    | Eigenschaft | Wert |
    | --- | --- |
    | Skripttyp | -Custom |
    | Name |Fix for kernel soft lock issue |
    | Bash-Skript-URI |`https://raw.githubusercontent.com/hdinsight/hdinsight.github.io/master/ClusterCRUD/KernelSoftLockFix/scripts/KernelSoftLockIssue_FixAndReboot.sh` |
    | Knotentyp(en) |Worker, Zookeeper |
    | Parameter |– |

    Wählen Sie **Speichern Sie diese Skriptaktion...** aus, wenn Sie das Skript beim Hinzufügen neuer Knoten ausführen möchten.

1. Klicken Sie auf **Erstellen**.

1. Warten Sie, bis die Ausführung erfolgreich abgeschlossen wurde.

1. Führen Sie die Skriptaktion für den Hauptknoten aus, indem Sie Schritt 3 erneut ausführen. Dieses Mal jedoch mit dem Hauptknotentyp.

1. Warten Sie, bis die Ausführung erfolgreich abgeschlossen wurde.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../includes/hdinsight-troubleshooting-next-steps.md)]