---
title: Richtlinien für den Entwurf von Azure Storage-Tabellen | Microsoft Docs
description: Hier erfahren Sie mehr über Richtlinien zum Erstellen von Azure Storage-Tabellendiensten, um Lese- und Schreibvorgänge effizient zu unterstützen.
services: storage
ms.service: storage
author: tamram
ms.author: tamram
ms.topic: article
ms.date: 04/23/2018
ms.subservice: tables
ms.openlocfilehash: f84707e454a8b1f5d5947478fe65108a142a9757
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88236317"
---
# <a name="guidelines-for-table-design"></a>Richtlinien für den Entwurf von Tabellen

Das Entwerfen von Tabellen für die Verwendung mit dem Azure Storage-Tabellenspeicherdienst unterscheidet sich grundlegend von dem Entwerfen einer relationalen Datenbank. In diesem Artikel werden Richtlinien beschrieben, die Sie beim Entwerfen einer Tabellenspeicherdienstlösung mit effizienten Lese- und Schreibvorgängen unterstützen.

## <a name="design-your-table-service-solution-to-be-read-efficient"></a>Entwerfen Ihrer Tabellenspeicherdienstlösung für größtmögliche Effizienz von Lesevorgängen

* ***Legen Sie den Entwurf für Abfragen in leseintensiven Anwendungen aus.*** Bedenken Sie beim Entwerfen von Tabellen die Abfragen (insbesondere Abfragen mit sensiblem Latenzverhalten), die Sie ausführen werden, bevor Sie überlegen, wie die Entitäten aktualisiert werden sollen. Dies führt in der Regel zu einer effizienten und leistungsstarken Lösung.  
* ***Geben Sie sowohl „PartitionKey „ als auch „RowKey“ in Ihren Abfragen an:** _ _Punktabfragen* wie diese stellen die effizientesten Tabellendienstabfragen dar.  
* ***Erwägen Sie die Speicherung duplizierter Kopien von Entitäten.*** Tabellenspeicher ist günstig. Erwägen Sie daher, ein und dieselbe Entität mehrfach zu speichern (mit verschiedenen Schlüsseln), um effizientere Abfragen zu ermöglichen.  
* ***Erwägen Sie das Denormalisieren der Daten.*** Tabellenspeicher ist günstig. Erwägen Sie daher, Ihre Daten zu denormalisieren. Speichern Sie z. B. zusammengefasste Entitäten, damit Abfragen für zusammengefasste Daten nur auf eine einzelne Entität zugreifen müssen.  
* ***Verwenden Sie zusammengesetzte Schlüsselwerte:** _ *PartitionKey** und **RowKey** sind die einzigen Schlüssel, die Sie verwenden können. Verwenden Sie z. B. Verbundschlüsselwerte, um alternative Schlüsselzugriffspfade für Entitäten zu aktivieren.  
* ***Verwenden Sie die Abfrageprojektion.*** Sie können die Datenmenge reduzieren, die Sie über das Netzwerk übertragen, indem Sie Abfragen verwenden, die nur die benötigten Felder auswählen.  

## <a name="design-your-table-service-solution-to-be-write-efficient"></a>Entwerfen Ihrer Tabellenspeicherdienstlösung für größtmögliche Effizienz von Schreibvorgängen  

* ***Erstellen Sie keine Hot-Partitionen.*** Wählen Sie Schlüssel aus, die es Ihnen ermöglichen, Ihre Anforderungen zu jedem Zeitpunkt auf mehrere Partitionen zu verteilen.  
* ***Vermeiden Sie Lastspitzen.*** Sorgen Sie für einen reibungslosen Datenverkehr über einen angemessenen Zeitraum und vermeiden Sie Lastspitzen.
* ***Erstellen Sie nicht unbedingt eine separate Tabelle für jeden Entitätstyp.*** Wenn atomische Transaktionen über Entitätstypen hinweg erforderlich sind, können Sie diese verschiedenen Entitätstypen in derselben Tabelle in derselben Partition speichern.
* ***Bedenken Sie den maximalen Durchsatz, der erzielt werden muss.*** Sie müssen die Skalierbarkeitsziele für den Tabellenspeicherdienst berücksichtigen und sicherstellen, dass diese durch Ihren Entwurf nicht überschritten werden.  

Beim Durchlesen dieses Handbuch finden Sie Beispiele für die Umsetzung dieser Grundlagen in die Praxis. 

## <a name="next-steps"></a>Nächste Schritte

- [Entwurfsmuster für die Tabelle](table-storage-design-patterns.md)
- [Entwurf für Abfragen](table-storage-design-for-query.md)
- [Verschlüsseln von Tabellendaten](table-storage-design-encrypt-data.md)
- [Entwurf für die Datenänderung](table-storage-design-for-modification.md)
