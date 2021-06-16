---
title: SnapCenter-Integration für Oracle in BareMetal-Infrastruktur
description: Erfahren Sie, wie Sie Momentaufnahmetechnologien von Oracle und NetApp verwenden, um Sicherungen für die operative Wiederherstellung für Oracle-Datenbanken in einer BareMetal-Infrastruktur zu erstellen.
ms.topic: how-to
ms.subservice: baremetal-oracle
ms.date: 05/05/2021
ms.openlocfilehash: d9cf6933287167da82b1a5ad4ab8848d5c34d404
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110579887"
---
# <a name="snapcenter-integration-for-oracle-on-baremetal-infrastructure"></a>SnapCenter-Integration für Oracle in BareMetal-Infrastruktur

In dieser Schrittanleitung erfahren Sie, wie Sie Momentaufnahmetechnologien von Oracle und NetApp zum Erstellen von Sicherungen für die operative Wiederherstellung für Oracle-Datenbanken verwenden. Die Verwendung von Momentaufnahmen ist nur einer von mehreren Ansätzen zum Schutz von Daten für Oracle. Momentaufnahmen können Downtime und Datenverluste minimieren, wenn eine Oracle-Datenbank in der BareMetal-Infrastruktur ausgeführt wird. 

>[!IMPORTANT]
>SnapCenter unterstützt Oracle für SAP-Anwendungen, bietet jedoch keine Integration von SAP BR\*Tools.

Obwohl Oracle zwei verschiedene Sicherungsmethoden für Momentaufnahmen bietet, unterstützt SnapCenter von NetApp nur eine Methode: den Sicherungsmodus „Heiß“. Der Sicherungsmodus „Heiß“ ist die herkömmliche Version des Sicherns und Erstellens von Oracle-Momentaufnahmen. Er erfordert eine Interaktion mit dem Oracle-Host, um die Datenbank vorübergehend in einen Sicherungsmodus „Heiß“ zu versetzen, um die Archivprotokolle ordnungsgemäß zu katalogisieren. Der Sicherungsmodus „Heiß“ ermöglicht auch eine bessere Interaktion mit der RMAN-Datenbank, um die verfügbaren Momentaufnahmen für die Wiederherstellung genauer nachzuverfolgen. 

Die Artikel in diesem Leitfaden führen Sie durch die Erstellung eines operativen Frameworks für die Wiederherstellung und Notfallwiederherstellung für Oracle im Sicherungsmodus „Heiß“. Die Notfallwiederherstellung ist die Wiederherstellung einer Datenbank oder eines Teils einer Datenbank nach einem Notfall. Ein Beispiel hierfür ist ein fehlerhaftes Laufwerk oder eine beschädigte Datenbank. Bei der operativen Wiederherstellung ist der Grund für die Wiederherstellung kein Notfall. Ein Beispiel hierfür ist der Verlust einiger Datenzeilen, die Ihr Unternehmen nicht beeinträchtigen.

## <a name="snapcenter-database-organization"></a>SnapCenter-Datenbankorganisation
SnapCenter organisiert Datenbanken in Ressourcengruppen. Eine Ressourcengruppe kann eine oder mehrere Datenbanken mit derselben Schutzrichtlinie sein. Daher müssen Sie keine einzelnen Volumes auswählen, die Teil der Sicherung sind.

:::image type="content" source="media/netapp-snapcenter-integration-oracle-baremetal/snapcenter-database-resource-group.png" alt-text="Diagramm, das die Organisation von SnapCenter-Datenbanken in Ressourcengruppen zeigt" border="false":::

## <a name="oracle-disaster-recovery-architecture"></a>Architektur der Oracle-Notfallwiederherstellung

Oracle-Sicherungen im Modus „Heiß“ sind in zwei verschiedene Sicherungen unterteilt, um ein Rollforward mithilfe von Archivprotokollen nach dem Wiederherstellen der Datendateien auszuführen. Der Schutz der Datendateien und Steuerungsdateien erfolgt „stündlich“, aber eine längere Sicherungshäufigkeit ist akzeptabel. Je länger die Intervalle zwischen den Sicherungen sind, desto länger ist die Wiederherstellungszeit.  

SnapCenter erstellt lokale Momentaufnahmen der Datenbank an den Standardspeicherorten für Datendateien (nfs01). Momentaufnahmen werden auf jedem Dateisystem erstellt, das entweder Datendateien oder Steuerungsdateien hostet. Diese Sicherungen stellen die schnelle Wiederherstellung der Datenbank sicher. Es werden keine Daten bei einem Ausfall mehrerer Datenträger oder bei einem Standortfehler geschützt. 

Die Anzahl der stündlich erstellten Momentaufnahmen hängt von den festgelegten Sicherungsrichtlinien ab. Eine Oracle-Datenbank verfügt über eine Betriebswiederherstellungsfunktion von 2 bis 5 Tagen aus Momentaufnahmen.
 
Es müssen genügend Archivprotokolle vorhanden sein, und es ist die Ausführung eines Rollforwards der Oracle-Datenbank vom letzten, funktionierenden Wiederherstellungspunkt für Datendateien erforderlich. Verwenden Sie Momentaufnahmen von Archivprotokollen, um die RPO (Recovery Point Objective) für eine Oracle-Datenbank zu verringern. Je kürzer die Abstände von Momentaufnahmen in den Archivprotokollen, desto niedriger ist die RPO. Das empfohlene Momentaufnahmeintervall für die Archivprotokolle beträgt entweder fünf oder 15 Minuten. Das kürzere Intervall von fünf Minuten hat die kürzeste RPO.  Das kürzere Intervall erhöht die Komplexität, da im Rahmen des Wiederherstellungsprozesses mehr Momentaufnahmen verwaltet werden müssen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie NetApp SnapCenter einrichten, um Datenverkehr von Azure an Oracle BareMetal-Infrastrukturserver weiterzuleiten.

> [!div class="nextstepaction"]
> [Einrichten von SnapCenter zum Weiterleiten von Datenverkehr](set-up-snapcenter-to-route-traffic.md)
