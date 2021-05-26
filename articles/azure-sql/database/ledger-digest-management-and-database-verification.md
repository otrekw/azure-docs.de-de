---
title: Digest-Verwaltung und Datenbanküberprüfung
description: Dieser Artikel enthält Informationen zum Digest der Ledger-Datenbank und zur Datenbanküberprüfung in Azure SQL-Datenbank
ms.custom: ''
ms.date: 05/25/2021
ms.service: sql-database
ms.subservice: security
ms.reviewer: vanto
ms.topic: conceptual
author: JasonMAnderson
ms.author: janders
ms.openlocfilehash: cb5c0d4b6a569e93052c2923b609935cd3934ec8
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386330"
---
# <a name="digest-management-and-database-verification"></a>Digest-Verwaltung und Datenbanküberprüfung

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Der Azure SQL-Datenbank-Ledger ist zurzeit als **öffentliche Vorschauversion** verfügbar.

Azure SQL-Datenbank Ledger bietet eine Form der Datenintegrität namens Forward-Integrity, die einen Beweis für die Manipulation von Daten in Ihren Ledger-Tabellen liefert. Wenn beispielsweise eine Banktransaktion in einer Ledger-Tabelle auftritt, in der ein Saldo auf den Wert `x` aktualisiert wurde, wird diese Manipulationsaktivität durch die Datenbanküberprüfung erkannt, wenn ein Angreifer die Daten später ändert und den Saldo von `x` in `y` ändert.  

Der Datenbank-Überprüfungsprozess verwendet einen oder mehrere zuvor generierte Datenbank Digests als Eingabe und berechnet die im Datenbank-Ledger gespeicherten Hashes – basierend auf dem aktuellen Status der Ledger-Tabellen – neu. Wenn die berechneten Hashes nicht mit den Eingabe-Digests übereinstimmen, schlägt die Überprüfung fehl (was auf eine Manipulation der Daten hinweist) und meldet alle erkannten Inkonsistenzen.

## <a name="database-digests"></a>Datenbank-Digests

Der Hash des letzten Blocks im Datenbank-Ledger wird als „Datenbank-Digest“ bezeichnet und stellt den Status aller Ledger-Tabellen in der Datenbank zum Zeitpunkt der Blockgenerierung dar. Das Generieren eines Datenbank-Digests ist effizient, da es nur das Berechnen der Hashes der Blöcke umfasst, die vor Kurzem angefügt wurden. Datenbank-Digests können entweder automatisch vom System oder manuell vom Benutzer generiert und später zum Überprüfen der Datenintegrität der Datenbank verwendet werden. Datenbank-Digests werden in Form eines JSON-Dokuments generiert, das den Hash des letzten Blocks zusammen mit Metadaten zur Block-ID enthält. Die Metadaten enthalten den Zeitpunkt der Generierung des Digests und den Commit-Zeitstempel der letzten Transaktion in diesem Block.

Der Überprüfungsprozess und die Integrität der Datenbank hängen von der Integrität der Eingabe-Digests ab. Zu diesem Zweck müssen Datenbank-Digests, die aus der Datenbank extrahiert werden, in einem vertrauenswürdigen Speicher gespeichert werden, die nicht von den Benutzern oder Angreifern mit hohen Berechtigungen des Azure SQL-Datenbank-Servers manipuliert werden können.

### <a name="automatic-generation-and-storage-of-database-digests"></a>Automatisches Generieren und Speichern von Datenbank-Digests

Azure SQL-Datenbank Ledger lässt sich in [unveränderlichen Speicher für Azure Blob Storage](../../storage/blobs/storage-blob-immutable-storage.md) und [Azure Confidential Ledger](/azure/confidential-ledger/) integrieren und bietet sichere Speicherdienste in Azure, um die Datenbank-Digests vor potenziellen Manipulationen zu schützen. Diese Integration bietet Benutzern eine einfache und kostengünstige Möglichkeit, die Digest-Verwaltung zu automatisieren, ohne sich um ihre Verfügbarkeit und geografische Replikation kümmern zu müssen. 

Das Konfigurieren des automatischen Generierens und Speichern von Datenbank-Digests kann entweder über das Azure-Portal, PowerShell oder Azure CLI erfolgen. Bei der Konfiguration werden Datenbank-Digests in einem vordefinierten Intervall von 30 Sekunden generiert und in den ausgewählten Speicherdienst hochgeladen. Wenn im System im 30-Sekunden-Intervall keine Transaktionen auftreten, wird kein Datenbank-Digest generiert und hochgeladen. Dadurch wird sichergestellt, dass Datenbank-Digests nur generiert werden, wenn Daten in Ihrer Datenbank aktualisiert wurden.

:::image type="content" source="media/ledger/automatic-digest-management.png" alt-text="Aktivieren von Digest-Speicher"::: 

> [!IMPORTANT]
> Nach der Bereitstellung sollte eine [Unveränderlichkeitsrichtlinie](../../storage/blobs/storage-blob-immutability-policies-manage.md) für Ihren Container konfiguriert werden, um sicherzustellen, dass die Datenbank-Digests vor Manipulationen geschützt sind.

### <a name="manual-generation-and-storage-of-database-digests"></a>Manuelles Generieren und Speichern von Datenbank-Digests

Das Azure SQL-Datenbank Ledger ermöglicht es Benutzern auch, bei Bedarf einen Datenbank-Digest zu generieren, sodass sie den Digest manuell in jedem Dienst oder jedes Gerät speichern können, der bzw. das sie als vertrauenswürdiges Speicherziel betrachten, z. B. in einem lokalen Write-Once-Read-Many-Gerät (WORM). Das manuelle Generieren eines Datenbank-Digests erfolgt durch Ausführen der gespeicherten [sys.sp_generate_database_ledger_digest](/sql/relational-databases/system-stored-procedures/sys-sp-generate-database-ledger-digest-transact-sql)-Prozedur in [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) oder [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio).

> [!IMPORTANT]
> Zum Generieren von Datenbank-Digests ist die **GENERATE LEDGER DIGEST**-Berechtigung erforderlich. Weitere Informationen zu Berechtigungen im Zusammenhang mit Ledger-Tabellen finden Sie unter [Berechtigungen](/sql/relational-databases/security/permissions-database-engine#asdbpermissions). 

```sql
EXECUTE sp_generate_database_ledger_digest
```

Das zurückgegebene Ergebnisset ist eine einzelne Datenzeile, die wie folgt als JSON-Dokument am vertrauenswürdigen Speicherort gespeichert werden sollte:

```json
    {
        "database_name":  "ledgerdb",
        "block_id":  0,
        "hash":  "0xDC160697D823C51377F97020796486A59047EBDBF77C3E8F94EEE0FFF7B38A6A",
        "last_transaction_commit_time":  "2020-11-12T18:01:56.6200000",
        "digest_time":  "2020-11-12T18:39:27.7385724"
    }
```

## <a name="database-verification"></a>Datenbanküberprüfung

Der Überprüfungsprozess scannt alle Ledger- und Verlaufstabellen und berechnet die SHA-256-Hashes ihrer Zeilen neu und vergleicht sie mit den Datenbank-Digest-Dateien, die an die gespeicherte Überprüfungsprozedur übergeben werden. Bei großen Ledger-Tabellen kann die Datenbanküberprüfung ein ressourcenintensiver Prozess sein und sollte nur ausgeführt werden, wenn Benutzer die Integrität ihrer Datenbank überprüfen müssen. Sie kann stündlich oder täglich ausgeführt werden, wenn die Integrität der Datenbank häufig überwacht werden muss oder nur, wenn die Organisation, die die Daten hostet, eine Überwachung durchläuft und kryptografische Beweise in Bezug auf die Integrität ihrer Daten bereitstellen muss. Zur Senkung der Überprüfungskosten macht Ledger Optionen verfügbar, um einzelne Ledger-Tabellen oder nur eine Teilmenge des Ledgers zu überprüfen. 

Die Datenbanküberprüfung erfolgt über zwei gespeicherte Prozeduren, je nachdem, ob ein [automatischer Digest-Speicher](#database-verification-using-automatic-digest-storage) verwendet wird oder ob [Digests manuell vom Benutzer verwaltet werden](#database-verification-using-manual-digest-storage).

> [!IMPORTANT]
> Für die Datenbanküberprüfung ist die Berechtigung **VIEW LEDGER CONTENT** erforderlich. Weitere Informationen zu Berechtigungen im Zusammenhang mit Ledger-Tabellen finden Sie unter [Berechtigungen](/sql/relational-databases/security/permissions-database-engine#asdbpermissions). 

### <a name="database-verification-using-automatic-digest-storage"></a>Datenbanküberprüfung mithilfe der automatischen Digest-Speicherung

Wenn Sie den automatischen Digest-Speicher zum Generieren und Speichern von Datenbank-Digests verwenden, befindet sich der Speicherort des Digest-Speichers in der Systemkatalogsicht [sys.database_ledger_digest_locations](/sql/relational-databases/system-catalog-views/sys-database-ledger-digest-locations-transact-sql) als JSON-Objekte. Das Ausführen der Datenbanküberprüfung besteht aus der Ausführung der [sp_verify_database_ledger_from_digest_storage](/sql/relational-databases/system-stored-procedures/sys-sp-verify-database-ledger-from-digest-storage-transact-sql) gespeicherten Systemprozedur, wobei die JSON-Objekte aus  der [sys.database_ledger_digest_locations](/sql/relational-databases/system-catalog-views/sys-database-ledger-digest-locations-transact-sql)-Systemkatalogsicht angegeben werden, in der Datenbank-Digests für die Speicherung konfiguriert sind. 

Mithilfe des automatischen Digest-Speichers können Sie den Speicherort während des gesamten Lebenszyklus der Ledger-Tabellen ändern.  Wenn Sie beispielsweise Azure Immutable Blob Storage verwenden, um Ihre Digest-Dateien zu speichern, aber später stattdessen Azure Confidential Ledger verwenden möchten, können Sie dies tun. Diese Änderung des Speicherorts wird in [sys.database_ledger_digest_locations](/sql/relational-databases/system-catalog-views/sys-database-ledger-digest-locations-transact-sql)gespeichert. Um die Ausführung der Überprüfung zu vereinfachen, wenn mehrere Digest-Speicherorte verwendet wurden, ruft das folgende Skript die Speicherorte der Digests ab und führt die Überprüfung mit diesen Speicherorten aus.

```sql
DECLARE @digest_locations NVARCHAR(MAX) = (SELECT * FROM sys.database_ledger_digest_locations FOR JSON AUTO, INCLUDE_NULL_VALUES);
SELECT @digest_locations as digest_locations;
BEGIN TRY
    EXEC sys.sp_verify_database_ledger_from_digest_storage @digest_locations;
    SELECT 'Ledger verification succeeded.' AS Result;
END TRY
BEGIN CATCH
    THROW;
END CATCH
```

### <a name="database-verification-using-manual-digest-storage"></a>Datenbanküberprüfung mithilfe der automatischen Digest-Speicherung

Wenn Sie manuellen Digest-Sspeicher zum Generieren und Speichern von Datenbank-Digests verwenden, wird die folgende gespeicherte Prozedur verwendet, um das Ledger zu überprüfen und den JSON-Inhalt des Digests in der gespeicherten Prozedur anzufügen. Beim Ausführen der Datenbanküberprüfung können Sie auswählen, ob alle Tabellen in der Datenbank oder bestimmte Tabellen überprüft werden sollen. Im Folgenden finden Sie die Syntax für die gespeicherte [sp_verify_database_ledger](/sql/relational-databases/system-stored-procedures/sys-sp-verify-database-ledger-transact-sql)-Prozedur:

```sql
sp_verify_database_ledger <JSON_document_containing_digests>, <table_name> 
```

Im Folgenden finden Sie ein Beispiel für die Ausführung der gespeicherten [sp_verify_database_ledger](/sql/relational-databases/system-stored-procedures/sys-sp-verify-database-ledger-transact-sql)-Prozedur, indem zwei Digests zur Überprüfung übergeben werden: 

```sql
EXECUTE sp_verify_database_ledger N'
[
    {
        "database_name":  "ledgerdb",
        "block_id":  0,
        "hash":  "0xDC160697D823C51377F97020796486A59047EBDBF77C3E8F94EEE0FFF7B38A6A",
        "last_transaction_commit_time":  "2020-11-12T18:01:56.6200000",
        "digest_time":  "2020-11-12T18:39:27.7385724"
    },
    {
        "database_name":  "ledgerdb",
        "block_id":  1,
        "hash":  "0xE5BE97FDFFA4A16ADF7301C8B2BEBC4BAE5895CD76785D699B815ED2653D9EF8",
        "last_transaction_commit_time":  "2020-11-12T18:39:35.6633333",
        "digest_time":  "2020-11-12T18:43:30.4701575"
    }
]
```

Rückgabecodes für `sp_verify_database_ledger` und `sp_verify_database_ledger_from_digest_storage` sind `0` (**Erfolg**) oder `1` (**Fehler**).

## <a name="next-steps"></a>Nächste Schritte

- [Azure SQL-Datenbank-Ledger – Übersicht](ledger-overview.md)
- [Aktualisierbare Ledger-Tabellen](ledger-updatable-ledger-tables.md)   
- [Ledger-Tabellen, die nur Anfügevorgänge unterstützen](ledger-append-only-ledger-tables.md)   
- [Datenbank-Ledger](ledger-database-ledger.md)   
