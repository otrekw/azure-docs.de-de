---
title: 'Registrieren einer Peering Service-Verbindung: Azure PowerShell '
description: In diesem Tutorial erfahren Sie, wie Sie mit PowerShell eine Peering Service-Verbindung registrieren.
services: peering-service
author: derekolo
ms.service: peering-service
ms.topic: tutorial
ms.date: 05/18/2020
ms.author: derekol
Customer intent: Customer wants to measure their connection telemetry per prefix to Microsoft services with Azure Peering Service .
ms.openlocfilehash: a12bfa042c8741814cedc70f1dcb67dedbfd331e
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2020
ms.locfileid: "91400433"
---
# <a name="tutorial-register-a-peering-service-connection-using-azure-powershell"></a>Tutorial: Registrieren einer Peering Service-Verbindung mithilfe von Azure PowerShell

In diesem Tutorial erfahren Sie, wie Sie Peering Service mithilfe von Azure PowerShell registrieren.

Azure Peering Service ist ein Netzwerkdienst zur Verbesserung der Kundenkonnektivität mit Microsoft Cloud Services wie Microsoft 365, Dynamics 365, SaaS-Diensten (Software-as-a-Service), Azure und anderen Microsoft-Diensten, auf die über das öffentliche Internet zugegriffen werden kann. In diesem Artikel erfahren Sie, wie Sie eine Peering Service-Verbindung mithilfe von Azure PowerShell registrieren.

Sollten Sie über kein Azure-Abonnement verfügen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie PowerShell stattdessen lokal installieren und verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens Version 1.0.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierte Version zu ermitteln. Informationen zum Installieren und Upgraden finden Sie unter [Installieren von Azure PowerShell](/powershell/azure/install-az-ps).

Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen. Mit diesem Befehl wird eine Verbindung mit Azure hergestellt.

Verwenden Sie das Azure PowerShell-Modul, um Peering Service zu registrieren und zu verwalten. Sie können Peering Service über die PowerShell-Befehlszeile oder in Skripts registrieren/verwalten.


## <a name="prerequisites"></a>Voraussetzungen  
Sie benötigen Folgendes:

### <a name="azure-account"></a>Azure-Konto

Sie müssen über ein gültiges und aktives Microsoft Azure-Konto verfügen. Dieses Konto wird zum Einrichten der Peering Service-Verbindung benötigt. Peering Service ist eine Ressource innerhalb von Azure-Abonnements.

### <a name="connectivity-provider"></a>Konnektivitätsanbieter

Sie können mit einem Internetdienst- oder Internet Exchange-Anbieter zusammenarbeiten, um Peering Service zu erhalten und eine Verbindung zwischen Ihrem Netzwerk und dem Microsoft-Netzwerk herzustellen.

Achten Sie darauf, dass es sich bei den Konnektivitätsanbietern um Microsoft-Partner handelt.

### <a name="register-a-subscription-with-the-resource-provider-and-feature-flag"></a>Registrieren eines Abonnements mit dem Ressourcenanbieter und Featureflag

Registrieren Sie zunächst mithilfe von Azure PowerShell Ihr Abonnement mit dem Ressourcenanbieter und Featureflag, bevor Sie mit der Registrierung von Peering Service beginnen. Die Azure PowerShell-Befehle finden Sie hier:

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName AllowPeeringService ProviderNamespace Microsoft.Peering 

Register-AzResourceProvider -ProviderNamespace Microsoft.Peering 

```

### <a name="fetch-the-location-and-service-provider"></a>Abrufen von Standort und Dienstanbieter 

Führen Sie in Azure PowerShell die folgenden Befehle aus, um den Standort und den Dienstanbieter zu erhalten, für den Peering Service aktiviert werden soll. 

Abrufen von Peering Service-Standorten:

```azurepowershell-interactive
# Gets a list of available countries
Get-AzPeeringServiceCountry 
# Gets a list of metro locations serviced by country
Get-AzPeeringServiceLocation -Country "United States"
```

Abrufen von Peering Service-Anbietern:
              
```azurepowershell-interactive
Get-AzPeeringServiceProvider
```

### <a name="register-the-peering-service-connection"></a>Registrieren der Peering Service-Verbindung

Registrieren Sie die Peering Service-Verbindung mithilfe der folgenden Befehle über Azure PowerShell. In diesem Beispiel wird eine Peering Service-Instanz namens „myPeeringService“ registriert.

```azurepowershell-interactive
$loc = "Washington"
$provider = "TestPeer1"
$resourceGroup = "MyResourceGroup"
$name = “myPeeringService”
$peeringService = New-AzPeeringService -ResourceGroupName $resourceGroup -Name $name -PeeringLocation $loc -PeeringServiceProvider $provider
```

### <a name="register-the-peering-service-prefix"></a>Registrieren des Peering Service-Präfix

Führen Sie die folgenden Befehle über Azure PowerShell aus, um das vom Konnektivitätsanbieter bereitgestellte Präfix zu registrieren. In diesem Beispiel wird das Präfix „myPrefix“ registriert.

```azurepowershell-interactive
$loc = "Washington"
$provider = "TestPeer1"
$resourceGroup = "MyResourceGroup"
$name = “myPeeringService”
$peeringService = New-AzPeeringService -ResourceGroupName $resourceGroup -Name $name -PeeringLocation $loc -PeeringServiceProvider $provider
$prefixName = "myPrefix"
$prefix = “192.168.1.0/24”
$serviceKey = "6f48cdd6-2c2e-4722-af89-47e27b2513af"
$prefixService = $peeringService | New-AzPeeringServicePrefix -Name $prefixName -Prefix $prefix -ServiceKey $serviceKey
```

### <a name="list-all-the-peering-services-connections"></a>Auflisten aller Peering Service-Verbindungen

Führen Sie den folgenden Befehl aus, um eine Liste aller Peering Service-Instanzen anzuzeigen:

```azurepowershell-interactive
$peeringService = Get-AzPeeringService
```

### <a name="list-all-the-peering-service-prefixes"></a>Auflisten aller Peering Service-Präfixe

Führen Sie den folgenden Befehl aus, um eine Liste aller Peering Service-Präfixe anzuzeigen:

```azurepowershell-interactive
 $prefixName = "myPrefix"
```

```azurepowershell-interactive
$prefix = Get-AzPeeringServicePrefix -PeeringServiceName "myPeeringService" -ResourceGroupName "MyResourceGroup" -Name "myPrefix"
```

### <a name="remove-the-peering-service-prefix"></a>Entfernen des Peering Service-Präfix

Führen Sie den folgenden Befehl aus, um das Peering Service-Präfix Service zu entfernen:

```azurepowershell-interactive
Remove-AzPeeringServicePrefix -ResourceGroupName  "MyResourceGroup" -Name "myPrefix" -PeeringServiceName "myPeeringService"
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Peering Service-Verbindung finden Sie unter [Peering Service-Verbindung](connection.md).
- Informationen zur Peering Service-Verbindungstelemetrie finden Sie unter [Peering Service-Verbindungstelemetrie](connection-telemetry.md).
- Informationen zum Registrieren einer Peering Service-Verbindung über das Azure-Portal finden Sie unter [Registrieren von Peering Service über das Azure-Portal](azure-portal.md).
- Informationen zum Registrieren einer Peering Service-Verbindung über die Azure-Befehlszeilenschnittstelle finden Sie unter [Registrieren einer Peering Service-Verbindung über die Azure-Befehlszeilenschnittstelle](cli.md).
