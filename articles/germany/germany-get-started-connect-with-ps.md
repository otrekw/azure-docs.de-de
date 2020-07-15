---
title: Herstellen einer Verbindung mit Azure Deutschland über PowerShell | Microsoft-Dokumentation
description: Informationen zum Verwalten Ihres Abonnements in Azure Deutschland über PowerShell
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2019
ms.author: ralfwi
ms.openlocfilehash: 5fd3e3f46ded877f9b22499aa5400e6b113d8257
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86206420"
---
# <a name="connect-to-azure-germany-by-using-powershell"></a>Herstellen einer Verbindung mit Azure Deutschland über PowerShell

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/europe/2018/08/31/microsoft-to-deliver-cloud-services-from-new-datacentres-in-germany-in-2019-to-meet-evolving-customer-needs/) haben wir keine neuen Kunden akzeptiert und keine neuen Features und Dienste an den ursprünglichen Microsoft Cloud Deutschland-Standorten bereitgestellt.
>
> Aufgrund der Weiterentwicklung der Kundenbedürfnisse haben wir vor Kurzem zwei neue Rechenzentrumsregionen in Deutschland [gestartet](https://azure.microsoft.com/blog/microsoft-azure-available-from-new-cloud-regions-in-germany/), die Datenresidenz für Kundendaten, umfassende Konnektivität mit dem globalen Cloudnetzwerk von Microsoft sowie wettbewerbsfähige Preise bieten. 
>
> Profitieren Sie von der Vielfalt der Funktionen, Sicherheit auf Unternehmensniveau und den umfangreichen Features, die in unseren neuen deutschen Rechenzentrumsregionen zur Verfügung stehen, und [migrieren](germany-migration-main.md) Sie noch heute.

Zur Verwendung von Azure PowerShell mit Azure Deutschland müssen Sie anstelle einer Verbindung mit der globalen Azure-Umgebung eine Verbindung mit Azure Deutschland herstellen. Sie können Azure PowerShell zum Verwalten eines umfangreichen Abonnements über Skripts oder für den Zugriff auf Features verwenden, die derzeit im Azure-Portal nicht verfügbar sind. Wenn Sie PowerShell bereits in der globalen Azure-Umgebung verwendet haben, ist dies größtenteils identisch. In Azure Deutschland bestehen folgende Unterschiede:

* Herstellen der Verbindung mit Ihrem Konto
* Regionsnamen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!NOTE]
> Wenn Sie PowerShell noch nicht verwendet haben, lesen Sie zunächst die [Einführung in Azure PowerShell](/powershell/azure/overview).

Wenn Sie PowerShell starten, müssen Sie durch Angeben eines Umgebungsparameters festlegen, dass eine Verbindung mit Azure Deutschland hergestellt wird. Mit dem Parameter wird sichergestellt, dass PowerShell eine Verbindung mit der richtigen Umgebung herstellt. Die Sammlung von Endpunkten wird bestimmt, wenn Sie eine Verbindung mit Ihrem Konto herstellen. Für unterschiedliche APIs sind verschiedene Varianten des Umgebungsparameters erforderlich:

| Verbindungstyp | Get-Help |
| --- | --- |
| Befehle für [Azure (klassisches Bereitstellungsmodell)](/powershell/azure) |`Add-AzureAccount -Environment AzureGermanCloud` |
| Befehle für [Azure (Resource Manager-Bereitstellungsmodell)](/powershell/azure) |`Connect-AzAccount -EnvironmentName AzureGermanCloud` |
| Befehle für [Azure Active Directory (klassisches Bereitstellungsmodell)](/previous-versions/azure/jj151815(v=azure.100)) |`Connect-MsolService -AzureEnvironment AzureGermanyCloud` |
| Befehle für [Azure Active Directory (Resource Manager-Bereitstellungsmodell)](https://msdn.microsoft.com/library/azure/mt757189.aspx) |`Connect-AzureAD -AzureEnvironmentName AzureGermanyCloud` |

Sie können auch die Option `Environment` verwenden, wenn Sie eine Verbindung mit einem Speicherkonto über `New-AzStorageContext` herstellen, und `AzureGermanCloud` angeben.

## <a name="determining-region"></a>Festlegen der Region
Nach erfolgreicher Verbindungsherstellung besteht ein weiterer Unterschied in den verfügbaren Zielregionen für einen Dienst. Jeder Cloud Service von Azure verfügt über unterschiedliche Regionen. Diese sind auf der Seite für die Dienstverfügbarkeit aufgeführt. Normalerweise verwenden Sie die Region im Parameter `Location` für einen Befehl.


| Allgemeiner Name | `Display name` | Standortname |
| --- | --- | --- |
| Deutschland, Mitte |`Germany Central` | `germanycentral` |
| Deutschland, Nordosten |`Germany Northeast` | `germanynortheast` |


> [!NOTE]
> Wie bei PowerShell für die globale Azure-Umgebung können Sie entweder den Anzeigenamen oder den Standortnamen für den `Location`-Parameter verwenden.
>
>

Wenn Sie die verfügbaren Regionen in Azure Deutschland überprüfen möchten, können Sie den folgenden Befehl ausführen und die aktuelle Liste ausgeben. Verwenden Sie für klassische Bereitstellungen den ersten Befehl. Verwenden Sie für Resource Manager-Bereitstellungen den zweiten Befehl.

```azurepowershell
Get-AzureLocation
Get-AzLocation
```

Wenn Sie sich für die in Azure verfügbaren Umgebungen interessieren, können Sie den folgenden Befehl ausführen:

```azurepowershell
Get-AzureEnvironment
Get-AzEnvironment
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Herstellen der Verbindung mit Azure Deutschland finden Sie in den folgenden Ressourcen:

* [Herstellen einer Verbindung mit Azure Deutschland über die Azure CLI](./germany-get-started-connect-with-cli.md)
* [Herstellen einer Verbindung mit Azure Deutschland über Visual Studio](./germany-get-started-connect-with-vs.md)
* [Herstellen einer Verbindung mit Azure Deutschland über das Azure-Portal](./germany-get-started-connect-with-portal.md)




