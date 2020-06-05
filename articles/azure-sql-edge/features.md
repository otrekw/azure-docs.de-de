---
title: Unterstützte Features von Azure SQL Edge (Vorschau)
description: Erfahren Sie mehr über die Details der von Azure SQL Edge unterstützten Funktionen (Vorschau).
keywords: Einführung in SQL Edge, was ist SQL Edge, Übersicht über SQL Edge
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: e3bac1cbb0e57f9be4a489207f2fe8b30b5d457e
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84233207"
---
# <a name="supported-features-of-azure-sql-edge-preview"></a>Unterstützte Features von Azure SQL Edge (Vorschau) 

Dieser Artikel beschreibt die Details der von Azure SQL Edge unterstützten Funktionen. Azure SQL Edge baut auf der neuesten Version der Microsoft SQL Server-Datenbank-Engine unter Linux auf und unterstützt eine Teilmenge der in SQL Server 2019 für Linux unterstützten Features sowie einige Funktionen, die derzeit in SQL Server 2019 unter Linux oder Windows nicht unterstützt werden oder nicht verfügbar sind. Eine vollständige Liste der Funktionen, die in SQL Server für Linux unterstützt werden, finden Sie unter [Editionen und unterstützte Funktionen von SQL Server 2019 unter Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-editions-and-components-2019). Informationen zu den Editionen und unterstützten Funktionen von SQL Server unter Windows finden Sie unter [Editionen und unterstützte Funktionen von SQL Server 2019 (15.x)](https://docs.microsoft.com/sql/sql-server/editions-and-components-of-sql-server-version-15).

> [!NOTE]
> Azure SQL Edge ist derzeit als Vorschau verfügbar und sollte daher NICHT in Produktionsumgebungen verwendet werden. Microsoft empfiehlt möglicherweise, Azure SQL Edge nach der Überprüfung der Bereitstellung und der Anwendungsfallszenarien in Produktionsumgebungen zu verwenden.

## <a name="azure-sql-edge-editions"></a>Azure SQL Edge-Editionen

SQL Database Edge wird in zwei unterschiedlichen Editionen oder Softwareplänen bereitgestellt. Diese Editionen weisen denselben Umfang an Features auf und unterscheiden sich lediglich in Bezug auf die Nutzungsrechte und die Menge an CPU-/Arbeitsspeicherressourcen, auf die sie auf dem Hostsystem zugreifen können.

   |**Planen**  |**Beschreibung**  |
   |---------|---------|
   |Azure SQL Edge Developer  |  Reine Entwicklungs-SKU, jeder Azure SQL Edge Developer-Container ist auf bis zu vier Kerne und 32 GB Arbeitsspeicher beschränkt.  |
   |Azure SQL Edge    |  Produktions-SKU, jeder Azure SQL Edge-Container ist auf bis zu acht Kerne und 64 GB Arbeitsspeicher beschränkt.  |

## <a name="operating-system"></a>Betriebssystem

Azure SQL Edge-Container basieren derzeit auf Ubuntu 16.04 und werden daher nur für die Ausführung auf Docker-Hosts mit Ubuntu 16.04 (empfohlene Version) oder Ubuntu 18.04 unterstützt. Azure SQL Edge kann auch auf anderen Betriebssystemhosts ausgeführt werden, z. B. mit anderen Distributionen von Linux oder Windows (mithilfe von Docker CE oder Docker EE). Diese Konfigurationen werden jedoch von Microsoft nicht ausgiebig getestet.

Azure SQL Edge wird zurzeit nur für die Bereitstellung über Azure IoT Edge unterstützt. Weitere Informationen zu den unterstützten Systemen für Azure IoT Edge finden Sie unter [Von Azure IoT Edge unterstützte Systeme](https://docs.microsoft.com/azure/iot-edge/support).

Die empfohlene Konfiguration für die Ausführung von Azure SQL Edge unter Windows besteht darin, ein Ubuntu-VM auf dem Windows-Host zu konfigurieren und dann SQL Edge innerhalb der Linux-VM auszuführen.

## <a name="hardware-support"></a>Hardwareunterstützung

Azure SQL Edge erfordert einen 64-Bit-Prozessor (von Intel, AMD oder ARM) mit mindestens einem Prozessor und einem GB RAM auf dem Host. Während der Startspeicherbedarf von Azure SQL Edge ungefähr 500 MB beträgt, wird zusätzlicher Arbeitsspeicher für andere IoT Edge-Module benötigt, die auf dem Edge-Gerät ausgeführt werden.

## <a name="azure-sql-edge-components"></a>Azure SQL Edge-Komponenten

Azure SQL Edge unterstützt nur die Datenbank-Engine und bietet keine Unterstützung für andere Komponenten, die mit SQL Server 2019 unter Windows oder mit SQL Server 2019 unter Linux verfügbar sind. Insbesondere unterstützt Azure SQL Edge keine SQL Server-Komponenten wie Analysis Services, Reporting Services, Integration Services, Master Data Services, Machine Learning Services ( datenbankintern) und Machine Learning Server (eigenständig).

## <a name="supported-features"></a>Unterstützte Funktionen

Zusätzlich zur Unterstützung einer Teilmenge der Features von SQL Server unter Linux bietet Azure SQL Edge Unterstützung für die folgenden neuen Features. 

- SQL-Streaming, das auf derselben Engine basiert, die Azure Stream Analytics unterstützt, bietet Datenstreamingfunktionen in Echtzeit in Azure SQL Edge. 
- Neuer T-SQL-Funktionsaufruf Date_Bucket für die Analyse von Zeitreihendaten.
- Machine Learning-Funktionen über die ONNX-Runtime, die in der SQL-Engine enthalten ist.

## <a name="sql-server-on-linux-features-not-supported-in-azure-sql-edge"></a>Features von SQL Server unter Linux, die in Azure SQL Edge nicht unterstützt werden

In der folgenden Liste sind die Features von SQL Server 2019 unter Linux enthalten, die in Azure SQL Edge derzeit nicht unterstützt werden.

| Bereich | Nicht unterstütztes Feature oder Dienst |
|-----|-----|
| **Datenbankentwurf** | In-Memory-OLTP und zugehörige DDL-Befehle und Transact-SQL-Funktionen, Katalogsichten und dynamische Verwaltungssichten |
| &nbsp; | HierarchyID-Datentyp und zugehörige DDL-Befehle und Transact-SQL-Funktionen, Katalogsichten und dynamische Verwaltungssichten |
| &nbsp; | Spatial-Datentyp und zugehörige DDL-Befehle und Transact-SQL-Funktionen, Katalogsichten und dynamische Verwaltungssichten |
| &nbsp; | Stretch Database und zugehörige DDL-Befehle und Transact-SQL-Funktionen, Katalogsichten und dynamische Verwaltungssichten |
| &nbsp; | Volltextindizes und -suche und zugehörige DDL-Befehle und Transact-SQL-Funktionen, Katalogsichten und dynamische Verwaltungssichten|
| &nbsp; | FileTable, FILESTREAM und zugehörige DDL-Befehle und Transact-SQL-Funktionen, Katalogsichten und dynamische Verwaltungssichten|
| **Datenbank-Engine** | Replikation. Azure SQL Edge kann jedoch als Pushabonnent einer Replikationstopologie konfiguriert werden. |
| &nbsp; | Polybase. Azure SQL Edge kann jedoch als Ziel für externe Tabellen in Polybase konfiguriert werden. |
| &nbsp; | Spracherweiterbarkeit über JavaScript und Spark |
| &nbsp; | Active Directory-Integration |
| &nbsp; | Datenbank-Momentaufnahmen |
| &nbsp; | Unterstützung für persistenten Speicher |
| &nbsp; | Microsoft Distributed Transaction Coordinator |
| &nbsp; | Resource Governor und Ressourcenkontrolle für E/A-Vorgänge |
| &nbsp; | Pufferpoolerweiterung |
| &nbsp; | Verteilte Abfrage mit Drittanbieterverbindungen |
| &nbsp; | Verbindungsserver |
| &nbsp; | Erweiterte gespeicherte Systemprozeduren (XP_CMDSHELL usw.) |
| &nbsp; | CLR-Assemblys und zugehörige DDL-Befehle und Transact-SQL-Funktionen, Katalogsichten und dynamische Verwaltungssichten |
| &nbsp; | CLR-abhängige T-SQL-Funktionen wie ASSEMBLYPROPERTY, FORMAT, PARSE, TRY_PARSE |
| &nbsp; | CLR-abhängige Datums- und Uhrzeitkatalogsichten, Funktionen und Abfrageklauseln |
| &nbsp; | Pufferpoolerweiterung |
| &nbsp; | Datenbank-E-Mail |
| **SQL Server-Agent** |  Subsysteme: CmdExec, PowerShell, Queue Reader, SSIS, SSAS, SSRS |
| &nbsp; | Alerts |
| &nbsp; | Managed Backup |
| **Hochverfügbarkeit** | Always On-Verfügbarkeitsgruppen  |
| &nbsp; | Basis-Verfügbarkeitsgruppen |
| &nbsp; | Always On-Failoverclusterinstanz |
| &nbsp; | Datenbankspiegelung |
| &nbsp; | Hinzufügen von Speicher im laufenden Systembetrieb und CPU |
| **Security** | Erweiterbare Schlüsselverwaltung |
| &nbsp; | Active Directory-Integration|
| &nbsp; | Unterstützung für Secure Enclaves|
| **Dienste** | SQL Server-Browser |
| &nbsp; | Machine Learning über R und Python |
| &nbsp; | StreamInsight |
| &nbsp; | Analysis Services |
| &nbsp; | Reporting Services |
| &nbsp; | Data Quality Services |
| &nbsp; | Master Data Services |
| &nbsp; | Distributed Replay |
| **Verwaltbarkeit** | SQL Server-Steuerungspunkt für das Hilfsprogramm |

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von Azure SQL Edge](deploy-portal.md)
- [Konfigurieren von Azure SQL Edge](configure.md)
- [Herstellen einer Verbindung mit einer Instanz von Azure SQL Edge mithilfe von SQL Server-Clienttools](connect.md)
- [Sichern und Wiederherstellen von Datenbanken in Azure SQL Edge](backup-restore.md)
