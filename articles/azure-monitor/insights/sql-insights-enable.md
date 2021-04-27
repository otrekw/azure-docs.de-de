---
title: Aktivieren von SQL Insights
description: Aktivieren von SQL Insights in Azure Monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/15/2021
ms.openlocfilehash: e8dd887d151eb553131048f232940555dbef324b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105025032"
---
# <a name="enable-sql-insights-preview"></a>Aktivieren von SQL Insights (Vorschau)
In diesem Artikel wird beschrieben, wie Sie [SQL Insights](sql-insights-overview.md) zum Überwachen Ihrer SQL-Bereitstellungen aktivieren. Die Überwachung erfolgt über einen virtuellen Azure-Computer, der eine Verbindung mit Ihren SQL-Bereitstellungen herstellt und dynamische Verwaltungssichten (Dynamic Management Views, DMVs) verwendet, um Überwachungsdaten zu erfassen. Sie können mithilfe eines Überwachungsprofils steuern, welche Datasets mit welcher Häufigkeit gesammelt werden.

## <a name="create-log-analytics-workspace"></a>Erstellen eines Log Analytics-Arbeitsbereichs
SQL Insights speichert die Daten in mindestens einem [Log Analytics-Arbeitsbereich](../logs/data-platform-logs.md#log-analytics-workspaces).  Bevor Sie SQL Insights aktivieren können, müssen Sie entweder [einen Arbeitsbereich erstellen](../logs/quick-create-workspace.md) oder einen vorhandenen auswählen. Ein einzelner Arbeitsbereich kann mit mehreren Überwachungsprofilen verwendet werden, aber der Arbeitsbereich und die Profile müssen sich in derselben Azure-Region befinden. Um die Features in SQL Insights aktivieren und darauf zugreifen zu können, müssen Sie im Arbeitsbereich über die Rolle [Log Analytics-Mitwirkender](../logs/manage-access.md) verfügen. 

## <a name="create-monitoring-user"></a>Erstellen eines Überwachungsbenutzers 
Sie benötigen einen Benutzer für die SQL-Bereitstellungen, die Sie überwachen möchten. Führen Sie die folgenden Verfahren für verschiedene Arten von SQL-Bereitstellungen aus.

### <a name="azure-sql-database"></a>Azure SQL-Datenbank
Öffnen Sie Azure SQL-Datenbank mit [SQL Server Management Studio](../../azure-sql/database/connect-query-ssms.md) oder dem [Abfrage-Editor (Vorschau)](../../azure-sql/database/connect-query-portal.md) im Azure-Portal.

Führen Sie das folgende Skript aus, um einen Benutzer mit den erforderlichen Berechtigungen zu erstellen. Ersetzen Sie *user* durch einen Benutzernamen und *mystrongpassword* durch ein Kennwort.

```
CREATE USER [user] WITH PASSWORD = N'mystrongpassword'; 
GO 
GRANT VIEW DATABASE STATE TO [user]; 
GO 
```

:::image type="content" source="media/sql-insights-enable/telegraf-user-database-script.png" alt-text="Erstellen eines Telegraf-Benutzerskripts." lightbox="media/sql-insights-enable/telegraf-user-database-script.png":::

Vergewissern Sie sich, dass der Benutzer erstellt wurde.

:::image type="content" source="media/sql-insights-enable/telegraf-user-database-verify.png" alt-text="Überprüfen eines Telegraf-Benutzerskripts." lightbox="media/sql-insights-enable/telegraf-user-database-verify.png":::

### <a name="azure-sql-managed-instance"></a>Verwaltete Azure SQL-Instanz
Melden Sie sich an Ihrer Azure SQL Managed Instance an und führen Sie mit [SQL Server Management Studio](../../azure-sql/database/connect-query-ssms.md) oder einem ähnlichen Tool das folgende Skript aus, um den Überwachungsbenutzer mit den erforderlichen Berechtigungen zu erstellen. Ersetzen Sie *user* durch einen Benutzernamen und *mystrongpassword* durch ein Kennwort.

 
```
USE master; 
GO 
CREATE LOGIN [user] WITH PASSWORD = N'mystrongpassword'; 
GO 
GRANT VIEW SERVER STATE TO [user]; 
GO 
GRANT VIEW ANY DEFINITION TO [user]; 
GO 
```

### <a name="sql-server"></a>SQL Server
Melden Sie sich bei Ihrem virtuellen Azure-Computer an, auf dem SQL Server ausgeführt wird, und verwenden Sie [SQL Server Management Studio](../../azure-sql/database/connect-query-ssms.md) oder ein ähnliches Tool, um das folgende Skript zum Erstellen des Überwachungsbenutzers mit den erforderlichen Berechtigungen auszuführen. Ersetzen Sie *user* durch einen Benutzernamen und *mystrongpassword* durch ein Kennwort.

 
```
USE master; 
GO 
CREATE LOGIN [user] WITH PASSWORD = N'mystrongpassword'; 
GO 
GRANT VIEW SERVER STATE TO [user]; 
GO 
GRANT VIEW ANY DEFINITION TO [user]; 
GO
```

## <a name="create-azure-virtual-machine"></a>Erstellen eines virtuellen Azure-Computers 
Sie müssen mindestens einen virtuellen Azure-Computer erstellen, der zum Sammeln von Daten zum Überwachen von SQL verwendet wird.  

> [!NOTE]
>  Mit den [Überwachungsprofilen](#create-sql-monitoring-profile) wird angegeben, welche Daten Sie von den verschiedenen SQL-Typen erfassen, die Sie überwachen möchten. Jedem virtuellen Überwachungscomputer kann nur ein Überwachungsprofil zugeordnet werden. Wenn Sie mehrere Überwachungsprofile benötigen, müssen Sie jeweils einen virtuellen Computer erstellen.

### <a name="azure-virtual-machine-requirements"></a>Anforderungen für virtuelle Azure-Maschinen
Für die virtuellen Azure-Computer gelten folgende Anforderungen.

- Betriebssystem: Ubuntu 18.04 
- Empfohlene Größen für virtuelle Azure-Computer: Standard_B2s (2 CPUs, 4 GiB Arbeitsspeicher) 
- Unterstützte Regionen: Jede [Region, die vom Azure Monitor-Agent unterstützt wird](../agents/azure-monitor-agent-overview.md#supported-regions)

> [!NOTE]
> Die Größe des virtuellen Computers Standard_B2s (2 CPUs, 4 GiB Arbeitsspeicher) unterstützt bis zu 100 Verbindungszeichenfolgen. Sie sollten einem einzelnen virtuellen Computer nicht mehr als 100 Verbindungen zuordnen.

Abhängig von den Netzwerkeinstellungen Ihrer SQL-Ressourcen müssen die virtuellen Maschinen möglicherweise in dasselbe virtuelle Netzwerk wie Ihre SQL-Ressourcen platziert werden, damit sie Netzwerkverbindungen herstellen können, um Überwachungsdaten zu sammeln.  

## <a name="configure-network-settings"></a>Konfigurieren der Netzwerkeinstellungen
Jeder SQL-Typ bietet Methoden, mit denen der virtuelle Computer für die Überwachung sicher auf SQL zugreifen können.  In den folgenden Abschnitten werden die Optionen auf Basis des SQL-Typs aufgeführt.

### <a name="azure-sql-databases"></a>Azure SQL-Datenbank-Instanzen  

SQL Insights unterstützt den Zugriff auf Ihre Azure SQL-Datenbank-Instanz sowohl über den öffentlichen Endpunkt als auch über das virtuelle Netzwerk.

Für den Zugriff über den öffentlichen Endpunkt müssen Sie auf der Seite **Firewalleinstellungen** im Abschnitt [IP-Firewalleinstellungen](https://docs.microsoft.com/azure/azure-sql/database/network-access-controls-overview#ip-firewall-rules) eine Regel hinzufügen.  Um den Zugriff aus einem virtuellen Netzwerk anzugeben, können Sie [Firewallregeln für virtuelle Netzwerke](https://docs.microsoft.com/azure/azure-sql/database/network-access-controls-overview#virtual-network-firewall-rules) und die [vom Azure Monitor-Agent benötigten Diensttags](https://docs.microsoft.com/azure/azure-monitor/agents/azure-monitor-agent-overview#networking) festlegen.  In [diesem Artikel](https://docs.microsoft.com/azure/azure-sql/database/network-access-controls-overview#ip-vs-virtual-network-firewall-rules) werden die Unterschiede zwischen diesen beiden Arten von Firewallregeln beschrieben.

:::image type="content" source="media/sql-insights-enable/set-server-firewall.png" alt-text="Festlegen der Serverfirewall" lightbox="media/sql-insights-enable/set-server-firewall.png":::

:::image type="content" source="media/sql-insights-enable/firewall-settings.png" alt-text="Firewalleinstellungen." lightbox="media/sql-insights-enable/firewall-settings.png":::


### <a name="azure-sql-managed-instances"></a>Verwaltete Azure SQL-Instanzen 

Wenn sich der virtuelle Computer für die Überwachung im selben VNET wie Ihre SQL MI-Ressourcen befindet, informieren Sie sich unter [Herstellen einer Verbindung im selben VNET](https://docs.microsoft.com/azure/azure-sql/managed-instance/connect-application-instance#connect-inside-the-same-vnet). Wenn sich der virtuelle Computer für die Überwachung in einem anderen VNET als die SQL MI-Ressourcen befindet, informieren Sie sich unter [Herstellen einer Verbindung in einem anderen VNET](https://docs.microsoft.com/azure/azure-sql/managed-instance/connect-application-instance#connect-inside-a-different-vnet).


### <a name="azure-virtual-machine-and-azure-sql-virtual-machine"></a>Virtueller Azure-Computer und virtueller Azure SQL-Computer  
Wenn sich der virtuelle Computer für die Überwachung im selben VNET wie Ihre SQL-VM-Ressourcen befindet, informieren Sie sich unter [Verbinden mit SQL Server innerhalb eines virtuellen Netzwerks](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/ways-to-connect-to-sql#connect-to-sql-server-within-a-virtual-network). Wenn sich der virtuelle Computer für die Überwachung in einem anderen VNET als Ihre SQL-VM-Ressourcen befindet, informieren Sie sich unter [Verbinden mit SQL Server über das Internet](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/ways-to-connect-to-sql#connect-to-sql-server-over-the-internet).

## <a name="store-monitoring-password-in-key-vault"></a>Speichern des Überwachungskennworts in Key Vault
Sie sollten Ihre SQL-Benutzerverbindungs-Kennwörter in einer Key Vault-Instanz speichern, anstatt sie direkt in die Verbindungszeichenfolgen für das Überwachungsprofil einzugeben.

Wenn Sie Ihr Profil für die SQL-Überwachung einrichten, benötigen Sie eine der folgenden Berechtigungen für die Key Vault-Ressource, die Sie verwenden möchten:

- Microsoft.Authorization/roleAssignments/write 
- Microsoft.Authorization/roleAssignments/delete-Berechtigungen wie Benutzerzugriffsadministrator oder Besitzer 

Beim Erstellen des SQL-Überwachungsprofils, das die von Ihnen angegebene Key Vault-Instanz verwendet, wird automatisch eine neue Zugriffsrichtlinie erstellt. Verwenden Sie für die Key Vault-Netzwerkeinstellungen die Option *Zugriff aus allen Netzwerken zulassen*.


## <a name="create-sql-monitoring-profile"></a>Erstellen eines SQL-Überwachungsprofils
Sie können SQL Insights durch Auswählen von **SQL (Vorschau)** im Bereich **Insights** des Menüs **Azure Monitor** im Azure-Portal öffnen. Klicken Sie auf **Neues Profil erstellen**. 

:::image type="content" source="media/sql-insights-enable/create-new-profile.png" alt-text="Erstellen Sie ein neues Profil." lightbox="media/sql-insights-enable/create-new-profile.png":::

Im Profil werden die Informationen gespeichert, die Sie aus Ihren SQL-Systemen erfassen möchten.  Es bietet spezifische Einstellungen für: 

- Azure SQL-Datenbank 
- Verwaltete Azure SQL-Instanzen 
- Ausführung von SQL Server auf virtuellen Computern  

Sie können z. B. ein Profil mit dem Namen *SQL-Produktion* und ein anderes namens *SQL-Staging* mit anderen Einstellungen für die Häufigkeit der Datensammlung, die zu sammelnden Daten und den Arbeitsbereich, an den die Daten gesendet werden sollen, erstellen. 

Das Profil wird als [Datensammlungsregel](../agents/data-collection-rule-overview.md)-Ressource in dem Abonnement und der Ressourcengruppe Ihrer Wahl gespeichert. Für jedes Profil ist Folgendes erforderlich:

- Name. Kann nach der Erstellung nicht mehr bearbeitet werden.
- Der Standort. Dies ist eine Azure-Region.
- Log Analytics-Arbeitsbereich zum Speichern von Überwachungsdaten.
- Sammlungseinstellungen für die Häufigkeit und den Typ der zu sammelnden SQL-Überwachungsdaten.

> [!NOTE]
> Der Speicherort des Profils sollte sich an demselben Speicherort befinden wie der Log Analytics-Arbeitsbereich, an den die Überwachungsdaten gesendet werden sollen.


:::image type="content" source="media/sql-insights-enable/profile-details.png" alt-text="Profildetails." lightbox="media/sql-insights-enable/profile-details.png":::

Klicken Sie auf **Überwachungsprofil erstellen**, nachdem Sie die Details für Ihr Überwachungsprofil eingegeben haben. Es kann bis zu einer Minute dauern, bis das Profil bereitgestellt wird.  Wenn das neue Profil nicht im Kombinationsfeld **Überwachungsprofil** angezeigt wird, klicken Sie auf die Aktualisierungsschaltfläche, und nach Abschluss der Bereitstellung sollte es angezeigt werden.  Nachdem Sie das neue Profil ausgewählt haben, wählen Sie die Registerkarte **Profil verwalten** aus, um einen Überwachungscomputer hinzuzufügen, der dem Profil zugeordnet wird.

### <a name="add-monitoring-machine"></a>Hinzufügen des Überwachungscomputers
Wählen Sie **Überwachungscomputer hinzufügen** aus, um einen Kontextbereich zu öffnen und den virtuellen Computer auszuwählen, den Sie zum Überwachen Ihrer SQL-Instanzen einrichten möchten, und geben Sie die Verbindungszeichenfolgen an.

Wählen Sie das Abonnement und den Namen des virtuellen Computers zur Überwachung aus. Wenn Sie Key Vault zum Speichern Ihres Kennworts für den Überwachungsbenutzer verwenden, wählen Sie die Key Vault-Ressourcen mit diesen Geheimnissen aus, und geben Sie die URL und den geheimen Namen ein, die in den Verbindungszeichenfolgen verwendet werden sollen. Weitere Informationen zum Identifizieren der Verbindungszeichenfolge für verschiedene SQL-Bereitstellungen finden Sie im nächsten Abschnitt.


:::image type="content" source="media/sql-insights-enable/add-monitoring-machine.png" alt-text="Fügen Sie den Überwachungscomputer hinzu." lightbox="media/sql-insights-enable/add-monitoring-machine.png":::


### <a name="add-connection-strings"></a>Hinzufügen von Verbindungszeichenfolgen 
Die Verbindungszeichenfolge gibt den Benutzernamen an, den SQL Insights bei der Anmeldung bei SQL zum Ausführen der dynamischen Verwaltungssichten verwenden sollte. Wenn Sie Key Vault verwenden, um das Kennwort für den Überwachungsbenutzer zu speichern, geben Sie die URL und den Namen des zu verwendenden Geheimnisses an. 

Die Verbindungszeichenfolge ist für jeden SQL-Ressourcentyp unterschiedlich:

#### <a name="azure-sql-databases"></a>Azure SQL-Datenbank-Instanzen 
Geben Sie Ihre Verbindungszeichenfolge in dieser Form ein:

```
sqlAzureConnections": [ 
   "Server=mysqlserver.database.windows.net;Port=1433;Database=mydatabase;User Id=$username;Password=$password;" 
}
```

Rufen Sie die Details aus dem Menüelement **Verbindungszeichenfolgen** für die Datenbank ab.

:::image type="content" source="media/sql-insights-enable/connection-string-sql-database.png" alt-text="Verbindungszeichenfolge für die SQL-Datenbank" lightbox="media/sql-insights-enable/connection-string-sql-database.png":::

Zum Überwachen eines lesbaren sekundären Replikats fügen Sie den Schlüsselwert `ApplicationIntent=ReadOnly` in die Verbindungszeichenfolge ein.


#### <a name="azure-virtual-machines-running-sql-server"></a>Azure-VMs mit SQL Server 
Geben Sie Ihre Verbindungszeichenfolge in dieser Form ein:

```
"sqlVmConnections": [ 
   "Server=MyServerIPAddress;Port=1433;User Id=$username;Password=$password;" 
] 
```

Wenn sich der virtuelle Computer für die Überwachung im selben VNET befindet, verwenden Sie die private IP-Adresse des Servers.  Verwenden Sie andernfalls die öffentliche IP-Adresse. Wenn Sie den virtuellen Azure SQL-Computer verwenden, können Sie auf der Seite **Sicherheit** für die Ressource sehen, welcher Port verwendet werden soll.

:::image type="content" source="media/sql-insights-enable/sql-vm-security.png" alt-text="Sicherheit virtueller SQL-Computer" lightbox="media/sql-insights-enable/sql-vm-security.png":::

Zum Überwachen eines lesbaren sekundären Replikats fügen Sie den Schlüsselwert `ApplicationIntent=ReadOnly` in die Verbindungszeichenfolge ein.


### <a name="azure-sql-managed-instances"></a>Verwaltete Azure SQL-Instanzen 
Geben Sie Ihre Verbindungszeichenfolge in dieser Form ein:

```
"sqlManagedInstanceConnections": [ 
      "Server= mysqlserver.database.windows.net;Port=1433;User Id=$username;Password=$password;", 
    ] 
```
Rufen Sie die Details aus dem Menüelement **Verbindungszeichenfolgen** für die verwaltete Instanz ab.


:::image type="content" source="media/sql-insights-enable/connection-string-sql-managed-instance.png" alt-text="SQL Managed Instance-Verbindungszeichenfolge" lightbox="media/sql-insights-enable/connection-string-sql-managed-instance.png":::

Zum Überwachen eines lesbaren sekundären Replikats fügen Sie den Schlüsselwert `ApplicationIntent=ReadOnly` in die Verbindungszeichenfolge ein.



## <a name="monitoring-profile-created"></a>Überwachungs Profil erstellt 

Wählen Sie **Virtuelle Maschine zur Überwachung hinzufügen**, um die virtuelle Maschine so zu konfigurieren, dass sie Daten von Ihren SQL-Ressourcen sammelt. Kehren Sie nicht zur **Übersicht**-Registerkarte zurück.  In ein paar Minuten sollte die Status-Spalte auf "Erfassen" geändert werden, Sie sollten Daten für die SQL-Ressourcen sehen können, die Sie zur Überwachung ausgewählt haben.

Wenn keine Daten angezeigt werden, finden Sie weitere Informationen zum Identifizieren des Problems unter [Problembehandlung in SQL Insights](sql-insights-troubleshoot.md). 

:::image type="content" source="media/sql-insights-enable/profile-created.png" alt-text="Profil erstellt" lightbox="media/sql-insights-enable/profile-created.png":::

> [!NOTE]
> Wenn Sie Ihr Überwachungsprofil oder die Verbindungszeichenfolgen auf Ihren Überwachungs-VMs aktualisieren müssen, können Sie dies über die Registerkarte SQL Insights **Profil verwalten** tun.  Sobald Ihre Aktualisierungen gespeichert wurden, werden die Änderungen in etwa 5 Minuten übernommen.

## <a name="next-steps"></a>Nächste Schritte

- Wenn SQL Insights nach der Aktivierung nicht ordnungsgemäß ausgeführt wird, finden Sie weitere Informationen unter [Problembehandlung in SQL Insights](sql-insights-troubleshoot.md).
