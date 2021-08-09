---
title: 'Remotearbeit mit Bastion: Azure Bastion'
description: Erfahren Sie, wie Sie Azure Bastion nutzen können, um Remotearbeit aufgrund der COVID-19-Pandemie zu ermöglichen.
services: bastion
author: mialdrid
ms.service: bastion
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: mialdrid
ms.openlocfilehash: 60709bf337c945d1ece7ecc5453350b0e126b046
ms.sourcegitcommit: 9ad20581c9fe2c35339acc34d74d0d9cb38eb9aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110540348"
---
# <a name="working-remotely-using-azure-bastion"></a>Remotearbeit mit Azure Bastion

Azure Bastion spielt eine zentrale Rolle bei der Unterstützung von Remotearbeitsszenarien, indem es Benutzern mit Internetkonnektivität den Zugriff auf die virtuellen Azure-Computer ermöglicht. Insbesondere ermöglicht es IT-Administratoren, ihre Anwendungen, die unter Azure ausgeführt werden, jederzeit und von überall auf der Welt zu verwalten.

>[!NOTE]
>In diesem Artikel wird beschrieben, wie Sie Azure Bastion, Azure, das Microsoft-Netzwerk und das Azure-Partnerökosystem nutzen können, um remote zu arbeiten und Netzwerkprobleme zu beheben, die Sie aufgrund der COVID-19-Krise bewältigen müssen.
>

## <a name="securely-access-virtual-machines"></a>Sicherer Zugriff auf virtuelle Computer

Insbesondere bietet Azure Bastion sichere und problemlose RDP/SSH-Konnektivität zu virtuellen Computern innerhalb des virtuellen Azure-Netzwerks, direkt im Azure-Portal, ohne die Verwendung einer öffentlichen IP-Adresse. Weitere Informationen über die Architektur und die wichtigsten Features von Azure Bastion finden Sie unter [Was ist Azure Bastion?](bastion-overview.md).

Azure Bastion wird pro virtuellem Netzwerk bereitgestellt, d. h. Unternehmen können eine Azure Bastion-Instanz konfigurieren und verwalten, um den Remotezugriff von Benutzern auf virtuelle Computer innerhalb eines virtuellen Azure-Netzwerks schnell zu unterstützen. Informationen zum Erstellen und Verwalten von Azure Bastion finden Sie unter [Erstellen eines Bastion-Hosts](./tutorial-create-host-portal.md).

## <a name="next-steps"></a>Nächste Schritte

* Konfigurieren Sie Azure Bastion über das [Azure-Portal](./tutorial-create-host-portal.md), mit [PowerShell](bastion-create-host-powershell.md) oder der Azure CLI.

* Weitere Informationen finden Sie unter [Häufig gestellte Fragen zu Azure Bastion](bastion-faq.md).