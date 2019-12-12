---
title: Registrieren beim SQL-VM-Ressourcenanbieter
description: Registrieren Sie Ihren virtuellen Azure SQL Server-Computer beim SQL-VM-Ressourcenanbieter, um Features für SQL Server-VMs zu aktivieren, die außerhalb von Azure Marketplace bereitgestellt werden, und um Compliance und eine einfachere Verwaltung zu gewährleisten.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 6b2430b5135a5d3f7ad1f9ef0bd17d9149bf48ee
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74793459"
---
# <a name="register-a-sql-server-virtual-machine-in-azure-with-the-sql-vm-resource-provider"></a>Registrieren von virtuellen SQL Server-Computern in Azure mit dem SQL-VM-Ressourcenanbieter

In diesem Artikel wird beschrieben, wie Sie Ihren virtuellen SQL Server-Computer (VM) in Azure beim SQL-VM-Ressourcenanbieter registrieren. Durch die Registrierung beim Ressourcenanbieter wird die _Ressource_ **Virtueller SQL-Computer** innerhalb Ihres Abonnements erstellt. Dabei handelt es sich um eine andere Ressource als die Ressource für Ihren virtuellen Computer. Wenn Sie die Registrierung Ihrer SQL Server-VM beim Ressourcenanbieter aufheben, wird die _Ressource_ **Virtueller SQL-Computer** entfernt, während der tatsächliche virtuelle Computer jedoch erhalten bleibt. 

Durch das Bereitstellen eines SQL Server-VM-Azure Marketplace-Images über das Azure-Portal wird die SQL Server-VM automatisch beim Ressourcenanbieter registriert. Wenn Sie SQL Server aber auf einem virtuellen Azure-Computer selbst installieren oder einen virtuellen Azure-Computer von einer benutzerdefinierten VHD bereitstellen, sollten Sie Ihre SQL Server-VM beim Ressourcenanbieter registrieren, um folgende Vorteile zu nutzen:

- **Featurevorteile:** Wenn Sie Ihre SQL Server-VM beim Ressourcenanbieter registrieren, haben Sie Zugriff auf Funktionen wie [automatisiertes Patchen](virtual-machines-windows-sql-automated-patching.md) und [automatisierte Sicherung](virtual-machines-windows-sql-automated-backup-v2.md) sowie auf Überwachungs- und Verwaltungsfunktionen. Außerdem erhalten Sie mehr Flexibilität bei der [Lizenzierung](virtual-machines-windows-sql-ahb.md) und bei der Auswahl der [Edition](virtual-machines-windows-sql-change-edition.md). Zuvor waren diese Funktionen nur für SQL Server-VM-Images verfügbar, die aus dem Azure Marketplace bereitgestellt wurden. 

- **Compliance**: Durch die Registrierung beim SQL-VM-Ressourcenanbieter können Sie auch einfacher Ihrer Verpflichtung nachkommen, Microsoft zu benachrichtigen, wenn der Azure-Hybridvorteil aktiviert wurde (wie in den Nutzungsbedingungen zum Produkt angegeben). Auf diese Weise können Sie darauf verzichten, für jede Ressource ein Lizenzregistrierungsformular zu verwalten.  

- **Kostenlose Verwaltung**:  Die Registrierung beim SQL-VM-Ressourcenanbieter ist in allen drei Verwaltungsmodi völlig kostenlos. Es fallen keine zusätzlichen Kosten für den Ressourcenanbieter oder das Ändern von Verwaltungsmodi an. 

- **Vereinfachte Lizenzverwaltung**: Durch die Registrierung beim SQL-VM-Ressourcenanbieter wird die SQL Server-Lizenzverwaltung vereinfacht. Außerdem können Sie über das [Azure-Portal](virtual-machines-windows-sql-manage-portal.md), die Azure-Befehlszeilenschnittstelle oder PowerShell schnell SQL Server-VMs mit aktiviertem Azure-Hybridvorteil identifizieren: 

   # <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

   ```azurecli-interactive
   $vms = az sql vm list | ConvertFrom-Json
   $vms | Where-Object {$_.sqlServerLicenseType -eq "AHUB"}
   ```

   # <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

   ```powershell-interactive
   Get-AzSqlVM | Where-Object {$_.LicenseType -eq 'AHUB'}
   ```

   ---

Wenn Sie den SQL-VM-Ressourcenanbieter verwenden möchten, müssen Sie zunächst [Ihr Abonnement beim Ressourcenanbieter registrieren](#register-subscription-with-rp). Dadurch erhält der Ressourcenanbieter die Möglichkeit, Ressourcen innerhalb dieses jeweiligen Abonnements zu erstellen.

Um mehr über die Vorteile zu erfahren, die sich aus der Verwendung des SQL-VM-Ressourcenanbieters ergeben, sehen Sie sich dieses [channel9](https://channel9.msdn.com/Shows/Data-Exposed/Benefit-from-SQL-VM-Resource-Provider-when-self-installing-SQL-Server-on-Azure?WT.mc_id=dataexposed-c9-niner)-Video an: 

<iframe src="https://channel9.msdn.com/Shows/Data-Exposed/Benefit-from-SQL-VM-Resource-Provider-when-self-installing-SQL-Server-on-Azure/player" width="960" height="540" allowFullScreen frameBorder="0" title="Benefit from SQL VM Resource Provider when self-installing SQL Server on Azure - Microsoft Channel 9 Video (Vorteil durch den SQL VM-Ressourcenanbieter bei der Eigeninstallation von SQL Server in Azure – Microsoft Channel 9-Video)"></iframe>


## <a name="prerequisites"></a>Voraussetzungen

Zum Registrieren der SQL Server-VM beim Ressourcenanbieter benötigen Sie: 

- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- Ein in der öffentlichen Cloud bereitgestelltes Azure-Ressourcenmodell vom Typ [SQL Server-VM](virtual-machines-windows-portal-sql-server-provision.md). 
- Die aktuelle Version von [Azure CLI](/cli/azure/install-azure-cli) oder [PowerShell](/powershell/azure/new-azureps-module-az). 

## <a name="management-modes"></a>Verwaltungsmodi

Wenn die [SQL-IaaS-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md) noch nicht installiert wurde, wird die SQL Server-IaaS-Erweiterung bei der Registrierung beim SQL-VM-Ressourcenanbieter automatisch in einem von drei Verwaltungsmodi installiert, die während des Registrierungsvorgangs angegeben wurden. Wenn Sie keinen Verwaltungsmodus angeben, wird die SQL-IaaS-Erweiterung im Verwaltungsmodus „Vollständig“ installiert.  

Wenn die SQL-IaaS-Erweiterung bereits manuell installiert wurde, befindet sie sich bereits im Verwaltungsmodus „Vollständig“. Daher wird der SQL Server-Dienst bei der Registrierung beim Ressourcenanbieter im Modus „Vollständig“ nicht neu gestartet.

Die drei Verwaltungsmodi lauten wie folgt:

- Der Modus **Lightweight** erfordert keinen Neustart von SQL Server, unterstützt jedoch nur das Ändern des Lizenztyps und der Edition von SQL Server. Verwenden Sie diese Option für SQL Server-VMs mit mehreren Instanzen oder für die Teilnahme an einer Failoverclusterinstanz (FCI). Die Verwendung des Modus „Lightweight“ hat keine Auswirkungen auf den Arbeitsspeicher oder die CPU. Außerdem fallen keine Kosten an. Es wird empfohlen, die SQL Server-VM zunächst im Modus „Lightweight“ zu registrieren und dann während eines Fenster für die geplante Wartung ein Upgrade auf den Modus „Vollständig“ auszuführen.  

- Der Modus **Vollständig** bietet alle Funktionen, erfordert aber einen Neustart des SQL Server-Computers sowie Systemadministratorberechtigungen. Diese Option wird standardmäßig installiert, wenn die SQL-IaaS-Erweiterung manuell installiert wird. Verwenden Sie diesen Modus zum Verwalten einer SQL Server-VM mit einer einzelnen Instanz. Für den Modus „Vollständig“ werden zwei Windows-Dienste installiert, die minimale Auswirkungen auf den Arbeitsspeicher und die CPU haben. Diese Dienste können über den Task-Manager überwacht werden. Mit der Verwendung des Verwaltbarkeitsmodus „Vollständig“ sind keine Kosten verbunden. 

- Der Modus **NoAgent** ist speziell für Installationen von SQL Server 2008 und SQL Server 2008 R2 unter Windows Server 2008 vorgesehen. Es gibt keine Auswirkung auf den Arbeitsspeicher oder die CPU, wenn der Modus „NoAgent“ verwendet wird. Mit der Verwendung des Verwaltbarkeitsmodus „NoAgent“ sind keine Kosten verbunden. 

Über PowerShell können Sie den aktuellen Modus des SQL Server-IaaS-Agents anzeigen: 

  ```powershell-interactive
  # Get the SqlVirtualMachine
  $sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
  $sqlvm.SqlManagementType
  ```

## <a name="register-subscription-with-rp"></a>Registrieren eines Abonnements mit RP

Um Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter zu registrieren, müssen Sie zunächst Ihr Abonnement beim Ressourcenanbieter registrieren. Dadurch hat der SQL-VM-Ressourcenanbieter die Möglichkeit, Ressourcen in Ihrem Abonnement zu erstellen.  Verwenden Sie hierzu das Azure-Portal, die Azure-Befehlszeilenschnittstelle oder PowerShell.

### <a name="azure-portal"></a>Azure-Portal

1. Öffnen Sie das Azure-Portal, und wechseln Sie zu **Alle Dienste**. 
1. Wechseln Sie zu **Abonnements**, und wählen Sie das gewünschte Abonnement aus.  
1. Wechseln Sie auf der Seite **Abonnements** zu **Ressourcenanbieter**. 
1. Geben Sie im Filter **sql** ein, um die SQL-bezogenen Ressourcenanbieter aufzurufen. 
1. Wählen Sie je nach gewünschter Aktion für den Anbieter **Microsoft.SqlVirtualMachine** eine der Optionen **Registrieren**, **Erneut registrieren** oder **Registrierung aufheben** aus. 

![Ändern des Anbieters](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)


### <a name="command-line"></a>Befehlszeile

Registrieren Sie Ihren SQL-VM-Ressourcenanbieter mithilfe von Azure-Befehlszeilenschnittstelle oder PowerShell bei Ihrem Azure-Abonnement. 

# <a name="az-clitabbash"></a>[Azure CLI](#tab/bash)

```azurecli-interactive
# Register the SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

```powershell-interactive
# Register the SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

---

## <a name="register-sql-vm-with-rp"></a>Registrieren einer SQL-VM beim RP 

### <a name="lightweight-management-mode"></a>Verwaltungsmodus „Lightweight“

Wenn die [SQL Server-IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md) nicht auf der VM installiert wurde, wird empfohlen, die Registrierung beim SQL-VM-Ressourcenanbieter im Modus „Lightweight“ auszuführen. Dadurch wird die SQL-IaaS-Erweiterung im [Modus „Lightweight“](#management-modes) installiert. Darüber hinaus wird verhindert, dass der SQL Server-Dienst neu gestartet wird. Sie können dann jederzeit ein Upgrade auf den Modus „Vollständig“ ausführen. Da hierdurch jedoch der SQL Server-Dienst neu gestartet wird, wird empfohlen, auf ein Fenster für die geplante Wartung zu warten. Sie müssen den Typ der SQL Server-Lizenz entweder als „Nutzungsbasierte Bezahlung“ (`PAYG`) angeben, um nutzungsbasiert zu bezahlen, oder als „Azure-Hybridvorteil“ (`AHUB`), um Ihre eigene Lizenz zu verwenden.

Failoverclusterinstanzen und Bereitstellungen mit mehreren Instanzen können nur im Modus „Lightweight“ beim SQL-VM-Ressourcenanbieter registriert werden. 

# <a name="az-clitabbash"></a>[Azure CLI](#tab/bash)

Registrieren der SQL Server-VM im Modus „Lightweight“ mit der Azure-Befehlszeilenschnittstelle: 

  ```azurecli-interactive
  # Register Enterprise or Standard self-installed VM in Lightweight mode
  az sql vm create --name <vm_name> --resource-group <resource_group_name> --location <vm_location> --license-type PAYG 
  ```


# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Registrieren der SQL Server-VM im Modus „Lightweight“ mit PowerShell:  


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  # Register SQL VM with 'Lightweight' SQL IaaS agent
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType LightWeight  
  ```

---

### <a name="full-management-mode"></a>Verwaltungsmodus „Vollständig“


Wenn die SQL-IaaS-Erweiterung bereits manuell auf der VM installiert wurde, können Sie die SQL Server-VM im Modus „Vollständig“ registrieren, ohne den SQL Server-Dienst neu zu starten. **Wenn die SQL-IaaS-Erweiterung jedoch nicht installiert wurde, wird die SQL-IaaS-Erweiterung durch die Registrierung im Modus „Vollständig“ ebenfalls im Modus „Vollständig“ installiert. Darüber hinaus wird der SQL Server-Dienst neu gestartet. Sie sollten daher mit Vorsicht vorgehen.**

Im Folgenden der Codeausschnitt für die Registrierung beim SQL-VM-Ressourcenanbieter im Modus „Vollständig“. Verwenden Sie den folgenden PowerShell-Befehl, um eine Registrierung im Verwaltungsmodus „Vollständig“ auszuführen:

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  Update-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```


### <a name="noagent-management-mode"></a>Verwaltungsmodus „NoAgent“ 

Installationen von SQL Server 2008 und 2008 R2 unter Windows Server 2008 können beim SQL-VM-Ressourcenanbieter im [Modus „NoAgent“](#management-modes) registriert werden. Diese Option stellt die Konformität sicher und ermöglicht die Überwachung der SQL Server-VM im Azure-Portal mit eingeschränktem Funktionsumfang.

Geben Sie entweder `AHUB` oder `PAYG` als **sqlLicenseType** und entweder `SQL2008-WS2008` oder `SQL2008R2-WS2008` als **sqlImageOffer** an. 

Verwenden Sie für die Registrierung Ihrer SQL Server 2008- oder 2008 R2-Instanz bei einer Windows Server 2008-Instanz den folgenden Codeausschnitt für die Azure-Befehlszeilenschnittstelle oder PowerShell: 


# <a name="az-clitabbash"></a>[Azure CLI](#tab/bash)

Registrieren der SQL Server-VM im Modus „NoAgent“ mit der Azure-Befehlszeilenschnittstelle: 

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008-WS2008R2
 ```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Registrieren der SQL Server-VM im Modus „NoAgent“ mit PowerShell: 


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008R2-WS2008
  ```

---

## <a name="upgrade-to-full-management-mode"></a>Aktualisieren auf den Verwaltungsmodus „Vollständig“ 

Für SQL Server-VMs, auf denen die IaaS-Erweiterung im Modus *Lightweight* installiert ist, können Sie über das Azure-Portal, die Azure-Befehlszeilenschnittstelle oder PowerShell ein Upgrade auf den Modus _Vollständig_ ausführen. Für SQL Server-VMs im Modus _NoAgent_ kann ein Upgrade auf _Vollständig_ durchgeführt werden, nachdem das Betriebssystem auf Windows 2008 R2 oder höher aktualisiert wurde. Es ist nicht möglich, ein Downgrade auszuführen. Dazu müssen Sie die Registrierung der SQL Server-VM beim SQL-VM-Ressourcenanbieter [aufheben](#unregister-vm-from-rp). Dadurch wird die _Ressource_ **Virtueller SQL-Computer** entfernt, der tatsächliche virtuelle Computer aber nicht gelöscht. 

Über PowerShell können Sie den aktuellen Modus des SQL Server-IaaS-Agents anzeigen: 

  ```powershell-interactive
  # Get the SqlVirtualMachine
  $sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
  $sqlvm.SqlManagementType
  ```

So führen Sie Upgrade des Agent-Modus auf „Vollständig“ durch: 


### <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wechseln Sie zu Ihrer Ressource [Virtuelle SQL-Computer](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource). 
1. Wählen Sie Ihren virtuellen SQL Server-Computer aus, und klicken Sie auf **Übersicht**. 
1. Wählen Sie für SQL Server-VMs mit dem IaaS-Modus „NoAgent“ oder „Lightweight“ die Meldung **Mit der SQL-IaaS-Erweiterung sind nur Lizenztyp- und Editionsaktualisierungen verfügbar** aus.

   ![Auswahlmöglichkeiten zum Ändern des Modus im Portal](media/virtual-machines-windows-sql-server-agent-extension/change-sql-iaas-mode-portal.png)

1. Aktivieren Sie das Kontrollkästchen **Ich stimme dem Neustart des SQL Server-Diensts auf dem virtuellen Computer** zu, und klicken Sie dann auf **Bestätigen**, um ein Upgrade des IaaS-Modus auf „Vollständig“ durchzuführen. 

    ![Kontrollkästchen für die Zustimmung zum Neustart des SQL Server-Diensts auf dem virtuellen Computer](media/virtual-machines-windows-sql-server-agent-extension/enable-full-mode-iaas.png)

### <a name="command-line"></a>Befehlszeile

# <a name="az-clitabbash"></a>[Azure CLI](#tab/bash)

Führen Sie den folgenden Codeausschnitt in der Azure CLI aus:

  ```azurecli-interactive
  # Update to full mode
  az sql vm update --name <vm_name> --resource-group <resource_group_name> --sql-mgmt-type full  
  ```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Führen Sie den folgenden Codeausschnitt in PowerShell aus:

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>

  # Update to full mode
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
     -LicenseType PAYG -SqlManagementType Full
  ```

---

## <a name="verify-registration-status"></a>Überprüfen des Registrierungsstatus
Sie können über das Azure-Portal, die Azure-Befehlszeilenschnittstelle oder PowerShell überprüfen, ob Ihre SQL Server-VM bereits beim SQL-VM-Ressourcenanbieter registriert wurde. 

### <a name="azure-portal"></a>Azure-Portal 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Navigieren Sie zu Ihren [SQL Server-VMs](virtual-machines-windows-sql-manage-portal.md).
1. Wählen Sie die SQL Server-VM in der Liste aus. Wenn Ihre SQL Server-VM hier nicht aufgeführt ist, wurde sie wahrscheinlich nicht beim SQL-VM-Ressourcenanbieter registriert. 
1. Überprüfen Sie den Wert unter **Status**. Wenn **Status** den Wert **Erfolgreich** aufweist, wurde die SQL Server-VM beim SQL-VM-Ressourcenanbieter registriert. 

![Überprüfen des Status per SQL RP-Registrierung](media/virtual-machines-windows-sql-register-with-rp/verify-registration-status.png)

### <a name="command-line"></a>Befehlszeile

Überprüfen Sie den aktuellen Registrierungsstatus der SQL Server-VM per Azure-Befehlszeilenschnittstelle oder PowerShell. `ProvisioningState` zeigt `Succeeded` an, wenn die Registrierung erfolgreich abgeschlossen wurde. 

# <a name="az-clitabbash"></a>[Azure CLI](#tab/bash)


  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
 ```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

  ```powershell-interactive
  Get-AzResource -ResourceName <vm_name> -ResourceGroupName <resource_group> `
  -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines
  ```

---

Ein Fehler zeigt an, dass die SQL Server-VM nicht beim Ressourcenanbieter registriert wurde. 


## <a name="unregister-vm-from-rp"></a>Aufheben der VM-Registrierung beim RP

Um die Registrierung Ihrer SQL Server-VM beim SQL-VM-Ressourcenanbieter aufzuheben, löschen Sie die SQL-VM-*Ressource* über das Azure-Portal oder die Azure-Befehlszeilenschnittstelle. Das Löschen der SQL-VM-*Ressource* löscht nicht die SQL Server-VM. Seien Sie jedoch vorsichtig, und führen Sie die Schritte sorgfältig aus, da es möglich ist, den virtuellen Computer versehentlich zu löschen, wenn Sie versuchen, die *Ressource* zu entfernen. 

Das Aufheben der Registrierung der SQL-VM beim SQL-VM-Ressourcenanbieter ist notwendig, um den Verwaltungsmodus von „Vollständig“ herabzustufen. 

### <a name="azure-portal"></a>Azure-Portal

Gehen Sie wie folgt vor, um Ihre SQL Server-VM beim Ressourcenanbieter über das Azure-Portal abzumelden:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zur SQL Server-VM-Ressource. 
  
   ![Ressource „Virtuelle SQL-Computer“](media/virtual-machines-windows-sql-manage-portal/sql-vm-manage.png)

1. Klicken Sie auf **Löschen**. 

   ![Löschen des SQL-VM-Ressourcenanbieters](media/virtual-machines-windows-sql-register-with-rp/delete-sql-vm-resource-provider.png)

1. Geben Sie den Namen der SQL-VM ein, und **deaktivieren Sie das Kontrollkästchen neben dem virtuellen Computer**.

   ![Löschen des SQL-VM-Ressourcenanbieters](media/virtual-machines-windows-sql-register-with-rp/confirm-delete-of-resource-uncheck-box.png)

   >[!WARNING]
   > Wenn Sie das Kontrollkästchen neben dem Namen des virtuellen Computers nicht deaktivieren, wird der virtuelle Computer vollständig *gelöscht*. Deaktivieren Sie das Kontrollkästchen, um die Registrierung der SQL Server-VM beim Ressourcenanbieter aufzuheben, aber *den aktuellen virtuellen Computer nicht zu löschen*. 

1. Wählen Sie **Löschen** aus, um das Löschen der SQL-VM-*Ressource* und nicht der SQL Server-VM zu bestätigen. 

### <a name="command-line"></a>Befehlszeile

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Um die Registrierung Ihrer SQL Server-VM beim Ressourcenanbieter mithilfe der Azure-Befehlszeilenschnittstelle aufzuheben, verwenden Sie den Befehl [az sql vm delete](/cli/azure/sql/vm?view=azure-cli-latest#az-sql-vm-delete). Dadurch wird die SQL Server-VM-*Ressource* entfernt, aber der virtuelle Computer nicht gelöscht. 


```azurecli-interactive
az sql vm delete 
  --name <SQL VM resource name> |
  --resource-group <Resource group name> |
  --yes 
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Um die Registrierung Ihrer SQL Server-VM beim Ressourcenanbieter mithilfe der Azure-Befehlszeilenschnittstelle aufzuheben, verwenden Sie den Befehl [New-AzSqlVM](/powershell/module/az.sqlvirtualmachine/new-azsqlvm). Dadurch wird die SQL Server-VM-*Ressource* entfernt, aber der virtuelle Computer nicht gelöscht. 

```powershell-interactive
Remove-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name>
```

---

## <a name="limitations"></a>Einschränkungen

Der SQL-VM-Ressourcenanbieter unterstützt ausschließlich:
- Über den Azure Resource Manager bereitgestellte SQL Server-VMs. SQL Server-VMs, die mit dem klassischen Modell bereitgestellt wurden, werden nicht unterstützt. 
- In der öffentlichen Cloud bereitgestellte SQL Server-VMs. Bereitstellungen in der Private Cloud oder der Government Cloud werden nicht unterstützt. 


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 

**Sollte ich meine SQL Server-VM registrieren, die über ein SQL Server-Image im Azure Marketplace bereitgestellt wird?**

Nein. Microsoft registriert VMs, die über die SQL Server-Images im Azure Marketplace bereitgestellt werden, automatisch. Eine Registrierung beim SQL-VM-Ressourcenanbieter ist nur erforderlich, wenn die VM *nicht* über die SQL Server-Images im Azure Marketplace bereitgestellt und SQL Server selbst installiert wurde.

**Ist der SQL-VM-Ressourcenanbieter für alle Kunden verfügbar?** 

Ja. Kunden sollten Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter registrieren, wenn sie kein SQL Server-Image aus dem Azure Marketplace verwenden, sondern stattdessen SQL Server selbst installiert oder ihre benutzerdefinierte VHD verwendet haben. Beim SQL-VM-Ressourcenanbieter können VMs im Besitz aller Abonnementtypen (direkt, Enterprise Agreement und Cloud Solution Provider) registriert werden.

**Sollte ich mich beim SQL-VM-Ressourcenanbieter registrieren, wenn bei meiner SQL Server-VM die SQL Server-IaaS-Erweiterung bereits installiert wurde?**

Wenn Ihre SQL Server-VM selbst installiert und nicht über die SQL-Images im Azure Marketplace bereitgestellt wurde, sollten Sie sich beim SQL-VM-Ressourcenanbieter auch dann registrieren, wenn Sie die SQL Server-IaaS-Erweiterung installiert haben. Bei der Registrierung beim SQL-VM-Ressourcenanbieter wird eine neue Ressource vom Typ „Microsoft.SqlVirtualMachines“ erstellt. Bei der Installation der SQL Server-IaaS-Erweiterung wird diese Ressource nicht erstellt.

**Was ist der Standardverwaltungsmodus bei der Registrierung beim SQL-VM-Ressourcenanbieter?**

Der Standardverwaltungsmodus bei der Registrierung beim SQL-VM-Ressourcenanbieter ist *Vollständig*. Wenn die Eigenschaft zur Verwaltung von SQL Server bei der Registrierung beim SQL-VM-Ressourcenanbieter nicht festgelegt wurde, wird der Verwaltungsmodus „Vollständig“ festgelegt. Darüber hinaus wird der SQL Server-Dienst neu gestartet. Es wird empfohlen, die Registrierung beim SQL-VM-Ressourcenanbieter zunächst im Modus „Lightweight“ auszuführen und dann während eines Wartungsfensters ein Upgrade auf den Modus „Vollständig“ auszuführen. 

**Was sind die Voraussetzungen für die Registrierung beim SQL-VM-Ressourcenanbieter?**

Es gibt keine Voraussetzungen für die Registrierung beim SQL-VM-Ressourcenanbieter im Lightweight- oder NoAgent-Modus. Als Voraussetzung für die Registrierung beim SQL-VM-Ressourcenanbieter im Modus „Vollständig“ muss die SQL Server-IaaS-Erweiterung auf der VM installiert sein. Andernfalls wird der SQL Server-Dienst neu gestartet. 

**Kann ich mich beim SQL-VM-Ressourcenanbieter registrieren, wenn die SQL Server-IaaS-Erweiterung nicht auf der VM installiert ist?**

Ja, Sie können sich beim SQL-VM-Ressourcenanbieter im Lightweight-Verwaltungsmodus registrieren, wenn die SQL Server-IaaS-Erweiterung auf der VM nicht installiert wurde. Im Lightweight-Modus verwendet der SQL-VM-Ressourcenanbieter eine Konsolen-App, um die Version und Edition der SQL Server-Instanz zu überprüfen. 

Der SQL-Standardverwaltungsmodus bei der Registrierung beim SQL-VM-Ressourcenanbieter ist _Vollständig_. Wenn die Eigenschaft „SQL-Verwaltung“ bei der Registrierung beim SQL-VM-Ressourcenanbieter nicht festgelegt wird, wird für den Modus „Vollständige Verwaltbarkeit“ festgelegt. Es wird empfohlen, die Registrierung beim SQL-VM-Ressourcenanbieter zunächst im Modus „Lightweight“ auszuführen und dann während eines Wartungsfensters ein Upgrade auf den Modus „Vollständig“ auszuführen. 

**Wird bei der Registrierung beim SQL-VM-Ressourcenanbieter ein Agent auf meiner VM installiert?**

Nein. Bei der Registrierung beim SQL-VM-Ressourcenanbieter wird lediglich eine neue Metadatenressource erstellt. Es wird kein Agent auf der VM installiert.

Die SQL Server-IaaS-Erweiterung ist nur erforderlich, um vollständige Verwaltbarkeit zu aktivieren. Bei einem Upgrade des Verwaltungsmodus von „Lightweight“ auf „Vollständig“ wird die SQL Server-IaaS-Erweiterung installiert, und SQL Server wird neu gestartet.

**Wird SQL Server durch die Registrierung beim SQL-VM-Ressourcenanbieter auf meiner VM neu gestartet?**

Das hängt von dem während der Registrierung angegebenen Modus ab. Wenn der Modus „Lightweight“ oder „NoAgent“ angegeben wurde, wird der SQL Server-Dienst nicht neu gestartet. Wenn jedoch der Verwaltungsmodus „Vollständig“ oder gar kein Verwaltungsmodus angegeben wurde, wird die SQL-IaaS-Erweiterung im Verwaltungsmodus „Vollständig“ installiert, wodurch der SQL Server-Dienst neu gestartet wird. 

**Worin besteht der Unterschied zwischen den Verwaltungsmodi „Lightweight“ und „NoAgent“ bei der Registrierung beim SQL-VM-Ressourcenanbieter?** 

Der Verwaltungsmodus „NoAgent“ ist nur für SQL Server 2008 und SQL Server 2008 R2 unter Windows Server 2008 verfügbar. Es handelt sich um den einzigen verfügbaren Verwaltungsmodus für diese Versionen. Für alle anderen Versionen von SQL Server sind die beiden verfügbaren Verwaltungsmodi „Lightweight“ und „Vollständig“. 

Für den Modus „NoAgent“ müssen die Eigenschaften für die SQL Server-Version und die Edition vom Kunden festgelegt werden. Der Modus „Lightweight“ fragt die VM ab, um die Version und Edition der SQL Server-Instanz zu ermitteln.

**Kann ich mich beim SQL-VM-Ressourcenanbieter registrieren, ohne den SQL Server-Lizenztyp anzugeben?**

Nein. Der SQL Server-Lizenztyp ist bei der Registrierung beim SQL-VM-Ressourcenanbieter keine optionale Eigenschaft. Sie müssen in allen Verwaltungsmodi („NoAgent“, „Lightweight“ und „Vollständig“) den SQL Server-Lizenztyp „Nutzungsbasierte Bezahlung“ oder „Azure-Hybridvorteil“ festlegen, wenn Sie eine Registrierung beim SQL-VM-Ressourcenanbieter ausführen.

**Kann ich die SQL Server-IaaS-Erweiterung vom Modus „NoAgent“ auf „Vollständig“ upgraden?**

Nein. Das Upgrade des SQL-Verwaltungsmodus auf „Vollständig“ oder „Lightweight“ steht für den Modus „NoAgent“ nicht zur Verfügung. Dies ist eine technische Einschränkung von Windows Server 2008. Sie müssen für das Betriebssystem zunächst ein Upgrade auf Windows Server 2008 R2 oder höher ausführen. Danach können Sie ein Upgrade auf den Verwaltungsmodus „Vollständig“ ausführen. 

**Kann ich die SQL Server-IaaS-Erweiterung vom Modus „Lightweight“ auf „Vollständig“ upgraden?**

Ja. Das Upgrade des SQL-Verwaltbarkeitsmodus von „Lightweight“ auf „Vollständig“ wird über PowerShell und das Azure-Portal unterstützt. Dazu muss der SQL Server-Dienst neu gestartet werden.

**Kann ich eine Herabstufung der SQL Server-IaaS-Erweiterung vom Modus „Vollständig“ zu den Verwaltungsmodi „NoAgent“ oder „Lightweight“ durchführen?**

Nein. Eine Herabstufung des Verwaltungsmodus für die SQL Server-IaaS-Erweiterung wird nicht unterstützt. Der SQL Server-Verwaltungsmodus kann vom Modus „Vollständig“ nicht auf „Lightweight“ oder „NoAgent“ herabgestuft werden und auch nicht vom Modus „Lightweight“ auf den Modus „NoAgent“. 

Um den Verwaltbarkeitsmodus von „Vollständige Verwaltbarkeit“ zu ändern, heben Sie die [Registrierung](#unregister-vm-from-rp) der SQL Server-VM beim SQL Server-Ressourcenanbieter auf, indem Sie die SQL Server-*Ressource* verwerfen und die SQL Server-VM beim SQL-VM-Ressourcenanbieter in einem anderen Verwaltungsmodus erneut registrieren.

**Kann ich mich über das Azure-Portal beim SQL-VM-Ressourcenanbieter registrieren?**

Nein. Die Registrierung beim SQL-VM-Ressourcenanbieter steht im Azure-Portal nicht zur Verfügung. Die Registrierung beim SQL-VM-Ressourcenanbieter wird nur über die Azure-Befehlszeilenschnittstelle oder PowerShell unterstützt. 

**Kann ich eine VM beim SQL-VM-Ressourcenanbieter registrieren, bevor SQL Server installiert wird?**

Nein. Die VM muss mindestens eine SQL Server-Instanz aufweisen, damit sie beim SQL-VM-Ressourcenanbieter registriert werden kann. Wenn auf der VM keine SQL Server-Instanz vorhanden ist, weist die neue Ressource Microsoft.SqlVirtualMachine einen Fehlerzustand auf.

**Kann ich eine VM bei einem SQL-VM-Ressourcenanbieter registrieren, wenn mehrere SQL Server-Instanzen vorhanden sind?**

Ja. Der SQL-VM-Ressourcenanbieter registriert nur eine SQL Server-Instanz. Wenn mehrere Instanzen vorhanden sind, registriert der SQL-VM-Ressourcenanbieter die SQL Server-Standardinstanz. Gibt es keine Standardinstanz, wird nur die Registrierung im Lightweight-Modus unterstützt. Für ein Upgrade vom Verwaltbarkeitsmodus „Lightweight“ auf „Vollständig“ sollte entweder die SQL Server-Standardinstanz vorhanden sein, oder die VM sollte nur eine benannte SQL Server-Instanz aufweisen.

**Kann ich eine SQL Server-Failoverclusterinstanz beim SQL-VM-Ressourcenanbieter registrieren?**

Ja. SQL Server-Failoverclusterinstanzen auf einer Azure-VM können beim SQL-VM-Ressourcenanbieter im Lightweight-Modus registriert werden. Ein Upgrade von SQL Server-Failoverclusterinstanzen auf den Verwaltungsmodus „Vollständig“ ist jedoch nicht möglich.

**Kann ich meine VM beim SQL-VM-Ressourcenanbieter registrieren, wenn eine Always On-Verfügbarkeitsgruppe konfiguriert wurde?**

Ja. Es gibt keine Einschränkungen beim Registrieren einer SQL Server-Instanz auf einer Azure-VM beim SQL-VM-Ressourcenanbieter, wenn Sie einer Always On-Verfügbarkeitsgruppenkonfiguration angehören.

**Welche Kosten fallen für das Registrieren beim SQL-VM-Ressourcenanbieter oder beim Upgraden auf den Verwaltbarkeitsmodus „Vollständig“ an?**
None (Keine): Für das Registrieren beim SQL-VM-Ressourcenanbieter oder das Verwenden eines der drei Verwaltbarkeitsmodi fallen keine Gebühren an. Das Verwalten Ihrer SQL Server-VM mit dem Ressourcenanbieter ist vollständig kostenlos. 

**Welche Auswirkungen hat ein Verwenden der verschiedenen Verwaltbarkeitsmodi auf die Leistung?**
Es gibt keine Auswirkungen, wenn der Verwaltbarkeitsmodus *NoAgent* oder *Lightweight* verwendet wird. Es gibt minimale Auswirkungen, wenn der Verwaltbarkeitsmodus *Vollständig* von zwei Diensten verwendet wird, die in das Betriebssystem installiert wurden. Diese können über den Task-Manager überwacht und in der integrierten Konsole „Dienste“ in Windows angezeigt werden. 

Die beiden Dienstnamen lauten:
- `SqlIaaSExtensionQuery` (Anzeigename – `Microsoft SQL Server IaaS Query Service`)
- `SQLIaaSExtension` (Anzeigename – `Microsoft SQL Server IaaS Agent`)


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für SQL Server auf virtuellen Windows-Computern](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server auf Windows-VMs – Versionshinweise](virtual-machines-windows-sql-server-iaas-release-notes.md)
