---
title: Automatische Registrierung beim SQL-VM-Ressourcenanbieter
description: Erfahren Sie, wie Sie im Azure-Portal die automatische Registrierungsfunktion aktivieren, um alle früheren und zukünftigen SQL Server-VMs beim SQL-VM-Ressourcenanbieter zu registrieren.
author: MashaMSFT
ms.author: mathoma
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 09/21/2020
ms.openlocfilehash: b986832e5febbb2a0f88b65213f9acf0dd4c5ab5
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91996890"
---
# <a name="automatic-registration-with-sql-vm-resource-provider"></a>Automatische Registrierung beim SQL-VM-Ressourcenanbieter
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Aktivieren Sie die automatische Registrierungsfunktion im Azure-Portal, um alle aktuellen und zukünftigen SQL Server auf Azure-VMs automatisch beim SQL-VM-Ressourcenanbieter im Modus „Lightweight“ zu registrieren.

In diesem Artikel erfahren Sie, wie Sie die automatische Registrierungsfunktion aktivieren. Alternativ können Sie [eine einzelne VM](sql-vm-resource-provider-register.md) oder [mehrere Ihrer VMs](sql-vm-resource-provider-bulk-register.md) beim SQL-VM-Ressourcenanbieter registrieren. 

## <a name="overview"></a>Übersicht

Mit dem [SQL-VM-Ressourcenanbieter](sql-vm-resource-provider-register.md#overview) können Sie Ihre SQL Server-VMs über das Azure-Portal verwalten. Zusätzlich bietet der Ressourcenanbieter einen robusten Funktionssatz, wozu [automatisiertes Patchen](automated-patching.md) und [automatisierte Sicherung](automated-backup.md) sowie Überwachungs- und Verwaltungsfunktionen gehören. Außerdem erhalten Sie mehr Flexibilität bei der [Lizenzierung](licensing-model-azure-hybrid-benefit-ahb-change.md) und bei der Auswahl der [Edition](change-sql-server-edition.md). Zuvor waren diese Funktionen nur für SQL Server-VM-Images verfügbar, die aus dem Azure Marketplace bereitgestellt wurden. 

Die automatische Registrierungsfunktion versetzt Kunden in die Lage, alle aktuellen und zukünftigen SQL Server in ihren Azure-Abonnements beim SQL-VM-Ressourcenanbieter automatisch zu registrieren. Dies unterscheidet sich von der manuellen Registrierung, bei es nur um die aktuellen SQL Server-VMs geht. 

Bei der automatischen Registrierung werden Ihre SQL Server-VMs im Modus „Lightweight“ registriert. Sie müssen weiterhin [manuell auf den Modus „Vollständige Verwaltbarkeit“ aktualisieren](sql-vm-resource-provider-register.md#upgrade-to-full), um den vollständigen Fuktionssatz nutzen zu können. 

## <a name="prerequisites"></a>Voraussetzungen

Zum Registrieren der SQL Server-VM beim Ressourcenanbieter benötigen Sie: 

- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- Ein in der öffentlichen Cloud oder Azure Government-Cloud bereitgestelltes Azure-Ressourcenmodell vom Typ [Windows-VM](../../../virtual-machines/windows/quick-create-portal.md) mit [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads). 


## <a name="enable"></a>Aktivieren

Führen Sie die folgenden Schritte aus, um im Azure-Portal die automatische Registrierung Ihrer SQL Server-VMs zu aktivieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zur Ressourcenseite [**Virtuelle SQL-Computer**](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines). 
1. Wählen Sie **Automatische SQL Server-VM-Registrierung** aus, um die Seite **Automatische Registrierung** zu öffnen. 

   :::image type="content" source="media/sql-vm-resource-provider-automatic-registration/automatic-registration.png" alt-text="„Automatische SQL Server-VM-Registrierung“ auswählen, um die Seite „Automatische Registrierung“ zu öffnen":::

1. Wählen Sie im Dropdownmenü Ihr Abonnement aus. 
1. Lesen Sie die Nutzungsbedingungen, und wählen Sie, wenn Sie zustimmen, **Ich stimme zu** aus. 
1. Wählen Sie **Registrieren** aus, um die Funktion zu aktivieren und alle aktuellen und zukünftigen SQL Server-VMs automatisch beim SQL-VM-Ressourcenanbieter zu registrieren. Dadurch wird der SQL Server-Dienst auf keiner der VMs neu gestartet. 

## <a name="disable"></a>Deaktivieren

Verwenden Sie die [Azure CLI](/cli/azure/install-azure-cli) oder [Azure PowerShell](/powershell/azure/install-az-ps), um die automatische Registrierungsfunktion zu deaktivieren. Wenn die automatische Registrierungsfunktion deaktiviert ist, müssen SQL Server-VMS, die dem Abonnement hinzugefügt werden, manuell beim SQL-VM-Ressourcenanbieter registriert werden. Eine Deaktivierung der Funktion führt nicht dazu, dass die Registrierung vorhandener SQL Server-VMs aufgehoben wird, die bereits registriert wurden.



# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Führen Sie den folgenden Befehl aus, um die automatische Registrierung mit Azure CLI zu deaktivieren: 

```azurecli-interactive
az feature unregister --namespace Microsoft.SqlVirtualMachine --name BulkRegistration
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Führen Sie den folgenden Befehl aus, um die automatische Registrierung mit Azure PowerShell zu deaktivieren: 

```powershell-interactive
Unregister-AzProviderFeature -FeatureName BulkRegistration -ProviderNamespace Microsoft.SqlVirtualMachine
```

---

## <a name="enable-for-multiple-subscriptions"></a>Aktivieren für mehrere Abonnements

Sie können die automatische Registrierungsfunktion mit PowerShell für mehrere Azure-Abonnements aktivieren. 

Gehen Sie dazu folgendermaßen vor:

1. Speichern Sie [dieses Skript](https://github.com/microsoft/tigertoolbox/blob/master/AzureSQLVM/RegisterSubscriptionsToSqlVmAutomaticRegistration.ps1) in einer `.ps1`-Datei, z. B. `EnableBySubscription.ps1`. 
1. Navigieren Sie in einem administrativen Eingabeaufforderungs- oder PowerShell-Fenster zu dem Speicherort, in dem Sie das Skript gespeichert haben. 
1. Stellen Sie eine Verbindung mit Azure her `az login`.
1. Führen Sie das Skript aus, wobei Sie die Abonnement-IDs (SubscriptionIds) wie folgt als Parameter übergeben:   
   `.\EnableBySubscription.ps1 -SubscriptionList SubscriptionId1,SubscriptionId2`

   Beispiel: 

   ```console
   .\EnableBySubscription.ps1 -SubscriptionList a1a1a-aa11-11aa-a1a1-a11a111a1,b2b2b2-bb22-22bb-b2b2-b2b2b2bb
   ```

Fehler bei der Registrierung werden in der Datei `RegistrationErrors.csv` gespeichert, die sich in dem Verzeichnis befindet, in dem Sie das `.ps1`-Skript gespeichert und ausgeführt haben. 

## <a name="next-steps"></a>Nächste Schritte

Aktualisieren Sie den Verwaltbarkeitsmodus auf [Vollständig](sql-vm-resource-provider-register.md#upgrade-to-full), um den vollständigen Funktionssatz zu nutzen, der Ihnen vom SQL-VM-Ressourcenanbieter bereitgestellt wird. 
