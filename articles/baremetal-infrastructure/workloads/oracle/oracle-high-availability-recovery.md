---
title: Wiederherstellen Ihrer Oracle-Datenbank in der Azure-Bare-Metal-Infrastruktur
description: Erfahren Sie, wie Sie Ihre Oracle-Datenbank in der Azure-Bare-Metal-Infrastruktur wiederherstellen können.
ms.topic: reference
ms.subservice: workloads
ms.date: 04/15/2021
ms.openlocfilehash: fc7464474d01314a52a77e0f28b1df160a9f42a0
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107589769"
---
# <a name="recover-your-oracle-database-on-azure-baremetal-infrastructure"></a>Wiederherstellen Ihrer Oracle-Datenbank in der Azure-Bare-Metal-Infrastruktur

Auch wenn eine einzelne Technologie nicht vor allen Fehlerszenarien schützt, bietet die Kombination von Features Datenbankadministratoren die Möglichkeit, ihre Datenbank in nahezu allen Situationen wiederherstellen zu können.

## <a name="causes-of-database-failure"></a>Ursachen für Datenbankfehler

Datenbankfehler können aus vielen Gründen auftreten, lassen sich aber in der Regel in verschiedene Kategorien unterteilen:

- Datenbearbeitungsfehler
- Verlust von Onlinewiederholungsprotokollen
- Verlust von Datenbanksteuerungsdateien
- Verlust von Datenbankdatendateien
- Beschädigung physischer Daten

## <a name="choose-your-method-of-recovery"></a>Auswählen der Wiederherstellungsmethode

Der Wiederherstellungstyp hängt vom Typ des Fehlers ab. Angenommen, ein Objekt wird gelöscht, oder Daten werden fälschlicherweise geändert. Die schnellste Lösung besteht dann in der Regel in einem Flashback-Datenbankvorgang. In anderen Fällen kann die Wiederherstellung über eine Azure NetApp Files-Momentaufnahme das Mittel der Wahl sein. Die Entscheidungsstruktur in der folgenden Abbildung stellt häufige Fehler- und Wiederherstellungsszenarien dar, wenn alle oben beschriebenen Datenschutzoptionen implementiert sind.

:::image type="content" source="media/oracle-high-availability/db-recovery-decision-tree.png" alt-text="Diagramm der Entscheidungsstruktur für die Datenbankwiederherstellung." lightbox="media/oracle-high-availability/db-recovery-decision-tree.png":::

Beachten Sie, dass diese Beispielentscheidungsstruktur nur aus Sicht eines Datenbankadministrators dargestellt wird. Jede Bereitstellung kann unterschiedliche Anforderungen aufweisen, durch die sich die Reihenfolge der Optionen ändern kann. Beispielsweise kann der Wechsel einer Datenbankrolle zu einer anderen Region über Data Guard negative Auswirkungen auf die Anwendungsleistung haben. So erhält die Wiederherstellungsmethode durch Momentaufnahmen möglicherweise einen niedrigeren RTO-Wert (Recovery Time Objective). Um sicherzustellen, dass die RTO-/RPO-Anforderungen erfüllt werden, empfehlen wir Ihnen, diese Vorgänge auszuführen und dokumentierte Vorgehensweisen zu erstellen, um sie bei Bedarf auszuführen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Bare-Metal-Infrastruktur:

- [Worum handelt es sich bei der Bare-Metal-Infrastruktur in Azure?](../../concepts-baremetal-infrastructure-overview.md)
- [Herstellen von Verbindungen mit Bare-Metal-Infrastrukturinstanzen in Azure](../../connect-baremetal-infrastructure.md)
