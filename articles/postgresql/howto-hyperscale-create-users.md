---
title: Erstellen von Benutzern – Hyperscale (Citus) – Azure Database for PostgreSQL
description: In diesem Artikel wird beschrieben, wie Sie neue Benutzerkonten für die Interaktion mit einer Azure Database for PostgreSQL – Hyperscale (Citus) erstellen können.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 1/8/2019
ms.openlocfilehash: 3d23ee6119b625e11ce44bb9ad11ce4b3ee0280d
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91295735"
---
# <a name="create-users-in-azure-database-for-postgresql---hyperscale-citus"></a>Erstellen von Benutzern in Azure Database for PostgreSQL: Hyperscale (Citus)

> [!NOTE]
> Der Begriff „Benutzer“ bezieht sich auf Benutzer innerhalb einer Hyperscale (Citus)-Servergruppe. Um stattdessen mehr über Azure-Abonnementbenutzer und ihre Berechtigungen zu erfahren, lesen Sie den Artikel zur [Rollenbasierten Azure-Zugriffssteuerung (Azure RBAC) ](../role-based-access-control/built-in-roles.md), oder informieren Sie sich darüber, [wie Rollen angepasst werden können](../role-based-access-control/custom-roles.md).

## <a name="the-server-admin-account"></a>Das Serveradministratorkonto

Die PostgreSQL-Engine verwendet [Rollen](https://www.postgresql.org/docs/current/sql-createrole.html), um den Zugriff auf Datenbankobjekte zu steuern, und eine neu erstellte Hyperscale (Citus)-Servergruppe verfügt über mehrere vordefinierte Rollen:

* Die [PostgreSQL-Standardrollen](https://www.postgresql.org/docs/current/default-roles.html)
* `azure_pg_admin`
* `postgres`
* `citus`

Da Hyperscale (Citus) ein verwalteter PaaS-Dienst ist, kann sich nur Microsoft mit der Rolle „`postgres` superuser“ anmelden. Für eingeschränkten Administratorzugriff bietet Hyperscale (Citus) die Rolle `citus`.

Berechtigungen der Rolle `citus`:

* Lesen aller Konfigurationsvariablen, auch der Variablen, die normalerweise nur für Superuser sichtbar sind
* Lesen aller Sichten vom Typ „pg\_stat\_\*“ und Verwenden von verschiedenen statistikbezogenen Erweiterungen, einschließlich der Sichten oder Erweiterungen, die normalerweise nur für Superuser sichtbar sind
* Ausführen von Überwachungsfunktionen, die möglicherweise für einen längeren Zeitraum ACCESS SHARE-Sperren auf Tabellen anwenden
* [Erstellen von PostgreSQL-Erweiterungen](concepts-hyperscale-extensions.md) (weil die Rolle ein Mitglied von `azure_pg_admin` ist)

Insbesondere gelten für die Rolle `citus` einige Einschränkungen:

* Kann keine Rollen erstellen
* Kann keine Datenbanken erstellen

## <a name="how-to-create-additional-user-roles"></a>So erstellen Sie zusätzliche Benutzerrollen

Wie bereits erwähnt, verfügt das `citus`-Administratorkonto nicht über die Berechtigung zum Erstellen zusätzlicher Benutzer. Um einen Benutzer hinzuzufügen, verwenden Sie das Azure-Portal.

1. Navigieren Sie zur Seite **Rollen** für Ihre Servergruppe mit Hyperscale (Citus), und klicken Sie auf **+ Hinzufügen**:

   :::image type="content" source="media/howto-hyperscale-create-users/1-role-page.png" alt-text="Die Seite „Rollen“":::

2. Geben Sie den Rollennamen und das Kennwort ein. Klicken Sie auf **Speichern**.

   :::image type="content" source="media/howto-hyperscale-create-users/2-add-user-fields.png" alt-text="Die Seite „Rollen“":::

Der Benutzer wird auf dem Koordinatorknoten der Servergruppe erstellt und an alle Workerknoten weitergegeben. Über das Azure-Portal erstellte Rollen weisen das `LOGIN`-Attribut auf. Dies bedeutet, dass es sich um echte Benutzer handelt, die sich bei der Datenbank anmelden können.

## <a name="how-to-modify-privileges-for-user-role"></a>So ändern Sie Berechtigungen für eine Benutzerrolle

Neue Benutzerrollen werden häufig verwendet, um Datenbankzugriff mit eingeschränkten Berechtigungen bereitzustellen. Verwenden Sie zum Ändern von Benutzerberechtigungen PostgreSQL-Standardbefehle mithilfe eines Tools wie PgAdmin oder psql. (Weitere Informationen finden Sie im Schnellstart zu Hyperscale (Citus) unter [Herstellen einer Verbindung mit psql](quickstart-create-hyperscale-portal.md#connect-to-the-database-using-psql).)

Um z. B. `db_user` das Lesen von `mytable` zu erlauben, erteilen Sie die folgende Berechtigung:

```sql
GRANT SELECT ON mytable TO db_user;
```

Hyperscale (Citus) gibt eine GRANT-Anweisung für einzelne Tabellen über den gesamten Cluster weiter und wendet sie auf alle Workerknoten an. Außerdem werden systemweite GRANT-Anweisungen weitergegeben (z. B. für alle Tabellen in einem Schema):

```sql
-- applies to the coordinator node and propagates to workers
GRANT SELECT ON ALL TABLES IN SCHEMA public TO db_user;
```

## <a name="how-to-delete-a-user-role-or-change-their-password"></a>So löschen Sie eine Benutzerrolle oder ändern das Kennwort eines Benutzers

Um einen Benutzer zu aktualisieren, navigieren Sie zur Seite **Rollen** für Ihre Servergruppe mit Hyperscale (Citus), und klicken Sie auf die Auslassungspunkte ( **...** ) neben dem Benutzer. Die Auslassungspunkte öffnen ein Menü, in dem der Benutzer gelöscht oder sein Kennwort zurückgesetzt werden kann.

   :::image type="content" source="media/howto-hyperscale-create-users/edit-role.png" alt-text="Die Seite „Rollen“":::

Die Rolle `citus` ist privilegiert und kann nicht gelöscht werden.

## <a name="next-steps"></a>Nächste Schritte

Öffnen Sie die Firewall für die IP-Adressen der Computer der neuen Benutzer, um ihnen das Herstellen einer Verbindung zu ermöglichen: [Erstellen und Verwalten von Firewallregeln für Hyperscale (Citus) mithilfe des Azure-Portals](howto-hyperscale-manage-firewall-using-portal.md).

Weitere Informationen zur Verwaltung von Datenbankbenutzerkonten finden Sie in der PostgreSQL-Produktdokumentation:

* [Datenbankrollen und Berechtigungen](https://www.postgresql.org/docs/current/static/user-manag.html)
* [GRANT-Syntax](https://www.postgresql.org/docs/current/static/sql-grant.html)
* [Berechtigungen](https://www.postgresql.org/docs/current/static/ddl-priv.html)
