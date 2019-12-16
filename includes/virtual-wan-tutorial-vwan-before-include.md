---
title: include file
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 945d701a2a7dffc259c601b4dab9fa1333ccc066
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896607"
---
Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

* Falls Sie bereits über ein virtuelles Netzwerk verfügen, mit dem Sie eine Verbindung herstellen möchten, stellen Sie sicher, dass sich kein Subnetz Ihres lokalen Netzwerks damit überschneidet. Für Ihr virtuelles Netzwerk ist kein Gatewaysubnetz erforderlich, und es können keine Gateways für virtuelle Netzwerke verwendet werden. Falls Sie nicht über ein virtuelles Netzwerk verfügen, können Sie mit den Schritten in diesem Artikel ein Netzwerk dieser Art erstellen.
* Beschaffen Sie sich einen IP-Adressbereich für Ihre Hubregion. Der Hub ist ein virtuelles Netzwerk, und der von Ihnen für die Hubregion angegebene Adressbereich darf sich nicht mit einem vorhandenen virtuellen Netzwerk überlappen, mit dem Sie eine Verbindung herstellen. Außerdem ist keine Überlappung mit den Adressbereichen möglich, mit denen Sie lokal eine Verbindung herstellen. Falls Sie nicht mit den IP-Adressbereichen in Ihrer lokalen Netzwerkkonfiguration vertraut sind, sollten Sie sich an eine Person wenden, die Ihnen diese Informationen zur Verfügung stellen kann.
* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.