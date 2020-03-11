---
title: Herstellen einer Verbindung von Office 365-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Verbindung von Office 365-Daten mit Azure Sentinel herstellen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/12/2020
ms.author: yelevin
ms.openlocfilehash: 43eba727b1dc724aae6eea3ec77de1363c5db73f
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78252510"
---
# <a name="connect-data-from-office-365-logs"></a>Herstellen der Verbindung von Daten aus Office 365-Protokollen



Sie können Überwachungsprotokolle von [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) mit einem einzigen Klick an Azure Sentinel streamen. Sie können Überwachungsprotokolle von Ihrem Office 365 in Ihren Azure Sentinel-Arbeitsbereich auf demselben Mandanten streamen. Der Office 365-Aktivitätsprotokoll-Connector bietet Einblick in die aktuellen Benutzeraktivitäten. Sie erhalten Informationen über unterschiedliche Benutzer-, Administrator-, System- und Richtlinienaktionen und -ereignisse aus Office 365. Durch Verbinden von Office 365-Protokollen mit Azure Sentinel können Sie diese Daten verwenden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Ihren Untersuchungsprozess zu verbessern.

> [!IMPORTANT]
> Wenn Sie über eine E3-Lizenz verfügen, müssen Sie, bevor Sie über die Verwaltungsaktivitäts-API von Office 365 auf Daten zugreifen können, die einheitliche Überwachungsprotokollierung für Ihre Office 365-Organisation aktivieren. Hierzu aktivieren Sie das Office 365-Überwachungsprotokoll. Anweisungen finden Sie unter [Aktivieren oder Deaktivieren der Office 365-Überwachungsprotokollsuche](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off). Weitere Informationen finden Sie unter [Office 365-Verwaltungsaktivitäts-API – Referenz](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference).

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen globaler Administrator oder Sicherheitsadministrator für Ihren Mandanten sein.
- Für Ihren Mandanten muss einheitliche Überwachung aktiviert sein. Für Mandanten mit Office 365 E3- oder E5-Lizenzen ist einheitliche Überwachung standardmäßig aktiviert. <br>Wenn Ihr Mandant nicht über eine dieser Lizenzen verfügt, müssen Sie einheitliche Überwachung auf Ihrem Mandanten mithilfe einer der folgenden Methoden aktivieren:
    - [Verwenden des Cmdlets „Set-AdminAuditLogConfig“](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-audit/set-adminauditlogconfig?view=exchange-ps) und Aktivieren des Parameters „UnifiedAuditLogIngestionEnabled“.
    - [Verwenden der Benutzeroberfläche von Security & Compliance Center](https://docs.microsoft.com/office365/securitycompliance/search-the-audit-log-in-security-and-compliance#before-you-begin).

## <a name="connect-to-office-365"></a>Herstellen einer Verbindung mit Office 365

1. Wählen Sie in Azure Sentinel die Option **Datenkonnektoren** aus, und klicken Sie dann auf die Kachel **Office 365**.

2. Wenn Sie ihn noch nicht aktiviert haben, können Sie dies tun, indem Sie zum Blatt **Datenconnectors** navigieren und den **Office 365**-Connector auswählen. Hier können Sie auf **Connectorseite öffnen** klicken und unter dem Konfigurationsabschnitt mit der Bezeichnung **Konfiguration** alle Office 365-Aktivitätsprotokolle auswählen, die Sie mit Azure Sentinel verbinden möchten. 
   > [!NOTE]
   > Wenn Sie bereits mehrere Mandanten in einer zuvor unterstützten Version des Office 365-Connectors in Azure Sentinel verbunden haben, können Sie anzeigen und ändern, welche Protokolle Sie von den einzelnen Mandanten erfassen. Sie können keine weiteren Mandanten hinzufügen, aber Sie können zuvor hinzugefügte Mandanten entfernen.
3. Um das entsprechende Schema in Log Analytics für die Office 365-Protokolle zu verwenden, suchen Sie nach **OfficeActivity**.


## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Office 365 mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).

