---
title: Unterstützungsmatrix für SAP HANA-Sicherungen
description: Dieser Artikel enthält Informationen zu den unterstützten Szenarien und zu Einschränkungen beim Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern unter Verwendung von Azure Backup.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: a0a7c25ec718dcd6a903d2149a8b3930fb25941e
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514289"
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
| **Geografische Regionen**                   | „Australien, Südosten“, „Australien, Osten“, „Brasilien, Süden“, „Kanada, Mitte“, „Kanada, Osten“, „Asien, Südosten“, „Asien, Osten“, „USA, Osten“, „USA, Osten 2“, „USA, Westen-Mitte“, „USA, Westen“, „USA, Westen 2“, „USA, Norden-Mitte“, „USA, Mitte“, „USA, Süden-Mitte“, „Indien, Mitte“, „Indien, Süden“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „Europa, Norden“, „Europa, Westen“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“ | „Australien, Mitte“, „Australien, Mitte 2“, „China, Osten“, „China, Norden“, „China, Osten 2“, „China, Norden 2“, „Indien, Westen“, „Frankreich, Mitte“, „Frankreich, Süden“, „Deutschland, Norden“, „Deutschland, Westen-Mitte“, „Schweiz, Norden“, „Schweiz, Westen“, „Südafrika, Norden“, „Südafrika, Westen“, „VAE, Norden“, „VAE, Mitte“, Azure Government-Regionen |
| **Betriebssystemversionen**            | SLES 12 mit SP2, SP3 oder SP4           | SLES 15, RHEL                                                |
| **HANA-Versionen**          | SDC unter HANA 1.x, MDC unter HANA 2.x <= SPS04 Rev 44           | -                                                            |
| **HANA-Bereitstellungen**       | SAP HANA auf einem einzelnen virtuellen Azure-Computer: nur zentrales Hochskalieren               | Horizontales Skalieren                                                    |
| **HANA-Instanzen**         | Eine einzelne SAP HANA-Instanz auf einem einzelnen virtuellen Azure-Computer: nur zentrales Hochskalieren | Mehrere SAP HANA-Instanzen auf einem einzelnen virtuellen Computer                  |
| **HANA-Datenbanktypen**    | Container mit Einzeldatenbank (Single Database Container, SDC) unter 1.x, Container mit mehreren Datenbanken (Multi-Database Container, MDC) unter 2.x | MDC unter HANA 1.x                                              |
| **HANA-Datenbankgröße**     | Größe der vollständigen Sicherung 2-TB, wie von HANA berichtet) |                                                              |
| **Sicherungstypen**           | Vollständige Sicherungen, differenzielle Sicherungen und Protokollsicherungen                           | Inkrementelle Sicherungen, Momentaufnahmen                                       |
| **Wiederherstellungstypen**          | Informationen zu den unterstützten Wiederherstellungstypen finden Sie im SAP HANA-Hinweis [1642148](https://launchpad.support.sap.com/#/notes/1642148). |                                                              |
| **Grenzwerte für Sicherungen**          | Vollständige Sicherungen bis zu einer Größe von 2 TB pro SAP HANA-Instanz  |                                                              |
| **Besondere Konfigurationen** |                                                              | SAP HANA + Dynamic Tiering <br>  Klonen über LaMa            |

------

> [!NOTE]
> Sicherungs- und Wiederherstellungsvorgänge von nativen SAP HANA-Clients (SAP HANA Studio/Cockpit/DBA Cockpit) werden aktuell nicht unterstützt.



## <a name="next-steps"></a>Nächste Schritte

* [Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database)
* [Wiederherstellen von SAP HANA-Datenbanken auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/sap-hana-db-restore)
* [Verwalten und Überwachen gesicherter SAP HANA-Datenbanken](sap-hana-db-manage.md)
* [Behandeln von Problemen beim Sichern von SAP HANA-Datenbanken in Azure](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot)
