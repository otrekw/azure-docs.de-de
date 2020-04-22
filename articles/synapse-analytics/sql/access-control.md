---
title: Verwalten des Zugriffs auf Arbeitsbereiche, Daten und Pipelines
description: Hier erfahren Sie, wie Sie die Zugriffssteuerung für Arbeitsbereiche, Daten und Pipelines in einem Azure Synapse Analytics-Arbeitsbereich (Vorschauversion) verwalten.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 89d2105ab080309639c4341072c3f5f36608dfce
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421104"
---
# <a name="manage-access-to-workspaces-data-and-pipelines"></a>Verwalten des Zugriffs auf Arbeitsbereiche, Daten und Pipelines

Hier erfahren Sie, wie Sie die Zugriffssteuerung für Arbeitsbereiche, Daten und Pipelines in einem Azure Synapse Analytics-Arbeitsbereich (Vorschauversion) verwalten.

> [!NOTE]
> In der allgemein verfügbaren Version werden zur Weiterentwicklung der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) Synapse-spezifische Azure RBAC-Rollen eingeführt.

## <a name="access-control-for-workspace"></a>Zugriffssteuerung für den Arbeitsbereich

Bei einer Produktionsbereitstellung in einem Azure Synapse-Arbeitsbereich empfiehlt es sich, die Umgebung zu strukturieren, um die Bereitstellung von Benutzern und Administratoren zu erleichtern.

> [!NOTE]
> Der hier verfolgte Ansatz besteht darin, mehrere Sicherheitsgruppen zu erstellen und anschließend den Arbeitsbereich für eine konsistente Verwendung zu konfigurieren. Nach Einrichtung der Gruppen muss ein Administrator nur noch die Mitgliedschaft innerhalb der Sicherheitsgruppen verwalten.

### <a name="step-1-set-up-security-groups-with-names-following-this-pattern"></a>Schritt 1: Einrichten von Sicherheitsgruppen mit Namen nach dem folgenden Muster

1. Erstellen Sie eine Sicherheitsgruppe namens `Synapse_WORKSPACENAME_Users`.
2. Erstellen Sie eine Sicherheitsgruppe namens `Synapse_WORKSPACENAME_Admins`.
3. Fügen Sie `Synapse_WORKSPACENAME_Admins` zu `ProjectSynapse_WORKSPACENAME_Users` hinzu.

### <a name="step-2-prepare-the-default-adls-gen2-account"></a>Schritt 2: Vorbereiten des ADLS Gen2-Standardkontos

Beim Bereitstellen Ihres Arbeitsbereichs mussten Sie für den Arbeitsbereich ein ADLSGEN2-Konto und einen Container für das Dateisystem auswählen.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zum ADLSGEN2-Konto.
3. Navigieren Sie zu dem Container (Dateisystem), den Sie für den Azure Synapse-Arbeitsbereich ausgewählt haben.
4. Klicken Sie auf **Zugriffssteuerung (IAM)** .
5. Weisen Sie die folgenden Rollen zu:
   1. **Leser**: `Synapse_WORKSPACENAME_Users`
   2. **Besitzer von Speicherblobdaten**: `Synapse_WORKSPACENAME_Admins`
   3. **Mitwirkender an Storage-Blobdaten**: `Synapse_WORKSPACENAME_Users`
   4. **Besitzer von Speicherblobdaten**: `WORKSPACENAME`
  
### <a name="step-3-configure-the-workspace-admin-list"></a>Schritt 3: Konfigurieren der Arbeitsbereichsadministratorliste

1. Navigieren Sie zur [**Webbenutzeroberfläche von Azure Synapse**](https://web.azuresynapse.net).
2. Navigieren Sie zu **Verwalten**  > **Sicherheit** > **Zugriffssteuerung**.
3. Klicken Sie auf **Administrator hinzufügen**, und wählen Sie `Synapse_WORKSPACENAME_Admins` aus.

### <a name="step-4-configure-sql-admin-access-for-the-workspace"></a>Schritt 4: Konfigurieren des SQL-Administratorzugriffs für den Arbeitsbereich

1. [Navigieren Sie zum Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zu Ihrem Arbeitsbereich.
3. Navigieren Sie zu **Einstellungen** > **Active Directory-Administrator**.
4. Klicken Sie auf **Administrator festlegen**.
5. Wählen Sie `Synapse_WORKSPACENAME_Admins` aus.
6. Klicken Sie auf **Auswählen**.
7. Klicken Sie auf **Speichern**.

### <a name="step-5-add-and-remove-users-and-admins-to-security-groups"></a>Schritt 5: Hinzufügen und Entfernen von Benutzern und Administratoren zu bzw. aus Sicherheitsgruppen

1. Fügen Sie Benutzer, die Administratorzugriff benötigen, `Synapse_WORKSPACENAME_Admins` hinzu.
2. Fügen Sie alle anderen Benutzer `Synapse_WORKSPACENAME_Users` hinzu.

## <a name="access-control-to-data"></a>Zugriffssteuerung für Daten

Die Zugriffssteuerung für die zugrunde liegenden Daten ist in drei Bereiche unterteilt:

- Datenebenenzugriff auf das Speicherkonto (wurde bereits oben in Schritt 2 konfiguriert)
- Datenebenenzugriff auf die SQL-Datenbanken (sowohl für SQL-Pools als auch für SQL On-Demand)
- Erstellen von Anmeldeinformationen für SQL On-Demand-Datenbanken über das Speicherkonto

## <a name="access-control-to-sql-databases"></a>Zugriffssteuerung für SQL-Datenbanken

> [!TIP]
> Die folgenden Schritte müssen für **jede** SQL-Datenbank ausgeführt werden, um Benutzern Zugriff auf alle SQL-Datenbanken zu gewähren.

### <a name="sql-on-demand"></a>SQL On-Demand

Führen Sie die Schritte des folgenden Beispiels aus, um einem Benutzer Zugriff auf eine **einzelne** SQL On-Demand-Datenbank zu gewähren:

1. Erstellen Sie eine Anmeldung:

    ```sql
    use master
    go
    CREATE LOGIN [John.Thomas@microsoft.com] FROM EXTERNAL PROVIDER;
    go
    ```

2. Erstellen Sie einen Benutzer:

    ```sql
    use yourdb -- Use your DB name
    go
    CREATE USER john FROM LOGIN [John.Thomas@microsoft.com];
    ```

3. Fügen Sie den Benutzer den Mitgliedern der angegebenen Rolle hinzu:

    ```sql
    use yourdb -- Use your DB name
    go
    alter role db_owner Add member john -- Type USER name from step 2
    ```

### <a name="sql-pools"></a>SQL-Pools

Führen Sie die folgenden Schritte aus, um einem Benutzer Zugriff auf eine **einzelne** SQL-Datenbank zu gewähren:

1. Erstellen Sie den Benutzer in der Datenbank, indem Sie den folgenden Befehl ausführen, und verwenden Sie dabei die gewünschte Datenbank als Ziel in der Kontextauswahl (Dropdownliste zum Auswählen von Datenbanken):

    ```sql
    --Create user in SQL DB
    CREATE USER [<alias@domain.com>] FROM EXTERNAL PROVIDER;
    ```

2. Weisen Sie dem Benutzer eine Rolle für den Zugriff auf die Datenbank zu:

    ```sql
    --Create user in SQL DB
    EXEC sp_addrolemember 'db_owner', '<alias@domain.com>';
    ```

> [!IMPORTANT]
> Wenn Sie die Berechtigung *db_owner* nicht zuweisen möchten, können Sie *db_datareader* und *db_datawriter* für Lese-/Schreibberechtigungen verwenden.
> Damit ein Spark-Benutzer Daten direkt über Spark aus einem SQL-Pool lesen bzw. in einen SQL-Pool schreiben kann, benötigt er die Berechtigung *db_owner*.

Vergewissern Sie sich im Anschluss an die Benutzererstellung, dass das Speicherkonto von SQL On-Demand abgefragt werden kann:

- Führen Sie den folgenden Befehl aus, und verwenden Sie dabei die **Masterdatenbank** von SQL On-Demand als Ziel:

    ```sql
    CREATE CREDENTIAL [https://<storageaccountname>.dfs.core.windows.net]
    WITH IDENTITY='User Identity';
    ```

## <a name="access-control-to-workspace-pipeline-runs"></a>Zugriffssteuerung für Pipelineausführungen im Arbeitsbereich

### <a name="workspace-managed-identity"></a>Vom Arbeitsbereich verwaltete Identität

> [!IMPORTANT]
> Zur erfolgreichen Ausführung von Pipelines mit Datasets oder Aktivitäten, die auf einen SQL-Pool verweisen, muss der Arbeitsbereichsidentität direkter Zugriff auf den SQL-Pool gewährt werden.

Führen Sie die folgenden Befehle für jeden SQL-Pool aus, um der vom Arbeitsbereich verwalteten Identität das Ausführen von Pipelines für die SQL-Pooldatenbank zu ermöglichen:

```sql
--Create user in DB
CREATE USER [<workspacename>] FROM EXTERNAL PROVIDER;

--Granting permission to the identity
GRANT CONTROL ON DATABASE::<SQLpoolname> TO <workspacename>;
```

Wenn Sie diese Berechtigung wieder entfernen möchten, führen Sie das folgende Skript für den gleichen SQL-Pool aus:

```sql
--Revoking permission to the identity
REVOKE CONTROL ON DATABASE::<SQLpoolname> TO <workspacename>;

--Deleting the user in the DB
DROP USER [<workspacename>];
```

## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über den Zugriff und die Steuerung in Synapse SQL finden Sie unter [Verwalten des Zugriffs auf Arbeitsbereiche, Daten und Pipelines](../sql/access-control.md). Weitere Informationen zu Datenbankprinzipalen finden Sie unter [Prinzipale](https://msdn.microsoft.com/library/ms181127.aspx). Weitere Informationen zu Datenbankrollen finden Sie im Artikel zu [Datenbankrollen](https://msdn.microsoft.com/library/ms189121.aspx).
