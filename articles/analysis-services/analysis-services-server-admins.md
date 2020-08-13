---
title: Verwalten von Serveradministratoren in Azure Analysis Services | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Serveradministratoren für einen Azure Analysis Services-Server mithilfe des Azure-Portals bzw. mit PowerShell- oder REST-APIs verwalten können.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: bc17d27837d5b96f06b5172fb019db873418db94
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87922947"
---
# <a name="manage-server-administrators"></a>Verwalten von Serveradministratoren

Serveradministratoren müssen ein gültiger Benutzer, ein Dienstprinzipal oder eine Sicherheitsgruppe in Azure Active Directory (Azure AD) für den Mandanten sein, in dem sich der Server befindet. **Analysis Services-Administratoren** für Ihren Server können im Azure-Portal, in Servereigenschaften in SSMS, in PowerShell oder in der REST-API verwendet werden, um Serveradministratoren zu verwalten. 

Verwenden Sie beim Hinzufügen einer **Sicherheitsgruppe** `obj:groupid@tenantid`. Dienstprinzipale werden nicht in Sicherheitsgruppen unterstützt, die der Serveradministratorrolle hinzugefügt sind.

## <a name="to-add-server-administrators-by-using-azure-portal"></a>So fügen Sie Serveradministratoren über das Azure-Portal hinzu

1. Klicken Sie im Portal im Bereich für Ihren Server auf **Analysis Services-Administratoren**.
2. Klicken Sie unter **\<servername> – Analysis Services-Administratoren** auf **Hinzufügen**.
3. Wählen Sie unter **Serveradministratoren hinzufügen** Benutzerkonten aus Ihrem Azure AD-Verzeichnis aus, oder laden Sie externe Benutzer per E-Mail ein.

    ![Serveradministratoren im Azure-Portal](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>So fügen Sie Serveradministratoren über SSMS hinzu

1. Klicken Sie mit der rechten Maustaste auf den Server, und wählen Sie dann **Eigenschaften** aus.
2. Klicken Sie in **Eigenschaften für Analysis-Server** auf **Sicherheit**.
3. Klicken Sie auf **Hinzufügen**, und geben Sie dann die E-Mail-Adresse eines Benutzers oder einer Gruppe in Ihrem Azure AD ein.
   
    ![Hinzufügen von Serveradministratoren in SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Verwenden Sie das Cmdlet [New-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/new-azanalysisservicesserver), um den Administrator-Parameter beim Erstellen eines neuen Servers anzugeben. <br>
Verwenden Sie das Cmdlet [Set-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver), um den Administrator-Parameter für einen bereits vorhandenen Server zu ändern.

## <a name="rest-api"></a>REST-API

Verwenden Sie [Create](https://docs.microsoft.com/rest/api/analysisservices/servers/create), um die Eigenschaft „asAdministrator“ beim Erstellen eines neuen Servers anzugeben. <br>
Verwenden Sie [Update](https://docs.microsoft.com/rest/api/analysisservices/servers/update), um die Eigenschaft „asAdministrator“ beim Ändern eines bereits vorhandenen Servers anzugeben. <br>



## <a name="next-steps"></a>Nächste Schritte 

[Authentifizierung und Benutzerberechtigungen](analysis-services-manage-users.md)  
[Verwalten von Datenbankrollen und Benutzern](analysis-services-database-users.md)  
[Rollenbasierte Zugriffssteuerung von Azure (Azure-RBAC)](../role-based-access-control/overview.md)  
