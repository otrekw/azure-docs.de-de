---
title: Installieren der SQL Server-IaaS-Agent-Erweiterung
description: Registrieren Sie Ihren virtuellen Azure SQL Server-Computer beim SQL-VM-Ressourcenanbieter, um Features für virtuelle SQL Server-Computer zu aktivieren, die außerhalb von Azure Marketplace bereitgestellt werden, und um Compliance und eine einfachere Verwaltung zu gewährleisten.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/30/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 8099bf87410e8359894d8b40d7637451891ad664
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286141"
---
# <a name="install-sql-server-iaas-agent-extension"></a>Installieren der SQL Server-IaaS-Agent-Erweiterung
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Durch die Registrierung Ihrer SQL Server-VM beim SQL VM-Ressourcenanbieter wird die [SQL Server-IaaS-Agent-Erweiterung](sql-server-iaas-agent-extension-automate-management.md) installiert. 

In diesem Artikel erfahren Sie, wie Sie eine einzelne SQL Server-VM bei dem SQL-VM-Ressourcenanbieter registrieren. Alternativ können Sie alle SQL Server-VMs [automatisch](sql-vm-resource-provider-automatic-registration.md) oder [per Skript im Massenverfahren](sql-vm-resource-provider-bulk-register.md) registrieren.


## <a name="overview"></a>Übersicht

Durch die Registrierung beim Ressourcenanbieter werden die [SQL Server-IaaS-Agent-Erweiterung](sql-server-iaas-agent-extension-automate-management.md) installiert und die _Ressource_ **Virtueller SQL-Computer** in Ihrem Abonnement erstellt. Dabei handelt es sich um eine _andere_ Ressource als diejenige für Ihren virtuellen Computer. Wenn Sie die Registrierung Ihrer SQL Server-VM beim Ressourcenanbieter aufheben, wird die _Ressource_ **Virtueller SQL-Computer** entfernt, während der tatsächliche virtuelle Computer jedoch erhalten bleibt.

Durch das Bereitstellen eines SQL Server-VM-Azure Marketplace-Images über das Azure-Portal wird die SQL Server-VM automatisch beim Ressourcenanbieter registriert. Wenn Sie SQL Server aber selbst auf einer Azure-VM installieren oder eine Azure-VM anhand einer benutzerdefinierten VHD bereitstellen, müssen Sie Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter registrieren, um die SQL Server-IaaS-Agent-Erweiterung zu installieren. 

Wenn Sie den SQL-VM-Ressourcenanbieter verwenden möchten, müssen Sie zunächst [Ihr Abonnement beim Ressourcenanbieter registrieren](#register-subscription-with-rp). Dadurch erhält der Ressourcenanbieter die Möglichkeit, Ressourcen innerhalb dieses jeweiligen Abonnements zu erstellen.

> [!IMPORTANT]
> Die SQL Server-IaaS-Agent-Erweiterung sammelt Daten ausschließlich, um Kunden bei Verwendung von SQL Server in Azure Virtual Machines optionale Vorteile zu bieten. Microsoft verwendet diese Daten ohne vorherige Zustimmung des Kunden nicht für Lizenzierungsüberprüfungen. Weitere Informationen finden Sie unter [Ergänzende Datenschutzbestimmungen zu SQL Server](/sql/sql-server/sql-server-privacy#non-personal-data).

## <a name="prerequisites"></a>Voraussetzungen

Zum Registrieren der SQL Server-VM beim Ressourcenanbieter benötigen Sie: 

- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- Ein in der öffentlichen Cloud oder Azure Government-Cloud bereitgestelltes Azure-Ressourcenmodell des Typs [Windows Server 2008-VM (oder höher)](../../../virtual-machines/windows/quick-create-portal.md) mit [SQL Server 2008 (oder höher)](https://www.microsoft.com/sql-server/sql-server-downloads). 
- Die aktuelle Version von [Azure CLI](/cli/azure/install-azure-cli) oder [Azure PowerShell (mindestens 5.0)](/powershell/azure/install-az-ps). 


## <a name="register-subscription-with-rp"></a>Registrieren eines Abonnements mit RP

Um Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter zu registrieren, müssen Sie zunächst Ihr Abonnement beim Ressourcenanbieter registrieren. Dadurch hat der SQL-VM-Ressourcenanbieter die Möglichkeit, Ressourcen in Ihrem Abonnement zu erstellen.  Verwenden Sie hierzu das Azure-Portal, die Azure CLI oder Azure PowerShell.

### <a name="azure-portal"></a>Azure-Portal

1. Öffnen Sie das Azure-Portal, und wechseln Sie zu **Alle Dienste**. 
1. Wechseln Sie zu **Abonnements** , und wählen Sie das gewünschte Abonnement aus.  
1. Wechseln Sie auf der Seite **Abonnements** zu **Ressourcenanbieter**. 
1. Geben Sie im Filter **sql** ein, um die SQL-bezogenen Ressourcenanbieter aufzurufen. 
1. Wählen Sie je nach gewünschter Aktion für den Anbieter **Microsoft.SqlVirtualMachine** eine der Optionen **Registrieren** , **Erneut registrieren** oder **Registrierung aufheben** aus. 

   ![Ändern des Anbieters](./media/sql-vm-resource-provider-register/select-resource-provider-sql.png)


### <a name="command-line"></a>Befehlszeile

Registrieren Sie Ihren SQL-VM-Ressourcenanbieter mithilfe der Azure CLI oder von Azure PowerShell bei Ihrem Azure-Abonnement. 

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/bash)

```azurecli-interactive
# Register the SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

```powershell-interactive
# Register the SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

---

## <a name="register-with-rp"></a>Mit Ressourcenanbieter registrieren

Für die [SQL Server-IaaS-Agent-Erweiterung](sql-server-iaas-agent-extension-automate-management.md#management-modes) gibt es drei Verwaltungsmodi. 

Bei der Installation der Erweiterung im vollständigen Verwaltungsmodus wird der SQL Server-Dienst neu gestartet. Es wird daher empfohlen, die Erweiterung zunächst im Modus „Lightweight“ zu installieren und dann in einem Wartungsfenster ein [Upgrade auf „Vollständig“](#upgrade-to-full) durchzuführen. 

### <a name="lightweight-management-mode"></a>Verwaltungsmodus „Lightweight“

Registrieren Sie Ihre SQL Server-VM mit der Azure CLI oder Azure PowerShell beim Ressourcenanbieter, und installieren Sie die SQL-IaaS-Erweiterung im Modus „Lightweight“. Dadurch wird der SQL Server-Dienst nicht neu gestartet. Sie können dann jederzeit ein Upgrade auf den Modus „Vollständig“ ausführen. Da hierdurch jedoch der SQL Server-Dienst neu gestartet wird, wird empfohlen, auf ein Fenster für die geplante Wartung zu warten. 

Den SQL Server-Lizenztyp können Sie als nutzungsbasierte Bezahlung (`PAYG`), Azure-Hybridvorteil (`AHUB`), um Ihre eigene Lizenz zu verwenden, oder Notfallwiederherstellung (`DR`), um das [kostenlose DR-Replikat in Azure](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure) zu aktivieren, angeben.

Failoverclusterinstanzen und Bereitstellungen mit mehreren Instanzen können nur im Modus „Lightweight“ beim SQL-VM-Ressourcenanbieter registriert werden. 

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/bash)

Registrieren einer SQL Server-VM im Modus „Lightweight“ mit der Azure CLI: 

  ```azurecli-interactive
  # Register Enterprise or Standard self-installed VM in Lightweight mode
  az sql vm create --name <vm_name> --resource-group <resource_group_name> --location <vm_location> --license-type PAYG 
  ```


# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Registrieren einer SQL Server-VM im Modus „Lightweight“ mit Azure PowerShell:  


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  # Register SQL VM with 'Lightweight' SQL IaaS agent
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType LightWeight  
  ```

---

### <a name="full-management-mode"></a>Verwaltungsmodus „Vollständig“

Beim Registrieren Ihrer SQL Server-VM im Modus „Vollständig“ wird der SQL Server-Dienst neu gestartet. Sie sollten daher mit Vorsicht vorgehen. 

Verwenden Sie den folgenden Azure PowerShell-Befehl, um Ihre SQL Server-VM direkt im Modus „Vollständig“ zu registrieren (und ggf. den SQL Server-Dienst neu zu starten): 

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

### <a name="noagent-management-mode"></a>Verwaltungsmodus „NoAgent“ 

Installationen von SQL Server 2008 und 2008 R2 unter Windows Server 2008 ( _nicht R2_ ) können beim SQL-VM-Ressourcenanbieter im [NoAgent-Modus](sql-server-iaas-agent-extension-automate-management.md#management-modes) registriert werden. Diese Option stellt die Konformität sicher und ermöglicht die Überwachung der SQL Server-VM im Azure-Portal mit eingeschränktem Funktionsumfang.

Geben Sie entweder `AHUB`, `PAYG` oder `DR` als **sqlLicenseType** und entweder `SQL2008-WS2008` oder `SQL2008R2-WS2008` als **sqlImageOffer** an. 

Verwenden Sie für die Registrierung Ihrer SQL Server 2008- oder 2008 R2-Instanz bei einer Windows Server 2008-Instanz den folgenden Azure CLI- oder Azure PowerShell-Ausschnitt: 


# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/bash)

Registrieren Ihres virtuellen SQL Server 2008-Computers im NoAgent-Modus mit der Azure CLI: 

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008-WS2008
 ```
 
 
Registrieren Ihres virtuellen SQL Server 2008 R2-Computers im NoAgent-Modus mit der Azure CLI: 

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008R2-WS2008
 ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Registrieren Ihres virtuellen SQL Server 2008-Computers im Modus „NoAgent“ mit Azure PowerShell: 

  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008-WS2008
  ```
  

Registrieren Ihres virtuellen SQL Server 2008 R2-Computers im Modus „NoAgent“ mit Azure PowerShell: 

  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008R2-WS2008
  ```

---

## <a name="verify-mode"></a>Überprüfen des Modus

Über Azure PowerShell können Sie den aktuellen Modus des SQL Server-IaaS-Agents anzeigen: 

```powershell-interactive
# Get the SqlVirtualMachine
$sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
$sqlvm.SqlManagementType
```

## <a name="upgrade-to-full"></a>Upgrade auf „Vollständig“  

Für SQL Server-VMs, auf denen die IaaS-Erweiterung im Modus *Lightweight* installiert ist, können Sie über das Azure-Portal, die Azure CLI oder Azure PowerShell ein Upgrade auf den Modus _Vollständig_ durchführen. Für SQL Server-VMs im Modus _NoAgent_ kann ein Upgrade auf _Vollständig_ durchgeführt werden, nachdem das Betriebssystem auf Windows 2008 R2 oder höher aktualisiert wurde. Es ist nicht möglich, ein Downgrade auszuführen. Dazu müssen Sie die Registrierung der SQL Server-VM beim SQL-VM-Ressourcenanbieter [aufheben](#unregister-from-rp). Dadurch wird die _Ressource_ **Virtueller SQL-Computer** entfernt, der tatsächliche virtuelle Computer aber nicht gelöscht. 


### <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wechseln Sie zu Ihrer Ressource [Virtuelle SQL-Computer](manage-sql-vm-portal.md#access-the-sql-virtual-machines-resource). 
1. Wählen Sie Ihre SQL Server-VM aus, und klicken Sie auf **Übersicht**. 
1. Wählen Sie für SQL Server-VMs mit dem IaaS-Modus „NoAgent“ oder „Lightweight“ die Meldung **Mit der SQL-IaaS-Erweiterung sind nur Lizenztyp- und Editionsaktualisierungen verfügbar** aus.

   ![Auswahlmöglichkeiten zum Ändern des Modus im Portal](./media/sql-vm-resource-provider-register/change-sql-iaas-mode-portal.png)

1. Aktivieren Sie das Kontrollkästchen **Ich stimme dem Neustart des SQL Server-Diensts auf dem virtuellen Computer** zu, und klicken Sie dann auf **Bestätigen** , um ein Upgrade des IaaS-Modus auf „Vollständig“ durchzuführen. 

    ![Kontrollkästchen für die Zustimmung zum Neustart des SQL Server-Diensts auf dem virtuellen Computer](./media/sql-vm-resource-provider-register/enable-full-mode-iaas.png)

### <a name="command-line"></a>Befehlszeile

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/bash)

Führen Sie den folgenden Codeausschnitt in der Azure-Befehlszeilenschnittstelle aus:

  ```azurecli-interactive
  # Update to full mode
  az sql vm update --name <vm_name> --resource-group <resource_group_name> --sql-mgmt-type full  
  ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Führen Sie den folgenden Codeausschnitt in Azure PowerShell aus:

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  Update-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

---

## <a name="verify-registration-status"></a>Überprüfen des Registrierungsstatus
Sie können über das Azure-Portal, die Azure CLI oder Azure PowerShell überprüfen, ob Ihre SQL Server-VM bereits beim SQL-VM-Ressourcenanbieter registriert ist. 

### <a name="azure-portal"></a>Azure-Portal 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Navigieren Sie zu Ihrem [SQL Server-VMs](manage-sql-vm-portal.md).
1. Wählen Sie die SQL Server-VM in der Liste aus. Wenn Ihre SQL Server-VM hier nicht aufgeführt ist, wurde sie wahrscheinlich nicht beim SQL-VM-Ressourcenanbieter registriert. 
1. Überprüfen Sie den Wert unter **Status**. Wenn **Status** den Wert **Erfolgreich** aufweist, wurde die SQL Server-VM beim SQL-VM-Ressourcenanbieter registriert. 

   ![Überprüfen des Status per SQL RP-Registrierung](./media/sql-vm-resource-provider-register/verify-registration-status.png)

### <a name="command-line"></a>Befehlszeile

Überprüfen Sie den aktuellen Registrierungsstatus der SQL Server-VM mit der Azure CLI oder Azure PowerShell. `ProvisioningState` zeigt `Succeeded` an, wenn die Registrierung erfolgreich abgeschlossen wurde. 

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/bash)


  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
 ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

  ```powershell-interactive
  Get-AzSqlVM -Name <vm_name> -ResourceGroupName <resource_group>
  ```

---

Ein Fehler zeigt an, dass die SQL Server-VM nicht beim Ressourcenanbieter registriert wurde. 


## <a name="unregister-from-rp"></a>Aufheben der Registrierung beim Ressourcenanbieter

Um die Registrierung Ihrer SQL Server-VM beim SQL-VM-Ressourcenanbieter aufzuheben, löschen Sie die SQL-VM- *Ressource* über das Azure-Portal oder die Azure CLI. Das Löschen der SQL-VM- *Ressource* löscht nicht die SQL Server-VM. Seien Sie jedoch vorsichtig, und führen Sie die Schritte sorgfältig aus, da es möglich ist, den virtuellen Computer versehentlich zu löschen, wenn Sie versuchen, die *Ressource* zu entfernen. 

Das Aufheben der Registrierung des virtuellen SQL-Computers beim SQL-VM-Ressourcenanbieter ist notwendig, um den Verwaltungsmodus von „Vollständig“ herabzustufen. 

### <a name="azure-portal"></a>Azure-Portal

Gehen Sie wie folgt vor, um Ihre SQL Server-VM beim Ressourcenanbieter über das Azure-Portal abzumelden:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zur SQL Server-VM-Ressource. 
  
   ![Ressource „Virtuelle SQL-Computer“](./media/sql-vm-resource-provider-register/sql-vm-manage.png)

1. Klicken Sie auf **Löschen**. 

   ![Auswählen von „Löschen“ in der oberen Navigationsleiste](./media/sql-vm-resource-provider-register/delete-sql-vm-resource-provider.png)

1. Geben Sie den Namen der SQL-VM ein, und **deaktivieren Sie das Kontrollkästchen neben dem virtuellen Computer**.

   ![Deaktivieren der VM, um das Löschen der eigentlichen VM zu verhindern, und anschließend Auswählen von „Löschen“, um mit dem Löschen der SQL-VM-Ressource fortzufahren](./media/sql-vm-resource-provider-register/confirm-delete-of-resource-uncheck-box.png)

   >[!WARNING]
   > Wenn Sie das Kontrollkästchen neben dem Namen des virtuellen Computers nicht deaktivieren, wird der virtuelle Computer vollständig *gelöscht*. Deaktivieren Sie das Kontrollkästchen, um die Registrierung der SQL Server-VM beim Ressourcenanbieter aufzuheben, aber *den aktuellen virtuellen Computer nicht zu löschen*. 

1. Wählen Sie **Löschen** aus, um das Löschen der SQL-VM- *Ressource* und nicht der SQL Server-VM zu bestätigen. 

### <a name="command-line"></a>Befehlszeile

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Um die Registrierung Ihrer SQL Server-VM beim Ressourcenanbieter mithilfe der Azure CLI aufzuheben, verwenden Sie den Befehl [az sql vm delete](/cli/azure/sql/vm?view=azure-cli-latest&preserve-view=true#az-sql-vm-delete). Dadurch wird die SQL Server-VM- *Ressource* entfernt, aber der virtuelle Computer nicht gelöscht. 


```azurecli-interactive
az sql vm delete 
  --name <SQL VM resource name> |
  --resource-group <Resource group name> |
  --yes 
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Um die Registrierung Ihrer SQL Server-VM beim Ressourcenanbieter mit Azure PowerShell aufzuheben, verwenden Sie den Befehl [Remove-AzSqlVM](/powershell/module/az.sqlvirtualmachine/remove-azsqlvm). Dadurch wird die SQL Server-VM- *Ressource* entfernt, aber der virtuelle Computer nicht gelöscht. 

```powershell-interactive
Remove-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name>
```

---



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern](frequently-asked-questions-faq.md)  
* [Preisinformationen für SQL Server auf virtuellen Windows-Computern](pricing-guidance.md)
* [SQL Server auf Windows-VMs – Versionshinweise](../../database/doc-changes-updates-release-notes.md)
