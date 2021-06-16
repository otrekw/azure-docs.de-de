---
title: Datei einfügen
description: Datei einfügen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/27/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 83bc155050c0c2aa27c565c757d4cb8a38046af6
ms.sourcegitcommit: 1b698fb8ceb46e75c2ef9ef8fece697852c0356c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110658200"
---
Das Gateway für virtuelle Netzwerke verwendet ein bestimmtes Subnetz, das als „Gatewaysubnetz“ bezeichnet wird. Das Gatewaysubnetz ist Teil des IP-Adressbereichs für das virtuelle Netzwerk, den Sie beim Konfigurieren Ihres virtuellen Netzwerks angeben. Es enthält die IP-Adressen, die von den Ressourcen und Diensten des Gateways für virtuelle Netzwerke verwendet werden. 


Bei der Gatewayerstellung geben Sie die Anzahl der im Subnetz enthaltenen IP-Adressen an. Wie viele IP-Adressen erforderlich sind, hängt von der VPN-Gatewaykonfiguration ab, die Sie erstellen möchten. Einige Konfigurationen erfordern mehr IP-Adressen als andere. Es empfiehlt sich, ein Gatewaysubnetz mit /27 oder /28 zu erstellen.


Sollte ein Fehler mit dem Hinweis angezeigt werden, dass sich der Adressraum mit einem Subnetz überschneidet oder dass das Subnetz nicht im Adressraum für Ihr virtuelles Netzwerk enthalten ist, überprüfen Sie Ihren VNet-Adressbereich. Möglicherweise sind in dem Adressbereich, den Sie für Ihr virtuelles Netzwerk erstellt haben, nicht genügend IP-Adressen verfügbar. Wenn Ihr Standardsubnetz beispielsweise den gesamten Adressbereich umfasst, stehen für die Erstellung zusätzlicher Subnetze keine IP-Adressen mehr zur Verfügung. Sie können entweder Ihre Subnetze innerhalb des vorhandenen Adressraums anpassen, um IP-Adressen freizugeben, oder einen zusätzlichen Adressbereich angeben und das Gatewaysubnetz dort erstellen.