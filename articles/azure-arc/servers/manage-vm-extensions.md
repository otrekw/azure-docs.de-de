---
title: Verwaltung von VM-Erweiterungen mit Azure Arc-fähigen Servern
description: Mit Azure Arc-fähigen Servern kann die Bereitstellung von Erweiterungen für virtuelle Computer verwaltet werden, die Konfigurations- und Automatisierungsaufgaben nach der Bereitstellung für nicht in Azure gehostete VMs bereitstellen.
ms.date: 12/14/2020
ms.topic: conceptual
ms.openlocfilehash: 55e21f9c6bcd2dfe5f995093034773f2a87d9b03
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2020
ms.locfileid: "97504507"
---
# <a name="virtual-machine-extension-management-with-azure-arc-enabled-servers"></a>Verwaltung von Erweiterungen für virtuelle Computer mit Azure Arc-fähigen Servern

Erweiterungen für virtuelle Computer sind kleine Anwendungen, die Konfigurations- und Automatisierungsaufgaben auf virtuellen Azure-Computern nach der Bereitstellung ermöglichen. Wenn z. B. Software auf einer VM installiert werden muss, Virenschutz oder die Ausführung eines Skripts erforderlich ist, kann eine VM-Erweiterung verwendet werden.

Mit Azure Arc-fähigen Servern können Sie Azure-VM-Erweiterungen für nicht in Azure gehostete Windows- und Linux-VMs bereitstellen, um die Verwaltung Ihrer Hybridcomputer über ihren gesamten Lebenszyklus zu vereinfachen. VM-Erweiterungen können mit den folgenden Methoden auf Ihren hybriden Computern oder Servern verwaltet werden, die von Azure Arc-fähigen Servern verwaltet werden:

- Das [Azure-Portal](manage-vm-extensions-portal.md)
- Die [Azure CLI](manage-vm-extensions-cli.md)
- [Azure PowerShell](manage-vm-extensions-powershell.md)
- [Azure Resource Manager-Vorlagen](manage-vm-extensions-template.md)

## <a name="key-benefits"></a>Hauptvorteile

Die Unterstützung von VM-Erweiterungen durch Azure Arc-fähige Server bietet die folgenden Hauptvorteile:

- Verwendung von [Azure Automation State Configuration](../../automation/automation-dsc-overview.md), um Konfigurationen zentral zu speichern und den gewünschten Zustand hybrid verbundener Computer mithilfe der DSC-VM-Erweiterung zu verwalten.

- Sammeln von Protokolldaten für die Analyse mit [Protokollen in Azure Monitor](../../azure-monitor/platform/data-platform-logs.md), die durch die Log Analytics-Agent-VM-Erweiterung unterstützt werden. Dies ist nützlich, um komplexe Analysen über Daten aus unterschiedlichen Typen von Quellen hinweg auszuführen.

- Mit [Azure Monitor für VMs](../../azure-monitor/insights/vminsights-overview.md) werden die Leistung Ihrer Windows- und Linux-VMs analysiert sowie ihre Prozesse und Abhängigkeiten von anderen Ressourcen und externen Prozessen überwacht. Dies wird durch Aktivieren sowohl der Log Analytics-Agent- als auch der Dependency-Agent-VM-Erweiterung erreicht.

- Herunterladen und Ausführen von Skripts auf hybrid verbundenen Computern mithilfe der Erweiterung für benutzerdefinierte Skripts. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations- oder Verwaltungsaufgaben.

- Automatisches Aktualisieren von Zertifikaten, die in einer [Azure Key Vault](../../key-vault/general/overview.md)-Instanz gespeichert sind.

## <a name="availability"></a>Verfügbarkeit

Die Funktionalität der VM-Erweiterungen ist nur in den [unterstützten Regionen](overview.md#supported-regions) aus der Liste verfügbar. Achten Sie darauf, das Onboarding Ihres Computers in einer dieser Regionen durchzuführen.

## <a name="extensions"></a>Erweiterungen

In dieser Version unterstützen wir die folgenden VM-Erweiterungen auf Windows- und Linux-Computern.

|Durchwahl |OS |Herausgeber |Zusätzliche Informationen |
|----------|---|----------|-----------------------|
|CustomScriptExtension |Windows |Microsoft.Compute |[Erweiterung für benutzerdefinierte Windows-Skripts](../../virtual-machines/extensions/custom-script-windows.md)|
|DSC |Windows |Microsoft.PowerShell|[Windows PowerShell DSC-Erweiterung](../../virtual-machines/extensions/dsc-windows.md)|
|Log Analytics-Agent |Windows |Microsoft.EnterpriseCloud.Monitoring |[Log Analytics-Erweiterung für virtuelle Computer für Windows](../../virtual-machines/extensions/oms-windows.md)|
|Microsoft Dependency-Agent | Windows |Microsoft.Compute | [VM-Erweiterung für den Dependency-Agent für Windows](../../virtual-machines/extensions/agent-dependency-windows.md)|
|Key Vault | Windows | Microsoft.Compute | [Key Vault-VM-Erweiterung für Windows](../../virtual-machines/extensions/key-vault-windows.md) |
|CustomScript|Linux |Microsoft.Azure.Extension |[Erweiterung für benutzerdefinierte Linux-Skripts, Version 2](../../virtual-machines/extensions/custom-script-linux.md) |
|DSC |Linux |Microsoft.OSTCExtensions |[PowerShell DSC.-Erweiterung für Linux](../../virtual-machines/extensions/dsc-linux.md) |
|Log Analytics-Agent |Linux |Microsoft.EnterpriseCloud.Monitoring |[Log Analytics VM-Erweiterung für Linux](../../virtual-machines/extensions/oms-linux.md) |
|Microsoft Dependency-Agent | Linux |Microsoft.Compute | [VM-Erweiterung für den Dependency-Agent für Linux](../../virtual-machines/extensions/agent-dependency-linux.md) |
|Key Vault | Linux | Microsoft.Compute | [Key Vault-VM-Erweiterung für Linux](../../virtual-machines/extensions/key-vault-linux.md) |

Weitere Informationen zum Azure Connected Machine-Agent-Paket und Details zur Erweiterungs-Agent-Komponente finden Sie in der [Agent-Übersicht](agent-overview.md#agent-component-details).

## <a name="prerequisites"></a>Voraussetzungen

Diese Funktion hängt von den folgenden Azure-Ressourcenanbietern in Ihrem Abonnement ab:

- **Microsoft.HybridCompute**
- **Microsoft.GuestConfiguration**

Wenn diese noch nicht registriert sind, führen Sie die Schritte unter [Registrieren von Azure-Ressourcenanbietern](agent-overview.md#register-azure-resource-providers) aus.

Lesen Sie unbedingt die Dokumentation zu jeder VM-Erweiterung, auf die in der vorherigen Tabelle verwiesen wird, um mehr über potenzielle Netzwerk- oder Systemanforderungen zu erfahren. Dies kann dabei helfen, Konnektivitätsprobleme mit einem Azure-Dienst oder -Feature zu vermeiden, der oder das von dieser VM-Erweiterung abhängt.

### <a name="log-analytics-vm-extension"></a>Log Analytics-Erweiterung für virtuelle Computer

Für die Log Analytics-Agent-VM-Erweiterung für Linux muss Python 2.x auf dem Zielcomputer installiert sein. 

### <a name="azure-key-vault-vm-extension-preview"></a>Azure Key Vault-VM-Erweiterung (Vorschau)

Die folgenden Linux-Betriebssysteme werden von der Key Vault-VM-Erweiterung (Vorschau) nicht unterstützt:

- CentOS Linux 7 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64)
- Amazon Linux 2 (x64)

Die Bereitstellung der Key Vault-VM-Erweiterung (Vorschau) wird nur mit Folgendem unterstützt:

- Die Azure-CLI
- Azure PowerShell
- Azure Resource Manager-Vorlage

Vor der Bereitstellung der Erweiterung müssen Sie die folgenden Schritte ausführen:

1. [Erstellen Sie einen Tresor und ein Zertifikat](../../key-vault/certificates/quick-create-portal.md) (selbstsigniert oder importiert).

2. Gewähren Sie dem Azure Arc-fähigen Server Zugriff auf das Zertifikatgeheimnis. Wenn Sie die [rollenbasierte Zugriffssteuerung (Vorschau)](../../key-vault/general/rbac-guide.md) verwenden, suchen Sie den Namen der Azure Arc-Ressource, und weisen Sie sie der Rolle **Key Vault-Geheimnisbenutzer (Vorschau)** zu. Wenn Sie die [Key Vault-Zugriffsrichtlinie](../../key-vault/general/assign-access-policy-portal.md) verwenden, weisen Sie der vom System zugewiesenen Identität der Azure Arc-Ressource die **Get**-Berechtigungen des Geheimnisses zu.

### <a name="connected-machine-agent"></a>Connected Machine-Agent

Überprüfen Sie, ob Ihr Computer mit den [unterstützten Versionen](agent-overview.md#supported-operating-systems) des Windows- und Linux-Betriebssystems für den Azure Connected Machine-Agent übereinstimmt.

Die Mindestversion des Connected Machine-Agents, die mit dieser Funktion unter Windows und Linux unterstütz wird, ist Version 1.0.

Informationen für das Upgrade Ihres Computers auf die erforderliche Agent-Version finden Sie unter [Agent aktualisieren](manage-agent.md#upgrading-agent).

## <a name="next-steps"></a>Nächste Schritte

Sie können VM-Erweiterungen über die [Azure-Befehlszeilenschnittstelle](manage-vm-extensions-cli.md), [Azure PowerShell](manage-vm-extensions-powershell.md), das [Azure-Portal](manage-vm-extensions-portal.md) oder [Azure Resource Manager-Vorlagen](manage-vm-extensions-template.md) bereitstellen, verwalten und entfernen.
