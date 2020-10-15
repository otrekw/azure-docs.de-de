---
title: Unterstützte Versionen für Azure Database for MySQL Flexible Server
description: Erfahren Sie, welche Versionen des MySQL-Servers in Azure Database for MySQL Flexible Server unterstützt werden.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: f29a34fd8916110355c0122fee9db29599a01231
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90930332"
---
# <a name="supported-versions-for-azure-database-for-mysql---flexible-server"></a>Unterstützte Versionen für Azure Database for MySQL – Flexible Server


> [!IMPORTANT]
> Azure Database for MySQL Flexible Server befindet sich aktuell in der öffentlichen Vorschau.


Azure Database for MySQL Flexible Server basiert auf der [MySQL Community-Edition](https://www.mysql.com/products/community/) und verwendet die InnoDB-Engine.

MySQL verwendet das Benennungsschema „X.Y.Z“. X ist die Hauptversion, Y die Nebenversion und Z die Version zur Fehlerbehebung. Weitere Informationen zum Schema finden Sie in der [MySQL-Dokumentation](https://dev.mysql.com/doc/refman/5.7/en/which-version.html).

> [!NOTE]
> Um die Version Ihrer MySQL-Serverinstanz zu ermitteln, geben Sie den `SELECT VERSION();`-Befehl an der MySQL-Eingabeaufforderung ein.

Azure Database for MySQL unterstützt derzeit die folgenden Versionen:

## <a name="mysql-version-57"></a>MySQL-Version 5.7

Version zur Fehlerbehebung: 5.7.29

Der Dienst führt automatisches Patchen für die zugrunde liegende Hardware, das Betriebssystem und die Datenbank-Engine durch. Das Patchen umfasst Sicherheits- und Softwareupdates. Für die MySQL-Engine sind im Rahmen des geplanten Wartungsreleases auch kleinere Versionsupgrades enthalten. Benutzer können den Patchzeitplan so konfigurieren, dass er vom System verwaltet wird, oder einen eigenen Zeitplan definieren. Im Rahmen des Wartungszeitplans wird der Patch angewendet. Möglicherweise muss der Server als Teil des Patchprozesses neu gestartet werden, um das Update abzuschließen. Mithilfe des benutzerdefinierten Zeitplans können Benutzer ihren Patchzyklus überlegt gestalten und ein Wartungsfenster mit minimalen Auswirkungen auf das Unternehmen wählen. Im Allgemeinen gilt für den Dienst im Rahmen der kontinuierlichen Integration und Veröffentlichung ein monatlicher Releasezeitplan.

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in dieser Version finden Sie in den [Versionshinweisen](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-29.html) zu MySQL.

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Das Patchen von Versionsupdates zur Fehlerbehebung wird vom Dienst automatisch verwaltet. Beispiel: 5.7.29 auf 5.7.30.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Erstellen einer PHP-App unter Windows mit MySQL](../../app-service/app-service-web-tutorial-php-mysql.md)<br/>
>[Erstellen einer PHP-App unter Linux mit MySQL](../../app-service/containers/tutorial-php-mysql-app.md)<br/>
>[Erstellen einer Java-basierten Spring-App mit MySQL](https://docs.microsoft.com/azure/developer/java/spring-framework/spring-app-service-e2e?tabs=bash)<br/>
