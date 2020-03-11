---
title: include file
description: include file
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 03/02/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 17d391a7e6b8ef0558fb73afe363cd96deb60a7d
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78262335"
---
### <a name="regions"></a>Welche Regionen sind verfügbar?

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="publicip"></a>Benötige ich auf meinem virtuellen Computer eine öffentliche IP-Adresse?

Wenn Sie mithilfe von Azure Bastion eine Verbindung mit einem virtuellen Computer herstellen, benötigen Sie auf diesem virtuellen Azure-Computer KEINE öffentliche IP-Adresse. Der Bastion-Dienst öffnet die RDP-/SSH-Sitzung oder -Verbindung mit Ihrem virtuellen Computer über die private IP des virtuellen Computers in Ihrem virtuellen Netzwerk.

### <a name="is-ipv6-supported"></a>Wird IPv6 unterstützt?

Derzeit wird IPv6 nicht unterstützt. Azure Bastion unterstützt nur IPv4.

### <a name="rdpssh"></a>Benötige ich einen RDP- oder SSH-Client?

Sie benötigen keinen RDP- oder SSH-Client, um auf das RDP/SSH Ihres virtuellen Computers über das Azure-Portal zuzugreifen. Verwenden Sie das [Azure-Portal](https://portal.azure.com), um direkt über den Browser RDP-/SSH-Zugriff auf Ihren virtuellen Computer zu erhalten.

### <a name="rdscal"></a>Erfordert Azure Bastion auf virtuellen Computern, die von Azure gehostet werden, eine RDS-CAL für administrative Aufgaben?
Nein. Für den Zugriff auf virtuelle Windows Server-Computer durch Azure Bastion ist keine [RDS-CAL](https://www.microsoft.com/en-us/p/windows-server-remote-desktop-services-cal/dg7gmgf0dvsv?activetab=pivot:overviewtab) erforderlich, wenn sich die Verwendung rein auf administrative Aufgaben beschränkt.

### <a name="agent"></a>Muss auf dem virtuellen Azure-Computer ein Agent ausgeführt werden?

Sie müssen keinen Agent oder eine andere Software in Ihrem Browser oder auf Ihrem virtuellen Azure-Computer installieren. Für den Bastion-Dienst ist kein Agent und keine zusätzliche Software für RDP/SSH erforderlich.

### <a name="browsers"></a>Welche Browser werden unterstützt?

Verwenden Sie unter Windows den Microsoft Edge-Browser oder Google Chrome. Verwenden Sie unter macOS Google Chrome. Microsoft Edge Chromium wird auch für Windows und Mac unterstützt.

### <a name="roles"></a>Sind für den Zugriff auf einen virtuellen Computer Rollen erforderlich?

Zum Herstellen einer Verbindung sind die folgenden Rollen erforderlich:

* Rolle „Leser“ auf dem virtuellen Computer
* Rolle „Leser“ auf dem Netzwerkadapter mit privater IP-Adresse des virtuellen Computers
* Rolle „Leser“ für die Azure Bastion-Ressource

### <a name="pricingpage"></a>Wie sieht die Preisgestaltung aus?

Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://aka.ms/BastionHostPricing).

### <a name="session"></a>Warum erhalte ich vor Beginn der Bastion-Sitzung die Fehlermeldung „Ihre Sitzung ist abgelaufen.“?

Eine Sitzung darf nur über das Azure-Portal initiiert werden. Melden Sie sich beim Azure-Portal an, und starten Sie Ihre Sitzung erneut. Dieser Fehler ist zu erwarten, wenn Sie direkt von einer anderen Browsersitzung oder -registerkarte aus zu der URL navigieren. Er trägt zum Schutz Ihrer Sitzung bei und sorgt dafür, dass nur über das Azure-Portal auf die Sitzung zugegriffen werden kann.

### <a name="keyboard"></a>Welche Tastaturlayouts werden während der Bastion-Remotesitzung unterstützt?

Azure Bastion unterstützt derzeit das Tastaturlayout „en-us-qwerty“ innerhalb der VM.  Die Unterstützung anderer Gebietsschemas für das Tastaturlayout ist in Bearbeitung.

### <a name="udr"></a>Wird benutzerdefiniertes Routing (User-Defined Routing, UDR) in einem Azure Bastion-Subnetz unterstützt?

Nein. UDR wird in einem Azure Bastion-Subnetz nicht unterstützt.
In Szenarien, bei denen Azure Bastion und Azure Firewall bzw. ein virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) im gleichen virtuellen Netzwerk vorhanden sind, müssen Sie den Datenverkehr zwischen einem Azure Bastion-Subnetz und Azure Firewall nicht erzwingen, da die Kommunikation zwischen Azure Bastion und Ihren VMs privat ist. Weitere Informationen finden Sie unter [Zugreifen auf virtuelle Computer hinter Azure Firewall mit Bastion](https://azure.microsoft.com/blog/accessing-virtual-machines-behind-azure-firewall-with-azure-bastion/).

### <a name="filetransfer"></a>Wird die Dateiübertragung mit einer Azure Bastion-RDP-Sitzung unterstützt?

Wir arbeiten daran, neue Funktionen hinzuzufügen. Derzeit wird die Dateiübertragung nicht unterstützt, sie ist jedoch Teil unserer Roadmap. Sie können uns Ihr Feedback zu neuen Features auf der [Azure Bastion-Feedbackseite](https://feedback.azure.com/forums/217313-networking?category_id=367303) mitteilen.
