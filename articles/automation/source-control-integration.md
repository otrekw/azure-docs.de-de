---
title: Integration der Quellcodeverwaltung in Azure Automation
description: In diesem Artikel wird die Integration der Quellcodeverwaltung mit GitHub in Azure Automation erläutert.
services: automation
ms.subservice: process-automation
ms.date: 12/10/2019
ms.topic: conceptual
ms.openlocfilehash: eef67ca8111983adb4d9994894ba215240daee6f
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78253731"
---
# <a name="source-control-integration-in-azure-automation"></a>Integration der Quellcodeverwaltung in Azure Automation

 Bei der Integration der Quellcodeverwaltung in Azure Automation wird die unidirektionale Synchronisierung aus Ihrem Repository der Quellcodeverwaltung unterstützt. Mit der Quellcodeverwaltung halten Sie die Runbooks in Ihrem Automation-Konto auf demselben aktuellen Stand wie Skripts in der Quellcodeverwaltung Ihres GitHub- oder Azure Repos-Repositorys. Mit dieser Funktion ist es einfach, Code, der in Ihrer Entwicklungsumgebung getestet wurde, in Ihr Automation-Produktionskonto höher zu stufen.
 
 Mithilfe der Integration der Quellcodeverwaltung können Sie einfach mit Ihrem Team zusammenarbeiten, Änderungen nachverfolgen und Ihre Runbooks auf frühere Versionen zurücksetzen. Mit der Quellcodeverwaltung können Sie beispielsweise verschiedene Verzweigungen in der Quellcodeverwaltung mit Ihren Automation-Entwicklungs-, Produktions- und Testkonten synchronisieren. 

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="source-control-types"></a>Typen der Quellcodeverwaltung

Azure Automation unterstützt drei Arten von Quellcodeverwaltung:

* GitHub
* Azure Repos (Git)
* Azure Repos (TFVC)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Quellcodeverwaltungsrepository (GitHub oder Azure Repos)
* Ein [ausführendes Konto](manage-runas-account.md).
* Die [neuesten Azure-Module](automation-update-azure-modules.md) in Ihrem Automation-Konto, einschließlich des **Az.Accounts**-Moduls (Az-Modul-Entsprechung von „AzureRM.Profile“).

> [!NOTE]
> Synchronisierungsaufträge für die Quellcodeverwaltung werden unter dem Automation-Konto eines Benutzers ausgeführt und mit der gleichen Rate wie andere Automation-Aufträge berechnet.

## <a name="configuring-source-control"></a>Konfigurieren der Quellcodeverwaltung

In diesem Abschnitt erfahren Sie, wie Sie die Quellcodeverwaltung für Ihr Automation-Konto konfigurieren. Sie können das Azure-Portal oder PowerShell verwenden.

### <a name="configure-source-control----azure-portal"></a>Konfigurieren der Quellcodeverwaltung – Azure-Portal

Verwenden Sie dieses Verfahren, um die Quellcodeverwaltung über das Azure-Portal zu konfigurieren.

1. Wählen Sie in Ihrem Automation-Konto **Quellcodeverwaltung** aus, und klicken Sie auf **+ Hinzufügen**.

    ![Auswählen der Quellcodeverwaltung](./media/source-control-integration/select-source-control.png)

2. Wählen Sie **Typ der Quellcodeverwaltung** aus, und klicken Sie dann auf **Authentifizieren**. 

3. Ein Browserfenster wird geöffnet, und Sie werden zur Anmeldung aufgefordert. Befolgen Sie die Aufforderungen, um die Authentifizierung abzuschließen.

4. Verwenden Sie die Felder auf der Seite **Zusammenfassung der Quellcodeverwaltung**, um die unten definierten Eigenschaften der Quellcodeverwaltung einzutragen. Klicken Sie abschließend auf **Speichern**. 

    |Eigenschaft  |BESCHREIBUNG  |
    |---------|---------|
    |Name der Quellcodeverwaltung     | Ein Anzeigename für die Quellcodeverwaltung. Dieser Name darf nur Buchstaben und Zahlen enthalten.        |
    |Typ der Quellcodeverwaltung     | Der Typ des Quellcodeverwaltungs-Mechanismus. Die verfügbaren Optionen lauten wie folgt:</br> GitHub</br>Azure Repos (Git)</br> Azure Repos (TFVC)        |
    |Repository     | Der Name des Repositorys oder Projekts. Die ersten 200 Repositorys werden abgerufen. Geben Sie zum Suchen nach einem Repository den Namen in das Feld ein, und klicken Sie auf **Search on GitHub** (Auf GitHub suchen).|
    |Verzweigung     | Die Verzweigung, aus der die Quelldateien abgerufen werden sollen. Die Ausrichtung auf Verzweigungen ist für den TFVC-Quellcodeverwaltungstyp nicht verfügbar.          |
    |Ordnerpfad     | Der Ordner, der die zu synchronisierenden Runbooks enthält, z. B. „/Runbooks“. Nur im angegebenen Ordner enthaltene Runbooks werden synchronisiert. Rekursion wird nicht unterstützt.        |
    |Automatische Synchronisierung<sup>1</sup>     | Eine Einstellung, die die automatische Synchronisierung aktiviert oder deaktiviert, wenn im Repository für die Quellcodeverwaltung ein Commit durchgeführt wird.        |
    |Runbook veröffentlichen     | Die Einstellung ist „Ein“ (On), wenn Runbooks nach der Synchronisierung aus der Quellcodeverwaltung automatisch veröffentlicht werden, andernfalls ist sie „Aus“ (Off).           |
    |BESCHREIBUNG     | Text, der zusätzliche Details zur Quellcodeverwaltung angibt.        |

    <sup>1</sup> Um die automatische Synchronisierung beim Konfigurieren der Integration der Quellcodeverwaltung in Azure Repos zu aktivieren, müssen Sie Projektadministrator sein.

   ![Zusammenfassung der Quellcodeverwaltung](./media/source-control-integration/source-control-summary.png)

> [!NOTE]
> Ihre Anmeldung für Ihr Quellcodeverwaltungs-Repository kann von Ihrer Anmeldung beim Azure-Portal abweichen. Stellen Sie sicher, dass Sie mit dem richtigen Konto für Ihr Quellcodeverwaltungs-Repository angemeldet sind, wenn Sie die Quellcodeverwaltung konfigurieren. Wenn Sie diesbezüglich unsicher sind, öffnen Sie in Ihrem Browser eine neue Registerkarte, melden Sie sich von „visualstudio.com“ oder „github.com“ ab, und stellen Sie erneut eine Verbindung mit der Quellcodeverwaltung her.

### <a name="configure-source-control----powershell"></a>Konfigurieren der Quellcodeverwaltung – PowerShell

Sie können auch PowerShell verwenden, um die Quellcodeverwaltung in Azure Automation zu konfigurieren. Um PowerShell-Cmdlets für diesen Vorgang zu verwenden, benötigen Sie ein persönliches Zugriffstoken (Personal Access Token, PAT). Verwenden Sie das Cmdlet [New-AzAutomationSourceControl](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationsourcecontrol?view=azps-3.5.0
), um die Verbindung für die Quellcodeverwaltung zu erstellen. Dieses Cmdlet erfordert eine sichere Zeichenfolge für das PAT. Informationen zum Erstellen einer sicheren Zeichenfolge finden Sie unter [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-6).

In den folgenden Unterabschnitten wird die Erstellung der Quellcodeverwaltung mit der PowerShell für GitHub, Azure Repos (Git) und Azure Repos (TFVC) veranschaulicht.

#### <a name="create-source-control-connection-for-github"></a>Erstellen einer Verbindung mit der Quellcodeverwaltung für GitHub

```powershell-interactive
New-AzAutomationSourceControl -Name SCGitHub -RepoUrl https://github.com/<accountname>/<reponame>.git -SourceType GitHub -FolderPath "/MyRunbooks" -Branch master -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

#### <a name="create-source-control-connection-for-azure-repos-git"></a>Erstellen einer Verbindung mit der Quellcodeverwaltung für Azure Repos (Git)

```powershell-interactive
New-AzAutomationSourceControl -Name SCReposGit -RepoUrl https://<accountname>.visualstudio.com/<projectname>/_git/<repositoryname> -SourceType VsoGit -AccessToken <secureStringofPAT> -Branch master -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

#### <a name="create-source-control-connection-for-azure-repos-tfvc"></a>Erstellen einer Verbindung mit der Quellcodeverwaltung für Azure Repos (TFVC)

```powershell-interactive
New-AzAutomationSourceControl -Name SCReposTFVC -RepoUrl https://<accountname>.visualstudio.com/<projectname>/_versionControl -SourceType VsoTfvc -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

#### <a name="personal-access-token-pat-permissions"></a>Berechtigungen für persönliche Zugriffstoken (PAT)

Die Quellcodeverwaltung erfordert einige Mindestberechtigungen für PATs. Die folgenden Unterabschnitte enthalten die für GitHub und Azure Repos erforderlichen Mindestberechtigungen.

##### <a name="minimum-pat-permissions-for-github"></a>PAT-Mindestberechtigungen für GitHub

In der folgenden Tabelle werden die für GitHub erforderlichen PAT-Mindestberechtigungen definiert. Weitere Informationen zum Erstellen eines PAT in GitHub finden Sie unter [Erstellen eines persönlichen Zugriffstokens für die Befehlszeile](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).

|`Scope`  |BESCHREIBUNG  |
|---------|---------|
|**Repository**     |         |
|repo:status     | Zugriff auf den Commitstatus         |
|repo_deployment      | Zugriff auf den Bereitstellungsstatus         |
|public_repo     | Zugriff auf öffentliche Repositorys         |
|**admin:repo_hook**     |         |
|write:repo_hook     | Schreiben von Repositoryhooks         |
|read:repo_hook|Lesen von Repositoryhooks|

##### <a name="minimum-pat-permissions-for-azure-repos"></a>PAT-Mindestberechtigungen für Azure Repos

In der folgenden Liste werden die für Azure Repos erforderlichen PAT-Mindestberechtigungen definiert. Weitere Informationen zum Erstellen eines PAT in Azure Repos finden Sie unter [Authentifizieren des Zugriffs mit persönlichen Zugriffstoken](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

| `Scope`  |  Zugriffstyp  |
|---------| ----------|
| Code      | Lesen  |
| Projekt und Team | Lesen |
| Identity | Lesen     |
| Benutzerprofil | Lesen     |
| Arbeitselemente | Lesen    |
| Dienstverbindungen | Lesen, Abfragen und Verwalten<sup>1</sup>    |

<sup>1</sup> Die Berechtigung für Dienstverbindungen ist nur erforderlich, wenn die automatische Synchronisierung aktiviert ist.

## <a name="synchronizing"></a>Wird synchronisiert

Führen Sie Folgendes aus, um eine Synchronisierung mit der Quellcodeverwaltung durchzuführen. 

1. Wählen Sie die Quelle in der Tabelle auf der Seite **Quellcodeverwaltung** aus. 

2. Klicken Sie auf **Synchronisierung starten**, um den Synchronisierungsprozess zu starten. 

3. Zeigen Sie den Status des aktuellen Synchronisierungsauftrags oder von früheren Aufträgen an, indem Sie auf die Registerkarte **Synchronisierungsaufträge** klicken. 

4. Wählen Sie im Dropdownmenü **Quellcodeverwaltung** einen Quellcodeverwaltungs-Mechanismus aus.

    ![Synchronisierungsstatus](./media/source-control-integration/sync-status.png)

5. Sie können die Ausgabe des Auftrags anzeigen, indem Sie auf einen Auftrag klicken. Dies ist ein Beispiel für die Ausgabe eines Synchronisierungsauftrags der Quellcodeverwaltung:

    ```output
    ============================================================================

    Azure Automation Source Control.
    Supported runbooks to sync: PowerShell Workflow, PowerShell Scripts, DSC Configurations, Graphical, and Python 2.

    Setting AzureRmEnvironment.

    Getting AzureRunAsConnection.

    Logging in to Azure...

    Source control information for syncing:

    [Url = https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl] [FolderPath = /Runbooks]

    Verifying url: https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl

    Connecting to VSTS...

    Source Control Sync Summary:

    2 files synced:
     - ExampleRunbook1.ps1
     - ExampleRunbook2.ps1

     =========================================================================

    ```

6. Eine zusätzliche Protokollierung ist verfügbar, indem Sie auf der Seite **Quellcodeverwaltung – Übersicht zu Synchronisierungsauftrag** die Option **Alle Protokolle** wählen. Diese zusätzlichen Protokolleinträge können Ihnen als Hilfe beim Behandeln von Problemen dienen, die bei Verwendung der Quellcodeverwaltung auftreten können.

## <a name="disconnecting-source-control"></a>Trennen der Verbindung zur Quellcodeverwaltung

So trennen Sie die Verbindung mit einem Quellcodeverwaltungs-Repository

1. Öffnen Sie in Ihrem Automation-Konto unter **Kontoeinstellungen** die Option **Quellcodeverwaltung**.

2. Wählen Sie den Quellcodeverwaltungs-Mechanismus aus, den Sie entfernen möchten. 

3. Klicken Sie auf der Seite **Quellcodeverwaltung – Übersicht** auf **Löschen**.

## <a name="handling-encoding-issues"></a>Behandeln von Codierungsproblemen

Wenn mehrere Personen in Ihrem Quellcodeverwaltungs-Repository Runbooks mit unterschiedlichen Editoren bearbeiten, kann es zu Codierungsproblemen kommen. Weitere Informationen zu dieser Situation finden Sie unter [Häufige Gründe für Codierungsprobleme](/powershell/scripting/components/vscode/understanding-file-encoding#common-causes-of-encoding-issues).

## <a name="updating-the-pat"></a>Aktualisieren des PAT

Derzeit gibt es keine Möglichkeit, das PAT in der Quellcodeverwaltung über das Azure-Portal zu aktualisieren. Nach Ablauf oder Widerruf Ihres PAT können Sie die Quellcodeverwaltung auf eine der folgenden Weisen mit einem neuen Zugriffstoken aktualisieren:

* Verwenden Sie die [REST-API](https://docs.microsoft.com/rest/api/automation/sourcecontrol/update).
* Verwenden Sie das Cmdlet [Update-AzAutomationSourceControl](/powershell/module/az.automation/update-azautomationsourcecontrol).

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den verschiedenen Runbooktypen mit ihren Vorteilen und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md).
