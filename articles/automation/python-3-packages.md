---
title: Verwalten von Python 3-Paketen in Azure Automation
description: In diesem Artikel wird erläutert, wie Sie Python 3-Pakete (Vorschau) in Azure Automation verwalten.
services: automation
ms.subservice: process-automation
ms.date: 02/19/2021
ms.topic: conceptual
ms.openlocfilehash: fd4d8ee92b670bc2544619a0dce16a26d9342c13
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102122033"
---
# <a name="manage-python-3-packages-preview-in-azure-automation"></a>Verwalten von Python 3-Paketen (Vorschau) in Azure Automation

Mit Azure Automation können Sie Python 3-Runbooks (Vorschau) in Azure und Hybrid Runbook Worker unter Linux ausführen. Um die Runbooks zu vereinfachen, können Sie mithilfe von Python-Paketen die erforderlichen Module importieren. Informationen zum Importieren eines einzelnen Pakets finden Sie unter [Importieren eines Pakets](#import-a-package). Informationen zum Importieren eines Pakets mit mehreren Paketen finden Sie unter [Importieren eines Pakets mit Abhängigkeiten](#import-a-package-with-dependencies). In diesem Artikel wird beschrieben, wie Sie Python 3-Pakete (Vorschau) in Azure Automation verwalten und verwenden.

## <a name="import-a-package"></a>Importieren eines Pakets

Wählen Sie in Ihrem Azure Automation-Konto unter **Python-Pakete** die Option **Freigegebene Ressourcen** aus. Wählen Sie **+ Python-Paket hinzufügen** aus.

:::image type="content" source="media/python-3-packages/add-python-3-package.png" alt-text="Screenshot der Seite „Python 3-Pakete“ mit Python 3-Paketen im linken Menü und Hervorhebung von „Python-Paket hinzufügen“.":::

Wählen Sie auf der Seite **Python-Paket hinzufügen** unter **Version** die Option **Python 3** sowie ein hochzuladendes lokales Paket aus. Das Paket kann eine Datei im Format **.whl** oder **.tar.gz** sein. Wählen Sie nach Auswahl des Pakets **OK** aus, um es hochzuladen.

:::image type="content" source="media/python-3-packages/upload-package.png" alt-text="Screenshot der Seite „Python 3-Paket hinzufügen“ mit Auswahl der hochgeladenen Datei „tar.gz“.":::

Sobald ein Paket importiert wurde, wird es in Ihrem Azure Automation-Konto auf der Seite „Python-Pakete“ auf der Registerkarte **Python 3-Pakete (Vorschau)** aufgelistet. Um ein Paket zu entfernen, wählen Sie das Paket aus, und klicken Sie dann auf **Löschen**.

:::image type="content" source="media/python-3-packages/python-3-packages-list.png" alt-text="Screenshot der Seite „Python 3-Pakete“ nach Import eines Pakets.":::

### <a name="import-a-package-with-dependencies"></a>Importieren eines Pakets mit Abhängigkeiten

Sie können ein Python 3-Paket und die zugehörigen Abhängigkeiten importieren, indem Sie das folgende Python-Skript in ein Python 3-Runbook importieren und es anschließend ausführen.

```cmd
https://github.com/azureautomation/runbooks/blob/master/Utility/Python/import_py3package_from_pypi.py
```

#### <a name="importing-the-script-into-a-runbook"></a>Importieren des Skripts in ein Runbook
Informationen zum Importieren des Runbooks finden Sie unter [Importieren eines Runbooks im Azure-Portal](manage-runbooks.md#import-a-runbook-from-the-azure-portal). Kopieren Sie die Datei aus GitHub an einen Speicherort, auf den das Portal zugreifen kann, bevor Sie den Import ausführen.

Auf der Seite zum **Importieren eines Runbooks** wird als Name des Runbooks standardmäßig der Name des Skripts verwendet. Wenn Sie Zugriff auf das Feld haben, können Sie den Namen ändern. Der **Runbooktyp** ist möglicherweise standardmäßig auf **Python 2** festgelegt. Falls ja, ändern Sie ihn in **Python 3**.

:::image type="content" source="media/python-3-packages/import-python-3-package.png" alt-text="Screenshot: Seite zum Importieren eines Python 3-Runbooks":::

#### <a name="executing-the-runbook-to-import-the-package-and-dependencies"></a>Ausführen des Runbooks zum Importieren des Pakets und der Abhängigkeiten

Nachdem Sie das Runbook erstellt und veröffentlicht haben, können Sie es ausführen, um das Paket zu importieren. Ausführliche Informationen zum Ausführen des Runbooks finden Sie unter [Starten eines Runbooks in Azure Automation](start-runbooks.md).

Für das Skript `import_py3package_from_pypi.py` sind folgende Parameter erforderlich:

| Parameter | BESCHREIBUNG |
|---------------|-----------------|
|subscription_id | Abonnement-ID des Automation-Kontos |
| resource_group | Name der Ressourcengruppe, in der das Automation-Konto definiert ist |
| automation_account | Name des Automation-Kontos |
| module_name | Name des Moduls für den Import aus `pypi.org` |

Weitere Informationen zur Verwendung von Parametern mit Runbooks finden Sie unter [Verwenden von Runbookparametern](start-runbooks.md#work-with-runbook-parameters).

## <a name="use-a-package-in-a-runbook"></a>Verwenden eines Pakets in einem Runbook

Nachdem Sie das Paket importiert haben, können Sie es in einem Runbook verwenden. Fügen Sie den folgenden Code hinzu, um alle Ressourcengruppen in einem Azure-Abonnement aufzulisten.

```python
import os  
import azure.mgmt.resource  
import automationassets  

def get_automation_runas_credential(runas_connection):  
    from OpenSSL import crypto  
    import binascii  
    from msrestazure import azure_active_directory  
    import adal 

    # Get the Azure Automation RunAs service principal certificate  
    cert = automationassets.get_automation_certificate("AzureRunAsCertificate")  
    pks12_cert = crypto.load_pkcs12(cert)  
    pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM,pks12_cert.get_privatekey())  

    # Get run as connection information for the Azure Automation service principal 
    application_id = runas_connection["ApplicationId"]  
    thumbprint = runas_connection["CertificateThumbprint"]  
    tenant_id = runas_connection["TenantId"]  

    # Authenticate with service principal certificate  
    resource ="https://management.core.windows.net/"  
    authority_url = ("https://login.microsoftonline.com/"+tenant_id)  
    context = adal.AuthenticationContext(authority_url)  
    return azure_active_directory.AdalAuthentication(  
    lambda: context.acquire_token_with_client_certificate(  
            resource,  
            application_id,  
            pem_pkey,  
            thumbprint) 
    ) 

# Authenticate to Azure using the Azure Automation RunAs service principal  
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")  
azure_credential = get_automation_runas_credential(runas_connection)  

# Intialize the resource management client with the RunAs credential and subscription  
resource_client = azure.mgmt.resource.ResourceManagementClient(  
    azure_credential,  
    str(runas_connection["SubscriptionId"]))  

# Get list of resource groups and print them out  
groups = resource_client.resource_groups.list()  
for group in groups:  
    print(group.name) 
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Vorbereiten eines Python-Runbooks finden Sie unter [Tutorial: Erstellen eines Python-Runbooks](learn/automation-tutorial-runbook-textual-python-3.md).
