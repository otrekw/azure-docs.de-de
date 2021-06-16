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
ms.date: 01/07/2021
ms.author: damendo
ms.openlocfilehash: 273dcea805e3b6ec78dfc4bd14be616f6109c097
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112032067"
---
# <a name="moving-azure-network-watcher-resources-across-regions"></a>Verschieben von Azure Network Watcher-Ressourcen in andere Regionen

## <a name="moving-the-network-watcher-resource"></a>Verschieben der Network Watcher-Ressource
Die Network Watcher-Ressource stellt den Back-End-Dienst für Network Watcher dar und wird vollständig von Azure verwaltet. Kunden müssen sie nicht verwalten. Der Verschiebungsvorgang wird für diese Ressource nicht unterstützt.

## <a name="moving-child-resources-of-network-watcher"></a>Verschieben von untergeordneten Ressourcen von Network Watcher
Das regionsübergreifende Verschieben von Ressourcen wird für eine untergeordnete Ressource des Ressourcentyps `*networkWatcher*` derzeit nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die [Übersicht über Network Watcher](./network-watcher-monitoring-overview.md).
* Weitere Informationen finden Sie unter [FAQ zu Network Watcher](./frequently-asked-questions.yml).