---
title: Unterstützungsmatrix für SAP HANA-Sicherungen
description: Dieser Artikel enthält Informationen zu den unterstützten Szenarien und zu Einschränkungen beim Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern unter Verwendung von Azure Backup.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 19d62fac6a7072018508808b1c9695eb8b260b15
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86170614"
---
# <a name="support-matrix-for-backup-of-sap-hana-databases-on-azure-vms"></a>Unterstützungsmatrix für die Sicherung von SAP HANA-Datenbanken auf virtuellen Azure-Computern

Azure Backup unterstützt die Sicherung von SAP HANA-Datenbanken in Azure. In diesem Artikel werden die unterstützten Szenarien sowie Einschränkungen beim Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern unter Verwendung von Azure Backup zusammengefasst.

> [!NOTE]
> Die Häufigkeit von Protokollsicherungen kann nun auf einen Mindestwert von 15 Minuten festgelegt werden. Protokollsicherungen werden erst nach dem erfolgreichen Abschluss einer vollständigen Sicherung der Datenbank übertragen.

## <a name="scenario-support"></a>Unterstützung von Szenarien

| **Szenario**               | **Unterstützte Konfigurationen**                                | **Nicht unterstützte Konfigurationen**                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Topologie**               | Nur SAP HANA-Ausführung auf virtuellen Azure-Computern unter Linux                    | SAP HANA (große Instanzen)                                   |
| **Regionen**                   | **GA:**<br> **Nord- und Südamerika** – USA, Mitte; USA, Osten 2; USA, Osten; USA, Norden-Mitte; USA, Süden-Mitte; USA, Westen 2; USA, Westen-Mitte; USA, Westen; Kanada, Mitte; Kanada, Osten; Brasilien, Süden <br> **Asien-Pazifik** – Australien, Mitte; Australien, Mitte 2; Australien, Osten; Australien, Südosten; Japan, Osten; Japan, Westen; Südkorea, Mitte; Südkorea, Süden; Asien, Osten; Asien, Südosten; Indien, Mitte; Indien, Süden; Indien, Westen; China, Osten; China, Norden; China Osten 2; China, Norden 2 <br> **Europa** – Europa, Westen; Europa, Norden; Frankreich, Mitte; Vereinigtes Königreich, Süden; Vereinigtes Königreich, Westen; Deutschland, Norden; Deutschland, Westen-Mitte; Schweiz, Norden; Schweiz, Westen; Schweiz, Norden; Norwegen, Osten; Norwegen, Westen <br> **Afrika/Naher Osten** – Südafrika, Norden; Südafrika, Westen; VAE, Norden; VAE, Mitte  <BR>  **Azure Government-Regionen** | Frankreich, Süden; Deutschland, Mitte; Deutschland, Nordosten; US Gov IOWA |
| **Betriebssystemversionen**            | SLES 12 mit SP2, SP3 und SP4; SLES 15 mit SP0 und SP1 <br><br>   **Vorschau** – 7.4, 7.6, 7.7 und 8.1  <br>     [Erste Schritte](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db) mit SAP HANA-Sicherung (Vorschau) für RHEL (7.4, 7.6, 7.7 und 8.1). Für weitere Anfragen schreiben Sie uns unter [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com).                |                                             |
| **HANA-Versionen**          | SDC unter HANA 1.x, MDC unter HANA 2.x <= SPS04 Rev 48, SPS05 (für Szenarien mit Verschlüsselung noch nicht überprüft)      |                                                            |
| **HANA-Bereitstellungen**       | SAP HANA auf einem einzelnen virtuellen Azure-Computer: nur zentrales Hochskalieren. <br><br> Bei Bereitstellungen mit hoher Verfügbarkeit werden beide Knoten auf den beiden verschiedenen Computern als einzelne Knoten mit separaten Datenketten behandelt.               | Horizontales Skalieren <br><br> Bei Bereitstellungen mit hoher Verfügbarkeit wird kein automatisches Failover der Sicherung auf den sekundären Knoten ausgeführt. Das Konfigurieren der Sicherung sollte für jeden Knoten separat durchgeführt werden.                                           |
| **HANA-Instanzen**         | Eine einzelne SAP HANA-Instanz auf einem einzelnen virtuellen Azure-Computer: nur Hochskalieren | Mehrere SAP HANA-Instanzen auf einem einzelnen virtuellen Computer                  |
| **HANA-Datenbanktypen**    | Container mit Einzeldatenbank (Single Database Container, SDC) unter 1.x, Container mit mehreren Datenbanken (Multi-Database Container, MDC) unter 2.x | MDC unter HANA 1.x                                              |
| **HANA-Datenbankgröße**     | HANA-Datenbanken mit einer Größe von <= 2 TB (dies ist nicht die Arbeitsspeichergröße des HANA-Systems)               |                                                              |
| **Sicherungstypen**           | Vollständige Sicherungen, differenzielle Sicherungen und Protokollsicherungen                          | Inkrementelle Sicherungen, Momentaufnahmen                                       |
| **Wiederherstellungstypen**          | Informationen zu den unterstützten Wiederherstellungstypen finden Sie im SAP HANA-Hinweis [1642148](https://launchpad.support.sap.com/#/notes/1642148). |                                                              |
| **Grenzwerte für Sicherungen**          | Vollständige Sicherungen bis zu einer Größe von 2 TB pro SAP HANA-Instanz         |                                                              |
| **Besondere Konfigurationen** |                                                              | SAP HANA + Dynamic Tiering <br>  Klonen über LaMa        |

------

>[!NOTE]
>Azure Backup passt Änderungen an der Sommer- oder Winterzeit bei der Sicherung einer auf einem virtuellen Azure-Computer ausgeführten SAP HANA-Datenbank nicht automatisch an.
>
>Ändern Sie die Richtlinie nach Bedarf manuell.


> [!NOTE]
> Sie können jetzt [Sicherungs- und Wiederherstellungsaufträge](https://docs.microsoft.com/azure/backup/sap-hana-db-manage#monitor-manual-backup-jobs-in-the-portal) (auf demselben Computer) überwachen, die von nativen HANA-Clients (SAP HANA Studio/Cockpit/DBA Cockpit) im Azure-Portal ausgelöst wurden.

## <a name="next-steps"></a>Nächste Schritte

* [Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database)
* [Wiederherstellen von SAP HANA-Datenbanken auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/sap-hana-db-restore)
* [Verwalten und Überwachen gesicherter SAP HANA-Datenbanken](sap-hana-db-manage.md)
* [Behandeln von Problemen beim Sichern von SAP HANA-Datenbanken in Azure](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot)
