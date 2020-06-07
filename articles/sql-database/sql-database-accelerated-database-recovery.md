---
title: Verbesserte Wiederherstellung von Datenbanken
description: Die Azure SQL-Datenbank verfügt über ein neues Feature, das eine schnelle und einheitliche Datenbankwiederherstellung, den sofortigen Transaktionsrollback und eine aggressive Protokollkürzung für Einzel- und Pooldatenbanken in Azure SQL-Datenbank sowie Datenbanken in Azure SQL Data Warehouse ermöglicht.
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 05/19/2020
ms.openlocfilehash: f259d423f465d93031c3a72855fd7aac4e320573
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684958"
---
# <a name="accelerated-database-recovery"></a>Verbesserte Wiederherstellung von Datenbanken

Die **beschleunigte Datenbankwiederherstellung (Accelerated Database Recovery, ADR)** ist ein Feature der SQL-Datenbank-Engine. Damit wird die Datenbankverfügbarkeit aufgrund eines neuen Entwurfs des Wiederherstellungsprozesses der SQL-Datenbank-Engine erheblich verbessert, insbesondere bei zeitintensiven Transaktionen. ADR ist derzeit für die Azure SQL-Datenbank-Typen „Einzeldatenbank“, „Pool für elastische Datenbanken“ und „Verwaltete Instanz“ sowie für Datenbanken in Azure SQL Data Warehouse (derzeit in der Vorschau) verfügbar. Die Hauptvorteile der schnellen Datenbankwiederherstellung sind:

- **Schnelle und konsistente Datenbankwiederherstellung**

  Mit ADR haben zeitintensive Transaktionen keinen Einfluss auf die gesamte Wiederherstellungszeit und ermöglichen eine schnelle und konsistente Wiederherstellung der Datenbank, unabhängig von der Anzahl der aktiven Transaktionen im System und deren Größe.

- **Sofortiges Transaktionsrollback**

  Mit ADR erfolgt das Transaktionsrollback sofort, unabhängig davon, wann die Transaktion aktiv war oder wie viele Updates ausgeführt wurden.

- **Drastische Protokollkürzung**

  Durch ADR wird das Transaktionsprotokoll auch bei aktiven, zeitintensiven Transaktionen konsequent verkürzt, sodass der Vorgang kontrollierbar bleibt.

## <a name="standard-database-recovery-process"></a>Standardprozess zur Datenbankwiederherstellung

Die Datenbankwiederherstellung in SQL Server basiert auf dem [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf)-Wiederherstellungsmodell und umfasst drei Phasen. Diese Phasen sind im folgenden Diagramm dargestellt und werden im darauffolgenden Diagramm ausführlicher erläutert.

![aktueller Wiederherstellungsprozess](./media/sql-database-accelerated-database-recovery/current-recovery-process.png)

- **Die Analysephase**

  Vorwärts-Scan des Transaktionsprotokolls ab dem Beginn des letzten erfolgreichen Prüfpunkts (oder der ältesten modifizierten Seiten-LSN) bis zum Ende, um den Zustand jeder Transaktion zum Beendigungszeitpunkt von SQL Server zu ermitteln.

- **Die Rollforwardphase**

  Vorwärts-Scan des Transaktionsprotokolls von der ältesten Transaktion ohne Commit bis zum Ende, um die Datenbank in den Zustand zum Zeitpunkt des Absturzes zu versetzen, indem alle Commit-Vorgänge erneut durchgeführt werden.

- **Die Rollbackphase**

  Für jede Transaktion, die zum Zeitpunkt des Absturzes aktiv war, wird das Protokoll in umgekehrter Richtung durchlaufen, um die von dieser Transaktion durchgeführten Vorgänge rückgängig zu machen.

Basierend auf diesem Entwurf ist die Zeit, die das SQL-Datenbankmodul für die Wiederherstellung nach einem unerwarteten Neustart benötigt, (ungefähr) proportional zur Größe der Transaktion, die zum Zeitpunkt des Absturzes im System am längsten aktiv war. Die Wiederherstellung erfordert ein Rollback aller unvollständigen Transaktionen. Die erforderliche Zeitspanne verhält sich proportional zum Umfang der von der Transaktion ausgeführten Aufgaben und der Dauer ihrer Aktivität. Daher kann der SQL Server-Wiederherstellungsprozess bei zeitintensiven Transaktionen (z. B. umfangreichen Vorgänge bei Masseneinfügungen und Indexerstellungen für eine große Tabelle) sehr lange dauern.

Auch das Abbrechen bzw. Durchführen eines Rollbacks für eine Transaktion kann bei diesem Entwurf sehr lange dauern, da die gleiche Rückgängig/Wiederherstellen-Phase wie oben beschrieben genutzt wird.

Darüber hinaus kann die SQL-Datenbank-Engine das Transaktionsprotokoll nicht kürzen, wenn Transaktionen mit langer Laufzeit vorhanden sind, da die entsprechenden Protokolldatensätze für die Wiederherstellungs- und Rollbackprozesse benötigt werden. Aufgrund dieses Designs der SQL-Datenbank-Engine hatten einige Kunden das Problem, dass das Transaktionsprotokoll sehr groß wird und sehr viel Festplattenspeicher belegt.

## <a name="the-accelerated-database-recovery-process"></a>Prozess für die schnellere Datenbankwiederherstellung

Mit ADR wird auf die obigen Probleme reagiert, indem der Wiederherstellungsprozess des SQL-Datenbankmoduls vollständig neu entworfen wird, um Folgendes zu erreichen:

- Wählen Sie eine konstante Zeit bzw. die umgehende Wiederherstellung, indem Sie vermeiden, dass das Protokoll vom/zum Anfang der ältesten aktiven Transaktion gescannt werden muss. Bei ADR wird das Transaktionsprotokoll nur ab dem letzten erfolgreichen Prüfpunkt verarbeitet (bzw. ab der Protokollfolgenummer (Log Sequence Number, LSN) der ältesten modifizierten Seite). Dadurch wird die Wiederherstellungszeit nicht durch zeitintensive Transaktionen beeinflusst.
- Minimieren Sie den erforderlichen Speicherplatz für das Transaktionsprotokoll, da das Protokoll für die gesamte Transaktion nicht mehr verarbeitet werden muss. Folglich kann das Transaktionsprotokoll beim Auftreten von Prüfpunkten und Sicherungen konsequent abgeschnitten werden.

In allgemeiner Hinsicht wird die schnelle Datenbankwiederherstellung mit ADR erreicht, indem alle physischen Datenbankänderungen mit Versionsangaben versehen werden und nur logische Vorgänge rückgängig gemacht werden, die begrenzt sind und für die das nahezu sofortige Rückgängigmachen möglich ist. Alle Transaktionen, die zum Zeitpunkt des Absturzes aktiv waren, werden als „Abgebrochen“ gekennzeichnet. Daher können Versionen, die von diesen Transaktionen generiert werden, von gleichzeitigen Benutzerabfragen ignoriert werden.

Die schnelle Datenbankwiederherstellung (ADR) besteht aus den gleichen drei Phasen wie der aktuelle Wiederherstellungsprozess. Wie diese Phasen für ADR durchgeführt werden, ist im folgenden Diagramm dargestellt und wird im darauffolgenden Diagramm ausführlicher erläutert.

![ADR-Wiederherstellungsprozess](./media/sql-database-accelerated-database-recovery/adr-recovery-process.png)

- **Die Analysephase**

  Der Prozess bleibt derselbe, wird jedoch durch die Rekonstruktion von sLog und das Kopieren von Protokolldatensätzen für nicht versionierte Vorgänge ergänzt.
  
- **Die Rollforwardphase**

  Aufgeteilt in zwei Phasen (P)
  - Phase 1

      Rollforward vom sLog (älteste Transaktion ohne Commit bis zum letzten Prüfpunkt). Dies ist ein schneller Vorgang, da nur wenige Datensätze aus dem sLog verarbeitet werden müssen.
      
  - Phase 2

     Die Wiederholung über das Transaktionsprotokoll beginnt ab dem letzten Prüfpunkt (anstatt ab der ältesten Transaktion ohne Commit).
     
- **Die Rollbackphase**

   Die Phase „Rückgängig“ mit ADR wird nahezu sofort durchgeführt, indem sLog genutzt wird, um Vorgänge ohne Versionsangabe und persistenten Versionsspeicher mit logischer Wiederherstellung rückgängig zu machen. Auf diese Weise erfolgt das versionsbasierte Rückgängigmachen auf Zeilenebene.

## <a name="adr-recovery-components"></a>Komponenten der ADR-Wiederherstellung

Die vier wichtigsten Komponenten von ADR sind:

- **Persistenter Versionsspeicher**

  Der persistente Versionsspeicher ist ein neuer Mechanismus der SQL-Datenbank-Engine, mit dem die in der Datenbank generierten Zeilenversionen persistent gemacht werden, anstatt im herkömmlichen `tempdb`-Versionsspeicher. Mit dem persistenten Versionsspeicher wird die Ressourcenisolation ermöglicht und die Verfügbarkeit von lesbaren sekundären Replikaten verbessert.

- **Logische Wiederherstellung**

  Die logische Wiederherstellung ist der asynchrone Prozess zur Durchführung des Rückgängigmachens basierend auf der Zeilenebenenversion – mit sofortigem Transaktionsrollback und Rückgängig-Vorgang für alle Vorgänge mit Versionsangabe. Die logische Wiederherstellung erfolgt wie folgt:

  - Alle abgebrochenen Transaktionen werden nachverfolgt und für andere Transaktionen als unsichtbar markiert. 
  - Es wird ein Rollback mithilfe von PVS für alle Benutzertransaktionen ausgeführt, anstatt das Transaktionsprotokoll physisch zu scannen und die Änderungen einzeln rückgängig zu machen.
  - Nach dem Abbruch der Transaktion werden alle Sperren sofort aufgehoben. Da der Abbruch das einfache Markieren von Änderungen im Arbeitsspeicher beinhaltet, ist der Prozess sehr effizient, sodass die Sperren nicht sehr lange aufrechterhalten werden müssen.

- **sLog**

  sLog ist ein sekundärer In-Memory-Protokolldatenstrom, der Protokolldatensätze für nicht versionierte Vorgänge (wie z.B. die Invalidierung von Metadatencaches oder das Einrichten von Sperren) speichert. Merkmale des sLog:

  - Geringe Datenmenge und wenig Speicherbedarf
  - Persistent auf Datenträgern durch Serialisierung während des Prüfpunktverfahrens
  - Wird beim Commit von Transaktionen periodisch gekürzt
  - Beschleunigt das Rollforward und Rollback durch ausschließliche Verarbeitung von nicht versionierten Vorgängen  
  - Ermöglicht ein konsequentes Abschneiden des Transaktionsprotokolls, wobei nur die erforderlichen Protokolldatensätze beibehalten werden

- **Bereinigung**

  Die Bereinigung ist ein asynchroner Prozess, der periodisch reaktiviert wird und nicht benötigte Seitenversionen bereinigt.

## <a name="accelerated-database-recovery-patterns"></a>Muster bei der beschleunigten Datenbankwiederherstellung

Die folgenden Arten von Workloads profitieren am meisten von ADR:

- Workloads mit zeitintensiven Transaktionen.
- Workloads, bei denen aktive Transaktionen mitunter dazu führen, dass die Transaktionsprotokolle stark anwachsen.  
- Workloads, bei denen es vorgekommen ist, dass die Datenbank länger nicht verfügbar war, da die SQL Server-Wiederherstellung lange gedauert hat (wie bei einem unerwarteten SQL Server-Neustart oder einem manuellen Transaktionsrollback).

