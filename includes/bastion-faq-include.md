---
title: include file
description: include file
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 05/04/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: efb32631c5ee1eedece6d2a06b94702b602ed418
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86276112"
---
### <a name="which-regions-are-available"></a><a name="regions"></a>Welche Regionen sind verfügbar?

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="do-i-need-a-public-ip-on-my-virtual-machine"></a><a name="publicip"></a>Benötige ich auf meinem virtuellen Computer eine öffentliche IP-Adresse?

Wenn Sie mithilfe von Azure Bastion eine Verbindung mit einem virtuellen Computer herstellen, benötigen Sie auf diesem virtuellen Azure-Computer KEINE öffentliche IP-Adresse. Der Bastion-Dienst öffnet die RDP-/SSH-Sitzung oder -Verbindung mit Ihrem virtuellen Computer über die private IP des virtuellen Computers in Ihrem virtuellen Netzwerk.

### <a name="is-ipv6-supported"></a>Wird IPv6 unterstützt?

Derzeit wird IPv6 nicht unterstützt. Azure Bastion unterstützt nur IPv4.

### <a name="do-i-need-an-rdp-or-ssh-client"></a><a name="rdpssh"></a>Benötige ich einen RDP- oder SSH-Client?

Sie benötigen keinen RDP- oder SSH-Client, um auf das RDP/SSH Ihres virtuellen Computers über das Azure-Portal zuzugreifen. Verwenden Sie das [Azure-Portal](https://portal.azure.com), um direkt über den Browser RDP-/SSH-Zugriff auf Ihren virtuellen Computer zu erhalten.

### <a name="do-i-need-an-agent-running-in-the-azure-virtual-machine"></a><a name="agent"></a>Muss auf dem virtuellen Azure-Computer ein Agent ausgeführt werden?

Sie müssen keinen Agent oder eine andere Software in Ihrem Browser oder auf Ihrem virtuellen Azure-Computer installieren. Für den Bastion-Dienst ist kein Agent und keine zusätzliche Software für RDP/SSH erforderlich.

### <a name="how-many-concurrent-rdp-and-ssh-sessions-does-each-azure-bastion-support"></a><a name="limits"></a>Wie viele gleichzeitige RDP-und SSH-Sitzungen werden von den einzelnen Azure Bastion-Instanzen unterstütz?

RDP und SSH sind beides nutzungsbasierte Protokolle. Eine umfassende Nutzung von Sitzungen führt dazu, dass der Bastionhost eine niedrigere Gesamtzahl von Sitzungen unterstützt. Bei den folgenden Zahlen wird von normalen täglichen Workflows ausgegangen:

[!INCLUDE [limits](bastion-limits.md)]

### <a name="what-features-are-supported-in-an-rdp-session"></a><a name="rdpfeaturesupport"></a>Welche Funktionen werden in einer RDP-Sitzung unterstützt?

Derzeit wird nur das Kopieren/Einfügen von Text unterstützt. Funktionen wie das Kopieren von Dateien werden nicht unterstützt. Sie können uns Ihr Feedback zu neuen Features auf der [Azure Bastion-Feedbackseite](https://feedback.azure.com/forums/217313-networking?category_id=367303) mitteilen.

### <a name="which-browsers-are-supported"></a><a name="browsers"></a>Welche Browser werden unterstützt?

Verwenden Sie unter Windows den Microsoft Edge-Browser oder Google Chrome. Verwenden Sie unter macOS Google Chrome. Microsoft Edge Chromium wird auch für Windows und Mac unterstützt.

### <a name="where-does-azure-bastion-store-customer-data"></a><a name="data"></a>Wo speichert Azure Bastion Kundendaten?

Azure Bastion speichert Kundendaten in der Region, in der sie bereitgestellt werden, und verschiebt sie nicht aus dieser Region.

### <a name="are-any-roles-required-to-access-a-virtual-machine"></a><a name="roles"></a>Sind für den Zugriff auf einen virtuellen Computer Rollen erforderlich?

Zum Herstellen einer Verbindung sind die folgenden Rollen erforderlich:

* Rolle „Leser“ auf dem virtuellen Computer
* Rolle „Leser“ auf dem Netzwerkadapter mit privater IP-Adresse des virtuellen Computers
* Rolle „Leser“ für die Azure Bastion-Ressource

### <a name="what-is-the-pricing"></a><a name="pricingpage"></a>Wie sieht die Preisgestaltung aus?

Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://aka.ms/BastionHostPricing).

### <a name="does-azure-bastion-require-an-rds-cal-for-administrative-purposes-on-azure-hosted-vms"></a><a name="rdscal"></a>Erfordert Azure Bastion auf virtuellen Computern, die von Azure gehostet werden, eine RDS-CAL für administrative Aufgaben?
Nein. Für den Zugriff auf virtuelle Windows Server-Computer durch Azure Bastion ist keine [RDS-CAL](https://www.microsoft.com/en-us/p/windows-server-remote-desktop-services-cal/dg7gmgf0dvsv?activetab=pivot:overviewtab) erforderlich, wenn sich die Verwendung rein auf administrative Aufgaben beschränkt.

### <a name="what-keyboard-layouts-are-supported-during-the-bastion-remote-session"></a><a name="keyboard"></a>Welche Tastaturlayouts werden während der Bastion-Remotesitzung unterstützt?

Azure Bastion unterstützt derzeit das Tastaturlayout „en-us-qwerty“ innerhalb der VM.  Die Unterstützung anderer Gebietsschemas für das Tastaturlayout ist in Bearbeitung.

### <a name="is-user-defined-routing-udr-supported-on-an-azure-bastion-subnet"></a><a name="udr"></a>Wird benutzerdefiniertes Routing (User-Defined Routing, UDR) in einem Azure Bastion-Subnetz unterstützt?

Nein. UDR wird in einem Azure Bastion-Subnetz nicht unterstützt.
In Szenarien, bei denen Azure Bastion und Azure Firewall bzw. ein virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) im gleichen virtuellen Netzwerk vorhanden sind, müssen Sie den Datenverkehr zwischen einem Azure Bastion-Subnetz und Azure Firewall nicht erzwingen, da die Kommunikation zwischen Azure Bastion und Ihren VMs privat ist. Weitere Informationen finden Sie unter [Zugreifen auf virtuelle Computer hinter Azure Firewall mit Bastion](https://azure.microsoft.com/blog/accessing-virtual-machines-behind-azure-firewall-with-azure-bastion/).

### <a name="why-do-i-get-your-session-has-expired-error-message-before-the-bastion-session-starts"></a><a name="session"></a>Warum erhalte ich vor Beginn der Bastion-Sitzung die Fehlermeldung „Ihre Sitzung ist abgelaufen.“?

Eine Sitzung darf nur über das Azure-Portal initiiert werden. Melden Sie sich beim Azure-Portal an, und starten Sie Ihre Sitzung erneut. Dieser Fehler ist zu erwarten, wenn Sie direkt von einer anderen Browsersitzung oder -registerkarte aus zu der URL navigieren. Er trägt zum Schutz Ihrer Sitzung bei und sorgt dafür, dass nur über das Azure-Portal auf die Sitzung zugegriffen werden kann.

### <a name="how-do-i-handle-deployment-failures"></a><a name="udr"></a>Behandeln von Bereitstellungsfehlern

Überprüfen Sie die Fehlermeldungen, und stellen Sie bei Bedarf eine [Supportanfrage im Azure-Portal](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Bereitstellungsfehler können durch [Einschränkungen für Azure-Abonnements, Kontingente und sonstige Einschränkungen](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) verursacht werden. Insbesondere können Kunden eine Beschränkung der Anzahl von zulässigen öffentlichen IP-Adressen pro Abonnement feststellen, die zu einem Fehler bei der Azure Bastion-Bereitstellung führen.
