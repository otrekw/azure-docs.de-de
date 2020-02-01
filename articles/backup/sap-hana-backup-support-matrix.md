---
title: Unterstützungsmatrix für SAP HANA-Sicherungen
description: Dieser Artikel enthält Informationen zu den unterstützten Szenarien und zu Einschränkungen beim Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern unter Verwendung von Azure Backup.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 82d844385290ab0dc2953537c1f9a3387dd7b2b2
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842630"
---
# <a name="support-matrix-for-backup-of-sap-hana-databases-on-azure-vms"></a>Unterstützungsmatrix für die Sicherung von SAP HANA-Datenbanken auf virtuellen Azure-Computern

Azure Backup unterstützt die Sicherung von SAP HANA-Datenbanken in Azure. In diesem Artikel werden die unterstützten Szenarien sowie Einschränkungen beim Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern unter Verwendung von Azure Backup zusammengefasst.

## <a name="onboard-to-the-public-preview"></a>Onboarding in die öffentliche Vorschau

Führen Sie das Onboarding in die öffentliche Vorschau wie folgt durch:

* Registrieren Sie im Portal Ihre Abonnement-ID bei dem Recovery Services-Dienstanbieter [gemäß der Beschreibung in diesem Artikel](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal).
* Führen Sie bei Verwendung von PowerShell das folgende Cmdlet aus. Der Vorgang sollte mit „Registered“ (Registriert) abgeschlossen werden.

```PowerShell
Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
```

> [!NOTE]
> Die Häufigkeit von Protokollsicherungen kann nun auf einen Mindestwert von 15 Minuten festgelegt werden. Protokollsicherungen werden erst nach dem erfolgreichen Abschluss einer vollständigen Sicherung der Datenbank übertragen.

## <a name="scenario-support"></a>Unterstützung von Szenarien

| **Szenario**               | **Unterstützte Konfigurationen**                                | **Nicht unterstützte Konfigurationen**                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Topologie**               | Nur SAP HANA-Ausführung auf virtuellen Azure-Computern unter Linux                    | SAP HANA (große Instanzen)                                   |
| **Geografische Regionen**                   | **GA:**<br />**Europa** – Europa, Westen; Europa, Norden; Frankreich, Mitte; Vereinigtes Königreich, Süden; Vereinigtes Königreich, Westen; Deutschland, Norden; Deutschland, Westen-Mitte; Schweiz, Norden; Schweiz, Westen<br />**Asien-Pazifik** – Australien, Mitte, Australien, Mitte 2, Australien, Osten, Australien, Südosten, Japan, Osten, Japan, Westen, Südkorea, Mitte, Südkorea, Süden<br /><br>**Vorschau:**<br />**Nord- und Südamerika** – USA, Mitte; USA, Osten 2; USA, Osten; USA, Norden-Mitte; USA, Süden-Mitte; USA, Westen 2; USA, Westen-Mitte; USA, Westen; Kanada, Mitte; Kanada, Osten <br />**Asien-Pazifik** – Asien, Osten; Asien, Südosten; Indien, Mitte; Indien, Süden | China, Osten; China, Norden; China, Osten 2; China, Norden 2; Indien, Westen; Schweiz, Norden; Südafrika, Norden; Südafrika, Westen; VAE, Norden; VAE, Mitte; Azure Government-Regionen; Frankreich, Süden; Brasilien, Süden |
| **Betriebssystemversionen**            | SLES 12 mit SP2, SP3 oder SP4                                | SLES 15, RHEL                                                |
| **HANA-Versionen**          | SDC unter HANA 1.x, MDC unter HANA 2.x <= SPS04 Rev 44            | -                                                            |
| **HANA-Bereitstellungen**       | SAP HANA auf einem einzelnen virtuellen Azure-Computer: nur zentrales Hochskalieren               | Horizontales Skalieren                                                    |
| **HANA-Instanzen**         | Eine einzelne SAP HANA-Instanz auf einem einzelnen virtuellen Azure-Computer: nur zentrales Hochskalieren | Mehrere SAP HANA-Instanzen auf einem einzelnen virtuellen Computer                  |
| **HANA-Datenbanktypen**    | Container mit Einzeldatenbank (Single Database Container, SDC) unter 1.x, Container mit mehreren Datenbanken (Multi-Database Container, MDC) unter 2.x | MDC unter HANA 1.x                                              |
| **HANA-Datenbankgröße**     | Größe der vollständigen Sicherung 2-TB, wie von HANA berichtet)                   |                                                              |
| **Sicherungstypen**           | Vollständige Sicherungen, differenzielle Sicherungen und Protokollsicherungen                          | Inkrementelle Sicherungen, Momentaufnahmen                                       |
| **Wiederherstellungstypen**          | Informationen zu den unterstützten Wiederherstellungstypen finden Sie im SAP HANA-Hinweis [1642148](https://launchpad.support.sap.com/#/notes/1642148). |                                                              |
| **Grenzwerte für Sicherungen**          | Vollständige Sicherungen bis zu einer Größe von 2 TB pro SAP HANA-Instanz         |                                                              |
| **Besondere Konfigurationen** |                                                              | SAP HANA + Dynamic Tiering <br>  Klonen über LaMa        |

------

> [!NOTE]
> Sicherungs- und Wiederherstellungsvorgänge von nativen SAP HANA-Clients (SAP HANA Studio/Cockpit/DBA Cockpit) werden aktuell nicht unterstützt.



## <a name="next-steps"></a>Nächste Schritte

* [Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database)
* [Wiederherstellen von SAP HANA-Datenbanken auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/sap-hana-db-restore)
* [Verwalten und Überwachen gesicherter SAP HANA-Datenbanken](sap-hana-db-manage.md)
* [Behandeln von Problemen beim Sichern von SAP HANA-Datenbanken in Azure](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot)
