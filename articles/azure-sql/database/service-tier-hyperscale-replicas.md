---
title: Sekundäre Hyperscale-Replikate
description: In diesem Artikel werden die unterschiedlichen Arten sekundärer Replikate beschrieben, die auf der Hyperscale-Dienstebene verfügbar sind.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: overview
author: yorek
ms.author: damauri
ms.reviewer: ''
ms.date: 6/9/2021
ms.openlocfilehash: 3c3ff0dd5e8a9dd0b17aad116215a1f6a254d139
ms.sourcegitcommit: d137460f55a38a0e8f8b9e6594e480d5e5f662ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2021
ms.locfileid: "112428879"
---
# <a name="hyperscale-secondary-replicas"></a>Sekundäre Hyperscale-Replikate
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Wie unter [Hyperscale-Dienstebene](service-tier-hyperscale.md) beschrieben bietet die Hyperscale-Dienstebene in Azure SQL-Datenbank zwei verschiedene Arten von Computeknoten, die auch als „Replikate“ bezeichnet werden.
- Primär: Dieser Knoten wird für Lese- und Schreibvorgänge verwendet.
- Sekundär: Dieser Knoten bieten horizontale Leseskalierung, Hochverfügbarkeit und Georeplikation.

Ein sekundäres Replikat kann eine von drei verschiedenen Typen aufweisen:

- Hochverfügbarkeitsreplikat
- Benanntes Replikat (Vorschauversion)
- Georeplikation (Vorschauversion)

Jeder Typ verfügt über eine unterschiedliche Architektur und über verschiedene Features, Zwecke und Kosten. Je nach Features, die Sie benötigen, können Sie nur einen oder alle drei Typen zusammen verwenden.

## <a name="high-availability-replica"></a>Hochverfügbarkeitsreplikat

Ein Hochverfügbarkeitsreplikat verwendet dieselben Seitenserver wie das primäre Replikat, es sind also keine Datenkopien erforderlich, um ein Hochverfügbarkeitsreplikat hinzuzufügen. Hochverfügbarkeitsreplikate werden hauptsächlich verwendet, um Hochverfügbarkeit bieten zu können, da sie zu Failoverzwecken als unmittelbar betriebsbereite Standbyserver verwendet werden können. Wenn die Verfügbarkeit des primären Replikats nicht gegeben ist, erfolgt das Failover zu einem der vorhandenen Hochverfügbarkeitsreplikate automatisch. Die Verbindungszeichenfolge muss nicht geändert werden. Während eines Failovers kann für Anwendungen eine minimale Downtime auftreten. Dies ist aktiven Verbindungen geschuldet, die getrennt werden. Wie gewöhnlich wird für dieses Szenario eine geeignete Wiederholungslogik für Verbindungen empfohlen. Mehrere Treiber bieten zu einem gewissen Grad bereits eine automatische Wiederholungslogik. 

Wenn Sie .NET verwenden, bietet die aktuelle Version der Bibliothek [Microsoft.Data.SqlClient](https://devblogs.microsoft.com/azure-sql/configurable-retry-logic-for-microsoft-data-sqlclient/) nativ vollständige Unterstützung für eine konfigurierbare automatische Wiederholungslogik. Hochverfügbarkeitsreplikate verwenden denselben Server- und Datenbanknamen wie das primäre Replikat. Das Servicelevelziel dieser Replikate ist oft identisch mit dem des primären Replikats. Hochverfügbarkeitsreplikate können nicht als eigenständige Ressource über das Portal oder über ein anderes Tool oder eine andere dynamische Verwaltungssicht verwaltet werden. 

Es kann null bis vier Hochverfügbarkeitsreplikate geben. Die Anzahl kann während der Erstellung einer Datenbank oder nach Erstellung einer Datenbank über den üblichen Verwaltungsendpunkt und die üblichen Verwaltungstools (z. B. PowerShell, Azure CLI, Azure-Portal, REST-API) geändert werden. Das Erstellen oder Entfernen von Hochverfügbarkeitsreplikaten wirkt sich nicht auf Verbindungen aus, die auf dem primären Replikat ausgeführt werden.

### <a name="connecting-to-an-ha-replica"></a>Herstellen einer Verbindung zu einem Hochverfügbarkeitsreplikat

Bei Hyperscale-Datenbanken bestimmt das Argument „ApplicationIntent“ in der vom Client verwendeten Verbindungszeichenfolge, ob eine Verbindung an das primäre Replikat mit Schreibzugriff oder an ein schreibgeschütztes Hochverfügbarkeitsreplikat weitergeleitet wird. Wenn ApplicationIntent auf `ReadOnly` festgelegt ist und die Datenbank kein sekundäres Replikat aufweist, wird die Verbindung an das primäre Replikat weitergeleitet, und sie weist standardmäßig das Verhalten `ReadWrite` auf.

```csharp
-- Connection string with application intent
Server=tcp:<myserver>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Angesichts der Tatsache, dass für eine gegebene Hyperscale-Datenbank alle Hochverfügbarkeitsreplikate im Hinblick auf ihre Ressourcenkapazität identisch bleiben, wird die Workload mit Leseabsicht auf alle verfügbaren sekundären Hochverfügbarkeitsreplikate verteilt, wenn mehr als ein sekundäres Replikat vorhanden ist. Wenn es mehrere Hochverfügbarkeitsreplikate gibt, denken Sie daran, dass jedes im Hinblick auf am primären Replikat vorgenommene Datenänderungen über eine unterschiedliche Datenlatenz verfügen könnte. Jedes Hochverfügbarkeitsreplikat verwendet dieselben Daten wie das primäre Replikat für dieselben Seitenserver. Lokale Caches auf den einzelnen Hochverfügbarkeitsreplikaten spiegeln die am primären Replikat vorgenommenen Änderungen über den Transaktionsprotokolldienst wieder, der Protokolldatensätze vom primären Replikat an Hochverfügbarkeitsreplikate weiterleitet. Dies führt dazu, dass die Anwendung von Protokolldatensätzen, je nach Workload, die von einem Hochverfügbarkeitsreplikat verarbeitet wird, mit unterschiedlicher Geschwindigkeit durchgeführt werden kann, was dazu führen kann, dass verschiedene Replikate eine unterschiedliche Datenlatenz im Hinblick auf das primäre Replikat aufweisen können.

## <a name="named-replica-in-preview"></a>Benanntes Replikat (Vorschauversion)

Ein benanntes Replikat verwendet genau wie ein Hochverfügbarkeitsreplikat dieselben Seitenserver wie das primäre Replikat. Ähnlich wie Hochverfügbarkeitsreplikate sind keine Datenkopien erforderlich, um ein benanntes Replikat hinzuzufügen. 

> [!NOTE]
> Antworten auf häufig gestellte Fragen zu benannten Hyperscale-Replikaten finden Sie unter [Benannte Hyperscale-Replikate in Azure SQL-Datenbank: Häufig gestellte Fragen](service-tier-hyperscale-named-replicas-faq.yml).

Benannte Replikate unterscheiden sich in den folgenden Punkten von Hochverfügbarkeitsreplikaten: 

- Benannte Replikate werden im Azure-Portal und in API-Aufrufen (CLI, PowerShell, T-SQL) als reguläre (schreibgeschützte) Azure SQL-Datenbank-Instanzen angezeigt. 
- Benannte Replikate können Datenbanknamen haben, die sich vom primären Replikat unterscheiden. Optional können sie sich auf einem anderen logischen Server befinden, solange es sich um dieselbe Region wie die des primären Replikats handelt. 
- Benannte Replikate verfügen über ein eigenes Servicelevelziel, das unabhängig vom primären Replikat festgelegt und geändert werden kann.
- Es werden bis zu 30 benannte Replikate unterstützt (pro primärem Replikat) 
- Es werden verschiedene Authentifizierungs- und Autorisierungslösungen für jedes benannte Replikat unterstützt, indem verschiedene Anmeldungen für logische Server erstellt werden, die benannte Replikate hosten.

Das Hauptziel benannter Replikate besteht darin, große OLTP-Szenarios mit horizontaler Leseskalierung zu ermöglichen und HTAP-Workloads (Hybrid Transactional and Analytical Processing, hybride Transaktions- und Analyseverarbeitung) zu optimieren. Beispiele für das Erstellen solcher Lösungen finden Sie hier:

- [OLTP: Beispiel für horizontale Skalierung](https://github.com/Azure-Samples/azure-sql-db-named-replica-oltp-scaleout)
- [HTAP: Beispiel für horizontale Skalierung](https://github.com/Azure-Samples/azure-sql-db-named-replica-htap)

Abgesehen von den oben aufgeführten Hauptszenarios bieten benannte Replikate Flexibilität und Elastizität, sodass sie auch für viele anderen Anwendungsfälle geeignet sind:
- [Zugriffsisolation:](hyperscale-named-replica-security-configure.md) Gewähren Sie einem benannten Replikat ausschließlich Anmeldezugriff auf ein benanntes Replikat, und verhindern Sie, dass es auf das primäre Replikat oder andere benannte Replikate zugreifen kann.
- Workloadabhängige Servicelevelziele: Da ein benanntes Replikat über ein eigenes Servicelevelziel verfügen kann, ist es möglich, andere benannte Replikate für andere Workloads und Anwendungsfälle zu verwenden. Ein benanntes Replikat könnte beispielsweise für Power BI-Anforderungen verwendet werden, während ein anderes für Daten für Data-Science-Aufgaben in Apache Spark verwendet werden kann. Jedes kann über ein unabhängiges Servicelevelziel verfügen und unabhängig skaliert werden.
- Workloadabhängiges Routing: Mit bis zu 30 benannten Replikaten ist es möglich, benannte Replikate in Gruppen zu verwenden, damit eine Anwendung von einer anderen isoliert werden kann. Sie könnten beispielsweise eine Gruppe vier benannter Replikate verwenden, um Anforderungen zu verarbeiten, die von mobilen Anwendungen stammen. Eine andere aus zwei benannten Replikaten bestehende Gruppe kann verwendet werden, um Anforderungen zu verarbeiten, die von einer Webanwendung stammen. Dieser Ansatz ermöglicht eine genaue Anpassung der Leistung und der Kosten der einzelnen Gruppen.

Im folgenden Beispiel wird das benannte Replikat `WideWorldImporters_NR` für die Datenbank `WideWorldImporters` mit dem Servicelevelziel HS_Gen5_4 erstellt. Beide verwenden denselben logischen Server `MyServer`. Wenn Sie lieber direkt eine REST-API verwenden möchten, ist auch die folgende Option möglich: [Datenbanken: Erstellen einer Datenbank als benanntes sekundäres Replikat](/rest/api/sql/2020-11-01-preview/databases/createorupdate#creates-a-database-as-named-replica-secondary).

# <a name="t-sql"></a>[T-SQL](#tab/tsql)
```sql
ALTER DATABASE [WideWorldImporters]
ADD SECONDARY ON SERVER [MyServer] 
WITH (SERVICE_OBJECTIVE = 'HS_Gen5_2', SECONDARY_TYPE = Named, DATABASE_NAME = [WideWorldImporters_NR]);
```
# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```azurepowershell
New-AzSqlDatabaseSecondary -ResourceGroupName "MyResourceGroup" -ServerName "MyServer" -DatabaseName "WideWorldImporters" -PartnerResourceGroupName "MyResourceGroup" -PartnerServerName "MyServer" -PartnerDatabaseName "WideWorldImporters_NR_" -SecondaryServiceObjectiveName HS_Gen5_2
```
# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
```azurecli
az sql db replica create -g MyResourceGroup -n WideWorldImporters -s MyServer --secondary-type named --partner-database WideWorldImporters_NR --partner-server MyServer --service-objective HS_Gen5_2
```

---

Da keine Datenverschiebungen erforderlich sind, kann ein benanntes Replikat in den meisten Fällen in etwa einer Minute erstellt werden. Sobald das benannte Replikat verfügbar ist, kann es im Portal oder mit einem beliebigem Befehlszeilentool wie der Azure CLI oder PowerShell angezeigt werden. Ein benanntes Replikat kann als reguläre Datenbank verwendet werden, mit der Ausnahme, dass es schreibgeschützt ist. 

### <a name="connecting-to-a-named-replica"></a>Herstellen einer Verbindung zu einem benannten Replikat

Zum Herstellen einer Verbindung zu einem benannten Replikat müssen Sie die Verbindungszeichenfolge für dieses benannte Replikat verwenden. Die Option „ApplicationIntent“ muss nicht angegeben werden, da benannte Replikate immer schreibgeschützt sind. Sie können die Option dennoch verwenden, sie hat jedoch keine Auswirkungen.
Wie bei Hochverfügbarkeitsreplikaten werden die Caches auf den einzelnen benannten Replikaten mit dem primären Replikat über den Transaktionsprotokolldienst synchronisiert, obwohl das primäre Replikat, die Hochverfügbarkeitsreplikate und die benannten Replikate dieselben Daten auf denselben Seitenservern gemeinsam nutzen. Der Transaktionsprotokolldienst leitet Protokolldatensätze vom primären Replikat an die benannten Replikate weiter. Dies führt dazu, dass die Anwendung von Protokolldatensätzen, je nach Workload, die von einem benannten Replikat verarbeitet wird, mit unterschiedlicher Geschwindigkeit durchgeführt werden kann, was dazu führen kann, dass verschiedene Replikate eine unterschiedliche Datenlatenz im Hinblick auf das primäre Replikat aufweisen können.

### <a name="modifying-a-named-replica"></a>Ändern eines benannten Replikats

Sie können das Servicelevelziel eines benannten Replikats bei seiner Erstellung definieren. Dies erfolgt über den Befehl `ALTER DATABASE` oder auf andere unterstützte Weise (Azure CLI, PowerShell, REST-API). Wenn Sie das Servicelevelziel ändern möchten, nachdem das Replikat erstellt wurde, können Sie dies mithilfe des regulären Befehls `ALTER DATABASE…MODIFY` für das benannte Replikat selbst tun. Wenn es sich beispielsweise bei `WideWorldImporters_NR` um das benannte Replikat der Datenbank `WideWorldImporters` handelt, können Sie die unten dargestellte Vorgehensweise verwenden. 

# <a name="t-sql"></a>[T-SQL](#tab/tsql)
```sql
ALTER DATABASE [WideWorldImporters_NR] MODIFY (SERVICE_OBJECTIVE = 'HS_Gen5_4')
```
# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```azurepowershell
Set-AzSqlDatabase -ResourceGroup "MyResourceGroup" -ServerName "MyServer" -DatabaseName "WideWorldImporters_NR" -RequestedServiceObjectiveName "HS_Gen5_4"
```
# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
```azurecli
az sql db update -g MyResourceGroup -s MyServer -n WideWorldImporters_NR --service-objective HS_Gen5_4
```

---

### <a name="removing-a-named-replica"></a>Entfernen eines benannten Replikats

Wenn Sie ein benanntes Replikat entfernen möchten, ist die Vorgehensweise identisch mit der für reguläre Datenbanken. Sorgen Sie dafür, dass Sie mit der Datenbank `master` des Servers verbunden sind, der das benannte Replikat enthält, das Sie löschen möchten. Verwenden Sie anschließend den folgenden Befehl:

# <a name="t-sql"></a>[T-SQL](#tab/tsql)
```sql
DROP DATABASE [WideWorldImporters_NR];
```
# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```azurepowershell
Remove-AzSqlDatabase -ResourceGroupName "MyResourceGroup" -ServerName "MyServer" -DatabaseName "WideWorldImporters_NR"
```
# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
```azurecli
az sql db delete -g MyResourceGroup -s MyServer -n WideWorldImporters_NR
```
---

> [!NOTE]
> Benannte Replikate werden auch entfernt, wenn das primäre Replikat, auf deren Grundlage sie erstellt wurden, gelöscht wird.

### <a name="known-issues"></a>Bekannte Probleme

#### <a name="partially-incorrect-data-returned-from-sysdatabases"></a>Teilweise fehlerhafte von sys.databases zurückgegebene Daten
Während der öffentlichen Vorschau ist es möglich, dass Zeilenwerte, die von `sys.databases` zurückgegeben werden, für benannte Replikate in Spalten außer `name` und `database_id` nicht konsistent oder korrekt sind. Für die Spalte `compatibility_level` eines benannten Replikats könnte beispielsweise der Wert 140 gemeldet werden, selbst wenn die primäre Datenbank, auf deren Grundlage das benannte Replikat erstellt wurde, auf 150 festgelegt ist. Eine Umgehung, sofern möglich, ist das Abrufen derselben Daten mithilfe der Systemfunktion `databasepropertyex`, die stattdessen die korrekten Daten zurückgibt. 


## <a name="geo-replica-in-preview"></a>Georeplikation (Vorschauversion)

Mit [aktiver Georeplikation](active-geo-replication-overview.md) können Sie ein lesbares sekundäres Replikat der primären Hyperscale-Datenbank in derselben oder einer anderen Region erstellen. Georeplikate müssen auf einem anderen logischen Server erstellt werden. Der Datenbankname eines Georeplikats stimmt immer mit dem Datenbanknamen des primären Replikats überein.

Wenn ein Georeplikat erstellt wird, werden alle Daten vom primären Replikat auf andere Seitenserver kopiert. Ein Georeplikat teilt seine Seitenserver nicht mit dem primären Replikat, selbst wenn sie sich in derselben Region befinden. Diese Architektur bietet die notwendige Redundanz für Geofailover.

Georeplikate werden hauptsächlich verwendet, um eine bei Transaktionen konsistente Kopie der Datenbank über eine asynchrone Replikation in eine andere geografische Region sicherzustellen. Diese kann dann für Notfallwiederherstellungen im Fall eines Notfalls oder Ausfalls in der primären Region verwendet werden. Georeplikate können auch für geografische Szenarios mit horizontaler Leseskalierung verwendet werden.

Bei [aktiver Georeplikation in Hyperscale](active-geo-replication-overview.md) müssen Failover manuell initiiert werden. Nach dem Failover verfügt das primäre Replikat über einen anderen Verbindungsendpunkt, der auf den logischen Servername verweist, der das neue primäre Replikat hostet. Weitere Informationen finden Sie unter [Erstellen und Verwenden der aktiven Georeplikation: Azure SQL-Datenbank](active-geo-replication-overview.md).

Die Georeplikation für Hyperscale-Datenbanken befindet sich aktuell in der Vorschau und weist die folgenden Einschränkungen auf:
- Nur ein Georeplikat kann (in derselben oder einer anderen Region) erstellt werden.
- Failovergruppen werden nicht unterstützt. 
- Geplante Failover werden nicht unterstützt.
- Point-in-Time-Wiederherstellungen von Georeplikaten werden nicht unterstützt.
- Das Erstellen einer Datenbankkopie des Georeplikats wird nicht unterstützt. 
- Ein sekundäres Replikat eines sekundären Replikats (auch bekannt als „Verkettung von Georeplikaten“) wird nicht unterstützt. 

## <a name="next-steps"></a>Nächste Schritte

- [Hyperscale-Dienstebene](service-tier-hyperscale.md)
- [Aktive Georeplikation](active-geo-replication-overview.md)
- [Konfigurieren von isoliertem Zugriff auf ein benanntes Hyperscale-Replikat](hyperscale-named-replica-security-configure.md)
- [Benannte Hyperscale-Replikate in Azure SQL-Datenbank: Häufig gestellte Fragen](service-tier-hyperscale-named-replicas-faq.yml)
