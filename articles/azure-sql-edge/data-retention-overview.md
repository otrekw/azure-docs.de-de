---
title: Übersicht über die Datenaufbewahrungsrichtlinie – Azure SQL Edge
description: Hier erfahren Sie mehr über die Datenaufbewahrungsrichtlinie in Azure SQL Edge.
keywords: SQL Edge, Datenaufbewahrung
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/04/2020
ms.openlocfilehash: bb059a946c03f41e5b65944eec67070f84ee6b08
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90976346"
---
# <a name="data-retention-overview"></a>Übersicht über die Datenaufbewahrung

Das Erfassen und Speichern von Daten von verbundenen IoT-Geräten ist wichtig für die Förderung und Gewinnung von betrieblichen und geschäftlichen Erkenntnissen. Aufgrund der großen Menge von Daten von diesen Geräten ist es jedoch wichtig, dass Organisationen die Datenmengen, die sie aufbewahren möchten, und deren Granularität sorgfältig planen. Eine größtmögliche Granularität für alle Daten ist zwar wünschenswert, allerdings nicht immer praktikabel. Darüber hinaus ist die Menge von Daten, die aufbewahrt werden können, durch die auf den IoT-Geräten oder Edgegeräten verfügbare Speichermenge begrenzt. 

In Azure SQL Edge können Datenbankadministratoren die Datenaufbewahrungsrichtlinie für eine SQL Edge-Datenbank und deren zugrunde liegende Tabellen definieren. Sobald die Datenaufbewahrungsrichtlinie definiert wurde, wird ein Hintergrundsystemtask ausgeführt, der veraltete (alte) Daten endgültig aus den Benutzertabellen löscht. 

> [!Note]
> Endgültig aus der Tabelle gelöschte Daten können nicht wiederhergestellt werden. Die einzige Möglichkeit, die endgültig gelöschten Daten wiederherzustellen, besteht darin, die Datenbank aus einer älteren Sicherung wiederherzustellen.

Schnellstartanleitungen:

- [Aktivieren und Deaktivieren von Datenaufbewahrungsrichtlinien](data-retention-enable-disable.md)
- [Verwalten von historischen Daten mit einer Aufbewahrungsrichtlinie](data-retention-cleanup.md)

## <a name="how-data-retention-works"></a>Funktionsweise der Datenaufbewahrung

Sie können DDL-Anweisungen verwenden, um die Datenaufbewahrung zu konfigurieren. Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Datenaufbewahrungsrichtlinien](data-retention-enable-disable.md). Zum automatischen Löschen der veralteten Datensätze muss erst die Datenaufbewahrung für die Datenbank und die Tabellen aktiviert werden, die endgültig aus der Datenbank gelöscht werden sollen. 

Nach dem Konfigurieren der Datenaufbewahrung für eine Tabelle wird ein Hintergrundtask ausgeführt, der die veralteten Datensätze in einer Tabelle identifiziert und löscht. Wenn die automatische Bereinigung der Tasks aus irgendeinem Grund nicht ausgeführt wird oder mit den Löschvorgängen nicht Schritt halten kann, kann für die entsprechenden Tabellen ein manueller Bereinigungsvorgang durchgeführt werden. Weitere Informationen zur automatischen und manuellen Bereinigung finden Sie unter [Verwalten von historischen Daten mit einer Aufbewahrungsrichtlinie](data-retention-cleanup.md).

## <a name="limitations-and-restrictions"></a>Einschränkungen

- Wenn die Datenbank aus einer vollständigen Sicherung wiederhergestellt oder erneut angefügt wird, wird die Datenaufbewahrung, sofern sie aktiviert ist, automatisch deaktiviert. 
- Die Datenaufbewahrung kann für temporale Verlaufstabellen nicht aktiviert werden.
- Die Filterspalte für Datenaufbewahrung kann nicht geändert werden. Um die Spalte zu ändern, deaktivieren Sie Datenaufbewahrung für die Tabelle.  

## <a name="next-steps"></a>Nächste Schritte

- [Machine Learning und künstliche Intelligenz mit ONNX in SQL Edge](onnx-overview.md)
- [Entwickeln einer End-to-End-IoT-Lösung mit SQL Edge unter Verwendung von IoT Edge](tutorial-deploy-azure-resources.md)
- [Datenstreaming in Azure SQL Edge](stream-data.md)
