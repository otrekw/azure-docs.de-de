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
ms.openlocfilehash: ea7953be25473357f7ed572fa8b76076edc6f75a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87038116"
---
# <a name="connect-your-illusive-attack-management-system-to-azure-sentinel"></a>Verknüpfen Ihrer Illusive Attack Management System-Daten mit Azure Sentinel

> [!IMPORTANT]
> Der Illusive Attack Management System-Datenconnector in Azure Sentinel befindet sich derzeit in der öffentlichen Vorschau.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Artikel erfahren Sie, wie Sie [Illusive Attack Management System](https://www.illusivenetworks.com/technology/platform/attack-detection-system) mit Azure Sentinel verknüpfen. Mit dem Connector für Illusive Attack Management System-Daten können Sie Analysedaten zur Angriffsfläche und Incidentprotokolle von Illusive für Azure Sentinel freigeben und diese Informationen in speziellen Dashboards anzeigen. So erhalten Sie Einblick in potenzielle Angriffsflächen für Ihre Organisation (ASM-Dashboard) und können Lateral Movement-Ereignisse im Netzwerk Ihrer Organisation nachverfolgen (ADS-Dashboard).

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="forward-illusive-attack-management-system-logs-to-the-syslog-agent"></a>Weiterleiten von Illusive Attack Management System-Protokollen an den Syslog-Agent  

Konfigurieren Sie Attack Management System so, dass Syslog-Nachrichten im CEF-Format über den Syslog-Agent an Ihren Azure-Arbeitsbereich weitergeleitet werden.

1. Melden Sie sich bei der Illusive-Konsole an, und navigieren Sie zu „Settings“ (Einstellungen) > „Reporting“ (Berichterstellung).

1. Suchen Sie nach Syslog-Servern.

1. Geben Sie die folgenden Informationen an:
   - Hostname: IP-Adresse des Linux-Syslog-Agents oder FQDN-Hostname
   - Port: 514
   - Protokoll: TCP
   - Überwachungsmeldungen: Überwachungsmeldungen an Server senden

1. Klicken Sie zum Hinzufügen des Syslog-Servers auf die entsprechende Option.

1. Suchen Sie nach **CommonSecurityLog**, um das relevante Schema in den **Protokollen** für Illusive Attack Management System zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie gelernt, wie Sie Illusive Attack Management System mit Azure Sentinel verknüpfen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
