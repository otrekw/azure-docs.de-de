---
title: Datenreplikation – Azure Database for MySQL Flexible
description: Erfahren Sie mehr über das Verwenden der Datenreplikation zum Synchronisieren von Daten von einem externen Server mit dem Dienst Azure Database for MySQL Flexible.
author: SudheeshGH
ms.author: sunaray
ms.service: mysql
ms.topic: conceptual
ms.date: 06/08/2021
ms.openlocfilehash: 5cc19531f076d3b630faced7fef4ea5cb05be9f1
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111751377"
---
# <a name="replicate-data-into-azure-database-for-mysql-flexible--server-preview"></a>Replizieren von Daten in Azure Database for MySQL Flexible Server (Vorschau)

Die Datenreplikation ermöglicht das Synchronisieren von Daten von einem externen MySQL-Server mit dem Dienst Azure Database for MySQL Flexible. Der externe Server kann lokal, in virtuellen Computern, in Azure Database for MySQL Single Server oder in einem Datenbankdienst vorhanden sein, der von anderen Cloudanbietern gehostet wird. Die Datenreplikation basiert auf der Position der binären Protokolldatei (binlog). Weitere Informationen zur binlog-Replikation finden Sie unter [Binary Log File Position Based Replication Configuration Overview](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html) (Konfiguration der auf der Position der binären Protokolldatei basierenden Replikation – Übersicht).

> [!Note]
> Die GTID-basierte Replikation wird für Azure Database for MySQL Flexible Server derzeit nicht unterstützt. 

## <a name="when-to-use-data-in-replication"></a>Szenarien für die Verwendung der Datenreplikation

Zu den wichtigsten Szenarien, bei denen die Verwendung der Datenreplikation infrage kommt, zählen Folgende:

- **Synchronisierung von Hybriddaten:** Mit der Datenreplikation können Sie Daten zwischen Ihren lokalen Servern und Azure Database for MySQL Flexible Server synchron halten. Eine solche Synchronisierung ist für die Erstellung von Hybridanwendungen nützlich. Diese Methode ist optimal geeignet, wenn Sie über einen lokalen Datenbankserver verfügen, die Daten jedoch in eine Region verschieben möchten, die sich näher bei den Endbenutzern befindet.
- **Synchronisierung von Daten in verschiedenen Clouds:** Verwenden Sie bei komplexen Cloudlösungen die Datenreplikation, um Daten zwischen Azure Database for MySQL Flexible Server und unterschiedlichen Cloudanbietern zu synchronisieren, einschließlich virtueller Computer und Datenbankdienste, die in diesen Clouds gehostet werden.
- **Migration:** Kunden können die Migration mit minimalem Zeitaufwand mithilfe von Open-Source-Tools wie [MyDumper/MyLoader](https://centminmod.com/mydumper.html) mit der Datenreplikation durchführen. Eine selektive Übernahme der Produktionslast von der Quell- zur Zieldatenbank ist mit der Datenreplikation möglich. 

Verwenden Sie bei Migrationsszenarien den [Azure Database Migration Service](https://azure.microsoft.com/services/database-migration/)(DMS).

## <a name="limitations-and-considerations"></a>Einschränkungen und Aspekte

### <a name="data-not-replicated"></a>Nicht replizierte Daten

Die [*Systemdatenbank „mysql“*](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html) auf dem Quellserver wird nicht repliziert. Außerdem werden Änderungen an Konten und Berechtigungen auf dem Quellserver nicht repliziert. Wenn Sie ein Konto auf dem Quellserver erstellen und dieses Konto Zugriff auf den Replikatserver erfordert, erstellen Sie dasselbe Konto manuell auf dem Replikatserver. Einen Überblick über die Tabellen, die in der Systemdatenbank enthalten sind, finden Sie im [Leitfaden zu MySQL](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html).

### <a name="filtering"></a>Filterung

Um das Replizieren von Tabellen vom Quellserver (lokal, auf virtuellen Computern oder in einem von anderen Cloudanbietern gehosteten Datenbankdienst gehostet) zu überspringen, wird der `replicate_wild_ignore_table`-Parameter unterstützt. Optional können Sie diesen Parameter auf dem Replikatserver, der in Azure gehostet wird, mithilfe des [Azure-Portals](how-to-configure-server-parameters-portal.md) oder der [Azure CLI](how-to-configure-server-parameters-cli.md) aktualisieren.

Weitere Informationen zu diesem Parameter finden Sie in der [MySQL-Dokumentation](https://dev.mysql.com/doc/refman/8.0/en/replication-options-replica.html#option_mysqld_replicate-wild-ignore-table).

### <a name="requirements"></a>Requirements (Anforderungen)

- Der Quellserver muss mindestens die MySQL-Version 5.7 aufweisen.
- Es wird empfohlen, dass der Quellserver und der Replikatserver dieselbe Version aufweisen. Beispielsweise müssen beide die Version MySQL 5.7 oder beide die Version MySQL 8.0 aufweisen.
- Es wird empfohlen, dass in jeder Tabelle ein Primärschlüssel vorhanden ist. Wenn eine Tabelle ohne Primärschlüssel vorhanden ist, kann sich die Replikation verlangsamen.
- Der Quellserver sollte die MySQL InnoDB-Engine verwenden.
- Benutzer müssen über Berechtigungen zum Konfigurieren der binären Protokollierung und zum Erstellen neuer Benutzer auf dem Quellserver verfügen.
- Wenn für den Quellserver SSL aktiviert ist, vergewissern Sie sich, dass das für die Domäne bereitgestellte SSL-Zertifizierungsstellenzertifikat in die gespeicherte Prozedur `mysql.az_replication_change_master` eingefügt wurde. Sehen Sie sich die folgenden [Beispiele](./how-to-data-in-replication.md#link-source-and-replica-servers-to-start-data-in-replication) und den Parameter `master_ssl_ca` an.
- Vergewissern Sie sich, dass für den Computer, der den Quellserver hostet, sowohl ein- als auch ausgehender Datenverkehr am Port 3306 zugelassen wird.
- Vergewissern Sie sich, dass der Quellserver eine **öffentliche IP-Adresse** hat, dass DNS öffentlich zugänglich ist oder ein vollqualifizierter Domänenname (FQDN) zur Verfügung steht.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [die Datenreplikation für Azure Database for MySQL einrichten](how-to-data-in-replication.md).
- Erfahren Sie mehr über das [Replizieren in Azure mit Lesereplikaten](concepts-read-replicas.md).
