---
title: Hinzufügen eines Dienstprinzipals zur Azure Analysis Services-Administratorrolle | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Automatisierungsdienstprinzipal der Azure Analysis Services-Serveradministratorrolle hinzufügen.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.custom: fasttrack-edit
ms.openlocfilehash: b75740e9bff714ad68c93bea7e387e60da2f1c59
ms.sourcegitcommit: 0eb0673e7dd9ca21525001a1cab6ad1c54f2e929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2020
ms.locfileid: "77212498"
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>Hinzufügen eines Dienstprinzipals zur Serveradministratorrolle 

 Um unbeaufsichtigte PowerShell-Tasks zu automatisieren, muss ein Dienstprinzipal auf dem verwalteten Analysis Services-Server über die Rechte eines **Serveradministrators** verfügen. In diesem Artikel wird beschrieben, wie Sie auf einem Azure AS-Server einen Dienstprinzipal zur Serveradministratorrolle hinzufügen. Sie können dies mithilfe von SQL Server Management Studio oder einer Resource Manager-Vorlage durchführen.
 
> [!NOTE]
> Bei Servervorgängen, für die Azure PowerShell-Cmdlets verwendet werden, muss der Dienstprinzipal außerdem zur Rolle **Besitzer** für die Ressource in der [rollenbasierten Zugriffssteuerung in Azure (Role-Based Access Control, RBAC)](../role-based-access-control/overview.md) gehören. 

## <a name="before-you-begin"></a>Voraussetzungen
Bevor Sie diese Aufgabe ausführen, muss ein Dienstprinzipal in Azure Active Directory registriert sein.

[Erstellen eines Dienstprinzipals – Azure-Portal](../active-directory/develop/howto-create-service-principal-portal.md)   
[Erstellen eines Dienstprinzipals – PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="using-sql-server-management-studio"></a>Verwendung von SQL Server Management Studio

Sie können mit SQL Server Management Studio (SSMS) Serveradministrator konfigurieren. Für die Durchführung dieser Aufgabe benötigen Sie die Rechte eines [Serveradministrators](analysis-services-server-admins.md) auf dem Azure AS-Server. 

1. Stellen Sie in SSMS eine Verbindung mit Ihrem Azure AS-Server her.
2. Klicken Sie unter **Servereigenschaften** > **Sicherheit** auf **Hinzufügen**.
3. Suchen Sie unter **Benutzer oder Gruppe auswählen** anhand des Namens nach Ihrer registrierten App, und klicken Sie dann auf **Hinzufügen**.

    ![Suchen nach dem Dienstprinzipalkonto](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Überprüfen Sie die ID des Dienstprinzipalkontos, und klicken Sie dann auf **OK**.
    
    ![Suchen nach dem Dienstprinzipalkonto](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)

## <a name="using-a-resource-manager-template"></a>Verwenden einer Resource Manager-Vorlage

Sie können Serveradministratoren auch durch Bereitstellen des Analysis Services-Servers mithilfe einer Azure Resource Manager-Vorlage konfigurieren. Die Identität, die die Bereitstellung ausführt, muss zu der Rolle **Mitwirkender** für die Ressource in der [Rollenbasierten Zugriffssteuerung von Azure (Azure Role-Based Access Control, RBAC)](../role-based-access-control/overview.md) gehören.

> [!IMPORTANT]
> Der Dienstprinzipal muss mit dem Format `app:{service-principal-client-id}@{azure-ad-tenant-id}` hinzugefügt werden.

Mit der folgenden Resource Manager-Vorlage wird ein Analysis Services-Server mit einem angegebenen Dienstprinzipal bereitgestellt, der der Rolle „Analysis Services-Administrator“ hinzugefügt wurde:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "analysisServicesServerName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "analysisServicesSkuName": {
            "type": "string"
        },
        "analysisServicesCapacity": {
            "type": "int"
        },
        "servicePrincipalClientId": {
            "type": "string"
        },
        "servicePrincipalTenantId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('analysisServicesServerName')]",
            "type": "Microsoft.AnalysisServices/servers",
            "apiVersion": "2017-08-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('analysisServicesSkuName')]",
                "capacity": "[parameters('analysisServicesCapacity')]"
            },
            "properties": {
                "asAdministrators": {
                    "members": [
                        "[concat('app:', parameters('servicePrincipalClientId'), '@', parameters('servicePrincipalTenantId'))]"
                    ]
                }
            }
        }
    ]
}
```

## <a name="related-information"></a>Verwandte Informationen

* [Herunterladen des SQL Server PowerShell-Moduls](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Herunterladen von SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


