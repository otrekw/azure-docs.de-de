---
title: Häufig gestellte Fragen zur Netzwerkkonnektivität von Analysis Services | Microsoft-Dokumentation
description: Dieser Artikel bietet Antworten auf einige der häufig gestellten Fragen zur Netzwerkkonnektivität von Analysis Services.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b60cf34e8efed2ed63b6e35cfaf7445edb701610
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82838500"
---
# <a name="frequently-asked-questions-about-analysis-services-network-connectivity"></a>Häufig gestellte Fragen zur Netzwerkkonnektivität von Analysis Services

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zum Herstellen einer Verbindung mit Speicherkonten, Datenquellen, Firewalls und IP-Adressen.

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

**Frage**: Wie kann ich Sicherungen und Wiederherstellungen mit einem Speicherkonto durchführen, das sich hinter einer Firewall befindet?   
**Antwort**: Azure Analysis Services verwendet keine festen IP-Adressen oder Diensttags. Der Bereich der IP-Adressen, den Ihre Analysis Services-Server verwenden, kann alles im Bereich der IP-Adressen für die *Azure-Region* sein. Da die IP-Adressen Ihrer Server variabel sind und sich im Laufe der Zeit ändern können, müssen Ihre Firewallregeln den gesamten Bereich der IP-Adressen der Azure-Region berücksichtigen, in dem sich Ihr Server befindet.

**Frage**: Mein Azure-Speicherkonto befindet sich in einer anderen Region als mein Analysis Services-Server. Wie konfiguriere ich die Firewalleinstellungen für Speicherkonten?   
**Antwort**: Wenn sich das Speicherkonto in einer anderen Region befindet, konfigurieren Sie die Firewalleinstellungen für das Speicherkonto so, dass der Zugriff von **ausgewählten Netzwerken** zulässig ist. Geben Sie im **Adressbereich** der Firewall den IP-Adressbereich für die Region an, in der sich der Analysis Services-Server befindet. Informationen zum Ermitteln der IP-Adressbereiche für Azure-Regionen finden Sie unter [Azure-IP-Bereiche und -Diensttags – Public Cloud](https://www.microsoft.com/download/details.aspx?id=56519). Die Konfiguration der Firewalleinstellungen für das Speicherkonto wird unterstützt, um den Zugriff von allen Netzwerken zuzulassen. Allerdings wird die Auswahl bestimmter Netzwerke und die Angabe eines IP-Adressbereichs bevorzugt. 

**Frage**: Mein Azure-Speicherkonto befindet sich in derselben Region wie mein Analysis Services-Server. Wie konfiguriere ich die Firewalleinstellungen für Speicherkonten?   
**Antwort**: Da sich Ihr Analysis Services-Server und Speicherkonto in derselben Region befinden, werden für die Kommunikation zwischen Server und Konto interne IP-Adressbereiche verwendet. Daher werden die Konfiguration einer Firewall für die Verwendung ausgewählter Netzwerke und die Angabe eines IP-Adressbereichs nicht unterstützt. Wenn Unternehmensrichtlinien eine Firewall erfordern, muss diese so konfiguriert werden, dass sie den Zugriff aus allen Netzwerken zulässt.


## <a name="data-source-connections"></a>Datenquellenverbindungen

**Frage**: Ich verfüge über ein VNET für mein Datenquellensystem. Wie kann ich meinen Analysis Services-Servern den Zugriff auf die Datenbank über das VNET gestatten?   
**Antwort**: Azure Analysis Services kann keinem VNET beitreten. Die beste Lösung ist hier die Installation und Konfiguration eines lokalen Datengateways auf dem VNET und die anschließende Konfiguration Ihrer Analysis Services-Server mit der Servereigenschaft **AlwaysUseGateway**. Weitere Informationen finden Sie unter [Verwenden eines Gateways für Datenquellen in einer Azure Virtual Network-Instanz (VNET)](analysis-services-vnet-gateway.md).

**Frage**: Ich verfüge über eine Quelldatenbank hinter einer Firewall. Wie kann ich die Firewall konfigurieren, dass mein Analysis Services-Server darauf zugreifen kann?   
**Antwort**: Azure Analysis Services verwendet keine festen IP-Adressen oder Diensttags. Der Bereich der IP-Adressen, den Ihre Analysis Services-Server verwenden, kann alles im Bereich der IP-Adressen für die *Azure-Region* sein. Sie müssen den *vollständigen Bereich* von IP-Adressen für die Azure-Region Ihres Servers in den Firewallregeln der Quelldatenbank angeben. Eine weitere und möglicherweise sicherere Alternative besteht darin, ein lokales Datengateway zu konfigurieren. Sie können dann Ihre Analysis Services-Server mit der Servereigenschaft [AlwaysUseGateway](analysis-services-vnet-gateway.md#configure-alwaysusegateway-property) konfigurieren und dann sicherstellen, dass das lokale Datengateway über eine durch die Firewallregeln der Datenquelle zugelassene IP-Adresse verfügt.

## <a name="azure-apps-with-ip-address"></a>Azure-Apps mit IP-Adresse

**Frage**: Ich verwende eine auf Azure basierende Anwendung (z. B. Azure Functions, Azure Data Factory) mit einer sich dynamisch ändernden IP-Adresse. Wie kann ich die Azure Analysis Services-Firewallregeln verwalten, um die IP-Adresse, unter der die App ausgeführt wird, dynamisch zuzulassen?   
**Antwort**: Azure Analysis Services unterstützt keine privaten Links, VNETs oder Diensttags. Es gibt einige Open-Source-Lösungen (z. B. https://github.com/mathwro/Scripts/blob/master/Azure/AllowAzure-AnalysisServer.ps1), die die IP-Adresse der Clientanwendung ermitteln und die Firewallregeln automatisch und vorübergehend aktualisieren.


## <a name="next-steps"></a>Nächste Schritte

[Installieren und Konfigurieren eines lokalen Datengateways](analysis-services-gateway-install.md)   
[Herstellen einer Verbindung mit lokalen Datenquellen über ein lokales Datengateway](analysis-services-gateway.md)   
[Verwenden eines Gateways für Datenquellen in einer Azure Virtual Network-Instanz (VNET)](analysis-services-vnet-gateway.md)
