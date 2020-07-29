---
title: Verbinden von Symantec ProxySG-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Symantec ProxySG-Daten mit Azure Sentinel verbinden.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: 51a5c5de7bce07aa6a54b9ff81e957c38cfffdd2
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528092"
---
# <a name="connect-your-symantec-proxysg-to-azure-sentinel"></a>Verbinden von Symantec ProxySG mit Azure Sentinel

In diesem Artikel wird erläutert, wie Sie Ihre [Symantec ProxySG](https://www.broadcom.com/products/cyber-security/network/gateway/proxy-sg-and-advanced-secure-gateway)-Appliance mit Azure Sentinel verbinden. Der Symantec ProxySG-Datenconnector ermöglicht Ihnen, Ihre Symantec ProxySG-Protokolle auf einfache Weise mit Azure Sentinel zu verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und die Untersuchung von Daten zu verbessern. Für die Integration von Symantec ProxySG und Azure Sentinel wird Syslog genutzt.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="forward-symantec-proxysg-logs-to-the-syslog-agent"></a>Weiterleiten von Symantec ProxySG-Protokollen an den Syslog-Agent  

Konfigurieren Sie Symantec ProxySG für das Weiterleiten von Syslog-Nachrichten an Ihren Azure-Arbeitsbereich über den Syslog-Agent.

1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**, und wählen Sie den Connector **Symantec ProxySG** aus.

1. Wählen Sie **Connectorseite öffnen** aus.

1. Befolgen Sie die Anweisungen auf der Seite **Symantec ProxySG**.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem Sie eine Verbindung hergestellt haben, werden die Daten in Log Analytics unter „Syslog“ angezeigt.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie Symantec ProxySG mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.