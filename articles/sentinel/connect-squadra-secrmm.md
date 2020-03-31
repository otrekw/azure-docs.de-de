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
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: d904e51321870fb4b61a237c23e425034b76dc0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588109"
---
# <a name="connect-your-squadra-technologies-secrmm-data-to-azure-sentinel"></a>Verbinden Ihrer Squadra Technologies secRMM-Daten mit Azure Sentinel 

> [!IMPORTANT]
> Der Squadra Technologies-Datenconnector Security Removable Media Manager (secRMM) in Azure Sentinel befindet sich derzeit in der Public Preview.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Der Squadra Technologies secRMM-Connector ermöglicht Ihnen das einfache Verbinden Ihrer Protokolle der Sicherheitslösung Squadra Technologies secRMM mit Azure Sentinel. Sie können hiermit Dashboards anzeigen, benutzerdefinierte Warnungen erstellen und Untersuchungen verbessern. Mit diesem Connector gewinnen Sie Erkenntnisse aus USB-Wechselspeicherereignissen. Die Integration von Squadra Technologies secRMM mit Azure Sentinel stützt sich auf die REST-API.


> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-squadra-technologies-secrmm"></a>Konfigurieren und Verbinden von Squadra Technologies secRMM 

Squadra Technologies secRMM kann mit Azure Sentinel integriert werden und Protokolle direkt in Azure Sentinel exportieren.
1. Klicken Sie im Azure Sentinel-Portal auf „Data connectors“ (Datenconnectors), und wählen Sie „Squadra Technologies secRMM“ und dann „Open connector page“ (Connectorseite öffnen) aus.

2. Folgen Sie den Schritten im [Squadra Technologies onboarding guide for Azure Sentinel](http://www.squadratechnologies.com/StaticContent/ProductDownload/secRMM/9.9.0.0/secRMMAzureSentinelAdministratorGuide.pdf) (Squadra Technologies-Onboardinghandbuch für Azure Sentinel), um Squadra secRMM-Daten in Azure Sentinel zu kopieren.   


## <a name="find-your-data"></a>Suchen nach den Daten

Nachdem Sie eine erfolgreiche Verbindung hergestellt haben, werden die Daten in Log Analytics unter „CustomLogs secRMM_CL“ angezeigt.
Um das relevante Schema für die Squadra Technologies secRMM-Suche in Log Analytics zu verwenden, suchen Sie nach „secRMM_CL“.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität
Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Squadra Technologies secRMM mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.

