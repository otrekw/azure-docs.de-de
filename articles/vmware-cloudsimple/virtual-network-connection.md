---
title: Verbinden des virtuellen Azure-Netzwerks mit AVS mithilfe von ExpressRoute
description: Beschreibt, wie Peeringinformationen für eine Verbindung zwischen dem virtuellen Azure-Netzwerk und Ihrer AVS-Umgebung abgerufen werden
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1ff11bbafe6f9057ce70c799e0437691d89ccb40
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014401"
---
# <a name="connect-azure-virtual-network-to-avs-using-expressroute"></a>Verbinden des virtuellen Azure-Netzwerks mit AVS mithilfe von ExpressRoute

Sie können Ihr privates AVS-Cloudnetzwerk auf Ihr virtuelles Azure-Netzwerk und Ihre Azure-Ressourcen erweitern. Eine ExpressRoute-Verbindung ermöglicht Ihnen den Zugriff auf verschiedene Ressourcen, die in Ihrem Azure-Abonnement über Ihre private AVS-Cloud ausgeführt werden.

## <a name="request-authorization-key"></a>Anfordern eines Autorisierungsschlüssels

Für die ExpressRoute-Verbindung zwischen Ihrer privaten AVS-Cloud und dem virtuellen Azure-Netzwerk ist ein Autorisierungsschlüssel erforderlich. Um einen Schlüssel zu erhalten, müssen Sie ein <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">Supportticket</a> einreichen. Verwenden Sie die folgenden Informationen für die Anforderung:

* Problemtyp: **Technisch**
* Abonnement: **Wählen Sie das Abonnement aus, in dem der AVS-Dienst bereitgestellt wird.**
* Dienst: **VMware Solutions (AVS)**
* Problemtyp: **Dienstanforderung**
* Problemuntertyp: **Autorisierungsschlüssel für Azure VNET-Verbindung**
* Antragsteller: **Anforderung eines Autorisierungsschlüssels für Azure VNET-Verbindung**

## <a name="get-peering-information-from-avs-portal"></a>Abrufen von Peeringinformationen im AVS-Portal

Zum Einrichten der Verbindung müssen Sie eine Verbindung zwischen dem virtuellen Azure-Netzwerk und Ihrer AVS-Umgebung herstellen. Als Teil dieses Verfahrens müssen Sie den Peerleitungs-URI und den Autorisierungsschlüssel angeben. Rufen Sie den URI und den Autorisierungsschlüssel aus dem [AVS-Portal](access-cloudsimple-portal.md) ab. Wählen Sie im seitlichen Menü **Network** (Netzwerk) aus, und wählen Sie dann **Azure Network Connection** (Azure-Netzwerkverbindung) aus. Oder wählen Sie im seitlichen Menü **Account** (Konto) aus, und wählen Sie dann **Azure Network Connection** (Azure-Netzwerkverbindung) aus.

Kopieren Sie den Peerleitungs-URI und für den Autorisierungsschlüssel für jede der Regionen mithilfe des *Kopieren*-Symbols. Gehen Sie für jede AVS-Region, die Sie verbinden möchten, folgendermaßen vor:

1. Klicken Sie auf **Copy** (Kopieren), um den URI zu kopieren. Fügen Sie ihn in eine Datei ein, aus der er dem Azure-Portal hinzugefügt werden kann. 
2. Klicken Sie auf **Copy** (Kopieren), um den Autorisierungsschlüssel zu kopieren und ebenfalls in die Datei einzufügen.

Kopieren Sie den Autorisierungsschlüssel und den Peerleitungs-URI, der sich im Zustand **Verfügbar** befindet. Der Status **Verwendet** zeigt an, dass der Schlüssel bereits zum Erstellen einer virtuellen Netzwerkverbindung verwendet wurde.

![Seite „Verbindung mit virtuellem Netzwerk“](media/virtual-network-connection.png)

Weitere Informationen zum Einrichten der Verbindung zwischen dem virtuellen Azure-Netzwerk und AVS finden Sie unter [Verbinden Ihrer privaten AVS-Cloudumgebung mit dem virtuellen Azure-Netzwerk über ExpressRoute](azure-expressroute-connection.md).

## <a name="next-steps"></a>Nächste Schritte

* [Virtuelle Azure-Netzwerkverbindung mit der privaten AVS-Cloud](azure-expressroute-connection.md)
* [Connect from on-premises to CloudSimple using ExpressRoute](on-premises-connection.md)
