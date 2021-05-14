---
title: Konfigurieren zusätzlicher Kontrollen zur Erfüllung von „FedRAMP High Impact“
description: Ausführlicher Leitfaden zum Konfigurieren zusätzlicher Kontrollen zur Erfüllung von FedRAMP High Impact-Stufen.
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: barbaraselden
ms.author: baselden
manager: celested
ms.reviewer: martinco
ms.date: 4/26/2021
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 16b9842214b107760bf6e4a677099a7415bc6ff2
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108294127"
---
# <a name="configure-additional-controls-to-achieve-fedramp-high-impact-level"></a>Konfigurieren zusätzlicher Kontrollen, um die Stufe „FedRAMP High Impact“ zu erreichen

Die folgende Liste von Kontrollen (und Kontrollerweiterungen) in den unten angegebenen Gruppen erfordert möglicherweise Konfigurationsschritte in Ihrem Azure AD-Mandanten.

Jede Zeile in den folgenden Tabellen enthält Anleitungen, die Sie bei der Entwicklung von Maßnahmen Ihrer Organisation für gemeinsame Verantwortlichkeiten in Bezug auf Kontrolle und/oder Kontrollverbesserungen unterstützen.

## <a name="audit--accountability"></a>Überwachung und Verantwortlichkeit

* AU-02: Überwachungsereignisse

* AU-03: Inhalt der Überwachung
* AU-06: Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung


| Kontroll-ID und Teilbereich| Kundenverantwortungen und Anleitung |
| - | - |
| AU-02 <br>AU-03 <br>AU-03(1)<br>AU-03(2)| **Stellen Sie sicher, dass das System Ereignisse überwachen kann, die in AU-02 (Teil a) definiert sind, und stimmen Sie sich mit anderen Entitäten innerhalb der Teilmenge überwachbarer Ereignisse der Organisation ab, um nachträgliche Untersuchungen zu unterstützen. Implementieren Sie eine zentralisierte Verwaltung von Überwachungsdatensätzen.**<p>Alle Vorgänge für den Kontolebenszyklus (Kontoerstellung, Änderungen, Aktivierung, Deaktivierung und Entfernung) werden innerhalb der Azure AD Überwachungsprotokolle überwacht. Alle Authentifizierungs- und Autorisierungsereignisse werden in Azure AD-Anmeldeprotokollen und alle erkannten Risiken in den Identity Protection-Protokollen überwacht. Jedes dieser Protokolle kann direkt an eine SIEM-Lösung (Security Information & Event Management) wie Azure Sentinel gestreamt werden. Alternativ können Sie auch Azure Event Hub verwenden, um Protokolle in SIEM-Lösungen von Drittanbietern zu integrieren.<p>Überwachen von Ereignissen<li> [Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal](https://docs.microsoft.com///azure/active-directory/reports-monitoring/concept-audit-logs)<li> [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal](https://docs.microsoft.com///azure/active-directory/reports-monitoring/concept-sign-ins)<li>[Anleitung: Untersuchen eines Risikos](https://docs.microsoft.com///azure/active-directory/identity-protection/howto-identity-protection-investigate-risk)<p>SIEM-Integrationen<li> [Verknüpfen von Azure AD-Daten (Azure Active Directory) mit Azure Sentinel](https://docs.microsoft.com///azure/sentinel/connect-azure-active-directory)<li>[Tutorial: Streamen von Azure Active Directory-Protokollen an einen Azure Event Hub](https://docs.microsoft.com///azure/active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub) |
| AU-06<br>AU-06(1)<br>AU-06(3)<br>AU-06(4)<br>AU-06(5)<br>AU-06(6)<br>AU-06(7)<br>AU-06(10)<br>| **Überprüfen und analysieren Sie Überwachungsdatensätze mindestens einmal pro Woche, um unangemessene oder ungewöhnliche Aktivitäten zu identifizieren, und geben Sie die Ergebnisse an die entsprechenden Mitarbeiter weiter.** <p>Die oben für AU-02 und AU-03 bereitgestellten Leitfäden ermöglichen die wöchentliche Überprüfung von Überwachungsdatensätzen und die Weitergabe von Informationen an entsprechende Mitarbeiter. Diese Anforderungen können nicht nur mithilfe von Azure AD erfüllt werden. Dafür ist auch eine SIEM-Lösung wie Azure Sentinel erforderlich.<p>[Was ist Azure Sentinel?](https://docs.microsoft.com///azure/sentinel/overview) |

## <a name="incident-response"></a>Reaktion auf Vorfälle

* IR-04: Vorfallbehandlung

* IR-05: Überwachung von Vorfällen

| Kontroll-ID und Teilbereich| Kundenverantwortungen und Anleitung |
| - | - |
| IR-04<br>IR-04(1)<br>IR-04(2)<br>IR-04(3)<br>IR-04(4)<br>IR-04(6)<br>IR-04(8)<br>IR-05<br>IR-05(1)| **Implementieren Sie Funktionen zur Behandlung und Überwachung von Vorfällen – einschließlich automatisierter Vorfallbearbeitung, dynamischer Neukonfiguration, Betriebskontinuität, Informationskorrelation, Insiderbedrohungen, Korrelation mit externen Organisationen, Überwachung von Vorfällen und automatisierter Nachverfolgung.** <p>Alle Konfigurationsänderungen werden in den Überwachungsprotokollen protokolliert. Authentifizierungs- und Autorisierungsereignisse werden in den Anmeldeprotokollen und ggf. erkannte Risiken in den Identity Protection-Protokollen überwacht. Jedes dieser Protokolle kann direkt an eine SIEM-Lösung (Security Information & Event Management) wie Azure Sentinel gestreamt werden. Alternativ können Sie auch Azure Event Hub verwenden, um Protokolle in SIEM-Lösungen von Drittanbietern zu integrieren. Automatisieren Sie die dynamische Neukonfiguration basierend auf Ereignissen innerhalb von SIEM mithilfe von MS Graph und/oder Azure AD PowerShell.<p>Überwachen von Ereignissen<br><li>[Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal](https://docs.microsoft.com///azure/active-directory/reports-monitoring/concept-audit-logs)<li>[Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal](https://docs.microsoft.com///azure/active-directory/reports-monitoring/concept-sign-ins)<li>[Anleitung: Untersuchen eines Risikos](https://docs.microsoft.com///azure/active-directory/identity-protection/howto-identity-protection-investigate-risk)<p>SIEM-Integrationen<li>[Verknüpfen von Azure AD-Daten (Azure Active Directory) mit Azure Sentinel](https://docs.microsoft.com///azure/sentinel/connect-azure-active-directory)<li>[Tutorial: Streamen von Azure Active Directory-Protokollen an einen Azure Event Hub](https://docs.microsoft.com///azure/active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub)<p>Dynamische Neukonfiguration<li>[AzureAD-Modul](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0)<li>[Übersicht über Microsoft Graph](https://docs.microsoft.com/graph/overview?view=graph-rest-1.0) |


  
## <a name="personnel-security"></a>Personalsicherheit

* PS-04: Personalkündigung

| Kontroll-ID und Teilbereich| Kundenverantwortungen und Anleitung |
| - | - |
| PS-04<br>PS-04(2)| **Stellen Sie sicher, dass Mitarbeiter, die für die Deaktivierung des Systemzugriffs zuständig sind, automatisch benachrichtigt werden.** <p>Deaktivieren Sie Konten, und widerrufen Sie alle zugehörigen Authentifikatoren und Anmeldeinformationen innerhalb von acht Stunden. <p>Konfigurieren Sie die Bereitstellung von Konten in Azure AD (einschließlich Deaktivierung bei Kündigung) über externe Personalsysteme, lokales Active Directory oder direkt in der Cloud. Widerrufen Sie vorhandene Sitzungen, um den Systemzugriff vollständig zu beenden. <p>Kontobereitstellung<li> Einen ausführlichen Leitfaden finden Sie in AC-02. <p>Widerrufen Sie alle zugeordneten Authentifikatoren. <li> [Widerrufen des Benutzerzugriffs in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/enterprise-users/users-revoke-access) |


## <a name="system--information-integrity"></a>System- und Informationsintegrität

* SI-04: Überwachung des Informationssystems

 Kontroll-ID und Teilbereich| Kundenverantwortungen und Anleitung |
| - | - |
| SI-04<br>SI-04(1)| **Implementieren Sie ein informationssystemweites Überwachungs- und Intrusion-Detection-System.**<p>Schließen Sie alle Azure AD-Protokolle (Überwachung, Anmeldung, Identity Protection) in die Überwachungslösung des Informationssystems ein. <p>Streamen Sie Azure AD-Protokolle an eine SIEM-Lösung (siehe IA-04). |

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren der Zugriffssteuerung](fedramp-access-controls.md)

[Konfigurieren Sie Identifizierungs- und Authentifizierungskontrollen, um die FedRAMP High Impact-Stufe mit Azure Active Directory zu erfüllen](fedramp-identification-and-authentication-controls.md)

[Konfigurieren zusätzlicher Kontrollen, um die Stufe „FedRAMP High Impact“ zu erreichen](fedramp-other-controls.md)
 
