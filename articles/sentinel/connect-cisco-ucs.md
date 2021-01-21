---
title: Verknüpfen von Cisco UCS-Daten (Unified Computing System) mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Cisco UCS-Datenconnector Cisco UCS-Protokolle in Azure Sentinel pullen. Zeigen Sie Cisco UCS-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
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
ms.date: 01/17/2021
ms.author: yelevin
ms.openlocfilehash: caa83b9149f39f69d0cbf44a2d6cb01fdaf29721
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2021
ms.locfileid: "98567813"
---
# <a name="connect-your-cisco-unified-computing-system-ucs-to-azure-sentinel"></a>Verknüpfen Ihres Cisco UCS (Unified Computing System) mit Azure Sentinel

> [!IMPORTANT]
> Der Cisco UCS-Connector befindet sich derzeit in der **Vorschauversion**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

In diesem Artikel wird erläutert, wie Sie Ihre Cisco UCS-Appliance (Unified Computing System) mit Azure Sentinel verbinden. Mit dem Cisco UCS-Datenconnector lassen sich Ihre UCS-Protokolle problemlos mit Azure Sentinel verbinden, sodass Sie die Daten dann in Arbeitsmappen anzeigen, als Vorlage für benutzerdefinierte Warnungen und zur Verbesserung der Untersuchung von Problemen verwenden können. Für die Integration von Cisco UCS und Azure Sentinel wird Syslog genutzt.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Ihre Cisco UCS-Lösung muss so konfiguriert sein, dass Protokolle über Syslog exportiert werden.

## <a name="forward-cisco-ucs-logs-to-the-syslog-agent"></a>Weiterleiten von Cisco UCS-Protokollen an den Syslog-Agent  

Konfigurieren Sie Cisco UCS für das Weiterleiten von Syslog-Nachrichten an Ihren Azure Sentinel-Arbeitsbereich über den Syslog-Agent.

1. Wählen Sie im Azure Sentinel-Navigationsmenü **Datenconnectors** aus.

1. Wählen Sie im **Datenconnectors**-Katalog den Connector **Cisco UCS (Vorschau)** und dann **Connectorseite öffnen** aus.

1. Befolgen Sie die Anweisungen auf der Connectorseite **Cisco UCS**:

    1. Installieren und Onboarding des Agents für Linux

        - Wählen Sie eine Azure Linux-VM oder einen Nicht-Azure-Linux-Computer (physisch oder virtuell) aus.

    1. Konfigurieren der zu erfassenden Protokolle

        - Wählen Sie die Einrichtungen und Schweregrade in der Konfiguration der erweiterten Einstellungen für den Arbeitsbereich aus.

    1. Konfigurieren und Verbinden von Cisco UCS

        - Befolgen Sie [diese Anweisungen](https://www.cisco.com/c/en/us/support/docs/servers-unified-computing/ucs-manager/110265-setup-syslog-for-ucs.html#configsremotesyslog), um Cisco UCS für die Weiterleitung an syslog zu konfigurieren. Verwenden Sie für den Remoteserver die IP-Adresse des Linux-Computers, auf dem Sie den Linux-Agent installiert haben.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem Sie eine Verbindung hergestellt haben, werden die Daten in Log Analytics unter „Syslog“ angezeigt.

Einige hilfreiche Beispielabfragen finden Sie auf der Registerkarte **Nächste Schritte** auf der Connectorseite.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde beschrieben, wie Sie die Cisco UCS mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
