---
title: Konfigurieren der Replikation in Azure SQL Edge (Vorschau)
description: Informationen zum Konfigurieren der Replikation in Azure SQL Edge (Vorschau)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: e2b37e0f3ccf5fcebe4723c05d644f2cbb7c1d56
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83593989"
---
# <a name="configure-replication-to-azure-sql-edge-preview"></a>Konfigurieren der Replikation in Azure SQL Edge (Vorschau) 

Die Azure SQL Edge-Instanz kann als Pushabonnent für eine unidirektionale Transaktionsreplikation oder eine Momentaufnahmereplikation konfiguriert werden. Die Azure SQL Edge-Instanz kann nicht als Verleger oder Verteiler für eine Transaktionsreplikationskonfiguration fungieren. Mergereplikation, P2P-Replikation und Oracle Publishing werden mit Azure SQL Edge nicht unterstützt.

## <a name="supported-configurations"></a>**Unterstützte Konfigurationen**:
  
- Die Azure SQL Edge-Instanz muss ein Pushabonnent für einen Verleger sein.
- Der Verleger und der Verteiler können Folgendes sein:
   - Eine SQL Server-Instanz, die lokal ausgeführt wird, oder eine Instanz von SQL Server, die auf einer Azure-VM ausgeführt wird. Weitere Informationen finden Sie unter [Was ist SQL Server auf virtuellen Azure-Computern? (Windows)](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-infrastructure-services/). SQL Server-Instanzen müssen eine Version verwenden, die größer als SQL Server 2016 ist.
   - Eine verwaltete Azure SQL-Datenbank-Instanz. Eine verwaltete Instanz kann Verleger-, Verteiler- und Abonnentendatenbanken hosten. Weitere Informationen finden Sie unter [Replikation mit einer verwalteten SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/sql-database/replication-with-sql-database-managed-instance/).

- Die Verteilungsdatenbank und die Replikations-Agents können nicht in einer Azure SQL Edge-Instanz platziert werden.  

> [!NOTE]
> Der Versuch, Replikationen mit einer nicht unterstützten Version zu konfigurieren, kann zu dem Fehler mit der Nummer MSSQL_REPL20084 (Der Prozess konnte keine Verbindung mit dem Abonnenten herstellen.) oder MSSQL_REPL40532 (Der von der Anmeldung angeforderte Server \<Name> kann nicht geöffnet werden. Fehler bei der Anmeldung.) führen.  

Um alle Features von Azure SQL Edge verwenden zu können, müssen Sie die neuesten Versionen von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) und [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) verwenden.  

## <a name="remarks"></a>Bemerkungen

- Die Replikation kann mithilfe von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) oder durch Ausführen von Transact-SQL-Anweisungen im Verleger mit SQL Server Management Studio oder [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) konfiguriert werden.
- Die Replikation kann nur SQL Server-Authentifizierungsanmeldungen verwenden, um eine Verbindung mit einer Azure SQL Edge-Instanz herzustellen.
- Replizierte Tabellen müssen einen Primärschlüssel aufweisen.
- Eine einzelne Veröffentlichung in SQL Server kann Azure SQL Edge und SQL Server-Abonnenten (lokal und SQL Server auf einer Azure-VM) unterstützen.  
- Die Verwaltung, Überwachung und Problembehandlung bei Replikationen müssen über die lokale SQL Server-Instanz ausgeführt werden.  
- Es werden nur Pushabonnements für Azure SQL Edge unterstützt.  
- Nur `@subscriber_type = 0` wird in **sp_addsubscription** für Azure SQL Edge unterstützt.  
- Azure SQL Edge unterstützt keine bidirektionale, sofortige, aktualisierbare oder Peer-zu-Peer-Replikation.
- Azure SQL Edge unterstützt nur eine Teilmenge der Features, die in SQL Server oder der verwalteten Azure SQL-Datenbank-Instanz verfügbar sind, da ein solcher Versuch, eine Datenbank (oder Objekte innerhalb der Datenbank) zu replizieren, die mindestens eine nicht unterstützte Funktion enthält, zu einem Fehler führt. Wenn Sie z. B. versuchen, eine Datenbank zu replizieren, die Objekte mit räumlichen Datentypen enthält, führt dies zu einem Fehler. Weitere Informationen zu Funktionen, die von Azure SQL Edge unterstützt werden, finden Sie unter [Unterstützte Features von Azure SQL Edge](features.md).

## <a name="scenarios"></a>Szenarien  

### <a name="initializing-reference-data-on-an-edge-instance"></a>Initialisieren von Verweisdaten für eine Edge-Instanz

Ein häufiges Szenario, in dem Replikation nützlich sein kann, ist die Notwendigkeit, die Edge-Instanz mit Verweisdaten zu initialisieren, die sich im Laufe der Zeit ändern. Beispielsweise das Aktualisieren von ML-Modellen für die Edge-Instanz, nachdem Sie mit einer lokalen SQL Server-Instanz trainiert wurden.

1. Erstellen Sie eine Transaktionsreplikationsveröffentlichung in einer lokalen SQL Server-Datenbank-Instanz.  
2. Erstellen Sie in der lokalen SQL Server-Instanz mit dem **Assistenten für neue Abonnements** oder mithilfe von Transact-SQL-Anweisungen ein Pushabonnement für Azure SQL Edge.  
3. Die replizierte Datenbank in Azure SQL Edge kann entweder mithilfe einer vom Momentaufnahme-Agent generierten Momentaufnahme initialisiert und vom Verteilungs-Agent verteilt und bereitgestellt werden, oder es wird eine Sicherung der Datenbank vom Verleger verwendet. Auch hier gilt: Wenn die Datenbanksicherung Objekte/Features enthält, die nicht von Azure SQL Edge unterstützt werden, tritt beim Wiederherstellungsvorgang ein Fehler auf.

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
- Kopieren von Filestream-, Hierarchyid- oder Spatial-Datentypen.
- Konvertieren von hierarchyid in MAX-Datentypen  
- Konvertieren von spatial in MAX-Datentypen  
- Kopieren von erweiterten Eigenschaften  
- Kopieren von Berechtigungen  

## <a name="examples"></a>Beispiele

Erstellen Sie eine Veröffentlichung und ein Pushabonnement. Weitere Informationen finden Sie unter
  
- [Erstellen einer Veröffentlichung](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Erstellen Sie ein Pushabonnement](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/), indem Sie den Azure SQL Edge-Servernamen biw. die IP-Adresse als Abonnenten (z. B. **myEdgeinstance,1433**) und einen Datenbanknamen für die Azure SQL Edge-Instanz als Zieldatenbank (z. B. **AdventureWorks**) verwenden.  

## <a name="see-also"></a>Weitere Informationen  

- [Erstellen einer Veröffentlichung](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Erstellen eines Pushabonnements](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Replikationstypen](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [Überwachen (Replikation)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Initialisieren eines Abonnements](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  


