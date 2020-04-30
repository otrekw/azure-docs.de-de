---
title: Erstellen eines Benutzer-VM-Images für den Azure Marketplace
description: Listet die Schritte und Referenzen auf, die zum Erstellen eines Benutzer-VM-Images erforderlich sind.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 9d82d50769925480d461c122096c3919d7e8940d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146566"
---
# <a name="create-a-user-vm-image"></a>Erstellen eines Benutzer-VM-Images

> [!IMPORTANT]
> Ab dem 13. April 2020 beginnen wir mit der Umstellung der Verwaltung Ihrer Azure Virtual Machines-Angebote auf Partner Center. Nach der Migration erstellen und verwalten Sie Ihre Angebote in Partner Center. Befolgen Sie zum Verwalten Ihrer migrierten Angebote die Anweisungen unter [Erstellen Ihrer technischen Ressourcen für Azure Virtual Machines](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer).

In diesem Artikel sind die beiden allgemeinen Schritte beschrieben, die erforderlich sind, um ein nicht verwaltetes Image aus einer generalisierten VHD zu erstellen.  Über Referenzen werden Sie durch die einzelnen Schritte geführt: Erfassen des Images und Generalisieren des Images.


## <a name="capture-the-vm-image"></a>Erfassen des VM-Images

Befolgen Sie die Anweisungen im folgenden Artikel zum Erfassen der VM, die Ihrer Zugriffsweise entspricht:

-  Mit PowerShell: [Erstellen eines verwalteten Images eines generalisierten virtuellen Computers in Azure](../../../virtual-machines/windows/capture-image-resource.md)
-  Azure CLI: [Vorgehensweise zum Erstellen eines Image von einem virtuellen Computer oder einer VHD](../../../virtual-machines/linux/capture-image.md)
-  API: [VMs – Erfassen](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)


## <a name="generalize-the-vm-image"></a>Generalisieren des VM-Images

Da Sie das Benutzerimage aus einer zuvor generalisierten virtuellen Festplatte generiert haben, sollte das Image ebenfalls generalisiert werden.  Wählen Sie aus der folgenden Liste denjenigen Artikel aus, der Ihrer Zugriffsweise entspricht.  (Möglicherweise haben Sie die Festplatte bereits beim Erfassen generalisiert.)

-  Mit PowerShell: [Generalisieren des virtuellen Computers](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  Azure CLI: [Schritt 2: Erstellen des VM-Image](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [VMs – Generalisieren](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize)


## <a name="next-steps"></a>Nächste Schritte

Als Nächstes [erstellen Sie ein Zertifikat](cpp-create-key-vault-cert.md) und speichern dieses in einem neuen Azure Key Vault.  Dieses Zertifikat ist erforderlich, um eine sichere WinRM-Verbindung mit dem virtuellen Computer herzustellen.
