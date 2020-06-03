---
title: Azure Backup-Unterstützungsmatrix für die SQL Server-Sicherung auf Azure-VMs
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen für die Sicherung von SQL Server auf Azure-VMs mit dem Azure Backup-Dienst.
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: f9594b9157f84a0536ffd4b62f792fd86fb1c243
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84234246"
---
# <a name="support-matrix-for-sql-server-backup-in-azure-vms"></a>Unterstützungsmatrix für die SQL Server-Sicherung auf Azure-VMs

Mit Azure Backup können Sie SQL Server-Datenbanken auf Azure-VMs sichern, die auf der Microsoft Azure-Cloudplattform gehostet werden. Dieser Artikel enthält eine Zusammenfassung der allgemeinen Unterstützungseinstellungen und Einschränkungen für Szenarien und Bereitstellungen der SQL Server-Sicherung auf Azure-VMs.

## <a name="scenario-support"></a>Unterstützung von Szenarien

**Unterstützung** | **Details**
--- | ---
**Unterstützte Bereitstellungen** | SQL-Marketplace-Azure-VMs und Nicht-Marketplace-VMs (manuelle SQL Server-Installation) werden unterstützt.
**Unterstützte Regionen** | „Australien, Südosten (ASE)“, „Australien, Osten (AE)“, „Australien, Mitte (AC)“, „Australien, Mitte 2 (AC)“ <br> Brasilien, Süden (BRS)<br> „Kanada, Mitte (CNC)“, „Kanada, Osten (CE)“<br> „Asien, Südosten (SEA)“, „Asien, Osten (EA)“ <br> „USA, Osten (EUS)“, „USA, Osten 2 (EUS2)“, „USA, Westen-Mitte (WCUS)“, „USA, Westen (WUS)“, „USA, Westen 2 (WUS 2)“, „USA, Norden-Mitte (NCUS)“, „USA, Mitte (CUS)“, „USA, Süden-Mitte (SCUS)“ <br> „Indien, Mitte (INC)“, „Indien, Süden (INS)“, „Indien, Westen“ <br> „Japan, Osten (JPE)“, „Japan, Westen (JPW)“ <br> „Südkorea, Mitte (KRC)“, „Südkorea, Süden (KRS)“ <br> „Europa, Norden (NE)“, „Europa, Westen“ <br> „Vereinigtes Königreich, Süden (UKS)“, „Vereinigtes Königreich, Westen (UKW)“ <br> „US Gov Arizona“, „US Gov Virginia“, „US Gov Texas“, „US DoD, Mitte“, „US DoD, Osten“ <br> „Deutschland, Norden“, „Deutschland, Westen-Mitte“ <br> „Schweiz, Norden“, „Schweiz, Westen“ <br> Frankreich, Mitte <br> „China, Osten“, „China, Osten 2“, „China, Norden“, „China, Norden 2“
**Unterstützte Betriebssysteme** | Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2 SP1 <br/><br/> Linux wird derzeit nicht unterstützt.
**Unterstützte SQL Server-Versionen** | SQL Server 2019, SQL Server 2017 wie auf der Seite [Lebenszyklus für Produkt suchen](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202017) beschrieben, SQL Server 2016 und SPs wie auf der Seite [Lebenszyklus für Produkt suchen](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202016%20service%20pack) beschrieben, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008 <br/><br/> Enterprise, Standard, Web, Developer, Express
**Unterstützte .NET-Versionen** | .NET Framework 4.5.2 oder höher auf dem virtuellen Computer installiert

## <a name="feature-consideration-and-limitations"></a>Funktionsaspekte und Einschränkungen

* SQL Server-Sicherung kann im Azure-Portal oder mit **PowerShell** konfiguriert werden. Die Befehlszeilenschnittstelle wird nicht unterstützt.
* Die Lösung wird für beide Arten von [Bereitstellungen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) unterstützt: Azure Resource Manager-VMs und klassische VMs.
* Der virtuelle Computer, auf dem SQL Server ausgeführt wird, benötigt eine Internetverbindung, um auf öffentliche IP-Adressen von Azure zuzugreifen.
* Eine SQL Server-**Failoverclusterinstanz (FCI)** wird nicht unterstützt.
* Sicherungs- und Wiederherstellungsvorgänge für Spiegeldatenbanken und Datenbankmomentaufnahmen werden nicht unterstützt.
* Die Verwendung mehrerer Sicherungslösungen zum Sichern Ihrer eigenständigen SQL Server-Instanz oder SQL Always On-Verfügbarkeitsgruppe kann zu Fehlern bei der Sicherung führen. Es ist daher ratsam, von dieser Vorgehensweise abzusehen.
* Das Sichern von zwei Knoten einer Verfügbarkeitsgruppe mit derselben Lösung oder anderen Lösungen kann auch zu Fehlern bei der Sicherung führen.
* Azure Backup unterstützt für **schreibgeschützte** Datenbanken nur die Sicherungstypen „Vollständig“ und „Nur vollständig kopieren“.
* Datenbanken mit einer großen Anzahl von Dateien können nicht geschützt werden. Die maximale Anzahl von unterstützten Dateien beträgt **~1.000**.  
* Sie können bis zu **~2.000** SQL Server-Datenbanken in einem Tresor sichern. Sie können mehrere Tresore erstellen, falls Sie über eine größere Anzahl von Datenbanken verfügen.
* Sie können die Sicherung für bis zu **50** Datenbanken auf einmal konfigurieren; diese Einschränkung trägt dazu bei, Sicherungslasten zu optimieren.
* Wir unterstützen Datenbanken bis zu einer Größe von **2 TB**. Über 2 TB treten unter Umständen Leistungsprobleme auf.
* Um einen Eindruck davon zu bekommen, wie viele Datenbanken pro Server geschützt werden können, müssen Sie Faktoren wie Bandbreite, VM-Größe, Sicherungshäufigkeit, Datenbankgröße usw. berücksichtigen. Laden Sie den Ressourcenplaner [hier](https://download.microsoft.com/download/A/B/5/AB5D86F0-DCB7-4DC3-9872-6155C96DE500/SQL%20Server%20in%20Azure%20VM%20Backup%20Scale%20Calculator.xlsx) herunter. Er berechnet die ungefähre Anzahl von Datenbanken, die basierend auf den VM-Ressourcen und der Sicherungsrichtlinie pro Server zulässig sind.
* Wenn Verfügbarkeitsgruppen konfiguriert sind, beruht die Erstellung von Sicherungen der verschiedenen Knoten auf mehreren Faktoren. Das Sicherungsverhalten für eine Verfügbarkeitsgruppe ist unten zusammengefasst.

### <a name="back-up-behavior-with-always-on-availability-groups"></a>Sicherungsverhalten bei Verwendung von Always On-Verfügbarkeitsgruppen

Wir empfehlen, die Sicherung nur auf einem Knoten der Verfügbarkeitsgruppe zu konfigurieren. Konfigurieren Sie die Sicherung immer in derselben Region wie den primären Knoten. Anders ausgedrückt: Der primäre Knoten muss sich immer in der Region befinden, in der Sie die Sicherung konfigurieren. Wenn sich alle Knoten der Verfügbarkeitsgruppe in der Region befinden, in der die Sicherung konfiguriert ist, ist alles in Ordnung.

#### <a name="for-cross-region-ag"></a>Für regionsübergreifende Verfügbarkeitsgruppe

* Sicherungen werden unabhängig von der Sicherungseinstellung nur für die Knoten ausgeführt, die sich in der Region befinden, in der die Sicherung konfiguriert ist. Dies liegt daran, dass regionsübergreifende Sicherungen nicht unterstützt werden. Wenn Sie nur zwei Knoten haben und sich der sekundäre Knoten in der anderen Region befindet, werden die Sicherungen weiterhin über den primären Knoten ausgeführt (sofern Ihre Sicherungseinstellung nicht „Nur sekundär“ lautet).
* Wenn für einen Knoten ein Failover auf eine Region erfolgt, bei der es sich nicht um die Region handelt, in der die Sicherung konfiguriert ist, schlagen Sicherungen auf den Knoten in der Failoverregion fehl.

Abhängig von der Sicherungseinstellung und den Sicherungstypen (Vollständig/Differenziell/Protokoll/Nur vollständig kopieren) werden die Sicherungen von einem bestimmten Knoten (Primär/Sekundär) erstellt.

#### <a name="backup-preference-primary"></a>Sicherungseinstellung: Primär

**Sicherungstyp** | **Node**
--- | ---
Vollständig | Primär
Differenziell | Primär
Log |  Primär
Nur vollständig kopieren |  Primär

#### <a name="backup-preference-secondary-only"></a>Sicherungseinstellung: Nur sekundär

**Sicherungstyp** | **Node**
--- | ---
Vollständig | Primär
Differenziell | Primär
Log |  Secondary
Nur vollständig kopieren |  Secondary

#### <a name="backup-preference-secondary"></a>Sicherungseinstellung: Secondary

**Sicherungstyp** | **Node**
--- | ---
Vollständig | Primär
Differenziell | Primär
Log |  Secondary
Nur vollständig kopieren |  Secondary

#### <a name="no-backup-preference"></a>Keine Sicherungseinstellung

**Sicherungstyp** | **Node**
--- | ---
Vollständig | Primär
Differenziell | Primär
Log |  Secondary
Nur vollständig kopieren |  Secondary

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie Sie eine [SQL Server-Datenbank sichern](backup-azure-sql-database.md), die auf einer Azure-VM ausgeführt wird.
