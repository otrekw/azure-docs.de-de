---
title: Konfigurieren der Replikation in Azure SQL Edge (Vorschau)
description: Informationen zum Konfigurieren der Replikation in Azure SQL Edge (Vorschau)
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: ad92f796205d84a372de610cb210bbf8878a6c9b
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87282771"
---
# <a name="configure-replication-to-azure-sql-edge-preview"></a>Konfigurieren der Replikation in Azure SQL Edge (Vorschau) 

Sie können eine Azure SQL Edge-Instanz als Pushabonnenten für eine unidirektionale Transaktions- oder Momentaufnahmereplikation konfigurieren. Diese Instanz kann nicht für eine Transaktionsreplikationskonfiguration als Verleger oder Verteiler fungieren. Beachten Sie, dass Mergereplikation, Peer-zu-Peer-Replikation und Oracle Publishing von Azure SQL Edge nicht unterstützt werden.

## <a name="supported-configurations"></a>Unterstützte Konfigurationen
  
- Die Azure SQL Edge-Instanz muss ein Pushabonnent für einen Verleger sein.
- Verleger und Verteiler können Folgendes sein:
   - Eine lokal ausgeführte SQL Server-Instanz oder eine auf einer Azure-VM ausgeführte SQL Server-Instanz. Weitere Informationen finden Sie unter [Was ist SQL Server auf virtuellen Azure-Computern? (Windows)](https://docs.microsoft.com/azure/azure-sql/virtual-machines/). SQL Server-Instanzen müssen eine neuere Version als SQL Server 2016 verwenden.
   - Eine verwaltete Azure SQL-Instanz. SQL Managed Instance kann Verleger-, Verteiler- und Abonnentendatenbanken hosten. Weitere Informationen finden Sie unter [Replikation mit einer verwalteten SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/sql-database/replication-with-sql-database-managed-instance/).

- Die Verteilungsdatenbank und Replikations-Agents können nicht in einer Azure SQL Edge-Instanz platziert werden.  

> [!NOTE]
> Wenn Sie versuchen, die Replikation mit einer nicht unterstützten Version zu konfigurieren, erhalten Sie möglicherweise die folgenden beiden Fehlermeldungen: MSSQL_REPL20084 (Der Prozess konnte keine Verbindung mit dem Abonnenten herstellen.) oder MSSQL_REPL40532 (Der von der Anmeldung angeforderte Server \<name> kann nicht geöffnet werden. Die Anmeldung ist fehlgeschlagen.)  

## <a name="remarks"></a>Bemerkungen

Bei der Konfiguration der Replikation ist es wichtig, die folgenden Anforderungen und bewährten Methoden zu verstehen:

- Sie können die Replikation mithilfe von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) konfigurieren. Sie können dazu auch Transact-SQL-Anweisungen auf dem Verleger entweder in SQL Server Management Studio oder [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) ausführen.
- Um in eine Instanz von Azure SQL Edge zu replizieren, müssen Sie sich mit SQL Server-Authentifizierung anmelden.
- Replizierte Tabellen müssen einen Primärschlüssel aufweisen.
- Eine einzelne Veröffentlichung in SQL Server kann Azure SQL Edge und SQL Server-Abonnenten (lokal und SQL Server auf einer Azure-VM) unterstützen.  
- Verwaltungs-, Überwachungs- und Problembehandlungsvorgänge bei Replikationen müssen über die SQL Server-Instanz durchgeführt werden.  
- Es werden nur Pushabonnements für Azure SQL Edge unterstützt.  
- In der gespeicherten Prozedur `sp_addsubscription` für Azure SQL Edge wird nur `@subscriber_type = 0` unterstützt.  
- Azure SQL Edge unterstützt keine bidirektionale, sofortige, aktualisierbare oder Peer-zu-Peer-Replikation.
- Azure SQL Edge unterstützt nur eine Teilmenge der in SQL Server oder SQL Managed Instance verfügbaren Features. Wenn Sie versuchen, eine Datenbank (oder Objekte innerhalb der Datenbank) zu replizieren, die ein oder mehrere nicht unterstützte Features enthält, schlägt der Versuch fehl. Wenn Sie beispielsweise versuchen, eine Datenbank zu replizieren, die Objekte mit räumlichen Datentypen enthält, erhalten Sie eine Fehlermeldung. Weitere Informationen finden Sie unter [Unterstützte Features von Azure SQL Edge](features.md).

## <a name="initialize-reference-data-on-an-instance-of-azure-sql-edge"></a>Initialisieren von Verweisdaten für eine Azure SQL Edge-Instanz

Möglicherweise möchten Sie Ihre Instanz mit Verweisdaten initialisieren, die sich im Laufe der Zeit ändern. Beispielsweise können Sie Machine Learning-Modelle in Ihrer Instanz von Azure SQL Edge aktualisieren, nachdem Sie in einer SQL Server-Instanz trainiert wurden. Im Folgenden wird erläutert, wie Sie Ihre Instanz in einem solchen Szenario initialisieren:

1. Erstellen Sie eine Transaktionsreplikationsveröffentlichung in einer SQL Server-Datenbank-Instanz.  
2. Erstellen Sie auf der SQL Server-Instanz mit dem **Assistenten für neue Abonnements** oder mithilfe von Transact-SQL-Anweisungen ein Pushabonnement für Azure SQL Edge.  
3. Sie können die replizierte Datenbank für Azure SQL Edge initialisieren, indem Sie eine Momentaufnahme verwenden, die vom Momentaufnahme-Agent generiert und vom Verteilungs-Agent verteilt und bereitgestellt wird. Alternativ können Sie die Initialisierung mit einer Sicherung der Datenbank vom Verleger durchführen. Zur Erinnerung: Wenn die Datenbanksicherung Objekte oder Features enthält, die nicht von Azure SQL Edge unterstützt werden, schlägt der Wiederherstellungsvorgang fehl.

## <a name="limitations"></a>Einschränkungen

Die folgenden Optionen werden für Azure SQL Edge-Abonnements nicht unterstützt:

- Kopieren einer Dateigruppenzuordnung  
- Kopieren von Tabellenpartitionierungsschemas  
- Kopieren von Indexpartitionierungsschemas  
- Kopieren von benutzerdefinierten Statistiken  
- Kopieren von Standardbindungen  
- Kopieren von Regelbindungen  
- Kopieren von Volltextindizes  
- Kopieren einer XML-XSD  
- Kopieren von XML-Indizes  
- Kopieren von Berechtigungen  
- Kopieren von räumlichen Indizes  
- Kopieren von gefilterten Indizes  
- Kopieren von Datenkomprimierungsattributen  
- Kopieren von Sparsespaltenattributen  
- Kopieren von Filestream-, `hierarchyid`- oder räumlichen Datentypen
- Konvertieren von `hierarchyid` in MAX-Datentypen  
- Konvertieren von spatial in MAX-Datentypen  
- Kopieren von erweiterten Eigenschaften  
- Kopieren von Berechtigungen  

## <a name="examples"></a>Beispiele

Erstellen Sie eine Veröffentlichung und ein Pushabonnement. Weitere Informationen finden Sie unter
  
- [Erstellen einer Veröffentlichung](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Erstellen Sie ein Pushabonnement](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/), indem Sie den Azure SQL Edge-Servernamen bzw. die IP-Adresse als Abonnenten (z. B. **myEdgeinstance,1433**) und einen Datenbanknamen für die Azure SQL Edge-Instanz als Zieldatenbank (z. B. **AdventureWorks**) verwenden.  

## <a name="next-steps"></a>Nächste Schritte  

- [Erstellen einer Veröffentlichung](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Erstellen eines Pushabonnements](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Replikationstypen](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [Überwachen (Replikation)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Initialisieren eines Abonnements](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  


