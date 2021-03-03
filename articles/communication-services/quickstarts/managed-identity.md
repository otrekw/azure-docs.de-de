---
title: Verwenden von verwalteten Identitäten in Communication Services (.NET)
titleSuffix: An Azure Communication Services quickstart
description: Mithilfe von verwalteten Identitäten können Sie den Zugriff auf Azure Communication Services aus Anwendungen autorisieren, die auf Azure-VMs, in Funktions-Apps und in anderen Ressourcen ausgeführt werden.
services: azure-communication-services
author: stefang931
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 12/04/2020
ms.author: gistefan
ms.reviewer: mikben
ms.openlocfilehash: 7e8d9b56077819fc404d6c2bdc39f9f697224136
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101692180"
---
# <a name="use-managed-identities-net"></a>Verwenden verwalteter Identitäten (.NET)

Erste Schritte bei der Verwendung verwalteter Identitäten mit Azure Communication Services in .NET. Die Verwaltungs- und SMS-Clientbibliotheken von Communication Services unterstützen die Azure AD-Authentifizierung (Azure Active Directory) mit [verwalteten Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md).

In diesem Schnellstart erfahren Sie, wie Sie den Zugriff auf die Verwaltungs- und SMS-Clientbibliotheken aus einer Azure-Umgebung autorisieren, die verwaltete Identitäten unterstützt. Darüber hinaus erfahren Sie, wie Sie Ihren Code in einer Entwicklungsumgebung testen.

## <a name="prerequisites"></a>Voraussetzungen

 - Ein Azure-Konto mit einem aktiven Abonnement. [Kostenlos ein Konto erstellen](https://azure.microsoft.com/free)
 - Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource.](./create-communication-resource.md?pivots=platform-azp&tabs=windows)

## <a name="setting-up"></a>Einrichten

### <a name="enable-managed-identities-on-a-virtual-machine-or-app-service"></a>Aktivieren verwalteter Identitäten auf einer VM oder in App Service

Verwaltete Identitäten sollten für die autorisierten Azure-Ressourcen aktiviert werden. Informationen zum Aktivieren von verwalteten Identitäten für Azure-Ressourcen finden Sie in diesen Artikeln:

- [Azure portal](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure-Befehlszeilenschnittstelle](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-Vorlage](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager-Clientbibliotheken](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)
- [App Services](../../app-service/overview-managed-identity.md)

#### <a name="assign-azure-roles-with-the-azure-portal"></a>Zuweisen von Azure-Rollen über das Azure-Portal

1. Navigieren Sie zum Azure-Portal.
1. Navigieren Sie zur Azure Communication Services-Ressource.
1. Navigieren Sie im Menü „Zugriffssteuerung (IAM)“ zu „Hinzufügen“ und dann zu „Rollenzuweisung hinzufügen“.
1. Wählen Sie die Rolle „Mitwirkender“ aus (die einzige unterstützte Rolle).
1. Wählen Sie „Benutzerseitig zugewiesene verwaltete Identität“ (oder „Systemseitig zugewiesene verwaltete Identität“) und dann die gewünschte Identität aus. Speichern Sie Ihre Auswahl.

![Rolle der verwalteten Identität](media/managed-identity-assign-role.png)

#### <a name="assign-azure-roles-with-powershell"></a>Zuweisen von Azure-Rollen mit PowerShell

Weitere Informationen zum Zuweisen von Rollen und Berechtigungen mithilfe von PowerShell finden Sie unter [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe von Azure PowerShell](../../../articles/role-based-access-control/role-assignments-powershell.md).

## <a name="add-managed-identity-to-your-communication-services-solution"></a>Hinzufügen einer verwalteten Identität zu Ihrer Communication Services-Lösung

### <a name="install-the-client-library-packages"></a>Installieren der Pakete der Clientbibliothek

```console
dotnet add package Azure.Communication.Identity
dotnet add package Azure.Communication.Configuration
dotnet add package Azure.Communication.Sms
dotnet add package Azure.Identity
```

### <a name="use-the-client-library-packages"></a>Verwenden der Pakete der Clientbibliothek

Fügen Sie dem Code die folgenden `using`-Anweisungen hinzu, um die Azure Identity- und Azure Storage-Clientbibliotheken zu verwenden.

```csharp
using Azure.Identity;
using Azure.Communication.Identity;
using Azure.Communication.Configuration;
using Azure.Communication.Sms;
```

In den nachstehenden Beispielen wird [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) verwendet. Diese Anmeldeinformationen eignen sich für Produktions- und Entwicklungsumgebungen.

### <a name="create-an-identity-and-issue-a-token"></a>Erstellen einer Identität und Ausstellen eines Tokens

Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienstclientobjekt mit Azure Active Directory-Token erstellt und der Client anschließend verwendet wird, um ein Token für einen neuen Benutzer auszustellen:

```csharp
     public async Task<Response<CommunicationUserToken>> CreateIdentityAndIssueTokenAsync(Uri resourceEdnpoint) 
     {
          TokenCredential credential = new DefaultAzureCredential();
     
          var client = new CommunicationIdentityClient(resourceEndpoint, credential);
          var identityResponse = await client.CreateUserAsync();
     
          var tokenResponse = await client.IssueTokenAsync(identity, scopes: new [] { CommunicationTokenScope.VoIP });

          return tokenResponse;
     }
```

### <a name="send-an-sms-with-azure-active-directory-tokens"></a>Senden einer SMS mit Azure Active Directory-Token

Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienstclientobjekt mit Azure Active Directory-Token erstellt und der Client anschließend verwendet wird, um eine SMS zu senden:

```csharp

     public async Task SendSmsAsync(Uri resourceEndpoint, PhoneNumber from, PhoneNumber to, string message)
     {
          TokenCredential credential = new DefaultAzureCredential();
     
          SmsClient smsClient = new SmsClient(resourceEndpoint, credential);
          smsClient.Send(
               from: from,
               to: to,
               message: message,
               new SendSmsOptions { EnableDeliveryReport = true } // optional
          );
     }
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Authentifizierung](../concepts/authentication.md)

Das könnte Sie auch interessieren:

- [Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure](../../../articles/role-based-access-control/index.yml)
- [Weitere Informationen zur Azure-Identitätsbibliothek für .NET](/dotnet/api/overview/azure/identity-readme)
- [Erstellen von Benutzerzugriffstoken](../quickstarts/access-tokens.md)
- [Senden einer SMS](../quickstarts/telephony-sms/send.md)
- [Weitere Informationen zu SMS](../concepts/telephony-sms/concepts.md)