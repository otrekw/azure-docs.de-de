---
title: Verbinden von Squadra Technologies secRMM-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Squadra Technologies secRMM-Daten mit Azure Sentinel verbinden.
services: sentinel
author: yelevin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/20/2021
ms.author: yelevin
ms.openlocfilehash: a00b4b1e81c0d644cf1475aa46dda3848fda1365
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98632900"
---
# <a name="connect-your-squadra-technologies-secrmm-data-to-azure-sentinel"></a>Verbinden Ihrer Squadra Technologies secRMM-Daten mit Azure Sentinel 

Der Squadra Technologies secRMM-Connector ermöglicht Ihnen das einfache Verbinden Ihrer Protokolle der Sicherheitslösung Squadra Technologies secRMM mit Azure Sentinel. Sie können hiermit Dashboards anzeigen, benutzerdefinierte Warnungen erstellen und Untersuchungen verbessern. Mit diesem Connector gewinnen Sie Erkenntnisse aus USB-Wechselspeicherereignissen. Die Integration von Squadra Technologies secRMM mit Azure Sentinel stützt sich auf die REST-API.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-squadra-technologies-secrmm"></a>Konfigurieren und Verbinden von Squadra Technologies secRMM 

Squadra Technologies secRMM kann mit Azure Sentinel integriert werden und Protokolle direkt in Azure Sentinel exportieren.
1. Klicken Sie im Azure Sentinel-Portal auf „Data connectors“ (Datenconnectors), und wählen Sie „Squadra Technologies secRMM“ und dann „Open connector page“ (Connectorseite öffnen) aus.

2. Folgen Sie den Schritten im [Squadra Technologies onboarding guide for Azure Sentinel](http://www.squadratechnologies.com/StaticContent/ProductDownload/secRMM/9.9.0.0/secRMMAzureSentinelAdministratorGuide.pdf) (Squadra Technologies-Onboardinghandbuch für Azure Sentinel), um Squadra secRMM-Daten in Azure Sentinel zu kopieren.   

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in der Tabelle `secRMM_CL` im Abschnitt **CustomLogs** unter **Protokolle** angezeigt.

Geben Sie zum Abfragen der secRMM-Protokolle von Squadra Technologies im oberen Bereich des Abfragefensters den Tabellennamen ein.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie Squadra Technologies secRMM mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
