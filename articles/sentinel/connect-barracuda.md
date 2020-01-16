---
title: Verknüpfen von Barracuda-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Barracuda-Daten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 3b33b4aa-7286-4d79-b461-8e1812edc2e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: rkarlin
ms.openlocfilehash: 97f10058038e3bf4fbcca2cf86d074869ffefed4
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2020
ms.locfileid: "75610691"
---
# <a name="connect-your-barracuda-appliance"></a>Verbinden Ihrer Barracuda-Appliance 



Mit dem Connector für Barracuda Web Application Firewall (WAF) können Sie Ihre Barracuda-Protokolle einfach mit Azure Sentinel verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Dadurch erhalten Sie einen besseren Einblick in das Netzwerk Ihrer Organisation und bessere Möglichkeiten für Sicherheitsvorgänge. Azure Sentinel nutzt die native Integration zwischen **Barracuda** und Log Analytics-Agent, um nahtlose Integration zu bieten. 


> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-barracuda-waf"></a>Konfigurieren und Verbinden von Barracuda WAF
Barracuda Web Application Firewall kann Protokolle über den Log Analytics-Agent direkt in Azure Sentinel integrieren und exportieren.
1. Wechseln Sie zu [Barracuda WAF configuration flow](https://campus.barracuda.com/product/webapplicationfirewall/doc/73696965/configure-the-barracuda-web-application-firewall-to-integrate-with-the-oms-server-and-export-logs/) (Barracuda WAF-Konfigurationsflow), und befolgen Sie die Anweisungen zum Einrichten der Verbindung unter Verwendung der folgenden Parameter:
    - **Workspace ID** (Arbeitsbereichs-ID): Kopieren Sie den Wert Ihrer Arbeitsbereichs-ID von der Seite des Azure Sentinel-Barracuda-Connectors.
    - **Primary key** (Primärer Schlüssel): Kopieren Sie den Wert Ihres primären Schlüssels von der Seite des Azure Sentinel-Barracuda-Connectors.
1. Um das relevante Schema in Log Analytics für die Barracuda-Ereignisse zu verwenden, suchen Sie nach **CommonSecurityLog** und **barracuda_CL**.


## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 



## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Barracuda-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.


