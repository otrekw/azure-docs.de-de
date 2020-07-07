---
title: Erstellen und Verschlüsseln einer VM-Skalierungsgruppe mit Azure Resource Manager-Vorlagen
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von Azure Resource Manager-Vorlagen eine VM-Skalierungsgruppe erstellen und verschlüsseln.
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: c2b49a7e7e14bfbefcca64133ff23fdfabe53e7b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83198424"
---
# <a name="encrypt-virtual-machine-scale-sets-with-azure-resource-manager"></a>Verschlüsseln von VM-Skalierungsgruppen mit Azure Resource Manager

Linux-basierte VM-Skalierungsgruppen können unter Verwendung von Azure Resource Manager-Vorlagen verschlüsselt und entschlüsselt werden.

## <a name="deploying-templates"></a>Bereitstellen von Vorlagen

Wählen Sie zunächst eine geeignete Vorlage für Ihr Szenario aus.

- [Enable disk encryption on a running Linux virtual machine scale set](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux) (Aktivieren der Datenträgerverschlüsselung in einer ausgeführten VM-Skalierungsgruppe unter Linux)

- [Enable disk encryption on a running Windows virtual machine scale set](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows) (Aktivieren der Datenträgerverschlüsselung in einer ausgeführten VM-Skalierungsgruppe unter Windows)

  - [Deploy a virtual machine scale set of Linux VMs with a jumpbox and enables encryption on Linux virtual machine scale sets](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox) (Bereitstellen einer VM-Skalierungsgruppe mit virtuellen Linux-Computern und einer Jumpbox sowie Aktivieren der Verschlüsselung in Linux-basierten VM-Skalierungsgruppen)

  - [Deploy a virtual machine scale set of Windows VMs with a jumpbox and enables encryption on Windows virtual machine scale sets](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) (Bereitstellen einer VM-Skalierungsgruppe mit virtuellen Windows-Computern und einer Jumpbox sowie Aktivieren der Verschlüsselung in Windows-basierten VM-Skalierungsgruppen)

- [Disable disk encryption on a running Linux virtual machine scale set](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux) (Deaktivieren der Datenträgerverschlüsselung in einer ausgeführten VM-Skalierungsgruppe unter Linux)

- [Disable disk encryption on a running Windows virtual machine scale set](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows) (Deaktivieren der Datenträgerverschlüsselung in einer ausgeführten VM-Skalierungsgruppe unter Windows)

Führen Sie dann die folgenden Schritte durch:

     1. Click **Deploy to Azure**.
     2. Fill in the required fields then agree to the terms and conditions.
     3. Click **Purchase** to deploy the template.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Disk Encryption für VM-Skalierungsgruppen](disk-encryption-overview.md)
- [Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure CLI](disk-encryption-cli.md)
- [Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure PowerShell](disk-encryption-powershell.md)
- [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption](disk-encryption-key-vault.md)
- [Verwenden von Azure Disk Encryption mit Erweiterungssequenzierung der VM-Skalierungsgruppe](disk-encryption-extension-sequencing.md)
