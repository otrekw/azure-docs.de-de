---
title: Verknüpfen von Illusive Attack Management System-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Illusive Attack Management System-Daten mit Azure Sentinel verknüpfen.
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
ms.openlocfilehash: 7d2d3871dd9836e2c68155aa82ce01dced128bf3
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528120"
---
# <a name="connect-your-illusive-attack-management-system-to-azure-sentinel"></a>Verknüpfen Ihrer Illusive Attack Management System-Daten mit Azure Sentinel

In diesem Artikel erfahren Sie, wie Sie [Illusive Attack Management System](https://www.illusivenetworks.com/technology/platform/attack-detection-system) mit Azure Sentinel verknüpfen. Mit dem Connector für Illusive Attack Management System-Daten können Sie Analysedaten zur Angriffsfläche und Incidentprotokolle von Illusive für Azure Sentinel freigeben und diese Informationen in speziellen Dashboards anzeigen. So erhalten Sie Einblick in potenzielle Angriffsflächen für Ihre Organisation (ASM-Dashboard) und können Lateral Movement-Ereignisse im Netzwerk Ihrer Organisation nachverfolgen (ADS-Dashboard).

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="forward-illusive-attack-management-system-logs-to-the-syslog-agent"></a>Weiterleiten von Illusive Attack Management System-Protokollen an den Syslog-Agent  

Konfigurieren Sie Attack Management System so, dass Syslog-Nachrichten im CEF-Format über den Syslog-Agent an Ihren Azure-Arbeitsbereich weitergeleitet werden.

1. Melden Sie sich bei der Illusive-Konsole an, und navigieren Sie zu „Settings“ (Einstellungen) > „Reporting“ (Berichterstellung).

1. Suchen Sie nach „Syslog Servers“ (Syslog-Server).

1. Geben Sie die folgenden Informationen an:
   - Hostname: IP-Adresse des Linux-Syslog-Agents oder FQDN-Hostname
   - Port: 514
   - Protokoll: TCP
   - Überwachungsmeldungen: Überwachungsmeldungen an Server senden

1. Klicken Sie zum Hinzufügen des Syslog-Servers auf die entsprechende Option.

1. Suchen Sie nach „CommonSecurityLog“, um das relevante Log Analytics-Schema für Illusive Attack Management System zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie gelernt, wie Sie Illusive Attack Management System mit Azure Sentinel verknüpfen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
