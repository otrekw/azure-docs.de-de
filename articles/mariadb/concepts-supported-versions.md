---
title: 'Unterstützte Versionen: Azure Database for MariaDB'
description: Erfahren Sie, welche Versionen des MariaDB-Servers im Dienst Azure Database for MariaDB unterstützt werden.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 03/18/2020
ms.openlocfilehash: 361ba17532d27a7020be1b6874993da999f48604
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527706"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>Unterstützte Serverversionen von Azure Database for MariaDB

Azure Database for MariaDB wurde mithilfe der InnoDB-Engine auf der Grundlage von [MariaDB Server](https://downloads.mariadb.org/) entwickelt, einem Open-Source-Produkt.

MariaDB verwendet das Benennungsschema „X.Y.Z“. X ist die Hauptversion, Y die Nebenversion und Z die Patchversion.

> [!NOTE]
> Im Dienst wird ein Gateway verwendet, um die Verbindungen an die Serverinstanzen umzuleiten. Sobald die Verbindung hergestellt ist, zeigt der MySQL-Client die im Gateway festgelegte Version von MariaDB an, nicht die tatsächliche Version, die auf Ihrer MariaDB-Serverinstanz ausgeführt wird. Wenn Sie die Version Ihrer MariaDB-Serverinstanz ermitteln möchten, verwenden Sie den Befehl `SELECT VERSION();`.

Azure Database for MariaDB unterstützt derzeit die folgende Version:

## <a name="mariadb-version-102"></a>MariaDB Version 10.2

Patchversion: 10.2.25

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in dieser Version finden Sie in der [Dokumentation zu MariaDB](https://mariadb.com/kb/en/library/mariadb-10225-release-notes/).

## <a name="mariadb-version-103"></a>MariaDB Version 10.3

Patchversion: 10.3.16

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in dieser Version finden Sie in der [Dokumentation zu MariaDB](https://mariadb.com/kb/en/library/mariadb-10316-release-notes/).

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Der Dienst verwaltet Upgrades für Patchupdates automatisch. Beispiel: 10.2.21 auf 10.2.23.  

Zurzeit werden Upgrades von Neben- und Hauptversionen nicht unterstützt. Ein Upgrade von MariaDB 10.2 auf MariaDB 10.3 wird beispielsweise nicht unterstützt. Wenn Sie von 10.2 auf 10.3 aktualisieren möchten, führen Sie eine [Sicherung und Wiederherstellung](./howto-migrate-dump-restore.md) auf einem Server aus, der mit der neuen Modulversion erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrer **Dienstebene** finden Sie unter [Dienstebenen](./concepts-pricing-tiers.md).
