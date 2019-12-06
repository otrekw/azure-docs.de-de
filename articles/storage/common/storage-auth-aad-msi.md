---
title: Autorisieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory und verwalteten Identitäten für Azure-Ressourcen – Azure Storage
description: Azure Blob Storage und Azure Queue Storage unterstützen das Autorisieren des Zugriffs auf Ressourcen mit Azure Active Directory und verwaltete Identitäten für Azure-Ressourcen. Sie können verwaltete Identitäten für Azure-Ressourcen verwenden, um den Zugriff auf Blobs und Warteschlangen über Anwendungen zu autorisieren, die auf virtuellen Azure-Computern, in Funktions-Apps, in VM-Skalierungsgruppen o. Ä. ausgeführt werden.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/25/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: ea1d286d00564587a9692dac1b04c5bbb04742cc
ms.sourcegitcommit: c31dbf646682c0f9d731f8df8cfd43d36a041f85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2019
ms.locfileid: "74561473"
---
# <a name="authorize-access-to-blobs-and-queues-with-azure-active-directory-and-managed-identities-for-azure-resources"></a>Autorisieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory und verwalteten Identitäten für Azure-Ressourcen

Azure Blob Storage und Azure Queue Storage unterstützen die Azure Active Directory-Authentifizierung (Azure AD-Authentifizierung) mit [verwalteten Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md). Sie können verwaltete Identitäten für Azure-Ressourcen verwenden, um den Zugriff auf Blob- und Warteschlangendaten mithilfe von Azure AD-Anmeldeinformationen über Anwendungen zu autorisieren, die auf virtuellen Azure-Computern, in Funktions-Apps, in VM-Skalierungsgruppen und anderen Diensten ausgeführt werden. Durch Verwendung von verwalteten Identitäten für Azure-Ressourcen zusammen mit der Azure AD-Authentifizierung können Sie vermeiden, dass Anmeldeinformationen mit den in der Cloud ausgeführten Anwendungen gespeichert werden.  

In diesem Artikel wird die Autorisierung des Zugriffs auf Blob- oder Warteschlangendaten über einen virtuellen Azure-Computer unter Verwendung verwalteter Identitäten erläutert. Darüber hinaus erfahren Sie, wie Sie Ihren Code in der Entwicklungsumgebung testen.

## <a name="enable-managed-identities-on-a-vm"></a>Aktivieren von verwalteten Identitäten auf einem virtuellen Computer

Damit Sie verwaltete Identitäten für Azure-Ressourcen zum Autorisieren des Zugriffs auf Blobs und Warteschlangen über Ihren virtuellen Computer verwenden können, müssen Sie zunächst verwaltete Identitäten für Ressourcen auf dem virtuellen Computer aktivieren. Informationen zum Aktivieren von verwalteten Identitäten für Azure-Ressourcen finden Sie in diesen Artikeln:

- [Azure-Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure-Befehlszeilenschnittstelle](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-Vorlage](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager-Clientbibliotheken](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="authenticate-with-the-azure-identity-library"></a>Authentifizieren mit der Azure Identity-Bibliothek

Ein Vorteil der Azure Identity-Clientbibliothek besteht darin, dass Sie den gleichen Code verwenden können, um zu authentifizieren, ob Ihre Anwendung in der Entwicklungsumgebung oder in Azure läuft. Beim Ausführen von Code in der Azure-Umgebung authentifiziert die Clientbibliothek eine verwaltete Identität für Azure-Ressourcen. In der Entwicklungsumgebung ist die verwaltete Identität nicht vorhanden, sodass die Clientbibliothek für Testzwecke entweder den Benutzer oder einen Dienstprinzipal authentifiziert.

Die Azure Identity-Clientbibliothek für .NET dient zur Authentifizierung eines Sicherheitsprinzipals. Wenn Ihr Code in Azure ausgeführt wird, ist der Sicherheitsprinzipal eine verwaltete Identität für Azure-Ressourcen.

Nach der Authentifizierung erhält die Azure Identity-Clientbibliothek Tokenanmeldeinformationen. Diese Tokenanmeldeinformationen werden im Dienstclientobjekt gekapselt, das Sie erstellen, um Vorgänge für Azure Storage auszuführen. Die Bibliothek führt diesen Schritt automatisch für Sie durch und ruft die entsprechenden Tokenanmeldeinformationen ab.

Weitere Informationen zur Azure Identity-Clientbibliothek finden Sie unter [Azure Identity-Clientbibliothek für .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity).

### <a name="assign-role-based-access-control-rbac-roles-for-access-to-data"></a>Zuweisen der rollenbasierten Zugriffssteuerung (RBAC) für den Datenzugriff

Wenn ein Azure AD-Sicherheitsprinzipal versucht, auf Blob- oder Warteschlangendaten zuzugreifen, muss dieser Sicherheitsprinzipal über Berechtigungen für die Ressource verfügen. Dem Sicherheitsprinzipal muss eine RBAC-Rolle zugewiesen werden, die den Zugriff auf Blob- oder Warteschlangendaten in Azure Storage ermöglicht. Dabei spielt es keine Rolle, ob es sich bei dem Sicherheitsprinzipal um eine verwaltete Identität in Azure oder um ein Azure AD-Benutzerkonto handelt, mit dem Code in der Entwicklungsumgebung ausgeführt wird. Informationen zur Zuweisung von Berechtigungen per RBAC finden Sie unter **Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory** im Abschnitt [Zuweisen von RBAC-Rollen für Zugriffsrechte](../common/storage-auth-aad.md#assign-rbac-roles-for-access-rights).

### <a name="authenticate-the-user-in-the-development-environment"></a>Authentifizieren des Benutzers in der Entwicklungsumgebung

Wenn Ihr Code in der Entwicklungsumgebung ausgeführt wird, erfolgt die Authentifizierung ggf. automatisch. Abhängig von den verwendeten Tools kann aber auch eine Browseranmeldung erforderlich sein. Microsoft Visual Studio unterstützt einmaliges Anmelden (Single Sign-On, SSO), sodass automatisch das aktive Azure AD-Benutzerkonto für die Authentifizierung verwendet wird. Weitere Informationen zu SSO finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](../../active-directory/manage-apps/what-is-single-sign-on.md).

Bei anderen Entwicklungstools ist ggf. eine Anmeldung über einen Webbrowser erforderlich.

### <a name="authenticate-a-service-principal-in-the-development-environment"></a>Authentifizieren des Dienstprinzipals in der Entwicklungsumgebung

Wenn Ihre Entwicklungsumgebung kein einmaliges Anmelden bzw. keine Anmeldung über einen Webbrowser unterstützt, können Sie für die Authentifizierung aus der Entwicklungsumgebung einen Dienstprinzipal verwenden.

#### <a name="create-the-service-principal"></a>Erstellen des Dienstprinzipals

Rufen Sie den Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) auf, um einen Dienstprinzipal über die Azure-Befehlszeilenschnittstelle zu erstellen und eine RBAC-Rolle zuzuweisen. Geben Sie eine Azure Storage-Datenzugriffsrolle an, die dem neuen Dienstprinzipal zugewiesen werden soll. Geben Sie außerdem den Bereich für die Rollenzuweisung an. Weitere Informationen zu den für Azure Storage bereitgestellten integrierten Rollen finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../../role-based-access-control/built-in-roles.md).

Wenn Sie nicht über ausreichende Berechtigungen zum Zuweisen einer Rolle zum Dienstprinzipal verfügen, müssen Sie möglicherweise den Kontobesitzer oder den Administrator bitten, die Rollenzuweisung vorzunehmen.

Im folgenden Beispiel wird ein neuer Dienstprinzipal über die Azure-Befehlszeilenschnittstelle erstellt. Außerdem wird ihm die Rolle **Storage Blob Data Reader** im Kontobereich zugewiesen.

```azurecli-interactive
az ad sp create-for-rbac \
    --name <service-principal> \
    --role "Storage Blob Data Reader" \
    --scopes /subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

Mit dem Befehl `az ad sp create-for-rbac` wird eine Liste der Dienstprinzipaleigenschaften im JSON-Format zurückgegeben. Kopieren Sie diese Werte, damit Sie sie zum Erstellen der erforderlichen Umgebungsvariablen im nächsten Schritt verwenden können.

```json
{
    "appId": "generated-app-ID",
    "displayName": "service-principal-name",
    "name": "http://service-principal-uri",
    "password": "generated-password",
    "tenant": "tenant-ID"
}
```

> [!IMPORTANT]
> Die RBAC-Rollenzuweisungen können einige Minuten dauern.

#### <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen

Über die Azure Identity-Clientbibliothek werden zur Laufzeit Werte aus drei Umgebungsvariablen gelesen, um den Dienstprinzipal zu authentifizieren. In der folgenden Tabelle sind die für die einzelnen Umgebungsvariablen festzulegenden Werte beschrieben.

|Umgebungsvariable|Wert
|-|-
|`AZURE_CLIENT_ID`|Die App-ID für den Dienstprinzipal
|`AZURE_TENANT_ID`|Die Azure AD-Mandanten-ID des Dienstprinzipals
|`AZURE_CLIENT_SECRET`|Das für den Dienstprinzipal generierte Kennwort

> [!IMPORTANT]
> Nachdem Sie die Umgebungsvariablen festgelegt haben, schließen Sie das Konsolenfenster, und öffnen Sie es dann erneut. Wenn Sie Visual Studio oder eine andere Entwicklungsumgebung verwenden, müssen Sie möglicherweise die Entwicklungsumgebung neu starten, damit die neuen Umgebungsvariablen registriert werden.

Weitere Informationen finden Sie unter [Gewusst wie: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](../../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="install-client-library-packages"></a>Installieren von Clientbibliothekspaketen

In den Beispielen in diesem Artikel wird die neueste Version der Azure Storage-Clientbibliothek für Blob Storage verwendet. Führen Sie zum Installieren des Pakets den folgenden Befehl über die NuGet-Paket-Manager-Konsole aus:

```powershell
Install-Package Azure.Storage.Blobs
```

In den Beispielen in diesem Artikel wird auch die neueste Version der [Azure Identity-Clientbibliothek für .NET](https://www.nuget.org/packages/Azure.Identity/) zur Authentifizierung mit Azure AD-Anmeldeinformationen verwendet. Führen Sie zum Installieren des Pakets den folgenden Befehl über die NuGet-Paket-Manager-Konsole aus:

```powershell
Install-Package Azure.Identity
```

## <a name="net-code-example-create-a-block-blob"></a>Codebeispiel für .NET: Erstellen eines Blockblobs

Fügen Sie dem Code die folgenden `using`-Anweisungen hinzu, um die Azure Identity- und Azure Storage-Clientbibliotheken zu verwenden.

```csharp
using Azure;
using Azure.Identity;
using Azure.Storage.Blobs;
using System;
using System.IO;
using System.Text;
using System.Threading.Tasks;
```

Erstellen Sie eine Instanz der Klasse [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential), um Tokenanmeldeinformationen abzurufen, die in Ihrem Code zum Autorisieren von Anforderungen für Azure Storage verwendet werden können. Das folgende Codebeispiel zeigt, wie Sie die authentifizierten Tokenanmeldeinformationen abrufen, mit ihnen ein Dienstclientobjekt erstellen und anschließend unter Verwendung des Dienstclients ein neues Blob hochladen:

```csharp
async static Task CreateBlockBlobAsync(string accountName, string containerName, string blobName)
{
    // Construct the blob container endpoint from the arguments.
    string containerEndpoint = string.Format("https://{0}.blob.core.windows.net/{1}",
                                                accountName,
                                                containerName);

    // Get a credential and create a client object for the blob container.
    BlobContainerClient containerClient = new BlobContainerClient(new Uri(containerEndpoint),
                                                                    new DefaultAzureCredential());

    try
    {
        // Create the container if it does not exist.
        await containerClient.CreateIfNotExistsAsync();

        // Upload text to a new block blob.
        string blobContents = "This is a block blob.";
        byte[] byteArray = Encoding.ASCII.GetBytes(blobContents);

        using (MemoryStream stream = new MemoryStream(byteArray))
        {
            await containerClient.UploadBlobAsync(blobName, stream);
        }
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

> [!NOTE]
> Zum Autorisieren von Anforderungen für Blob- oder Warteschlangendaten mit Azure AD müssen Sie für die Anforderungen HTTPS verwenden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu RBAC-Rollen für Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Speicherdaten mit RBAC](storage-auth-aad-rbac.md).
- Informationen zum Autorisieren des Zugriffs auf Container und Warteschlangen aus Ihren Speicheranwendungen finden Sie unter [Verwenden von Azure AD mit Speicheranwendungen](storage-auth-aad-app.md).
- Informationen zum Ausführen von Azure CLI- und PowerShell-Befehlen mit Azure AD-Anmeldeinformationen finden Sie unter [Ausführen von Azure CLI- oder PowerShell-Befehlen mit Azure AD-Anmeldeinformationen für den Zugriff auf Blob- oder Warteschlangendaten](storage-auth-aad-script.md).
