---
title: Ändern des Lizenzierungsmodells für eine SQL-VM in Azure
description: Es wird beschrieben, wie Sie die Lizenzierung für einen virtuellen SQL Server-Computer in Azure von „nutzungsbasierter Bezahlung“ (Pay-as-you-go, PAYG) in „Bring-Your-Own-License“ (BYOL) ändern, indem Sie den Azure-Hybridvorteil nutzen.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: b09d808201d58b571b2fe5ceb2e228d4e1c21d11
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91316952"
---
# <a name="change-the-license-model-for-a-sql-virtual-machine-in-azure"></a>Ändern des Lizenzierungsmodells für einen virtuellen SQL-Computer in Azure
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]


In diesem Artikel wird beschrieben, wie Sie das Lizenzierungsmodell für einen virtuellen SQL Server-Computer (VM) in Azure ändern, indem Sie den neuen SQL Server-VM-Ressourcenanbieter **Microsoft.SqlVirtualMachine** verwenden.

Es gibt drei Lizenzierungsmodelle für eine VM, die SQL Server hostet: nutzungsbasierte Bezahlung, Azure-Hybridvorteil (Azure Hybrid Benefit, AHB) und Notfallwiederherstellung (Disaster Recovery, DR). Sie können das Lizenzierungsmodell Ihrer SQL Server-VM im Azure-Portal, mit der Azure-Befehlszeilenschnittstelle (Azure CLI) oder mit PowerShell ändern. 

- **Nutzungsbasierte Bezahlung** bedeutet, dass die pro Sekunde berechneten Kosten für die Ausführung des virtuellen Azure-Computers die Kosten für die SQL Server-Lizenz enthalten.
- Der [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) ermöglicht Ihnen die Verwendung Ihrer eigenen SQL Server-Lizenz mit einer VM, auf der SQL Server ausgeführt wird. 
- Der Lizenztyp **Notfallwiederherstellung** wird für das [kostenlose DR-Replikat](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure) in Azure verwendet. 

Beim Azure-Hybridvorteil können Sie SQL Server-Lizenzen mit Software Assurance („qualifizierte Lizenz“) auf Azure-VMs verwenden. Beim Azure-Hybridvorteil wird Kunden nicht die Verwendung einer SQL Server-Lizenz auf einem virtuellen Computer in Rechnung gestellt. Sie müssen jedoch weiterhin die Kosten des zugrunde liegenden Cloudcomputings (d. h. den Basistarif), des Speichers und der Sicherungen bezahlen. Außerdem müssen sie für die E/A-Vorgänge bezahlen, die mit der Nutzung der Dienste in Verbindung stehen (sofern zutreffend).

Gemäß den Microsoft-Produktbestimmungen: „...müssen Kunden beim Konfigurieren von Workloads in Azure angeben, dass sie Azure SQL-Datenbank (verwaltete Instanz, Pool für elastische Datenbanken und Einzeldatenbank), Azure Data Factory, SQL Server Integration Services oder SQL Server-VMs im Rahmen des Azure-Hybridvorteils für SQL Server verwenden“.

Um die Verwendung des Azure-Hybridvorteils für SQL Server auf einer Azure-VM anzugeben und die Konformität zu wahren, stehen drei Optionen für Sie zur Auswahl:

- Stellen Sie eine VM mit einem „Bring-Your-Own-License“-SQL Server-Image aus dem Azure Marketplace bereit. Diese Option ist nur für Kunden verfügbar, die über ein Enterprise Agreement verfügen.
- Stellen Sie eine VM mit einem SQL Server-Image mit nutzungsbasierter Bezahlung aus dem Azure Marketplace bereit, und aktivieren Sie den Azure-Hybridvorteil.
- Installieren Sie SQL Server selbst auf einer Azure-VM, [registrieren Sie sich manuell bei Ihrem SQL-VM-Ressourcenanbieter](sql-vm-resource-provider-register.md), und aktivieren Sie den Azure-Hybridvorteil.

Der SQL Server-Lizenztyp kann beim Bereitstellen der VM konfiguriert und danach jederzeit geändert werden. Der Wechsel zwischen den Lizenzmodellen führt zu keinen Ausfallzeiten. Die VM oder der SQL Server-Dienst startet nicht neu, es entstehen keine zusätzlichen Kosten, und das neue Modell ist sofort wirksam. Durch die Aktivierung des Azure-Hybridvorteils werden die Kosten *reduziert*.

## <a name="prerequisites"></a>Voraussetzungen

Das Ändern des Lizenzierungsmodells Ihrer SQL Server-VM stellt die folgenden Anforderungen: 

- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- Eine [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision), die beim [SQL-VM-Ressourcenanbieter](sql-vm-resource-provider-register.md) registriert ist.
- [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) ist eine Voraussetzung für die Verwendung des [Azure-Hybridvorteils](https://azure.microsoft.com/pricing/hybrid-benefit/). 


## <a name="vms-already-registered-with-the-resource-provider"></a>Bereits beim Ressourcenanbieter registrierte VMs 

# <a name="the-azure-portal"></a>[Azure-Portal](#tab/azure-portal)

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Sie können das Lizenzierungsmodell direkt über das Portal ändern: 

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und starten Sie die Ressource [Virtueller SQL-Computer](manage-sql-vm-portal.md#access-the-sql-virtual-machines-resource) für Ihre SQL Server-VM. 
1. Wählen Sie **Konfigurieren** unter **Einstellungen** aus. 
1. Wählen Sie **Azure-Hybridvorteil** aus, und aktivieren Sie das Kontrollkästchen, um zu bestätigen, dass Sie über eine SQL Server-Lizenz mit Software Assurance verfügen. 
1. Wählen Sie **Übernehmen** am unteren Rand der Seite **Konfigurieren** aus. 

![Azure-Hybridvorteil im Portal](./media/licensing-model-azure-hybrid-benefit-ahb-change/ahb-in-portal.png)


# <a name="the-azure-cli"></a>[Die Azure-CLI](#tab/azure-cli)

Sie können die Azure CLI (Azure-Befehlszeilenschnittstelle) nutzen, um das Lizenzierungsmodell zu wechseln.  


**Azure-Hybridvorteil**

```azurecli-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

**Nutzungsbasierte Bezahlung**: 

```azurecli-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type PAYG

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type PAYG
```

**Disaster Recovery** (DR, Notfallwiederherstellung)

```azurecli-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type DR

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type DR
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Sie können Ihr Lizenzierungsmodell auch mit PowerShell ändern.

**Azure-Hybridvorteil**

```powershell-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
Update-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name> -LicenseType AHUB
```

**Nutzungsbasierte Bezahlung**

```powershell-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
Update-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name> -LicenseType PAYG
```

**Notfallwiederherstellung** 

```powershell-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
Update-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name> -LicenseType DR
```

---

## <a name="vms-not-registered-with-the-resource-provider"></a>Nicht beim Ressourcenanbieter registrierte VMs

Wenn Sie eine SQL Server-VM mit Images mit nutzungsbasierter Bezahlung aus dem Azure Marketplace bereitgestellt haben, lautet der SQL Server-Lizenztyp „nutzungsbasierte Bezahlung“. Wenn Sie eine SQL Server-VM mit einem Bring-Your-Own-License-Image aus dem Azure Marketplace bereitgestellt haben, lautet der Lizenztyp „AHUB“. Alle über standardmäßige (nutzungsbasierte Bezahlung) oder Bring-Your-Own-License-Azure Marketplace-Images bereitgestellten SQL Server-VMs werden automatisch beim SQL-VM-Ressourcenanbieter registriert, sodass der [Lizenztyp](#vms-already-registered-with-the-resource-provider) geändert werden kann.

Sie sind nur dazu berechtigt, SQL Server auf einer Azure-VM über den Azure-Hybridvorteil selbst zu installieren. Sie sollten [diese VMs beim SQL-VM-Ressourcenanbieter registrieren](sql-vm-resource-provider-register.md), indem Sie die SQL Server-Lizenz auf den Azure-Hybridvorteil festlegen, um die Nutzung des Azure-Hybridvorteils gemäß der Microsoft-Produktbestimmungen anzugeben.

Sie können den Lizenztyp „Nutzungsbasierte Bezahlung“ oder Azure-Hybridvorteil einer SQL Server-VM nur ändern, wenn die SQL Server-VM beim SQL-VM-Ressourcenanbieter registriert ist.

## <a name="remarks"></a>Bemerkungen

- Azure CSP-Kunden (Azure Cloud Solution Provider) können den Azure-Hybridvorteil nutzen, indem sie zuerst eine VM mit nutzungsbasierter Bezahlung bereitstellen und sie dann in Bring-Your-Own-License umwandeln, wenn sie über aktive Software Assurance verfügen.
- Wenn Sie Ihre SQL Server-VM-Ressource verwerfen, kehren Sie wieder zur hartcodierten Lizenzeinstellung des Images zurück. 
- Die Möglichkeit, das Lizenzierungsmodell zu ändern, ist ein Feature des SQL-VM-Ressourcenanbieters. Bei der Bereitstellung eines Azure Marketplace-Images über das Azure-Portal wird eine SQL Server-VM automatisch beim Ressourcenanbieter registriert. Kunden, die SQL Server selbst installieren, müssen [ihre SQL Server-VM jedoch manuell registrieren](sql-vm-resource-provider-register.md). 
- Das Hinzufügen einer SQL Server-VM zu einer Verfügbarkeitsgruppe erfordert die Neuerstellung der VM. Daher kehren alle VMs, die einer Verfügbarkeitsgruppe hinzugefügt werden, zum Standardlizenztyp „Nutzungsbasierte Bezahlung“ zurück. Der Azure-Hybridvorteil muss erneut aktiviert werden. 


## <a name="limitations"></a>Einschränkungen

Das Ändern des Lizenzmodells
   - ist nur für Kunden mit [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-overview) verfügbar.
   - wird nur für die Standard Edition und Enterprise Edition von SQL Server unterstützt. unterstützt keine Lizenzänderungen für Express, Web und Developer. 
   - wird nur für virtuelle Computer unterstützt, die mit dem Azure Resource Manager-Modell bereitgestellt wurden. unterstützt keine SQL Server-VMs, die mit dem klassischen Modell bereitgestellt wurden. 
   - Nur für öffentliche oder Azure Government-Clouds verfügbar. 
   - wird nur auf virtuellen Computern unterstützt, die eine einzelne Netzwerkschnittstelle haben. 


## <a name="known-errors"></a>Bekannte Fehler

Überprüfen Sie die allgemein bekannten Fehler und ihre Lösungen. 

**Die Ressource „Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group>“ unter der Ressourcengruppe „\<resource-group>“ wurde nicht gefunden.**

Dieser Fehler tritt auf, wenn Sie versuchen, das Lizenzierungsmodell auf einer SQL Server-VM zu ändern, die nicht beim SQL-VM-Ressourcenanbieter registriert wurde:

`The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group>' under resource group '\<resource-group>' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set.`

Sie müssen Ihr Abonnement beim Ressourcenanbieter registrieren und anschließend [Ihre SQL Server-VM beim Ressourcenanbieter registrieren](sql-vm-resource-provider-register.md). 


**Der virtuelle Computer „\<vmname\>“ hat mehr als eine zugeordnete Netzwerkschnittstelle**

Dieser Fehler tritt auf virtuellen Computern auf, die über mehr als eine Netzwerkschnittstelle verfügen. Entfernen Sie eine der Netzwerkschnittstellen, bevor Sie das Lizenzierungsmodell ändern. Obwohl Sie die Netzwerkschnittstelle nach dem Ändern des Lizenzmodells wieder zur VM hinzufügen können, werden Vorgänge im Azure-Portal wie die automatische Sicherung und das automatische Patchen nicht mehr unterstützt. 


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern](frequently-asked-questions-faq.md)
* [Preisinformationen für SQL Server auf virtuellen Windows-Computern](pricing-guidance.md)
* [SQL Server auf Windows-VMs – Versionshinweise](../../database/doc-changes-updates-release-notes.md)


