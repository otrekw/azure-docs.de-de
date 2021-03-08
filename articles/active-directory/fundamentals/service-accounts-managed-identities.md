---
title: Sichern von verwalteten Identitäten in Azure Active Directory
description: Erläuterung zum Ermitteln, Bewerten und Erhöhen der Sicherheit verwalteter Identitäten.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 3/1/2021
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88a7600239d6e960fa2e635c9e7d9049a7c02db3
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102032353"
---
# <a name="securing-managed-identities"></a>Sichern von verwalteten Identitäten

Die Verwaltung von Geheimnissen und Anmeldeinformationen für eine sichere Kommunikation zwischen verschiedenen Diensten stellt für Entwickler häufig eine Herausforderung dar. Verwaltete Identitäten sind sichere Azure AD-Identitäten (Azure Active Directory), die für die Bereitstellung von Identitäten für Azure-Ressourcen erstellt wurden.

## <a name="benefits-of-using-managed-identities-for-azure-resources"></a>Vorteile der Verwendung von verwalteten Identitäten für Azure-Ressourcen

Die Verwendung von verwalteten Identitäten bietet folgende Vorteile:

* Sie brauchen keine Anmeldeinformationen zu verwalten. Mit verwalteten Identitäten werden Anmeldeinformationen vollständig verwaltet, rotiert und durch Azure geschützt. Identitäten werden mit Azure-Ressourcen automatisch bereitgestellt und gelöscht. Verwaltete Identitäten ermöglichen Azure-Ressourcen die Kommunikation mit allen Diensten, die die Azure AD-Authentifizierung unterstützen.

* Niemand (einschließlich eines globalen Administrators) hat Zugriff auf die Anmeldeinformationen, sodass sie nicht versehentlich kompromittiert, z. B. in Code einbezogen werden können.

## <a name="when-to-use-managed-identities"></a>Wann sollten verwaltete Identitäten verwendet werden?

Verwaltete Identitäten werden am besten für die Kommunikation zwischen Diensten verwendet, die die Azure AD-Authentifizierung unterstützen. 

Ein Quellsystem fordert den Zugriff auf einen Zieldienst an. Jede Azure-Ressource kann ein Quellsystem sein. Beispielsweise unterstützen eine Azure-VM, eine Azure Functions-Instanz und Azure App Service-Instanzen verwaltete Identitäten.

[!VIDEO https://www.youtube.com/embed/5lqayO_oeEo]

### <a name="how-authentication-and-authorization-work"></a>Die Funktionsweise von Authentifizierung und Autorisierung

Mit verwalteten Identitäten kann das Quellsystem ein Token von Azure AD abrufen, ohne dass der Quellbesitzer Anmeldeinformationen verwalten muss. Azure verwaltet die Anmeldeinformationen. Das Token, das vom Quellsystem abgerufen wird, wird dem Zielsystem zur Authentifizierung angezeigt. 

Das Zielsystem muss das Quellsystem authentifizieren (identifizieren) und autorisieren, bevor der Zugriff zugelassen wird. Wenn der Zieldienst die auf Azure AD basierende Authentifizierung unterstützt, akzeptiert er ein von Azure AD ausgestelltes Zugriffstoken. 

Azure verfügt über eine Steuerungsebene und eine Datenebene. Auf der Steuerungsebene erstellen Sie Ressourcen, und auf der Datenebene greifen Sie darauf zu. Beispielsweise erstellen Sie eine Cosmos-Datenbank auf der Steuerungsebene, fragen sie jedoch auf der Datenebene ab.

Nachdem das Zielsystem das Token für die Authentifizierung akzeptiert hat, kann es verschiedene Autorisierungsmechanismen für die Steuerungsebene und die Datenebene unterstützen.

Alle Vorgänge der Azure-Steuerungsebene werden vom [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) verwaltet und verwenden die [rollenbasierte Zugriffssteuerung in Azure](https://docs.microsoft.com/azure/role-based-access-control/overview). In der Datenebene verfügt jedes Zielsystem über einen eigenen Autorisierungsmechanismus. Azure Storage unterstützt Azure RBAC auf der Datenebene. Beispielsweise können Anwendungen, die Azure App Service verwenden, Daten aus Azure Storage lesen, und Anwendungen, die Azure Kubernetes Service verwenden, können in Azure Key Vault gespeicherte Geheimnisse lesen.

Weitere Informationen zu Steuerungs- und Datenebenen finden Sie unter [Vorgänge auf Steuerungsebene und auf Datenebene – Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/control-plane-and-data-plane).

Alle Azure-Dienste unterstützen letztendlich verwaltete Identitäten. Weitere Informationen finden Sie unter [Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities).

##  

## <a name="types-of-managed-identities"></a>Typen verwalteter Identitäten

Es gibt zwei Arten von verwalteten Identitäten: systemseitig und benutzerseitig zugewiesene Identitäten.

Systemseitig zugewiesene verwaltete Identitäten besitzen folgende Eigenschaften:

* Sie steht mit der Azure-Ressource in einer 1:1-Beziehung. Beispielsweise ist jeder VM eine eindeutige verwaltete Identität zugeordnet.

* Sie ist an den Lebenszyklus von Azure-Ressourcen gebunden. Wenn die Ressource gelöscht wird, wird die zugeordnete verwaltete Identität automatisch gelöscht, um das mit verwaisten Konten verbundene Risiko zu vermeiden. 

Benutzerseitig zugewiesene verwaltete Identitäten besitzen folgende Eigenschaften:

* Der Lebenszyklus dieser Identitäten ist unabhängig von einer Azure-Ressource, und Sie müssen den Lebenszyklus verwalten. Wenn die Azure-Ressource gelöscht wird, wird die zugeordnete benutzerseitig zugewiesene verwaltete Identität nicht automatisch für Sie gelöscht.

* Eine einzelne benutzerseitig zugewiesene verwaltete Identität kann null (0) oder mehr Azure-Ressourcen zugewiesen werden.

* Sie kann im Voraus erstellt und dann einer Ressource zugewiesen werden.

## <a name="find-managed-identity-service-principals-in-azure-ad"></a>Suchen von Dienstprinzipalen verwalteter Identitäten in Azure AD

Es gibt mehrere Möglichkeiten, verwaltete Identitäten zu finden:

* Verwenden der Seite „Unternehmensanwendungen“ im Azure-Portal

* Verwenden von Microsoft Graph

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

1. Wählen Sie in Azure AD die Option „Unternehmensanwendung“ aus.

2. Wählen Sie den Filter für „Verwaltete Identitäten“ aus. 

   ![Bild des Bildschirms aller Anwendungen mit der Dropdownliste „Anwendungstyp“, wobei „Verwaltete Identitäten“ hervorgehoben ist.](./media/securing-service-accounts/service-accounts-managed-identities.png)

 

### <a name="using-microsoft-graph"></a>Verwenden von Microsoft Graph

Sie können eine Liste aller verwalteten Identitäten in Ihrem Mandanten erhalten, wenn Sie folgende GET-Anforderung an Microsoft Graph richten:

`https://graph.microsoft.com/v1.0/servicePrincipals?$filter=(servicePrincipalType eq 'ManagedIdentity') `

Sie können diese Anforderungen filtern. Weitere Informationen finden Sie in der Graph-Dokumentation [Abrufen des Dienstprinzipals](/graph/api/serviceprincipal-get?view=).

## <a name="assess-the-security-of-managed-identities"></a>Bewerten der Sicherheit verwalteter Identitäten 

Es gibt folgende Möglichkeiten, die Sicherheit verwalteter Identitäten zu bewerten:

* Überprüfen Sie die Berechtigungen, und stellen Sie sicher, dass das Modell mit den geringsten Rechten ausgewählt wird. Verwenden Sie das folgende PowerShell-Cmdlet, um die Berechtigungen abzurufen, die den verwalteten Identitäten zugewiesen sind.

   ` Get-AzureADServicePrincipal | % { Get-AzureADServiceAppRoleAssignment -ObjectId $_ }`

 
* Stellen Sie sicher, dass die verwaltete Identität nicht Teil einer privilegierten Gruppe ist, z. B. einer Administratorengruppe.  
Hierzu können Sie die Mitglieder ihrer stark privilegierten Gruppen mit PowerShell auflisten.

   `Get-AzureADGroupMember -ObjectId <String> [-All <Boolean>] [-Top <Int32>] [<CommonParameters>]`

* [Ermitteln Sie, auf welche Ressourcen die verwaltete Identität zugreift](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-list-powershell).

## <a name="move-to-managed-identities"></a>Wechseln zu verwalteten Identitäten

Wenn Sie einen Dienstprinzipal oder ein Azure AD-Benutzerkonto verwenden, sollten Sie auswerten, ob Sie stattdessen eine verwaltete Identität verwenden können, damit Sie Anmeldeinformationen nicht schützen, rotieren und verwalten müssen. 

## <a name="next-steps"></a>Nächste Schritte

**Informationen zum Erstellen von verwalteten Identitäten finden Sie unter:** 

[Erstellen einer benutzerseitig zugewiesenen verwalteten Identität](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal). 

[Aktivieren einer vom System zugewiesenen verwalteten Identität beim Erstellen einer Ressource](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm)

[Aktivieren einer vom System zugewiesenen verwalteten Identität auf einer vorhandenen Ressource](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm)

**Weitere Informationen zu Dienstkonten finden Sie unter:**

[Einführung in Active Directory-Dienstkonten](service-accounts-introduction-azure.md)

[Sichern von Dienstprinzipalen](service-accounts-principal.md)

[Steuern von Azure-Dienstkonten](service-accounts-governing-azure.md)

[Einführung in lokale Dienstkonten](service-accounts-on-premises.md)

 

 

 
