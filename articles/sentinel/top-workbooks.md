---
title: Häufig verwendete Azure Sentinel-Arbeitsmappen | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die am häufigsten verwendeten Arbeitsmappen für die Verwendung von beliebten integrierten Azure Sentinel-Ressourcen.
services: sentinel
cloud: na
documentationcenter: na
author: batamig
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 03/07/2021
ms.author: bagol
ms.openlocfilehash: b096bcc59465bae0a37013c148880b6beb9eecb0
ms.sourcegitcommit: 89c4843ec85d1baea248e81724781d55bed86417
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108795652"
---
# <a name="commonly-used-azure-sentinel-workbooks"></a>Häufig verwendete Azure Sentinel-Arbeitsmappen

In der folgenden Tabelle sind die am häufigsten verwendeten integrierten Azure Sentinel-Arbeitsmappen aufgeführt.

Greifen Sie in Azure Sentinel auf der linken Seite unter **Bedrohungsmanagement** > **Arbeitsmappen** auf Arbeitsmappen zu, und suchen Sie dann nach der Arbeitsmappe, die Sie verwenden möchten. Weitere Informationen finden Sie unter [Tutorial: Visualisieren und Überwachen Ihrer Daten](tutorial-monitor-your-data.md).

|Arbeitsmappenname  |Beschreibung  |
|---------|---------|
|**Analyseeffizienz**     |  Mit dieser Arbeitsmappe gewinnen Sie Erkenntnisse über die Wirksamkeit Ihrer Analyseregeln, damit Sie eine bessere SOC-Leistung erzielen können. <br><br>Weitere Informationen finden Sie im Artikel zum [Toolkit für datengesteuerte SOCs](https://techcommunity.microsoft.com/t5/azure-sentinel/the-toolkit-for-data-driven-socs/ba-p/2143152).|
|**Azure-Aktivität**     |     Mit dieser Arbeitsmappe gewinnen Sie umfassende Erkenntnisse über die Azure-Aktivität Ihrer Organisation, indem alle Benutzervorgänge und Ereignisse analysiert und korreliert werden. <br><br>Weitere Informationen finden Sie unter [Überwachen mit Azure-Aktivitätsprotokollen](audit-sentinel-data.md#auditing-with-azure-activity-logs).    |
|**Azure AD-Überwachungsprotokolle**     |  Diese Arbeitsmappe verwendet Azure Active Directory-Überwachungsprotokolle zum Gewinnen von Erkenntnissen über Azure AD-Szenarios. <br><br>Weitere Informationen finden Sie unter [Schnellstart: Erste Schritte mit Azure Sentinel](quickstart-get-visibility.md).     |
|**Überwachungs-, Aktivitäts- und Anmeldeprotokolle von Azure AD**     |   Mit dieser Arbeitsmappe gewinnen Sie mit nur einer Arbeitsmappe Erkenntnisse über Überwachungs-, Aktivitäts- und Anmeldedaten von Azure Active Directory. Diese Arbeitsmappe kann sowohl von der Sicherheit als auch von Azure-Administratoren verwendet werden.      |
|**Azure AD-Anmeldeprotokolle**     | Diese Arbeitsmappe verwendet die Azure AD-Anmeldeprotokolle zum Gewinnen von Erkenntnissen über Azure AD-Szenarios.        |
|**Cybersecurity Maturity Model Certification (CMMC)**     |   Diese Arbeitsmappe stellt einen Mechanismus zum Anzeigen von Protokollabfragen bereit, die auf CMMC-Steuerelemente im gesamten Microsoft-Portfolio ausgerichtet sind, einschließlich Microsoft-Sicherheitsangebote, Office 365, Teams, Intune, Windows Virtual Desktop und so weiter. <br><br>Weitere Informationen finden Sie im Artikel zur [Cybersecurity Maturity Model Certification-Arbeitsmappe (CMMC) in der Public Preview](https://techcommunity.microsoft.com/t5/azure-sentinel/what-s-new-cybersecurity-maturity-model-certification-cmmc/ba-p/2111184).|
|**Überwachung der Datenerfassungsintegrität**     |   Mit dieser Arbeitsmappe gewinnen Sie Erkenntnisse über den Datenerfassungsstatus Ihres Arbeitsbereichs. Durch die Anzeige von Monitoren und das Erkennen von Anomalien können Sie die Datenerfassungsintegrität Ihres Arbeitsbereichs bestimmen.  <br><br>Weitere Informationen finden Sie unter [Überwachen der Integrität Ihrer Datenconnectors mit dieser Azure Sentinel-Arbeitsmappe](monitor-data-connector-health.md).    |
|**Ereignisanalyse**     |  Diese Arbeitsmappe ermöglicht das Durchsuchen, Überwachen und Beschleunigen der Analyse von Windows-Ereignisprotokollen, einschließlich aller Ereignisdetails und -attribute wie Sicherheit, Anwendung, System, Setup, Verzeichnisdienst, DNS und so weiter.       |
|**Exchange Online**     |Mit dieser Arbeitsmappe gewinnen Sie Erkenntnisse über Microsoft Exchange Online, indem alle Exchange-Vorgänge und -Benutzeraktivitäten nachverfolgt und analysiert werden.         |
|**Identität & Zugriff**     |   Mit dieser Arbeitsmappe gewinnen Sie über Sicherheitsprotokolle, die Überwachungs- und Anmeldeprotokolle enthalten, Erkenntnisse über Identitäts- und Zugriffsvorgänge bei der Nutzung von Microsoft-Produkten.     |
|**Übersicht über Incidents**     |   Diese Arbeitsmappe dient der Unterstützung bei der Selektierung und Untersuchungen, indem sie umfassende Informationen über einen Incident bereitstellt. Dazu zählen allgemeine Informationen, Entitätsdaten, Selektierungszeit, Entschärfungszeit sowie Kommentare. <br><br>Weitere Informationen finden Sie im Artikel zum [Toolkit für datengesteuerte SOCs](https://techcommunity.microsoft.com/t5/azure-sentinel/the-toolkit-for-data-driven-socs/ba-p/2143152).      |
|**Untersuchungserkenntnisse**     | Mit dieser Arbeitsmappe gewinnen Analysten Erkenntnisse über Incident-, Textmarken- und Entitätsdaten. Allgemeine Abfragen und ausführliche Visualisierungen können Analysten bei der Untersuchung verdächtiger Aktivitäten unterstützen.       |
|**Microsoft Cloud App Security – Ermittlungsprotokolle**     |   Diese Arbeitsmappe stellt ausführliche Informationen zu den Cloud-Apps bereit, die in Ihrer Organisation verwendet werden. Mit ihr gewinnen Sie zudem Erkenntnisse über Nutzungstrends und erhalten Detailinformationen zu bestimmten Benutzern und Anwendungen.  <br><br>Weitere Informationen finden Sie unter [Verknüpfen von Microsoft Cloud App Security-Daten](connect-cloud-app-security.md).|
|**MITRE ATT&CK-Arbeitsmappe**     |   Diese Arbeitsmappe stellt Details zur MITRE ATT&CK-Abdeckung für Azure Sentinel bereit.      |
|**Office 365**     |  Mit dieser Arbeitsmappe gewinnen Sie Erkenntnisse über Office 365, indem alle Vorgänge und Aktivitäten nachverfolgt und analysiert werden. Zeigen Sie Detailinformationen zu SharePoint-, OneDrive- und Exchange-Daten an.       |
|**Sicherheitswarnungen**     |  Diese Arbeitsmappe stellt ein Dashboard für Sicherheitswarnungen in Ihrer Azure Sentinel-Umgebung bereit. <br><br>Weitere Informationen finden Sie unter [Automatisches Erstellen von Incidents aus Microsoft-Sicherheitswarnungen](create-incidents-from-alerts.md).      |
|**Effizienz von Sicherheitsvorgängen**     |  Diese Arbeitsmappe ist für SOC-Manager (Security Operations Center) vorgesehen, die damit allgemeine Effizienzmetriken und Measures zur Leistung des Teams anzeigen können. <br><br>Weitere Informationen finden Sie unter [Bessere Verwaltung des SOC mit Incidentmetriken](manage-soc-with-incident-metrics.md).  |
|**Threat Intelligence**     | Mit dieser Arbeitsmappe gewinnen Sie Erkenntnisse über Bedrohungsindikatoren, einschließlich Typ und Schweregrad von Bedrohungen, Bedrohungsaktivität im Zeitverlauf und Korrelation mit anderen Datenquellen, darunter Office 365 und Firewalls.  <br><br>Weitere Informationen finden Sie unter [Importieren von Threat Intelligence in Azure Sentinel](import-threat-intelligence.md).      |
|**Zero Trust (TIC 3.0)**     |  Stellt eine automatisierte Visualisierung von Zero Trust-Prinzipien bereit, die im Zusammenhang mit dem [Framework für vertrauenswürdige Internetverbindungen](https://www.cisa.gov/trusted-internet-connections) erläutert werden.   <br><br>Weitere Informationen finden Sie im [Blogbeitrag mit der Ankündigung der Zero Trust TIC 3.0-Arbeitsmappe](https://techcommunity.microsoft.com/t5/public-sector-blog/announcing-the-azure-sentinel-zero-trust-tic3-0-workbook/ba-p/2313761).  |


