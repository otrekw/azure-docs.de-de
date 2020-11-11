---
title: Die Erweiterung für den SQL Server-IaaS-Agent
description: In diesem Artikel erfahren Sie, wie die SQL Server-IaaS-Agent-Erweiterung bei der Automatisierung bestimmter SQL Server-Verwaltungsaufgaben auf Azure-VMs hilft. Hierzu gehören Features wie automatisierte Sicherungen und Patches, Azure Key Vault-Integration, Lizenzierungsverwaltung, Speicherkonfiguration und zentrale Verwaltung aller SQL Server-VM-Instanzen.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/30/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 070058eae36bf4f8546cfcf4beb85ac5023e9c79
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286173"
---
# <a name="what-is-the-sql-server-iaas-agent-extension"></a>Die Erweiterung für den SQL Server-IaaS-Agent
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]


Die Erweiterung für den SQL Server-IaaS-Agent (SqlIaasExtension) kann auf virtuellen Azure-Computern ausgeführt werden, um Verwaltungsaufgaben zu automatisieren. 

Dieser Artikel bietet eine Übersicht über die Erweiterung. Informationen zum Installieren der SQL Server-IaaS-Erweiterung für SQL Server auf Azure-VMs finden Sie in den Artikeln zur [automatischen Installation](sql-vm-resource-provider-automatic-registration.md) und zur [Installation auf einzelnen VMs](sql-vm-resource-provider-register.md) bzw. zur [Masseninstallation auf VMs](sql-vm-resource-provider-bulk-register.md). 

## <a name="overview"></a>Übersicht

Die Erweiterung für den SQL Server-IaaS-Agent bietet für SQL Server auf Azure-VMs zahlreiche Vorteile: 

- **Featurevorteile:** Die Erweiterung macht verschiedene Automatisierungsfeatures wie Portalverwaltung, flexible Lizenzierung, automatisierte Sicherungen und Patches und andere verfügbar. Weitere Informationen finden Sie unter [Featurevorteile](#feature-benefits) weiter unten in diesem Artikel. 

- **Compliance** : Mit der Erweiterung können Sie auch einfacher Ihrer Verpflichtung gemäß den Nutzungsbedingungen zum Produkt nachkommen, Microsoft zu benachrichtigen, wenn der Azure-Hybridvorteil aktiviert wurde. Auf diese Weise können Sie darauf verzichten, für jede Ressource ein Lizenzregistrierungsformular zu verwalten.  

- **Free:** Die Erweiterung ist in allen drei Verwaltbarkeitsmodi vollständig kostenlos. Es fallen keine zusätzlichen Kosten für den Ressourcenanbieter oder das Ändern von Verwaltungsmodi an. 

- **Vereinfachte Lizenzverwaltung** : Die Erweiterung vereinfacht die SQL Server-Lizenzverwaltung. Außerdem können Sie über das [Azure-Portal](manage-sql-vm-portal.md), die Azure-Befehlszeilenschnittstelle oder PowerShell schnell SQL Server-VMs mit aktiviertem Azure-Hybridvorteil identifizieren: 

   # <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

   ```azurecli-interactive
   $vms = az sql vm list | ConvertFrom-Json
   $vms | Where-Object {$_.sqlServerLicenseType -eq "AHUB"}
   ```

   # <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

   ```powershell-interactive
   Get-AzSqlVM | Where-Object {$_.LicenseType -eq 'AHUB'}
   ```

   ---


> [!IMPORTANT]
> Die SQL-IaaS-Agent-Erweiterung sammelt Daten ausschließlich, um Kunden zu ermöglichen, bei der Verwendung von SQL Server in Azure Virtual Machines optionale Vorteile zu nutzen. Microsoft verwendet diese Daten ohne vorherige Zustimmung des Kunden nicht für Lizenzierungsüberprüfungen. Weitere Informationen finden Sie unter [Ergänzende Datenschutzbestimmungen zu SQL Server](/sql/sql-server/sql-server-privacy#non-personal-data).


## <a name="feature-benefits"></a>Featurevorteile 

Die SQL Server-IaaS-Agent-Erweiterung macht eine Reihe von Featurevorteilen für die Verwaltung Ihrer SQL Server-VMs verfügbar. 

Diese Vorteile werden in der folgenden Tabelle genauer erläutert: 


| Funktion | Beschreibung |
| --- | --- |
| **Portalverwaltung** | Ermöglicht die [Verwaltung im Portal](manage-sql-vm-portal.md), sodass Sie alle Ihre SQL Server-VMs an einem Ort anzeigen und SQL-spezifische Features direkt über das Portal aktivieren und deaktivieren können. 
| **Automatisierte Sicherung** |Automatisiert die Planung von Sicherungen für alle Datenbanken entweder der Standardinstanz oder einer [ordnungsgemäß installierten](frequently-asked-questions-faq.md#administration) benannten Instanz von SQL Server in der VM. Weitere Informationen finden Sie unter [Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (Resource Manager)](automated-backup-sql-2014.md). |
| **Automatisiertes Patchen** |Mit diesem Feature können Sie ein Wartungsfenster konfigurieren, in dem wichtige Windows- und SQL Server-Sicherheitsupdates für die VM ausgeführt werden. Auf diese Weise werden Updates für Ihre Workload während der Spitzenzeiten vermieden. Weitere Informationen finden Sie unter [Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (Resource Manager)](automated-patching.md). |
| **Azure Key Vault-Integration** |Mit diesem Dienst können Sie Azure Key Vault auf Ihrem virtuellen SQL Server-Computer automatisch installieren und konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren der Azure Key Vault-Integration für SQL Server auf virtuellen Azure-Computern (Resource Manager)](azure-key-vault-integration-configure.md). |
| **Flexible Lizenzierung** | Durch einen [nahtlosen Wechsel](licensing-model-azure-hybrid-benefit-ahb-change.md) zwischen den Lizenzmodellen „Bring-Your-Own-License“ (BYOL, auch als „Azure-Hybridvorteil“ bezeichnet) und „Nutzungsbasierte Bezahlung“ können Sie Kosten sparen. | 
| **Flexible Version/Edition** | Wenn Sie die [Version](change-sql-server-version.md) oder [Edition](change-sql-server-edition.md) von SQL Server ändern möchten, können Sie einfach die Metadaten im Azure-Portal aktualisieren, ohne die gesamte SQL Server-VM erneut bereitstellen zu müssen.  | 


## <a name="management-modes"></a>Verwaltungsmodi

Die SQL-IaaS-Erweiterung bietet drei Verwaltungsmodi:

- Der Modus **Lightweight** erfordert keinen Neustart von SQL Server, unterstützt jedoch nur das Ändern des Lizenztyps und der Edition von SQL Server. Verwenden Sie diese Option für SQL Server-VMs mit mehreren Instanzen oder für die Teilnahme an einer Failoverclusterinstanz (FCI). In diesem Verwaltungsmodus werden die Binärdateien der SQL-IaaS-Agent-Erweiterung auf dem Computer gespeichert, der Agent wird jedoch nicht installiert. Der Lightweight-Modus ist der Standardverwaltungsmodus bei Verwendung des Features [Automatische Registrierung](sql-vm-resource-provider-automatic-registration.md) und wenn bei der manuellen Registrierung kein Verwaltungstyp angegeben wurde. Die Verwendung des Modus „Lightweight“ hat keine Auswirkungen auf den Arbeitsspeicher oder die CPU. Außerdem fallen keine Kosten an. Es wird empfohlen, die SQL Server-VM zunächst im Modus „Lightweight“ zu registrieren und dann während eines Fenster für die geplante Wartung ein Upgrade auf den Modus „Vollständig“ auszuführen.

- Der Modus **Vollständig** bietet alle Funktionen, erfordert aber einen Neustart des SQL Server-Computers sowie Systemadministratorberechtigungen. Verwenden Sie diesen Modus zum Verwalten einer SQL Server-VM mit einer einzelnen Instanz. Für den Modus „Vollständig“ werden zwei Windows-Dienste installiert, die minimale Auswirkungen auf den Arbeitsspeicher und die CPU haben. Diese Dienste können über den Task-Manager überwacht werden. Mit der Verwendung des Verwaltbarkeitsmodus „Vollständig“ sind keine Kosten verbunden. 

- Der Modus **NoAgent** ist speziell für Installationen von SQL Server 2008 und SQL Server 2008 R2 unter Windows Server 2008 vorgesehen. Es gibt keine Auswirkung auf den Arbeitsspeicher oder die CPU, wenn der Modus „NoAgent“ verwendet wird. Mit der Verwendung des Verwaltbarkeitsmodus „NoAgent“ sind keine Kosten verbunden. 

Über Azure PowerShell können Sie den aktuellen Modus des SQL Server-IaaS-Agents anzeigen: 

  ```powershell-interactive
  # Get the SqlVirtualMachine
  $sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
  $sqlvm.SqlManagementType
  ```

## <a name="installation"></a>Installation

Die SQL Server-IaaS-Agent-Erweiterung wird installiert, wenn Sie Ihre SQL Server-VMs beim SQL-VM-Ressourcenanbieter registrieren. Durch die Registrierung beim Ressourcenanbieter wird die _Ressource_ **Virtueller SQL-Computer** in Ihrem Abonnement erstellt. Dabei handelt es sich um eine _andere_ Ressource als die für Ihre VM. Wenn Sie die Registrierung Ihrer SQL Server-VM beim Ressourcenanbieter aufheben, wird die _Ressource_ **Virtueller SQL-Computer** entfernt, während der tatsächliche virtuelle Computer jedoch erhalten bleibt.

Durch das Bereitstellen eines SQL Server-VM-Azure Marketplace-Images über das Azure-Portal wird die SQL Server-VM automatisch beim Ressourcenanbieter registriert. Wenn Sie SQL Server aber selbst auf einer Azure-VM installieren oder eine Azure-VM von einer benutzerdefinierten VHD bereitstellen, müssen Sie Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter registrieren, um die SQL-IaaS-Agent-Erweiterung zu installieren. 

Um die Erweiterung zu installieren, registrieren Sie die SQL Server-VM beim Ressourcenanbieter:
- [Automatisch für alle aktuellen und zukünftigen VMs in einem Abonnement](sql-vm-resource-provider-automatic-registration.md)
- [Für eine einzelne VM](sql-vm-resource-provider-register.md)
- [Für mehrere VMs in einem Massenvorgang](sql-vm-resource-provider-bulk-register.md)

### <a name="named-instance-support"></a>Unterstützung für benannte Instanzen

Die SQL Server-IaaS-Erweiterung funktioniert mit einer benannten Instanz von SQL Server, wenn dies die einzige SQL Server Instanz ist, die auf der VM verfügbar ist. Die Erweiterung kann nicht auf VMs installiert werden, auf denen sich SQL Server-Instanzen befinden. 

Wenn Sie eine benannte Instanz von SQL Server verwenden möchten, stellen Sie eine Azure-VM bereit, installieren Sie eine einzelne benannte SQL Server-Instanz, und registrieren Sie sie dann beim [SQL-VM-Ressourcenanbieter](sql-vm-resource-provider-register.md), um die Erweiterung zu installieren.

Falls Sie stattdessen eine benannte Instanz mit einem SQL Server-Image aus dem Azure Marketplace verwenden möchten, führen Sie die folgenden Schritte aus: 

   1. Stellen Sie eine SQL Server-VM vom Azure Marketplace bereit. 
   1. [Heben Sie die Registrierung der SQL Server-VM beim SQL-VM-Ressourcenanbieter auf.](sql-vm-resource-provider-register.md#unregister-from-rp) 
   1. Deinstallieren Sie SQL Server in der SQL Server-VM vollständig.
   1. Installieren Sie SQL Server mit einer benannten Instanz in der SQL Server-VM. 
   1. Installieren Sie die SQL-IaaS-Agent-Erweiterung, indem Sie die [SQL Server-VM beim SQL-VM-Ressourcenanbieter registrieren](sql-vm-resource-provider-register.md#register-with-rp). 

## <a name="verify-status-of-extension"></a>Überprüfen des Status der Erweiterung

Sie können den Status der Erweiterung über das Azure-Portal oder mit Azure PowerShell überprüfen. 


### <a name="azure-portal"></a>Azure-Portal

Vergewissern Sie sich im Azure-Portal, dass die Erweiterung installiert ist. 

Wählen Sie im Bereich der VM die Option **Alle Einstellungen** und dann **Erweiterungen** aus. Die Erweiterung **SqlIaasExtension** sollte aufgeführt sein.

![Status der SQL Server-IaaS-Agent-Erweiterung im Azure-Portal](./media/sql-server-iaas-agent-extension-automate-management/azure-rm-sql-server-iaas-agent-portal.png)


### <a name="azure-powershell"></a>Azure PowerShell

Sie können auch das Azure PowerShell-Cmdlet **Get-AzVMSqlServerExtension** verwenden:

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

Der Befehl oben überprüft, ob der Agent installiert ist, und stellt allgemeine Statusinformationen bereit. Mit den folgenden Befehlen können Sie spezifische Statusinformationen zu automatischen Sicherungen und Patches abrufen:

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```


## <a name="limitations"></a>Einschränkungen

Die SQL-IaaS-Agent-Erweiterung unterstützt nur Folgendes: 

- Über den Azure Resource Manager bereitgestellte SQL Server-VMs. SQL Server-VMs, die mit dem klassischen Modell bereitgestellt wurden, werden nicht unterstützt. 
- In der öffentlichen Cloud oder Azure Government-Cloud bereitgestellte SQL Server-VMs. Bereitstellungen in anderen privaten Clouds oder Government Clouds werden nicht unterstützt. 


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 

**Sollte ich meine SQL Server-VM registrieren, die über ein SQL Server-Image im Azure Marketplace bereitgestellt wird?**

Nein. Microsoft registriert VMs, die über die SQL Server-Images im Azure Marketplace bereitgestellt werden, automatisch. Eine Registrierung beim SQL-VM-Ressourcenanbieter ist nur erforderlich, wenn die VM *nicht* über die SQL Server-Images im Azure Marketplace bereitgestellt und SQL Server selbst installiert wurde.

**Ist der SQL-VM-Ressourcenanbieter für alle Kunden verfügbar?** 

Ja. Kunden sollten Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter registrieren, wenn sie kein SQL Server-Image aus dem Azure Marketplace verwenden, sondern stattdessen SQL Server selbst installiert oder ihre benutzerdefinierte VHD verwendet haben. Beim SQL-VM-Ressourcenanbieter können VMs im Besitz aller Abonnementtypen (direkt, Enterprise Agreement und Cloud Solution Provider) registriert werden.

**Was ist der Standardverwaltungsmodus bei der Registrierung beim SQL-VM-Ressourcenanbieter?**

Der Standardverwaltungsmodus bei der Registrierung beim SQL-VM-Ressourcenanbieter ist *Lightweight*. Wenn die Eigenschaft zur Verwaltung von SQL Server bei der Registrierung beim SQL-VM-Ressourcenanbieter nicht festgelegt wurde, wird der Modus „Lightweight“ festgelegt und der SQL Server-Dienst nicht neu gestartet. Es wird empfohlen, die Registrierung beim SQL-VM-Ressourcenanbieter zunächst im Modus „Lightweight“ auszuführen und dann während eines Wartungsfensters ein Upgrade auf den Modus „Vollständig“ auszuführen. Der Standardverwaltungsmodus ist auch dann „Lightweight“, wenn das Feature zur [automatischen Registrierung](sql-vm-resource-provider-automatic-registration.md) verwendet wird.

**Was sind die Voraussetzungen für die Registrierung beim SQL-VM-Ressourcenanbieter?**

Als Voraussetzung für die Registrierung beim SQL-VM-Ressourcenanbieter muss lediglich SQL Server auf der VM installiert sein. Beachten Sie, dass bei einer Installation der SQL-IaaS-Agent-Erweiterung im Modus „Vollständig“ ein Neustart des SQL Server-Diensts erfolgt. Es wird daher empfohlen, dies während eines Wartungsfensters durchzuführen.

**Wird bei der Registrierung beim SQL-VM-Ressourcenanbieter ein Agent auf meiner VM installiert?**

Ja, bei der Registrierung beim SQL-VM-Ressourcenanbieter im Verwaltbarkeitsmodus „Vollständig“ wird auf der VM ein Agent installiert. Bei einer Registrierung in den Modi „Lightweight“ oder „NoAgent“ ist dies nicht der Fall. 

Bei der Registrierung beim SQL-VM-Ressourcenanbieter im Modus „Lightweight“ werden nur die *Binärdateien* der SQL-IaaS-Agent-Erweiterung auf die VM kopiert, der Agent wird jedoch nicht installiert. Diese Binärdateien werden dann für die Installation des Agents verwendet, wenn der Verwaltungsmodus in „Vollständig“ geändert wird.


**Wird SQL Server durch die Registrierung beim SQL-VM-Ressourcenanbieter auf meiner VM neu gestartet?**

Das hängt von dem während der Registrierung angegebenen Modus ab. Wenn als Modus „Lightweight“ oder „NoAgent“ angegeben wurde, wird der SQL Server-Dienst nicht neu gestartet. Wird als Verwaltungsmodus jedoch „Vollständig“ angegeben, erfolgt ein Neustart des SQL Server-Diensts. Beim Feature für die automatische Registrierung werden Ihre SQL Server-VMs im Modus „Lightweight“ registriert, es sei denn, die Windows Server-Version ist 2008. In diesem Fall wird die SQL Server-VM im Modus „NoAgent“ registriert. 

**Worin besteht der Unterschied zwischen den Verwaltungsmodi „Lightweight“ und „NoAgent“ bei der Registrierung beim SQL-VM-Ressourcenanbieter?** 

Der Verwaltungsmodus „NoAgent“ ist der einzige verfügbare Modus für SQL Server 2008 und SQL Server 2008 R2 unter Windows Server 2008. Für alle anderen Versionen von Windows Server sind die beiden Verwaltbarkeitsmodi „Lightweight“ und „Vollständig“ verfügbar. 

Für den Modus „NoAgent“ müssen die Eigenschaften für die SQL Server-Version und -Edition vom Kunden festgelegt werden. Der Modus „Lightweight“ fragt die VM ab, um die Version und Edition der SQL Server-Instanz zu ermitteln.

**Kann ich mich beim SQL-VM-Ressourcenanbieter registrieren, ohne den SQL Server-Lizenztyp anzugeben?**

Nein. Der SQL Server-Lizenztyp ist bei der Registrierung beim SQL-VM-Ressourcenanbieter keine optionale Eigenschaft. Sie müssen in allen Verwaltbarkeitsmodi („NoAgent“, „Lightweight“ und „Vollständig“) den SQL Server-Lizenztyp „Nutzungsbasierte Bezahlung“ oder „Azure-Hybridvorteil“ festlegen, wenn Sie eine Registrierung beim SQL-VM-Ressourcenanbieter ausführen. Wenn Sie eine der kostenlosen Versionen von SQL Server installiert haben (z. B. die Editionen Developer oder Evaluation), müssen Sie bei der Registrierung die Lizenzierung mit nutzungsbasierter Bezahlung angeben. Der Azure-Hybridvorteil ist nur für kostenpflichtige Versionen von SQL Server verfügbar, z. B. für die Editionen Enterprise und Standard.

**Kann ich die SQL Server-IaaS-Erweiterung vom Modus „NoAgent“ auf „Vollständig“ upgraden?**

Nein. Das Upgrade des Verwaltbarkeitsmodus auf „Vollständig“ oder „Lightweight“ steht für den Modus „NoAgent“ nicht zur Verfügung. Dies ist eine technische Einschränkung von Windows Server 2008. Sie müssen für das Betriebssystem zunächst ein Upgrade auf Windows Server 2008 R2 oder höher ausführen. Danach können Sie ein Upgrade auf den Verwaltungsmodus „Vollständig“ ausführen. 

**Kann ich die SQL Server-IaaS-Erweiterung vom Modus „Lightweight“ auf „Vollständig“ upgraden?**

Ja. Das Upgrade des Verwaltbarkeitsmodus von „Lightweight“ auf „Vollständig“ wird über Azure PowerShell und das Azure-Portal unterstützt. Dadurch wird ein Neustart des SQL Server-Diensts auslöst.

**Kann ich eine Herabstufung der SQL Server-IaaS-Erweiterung vom Modus „Vollständig“ auf die Verwaltungsmodi „NoAgent“ oder „Lightweight“ durchführen?**

Nein. Eine Herabstufung des Verwaltungsmodus für die SQL Server-IaaS-Erweiterung wird nicht unterstützt. Der Verwaltbarkeitsmodus kann vom Modus „Vollständig“ nicht auf „Lightweight“ oder „NoAgent“ herabgestuft werden und auch nicht vom Modus „Lightweight“ auf den Modus „NoAgent“. 

Um den Verwaltbarkeitsmodus von „Vollständig“ zu ändern, heben Sie die [Registrierung](sql-vm-resource-provider-register.md#unregister-from-rp) der SQL Server-VM beim SQL-VM-Ressourcenanbieter auf, indem Sie die SQL-VM- _Ressource_ löschen und die SQL Server-VM beim SQL-VM-Ressourcenanbieter in einem anderen Verwaltbarkeitsmodus erneut registrieren.

**Kann ich mich über das Azure-Portal beim SQL-VM-Ressourcenanbieter registrieren?**

Nein. Die Registrierung beim SQL-VM-Ressourcenanbieter steht im Azure-Portal nicht zur Verfügung. Die Registrierung beim SQL-VM-Ressourcenanbieter wird nur über die Azure-Befehlszeilenschnittstelle oder Azure PowerShell unterstützt. 

**Kann ich eine VM beim SQL-VM-Ressourcenanbieter registrieren, bevor SQL Server installiert wird?**

Nein. Die VM muss mindestens eine SQL Server-Instanz (Datenbank-Engine) aufweisen, damit sie beim SQL-VM-Ressourcenanbieter registriert werden kann. Wenn auf der VM keine SQL Server-Instanz vorhanden ist, weist die neue Ressource Microsoft.SqlVirtualMachine einen Fehlerzustand auf.

**Kann ich eine VM bei einem SQL-VM-Ressourcenanbieter registrieren, wenn mehrere SQL Server-Instanzen vorhanden sind?**

Ja. Der SQL-VM-Ressourcenanbieter registriert nur eine SQL Server-Instanz (Datenbank-Engine). Wenn mehrere Instanzen vorhanden sind, registriert der SQL-VM-Ressourcenanbieter die SQL Server-Standardinstanz. Gibt es keine Standardinstanz, wird nur die Registrierung im Lightweight-Modus unterstützt. Für ein Upgrade vom Verwaltbarkeitsmodus „Lightweight“ auf „Vollständig“ sollte entweder die SQL Server-Standardinstanz vorhanden sein, oder die VM sollte nur eine benannte SQL Server-Instanz aufweisen.

**Kann ich eine SQL Server-Failoverclusterinstanz beim SQL-VM-Ressourcenanbieter registrieren?**

Ja. SQL Server-Failoverclusterinstanzen auf einer Azure-VM können beim SQL-VM-Ressourcenanbieter im Lightweight-Modus registriert werden. Ein Upgrade von SQL Server-Failoverclusterinstanzen auf den Verwaltungsmodus „Vollständig“ ist jedoch nicht möglich.

**Kann ich meine VM beim SQL-VM-Ressourcenanbieter registrieren, wenn eine Always On-Verfügbarkeitsgruppe konfiguriert wurde?**

Ja. Es gibt keine Einschränkungen beim Registrieren einer SQL Server-Instanz auf einer Azure-VM beim SQL-VM-Ressourcenanbieter, wenn Sie einer Always On-Verfügbarkeitsgruppenkonfiguration angehören.

**Welche Kosten fallen für das Registrieren beim SQL-VM-Ressourcenanbieter oder beim Upgraden auf den Verwaltbarkeitsmodus „Vollständig“ an?**

Keine. Für das Registrieren beim SQL-VM-Ressourcenanbieter oder das Verwenden eines der drei Verwaltbarkeitsmodi fallen keine Gebühren an. Das Verwalten Ihrer SQL Server-VM mit dem Ressourcenanbieter ist vollständig kostenlos. 

**Welche Auswirkungen hat ein Verwenden der verschiedenen Verwaltbarkeitsmodi auf die Leistung?**

Es gibt keine Auswirkungen, wenn der Verwaltbarkeitsmodus *NoAgent* oder *Lightweight* verwendet wird. Es gibt minimale Auswirkungen, wenn der Verwaltbarkeitsmodus *Vollständig* von zwei Diensten verwendet wird, die in das Betriebssystem installiert wurden. Diese können über den Task-Manager überwacht und in der integrierten Konsole „Dienste“ in Windows angezeigt werden. 

Die beiden Dienstnamen lauten:
- `SqlIaaSExtensionQuery` (Anzeigename – `Microsoft SQL Server IaaS Query Service`)
- `SQLIaaSExtension` (Anzeigename – `Microsoft SQL Server IaaS Agent`)

**Wie entferne ich die Erweiterung?**

Sie entfernen die Erweiterung, indem Sie die [Registrierung der SQL Server-VM beim SQL-VM-Ressourcenanbieter aufheben](sql-vm-resource-provider-register.md#unregister-from-rp). 

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Installieren der SQL Server-IaaS-Erweiterung für SQL Server auf Azure-VMs finden Sie in den Artikeln zur [automatischen Installation](sql-vm-resource-provider-automatic-registration.md) und zur [Installation auf einzelnen VMs](sql-vm-resource-provider-register.md) bzw. zur [Masseninstallation auf VMs](sql-vm-resource-provider-bulk-register.md).

Ausführlichere Informationen zur Verwendung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Was ist SQL Server auf virtuellen Azure-Computern?](sql-server-on-azure-vm-iaas-what-is-overview.md).
