---
title: Überprüfen der Konnektivität mit Azure Sentinel | Microsoft-Dokumentation
description: Überprüfen Sie die Konnektivität ihrer Sicherheitslösung, um sicherzustellen, dass CEF-Nachrichten an Azure Sentinel weitergeleitet werden.
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
ms.openlocfilehash: e224f6d5cfd82dfc6cb1ce107d111ee0e031247b
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588432"
---
# <a name="step-3-validate-connectivity"></a>SCHRITT 3: Überprüfen der Konnektivität



Wenn Sie den Agenten bereitgestellt und Ihre Sicherheitslösung für die Weiterleitung von CEF-Nachrichten konfiguriert haben, erfahren Sie in diesem Artikel, wie Sie die Konnektivität zwischen Azure Sentinel und Ihrer Sicherheitslösung überprüfen können. 

## <a name="how-to-validate-connectivity"></a>Überprüfen der Konnektivität

1. Öffnen Sie Log Analytics, um sicherzustellen, dass Protokolle mithilfe des CommonSecurityLog-Schemas empfangen werden.<br> Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 

1. Vor dem Ausführen des Skripts empfiehlt es sich, Nachrichten aus Ihrer Sicherheitslösung zu senden, um sicherzustellen, dass sie an den von Ihnen konfigurierten Syslog-Proxycomputer weitergeleitet werden. 
1. Sie müssen über erhöhte Berechtigungen (sudo) auf dem Computer verfügen. Stellen Sie mithilfe des folgenden Befehls sicher, dass Sie auf dem Computer über Python verfügen: `python –version`
1. Führen Sie das folgende Skript aus, um die Konnektivität zwischen Agent, Azure Sentinel und Ihrer Sicherheitslösung zu überprüfen. Es überprüft, ob die Daemonweiterleitung ordnungsgemäß konfiguriert ist, an den richtigen Ports gelauscht wird und die Kommunikation zwischen dem Daemon und dem Log Analytics-Agent nicht blockiert wird. Das Skript sendet auch Pseudonachrichten „TestCommonEventFormat“, um die End-to-End-Konnektivität zu überprüfen. <br>
 `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_troubleshoot.py&&sudo python cef_troubleshoot.py [WorkspaceID]`


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie CEF-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.

