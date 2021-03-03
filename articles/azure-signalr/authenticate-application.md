---
title: Authentifizieren einer Anwendung für den Zugriff auf Azure SignalR Service
description: Dieser Artikel enthält Informationen zur Authentifizierung einer Anwendung mit Azure Active Directory für den Zugriff auf Azure SignalR Service.
author: terencefan
ms.author: tefa
ms.service: signalr
ms.topic: conceptual
ms.date: 08/03/2020
ms.openlocfilehash: 597b69c1180ea1fb2a6812d648f8b8ad37707d66
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2021
ms.locfileid: "101092607"
---
# <a name="authenticate-an-application-with-azure-active-directory-to-access-azure-signalr-service"></a>Authentifizieren einer Anwendung mit Azure Active Directory für den Zugriff auf Azure SignalR Service
Microsoft Azure bietet eine integrierte Verwaltung der Zugriffssteuerung für Ressourcen und Anwendungen, basierend auf Azure Active Directory (Azure AD). Ein wesentlicher Vorteil der Verwendung von Azure AD mit Azure SignalR Service besteht darin, dass Sie Ihre Anmeldeinformationen nicht mehr im Code speichern müssen. Stattdessen können Sie ein OAuth 2.0-Zugriffstoken von der Microsoft Identity Platform anfordern. Der Ressourcenname zum Anfordern eines Tokens ist `https://signalr.azure.com/`. Azure AD übernimmt die Authentifizierung des Sicherheitsprinzipals (Anwendung, Ressourcengruppe oder Dienstprinzipal), der die Anwendung ausführt. Wenn die Authentifizierung erfolgreich ist, gibt Azure AD ein Zugriffstoken an die Anwendung zurück, und die Anwendung kann dann das Zugriffstoken zum Autorisieren von Anforderungen an Azure SignalR Service-Ressourcen verwenden.

Wenn einem Azure AD-Sicherheitsprinzipal eine Rolle zugewiesen wird, gewährt Azure diesem Sicherheitsprinzipal Zugriff auf diese Ressourcen. Der Zugriff kann sich auf den Bereich des Abonnements, der Ressourcengruppe oder der Azure SignalR Service-Ressource beziehen. Ein Azure AD-Sicherheitsprinzipal kann einem Benutzer, einer Gruppe, einem Anwendungsdienstprinzipal oder einer [verwalteten Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) Rollen zuweisen. 

> [!NOTE]
> Eine Rollendefinition ist eine Sammlung von Berechtigungen. Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) kontrolliert, wie diese Berechtigungen mittels Rollenzuweisung erzwungen werden. Eine Rollenzuweisung besteht aus drei Elementen: Sicherheitsprinzipal, Rollendefinition und Bereich. Weitere Informationen finden Sie unter [Grundlegendes zu den verschiedenen Rollen](../role-based-access-control/overview.md).

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Registrieren der Anwendung bei einem Azure AD-Mandanten
Der erste Schritt bei der Verwendung von Azure AD zum Autorisieren von Azure SignalR Service-Ressourcen ist die Registrierung Ihrer Anwendung mit einem Azure AD-Mandanten im [Azure-Portal](https://portal.azure.com/). Wenn Sie Ihre Anwendung registrieren, stellen Sie Azure AD Informationen zu Ihrer Anwendung bereit. Azure AD stellt dann eine Client-ID (auch als Anwendungs-ID bezeichnet) bereit, mit der Sie Ihre Anwendung zur Laufzeit Azure AD zuordnen können. Weitere Informationen zur Client-ID finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory (Azure AD)](../active-directory/develop/app-objects-and-service-principals.md). 

Die folgenden Abbildungen zeigen Schritte zum Registrieren einer Webanwendung:

![Registrieren einer Anwendung](./media/authenticate/app-registrations-register.png)

> [!Note]
> Wenn Sie Ihre Anwendung als native Anwendung registrieren, können Sie jeden gültigen URI als Umleitungs-URI angeben. Bei nativen Anwendungen muss dieser Wert keine echte URL sein. Bei Webanwendungen muss der Umleitungs-URI ein gültiger URI sein, da er die URL definiert, für die Token bereitgestellt werden.

Nachdem Sie Ihre Anwendung registriert haben, wird die **Anwendungs-ID (Client-ID)** unter **Einstellungen** angezeigt:

![Anwendungs-ID der registrierten Anwendung](./media/authenticate/application-id.png)

Ausführlichere Informationen zum Registrieren einer Anwendung bei Azure AD finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](../active-directory/develop/quickstart-register-app.md).


### <a name="create-a-client-secret"></a>Erstellen eines Clientgeheimnisses   
Die Anwendung benötigt zum Beweis ihrer Identität einen geheimen Clientschlüssel, wenn sie ein Token anfordert. Führen Sie folgende Schritte aus, um den geheimen Clientschlüssel hinzuzufügen.

1. Navigieren Sie im Azure-Portal zu Ihrer App-Registrierung.
1. Klicken Sie auf die Einstellung **Certificates & secrets** (Zertifikate & Geheime Schlüssel).
1. Wählen Sie unter **Geheime Clientschlüssel** die Option **Neuer geheimer Clientschlüssel** aus, um einen neuen geheimen Schlüssel zu erstellen.
1. Geben Sie eine Beschreibung für den geheimen Schlüssel an, und wählen Sie das gewünschte Ablaufintervall aus.
1. Kopieren Sie den Wert des neuen geheimen Schlüssels sofort, und legen Sie die Kopie an einem sicheren Speicherort ab. Der Wert wird Ihnen nur ein Mal angezeigt.

![Erstellen eines geheimen Clientschlüssels](./media/authenticate/client-secret.png)

### <a name="upload-a-certificate"></a>Hochladen eines Zertifikats

Sie können auch ein Zertifikat hochladen, anstatt einen geheimen Clientschlüssel zu erstellen.

![Hochladen eines Zertifikats](./media/authenticate/certification.png)

## <a name="assign-azure-roles-using-the-azure-portal"></a>Zuweisen von Azure-Rollen über das Azure-Portal  
Weitere Informationen zum Verwalten des Zugriffs auf Azure-Ressourcen mithilfe der Azure RBAC und des Azure-Portals finden Sie in [diesem Artikel](..//role-based-access-control/role-assignments-portal.md). 

Nachdem Sie den gewünschten Bereich für eine Rollenzuweisung festgelegt haben, navigieren Sie im Azure-Portal zur entsprechenden Ressource. Zeigen Sie die Einstellungen für die Zugriffssteuerung (IAM) für die Ressource an, und befolgen Sie diese Anweisungen zum Verwalten von Rollenzuweisungen:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrer SignalR-Ressource.
1. Wählen Sie **Zugriffssteuerung (IAM)** aus, um Zugriffssteuerungseinstellungen für Azure SignalR Service anzuzeigen. 
1. Wählen Sie die Registerkarte **Rollenzuweisungen** aus, um die Liste mit den Rollenzuweisungen anzuzeigen. Wählen Sie die Schaltfläche **Hinzufügen** auf der Symbolleiste und dann **Rollenzuweisung hinzufügen** aus. 

    ![Schaltfläche „Hinzufügen“ auf der Symbolleiste](./media/authenticate/role-assignments-add-button.png)

1. Führen Sie auf der Seite **Rollenzuweisung hinzufügen** die folgenden Schritte aus:
    1. Wählen Sie die **Azure SignalR Service-Rolle** aus, die Sie zuweisen möchten. 
    1. Suchen Sie nach dem **Sicherheitsprinzipal** (Benutzer, Gruppe, Dienstprinzipal), dem Sie die Rolle zuweisen möchten.
    1. Klicken Sie auf **Speichern**, um die Rollenzuweisung zu speichern. 

        ![Zuweisen einer Rolle zu einer Anwendung](./media/authenticate/assign-role-to-application.png)

    1. Die Identität, der Sie die Rolle zugewiesen haben, wird unter dieser Rolle angezeigt. Die folgende Abbildung zeigt z. B., dass die Anwendungen `signalr-dev` und `signalr-service` die Rolle „SignalR-App-Server“ besitzen. 
        
        ![Rollenzuweisungsliste](./media/authenticate/role-assignment-list.png)

Sie können ähnliche Schritte ausführen, um eine Rolle zuzuweisen, die für eine Ressourcengruppe oder ein Abonnement gilt. Nachdem Sie die Rolle und ihren Bereich definiert haben, können Sie dieses Verhalten mit den Beispielen [an diesem GitHub-Speicherort](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac) testen.

## <a name="sample-codes-while-configuring-your-app-server"></a>Codebeispiele für das Konfigurieren des App-Servers

Fügen Sie bei Verwendung von `AddAzureSignalR` folgende Optionen hinzu:

```C#
services.AddSignalR().AddAzureSignalR(option =>
{
    option.ConnectionString = "Endpoint=https://<name>.signalr.net;AuthType=aad;clientId=<clientId>;clientSecret=<clientSecret>;tenantId=<tenantId>";
});
```

Oder konfigurieren Sie einfach die `ConnectionString` in Ihrer Datei `appsettings.json`:

```json
{
"Azure": {
    "SignalR": {
      "Enabled": true,
      "ConnectionString": "Endpoint=https://<name>.signalr.net;AuthType=aad;clientId=<clientId>;clientSecret=<clientSecret>;tenantId=<tenantId>"
    }
  },
}
```

Wenn Sie `certificate` verwenden, ändern Sie `clientSecret` wie folgt in `clientCert`:

```C#
    option.ConnectionString = "Endpoint=https://<name>.signalr.net;AuthType=aad;clientId=<clientId>;clientCert=<clientCertFilepath>;tenantId=<tenantId>";
```

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie in folgendem Artikel: [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../role-based-access-control/overview.md)
- Informationen zum Zuweisen und Verwalten von Azure-Rollenzuweisungen mit Azure PowerShell, der Azure-Befehlszeilenschnittstelle oder der REST-API finden Sie in diesen Artikeln:
    - [Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell](../role-based-access-control/role-assignments-powershell.md)  
    - [Verwalten der rollenbasierten Zugriffssteuerung mit der Azure-Befehlszeilenschnittstelle](../role-based-access-control/role-assignments-cli.md)
    - [Verwalten der rollenbasierten Zugriffssteuerung mit der REST-API](../role-based-access-control/role-assignments-rest.md)
    - [Verwalten der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) mit Azure Resource Manager-Vorlagen](../role-based-access-control/role-assignments-template.md)

Weitere Informationen finden Sie in den folgenden verwandten Artikeln:
- [Authentifizieren einer verwalteten Identität mit Azure Active Directory für den Zugriff auf Azure SignalR Service](authenticate-managed-identity.md)
- [Autorisieren des Zugriffs auf Azure SignalR Service mit Azure Active Directory](authorize-access-azure-active-directory.md)