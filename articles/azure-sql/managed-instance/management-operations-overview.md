---
title: Verwaltungsvorgänge
titleSuffix: Azure SQL Managed Instance
description: Enthält Informationen zur Dauer der Verwaltungsvorgänge für Azure SQL Managed Instance und zu den bewährten Methoden.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: urosmil
ms.author: urmilano
ms.reviewer: sstein, carlrab, MashaMSFT
ms.date: 07/10/2020
ms.openlocfilehash: 5cff54b1f71d30f7932c4ead722d1dda0a7aec57
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528076"
---
# <a name="overview-of-azure-sql-managed-instance-management-operations"></a>Übersicht über die Verwaltungsvorgänge für Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

## <a name="what-are-management-operations"></a>Was sind Verwaltungsvorgänge?
Azure SQL Managed Instance bietet Verwaltungsvorgänge, die Sie zum automatischen Bereitstellen von neuen verwalteten Instanzen, Aktualisieren von Instanzeigenschaften und Löschen von nicht mehr benötigten Instanzen verwenden können.

Für die Unterstützung von [Bereitstellungen in virtuellen Azure-Netzwerken](../../virtual-network/virtual-network-for-azure-services.md) und zur Sicherstellung der Isolation und Sicherheit für Kunden werden für SQL Managed Instance [virtuelle Cluster](connectivity-architecture-overview.md#high-level-connectivity-architecture) genutzt. Ein virtueller Cluster stellt eine dedizierte Gruppe mit isolierten virtuellen Computern dar, die im Subnetz des virtuellen Netzwerks des Kunden bereitgestellt werden. Praktisch jede Bereitstellung einer verwalteten Instanz in einem leeren Subnetz führt dazu, dass ein neuer virtueller Cluster erstellt wird.

Nachfolgende Vorgänge für bereitgestellte verwaltete Instanzen können auch Auswirkungen auf den zugrunde liegenden virtuellen Cluster haben. Diese Vorgänge wirken sich auf die Dauer von Verwaltungsvorgängen aus, da die Bereitstellung von zusätzlichen virtuellen Computern mit Mehraufwand verbunden ist. Sie müssen dies berücksichtigen, wenn Sie neue Bereitstellungen oder Updates für vorhandene verwaltete Instanzen planen.

Alle Verwaltungsvorgänge können wie folgt kategorisiert werden:

- Instanzbereitstellung (Erstellung neuer Instanzen)
- Instanzupdate (Änderung von Instanzeigenschaften, z. B. virtuelle Kerne oder reservierter Speicher)
- Instanzlöschung

## <a name="management-operations-duration"></a>Dauer von Verwaltungsvorgängen
Normalerweise dauern Vorgänge in virtuellen Clustern am längsten. Die Dauer der Vorgänge in virtuellen Clustern variiert. Unten sind die Werte angegeben, die für vorhandene Diensttelemetriedaten normalerweise zu erwarten sind:

- **Erstellung von virtuellen Clustern**:  Die Erstellung ist ein synchroner Schritt der Vorgänge für die Instanzverwaltung. **90 % der Vorgänge werden innerhalb von vier Stunden abgeschlossen**.
- **Änderung der Größe von virtuellen Clustern (Vergrößerung oder Verkleinerung)** : Die Vergrößerung ist ein synchroner Schritt, und die Verkleinerung wird asynchron durchgeführt (ohne Auswirkung auf die Dauer von Instanzverwaltungsvorgängen). **90 % der Clustervergrößerungen werden in weniger als 2,5 Stunden abgeschlossen**.
- **Löschung von virtuellen Clustern**: Der Löschvorgang ist ein asynchroner Schritt, der in einem leeren virtuellen Cluster aber auch [manuell initiiert](virtual-cluster-delete.md) werden kann. In diesem Fall wird der Vorgang synchron durchgeführt. **90 % der Löschungen virtueller Cluster werden innerhalb von 1,5 Stunden abgeschlossen**.

Darüber hinaus kann die Verwaltung von Instanzen auch einen der Vorgänge in gehosteten Datenbanken umfassen. Dies führt zu einer längeren Dauer:

- **Anfügen von Datenbankdateien aus Azure Storage**:  Ein synchroner Schritt, z. B. Compute (V-Kerne) oder das Hoch- oder Herunterskalieren im Tarif „Universell“. **90 % dieser Vorgänge werden innerhalb von fünf Minuten abgeschlossen**.
- **Seeding der Always On-Verfügbarkeitsgruppe**: Ein synchroner Schritt, z. B. Compute (V-Kerne) oder die Skalierung des Speichers im Tarif „Unternehmenskritisch“ sowie die Änderung der Dienstebene von „Universell“ in „Unternehmenskritisch“ (oder umgekehrt). Die Dauer dieses Vorgangs ist proportional zur Gesamtgröße der Datenbank sowie zur aktuellen Datenbankaktivität (Anzahl aktiver Transaktionen). Aufgrund der Datenbankaktivität beim Aktualisieren einer Instanz kann die Gesamtdauer erheblich variieren. **90 % dieser Vorgänge werden mindestens mit 220 GB pro Stunde durchgeführt.**

In der folgenden Tabelle sind die Vorgänge und die typischen Gesamtdauern zusammengefasst:

|Category  |Vorgang  |Segment mit langer Ausführungsdauer  |Geschätzte Dauer  |
|---------|---------|---------|---------|
|**Bereitstellung** |Erste Instanz in einem leeren Subnetz|Erstellung eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von 4 Stunden abgeschlossen.|
|Bereitstellung |Erste Instanz einer anderen Hardwaregeneration in einem nicht leeren Subnetz (z. B. erste Gen5-Instanz in einem Subnetz mit Gen4-Instanzen)|Erstellung eines virtuellen Clusters*|90 % der Vorgänge werden innerhalb von 4 Stunden abgeschlossen.|
|Bereitstellung |Erstellung der ersten Instanz mit vier virtuellen Kernen in einem leeren oder nicht leeren Subnetz|Erstellung eines virtuellen Clusters**|90 % der Vorgänge werden innerhalb von 4 Stunden abgeschlossen.|
|Bereitstellung |Nachfolgende Instanzerstellung im nicht leeren Subnetz (2. Instanz, 3. Instanz usw.)|Änderung der Größe eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von 2,5 Stunden abgeschlossen.|
|**Aktualisieren** |Änderung der Instanzeigenschaft (Administratorkennwort, Azure AD-Anmeldung, Azure-Hybridvorteil-Flag)|–|Bis zu 1 Minute|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren des Instanzspeichers (Dienstebene „Universell“)|Anfügung von Datenbankdateien|90 % der Vorgänge werden innerhalb von fünf Minuten abgeschlossen.|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren des Instanzspeichers (Tarif „Unternehmenskritisch“)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen.|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Universell)|- Änderung der Größe eines virtuellen Clusters<br>- Anfügung von Datenbankdateien|90 % der Vorgänge werden innerhalb von 2,5 Stunden abgeschlossen.|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Unternehmenskritisch)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen.|
|Aktualisieren |Änderung der Instanzdienstebene („Universell“ in „Unternehmenskritisch“ und umgekehrt)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen.|
|**Löschung**|Instanzlöschung|Log Tail-Sicherung für alle Datenbanken|90 % der Vorgänge werden innerhalb einer Minute abgeschlossen.<br>Hinweis: Wenn die letzte Instanz im Subnetz gelöscht wird, wird bei diesem Vorgang das Löschen des virtuellen Clusters nach Ablauf von zwölf Stunden geplant.**|
|Löschen|Löschung eines virtuellen Clusters (als vom Benutzer initiierter Vorgang)|Löschung eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von 1,5 Stunden abgeschlossen.|

\* Der virtuelle Cluster wird pro Hardwaregeneration erstellt.

\*\* Bei der aktuellen Konfiguration wird „12 Stunden“ verwendet, aber da sich dies in Zukunft ändern kann, sollten Sie hierfür keine festen Abhängigkeiten einrichten. Falls Sie einen virtuellen Cluster früher löschen müssen (z. B. um das Subnetz freizugeben), helfen Ihnen die Informationen unter [Löschen eines Subnetzes nach Löschen einer verwalteten Instanz](virtual-cluster-delete.md) weiter.

## <a name="instance-availability-during-management-operations"></a>Instanzverfügbarkeit während Verwaltungsvorgängen

SQL Managed Instance ist **bei Updatevorgängen verfügbar**, aber es kommt aufgrund des Failovers zu einem kurzen Ausfall, das am Ende von Updates ausgeführt wird. Er dauert dank der [beschleunigten Datenbankwiederherstellung](../accelerated-database-recovery.md) in der Regel bis zu 10 Sekunden, auch bei unterbrochenen Transaktionen mit langer Ausführungszeit.

> [!IMPORTANT]
> Es wird nicht empfohlen, die Compute- oder Speicherkapazität von Azure SQL Managed Instance zu skalieren oder die Dienstebene gleichzeitig mit den Transaktionen mit langer Ausführungsdauer (Datenimport, Datenverarbeitungsaufträge, Neuerstellung des Index usw.) zu ändern. Beim Datenbankfailover, das am Ende des Vorgangs ausgeführt wird, werden alle laufenden Transaktionen abgebrochen.

SQL Managed Instance ist während Bereitstellungs- und Löschvorgängen für Clientanwendungen nicht verfügbar.

## <a name="management-operations-cross-impact"></a>Wechselseitige Auswirkungen von Verwaltungsvorgängen

Verwaltungsvorgänge für verwaltete Instanzen können sich auf andere Verwaltungsvorgänge der Instanzen auswirken, die sich in demselben virtuellen Cluster befinden:

- **Wiederherstellungsvorgänge mit langer Ausführungszeit** in einem virtuellen Cluster stellen Instanzerstellungs- oder Skalierungsvorgänge anderer Instanzen im gleichen Subnetz zurück.<br/>**Beispiel:** Wenn ein langer Wiederherstellungsvorgang ausgeführt wird und eine Anforderung zum Erstellen oder Skalieren im gleichen Subnetz vorhanden ist, dauert es bis zum Abschluss länger, weil gewartet wird, bis der Wiederherstellungsvorgang beendet ist.
    
- **Nachfolgende Instanzerstellungs- oder Skalierungsvorgänge** werden durch zuvor initiierte Instanzerstellungen oder -skalierungen zurückgestellt, die die Größenänderung des virtuellen Clusters veranlasst haben.<br/>**Beispiel:** Wenn es im selben Subnetz auf demselben virtuellen Cluster mehrere Anforderungen zum Erstellen und/oder Skalieren gibt und eine davon die Größenänderung des virtuellen Clusters initiiert, dauern alle Anforderungen, die mehr als fünf Minuten nach der Anforderung der Größenänderung des virtuellen Clusters übermittelt wurden, länger als erwartet, da diese Anforderungen auf den Abschluss der Größenänderung warten müssen, bevor sie fortgesetzt werden können.

- **Erstellungs-/Skalierungsvorgänge, die im fünfminütigen Fenster übermittelt werden**, werden in einem Batch zusammengefasst und parallel ausgeführt.<br/>**Beispiel:** Für alle Vorgänge, die in einem fünfminütigen Fenster übermittelt werden, wird nur eine Größenänderung des virtuellen Clusters ausgeführt (gemessen ab dem Zeitpunkt der Ausführung der ersten Vorgangsanforderung). Wenn eine andere Anforderung mehr als fünf Minuten nach der ersten Anforderung übermittelt wird, wartet sie, bis die Größenänderung des virtuellen Clusters abgeschlossen ist, bevor die Ausführung beginnt.

> [!IMPORTANT]
> Verwaltungsvorgänge, die aufgrund eines anderen aktuell ausgeführten Vorgangs zurückgestellt werden, werden automatisch fortgesetzt, sobald die Bedingungen für eine Fortsetzung erfüllt sind. Es ist keine Benutzeraktion erforderlich, um den temporär angehaltenen Verwaltungsvorgang fortzusetzen.

## <a name="canceling-management-operations"></a>Abbrechen von Verwaltungsvorgängen

In der folgenden Tabelle sind die Möglichkeit zum Abbrechen von speziellen Verwaltungsvorgängen und die typischen Gesamtdauern zusammengefasst:

Category  |Vorgang  |Kann abgebrochen werden  |Geschätzte Abbruchdauer  |
|---------|---------|---------|---------|
|Bereitstellung |Erstellen einer Instanz |Nein |  |
|Aktualisieren |Zentrales Hoch-/Herunterskalieren des Instanzspeichers (universell) |Nein |  |
|Aktualisieren |Zentrales Hoch-/Herunterskalieren des Instanzspeichers (unternehmenskritisch) |Ja |90 % der Vorgänge werden innerhalb von fünf Minuten abgeschlossen. |
|Aktualisieren |Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Universell) |Ja |90 % der Vorgänge werden innerhalb von fünf Minuten abgeschlossen. |
|Aktualisieren |Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Unternehmenskritisch) |Ja |90 % der Vorgänge werden innerhalb von fünf Minuten abgeschlossen. |
|Aktualisieren |Änderung der Instanzdienstebene („Universell“ in „Unternehmenskritisch“ und umgekehrt) |Ja |90 % der Vorgänge werden innerhalb von fünf Minuten abgeschlossen. |
|Löschen |Instanzlöschung |Nein |  |
|Löschen |Löschung eines virtuellen Clusters (als vom Benutzer initiierter Vorgang) |Nein |  |

# <a name="portal"></a>[Portal](#tab/azure-portal)

Wechseln Sie zum Blatt „Übersicht“, und klicken Sie auf das Benachrichtigungsfeld des laufenden Vorgangs, um den Verwaltungsvorgang abzubrechen. Auf der rechten Seite wird ein Bildschirm mit dem laufenden Vorgang und einer Schaltfläche zum Abbrechen des Vorgangs angezeigt. Nach dem ersten Klick werden Sie aufgefordert, noch mal zu klicken und zu bestätigen, dass Sie den Vorgang abbrechen möchten.

[![Abbrechen eines Vorgangs](./media/management-operations-overview/canceling-operation.png)](./media/management-operations-overview/canceling-operation.png#lightbox)

Nachdem die Abbruchanforderung übermittelt und verarbeitet wurde, erhalten Sie eine Benachrichtigung, wenn die Übermittlung erfolgreich war.

Wenn die gesendete Abbruchanforderung erfolgreich war, wird der Verwaltungsvorgang innerhalb weniger Minuten abgebrochen, und dies führt zu einem Fehler.

![Ergebnis des Vorgangsabbruchs](./media/management-operations-overview/canceling-operation-result.png)

Wenn bei der Abbruchanforderung ein Fehler auftritt oder die Schaltfläche „Abbrechen“ nicht aktiv ist, bedeutet das, dass der Verwaltungsvorgang nicht abgebrochen werden konnte und in wenigen Minuten abgeschlossen wird. Der Verwaltungsvorgang wird so lange ausgeführt, bis er abgeschlossen wurde.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Sollte Azure PowerShell noch nicht installiert sein, finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps) weitere Informationen.

Zum Abbrechen des Verwaltungsvorgangs müssen Sie seinen Namen angeben. Verwenden Sie daher zuerst den get-Befehl zum Abrufen der Vorgangsliste, und brechen Sie den Vorgang anschließend ab.

```powershell-interactive
$managedInstance = "yourInstanceName"
$resourceGroup = "yourResourceGroupName"

$managementOperations = Get-AzSqlInstanceOperation -ManagedInstanceName $managedInstance  -ResourceGroupName $resourceGroup

foreach ($mo in $managementOperations ) {
    if($mo.State -eq "InProgress" -and $mo.IsCancellable){
        $cancelRequest = Stop-AzSqlInstanceOperation -ResourceGroupName $resourceGroup -ManagedInstanceName $managedInstance -Name $mo.Name
        Get-AzSqlInstanceOperation -ManagedInstanceName $managedInstance  -ResourceGroupName $resourceGroup -Name $mo.Name
    }
}
```

Eine ausführliche Beschreibung der Befehle finden Sie unter [Get-AzSqlInstanceOperation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstanceoperation) und [Stop-AzSqlInstanceOperation](https://docs.microsoft.com/powershell/module/az.sql/stop-azsqlinstanceoperation).

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Sollte die Azure CLI noch nicht installiert sein, lesen Sie [Installieren der Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

Zum Abbrechen des Verwaltungsvorgangs müssen Sie seinen Namen angeben. Verwenden Sie daher zuerst den get-Befehl zum Abrufen der Vorgangsliste, und brechen Sie den Vorgang anschließend ab.

```azurecli-interactive
az sql mi op list -g yourResourceGroupName --mi yourInstanceName --query "[?state=='InProgress' && isCancellable].{Name: name}" -o tsv |
while read -r operationName; do
    az sql mi op cancel -g yourResourceGroupName --mi yourInstanceName -n $operationName
done
```

Eine ausführliche Beschreibung der Befehle finden Sie unter [az sql mi op](https://docs.microsoft.com/cli/azure/sql/mi/op?view=azure-cli-latest).

---

## <a name="next-steps"></a>Nächste Schritte
- Im [Schnellstarthandbuch](instance-create-quickstart.md) erfahren Sie, wie Sie Ihre erste verwaltete Instanz erstellen.
- Eine Liste der Features und einen Funktionsvergleich finden Sie unter [Allgemeine SQL-Features](../database/features-comparison.md).
- Weitere Informationen zur VNET-Konfiguration finden Sie unter [Konfigurieren eines VNET für SQL Managed Instance](connectivity-architecture-overview.md).
- Eine Schnellstartanleitung, in der eine verwaltete Instanz erstellt und eine Datenbank aus einer Sicherungsdatei wiederhergestellt wird, finden Sie unter [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](instance-create-quickstart.md).
- Ein Tutorial zur Verwendung des Azure Database Migration Service für die Migration finden Sie unter [SQL Managed Instance-Migration mithilfe des Database Migration Service](../../dms/tutorial-sql-server-to-managed-instance.md).
