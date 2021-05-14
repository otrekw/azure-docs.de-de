---
title: Microsoft Cloud App Security-Warnungen werden nicht in Azure Sentinel durch Microsoft 365 Defender-Integration importiert | Microsoft-Dokumentation
description: Dieser Artikel zeigt die Alarme von Microsoft Cloud App Security, die direkt in Azure Sentinel aufgenommen werden müssen, da sie nicht von Microsoft 365 Defender gesammelt werden.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 04/21/2021
ms.author: yelevin
ms.openlocfilehash: 0232cbde5aaddad268aa0e4725ee3587a2461800
ms.sourcegitcommit: 18cd3c1c8cc47258c6a1a04e0e03d6248c52ef24
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107992733"
---
# <a name="microsoft-cloud-app-security-alerts-not-imported-into-azure-sentinel-through-microsoft-365-defender-integration"></a>Microsoft Cloud App Security-Warnungen werden nicht in Azure Sentinel durch Microsoft 365 Defender-Integration importiert

Wie die anderen Microsoft Defender-Komponenten (Defender for Endpoint, Defender for Identity und Defender for Office 365) generiert auch Microsoft Cloud App Security Alarme, die von Microsoft 365 Defender gesammelt werden. Microsoft 365 Defender wiederum erzeugt Vorfälle, die von [Azure Sentinel aufgenommen und mit diesem synchronisiert](microsoft-365-defender-sentinel-integration.md#microsoft-365-defender-incidents-and-microsoft-incident-creation-rules) werden, wenn der Microsoft 365 Defender-Connector aktiviert ist.

Im Gegensatz zu den anderen drei Komponenten werden **nicht alle Arten von** Cloud App Security-Warnungen in Microsoft 365 Defender integriert. Wenn Sie also die Vorfälle aller Cloud App Security-Warnungen in Azure Sentinel haben möchten, müssen Sie Ihre Analyseregeln für die Erstellung von Microsoft-Vorfällen entsprechend anpassen, damit die direkt in Sentinel aufgenommenen Warnmeldungen weiterhin Vorfälle generieren, während die in Microsoft 365 Defender integrierten Warnmeldungen dies nicht tun (damit es keine Duplikate gibt).

## <a name="cloud-app-security-alerts-not-onboarded-to-microsoft-365-defender"></a>Cloud App Security-Warnungen, für die das Onboarding in Microsoft 365 Defender nicht durchgeführt worden ist

Die folgenden Alarme sind nicht in Microsoft 365 Defender integriert, und die Regeln zur Erstellung von Vorfällen von Microsoft, die zu diesen Alarmen führen, sollten weiterhin so konfiguriert werden, dass sie Vorfälle erzeugen.

| Cloud App Security des Anzeigenamens der Warnung | Cloud App Security Alarmname |
|-|-|
| **Warnung zu Zugriffsrichtlinie** | `ALERT_CABINET_INLINE_EVENT_MATCH` |
| **Bei der Aktivitätserstellung über das Protokoll für ermittelten Datenverkehr wurde das Tageslimit überschritten** | `ALERT_DISCOVERY_TRAFFIC_LOG_EXCEEDED_LIMIT` |
| **Warnung der Aktivitätsrichtlinie** | `ALERT_CABINET_EVENT_MATCH_AUDIT` |
| **Warnung zu anomaler Exfiltration** | `ALERT_EXFILTRATION_DISCOVERY_ANOMALY_DETECTION` |
| **Kompromittiertes Konto** | `ALERT_COMPROMISED_ACCOUNT` |
| **Warnung zu App-Sicherheitsverletzung ermittelt** | `ALERT_MANAGEMENT_DISCOVERY_BREACHED_APP` |
| **Inaktives Konto** | `ALERT_ZOMBIE_USER` |
| **Bewertung der Untersuchungspriorität erhöht** | `ALERT_UEBA_INVESTIGATION_PRIORITY_INCREASE` |
| **Einwilligung zu schädlicher OAuth-App** | `ALERT_CABINET_APP_PERMISSION_ANOMALY_MALICIOUS_OAUTH_APP_CONSENT` |
| **Irreführender OAuth-App-Name** | `ALERT_CABINET_APP_PERMISSION_ANOMALY_MISLEADING_APP_NAME` |
| **Irreführender Herausgebername für eine OAuth-App** | `ALERT_CABINET_APP_PERMISSION_ANOMALY_MISLEADING_PUBLISHER_NAME` |
| **Neue App ermittelt** | `ALERT_CABINET_DISCOVERY_NEW_SERVICE` |
| **Verwendung einer nicht sicheren Umleitungs-URL durch eine OAuth-App** | `ALERT_CABINET_APP_PERMISSION_ANOMALY_NON_SECURE_REDIRECT_URL` |
| **Warnung zu OAuth-App-Richtlinie** | `ALERT_CABINET_APP_PERMISSION` |
| **Warnung über verdächtige Aktivität** | `ALERT_SUSPICIOUS_ACTIVITY` |
| **Warnung über verdächtige Cloudnutzung** | `ALERT_DISCOVERY_ANOMALY_DETECTION` |
| **Verdächtiger OAuth-App-Name** | `ALERT_CABINET_APP_PERMISSION_ANOMALY_SUSPICIOUS_APP_NAME` |
| **Fehler bei App-Connector für Systemwarnungen** | `ALERT_MANAGEMENT_DISCONNECTED_API` |
| **Systemwarnung: Fehler beim automatischen Protokollupload in Cloud Discovery** | `ALERT_MANAGEMENT_LOG_COLLECTOR_LOW_RATE` |
| **Systemwarnung: Fehler bei der Cloud Discovery-Protokollverarbeitung** | `ALERT_MANAGEMENT_LOG_COLLECTOR_CONSTANTLY_FAILED_PARSING` |
| **Fehler bei ICAP-Connector für Systemwarnungen** | `ALERT_MANAGEMENT_DLP_CONNECTOR_ERROR` |
| **Fehler bei SIEM-Agent für Systemwarnungen** | `ALERT_MANAGEMENT_DISCONNECTED_SIEM` |
| **Benachrichtigungen durch SIEM-Agent für Systemwarnungen** | `ALERT_MANAGEMENT_NOTIFICATIONS_SIEM` |
| **Ungewöhnliche Region für Cloudressource** | `MCAS_ALERT_ANUBIS_DETECTION_UNCOMMON_CLOUD_REGION` |
|

## <a name="next-steps"></a>Nächste Schritte

- [Verbinden von Microsoft 365 Defender](connect-microsoft-365-defender.md) mit Azure Sentinel.
- Erfahren Sie mehr über [Azure Sentinel](overview.md), [Microsoft 365 Defender](/microsoft-365/security/defender/microsoft-365-defender)und [Cloud App Security](/cloud-app-security/what-is-cloud-app-security).
