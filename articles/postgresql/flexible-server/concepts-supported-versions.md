---
title: Unterstützte Versionen für Azure Database for PostgreSQL – Flexible Server
description: In diesem Artikel werden die unterstützten Haupt- und Nebenversionen von PostgreSQL in Azure Database for PostgreSQL – Flexible Server beschrieben.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: 474545b7c5eca64b558e5fe26e23e6800b0cab96
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102037165"
---
# <a name="supported-postgresql-major-versions-in-azure-database-for-postgresql---flexible-server"></a>Unterstützte Haupt- und Nebenversionen von PostgreSQL für Azure Database for PostgreSQL – Flexible Server

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

Azure Database for PostgreSQL – Flexible Server unterstützt derzeit die folgenden Hauptversionen:

## <a name="postgresql-version-12"></a>PostgreSQL Version 12

Das aktuelle Nebenrelease ist 12.5. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Nebenrelease finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/12/static/release-12-4.html).

## <a name="postgresql-version-11"></a>PostgreSQL Version 11

Das aktuelle Nebenrelease ist 11.10. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Nebenrelease finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-9.html).

## <a name="postgresql-version-10-and-older"></a>PostgreSQL Version 10 und früher

PostgreSQL Version 10 und frühere Versionen werden für Azure Database for PostgreSQL – Flexible Server nicht unterstützt. Verwenden Sie die Bereitstellungsoption [Einzelserver](../concepts-supported-versions.md), wenn Sie eine ältere Version benötigen.

## <a name="managing-upgrades"></a>Verwalten von Upgrades

Das PostgreSQL-Projekt gibt regelmäßig kleinere Releases aus, um gemeldete Fehler zu beheben. Azure Database for PostgreSQL führt automatische Patches der Server mit diesen Nebenreleases während der monatlichen Dienstbereitstellung durch.

Automation für ein Upgrade auf die Hauptversion wird noch nicht unterstützt. Beispielsweise erfolgt derzeit kein automatisches Upgrade von PostgreSQL 11 auf PostgreSQL 12.<!-- To upgrade to the next major version, create a [database dump and restore](howto-migrate-using-dump-and-restore.md) to a server that was created with the new engine version.-->

<!--
## Next steps

For information on supported PostgreSQL extensions, see [the extensions document](concepts-extensions.md).
-->