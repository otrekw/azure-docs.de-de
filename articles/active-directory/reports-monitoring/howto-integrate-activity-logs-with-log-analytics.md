---
title: Streamen von Azure Active Directory-Protokollen in Azure Monitor-Protokolle | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure Active Directory-Protokolle in Azure Monitor-Protokolle integrieren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 06/11/2021
ms.author: markvi
ms.reviewer: besiler
ms.collection: M365-identity-device-management
ms.openlocfilehash: c29b631d3002f0c79fb2dd1b1f26dfa65051368c
ms.sourcegitcommit: 942a1c6df387438acbeb6d8ca50a831847ecc6dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112018955"
---
# <a name="integrate-azure-ad-logs-with-azure-monitor-logs"></a>Integrieren von Azure AD-Protokollen in Azure Monitor-Protokolle

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Mit Azure Monitor-Protokollen können Sie Daten abfragen, um bestimmte Ereignisse zu finden, Trends zu analysieren und datenquellenübergreifende Korrelationen vorzunehmen. Durch die Integration von Azure AD-Aktivitätsprotokollen in Azure Monitor-Protokolle können Sie nun unter anderem folgende Aufgaben ausführen:

 * Vergleichen Ihrer Azure AD-Anmeldeprotokolle mit von Azure Security Center veröffentlichten Sicherheitsprotokollen

 * Behandeln von Leistungsengpässen auf der Anmeldeseite Ihrer Anwendung durch Korrelieren von Anwendungsleistungsdaten aus Azure Application Insights  

Das folgende Video einer Ignite-Sitzung veranschaulicht die Vorteile der Verwendung von Azure Monitor-Protokollen für Azure AD-Protokolle in praktischen Benutzerszenarien.

> [!VIDEO https://www.youtube.com/embed/MP5IaCTwkQg?start=1894]

In diesem Artikel erfahren Sie, wie Sie Azure Active Directory-Protokolle (Azure AD) in Azure Monitor integrieren.

## <a name="supported-reports"></a>Unterstützte Berichte

Sie können Überwachungs- und Anmeldeaktivitätsprotokolle zur weiteren Analyse an Azure Monitor-Protokolle weiterleiten. 

* **Überwachungsprotokolle**: Mit dem [Aktivitätsbericht zu Überwachungsprotokollen](concept-audit-logs.md) erhalten Sie Zugriff auf den Verlauf aller Aufgaben, die in Ihrem Mandanten durchgeführt werden.
* **Anmeldeprotokolle**: Mit dem [Aktivitätsbericht zu Anmeldungen](concept-sign-ins.md) können Sie ermitteln, von wem die Aufgaben durchgeführt wurden, die in den Überwachungsprotokollen aufgeführt sind.
* **Bereitstellungsprotokolle:** Mithilfe von [Bereitstellungsprotokollen](../app-provisioning/application-provisioning-log-analytics.md) können Sie überwachen, welche Benutzer in allen Ihren Anwendungen von Drittanbietern erstellt, aktualisiert und gelöscht wurden. 

> [!NOTE]
> B2C-bezogene Aktivitätsprotokolle für Überwachungen und Anmeldungen werden derzeit nicht unterstützt.
>

## <a name="prerequisites"></a>Voraussetzungen 

Sie benötigen Folgendes, um dieses Feature verwenden zu können:

* Ein Azure-Abonnement. Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/free/).
* Einen Azure AD-Mandanten.
* Einen Benutzer, der als *globaler Administrator* oder *Sicherheitsadministrator* für den Azure AD-Mandanten fungiert.
* Einen Log Analytics-Arbeitsbereich in Ihrem Azure-Abonnement. Informationen zum Erstellen eines Log Analytics-Arbeitsbereichs finden Sie [hier](../../azure-monitor/logs/quick-create-workspace.md).

## <a name="licensing-requirements"></a>Lizenzanforderungen

Für die Verwendung dieses Features ist ein Azure AD Premium P1- oder P2-Mandant erforderlich. Den Lizenztyp Ihres Mandanten finden Sie auf der Seite **[Übersicht](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview)** in **Azure Active Directory**.

![Mandanteninformationen](./media/howto-integrate-activity-logs-with-log-analytics/tenant-information.png)
 
Wenn Sie wissen möchten, wie lange die Aktivitätsdaten in einem Premium-Mandanten gespeichert werden, finden Sie weitere Informationen unter [Wie lange speichert Azure AD die Daten?](reference-reports-data-retention.md#how-long-does-azure-ad-store-the-data)

## <a name="send-logs-to-azure-monitor"></a>Senden von Protokollen an Azure Monitor

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

2. Klicken Sie auf **Azure Active Directory** > **Diagnoseeinstellungen** -> **Diagnoseeinstellung hinzufügen**. Sie können auch auf der Seite **Überwachungsprotokolle** oder **Anmeldungen** auf **Exporteinstellungen** klicken, um zur Konfigurationsseite für die Diagnoseeinstellungen zu gelangen.  
    
3. Aktivieren Sie im Menü **Diagnoseeinstellungen** das Kontrollkästchen **An Log Analytics-Arbeitsbereich senden**, und wählen Sie anschließend **Konfigurieren** aus.

4. Wählen Sie den Log Analytics-Arbeitsbereich aus, an den Sie die Protokolle senden möchten, oder erstellen Sie einen neuen Arbeitsbereich im dafür vorgesehenen Dialogfeld.  

5. Führen Sie einen oder beide der folgenden Schritte aus:
    * Aktivieren Sie das Kontrollkästchen **AuditLogs**, um Überwachungsprotokolle an den Log Analytics-Arbeitsbereich zu senden. 
    * Aktivieren Sie das Kontrollkästchen **SignInLogs**, um Anmeldeprotokolle an den Log Analytics-Arbeitsbereich zu senden.

6. Klicken Sie auf **Speichern**, um die Einstellung zu speichern.

    ![Diagnoseeinstellungen](./media/howto-integrate-activity-logs-with-log-analytics/Configure.png)

7. Vergewissern Sie sich nach etwa 15 Minuten, dass Ereignisse an Ihren Log Analytics-Arbeitsbereich gestreamt werden.

## <a name="next-steps"></a>Nächste Schritte

* [Analysieren von Azure AD-Aktivitätsprotokollen mit Azure Monitor-Protokollen](howto-analyze-activity-logs-log-analytics.md)
* [Installieren und Verwenden der Log Analytics-Ansichten für Azure Active Directory](howto-install-use-log-analytics-views.md)