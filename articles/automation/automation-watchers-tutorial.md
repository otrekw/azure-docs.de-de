---
title: Nachverfolgen aktualisierter Dateien mit einem Azure Automation-Watchertask
description: In diesem Artikel erfahren Sie, wie Sie einen Watchertask im Azure Automation-Konto erstellen, um die Erstellung neuer Dateien in einem Ordner zu überwachen.
services: automation
ms.subservice: process-automation
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 3369a807410e9e959e8091d5b16c8480803d26bb
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83830581"
---
# <a name="track-updated-files-with-a-watcher-task"></a>Nachverfolgen von aktualisierten Dateien mit einem Watchertask

Azure Automation verwendet einen Watchertask für eine Überwachung auf Ereignisse und Triggeraktionen in PowerShell-Runbooks. Der Watchertask besteht aus zwei Teilen: Watcher und Aktion. Ein Watcherrunbook wird in einem Intervall ausgeführt, das im Watchertask definiert ist, und gibt Daten in ein Aktionsrunbook aus. 

> [!NOTE]
> Watchertasks werden in Azure China 21Vianet nicht unterstützt.

> [!IMPORTANT]
> Seit Mai 2020 ist die Verwendung von Azure Logic Apps die unterstützte Methode zum Überwachen auf Ereignisse, zum Planen von wiederkehrenden Aufgaben und zum Auslösen von Aktionen. Informationen finden Sie unter [Planen und Ausführen von wiederkehrenden automatisierten Aufgaben, Prozessen und Workflows mit Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/concepts-schedule-automated-recurring-tasks-workflows).

Dieses Tutorial führt Sie durch die Schritte zum Erstellen eines Watchertasks, mit dem Sie überwachen können, ob einem Verzeichnis eine neue Datei hinzugefügt wird. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Importieren eines Watcherrunbooks
> * Erstellen einer Automation-Variable
> * Erstellen eines Aktionsrunbooks
> * Erstellen eines Watchertasks
> * Auslösen eines Watchers
> * Untersuchen der Ausgabe

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorials müssen folgende Voraussetzungen erfüllt sein:

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md), um die Watcher- und Aktionsrunbooks und den Watchertask aufzunehmen.
* [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md), in dem der Watchertask ausgeführt wird.
* PowerShell-Runbooks. PowerShell-Workflow-Runbooks werden von Watchertasks nicht unterstützt.

## <a name="import-a-watcher-runbook"></a>Importieren eines Watcherrunbooks

Dieses Tutorial verwendet ein Watcherrunbook namens **Watch-NewFile**, um nach neuen Dateien in einem Verzeichnis zu suchen. Das Watcherrunbook ruft die Uhrzeit des letzten bekannten Schreibzugriffs auf die Dateien in einem Ordner ab und sucht nach Dateien, die jünger sind als dieser Zeitpunkt.

Dieser Importvorgang kann auch über den [PowerShell-Katalog](https://www.powershellgallery.com) durchgeführt werden.

1. Navigieren Sie zur Katalogseite für [Watch-NewFile.ps1](https://gallery.technet.microsoft.com/scriptcenter/Watcher-runbook-that-looks-36fc82cd).
2. Klicken Sie auf der Registerkarte **Azure Automation** auf **Deploy to Azure Automation** (In Azure Automation bereitstellen).

Sie können dieses Runbook auch mithilfe der folgenden Schritte über das Portal in Ihr Automation-Konto importieren.

1. Öffnen Sie Ihr Automation-Konto, und klicken Sie auf die Seite „Runbooks“.
2. Klicken Sie auf **Katalog durchsuchen**.
3. Suchen Sie nach **Watcherrunbook**. Wählen Sie **Watcherrunbook, das nach neuen Dateien in einem Verzeichnis sucht** aus, und klicken Sie auf **Importieren**.
  ![Importieren eines Automation-Runbooks über die Benutzeroberfläche](media/automation-watchers-tutorial/importsourcewatcher.png)
4. Geben Sie einen Namen und eine Beschreibung für das Runbook ein, und klicken Sie auf **OK**, um das Runbook in Ihr Automation-Konto zu importieren.
5. Wählen Sie **Bearbeiten** aus, und klicken Sie dann auf **Veröffentlichen**. Wählen Sie bei der Aufforderung **Ja** aus, um das Runbook zu veröffentlichen.

## <a name="create-an-automation-variable"></a>Erstellen einer Automation-Variable

Eine [Automation-Variable](automation-variables.md) wird verwendet, um die Zeitstempel zu speichern, die das vorhergehende Runbook aus jeder Datei liest und speichert.

1. Wählen Sie unter **Freigegebene Ressourcen** die Option **Variablen** aus, und klicken Sie dann auf **+Variable hinzufügen**.
1. Geben Sie als Namen „Watch-NewFileTimestamp“ ein.
1. Wählen Sie den Typ „DateTime“ aus.
1. Klicken Sie auf **Erstellen**, um die Automation-Variable zu erstellen.

## <a name="create-an-action-runbook"></a>Erstellen eines Aktionsrunbooks

Ein Aktionsrunbook wird in einem Watchertask verwendet, um Aktionen für die von einem Watcherrunbook übergebenen Daten auszuführen. Sie müssen ein vordefiniertes Aktionsrunbook namens **Process-NewFile** aus dem [PowerShell-Katalog](https://www.powershellgallery.com) importieren. 

So erstellen Sie ein Aktionsrunbook

1. Navigieren Sie zur Katalogseite für [Process-NewFile.ps1](https://gallery.technet.microsoft.com/scriptcenter/Watcher-action-that-b4ff7cdf).
2. Klicken Sie auf der Registerkarte **Azure Automation** auf **Deploy to Azure Automation** (In Azure Automation bereitstellen).

Sie können dieses Runbook auch im Azure-Portal in Ihr Automation-Konto importieren:

1. Navigieren Sie zu Ihrem Automation-Konto, und wählen Sie unter **Prozessautomatisierung** die Option **Runbooks** aus.
1. Klicken Sie auf **Katalog durchsuchen**.
1. Suchen Sie nach **Watcheraktion**. Wählen Sie **Watcheraktion, die von einem Watcherrunbook ausgelöste Ereignisse verarbeitet** aus, und klicken Sie auf **Importieren**.
  ![Importieren eines Aktionsrunbooks über die Benutzeroberfläche](media/automation-watchers-tutorial/importsourceaction.png)
1. Geben Sie einen Namen und eine Beschreibung für das Runbook ein, und klicken Sie auf **OK**, um das Runbook in Ihr Automation-Konto zu importieren.
1. Wählen Sie **Bearbeiten** aus, und klicken Sie dann auf **Veröffentlichen**. Wählen Sie bei der Aufforderung **Ja** aus, um das Runbook zu veröffentlichen.

## <a name="create-a-watcher-task"></a>Erstellen eines Watchertasks

In diesem Schritt konfigurieren Sie den Watchertask und verweisen dabei auf die Watcher- und Aktionsrunbooks, die in den vorherigen Abschnitten definiert wurden.

1. Navigieren Sie zu Ihrem Automation-Konto, und wählen Sie unter **Prozessautomatisierung** die Option **Watchertasks** aus.
1. Wählen Sie die Seite „Watchertasks“ aus, und klicken Sie auf **+Watchertask hinzufügen**.
1. Geben Sie **WatchMyFolder** als Namen ein.

1. Wählen Sie **Watcher konfigurieren** und dann das Runbook **Watch-NewFile** aus.

1. Geben Sie die folgende Werte für die Parameter ein:

   * **FOLDERPATH**: Ein Ordner in dem Hybrid Runbook Worker, in dem neue Dateien erstellt werden. Beispiel: **d:\examplefiles**.
   * **EXTENSION**: Erweiterung für die Konfiguration. Lassen Sie dieses Feld leer, um alle Dateierweiterungen zu verarbeiten.
   * **RECURSE**: Rekursiver Vorgang. Behalten Sie hier den Standardwert bei.
   * **RUN SETTINGS**: Einstellung für die Ausführung des Runbooks. Wählen Sie den Hybrid Worker aus.

1. Klicken Sie auf **OK** und dann auf **Auswählen**, um zur Seite „Watcher“ zurückzukehren.
1. Wählen Sie **Aktion konfigurieren** und dann das Runbook **Process-NewFile** aus.
1. Geben Sie die folgende Werte für die Parameter ein:

   * **EVENTDATA**: Ereignisdaten. Lassen Sie dieses Feld leer. Die Daten werden vom Watcherrunbook übergeben.
   * **Laufzeiteinstellungen** Einstellung für die Ausführung des Runbooks. Übernehmen Sie hier „Azure“, da dieses Runbook in Azure Automation ausgeführt wird.

1. Klicken Sie auf **OK** und dann auf **Auswählen**, um zur Seite „Watcher“ zurückzukehren.
1. Klicken Sie auf **OK**, um den Watchertask zu erstellen.

![Konfigurieren der Watcheraktion über die Benutzeroberfläche](media/automation-watchers-tutorial/watchertaskcreation.png)

## <a name="trigger-a-watcher"></a>Auslösen eines Watchers

Sie müssen einen Test wie unten beschrieben ausführen, um sicherzustellen, dass der Watchertask erwartungsgemäß funktioniert. 

1. Stellen Sie eine Remoteverbindung mit dem Hybrid Runbook Worker her. 
2. Öffnen Sie **PowerShell**, und erstellen Sie im Ordner eine Testdatei.

```azurepowerShell-interactive
New-Item -Name ExampleFile1.txt
```

Das folgende Beispiel zeigt die erwartete Ausgabe.

```output
    Directory: D:\examplefiles


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       12/11/2017   9:05 PM              0 ExampleFile1.txt
```

## <a name="inspect-the-output"></a>Untersuchen der Ausgabe

1. Navigieren Sie zu Ihrem Automation-Konto, und wählen Sie unter **Prozessautomatisierung** die Option **Watchertasks** aus.
1. Wählen Sie den Watchertask **WatchMyFolder** aus.
1. Klicken Sie unter **Datenströme** auf **Watcherdatenströme anzeigen**, um zu überprüfen, ob der Watcher die neue Datei gefunden und das Aktionsrunbook gestartet hat.
1. Klicken Sie auf **Watcheraktionsaufträge anzeigen**, um den Aktionsrunbookauftrag anzuzeigen. Sie können jeden einzelnen Auftrag auswählen, um die Details anzuzeigen.

   ![Watcheraktionsaufträge über die Benutzeroberfläche](media/automation-watchers-tutorial/WatcherActionJobs.png)

Im folgenden Beispiel sehen Sie die erwartete Ausgabe, wenn die neue Datei gefunden wurde:

```output
Message is Process new file...

Passed in data is @{FileName=D:\examplefiles\ExampleFile1.txt; Length=0}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Importieren eines Watcherrunbooks
> * Erstellen einer Automation-Variable
> * Erstellen eines Aktionsrunbooks
> * Erstellen eines Watchertasks
> * Auslösen eines Watchers
> * Untersuchen der Ausgabe

Klicken Sie auf folgenden Link, um weitere Informationen zum Erstellen von Runbooks zu erhalten:

> [!div class="nextstepaction"]
> [Erstellen eines PowerShell-Runbooks](learn/automation-tutorial-runbook-textual-powershell.md)