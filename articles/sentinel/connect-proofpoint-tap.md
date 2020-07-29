---
title: Verknüpfen von Proofpoint TAP-Daten (Targeted Attack Protection) mit Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Proofpoint TAP-Daten (Targeted Attack Protection) mit Azure Sentinel verknüpfen.
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
ms.openlocfilehash: e4185de879fa6136531e6d4c64908befa1d3bc45
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528068"
---
# <a name="connect-your-proofpoint-tap-to-azure-sentinel-with-azure-function"></a>Verknüpfen von Proofpoint TAP mit Azure Sentinel per Azure-Funktion

Mit dem Connector für Proofpoint Targeted Attack Protection (TAP) können Sie alle Protokolle Ihrer [Proofpoint TAP](https://www.proofpoint.com/us/products/advanced-threat-protection/targeted-attack-protection)-Sicherheitslösungen einfach mit Azure Sentinel verknüpfen, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Bei der Integration von Proofpoint TAP und Azure Sentinel wird Azure Functions verwendet, um Protokolldaten per REST-API zu pullen.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-proofpoint-tap"></a>Konfigurieren und Verknüpfen von Proofpoint TAP

Azure Functions kann in Proofpoint TAP integriert werden und Ereignisse und Protokolle direkt aus Proofpoint TAP pullen und an Azure Sentinel weiterleiten.

1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**, und wählen Sie den Connector **Proofpoint TAP** aus.

1. Wählen Sie **Connectorseite öffnen** aus.

1. Befolgen Sie die Anweisungen auf der Seite **Proofpoint TAP**.

## <a name="find-your-data"></a>Suchen von Daten

Nach erfolgreicher Verbindungsherstellung werden die Daten in Log Analytics unter den Tabellen **ProofpointTAPMessagesBlocked_CL**, **ProofpointTAPMessagesDelivered_CL**, **ProofpointTAPClicksPermitted_CL** und **ProofpointTAPClicksBlocked_CL** angezeigt.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie Proofpoint TAP mithilfe von Azure-Funktions-Apps mit Azure Sentinel verknüpfen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
