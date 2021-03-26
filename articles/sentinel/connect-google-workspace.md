---
title: Verbinden von Google Workspace-Daten (G Suite) mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Google Workspace-Datenconnector (G Suite) Google Workspace-Aktivitätsereignisse in Azure Sentinel erfassen können. Zeigen Sie Google Workspace-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
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
ms.date: 02/28/2021
ms.author: yelevin
ms.openlocfilehash: 4ada570502d913283ba9ee4cc4c65b7bdd853935
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743343"
---
# <a name="connect-your-google-workspace-to-azure-sentinel"></a>Verbinden von Google Workspace mit Azure Sentinel

> [!IMPORTANT]
> Der Google Workspace-Connector befindet sich derzeit in der **VORSCHAUVERSION**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

Der [Google Workspace](https://workspace.google.com/)-Datenconnector (ehemals G Suite) bietet die Möglichkeit, Google Workspace-Aktivitätsereignisse über die REST-API in Azure Sentinel zu erfassen. Der Connector zeigt diese [Ereignisse](https://developers.google.com/admin-sdk/reports/reference/rest/v1/activities) in Ihrem SOC an, was Ihnen hilft, potenzielle Sicherheitsrisiken zu untersuchen, die Zusammenarbeit im Team zu analysieren, Konfigurationsprobleme zu diagnostizieren, Anmeldungen nachzuverfolgen, Administratoraktivitäten zu analysieren, die Inhaltserstellung und -nutzung durch die Benutzer zu untersuchen und weitere Ereignisse in Ihrer Organisation zu überprüfen.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen über die folgenden Berechtigungen und Anmeldeinformationen verfügen, um Google Workspace-Daten zu sammeln:

- Lese- und Schreibberechtigungen für Ihre Azure Sentinel-Arbeitsbereich.

- Leseberechtigungen für freigegebene Schlüssel für den Arbeitsbereich. [Weitere Informationen zu Arbeitsbereichsschlüsseln](../azure-monitor/agents/log-analytics-agent.md#workspace-id-and-key)

- Lese- und Schreibberechtigungen für Azure Functions, um eine Funktions-App erstellen zu können. [Informieren Sie sich ausführlicher über Azure Functions](../azure-functions/index.yml).

- Die Anmeldeinformation **GooglePickleString** ist für die REST-API erforderlich. [Erfahren Sie mehr über die Google REST-API](https://developers.google.com/admin-sdk/reports/v1/reference/activities). [Erfahren Sie mehr über das Abrufen von Anmeldeinformationen](https://developers.google.com/admin-sdk/reports/v1/quickstart/python).

## <a name="configure-and-connect-google-workspace"></a>Konfigurieren und Verbinden von Google Workspace

Mit Google Workspace lassen sich Protokolle über eine Azure Functions-App direkt in Azure Sentinel integrieren und exportieren.

> [!NOTE]
> Dieser Connector verwendet Azure Functions, um eine Verbindung mit der Google Reports-API herzustellen und Aktivitätsereignisse in Azure Sentinel zu pullen. Dies kann zusätzliche Datenerfassungskosten verursachen. Ausführliche Informationen finden Sie auf der Seite mit den [Preisen von Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**. 

1. Wählen Sie aus dem Connectorkatalog die Option **Google Workspace (G Suite) (Vorschau)** und dann **Connectorseite öffnen** aus.

1. Führen Sie die im Abschnitt **Konfiguration** auf der Connectorseite beschriebenen Schritte aus.

## <a name="validate-connectivity-and-find-your-data"></a>Überprüfen der Konnektivität und Suchen Ihrer Daten

Es kann bis zu 20 Minuten dauern, bis die erfassten Daten in Azure Sentinel angezeigt werden.

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in **Protokolle** unter dem Abschnitt **Benutzerdefinierte Protokolle** in den folgenden Tabellen angezeigt:
- `GWorkspace_ReportsAPI_admin_CL`
- `GWorkspace_ReportsAPI_calendar_CL`
- `GWorkspace_ReportsAPI_drive_CL`
- `GWorkspace_ReportsAPI_login_CL`
- `GWorkspace_ReportsAPI_mobile_CL`
- `GWorkspace_ReportsAPI_token_CL`
- `GWorkspace_ReportsAPI_user_accounts_CL`

Dieser Datenconnector ist von einem Parser abhängig, der auf einer Kusto-Funktion basiert und erwartungsgemäß funktionieren muss. [Befolgen Sie diese Schritte](https://aka.ms/sentinel-GWorkspaceReportsAPI-parser) zum Erstellen des **GWorkspaceActivityReports** Kusto-Funktionsalias.

Einige hilfreiche Beispielabfragen finden Sie auf der Registerkarte **Nächste Schritte** auf der Connectorseite.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde beschrieben, wie Sie Google Workspace mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
