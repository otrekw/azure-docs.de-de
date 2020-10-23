---
title: Löschen und Wiederherstellen eines Azure Log Analytics-Arbeitsbereichs | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Log Analytics-Arbeitsbereich löschen, wenn Sie einen Arbeitsbereich in einem persönlichen Abonnement erstellt haben oder Ihr Arbeitsbereichsmodell neu strukturieren.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/26/2020
ms.openlocfilehash: 292e446d5b713a43f77ee5e579d7e6dd5905ff69
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91448527"
---
# <a name="delete-and-recover-azure-log-analytics-workspace"></a>Löschen und Wiederherstellen eines Azure Log Analytics-Arbeitsbereichs

In diesem Artikel werden das Konzept des vorläufigen Löschens eines Azure Log Analytics-Arbeitsbereichs und die Wiederherstellung eines gelöschten Arbeitsbereichs erläutert.

## <a name="considerations-when-deleting-a-workspace"></a>Überlegungen zum Löschen eines Arbeitsbereichs

Wenn Sie einen Log Analytics-Arbeitsbereich löschen, wird ein vorläufiger Löschvorgang durchgeführt, um die Wiederherstellung des Arbeitsbereichs einschließlich der zugehörigen Daten und verbundenen Agents innerhalb von 14 Tagen zu ermöglichen, unabhängig davon, ob der Löschvorgang versehentlich oder gezielt durchgeführt wurde. Nach Ablauf des Zeitraums für vorläufiges Löschen können die Arbeitsbereichsressource und die zugehörigen Daten nicht mehr wiederhergestellt werden. Die Daten werden in die Warteschlange zum dauerhaften Löschen gestellt und innerhalb von 30 Tagen vollständig gelöscht. Der Arbeitsbereichsname wird „freigegeben“ und kann zum Erstellen eines neuen Arbeitsbereichs verwendet werden.

> [!NOTE]
> Wenn Sie das vorläufige Löschen außer Kraft setzen und den Arbeitsbereich dauerhaft löschen möchten, führen Sie die Schritte unter [Dauerhaftes Löschen eines Arbeitsbereichs](#permanent-workspace-delete) aus.

Gehen Sie beim Löschen eines Arbeitsbereichs vorsichtig vor, da er unter Umständen wichtige Daten und Konfigurationen enthält, deren Löschung sich negativ auf den Dienstvorgang auswirken kann. Überprüfen Sie die Agents, Lösungen und anderen Azure-Dienste, deren Daten in Log Analytics gespeichert werden, z. B.:

* Verwaltungslösungen
* Azure-Automatisierung
* Auf virtuellen Windows- und Linux-Computern ausgeführte Agents
* Auf Windows- und Linux-Computern in Ihrer Umgebung ausgeführte Agents
* System Center Operations Manager

Beim vorläufigen Löschen wird die Arbeitsbereichsressource gelöscht und die Berechtigung aller zugeordneten Benutzer aufgehoben. Wenn Benutzer anderen Arbeitsbereichen zugeordnet sind, können sie Log Analytics mit diesen Arbeitsbereichen weiter nutzen.

## <a name="soft-delete-behavior"></a>Verhalten des vorläufigen Löschens

Mit dem Löschvorgang des Arbeitsbereichs wird die Resource Manager-Ressource des Arbeitsbereichs gelöscht, die zugehörige Konfiguration und die zugehörigen Daten werden jedoch 14 Tage lang beibehalten, obwohl der Arbeitsbereich dem Anschein nach gelöscht ist. Alle für die Berichterstellung im Arbeitsbereich konfigurierten Agents und System Center Operations Manager-Verwaltungsgruppen werden während des Zeitraums des vorläufigen Löschens in einem verwaisten Status beibehalten. Der Dienst umfasst darüber hinaus einen Mechanismus zur Wiederherstellung des gelöschten Arbeitsbereichs einschließlich der zugehörigen Daten und verbundenen Ressourcen, bei dem der Löschvorgang im Wesentlichen rückgängig gemacht wird.

> [!NOTE] 
> Installierte Lösungen und verknüpfte Dienste wie Ihr Azure Automation-Konto werden zum Zeitpunkt der Löschung dauerhaft aus dem Arbeitsbereich entfernt und können nicht wiederhergestellt werden. Diese sollten nach dem Wiederherstellungsvorgang neu konfiguriert werden, um den vorherigen konfigurierten Zustand des Arbeitsbereichs wiederherzustellen.

Sie können einen Arbeitsbereich mithilfe von [PowerShell](/powershell/module/azurerm.operationalinsights/remove-azurermoperationalinsightsworkspace?view=azurermps-6.13.0), über die [REST-API](/rest/api/loganalytics/workspaces/delete) oder im [Azure-Portal](https://portal.azure.com) löschen.

### <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics-Arbeitsbereiche** aus.
3. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche einen Arbeitsbereich aus, und klicken Sie dann oben im mittleren Bereich auf **Löschen**.
4. Es wird eine Bestätigungsseite mit der Datenerfassung für den Arbeitsbereich in der letzten Woche angezeigt. Geben Sie zur Bestätigung den Namen des Arbeitsbereichs ein, und klicken Sie dann auf **Löschen**.

   ![Bestätigen des Löschvorgangs für den Arbeitsbereich](media/delete-workspace/workspace-delete.png)

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Remove-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

## <a name="permanent-workspace-delete"></a>Dauerhaftes Löschen eines Arbeitsbereichs
Die Methode des vorläufigen Löschens ist in einigen Szenarien möglicherweise nicht geeignet, in denen Sie eine Bereitstellung mit denselben Einstellungen und demselben Arbeitsbereichsnamen wiederholen müssen (z.B. bei Entwicklung und Test). In solchen Fällen können Sie den Arbeitsbereich dauerhaft löschen und den Zeitraum der vorläufigen Löschung überschreiben. Bei der dauerhaften Löschung des Arbeitsbereichs wird der Arbeitsbereichsname freigegeben, und Sie können einen neuen Arbeitsbereich mit demselben Namen erstellen.


> [!IMPORTANT]
> Verwenden Sie dauerhafte Löschungen von Arbeitsbereichen mit Vorsicht, da dieser Vorgang nicht rückgängig gemacht werden kann. Sie können dann weder den Arbeitsbereich noch dessen Daten wiederherstellen.

Fügen Sie das Tag „-forceDelete“ hinzu, um Ihren Arbeitsbereich dauerhaft zu löschen. Die Option „-ForceDelete“ ist derzeit in Az.OperationalInsights 2.3.0 oder höher verfügbar. 

```powershell
PS C:\>Remove-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name" -ForceDelete
```

## <a name="recover-workspace"></a>Wiederherstellen eines Arbeitsbereichs
Wenn Sie einen Log Analytics-Arbeitsbereich versehentlich oder absichtlich löschen, versetzt der Dienst den Arbeitsbereich in den Zustand des vorläufigen Löschens, sodass von keinem Vorgang darauf zugegriffen werden kann. Der Name des gelöschten Arbeitsbereichs bleibt während des Zeitraums des vorläufigen Löschens erhalten und kann nicht zum Erstellen eines neuen Arbeitsbereichs verwendet werden. Sobald der Zeitraum des vorläufigen Löschens abgelaufen ist, kann der Arbeitsbereich nicht wiederhergestellt werden, er ist für dauerhaftes Löschen geplant, und der Name des Arbeitsbereichs wird freigegeben und kann zum Erstellen eines neuen Arbeitsbereichs verwendet werden.

Während des Zeitraums des vorläufigen Löschens kann der Arbeitsbereich wiederhergestellt werden, einschließlich Daten, Konfiguration und verbundener Agents. Sie benötigen Berechtigungen vom Typ „Mitwirkender“ für das Abonnement und die Ressourcengruppe, in der sich der Arbeitsbereich vor dem vorläufigen Löschen befand. Zum Wiederherstellen des Arbeitsbereichs wird ein Log Analytics-Arbeitsbereich mit den Details des gelöschten Arbeitsbereichs erstellt. Dazu gehört Folgendes:

- Abonnement-ID
- Ressourcengruppenname
- Arbeitsbereichname
- Region

### <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics-Arbeitsbereiche** aus. Die Liste der Arbeitsbereiche im ausgewählten Bereich wird angezeigt.
3. Klicken Sie im Menü oben links auf **Wiederherstellen**, um eine Seite mit Arbeitsbereichen im Zustand des vorläufigen Löschens, die wiederhergestellt werden können, zu öffnen.

   ![Screenshot der Anzeige von Log Analytics-Arbeitsbereichen im Azure-Portal mit in der Menüleiste hervorgehobener Option „Wiederherstellen“](media/delete-workspace/recover-menu.png)

4. Wählen Sie den Arbeitsbereich aus, und klicken Sie zum Wiederherstellen dieses Arbeitsbereichs auf **Wiederherstellen**.

   ![Screenshot des Dialogfelds „Recover deleted Log Analytics workspaces“ (Wiederherstellen gelöschter Log Analytics-Arbeitsbereiche) im Azure-Portal mit hervorgehobenem Arbeitsbereich und ausgewählter Schaltfläche „Wiederherstellen“](media/delete-workspace/recover-workspace.png)


### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Select-AzSubscription "subscription-name-the-workspace-was-in"
PS C:\>New-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name-the-workspace-was-in" -Name "deleted-workspace-name" -Location "region-name-the-workspace-was-in"
```

Der Arbeitsbereich und alle zugehörigen Daten sind nach dem Wiederherstellungsvorgang wieder vorhanden. Lösungen und verknüpfte Dienste wurden dauerhaft aus dem Arbeitsbereich entfernt, als dieser gelöscht wurde, und sollten daher neu konfiguriert werden, um den zuvor konfigurierten Zustand des Arbeitsbereichs wiederherzustellen. Einige Daten können nach der Wiederherstellung des Arbeitsbereichs möglicherweise nicht abgefragt werden, ehe die zugehörigen Lösungen neu installiert und deren Schemas dem Arbeitsbereich hinzugefügt wurden.

> [!NOTE]
> * Beim erneuten Erstellen eines Arbeitsbereichs während des Zeitraums des vorläufigen Löschens wird darauf hingewiesen, dass der Name des Arbeitsbereichs bereits verwendet wird. 
 
## <a name="troubleshooting"></a>Problembehandlung

Sie benötigen mindestens die Berechtigungen der Rolle *Log Analytics-Mitwirkender*, um einen Arbeitsbereich zu löschen.

* Wenn Sie sich nicht sicher sind, ob sich der gelöschte Arbeitsbereich im Zustand des vorläufigen Löschens befindet und wiederhergestellt werden kann, klicken Sie auf der Seite *Log Analytics-Arbeitsbereiche* auf [Wiederherstellen](#recover-workspace), um eine Liste vorläufig gelöschter Arbeitsbereiche nach Abonnement anzuzeigen. Dauerhaft gelöschte Arbeitsbereiche sind in der Liste nicht enthalten.
* Wenn Sie beim Erstellen eines Arbeitsbereichs die Fehlermeldung *Dieser Arbeitsbereichsname wird bereits verwendet* oder einen *Konflikt* erhalten, kann dies folgende Gründe haben:
  * Der Name des Arbeitsbereichs ist nicht verfügbar und wird bereits von jemandem in Ihrer Organisation oder von einem anderen Kunden verwendet.
  * Der Arbeitsbereich wurde innerhalb der letzten 14 Tage gelöscht, und der Name wurde für den Zeitraum der vorläufigen Löschung reserviert. Zum Überschreiben der vorübergehenden Löschung und der dauerhaften Löschen des Arbeitsbereichs, um einen neuen, gleichnamigen Arbeitsbereich zu erstellen, gehen Sie folgendermaßen vor, um den Arbeitsbereich zunächst wiederherzustellen und dann dauerhaft zu löschen:<br>
     1. [Stellen Sie Ihren Arbeitsbereich wieder her.](#recover-workspace)
     2. [Löschen Sie Ihren Arbeitsbereich dauerhaft.](#permanent-workspace-delete)
     3. Erstellen Sie einen neuen Arbeitsbereich mit demselben Arbeitsbereichnamen.
