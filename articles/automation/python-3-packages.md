---
title: Verwalten von Python 3-Paketen in Azure Automation
description: In diesem Artikel wird erläutert, wie Sie Python 3-Pakete (Vorschau) in Azure Automation verwalten.
services: automation
ms.subservice: process-automation
ms.date: 12/22/2020
ms.topic: conceptual
ms.openlocfilehash: 3f39f49ff47b935da7ffc777ee45bd219f5740b5
ms.sourcegitcommit: f7084d3d80c4bc8e69b9eb05dfd30e8e195994d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2020
ms.locfileid: "97734300"
---
# <a name="manage-python-3-packages-preview-in-azure-automation"></a>Verwalten von Python 3-Paketen (Vorschau) in Azure Automation

Mit Azure Automation können Sie Python 3-Runbooks (Vorschau) in Azure und Hybrid Runbook Worker unter Linux ausführen. Um die Runbooks zu vereinfachen, können Sie mithilfe von Python-Paketen die erforderlichen Module importieren. In diesem Artikel wird beschrieben, wie Sie Python 3-Pakete (Vorschau) in Azure Automation verwalten und verwenden.

## <a name="import-packages"></a>Importieren von Paketen

Wählen Sie in Ihrem Azure Automation-Konto unter **Python-Pakete** die Option **Freigegebene Ressourcen** aus. Wählen Sie **+ Python-Paket hinzufügen** aus.

:::image type="content" source="media/python-3-packages/add-python-3-package.png" alt-text="Screenshot der Seite „Python 3-Pakete“ mit Python 3-Paketen im linken Menü und Hervorhebung von „Python-Paket hinzufügen“.":::

Wählen Sie auf der Seite **Python-Paket hinzufügen** „Python 3“ als **Version** und ein hochzuladendes lokales Paket aus. Das Paket kann eine Datei im Format **.whl** oder **.tar.gz** sein. Wählen Sie nach Auswahl des Pakets **OK** aus, um es hochzuladen.

:::image type="content" source="media/python-3-packages/upload-package.png" alt-text="Screenshot der Seite „Python 3-Paket hinzufügen“ mit Auswahl der hochgeladenen Datei „tar.gz“.":::

Sobald ein Paket importiert wurde, wird es in Ihrem Azure Automation-Konto auf der Seite „Python-Pakete“ auf der Registerkarte **Python 3-Pakete (Vorschau)** aufgelistet. Um ein Paket zu entfernen, wählen Sie das Paket aus, und klicken Sie dann auf **Löschen**.

:::image type="content" source="media/python-3-packages/python-3-packages-list.png" alt-text="Screenshot der Seite „Python 3-Pakete“ nach Import eines Pakets.":::

## <a name="import-packages-with-dependencies"></a>Importieren von Paketen mit Abhängigkeiten

Azure Automation löst Abhängigkeiten für Python-Pakete während des Importvorgangs nicht auf. Es gibt jedoch eine Möglichkeit, ein Paket mit allen seinen Abhängigkeiten zu importieren.

### <a name="manually-download"></a>Manuelles Herunterladen

Führen Sie auf einem Windows-Computer (64 Bit), auf dem [Python 3.8](https://www.python.org/downloads/release/python-380/) und [pip](https://pip.pypa.io/en/stable/) installiert sind, den folgenden Befehl aus, um ein Paket mit allen seinen Abhängigkeiten herunterzuladen:

```cmd
C:\Python38\Scripts\pip3.8.exe download -d <output dir> <package name>
```

Sobald die Pakete heruntergeladen sind, können Sie sie in Ihr Azure Automation-Konto importieren.

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
