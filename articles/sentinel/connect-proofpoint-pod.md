---
title: Verknüpfen von Proofpoint On Demand Email Security-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Datenconnector für Proofpoint On Demand Email Security zum Pullen von POD Email Security-Protokollen in Azure Sentinel verwenden. Zeigen Sie POD Email Security-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
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
ms.openlocfilehash: 0e256f2fe4b8d4275e331e0fe1fadd67e1d96655
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2021
ms.locfileid: "98567832"
---
# <a name="connect-your-proofpoint-on-demand-email-security-pod-solution-to-azure-sentinel"></a>Verknüpfen der Proofpoint On Demand Email Security-Lösung (POD) mit Azure Sentinel

> [!IMPORTANT]
> Der Connector für Proofpoint On Demand Email Security befindet sich derzeit in der **Vorschauphase**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

In diesem Artikel wird erläutert, wie Sie Ihre Proofpoint On Demand Email Security-Appliance mit Azure Sentinel verbinden. Mit dem POD-Datenconnector lassen sich Ihre POD-Protokolle problemlos mit Azure Sentinel verbinden, sodass Sie die Daten dann in Arbeitsmappen anzeigen, als Vorlage für benutzerdefinierte Warnungen und zur Verbesserung der Untersuchung von Problemen verwenden können.  Die Integration von Proofpoint On Demand Email Security und Azure Sentinel stützt sich auf die WebSocket-API.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Sie müssen über Leseberechtigungen für freigegebene Schlüssel für den Arbeitsbereich verfügen. [Weitere Informationen zu Arbeitsbereichsschlüsseln](../azure-monitor/platform/log-analytics-agent.md#workspace-id-and-key)

- Sie müssen über Lese- und Schreibberechtigungen für Azure Functions verfügen, um eine Funktions-App erstellen zu können. [Informieren Sie sich ausführlicher über Azure Functions](/azure/azure-functions/).

- Sie benötigen die folgenden Anmeldeinformationen für die WebSocket-API: ProofpointClusterID, ProofpointToken. [Weitere Informationen zur WebSocket-API](https://proofpointcommunities.force.com/community/s/article/Proofpoint-on-Demand-Pod-Log-API)

## <a name="configure-and-connect-proofpoint-on-demand-email-security"></a>Konfigurieren und Verbinden von Proofpoint On Demand Email Security

Proofpoint On Demand Email Security kann in Azure Sentinel integriert werden, und Protokolle können dann direkt in Azure Sentinel exportiert werden.

1. Wählen Sie im Azure Sentinel-Navigationsmenü **Datenconnectors** aus.

1. Wählen Sie im **Datenconnectors**-Katalog den Eintrag **Proofpoint On Demand Email Security (Vorschau)** und dann **Connectorseite öffnen** aus.

1. Führen Sie die im Abschnitt **Konfiguration** auf der Connectorseite beschriebenen Schritte aus.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in **Protokolle** unter *Benutzerdefinierte Protokolle* in den folgenden Tabellen angezeigt:
- `ProofpointPOD_message_CL`
- `ProofpointPOD_maillog_CL`

Einige hilfreiche Beispielabfragen finden Sie auf der Registerkarte **Nächste Schritte** auf der Connectorseite.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 60 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde erläutert, wie Sie Proofpoint On Demand Email Security mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
