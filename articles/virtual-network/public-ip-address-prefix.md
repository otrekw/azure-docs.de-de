---
title: Präfix für öffentliche Azure-IP-Adressen
description: Erfahren Sie, was ein Präfix für öffentliche Azure-IP-Adressen ist und wie Sie mit einem solchen Präfix Ihren Ressourcen vorhersagbare öffentliche IP-Adressen zuweisen können.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.subservice: ip-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/29/2020
ms.author: allensu
ms.openlocfilehash: 8bfc76afc1ef799be0fb29654bd341f53d1b0a8c
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105048458"
---
# <a name="public-ip-address-prefix"></a>Präfix für öffentliche IP-Adressen

Ein Präfix für öffentliche IP-Adressen ist ein reservierter Bereich von IP-Adressen in Azure. Azure weist Ihrem Abonnement einen zusammenhängenden Adressbereich zu, der auf der Anzahl der von Ihnen angegebenen IP-Adressen basiert. 

Wenn Sie mit öffentlichen IP-Adressen nicht vertraut sind, finden Sie weitere Informationen unter [Öffentliche IP-Adressen](./public-ip-addresses.md#public-ip-addresses).

Öffentliche IP-Adressen werden aus einem Pool mit Adressen in jeder Azure-Region zugewiesen. Sie können die Liste der Bereiche [herunterladen](https://www.microsoft.com/download/details.aspx?id=56519), die Azure in jeder Region verwendet. 40.121.0.0/16 ist beispielsweise einer von mehr als 100 Bereichen, die Azure in der Region „USA, Osten“ verwendet. Der Bereich umfasst nutzbare Adressen von 40.121.0.1 bis 40.121.255.254.

Sie erstellen ein Präfix für öffentliche IP-Adressen in einer Azure-Region und einem Azure-Abonnement, indem Sie einen Namen angeben und festlegen, wie viele Adressen das Präfix umfassen soll. 

Öffentliche IP-Adressbereiche werden mit einem Präfix Ihrer Wahl zugewiesen. Wenn Sie ein /28-Präfix erstellen, weist Azure 16 IP-Adressen aus einem der Azure-Bereiche zu.

Sie wissen erst dann, welchen Bereich Azure zuweist, wenn Sie den Bereich erstellen, aber die Adressen sind zusammenhängend. 

Für Präfixe für öffentliche IP-Adressen wird eine Gebühr berechnet. Weitere Informationen dazu finden Sie unter [Preise für öffentliche IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="why-create-a-public-ip-address-prefix"></a>Warum sollte ich ein Präfix für öffentliche IP-Adressen erstellen?

Wenn Sie Ressourcen mit öffentlichen IP-Adressen erstellen, weist Azure eine verfügbare öffentliche IP-Adresse aus einem der in der entsprechenden Region verwendeten Bereiche zu. 

Sie kennen die exakte IP-Adresse erst dann, wenn Azure sie zuweist. Dieser Prozess kann problematisch sein, wenn Sie Firewallregeln erstellen, die nur bestimmte IP-Adressen zulassen. Für jede hinzugefügte IP-Adresse muss eine entsprechende Firewallregel erstellt werden.

Wenn Sie Ihren Ressourcen Adressen mit einem Präfix für öffentliche IP-Adressen zuweisen, sind keine Aktualisierungen von Firewallregeln mehr erforderlich. Sie fügen einfach den gesamten Bereich zur Regel hinzu.

## <a name="benefits"></a>Vorteile

- Öffentliche IP-Adressen für Ressourcen werden aus einem bekannten Bereich erstellt.
- Firewallregeln werden mit Bereichen konfiguriert, die sowohl die derzeit zugewiesenen öffentlichen IP-Adressen als auch noch nicht zugewiesene Adressen umfassen. Damit entfällt die Notwendigkeit, Firewallregeln zu ändern, sobald Sie neuen Ressourcen IP-Adressen zuweisen.
- Die Standardgrößen für erstellbare Bereichs sind /28 oder /16 IP-Adressen.
- Sie können beliebig viele Bereiche erstellen. Allerdings ist die Anzahl von statischen öffentlichen IP-Adressen begrenzt, die in einem Azure-Abonnement vorhanden sein können. Die von Ihnen erstellten Bereiche können nicht mehr statische öffentliche IP-Adressen umfassen, als in Ihrem Abonnement zulässig sind. Weitere Informationen finden Sie unter [Azure-Grenzwerte](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Die Adressen, die Sie mithilfe von Adressen aus dem Präfix erstellen, können jeder Azure-Ressource zugewiesen werden, die das Zuweisen einer öffentlichen IP-Adresse zulässt.
- Sie können ganz leicht erkennen, welche IP-Adressen in einem Bereich zugewiesen sind und welche nicht.

## <a name="scenarios"></a>Szenarien
Sie können die folgenden Ressourcen einer statischen öffentlichen IP-Adresse aus einem Präfix zuweisen:

|Resource|Szenario|Schritte|
|---|---|---|
|Virtuelle Computer| Indem Sie Ihren virtuellen Computern in Azure öffentliche IP-Adressen aus einem Präfix zuweisen, reduzieren Sie den Verwaltungsaufwand beim Hinzufügen der Adressen zu einer Zulassungsliste in der Firewall. Sie können ganz einfach ein ganzes Präfix mit einer einzigen Firewallregel aufnehmen. Wenn Sie Ihre virtuellen Computer in Azure skalieren, können Sie IP-Adressen aus dem gleichen Präfix zuweisen und so viel Zeit, Geld und Verwaltungsaufwand sparen.| So weisen Sie Ihrem virtuellen Computer IP-Adressen aus einem Präfix zu </br> 1. [Erstellen Sie ein Präfix](manage-public-ip-address-prefix.md). </br> 2. [Erstellen Sie eine IP-Adresse aus dem Präfix](manage-public-ip-address-prefix.md). </br> 3. [Weisen Sie die IP-Adresse der Netzwerkschnittstelle Ihres virtuellen Computers zu](virtual-network-network-interface-addresses.md#add-ip-addresses). </br> Sie können [die IPs auch einem Virtual Machine Scale Set zuordnen](https://azure.microsoft.com/resources/templates/101-vmms-with-public-ip-prefix/).
| Standardmäßige Lastenausgleichsmodule | Indem Sie Ihrer Front-End-IP-Konfiguration oder der Ausgangsregel eines Lastenausgleichsmoduls öffentliche IP-Adressen aus einem Präfix zuweisen, vereinfachen Sie die Verwaltung Ihres öffentlichen IP-Adressraums für Azure. Vereinfachen Sie das Szenario, indem Sie für ausgehende Verbindungen IP-Adressen aus einem Bereich zusammenhängender IP-Adressen verwenden. | So weisen Sie Ihrem Lastenausgleichsmodul IP-Adressen aus einem Präfix zu: </br> 1. [Erstellen Sie ein Präfix](manage-public-ip-address-prefix.md). </br> 2. [Erstellen Sie eine IP-Adresse aus dem Präfix](manage-public-ip-address-prefix.md). </br> 3. Wenn Sie das Lastenausgleichsmodul erstellen, wählen Sie die in Schritt 2 erstellte IP-Adresse als Front-End-IP-Adresse Ihres Lastenausgleichsmoduls aus, oder aktualisieren Sie die Adresse. |
| Azure Firewall | Sie können eine öffentliche IP-Adresse aus einem Präfix für ausgehende SNAT-Vorgänge verwenden. Der gesamte ausgehende VNET-Datenverkehr wird in die öffentliche IP-Adresse von [Azure Firewall](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) übersetzt. | So weisen Sie Ihrer Firewall eine IP-Adresse aus einem Präfix zu: </br> 1. [Erstellen Sie ein Präfix](manage-public-ip-address-prefix.md). </br> 2. [Erstellen Sie eine IP-Adresse aus dem Präfix](manage-public-ip-address-prefix.md). </br> 3. Wenn Sie [die Azure Firewall bereitstellen](../firewall/tutorial-firewall-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-the-firewall), stellen Sie sicher, dass Sie die IP-Adresse auswählen, die Sie zuvor aus dem Präfix zugeordnet haben.|
| VPN Gateway (Azure-SKU) oder Application Gateway v2 | Sie können eine öffentliche IP-Adresse aus einem Präfix für Ihr zonenredundantes VPN oder Ihre Application Gateway v2-Instanz verwenden. | So weisen Sie Ihrem Gateway eine IP-Adresse zu: </br> 1. [Erstellen Sie ein Präfix](manage-public-ip-address-prefix.md). </br> 2. [Erstellen Sie eine IP-Adresse aus dem Präfix](manage-public-ip-address-prefix.md). </br> 3. Wenn Sie das [VPN Gateway](../vpn-gateway/tutorial-create-gateway-portal.md) oder [Application Gateway](../application-gateway/quick-create-portal.md#create-an-application-gateway) einsetzen, stellen Sie sicher, dass Sie die IP, die Sie zuvor angegeben haben, aus dem Präfix auswählen.|

## <a name="constraints"></a>Einschränkungen

- Sie können die IP-Adressen für das Präfix nicht angeben. Azure ordnet die IP-Adressen für das Präfix basierend auf der von Ihnen angegebenen Größe zu.
- Sie können standardmäßig ein Präfix von bis zu 16 IP-Adressen oder ein /28 erstellen. Weitere Informationen finden Sie unter den [Anforderungen zum Erhöhen der Netzwerkgrenzwerte](../azure-portal/supportability/networking-quota-requests.md) und den [Einschränkungen bei Azure](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Nachdem Sie das Präfix erstellt haben, können Sie den Bereich nicht mehr ändern.
- Nur statische öffentliche IP-Adressen, die mit der Standard-SKU erstellt wurden, können aus dem Bereich des Präfix zugewiesen werden. Weitere Informationen zu SKUs für öffentliche IP-Adressen finden Sie unter [Öffentliche IP-Adressen](./public-ip-addresses.md#public-ip-addresses).
- Adressen aus dem Bereich können nur Azure Resource Manager-Ressourcen zugewiesen werden. Adressen können keinen Ressourcen im klassischen Bereitstellungsmodell zugewiesen werden.
- Alle aus dem Präfix erstellten öffentlichen IP-Adressen müssen sich in der gleichen Azure-Region und im gleichen Azure-Abonnement wie das Präfix befinden. Die Adressen müssen Ressourcen in der gleichen Region und im gleichen Abonnement zugewiesen werden.
- Sie können ein Präfix nicht löschen, wenn darin befindliche IP-Adressen öffentlichen IP-Adressressourcen zugewiesen sind, die einer Ressource zugeordnet sind. Heben Sie zuerst die Zuordnung aller öffentlichen IP-Adressressourcen auf, denen IP-Adressen aus dem Präfix zugewiesen sind.


## <a name="next-steps"></a>Nächste Schritte

- [Erstellen](manage-public-ip-address-prefix.md) des Präfix für öffentliche IP-Adressen