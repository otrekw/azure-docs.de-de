---
title: 'Apache Spark in Azure Synapse Analytics: Grundlegende Konzepte'
description: Dieser Artikel enthält eine Einführung in Apache Spark in Azure Synapse Analytics sowie in die verschiedenen Konzepte.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: spark
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: euang
ms.openlocfilehash: aa1b4287899aaff9ede4832e999603773d79d28c
ms.sourcegitcommit: 3988965cc52a30fc5fed0794a89db15212ab23d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2020
ms.locfileid: "85194126"
---
# <a name="apache-spark-in-azure-synapse-analytics-core-concepts"></a>Grundlegende Konzepte für Apache Spark in Azure Synapse Analytics

Apache Spark ist ein Framework für die Parallelverarbeitung, das In-Memory-Verarbeitung unterstützt, um die Leistung von Big Data-Analyseanwendungen zu steigern. Apache Spark in Azure Synapse Analytics ist eine der cloudbasierten Apache Spark-Implementierungen von Microsoft. 

Azure Synapse vereinfacht das Erstellen und Konfigurieren von Spark-Funktionen in Azure. Azure Synapse bietet eine andere Implementierung der hier dokumentierten Spark-Funktionen.

## <a name="spark-pools-preview"></a>Spark-Pools (Vorschauversion)

Ein Spark-Pool (Vorschauversion) wird im Azure-Portal erstellt. Bei der Instanziierung eines Spark-Pools wird die Definition des Pools verwendet, um eine Spark-Instanz für die Datenverarbeitung zu erstellen. Wenn ein Spark-Pool erstellt wird, ist er nur in Form von Metadaten vorhanden. Es werden keine Ressourcen verbraucht, ausgeführt oder in Rechnung gestellt. Ein Spark-Pool verfügt über eine Reihe von Eigenschaften zur Steuerung der Merkmale einer Spark-Instanz. Zu diesen Merkmalen zählen unter anderem der Name, die Größe, das Skalierungsverhalten und die Gültigkeitsdauer.

Da im Zusammenhang mit der Erstellung von Spark-Pools keine Gebühren oder Ressourcenkosten entstehen, kann eine beliebige Anzahl mit beliebig vielen verschiedenen Konfigurationen erstellt werden. Auf Spark-Pools können auch Berechtigungen angewendet werden, um zu steuern, auf welche Pools Benutzer zugreifen können.

Eine bewährte Methode besteht darin, kleinere Spark-Pools für die Entwicklung und das Debugging und anschließend größere Spark-Pools für die Ausführung von Produktionsworkloads zu erstellen.

Informationen zum Erstellen eines Spark-Pools sowie alle verfügbaren Eigenschaften finden Sie unter [Erstellen eines Apache Spark-Pools](../quickstart-create-apache-spark-pool-portal.md).

## <a name="spark-instances"></a>Spark-Instanzen

Spark-Instanzen werden erstellt, wenn Sie eine Verbindung mit einem Spark-Pool herstellen, eine Sitzung erstellen und einen Auftrag ausführen. Da mehrere Benutzer Zugriff auf einen einzelnen Spark-Pool haben können, wird für jeden Benutzer, der eine Verbindung herstellt, eine neue Spark-Instanz erstellt. 

Wenn Sie einen zweiten Auftrag übermitteln und Kapazität im Pool sowie in der Spark-Instanz verfügbar ist, wird der Auftrag von der bereits vorhandenen Instanz verarbeitet. Ist dagegen nur Kapazität auf der Poolebene verfügbar, wird eine neue Spark-Instanz erstellt.

## <a name="examples"></a>Beispiele

### <a name="example-1"></a>Beispiel 1

- Sie erstellen einen Spark-Pool namens „SP1“ mit einer festen Clustergröße von 20 Knoten.
- Sie übermitteln einen Notebookauftrag (J1), der zehn Knoten beansprucht. Daraufhin wird eine Spark-Instanz (SI1) erstellt, um den Auftrag zu verarbeiten.
- Anschließend übermitteln Sie einen weiteren Auftrag (J2), der zehn Knoten beansprucht, und da noch Kapazität im Pool und in der Instanz verfügbar ist, wird „J2“ von „SI1“ verarbeitet.
- Wären für „J2“ elf Knoten erforderlich gewesen, hätte die Kapazität in „SP1“ oder „SI1“ nicht ausgereicht. In diesem Fall gilt: Wenn „J2“ von einem Notebook stammt, wird der Auftrag abgelehnt. Wenn „J2“ aus einem Batchauftrag stammt, wird er in die Warteschlange eingereiht.

### <a name="example-2"></a>Beispiel 2

- Sie erstellen einen Spark-Pool namens „SP2“ mit automatischer Skalierung zwischen zehn und 20 Knoten.
- Sie übermitteln einen Notebookauftrag (J1), der zehn Knoten beansprucht. Daraufhin wird eine Spark-Instanz (SI1) erstellt, um den Auftrag zu verarbeiten.
- Anschließend übermitteln Sie einen weiteren Auftrag (J2), der zehn Knoten beansprucht, und da noch Kapazität im Pool verfügbar ist, wird die Instanz zur Verarbeitung von „J2“ automatisch auf 20 Knoten vergrößert.

### <a name="example-3"></a>Beispiel 3

- Sie erstellen einen Spark-Pool namens „SP1“ mit einer festen Clustergröße von 20 Knoten.
- Sie übermitteln einen Notebookauftrag (J1), der zehn Knoten beansprucht. Daraufhin wird eine Spark-Instanz (SI1) erstellt, um den Auftrag zu verarbeiten.
- Ein anderer Benutzer (U2) übermittelt einen Auftrag (J3), der zehn Knoten beansprucht, und es wird eine neue Spark-Instanz (SI2) erstellt, um den Auftrag zu verarbeiten.
- Anschließend übermitteln Sie einen weiteren Auftrag (J2), der zehn Knoten beansprucht, und da noch Kapazität im Pool und in der Instanz verfügbar ist, wird „J2“ von „SI1“ verarbeitet.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
- [Apache Spark-Dokumentation](https://spark.apache.org/docs/2.4.4/)
