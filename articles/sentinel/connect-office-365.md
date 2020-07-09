---
title: Herstellen einer Verbindung von Office 365-Protokollen mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Verbindung von Office 365-Daten mit Azure Sentinel herstellen.
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
ms.date: 05/21/2020
ms.author: yelevin
ms.openlocfilehash: bcd00247486faeea47ef4a4a43fa1df5420321e6
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84248939"
---
# <a name="connect-office-365-logs-to-azure-sentinel"></a>Herstellen einer Verbindung von Office 365-Protokollen mit Azure Sentinel

Der [Office 365](https://docs.microsoft.com/office/)-Protokollconnector liefert Azure Sentinel Informationen zu laufenden Benutzer- und Administratoraktivitäten in **Exchange** und **SharePoint** (einschließlich **OneDrive**). Diese Informationen umfassen Details zu Aktionen wie z. B. Dateidownloads, gesendeten Zugriffsanforderungen, Änderungen an Gruppenereignissen und Postfachvorgängen sowie nähere Informationen zu dem Benutzer, der die Aktionen ausgeführt hat. Das Verbinden von Office 365-Protokollen mit Azure Sentinel ermöglicht Ihnen, diese Daten in Ihren Arbeitsmappen anzuzeigen und zu analysieren, sie zum Erstellen benutzerdefinierter Warnungen abzufragen und zu integrieren, um den Untersuchungsprozess zu verbessern und Ihnen einen besseren Einblick in Ihre Office 365-Sicherheit zu geben.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für Ihren Azure Sentinel-Arbeitsbereich.

- Sie müssen globaler Administrator oder Sicherheitsadministrator für Ihren Mandanten sein.

- Ihre Office 365-Bereitstellung muss sich auf demselben Mandanten wie Ihr Azure Sentinel-Arbeitsbereich befinden.

> [!IMPORTANT]
> - Damit der Connector über die Verwaltungsaktivitäts-API von Office 365 auf Daten zugreifen kann, muss für Ihre Office 365-Bereitstellung die **einheitliche Überwachungsprotokollierung** aktiviert sein. Abhängig vom Typ Ihrer Lizenz für Office 365/Microsoft 365 ist sie möglicherweise standardmäßig aktiviert. Informieren Sie sich im [Security & Compliance Center](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-securitycompliance-center) über den Status der einheitlichen Überwachungsprotokollierung gemäß Ihrem Lizenztyp.
> - Sie können den aktuellen Status der einheitlichen Überwachungsprotokollierung von Office 365 auch manuell aktivieren, deaktivieren und überprüfen. Anweisungen finden Sie unter [Aktivieren oder Deaktivieren der Office 365-Überwachungsprotokollsuche](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off).
> - Weitere Informationen finden Sie unter [Office 365-Verwaltungsaktivitäts-API – Referenz](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference).


   > [!NOTE]
   > Wie bereits oben und auf der Connectorseite unter **Datentypen** erwähnt, unterstützt der Azure Sentinel Office 365-Connector die Erfassung von Überwachungsprotokollen derzeit nur von Microsoft Exchange und SharePoint (einschließlich OneDrive). Es gibt jedoch einige externe Lösungen, wenn Sie [Daten von Teams](https://techcommunity.microsoft.com/t5/azure-sentinel/protecting-your-teams-with-azure-sentinel/ba-p/1265761) oder [andere Office-Daten](https://techcommunity.microsoft.com/t5/azure-sentinel/ingesting-office-365-alerts-with-graph-security-api/ba-p/984888) in Azure Sentinel einbinden möchten. 

## <a name="enable-the-office-365-log-connector"></a>Aktivieren des Office 365-Protokollconnectors

1. Klicken Sie im Azure Sentinel-Navigationsmenü auf **Datenconnectors**.

1. Klicken Sie in der Liste **Datenconnectors** auf **Office 365** und dann unten rechts auf die Schaltfläche **Connectorseite** öffnen.

1. Markieren Sie im Abschnitt mit der Bezeichnung **Konfiguration** die Kontrollkästchen der Office 365-Aktivitätsprotokolle, die Sie mit Azure Sentinel verbinden möchten, und klicken Sie auf **Änderungen** anwenden. 

   > [!NOTE]
   > Wenn Sie bereits mehrere Mandanten mit einer älteren Version des Office 365-Connectors mit Azure Sentinel, die dies unterstützt, verbunden haben, können Sie anzeigen und ändern, welche Protokolle Sie von den einzelnen Mandanten erfassen. Sie können keine weiteren Mandanten hinzufügen, aber Sie können zuvor hinzugefügte Mandanten entfernen.

1. Geben Sie `OfficeActivity` in der ersten Zeile des Abfragefensters ein, um Protokolldaten von Office 365 in Log Analytics abzufragen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Office 365 mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit dem Erkennen von Bedrohungen mit Azure Sentinel mithilfe von [integrierten](tutorial-detect-threats-built-in.md) oder [benutzerdefinierten](tutorial-detect-threats-custom.md) Regeln.

