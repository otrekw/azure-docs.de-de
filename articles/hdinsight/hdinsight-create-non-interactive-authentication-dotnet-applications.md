---
title: .NET-Anwendung für die nicht interaktive Authentifizierung – Azure HDInsight
description: Erfahren Sie, wie Sie Microsoft .NET-Anwendungen für die nicht interaktive Authentifizierung in Azure HDInsight erstellen.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive, devx-track-csharp
ms.date: 12/23/2019
ms.openlocfilehash: 0fffe763f77fffcec6010d43deb92ffea9ece15d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101094915"
---
# <a name="create-a-non-interactive-authentication-net-hdinsight-application"></a>Erstellen einer .NET HDInsight-Anwendung für die nicht interaktive Authentifizierung

Führen Sie die Microsoft .NET Azure HDInsight-Anwendung entweder unter der eigenen Identität der Anwendung (nicht interaktiv) oder unter der Identität des angemeldeten Benutzers der Anwendung (interaktiv) aus. In diesem Artikel erfahren Sie, wie Sie eine .NET-Anwendung für die nicht interaktive Authentifizierung erstellen, um eine Verbindung mit Azure herzustellen und HDInsight zu verwalten. Ein Beispiel für die interaktive Anwendung finden Sie unter [Verbinden mit Azure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight).

Für die nicht interaktive .NET-Anwendung benötigen Sie Folgendes:

* Mandanten-ID Ihres Azure-Abonnements auch *Verzeichnis-ID*). Siehe [Abrufen der Mandanten-ID](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in).
* Client-ID der Azure Active Directory-Anwendung (Azure AD). Weitere Informationen finden Sie unter [Erstellen einer Azure Active Directory-Anwendung](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal) und [Abrufen der Anwendungs-ID](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in).
* Geheimer Schlüssel der Azure AD-Anwendung. Siehe [Abrufen des Anwendungsauthentifizierungsschlüssels](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in).

## <a name="prerequisites"></a>Voraussetzungen

Einen HDInsight-Cluster Siehe [Erste-Schritte-Tutorial](hadoop/apache-hadoop-linux-tutorial-get-started.md).

## <a name="assign-a-role-to-the-azure-ad-application"></a>Zuweisen einer Rolle zur Azure AD-Anwendung

Weisen Sie Ihrer Azure AD-Anwendung eine [Rolle](../role-based-access-control/built-in-roles.md) zu, um ihr Berechtigungen zum Ausführen von Aktionen zu gewähren. Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Die Berechtigungen werden von niedrigeren Ebenen mit geringerem Umfang geerbt. Wenn z. B. der Leserolle für eine Ressourcengruppe eine Anwendung hinzugefügt wird, kann diese Anwendung die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen. In diesem Artikel legen Sie den Bereich auf Ressourcengruppenebene fest. Weitere Informationen finden Sie unter [Zuweisen von Azure-Rollen zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](../role-based-access-control/role-assignments-portal.md).

**So fügen Sie der Azure AD-Anwendung die Rolle „Besitzer“ hinzu**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu der Ressourcengruppe mit dem HDInsight-Cluster, in dem Sie später in diesem Artikel die Hive-Abfrage ausführen möchten. Wenn Sie über eine große Anzahl von Ressourcengruppen verfügen, können Sie mit dem Filter die gewünschte suchen.
1. Wählen Sie im Ressourcengruppenmenü **Zugriffssteuerung (IAM)** .
1. Wählen Sie die Registerkarte **Rollenzuweisungen** aus, um die aktuellen Rollenzuweisungen anzuzeigen.
1. Wählen Sie oben auf der Seite die Option **+ Hinzufügen** aus.
1. Führen Sie die Anweisungen zum Hinzufügen der Rolle „Besitzer“ zu Ihrer Azure AD-Anwendung aus. Nachdem Sie die Rolle erfolgreich hinzugefügt haben, wird die Anwendung unter der Rolle „Besitzer“ aufgelistet.

## <a name="develop-an-hdinsight-client-application"></a>Entwickeln einer HDInsight-Clientanwendung

1. Erstellen Sie eine C#-Konsolenanwendung.
2. Fügen Sie die folgenden [NuGet](https://www.nuget.org/)-Pakete hinzu:

    * `Install-Package Microsoft.Azure.Common.Authentication -Pre`
    * `Install-Package Microsoft.Azure.Management.HDInsight -Pre`
    * `Install-Package Microsoft.Azure.Management.Resources -Pre`

3. Führen Sie den folgenden Code aus:

    ```csharp
    using System;
    using System.Security;
    using Microsoft.Azure;
    using Microsoft.Azure.Common.Authentication;
    using Microsoft.Azure.Common.Authentication.Factories;
    using Microsoft.Azure.Common.Authentication.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.HDInsight;
    
    namespace CreateHDICluster
    {
        internal class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
    
            private static Guid SubscriptionId = new Guid("<Enter your Azure subscription ID>");
            private static string tenantID = "<Enter your tenant ID (also called directory ID)>";
            private static string applicationID = "<Enter your application ID>";
            private static string secretKey = "<Enter the application secret key>";
    
            private static void Main(string[] args)
            {
                var key = new SecureString();
                foreach (char c in secretKey) { key.AppendChar(c); }
    
                var tokenCreds = GetTokenCloudCredentials(tenantID, applicationID, key);
                var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
    
                var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
                resourceManagementClient.Providers.Register("Microsoft.HDInsight");
    
                _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
    
                var results = _hdiManagementClient.Clusters.List();
                foreach (var name in results.Clusters)
                {
                    Console.WriteLine("Cluster Name: " + name.Name);
                    Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
                    Console.WriteLine("\t Cluster location: " + name.Location);
                    Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
                }
                Console.WriteLine("Press Enter to continue");
                Console.ReadLine();
            }
    
            /// Get the access token for a service principal and provided key.          
            public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
            {
                var authFactory = new AuthenticationFactory();
                var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
                var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
                var accessToken =
                    authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
    
                return new TokenCloudCredentials(accessToken);
            }
    
            public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
            {
                return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
            }
        }
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../active-directory/develop/howto-create-service-principal-portal.md).
* Weitere Informationen finden Sie unter [Authentifizieren eines Dienstprinzipals mit Azure Resource Manager](../active-directory/develop/howto-authenticate-service-principal-powershell.md).
* Erfahren Sie mehr über die [rollenbasierte Zugriffssteuerung von Azure (Azure RBAC)](../role-based-access-control/role-assignments-portal.md).
