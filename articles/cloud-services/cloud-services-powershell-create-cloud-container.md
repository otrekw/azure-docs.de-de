---
title: Erstellen eines Clouddienstcontainers mit PowerShell | Microsoft-Dokumentation
description: Dieser Artikel beschreibt das Erstellen eines Clouddienstcontainers mit PowerShell. Der Container hostet Web- und Workerrollen.
services: cloud-services
documentationcenter: .net
author: cawaMS
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: d40a5b64cc8018f45bf08158ce808b2baae27962
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87049085"
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Verwenden des Azure PowerShell-Befehls zum Erstellen eines leeren Clouddienstcontainers

Dieser Artikel erläutert das schnelle Erstellen eines Cloud Services-Containers mithilfe von Azure PowerShell-Cmdlets. Führen Sie die folgenden Schritte aus:

1. Installieren Sie das Microsoft Azure PowerShell-Cmdlet über die [Azure PowerShell-Downloadseite](https://aka.ms/webpi-azps) .
2. Öffnen Sie die PowerShell-Eingabeaufforderung.
3. Melden Sie sich mithilfe von [Add-AzureAccount](/powershell/module/servicemanagement/azure.service/add-azureaccount?view=azuresmps-4.0.0) an.

   > [!NOTE]
   > Weitere Anweisungen zum Installieren des Azure PowerShell-Cmdlets und zum Herstellen einer Verbindung zu Ihrem Azure-Abonnement finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/).
   >
   >
4. Verwenden Sie das **New-AzureService** -Cmdlet zum Erstellen eines leeren Clouddienstcontainers.

   ```
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```

5. Folgen Sie diesem Beispiel, um das Cmdlet aufzurufen:

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Führen Sie Folgendes aus, um weitere Informationen zum Erstellen eines Azure-Clouddiensts zu erhalten:

```powershell
Get-help New-AzureService
```

### <a name="next-steps"></a>Nächste Schritte

* Informationen zur Verwaltung der Clouddienstbereitstellung erhalten Sie unter den Befehlen [Get-AzureService](/powershell/module/servicemanagement/azure.service/Get-AzureService?view=azuresmps-4.0.0), [Remove-AzureService](/powershell/module/servicemanagement/azure.service/Remove-AzureService?view=azuresmps-4.0.0) und [Set-AzureService](/powershell/module/servicemanagement/azure.service/set-azureservice?view=azuresmps-4.0.0). Weitere Informationen finden Sie auch unter [Konfigurieren von Clouddiensten](cloud-services-how-to-configure-portal.md) .
* Um Ihr Clouddienstprojekt in Azure zu veröffentlichen, nutzen Sie das Codebeispiel **PublishCloudService.ps1** aus dem [archivierten Cloud Services-Repository](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery).