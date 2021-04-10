---
title: 'Unterstützte Versionen: Azure Database for MySQL'
description: Erfahren Sie, welche Versionen des MySQL-Servers im Dienst Azure Database for MySQL unterstützt werden.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 6/3/2020
ms.openlocfilehash: 8b85307f01a11366a2147c947f26658f548932e8
ms.sourcegitcommit: 3ea12ce4f6c142c5a1a2f04d6e329e3456d2bda5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2021
ms.locfileid: "103467713"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Unterstützte Azure-Datenbank für MySQL-Serverversionen

Azure Database for MySQL wurde basierend auf [MySQL Community Edition](https://www.mysql.com/products/community/) mit der InnoDB-Speicher-Engine entwickelt. Dieser Dienst unterstützt alle aktuellen Hauptversionen, die von der Community unterstützt werden, und zwar MySQL 5.6, 5.7 und 8.0. Für MySQL wird das Benennungsschema „X.Y.Z“ verwendet. „X“ steht dabei für die Hauptversion, „Y“ für die Nebenversion und „Z“ für das Programmfehlerbehebungs-Release. Weitere Informationen zum Schema finden Sie in der [MySQL-Dokumentation](https://dev.mysql.com/doc/refman/5.7/en/which-version.html).

> [!NOTE]
> Bei der Bereitstellungsoption „Einzelserver“ werden die Verbindungen mit einem Gateway zu Serverinstanzen umgeleitet. Sobald die Verbindung hergestellt ist, zeigt der MySQL-Client die im Gateway festgelegte Version von MySQL an, nicht die tatsächliche Version, die auf Ihrer MySQL-Serverinstanz ausgeführt wird. Um die Version Ihrer MySQL-Serverinstanz zu ermitteln, geben Sie den `SELECT VERSION();`-Befehl an der MySQL-Eingabeaufforderung ein.

Azure Database for MySQL unterstützt derzeit die folgenden Haupt- und Nebenversionen von MySQL:


| Version | Einzelner Server <br/> Aktuelle Nebenversion |Flexible Server (Vorschau) <br/> Aktuelle Nebenversion  |
|:-------------------|:-------------------------------------------|:---------------------------------------------|
|MySQL-Version 5.6 |  [5.6.47](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-47.html) (eingestellt) | Nicht unterstützt|
|MySQL-Version 5.7 | [5.7.29](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-29.html) | [5.7.29](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-29.html)|
|MySQL, Version 8.0 | [8.0.15](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-15.html) | [8.0.21](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-21.html)|

Lesen Sie die Richtlinie zur Versionsunterstützung für eingestellte Versionen in der [Dokumentation zur Richtlinie für die Versionsunterstützung](concepts-version-policy.md#retired-mysql-engine-versions-not-supported-in-azure-database-for-mysql).

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Das Patchen von Versionsupdates zur Fehlerbehebung wird vom Dienst automatisch verwaltet. Beispiel: 5.7.20 bis 5.7.21.  

Ein Upgrade der Hauptversion wird zurzeit vom Dienst für Upgrades von MySQL v5.6 auf v5.7 unterstützt. Weitere Einzelheiten finden Sie unter [Upgraden von Hauptversionen](how-to-major-version-upgrade.md). Wenn Sie von 5.7 auf 8.0 upgraden möchten, sollten Sie eine [Sicherung und dann die Wiederherstellung](./concepts-migrate-dump-restore.md) auf einem Server ausführen, der mit der neuen Engine-Version erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

- Ausführliche Informationen zu Versionsrichtlinien für Azure Database for MySQL finden Sie in [diesem Dokument](concepts-version-policy.md).
- Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrer **Dienstebene** finden Sie unter [Dienstebenen](./concepts-pricing-tiers.md).
