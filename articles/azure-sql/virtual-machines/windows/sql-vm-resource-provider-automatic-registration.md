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
ms.openlocfilehash: d4a2d9e43dadc53008c04b44ea1dda9cb337da99
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93308349"
---
# <a name="automatic-registration-with-sql-vm-resource-provider"></a>Automatische Registrierung beim SQL-VM-Ressourcenanbieter
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Aktivieren Sie die automatische Registrierungsfunktion im Azure-Portal, um alle aktuellen und zukünftigen SQL Server-Instanzen auf virtuellen Azure-Computern (virtual machines, VMs) automatisch im einfachen Modus beim SQL-VM-Ressourcenanbieter zu registrieren. Im Zuge der Registrierung beim SQL-VM-Ressourcenanbieter wird die [SQL-IaaS-Agent-Erweiterung](sql-server-iaas-agent-extension-automate-management.md) installiert.

In diesem Artikel erfahren Sie, wie Sie die automatische Registrierungsfunktion aktivieren. Alternativ können Sie [eine einzelne VM](sql-vm-resource-provider-register.md) oder [mehrere Ihrer VMs](sql-vm-resource-provider-bulk-register.md) beim SQL-VM-Ressourcenanbieter registrieren. 

## <a name="overview"></a>Übersicht

Durch die Registrierung Ihres virtuellen SQL Server-Computers beim SQL-VM-Ressourcenanbieter wird die [Erweiterung für den SQL-IaaS-Agent](sql-server-iaas-agent-extension-automate-management.md) installiert. 

Wenn die automatische Registrierung aktiviert ist, wird täglich ein Auftrag ausgeführt, um zu ermitteln, ob SQL Server auf allen nicht registrierten virtuellen Computern im Abonnement installiert ist. Hierzu werden die Binärdateien der Erweiterung für den SQL-IaaS-Agent auf den virtuellen Computer kopiert. Anschließend wird einmalig ein Hilfsprogramm ausgeführt, um nach der SQL Server-Registrierungsstruktur zu suchen. Wird die SQL Server-Registrierungsstruktur erkannt, wird der virtuelle Computer im einfachen Modus beim [SQL-VM-Ressourcenanbieter](sql-vm-resource-provider-register.md) registriert. Ist in der Registrierung keine SQL Server-Struktur vorhanden, werden die Binärdateien entfernt.

Nach Aktivierung der automatischen Registrierung für ein Abonnement werden alle aktuellen und zukünftigen virtuellen Computer, auf denen SQL Server installiert ist, im einfachen Modus beim SQL-VM-Ressourcenanbieter registriert. Sie müssen weiterhin [manuell auf den Modus „Vollständige Verwaltbarkeit“ aktualisieren](sql-vm-resource-provider-register.md#upgrade-to-full), um den vollständigen Fuktionssatz nutzen zu können. 

> [!IMPORTANT]
> Die Erweiterung für den SQL-IaaS-Agent sammelt Daten ausschließlich, um Kunden bei der Verwendung von SQL Server in Azure Virtual Machines optionale Vorteile zu bieten. Microsoft verwendet diese Daten ohne vorherige Zustimmung des Kunden nicht für Lizenzierungsüberprüfungen. Weitere Informationen finden Sie unter [Ergänzende Datenschutzbestimmungen zu SQL Server](/sql/sql-server/sql-server-privacy#non-personal-data).

## <a name="prerequisites"></a>Voraussetzungen

Zum Registrieren der SQL Server-VM beim Ressourcenanbieter benötigen Sie: 

- Ein [Azure-Abonnement](https://azure.microsoft.com/free/) und mindestens Berechtigungen der [Rolle „Mitwirkender“](../../../role-based-access-control/built-in-roles.md#all).
- Ein in der öffentlichen Cloud oder in der Azure Government-Cloud bereitgestelltes Azure-Ressourcenmodell vom Typ [Virtueller Computer mit Windows Server 2008 R2 (oder höher)](../../../virtual-machines/windows/quick-create-portal.md) mit [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads). Windows Server 2008 wird nicht unterstützt. 


## <a name="enable"></a>Aktivieren

Führen Sie die folgenden Schritte aus, um im Azure-Portal die automatische Registrierung Ihrer virtuellen SQL Server-Computer zu aktivieren:

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

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

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
