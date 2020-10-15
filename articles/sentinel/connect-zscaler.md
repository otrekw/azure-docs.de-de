---
title: Verbinden von Zscaler-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Zscaler-Daten mit Azure Sentinel verbinden.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: ef6677b6f1103c26bd719a3585800765a029f7fb
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92056837"
---
# <a name="connect-zscaler-internet-access-to-azure-sentinel"></a>Verbinden von Zscaler Internet Access mit Azure Sentinel

In diesem Artikel wird erläutert, wie Sie die Zscaler Internet Access-Appliance mit Azure Sentinel verbinden. Der Zscaler-Datenconnektor ermöglicht es Ihnen, Ihre ZIA-Protokolle (Zscaler Internet Access) auf einfache Weise mit Azure Sentinel zu verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und die Untersuchung von Daten zu verbessern. Die Verwendung von Zscaler für Azure Sentinel bietet Ihnen mehr Einblicke in die Internetnutzung Ihrer Organisation und erweitert die Funktionen für Sicherheitsvorgänge. 


## <a name="configure-your-zscaler-to-send-cef-messages"></a>Konfigurieren von Zscaler zum Senden von CEF-Nachrichten

1. Auf der Zscaler-Appliance müssen Sie folgende Werte festlegen, damit die erforderlichen Protokolle von der Appliance im erforderlichen Format an den Azure Sentinel-Syslog-Agent (der auf dem Log Analytics-Agent basiert) gesendet werden. Sie können diese Parameter in Ihrer Appliance ändern, sofern Sie diese im Syslog-Daemon auf dem Azure Sentinel-Agent ändern.
    - Protokoll = TCP
    - Port = 514
    - Format = CEF
    - IP-Adresse: Achten Sie darauf, die CEF-Nachrichten an die IP-Adresse des virtuellen Computers zu senden, den Sie für diesen Zweck reserviert haben.
 Weitere Informationen finden Sie im [Leitfaden zur Bereitstellung von Zscaler in Azure Sentinel](https://aka.ms/ZscalerCEFInstructions).
 
   > [!NOTE]
   > Diese Lösung unterstützt Syslog RFC 3164 und RFC 5424.


1. Um das relevante Schema in Log Analytics für die CEF-Ereignisse zu verwenden, suchen Sie nach `CommonSecurityLog`.
1. Fahren Sie mit [SCHRITT 3: Überprüfen der Konnektivität](connect-cef-verify.md) fort.


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Zscaler Internet Access mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.


