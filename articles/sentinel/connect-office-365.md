---
title: Herstellen einer Verbindung von Office 365-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Verbindung von Office 365-Daten mit Azure Sentinel herstellen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: df5aade7244f69e7264f901364ecc164351eec50
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74815789"
---
# <a name="connect-data-from-office-365-logs"></a>Herstellen der Verbindung von Daten aus Office 365-Protokollen



Sie können Überwachungsprotokolle von [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) mit einem einzigen Klick an Azure Sentinel streamen. Sie können Überwachungsprotokolle von mehreren Mandanten an einen einzigen Arbeitsbereich in Azure Sentinel streamen. Der Office 365-Aktivitätsprotokoll-Connector bietet Einblick in die aktuellen Benutzeraktivitäten. Sie erhalten Informationen über unterschiedliche Benutzer-, Administrator-, System- und Richtlinienaktionen und -ereignisse aus Office 365. Durch Verbinden von Office 365-Protokollen mit Azure Sentinel können Sie diese Daten verwenden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Ihren Untersuchungsprozess zu verbessern.

> [!IMPORTANT]
> Wenn Sie über eine E3-Lizenz verfügen, müssen Sie, bevor Sie über die Verwaltungsaktivitäts-API von Office 365 auf Daten zugreifen können, die einheitliche Überwachungsprotokollierung für Ihre Office 365-Organisation aktivieren. Hierzu aktivieren Sie das Office 365-Überwachungsprotokoll. Anweisungen finden Sie unter [Aktivieren oder Deaktivieren der Office 365-Überwachungsprotokollsuche](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off). Weitere Informationen finden Sie unter [Office 365-Verwaltungsaktivitäts-API – Referenz](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference).

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen globaler Administrator oder Sicherheitsadministrator für Ihren Mandanten sein.
- Stellen Sie sicher, dass auf dem Computer, von dem Sie sich bei Azure Sentinel zum Erstellen der Verbindung angemeldet haben, Port 4433 für Webdatenverkehr geöffnet ist. Dieser Port kann wieder geschlossen werden, nachdem die Verbindung erfolgreich hergestellt wurde.
- Wenn Ihr Mandant nicht über eine Office 365 E3- oder Office 365 E5-Lizenz verfügt, müssen Sie die einheitliche Überwachung auf Ihrem Mandanten mithilfe eines der folgenden Prozesse aktivieren:
    - [Verwenden des Cmdlets „Set-AdminAuditLogConfig“](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-audit/set-adminauditlogconfig?view=exchange-ps) und Aktivieren des Parameters „UnifiedAuditLogIngestionEnabled“.
    - [Oder Verwenden der Benutzeroberfläche des Security & Compliance Centers](https://docs.microsoft.com/office365/securitycompliance/search-the-audit-log-in-security-and-compliance#before-you-begin).

## <a name="connect-to-office-365"></a>Herstellen einer Verbindung mit Office 365

1. Wählen Sie in Azure Sentinel die Option **Datenkonnektoren** aus, und klicken Sie dann auf die Kachel **Office 365**.

2. Wenn Sie ihn noch nicht aktiviert haben, können Sie dies tun, indem Sie zum Blatt **Datenconnectors** navigieren und den **Office 365**-Connector auswählen. Hier können Sie auf **Connectorseite öffnen** klicken, und den Connector aktivieren, indem Sie im Konfigurationsabschnitt **Office 365-Lösung für Ihren Arbeitsbereich aktivieren** auf **Lösung installieren** klicken. War die Office 365-Lösung bereits aktiviert, wird sie in der Verbindungsanzeige als bereits aktiviert gekennzeichnet.
1. Office 365 ermöglicht es Ihnen, Daten von mehreren Mandanten an Azure Sentinel zu streamen. Für jeden Mandanten, mit dem Sie eine Verbindung herstellen möchten, fügen Sie den Mandanten unter **Mandanten mit Azure Sentinel verbinden** (Connect tenants to Azure Sentinel) hinzu. 
1. Es wird eine Active Directory-Anzeige geöffnet. Sie werden aufgefordert, sich für jeden Mandanten, den Sie mit Azure Sentinel verbinden möchten, mit einem globalen Administratorbenutzer zu authentifizieren und für Azure Sentinel Berechtigungen bereitzustellen, um dessen Protokolle lesen zu können. 
5. In der Mandantenliste werden die Azure AD-Verzeichnis-IDs (Mandanten-IDs) und zwei Kontrollkästchen für Exchange- und SharePoint-Protokolle angezeigt. Sie können beliebige oder alle aufgeführten Dienste auswählen, die Sie in Azure Sentinel einspeisen möchten. Azure Sentinel unterstützt derzeit Exchange- und SharePoint-Protokolle in vorhandenen Office 365-Diensten.

4. Sobald Sie die Dienste ausgewählt haben (Exchange, SharePoint usw.), können Sie auf der Seite im Bereich zum Hinzufügen des Mandanten auf „Speichern“ klicken. 

3. Um das entsprechende Schema in Log Analytics für die Office 365-Protokolle zu verwenden, suchen Sie nach **OfficeActivity**.


## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Office 365 mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).

