---
title: Verschieben von Azure Network Watcher-Ressourcen | Microsoft-Dokumentation
description: Verschieben von Azure Network Watcher-Ressourcen in andere Regionen
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2020
ms.author: damendo
ms.openlocfilehash: 97349071fee6a95623e5b5efdc0c9818cfe7b811
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87388326"
---
# <a name="moving-azure-network-watcher-resources-across-regions"></a>Verschieben von Azure Network Watcher-Ressourcen in andere Regionen

## <a name="moving-the-network-watcher-resource"></a>Verschieben der Network Watcher-Ressource
Die Network Watcher-Ressource stellt den Back-End-Dienst für Network Watcher dar und wird vollständig von Azure verwaltet. Kunden müssen sie nicht verwalten. Der Verschiebungsvorgang wird für diese Ressource nicht unterstützt.

## <a name="moving-child-resources-of-network-watcher"></a>Verschieben von untergeordneten Ressourcen von Network Watcher
Das regionsübergreifende Verschieben von Ressourcen wird für eine untergeordnete Ressource des Ressourcentyps `*networkWatcher*` derzeit nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die [Übersicht über Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).
* Weitere Informationen finden Sie unter [FAQ zu Network Watcher](https://docs.microsoft.com/azure/network-watcher/frequently-asked-questions).
