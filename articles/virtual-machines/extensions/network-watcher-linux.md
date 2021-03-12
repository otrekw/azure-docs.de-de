---
title: VM-Erweiterung für den Azure Network Watcher-Agent für Linux
description: Stellen Sie den Network Watcher-Agent mithilfe einer VM-Erweiterung auf einem virtuellen Linux-Computer bereit.
ms.topic: article
ms.service: virtual-machines
ms.subservice: extensions
author: amjads1
ms.author: amjads
ms.collection: linux
ms.date: 02/14/2017
ms.openlocfilehash: bc252e560df782625d795b30c6688a34f5c2bd79
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102563590"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>VM-Erweiterung für den Network Watcher-Agent für Linux

## <a name="overview"></a>Übersicht

[Azure Network Watcher](../../network-watcher/index.yml) ist ein Dienst zur Überwachung, Diagnose und Analyse der Netzwerkleistung, der die Überwachung von Azure-Netzwerken ermöglicht. Die VM-Erweiterung für den Network Watcher-Agent ist eine Voraussetzung für einige Network Watcher-Features auf Azure-VMs, wie etwa die bedarfsgesteuerte Erfassung von Netzwerkdatenverkehr und andere erweiterte Funktionalität.

Dieser Artikel enthält ausführliche Informationen zu den unterstützten Plattformen und Bereitstellungsoptionen für die VM-Erweiterung für den Network Watcher-Agent für Linux. Die Installation des Agents führt nicht zu einer Unterbrechung, und die VM muss nicht neu gestartet werden. Sie können die Erweiterung auf den von Ihnen bereitgestellten virtuellen Computern bereitstellen. Wenn der virtuelle Computer von einem Azure-Dienst bereitgestellt wird, lesen Sie die Dokumentation für den Dienst, um zu bestimmen, ob dieser das Installieren von Erweiterungen auf dem virtuellen Computer erlaubt.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="operating-system"></a>Betriebssystem

Die VM-Erweiterung für den Network Watcher-Agent kann für folgende Linux-Distributionen konfiguriert werden:

| Distribution | Version |
|---|---|
| Ubuntu | 12+ |
| Debian | 7 und 8 |
| Red Hat | 6 und 7 |
| Oracle Linux | 6.8+ und 7 |
| SUSE Linux Enterprise Server | 11 und 12 |
| OpenSUSE Leap | 42.3+ |
| CentOS | 6.5+ und 7 |
| CoreOS | 899.17.0+ |


### <a name="internet-connectivity"></a>Internetkonnektivität

Für einige Funktionen des Network Watcher-Agents muss eine VM mit dem Internet verbunden sein. Ohne die Möglichkeit, ausgehende Verbindungen herzustellen, treten bei einigen Features des Network Watcher-Agents möglicherweise Fehler auf, oder sie sind nicht verfügbar. Weitere Informationen zu Funktionen, die der Network Watcher-Agent benötigt, finden Sie in der [Dokumentation zu Network Watcher](../../network-watcher/index.yml).

## <a name="extension-schema"></a>Erweiterungsschema

Der folgende JSON-Code zeigt das Schema für die Network Watcher-Agent-Erweiterung. Für die Erweiterung müssen keine Einstellungen von Benutzern angegeben werden. Zudem wird keine Unterstützung für diese geboten. Die Erweiterung basiert auf seiner Standardkonfiguration.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentLinux",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Eigenschaftswerte

| Name | Wert/Beispiel |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| publisher | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Sie können Azure-VM-Erweiterungen mithilfe einer Azure Resource Manager-Vorlage bereitstellen. Verwenden Sie für die Bereitstellung der Erweiterung für den Network Watcher-Agent das vorangehende JSON-Schema in Ihrer Vorlage.

## <a name="azure-classic-cli-deployment"></a>Klassische Azure CLI-Bereitstellung

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Im folgenden Beispiel wird die VM-Erweiterung für den Network Watcher-Agent über das klassische Bereitstellungsmodell für eine vorhandene VM bereitgestellt:

```console
azure config mode asm
azure vm extension set myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="azure-cli-deployment"></a>Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle

Im folgenden Beispiel wird die VM-Erweiterung für den Network Watcher-Agent über den Resource Manager für eine vorhandene VM bereitgestellt:

```azurecli
az vm extension set --resource-group myResourceGroup1 --vm-name myVM1 --name NetworkWatcherAgentLinux --publisher Microsoft.Azure.NetworkWatcher --version 1.4
```

## <a name="troubleshooting-and-support"></a>Problembehandlung und Support

### <a name="troubleshooting"></a>Problembehandlung

Sie können Daten zum Status von Erweiterungsbereitstellungen entweder über das Azure-Portal oder die Azure CLI abrufen.

Das folgende Beispiel zeigt den Bereitstellungsstatus der NetworkWatcherAgentLinux-Erweiterung mit der Azure CLI für eine VM, die über den Resource Manager bereitgestellt wurde:

```azurecli
az vm extension show --name NetworkWatcherAgentLinux --resource-group myResourceGroup1 --vm-name myVM1
```

### <a name="support"></a>Support

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie die [Dokumentation zu Network Watcher](../../network-watcher/index.yml) lesen oder den Azure-Experten im [MSDN Azure- und Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) Fragen stellen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten** aus. Informationen zur Nutzung des Azure-Supports finden Sie unter [Häufig gestellte Fragen zum Azure-Support](https://azure.microsoft.com/support/faq/).
