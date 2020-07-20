---
title: Überwachungslösungen in Azure Monitor | Microsoft-Dokumentation
description: Bei Überwachungslösungen in Azure Monitor handelt es sich um eine Sammlung von Logik-, Visualisierungs- und Datenerfassungsregeln, die Metriken zu einem bestimmten Problembereich bereitstellen.  Dieser Artikel enthält Informationen zur Installation und Verwendung von Überwachungslösungen.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/12/2020
ms.openlocfilehash: 4edcb22ed6bd33b1174354cf0cbb9a590e35c207
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84906886"
---
# <a name="monitoring-solutions-in-azure-monitor"></a>Überwachungslösungen in Azure Monitor
Überwachungslösungen nutzen Dienste in Azure, um eine zusätzliche Analyse des Betriebs einer bestimmten Anwendung oder eines bestimmten Diensts zu ermöglichen. Dieser Artikel enthält eine kurze Übersicht über Überwachungslösungen in Azure sowie Details zu deren Verwendung und Installation. Überwachungslösungen können Azure Monitor für beliebige Anwendungen und Dienste hinzugefügt werden, die Sie verwenden. Die Lösungen sind in der Regel kostenlos, durch die Datensammlung können jedoch Kosten entstehen.

## <a name="use-monitoring-solutions"></a>Verwenden von Überwachungslösungen

Beim Öffnen der **Übersichtsseite** in Azure Monitor wird für jede im Workspace installierte Lösung eine Kachel angezeigt. 

1. Öffnen Sie das [Azure-Portal](https://ms.portal.azure.com). Suchen Sie nach **Überwachen**, und wählen Sie diese Option aus.
1. Wählen Sie im Menü **Insights** die Option **Mehr** aus.
1. Ändern Sie über die Dropdownfelder am oberen Bildschirmrand den Arbeitsbereich oder den Zeitbereich für die Kacheln.
1. Klicken Sie auf die Kachel für eine Lösung, um die dazugehörige Ansicht mit einer ausführlicheren Analyse der gesammelten Daten anzuzeigen.

![Übersicht](media/solutions/overview.png)

Überwachungslösungen können mehrere Arten von Azure-Ressourcen enthalten, und Sie können beliebige, in einer Lösung enthaltene Ressourcen anzeigen (genau wie bei anderen Ressourcen). So werden z.B. alle in der Lösung enthaltenen Protokollabfragen unter **Lösungsabfragen** im [Abfrage-Explorer](../log-query/get-started-portal.md#load-queries) aufgelistet. Sie können diese Abfragen verwenden, wenn Sie mit [Protokollabfragen](../log-query/log-query-overview.md) eine Ad-hoc-Analyse durchführen.

## <a name="list-installed-monitoring-solutions"></a>Auflisten installierter Überwachungslösungen

Gehen Sie wie folgt vor, um die in Ihrem Abonnement installierten Überwachungslösungen aufzulisten.

1. Öffnen Sie das [Azure-Portal](https://ms.portal.azure.com). Suchen Sie nach **Lösungen**, und wählen Sie diese Option aus.
1. Alle in Ihren Arbeitsbereichen installierten Lösungen werden aufgelistet. Auf den Namen der Lösung folgt der Name des Arbeitsbereichs, in dem sie installiert ist.
1. Mit den Dropdownfeldern am oberen Bildschirmrand können Sie die Anzeige nach Abonnement oder Ressourcengruppe filtern.


![Auflisten aller Lösungen](media/solutions/list-solutions-all.png)

Klicken Sie auf den Namen einer Lösung, um die dazugehörige Zusammenfassungsseite zu öffnen. Auf dieser Seite werden alle in der Lösung enthaltenen Ansichten angezeigt, und es stehen verschiedene Optionen für die Lösung und den Arbeitsbereich zur Verfügung. Zeigen Sie die Zusammenfassungsseite für eine Lösung an. Verwenden Sie hierzu eines der obigen Verfahren zum Auflisten der Lösungen, und klicken Sie anschließend auf den Namen der Lösung.

![Lösungseigenschaften](media/solutions/solution-properties.png)

## <a name="install-a-monitoring-solution"></a>Installieren einer Überwachungslösung

Überwachungslösungen von Microsoft und Partnern sind im [Azure Marketplace](https://azuremarketplace.microsoft.com) verfügbar. Sie können nach verfügbaren Lösungen suchen und sie mit folgendem Verfahren installieren. Wenn Sie eine Lösung installieren, müssen Sie einen [Log Analytics-Arbeitsbereich](../platform/manage-access.md) auswählen, in dem die Lösung installiert wird und in dem ihre Daten gesammelt werden.

1. Klicken Sie in der [Liste mit Lösungen für Ihr Abonnement](#list-installed-monitoring-solutions) auf **Hinzufügen**.
1. Durchsuchen Sie eine Projektmappe oder suchen Sie sie. Sie können auch Projektmappen über [diesen Suchlink](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/management-tools?page=1&subcategories=management-solutions) durchsuchen.
1. Suchen Sie die gewünschte Überwachungslösung, und lesen Sie ihre Beschreibung.
1. Klicken Sie auf **Erstellen**, um den Installationsprozess zu starten.
1. Wenn der Installationsvorgang gestartet wird, werden Sie aufgefordert, den Log Analytics-Arbeitsbereich anzugeben und die erforderliche Konfiguration für die Projektmappe bereitzustellen.

![Installieren einer Lösung](media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Installieren einer Lösung aus der Community

Mitglieder der Community können Verwaltungslösungen für Azure-Schnellstartvorlagen senden. Sie können diese Lösungen entweder direkt installieren oder Vorlagen für eine spätere Installation herunterladen.

1. Führen Sie den unter [Log Analytics-Arbeitsbereich und Automation-Konto](#log-analytics-workspace-and-automation-account) beschriebenen Prozess aus, um einen Arbeitsbereich und ein Konto zu verknüpfen.
2. Gehen Sie zu [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/). 
3. Suchen Sie nach einer Lösung, an der Sie interessiert sind.
4. Wählen Sie die Lösung aus den Ergebnissen aus, um ihre Details anzuzeigen.
5. Klicken Sie auf die Schaltfläche **Bereitstellen in Azure**.
6. Sie werden aufgefordert, Informationen wie die Ressourcengruppe und den Standort sowie Werte für Parameter in der Lösung anzugeben.
7. Klicken Sie auf **Kaufen**, um die Lösung zu installieren.

## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics-Arbeitsbereich und Automation-Konto

Für alle Überwachungslösungen wird ein [Log Analytics-Arbeitsbereich](../platform/manage-access.md) zum Speichern der von der Lösung gesammelten Daten sowie zum Hosten der Protokollsuchvorgänge und Ansichten benötigt. Einige Lösungen erfordern auch ein [Automation-Konto](../../automation/automation-security-overview.md) für Runbooks und zugehörige Ressourcen. Der Arbeitsbereich und das Konto müssen den folgenden Anforderungen entsprechen.

* Eine Lösungsinstallation kann jeweils nur einen Log Analytics-Arbeitsbereich und ein einzelnes Automation-Konto verwenden. Sie können die Lösung separat in mehreren Arbeitsbereichen installieren.
* Sollte eine Lösung ein Automation-Konto benötigen, müssen der Log Analytics-Arbeitsbereich und das Automation-Konto miteinander verknüpft werden. Ein Log Analytics-Arbeitsbereich kann nur mit einem Automation-Konto verknüpft werden, und ein Automation-Konto kann nur mit einem Log Analytics-Arbeitsbereich verknüpft werden.
* Für eine Verknüpfung müssen sich der Log Analytics-Arbeitsbereich und das Automation-Konto im selben Abonnement befinden, können sich aber in unterschiedlichen Ressourcengruppen befinden, die in derselben Region bereitgestellt sind. (Ausnahme: Ein Arbeitsbereich befindet sich in der Region „USA, Osten“ und ein Automation-Konto in der Region „USA, Osten 2“.)

Wenn Sie die Lösung über den Azure Marketplace installieren, werden Sie zur Angabe eines Arbeitsbereichs und eines Automation-Kontos aufgefordert. Die Verknüpfung zwischen beiden wird für Sie erstellt, sofern sie noch nicht besteht.

### <a name="verify-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Überprüfen der Verknüpfung zwischen einem Log Analytics-Arbeitsbereich und einem Automation-Konto

Sie können die Verknüpfung zwischen einem Log Analytics-Arbeitsbereich und einem Automation-Konto mithilfe des folgenden Verfahrens überprüfen.

1. Wählen Sie im Azure-Portal das Automation-Konto aus.
1. Scrollen Sie zum Abschnitt **Zugehörige Ressourcen** des Menüs, und wählen Sie **Verknüpfter Arbeitsbereich** aus.
1. Wenn der **Arbeitsbereich** mit einem Automation-Konto verknüpft ist, wird auf dieser Seite der Arbeitsbereich aufgelistet, mit dem es verknüpft ist. Wenn Sie den Namen des aufgelisteten Arbeitsbereichs auswählen, werden Sie zur Übersichtsseite für diesen Arbeitsbereich weitergeleitet.

## <a name="remove-a-monitoring-solution"></a>Entfernen einer Überwachungslösung

Wenn Sie eine installierte Lösung entfernen möchten, suchen Sie sie in der [Liste mit den installierten Lösungen](#list-installed-monitoring-solutions). Klicken Sie auf den Namen der Lösung, um die dazugehörige Zusammenfassungsseite zu öffnen, und klicken Sie anschließend auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

* Rufen Sie eine [Liste mit Überwachungslösungen von Microsoft](solutions-inventory.md) ab.
* Machen Sie sich mit dem [Erstellen von Abfragen](../log-query/log-query-overview.md) für die Analyse der von Überwachungslösungen gesammelten Daten vertraut.