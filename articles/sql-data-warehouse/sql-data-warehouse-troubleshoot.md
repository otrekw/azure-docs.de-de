---
title: Problembehandlung
description: Problembehandlung bei Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 11/25/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: ce57c48e568e840f3a651a5530f3fba6c0be60b7
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721046"
---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Problembehandlung bei Azure SQL Data Warehouse
Dieser Artikel enthält allgemeine Fragen zur Problembehandlung.

## <a name="connecting"></a>Verbindung
| Problem                                                        | Lösung                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Fehler bei der Anmeldung für den Benutzer 'NT-AUTORITÄT\ANONYME ANMELDUNG'. (Microsoft SQL Server, Fehler: 18456) | Dieser Fehler tritt auf, wenn ein AAD-Benutzer versucht, eine Verbindung mit der Masterdatenbank herzustellen, aber nicht über einen Masterdatenbank-Benutzer verfügt.  Zum Beheben dieses Problems geben Sie entweder das SQL Data Warehouse an, mit dem Sie gerade eine Verbindung herstellen möchten, oder fügen Sie den Benutzer der Masterdatenbank hinzu.  Weitere Informationen finden Sie im Artikel [Sichern einer Datenbank in SQL Data Warehouse](sql-data-warehouse-overview-manage-security.md) . |
| Der Serverprinzipal „MeinBenutzername“ kann unter dem aktuellen Sicherheitskontext nicht auf die Datenbank „Master“ zugreifen. Die Standarddatenbank des Benutzers kann nicht geöffnet werden. Fehler bei der Anmeldung. Fehler bei der Anmeldung für den Benutzer 'MeinBenutzername'. (Microsoft SQL Server, Fehler: 916) | Dieser Fehler tritt auf, wenn ein AAD-Benutzer versucht, eine Verbindung mit der Masterdatenbank herzustellen, aber nicht über einen Masterdatenbank-Benutzer verfügt.  Zum Beheben dieses Problems geben Sie entweder das SQL Data Warehouse an, mit dem Sie gerade eine Verbindung herstellen möchten, oder fügen Sie den Benutzer der Masterdatenbank hinzu.  Weitere Informationen finden Sie im Artikel [Sichern einer Datenbank in SQL Data Warehouse](sql-data-warehouse-overview-manage-security.md) . |
| CTAIP-Fehler                                                  | Dieser Fehler kann auftreten, wenn zwar eine Anmeldung für die SQL Server-Masterdatenbank erstellt wurde, dies jedoch in der SQL Data Warehouse-Datenbank unterlassen wurde.  Lesen Sie den [Übersichtsartikel zur Sicherheit](sql-data-warehouse-overview-manage-security.md) , wenn dieser Fehler auftritt.  In diesem Artikel wird erläutert, wie Sie zunächst eine Anmeldung und einen Benutzer für die Masterdatenbank erstellen und anschließend in der SQL Data Warehouse-Datenbank einen Benutzer erstellen. |
| Von der Firewall blockiert                                          | Azure SQL-Datenbanken sind durch Firewalls auf Server- und Datenbankebene geschützt, um sicherzustellen, dass nur bekannte IP-Adressen Zugriff auf eine Datenbank haben. Firewalls sind standardmäßig sicher. Sie müssen daher eine IP-Adresse oder einen Adressbereich explizit aktivieren, bevor Sie eine Verbindung herstellen können.  Um Ihre Firewall für den Zugriff zu konfigurieren, führen Sie die in den [Bereitstellungsanweisungen](sql-data-warehouse-get-started-provision.md) beschriebenen Schritte zum [Konfigurieren des Serverfirewallzugriffs für Ihre Client-IP](sql-data-warehouse-get-started-provision.md) aus. |
| Verbindung mit Tool oder Treiber kann nicht hergestellt werden                           | SQL Data Warehouse empfiehlt die Verwendung von [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15), [SSDT für Visual Studio](sql-data-warehouse-install-visual-studio.md) oder [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) zum Abfragen von Daten. Weitere Informationen zu Treibern und zum Herstellen einer Verbindung mit SQL Data Warehouse finden Sie in den Artikeln [Treiber für Azure SQL Data Warehouse](sql-data-warehouse-connection-strings.md) und [Verbinden mit Azure SQL Data Warehouse](sql-data-warehouse-connect-overview.md). |

## <a name="tools"></a>Tools
| Problem                                                        | Lösung                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| In Visual Studio-Objekt-Explorer fehlen AAD-Benutzer           | Dies ist ein bekanntes Problem.  Sie können die Benutzer in [sys.database_principals](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql?view=sql-server-ver15) anzeigen, um dieses Problem zu umgehen.  Weitere Informationen zur Verwendung von Azure Active Directory mit SQL Data Warehouse finden Sie unter [Authentifizierung in Azure SQL Data Warehouse](sql-data-warehouse-authentication.md) . |
| Manuelle, über den Assistenten für die Skripterstellung erstellte Skripts oder über SSMS hergestellte Verbindungen sind langsam, reagieren nicht mehr oder erzeugen Fehler | Vergewissern Sie sich, dass Benutzer in der Masterdatenbank erstellt wurden. Stellen Sie zudem in den Skriptoptionen sicher, dass die Engine-Edition auf „Microsoft Azure SQL Data Warehouse Edition“ und der Engine-Typ auf „Microsoft Azure SQL-Datenbank“ festgelegt ist. |
| Fehler beim Generieren von Skripts in SSMS                               | Beim Generieren von Skripts für SQL Data Warehouse tritt ein Fehler auf, wenn die Option „Generate script for dependent objects“ (Skript für abhängige Objekte generieren) auf TRUE festgelegt ist. Um dieses Problem zu umgehen, müssen Benutzer manuell zu **Extras > Optionen > SQL Server-Objekt-Explorer > Skript für abhängige Objekte generieren** navigieren und diese Option auf FALSE festlegen. |

## <a name="performance"></a>Leistung
| Problem                                                        | Lösung                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Behandlung von Problemen mit der Abfrageleistung                            | Wenn Sie die Problembehandlung für eine bestimmte Abfrage durchführen möchten, sollten Sie sich zunächst über das [Untersuchen der Ausführung von Abfragen](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md#monitor-query-execution)informieren. |
| TempDB-Speicherplatzprobleme | [Überwachen Sie die Speicherauslastung von TempDB.](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md#monitor-tempdb)  Häufige Ursachen für unzureichenden TempDB-Speicherplatz:<br>- Der Abfrage sind keine ausreichenden Ressourcen zugeordnet, wodurch ein Überlauf der Daten in TempDB stattfindet.  Siehe [Workloadverwaltung](resource-classes-for-workload-management.md). <br>- Fehlende oder veraltete Statistik, wodurch übermäßige Datenverschiebungen stattfinden.  Ausführliche Informationen zum Erstellen von Statistiken finden Sie unter [Tabellenstatistik in Azure SQL Data Warehouse](sql-data-warehouse-tables-statistics.md).<br>- Der TempDB-Speicherplatz wird nach Dienstebene zugeordnet.  [Skalieren Sie Ihr SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md#scaling-compute) auf eine höhere DWU-Einstellung, um mehr TempDB-Speicherplatz zuzuordnen.|
| Schlechte Abfrageleistung und Planung ist häufig das Ergebnis fehlender Statistiken | Die häufigste Ursache für schlechte Leistung ist das Fehlen von Statistiken für Ihre Tabellen.  Ausführliche Informationen dazu, wie Sie Statistiken erstellen und warum sie für die Leistung wichtig sind, finden Sie unter [Tabellenstatistik in Azure SQL Data Warehouse](sql-data-warehouse-tables-statistics.md). |
| Geringe Parallelität/Abfragen in der Warteschlange                             | Das Verständnis der [Workloadverwaltung](resource-classes-for-workload-management.md) ist wichtig, damit Sie wissen, wie Sie die Speicherbelegung und die Parallelität abwägen sollen. |
| Implementieren von bewährten Methoden                              | Wenn Sie die Leistung bei Ihren Abfragen verbessern möchten, ist der Artikel [Bewährte Methoden für SQL Data Warehouse](sql-data-warehouse-best-practices.md) ein idealer Ausgangspunkt. |
| Verbessern der Leistung mit der Skalierung                      | Mitunter besteht der Weg zum Verbessern der Leistung einfach darin, den Abfragen mehr Computeleistung hinzuzufügen, indem Sie Ihr [SQL Data Warehouse skalieren](sql-data-warehouse-manage-compute-overview.md). |
| Schlechte Leistung aufgrund von schlechter Indexqualität     | Es kann vorkommen, dass sich Abfragen aufgrund der [schlechten Qualität des Columnstore-Index](../sql-data-warehouse/sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality) verlangsamen.  Weitere Informationen finden Sie unter [Rebuild indexes to improve segment quality](../sql-data-warehouse/sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality)(Neuerstellen von Indizes zum Verbessern der Segmentqualität). |

## <a name="system-management"></a>Systemverwaltung
| Problem                                                        | Lösung                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Msg 40847: Der Vorgang konnte nicht ausgeführt werden, da der Server das zulässige Datenbanktransaktionseinheit-Kontingent von 45.000 überschreiten würde. | Reduzieren Sie entweder die [DWU](what-is-a-data-warehouse-unit-dwu-cdwu.md) der Datenbank, die Sie erstellen möchten, oder [fordern Sie eine Erhöhung des Kontingents an](sql-data-warehouse-get-started-create-support-ticket.md). |
| Untersuchen der Speicherauslastung                              | Informationen zu den Grundlagen der Speicherauslastung des Systems finden Sie unter [Tabellengrößen]( ../sql-data-warehouse/sql-data-warehouse-tables-overview.md#table-size-queries) . |
| Hilfe beim Verwalten von Tabellen                                    | Hilfreiche Informationen zur Tabellenverwaltung finden Sie in der [Tabellenübersicht][Übersicht].  Dieser Artikel enthält auch Links zu ausführlicheren Themen wie [Tabellendatentypen](sql-data-warehouse-tables-data-types.md), [Verteilen einer Tabelle](sql-data-warehouse-tables-distribute.md), [Indizieren einer Tabelle](sql-data-warehouse-tables-index.md), [Partitionieren einer Tabelle](sql-data-warehouse-tables-partition.md), [Verwalten von Tabellenstatistiken](sql-data-warehouse-tables-statistics.md) und [Temporäre Tabellen](sql-data-warehouse-tables-temporary.md). |
| Die Statusanzeige für Transparent Data Encryption (TDE) wird im Azure-Portal nicht aktualisiert | Sie können den Status von TDE über [PowerShell](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) überprüfen. |


## <a name="differences-from-sql-database"></a>Unterschiede zu SQL-Datenbank
| Problem                                 | Lösung                                                   |
| :------------------------------------ | :----------------------------------------------------------- |
| Nicht unterstützte Funktionen von SQL-Datenbank     | Siehe [Nicht unterstützte Tabellenfunktionen](../sql-data-warehouse/sql-data-warehouse-tables-overview.md#unsupported-table-features). |
| Nicht unterstützte SQL-Datenbank-Datentypen   | Siehe [Nicht unterstützte Datentypen](../sql-data-warehouse/sql-data-warehouse-tables-data-types.md#identify-unsupported-data-types).        |
| DELETE- und UPDATE-Einschränkungen         | Siehe [UPDATE-Problemumgehungen](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements), [DELETE-Problemumgehungen](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements) und [Umgehen von nicht unterstützten Funktionen mit CTAS](sql-data-warehouse-develop-ctas.md). |
| MERGE-Anweisung wird nicht unterstützt      | Siehe [MERGE-Problemumgehungen](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md#replace-merge-statements).                  |
| Einschränkungen für gespeicherte Prozeduren          | In „Gespeicherte Prozeduren in SQL Data Warehouse“ werden unter [Einschränkungen](../sql-data-warehouse/sql-data-warehouse-develop-stored-procedures.md#limitations) einige Einschränkungen für gespeicherte Prozeduren beschrieben. |
| UDFs unterstützen keine SELECT-Anweisungen | Dies ist eine aktuelle Beschränkung unserer benutzerdefinierten Funktionen (User-Defined Functions, UDFs).  Informationen zur unterstützten Syntax finden Sie unter [CREATE FUNCTION](https://docs.microsoft.com/sql/t-sql/statements/create-function-sql-data-warehouse?view=aps-pdw-2016-au7) . |

## <a name="next-steps"></a>Nächste Schritte
Für weitere Hilfe bei der Suche nach einer Lösung für Ihr Problem, stehen Ihnen hier weitere Ressourcen zur Verfügung.

* [Blogs](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
* [Funktionsanfragen](https://feedback.azure.com/forums/307516-sql-data-warehouse)
* [Videos](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
* [Erstellen eines Supporttickets](sql-data-warehouse-get-started-create-support-ticket.md)
* [MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse)
* [Stack Overflow-Forum](https://stackoverflow.com/questions/tagged/azure-sqldw)
* [Twitter](https://twitter.com/hashtag/SQLDW)