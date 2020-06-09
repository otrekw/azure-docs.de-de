---
title: Unterstützungsmatrix für Sicherungen von Azure-Dateifreigaben
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Sicherung von Azure-Dateifreigaben.
ms.topic: conceptual
ms.date: 5/07/2020
ms.openlocfilehash: 42578cc83ef193801fa700ec7d136385411e5f79
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684627"
---
# <a name="support-matrix-for-azure-file-share-backup"></a>Unterstützungsmatrix für Sicherungen von Azure-Dateifreigaben

Sie können den [Azure Backup-Dienst](https://docs.microsoft.com/azure/backup/backup-overview) verwenden, um Azure-Dateifreigaben zu sichern. Dieser Artikel enthält eine Zusammenfassung der Unterstützungseinstellungen bei der Sicherung von Azure-Dateifreigaben mit Azure Backup.

## <a name="supported-geos"></a>Unterstützte geografische Räume

Die Sicherung von Azure-Dateifreigaben ist in den folgenden geografischen Räumen verfügbar:

**Regionen mit allgemeiner Verfügbarkeit:**<br>
Australien, Südosten (ASE); Kanada, Mitte (CNC); USA, Westen-Mitte (WCUS); USA, Süden-Mitte (SCUS); USA, Westen 2 (WUS 2); Indien, Süden (INS); USA, Norden-Mitte (NCUS); Japan, Osten (JPE); Brasilien, Süden (BRS); Asien, Südosten (SEA); Schweiz, Westen (SZW); VAE, Mitte (UAC); Norwegen, Osten (NWE); Indien, Westen (INW); Australien, Mitte (ACL); Korea, Mitte (KRC); Japan, Westen (JPW); Südafrika, Norden (SAN); Vereinigtes Königreich, Süden (UKS); Vereinigtes Königreich, Westen (UKW); Korea, Süden (KRS); Europa, Norden (NE); Deutschland, Norden (GN); Norwegen, Westen (NWW); Südafrika, Westen (SAW); Schweiz, Norden (SZN); Deutschland, Westen-Mitte (GWC); VAE, Norden (UAN); Frankreich, Mitte (FRC); Indien, Mitte (INC); Kanada, Osten (CNE); Asien, Osten (EA); Australien, Osten (AE); USA, Mitte (CUS); USA, Westen (WUS); US Gov Arizona (UGA); US Gov Texas (UGT); US Gov Virginia (UGV); US DoD, Mitte (UDC); US DoD, Osten (UDE)

**Unterstützte Regionen (im Rahmen der Vorschau, aber noch nicht allgemein verfügbar):**<br>
USA, Osten (EUS); USA, Osten 2 (EUS2); Europa, Westen (WE)

## <a name="supported-storage-accounts"></a>Unterstützte Speicherkonten

| Speicherkontodetails | Support                                                      |
| ------------------------ | ------------------------------------------------------------ |
| Kontoart            | Azure Backup unterstützt Azure-Dateifreigaben unter den Speicherkonten Universell V1, Universell V2 und im Dateispeicher-Kontotyp. |
| Leistung              | Azure Backup unterstützt Dateifreigaben sowohl unter Standard- als auch unter Storage Premium-Konten. |
| Replikation              | Azure-Dateifreigaben werden unter Speicherkonten aller Replikationstypen unterstützt |

## <a name="supported-file-shares"></a>Ausgewählte Dateifreigaben

| Dateifreigabetyp                                   | Support   |
| -------------------------------------------------- | --------- |
| Standard                                           | Unterstützt |
| Groß                                              | Unterstützt |
| Premium                                            | Unterstützt |
| Mit dem Azure-Dateisynchronisierungsdienst verbundene Dateifreigaben | Unterstützt |

## <a name="protection-limits"></a>Schutzlimits

| Einstellung                                                      | Begrenzung |
| ------------------------------------------------------------ | ----- |
| Maximale Anzahl von Dateifreigaben, die pro Tag pro Tresor geschützt werden können | 200   |
| Maximale Anzahl von Speicherkonten, die pro Tresor pro Tag registriert werden können | 50    |

## <a name="backup-limits"></a>Backup-Grenzwerte

| Einstellung                                      | Begrenzung |
| -------------------------------------------- | ----- |
| Maximale Anzahl bedarfsgesteuerter Sicherungen pro Tag | 4     |
| Maximale Anzahl geplanter Sicherungen pro Tag | 1     |

## <a name="restore-limits"></a>Wiederherstellungslimits

| Einstellung                                                      | Begrenzung   |
| ------------------------------------------------------------ | ------- |
| Maximale Anzahl der Wiederherstellungen pro Tag                           | 10      |
| Maximale Anzahl der Dateien pro Wiederherstellung                         | 10      |
| Maximale empfohlene Wiederherstellungsgröße pro Wiederherstellung für große Dateifreigaben | 15 TiB |

## <a name="retention-limits"></a>Grenzwerte für die Aufbewahrung

| Einstellung                                                      | Begrenzung    |
| ------------------------------------------------------------ | -------- |
| Maximale Gesamtzahl der Wiederherstellungspunkte pro Dateifreigabe zu jedem beliebigen Zeitpunkt | 200      |
| Maximale Aufbewahrung von Wiederherstellungspunkten, die durch bedarfsgesteuerte Sicherung erstellt wurden | 10 Jahre |
| Maximale Aufbewahrung täglicher Wiederherstellungspunkte (Momentaufnahmen) pro Dateifreigabe| 200 Tage |
| Maximale Aufbewahrung wöchentlicher Wiederherstellungspunkte (Momentaufnahmen) pro Dateifreigabe | 200 Wochen |
| Maximale Aufbewahrung monatlicher Wiederherstellungspunkte (Momentaufnahmen) pro Dateifreigabe | 120 Monate |
| Maximale Aufbewahrung jährlicher Wiederherstellungspunkte (Momentaufnahmen) pro Dateifreigabe | 10 Jahre |

## <a name="supported-restore-methods"></a>Unterstützte Wiederherstellungsmethoden

| Wiederherstellungsmethode     | Details                                                      |
| ------------------ | ------------------------------------------------------------ |
| Wiederherstellung der vollständigen Freigabe | Sie können die gesamte Dateifreigabe am ursprünglichen oder an einem alternativen Speicherort wiederherstellen. |
| Wiederherstellung auf Elementebene | Sie können einzelne Dateien und Ordner am ursprünglichen oder an einem alternativen Speicherort wiederherstellen. |

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Sichern von Azure-Dateifreigaben](backup-afs.md).
* [Wiederherstellen von Azure-Dateifreigaben](restore-afs.md)
* [Verwalten der Sicherungen von Azure-Dateifreigaben](manage-afs-backup.md)
