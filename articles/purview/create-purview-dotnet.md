---
title: Erstellen eines Purview-Kontos mit dem .NET SDK
description: Erstellen Sie ein Azure Purview-Konto mithilfe des .NET SDK.
author: nayenama
ms.service: purview
ms.subservice: purview-data-catalog
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 4/2/2021
ms.author: nayenama
ms.openlocfilehash: 04ed5cef351c81355a2390dd0b983c162f2b9532
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106387171"
---
# <a name="quickstart-create-a-purview-account-using-net-sdk"></a>Schnellstart: Erstellen eines Purview-Kontos mithilfe des .NET SDK

Diese Schnellstartanleitung beschreibt, wie Sie mithilfe des .NET SDK eine Azure Purview-Konto erstellen 

> [!IMPORTANT]
> Azure Purview ist derzeit als VORSCHAUVERSION verfügbar. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen für Azure-Features, die sich in der Beta- oder Vorschauphase befinden oder anderweitig noch nicht allgemein verfügbar sind.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Ihr eigener [Azure Active Directory-Mandant](../active-directory/fundamentals/active-directory-access-create-new-tenant.md).

* Ihr Konto muss zum Erstellen von Ressourcen im Abonnement berechtigt sein.

* Wenn durch **Azure Policy** alle Anwendungen an der Erstellung eines **Speicherkontos** und eines **EventHub-Namespace** gehindert werden, benötigen Sie eine Richtlinienausnahme mit einem Tag, das im Rahmen der Erstellung eines Purview-Kontos eingegeben werden kann. Das liegt in erster Linie daran, dass für ein Purview-Konto jeweils eine verwaltete Ressourcengruppe mit einem darin enthaltenen Speicherkonto und einem EventHub-Namespace erstellt werden muss. Weitere Informationen finden Sie unter [Erstellen eines Katalog-Portals](create-catalog-portal.md)

### <a name="visual-studio"></a>Visual Studio

In der exemplarischen Vorgehensweise in diesem Artikel wird Visual Studio 2019 verwendet. Die Verfahren für Visual Studio 2013, 2015 oder 2017 unterscheiden sich geringfügig.

### <a name="azure-net-sdk"></a>Azure .NET SDK

Laden Sie das [Azure .NET SDK](https://azure.microsoft.com/downloads/) auf Ihren Computer herunter, und installieren Sie es.

## <a name="create-an-application-in-azure-active-directory"></a>Erstellen einer Anwendung in Azure Active Directory

Befolgen Sie aus den Abschnitten in *Vorgehensweise: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal* die Anweisungen zum Ausführen dieser Aufgaben:

1. Erstellen Sie unter [Erstellen einer Azure Active Directory-Anwendung](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal) eine Anwendung, die die .NET-Anwendung darstellt, die Sie in diesem Tutorial erstellen. Als Anmelde-URL können Sie wie in diesem Artikel gezeigt eine Platzhalter-URL (`https://contoso.org/exampleapp`) angeben.
2. Rufen Sie unter [Abrufen von Werten für die Anmeldung](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in) die **Anwendungs-ID** und die **Mandanten-ID** ab, und notieren Sie sich diese Werte zur späteren Verwendung in diesem Tutorial. 
3. Rufen Sie unter [Zertifikate und Geheimnisse](../active-directory/develop/howto-create-service-principal-portal.md#authentication-two-options) den **Authentifizierungsschlüssel** ab, und notieren Sie sich diesen Wert zur späteren Verwendung in diesem Tutorial.
4. Weisen Sie unter [Zuweisen der Anwendung zu einer Rolle](../active-directory/develop/howto-create-service-principal-portal.md#assign-a-role-to-the-application) die Anwendung der Rolle **Mitwirkender** auf Abonnementebene zu, damit die Anwendung Data Factorys im Abonnement erstellen kann.

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

Erstellen Sie im nächsten Schritt in Visual Studio eine C# .NET-Konsolenanwendung:

1. Starten Sie **Visual Studio**.
2. Wählen Sie im Fenster „Start“ die Option **Neues Projekt erstellen** > **Konsolen-App (.NET Framework)** aus. .NET-Version 4.5.2 oder höher ist erforderlich.
3. Geben Sie unter **Projektname** den Namen **ADFv2QuickStart** ein.
4. Wählen Sie **Create** (Erstellen), um das Projekt zu erstellen.

## <a name="install-nuget-packages"></a>Installieren von NuGet-Paketen

1. Klicken Sie auf **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.
2. Führen Sie im Bereich **Paket-Manager-Konsole** die folgenden Befehle zum Installieren von Paketen aus. Weitere Informationen finden Sie unter [Microsoft.Azure.Management.Purview NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Management.Purview/).

    ```powershell
    Install-Package Microsoft.Azure.Management.Purview
    Install-Package Microsoft.Azure.Management.ResourceManager -IncludePrerelease
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

## <a name="create-a-purview-client"></a>Erstellen eines Purview-Clients

1. Öffnen Sie **Program.cs**, und fügen Sie die folgenden Anweisungen ein, um Verweise auf Namespaces hinzuzufügen.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Rest;
    using Microsoft.Rest.Serialization;
      using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.Purview;
      using Microsoft.Azure.Management.Purview.Models;
      using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

2. Fügen Sie der **Main**-Methode den folgenden Code hinzu, der die Variablen festlegt. Ersetzen Sie die Platzhalter durch Ihre eigenen Werte. Eine Liste der Azure-Regionen, in denen Purview derzeit verfügbar ist, finden Sie, indem Sie in **Azure Purview** auf der Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/) nach den für Sie interessanten Regionen such und sie dann auswählen.

   ```csharp
   // Set variables
   string tenantID = "<your tenant ID>";
   string applicationId = "<your application ID>";
   string authenticationKey = "<your authentication key for the application>";
   string subscriptionId = "<your subscription ID where the data factory resides>";
   string resourceGroup = "<your resource group where the data factory resides>";
   string region = "<the location of your resource group>";
   string purviewAccountName = 
       "<specify the name of purview account to create. It must be globally unique.>";
   ```

3. Fügen Sie zur Methode **Main** den folgenden Code hinzu, mit dem eine Instanz der **PurviewManagementClient**-Klasse erstellt wird. Sie verwenden dieses Objekt, um ein Purview-Konto zu erstellen.

   ```csharp
   // Authenticate and create a purview management client
   var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
   ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
   AuthenticationResult result = context.AcquireTokenAsync(
   "https://management.azure.com/", cc).Result;
   ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
   var client = new PurviewManagementClient(cred)
   {
      SubscriptionId = subscriptionId           
   };
   ```

## <a name="create-a-purview-account"></a>Erstellen eines Purview-Kontos

Fügen Sie zur Methode **Main** den folgenden Code hinzu, mit dem ein **Purview-Konto** erstellt wird. 

```csharp
    // Create a purview Account
    Console.WriteLine("Creating Purview Account " + purviewAccountName + "...");
    Account account = new Account()
    {
    Location = region,
    Identity = new Identity(type: "SystemAssigned"),
    Sku = new AccountSku(name: "Standard", capacity: 4)
    };            
    try
    {
      client.Accounts.CreateOrUpdate(resourceGroup, purviewAccountName, account);
      Console.WriteLine(client.Accounts.Get(resourceGroup, purviewAccountName).ProvisioningState);                
    }
    catch (ErrorResponseModelException purviewException)
    {
      Console.WriteLine(purviewException.StackTrace);
    }
    Console.WriteLine(
      SafeJsonConvert.SerializeObject(account, client.SerializationSettings));
    while (client.Accounts.Get(resourceGroup, purviewAccountName).ProvisioningState ==
           "PendingCreation")
    {
      System.Threading.Thread.Sleep(1000);
    }
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
```

## <a name="run-the-code"></a>Ausführen des Codes

Erstellen und starten Sie die Anwendung, und überprüfen Sie dann die Ausführung.

Die Konsole den Fortschritt beim Erstellen des Purview-Kontos aus.

### <a name="sample-output"></a>Beispielausgabe

```json
Creating Purview Account testpurview...
Succeeded
{
  "sku": {
    "capacity": 4,
    "name": "Standard"
  },
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "southcentralus"
}

Press any key to exit...
```

## <a name="verify-the-output"></a>Überprüfen der Ausgabe

Wechseln Sie im [Azure-Portal](https://portal.azure.com) zur Seite mit den **Purview-Konten**, und überprüfen Sie das mit dem obigen Code erstellte Konto. 

## <a name="delete-purview-account"></a>Löschen eines Purview-Kontos

Um ein Purview-Konto programmgesteuert zu löschen, fügen Sie die folgenden Codezeilen zum Programm hinzu: 

```csharp
    Console.WriteLine("Deleting the Purview Account");
    client.Accounts.Delete(resourceGroup, purviewAccountName);
```

## <a name="check-if-purview-account-name-is-available"></a>Überprüfen Sie, ob der Name des Purview-Kontos verfügbar ist

Verwenden Sie den folgenden Code, um die Verfügbarkeit einer Purview-Kontos zu überprüfen: 

```csharp
    CheckNameAvailabilityRequest checkNameAvailabilityRequest = new CheckNameAvailabilityRequest()
    {
      Name = purviewAccountName,
      Type =  "Microsoft.Purview/accounts"
    };
    Console.WriteLine("Check Purview account name");
    Console.WriteLine(client.Accounts.CheckNameAvailability(checkNameAvailabilityRequest).NameAvailable);
```

Der obige Code gibt „true“ aus, wenn der Name verfügbar ist, und „false“, wenn der Name nicht verfügbar ist.


## <a name="next-steps"></a>Nächste Schritte

Mit dem Code in diesem Tutorial wird ein Purview-Konto erstellt oder gelöscht und die Verfügbarkeit eines Purview-Kontos mit einem bestimmten Namen überprüft. Sie können jetzt das .NET SDK herunterladen und sich über andere Aktionen des Ressourcenanbieters informieren, die Sie für ein Purview-Konto ausführen können.

Im nächsten Artikel erfahren Sie, wie Sie Benutzern Zugriff auf Ihr Azure Purview-Konto gewähren. 

> [!div class="nextstepaction"]
> [Rollenbasierte Zugriffssteuerung auf der Datenebene von Azure Purview](catalog-permissions.md)
