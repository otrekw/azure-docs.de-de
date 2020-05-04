---
title: Verwalten von Python 2-Paketen in Azure Automation
description: Dieser Artikel beschreibt, wie Sie Python 2-Pakete in Azure Automation verwalten.
services: automation
ms.subservice: process-automation
ms.date: 02/25/2019
ms.topic: conceptual
ms.openlocfilehash: 701a5aab7a0061f8b5abfaac1b699034db2671b9
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82508988"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Verwalten von Python 2-Paketen in Azure Automation

Mit Azure Automation können Sie Python 2-Runbooks in Azure und Hybrid Runbook Worker unter Linux ausführen. Um die Runbooks zu vereinfachen, können Sie mithilfe von Python-Paketen die erforderlichen Module importieren. Dieser Artikel beschreibt, wie Sie Python-Pakete in Azure Automation verwalten und verwenden.

## <a name="import-packages"></a>Importieren von Paketen

Wählen Sie in Ihrem Automation-Konto unter **Python 2-Pakete** die Option **Freigegebene Ressourcen** aus. Klicken Sie auf **Python 2-Paket hinzufügen**.

![Hinzufügen eines Python-Pakets](media/python-packages/add-python-package.png)

Wählen Sie auf der Seite „Python 2-Paket hinzufügen“ ein lokales Paket für den Upload aus. Das Paket kann eine Datei im Format **.whl** oder **.tar.gz** sein. Klicken Sie nach der Auswahl des Pakets auf **OK**, um es hochzuladen.

![Hinzufügen eines Python-Pakets](media/python-packages/upload-package.png)

Sobald ein Paket importiert wurde, wird es in Ihrem Automation-Konto auf der Seite „Python 2-Pakete“ aufgelistet. Um ein Paket zu entfernen, wählen Sie das Paket aus, und klicken Sie dann auf **Löschen**.

![Paketliste](media/python-packages/package-list.png)

## <a name="import-packages-with-dependencies"></a>Importieren von Paketen mit Abhängigkeiten

Azure Automation löst Abhängigkeiten für Python-Pakete während des Importvorgangs nicht auf. Es gibt zwei Möglichkeiten, um ein Paket mit allen seinen Abhängigkeiten zu importieren. Nur einer der folgenden Schritte muss verwendet werden, um die Pakete in Ihr Automation-Konto zu importieren.

### <a name="manually-download"></a>Manuelles Herunterladen

Führen Sie auf einem Windows-Computer (64-Bit), auf dem [Python2.7](https://www.python.org/downloads/release/latest/python2) und [pip](https://pip.pypa.io/en/stable/) installiert sind, den folgenden Befehl aus, um ein Paket mit allen seinen Abhängigkeiten herunterzuladen:

```cmd
C:\Python27\Scripts\pip2.7.exe download -d <output dir> <package name>
```

Sobald die Pakete heruntergeladen sind, können Sie sie in Ihr Automation-Konto importieren.

### <a name="runbook"></a>Runbook

 Um ein Runbook zu erhalten, importieren Sie aus dem Katalog [Python 2-Pakete aus pypi in Ihr Azure Automation-Konto](https://gallery.technet.microsoft.com/scriptcenter/Import-Python-2-packages-57f7d509). Stellen Sie sicher, dass die Ausführungseinstellungen auf **Azure** festgelegt sind, und starten Sie das Runbook mit den Parametern. Das Runbook erfordert ein „Ausführendes Konto“, damit das Automation-Konto funktioniert. Für jeden Parameter stellen Sie sicher, dass Sie ihn mit dem Parameter starten, wie er in der folgende Liste und Abbildung zu sehen ist:

* -s \<subscriptionId\>
* -g \<resourceGroup\>
* -a \<automationAccount\>
* -m \<modulePackage\>

![Paketliste](media/python-packages/import-python-runbook.png)

Mithilfe des Runbooks können Sie angeben, welches Paket heruntergeladen werden soll. Verwenden Sie beispielsweise den `Azure`-Parameter, um alle Azure-Module und alle Abhängigkeiten (etwa 105) herunterzuladen.

Nach Abschluss des Runbooks können Sie die Seite **Python 2-Pakete** unter **Freigegebene Ressourcen** in Ihrem Automation-Konto überprüfen, um zu überprüfen, ob das Paket ordnungsgemäß importiert wurde.

## <a name="use-a-package-in-a-runbook"></a>Verwenden eines Pakets in einem Runbook

Nachdem Sie ein Paket importiert haben, können Sie es in einem Runbook verwenden. Das folgende Beispiel verwendet das [Azure Automation-Hilfsprogrammpaket](https://github.com/azureautomation/azure_automation_utility). Dieses Paket erleichtert die Verwendung von Python mit Azure Automation. Um das Paket zu verwenden, folgen Sie den Anweisungen im GitHub-Repository, und fügen Sie es dem Runbook hinzu. Beispielsweise können Sie `from azure_automation_utility import get_automation_runas_credential` verwenden, um die Funktion zum Abrufen des ausführenden Kontos zu importieren.

```python
import azure.mgmt.resource
import automationassets
from azure_automation_utility import get_automation_runas_credential

# Authenticate to Azure using the Azure Automation RunAs service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential()

# Intialize the resource management client with the RunAs credential and subscription
resource_client = azure.mgmt.resource.ResourceManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"]))

# Get list of resource groups and print them out
groups = resource_client.resource_groups.list()
for group in groups:
    print group.name
```

## <a name="develop-and-test-runbooks-offline"></a>Entwickeln und Testen von Runbooks im Offlinemodus

Um Ihre Python 2-Runbooks offline zu entwickeln und zu testen, können Sie die für das [Python-Modul emulierten Azure Automation-Assets](https://github.com/azureautomation/python_emulated_assets) auf GitHub verwenden. Mit diesem Modul können Sie auf Ihre freigegebenen Ressourcen wie Anmeldeinformationen, Variablen, Verbindungen und Zertifikate verweisen.

## <a name="next-steps"></a>Nächste Schritte

Eine Einführung in Python 2-Runbooks finden Sie unter [Mein erstes Python 2-Runbook](automation-first-runbook-textual-python2.md).
