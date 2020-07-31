---
title: 'Tutorial: Verwenden von Azure Key Vault mit einem virtuellen Computer in Python | Microsoft-Dokumentation'
description: In diesem Tutorial konfigurieren Sie eine ASP.NET Core-Anwendung zum Lesen eines Geheimnisses aus Ihrem Schlüsseltresor.
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 07/20/2020
ms.author: mbaldwin
ms.custom: mvc, tracking-python
ms.openlocfilehash: 453307b304c4cb1899b1de31117c944ac66fcddb
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87093560"
---
# <a name="tutorial-use-azure-key-vault-with-a-virtual-machine-in-python"></a>Tutorial: Verwenden von Azure Key Vault mit einem virtuellen Windows in Python

Azure Key Vault unterstützt Sie beim Schutz von Geheimnissen wie API-Schlüsseln und Datenbank-Verbindungszeichenfolgen, die für den Zugriff auf Ihre Anwendungen, Dienste und IT-Ressourcen benötigt werden.

In diesem Tutorial wird beschrieben, wie Sie vorgehen müssen, damit eine Konsolenanwendung Informationen aus Azure Key Vault lesen kann. Verwenden Sie hierfür verwaltete Identitäten für Azure-Ressourcen. 

Das Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Erstellen eines Schlüsseltresors
> * Hinzufügen eines Geheimnisses zum Schlüsseltresor.
> * Abrufen eines Geheimnisses aus dem Schlüsseltresor
> * Erstellen Sie einen virtuellen Azure-Computer.
> * Aktivieren einer verwalteten Identität
> * Zuweisen von Berechtigungen zur VM-Identität

Machen Sie sich zunächst mit den [grundlegenden Konzepten von Key Vault](basic-concepts.md) vertraut. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Für Windows, Mac und Linux:
  * [Git-Client](https://git-scm.com/downloads)
  * Für dieses Tutorial ist es erforderlich, dass Sie die Azure CLI lokal ausführen. Hierfür muss Azure CLI Version 2.0.4 oder höher installiert sein. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zur Installation und Aktualisierung der CLI finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="log-in-to-azure"></a>Anmelden an Azure

Geben Sie Folgendes ein, um sich mithilfe der Azure-Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

### <a name="create-a-resource-group-and-key-vault"></a>Erstellen einer Ressourcengruppe und eines Schlüsseltresors

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

## <a name="add-a-secret-to-the-key-vault"></a>Hinzufügen eines Geheimnisses zum Schlüsseltresor

Wir fügen ein Geheimnis hinzu, um die Vorgehensweise zu veranschaulichen. Bei diesem Geheimnis kann es sich beispielsweise um eine SQL-Verbindungszeichenfolge oder um beliebige andere Informationen handeln, die sowohl sicher aufbewahrt werden als auch für Ihre Anwendung verfügbar sein müssen.

Geben Sie den folgenden Befehl ein, um im Schlüsseltresor ein Geheimnis namens **AppSecret** zu erstellen:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Dieses Geheimnis speichert den Wert **MySecret**.

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers
Mithilfe einer der folgenden Methoden können Sie einen virtuellen Computer erstellen:

* [Die Azure-CLI](../../virtual-machines/windows/quick-create-cli.md)
* [PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
* [Azure-Portal](../../virtual-machines/windows/quick-create-portal.md)

## <a name="assign-an-identity-to-the-vm"></a>Zuweisen einer Identität zum virtuellen Computer
In diesem Schritt erstellen Sie mithilfe des folgenden Befehls in der Azure CLI eine systemseitig zugewiesene Identität für den virtuellen Computer:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Beachten Sie die systemseitig zugewiesene Identität, die im folgenden Code angezeigt wird. Die Ausgabe des obigen Befehls lautet etwa wie folgt: 

```output
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="assign-permissions-to-the-vm-identity"></a>Zuweisen von Berechtigungen für die VM-Identität
Jetzt können Sie Ihrem Schlüsseltresor die zuvor erstellten Identitätsberechtigungen zuweisen, indem Sie den folgenden Befehl ausführen:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-on-to-the-virtual-machine"></a>Anmelden beim virtuellen Computer

Führen Sie die Anweisungen unter [Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](../../virtual-machines/windows/connect-logon.md) aus, um sich beim virtuellen Computer anzumelden.

## <a name="create-and-run-a-sample-python-app"></a>Erstellen und Ausführen einer Python-Beispiel-App

Im nächsten Abschnitt finden Sie eine Beispieldatei mit dem Namen *Sample.py*. Die Datei verwendet eine [Anforderungsbibliothek](https://2.python-requests.org/en/master/), um HTTP-GET-Aufrufe auszuführen.

## <a name="edit-samplepy"></a>Bearbeiten der Datei „Sample.py“

Nachdem Sie *Sample.py* erstellt haben, öffnen Sie die Datei, und kopieren Sie den Code in diesem Abschnitt. 

Der Code ist ein zweistufiger Prozess:
1. Rufen Sie ein Token vom lokalen MSI-Endpunkt auf dem virtuellen Computer ab.  
  Dadurch wird auch ein Token von Azure AD abgerufen.
1. Übergeben Sie das Token an Ihren Schlüsseltresor, und rufen Sie anschließend Ihr Geheimnis ab. 

```python
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource.
    # Resources with an MSI configured recieve an AAD access token by using the Azure Instance Metadata Service (IMDS)
    # IMDS provides an endpoint accessible to all IaaS VMs using a non-routable well-known IP Address
    # To learn more about IMDS and MSI Authentication see the following link: https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets an access_token from Azure AD by using the local MSI endpoint.
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to your key vault.
    KeyVaultURL = "https://{YOUR KEY VAULT NAME}.vault.azure.net/secrets/{YOUR SECRET NAME}?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Sie können den Geheimniswert anzeigen, indem Sie den folgenden Code ausführen: 

```console
python Sample.py
```

Der Code oben veranschaulicht, wie Sie mit Azure Key Vault Vorgänge auf einem virtuellen Windows-Computer ausführen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie den virtuellen Computer und Ihren Schlüsseltresor, wenn diese nicht mehr benötigt werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Azure Key Vault-REST-API](https://docs.microsoft.com/rest/api/keyvault/)
