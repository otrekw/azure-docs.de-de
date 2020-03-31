---
title: Verknüpfen von Web Application Firewall-Daten mit Azure Sentinel
description: Erfahren Sie, wie Sie Microsoft Web Application Firewall-Daten mit Azure Sentinel verknüpfen.
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: a5cef16694fa2cfae036152d22cfa4473956fc72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588177"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Verbinden von Daten von Microsoft Web Application Firewall



Sie können Protokolle von der Microsoft Web Application Firewall (WAF) für Azure Application Gateway in Azure Sentinel streamen. Diese Web Application Firewall schützt Ihre Anwendungen vor gängigen Sicherheitsrisiken im Web wie Einschleusung von SQL-Befehlen und Cross-Site-Scripting und ermöglicht Ihnen, Regeln anzupassen, um falsch positive Ergebnisse zu verringern. Befolgen Sie diese Anweisungen, um Protokolle Ihrer Microsoft Web Application Firewall in Azure Sentinel zu streamen.


## <a name="prerequisites"></a>Voraussetzungen

- Eine vorhandene Anwendungsgatewayressource

## <a name="connect-to-microsoft-web-application-firewall"></a>Eine Verbindung mit der Microsoft Web Application Firewall

Wenn Sie bereits über eine Microsoft Web Application Firewall verfügen, stellen Sie sicher, dass eine Gatewayressource vorhanden ist.
Sobald Ihre Microsoft Web Application Firewall bereitgestellt ist und Daten empfängt, können die Warnungsdaten problemlos in Azure Sentinel gestreamt werden.
    
1. Wählen Sie im Azure Sentinel-Portal **Datenconnectors** aus.
1. Wählen Sie auf der Seite „Datenconnectors“ die Kachel **WAF** aus.
1. Wechseln Sie zur [Application Gateway-Ressource](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways) , und wählen Sie Ihre Web Application Firewall aus.
    1. Wählen Sie **Diagnoseeinstellungen** aus.
    1. Klicken Sie unter der Tabelle auf **+ Diagnoseeinstellung hinzufügen**.
    1. Geben Sie auf der Seite **Diagnoseeinstellungen** einen **Namen** ein, und klicken Sie auf **An Log Analytics senden**.
    1. Wählen Sie unter **Log Analytics-Arbeitsbereich** den Azure Sentinel-Arbeitsbereich aus.
    1. Wählen Sie die Protokolltypen aus, die Sie analysieren möchten. Folgendes wird empfohlen: ApplicationGatewayAccessLog und ApplicationGatewayFirewallLog.
1. Um in Log Analytics das relevante Schema für die Microsoft Web Application Firewall-Warnungen zu verwenden, suchen Sie nach **AzureDiagnostics**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde erläutert, wie Sie Microsoft Web Application Firewall mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
