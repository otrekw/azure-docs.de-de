---
title: Verbinden von Squid Proxy-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Squid Proxy-Datenconnector Squid Proxy-Protokolle in Azure Sentinel pullen. Zeigen Sie Squid Proxy-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
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
ms.openlocfilehash: b183abf8d42e6f4b1c43db2d87b2650721e0c2a9
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2021
ms.locfileid: "98567831"
---
# <a name="connect-your-squid-proxy-to-azure-sentinel"></a>Verbinden von Squid Proxy mit Azure Sentinel

> [!IMPORTANT]
> Der Squid Proxy-Connector befindet sich derzeit in der **VORSCHAUVERSION**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

In diesem Artikel wird erläutert, wie Sie Ihre Squid Proxy-Appliance mit Azure Sentinel verbinden. Mit dem Squid Proxy-Datenconnector lassen sich Ihre Squid-Protokolle problemlos mit Azure Sentinel verbinden, sodass Sie die Daten dann in Arbeitsmappen anzeigen, als Vorlage für benutzerdefinierte Warnungen und zur Verbesserung der Untersuchung von Problemen verwenden können. Für die Integration von Squid Proxy und Azure Sentinel wird Syslog genutzt.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

## <a name="forward-squid-proxy-logs-to-the-syslog-agent"></a>Weiterleiten von Squid Proxy-Protokollen an den Syslog-Agent  

Konfigurieren Sie Squid Proxy für das Weiterleiten von Syslog-Nachrichten an Ihren Azure-Arbeitsbereich über den Syslog-Agent.

1. Wählen Sie im Azure Sentinel-Navigationsmenü **Datenconnectors** aus.

1. Wählen Sie im **Datenconnectors**-Katalog den Connector **Squid Proxy (Vorschau)** und dann **Connectorseite öffnen** aus.

1. Befolgen Sie die Anweisungen auf der Connectorseite **Squid Proxy**:

    1. Installieren und Onboarding des Agents für Linux

        - Wählen Sie eine Azure Linux-VM oder einen Nicht-Azure-Linux-Computer (physisch oder virtuell) aus.

    1. Konfigurieren der zu erfassenden Protokolle

        - Fügen Sie in den erweiterten Einstellungen des Arbeitsbereichs einen benutzerdefinierten Protokolltyp hinzu, laden Sie eine Beispieldatei hoch, und konfigurieren Sie gemäß den Anweisungen.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in **Protokolle** unter **Benutzerdefinierte Protokolle** in der Tabelle `SquidProxy_CL` angezeigt.

Einige hilfreiche Beispielabfragen finden Sie auf der Registerkarte **Nächste Schritte** auf der Connectorseite.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde beschrieben, wie Sie die Squid Proxy mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
