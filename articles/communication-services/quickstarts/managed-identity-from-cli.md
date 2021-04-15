---
title: Erstellen einer Azure Active Directory-Anwendung für verwaltete Identitäten über die Azure CLI
titleSuffix: An Azure Communication Services quickstart
description: Mithilfe von verwalteten Identitäten können Sie den Zugriff auf Azure Communication Services aus Anwendungen autorisieren, die auf Azure-VMs, in Funktions-Apps und in anderen Ressourcen ausgeführt werden. Dieser Schnellstart konzentriert sich auf die Verwaltung der Identität mithilfe der Azure CLI.
services: azure-communication-services
author: jbeauregardb
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 03/10/2021
ms.author: jbeauregardb
ms.reviewer: mikben
ms.openlocfilehash: 2ef5a3b162d62fa79ed01a156345070ee12b4862
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105110677"
---
# <a name="authorize-access-with-managed-identity-to-your-communication-resource-in-your-development-environment"></a>Autorisieren des Zugriffs mit der verwalteten Identität auf die Kommunikationsressource in Ihrer Entwicklungsumgebung

Mit dem Azure Identity SDK wird die Azure AD-Tokenauthentifizierung (Azure Active Directory) für das Azure SDK unterstützt. Die neuesten Versionen der Azure Communication Services-SDKs für .NET, Java, Python und JavaScript sind in die Azure-Identitätsbibliothek integriert und stellen so eine einfache und sichere Möglichkeit dar, ein OAuth 2.0-Token für die Autorisierung von Azure Communication Services-Anforderungen abzurufen.

Ein Vorteil der Azure Identity SDKs besteht darin, dass Sie den gleichen Code verwenden können, um in mehreren Diensten zu authentifizieren, ob Ihre Anwendung in der Entwicklungsumgebung oder in Azure ausgeführt wird. Mit dem Azure Identity SDK wird ein Sicherheitsprinzipal authentifiziert. Wenn Ihr Code in Azure ausgeführt wird, ist der Sicherheitsprinzipal eine verwaltete Identität für Azure-Ressourcen. In der Entwicklungsumgebung ist die verwaltete Identität nicht vorhanden, sodass das SDK für Testzwecke entweder den Benutzer oder eine registrierte Anwendung authentifiziert.

## <a name="prerequisites"></a>Voraussetzungen

 - Azure-Befehlszeilenschnittstelle. [Installationshandbuch](/cli/azure/install-azure-cli)
 - Ein Azure-Konto mit einem aktiven Abonnement. [Kostenlos ein Konto erstellen](https://azure.microsoft.com/free)

## <a name="setting-up"></a>Einrichten

Verwaltete Identitäten sollten für die autorisierten Azure-Ressourcen aktiviert werden. Informationen zum Aktivieren von verwalteten Identitäten für Azure-Ressourcen finden Sie in diesen Artikeln:

- [Azure portal](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure-Befehlszeilenschnittstelle](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-Vorlage](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager-SDKs](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)
- [App Services](../../app-service/overview-managed-identity.md)

## <a name="authenticate-a-registered-application-in-the-development-environment"></a>Authentifizieren einer registrierten Anwendung in der Entwicklungsumgebung

Wenn Ihre Entwicklungsumgebung kein einmaliges Anmelden bzw. keine Anmeldung über einen Webbrowser unterstützt, können Sie für die Authentifizierung aus der Entwicklungsumgebung eine registrierte Anwendung verwenden.

### <a name="creating-an-azure-active-directory-registered-application"></a>Erstellen einer registrierten Azure Active Directory-Anwendung

Zum Erstellen einer registrierten Anwendung über die Azure CLI müssen Sie bei dem Azure-Konto angemeldet sein, in dem die Vorgänge ausgeführt werden sollen. Dazu können Sie den Befehl `az login` verwenden und Ihre Anmeldeinformationen im Browser eingeben. Nachdem Sie sich über die CLI bei Ihrem Azure-Konto angemeldet haben, können Sie den Befehl `az ad sp create-for-rbac` zum Erstellen der registrierten Anwendung aufrufen.

In den folgenden Beispielen wird die Azure CLI verwendet, um eine neue registrierte Anwendung zu erstellen.

```azurecli
az ad sp create-for-rbac --name <application-name> 
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
> Die Azure-Rollenzuweisungen können einige Minuten dauern.

#### <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen

Über das Azure Identity SDK werden zur Laufzeit Werte aus drei Umgebungsvariablen gelesen, um die Anwendung zu authentifizieren. In der folgenden Tabelle sind die für die einzelnen Umgebungsvariablen festzulegenden Werte beschrieben.

|Umgebungsvariable|value
|-|-
|`AZURE_CLIENT_ID`|Wert `appId` aus dem generierten JSON-Code 
|`AZURE_TENANT_ID`|Wert `tenant` aus dem generierten JSON-Code
|`AZURE_CLIENT_SECRET`|Wert `password` aus dem generierten JSON-Code

> [!IMPORTANT]
> Nachdem Sie die Umgebungsvariablen festgelegt haben, schließen Sie das Konsolenfenster, und öffnen Sie es dann erneut. Wenn Sie Visual Studio oder eine andere Entwicklungsumgebung verwenden, müssen Sie möglicherweise einen Neustart vornehmen, damit die neuen Umgebungsvariablen registriert werden.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Authentifizierung](../concepts/authentication.md)

Das könnte Sie auch interessieren:

- [Weitere Informationen zur Azure-Identitätsbibliothek](/dotnet/api/overview/azure/identity-readme)