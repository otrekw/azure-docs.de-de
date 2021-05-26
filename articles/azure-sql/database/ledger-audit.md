---
title: Azure SQL-Datenbank – Überwachen von Ereignissen mit ledgerfähigen Tabellen
description: Übersicht über die Überwachungsfunktionen von Azure SQL-Datenbank-Ledger
ms.custom: ''
ms.date: 05/25/2021
ms.service: sql-database
ms.subservice: security
ms.reviewer: vanto
ms.topic: conceptual
author: JasonMAnderson
ms.author: janders
ms.openlocfilehash: eafca78dc22a01784eee79293fb7a2ec798818ad
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386366"
---
# <a name="azure-sql-database-audit-events-with-ledger-enabled-tables"></a>Azure SQL-Datenbank – Überwachen von Ereignissen mit ledgerfähigen Tabellen

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Der Azure SQL-Datenbank-Ledger ist zurzeit als **Public Preview** (Öffentliche Vorschau) verfügbar.

Bei der Ausführung forensischer Aktivitäten mit ledgerfähigen Tabellen werden den SQL-Überwachungsprotokollen – über die in der Ledgeransicht und im Datenbankledger erfassten Daten hinaus – zusätzliche Aktions-IDs hinzugefügt.  In der folgenden Tabelle sind diese neuen Überwachungsprotokollierungsereignisse zusammen mit den Bedingungen aufgeführt, die die Ereignisse auslösen.

## <a name="enable-ledger"></a>Ledger aktivieren

| Spalte | Wert |
|--|--|
| **action_id** | ENLR |
| **name** | ENABLE LEDGER  |
| **class_desc** | OBJECT |
| **covering_action_desc** | NULL |
| **parent_class_desc** | DATABASE |
| **covering_parent_action_name** | LEDGER_OPERATION_GROUP |
| **configuration_level** | NULL |
| **configuration_group_name** | LEDGER_OPERATION_GROUP |
| **action_in_log** | 1 |

**Bedingung, die das Ereignis auslöst**: Erstellen einer neuen Ledgertabelle oder Konvertieren einer regulären Tabelle in eine Ledgertabelle.

## <a name="alter-ledger"></a>Ledger ändern

| Spalte | Wert |
|--|--|
| **action_id** | ALLR |
| **name** | ALTER LEDGER |
| **class_desc** | OBJECT |
| **covering_action_desc** | NULL |
| **parent_class_desc** | DATABASE |
| **covering_parent_action_name** | LEDGER_OPERATION_GROUP |
| **configuration_level** | NULL |
| **configuration_group_name** | LEDGER_OPERATION_GROUP |
| **action_in_log** | 1 |

**Bedingung,die das Ereignis auslöst**: Löschen oder Umbenennen einer Ledgertabelle, Konvertieren einer Ledgertabelle in eine normale Tabelle, Hinzufügen, Löschen oder Umbenennen einer Spalte in einer Ledgertabelle.


## <a name="generate-ledger-digest"></a>Ledgerdigest generieren

| Spalte | Wert |
|--|--|
| **action_id** | GDLR |
| **name** | GENERATE LEDGER DIGEST |
| **class_desc** | DATABASE |
| **covering_action_desc** | LEDGER_OPERATION_GROUP |
| **parent_class_desc** | SERVER |
| **covering_parent_action_name** | LEDGER_OPERATION_GROUP |
| **configuration_level** | NULL |
| **configuration_group_name** | LEDGER_OPERATION_GROUP  |
| **action_in_log** | 1 |

**Bedingung, die das Ereignis auslöst**: Generieren eines Ledgerdigests.

## <a name="verify-ledger"></a>Ledger überprüfen

| Spalte | Wert |
|--|--|
| **action_id** | VFLR |
| **name** | VERIFY LEDGER |
| **class_desc** | DATABASE |
| **covering_action_desc** | LEDGER_OPERATION_GROUP |
| **parent_class_desc** | SERVER |
| **covering_parent_action_name** | LEDGER_OPERATION_GROUP |
| **configuration_level** | NULL |
| **configuration_group_name** | LEDGER_OPERATION_GROUP |
| **action_in_log** | 1 |

**Bedingung, die das Ereignis auslöst**: Überprüfen eines Ledgerdigests.

## <a name="ledger-operation-group"></a>Ledger-Vorgangsgruppe

| Spalte | Wert |
|--|--|
| **action_id** | OPLR |
| **name** | LEDGER_OPERATION_GROUP |
| **class_desc** | DATABASE |
| **covering_action_desc** | NULL |
| **parent_class_desc** | SERVER |
| **covering_parent_action_name** | NULL |
| **configuration_level** | GROUP |
| **configuration_group_name** | LEDGER_OPERATION_GROUP |
| **action_in_log** | 0 |

**Bedingung, die das Ereignis auslöst**: n. v.

| Spalte | Wert |
|--|--|
| **action_id** | OPLR |
| **name** | LEDGER_OPERATION_GROUP |
| **class_desc** | SERVER |
| **covering_action_desc** | NULL |
| **parent_class_desc** | NULL |
| **covering_parent_action_name** | NULL |
| **configuration_level** | GROUP |
| **configuration_group_name** | LEDGER_OPERATION_GROUP |
| **action_in_log** | 0 |

**Bedingung, die das Ereignis auslöst**: n. v. 

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen von Azure SQL-Datenbank und Azure Synapse Analytics](auditing-overview.md)
- [Azure SQL-Datenbank-Ledger – Übersicht](ledger-overview.md)
- [Schnellstart: Erstellen einer Azure SQL-Datenbank mit aktiviertem Ledger](ledger-create-a-single-database-with-ledger-enabled.md)
