---
title: Azure DDoS Protection Standard Geschäftskontinuität | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zur Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azur DDoS Protection Standard.
services: ddos-protection
documentationcenter: na
author: aletheatoh
ms.service: ddos-protection
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/16/2021
ms.author: yitoh
ms.topic: article
ms.openlocfilehash: 5085f1584a737e75adccf4ec23bf709bede28a4b
ms.sourcegitcommit: 6f1aa680588f5db41ed7fc78c934452d468ddb84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107730334"
---
# <a name="azure-ddos-protection-standard--business-continuity"></a>Azure DDoS Protection Standard – Geschäftskontinuität

Geschäftskontinuität und Notfallwiederherstellung in Azure DDoS Protection Standard ermöglicht es Ihrem Unternehmen, den Geschäftsbetrieb im Falle einer Störung aufrechtzuerhalten. Dieser Artikel beschäftigt sich mit der Verfügbarkeit (innerhalb der Region) und der Notfallwiederherstellung.

## <a name="overview"></a>Übersicht
Von Azure DDoS Protection Standard werden öffentliche IP-Adressen in virtuellen Netzwerken geschützt. Der Schutz kann einfach in jedem neuen oder vorhandenen virtuellen Netzwerk aktiviert werden und erfordert keine Änderung von Anwendung oder Ressource.

Ein virtuelles Netzwerk (VNet) ist eine logische Darstellung Ihres Netzwerks in der Cloud. VNETs dienen als Vertrauensgrenze zum Hosten Ihrer Ressourcen wie Azure Application Gateway, Azure Firewall und Azure Virtual Machines. Es wird innerhalb des Geltungsbereichs einer Region erstellt. Sie können VNETs mit demselben Adressraum in zwei verschiedenen Regionen *erstellen* (z. B. USA, Osten und USA, Westen), weil sie den gleichen Adressraum verwenden, sie aber nicht miteinander verbinden. 

## <a name="business-continuity"></a>Geschäftskontinuität

Es gibt verschiedene Möglichkeiten, warum Ihre Anwendung unterbrochen werden kann. Eine Region kann aufgrund einer Naturkatastrophe oder eines Teilausfalls mehrerer Geräte oder Dienste vollständig abgeschnitten sein. Die Auswirkungen auf Ihre geschützten VNETs unterscheiden sich in jeder dieser Situationen.

**F: Wie gehe ich bei einem Ausfall für eine gesamte Region vor? Wenn beispielsweise eine Region aufgrund einer Naturkatastrophe vollständig abgeschnitten ist? Was geschieht mit den virtuellen Netzwerken, die in der Region gehostet werden?**

A: Auf das virtuelle Netzwerk und die Ressourcen in der betroffenen Region kann während der Dienstunterbrechung nicht zugegriffen werden.

![Diagramm eines einfachen virtuellen Netzwerks.](../virtual-network/media/virtual-network-disaster-recovery-guidance/vnet.png)

**F: Was kann ich tun, um dasselbe virtuelle Netzwerk in einer anderen Region neu zu erstellen?**

A: Virtuelle Netzwerke sind relativ schlanke Ressourcen. Sie können Azure-APIs zum Erstellen eines VNet mit dem gleichen Adressraum in einer anderen Region aufrufen. Um die gleiche Umgebung neu zu erstellen, die in der betroffenen Region vorhanden war, müssen Sie APIs aufrufen, um die Ressourcen in den VNets erneut bereitzustellen. Wenn Sie über lokale Konnektivität wie beispielsweise bei einer Hybridbereitstellung verfügen, müssen Sie ein neues VPN Gateway bereitstellen und eine Verbindung mit Ihrem lokalen Netzwerk herstellen.

Informationen zum Erstellen eines virtuellen Netzwerks finden Sie unter [Erstellen eines virtuellen Netzwerks](../virtual-network/manage-virtual-network.md#create-a-virtual-network).

**F: Kann ein Replikat eines VNet in einer bestimmten Region in einer anderen Region im Voraus neu erstellt werden?**

A: Ja, Sie können im Voraus zwei VNets erstellen, die denselben privaten IP-Adressraum und dieselben Ressourcen in zwei verschiedenen Regionen nutzen. Wenn Sie im VNET Dienste mit Internetzugriff hostet, können Sie Traffic Manager einrichten, um den Datenverkehr geografisch an die aktive Region zu leiten. Sie können jedoch nicht zwei VNETs mit demselben Adressraum mit dem lokalen Netzwerk verbinden, da dies Routingprobleme verursachen würde. Im Falle eines Notfalls und Verlusts eines VNETs in einer Region können Sie das andere VNET in der verfügbaren Region mit dem entsprechenden Adressraum mit Ihrem lokalen Netzwerk verbinden.

Informationen zum Erstellen eines virtuellen Netzwerks finden Sie unter [Erstellen eines virtuellen Netzwerks](../virtual-network/manage-virtual-network.md#create-a-virtual-network).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [einen DDoS-Schutzplan erstellen](manage-ddos-protection.md).