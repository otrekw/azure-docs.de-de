---
title: Verwenden von Azure Automation-Runbooks und -Modulen im PowerShell-Katalog
description: In diesem Artikel wird erläutert, wie Sie Runbooks und Module von Microsoft und der Community im PowerShell-Katalog verwenden.
services: automation
ms.subservice: process-automation
ms.date: 03/20/2019
ms.topic: conceptual
ms.openlocfilehash: f2bf058ddce81ab9f04e97787a4dc93e44036b1b
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86186111"
---
# <a name="use-runbooks-and-modules-in-powershell-gallery"></a>Verwenden von Runbooks und Modulen im PowerShell-Katalog

Statt eigene Runbooks und Module in Azure Automation zu erstellen, können Sie Szenarien nutzen, die bereits von Microsoft und der Community entwickelt wurden. Sie können PowerShell-Runbooks und -[Module](#modules-in-powershell-gallery) aus dem PowerShell-Katalog sowie [Python-Runbooks](#use-python-runbooks) aus dem Script Center-Katalog abrufen. Sie können auch etwas zur Community beitragen, indem Sie [von Ihnen entwickelte Szenarien](#add-a-powershell-runbook-to-the-gallery) zur Verfügung stellen. 

## <a name="runbooks-in-powershell-gallery"></a>Runbooks im PowerShell-Katalog

Der [PowerShell-Katalog](https://www.powershellgallery.com/packages) bietet zahlreiche Runbooks von Microsoft und der Community, die Sie in Azure Automation importieren können. Um eins zu verwenden, können Sie ein Runbook aus dem Katalog herunterladen, oder Sie können Runbooks direkt aus dem Katalog oder aus Ihrem Automation-Konto im Azure-Portal importieren.

> [!NOTE]
> Grafische Runbooks werden im PowerShell-Katalog nicht unterstützt.

Das direkte Importieren aus dem PowerShell-Katalog ist nur über das Azure-Portal möglich. Diese Funktion kann nicht mithilfe von PowerShell ausgeführt werden.

> [!NOTE]
> Überprüfen Sie unbedingt den Inhalt der aus dem PowerShell-Katalog heruntergeladenen Runbooks, und seien Sie äußerst vorsichtig, wenn Sie sie in einer Produktionsumgebung installieren und ausführen.

## <a name="modules-in-powershell-gallery"></a>Module im PowerShell-Katalog

PowerShell-Module enthalten Cmdlets, die Sie in Ihren Runbooks verwenden können. Vorhandene Module, die Sie in Azure Automation installieren können, sind im [PowerShell-Katalog](https://www.powershellgallery.com) verfügbar. Sie starten diesen Katalog über das Azure-Portal und installieren die Module direkt in Azure Automation. Sie können sie auch herunterladen und manuell installieren.

## <a name="common-scenarios-available-in-powershell-gallery"></a>Allgemeine im PowerShell-Katalog verfügbare Szenarien

Die nachstehende Liste enthält einige Runbooks, die gängige Szenarien unterstützen. Eine vollständige Liste der vom Azure Automation-Team erstellten Runbooks finden Sie im [AzureAutomationTeam-Profil](https://www.powershellgallery.com/profiles/AzureAutomationTeam).

   * [Update-ModulesInAutomationToLatestVersion:](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/) importiert die neueste Version aller Module aus dem PowerShell-Katalog in ein Automation-Konto.
   * [Enable-AzureDiagnostics:](https://www.powershellgallery.com/packages/Enable-AzureDiagnostics/) konfiguriert Azure-Diagnose und Log Analytics, sodass Azure Automation-Protokolle mit Auftragsstatus- und Auftragsdatenströmen empfangen werden.
   * [Copy-ItemFromAzureVM:](https://www.powershellgallery.com/packages/Copy-ItemFromAzureVM/) kopiert eine Remotedatei von einem virtuellen Microsoft Azure-Computer.
   * [Copy-ItemFromAzureVM:](https://www.powershellgallery.com/packages/Copy-ItemToAzureVM/) kopiert eine lokale Datei auf einen virtuellen Azure-Computer.

## <a name="import-a-powershell-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Importieren eines PowerShell-Runbooks aus dem Runbookkatalog im Azure-Portal

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
2. Wählen Sie unter **Prozessautomatisierung** die Option **Runbookkatalog** aus.
3. Wählen Sie **Quelle: PowerShell-Katalog** aus.
4. Suchen Sie das gewünschte Katalogelement, und wählen Sie es zum Anzeigen der Details aus. Auf den linken Seite können Sie weitere Suchparameter für Herausgeber und Typ eingeben.

   ![Katalog durchsuchen](media/automation-runbook-gallery/browse-gallery.png)

5. Klicken Sie auf **Quellprojekt anzeigen** , um den Artikel im [TechNet Script Center](https://gallery.technet.microsoft.com/)anzuzeigen.
6. Klicken Sie zum Importieren eines Elements auf das Element, um seine Details anzuzeigen, und anschließend auf **Importieren**.

   ![Schaltfläche „Importieren“](media/automation-runbook-gallery/gallery-item-detail.png)

7. Ändern Sie optional den Namen des Runbooks, und klicken Sie zum Importieren des Runbooks auf **OK** .
8. Das Runbook wird auf der Registerkarte **Runbooks** des Automation-Kontos angezeigt.

## <a name="add-a-powershell-runbook-to-the-gallery"></a>Hinzufügen eines PowerShell-Runbooks zum Katalog

Microsoft empfiehlt, Runbooks aus dem PowerShell-Katalog hinzuzufügen, die für andere Kunden nützlich sein könnten. Der PowerShell-Katalog akzeptiert PowerShell-Module und PowerShell-Skripts. Sie können ein Runbook hinzufügen, indem Sie [es in den PowerShell-Katalog hochladen](/powershell/scripting/gallery/how-to/publishing-packages/publishing-a-package).

## <a name="import-a-module-from-the-module-gallery-with-the-azure-portal"></a>Importieren eines Moduls aus dem Modulkatalog im Azure-Portal

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
2. Wählen Sie **Module** unter **Freigegebene Ressourcen** aus, um die Liste der Module zu öffnen.
3. Klicken Sie oben auf der Seite auf **Katalog durchsuchen**.

   ![Modulkatalog](media/automation-runbook-gallery/modules-blade.png)

4. Auf der Seite „Katalog durchsuchen“ können Sie anhand der folgenden Felder suchen:

   * Name des Moduls
   * `Tags`
   * Autor
   * Cmdlet-/DSC-Ressourcenname

5. Suchen Sie das gewünschte Modul, und wählen Sie es aus, um seine Details anzuzeigen.

   Wenn Sie einen Drilldown in einem bestimmten Modul ausführen, können Sie weitere Informationen anzeigen. Diese Informationen umfassen einen Link zurück zum PowerShell-Katalog, alle erforderlichen Abhängigkeiten und alle Cmdlets oder DSC-Ressourcen, die das Modul enthält.

   ![PowerShell-Moduldetails](media/automation-runbook-gallery/gallery-item-details-blade.png)

6. Um das Modul direkt in Azure Automation zu installieren, klicken Sie auf **Importieren**.
7. Im Bereich „Importieren“ sehen Sie den Namen des zu importierenden Moduls. Wenn alle Abhängigkeiten installiert sind, ist die Schaltfläche **OK** aktiv. Falls Abhängigkeiten fehlen, müssen diese Abhängigkeiten importiert werden, bevor dieses Modul importiert werden kann.
8. Klicken Sie im Bereich „Importieren“ auf **OK**, um das Modul zu importieren. Wenn Azure Automation ein Modul in Ihr Konto importiert, werden Metadaten zum Modul und den Cmdlets extrahiert. Dieser Vorgang kann einige Minuten dauern, da jede Aktivität extrahiert werden muss.
9. Sie erhalten jeweils eine Benachrichtigung, wenn das Modul bereitgestellt wird und wenn der Vorgang abgeschlossen ist.
10. Nachdem das Modul importiert wurde, können Sie die verfügbaren Aktivitäten anzeigen. Sie können Modulressourcen in Ihren Runbooks und DSC-Ressourcen verwenden.

> [!NOTE]
> Module, die nur PowerShell Core unterstützen, werden in Azure Automation nicht unterstützt und können nicht in das Azure-Portal importiert werden oder direkt über den PowerShell-Katalog bereitgestellt werden.

## <a name="use-python-runbooks"></a>Verwenden von Python-Runbooks

Python-Runbooks finden Sie im [Script Center-Katalog](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B1%5D.Type=ProgrammingLanguage&f%5B1%5D.Value=Python&f%5B1%5D.Text=Python&sortBy=Date&username=). Sie haben die Möglichkeit, Python-Runbooks dem Script Center-Katalog hinzuzufügen, indem Sie auf **Upload a contribution** (Beitrag hochladen) klicken. Wenn Sie das tun, achten Sie darauf, dass Sie beim Hochladen Ihres Beitrags das Tag `Python` hinzufügen.

> [!NOTE]
> Um Inhalte in das [Script Center](https://gallery.technet.microsoft.com/scriptcenter) hochladen zu können, benötigen Sie mindestens 100 Punkte.

## <a name="request-a-runbook-or-module"></a>Anfordern eines Runbooks oder Moduls

Sie können Anforderungen an [User Voice](https://feedback.azure.com/forums/246290-azure-automation/)senden.  Wenn Sie Hilfe beim Schreiben eines Runbooks benötigen oder eine Frage zu PowerShell haben, stellen Sie eine Frage auf der [Frageseite von Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-automation.html).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu den ersten Schritten mit einem PowerShell-Runbook finden Sie unter [Tutorial: Erstellen eines PowerShell-Runbooks](learn/automation-tutorial-runbook-textual-powershell.md).
* Informationen zur Arbeit mit Runbooks finden Sie unter [Verwalten eines Runbooks in Azure Automation](manage-runbooks.md).
* Details zu PowerShell finden Sie in der [PowerShell-Dokumentation](/powershell/scripting/overview).
* * Eine Referenz zu den PowerShell-Cmdlets finden Sie unter [Az.Automation](/powershell/module/az.automation/?view=azps-3.7.0#automation).
