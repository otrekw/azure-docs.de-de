---
title: Unterstützung für 32-Bit-Betriebssysteme auf virtuellen Azure-Computern | Microsoft-Dokumentation
description: Informationen zu Betriebssystemen, die auf virtuellen Azure-Computern unterstützt werden
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/18/2019
ms.author: v-miegge
ms.openlocfilehash: 81b7efdd6bca0471719c11d130be95405f4d54e1
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98210187"
---
# <a name="support-for-32-bit-operating-systems-in-azure-virtual-machines"></a>Unterstützung für 32-Bit-Betriebssysteme auf virtuellen Azure-Computern

Microsoft Azure ermöglicht es Benutzern jetzt, ihre 32-Bit-Windows-Betriebssysteme in Azure zu übernehmen. Es werden ausschließlich spezialisierte VHDs unterstützt, und generalisierte Images funktionieren in Azure nicht. Da einige dieser Betriebssysteme das Ende des Supportlebensdauer-Vertrags erreicht haben, bietet Microsoft möglicherweise keinen weiteren Support für diese an. Ebenfalls kein Support wird für auf Linux oder BSD (Berkeley Software Distribution) basierende Betriebssysteme geboten, die auf einem virtuellen Microsoft Azure-Computer (VM) ausgeführt werden.

> [!NOTE]
> Die Azure-Plattform weist eine Beschränkung des Arbeitsspeicheradressraums für VMs mit 32-Bit-Betriebssystemen auf, bei der der VM möglicherweise nur 1 GB Arbeitsspeicher zur Verfügung gestellt wird (*insbesondere bei Client-SKUs wie Win7 oder Win10*), und der Rest des Arbeitsspeichers für die VM wird innerhalb der Gast-VM als reserviert angezeigt. Es handelt sich um ein bekanntes Problem, für das es derzeit keine ETA für eine voraussichtliche Lösung gibt. Wir empfehlen den Umstieg auf 64-Bit-Betriebssystemversionen.
> 

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu den auf virtuellen Azure-Computern unterstützten Betriebssystemen finden Sie in den folgenden Microsoft Knowledge Base-Artikeln:

* [Microsoft-Server-Software-Support für virtuelle Maschinen von Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)
* [Support für Linux und Open-Source-Technologie in Azure](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure)

## <a name="references"></a>References

* [Weitere Informationen zu kostenlosen verlängerten Sicherheitsupdates für Windows Server 2008/R2 in Azure](https://www.microsoft.com/cloud-platform/windows-server-2008)
* [Weitere Informationen zur Unterstützung für spezialisierte Windows Server 2008 SP2 32-Bit-Images in Azure](/windows-server/get-started/uploading-specialized-ws08-image-to-azure)
* [Weitere Informationen zur Unterstützung der Migration von Windows Server 2008-Images in Azure mithilfe von Azure Site Recovery](../../site-recovery/migrate-tutorial-windows-server-2008.md)
* [Weitere Informationen zu unterstützten Betriebssystemen für die Azure-Erweiterung](https://support.microsoft.com/help/4078134/azure-extension-supported-operating-systems)
* [Weitere Informationen zur Ausführung von Windows Server 2003 in Microsoft Azure](https://support.microsoft.com/help/3206074/running-windows-server-2003-on-microsoft-azure)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, wenden Sie sich an die Azure-Experten im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/).

Alternativ dazu können Sie eine Azure-Supportanfrage erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten** aus.
