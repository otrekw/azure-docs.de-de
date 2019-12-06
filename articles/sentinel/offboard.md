---
title: Ausführen des Offboardings für Azure Sentinel | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Ihre Azure Sentinel-Instanz löschen.
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
ms.date: 09/26/2019
ms.author: rkarlin
ms.openlocfilehash: a45f273078a622de5e256457fc45b6cb6cae512f
ms.sourcegitcommit: 95931aa19a9a2f208dedc9733b22c4cdff38addc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74464139"
---
# <a name="remove-azure-sentinel-from-your-tenant"></a>Entfernen von Azure Sentinel von Ihrem Mandanten

Wenn Sie Azure Sentinel nicht mehr verwenden möchten, können Sie die Lösung gemäß der Anleitung in diesem Artikel von Ihrem Mandanten entfernen.

## <a name="how-to-delete-azure-sentinel"></a>Gewusst wie: Löschen von Azure Sentinel

Beim Installieren von Azure Sentinel wird im Hintergrund die Lösung **SecurityInsights** in Ihrem ausgewählten Arbeitsbereich installiert. Als Erstes müssen Sie also die Lösung **SecurityInsights** entfernen.

1.  Navigieren Sie zu **Azure Sentinel** > **Konfiguration** > **Arbeitsbereichseinstellungen** > **Lösungen**.

2.  Wählen Sie `SecurityInsights` aus, und klicken Sie darauf.

    ![Suchen nach der Lösung „SecurityInsights“](media/offboard/find-solution.png)

3.  Wählen Sie am oberen Rand der Seite die Option **Löschen** aus.

    > [!IMPORTANT]
    > Wenn Sie den Arbeitsbereich entfernen, wirkt sich dies möglicherweise auf andere Lösungen und Datenquellen aus, die diesen Arbeitsbereich verwenden, darunter Azure Monitor. Unter [Auflisten installierter Überwachungslösungen](../azure-monitor/insights/solutions.md#list-installed-monitoring-solutions) erfahren Sie, wie Sie überprüfen können, welche Lösungen diesen Arbeitsbereich verwenden. Unter [Grundlegendes zur erfassten Datenmenge](../azure-monitor/platform/manage-cost-storage.md#understanding-ingested-data-volume) erfahren Sie, wie Sie überprüfen können, welche Daten welcher Lösungen im Arbeitsbereich erfasst werden.

    ![Löschen der Lösung „SecurityInsights“](media/offboard/delete-solution.png)

## <a name="what-happens-behind-the-scenes"></a>Was passiert im Hintergrund?

Wenn Sie die Lösung löschen, dauert es für Azure Sentinel bis zu 48 Stunden, bis die erste Phase des Löschvorgangs abgeschlossen ist.

Nach Abschluss der Verbindungstrennung beginnt der Offboardingprozess.

**Die Konfiguration der folgenden Connectors wird gelöscht:**
-   Office 365

-   AWS

-   Sicherheitswarnungen für Microsoft-Dienste (Azure ATP, Microsoft Cloud App Security mit Cloud Discovery-Schatten-IT-Berichterstellung, Azure AD Identity Protection, Microsoft Defender ATP, Azure Security Center)

-   Threat Intelligence

-   Häufig verwendete Sicherheitsprotokolle (z. B. CEF-basierte Protokolle, Barracuda und Syslog) (Wenn Sie über Azure Security Center verfügen, werden diese Protokolle weiterhin erfasst.)

-   Windows-Sicherheitsereignisse (Wenn Sie über Azure Security Center verfügen, werden diese Protokolle weiterhin erfasst.)

Innerhalb der ersten 48 Stunden kann auf die Daten und Warnungsregeln (einschließlich Automatisierungskonfiguration in Echtzeit) in Azure Sentinel nicht mehr zugegriffen werden, und sie können auch nicht mehr abgefragt werden.

**Nach 30 Tagen werden die folgenden Ressourcen gelöscht:**

-   Incidents (einschließlich Untersuchungsmetadaten)

-   Warnregeln

-   Lesezeichen

Ihre Playbooks, gespeicherten Arbeitsmappen, gespeicherten Hunting-Abfragen und Notebooks werden nicht gelöscht. **Aufgrund der entfernten Daten kann es in einigen Fällen zu Fehlern kommen. Diese können Sie manuell entfernen.**

Nach der Entfernung des Diensts gilt eine Toleranzperiode von 30 Tagen, während der Sie die Lösung wieder aktivieren können. Ihre Daten und Warnungsregeln werden dann automatisch wiederhergestellt, aber die Verbindungen für die konfigurierten Connectors, die getrennt wurden, müssen von Ihnen wiederhergestellt werden.

> [!NOTE]
> Wenn Sie die Lösung entfernen, ist Ihr Abonnement weiterhin beim Azure Sentinel-Ressourcenanbieter registriert. **Sie können dies manuell entfernen.**




## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde beschrieben, wie Sie den Azure Sentinel-Dienst entfernen. Gehen Sie wie folgt vor, falls Sie Ihre Meinung ändern und den Dienst wieder installieren möchten:
- Führen Sie das [Onboarding für Azure Sentinel](quickstart-onboard.md) durch.

