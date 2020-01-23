---
title: Vorgänge für Azure Resource Manager-Ressourcenanbieter | Microsoft-Dokumentation
description: Hier werden die verfügbaren Vorgängen für Microsoft Azure Resource Manager-Ressourcenanbieter aufgeführt.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: e2655a03fe315feb58668fcda0d19578a5df1a78
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75981090"
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Vorgänge für Azure Resource Manager-Ressourcenanbieter

In diesem Artikel werden die Vorgänge aufgeführt, die jedem Azure Resource Manager-Ressourcenanbieter zur Verfügung stehen. Die Vorgänge können in [benutzerdefinierten Rollen](custom-roles.md) verwendet werden, um eine präzise [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](overview.md) für Ressourcen in Azure bereitzustellen. Die Vorgangszeichenfolgen weisen folgendes Format auf: `{Company}.{ProviderName}/{resourceType}/{action}`. Eine Liste, wie die Namespaces von Ressourcenanbietern den Azure-Diensten zugeordnet werden, finden Sie unter [Zuordnung von Ressourcenanbieter zu Dienst](../azure-resource-manager/management/azure-services-resource-providers.md).

Die Vorgänge für Ressourcenanbieter werden ständig weiterentwickelt. Verwenden Sie [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) oder [az provider operation list](/cli/azure/provider/operation#az-provider-operation-list), um die neuesten Vorgänge abzurufen.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.AAD/domainServices/delete | Löscht Domänendienste. |
> | Action | Microsoft.AAD/domainServices/oucontainer/delete | Dient zum Löschen des Ou-Containers. |
> | Action | Microsoft.AAD/domainServices/oucontainer/read | Dient zum Lesen des Ou-Containers. |
> | Action | Microsoft.AAD/domainServices/oucontainer/write | Dient zum Schreiben in den Ou-Container. |
> | Action | Microsoft.AAD/domainServices/read | Dient zum Lesen der Domänendienste. |
> | Action | Microsoft.AAD/domainServices/write | Schreibt Domänendienste. |
> | Action | Microsoft.AAD/locations/operationresults/read |  |
> | Action | Microsoft.AAD/Operations/read |  |
> | Action | Microsoft.AAD/register/action | Registriert einen Domänendienst. |
> | Action | Microsoft.AAD/unregister/action | Hebt die Registrierung des Domänendiensts auf. |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | microsoft.aadiam/diagnosticsettings/delete | Löscht eine Diagnoseeinstellung. |
> | Action | microsoft.aadiam/diagnosticsettings/read | Liest eine Diagnoseeinstellung. |
> | Action | microsoft.aadiam/diagnosticsettings/write | Schreibt eine Diagnoseeinstellung. |
> | Action | microsoft.aadiam/diagnosticsettingscategories/read | Liest die Kategorien einer Diagnoseeinstellung. |
> | Action | microsoft.aadiam/metricDefinitions/read | Lesen von Metrikdefinitionen auf Mandantenebene |
> | Action | microsoft.aadiam/metrics/read | Lesen von Metriken auf Mandantenebene |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Addons/operations/read | Ruft unterstützte Vorgänge für Ressourcenanbieter ab. |
> | Action | Microsoft.Addons/register/action | Registriert das angegebene Abonnement in Microsoft.Addons. |
> | Action | Microsoft.Addons/supportProviders/listsupportplaninfo/action | Listet die aktuellen Informationen für den Supportplan für das angegebene Abonnement auf. |
> | Action | Microsoft.Addons/supportProviders/supportPlanTypes/delete | Entfernt den angegebenen kanonischen Supportplan. |
> | Action | Microsoft.Addons/supportProviders/supportPlanTypes/read | Ruft den Status des angegebenen kanonischen Supportplans ab. |
> | Action | Microsoft.Addons/supportProviders/supportPlanTypes/write | Fügt den angegebenen Typ des kanonischen Supportplans hinzu. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ADHybridHealthService/addsservices/action | Erstellt eine neue Gesamtstruktur für den Mandanten. |
> | Action | Microsoft.ADHybridHealthService/addsservices/addomainservicemembers/read | Ruft alle Server für den angegebenen Dienstnamen ab. |
> | Action | Microsoft.ADHybridHealthService/addsservices/alerts/read | Ruft Warnungsdetails für die Gesamtstruktur ab, z.B. die Warnungs-ID, das Datum der Warnung, die letzte erkannte Warnung, die Beschreibung der Warnung, den Zeitpunkt des letzten Updates, den Warnungsstatus und Links zur Problembehandlung der Warnung. |
> | Action | Microsoft.ADHybridHealthService/addsservices/configuration/read | Ruft die Dienstkonfiguration für die Gesamtstruktur ab. Beispiel: Name der Gesamtstruktur, Funktionsebene, FSMO-Rolle des Domänennamenmasters, FSMO-Rolle des Schemamasters usw. |
> | Action | Microsoft.ADHybridHealthService/addsservices/delete | Löscht einen Dienst und dessen Server zusammen mit den Integritätsdaten. |
> | Action | Microsoft.ADHybridHealthService/addsservices/dimensions/read | Ruft die Informationen zu Domänen und Standorten für die Gesamtstruktur ab. Beispiel: Integritätsstatus, aktive Warnungen, behobene Warnungen, Eigenschaften wie die Funktionsebene der Domäne, die Gesamtstruktur, der Infrastrukturmaster, PDC, RID-Master usw.  |
> | Action | Microsoft.ADHybridHealthService/addsservices/features/userpreference/read | Ruft die benutzerdefinierte Einstellung für die Gesamtstruktur ab.<br>Beispiel: Namen von metrischen Zählern wie ldapsuccessfulbinds, ntlmauthentications, kerberosauthentications, addsinsightsagentprivatebytes und ldapsearches.<br>Einstellungen für die Diagramme der Benutzeroberfläche (unter anderem). |
> | Action | Microsoft.ADHybridHealthService/addsservices/forestsummary/read | Ruft die Zusammenfassung für die angegebene Gesamtstruktur ab, z.B. den Namen der Gesamtstruktur, die Anzahl der Domänen in dieser Gesamtstruktur, die Anzahl der Standorte, Informationen zu Standorten usw. |
> | Action | Microsoft.ADHybridHealthService/addsservices/metricmetadata/read | Ruft die Liste der unterstützten Metriken für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst können dies z.B. Sperrungen von Extranet-Konten, die Gesamtanzahl der fehlgeschlagenen Anforderungen, ausstehende Tokenanforderungen (Proxy) oder Tokenanforderungen/Sekunde sein.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den ADSync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Action | Microsoft.ADHybridHealthService/addsservices/metrics/groups/read | Diese API ruft die Metrikinformationen für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst kann die API beispielsweise verwendet werden, um Informationen zu Folgendem abzurufen: Sperrungen von Extranet-Konten, Gesamtanzahl nicht erfolgreicher Anforderungen, ausstehende Tokenanforderungen (Proxy), Tokenanforderungen/Sekunde und Ähnliches.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den Sync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Action | Microsoft.ADHybridHealthService/addsservices/premiumcheck/read | Diese API ruft die Liste aller eingebundenen AD-Domänendienste für einen Premium-Mandanten ab. |
> | Action | Microsoft.ADHybridHealthService/addsservices/read | Ruft Dienstdetails für den angegebenen Dienstnamen ab. |
> | Action | Microsoft.ADHybridHealthService/addsservices/replicationdetails/read | Ruft Replikationsdetails für alle Server für den angegebenen Dienstnamen ab. |
> | Action | Microsoft.ADHybridHealthService/addsservices/replicationstatus/read | Ruft die Anzahl der Domänencontroller und deren Replikationsfehler (falls vorhanden) ab. |
> | Action | Microsoft.ADHybridHealthService/addsservices/replicationsummary/read | Ruft die vollständige Liste der Domänencontroller und die Replikationsdetails für die angegebene Gesamtstruktur ab. |
> | Action | Microsoft.ADHybridHealthService/addsservices/servicemembers/action | Fügt eine Serverinstanz zum Dienst hinzu. |
> | Action | Microsoft.ADHybridHealthService/addsservices/servicemembers/credentials/read | Während der Serverregistrierung des AD-Domänendiensts wird diese API aufgerufen, um die Anmeldeinformationen für das Onboarding neuer Server abzurufen. |
> | Action | Microsoft.ADHybridHealthService/addsservices/servicemembers/delete | Löscht einen Server für einen bestimmten Dienst und Mandanten. |
> | Action | Microsoft.ADHybridHealthService/addsservices/write | Erstellt oder aktualisiert die Instanz des AD-Domänendiensts für den Mandanten. |
> | Action | Microsoft.ADHybridHealthService/configuration/action | Aktualisiert die Mandantenkonfiguration. |
> | Action | Microsoft.ADHybridHealthService/configuration/read | Liest die Mandantenkonfiguration. |
> | Action | Microsoft.ADHybridHealthService/configuration/write | Erstellt eine Mandantenkonfiguration. |
> | Action | Microsoft.ADHybridHealthService/logs/contents/read | Ruft den Inhalt der Agent-Installation und die im Blob gespeicherten Registrierungsprotokolle ab. |
> | Action | Microsoft.ADHybridHealthService/logs/read | Ruft Protokolle für die Agent-Installation und die Registrierung für den Mandanten ab. |
> | Action | Microsoft.ADHybridHealthService/operations/read | Ruft die Liste der vom System unterstützten Vorgänge ab. |
> | Action | Microsoft.ADHybridHealthService/register/action | Registriert den Ressourcenanbieter für AD Hybrid Health Service und aktiviert die Erstellung von AD Hybrid Health Service-Ressourcen. |
> | Action | Microsoft.ADHybridHealthService/reports/availabledeployments/read | Ruft die Liste der verfügbaren Regionen ab, die von DevOps verwendet werden, um Incidents von Benutzern zu unterstützen. |
> | Action | Microsoft.ADHybridHealthService/reports/badpassword/read | Ruft die Liste ungültiger Kennworteingabeversuche für alle Benutzer des Active Directory-Verbunddiensts ab. |
> | Action | Microsoft.ADHybridHealthService/reports/badpassworduseridipfrequency/read | Ruft den SAS-URI von Blobs ab, der den Zustand und das mögliche Ergebnis für neu in die Warteschlange eingereihte Berichtsaufträge für die Häufigkeit von ungültigen Eingabeversuchen für Benutzername oder Kennwort pro Benutzer-ID, IP-Adresse und Tag für einen bestimmten Mandanten enthält. |
> | Action | Microsoft.ADHybridHealthService/reports/consentedtodevopstenants/read | Ruft die Liste der für DevOps genehmigten Mandanten ab. Diese wird üblicherweise für den Kundendienst verwendet. |
> | Action | Microsoft.ADHybridHealthService/reports/isdevops/read | Ruft einen Wert ab, der angibt, ob der Mandant für DevOps genehmigt ist. |
> | Action | Microsoft.ADHybridHealthService/reports/selectdevopstenant/read | Aktualisiert die Benutzer-ID (Objekt-ID) für den ausgewählten DevOps-Mandanten. |
> | Action | Microsoft.ADHybridHealthService/reports/selecteddeployment/read | Ruft die ausgewählte Bereitstellung für den angegebenen Mandanten ab. |
> | Action | Microsoft.ADHybridHealthService/reports/tenantassigneddeployment/read | Ruft den Speicherort eines Mandanten anhand der Mandanten-ID ab. |
> | Action | Microsoft.ADHybridHealthService/reports/updateselecteddeployment/read | Ruft den geografischen Standort ab, von dem aus auf die Daten zugegriffen wird. |
> | Action | Microsoft.ADHybridHealthService/services/action | Aktualisiert eine Dienstinstanz im Mandanten. |
> | Action | Microsoft.ADHybridHealthService/services/alerts/read | Liest die Warnungen für einen Dienst. |
> | Action | Microsoft.ADHybridHealthService/services/alerts/read | Liest die Warnungen für einen Dienst. |
> | Action | Microsoft.ADHybridHealthService/services/checkservicefeatureavailibility/read | Überprüft anhand eines Featurenamens, ob ein Dienst die Voraussetzungen für die Verwendung dieses Features erfüllt. |
> | Action | Microsoft.ADHybridHealthService/services/delete | Löscht eine Dienstinstanz im Mandanten. |
> | Action | Microsoft.ADHybridHealthService/services/exporterrors/read | Ruft die Exportfehler für einen bestimmten Synchronisierungsdienst ab. |
> | Action | Microsoft.ADHybridHealthService/services/exportstatus/read | Ruft den Exportstatus für einen bestimmten Dienst ab. |
> | Action | Microsoft.ADHybridHealthService/services/feedbacktype/feedback/read | Ruft Feedback zu Warnungen für einen bestimmten Dienst und Server ab. |
> | Action | Microsoft.ADHybridHealthService/services/metricmetadata/read | Ruft die Liste der unterstützten Metriken für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst können dies z.B. Sperrungen von Extranet-Konten, die Gesamtanzahl der fehlgeschlagenen Anforderungen, ausstehende Tokenanforderungen (Proxy) oder Tokenanforderungen/Sekunde sein.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den ADSync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Action | Microsoft.ADHybridHealthService/services/metrics/groups/average/read | Diese API ruft anhand eines Diensts den Durchschnitt der Metriken für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst kann die API beispielsweise verwendet werden, um Informationen zu Folgendem abzurufen: Sperrungen von Extranet-Konten, Gesamtanzahl nicht erfolgreicher Anforderungen, ausstehende Tokenanforderungen (Proxy), Tokenanforderungen/Sekunde und Ähnliches.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den Sync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Action | Microsoft.ADHybridHealthService/services/metrics/groups/read | Diese API ruft die Metrikinformationen für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst kann die API beispielsweise verwendet werden, um Informationen zu Folgendem abzurufen: Sperrungen von Extranet-Konten, Gesamtanzahl nicht erfolgreicher Anforderungen, ausstehende Tokenanforderungen (Proxy), Tokenanforderungen/Sekunde und Ähnliches.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den Sync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Action | Microsoft.ADHybridHealthService/services/metrics/groups/sum/read | Diese API ruft anhand eines Diensts die aggregierte Ansicht der Metriken für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst kann die API beispielsweise verwendet werden, um Informationen zu Folgendem abzurufen: Sperrungen von Extranet-Konten, Gesamtanzahl nicht erfolgreicher Anforderungen, ausstehende Tokenanforderungen (Proxy), Tokenanforderungen/Sekunde und Ähnliches.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den Sync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Action | Microsoft.ADHybridHealthService/services/monitoringconfiguration/write | Fügt die Überwachungskonfiguration für einen Dienst hinzu oder aktualisiert diese. |
> | Action | Microsoft.ADHybridHealthService/services/monitoringconfigurations/read | Ruft Überwachungskonfigurationen für einen bestimmten Dienst ab. |
> | Action | Microsoft.ADHybridHealthService/services/monitoringconfigurations/write | Fügt Überwachungskonfigurationen zu einem Dienst hinzu. |
> | Action | Microsoft.ADHybridHealthService/services/premiumcheck/read | Diese API ruft die Liste aller eingebundenen Dienste für einen Premium-Mandanten ab. |
> | Action | Microsoft.ADHybridHealthService/services/read | Liest die Dienstinstanz im Mandanten. |
> | Action | Microsoft.ADHybridHealthService/services/reports/blobUris/read | Ruft alle URIs für Berichte über riskante IP-Adressen für die letzten 7 Tage ab. |
> | Action | Microsoft.ADHybridHealthService/services/reports/details/read | Ruft einen Bericht mit den obersten 50 Benutzern mit dem Fehler „Falsches Kennwort“ der letzten sieben Tage ab. |
> | Action | Microsoft.ADHybridHealthService/services/reports/generateBlobUri/action | Generiert einen Bericht über riskante IP-Adressen und gibt einen URI zurück, der darauf verweist. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/action | Erstellt eine Dienstinstanz im Dienst. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/alerts/read | Liest die Warnungen für einen Server. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/credentials/read | Während der Serverregistrierung wird diese API aufgerufen, um die Anmeldeinformationen für das Onboarding neuer Server abzurufen. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/datafreshness/read | Diese API ruft für einen bestimmten Server die Liste der Datentypen, die von den Servern hochgeladen werden, sowie die letzte Uhrzeit für jeden Upload ab. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/delete | Löscht eine Dienstinstanz aus dem Dienst. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/exportstatus/read | Ruft Informationen zu Exportfehlern bei der Synchronisierung für einen bestimmten Synchronisierungsdienst ab. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/metrics/groups/read | Diese API ruft die Metrikinformationen für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst kann die API beispielsweise verwendet werden, um Informationen zu Folgendem abzurufen: Sperrungen von Extranet-Konten, Gesamtanzahl nicht erfolgreicher Anforderungen, ausstehende Tokenanforderungen (Proxy), Tokenanforderungen/Sekunde und Ähnliches.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den Sync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/metrics/read | Ruft die Liste der Connectors und Ausführungsprofilnamen für den angegebenen Dienst und das angegebene Dienstmitglied ab. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/read | Liest die Dienstinstanz im Dienst. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/serviceconfiguration/read | Ruft die Dienstkonfiguration für einen bestimmten Mandanten ab. |
> | Action | Microsoft.ADHybridHealthService/services/tenantwhitelisting/read | Ruft den Status eines Features für die Aufnahme in die Whitelist für einen bestimmten Mandanten ab. |
> | Action | Microsoft.ADHybridHealthService/services/write | Erstellt eine Dienstinstanz im Mandanten. |
> | Action | Microsoft.ADHybridHealthService/unregister/action | Hebt die Registrierung des Abonnements für den AD Hybrid Health Service-Ressourcenanbieter ab. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Advisor/configurations/read | Konfigurationen abrufen |
> | Action | Microsoft.Advisor/configurations/write | Erstellt oder aktualisiert Konfigurationen. |
> | Action | Microsoft.Advisor/generateRecommendations/action | Ruft den Status der Empfehlungsgenerierung ab. |
> | Action | Microsoft.Advisor/generateRecommendations/read | Ruft den Status der Empfehlungsgenerierung ab. |
> | Action | Microsoft.Advisor/metadata/read | Metadaten abrufen |
> | Action | Microsoft.Advisor/operations/read | Ruft die Vorgänge für den Microsoft Advisor ab. |
> | Action | Microsoft.Advisor/recommendations/available/action | Neue Empfehlung im Microsoft Advisor verfügbar |
> | Action | Microsoft.Advisor/recommendations/read | Liest Empfehlungen. |
> | Action | Microsoft.Advisor/recommendations/suppressions/delete | Löscht Unterdrückungen. |
> | Action | Microsoft.Advisor/recommendations/suppressions/read | Ruft Unterdrückungen ab. |
> | Action | Microsoft.Advisor/recommendations/suppressions/write | Erstellt/Aktualisiert Unterdrückungen. |
> | Action | Microsoft.Advisor/register/action | Registriert das Abonnement für den Microsoft Advisor. |
> | Action | Microsoft.Advisor/suppressions/delete | Löscht Unterdrückungen. |
> | Action | Microsoft.Advisor/suppressions/read | Ruft Unterdrückungen ab. |
> | Action | Microsoft.Advisor/suppressions/write | Erstellt/Aktualisiert Unterdrückungen. |
> | Action | Microsoft.Advisor/unregister/action | Hebt die Registrierung für das Abonnement für den Microsoft Advisor auf. |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.AlertsManagement/actionRules/delete | Löscht eine Aktionsregel in einem bestimmten Abonnement. |
> | Action | Microsoft.AlertsManagement/actionRules/read | Ruft alle Aktionsregeln für die Eingabefilter ab. |
> | Action | Microsoft.AlertsManagement/actionRules/write | Erstellt oder aktualisiert eine Aktionsregel in einem bestimmten Abonnement. |
> | Action | Microsoft.AlertsManagement/alerts/changestate/action | Ändert den Status der Warnung. |
> | Action | Microsoft.AlertsManagement/alerts/diagnostics/read | Ruft alle Diagnosen für die Warnung ab. |
> | Action | Microsoft.AlertsManagement/alerts/history/read | Ruft den Verlauf der Warnung ab. |
> | Action | Microsoft.AlertsManagement/alerts/read | Ruft alle Benachrichtigungen für die Eingabefilter ab. |
> | Action | Microsoft.AlertsManagement/alertsList/read | Ruft alle Benachrichtigungen für die Eingabefilter abonnementübergreifend ab. |
> | Action | Microsoft.AlertsManagement/alertsMetaData/read | Ruft Warnungsmetadaten für den Eingabeparameter ab. |
> | Action | Microsoft.AlertsManagement/alertsSummary/read | Ruft die Zusammenfassung der Benachrichtigungen ab. |
> | Action | Microsoft.AlertsManagement/alertsSummaryList/read | Ruft die Zusammenfassung der Benachrichtigungen abonnementübergreifend ab. |
> | Action | Microsoft.AlertsManagement/Operations/read | Liest die angegebenen Vorgänge. |
> | Action | Microsoft.AlertsManagement/register/action | Registriert das Abonnement für die Microsoft-Warnungsverwaltung. |
> | Action | Microsoft.AlertsManagement/smartDetectorAlertRules/delete | Löschen der Smart Detector-Warnungsregel in einem bestimmten Abonnement. |
> | Action | Microsoft.AlertsManagement/smartDetectorAlertRules/read | Abrufen aller Smart Detector-Warnungsregeln für die Eingabefilter. |
> | Action | Microsoft.AlertsManagement/smartDetectorAlertRules/write | Erstellen oder Aktualisieren der Smart Detector-Warnungsregel in einem bestimmten Abonnement. |
> | Action | Microsoft.AlertsManagement/smartGroups/changestate/action | Ändert den Status der intelligenten Gruppe. |
> | Action | Microsoft.AlertsManagement/smartGroups/history/read | Ruft den Verlauf der intelligenten Gruppe ab. |
> | Action | Microsoft.AlertsManagement/smartGroups/read | Ruft alle intelligenten Gruppen für die Eingabefilter ab. |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.AnalysisServices/locations/checkNameAvailability/action | Prüft, ob der angegebene Analysis-Servername gültig ist oder bereits verwendet wird. |
> | Action | Microsoft.AnalysisServices/locations/operationresults/read | Ruft die Informationen des angegebenen Vorgangsergebnisses ab. |
> | Action | Microsoft.AnalysisServices/locations/operationstatuses/read | Ruft die Informationen des angegebenen Vorgangsstatus ab. |
> | Action | Microsoft.AnalysisServices/operations/read | Ruft die Informationen von Vorgängen ab. |
> | Action | Microsoft.AnalysisServices/register/action | Registriert den Analysis Services-Ressourcenanbieter. |
> | Action | Microsoft.AnalysisServices/servers/delete | Löscht den Analysis-Server. |
> | Action | Microsoft.AnalysisServices/servers/listGatewayStatus/action | Listet den Status des Gateways auf, der dem Server zugeordnet ist. |
> | Action | Microsoft.AnalysisServices/servers/read | Ruft die Informationen des angegebenen Analysis-Servers ab. |
> | Action | Microsoft.AnalysisServices/servers/resume/action | Setzt den Analysis-Server fort. |
> | Action | Microsoft.AnalysisServices/servers/skus/read | Ruft die verfügbaren SKU-Informationen für den Server ab. |
> | Action | Microsoft.AnalysisServices/servers/suspend/action | Hält den Analysis-Server an. |
> | Action | Microsoft.AnalysisServices/servers/write | Erstellt oder aktualisiert den angegebenen Analysis-Server. |
> | Action | Microsoft.AnalysisServices/skus/read | Ruft die Informationen der SKUs ab. |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ApiManagement/checkNameAvailability/read | Prüft, ob der angegebene Dienstname verfügbar ist. |
> | Action | Microsoft.ApiManagement/operations/read | Liest alle API-Vorgänge, die für die Microsoft.ApiManagement-Ressource verfügbar sind. |
> | Action | Microsoft.ApiManagement/register/action | Dient zum Registrieren des Microsoft.ApiManagement-Ressourcenanbieters. |
> | Action | Microsoft.ApiManagement/reports/read | Ruft Berichte ab, die nach Zeiträumen, geografischer Region, Entwicklern, Produkten, APIs, Vorgängen, Abonnement und byRequest aggregiert sind. |
> | Action | Microsoft.ApiManagement/service/apis/delete | Löscht die angegebene API aus der API Management-Dienstinstanz |
> | Action | Microsoft.ApiManagement/service/apis/diagnostics/delete | Löscht die angegebene Diagnose aus einer API |
> | Action | Microsoft.ApiManagement/service/apis/diagnostics/read | Listet alle Diagnosen einer API auf oder ruft Details der Diagnose für eine durch ihren Bezeichner angegebene API ab |
> | Action | Microsoft.ApiManagement/service/apis/diagnostics/write | Erstellt eine neue Diagnose für eine API oder aktualisiert eine bereits vorhandene oder aktualisiert Details der Diagnose für eine durch ihren Bezeichner angegebene API |
> | Action | Microsoft.ApiManagement/service/apis/issues/attachments/delete | Löscht den angegebenen Kommentar aus einem Problem |
> | Action | Microsoft.ApiManagement/service/apis/issues/attachments/read | Listet alle Anlagen zu dem der angegebenen API zugeordneten Problem auf oder ruft Details der Problemanlage für eine durch ihren Bezeichner angegebene API ab |
> | Action | Microsoft.ApiManagement/service/apis/issues/attachments/write | Erstellt eine neue Anlage für das Problem in einer API oder aktualisiert eine bereits vorhandene |
> | Action | Microsoft.ApiManagement/service/apis/issues/comments/delete | Löscht den angegebenen Kommentar aus einem Problem |
> | Action | Microsoft.ApiManagement/service/apis/issues/comments/read | Listet alle Kommentare zu dem der angegebenen API zugeordneten Problem auf oder ruft Details des Problemkommentars für eine durch ihren Bezeichner angegebene API ab |
> | Action | Microsoft.ApiManagement/service/apis/issues/comments/write | Erstellt einen neuen Kommentar für das Problem in einer API oder aktualisiert einen bereits vorhandenen |
> | Action | Microsoft.ApiManagement/service/apis/issues/delete | Löscht das angegebene Problem aus einer API |
> | Action | Microsoft.ApiManagement/service/apis/issues/read | Listet die der angegebenen API zugeordneten Probleme auf oder ruft Details des Problems für eine durch ihren Bezeichner angegebene API ab |
> | Action | Microsoft.ApiManagement/service/apis/issues/write | Erstellt ein neues Problem für eine API oder aktualisiert ein bereits vorhandenes oder aktualisiert ein vorhandenes Problem für eine API |
> | Action | Microsoft.ApiManagement/service/apis/operations/delete | Löscht den angegebenen Vorgang aus der API |
> | Action | Microsoft.ApiManagement/service/apis/operations/policies/delete | Löscht die Richtlinienkonfiguration aus dem API-Vorgang |
> | Action | Microsoft.ApiManagement/service/apis/operations/policies/read | Ruft die Liste der Richtlinienkonfigurationen auf Ebene des API-Vorgangs ab oder ruft die Richtlinienkonfiguration auf Ebene des API-Vorgangs ab |
> | Action | Microsoft.ApiManagement/service/apis/operations/policies/write | Erstellt oder aktualisiert die Richtlinienkonfiguration auf Ebene des API-Vorgangs |
> | Action | Microsoft.ApiManagement/service/apis/operations/policy/delete | Löscht die Richtlinienkonfiguration auf Vorgangsebene |
> | Action | Microsoft.ApiManagement/service/apis/operations/policy/read | Ruft die Richtlinienkonfiguration auf Vorgangsebene ab |
> | Action | Microsoft.ApiManagement/service/apis/operations/policy/write | Erstellt die Richtlinienkonfiguration auf Vorgangsebene |
> | Action | Microsoft.ApiManagement/service/apis/operations/read | Listet eine Auflistung der Vorgänge für die angegebene API auf oder ruft Details des durch seinen Bezeichner angegebenen API-Vorgangs ab |
> | Action | Microsoft.ApiManagement/service/apis/operations/tags/delete | Trennt das Tag vom Vorgang |
> | Action | Microsoft.ApiManagement/service/apis/operations/tags/read | Listet alle dem Vorgang zugeordneten Tags auf oder ruft die dem Vorgang zugeordneten Tags ab |
> | Action | Microsoft.ApiManagement/service/apis/operations/tags/write | Weist dem Vorgang ein Tag zu |
> | Action | Microsoft.ApiManagement/service/apis/operations/write | Erstellt einen neuen Vorgang in der API oder aktualisiert einen bereits vorhandenen oder aktualisiert Details des Vorgangs in der durch ihren Bezeichner angegebenen API |
> | Action | Microsoft.ApiManagement/service/apis/operationsByTags/read | Listet eine Auflistung von Vorgängen auf, denen Tags zugeordnet sind |
> | Action | Microsoft.ApiManagement/service/apis/policies/delete | Löscht die Richtlinienkonfiguration aus der API |
> | Action | Microsoft.ApiManagement/service/apis/policies/read | Ruft die Richtlinienkonfiguration auf API-Ebene ab oder ruft die Richtlinienkonfiguration auf API-Ebene ab |
> | Action | Microsoft.ApiManagement/service/apis/policies/write | Erstellt oder aktualisiert die Richtlinienkonfiguration für die API |
> | Action | Microsoft.ApiManagement/service/apis/policy/delete | Löscht die Richtlinienkonfiguration auf API-Ebene |
> | Action | Microsoft.ApiManagement/service/apis/policy/read | Ruft die Richtlinienkonfiguration auf API-Ebene ab |
> | Action | Microsoft.ApiManagement/service/apis/policy/write | Erstellt eine Richtlinienkonfiguration auf API-Ebene |
> | Action | Microsoft.ApiManagement/service/apis/products/read | Listet alle Produkte auf, zu denen die API gehört |
> | Action | Microsoft.ApiManagement/service/apis/read | Listet alle APIs der API Management-Dienstinstanz auf oder ruft Details der durch ihren Bezeichner angegebenen API ab |
> | Action | Microsoft.ApiManagement/service/apis/releases/delete | Entfernt alle Releases der API oder löscht das angegebene Release auf der API |
> | Action | Microsoft.ApiManagement/service/apis/releases/read | Listet alle Releases einer API auf.<br>Ein API-Release wird erstellt, wenn Sie eine API-Revision zur aktuellen machen.<br>Releases werden auch für einen Rollback zu früheren Revisionen verwendet.<br>Die Ergebnisse werden seitenweise ausgegeben und können mit den Parametern $top und $skip eingeschränkt werden.<br>oder gibt Details eines API-Releases zurück |
> | Action | Microsoft.ApiManagement/service/apis/releases/write | Erstellt ein neues Release für die API oder aktualisiert Details des Releases in der durch ihren Bezeichner angegebenen API |
> | Action | Microsoft.ApiManagement/service/apis/revisions/delete | Entfernt alle Revisionen einer API. |
> | Action | Microsoft.ApiManagement/service/apis/revisions/read | Listet alle Revisionen einer API auf |
> | Action | Microsoft.ApiManagement/service/apis/schemas/delete | Löscht die Schemakonfiguration aus der API |
> | Action | Microsoft.ApiManagement/service/apis/schemas/read | Ruft die Schemakonfiguration auf API-Ebene ab oder ruft die Schemakonfiguration auf API-Ebene ab |
> | Action | Microsoft.ApiManagement/service/apis/schemas/write | Erstellt oder aktualisiert die Schemakonfiguration für die API |
> | Action | Microsoft.ApiManagement/service/apis/tagDescriptions/delete | Löscht die Tagbeschreibung aus der API |
> | Action | Microsoft.ApiManagement/service/apis/tagDescriptions/read | Listet alle Tagbeschreibungen im Bereich der API auf. Modell ähnlich wie Swagger: tagDescription wird auf API-Ebene definiert, Tags können jedoch Vorgängen zugewiesen werden – oder ruft die Tagbeschreibung im Bereich der API ab. |
> | Action | Microsoft.ApiManagement/service/apis/tagDescriptions/write | Erstellt oder aktualisiert eine Tagbeschreibung im Bereich der API |
> | Action | Microsoft.ApiManagement/service/apis/tags/delete | Trennt das Tag von der API |
> | Action | Microsoft.ApiManagement/service/apis/tags/read | Listet alle der API zugeordneten Tags auf oder ruft die der API zugeordneten Tags ab |
> | Action | Microsoft.ApiManagement/service/apis/tags/write | Weist der API ein Tag zu |
> | Action | Microsoft.ApiManagement/service/apis/write | Erstellt oder aktualisiert die angegebene API der API Management-Dienstinstanz oder aktualisiert die angegebene API der API Management-Dienstinstanz |
> | Action | Microsoft.ApiManagement/service/apisByTags/read | Listet eine Auflistung von APIs auf, denen Tags zugeordnet sind |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/delete | Löscht die angegebene API-Versionsgruppe |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/read | Listet eine Auflistung von API-Versionsgruppen in der angegebenen Dienstinstanz auf oder ruft Details der durch ihren Bezeichner angegebenen API-Versionsgruppe ab |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/versions/read | Ruft die Liste der Versionsentitäten ab. |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/write | Erstellt oder aktualisiert eine API-Versionsgruppe oder aktualisiert Details der durch ihren Bezeichner angegebenen API-Versionsgruppe |
> | Action | Microsoft.ApiManagement/service/applynetworkconfigurationupdates/action | Aktualisiert die in Virtual Network ausgeführten Microsoft.ApiManagement-Ressourcen, um aktualisierte Netzwerkeinstellungen zu verwenden. |
> | Action | Microsoft.ApiManagement/service/authorizationServers/delete | Löscht die angegebene Autorisierungsserverinstanz |
> | Action | Microsoft.ApiManagement/service/authorizationServers/listSecrets/action | Ruft die Geheimnisse für den Autorisierungsserver ab. |
> | Action | Microsoft.ApiManagement/service/authorizationServers/read | Listet eine Auflistung der in einer Dienstinstanz definierten Autorisierungsserver auf oder ruft die Details des Autorisierungsservers ohne Geheimnisse ab. |
> | Action | Microsoft.ApiManagement/service/authorizationServers/write | Erstellt einen neuen Autorisierungsserver oder aktualisiert Details eines bereits vorhandenen Autorisierungsservers oder aktualisiert Details des durch seinen Bezeichner angegebenen Autorisierungsservers |
> | Action | Microsoft.ApiManagement/service/backends/delete | Löscht das angegebene Back-End |
> | Action | Microsoft.ApiManagement/service/backends/read | Listet eine Auflistung von Back-Ends in der angegebenen Dienstinstanz auf oder ruft Details des durch seinen Bezeichner angegebenen Back-Ends ab |
> | Action | Microsoft.ApiManagement/service/backends/reconnect/action | Benachrichtigt den APIM-Proxy, um nach dem angegebenen Timeout eine neue Verbindung mit dem Back-End zu erstellen. Wenn kein Timeout angegeben wurde, wird ein Timeout von 2 Minuten verwendet. |
> | Action | Microsoft.ApiManagement/service/backends/write | Erstellt oder aktualisiert ein Back-End oder aktualisiert ein vorhandenes Back-End |
> | Action | Microsoft.ApiManagement/service/backup/action | Dient zum Sichern des API Management-Diensts im angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
> | Action | Microsoft.ApiManagement/service/caches/delete | Löscht den angegebenen Cache |
> | Action | Microsoft.ApiManagement/service/caches/read | Listet eine Auflistung aller externen Caches in der angegebenen Dienstinstanz auf oder ruft Details des durch seinen Bezeichner angegebenen Caches ab |
> | Action | Microsoft.ApiManagement/service/caches/write | Erstellt oder aktualisiert einen externen Cache, der in der API Management-Instanz verwendet werden soll oder aktualisiert Details des durch seinen Bezeichner angegebenen Caches |
> | Action | Microsoft.ApiManagement/service/certificates/delete | Löscht das angegebene Zertifikat |
> | Action | Microsoft.ApiManagement/service/certificates/read | Listet eine Auflistung aller Zertifikate in der angegebenen Dienstinstanz auf oder ruft Details des durch seinen Bezeichner angegebenen Zertifikats ab |
> | Action | Microsoft.ApiManagement/service/certificates/write | Erstellt oder aktualisiert das Zertifikat für die Authentifizierung beim Back-End |
> | Action | Microsoft.ApiManagement/service/contentTypes/contentItems/delete | Entfernt das angegebene Inhaltselement. |
> | Action | Microsoft.ApiManagement/service/contentTypes/contentItems/read | Gibt eine Liste mit Inhaltselementen oder Details zu Inhaltselementen zurück. |
> | Action | Microsoft.ApiManagement/service/contentTypes/contentItems/write | Erstellt ein neues Inhaltselement oder aktualisiert das angegebene Inhaltselement. |
> | Action | Microsoft.ApiManagement/service/contentTypes/read | Gibt eine Liste mit Inhaltstypen zurück. |
> | Action | Microsoft.ApiManagement/service/delete | Dient zum Löschen einer API Management-Dienstinstanz. |
> | Action | Microsoft.ApiManagement/service/diagnostics/delete | Löscht die angegebene Diagnose |
> | Action | Microsoft.ApiManagement/service/diagnostics/read | Listet alle Diagnosen der API Management-Dienstinstanz auf oder ruft Details der durch ihren Bezeichner angegebenen Diagnose ab |
> | Action | Microsoft.ApiManagement/service/diagnostics/write | Erstellt eine neue Diagnose oder aktualisiert eine bereits vorhandene oder aktualisiert Details der durch ihren Bezeichner angegebenen Diagnose |
> | Action | Microsoft.ApiManagement/service/gateways/action | Ruft die Gatewaykonfiguration ab oder aktualisiert den Gatewaytakt. |
> | Action | Microsoft.ApiManagement/service/gateways/apis/delete | Löscht die angegebene API aus dem angegebenen Gateway. |
> | Action | Microsoft.ApiManagement/service/gateways/apis/read | Listet eine Sammlung mit den APIs auf, die einem Gateway zugeordnet sind. |
> | Action | Microsoft.ApiManagement/service/gateways/apis/write | Fügt dem angegebenen Gateway eine API hinzu. |
> | Action | Microsoft.ApiManagement/service/gateways/delete | Löscht das angegebene Gateway. |
> | Action | Microsoft.ApiManagement/service/gateways/hostnameConfigurations/read | Listet die Sammlung mit den Hostnamenkonfigurationen für das angegebene Gateway auf. |
> | Action | Microsoft.ApiManagement/service/gateways/keys/action | Ruft Gatewayschlüssel ab. |
> | Action | Microsoft.ApiManagement/service/gateways/read | Listet eine Auflistung von Gateways auf, die bei der Dienstinstanz registriert sind, oder ruft Details des durch seinen Bezeichner angegebenen Gateways ab. |
> | Action | Microsoft.ApiManagement/service/gateways/regeneratePrimaryKey/action | Generiert den primären Gatewayschlüssel erneut und annulliert alle mit ihm erstellten Token. |
> | Action | Microsoft.ApiManagement/service/gateways/regenerateSecondaryKey/action | Generiert den sekundären Gatewayschlüssel erneut und annulliert alle mit ihm erstellten Token. |
> | Action | Microsoft.ApiManagement/service/gateways/token/action | Ruft das SAS-Autorisierungstoken für das Gateway ab. |
> | Action | Microsoft.ApiManagement/service/gateways/write | Erstellt oder aktualisiert ein externes Gateway, das in der API Management-Instanz verwendet werden soll, oder aktualisiert die Details des durch seinen Bezeichner angegebenen Gateways. |
> | Action | Microsoft.ApiManagement/service/getssotoken/action | Ruft ein SSO-Token ab, das verwendet werden kann, um sich als Administrator beim Legacyportal des API Management-Diensts anzumelden. |
> | Action | Microsoft.ApiManagement/service/groups/delete | Löscht die angegebene Gruppe aus der API Management-Dienstinstanz |
> | Action | Microsoft.ApiManagement/service/groups/read | Listet eine Auflistung der in einer Dienstinstanz definierten Gruppen auf oder ruft Details der durch ihren Bezeichner angegebenen Gruppe ab |
> | Action | Microsoft.ApiManagement/service/groups/users/delete | Entfernt vorhandene Benutzer aus vorhandenen Gruppen |
> | Action | Microsoft.ApiManagement/service/groups/users/read | Listet eine Auflistung der der Gruppe zugeordneten Benutzerentitäten auf |
> | Action | Microsoft.ApiManagement/service/groups/users/write | Dient zum Hinzufügen von vorhandenen Benutzern zu vorhandenen Gruppen. |
> | Action | Microsoft.ApiManagement/service/groups/write | Erstellt oder aktualisiert eine Gruppe oder aktualisiert Details der durch ihren Bezeichner angegebenen Gruppe |
> | Action | Microsoft.ApiManagement/service/identityProviders/delete | Löscht die angegebene Konfiguration des Identitätsanbieters |
> | Action | Microsoft.ApiManagement/service/identityProviders/listSecrets/action | Ruft die Geheimnisse des Identitätsanbieters ab. |
> | Action | Microsoft.ApiManagement/service/identityProviders/read | Listet eine Auflistung der in der angegebenen Dienstinstanz konfigurierten Identitätsanbieter auf oder ruft die Konfigurationsdetails des Identitätsanbieters ohne Geheimnisse auf. |
> | Action | Microsoft.ApiManagement/service/identityProviders/write | Erstellt oder aktualisiert die Konfiguration des Identitätsanbieters oder aktualisiert eine vorhandene Konfiguration eines Identitätsanbieters |
> | Action | Microsoft.ApiManagement/service/issues/read | Listet eine Auflistung von Problemen in der angegebenen Dienstinstanz auf oder ruft Details zum API Management-Problem ab |
> | Action | Microsoft.ApiManagement/service/locations/networkstatus/read | Ruft den Netzwerkzugriffsstatus von Ressourcen ab, von denen der Dienst am jeweiligen Standort abhängt. |
> | Action | Microsoft.ApiManagement/service/loggers/delete | Löscht die angegebene Protokollierung |
> | Action | Microsoft.ApiManagement/service/loggers/read | Listet eine Auflistung der Protokollierungen in der angegebenen Dienstinstanz auf oder ruft Details der durch ihren Bezeichner angegebenen Protokollierung ab |
> | Action | Microsoft.ApiManagement/service/loggers/write | Erstellt oder aktualisiert eine Protokollierung oder aktualisiert eine vorhandene Protokollierung |
> | Action | Microsoft.ApiManagement/service/managedeployments/action | Dient zum Ändern der SKU/Einheiten sowie zum Hinzufügen/Entfernen regionaler Bereitstellungen des API Management-Diensts. |
> | Action | Microsoft.ApiManagement/service/namedValues/delete | Löscht den angegebenen benannten Wert aus der API Management-Dienstinstanz. |
> | Action | Microsoft.ApiManagement/service/namedValues/listSecrets/action | Ruft die Geheimnisse des benannten Werts ab, der über den zugehörigen Bezeichner angegeben wird. |
> | Action | Microsoft.ApiManagement/service/namedValues/read | Listet eine Sammlung mit den in einer Dienstinstanz definierten benannten Werten auf oder ruft die Details des benannten Werts ab, der über den zugehörigen Bezeichner angegeben wird. |
> | Action | Microsoft.ApiManagement/service/namedValues/write | Erstellt oder aktualisiert einen benannten Wert oder aktualisiert den spezifischen benannten Wert. |
> | Action | Microsoft.ApiManagement/service/networkstatus/read | Ruft den Netzwerkzugriffsstatus von Ressourcen ab, von denen der Dienst abhängt. |
> | Action | Microsoft.ApiManagement/service/notifications/action | Sendet Benachrichtigungen an einen angegebenen Benutzer. |
> | Action | Microsoft.ApiManagement/service/notifications/read | Listet eine Auflistung der in einer Dienstinstanz definierten Eigenschaften auf oder ruft Details der durch ihren Bezeichner angegebenen Benachrichtigung ab |
> | Action | Microsoft.ApiManagement/service/notifications/recipientEmails/delete | Entfernt die E-Mail-Adresse aus der Liste der Benachrichtigung |
> | Action | Microsoft.ApiManagement/service/notifications/recipientEmails/read | Ruft die Liste der Empfänger-E-Mail-Adressen ab, die eine Benachrichtigung abonniert haben |
> | Action | Microsoft.ApiManagement/service/notifications/recipientEmails/write | Fügt die E-Mail-Adresse der Liste der Empfänger der Benachrichtigung hinzu |
> | Action | Microsoft.ApiManagement/service/notifications/recipientUsers/delete | Entfernt den API Management-Benutzer aus der Liste der Benachrichtigung |
> | Action | Microsoft.ApiManagement/service/notifications/recipientUsers/read | Ruft die Liste der Benutzer ab, die den Empfang der Benachrichtigung abonniert haben |
> | Action | Microsoft.ApiManagement/service/notifications/recipientUsers/write | Fügt den API Management-Benutzer der Liste der Empfänger der Benachrichtigung hinzu |
> | Action | Microsoft.ApiManagement/service/notifications/write | Erstellt oder aktualisiert eine API Management-Benachrichtigung für Herausgeber |
> | Action | Microsoft.ApiManagement/service/openidConnectProviders/delete | Löscht den angegebenen OpenID Connect-Anbieter aus der API Management-Dienstinstanz |
> | Action | Microsoft.ApiManagement/service/openidConnectProviders/listSecrets/action | Ruft die spezifischen Geheimnisse des OpenID Connect-Anbieters ab. |
> | Action | Microsoft.ApiManagement/service/openidConnectProviders/read | Listet alle OpenID Connect-Anbieter auf oder ruft den OpenID Connect-Anbieter ohne Geheimnisse ab. |
> | Action | Microsoft.ApiManagement/service/openidConnectProviders/write | Erstellt oder aktualisiert den OpenID Connect-Anbieter oder aktualisiert den angegebenen OpenID Connect-Anbieter |
> | Action | Microsoft.ApiManagement/service/operationresults/read | Ruft den aktuellen Status eines Vorgangs mit langer Ausführungsdauer ab. |
> | Action | Microsoft.ApiManagement/service/policies/delete | Löscht die globale Richtlinienkonfiguration des API Management-Diensts |
> | Action | Microsoft.ApiManagement/service/policies/read | Listet alle globalen Richtliniendefinitionen des API Management-Diensts auf oder ruft die globale Richtlinienkonfiguration des API Management-Diensts ab |
> | Action | Microsoft.ApiManagement/service/policies/write | Erstellt oder aktualisiert die globale Richtlinienkonfiguration des API Management-Diensts |
> | Action | Microsoft.ApiManagement/service/policy/delete | Löscht die Richtlinienkonfiguration auf Mandantenebene |
> | Action | Microsoft.ApiManagement/service/policy/read | Ruft die Richtlinienkonfiguration auf Mandantenebene ab |
> | Action | Microsoft.ApiManagement/service/policy/write | Erstellt die Richtlinienkonfiguration auf Mandantenebene |
> | Action | Microsoft.ApiManagement/service/policyDescriptions/read | Listet alle Richtlinienbeschreibungen auf. |
> | Action | Microsoft.ApiManagement/service/policySnippets/read | Listet alle Richtlinienausschnitte auf |
> | Action | Microsoft.ApiManagement/service/portalsettings/read | Ruft die Registrierungs-, Anmelde- oder Delegierungseinstellungen für das Portal ab |
> | Action | Microsoft.ApiManagement/service/portalsettings/write | Aktualisiert die Anmeldeeinstellungen oder erstellt oder aktualisiert die Anmeldeeinstellungen oder aktualisiert die Registrierungs-, Anmelde- oder Delegierungseinstellungen oder erstellt oder aktualisiert die Delegierungseinstellungen |
> | Action | Microsoft.ApiManagement/service/products/apis/delete | Löscht die angegebene API aus dem angegebenen Produkt |
> | Action | Microsoft.ApiManagement/service/products/apis/read | Listet eine Auflistung von APIs auf, die einem Produkt zugeordnet sind |
> | Action | Microsoft.ApiManagement/service/products/apis/write | Fügt dem angegebenen Produkt eine API hinzu |
> | Action | Microsoft.ApiManagement/service/products/delete | Löscht das Produkt |
> | Action | Microsoft.ApiManagement/service/products/groups/delete | Löscht die Zuordnung zwischen der angegebenen Gruppe und dem Produkt |
> | Action | Microsoft.ApiManagement/service/products/groups/read | Listet die Auflistung der dem angegebenen Produkt zugeordneten Entwicklergruppen auf |
> | Action | Microsoft.ApiManagement/service/products/groups/write | Fügt eine Zuordnung zwischen der angegebenen Entwicklergruppe und dem angegebenen Produkt hinzu |
> | Action | Microsoft.ApiManagement/service/products/policies/delete | Löscht die Richtlinienkonfiguration aus dem Produkt |
> | Action | Microsoft.ApiManagement/service/products/policies/read | Ruft die Richtlinienkonfiguration auf Produktebene ab oder ruft die Richtlinienkonfiguration auf Produktebene ab |
> | Action | Microsoft.ApiManagement/service/products/policies/write | Erstellt oder aktualisiert die Richtlinienkonfiguration für das Produkt |
> | Action | Microsoft.ApiManagement/service/products/policy/delete | Löscht die Richtlinienkonfiguration auf Produktebene |
> | Action | Microsoft.ApiManagement/service/products/policy/read | Ruft die Richtlinienkonfiguration auf Produktebene ab |
> | Action | Microsoft.ApiManagement/service/products/policy/write | Erstellt eine Richtlinienkonfiguration auf Produktebene |
> | Action | Microsoft.ApiManagement/service/products/read | Listet eine Auflistung der Produkte in der angegebenen Dienstinstanz auf oder ruft Details des durch seinen Bezeichner angegebenen Produkts ab |
> | Action | Microsoft.ApiManagement/service/products/subscriptions/read | Listet die Auflistung der Abonnements für das angegebene Produkt auf |
> | Action | Microsoft.ApiManagement/service/products/tags/delete | Trennt das Tag vom Produkt |
> | Action | Microsoft.ApiManagement/service/products/tags/read | Listet alle dem Produkt zugeordneten Tags auf oder ruft die dem Produkt zugeordneten Tags ab |
> | Action | Microsoft.ApiManagement/service/products/tags/write | Weist dem Produkt ein Tag zu |
> | Action | Microsoft.ApiManagement/service/products/write | Erstellt oder aktualisiert ein Produkt oder aktualisiert Details vorhandener Produkte |
> | Action | Microsoft.ApiManagement/service/productsByTags/read | Listet eine Auflistung von Produkten auf, denen Tags zugeordnet sind |
> | Action | Microsoft.ApiManagement/service/properties/delete | Löscht die angegebene Eigenschaft aus der API Management-Dienstinstanz |
> | Action | Microsoft.ApiManagement/service/properties/listSecrets/action | Ruft die Geheimnisse der Eigenschaft ab, die über den zugehörigen Bezeichner angegeben wird. |
> | Action | Microsoft.ApiManagement/service/properties/read | Listet eine Auflistung der in einer Dienstinstanz definierten Eigenschaften auf oder ruft Details der durch ihren Bezeichner angegebenen Eigenschaft ab |
> | Action | Microsoft.ApiManagement/service/properties/write | Erstellt oder aktualisiert eine Eigenschaft oder aktualisiert die angegebene Eigenschaft |
> | Action | Microsoft.ApiManagement/service/quotas/periods/read | Ruft den Wert des Kontingentzählers für einen bestimmten Zeitraum ab. |
> | Action | Microsoft.ApiManagement/service/quotas/periods/write | Legt den aktuellen Wert für den Kontingentzähler fest. |
> | Action | Microsoft.ApiManagement/service/quotas/read | Ruft Kontingentwerte ab. |
> | Action | Microsoft.ApiManagement/service/quotas/write | Legt den aktuellen Wert für den Kontingentzähler fest. |
> | Action | Microsoft.ApiManagement/service/read | Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz. |
> | Action | Microsoft.ApiManagement/service/regions/read | Listet alle Azure-Regionen auf, in denen der Dienst verfügbar ist |
> | Action | Microsoft.ApiManagement/service/reports/read | Ruft Berichte ab, die nach Zeiträumen, geografischer Region oder Entwicklern aggregiert sind.<br>Dient alternativ zum Abrufen von nach Produkten aggregierten Berichten.<br>Dient alternativ zum Abrufen von Berichten, die nach APIs, Vorgängen oder Abonnement aggregiert sind.<br>Dient alternativ zum Abrufen von Berichtsdaten für Anforderungen. |
> | Action | Microsoft.ApiManagement/service/restore/action | Dient zum Wiederherstellen des API Management-Diensts aus dem angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
> | Action | Microsoft.ApiManagement/service/subscriptions/delete | Löscht das angegebene Abonnement |
> | Action | Microsoft.ApiManagement/service/subscriptions/listSecrets/action | Ruft die angegebenen Abonnementschlüssel ab. |
> | Action | Microsoft.ApiManagement/service/subscriptions/read | Listet alle Abonnements der API Management-Dienstinstanz auf oder ruft die angegebene Abonnemententität ab (ohne Schlüssel). |
> | Action | Microsoft.ApiManagement/service/subscriptions/regeneratePrimaryKey/action | Generiert den Primärschlüssel eines vorhandenen Abonnements der API Management-Dienstinstanz neu |
> | Action | Microsoft.ApiManagement/service/subscriptions/regenerateSecondaryKey/action | Generiert den Sekundärschlüssel eines vorhandenen Abonnements der API Management-Dienstinstanz neu |
> | Action | Microsoft.ApiManagement/service/subscriptions/write | Erstellt oder aktualisiert das Abonnement des angegebenen Benutzers für das angegebene Produkt oder aktualisiert Details eines durch seinen Bezeichner angegebenen Abonnements |
> | Action | Microsoft.ApiManagement/service/tagResources/read | Listet eine Auflistung von Ressourcen auf, denen Tags zugeordnet sind |
> | Action | Microsoft.ApiManagement/service/tags/delete | Löscht das angegebene Tag aus der API Management-Dienstinstanz |
> | Action | Microsoft.ApiManagement/service/tags/read | Listet eine Auflistung der in einer Dienstinstanz definierten Tags auf oder ruft Details des durch seinen Bezeichner angegebenen Tags ab |
> | Action | Microsoft.ApiManagement/service/tags/write | Erstellt ein Tag oder aktualisiert Details des durch seinen Bezeichner angegebenen Tags |
> | Action | Microsoft.ApiManagement/service/templates/delete | Setzt die standardmäßige E-Mail-Vorlage des API Management zurück. |
> | Action | Microsoft.ApiManagement/service/templates/read | Ruft alle E-Mail-Vorlagen des API Management oder die entsprechenden Details ab. |
> | Action | Microsoft.ApiManagement/service/templates/write | Erstellt oder aktualisiert die E-Mail-Vorlage des API Management. |
> | Action | Microsoft.ApiManagement/service/tenant/delete | Dient zum Entfernen der Richtlinienkonfiguration für den Mandanten. |
> | Action | Microsoft.ApiManagement/service/tenant/deploy/action | Führt eine Bereitstellungsaufgabe aus, um Änderungen aus der angegebenen Git-Verzweigung auf die Konfiguration in der Datenbank anzuwenden. |
> | Action | Microsoft.ApiManagement/service/tenant/listSecrets/action | Dient zum Abrufen der Zugriffsinformationsdetails für den Mandanten. |
> | Action | Microsoft.ApiManagement/service/tenant/operationResults/read | Dient zum Abrufen einer Liste mit Vorgangsergebnissen oder zum Abrufen des Ergebnisses eines bestimmten Vorgangs. |
> | Action | Microsoft.ApiManagement/service/tenant/read | Ruft die globale Richtlinienkonfiguration des API Management-Diensts ab oder ruft Zugriffsinformationsdetails für den Mandanten ab |
> | Action | Microsoft.ApiManagement/service/tenant/regeneratePrimaryKey/action | Dient zum erneuten Generieren des primären Zugriffsschlüssels. |
> | Action | Microsoft.ApiManagement/service/tenant/regenerateSecondaryKey/action | Dient zum erneuten Generieren des sekundären Zugriffsschlüssels. |
> | Action | Microsoft.ApiManagement/service/tenant/save/action | Erstellt Commit mit Konfigurationsmomentaufnahme in der angegebenen Verzweigung im Repository. |
> | Action | Microsoft.ApiManagement/service/tenant/syncState/read | Dient zum Abrufen des Status der letzten Git-Synchronisierung. |
> | Action | Microsoft.ApiManagement/service/tenant/validate/action | Überprüft Änderungen aus der angegebenen Git-Verzweigung. |
> | Action | Microsoft.ApiManagement/service/tenant/write | Legt die Richtlinienkonfiguration für den Mandanten ab oder aktualisiert die Zugriffsdetails des Mandanten. |
> | Action | Microsoft.ApiManagement/service/updatecertificate/action | Dient zum Hochladen eines SSL-Zertifikats für einen API Management-Dienst. |
> | Action | Microsoft.ApiManagement/service/updatehostname/action | Dient zum Einrichten, Aktualisieren oder Entfernen benutzerdefinierter Domänennamen für einen API Management-Dienst. |
> | Action | Microsoft.ApiManagement/service/users/action | Registriert einen neuen Benutzer. |
> | Action | Microsoft.ApiManagement/service/users/confirmations/send/action | Sendet eine Bestätigung. |
> | Action | Microsoft.ApiManagement/service/users/delete | Löscht den angegebenen Benutzer |
> | Action | Microsoft.ApiManagement/service/users/generateSsoUrl/action | Ruft eine Umleitungs-URL mit einem Authentifizierungstoken zum Anmelden des angegebenen Benutzers beim Entwicklerportal ab |
> | Action | Microsoft.ApiManagement/service/users/groups/read | Listet alle Benutzergruppen auf |
> | Action | Microsoft.ApiManagement/service/users/identities/read | Listet alle Benutzeridentitäten auf |
> | Action | Microsoft.ApiManagement/service/users/keys/read | Abrufen von Benutzern zugeordneten Schlüsseln |
> | Action | Microsoft.ApiManagement/service/users/read | Listet eine Auflistung der in der angegebenen Dienstinstanz registrierten Benutzer auf oder ruft Details des durch seinen Bezeichner angegebenen Benutzers ab |
> | Action | Microsoft.ApiManagement/service/users/subscriptions/read | Listet die Auflistung der Abonnements für den angegebenen Benutzer auf |
> | Action | Microsoft.ApiManagement/service/users/token/action | Ruft das SAS-Autorisierungstoken für den Benutzer ab |
> | Action | Microsoft.ApiManagement/service/users/write | Erstellt oder aktualisiert einen Benutzer oder aktualisiert Details des durch seinen Bezeichner angegebenen Benutzers |
> | Action | Microsoft.ApiManagement/service/write | Dient zum Erstellen einer neuen Instanz des API Management-Diensts. |
> | Action | Microsoft.ApiManagement/unregister/action | Dient zum Aufheben der Registrierung des Microsoft.ApiManagement-Ressourcenanbieters. |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Authorization/classicAdministrators/delete | Entfernt den Administrator aus dem Abonnement. |
> | Action | Microsoft.Authorization/classicAdministrators/operationstatuses/read | Ruft die Administratorvorgangsstatus des Abonnements ab. |
> | Action | Microsoft.Authorization/classicAdministrators/read | Liest die Administratoren für das Abonnement. |
> | Action | Microsoft.Authorization/classicAdministrators/write | Dient zum Hinzufügen oder Ändern von Administratoren für ein Abonnement. |
> | Action | Microsoft.Authorization/denyAssignments/delete | Hiermit wird eine Ablehnungszuweisung mit dem angegebenen Bereich gelöscht. |
> | Action | Microsoft.Authorization/denyAssignments/read | Hiermit werden Informationen zu einer Ablehnungszuweisung abgerufen. |
> | Action | Microsoft.Authorization/denyAssignments/write | Hiermit wird eine Ablehnungszuweisung mit dem angegebenen Bereich erstellt. |
> | Action | Microsoft.Authorization/elevateAccess/action | Gewährt dem Aufrufer Zugriff vom Typ „Benutzerzugriffsadministrator“ auf der Mandantenebene. |
> | Action | Microsoft.Authorization/locks/delete | Dient zum Löschen von Sperren im angegebenen Bereich. |
> | Action | Microsoft.Authorization/locks/read | Ruft Sperren im angegebenen Bereich ab. |
> | Action | Microsoft.Authorization/locks/write | Dient zum Hinzufügen von Sperren im angegebenen Bereich. |
> | Action | Microsoft.Authorization/operations/read | Ruft die Liste mit den Vorgängen ab. |
> | Action | Microsoft.Authorization/permissions/read | Listet alle Berechtigungen auf, über die der Aufrufer in einem bestimmten Bereich verfügt. |
> | Action | Microsoft.Authorization/policies/audit/action | Als Ergebnis der Azure Policy-Auswertung ausgeführte Aktion mit „audit“-Effekt |
> | Action | Microsoft.Authorization/policies/auditIfNotExists/action | Als Ergebnis der Azure Policy-Auswertung ausgeführte Aktion mit „auditIfNotExists“-Effekt |
> | Action | Microsoft.Authorization/policies/deny/action | Als Ergebnis der Azure Policy-Auswertung ausgeführte Aktion mit „deny“-Effekt |
> | Action | Microsoft.Authorization/policies/deployIfNotExists/action | Als Ergebnis der Azure Policy-Auswertung ausgeführte Aktion mit „deployIfNotExists“-Effekt |
> | Action | Microsoft.Authorization/policyAssignments/delete | Dient zum Löschen einer Richtlinienzuweisung im angegebenen Bereich. |
> | Action | Microsoft.Authorization/policyAssignments/read | Dient zum Abrufen von Informationen zu einer Richtlinienzuweisung. |
> | Action | Microsoft.Authorization/policyAssignments/write | Dient zum Erstellen einer Richtlinienzuweisung im angegebenen Bereich. |
> | Action | Microsoft.Authorization/policyDefinitions/delete | Dient zum Löschen einer Richtliniendefinition. |
> | Action | Microsoft.Authorization/policyDefinitions/read | Dient zum Abrufen von Informationen zu einer Richtliniendefinition. |
> | Action | Microsoft.Authorization/policyDefinitions/write | Dient zum Erstellen einer benutzerdefinierten Richtliniendefinition. |
> | Action | Microsoft.Authorization/policySetDefinitions/delete | Löscht eine Richtliniengruppendefinition. |
> | Action | Microsoft.Authorization/policySetDefinitions/read | Ruft Informationen zu einer Richtliniengruppendefinition ab. |
> | Action | Microsoft.Authorization/policySetDefinitions/write | Erstellt eine benutzerdefinierte Richtliniengruppendefinition. |
> | Action | Microsoft.Authorization/providerOperations/read | Dient zum Abrufen von Vorgängen für alle Ressourcenanbieter, die in Rollendefinitionen verwendet werden können. |
> | Action | Microsoft.Authorization/roleAssignments/delete | Dient zum Löschen einer Rollenzuweisung im angegebenen Bereich. |
> | Action | Microsoft.Authorization/roleAssignments/read | Dient zum Abrufen von Informationen zu einer Rollenzuweisung. |
> | Action | Microsoft.Authorization/roleAssignments/write | Dient zum Erstellen einer Rollenzuweisung im angegebenen Bereich. |
> | Action | Microsoft.Authorization/roleDefinitions/delete | Dient zum Löschen der angegebenen benutzerdefinierten Rollendefinition. |
> | Action | Microsoft.Authorization/roleDefinitions/read | Dient zum Abrufen von Informationen zu einer Rollendefinition. |
> | Action | Microsoft.Authorization/roleDefinitions/write | Dient zum Erstellen oder Aktualisieren einer benutzerdefinierten Rollendefinition mit angegebenen Berechtigungen und zuweisbaren Bereichen. |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Automation/automationAccounts/agentRegistrationInformation/read | Liest die Registrierungsinformationen einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/agentRegistrationInformation/regenerateKey/action | Schreibt eine Anforderung zum erneuten Generieren von Azure Automation DSC-Schlüsseln. |
> | Action | Microsoft.Automation/automationAccounts/certificates/delete | Löscht ein Azure Automation-Zertifikatasset. |
> | Action | Microsoft.Automation/automationAccounts/certificates/getCount/action | Liest die Anzahl der Zertifikate. |
> | Action | Microsoft.Automation/automationAccounts/certificates/read | Ruft ein Azure Automation-Zertifikatasset ab. |
> | Action | Microsoft.Automation/automationAccounts/certificates/write | Erstellt oder aktualisiert ein Azure Automation-Zertifikatasset. |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/read | Liest die Kompilierung einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/read | Liest die Kompilierung einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/write | Schreibt die Kompilierung einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/write | Schreibt die Kompilierung einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/configurations/content/read | Liest den Inhalt der Konfigurationsmedien. |
> | Action | Microsoft.Automation/automationAccounts/configurations/delete | Löscht den Inhalt einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/configurations/getCount/action | Liest den Inhalt einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/configurations/read | Ruft den Inhalt einer Azure Automation DSC-Instanz ab. |
> | Action | Microsoft.Automation/automationAccounts/configurations/write | Schreibt den Inhalt einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/connections/delete | Löscht ein Azure Automation-Verbindungsasset. |
> | Action | Microsoft.Automation/automationAccounts/connections/getCount/action | Liest die Anzahl der Verbindungen. |
> | Action | Microsoft.Automation/automationAccounts/connections/read | Ruft ein Azure Automation-Verbindungsasset ab. |
> | Action | Microsoft.Automation/automationAccounts/connections/write | Erstellt oder aktualisiert ein Azure Automation-Verbindungsasset. |
> | Action | Microsoft.Automation/automationAccounts/connectionTypes/delete | Löscht ein Azure Automation-Verbindungstypasset. |
> | Action | Microsoft.Automation/automationAccounts/connectionTypes/read | Ruft ein Azure Automation-Verbindungstypasset ab. |
> | Action | Microsoft.Automation/automationAccounts/connectionTypes/write | Erstellt ein Azure Automation-Verbindungstypasset. |
> | Action | Microsoft.Automation/automationAccounts/credentials/delete | Löscht ein Azure Automation-Anmeldeinformationsasset. |
> | Action | Microsoft.Automation/automationAccounts/credentials/getCount/action | Liest die Anzahl der Anmeldeinformationen. |
> | Action | Microsoft.Automation/automationAccounts/credentials/read | Ruft ein Azure Automation-Anmeldeinformationsasset ab. |
> | Action | Microsoft.Automation/automationAccounts/credentials/write | Erstellt oder aktualisiert ein Azure Automation-Anmeldeinformationsasset. |
> | Action | Microsoft.Automation/automationAccounts/delete | Löscht ein Azure Automation-Konto. |
> | Action | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/delete | Löscht Hybrid Runbook Worker-Ressourcen. |
> | Action | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Liest Hybrid Runbook Worker-Ressourcen. |
> | Action | Microsoft.Automation/automationAccounts/jobs/output/read | Ruft die Ausgabe eines Auftrags ab. |
> | Action | Microsoft.Automation/automationAccounts/jobs/read | Ruft einen Azure Automation-Auftrag ab. |
> | Action | Microsoft.Automation/automationAccounts/jobs/resume/action | Setzt einen Azure Automation-Auftrag fort. |
> | Action | Microsoft.Automation/automationAccounts/jobs/runbookContent/action | Ruft den Inhalt des Azure Automation-Runbooks zum Zeitpunkt der Auftragsausführung ab. |
> | Action | Microsoft.Automation/automationAccounts/jobs/stop/action | Beendet einen Azure Automation-Auftrag. |
> | Action | Microsoft.Automation/automationAccounts/jobs/streams/read | Ruft einen Azure Automation-Auftragsdatenstrom ab. |
> | Action | Microsoft.Automation/automationAccounts/jobs/streams/read | Ruft einen Azure Automation-Auftragsdatenstrom ab. |
> | Action | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hält einen Azure Automation-Auftrag an. |
> | Action | Microsoft.Automation/automationAccounts/jobs/write | Erstellt einen Azure Automation-Auftrag. |
> | Action | Microsoft.Automation/automationAccounts/jobSchedules/delete | Löscht einen Azure Automation-Auftragszeitplan. |
> | Action | Microsoft.Automation/automationAccounts/jobSchedules/read | Ruft einen Azure Automation-Auftragszeitplan ab. |
> | Action | Microsoft.Automation/automationAccounts/jobSchedules/write | Erstellt einen Azure Automation-Auftragszeitplan. |
> | Action | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Ruft den Arbeitsbereich ab, der mit dem Automation-Konto verknüpft ist. |
> | Action | Microsoft.Automation/automationAccounts/listKeys/action | Liest die Schlüssel für das Automation-Konto. |
> | Action | Microsoft.Automation/automationAccounts/modules/activities/read | Ruft Azure Automation-Aktivitäten ab. |
> | Action | Microsoft.Automation/automationAccounts/modules/delete | Löscht ein Powershell-Modul in Azure Automation. |
> | Action | Microsoft.Automation/automationAccounts/modules/getCount/action | Ruft die Anzahl der Powershell-Module innerhalb des Automation-Kontos ab. |
> | Action | Microsoft.Automation/automationAccounts/modules/read | Ruft ein Powershell-Modul in Azure Automation ab. |
> | Action | Microsoft.Automation/automationAccounts/modules/write | Erstellt oder aktualisiert ein Powershell-Modul in Azure Automation. |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/delete | Löscht die Knotenkonfiguration einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/rawContent/action | Liest den Inhalt der Knotenkonfiguration einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/read | Liest die Knotenkonfiguration einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/write | Schreibt die Knotenkonfiguration einer Azure Automation DSC-Instanz. |
> | Action | Microsoft.Automation/automationAccounts/nodecounts/read | Liest die zusammengefasste Anzahl der Knoten für den angegebenen Typ. |
> | Action | Microsoft.Automation/automationAccounts/nodes/delete | Löscht Azure Automation DSC-Knoten. |
> | Action | Microsoft.Automation/automationAccounts/nodes/read | Liest Azure Automation DSC-Knoten. |
> | Action | Microsoft.Automation/automationAccounts/nodes/reports/content/read | Liest Azure Automation DSC-Berichtsinhalte. |
> | Action | Microsoft.Automation/automationAccounts/nodes/reports/read | Liest Azure Automation DSC-Berichte. |
> | Action | Microsoft.Automation/automationAccounts/nodes/write | Erstellt oder aktualisiert Azure Automation DSC-Knoten. |
> | Action | Microsoft.Automation/automationAccounts/objectDataTypes/fields/read | Ruft TypeFields-Elemente für Azure Automation ab. |
> | Action | Microsoft.Automation/automationAccounts/python2Packages/delete | Löscht ein Azure Automation Python 2-Paket. |
> | Action | Microsoft.Automation/automationAccounts/python2Packages/read | Ruft ein Azure Automation Python 2-Paket ab. |
> | Action | Microsoft.Automation/automationAccounts/python2Packages/write | Erstellt oder aktualisiert ein Azure Automation Python 2-Paket. |
> | Action | Microsoft.Automation/automationAccounts/python3Packages/delete | Löscht ein Azure Automation Python 3-Paket. |
> | Action | Microsoft.Automation/automationAccounts/python3Packages/read | Ruft ein Azure Automation Python 3-Paket ab. |
> | Action | Microsoft.Automation/automationAccounts/python3Packages/write | Erstellt oder aktualisiert ein Azure Automation Python 3-Paket. |
> | Action | Microsoft.Automation/automationAccounts/read | Ruft ein Azure Automation-Konto ab. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/content/read | Ruft den Inhalt eines Azure Automation-Runbooks ab. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/delete | Löscht ein Azure Automation-Runbook. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/content/write | Erstellt den Inhalt eines Azure Automation-Runbookentwurfs. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/operationResults/read | Ruft die Vorgangsergebnisse des Azure Automation-Runbookentwurfs ab. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/read | Ruft einen Azure Automation-Runbookentwurf ab. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/read | Ruft einen Testauftrag für einen Azure Automation-Runbookentwurf ab. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/resume/action | Setzt einen Testauftrag für einen Azure Automation-Runbookentwurf fort. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/stop/action | Beendet einen Testauftrag für einen Azure Automation-Runbookentwurf. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/suspend/action | Hält einen Testauftrag für einen Azure Automation-Runbookentwurf an. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/write | Erstellt einen Testauftrag für einen Azure Automation-Runbookentwurf. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/undoEdit/action | Macht die Bearbeitung eines Azure Automation-Runbookentwurfs rückgängig. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/getCount/action | Ruft die Anzahl der Azure Automation-Runbooks ab. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/publish/action | Veröffentlicht einen Azure Automation-Runbookentwurf. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/read | Ruft ein Azure Automation-Runbook ab. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/write | Erstellt oder aktualisiert ein Azure Automation-Runbook. |
> | Action | Microsoft.Automation/automationAccounts/schedules/delete | Löscht ein Azure Automation-Zeitplanasset. |
> | Action | Microsoft.Automation/automationAccounts/schedules/getCount/action | Ruft die Anzahl der Azure Automation-Zeitpläne ab. |
> | Action | Microsoft.Automation/automationAccounts/schedules/read | Ruft ein Azure Automation-Zeitplanasset ab. |
> | Action | Microsoft.Automation/automationAccounts/schedules/write | Erstellt oder aktualisiert ein Azure Automation-Zeitplanasset. |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurationMachineRuns/read | Ruft eine Konfigurationscomputerausführung zur Aktualisierung der Azure Automation-Software ab. |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurationRuns/read | Ruft eine Konfigurationsausführung zur Aktualisierung der Azure Automation-Software ab. |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/delete | Löscht die Konfiguration zur Aktualisierung der Azure Automation-Software. |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/delete | Löscht die Konfiguration zur Aktualisierung der Azure Automation-Software. |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/read | Ruft die Konfiguration zur Aktualisierung der Azure Automation-Software ab. |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/read | Ruft die Konfiguration zur Aktualisierung der Azure Automation-Software ab. |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/write | Erstellt oder aktualisiert die Konfiguration zur Aktualisierung der Azure Automation-Software. |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/write | Erstellt oder aktualisiert die Konfiguration zur Aktualisierung der Azure Automation-Software. |
> | Action | Microsoft.Automation/automationAccounts/statistics/read | Ruft die Statistik für Azure Automation ab. |
> | Action | Microsoft.Automation/automationAccounts/updateDeploymentMachineRuns/read | Ruft einen Bereitstellungscomputer für Azure Automation-Updates ab. |
> | Action | Microsoft.Automation/automationAccounts/updateManagementPatchJob/read | Ruft einen Patchauftrag für die Azure Automation-Updateverwaltung ab. |
> | Action | Microsoft.Automation/automationAccounts/usages/read | Ruft die Azure Automation-Nutzung ab. |
> | Action | Microsoft.Automation/automationAccounts/variables/delete | Löscht ein Azure Automation-Variablenasset. |
> | Action | Microsoft.Automation/automationAccounts/variables/read | Liest ein Azure Automation-Variablenasset. |
> | Action | Microsoft.Automation/automationAccounts/variables/write | Erstellt oder aktualisiert ein Azure Automation-Variablenasset. |
> | Action | Microsoft.Automation/automationAccounts/watchers/delete | Löscht einen Azure Automation Watcher-Auftrag. |
> | Action | Microsoft.Automation/automationAccounts/watchers/read | Ruft einen Azure Automation Watcher-Auftrag ab. |
> | Action | Microsoft.Automation/automationAccounts/watchers/start/action | Startet einen Azure Automation Watcher-Auftrag. |
> | Action | Microsoft.Automation/automationAccounts/watchers/stop/action | Beendet einen Azure Automation Watcher-Auftrag. |
> | Action | Microsoft.Automation/automationAccounts/watchers/streams/read | Ruft einen Azure Automation Watcher-Auftragsdatenstrom ab. |
> | Action | Microsoft.Automation/automationAccounts/watchers/watcherActions/delete | Löscht Aktionen eines Azure Automation Watcher-Auftrags. |
> | Action | Microsoft.Automation/automationAccounts/watchers/watcherActions/read | Ruft Aktionen eines Azure Automation Watcher-Auftrags ab. |
> | Action | Microsoft.Automation/automationAccounts/watchers/watcherActions/write | Erstellt Aktionen eines Azure Automation Watcher-Auftrags. |
> | Action | Microsoft.Automation/automationAccounts/watchers/write | Erstellt einen Azure Automation Watcher-Auftrag. |
> | Action | Microsoft.Automation/automationAccounts/webhooks/action | Generiert einen URI für einen Azure Automation-Webhook. |
> | Action | Microsoft.Automation/automationAccounts/webhooks/delete | Löscht einen Azure Automation-Webhook.  |
> | Action | Microsoft.Automation/automationAccounts/webhooks/read | Liest einen Azure Automation-Webhook. |
> | Action | Microsoft.Automation/automationAccounts/webhooks/write | Erstellt oder aktualisiert einen Azure Automation-Webhook. |
> | Action | Microsoft.Automation/automationAccounts/write | Erstellt oder aktualisiert ein Azure Automation-Konto. |
> | Action | Microsoft.Automation/operations/read | Ruft verfügbare Vorgänge für Azure Automation-Ressourcen ab. |
> | Action | Microsoft.Automation/register/action | Registriert das Abonnement für Azure Automation. |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.AzureActiveDirectory/b2cDirectories/delete | Löscht eine B2C-Verzeichnisressource. |
> | Action | Microsoft.AzureActiveDirectory/b2cDirectories/read | Zeigt eine B2C-Verzeichnisressource an. |
> | Action | Microsoft.AzureActiveDirectory/b2cDirectories/write | B2C-Verzeichnisressource erstellen oder aktualisieren |
> | Action | Microsoft.AzureActiveDirectory/b2ctenants/read | Listet alle B2C-Mandanten auf, in denen der Benutzer Mitglied ist. |
> | Action | Microsoft.AzureActiveDirectory/operations/read | Hiermit lesen Sie alle API-Vorgänge, die für den Microsoft.AzureActiveDirectory-Ressourcenanbieter verfügbar sind. |
> | Action | Microsoft.AzureActiveDirectory/register/action | Hiermit registrieren Sie das Abonnement für den Microsoft.AzureActiveDirectory-Ressourcenanbieter. |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.AzureStack/Operations/read | Ruft die Eigenschaften für einen Vorgang eines Ressourcenanbieters ab. |
> | Action | Microsoft.AzureStack/register/action | Registriert ein Abonnement beim Microsoft.AzureStack-Ressourcenanbieter. |
> | Action | Microsoft.AzureStack/registrations/customerSubscriptions/delete | Löscht ein Azure Stack-Kundenabonnement. |
> | Action | Microsoft.AzureStack/registrations/customerSubscriptions/read | Ruft die Eigenschaften eines Azure Stack-Kundenabonnements ab. |
> | Action | Microsoft.AzureStack/registrations/customerSubscriptions/write | Erstellt oder aktualisiert ein Azure Stack-Kundenabonnement. |
> | Action | Microsoft.AzureStack/registrations/delete | Löscht eine Azure Stack-Registrierung. |
> | Action | Microsoft.AzureStack/registrations/getActivationKey/action | Ruft den neuesten Azure Stack-Aktivierungsschlüssel ab. |
> | Action | Microsoft.AzureStack/registrations/products/getProduct/action | Ruft das Azure Stack-Marketplace-Produkt ab. |
> | Action | Microsoft.AzureStack/registrations/products/getProducts/action | Ruft eine Liste mit den Azure Stack-Marketplace-Produkten ab. |
> | Action | Microsoft.AzureStack/registrations/products/listDetails/action | Ruft erweiterte Details für ein Azure Stack-Marketplace-Produkt ab. |
> | Action | Microsoft.AzureStack/registrations/products/read | Ruft die Eigenschaften eines Azure Stack-Marketplace-Produkts ab. |
> | Action | Microsoft.AzureStack/registrations/products/uploadProductLog/action | Zeichnet den Azure Stack Marketplace-Produktbetriebsstatus und -Zeitstempel auf |
> | Action | Microsoft.AzureStack/registrations/read | Ruft die Eigenschaften einer Azure Stack-Registrierung ab. |
> | Action | Microsoft.AzureStack/registrations/write | Erstellt oder aktualisiert eine Azure Stack-Registrierung. |
> | Action | Microsoft.AzureStack/verificationKeys/getCurrentKey/action | Ruft die aktuelle Version des öffentlichen Signaturschlüssels von Azure Stack ab. |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Batch/batchAccounts/applications/delete | Löscht eine Anwendung. |
> | Action | Microsoft.Batch/batchAccounts/applications/read | Listet Anwendungen auf oder ruft die Eigenschaften einer Anwendung ab. |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/activate/action | Aktiviert ein Anwendungspaket. |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/delete | Löscht ein Anwendungspaket. |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/read | Ruft die Eigenschaften eines Anwendungspakets ab. |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/write | Erstellt ein neues Anwendungspaket oder aktualisiert ein vorhandenes Anwendungspaket. |
> | Action | Microsoft.Batch/batchAccounts/applications/write | Erstellt eine neue Anwendung oder aktualisiert eine vorhandene Anwendung. |
> | Action | Microsoft.Batch/batchAccounts/certificateOperationResults/read | Ruft die Ergebnisse eines lange andauernden Zertifikatvorgangs für ein Batch-Konto ab. |
> | Action | Microsoft.Batch/batchAccounts/certificates/cancelDelete/action | Bricht eine fehlerhafte Löschung eines Zertifikats für ein Batch-Konto ab. |
> | Action | Microsoft.Batch/batchAccounts/certificates/delete | Löscht ein Zertifikat aus einem Batch-Konto. |
> | Action | Microsoft.Batch/batchAccounts/certificates/read | Listet die Zertifikate für ein Batch-Konto auf oder ruft die Eigenschaften eines Zertifikats ab. |
> | Action | Microsoft.Batch/batchAccounts/certificates/write | Erstellt ein neues Zertifikat für ein Batch-Konto oder aktualisiert ein vorhandenes Zertifikat. |
> | Action | Microsoft.Batch/batchAccounts/delete | Löscht ein Batch-Konto. |
> | DataAction | Microsoft.Batch/batchAccounts/jobs/delete | Löscht einen Auftrag aus einem Batch-Konto. |
> | DataAction | Microsoft.Batch/batchAccounts/jobs/read | Listet Aufträge für ein Batch-Konto auf oder ruft die Eigenschaften eines Auftrags ab. |
> | DataAction | Microsoft.Batch/batchAccounts/jobs/write | Erstellt einen neuen Auftrag für ein Batch-Konto oder aktualisiert einen vorhandenen Auftrag. |
> | DataAction | Microsoft.Batch/batchAccounts/jobSchedules/delete | Löscht einen Auftragszeitplan aus einem Batch-Konto. |
> | DataAction | Microsoft.Batch/batchAccounts/jobSchedules/read | Listet Auftragszeitpläne für ein Batch-Konto auf oder ruft die Eigenschaften eines Auftragszeitplans ab. |
> | DataAction | Microsoft.Batch/batchAccounts/jobSchedules/write | Erstellt einen neuen Auftragszeitplan für ein Batch-Konto oder aktualisiert einen vorhandenen Auftragszeitplan. |
> | Action | Microsoft.Batch/batchAccounts/listkeys/action | Listet Zugriffsschlüssel für ein Batch-Konto auf. |
> | Action | Microsoft.Batch/batchAccounts/operationResults/read | Ruft die Ergebnisse eines lange andauernden Vorgangs im Batch-Konto ab. |
> | Action | Microsoft.Batch/batchAccounts/poolOperationResults/read | Ruft die Ergebnisse eines lange andauernden Poolvorgangs für ein Batch-Konto ab. |
> | Action | Microsoft.Batch/batchAccounts/pools/delete | Löscht einen Pool aus einem Batch-Konto. |
> | Action | Microsoft.Batch/batchAccounts/pools/disableAutoscale/action | Deaktiviert die automatische Skalierung für einen Pool des Batch-Kontos. |
> | Action | Microsoft.Batch/batchAccounts/pools/read | Listet Pools für ein Batch-Konto auf oder ruft die Eigenschaften eines Pools ab. |
> | Action | Microsoft.Batch/batchAccounts/pools/stopResize/action | Beendet einen aktiven Größenänderungsvorgang für einen Pool des Batch-Kontos. |
> | Action | Microsoft.Batch/batchAccounts/pools/write | Erstellt einen neuen Pool für ein Batch-Konto oder aktualisiert einen vorhandenen Pool. |
> | Action | Microsoft.Batch/batchAccounts/read | Listet Batch-Konten auf oder ruft die Eigenschaften eines Batch-Kontos ab. |
> | Action | Microsoft.Batch/batchAccounts/regeneratekeys/action | Generiert Zugriffsschlüssel für ein Batch-Konto neu. |
> | Action | Microsoft.Batch/batchAccounts/syncAutoStorageKeys/action | Synchronisiert Zugriffsschlüssel für das automatische Speicherkonto, das für ein Batch-Konto konfiguriert ist. |
> | Action | Microsoft.Batch/batchAccounts/write | Erstellt ein neues Batch-Konto oder aktualisiert ein vorhandenes Batch-Konto. |
> | Action | Microsoft.Batch/locations/accountOperationResults/read | Ruft die Ergebnisse eines lange andauernden Vorgangs im Batch-Konto ab. |
> | Action | Microsoft.Batch/locations/checkNameAvailability/action | Prüft, ob der Kontoname gültig ist und noch nicht verwendet wird. |
> | Action | Microsoft.Batch/locations/quotas/read | Ruft Batch-Kontingente des angegebenen Abonnements in der angegebenen Azure-Region ab. |
> | Action | Microsoft.Batch/operations/read | Listet die verfügbaren Vorgänge für den Microsoft.Batch-Ressourcenanbieter auf. |
> | Action | Microsoft.Batch/register/action | Registriert das Abonnement für den Batch-Ressourcenanbieter und ermöglicht die Erstellung von Batch-Konten. |
> | Action | Microsoft.Batch/unregister/action | Hebt die Registrierung für das Abonnement für den Batch-Ressourcenanbieter auf und verhindert so die Erstellung von Batch-Konten. |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Billing/billingAccounts/agreements/read |  |
> | Action | Microsoft.Billing/billingAccounts/billingPermissions/read |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/billingPermissions/read |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/customers/read |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/invoices/pricesheet/download/action |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections/billingPermissions/read |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections/billingSubscriptions/move/action |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections/billingSubscriptions/transfer/action |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections/billingSubscriptions/validateMoveEligibility/action |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections/products/move/action |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections/products/transfer/action |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections/products/validateMoveEligibility/action |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections/read |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections/write |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/pricesheet/download/action |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/read |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/write |  |
> | Action | Microsoft.Billing/billingAccounts/billingProfiles/write |  |
> | Action | Microsoft.Billing/billingAccounts/billingSubscriptions/read |  |
> | Action | Microsoft.Billing/billingAccounts/customers/billingPermissions/read |  |
> | Action | Microsoft.Billing/billingAccounts/customers/read |  |
> | Action | Microsoft.Billing/billingAccounts/departments/read |  |
> | Action | Microsoft.Billing/billingAccounts/enrollmentAccounts/billingPermissions/read |  |
> | Action | Microsoft.Billing/billingAccounts/enrollmentAccounts/read |  |
> | Action | Microsoft.Billing/billingAccounts/enrollmentDepartments/billingPermissions/read |  |
> | Action | Microsoft.Billing/billingAccounts/listInvoiceSectionsWithCreateSubscriptionPermission/action |  |
> | Action | Microsoft.Billing/billingAccounts/products/read |  |
> | Action | Microsoft.Billing/billingAccounts/read |  |
> | Action | Microsoft.Billing/billingAccounts/write |  |
> | Action | Microsoft.Billing/departments/read |  |
> | Action | Microsoft.Billing/invoices/download/action | Lädt die Rechnung über den Downloadlink aus der Liste herunter. |
> | Action | Microsoft.Billing/invoices/read |  |
> | Action | Microsoft.Billing/register/action |  |
> | Action | Microsoft.Billing/validateAddress/action |  |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.BingMaps/mapApis/Delete | Löschvorgang |
> | Action | Microsoft.BingMaps/mapApis/listSecrets/action | Dient zum Auflisten der Geheimnisse. |
> | Action | Microsoft.BingMaps/mapApis/listSingleSignOnToken/action | Dient zum Lesen des SSO-Autorisierungstokens für die Ressource. |
> | Action | Microsoft.BingMaps/mapApis/Read | Lesevorgang |
> | Action | Microsoft.BingMaps/mapApis/regenerateKey/action | Generiert den Schlüssel neu. |
> | Action | Microsoft.BingMaps/mapApis/Write | Schreibvorgang |
> | Action | Microsoft.BingMaps/Operations/read | Beschreibung des Vorgangs. |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Blockchain/blockchainMembers/delete | Löscht ein vorhandenes Blockchainmitglied |
> | Action | Microsoft.Blockchain/blockchainMembers/listApiKeys/action | Ruft die vorhandenen API-Schlüssel der Blockchainmitglieder ab oder listet sie auf. |
> | Action | Microsoft.Blockchain/blockchainMembers/read | Ruft die vorhandenen Blockchainmitglieder ab oder listet sie auf |
> | DataAction | Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action | Stellt eine Verbindung mit dem Transaktionsknoten eines Blockchainmitglieds her |
> | Action | Microsoft.Blockchain/blockchainMembers/transactionNodes/delete | Löscht einen vorhandenen Transaktionsknoten eines Blockchainmitglieds |
> | Action | Microsoft.Blockchain/blockchainMembers/transactionNodes/listApiKeys/action | Ruft die vorhandenen API-Schlüssel der Transaktionsknoten eines Blockchainmitglieds ab oder listet sie auf. |
> | Action | Microsoft.Blockchain/blockchainMembers/transactionNodes/read | Ruft die vorhandenen Transaktionsknoten eines Blockchainmitglieds ab oder listet sie auf |
> | Action | Microsoft.Blockchain/blockchainMembers/transactionNodes/write | Erstellt oder aktualisiert einen Transaktionsknoten eines Blockchainmitglieds |
> | Action | Microsoft.Blockchain/blockchainMembers/write | Erstellt oder aktualisiert ein Blockchainmitglied |
> | Action | Microsoft.Blockchain/cordaMembers/delete | Löscht ein vorhandenes Blockchain-Corda-Mitglied. |
> | Action | Microsoft.Blockchain/cordaMembers/read | Ruft die vorhandenen Blockchain-Corda-Mitglieder ab oder listet sie auf. |
> | Action | Microsoft.Blockchain/cordaMembers/write | Erstellt oder aktualisiert ein Blockchain-Corda-Mitglied. |
> | Action | Microsoft.Blockchain/locations/blockchainMemberOperationResults/read | Ruft die Vorgangsergebnisse von Blockchainmitgliedern ab |
> | Action | Microsoft.Blockchain/locations/checkNameAvailability/action | Prüft, ob der Ressourcenname gültig ist und noch nicht verwendet wird |
> | Action | Microsoft.Blockchain/operations/read | Listet alle Vorgänge im Microsoft Blockchain-Ressourcenanbieter auf |
> | Action | Microsoft.Blockchain/register/action | Registriert das Abonnement für den Blockchain-Ressourcenanbieter. |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Blueprint/blueprintAssignments/assignmentOperations/read | Liest alle Blaupausenartefakte. |
> | Action | Microsoft.Blueprint/blueprintAssignments/delete | Löscht alle Blaupausenartefakte. |
> | Action | Microsoft.Blueprint/blueprintAssignments/read | Liest alle Blaupausenartefakte. |
> | Action | Microsoft.Blueprint/blueprintAssignments/whoisblueprint/action | Ruft die Azure Blueprints-Dienstprinzipalobjekt-ID ab. |
> | Action | Microsoft.Blueprint/blueprintAssignments/write | Erstellt oder aktualisiert alle Blaupausenartefakte. |
> | Action | Microsoft.Blueprint/blueprints/artifacts/delete | Löscht alle Blaupausenartefakte. |
> | Action | Microsoft.Blueprint/blueprints/artifacts/read | Liest alle Blaupausenartefakte. |
> | Action | Microsoft.Blueprint/blueprints/artifacts/write | Erstellt oder aktualisiert alle Blaupausenartefakte. |
> | Action | Microsoft.Blueprint/blueprints/delete | Löscht alle Blaupausen. |
> | Action | Microsoft.Blueprint/blueprints/read | Liest alle Blaupausen. |
> | Action | Microsoft.Blueprint/blueprints/versions/artifacts/read | Liest alle Blaupausenartefakte. |
> | Action | Microsoft.Blueprint/blueprints/versions/delete | Löscht alle Blaupausen. |
> | Action | Microsoft.Blueprint/blueprints/versions/read | Liest alle Blaupausen. |
> | Action | Microsoft.Blueprint/blueprints/versions/write | Erstellt oder aktualisiert alle Blaupausen. |
> | Action | Microsoft.Blueprint/blueprints/write | Erstellt oder aktualisiert alle Blaupausen. |
> | Action | Microsoft.Blueprint/register/action | Registriert den Azure-Blaupausenressourcenanbieter. |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.BotService/botServices/channels/delete | Löscht einen Botdienst. |
> | Action | Microsoft.BotService/botServices/channels/read | Liest einen Botdienst. |
> | Action | Microsoft.BotService/botServices/channels/write | Schreibt einen Botdienst. |
> | Action | Microsoft.BotService/botServices/connections/delete | Löscht einen Botdienst. |
> | Action | Microsoft.BotService/botServices/connections/read | Liest einen Botdienst. |
> | Action | Microsoft.BotService/botServices/connections/write | Schreibt einen Botdienst. |
> | Action | Microsoft.BotService/botServices/delete | Löscht einen Botdienst. |
> | Action | Microsoft.BotService/botServices/read | Liest einen Botdienst. |
> | Action | Microsoft.BotService/botServices/write | Schreibt einen Botdienst. |
> | Action | Microsoft.BotService/locations/operationresults/read | Liest den Status eines asynchronen Vorgangs. |
> | Action | Microsoft.BotService/Operations/read | Liest die Vorgänge für alle Ressourcentypen. |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Cache/checknameavailability/action | Prüft, ob ein Name für die Verwendung mit einer neuen Redis Cache-Instanz verfügbar ist. |
> | Action | Microsoft.Cache/locations/operationresults/read | Ruft das Ergebnis eines lange andauernden Vorgangs ab, dessen Location-Header zuvor an den Client zurückgegeben wurde. |
> | Action | Microsoft.Cache/operations/read | Listet die Vorgänge auf, die der Microsoft.Cache-Anbieter unterstützt. |
> | Action | Microsoft.Cache/redis/delete | Dient zum Löschen der gesamten Redis Cache-Instanz. |
> | Action | Microsoft.Cache/redis/export/action | Dient dazu, Redis-Daten im angegebenen Format in Speicherblobs mit Präfix zu exportieren. |
> | Action | Microsoft.Cache/redis/firewallRules/delete | Dient zum Löschen der IP-Firewallregeln einer Redis Cache-Instanz. |
> | Action | Microsoft.Cache/redis/firewallRules/read | Dient zum Abrufen der IP-Firewallregeln einer Redis Cache-Instanz. |
> | Action | Microsoft.Cache/redis/firewallRules/write | Dient zum Bearbeiten der IP-Firewallregeln einer Redis Cache-Instanz. |
> | Action | Microsoft.Cache/redis/forceReboot/action | Dient zum Erzwingen des Neustarts einer Cache-Instanz (ggf. mit Datenverlust). |
> | Action | Microsoft.Cache/redis/import/action | Dient zum Importieren von Daten eines angegebenen Formats aus mehreren Blobs in Redis. |
> | Action | Microsoft.Cache/redis/linkedservers/delete | Dient zum Löschen von Verbindungsservern aus einer Redis Cache-Instanz. |
> | Action | Microsoft.Cache/redis/linkedservers/read | Dient zum Abrufen von Verbindungsservern, die einer Redis Cache-Instanz zugeordnet sind. |
> | Action | Microsoft.Cache/redis/linkedservers/write | Dient zum Hinzufügen von Verbindungsservern zu einer Redis Cache-Instanz. |
> | Action | Microsoft.Cache/redis/listKeys/action | Dient zum Anzeigen des Werts von Redis Cache-Zugriffsschlüsseln im Verwaltungsportal. |
> | Action | Microsoft.Cache/redis/listUpgradeNotifications/read | Dient zum Auflisten der neuesten Upgradebenachrichtigungen für den Cache-Mandanten. |
> | Action | Microsoft.Cache/redis/metricDefinitions/read | Ruft die verfügbaren Metriken für eine Redis Cache-Instanz ab. |
> | Action | Microsoft.Cache/redis/patchSchedules/delete | Dient zum Löschen des Patchzeitplans einer Redis Cache-Instanz. |
> | Action | Microsoft.Cache/redis/patchSchedules/read | Ruft den Patchzeitplan einer Redis Cache-Instanz ab. |
> | Action | Microsoft.Cache/redis/patchSchedules/write | Dient zum Ändern des Patchzeitplans einer Redis Cache-Instanz. |
> | Action | Microsoft.Cache/redis/read | Dient zum Anzeigen der Redis Cache-Einstellungen und -Konfiguration im Verwaltungsportal. |
> | Action | Microsoft.Cache/redis/regenerateKey/action | Dient zum Ändern des Werts von Redis Cache-Zugriffsschlüsseln im Verwaltungsportal. |
> | Action | Microsoft.Cache/redis/start/action | Dient zum Starten einer Cache-Instanz. |
> | Action | Microsoft.Cache/redis/stop/action | Dient zum Beenden einer Cache-Instanz. |
> | Action | Microsoft.Cache/redis/write | Dient zum Ändern der Redis Cache-Einstellungen und -Konfiguration im Verwaltungsportal. |
> | Action | Microsoft.Cache/register/action | Registriert den Ressourcenanbieter „Microsoft.Cache“ bei einem Abonnement. |
> | Action | Microsoft.Cache/unregister/action | Hebt die Registrierung des Ressourcenanbieters „Microsoft.Cache“ bei einem Abonnement auf. |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Capacity/appliedreservations/read | Liest alle Reservierungen. |
> | Action | Microsoft.Capacity/calculateexchange/action | Berechnet den Wechselbetrag und den Preis für den neuen Kauf und gibt Richtlinienfehler zurück |
> | Action | Microsoft.Capacity/calculateprice/action | Berechnet Reservierungspreise. |
> | Action | Microsoft.Capacity/catalogs/read | Liest den Katalog mit den Reservierungen. |
> | Action | Microsoft.Capacity/checkoffers/action | Überprüft Abonnementangebote. |
> | Action | Microsoft.Capacity/checkscopes/action | Überprüft Abonnements. |
> | Action | Microsoft.Capacity/commercialreservationorders/read | Ruft Reservierungsaufträge ab, die unter Mandanten erstellt wurden. |
> | Action | Microsoft.Capacity/exchange/action | Tauscht Reservierungen aus |
> | Action | Microsoft.Capacity/operations/read | Liest Vorgänge. |
> | Action | Microsoft.Capacity/register/action | Registriert den Kapazitätsressourcenanbieter und ermöglicht die Erstellung von Kapazitätsressourcen. |
> | Action | Microsoft.Capacity/reservationorders/action | Aktualisiert eine Reservierung. |
> | Action | Microsoft.Capacity/reservationorders/availablescopes/action | Sucht nach verfügbaren Bereichen. |
> | Action | Microsoft.Capacity/reservationorders/calculaterefund/action | Berechnet den Erstattungsbetrag und den Preis für den neuen Kauf und gibt Richtlinienfehler zurück. |
> | Action | Microsoft.Capacity/reservationorders/delete | Löscht eine Reservierung. |
> | Action | Microsoft.Capacity/reservationorders/merge/action | Führt Reservierungen zusammen. |
> | Action | Microsoft.Capacity/reservationorders/read | Liest alle Reservierungen. |
> | Action | Microsoft.Capacity/reservationorders/reservations/action | Aktualisiert eine Reservierung. |
> | Action | Microsoft.Capacity/reservationorders/reservations/availablescopes/action | Sucht nach verfügbaren Bereichen. |
> | Action | Microsoft.Capacity/reservationorders/reservations/delete | Löscht eine Reservierung. |
> | Action | Microsoft.Capacity/reservationorders/reservations/read | Liest alle Reservierungen. |
> | Action | Microsoft.Capacity/reservationorders/reservations/revisions/read | Liest alle Reservierungen. |
> | Action | Microsoft.Capacity/reservationorders/reservations/write | Erstellt eine Reservierung. |
> | Action | Microsoft.Capacity/reservationorders/return/action | Gibt Reservierungen zurück. |
> | Action | Microsoft.Capacity/reservationorders/split/action | Teilt Reservierungen. |
> | Action | Microsoft.Capacity/reservationorders/swap/action | Tauscht Reservierungen aus. |
> | Action | Microsoft.Capacity/reservationorders/write | Erstellt eine Reservierung. |
> | Action | Microsoft.Capacity/tenants/register/action | Registriert Mandanten. |
> | Action | Microsoft.Capacity/unregister/action | Hebt die Registrierung von Mandanten auf. |
> | Action | Microsoft.Capacity/validatereservationorder/action | Überprüft Reservierungen. |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Cdn/cdnwebapplicationfirewallmanagedrulesets/delete |  |
> | Action | Microsoft.Cdn/cdnwebapplicationfirewallmanagedrulesets/read |  |
> | Action | Microsoft.Cdn/cdnwebapplicationfirewallmanagedrulesets/write |  |
> | Action | Microsoft.Cdn/cdnwebapplicationfirewallpolicies/delete |  |
> | Action | Microsoft.Cdn/cdnwebapplicationfirewallpolicies/read |  |
> | Action | Microsoft.Cdn/cdnwebapplicationfirewallpolicies/write |  |
> | Action | Microsoft.Cdn/CheckNameAvailability/action |  |
> | Action | Microsoft.Cdn/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/edgenodes/delete |  |
> | Action | Microsoft.Cdn/edgenodes/read |  |
> | Action | Microsoft.Cdn/edgenodes/write |  |
> | Action | Microsoft.Cdn/operationresults/cdnwebapplicationfirewallpolicyresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/cdnwebapplicationfirewallpolicyresults/read |  |
> | Action | Microsoft.Cdn/operationresults/cdnwebapplicationfirewallpolicyresults/write |  |
> | Action | Microsoft.Cdn/operationresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/DisableCustomHttps/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/EnableCustomHttps/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/write |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Load/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/write |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Purge/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Start/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Stop/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/ValidateCustomDomain/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/write |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/GenerateSsoUri/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/GetSupportedOptimizationTypes/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/write |  |
> | Action | Microsoft.Cdn/operationresults/read |  |
> | Action | Microsoft.Cdn/operationresults/write |  |
> | Action | Microsoft.Cdn/operations/read |  |
> | Action | Microsoft.Cdn/profiles/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/profiles/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/DisableCustomHttps/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/EnableCustomHttps/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/read |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/write |  |
> | Action | Microsoft.Cdn/profiles/endpoints/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Load/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/origins/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/origins/read |  |
> | Action | Microsoft.Cdn/profiles/endpoints/origins/write |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Purge/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/read |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Start/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Stop/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/ValidateCustomDomain/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/write |  |
> | Action | Microsoft.Cdn/profiles/GenerateSsoUri/action |  |
> | Action | Microsoft.Cdn/profiles/GetSupportedOptimizationTypes/action |  |
> | Action | Microsoft.Cdn/profiles/read |  |
> | Action | Microsoft.Cdn/profiles/write |  |
> | Action | Microsoft.Cdn/register/action | Hiermit wird das Abonnement für den CDN-Ressourcenanbieter registriert und die Erstellung von CDN-Profilen aktiviert. |
> | Action | Microsoft.Cdn/ValidateProbe/action |  |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.CertificateRegistration/certificateOrders/certificates/Delete | Dient zum Löschen eines vorhandenen Zertifikats. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/certificates/Read | Dient zum Abrufen der Zertifikatliste. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/certificates/Write | Dient zum Hinzufügen eines neuen Zertifikats oder zum Aktualisieren eines bereits vorhandenen. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/Delete | Dient zum Löschen eines vorhandenen AppServiceCertificate-Elements. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/Read | Dient zum Abrufen der CertificateOrders-Liste. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/reissue/Action | Dient zum erneuten Ausstellen einer vorhandenen Zertifikatbestellung. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/renew/Action | Dient zum Verlängern einer vorhandenen Zertifikatbestellung. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/resendEmail/Action | Dient zum erneuten Senden von Zertifikat-E-Mails. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Dient zum erneuten Senden von Anforderungs-E-Mails an eine andere E-Mail-Adresse. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Dient zum Abrufen eines Websitesiegels für ein ausgestelltes App Service-Zertifikat. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | Dient zum Abrufen der Liste mit Zertifikataktionen. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | Dient zum Abrufen des Zertifikat-E-Mail-Verlaufs. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/verifyDomainOwnership/Action | Überprüfen des Domänenbesitzes |
> | Action | Microsoft.CertificateRegistration/certificateOrders/Write | Dient zum Hinzufügen eines neuen certificateOrder-Elements oder zum Aktualisieren eines bereits vorhandenen. |
> | Action | Microsoft.CertificateRegistration/operations/Read | Dient zum Auflisten aller Vorgänge über die App Service-Zertifikatregistrierung. |
> | Action | Microsoft.CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Dient zum Bereitstellen eines Dienstprinzipals für einen Dienst-App-Prinzipal. |
> | Action | Microsoft.CertificateRegistration/register/action | Dient zum Registrieren des Microsoft-Zertifikatressourcenanbieters für das Abonnement. |
> | Action | Microsoft.CertificateRegistration/validateCertificateRegistrationInformation/Action | Dient zum Überprüfen eines Zertifikaterwerbsobjekts, ohne es zu übermitteln. |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ClassicCompute/capabilities/read | Zeigt die Funktionen an. |
> | Action | Microsoft.ClassicCompute/checkDomainNameAvailability/action | Prüft die Verfügbarkeit eines angegebenen Domänennamens. |
> | Action | Microsoft.ClassicCompute/checkDomainNameAvailability/read | Überprüft die Verfügbarkeit eines angegebenen Domänennamens. |
> | Action | Microsoft.ClassicCompute/domainNames/active/write | Legt den aktiven Domänennamen fest. |
> | Action | Microsoft.ClassicCompute/domainNames/availabilitySets/read | Dient zum Anzeigen der Verfügbarkeitsgruppe für die Ressource. |
> | Action | Microsoft.ClassicCompute/domainNames/capabilities/read | Zeigt die Domänennamenfunktionen an. |
> | Action | Microsoft.ClassicCompute/domainNames/delete | Dient zum Entfernen der Domänennamen für Ressourcen. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/read | Zeigt die Bereitstellungsslots an. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/roles/read | Dient zum Abrufen der Rolle auf dem Bereitstellungsslot des Domänennamens. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/roles/roleinstances/read | Dient zum Abrufen der Rolleninstanz der Rolle auf dem Bereitstellungsslot des Domänennamens. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/state/read | Ruft den Zustand des Bereitstellungsslots ab. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/state/write | Fügt den Zustand des Bereitstellungsslots hinzu. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/upgradedomain/read | Dient zum Abrufen der Upgradedomäne für den Bereitstellungsslot auf dem Domänennamen. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/upgradedomain/write | Dient zum Aktualisieren der Upgradedomäne für den Bereitstellungsslot auf dem Domänennamen. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/write | Erstellt oder aktualisiert die Bereitstellung. |
> | Action | Microsoft.ClassicCompute/domainNames/extensions/delete | Dient zum Entfernen der Domänennamenerweiterungen. |
> | Action | Microsoft.ClassicCompute/domainNames/extensions/operationStatuses/read | Liest den Vorgangsstatus für die Domänennamenerweiterungen. |
> | Action | Microsoft.ClassicCompute/domainNames/extensions/read | Gibt die Domänennamenerweiterungen zurück. |
> | Action | Microsoft.ClassicCompute/domainNames/extensions/write | Dient zum Hinzufügen der Domänennamenerweiterungen. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/delete | Dient zum Entfernen eines neuen internen Lastenausgleichsmoduls. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/operationStatuses/read | Liest den Vorgangsstatus für die internen Lastenausgleichsmodule der Domänennamen. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/read | Ruft die internen Lastenausgleichsmodule ab. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/write | Erstellt ein neues internes Lastenausgleichsmodul. |
> | Action | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/operationStatuses/read | Liest den Vorgangsstatus für die Endpunktgruppen mit Lastenausgleich der Domänennamen. |
> | Action | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/read | Ruft Endpunktgruppen mit Lastenausgleich ab. |
> | Action | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/write | Fügt die Endpunktgruppe mit Lastenausgleich hinzu. |
> | Action | Microsoft.ClassicCompute/domainNames/operationstatuses/read | Ruft den Vorgangsstatus für den Domänennamen ab. |
> | Action | Microsoft.ClassicCompute/domainNames/operationStatuses/read | Liest den Vorgangsstatus für die Domänennamenerweiterungen. |
> | Action | Microsoft.ClassicCompute/domainNames/read | Dient zum Zurückgeben der Domänennamen für Ressourcen. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/delete | Dient zum Löschen der verwendeten Dienstzertifikate. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/operationStatuses/read | Liest den Vorgangsstatus für die Dienstzertifikate der Domänennamen. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/read | Gibt die verwendeten Dienstzertifikate zurück. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/write | Dient zum Hinzufügen oder Ändern der verwendeten Dienstzertifikate. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/abortMigration/action | Bricht die Migration eines Bereitstellungsslots ab. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/commitMigration/action | Führt für die Migration eines Bereitstellungsslots einen Commit durch. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/delete | Löscht einen angegebenen Bereitstellungsslot. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/operationStatuses/read | Liest den Vorgangsstatus für die Domänennamenslots. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/prepareMigration/action | Bereitet die Migration eines Bereitstellungsslots vor. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/read | Zeigt die Bereitstellungsslots an. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/delete | Dient zum Entfernen des Erweiterungsverweises für die Rolle „Bereitstellungsslot“. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/operationStatuses/read | Liest den Vorgangsstatus für die Slotrollen-Erweiterungsverweise der Domänennamen. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/read | Gibt den Erweiterungsverweis für die Rolle „Bereitstellungsslot“ zurück. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/write | Dient zum Hinzufügen oder Ändern des Erweiterungsverweises für die Rolle „Bereitstellungsslot“. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/metricdefinitions/read | Ruft die Rollenmetrikdefinition für den Domänennamen ab. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/metrics/read | Ruft die Rollenmetrik für den Domänennamen ab. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/operationstatuses/read | Ruft den Vorgangsstatus für die Rolle „Domänennamenslot“ ab. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen ab. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/read | Dient zum Abrufen der Rolle für den Bereitstellungsslot. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/downloadremotedesktopconnectionfile/action | Lädt eine Remotedesktop-Verbindungsdatei für die Rolleninstanz der Rolle „Domänennamenslot“ herunter. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/operationStatuses/read | Ruft den Vorgangsstatus für die Rolleninstanz der Rolle „Domänennamenslot“ ab. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/read | Dient zum Abrufen der Rolleninstanz. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/rebuild/action | Hiermit wird die Rolleninstanz neu erstellt. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/reimage/action | Führt ein Reimaging der Rolleninstanz aus. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/restart/action | Startet Rolleninstanzen neu. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/skus/read | Ruft die Rollen-SKU für den Bereitstellungsslot ab. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/write | Fügt die Rolle für den Bereitstellungsslot hinzu. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/start/action | Startet einen Bereitstellungsslot. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/state/start/write | Ändert den Zustand des Bereitstellungsslots in „Beendet“. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/state/stop/write | Ändert den Zustand des Bereitstellungsslots in „Gestartet“. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/stop/action | Hält den Bereitstellungsslot an. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/upgradeDomain/write | Dient zum Aufrüsten der Domäne. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/validateMigration/action | Überprüft die Migration eines Bereitstellungsslots. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/write | Erstellt oder aktualisiert die Bereitstellung. |
> | Action | Microsoft.ClassicCompute/domainNames/swap/action | Vertauscht Stagingslot und Produktionsslot. |
> | Action | Microsoft.ClassicCompute/domainNames/write | Dient zum Hinzufügen oder Ändern der Domänennamen für Ressourcen. |
> | Action | Microsoft.ClassicCompute/moveSubscriptionResources/action | Dient zum Verschieben aller klassischen Ressourcen in ein anderes Abonnement. |
> | Action | Microsoft.ClassicCompute/operatingSystemFamilies/read | Listet die in Microsoft Azure verfügbaren Gastbetriebssystemfamilien sowie die für jede Familie verfügbaren Betriebssystemversionen auf. |
> | Action | Microsoft.ClassicCompute/operatingSystems/read | Listet die Versionen des Gastbetriebssystems auf, die derzeit in Microsoft Azure verfügbar sind. |
> | Action | Microsoft.ClassicCompute/operations/read | Ruft die Liste der Vorgänge ab. |
> | Action | Microsoft.ClassicCompute/operationStatuses/read | Liest den Vorgangsstatus für die Ressource. |
> | Action | Microsoft.ClassicCompute/quotas/read | Dient zum Abrufen des Kontingents für das Abonnement. |
> | Action | Microsoft.ClassicCompute/register/action | Dient zum Registrieren für klassische Computeressourcen. |
> | Action | Microsoft.ClassicCompute/resourceTypes/skus/read | Ruft die SKU-Liste für unterstützte Ressourcentypen ab. |
> | Action | Microsoft.ClassicCompute/validateSubscriptionMoveAvailability/action | Dient zum Überprüfen der Verfügbarkeit des Abonnements für einen klassischen Verschiebevorgang. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/delete | Löscht die Netzwerksicherheitsgruppe, die dem virtuellen Computer zugeordnet ist. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read | Liest den Vorgangsstatus für die den virtuellen Computern zugeordneten Netzwerksicherheitsgruppen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/read | Ruft die Netzwerksicherheitsgruppe ab, die dem virtuellen Computer zugeordnet ist. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/write | Fügt eine Netzwerksicherheitsgruppe hinzu, die dem virtuellen Computer zugeordnet ist. |
> | Action | Microsoft.ClassicCompute/virtualMachines/asyncOperations/read | Ruft die möglichen asynchronen Vorgänge ab. |
> | Action | Microsoft.ClassicCompute/virtualMachines/attachDisk/action | Fügt einem virtuellen Computer einen Datenträger an. |
> | Action | Microsoft.ClassicCompute/virtualMachines/capture/action | Erfasst einen virtuellen Computer. |
> | Action | Microsoft.ClassicCompute/virtualMachines/delete | Entfernt virtuelle Computer. |
> | Action | Microsoft.ClassicCompute/virtualMachines/detachDisk/action | Trennt einen Datenträger von einem virtuellen Computer. |
> | Action | Microsoft.ClassicCompute/virtualMachines/diagnosticsettings/read | Ruft VM-Diagnoseeinstellungen ab. |
> | Action | Microsoft.ClassicCompute/virtualMachines/disks/read | Ruft die Liste mit den Datenträgern ab. |
> | Action | Microsoft.ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/action | Lädt die RDP-Datei für einen virtuellen Computer herunter. |
> | Action | Microsoft.ClassicCompute/virtualMachines/extensions/operationStatuses/read | Liest den Vorgangsstatus für die VM-Erweiterungen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/extensions/read | Ruft die VM-Erweiterung ab. |
> | Action | Microsoft.ClassicCompute/virtualMachines/extensions/write | Legt die VM-Erweiterung fest. |
> | Action | Microsoft.ClassicCompute/virtualMachines/metricdefinitions/read | Ruft die VM-Metrikdefinition ab. |
> | Action | Microsoft.ClassicCompute/virtualMachines/metrics/read | Ruft die Metriken ab. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete | Löscht die Netzwerksicherheitsgruppe, die der Netzwerkschnittstelle zugeordnet ist. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/operationStatuses/read | Liest den Vorgangsstatus für die den virtuellen Computern zugeordneten Netzwerksicherheitsgruppen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read | Ruft die Netzwerksicherheitsgruppe ab, die der Netzwerkschnittstelle zugeordnet ist. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write | Fügt eine Netzwerksicherheitsgruppe hinzu, die der Netzwerkschnittstelle zugeordnet ist. |
> | Action | Microsoft.ClassicCompute/virtualMachines/operationStatuses/read | Liest den Vorgangsstatus für die virtuellen Computer. |
> | Action | Microsoft.ClassicCompute/virtualMachines/performMaintenance/action | Hiermit wird eine Wartung für den virtuellen Computer durchgeführt. |
> | Action | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen ab. |
> | Action | Microsoft.ClassicCompute/virtualMachines/read | Ruft eine Liste mit virtuellen Computern ab. |
> | Action | Microsoft.ClassicCompute/virtualMachines/redeploy/action | Stellt den virtuellen Computer erneut bereit. |
> | Action | Microsoft.ClassicCompute/virtualMachines/restart/action | Startet virtuelle Computer neu. |
> | Action | Microsoft.ClassicCompute/virtualMachines/shutdown/action | Fährt den virtuellen Computer herunter. |
> | Action | Microsoft.ClassicCompute/virtualMachines/start/action | Starten Sie den virtuellen Computer. |
> | Action | Microsoft.ClassicCompute/virtualMachines/stop/action | Beendet den virtuellen Computer. |
> | Action | Microsoft.ClassicCompute/virtualMachines/write | Dient zum Hinzufügen oder Ändern virtueller Computer. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/operationstatuses/read | Hiermit wird der Vorgangsstatus für eine ExpressRoute-Querverbindung abgerufen. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/delete | Hiermit wird das Peering der ExpressRoute-Querverbindung gelöscht. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/delete | Hiermit wird der Vorgangsstatus für das Peering einer ExpressRoute-Querverbindung abgerufen. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/read | Hiermit wird das Peering einer ExpressRoute-Querverbindung abgerufen. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/write | Hiermit wird das Peering einer ExpressRoute-Querverbindung hinzugefügt. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/read | Hiermit werden ExpressRoute-Querverbindungen abgerufen. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/write | Hiermit werden ExpressRoute-Querverbindungen hinzugefügt. |
> | Action | Microsoft.ClassicNetwork/gatewaySupportedDevices/read | Ruft die Liste mit unterstützten Geräten ab. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/delete | Löscht die Netzwerksicherheitsgruppe. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/operationStatuses/read | Liest den Vorgangsstatus für die Netzwerksicherheitsgruppe. |
> | Action | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen für Netzwerksicherheitsgruppen ab. |
> | Action | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen für Netzwerksicherheitsgruppen. Dieser Vorgang wird durch den Insights-Ressourcenanbieter unterstützt. |
> | Action | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read | Ruft die Ereignisse für Netzwerksicherheitsgruppen ab. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/read | Ruft die Netzwerksicherheitsgruppe ab. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/delete | Löscht die Sicherheitsregel. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/operationStatuses/read | Liest den Vorgangsstatus für die Sicherheitsregeln der Netzwerksicherheitsgruppe. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/read | Ruft die Sicherheitsregel ab. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/write | Fügt eine Sicherheitsregel hinzu oder aktualisiert sie. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/write | Fügt eine neue Netzwerksicherheitsgruppe hinzu. |
> | Action | Microsoft.ClassicNetwork/operations/read | Hiermit werden klassische Netzwerkvorgänge abgerufen. |
> | Action | Microsoft.ClassicNetwork/quotas/read | Dient zum Abrufen des Kontingents für das Abonnement. |
> | Action | Microsoft.ClassicNetwork/register/action | Dient zum Registrieren für klassische Netzwerkressourcen. |
> | Action | Microsoft.ClassicNetwork/reservedIps/delete | Dient zum Löschen einer reservierten IP-Adresse. |
> | Action | Microsoft.ClassicNetwork/reservedIps/join/action | Dient zum Einbinden einer reservierten IP-Adresse. |
> | Action | Microsoft.ClassicNetwork/reservedIps/link/action | Dient zum Verknüpfen einer reservierten IP-Adresse. |
> | Action | Microsoft.ClassicNetwork/reservedIps/operationStatuses/read | Liest den Vorgangsstatus für die reservierten IP-Adressen. |
> | Action | Microsoft.ClassicNetwork/reservedIps/read | Ruft die reservierten IP-Adressen ab. |
> | Action | Microsoft.ClassicNetwork/reservedIps/write | Dient zum Hinzufügen einer neuen reservierten IP-Adresse. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/abortMigration/action | Bricht die Migration eines virtuellen Netzwerks ab. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/capabilities/read | Zeigt die Funktionen an. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/checkIPAddressAvailability/action | Prüft die Verfügbarkeit einer bestimmten IP-Adresse in einem virtuellen Netzwerk. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/commitMigration/action | Führt einen Commit für die Migration eines virtuellen Netzwerks aus. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/delete | Löscht das virtuelle Netzwerk. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/delete | Hebt die Sperrung eines Clientzertifikats auf. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/read | Dient zum Lesen der gesperrten Clientzertifikate. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/write | Sperrt ein Clientzertifikat. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/delete | Löscht das Clientzertifikat des virtuellen Netzwerkgateways. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/download/action | Lädt ein Zertifikat nach Fingerabdruck herunter. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/listPackage/action | Listet das Zertifikatpaket für das virtuelle Netzwerkgateway auf. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/read | Dient zum Suchen nach den Clientstammzertifikaten. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/write | Lädt ein neues Clientstammzertifikat hoch. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/connect/action | Stellt eine Site-to-Site-Gatewayverbindung her. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/disconnect/action | Trennt eine Site-to-Site-Gatewayverbindung. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/read | Ruft die Verbindungsliste ab. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/test/action | Testet eine Site-to-Site-Gatewayverbindung. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/delete | Löscht das virtuelle Netzwerkgateway. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDeviceConfigurationScript/action | Lädt das Gerätekonfigurationsskript herunter. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDiagnostics/action | Lädt die Gatewaydiagnose herunter. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/listCircuitServiceKey/action | Ruft den Schlüssel des Verbindungsdiensts ab. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/listPackage/action | Listet das Paket für das virtuelle Netzwerkgateway auf. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/operationStatuses/read | Liest den Vorgangsstatus für die virtuellen Netzwerkgateways. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/packages/read | Ruft das Paket für das virtuelle Netzwerkgateway ab. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/read | Ruft die virtuellen Netzwerkgateways ab. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/startDiagnostics/action | Startet die Diagnose für das Gateway für virtuelle Netzwerke. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/stopDiagnostics/action | Beendet die Diagnose für das Gateway für virtuelle Netzwerke. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/write | Fügt ein virtuelles Netzwerkgateway hinzu. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/join/action | Führt zum Beitritt zum virtuellen Netzwerk. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/operationStatuses/read | Liest den Vorgangsstatus für die virtuellen Netzwerke. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/peer/action | Richtet ein virtuelles Netzwerk als Peer für ein anderes virtuelles Netzwerk ein. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/prepareMigration/action | Bereitet die Migration eines virtuellen Netzwerks vor. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/read | Dient zum Abrufen des virtuellen Netzwerks. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete | Löscht den Proxy für das Peering virtueller Remotenetzwerke. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/read | Ruft den Proxy für das Peering virtueller Remotenetzwerke ab. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/write | Fügt den Proxy für das Peering virtueller Remotenetzwerke hinzu bzw. aktualisiert ihn. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete | Löscht die Netzwerksicherheitsgruppe, die dem Subnetz zugeordnet ist. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read | Liest den Vorgangsstatus für die dem Subnetz des virtuellen Netzwerks zugeordnete Netzwerksicherheitsgruppe. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/read | Ruft die Netzwerksicherheitsgruppe ab, die dem Subnetz zugeordnet ist. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/write | Fügt die Netzwerksicherheitsgruppe hinzu, die dem Subnetz zugeordnet ist. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/validateMigration/action | Überprüft die Migration eines virtuellen Netzwerks. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/virtualNetworkPeerings/read | Ruft das Peering virtueller Netzwerke ab. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/write | Dient zum Hinzufügen eines neuen virtuellen Netzwerks. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ClassicStorage/capabilities/read | Zeigt die Funktionen an. |
> | Action | Microsoft.ClassicStorage/checkStorageAccountAvailability/action | Prüft die Verfügbarkeit eines Speicherkontos. |
> | Action | Microsoft.ClassicStorage/checkStorageAccountAvailability/read | Hiermit wird die Verfügbarkeit eines Speicherkontos überprüft. |
> | Action | Microsoft.ClassicStorage/disks/read | Gibt den Speicherkontodatenträger zurück. |
> | Action | Microsoft.ClassicStorage/images/operationstatuses/read | Hiermit wird der Vorgangsstatus für ein Image abgerufen. |
> | Action | Microsoft.ClassicStorage/images/read | Gibt das Image zurück. |
> | Action | Microsoft.ClassicStorage/operations/read | Hiermit werden klassische Speichervorgänge abgerufen. |
> | Action | Microsoft.ClassicStorage/osImages/read | Gibt das Betriebssystemimage zurück. |
> | Action | Microsoft.ClassicStorage/osPlatformImages/read | Ruft das Image der Betriebssystemplattform ab. |
> | Action | Microsoft.ClassicStorage/publicImages/read | Ruft das öffentliche VM-Image ab. |
> | Action | Microsoft.ClassicStorage/quotas/read | Dient zum Abrufen des Kontingents für das Abonnement. |
> | Action | Microsoft.ClassicStorage/register/action | Dient zum Registrieren für klassischen Speicher. |
> | Action | Microsoft.ClassicStorage/storageAccounts/abortMigration/action | Bricht die Migration eines Speicherkontos ab. |
> | Action | Microsoft.ClassicStorage/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/blobServices/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen ab. |
> | Action | Microsoft.ClassicStorage/storageAccounts/commitMigration/action | Führt den Commit für die Migration eines Speicherkontos aus. |
> | Action | Microsoft.ClassicStorage/storageAccounts/delete | Löschen Sie das Speicherkonto. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/delete | Löscht den angegebenen Speicherkontodatenträger. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/operationStatuses/read | Liest den Vorgangsstatus für die Ressource. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/read | Gibt den Speicherkontodatenträger zurück. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/write | Fügt einen Speicherkontodatenträger hinzu. |
> | Action | Microsoft.ClassicStorage/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/fileServices/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen ab. |
> | Action | Microsoft.ClassicStorage/storageAccounts/images/delete | Löscht das angegebene Speicherkontoimage. (Veraltet. Verwenden Sie „Microsoft.ClassicStorage/storageAccounts/vmImages“) |
> | Action | Microsoft.ClassicStorage/storageAccounts/images/operationstatuses/read | Hiermit wird der Vorgangsstatus für das Speicherkontoimage zurückgegeben. |
> | Action | Microsoft.ClassicStorage/storageAccounts/images/read | Gibt das Speicherkontoimage zurück. (Veraltet. Verwenden Sie „Microsoft.ClassicStorage/storageAccounts/vmImages“) |
> | Action | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | Action | Microsoft.ClassicStorage/storageAccounts/operationStatuses/read | Liest den Vorgangsstatus für die Ressource. |
> | Action | Microsoft.ClassicStorage/storageAccounts/osImages/delete | Löscht das angegebene Betriebssystemimage für das Speicherkonto. |
> | Action | Microsoft.ClassicStorage/storageAccounts/osImages/read | Gibt das Betriebssystemimage für das Speicherkonto zurück. |
> | Action | Microsoft.ClassicStorage/storageAccounts/osImages/write | Fügt das Betriebssystemimage eines bestimmten Speicherkontos hinzu. |
> | Action | Microsoft.ClassicStorage/storageAccounts/prepareMigration/action | Bereitet die Migration eines Speicherkontos vor. |
> | Action | Microsoft.ClassicStorage/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen ab. |
> | Action | Microsoft.ClassicStorage/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/queueServices/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen ab. |
> | Action | Microsoft.ClassicStorage/storageAccounts/read | Dient zum Zurückgeben des Speicherkontos mit dem angegebenen Konto. |
> | Action | Microsoft.ClassicStorage/storageAccounts/regenerateKey/action | Generiert die vorhandenen Zugriffsschlüssel für das Speicherkonto neu. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/read | Dient zum Abrufen der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/metricDefinitions/read | Ruft die Metrikdefinitionen ab. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/metrics/read | Ruft die Metriken ab. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/read | Dient zum Abrufen der verfügbaren Dienste. |
> | Action | Microsoft.ClassicStorage/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/tableServices/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen ab. |
> | Action | Microsoft.ClassicStorage/storageAccounts/validateMigration/action | Überprüft die Migration eines Speicherkontos. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/delete | Löscht ein bestimmtes VM-Image. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/operationstatuses/read | Hiermit wird der Vorgangsstatus für ein angegebenes VM-Image abgerufen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/read | Gibt das VM-Image zurück. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/write | Fügt ein bestimmtes VM-Image hinzu. |
> | Action | Microsoft.ClassicStorage/storageAccounts/write | Fügt ein neues Speicherkonto hinzu. |
> | Action | Microsoft.ClassicStorage/vmImages/read | Listet die VM-Images auf. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | DataAction | Microsoft.CognitiveServices/accounts/AnomalyDetector/timeseries/entire/detect/action | Mit diesem Vorgang wird ein Modell generiert, indem eine gesamte Reihe verwendet wird. Jeder Punkt wird anhand desselben Modells erkannt.<br>Bei dieser Methode werden Punkte vor und nach einem bestimmten Punkt verwendet, um zu ermitteln, ob es sich um eine Anomalie handelt.<br>Anhand der gesamten Erkennung kann der Benutzer einen Gesamtstatus der Zeitreihe erhalten. |
> | DataAction | Microsoft.CognitiveServices/accounts/AnomalyDetector/timeseries/last/detect/action | Bei diesem Vorgang wird ein Modell generiert, indem die Punkte vor dem letzten Punkt verwendet werden. Hierbei werden nur zurückliegende Punkte genutzt, um zu ermitteln, ob der Zielpunkt eine Anomalie darstellt. Die Erkennung des letzten Punkts entspricht dem Szenario der Echtzeitüberwachung von Geschäftsmetriken. |
> | DataAction | Microsoft.CognitiveServices/accounts/Autosuggest/search/action | Mit diesem Vorgang werden Vorschläge für eine bestimmte Abfrage oder eine partielle Abfrage bereitgestellt. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/analyze/action | Durch diesen Vorgang wird ein umfangreicher Satz von Visualfeatures basierend auf dem Bildinhalt extrahiert.  |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/areaofinterest/action | Dieser Vorgang gibt einen Begrenzungsrahmen um den wichtigsten Bereich des Bilds zurück. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/describe/action | Durch diesen Vorgang wird eine Beschreibung eines Bilds mit vollständigen Sätzen in verständlicher Sprache generiert.<br> Die Beschreibung basiert auf einer Sammlung von Inhaltstags, die ebenfalls vom Vorgang zurückgegeben werden.<br>Für jedes Bild können mehrere Beschreibungen generiert werden.<br> Beschreibungen werden nach Zuverlässigkeitsbewertung sortiert.<br>Alle Beschreibungen werden in englischer Sprache verfasst. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/detect/action | Dieser Vorgang führt eine Objekterkennung für das angegebene Bild durch.  |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/generatethumbnail/action | Dieser Vorgang generiert eine Miniaturansicht mit der vom Benutzer angegebenen Breite und Höhe.<br> Standardmäßig analysiert der Dienst das Bild, identifiziert den Bereich, der von Interesse ist (Region of Interest, ROI), und generiert basierend auf dem ROI intelligente Koordinaten für den Zuschnitt.<br> Das intelligente Zuschneiden ist hilfreich, wenn Sie ein Seitenverhältnis angeben, das sich von dem des Eingabebilds unterscheidet. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/models/analyze/action | Dieser Vorgang erkennt Inhalte in einem Bild durch Anwenden eines domänenspezifischen Modells.<br> Die Liste domänenspezifischer Modelle, die von der Maschinelles Sehen-API unterstützt werden, kann über die /models-GET-Anforderung abgerufen werden.<br> Zurzeit bietet die API folgende domänenspezifische Modelle: celebrities (Prominente), landmarks (Sehenswürdigkeiten). |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/models/read | Dieser Vorgang gibt die Liste domänenspezifischer Modelle zurück, die von der Maschinelles Sehen-API unterstützt werden.  Zurzeit unterstützt die API folgende domänenspezifische Modelle: celebrity recognizer (Prominentenerkennung), landmark recognizer (Sehenswürdigkeitenerkennung). |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/ocr/action | Optische Zeichenerkennung (OCR) erkennt Text in einem Bild und extrahiert die erkannten Zeichen in eine vom Computer verwendbare Zeichenfolge.    |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/read/analyze/action | Verwenden Sie diese Schnittstelle, um einen Lesevorgang durchzuführen, bei dem die hochmodernen OCR-Algorithmen (Optical Character Recognition, Optische Zeichenerkennung) verwendet werden, die für textlastige Dokumente optimiert sind.<br>Hiermit können handgeschriebene, gedruckte oder gemischte Dokumente verarbeitet werden.<br>Wenn Sie die Leseschnittstelle verwenden, enthält die Antwort einen Header mit dem Namen „Operation-Location“.<br>Der Header „Operation-Location“ enthält die URL, die Sie für Ihren Vorgang zum Abrufen des Leseergebnisses verwenden müssen, um auf die OCR-Ergebnisse zuzugreifen. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/read/analyzeresults/read | Verwenden Sie diese Schnittstelle, um den Status und das OCR-Ergebnis eines Lesevorgangs abzurufen.  Die URL mit der „operationId“ wird im Antwortheader „Operation-Location“ des Lesevorgangs zurückgegeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/read/core/asyncbatchanalyze/action | Verwenden Sie diese Schnittstelle, um das Ergebnis eines Massenlesevorgangs für Dateien abzurufen, indem Sie die hochmoderne optische Zeichenerkennung nutzen. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/read/operations/read | Diese Schnittstelle wird verwendet, um die OCR-Ergebnisse eines Lesevorgangs abzurufen. Die URL für diese Schnittstelle muss aus dem Feld <b>„Operation-Location“</b> abgerufen werden, das von der Schnittstelle für Massenlesevorgänge für Dateien zurückgegeben wird. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/recognizetext/action | Verwenden Sie diese Schnittstelle, um das Ergebnis eines Texterkennungsvorgangs zu erhalten. Wenn Sie die Texterkennungsschnittstelle verwenden, enthält die Antwort ein Feld namens „Operation-Location“. Das Feld „Operation-Location“ enthält die URL, die Sie für Ihren Vorgang zum Abrufen des Ergebnisses der Texterkennung verwenden müssen. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/tag/action | Dieser Vorgang generiert eine Liste von Wörtern oder Tags, die für den Inhalt des angegebenen Bilds relevant sind.<br>Die Maschinelles Sehen-API kann Tags basierend auf Objekten, Lebewesen, Landschaften oder Aktionen zurückgegeben, die in Bildern ermittelt werden.<br>Im Gegensatz zu Kategorien werden Tags nicht anhand eines hierarchischen Klassifizierungssystems angeordnet, sondern entsprechen Bildinhalten.<br>Tags können Hinweise enthalten, um Mehrdeutigkeiten auszuschließen oder Kontext bereitzustellen. Das Tag „cello“ könnte beispielsweise mit dem Hinweis „musical instrument“ aufgeführt werden.<br>Alle Tags werden in englischer Sprache angegeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/textoperations/read | Diese Schnittstelle wird verwendet, um das Ergebnis des Vorgangs zur Texterkennung abzurufen. Die URL für diese Schnittstelle muss aus dem Feld <b>Operation-Location</b> abgerufen werden, das von der Schnittstelle zur Texterkennung zurückgegeben wird. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/imagelists/action | Dient zum Erstellen einer Bildliste. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/imagelists/delete | Bildlisten: Löschen |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/imagelists/images/delete | Dient zum Löschen eines Bilds aus Ihrer Bildliste. Die Bildliste kann verwendet werden, um nach Fuzzyübereinstimmungen mit anderen Bildern zu suchen, wenn eine Bild-/Übereinstimmungs-API genutzt wird. Dient zum Löschen aller Bilder aus Ihrer Liste. Die Bildliste kann verwendet werden, um nach Fuzzyübereinstimmungen mit anderen Bildern zu suchen, wenn eine Bild-/Übereinstimmungs-API genutzt wird.* |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/imagelists/images/read | Bild: Alle Bild-IDs abrufen |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/imagelists/images/write | Dient zum Hinzufügen eines Bilds zu Ihrer Bildliste. Die Bildliste kann verwendet werden, um nach Fuzzyübereinstimmungen mit anderen Bildern zu suchen, wenn eine Bild-/Übereinstimmungs-API genutzt wird. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/imagelists/read | Bildlisten: Details abrufen – Bildlisten: Alle abrufen |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/imagelists/refreshindex/action | Bildlisten: Suchindex aktualisieren |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/imagelists/write | Bildlisten: Details aktualisieren |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processimage/evaluate/action | Gibt Wahrscheinlichkeiten dazu zurück, ob das Bild freizügige oder nicht jugendfreie Inhalte aufweist. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processimage/findfaces/action | Dient zum Suchen nach Gesichtern in Bildern. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processimage/match/action | Dient zum Suchen nach Fuzzyübereinstimmungen mit Ihren benutzerdefinierten Bildlisten für ein Bild.<br>Mit dieser API können Sie Ihre benutzerdefinierten Bildlisten erstellen und verwalten.<br> |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processimage/ocr/action | Gibt alle im Bild gefundenen Texte für die angegebene Sprache zurück. Wenn in der Eingabe keine Sprache angegeben ist, wird für die Erkennung standardmäßig Englisch verwendet. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processtext/detectlanguage/action | Mit diesem Vorgang wird die Sprache der jeweiligen Eingabe erkannt.<br>Gibt den ISO 639-3-Code für die vorherrschende Sprache des übermittelten Texts zurück.<br>Es werden mehr als 110 Sprachen unterstützt. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processtext/screen/action | Bei diesem Vorgang werden Obszönitäten für mehr als 100 Sprachen erkannt und mit benutzerdefinierten und freigegebenen Blacklists verglichen. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/jobs/action | Eine Auftrags-ID wird für den auf diesem Endpunkt bereitgestellten Bildinhalt zurückgegeben.  |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/jobs/read | Dient zum Abrufen der Auftragsdetails für eine Auftrags-ID. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/accesskey/read | Dient zum Abrufen des Zugriffsschlüssels für die Inhaltsüberprüfung für Ihr Team. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/action | Die erstellten Überprüfungen werden für die Reviewer Ihres Teams angezeigt. Wenn Reviewer die Überprüfung durchführen, werden die Ergebnisse auf dem angegebenen CallBackEndpoint gepostet (HTTP POST). |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/frames/read | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/frames/write | Verwenden Sie diese Methode, um Frames für eine Videoüberprüfung hinzuzufügen. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/publish/action | Videoüberprüfungen werden ursprünglich in einem nicht veröffentlichten Zustand erstellt. Dies bedeutet, dass diese für die Reviewer Ihres Teams noch nicht verfügbar sind. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/read | Gibt Überprüfungsdetails für die übergebene Überprüfungs-ID zurück. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/transcript/action | Mit dieser API wird einer Videoüberprüfung eine Transkriptdatei (Textversion aller Wörter, die in einem Video gesprochen werden) hinzugefügt. Die Datei sollte hierbei ein gültiges WebVTT-Format aufweisen. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/transcriptmoderationresult/action | Mit dieser API wird die Ergebnisdatei eines Vorgangs zum Transkribieren von Fenstertext im Rahmen einer Videoüberprüfung hinzugefügt. Die Ergebnisdatei eines Vorgangs zum Transkribieren von Bildschirmtext ist ein Ergebnis der Fenstertext-API. Zum Generieren einer Ergebnisdatei eines Vorgangs zum Transkribieren von Fenstertext muss eine Transkriptdatei mit der Fenstertext-API auf Obszönitäten untersucht werden. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/settings/templates/delete | Löschen einer Vorlage für Ihr Team |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/settings/templates/read | Gibt ein Array mit Überprüfungsvorlagen zurück, die für dieses Team bereitgestellt wurden. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/settings/templates/write | Erstellt oder aktualisiert die angegebene Vorlage. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/workflows/read | Ruft die Details eines bestimmten Workflows Ihres Teams ab. Ruft alle für Ihr Team verfügbaren Workflows ab.* |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/workflows/write | Dient zum Erstellen eines neuen Workflows oder zum Aktualisieren eines bereits vorhandenen Workflows. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/action | Dient zum Erstellen einer Begriffsliste. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/bulkupdate/action | Begriffslisten: Massenaktualisierung |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/delete | Begriffslisten: Löschen |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/read | Begriffslisten: Alles abrufen – Begriffslisten: Details abrufen |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/refreshindex/action | Begriffslisten: Suchindex aktualisieren |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/terms/delete | Begriff: Löschen – Begriff: Alle Begriffe löschen |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/terms/read | Begriff: Alle Begriffe abrufen |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/terms/write | Begriff: Begriff hinzufügen |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/write | Begriffslisten: Details aktualisieren |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/classify/iterations/image/action | Klassifiziert ein Bild und speichert das Ergebnis. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/classify/iterations/image/nostore/action | Klassifiziert ein Bild, ohne das Ergebnis zu speichern. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/classify/iterations/url/action | Klassifiziert eine Bild-URL und speichert das Ergebnis. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/classify/iterations/url/nostore/action | Klassifiziert eine Bild-URL, ohne das Ergebnis zu speichern. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/detect/iterations/image/action | Erkennt Objekte in einem Bild und speichert das Ergebnis. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/detect/iterations/image/nostore/action | Erkennt Objekte in einem Bild, ohne das Ergebnis zu speichern. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/detect/iterations/url/action | Erkennt Objekte in einer Bild-URL und speichert das Ergebnis. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/detect/iterations/url/nostore/action | Erkennt Objekte in einer Bild-URL, ohne das Ergebnis zu speichern. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/domains/read | Ruft Informationen zu einer bestimmten Domäne ab. Ruft eine Liste mit den verfügbaren Domänen ab.* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/labelproposals/setting/action | Legt die Poolgröße für den Bezeichnungsvorschlag fest. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/labelproposals/setting/read | Ruft die Poolgröße des Bezeichnungsvorschlags für dieses Projekt ab. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/project/migrate/action | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/action | Erstellt ein Projekt. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/delete | Löscht ein bestimmtes Projekt. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/action | Bei dieser API wird Textinhalt der Form „multipart/form-data“ und „application/octet-stream“ akzeptiert. Bei Verwendung von mehrteiligen Daten |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/delete | Löscht Bilder aus einer Gruppe von Trainingsbildern. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/files/action | Diese API akzeptiert eine Gruppe von Dateien (und optional von Tags) für die Bilderstellung. Die Grenzwerte für die maximale Anzahl liegen bei 64 Bildern und 20 Tags. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/id/read | Diese API gibt eine Gruppe mit Bildern für die angegebenen Tags und optional eine Iteration zurück. Wenn keine Iteration angegeben wird, |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/predictions/action | Diese API erstellt aus den angegebenen vorhergesagten Bildern eine Gruppe mit Bildern. Die Grenzwerte für die maximale Anzahl liegen bei 64 Bildern und 20 Tags. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/regionproposals/action | Mit dieser API werden Regionsvorschläge für ein Bild sowie Zuverlässigkeitsbewertungen für die Region abgerufen. Wenn keine Vorschläge gefunden werden, wird ein leeres Array zurückgegeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/regions/action | Diese API akzeptiert eine Gruppe mit Bildregionen (und optional mit Tags), um vorhandene Bilder mit Regionsinformationen zu aktualisieren. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/regions/delete | Löscht eine Gruppe mit Bildregionen. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/suggested/action | Mit dieser API werden nicht per Tag markierte Bilder abgerufen, die nach den IDs der vorgeschlagenen Tags gefiltert sind. Wenn keine Bilder gefunden werden, wird ein leeres Array zurückgegeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/suggested/count/action | Bei dieser API werden tagIds verwendet, um die Anzahl von nicht per Tag markierten Bildern für vorgeschlagene Tags basierend auf einem bestimmten Schwellenwert abzurufen. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/tagged/count/read | Für die Filterung wird eine UND/ODER-Beziehung verwendet. Wenn die angegebenen Tag-IDs beispielsweise für „Dog“ gelten und |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/tagged/read | Diese API unterstützt die Batchverarbeitung und Bereichsauswahl. Standardmäßig werden nur die ersten 50 Bilder zurückgegeben, für die sich Übereinstimmungen ergeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/tags/action | Ordnet eine Gruppe mit Bildern einer Gruppe mit Tags zu. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/tags/delete | Entfernt eine Gruppe mit Tags aus einer Gruppe mit Bildern. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/untagged/count/read | Diese API gibt die Bilder zurück, die keine Tags für ein bestimmtes Projekt enthalten, und optional eine Iteration. Wenn keine Iteration angegeben wird, |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/untagged/read | Diese API unterstützt die Batchverarbeitung und Bereichsauswahl. Standardmäßig werden nur die ersten 50 Bilder zurückgegeben, für die sich Übereinstimmungen ergeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/images/urls/action | Diese API akzeptiert eine Gruppe mit URLs (und optional mit Tags) für die Bilderstellung. Die Grenzwerte für die maximale Anzahl liegen bei 64 Bildern und 20 Tags. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/delete | Löscht eine bestimmte Iteration eines Projekts. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/export/action | Exportiert eine trainierte Iteration. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/export/read | Ruft die Liste mit den Exporten für eine bestimmte Iteration ab. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/performance/images/count/read | Für die Filterung wird eine UND/ODER-Beziehung verwendet. Wenn die angegebenen Tag-IDs beispielsweise für „Dog“ gelten und |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/performance/images/read | Diese API unterstützt die Batchverarbeitung und Bereichsauswahl. Standardmäßig werden nur die ersten 50 Bilder zurückgegeben, für die sich Übereinstimmungen ergeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/performance/read | Ruft für eine Iteration ausführliche Informationen zur Leistung ab. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/publish/action | Veröffentlicht eine bestimmte Iteration. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/publish/delete | Hebt die Veröffentlichung einer bestimmten Iteration auf. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/read | Ruft eine bestimmte Iteration ab. Ruft Iterationen für das Projekt ab.* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/write | Aktualisiert eine bestimmte Iteration. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/predictions/delete | Löscht eine Gruppe mit vorhergesagten Bildern und die zugehörigen Vorhersageergebnisse. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/predictions/query/action | Ruft Bilder ab, die an Ihren Vorhersageendpunkt gesendet wurden. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/quicktest/image/action | Dient zum schnellen Testen eines Bilds. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/quicktest/url/action | Dient zum schnellen Testen einer Bild-URL. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/read | Ruft ein bestimmtes Projekt ab. Ruft Ihre Projekte ab.* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/tags/action | Erstellt ein Tag für das Projekt. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/tags/delete | Löscht ein Tag aus dem Projekt. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/tags/read | Ruft Informationen zu einem bestimmten Tag ab. Ruft die Tags für ein bestimmtes Projekt und eine Iteration ab.* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/tags/write | Aktualisiert ein Tag. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/tagsandregions/suggestions/action | Mit dieser API werden vorgeschlagene Tags und Regionen für ein Array bzw. einen Batch mit nicht getaggten Bildern sowie Zuverlässigkeitsbewertungen für die Tags abgerufen. Wenn keine Tags gefunden werden, wird ein leeres Array zurückgegeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/train/action | Reiht das Projekt für das Training in eine Warteschlange ein. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/write | Aktualisiert ein bestimmtes Projekt. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/quota/action | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/quota/delete | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/quota/refresh/write | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/usage/prediction/user/read | Ruft die Nutzungsdaten für die Vorhersageressource für Oxford-Benutzer ab. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/usage/training/resource/tier/read | Ruft die Nutzungsdaten für eine Trainingsressource für Azure-Benutzer ab. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/usage/training/user/read | Ruft die Nutzungsdaten für eine Trainingsressource für Oxford-Benutzer ab. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/user/action | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/user/delete | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/user/state/write | Aktualisiert einen Benutzerstatus. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/user/tier/write | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/users/read | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/whitelist/delete | Löscht einen in einer Whitelist enthaltenen Benutzer mit einer bestimmten Funktion. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/whitelist/read | Ruft eine Liste ab, die in einer Whitelist aufgeführte Benutzer mit einer bestimmten Funktion enthält. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/whitelist/write | Aktualisiert oder erstellt einen in der Whitelist enthaltenen Benutzer mit einer bestimmten Funktion. |
> | Action | Microsoft.CognitiveServices/accounts/delete | Löscht API-Konten. |
> | DataAction | Microsoft.CognitiveServices/accounts/EntitySearch/search/action | Abrufen von Entitäten und Bereitstellen von Ergebnissen für eine bestimmte Abfrage |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/detect/action | Hiermit werden menschliche Gesichter in einem Bild erkannt und Rechtecke für die Gesichter sowie optional faceIds, Sehenswürdigkeiten und Attribute zurückgegeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/delete | Hiermit löschen Sie eine angegebene Liste mit Gesichtern. Die zugehörigen Bilder der Gesichter in der Gesichterliste werden ebenfalls gelöscht. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/persistedfaces/delete | Hiermit löschen Sie ein Gesicht aus einer Liste mit Gesichtern anhand der angegebenen faceListId und persisitedFaceId. Auch das zugehörige Bild des Gesichts wird gelöscht. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/persistedfaces/write | Hiermit fügen Sie ein Gesicht einer angegebenen Liste mit Gesichtern hinzu. Bis zu 1.000 Gesichter können hinzugefügt werden. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/read | Hiermit rufen Sie die faceListId der Gesichterliste, den Namen, userData sowie Gesichter in der Gesichterliste ab. Hiermit listen Sie faceListId, Namen und userData der Gesichterliste auf. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/write | Hiermit erstellen Sie eine leere Gesichterliste mit benutzerdefinierten Angaben für faceListId, Name und optional userData. Bis zu 64 Gesichterlisten sind zulässig. Hiermit aktualisieren Sie die Informationen einer Gesichterliste, einschließlich Name und UserData. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/findsimilars/action | Hiermit können Sie anhand der faceId des abgefragten Gesichts ähnliche Gesichter aus einem faceId-Array, einer Gesichterliste oder einer umfangreichen Gesichterliste suchen. faceId |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/group/action | Hiermit unterteilen Sie die Gesichter der Kandidaten basierend auf der Gesichterähnlichkeit in Gruppen. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/identify/action | 1:n-Identifikationsvorgang zum Ermitteln der besten Ergebnisse für das Gesicht der abgefragten Person aus einer Personengruppe oder umfangreichen Personengruppe. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/delete | Hiermit löschen Sie eine angegebene umfangreiche Gesichterliste. Die zugehörigen Bilder der Gesichter in der umfangreichen Gesichterliste werden ebenfalls gelöscht. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/delete | Hiermit löschen Sie ein Gesicht aus einer umfangreichen Liste mit Gesichtern anhand der angegebenen largeFaceListId und persisitedFaceId. Auch das zugehörige Bild des Gesichts wird gelöscht. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/read | Hiermit rufen Sie das in einer umfangreichen Gesichterliste gespeicherte Gesicht über largeFaceListId und persistedFaceId ab. Hiermit nehmen Sie die persistedFaceId und userData in einer angegebenen umfangreichen Gesichterliste auf. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/write | Hiermit fügen Sie ein Gesicht einer angegebenen umfangreichen Liste mit Gesichtern hinzu. Bis zu 1.000.000 Gesichter können hinzugefügt werden. Hiermit aktualisieren Sie das userData-Feld des angegebenen Gesichts in einer umfangreichen Gesichterliste anhand seiner persistedFaceId. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/read | Hiermit rufen Sie largeFaceListId, Name und userData einer umfangreichen Gesichterliste ab. Hiermit führen Sie die Informationen largeFaceListId, Name und userData einer umfangreichen Gesichterliste auf. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/train/action | Hiermit senden Sie einen Trainingstask für eine umfangreiche Gesichterliste. Das Training ist ein wichtiger Schritt, den nur eine trainierte umfangreiche Gesichterliste verwenden kann. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/training/read | Hiermit prüfen Sie den Trainingsstatus für die umfangreiche Gesichterliste, um zu ermitteln, ob der Vorgang abgeschlossen ist oder noch durchgeführt wird. Das LargeFaceList-Training ist ein asynchroner Vorgang. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/write | Hiermit erstellen Sie eine leere umfangreiche Gesichterliste mit benutzerdefinierten Angaben für largeFaceListId, Name und optional userData. Hiermit aktualisieren Sie die Informationen einer umfangreichen Gesichterliste, einschließlich Name und UserData. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/delete | Hiermit löschen Sie eine vorhandene umfangreiche Personengruppe mit angegebener personGroupId. In dieser umfangreichen Personengruppe gespeicherte Daten werden gelöscht. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/action | Hiermit erstellen Sie eine neue Person in einer angegebenen umfangreichen Personengruppe. Um dieser Person ein Gesicht hinzuzufügen, rufen Sie Folgendes auf: |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/delete | Hiermit löschen Sie eine vorhandene Person aus einer umfangreichen Personengruppe. Alle im Personeneintrag gespeicherten Personendaten und Gesichtsbilder werden gelöscht. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/delete | Hiermit löschen Sie ein Gesicht von einer Person in einer umfangreichen Personengruppe. Die zu diesem Gesichtseintrag gehörigen Gesichtsdaten und das Bild werden ebenfalls gelöscht. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/read | Hiermit rufen Sie Informationen zum Gesicht einer Person ab. Das gespeicherte Gesicht der Person wird durch die largePersonGroupId, personId und persistedFaceId angegeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/write | Hiermit fügen Sie einer Person zur Gesichtsidentifikation oder -verifizierung ein Gesichtsbild in einer umfangreichen Personengruppe hinzu. So verarbeiten Sie das Bild von Hiermit aktualisieren Sie das userData-Feld des gespeicherten Gesichts einer Person. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/read | Hiermit rufen Sie den Namen und die userData-Angabe einer Person sowie die gespeicherten faceIds ab, die dem Gesichtsbild der registrierten Person entsprechen. Hiermit listen Sie die Informationen aller Personen in der angegebenen umfangreichen Personengruppe auf, einschließlich personId, Name, userData und persistedFaceIds. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/write | Hiermit aktualisieren Sie den Namen oder die userData-Angabe einer Person. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/read | Hiermit rufen Sie die Informationen einer umfangreichen Personengruppe ab, einschließlich Name und userData. Diese API gibt Informationen zu einer umfangreichen Personengruppe zurück. Listen Sie die largePesonGroupId, den Namen und die userData-Angabe aller vorhandenen umfangreichen Personengruppen auf. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/train/action | Hiermit übermitteln Sie einen Trainingstask für eine umfangreiche Personengruppe. Das Training ist ein wichtiger Schritt, den nur eine trainierte umfangreiche Personengruppe verwenden kann. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/training/read | Hiermit prüfen Sie den Trainingsstatus für die umfangreiche Personengruppe, um zu ermitteln, ob der Vorgang abgeschlossen ist oder noch durchgeführt wird. Das LargePersonGroup-Training ist ein asynchroner Vorgang. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/write | Hiermit erstellen Sie eine neue umfangreiche Personengruppe mit benutzerdefinierten Angaben zu largePersonGroupId, Name und optional userData. Hiermit aktualisieren Sie den Namen und die userData-Angabe einer vorhandenen umfangreichen Personengruppe. Die Eigenschaften bleiben unverändert, wenn sie nicht im Anforderungstext enthalten sind. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/delete | Hiermit löschen Sie eine vorhandene Personengruppe mit angegebener personGroupId. In dieser Personengruppe gespeicherte Daten werden gelöscht. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/action | Hiermit erstellen Sie eine neue Person in einer angegebenen Personengruppe. Um dieser Person ein Gesicht hinzuzufügen, rufen Sie Folgendes auf: |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/delete | Hiermit löschen Sie eine vorhandene Person aus einer Personengruppe. Alle im Personeneintrag gespeicherten Personendaten und Gesichtsbilder werden gelöscht. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/delete | Hiermit löschen Sie ein Gesicht von einer Person in einer Personengruppe. Die zu diesem Gesichtseintrag gehörigen Gesichtsdaten und das Bild werden ebenfalls gelöscht. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/read | Hiermit rufen Sie Informationen zum Gesicht einer Person ab. Das gespeicherte Gesicht der Person wird durch die personGroupId, personId und persistedFaceId angegeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/write | Hiermit fügen Sie einer Person zur Gesichtsidentifikation oder -verifizierung ein Gesichtsbild in einer Personengruppe hinzu. So verarbeiten Sie das Bild von mehreren Hiermit aktualisieren Sie das userData-Feld des gespeicherten Gesichts einer Person. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/read | Hiermit rufen Sie den Namen und die userData-Angabe einer Person sowie die gespeicherten faceIds ab, die dem Gesichtsbild der registrierten Person entsprechen. Hiermit listen Sie die Informationen aller Personen in der angegebenen Personengruppe auf, einschließlich personId, Name, userData und persistedFaceIds registrierter Personen. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/write | Hiermit aktualisieren Sie den Namen oder die userData-Angabe einer Person. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/read | Hiermit rufen Sie den Gruppennamen und die userData-Angabe der Person ab. Zum Abrufen von Informationen unter dieser personGroup verwenden Sie Hiermit listen Sie die Angaben zu personGroupId, Name und userData auf. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/train/action | Hiermit übermitteln Sie einen Trainingstask für eine Personengruppe. Das Training ist ein wichtiger Schritt, den nur eine trainierte Personengruppe verwenden kann. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/training/read | Hiermit prüfen Sie den Trainingsstatus für die Personengruppe, um zu ermitteln, ob der Vorgang abgeschlossen ist oder noch durchgeführt wird. Das PersonGroup-Training ist ein asynchroner Vorgang, ausgelöst durch |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/write | Hiermit erstellen Sie eine neue Personengruppe anhand der Angaben von personGroupId, Name und vom Benutzer bereitgestellten userData. Hiermit aktualisieren Sie den Namen und die userData-Angabe einer vorhandenen Personengruppe. Die Eigenschaften bleiben unverändert, wenn sie nicht im Anforderungstext enthalten sind. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/verify/action | Hiermit überprüfen Sie, ob zwei Gesichter zu derselben Person gehören oder ob je ein Gesicht zu einer Person gehört. |
> | DataAction | Microsoft.CognitiveServices/accounts/FormRecognizer/custom/models/analyze/action | Extrahiert Schlüssel-Wert-Paare aus einem bestimmten Dokument. Das Eingabedokument muss einen der unterstützten Inhaltstypen aufweisen: „application/pdf“, „image/jpeg“ oder „image/png“. Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/FormRecognizer/custom/models/delete | Löscht Modellartefakte. |
> | DataAction | Microsoft.CognitiveServices/accounts/FormRecognizer/custom/models/keys/read | Ruft die Schlüssel für das Modell ab. |
> | DataAction | Microsoft.CognitiveServices/accounts/FormRecognizer/custom/models/read | Ruft Informationen zu einem Modell ab. Ruft Informationen zu allen trainierten benutzerdefinierten Modellen ab.* |
> | DataAction | Microsoft.CognitiveServices/accounts/FormRecognizer/custom/train/action | Erstellt und trainiert ein benutzerdefiniertes Modell.<br>Die Trainingsanforderung muss einen Quellparameter enthalten, bei dem es sich entweder um einen extern zugänglichen Azure Storage-Blobcontainer-URI (vorzugsweise ein Shared Access Signature-URI) oder einen gültigen Pfad zu einem Datenordner auf einem lokal bereitgestellten Laufwerk handelt.<br>Wenn lokale Pfade angegeben werden, müssen diese das Linux-/Unix-Pfadformat aufweisen, und es muss ein absoluter Pfad sein, der der Konfiguration der Eingabeeinbindung entspricht. |
> | DataAction | Microsoft.CognitiveServices/accounts/FormRecognizer/prebuilt/receipt/asyncbatchanalyze/action | Extrahiert Feldtext und semantische Werte aus einem bestimmten Belegdokument. Das Eingabebilddokument muss einen der unterstützten Inhaltstypen aufweisen: JPEG, PNG, BMP, PDF oder TIFF. Eine erfolgreiche Antwort ist JSON-Code mit einem Feld mit dem Namen „Operation-Location“, das die URL für den Vorgang zum Abrufen des Belegergebnisses (Get Receipt Result) enthält, damit die Ergebnisse asynchron abgerufen werden können. |
> | DataAction | Microsoft.CognitiveServices/accounts/FormRecognizer/prebuilt/receipt/operations/action | Fragen Sie den Status ab, und rufen Sie das Ergebnis eines Vorgangs zum Analysieren des Belegs (Analyze Receipt) ab. Die URL dieser Schnittstelle finden Sie im „Operation-Location“-Header in der Analyze Receipt-Antwort. |
> | DataAction | Microsoft.CognitiveServices/accounts/ImageSearch/details/action | Gibt Erkenntnisse zu einem Bild zurück, z. B. Webseiten, die dieses Bild enthalten. |
> | DataAction | Microsoft.CognitiveServices/accounts/ImageSearch/search/action | Abrufen relevanter Bilder für eine bestimmte Abfrage |
> | DataAction | Microsoft.CognitiveServices/accounts/ImageSearch/trending/action | Abrufen derzeit beliebter Bilder |
> | DataAction | Microsoft.CognitiveServices/accounts/ImmersiveReader/getcontentmodelforreader/action | Erstellt eine Sitzung mit Plastischer Reader |
> | DataAction | Microsoft.CognitiveServices/accounts/InkRecognizer/recognize/action | Analysiert bei Übergabe einer Reihe von Strichdaten den Inhalt und generiert eine Liste bekannter Entitäten, einschließlich erkannten Texts. |
> | Action | Microsoft.CognitiveServices/accounts/listKeys/action | Dient zum Auflisten von Schlüsseln. |
> | DataAction | Microsoft.CognitiveServices/accounts/LUIS/predict/action | Ruft die Vorhersage für veröffentlichte Endpunkte für die angegebene Abfrage ab. |
> | DataAction | Microsoft.CognitiveServices/accounts/NewsSearch/categorysearch/action | Gibt Nachrichten für eine angegebene Kategorie zurück. |
> | DataAction | Microsoft.CognitiveServices/accounts/NewsSearch/search/action | Abrufen von Nachrichtenartikeln zu einer bestimmten Abfrage |
> | DataAction | Microsoft.CognitiveServices/accounts/NewsSearch/trendingtopics/action | Abrufen beliebter Themen, die von Bing ermittelt wurden. Dabei handelt es sich um dieselben Themen, die im Banner unten auf der Startseite von Bing angezeigt werden. |
> | Action | Microsoft.CognitiveServices/accounts/read | Liest API-Konten. |
> | Action | Microsoft.CognitiveServices/accounts/regenerateKey/action | Dient zum erneuten Generieren eines Schlüssels. |
> | Action | Microsoft.CognitiveServices/accounts/skus/read | Liest verfügbare SKUs für eine vorhandene Ressource. |
> | DataAction | Microsoft.CognitiveServices/accounts/SpellCheck/spellcheck/action | Abrufen der Ergebnisse einer Abfrage zur Rechtschreibprüfung per GET oder POST |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/entities/action | Die API gibt eine Liste bekannter Entitäten und allgemeiner benannter Entitäten in einem bestimmten Dokument zurück (\"Person\", \"Location\", \"Organization\" usw.). |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/keyphrases/action | Die API gibt eine Liste von Zeichenfolgen zurück, die die wichtigsten Themen im Eingabetext angeben. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/languages/action | Die API gibt die erkannte Sprache und eine numerische Bewertung zwischen 0 und 1 zurück. Werte nahe 1 stehen für 100 %-ige Sicherheit, dass die identifizierte Sprache stimmt. Insgesamt werden 120 Sprachen unterstützt. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/sentiment/action | Die API gibt eine numerische Bewertung zwischen 0 und 1 zurück.<br>Werte nahe 1 zeigen eine positive Absicht an, Werte nahe 0 zeigen eine negative Absicht an.<br>Eine Bewertung von 0,5 gibt das Fehlen der Stimmung an (z. B.<br>bei einer faktischen Aussage). |
> | Action | Microsoft.CognitiveServices/accounts/usages/read | Dient zum Abrufen der Kontingentnutzung für eine vorhandene Ressource. |
> | DataAction | Microsoft.CognitiveServices/accounts/VideoSearch/details/action | Abrufen von Erkenntnissen aus einem Video (z. B. verwandte Videos) |
> | DataAction | Microsoft.CognitiveServices/accounts/VideoSearch/search/action | Abrufen von Videos zu einer bestimmten Abfrage |
> | DataAction | Microsoft.CognitiveServices/accounts/VideoSearch/trending/action | Abrufen derzeit beliebter Videos |
> | DataAction | Microsoft.CognitiveServices/accounts/VisualSearch/search/action | Gibt eine Liste von Tags zum bereitgestellten Bild zurück |
> | DataAction | Microsoft.CognitiveServices/accounts/WebSearch/search/action | Abrufen von Web-, Bild-, News- und Videoergebnissen für eine bestimmte Abfrage |
> | Action | Microsoft.CognitiveServices/accounts/write | Schreibt API-Konten. |
> | Action | Microsoft.CognitiveServices/checkDomainAvailability/action | Liest verfügbare SKUs für ein Abonnement. |
> | Action | Microsoft.CognitiveServices/locations/checkSkuAvailability/action | Liest verfügbare SKUs für ein Abonnement. |
> | Action | Microsoft.CognitiveServices/locations/checkSkuAvailability/action | Liest verfügbare SKUs für ein Abonnement. |
> | Action | Microsoft.CognitiveServices/locations/deleteVirtualNetworkOrSubnets/action | Benachrichtigung von Microsoft.Network über das Löschen virtueller Netzwerke oder Subnetze |
> | Action | Microsoft.CognitiveServices/locations/operationresults/read | Liest den Status eines asynchronen Vorgangs. |
> | Action | Microsoft.CognitiveServices/Operations/read | Listet alle verfügbaren Vorgänge auf. |
> | Action | Microsoft.CognitiveServices/register/action | Registriert ein Abonnement für Cognitive Services. |
> | Action | Microsoft.CognitiveServices/register/action | Registriert ein Abonnement für Cognitive Services. |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Commerce/RateCard/read | Gibt Angebotsdaten, Metadaten für Ressourcen/Verbrauchseinheiten und Preise für das angegebene Abonnement zurück. |
> | Action | Microsoft.Commerce/register/action | Registriert das Abonnement für Microsoft Commerce UsageAggregate. |
> | Action | Microsoft.Commerce/unregister/action | Hebt die Registrierung des Abonnements für Microsoft Commerce UsageAggregate auf. |
> | Action | Microsoft.Commerce/UsageAggregates/read | Ruft die Nutzung von Microsoft Azure für ein Abonnement ab. Das Ergebnis enthält aggregierte Nutzungsdaten sowie abonnement- und ressourcenbezogene Informationen für einen bestimmten Zeitbereich. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Compute/availabilitySets/delete | Löscht die Verfügbarkeitsgruppe. |
> | Action | Microsoft.Compute/availabilitySets/read | Dient zum Abrufen der Eigenschaften einer Verfügbarkeitsgruppe. |
> | Action | Microsoft.Compute/availabilitySets/vmSizes/read | Dient zum Auflisten der Größen, die zum Erstellen oder Aktualisieren eines virtuellen Computers in der Verfügbarkeitsgruppe zur Verfügung stehen. |
> | Action | Microsoft.Compute/availabilitySets/write | Erstellt eine neue Verfügbarkeitsgruppe oder aktualisiert eine bereits vorhandene. |
> | Action | Microsoft.Compute/diskEncryptionSets/delete | Löschen eines Datenträgerverschlüsselungssatzes |
> | Action | Microsoft.Compute/diskEncryptionSets/read | Abrufen der Eigenschaften eines Datenträgerverschlüsselungssatzes |
> | Action | Microsoft.Compute/diskEncryptionSets/write | Erstellen einer neuen Datenträgerverschlüsselungssatzes oder Aktualisieren einer bereits vorhandenen |
> | Action | Microsoft.Compute/disks/beginGetAccess/action | Dient zum Abrufen des SAS-URIs des Datenträgers für den Blobzugriff. |
> | Action | Microsoft.Compute/disks/delete | Löscht den Datenträger. |
> | Action | Microsoft.Compute/disks/endGetAccess/action | Dient zum Sperren des SAS-URIs des Datenträgers. |
> | Action | Microsoft.Compute/disks/read | Dient zum Abrufen der Eigenschaften eines Datenträgers. |
> | Action | Microsoft.Compute/disks/write | Erstellt einen neuen Datenträger oder aktualisiert einen bereits vorhandenen. |
> | Action | Microsoft.Compute/galleries/applications/delete | Löscht die Kataloganwendung. |
> | Action | Microsoft.Compute/galleries/applications/read | Ruft die Eigenschaften der Kataloganwendung ab. |
> | Action | Microsoft.Compute/galleries/applications/versions/delete | Löscht die Version der Kataloganwendung. |
> | Action | Microsoft.Compute/galleries/applications/versions/read | Ruft die Eigenschaften der Kataloganwendungsversion ab. |
> | Action | Microsoft.Compute/galleries/applications/versions/write | Erstellt eine neue Kataloganwendungsversion oder aktualisiert eine vorhandene Version. |
> | Action | Microsoft.Compute/galleries/applications/write | Erstellt eine neue Kataloganwendung oder aktualisiert eine vorhandene. |
> | Action | Microsoft.Compute/galleries/delete | Hiermit wird der Katalog gelöscht. |
> | Action | Microsoft.Compute/galleries/images/delete | Hiermit wird das Katalogimage gelöscht. |
> | Action | Microsoft.Compute/galleries/images/read | Hiermit werden die Eigenschaften des Katalogimages abgerufen. |
> | Action | Microsoft.Compute/galleries/images/versions/delete | Hiermit wird die Katalogimageversion gelöscht. |
> | Action | Microsoft.Compute/galleries/images/versions/read | Hiermit werden die Eigenschaften der Katalogimageversion abgerufen. |
> | Action | Microsoft.Compute/galleries/images/versions/write | Hiermit wird eine neue Katalogimageversion erstellt oder eine vorhandene Katalogimageversion aktualisiert. |
> | Action | Microsoft.Compute/galleries/images/write | Hiermit wird ein neues Katalogimage erstellt oder ein vorhandenes Katalogimage aktualisiert. |
> | Action | Microsoft.Compute/galleries/read | Hiermit werden die Eigenschaften des Katalogs abgerufen. |
> | Action | Microsoft.Compute/galleries/write | Hiermit wird ein neuer Katalog erstellt oder ein vorhandener Katalog aktualisiert. |
> | Action | Microsoft.Compute/hostGroups/delete | Löscht die Hostgruppe |
> | Action | Microsoft.Compute/hostGroups/hosts/delete | Löscht den Host |
> | Action | Microsoft.Compute/hostGroups/hosts/read | Ruft die Eigenschaften eines Hosts ab |
> | Action | Microsoft.Compute/hostGroups/hosts/write | Erstellt einen neuen Host oder aktualisiert einen bereits vorhandenen Host |
> | Action | Microsoft.Compute/hostGroups/read | Ruft die Eigenschaften einer Hostgruppe ab |
> | Action | Microsoft.Compute/hostGroups/write | Erstellt eine neue Hostgruppe oder aktualisiert eine vorhandene Hostgruppe |
> | Action | Microsoft.Compute/images/delete | Löscht das Image. |
> | Action | Microsoft.Compute/images/read | Dient zum Abrufen der Eigenschaften des Images. |
> | Action | Microsoft.Compute/images/write | Erstellt ein neues Image oder aktualisiert ein bereits vorhandenes. |
> | Action | Microsoft.Compute/locations/capsOperations/read | Ruft den Status eines asynchronen Vorgangs zum Abrufen von Obergrenzen ab. |
> | Action | Microsoft.Compute/locations/diskOperations/read | Ruft den Status eines asynchronen Vorgangs zum Abrufen von Datenträgern ab. |
> | Action | Microsoft.Compute/locations/logAnalytics/getRequestRateByInterval/action | Dient zum Erstellen von Protokollen, die die gesamten Anforderungen nach Zeitintervall anzeigen, um bei der Drosselungsdiagnose zu helfen. |
> | Action | Microsoft.Compute/locations/logAnalytics/getThrottledRequests/action | Dient zum Erstellen von Protokollen, die Aggregate von gedrosselten Anforderungen, nach Ressourcenname, OperationName oder der angewendeten Drosselungsrichtlinie gruppiert, anzeigen. |
> | Action | Microsoft.Compute/locations/operations/read | Ruft den Status eines asynchronen Vorgangs ab. |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/offers/read | Ruft die Eigenschaften eines Plattformimageangebots ab. |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/read | Ruft die Eigenschaften einer Plattformimage-SKU ab. |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/versions/read | Ruft die Eigenschaften einer Plattformimageversion ab. |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/types/read | Dient zum Abrufen der Eigenschaften eines VMExtension-Typs. |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/types/versions/read | Dient zum Abrufen der Eigenschaften einer VMExtension-Version. |
> | Action | Microsoft.Compute/locations/publishers/read | Dient zum Abrufen der Eigenschaften eines Herausgebers. |
> | Action | Microsoft.Compute/locations/runCommands/read | Hiermit werden die am Standort verfügbaren Ausführungsbefehle aufgelistet. |
> | Action | Microsoft.Compute/locations/usages/read | Ruft Dienstlimits und aktuelle Nutzungsmengen für die Computeressourcen des Abonnements an einem Standort ab. |
> | Action | Microsoft.Compute/locations/vmSizes/read | Listet die verfügbaren VM-Größen für einen Standort auf. |
> | Action | Microsoft.Compute/locations/vsmOperations/read | Ruft den Status eines asynchronen Vorgangs für eine VM-Skalierungsgruppe mit der Virtual Machine Runtime Service-Erweiterung ab. |
> | Action | Microsoft.Compute/operations/read | Listet verfügbare Vorgänge für den Microsoft.Compute-Ressourcenanbieter auf. |
> | Action | Microsoft.Compute/proximityPlacementGroups/delete | Löscht die Näherungsplatzierungsgruppe |
> | Action | Microsoft.Compute/proximityPlacementGroups/read | Ruft die Eigenschaften einer Näherungsplatzierungsgruppe ab |
> | Action | Microsoft.Compute/proximityPlacementGroups/write | Erstellt eine neue Näherungsplatzierungsgruppe oder aktualisiert eine bereits vorhandene |
> | Action | Microsoft.Compute/register/action | Registriert ein Abonnement beim Microsoft.Compute-Ressourcenanbieter. |
> | Action | Microsoft.Compute/restorePointCollections/delete | Löscht die Wiederherstellungspunktsammlung und die darin enthaltenen Wiederherstellungspunkte. |
> | Action | Microsoft.Compute/restorePointCollections/read | Dient zum Abrufen der Eigenschaften einer Wiederherstellungspunktsammlung. |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/delete | Löscht den Wiederherstellungspunkt. |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/read | Dient zum Abrufen der Eigenschaften eines Wiederherstellungspunkts. |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/retrieveSasUris/action | Dient zum Abrufen der Eigenschaften eines Wiederherstellungspunkts sowie der Blob-SAS-URIs. |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/write | Erstellt einen neuen Wiederherstellungspunkt. |
> | Action | Microsoft.Compute/restorePointCollections/write | Erstellt eine neue Wiederherstellungspunktsammlung oder aktualisiert eine bereits vorhandene. |
> | Action | Microsoft.Compute/sharedVMImages/delete | Löscht SharedVMImage. |
> | Action | Microsoft.Compute/sharedVMImages/read | Dient zum Abrufen der Eigenschaften von SharedVMImage. |
> | Action | Microsoft.Compute/sharedVMImages/versions/delete | Dient zum Löschen von SharedVMImageVersion. |
> | Action | Microsoft.Compute/sharedVMImages/versions/read | Ruft die Eigenschaften von SharedVMImageVersion ab. |
> | Action | Microsoft.Compute/sharedVMImages/versions/replicate/action | Repliziert SharedVMImageVersion in Zielregionen. |
> | Action | Microsoft.Compute/sharedVMImages/versions/write | Erstellt eine neue SharedVMImageVersion oder aktualisiert eine vorhandene. |
> | Action | Microsoft.Compute/sharedVMImages/write | Erstellt ein neues SharedVMImage oder aktualisiert ein vorhandenes. |
> | Action | Microsoft.Compute/skus/read | Ruft die Liste der verfügbaren Microsoft.Compute-SKUs für Ihr Abonnement ab. |
> | Action | Microsoft.Compute/snapshots/beginGetAccess/action | Dient zum Abrufen des SAS-URI der Momentaufnahme für den Blobzugriff. |
> | Action | Microsoft.Compute/snapshots/delete | Dient zum Löschen einer Momentaufnahme. |
> | Action | Microsoft.Compute/snapshots/endGetAccess/action | Dient zum Sperren des SAS-URI der Momentaufnahme. |
> | Action | Microsoft.Compute/snapshots/read | Dient zum Abrufen der Eigenschaften einer Momentaufnahme. |
> | Action | Microsoft.Compute/snapshots/write | Dient zum Erstellen einer neuen Momentaufnahme oder zum Aktualisieren einer bereits vorhandenen. |
> | Action | Microsoft.Compute/unregister/action | Hebt die Registrierung eines Abonnements bei einem Microsoft.Compute-Ressourcenanbieter auf. |
> | Action | Microsoft.Compute/virtualMachines/capture/action | Erfasst den virtuellen Computer, indem die virtuellen Datenträger kopiert werden und eine Vorlage generiert wird, die dann zum Erstellen ähnlicher virtueller Computer verwendet werden kann. |
> | Action | Microsoft.Compute/virtualMachines/convertToManagedDisks/action | Konvertiert blobbasierte Datenträger des virtuellen Computers in verwaltete Datenträger. |
> | Action | Microsoft.Compute/virtualMachines/deallocate/action | Schaltet den virtuellen Computer aus und gibt die Computeressourcen frei. |
> | Action | Microsoft.Compute/virtualMachines/delete | Löscht den virtuellen Computer. |
> | Action | Microsoft.Compute/virtualMachines/extensions/delete | Löscht die VM-Erweiterung. |
> | Action | Microsoft.Compute/virtualMachines/extensions/read | Dient zum Abrufen der Eigenschaften einer VM-Erweiterung. |
> | Action | Microsoft.Compute/virtualMachines/extensions/write | Erstellt eine neue VM-Erweiterung oder aktualisiert eine bereits vorhandene. |
> | Action | Microsoft.Compute/virtualMachines/generalize/action | Legt den Zustand des virtuellen Computers auf „Generalisiert“ fest und bereitet den virtuellen Computer auf die Erfassung vor. |
> | Action | Microsoft.Compute/virtualMachines/instanceView/read | Ruft den detaillierten Laufzeitstatus des virtuellen Computers und seiner Ressourcen ab. |
> | DataAction | Microsoft.Compute/virtualMachines/login/action | Hiermit melden Sie sich bei einem virtuellen Computer als normaler Benutzer an. |
> | DataAction | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Hiermit melden Sie sich bei einem virtuellen Computer mit Windows-Administrator- oder Linux-Root-Benutzerrechten an. |
> | Action | Microsoft.Compute/virtualMachines/performMaintenance/action | Führt den Wartungsvorgang für die VM durch. |
> | Action | Microsoft.Compute/virtualMachines/powerOff/action | Schaltet den virtuellen Computer aus. Der virtuelle Computer wird weiterhin in Rechnung gestellt. |
> | Action | Microsoft.Compute/virtualMachines/read | Dient zum Abrufen der Eigenschaften eines virtuellen Computers. |
> | Action | Microsoft.Compute/virtualMachines/redeploy/action | Stellt den virtuellen Computer erneut bereit. |
> | Action | Microsoft.Compute/virtualMachines/reimage/action | Hiermit wird ein Reimaging für einen virtuellen Computer durchgeführt, der einen differenzierenden Datenträger verwendet. |
> | Action | Microsoft.Compute/virtualMachines/restart/action | Startet den virtuellen Computer neu. |
> | Action | Microsoft.Compute/virtualMachines/runCommand/action | Hiermit wird ein vordefiniertes Skript für den virtuellen Computer ausgeführt. |
> | Action | Microsoft.Compute/virtualMachines/start/action | Startet den virtuellen Computer. |
> | Action | Microsoft.Compute/virtualMachines/vmSizes/read | Listet die verfügbaren Größen auf, auf die der virtuelle Computer aktualisiert werden kann. |
> | Action | Microsoft.Compute/virtualMachines/write | Erstellt einen neuen virtuellen Computer oder aktualisiert einen vorhandenen virtuellen Computer. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/deallocate/action | Schaltet die Computeressourcen für die Instanzen der VM-Skalierungsgruppe aus und gibt sie frei.  |
> | Action | Microsoft.Compute/virtualMachineScaleSets/delete | Löscht die VM-Skalierungsgruppe. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/delete/action | Löscht die Instanzen der VM-Skalierungsgruppe. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/extensions/delete | Hiermit wird die VM-Skalierungsgruppenerweiterung gelöscht. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/extensions/read | Hiermit werden die Eigenschaften einer VM-Skalierungsgruppenerweiterung abgerufen. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/extensions/write | Hiermit wird eine neue VM-Skalierungsgruppenerweiterung erstellt oder eine vorhandene aktualisiert. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action | Hiermit werden Plattformupdatedomänen einer Service Fabric-VM-Skalierungsgruppe manuell durchgeführt, um eine ausstehende Aktualisierung fertigzustellen. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/instanceView/read | Ruft die Instanzansicht der VM-Skalierungsgruppe ab. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/manualUpgrade/action | Aktualisiert Instanzen manuell auf das neueste Modell der VM-Skalierungsgruppe. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/networkInterfaces/read | Ruft die Eigenschaften aller Netzwerkschnittstellen einer VM-Skalierungsgruppe ab. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/osRollingUpgrade/action | Hiermit wird ein paralleles Upgrade gestartet, um alle VM-Skalierungsgruppen auf die neueste verfügbare Plattformimage-Betriebssystemversion zu aktualisieren. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/osUpgradeHistory/read | Ruft den Verlauf von Betriebssystemupgrades für eine VM-Skalierungsgruppe ab. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/performMaintenance/action | Führt ungeplante Wartungsaufgaben für die Instanzen der VM-Skalierungsgruppe durch. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/powerOff/action | Schaltet die Instanzen der VM-Skalierungsgruppe aus. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/publicIPAddresses/read | Ruft die Eigenschaften aller öffentlichen IP-Adressen einer VM-Skalierungsgruppe ab. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/read | Dient zum Abrufen der Eigenschaften einer VM-Skalierungsgruppe. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/redeploy/action | Stellt erneut die Instanzen der VM-Skalierungsgruppe bereit. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/reimage/action | Führt ein Reimaging für die Instanzen der VM-Skalierungsgruppe durch. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/reimageAll/action | Hiermit wird ein Reimaging für alle Datenträger (Betriebssystem- und andere Datenträger) für die Instanzen einer VM-Skalierungsgruppe durchgeführt.  |
> | Action | Microsoft.Compute/virtualMachineScaleSets/restart/action | Startet die Instanzen der VM-Skalierungsgruppe neu. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/cancel/action | Bricht das parallele Upgrade einer VM-Skalierungsgruppe ab. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/read | Hiermit wird der aktuelle Status des parallelen Upgrades für eine VM-Skalierungsgruppe abgerufen. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/scale/action | Hiermit wird überprüft, ob eine vorhandene VM-Skalierungsgruppe horizontal auf die angegebene Anzahl von Instanzen hoch- oder herunterskaliert werden kann. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/skus/read | Listet die gültigen SKUs für eine vorhandene VM-Skalierungsgruppe auf. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/start/action | Startet die Instanzen der VM-Skalierungsgruppe. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/deallocate/action | Schaltet die Computeressourcen für einen virtuellen Computer in einer VM-Skalierungsgruppe aus und gibt sie frei. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/delete | Dient zum Löschen eines bestimmten virtuellen Computers in einer VM-Skalierungsgruppe. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/instanceView/read | Ruft die Instanzansicht eines virtuellen Computers in einer VM-Skalierungsgruppe ab. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/publicIPAddresses/read | Ruft die Eigenschaften einer öffentlichen IP-Adresse ab, die mithilfe einer VM-Skalierungsgruppe erstellt wurde. Eine VM-Skalierungsgruppe kann höchstens eine öffentliche IP pro IP-Konfiguration (private IP) erstellen. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read | Ruft die Eigenschaften einer oder aller IP-Konfigurationen einer Netzwerkschnittstelle ab, die mit der VM-Skalierungsgruppe erstellt wurde. IP-Konfigurationen stellen private IP-Adressen dar. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | Ruft die Eigenschaften einer oder aller Netzwerkschnittstelle eines virtuellen Computers ab, der mithilfe der VM-Skalierungsgruppe erstellt wurde. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/performMaintenance/action | Führt ungeplante Wartungsaufgaben für eine VM-Instanz in einer VM-Skalierungsgruppe durch. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/powerOff/action | Schaltet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe aus. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read | Ruft die Eigenschaften eines virtuellen Computers in einer VM-Skalierungsgruppe ab. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/redeploy/action | Hiermit wird eine erneute Bereitstellung für eine VM-Instanz in einer VM-Skalierungsgruppe durchgeführt. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimage/action | Hiermit wird ein Reimaging für eine VM-Instanz in einer VM-Skalierungsgruppe durchgeführt. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimageAll/action | Hiermit wird ein Reimaging für alle Datenträger (Betriebssystem- und andere Datenträger) für eine VM-Instanz in einer VM-Skalierungsgruppe durchgeführt. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/restart/action | Startet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe neu. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/runCommand/action | Hiermit wird ein vordefiniertes Skript für eine VM-Instanz in einer VM-Skalierungsgruppe ausgeführt. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/start/action | Startet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/write | Aktualisiert die Eigenschaften eines virtuellen Computers in einer VM-Skalierungsgruppe. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/vmSizes/read | Listet die Größen auf, die zum Erstellen oder Aktualisieren eines virtuellen Computers in der VM-Skalierungsgruppe zur Verfügung stehen |
> | Action | Microsoft.Compute/virtualMachineScaleSets/write | Erstellt eine neue VM-Skalierungsgruppe oder aktualisiert eine bereits vorhandene. |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Consumption/aggregatedcost/read | Listet AggregatedCost für eine Verwaltungsgruppe auf. |
> | Action | Microsoft.Consumption/balances/read | Listet die Auslastungszusammenfassung für einen Abrechnungszeitraum für eine Verwaltungsgruppe auf. |
> | Action | Microsoft.Consumption/budgets/delete | Löscht die Budgets anhand eines Abonnements oder einer Verwaltungsgruppe. |
> | Action | Microsoft.Consumption/budgets/read | Liste die Budgets nach einem Abonnement oder einer Verwaltungsgruppe auf. |
> | Action | Microsoft.Consumption/budgets/write | Erstellt und aktualisiert die Budgets anhand eines Abonnements oder einer Verwaltungsgruppe. |
> | Action | Microsoft.Consumption/charges/read | Listet Gebühren auf. |
> | Action | Microsoft.Consumption/credits/read | Listet Gutschriften auf. |
> | Action | Microsoft.Consumption/events/read | Listet Ereignisse auf. |
> | Action | Microsoft.Consumption/externalBillingAccounts/tags/read | Listet Tags für EA und Abonnements auf. |
> | Action | Microsoft.Consumption/externalSubscriptions/tags/read | Listet Tags für EA und Abonnements auf. |
> | Action | Microsoft.Consumption/forecasts/read | Listet Vorhersagen auf. |
> | Action | Microsoft.Consumption/lots/read | Listet Chargen auf. |
> | Action | Microsoft.Consumption/marketplaces/read | Listet die Nutzungsdetails von Marketplace-Ressourcen für einen Bereich für EA- und WebDirect-Abonnements auf. |
> | Action | Microsoft.Consumption/operationresults/read | Listet Vorgangsergebnisse auf. |
> | Action | Microsoft.Consumption/operations/read | Listet alle vom Microsoft.Consumption-Ressourcenanbieter unterstützte Vorgänge auf. |
> | Action | Microsoft.Consumption/operationstatus/read | Listet den Vorgangsstatus auf. |
> | Action | Microsoft.Consumption/pricesheets/read | Listet die Daten zu Preisblättern nach einem Abonnement oder einer Verwaltungsgruppe auf. |
> | Action | Microsoft.Consumption/register/action | Führt die Registrierung für den Verbrauchsressourcenanbieter durch. |
> | Action | Microsoft.Consumption/reservationDetails/read | Listet die Auslastungsdetails für reservierte Instanzen nach Reservierungsauftrag oder Verwaltungsgruppe auf. Die Detaildaten gelten pro Instanz und Tag. |
> | Action | Microsoft.Consumption/reservationRecommendations/read | Führt einzelne oder freigegebene Empfehlungen für reservierte Instanzen für ein Abonnement ab. |
> | Action | Microsoft.Consumption/reservationSummaries/read | Zeigt die Auslastungszusammenfassung für reservierte Instanzen nach Reservierungsauftrag oder Verwaltungsgruppe an. Die Zusammenfassungsdaten gelten entweder pro Monat oder Tag. |
> | Action | Microsoft.Consumption/reservationTransactions/read | Listet den Transaktionsverlauf für reservierte Instanzen nach Verwaltungsgruppe auf. |
> | Action | Microsoft.Consumption/tags/read | Listet Tags für EA und Abonnements auf. |
> | Action | Microsoft.Consumption/tenants/read | Hiermit listen Sie Mandanten auf. |
> | Action | Microsoft.Consumption/tenants/register/action | Hiermit registrieren Sie eine Aktion für den Umfang von Microsoft.Consumption durch einen Mandanten. |
> | Action | Microsoft.Consumption/terms/read | Liste die Bedingungen für ein Abonnement oder eine Verwaltungsgruppe auf. |
> | Action | Microsoft.Consumption/usageDetails/read | Listet die Nutzungsdetails für einen Bereich für EA- und WebDirect-Abonnements auf. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ContainerInstance/containerGroups/containers/buildlogs/read | Ruft die Buildprotokolle für einen bestimmten Container ab. |
> | Action | Microsoft.ContainerInstance/containerGroups/containers/exec/action | Hiermit erfolgt eine Ausführung in einem bestimmten Container. |
> | Action | Microsoft.ContainerInstance/containerGroups/containers/logs/read | Hiermit rufen Sie Protokolle für einen bestimmten Container ab. |
> | Action | Microsoft.ContainerInstance/containerGroups/delete | Hiermit löschen Sie eine bestimmte Containergruppe. |
> | Action | Microsoft.ContainerInstance/containerGroups/operationResults/read | Abrufen des Ergebnisses eines asynchronen Vorgangs |
> | Action | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Containergruppe ab. |
> | Action | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Containergruppe. |
> | Action | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für die Containergruppe ab. |
> | Action | Microsoft.ContainerInstance/containerGroups/read | Ruft alle Containergruppen ab. |
> | Action | Microsoft.ContainerInstance/containerGroups/restart/action | Startet eine bestimmte Containergruppe neu. |
> | Action | Microsoft.ContainerInstance/containerGroups/start/action | Startet eine bestimmte Containergruppe. |
> | Action | Microsoft.ContainerInstance/containerGroups/stop/action | Beendet eine bestimmte Containergruppe. Die Zuordnung von Computeressourcen wird aufgehoben, und die Berechnung von Gebühren wird beendet. |
> | Action | Microsoft.ContainerInstance/containerGroups/write | Hiermit erstellen oder aktualisieren Sie eine bestimmte Containergruppe. |
> | Action | Microsoft.ContainerInstance/locations/cachedImages/read | Ruft die zwischengespeicherten Images für das Abonnement in einer Region ab. |
> | Action | Microsoft.ContainerInstance/locations/capabilities/read | Ruft die Funktionen für eine Region ab. |
> | Action | Microsoft.ContainerInstance/locations/deleteVirtualNetworkOrSubnets/action | Benachrichtigt „Microsoft.ContainerInstance“ darüber, dass das virtuelle Netzwerk oder Subnetz gelöscht wird. |
> | Action | Microsoft.ContainerInstance/locations/operations/read | Auflisten der Vorgänge für den Azure Container Instances-Dienst |
> | Action | Microsoft.ContainerInstance/locations/usages/read | Ruft die Nutzung für eine bestimmte Region ab. |
> | Action | Microsoft.ContainerInstance/operations/read | Auflisten der Vorgänge für den Azure Container Instances-Dienst |
> | Action | Microsoft.ContainerInstance/register/action | Registriert das Abonnement für den Ressourcenanbieter der Containerinstanz und aktiviert die Erstellung von Containergruppen. |
> | Action | Microsoft.ContainerInstance/serviceassociationlinks/delete | Löschen des vom Azure Container Instances-Ressourcenanbieter erstellten Dienstzuordnungslinks in einem Subnetz |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ContainerRegistry/checkNameAvailability/read | Überprüft, ob der Name der Containerregistrierung für die Verwendung verfügbar ist. |
> | Action | Microsoft.ContainerRegistry/locations/deleteVirtualNetworkOrSubnets/action | Benachrichtigt Microsoft.ContainerRegistry darüber, dass das virtuelle Netzwerk oder Subnetz gelöscht wird. |
> | Action | Microsoft.ContainerRegistry/locations/operationResults/read | Ruft das Ergebnis eines asynchronen Vorgangs ab. |
> | Action | Microsoft.ContainerRegistry/operations/read | Listet alle verfügbaren REST-API-Vorgänge von Azure Container Registry auf. |
> | Action | Microsoft.ContainerRegistry/register/action | Registriert das Abonnement für den Containerregistrierungs-Ressourcenanbieter und ermöglicht die Erstellung von Containerregistrierungen. |
> | Action | Microsoft.ContainerRegistry/registries/artifacts/delete | Löschen von Artefakten aus einer Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/builds/cancel/action | Bricht einen vorhandenen Build ab. |
> | Action | Microsoft.ContainerRegistry/registries/builds/getLogLink/action | Ruft einen Link zum Herunterladen der Buildprotokolle ab. |
> | Action | Microsoft.ContainerRegistry/registries/builds/read | Ruft die Eigenschaften des angegebenen Builds ab oder listet alle Builds für die angegebene Containerregistrierung auf. |
> | Action | Microsoft.ContainerRegistry/registries/builds/write | Aktualisiert einen Build für eine Containerregistrierung mit den angegebenen Parametern. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/delete | Löscht eine Buildaufgabe aus einer Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/listSourceRepositoryProperties/action | Listet die Eigenschaften der Quellcodeverwaltung für eine Buildaufgabe auf. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/read | Ruft die Eigenschaften der angegebenen Buildaufgabe ab oder listet alle Buildaufgaben für die angegebene Containerregistrierung auf. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/delete | Löscht einen Buildschritt aus einer Buildaufgabe. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/listBuildArguments/action | Listet die Buildargumente für einen Buildschritt einschließlich der Geheimnisargumente auf. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/read | Ruft die Eigenschaften des angegebenen Buildschritts ab oder listet alle Buildschritte für die angegebene Buildaufgabe auf. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/write | Erstellt oder aktualisiert einen Buildschritt für eine Buildaufgabe mit den angegebenen Parametern. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/write | Erstellt oder aktualisiert eine Buildaufgabe für eine Containerregistrierung mit den angegebenen Parametern. |
> | Action | Microsoft.ContainerRegistry/registries/delete | Löscht eine Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/eventGridFilters/delete | Löscht einen Event Grid-Filter aus einer Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/eventGridFilters/read | Ruft die Eigenschaften des angegebenen Event Grid-Filters ab oder listet alle Event Grid-Filter für die angegebene Containerregistrierung auf. |
> | Action | Microsoft.ContainerRegistry/registries/eventGridFilters/write | Erstellt oder aktualisiert einen Event Grid-Filter für eine Containerregistrierung mit den angegebenen Parametern. |
> | Action | Microsoft.ContainerRegistry/registries/generateCredentials/action | Generiert Schlüssel für ein Token mit einer angegebenen Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/getBuildSourceUploadUrl/action | Ruft den Uploadspeicherort ab, damit der Benutzer die Quelle hochladen kann. |
> | Action | Microsoft.ContainerRegistry/registries/importImage/action | Importiert ein Image in die Containerregistrierung mit den angegebenen Parametern. |
> | Action | Microsoft.ContainerRegistry/registries/listBuildSourceUploadUrl/action | Ruft den Quellupload-URL-Speicherort für eine Containerregistrierung ab. |
> | Action | Microsoft.ContainerRegistry/registries/listCredentials/action | Listet die Anmeldeinformationen für die angegebene Containerregistrierung auf. |
> | Action | Microsoft.ContainerRegistry/registries/listPolicies/read | Listet die Richtlinien für die angegebene Containerregistrierung auf. |
> | Action | Microsoft.ContainerRegistry/registries/listUsages/read | Listet den Kontingentbedarf für die angegebene Containerregistrierung auf. |
> | Action | Microsoft.ContainerRegistry/registries/metadata/read | Ruft die Metadaten eines angegebenen Repositorys für eine Containerregistrierung ab |
> | Action | Microsoft.ContainerRegistry/registries/metadata/write | Aktualisiert die Metadaten eines Repositorys für eine Containerregistrierung |
> | Action | Microsoft.ContainerRegistry/registries/operationStatuses/read | Ruft den Status eines asynchronen Registrierungsvorgangs ab. |
> | Action | Microsoft.ContainerRegistry/registries/pull/read | Pullen oder Abrufen von Images aus einer Containerregistrierung |
> | Action | Microsoft.ContainerRegistry/registries/push/write | Pushen oder Schreiben von Images in eine Containerregistrierung |
> | Action | Microsoft.ContainerRegistry/registries/quarantine/read | Pullen oder Abrufen von Images in Quarantäne aus einer Containerregistrierung |
> | Action | Microsoft.ContainerRegistry/registries/quarantine/write | Schreiben/Ändern des Quarantänezustands von unter Quarantäne gestellten Images |
> | Action | Microsoft.ContainerRegistry/registries/queueBuild/action | Erstellt einen neuen Build basierend auf den Anforderungsparametern und fügt ihn der Buildwarteschlange hinzu. |
> | Action | Microsoft.ContainerRegistry/registries/read | Ruft die Eigenschaften der angegebenen Containerregistrierung ab oder listet alle Containerregistrierungen unter der angegebenen Ressourcengruppe oder dem angegebenen Abonnement auf. |
> | Action | Microsoft.ContainerRegistry/registries/regenerateCredential/action | Generiert die Anmeldeinformationen für die angegebene Containerregistrierung neu. |
> | Action | Microsoft.ContainerRegistry/registries/replications/delete | Löscht eine Replikation aus einer Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/replications/operationStatuses/read | Ruft den Status eines asynchronen Replikationsvorgangs ab. |
> | Action | Microsoft.ContainerRegistry/registries/replications/read | Ruft die Eigenschaften der angegebenen Replikation ab oder listet alle Replikationen für die angegebene Containerregistrierung auf. |
> | Action | Microsoft.ContainerRegistry/registries/replications/write | Erstellt oder aktualisiert eine Replikation für eine Containerregistrierung mit den angegebenen Parametern. |
> | Action | Microsoft.ContainerRegistry/registries/runs/cancel/action | Bricht eine vorhandene Ausführung ab. |
> | Action | Microsoft.ContainerRegistry/registries/runs/listLogSasUrl/action | Ruft die Protokoll-SAS-URL für eine Ausführung ab. |
> | Action | Microsoft.ContainerRegistry/registries/runs/read | Ruft die Eigenschaften einer Ausführung für eine Containerregistrierung oder Listenausführungen ab. |
> | Action | Microsoft.ContainerRegistry/registries/runs/write | Aktualisiert eine Ausführung. |
> | Action | Microsoft.ContainerRegistry/registries/scheduleRun/action | Plant eine Ausführung für eine Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/scopeMaps/delete | Löscht eine Bereichszuordnung aus einer Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/scopeMaps/operationStatuses/read | Ruft den Status eines asynchronen Vorgangs der Bereichszuordnung ab. |
> | Action | Microsoft.ContainerRegistry/registries/scopeMaps/read | Ruft die Eigenschaften der angegebenen Bereichszuordnung ab oder listet alle Bereichszuordnungen für die angegebene Containerregistrierung auf. |
> | Action | Microsoft.ContainerRegistry/registries/scopeMaps/write | Erstellt oder aktualisiert eine Bereichszuordnung für eine Containerregistrierung mit den angegebenen Parametern. |
> | Action | Microsoft.ContainerRegistry/registries/sign/write | Pushen/Pullen von Inhaltsvertrauen-Metadaten für eine Containerregistrierung |
> | Action | Microsoft.ContainerRegistry/registries/tasks/delete | Löscht einen Task für eine Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/tasks/listDetails/action | Listet die Details eines Tasks für eine Containerregistrierung auf. |
> | Action | Microsoft.ContainerRegistry/registries/tasks/read | Ruft einen Task für eine Containerregistrierung ab oder listet alle Tasks auf. |
> | Action | Microsoft.ContainerRegistry/registries/tasks/write | Erstellt oder aktualisiert einen Task für eine Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/tokens/delete | Löscht ein Token aus einer Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/tokens/operationStatuses/read | Ruft den Status des asynchronen Vorgangs eines Tokens ab. |
> | Action | Microsoft.ContainerRegistry/registries/tokens/read | Ruft die Eigenschaften des angegebenen Tokens ab oder listet alle Token für die angegebene Containerregistrierung auf. |
> | Action | Microsoft.ContainerRegistry/registries/tokens/write | Erstellt oder aktualisiert ein Token für eine Containerregistrierung mit den angegebenen Parametern. |
> | Action | Microsoft.ContainerRegistry/registries/updatePolicies/write | Aktualisiert die Richtlinien für die angegebene Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/delete | Löscht einen Webhook aus einer Containerregistrierung. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/getCallbackConfig/action | Ruft die Konfiguration des Dienst-URI und benutzerdefinierte Header für den Webhook ab. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/listEvents/action | Listet die aktuellen Ereignisse für den angegebenen Webhook auf. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/operationStatuses/read | Ruft den Status eines asynchronen Webhookvorgangs ab. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/ping/action | Löst ein Pingereignis aus, das an den Webhook gesendet werden muss. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/read | Ruft die Eigenschaften des angegebenen Webhooks ab oder listet alle Webhooks für die angegebene Containerregistrierung auf. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/write | Erstellt oder aktualisiert einen Webhook für eine Containerregistrierung mit den angegebenen Parametern. |
> | Action | Microsoft.ContainerRegistry/registries/write | Erstellt oder aktualisiert eine Containerregistrierung mit den angegebenen Parametern. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ContainerService/containerServices/delete | Erstellt einen Containerdienst. |
> | Action | Microsoft.ContainerService/containerServices/read | Ruft einen Containerdienst ab. |
> | Action | Microsoft.ContainerService/containerServices/write | Erstellt einen neuen Containerdienst oder aktualisiert einen vorhandenen. |
> | Action | Microsoft.ContainerService/locations/operationresults/read | Ruft den Status eines Ergebnisses eines asynchronen Vorgangs ab. |
> | Action | Microsoft.ContainerService/locations/operations/read | Ruft den Status eines asynchronen Vorgangs ab. |
> | Action | Microsoft.ContainerService/locations/orchestrators/read | Listet die unterstützten Orchestratoren auf. |
> | Action | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Ruft ein Zugriffsprofil für verwaltete Cluster anhand des Rollennamens mithilfe der Liste der Anmeldeinformationen ab. |
> | Action | Microsoft.ContainerService/managedClusters/accessProfiles/read | Ruft ein Zugriffsprofil für verwaltete Cluster anhand des Rollennamens ab. |
> | Action | Microsoft.ContainerService/managedClusters/agentPools/delete | Löscht einen Agentpool |
> | Action | Microsoft.ContainerService/managedClusters/agentPools/read | Ruft einen Agentpool ab |
> | Action | Microsoft.ContainerService/managedClusters/agentPools/upgradeProfiles/read | Ruft das Upgradeprofil des Agentpools ab |
> | Action | Microsoft.ContainerService/managedClusters/agentPools/write | Erstellt einen neuen Agentpool oder aktualisiert einen vorhandenen |
> | Action | Microsoft.ContainerService/managedClusters/availableAgentPoolVersions/read | Ruft die verfügbaren Agentpool-Versionen des Clusters ab. |
> | Action | Microsoft.ContainerService/managedClusters/delete | Löscht einen verwalteten Cluster. |
> | Action | Microsoft.ContainerService/managedClusters/detectors/read | Ruft die Erkennung verwalteter Cluster ab |
> | Action | Microsoft.ContainerService/managedClusters/diagnosticsState/read | Abrufen des Diagnosezustands des Clusters |
> | Action | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Listet die clusterAdmin-Anmeldeinformationen eines verwalteten Clusters auf. |
> | Action | Microsoft.ContainerService/managedClusters/listClusterMonitoringUserCredential/action | Listet die clusterMonitoringUser-Anmeldeinformationen eines verwalteten Clusters auf. |
> | Action | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Listet die clusterUser-Anmeldeinformationen eines verwalteten Clusters auf. |
> | Action | Microsoft.ContainerService/managedClusters/privateEndpointConnectionsApproval/action | Bestimmt, ob der Benutzer eine private Endpunktverbindung genehmigen darf. |
> | Action | Microsoft.ContainerService/managedClusters/read | Ruft einen verwalteten Cluster ab. |
> | Action | Microsoft.ContainerService/managedClusters/resetAADProfile/action | Setzt das AAD-Profil eines verwalteten Clusters zurück. |
> | Action | Microsoft.ContainerService/managedClusters/resetServicePrincipalProfile/action | Setzt das Dienstprinzipalprofil eines verwalteten Clusters zurück. |
> | Action | Microsoft.ContainerService/managedClusters/rotateClusterCertificates/action | Führt die Rotation von Zertifikaten eines verwalteten Clusters durch. |
> | Action | Microsoft.ContainerService/managedClusters/upgradeProfiles/read | Ruft das Upgradeprofil des Clusters ab. |
> | Action | Microsoft.ContainerService/managedClusters/write | Erstellt einen neuen verwalteten Cluster oder aktualisiert einen vorhandenen. |
> | Action | Microsoft.ContainerService/openShiftClusters/delete | Löscht einen geöffneten Shiftcluster. |
> | Action | Microsoft.ContainerService/openShiftClusters/read | Ruft einen geöffneten Shiftcluster ab. |
> | Action | Microsoft.ContainerService/openShiftClusters/write | Erstellt einen neuen geöffneten Shiftcluster oder aktualisiert einen vorhandenen. |
> | Action | Microsoft.ContainerService/openShiftManagedClusters/delete | Löscht einen geöffneten verwalteten Shiftcluster. |
> | Action | Microsoft.ContainerService/openShiftManagedClusters/read | Ruft einen geöffneten verwalteten Shiftcluster ab. |
> | Action | Microsoft.ContainerService/openShiftManagedClusters/write | Erstellt einen neuen verwalteten geöffneten Shiftcluster oder aktualisiert einen vorhandenen. |
> | Action | Microsoft.ContainerService/operations/read | Listet verfügbare Vorgänge für den Microsoft.ContainerService-Ressourcenanbieter auf. |
> | Action | Microsoft.ContainerService/register/action | Registriert ein Abonnement beim Microsoft.ContainerService-Ressourcenanbieter. |
> | Action | Microsoft.ContainerService/unregister/action | Hebt die Registrierung eines Abonnements beim Microsoft.ContainerService-Ressourcenanbieter auf. |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.CostManagement/alerts/write | Aktualisieren von Warnungen. |
> | Action | Microsoft.CostManagement/cloudConnectors/delete | Löscht den angegebenen Cloudconnector |
> | Action | Microsoft.CostManagement/cloudConnectors/read | Listet die Cloudconnectors für den authentifizierten Benutzer auf |
> | Action | Microsoft.CostManagement/cloudConnectors/write | Erstellt oder aktualisiert den angegebenen Cloudconnector |
> | Action | Microsoft.CostManagement/dimensions/read | Dient zum Auflisten aller unterstützten Dimensionen nach Bereich. |
> | Action | Microsoft.CostManagement/exports/action | Führt den angegebenen Export aus. |
> | Action | Microsoft.CostManagement/exports/delete | Löscht den angegebenen Export. |
> | Action | Microsoft.CostManagement/exports/read | Listet die Exporte nach Bereich auf. |
> | Action | Microsoft.CostManagement/exports/run/action | Führt Exporte aus |
> | Action | Microsoft.CostManagement/exports/write | Erstellt oder aktualisiert den angegebenen Export. |
> | Action | Microsoft.CostManagement/externalBillingAccounts/dimension/read | Listet alle unterstützten Dimensionen für externe BillingAccounts auf. |
> | Action | Microsoft.CostManagement/externalBillingAccounts/externalSubscriptions/read | Listet die externalSubscriptions in einem externalBillingAccount für den authentifizierten Benutzer auf |
> | Action | Microsoft.CostManagement/externalBillingAccounts/forecast/action | Prognostiziert die Nutzungsdaten für externe BillingAccounts. |
> | Action | Microsoft.CostManagement/externalBillingAccounts/forecast/read | Prognostiziert die Nutzungsdaten für externe BillingAccounts. |
> | Action | Microsoft.CostManagement/externalBillingAccounts/query/action | Fragt die Nutzungsdaten für externe BillingAccounts ab. |
> | Action | Microsoft.CostManagement/externalBillingAccounts/query/read | Fragt die Nutzungsdaten für externe BillingAccounts ab. |
> | Action | Microsoft.CostManagement/externalBillingAccounts/read | Listet die externalBillingAccounts für den authentifizierten Benutzer auf |
> | Action | Microsoft.CostManagement/externalSubscriptions/dimensions/read | Listet alle unterstützten Dimensionen für externe Abonnements auf. |
> | Action | Microsoft.CostManagement/externalSubscriptions/forecast/action | Prognostiziert die Nutzungsdaten für externe BillingAccounts. |
> | Action | Microsoft.CostManagement/externalSubscriptions/forecast/read | Prognostiziert die Nutzungsdaten für externe BillingAccounts. |
> | Action | Microsoft.CostManagement/externalSubscriptions/query/action | Fragt die Nutzungsdaten für das externe Abonnement ab. |
> | Action | Microsoft.CostManagement/externalSubscriptions/query/read | Fragt die Nutzungsdaten für das externe Abonnement ab. |
> | Action | Microsoft.CostManagement/externalSubscriptions/read | Listet die externalSubscriptions für den authentifizierten Benutzer auf |
> | Action | Microsoft.CostManagement/externalSubscriptions/write | Aktualisiert die zugeordnete Verwaltungsgruppe der externalSubscription |
> | Action | Microsoft.CostManagement/forecast/action | Prognostiziert Nutzungsdaten nach Bereich. |
> | Action | Microsoft.CostManagement/forecast/read | Prognostiziert Nutzungsdaten nach Bereich. |
> | Action | Microsoft.CostManagement/operations/read | Listet alle vom Microsoft.CostManagement-Ressourcenanbieter unterstützten Vorgänge auf. |
> | Action | Microsoft.CostManagement/query/action | Dient zum Abfragen von Nutzungsdaten nach Bereich. |
> | Action | Microsoft.CostManagement/query/read | Dient zum Abfragen von Nutzungsdaten nach Bereich. |
> | Action | Microsoft.CostManagement/register/action | Registriert eine Aktion im Bereich von Microsoft.CostManagement mit einem Abonnement |
> | Action | Microsoft.CostManagement/reports/action | Dient zum Planen von Berichten zu Nutzungsdaten nach Bereich. |
> | Action | Microsoft.CostManagement/reports/read | Dient zum Planen von Berichten zu Nutzungsdaten nach Bereich. |
> | Action | Microsoft.CostManagement/tenants/register/action | Registriert eine Aktion im Bereich von Microsoft.CostManagement mit einem Mandanten |
> | Action | Microsoft.CostManagement/views/action | Erstellt eine Ansicht. |
> | Action | Microsoft.CostManagement/views/delete | Löscht gespeicherte Ansichten. |
> | Action | Microsoft.CostManagement/views/read | Listet alle gespeicherten Ansichten auf. |
> | Action | Microsoft.CostManagement/views/write | Aktualisiert die Ansicht. |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DataBox/jobs/bookShipmentPickUp/action | Hiermit kann eine Abholung für Rücklieferungen gebucht werden. |
> | Action | Microsoft.DataBox/jobs/cancel/action | Hiermit wird ein laufender Auftrag storniert. |
> | Action | Microsoft.DataBox/jobs/delete | Hiermit werden die Aufträge gelöscht. |
> | Action | Microsoft.DataBox/jobs/listCredentials/action | Hiermit werden die unverschlüsselten Anmeldeinformationen für den Auftrag aufgelistet. |
> | Action | Microsoft.DataBox/jobs/read | Hiermit werden die Aufträge aufgelistet oder abgerufen. |
> | Action | Microsoft.DataBox/jobs/write | Dient zum Erstellen oder Aktualisieren der Aufträge. |
> | Action | Microsoft.DataBox/locations/availableSkus/action | Mit dieser Methode wird die Liste der verfügbaren SKUs zurückgegeben. |
> | Action | Microsoft.DataBox/locations/availableSkus/read | Liste die verfügbaren SKUs auf oder ruft sie ab |
> | Action | Microsoft.DataBox/locations/operationResults/read | Listet die Vorgangsergebnisse auf bzw. ruft sie ab. |
> | Action | Microsoft.DataBox/locations/regionConfiguration/action | Diese Methode gibt die Konfigurationen für die Region zurück. |
> | Action | Microsoft.DataBox/locations/validateAddress/action | Hiermit wird die Lieferadresse überprüft, und es werden – sofern vorhanden – alternative Adressen bereitgestellt. |
> | Action | Microsoft.DataBox/locations/validateInputs/action | Diese Methode führt alle Arten von Prüfungen aus. |
> | Action | Microsoft.DataBox/operations/read | Listet die Vorgänge auf bzw. ruft sie ab |
> | Action | Microsoft.DataBox/register/action | Registriert den Anbieter „Microsoft.Databox“. |
> | Action | Microsoft.DataBox/subscriptions/resourceGroups/moveResources/action | Diese Methode führt die Verschiebung der Ressource durch. |
> | Action | Microsoft.DataBox/subscriptions/resourceGroups/validateMoveResources/action | Diese Methode überprüft, ob die Verschiebung der Ressource zulässig ist oder nicht. |
> | Action | Microsoft.DataBox/unregister/action | Hebt die Registrierung des Anbieters „Microsoft.Databox“ auf |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/alerts/read | Listet die Warnungen auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/delete | Löscht die Bandbreitenzeitpläne. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/operationResults/read | Listet die Vorgangsergebnisse auf oder ruft sie ab |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | Listet die Bandbreitenzeitpläne auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | Listet die Bandbreitenzeitpläne auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/write | Hiermit werden die Bandbreitenzeitpläne erstellt oder aktualisiert. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/delete | Löscht die Data Box Edge-Geräte. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/downloadUpdates/action | Dient zum Herunterladen von Updates auf Geräte. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/getExtendedInformation/action | Ruft die erweiterten Informationen der Ressource ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/installUpdates/action | Dient zum Installieren von Updates auf einem Gerät. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/jobs/read | Listet die Aufträge auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/networkSettings/read | Listet die Netzwerkeinstellungen des Geräts auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/nodes/read | Listet die Knoten auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/operationResults/read | Listet die Vorgangsergebnisse auf oder ruft sie ab |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/operationsStatus/read | Dient zum Auflisten oder Abrufen des Vorgangsstatus. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/delete | Hiermit werden die Aufträge gelöscht. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/operationResults/read | Listet die Vorgangsergebnisse auf oder ruft sie ab |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/read | Listet die Aufträge auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/read | Listet die Aufträge auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/write | Erstellt oder aktualisiert die Aufträge. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Listet die Data Box Edge-Geräte auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Listet die Data Box Edge-Geräte auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Listet die Data Box Edge-Geräte auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/delete | Löscht die Rollen. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/operationResults/read | Listet die Vorgangsergebnisse auf oder ruft sie ab |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Listet die Rollen auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Listet die Rollen auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/write | Erstellt oder aktualisiert die Rollen. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/scanForUpdates/action | Dient zum Suchen nach Updates. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/operationResults/read | Listet die Vorgangsergebnisse auf oder ruft sie ab |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/update/action | Dient zum Aktualisieren der Sicherheitseinstellungen. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/delete | Löscht die Freigaben. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/operationResults/read | Listet die Vorgangsergebnisse auf oder ruft sie ab |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | Listet die Freigaben auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | Listet die Freigaben auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/refresh/action | Aktualisiert Freigabemetadaten mit Daten aus der Cloud. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/write | Erstellt oder aktualisiert die Freigaben. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/delete | Löscht die Speicherkonto-Anmeldedaten. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/operationResults/read | Listet die Vorgangsergebnisse auf oder ruft sie ab |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | Listet die Speicherkonto-Anmeldedaten auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | Listet die Speicherkonto-Anmeldedaten auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/write | Erstellt oder aktualisiert die Speicherkonto-Anmeldedaten. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/delete | Löscht die Trigger. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/operationResults/read | Listet die Vorgangsergebnisse auf oder ruft sie ab |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/read | Listet die Trigger auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/read | Listet die Trigger auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/write | Erstellt oder aktualisiert die Trigger. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/updateSummary/read | Listet die Updatezusammenfassung auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/uploadCertificate/action | Dient zum Hochladen des Zertifikats für die Geräteregistrierung. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/delete | Löscht die Benutzer der Freigabe. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/operationResults/read | Listet die Vorgangsergebnisse auf oder ruft sie ab |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Listet die Benutzer der Freigabe auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Listet die Benutzer der Freigabe auf oder ruft sie ab. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/write | Erstellt oder aktualisiert die Benutzer der Freigabe. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Erstellt oder aktualisiert die Data Box Edge-Geräte. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Erstellt oder aktualisiert die Data Box Edge-Geräte. |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Databricks/locations/getNetworkPolicies/action | Ruft die Netzwerkzielrichtlinien für ein Subnetz basierend auf dem Standort der NRP ab |
> | Action | Microsoft.Databricks/locations/operationstatuses/read | Liest den Vorgangsstatus für die Ressource. |
> | Action | Microsoft.Databricks/operations/read | Ruft die Liste der Vorgänge ab. |
> | Action | Microsoft.Databricks/register/action | Führt die Registrierung bei Databricks aus. |
> | Action | Microsoft.Databricks/workspaces/delete | Entfernt einen Databricks-Arbeitsbereich. |
> | Action | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/read | Legt die verfügbaren Diagnoseeinstellungen für den Databricks-Arbeitsbereich fest. |
> | Action | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Action | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolldefinitionen für den Databricks-Arbeitsbereich ab. |
> | Action | Microsoft.Databricks/workspaces/read | Ruft eine Liste der Databricks-Arbeitsbereiche ab. |
> | Action | Microsoft.Databricks/workspaces/refreshPermissions/action | Aktualisiert Berechtigungen für einen Arbeitsbereich. |
> | Action | Microsoft.Databricks/workspaces/updateDenyAssignment/action | Aktualisiert die nicht zulässigen Aktionen für Ablehnungszuweisungen für eine verwaltete Ressourcengruppe eines Arbeitsbereichs. |
> | Action | Microsoft.Databricks/workspaces/virtualNetworkPeerings/delete | Löscht ein Peering virtueller Netzwerke. |
> | Action | Microsoft.Databricks/workspaces/virtualNetworkPeerings/read | Ruft das Peering virtueller Netzwerke ab. |
> | Action | Microsoft.Databricks/workspaces/virtualNetworkPeerings/write | Dient zum Hinzufügen oder Ändern des Peerings virtueller Netzwerke. |
> | Action | Microsoft.Databricks/workspaces/write | Erstellt einen Databricks-Arbeitsbereich. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DataCatalog/catalogs/delete | Löscht die catalogs-Ressource für den Data Catalog-Ressourcenanbieter |
> | Action | Microsoft.DataCatalog/catalogs/read | Liest die catalogs-Ressource für den Data Catalog-Ressourcenanbieter |
> | Action | Microsoft.DataCatalog/catalogs/write | Schreibt die catalogs-Ressource für den Data Catalog-Ressourcenanbieter |
> | Action | Microsoft.DataCatalog/checkNameAvailability/read | Überprüft die Verfügbarkeit des Katalognamens für den Data Catalog-Ressourcenanbieter |
> | Action | Microsoft.DataCatalog/datacatalogs/delete | Löscht die DataCatalog-Ressource für den Data Catalog-Ressourcenanbieter |
> | Action | Microsoft.DataCatalog/datacatalogs/read | Liest die DataCatalog-Ressource für den Data Catalog-Ressourcenanbieter |
> | Action | Microsoft.DataCatalog/datacatalogs/write | Schreibt die DataCatalog-Ressource für den Data Catalog-Ressourcenanbieter |
> | Action | Microsoft.DataCatalog/operations/read | Liest alle verfügbaren Vorgänge im Data Catalog-Ressourcenanbieter |
> | Action | Microsoft.DataCatalog/register/action | Registriert das Abonnement für den Data Catalog-Ressourcenanbieter |
> | Action | Microsoft.DataCatalog/unregister/action | Hebt die Registrierung des Abonnements für den Data Catalog-Ressourcenanbieter auf |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DataFactory/checkazuredatafactorynameavailability/read | Überprüft, ob der Data Factory-Name für die Nutzung verfügbar ist. |
> | Action | Microsoft.DataFactory/datafactories/activitywindows/read | Liest die Aktivitätsfenster in der Data Factory mit den angegebenen Parametern. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/activities/activitywindows/read | Liest die Aktivitätsfenster für die Pipelineaktivität mit den angegebenen Parametern. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/activitywindows/read | Liest die Aktivitätsfenster für die Pipeline mit den angegebenen Parametern. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/delete | Löscht alle Pipelines. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/pause/action | Hält alle Pipelines an. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/read | Liest alle Pipelines. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/resume/action | Setzt alle Pipelines fort. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/update/action | Aktualisiert alle Pipelines. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/write | Erstellt oder aktualisiert alle Pipelines. |
> | Action | Microsoft.DataFactory/datafactories/datasets/activitywindows/read | Liest die Aktivitätsfenster für das Dataset mit den angegebenen Parametern. |
> | Action | Microsoft.DataFactory/datafactories/datasets/delete | Löscht alle Datasets. |
> | Action | Microsoft.DataFactory/datafactories/datasets/read | Liest alle Datasets. |
> | Action | Microsoft.DataFactory/datafactories/datasets/sliceruns/read | Liest die Ausführung des Datenslices für ein angegebenes Dataset mit der angegebenen Startzeit. |
> | Action | Microsoft.DataFactory/datafactories/datasets/slices/read | Ruft die Datenslices im angegebenen Zeitraum ab. |
> | Action | Microsoft.DataFactory/datafactories/datasets/slices/write | Aktualisiert den Status des Datenslices. |
> | Action | Microsoft.DataFactory/datafactories/datasets/write | Erstellt oder aktualisiert alle Datasets. |
> | Action | Microsoft.DataFactory/datafactories/delete | Löscht die Data Factory. |
> | Action | Microsoft.DataFactory/datafactories/gateways/connectioninfo/action | Liest die Verbindungsinformationen für alle Gateways. |
> | Action | Microsoft.DataFactory/datafactories/gateways/delete | Löscht alle Gateways. |
> | Action | Microsoft.DataFactory/datafactories/gateways/listauthkeys/action | Führt die Authentifizierungsschlüssel für alle Gateways auf. |
> | Action | Microsoft.DataFactory/datafactories/gateways/read | Liest alle Gateways. |
> | Action | Microsoft.DataFactory/datafactories/gateways/regenerateauthkey/action | Generiert die Authentifizierungsschlüssel für alle Gateways neu. |
> | Action | Microsoft.DataFactory/datafactories/gateways/write | Erstellt oder aktualisiert alle Gateways. |
> | Action | Microsoft.DataFactory/datafactories/linkedServices/delete | Löscht alle verknüpften Dienste. |
> | Action | Microsoft.DataFactory/datafactories/linkedServices/read | Liest alle verknüpften Dienste. |
> | Action | Microsoft.DataFactory/datafactories/linkedServices/write | Erstellt oder aktualisiert alle verknüpften Dienste. |
> | Action | Microsoft.DataFactory/datafactories/read | Liest die Data Factory. |
> | Action | Microsoft.DataFactory/datafactories/runs/loginfo/read | Liest den SAS-URI für einen Blobcontainer, der die Protokolle enthält. |
> | Action | Microsoft.DataFactory/datafactories/tables/delete | Löscht alle Datasets. |
> | Action | Microsoft.DataFactory/datafactories/tables/read | Liest alle Datasets. |
> | Action | Microsoft.DataFactory/datafactories/tables/write | Erstellt oder aktualisiert alle Datasets. |
> | Action | Microsoft.DataFactory/datafactories/write | Erstellt oder aktualisiert die Data Factory. |
> | Action | Microsoft.DataFactory/factories/addDataFlowToDebugSession/action | Fügt der Debugsitzung den Datenfluss zu Vorschauzwecken hinzu. |
> | Action | Microsoft.DataFactory/factories/cancelpipelinerun/action | Bricht die Ausführung der Pipeline ab, die von der Ausführungs-ID angegeben wurde. |
> | Action | Microsoft.DataFactory/factories/cancelSandboxPipelineRun/action | Bricht einen Debuglauf für die Pipeline ab. |
> | Action | Microsoft.DataFactory/factories/createdataflowdebugsession/action | Erstellt eine Debugsitzung für einen Datenfluss. |
> | Action | Microsoft.DataFactory/factories/dataflows/delete | Löscht den Datenfluss. |
> | Action | Microsoft.DataFactory/factories/dataflows/read | Liest den Datenfluss. |
> | Action | Microsoft.DataFactory/factories/dataflows/write | Erstellen oder Aktualisieren des Datenflusses |
> | Action | Microsoft.DataFactory/factories/datasets/delete | Löscht alle Datasets. |
> | Action | Microsoft.DataFactory/factories/datasets/read | Liest alle Datasets. |
> | Action | Microsoft.DataFactory/factories/datasets/write | Erstellt oder aktualisiert alle Datasets. |
> | Action | Microsoft.DataFactory/factories/debugpipelineruns/cancel/action | Bricht einen Debuglauf für die Pipeline ab. |
> | Action | Microsoft.DataFactory/factories/delete | Löscht Data Factorys. |
> | Action | Microsoft.DataFactory/factories/deletedataflowdebugsession/action | Löscht eine Debugsitzung für einen Datenfluss. |
> | Action | Microsoft.DataFactory/factories/executeDataFlowDebugCommand/action | Führt den Debugbefehl für den Datenfluss aus. |
> | Action | Microsoft.DataFactory/factories/getDataPlaneAccess/action | Ruft den Zugriff auf den ADF-DataPlane-Dienst ab. |
> | Action | Microsoft.DataFactory/factories/getDataPlaneAccess/read | Liest den Zugriff auf den ADF-DataPlane-Dienst. |
> | Action | Microsoft.DataFactory/factories/getFeatureValue/action | Ruft für den jeweiligen Standort den Wert des Features für die Anzeigesteuerung ab. |
> | Action | Microsoft.DataFactory/factories/getFeatureValue/read | Liest für den jeweiligen Standort den Wert des Features für die Anzeigesteuerung. |
> | Action | Microsoft.DataFactory/factories/getGitHubAccessToken/action | Ruft ein GitHub-Zugriffstoken ab. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/delete | Löscht alle Integration Runtimes. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/getconnectioninfo/read | Liest Verbindungsinformationen für die Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/getObjectMetadata/action | Ruft die SSIS Integration Runtime-Metadaten für die angegebene Integration Runtime ab. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/getstatus/read | Liest den Status der Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/linkedIntegrationRuntime/action | Erstellt den verknüpften Integration Runtime-Verweis aus der freigegebenen Integration Runtime, die angegeben wurde. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/listauthkeys/read | Führt die Authentifizierungsschlüssel für alle Integration Runtimes auf. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/monitoringdata/read | Ruft die Überwachungsdaten für alle Integration Runtimes ab. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/delete | Löscht den Knoten für die angegebene Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/ipAddress/action | Gibt die IP-Adresse für den angegebenen Knoten der Integration Runtime zurück. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/read | Liest den Knoten für die angegebene Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/write | Aktualisiert einen selbstgehosteten Integration Runtime-Knoten. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/read | Liest alle Integration Runtimes. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/refreshObjectMetadata/action | Aktualisiert die SSIS Integration Runtime-Metadaten für die angegebene Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/regenerateauthkey/action | Generiert die Authentifizierungsschlüssel für die angegebene Integration Runtime neu. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/removelinks/action | Entfernt verknüpfte Integration Runtime-Verweise aus der angegebenen Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/start/action | Startet alle Integration Runtimes. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/stop/action | Beendet alle Integration Runtimes. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/synccredentials/action | Synchronisiert die Anmeldeinformationen für die angegebene Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/upgrade/action | Führt ein Upgrade für die angegebene Integration Runtime durch. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/write | Erstellt oder aktualisiert alle Integration Runtimes. |
> | Action | Microsoft.DataFactory/factories/linkedServices/delete | Löscht verknüpfte Dienste. |
> | Action | Microsoft.DataFactory/factories/linkedServices/read | Liest verknüpfte Dienste. |
> | Action | Microsoft.DataFactory/factories/linkedServices/write | Erstellt oder aktualisiert verknüpfte Dienste. |
> | Action | Microsoft.DataFactory/factories/operationResults/read | Ruft die Ergebnisse eines Vorgangs ab. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/activityruns/read | Liest die Ausführung von Aktivitäten für die angegebene Ausführungs-ID der Pipeline. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/cancel/action | Bricht die Ausführung der Pipeline ab, die von der Ausführungs-ID angegeben wurde. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/action | Fragt die Aktivitätsausführungen für die angegebene Ausführungs-ID der Pipeline ab. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/read | Liest das Ergebnis der Ausführung von Aktivitäten für die angegebene Ausführungs-ID der Pipeline. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/read | Liest die Pipelineausführungen. |
> | Action | Microsoft.DataFactory/factories/pipelines/createrun/action | Erstellt eine Ausführung für die Pipeline. |
> | Action | Microsoft.DataFactory/factories/pipelines/delete | Löscht Pipelines. |
> | Action | Microsoft.DataFactory/factories/pipelines/pipelineruns/activityruns/progress/read | Ruft den Fortschritt von Aktivitätsausführungen ab. |
> | Action | Microsoft.DataFactory/factories/pipelines/pipelineruns/read | Liest die Pipelineausführung. |
> | Action | Microsoft.DataFactory/factories/pipelines/read | Liest Pipelines. |
> | Action | Microsoft.DataFactory/factories/pipelines/sandbox/action | Erstellt eine Debuglaufumgebung für die Pipeline. |
> | Action | Microsoft.DataFactory/factories/pipelines/sandbox/create/action | Erstellt eine Debuglaufumgebung für die Pipeline. |
> | Action | Microsoft.DataFactory/factories/pipelines/sandbox/run/action | Erstellt einen Debuglauf für die Pipeline. |
> | Action | Microsoft.DataFactory/factories/pipelines/write | Dient zum Erstellen oder Aktualisieren von Pipelines. |
> | Action | Microsoft.DataFactory/factories/querydataflowdebugsessions/action | Fragt eine Debugsitzung für einen Datenfluss ab. |
> | Action | Microsoft.DataFactory/factories/querydebugpipelineruns/action | Fragt die Debugpipelineausführungen ab. |
> | Action | Microsoft.DataFactory/factories/querypipelineruns/action | Fragt die Pipelineausführungen ab. |
> | Action | Microsoft.DataFactory/factories/querypipelineruns/read | Liest das Ergebnis von Ausführungen der Abfragepipeline. |
> | Action | Microsoft.DataFactory/factories/querytriggerruns/action | Fragt die Triggerausführungen ab. |
> | Action | Microsoft.DataFactory/factories/querytriggerruns/read | Liest das Ergebnis von Triggerausführungen. |
> | Action | Microsoft.DataFactory/factories/read | Liest Data Factorys. |
> | Action | Microsoft.DataFactory/factories/sandboxpipelineruns/action | Fragt die Debugpipelineausführungen ab. |
> | Action | Microsoft.DataFactory/factories/sandboxpipelineruns/read | Ruft die Debuglaufinformationen für die Pipeline ab. |
> | Action | Microsoft.DataFactory/factories/sandboxpipelineruns/sandboxActivityRuns/read | Ruft die Debuglaufinformationen für die Aktivität ab. |
> | Action | Microsoft.DataFactory/factories/startdataflowdebugsession/action | Startet eine Debugsitzung für einen Datenfluss. |
> | Action | Microsoft.DataFactory/factories/triggerruns/read | Liest die Triggerausführungen. |
> | Action | Microsoft.DataFactory/factories/triggers/delete | Löscht alle Trigger. |
> | Action | Microsoft.DataFactory/factories/triggers/geteventsubscriptionstatus/action | Status des Ereignisabonnements |
> | Action | Microsoft.DataFactory/factories/triggers/read | Liest alle Trigger. |
> | Action | Microsoft.DataFactory/factories/triggers/start/action | Startet alle Trigger. |
> | Action | Microsoft.DataFactory/factories/triggers/stop/action | Beendet alle Trigger. |
> | Action | Microsoft.DataFactory/factories/triggers/subscribetoevents/action | Abonniert Ereignisse. |
> | Action | Microsoft.DataFactory/factories/triggers/triggerruns/read | Liest die Triggerausführungen. |
> | Action | Microsoft.DataFactory/factories/triggers/unsubscribefromevents/action | Dient zum Kündigen des Abonnements von Ereignissen. |
> | Action | Microsoft.DataFactory/factories/triggers/write | Erstellt oder aktualisiert alle Trigger. |
> | Action | Microsoft.DataFactory/factories/write | Dient zum Erstellen oder Aktualisieren von Data Factorys. |
> | Action | Microsoft.DataFactory/locations/configureFactoryRepo/action | Konfiguriert das Repository für die Factory. |
> | Action | Microsoft.DataFactory/locations/getFeatureValue/action | Ruft für den jeweiligen Standort den Wert des Features für die Anzeigesteuerung ab. |
> | Action | Microsoft.DataFactory/locations/getFeatureValue/read | Liest für den jeweiligen Standort den Wert des Features für die Anzeigesteuerung. |
> | Action | Microsoft.DataFactory/operations/read | Liest alle Vorgänge im Microsoft Data Factory-Anbieter. |
> | Action | Microsoft.DataFactory/register/action | Registriert das Abonnement für den Data Factory-Ressourcenanbieter. |
> | Action | Microsoft.DataFactory/unregister/action | Hebt die Registrierung des Abonnements für den Data Factory-Ressourcenanbieter auf. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DataLakeAnalytics/accounts/computePolicies/delete | Löscht eine Computerichtlinie. |
> | Action | Microsoft.DataLakeAnalytics/accounts/computePolicies/read | Ruft Informationen zu einer Computerichtlinie ab. |
> | Action | Microsoft.DataLakeAnalytics/accounts/computePolicies/write | Erstellt oder aktualisiert eine Computerichtlinie. |
> | Action | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/delete | Hebt die Verknüpfung eines DataLakeStore-Kontos mit einem DataLakeAnalytics-Konto auf. |
> | Action | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/read | Ruft Informationen zu einem verknüpften DataLakeStore-Konto eines DataLakeAnalytics-Kontos ab. |
> | Action | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/write | Erstellt oder aktualisiert ein verknüpftes DataLakeStore-Konto eines DataLakeAnalytics-Kontos. |
> | Action | Microsoft.DataLakeAnalytics/accounts/delete | Löscht ein DataLakeAnalytics-Konto. |
> | Action | Microsoft.DataLakeAnalytics/accounts/firewallRules/delete | Dient zum Löschen einer Firewallregel. |
> | Action | Microsoft.DataLakeAnalytics/accounts/firewallRules/read | Dient zum Abrufen von Informationen zu einer Firewallregel. |
> | Action | Microsoft.DataLakeAnalytics/accounts/firewallRules/write | Dient zum Erstellen oder Aktualisieren einer Firewallregel. |
> | Action | Microsoft.DataLakeAnalytics/accounts/operationResults/read | Ruft das Ergebnis eines Vorgangs des DataLakeAnalytics-Kontos ab. |
> | Action | Microsoft.DataLakeAnalytics/accounts/read | Ruft Informationen zu einem vorhandenen DataLakeAnalytics-Konto ab. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/listSasTokens/action | Listet SAS-Tokens für Speichercontainer eines verknüpften Speicherkontos eines DataLakeAnalytics-Kontos auf. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/read | Ruft Container eines verknüpften Speicherkontos eines DataLakeAnalytics-Kontos ab. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/delete | Hebt die Verknüpfung eines Speicherkontos mit einem DataLakeAnalytics-Konto auf. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/read | Ruft Informationen zu einem verknüpften Speicherkonto eines DataLakeAnalytics-Kontos ab. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/write | Erstellt oder aktualisiert ein verknüpftes Speicherkonto eines DataLakeAnalytics-Kontos. |
> | Action | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Erteilt Berechtigungen zum Abbrechen von Aufträgen, die von anderen Benutzern übermittelt wurden. |
> | Action | Microsoft.DataLakeAnalytics/accounts/transferAnalyticsUnits/action | Überträgt SystemMaxAnalyticsUnits zwischen DataLakeAnalytics-Konten. |
> | Action | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/delete | Dient zum Löschen einer VNET-Regel. |
> | Action | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/read | Hiermit rufen Sie Informationen zu einer VNET-Regel ab. |
> | Action | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/write | Dient zum Erstellen oder Aktualisieren einer VNET-Regel. |
> | Action | Microsoft.DataLakeAnalytics/accounts/write | Erstellt oder aktualisiert ein DataLakeAnalytics-Konto. |
> | Action | Microsoft.DataLakeAnalytics/locations/capability/read | Ruft Funktionsinformationen eines Abonnements bezüglich der Verwendung von DataLakeAnalytics ab. |
> | Action | Microsoft.DataLakeAnalytics/locations/checkNameAvailability/action | Überprüft die Verfügbarkeit eines DataLakeAnalytics-Kontonamens. |
> | Action | Microsoft.DataLakeAnalytics/locations/operationResults/read | Ruft das Ergebnis eines Vorgangs des DataLakeAnalytics-Kontos ab. |
> | Action | Microsoft.DataLakeAnalytics/locations/usages/read | Ruft die Informationen zum Kontingentbedarf eines Abonnements bezüglich der Verwendung von DataLakeAnalytics ab. |
> | Action | Microsoft.DataLakeAnalytics/operations/read | Ruft verfügbare Vorgänge von DataLakeAnalytics ab. |
> | Action | Microsoft.DataLakeAnalytics/register/action | Registriert ein Abonnement für DataLakeAnalytics. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DataLakeStore/accounts/delete | Löscht ein DataLakeStore-Konto. |
> | Action | Microsoft.DataLakeStore/accounts/enableKeyVault/action | Aktiviert KeyVault für ein DataLakeStore-Konto. |
> | Action | Microsoft.DataLakeStore/accounts/eventGridFilters/delete | Hiermit löschen Sie einen EventGrid-Filter. |
> | Action | Microsoft.DataLakeStore/accounts/eventGridFilters/read | Hiermit rufen Sie einen EventGrid-Filter ab. |
> | Action | Microsoft.DataLakeStore/accounts/eventGridFilters/write | Hiermit erstellen oder aktualisieren Sie einen Event Grid-Filter. |
> | Action | Microsoft.DataLakeStore/accounts/firewallRules/delete | Dient zum Löschen einer Firewallregel. |
> | Action | Microsoft.DataLakeStore/accounts/firewallRules/read | Dient zum Abrufen von Informationen zu einer Firewallregel. |
> | Action | Microsoft.DataLakeStore/accounts/firewallRules/write | Dient zum Erstellen oder Aktualisieren einer Firewallregel. |
> | Action | Microsoft.DataLakeStore/accounts/operationResults/read | Ruft das Ergebnis eines Vorgangs des DataLakeStore-Kontos ab. |
> | Action | Microsoft.DataLakeStore/accounts/read | Ruft Informationen zu einem vorhandenen DataLakeStore-Konto ab. |
> | Action | Microsoft.DataLakeStore/accounts/shares/delete | Löschen einer Freigabe. |
> | Action | Microsoft.DataLakeStore/accounts/shares/read | Abrufen von Informationen zu einer Freigabe. |
> | Action | Microsoft.DataLakeStore/accounts/shares/write | Erstellen oder Aktualisieren einer Freigabe. |
> | Action | Microsoft.DataLakeStore/accounts/Superuser/action | Erteilt Superuser-Berechtigungen für Data Lake Store, sofern diese über „Microsoft.Authorization/roleAssignments/write“ erteilt werden. |
> | Action | Microsoft.DataLakeStore/accounts/trustedIdProviders/delete | Dient zum Löschen eines vertrauenswürdigen Identitätsanbieters. |
> | Action | Microsoft.DataLakeStore/accounts/trustedIdProviders/read | Dient zum Abrufen von Informationen zu einem vertrauenswürdigen Identitätsanbieter. |
> | Action | Microsoft.DataLakeStore/accounts/trustedIdProviders/write | Dient zum Erstellen oder Aktualisieren eines vertrauenswürdigen Identitätsanbieters. |
> | Action | Microsoft.DataLakeStore/accounts/virtualNetworkRules/delete | Dient zum Löschen einer VNET-Regel. |
> | Action | Microsoft.DataLakeStore/accounts/virtualNetworkRules/read | Hiermit rufen Sie Informationen zu einer VNET-Regel ab. |
> | Action | Microsoft.DataLakeStore/accounts/virtualNetworkRules/write | Dient zum Erstellen oder Aktualisieren einer VNET-Regel. |
> | Action | Microsoft.DataLakeStore/accounts/write | Erstellt oder aktualisiert ein DataLakeStore-Konto. |
> | Action | Microsoft.DataLakeStore/locations/capability/read | Ruft Funktionsinformationen eines Abonnements bezüglich der Verwendung von DataLakeStore ab. |
> | Action | Microsoft.DataLakeStore/locations/checkNameAvailability/action | Überprüft die Verfügbarkeit eines DataLakeStore-Kontonamens. |
> | Action | Microsoft.DataLakeStore/locations/operationResults/read | Ruft das Ergebnis eines Vorgangs des DataLakeStore-Kontos ab. |
> | Action | Microsoft.DataLakeStore/locations/usages/read | Ruft die Informationen zum Kontingentbedarf eines Abonnements bezüglich der Verwendung von DataLakeStore ab. |
> | Action | Microsoft.DataLakeStore/operations/read | Ruft verfügbare Vorgänge von DataLakeStore ab. |
> | Action | Microsoft.DataLakeStore/register/action | Registriert ein Abonnement für DataLakeStore. |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DataMigration/locations/operationResults/read | Hiermit rufen Sie den Status eines Vorgangs mit langer Ausführungsdauer in Zusammenhang mit einer Antwort "202: Akzeptiert" ab. |
> | Action | Microsoft.DataMigration/locations/operationStatuses/read | Hiermit rufen Sie den Status eines Vorgangs mit langer Ausführungsdauer in Zusammenhang mit einer Antwort "202: Akzeptiert" ab. |
> | Action | Microsoft.DataMigration/register/action | Registriert das Abonnement beim Azure Database Migration Service-Anbieter registriert. |
> | Action | Microsoft.DataMigration/services/addWorker/action | Fügt den verfügbaren Workern des Diensts einen DMS-Worker hinzu. |
> | Action | Microsoft.DataMigration/services/checkStatus/action | Hiermit überprüfen Sie, ob der Dienst bereitgestellt wurde und ausgeführt wird. |
> | Action | Microsoft.DataMigration/services/configureWorker/action | Konfiguriert einen DMS-Worker für die verfügbaren Worker des Diensts. |
> | Action | Microsoft.DataMigration/services/delete | Löscht eine Ressource und alle zugehörigen untergeordneten Elemente. |
> | Action | Microsoft.DataMigration/services/projects/accessArtifacts/action | Generiert eine URL, die für GET- oder PUT-Vorgänge für Projektartefakte verwendet werden kann. |
> | Action | Microsoft.DataMigration/services/projects/delete | Löscht eine Ressource und alle zugehörigen untergeordneten Elemente. |
> | Action | Microsoft.DataMigration/services/projects/files/delete | Löscht eine Ressource und alle zugehörigen untergeordneten Elemente. |
> | Action | Microsoft.DataMigration/services/projects/files/read | Hiermit lesen Sie Informationen zu Ressourcen. |
> | Action | Microsoft.DataMigration/services/projects/files/read/action | Hiermit wird eine URL abgerufen, die zum Lesen des Dateiinhalts verwendet werden kann. |
> | Action | Microsoft.DataMigration/services/projects/files/readWrite/action | Hiermit wird eine URL abgerufen, die zum Lesen oder Schreiben des Dateiinhalts verwendet werden kann. |
> | Action | Microsoft.DataMigration/services/projects/files/write | Hiermit erstellen oder aktualisieren Sie Ressourcen und ihre Eigenschaften. |
> | Action | Microsoft.DataMigration/services/projects/read | Hiermit lesen Sie Informationen zu Ressourcen. |
> | Action | Microsoft.DataMigration/services/projects/tasks/cancel/action | Hiermit brechen Sie die Aufgabe ab, wenn sie zurzeit ausgeführt wird. |
> | Action | Microsoft.DataMigration/services/projects/tasks/delete | Löscht eine Ressource und alle zugehörigen untergeordneten Elemente. |
> | Action | Microsoft.DataMigration/services/projects/tasks/read | Hiermit lesen Sie Informationen zu Ressourcen. |
> | Action | Microsoft.DataMigration/services/projects/tasks/write | Hiermit führen Sie Azure Database Migration Service-Aufgaben aus. |
> | Action | Microsoft.DataMigration/services/projects/write | Hiermit führen Sie Azure Database Migration Service-Aufgaben aus. |
> | Action | Microsoft.DataMigration/services/read | Hiermit lesen Sie Informationen zu Ressourcen. |
> | Action | Microsoft.DataMigration/services/removeWorker/action | Entfernt einen DMS-Worker aus den verfügbaren Workern des Diensts. |
> | Action | Microsoft.DataMigration/services/serviceTasks/cancel/action | Hiermit brechen Sie die Aufgabe ab, wenn sie zurzeit ausgeführt wird. |
> | Action | Microsoft.DataMigration/services/serviceTasks/delete | Löscht eine Ressource und alle zugehörigen untergeordneten Elemente. |
> | Action | Microsoft.DataMigration/services/serviceTasks/read | Hiermit lesen Sie Informationen zu Ressourcen. |
> | Action | Microsoft.DataMigration/services/serviceTasks/write | Hiermit führen Sie Azure Database Migration Service-Aufgaben aus. |
> | Action | Microsoft.DataMigration/services/slots/delete | Löscht eine Ressource und alle zugehörigen untergeordneten Elemente. |
> | Action | Microsoft.DataMigration/services/slots/read | Hiermit lesen Sie Informationen zu Ressourcen. |
> | Action | Microsoft.DataMigration/services/slots/write | Hiermit erstellen oder aktualisieren Sie Ressourcen und ihre Eigenschaften. |
> | Action | Microsoft.DataMigration/services/start/action | Hiermit starten Sie den DMS-Dienst, um das erneute Verarbeiten von Migrationen zuzulassen. |
> | Action | Microsoft.DataMigration/services/stop/action | Hiermit beenden Sie den DMS-Dienst, um seine Kosten zu minimieren. |
> | Action | Microsoft.DataMigration/services/updateAgentConfig/action | Aktualisiert die Konfiguration des DMS-Agents mit den angegebenen Werten |
> | Action | Microsoft.DataMigration/services/write | Hiermit erstellen oder aktualisieren Sie Ressourcen und ihre Eigenschaften. |
> | Action | Microsoft.DataMigration/skus/read | Hiermit rufen Sie eine Liste der von den DMS-Ressourcen unterstützten SKUs ab. |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DBforMariaDB/checkNameAvailability/action | Überprüfen Sie, ob der angegebene Servername für die weltweite Bereitstellung bei einem bestimmten Abonnement verfügbar ist. |
> | Action | Microsoft.DBforMariaDB/locations/azureAsyncOperation/read | Gibt die Ergebnisse von MariaDB-Servervorgängen zurück |
> | Action | Microsoft.DBforMariaDB/locations/operationResults/read | Gibt die ResourceGroup basierend auf den Ergebnissen des MariaDB-Servervorgangs zurück |
> | Action | Microsoft.DBforMariaDB/locations/operationResults/read | Gibt die Ergebnisse von MariaDB-Servervorgängen zurück |
> | Action | Microsoft.DBforMariaDB/locations/performanceTiers/read | Gibt die Liste der verfügbaren Leistungsstufen zurück. |
> | Action | Microsoft.DBforMariaDB/locations/privateEndpointConnectionAzureAsyncOperation/read | Ruft das Ergebnis für einen Vorgang auf der Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMariaDB/locations/privateEndpointConnectionOperationResults/read | Ruft das Ergebnis für einen Vorgang auf der Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMariaDB/locations/privateEndpointConnectionProxyAzureAsyncOperation/read | Ruft das Ergebnis für einen Vorgang auf dem Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMariaDB/locations/privateEndpointConnectionProxyOperationResults/read | Ruft das Ergebnis für einen Vorgang auf dem Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMariaDB/locations/securityAlertPoliciesAzureAsyncOperation/read | Gibt die Liste der Vorgangsergebnisse der Bedrohungserkennung auf dem Server zurück |
> | Action | Microsoft.DBforMariaDB/locations/securityAlertPoliciesOperationResults/read | Gibt die Liste der Vorgangsergebnisse der Bedrohungserkennung auf dem Server zurück |
> | Action | Microsoft.DBforMariaDB/locations/serverKeyAzureAsyncOperation/read | Ruft gegenwärtig ausgeführte Vorgänge für Serverschlüssel von Transparent Data Encryption ab. |
> | Action | Microsoft.DBforMariaDB/locations/serverKeyOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für Serverschlüssel von Transparent Data Encryption ab. |
> | Action | Microsoft.DBforMariaDB/operations/read | Gibt die Liste der MariaDB-Vorgänge zurück |
> | Action | Microsoft.DBforMariaDB/performanceTiers/read | Gibt die Liste der verfügbaren Leistungsstufen zurück. |
> | Action | Microsoft.DBforMariaDB/register/action | Registriert einen MariaDB-Ressourcenanbieter |
> | Action | Microsoft.DBforMariaDB/servers/administrators/delete | Löscht einen vorhandenen Administrator eines MariaDB-Servers |
> | Action | Microsoft.DBforMariaDB/servers/administrators/read | Ruft eine Liste der MariaDB-Serveradministratoren ab |
> | Action | Microsoft.DBforMariaDB/servers/administrators/write | Erstellt oder aktualisiert den MariaDB-Serveradministrator mit den angegebenen Parametern |
> | Action | Microsoft.DBforMariaDB/servers/advisors/createRecommendedActionSession/action | Erstellt eine neue Empfehlungsmaßnahmensitzung |
> | Action | Microsoft.DBforMariaDB/servers/advisors/read | Gibt die Liste mit den Ratgebern zurück. |
> | Action | Microsoft.DBforMariaDB/servers/advisors/read | Gibt einen Advisor zurück |
> | Action | Microsoft.DBforMariaDB/servers/advisors/recommendedActions/read | Gibt die Liste der empfohlenen Aktionen zurück. |
> | Action | Microsoft.DBforMariaDB/servers/advisors/recommendedActions/read | Gibt die Liste der empfohlenen Aktionen zurück. |
> | Action | Microsoft.DBforMariaDB/servers/advisors/recommendedActions/read | Gibt eine empfohlene Maßnahme zurück |
> | Action | Microsoft.DBforMariaDB/servers/configurations/read | Gibt die Liste mit den Konfigurationen für einen Server zurück oder ruft die Eigenschaften für die angegebene Konfiguration ab. |
> | Action | Microsoft.DBforMariaDB/servers/configurations/write | Aktualisiert den Wert für die angegebene Konfiguration. |
> | Action | Microsoft.DBforMariaDB/servers/databases/delete | Löscht eine vorhandene MariaDB-Datenbank |
> | Action | Microsoft.DBforMariaDB/servers/databases/read | Gibt die Liste der MariaDB-Datenbanken zurück oder ruft die Eigenschaften für die angegebene Datenbank ab |
> | Action | Microsoft.DBforMariaDB/servers/databases/write | Erstellt eine MariaDB-Datenbank mit den angegebenen Parametern oder aktualisiert die Eigenschaften für die angegebene Datenbank |
> | Action | Microsoft.DBforMariaDB/servers/delete | Löscht einen vorhandenen Server. |
> | Action | Microsoft.DBforMariaDB/servers/firewallRules/delete | Löscht eine vorhandene Firewallregel. |
> | Action | Microsoft.DBforMariaDB/servers/firewallRules/read | Gibt die Liste der Firewallregeln für einen Server zurück oder ruft die Eigenschaften für die angegebene Firewallregel ab. |
> | Action | Microsoft.DBforMariaDB/servers/firewallRules/write | Erstellt eine Firewallregel mit den angegebenen Parametern oder aktualisiert eine vorhandene Regel. |
> | Action | Microsoft.DBforMariaDB/servers/keys/delete | Löscht einen vorhandenen Serverschlüssel. |
> | Action | Microsoft.DBforMariaDB/servers/keys/read | Gibt die Liste der Serverschlüssel zurück oder ruft die Eigenschaften für den angegebenen Serverschlüssel ab. |
> | Action | Microsoft.DBforMariaDB/servers/keys/write | Erstellt einen Schlüssel mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Serverschlüssel. |
> | Action | Microsoft.DBforMariaDB/servers/logFiles/read | Gibt die Liste der MariaDB-Protokolldateien zurück |
> | Action | Microsoft.DBforMariaDB/servers/privateEndpointConnectionProxies/delete | Löscht einen Verbindungsproxy eines privaten Endpunkts |
> | Action | Microsoft.DBforMariaDB/servers/privateEndpointConnectionProxies/read | Gibt eine Liste der Verbindungsproxys eines privaten Endpunkts zurück oder ruft die Eigenschaften für den angegebenen Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMariaDB/servers/privateEndpointConnectionProxies/validate/action | Überprüft den Aufruf zum Erstellen einer Verbindung mit einem privaten Endpunkt auf NRP-Seite |
> | Action | Microsoft.DBforMariaDB/servers/privateEndpointConnectionProxies/write | Erstellt einen Verbindungsproxy eines privaten Endpunkts mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für den angegebenen Verbindungsproxy eines privaten Endpunkts |
> | Action | Microsoft.DBforMariaDB/servers/privateEndpointConnections/delete | Löscht eine Verbindung mit einem privaten Endpunkt |
> | Action | Microsoft.DBforMariaDB/servers/privateEndpointConnections/read | Gibt eine Liste der Verbindungen mit einem privaten Endpunkt zurück oder ruft die Eigenschaften für die angegebene Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMariaDB/servers/privateEndpointConnections/write | Genehmigt eine vorhandene Verbindung mit einem privaten Endpunkt, oder lehnt diese ab |
> | Action | Microsoft.DBforMariaDB/servers/privateLinkResources/read | Ruft die Ressourcen der privaten Verbindung für den entsprechenden MariaDB-Server ab. |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für MariaDB-Server ab. |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken. |
> | Action | Microsoft.DBforMariaDB/servers/queryTexts/action | Gibt die Texte für eine Liste von Abfragen zurück. |
> | Action | Microsoft.DBforMariaDB/servers/queryTexts/action | Zurückgeben des Texts einer Abfrage |
> | Action | Microsoft.DBforMariaDB/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Action | Microsoft.DBforMariaDB/servers/recoverableServers/read | Gibt die wiederherstellbaren MariaDB Server-Informationen zurück. |
> | Action | Microsoft.DBforMariaDB/servers/replicas/read | Ruft Lesereplikate eines MariaDB-Servers ab |
> | Action | Microsoft.DBforMariaDB/servers/restart/action | Startet einen bestimmten Server neu |
> | Action | Microsoft.DBforMariaDB/servers/securityAlertPolicies/read | Dient zum Abrufen von Details zur Richtlinie für die Serverbedrohungserkennung, die für einen bestimmten Server konfiguriert ist. |
> | Action | Microsoft.DBforMariaDB/servers/securityAlertPolicies/write | Dient zum Ändern der Richtlinie für die Serverbedrohungserkennung für einen bestimmten Server. |
> | Action | Microsoft.DBforMariaDB/servers/topQueryStatistics/read | Gibt die Liste mit der Abfragestatistik für die obersten Abfragen zurück. |
> | Action | Microsoft.DBforMariaDB/servers/topQueryStatistics/read | Gibt eine Abfragestatistik zurück |
> | Action | Microsoft.DBforMariaDB/servers/updateConfigurations/action | Aktualisiert Konfigurationen für den angegebenen Server. |
> | Action | Microsoft.DBforMariaDB/servers/virtualNetworkRules/delete | Löscht eine vorhandene VNET-Regel. |
> | Action | Microsoft.DBforMariaDB/servers/virtualNetworkRules/read | Gibt die Liste der VNET-Regeln zurück oder ruft die Eigenschaften für die angegebene VNET-Regel ab. |
> | Action | Microsoft.DBforMariaDB/servers/virtualNetworkRules/write | Erstellt eine VNET-Regel mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene VNET-Regel. |
> | Action | Microsoft.DBforMariaDB/servers/waitStatistics/read | Gibt Wartestatistiken für eine Instanz zurück. |
> | Action | Microsoft.DBforMariaDB/servers/waitStatistics/read | Gibt eine Wartezeitstatistik zurück |
> | Action | Microsoft.DBforMariaDB/servers/write | Erstellt einen Server mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Server. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DBforMySQL/checkNameAvailability/action | Überprüfen Sie, ob der angegebene Servername für die weltweite Bereitstellung bei einem bestimmten Abonnement verfügbar ist. |
> | Action | Microsoft.DBforMySQL/locations/azureAsyncOperation/read | Gibt die Ergebnisse von MySQL-Servervorgängen zurück |
> | Action | Microsoft.DBforMySQL/locations/operationResults/read | Gibt die ResourceGroup basierend auf den Ergebnissen des MySQL-Servervorgangs zurück |
> | Action | Microsoft.DBforMySQL/locations/operationResults/read | Gibt die Ergebnisse von MySQL-Servervorgängen zurück |
> | Action | Microsoft.DBforMySQL/locations/performanceTiers/read | Gibt die Liste der verfügbaren Leistungsstufen zurück. |
> | Action | Microsoft.DBforMySQL/locations/privateEndpointConnectionAzureAsyncOperation/read | Ruft das Ergebnis für einen Vorgang auf der Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMySQL/locations/privateEndpointConnectionOperationResults/read | Ruft das Ergebnis für einen Vorgang auf der Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMySQL/locations/privateEndpointConnectionProxyAzureAsyncOperation/read | Ruft das Ergebnis für einen Vorgang auf dem Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMySQL/locations/privateEndpointConnectionProxyOperationResults/read | Ruft das Ergebnis für einen Vorgang auf dem Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMySQL/locations/securityAlertPoliciesAzureAsyncOperation/read | Gibt die Liste der Vorgangsergebnisse der Bedrohungserkennung auf dem Server zurück |
> | Action | Microsoft.DBforMySQL/locations/securityAlertPoliciesOperationResults/read | Gibt die Liste der Vorgangsergebnisse der Bedrohungserkennung auf dem Server zurück |
> | Action | Microsoft.DBforMySQL/locations/serverKeyAzureAsyncOperation/read | Ruft gegenwärtig ausgeführte Vorgänge für Serverschlüssel von Transparent Data Encryption ab. |
> | Action | Microsoft.DBforMySQL/locations/serverKeyOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für Serverschlüssel von Transparent Data Encryption ab. |
> | Action | Microsoft.DBforMySQL/operations/read | Gibt die Liste der MySQL-Vorgänge zurück |
> | Action | Microsoft.DBforMySQL/performanceTiers/read | Gibt die Liste der verfügbaren Leistungsstufen zurück. |
> | Action | Microsoft.DBforMySQL/register/action | Registriert den MySQL-Ressourcenanbieter |
> | Action | Microsoft.DBforMySQL/servers/administrators/delete | Löscht einen vorhandenen Administrator eines MySQL-Servers |
> | Action | Microsoft.DBforMySQL/servers/administrators/read | Ruft eine Liste der MySQL-Serveradministratoren ab |
> | Action | Microsoft.DBforMySQL/servers/administrators/write | Erstellt oder aktualisiert den MySQL-Serveradministrator mit den angegebenen Parametern |
> | Action | Microsoft.DBforMySQL/servers/advisors/createRecommendedActionSession/action | Erstellt eine neue Empfehlungsmaßnahmensitzung |
> | Action | Microsoft.DBforMySQL/servers/advisors/read | Gibt die Liste mit den Ratgebern zurück. |
> | Action | Microsoft.DBforMySQL/servers/advisors/read | Gibt einen Advisor zurück |
> | Action | Microsoft.DBforMySQL/servers/advisors/recommendedActions/read | Gibt die Liste der empfohlenen Aktionen zurück. |
> | Action | Microsoft.DBforMySQL/servers/advisors/recommendedActions/read | Gibt die Liste der empfohlenen Aktionen zurück. |
> | Action | Microsoft.DBforMySQL/servers/advisors/recommendedActions/read | Gibt eine empfohlene Maßnahme zurück |
> | Action | Microsoft.DBforMySQL/servers/configurations/read | Gibt die Liste mit den Konfigurationen für einen Server zurück oder ruft die Eigenschaften für die angegebene Konfiguration ab. |
> | Action | Microsoft.DBforMySQL/servers/configurations/write | Aktualisiert den Wert für die angegebene Konfiguration. |
> | Action | Microsoft.DBforMySQL/servers/databases/delete | Löscht eine vorhandene MySQL-Datenbank |
> | Action | Microsoft.DBforMySQL/servers/databases/read | Gibt die Liste der MySQL-Datenbanken zurück oder ruft die Eigenschaften für die angegebene Datenbank ab |
> | Action | Microsoft.DBforMySQL/servers/databases/write | Erstellt eine MySQL-Datenbank mit den angegebenen Parametern oder aktualisiert die Eigenschaften für die angegebene Datenbank |
> | Action | Microsoft.DBforMySQL/servers/delete | Löscht einen vorhandenen Server. |
> | Action | Microsoft.DBforMySQL/servers/firewallRules/delete | Löscht eine vorhandene Firewallregel. |
> | Action | Microsoft.DBforMySQL/servers/firewallRules/read | Gibt die Liste der Firewallregeln für einen Server zurück oder ruft die Eigenschaften für die angegebene Firewallregel ab. |
> | Action | Microsoft.DBforMySQL/servers/firewallRules/write | Erstellt eine Firewallregel mit den angegebenen Parametern oder aktualisiert eine vorhandene Regel. |
> | Action | Microsoft.DBforMySQL/servers/keys/delete | Löscht einen vorhandenen Serverschlüssel. |
> | Action | Microsoft.DBforMySQL/servers/keys/read | Gibt die Liste der Serverschlüssel zurück oder ruft die Eigenschaften für den angegebenen Serverschlüssel ab. |
> | Action | Microsoft.DBforMySQL/servers/keys/write | Erstellt einen Schlüssel mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Serverschlüssel. |
> | Action | Microsoft.DBforMySQL/servers/logFiles/read | Gibt die Liste mit MySQL LogFiles zurück. |
> | Action | Microsoft.DBforMySQL/servers/privateEndpointConnectionProxies/delete | Löscht einen Verbindungsproxy eines privaten Endpunkts |
> | Action | Microsoft.DBforMySQL/servers/privateEndpointConnectionProxies/read | Gibt eine Liste der Verbindungsproxys eines privaten Endpunkts zurück oder ruft die Eigenschaften für den angegebenen Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMySQL/servers/privateEndpointConnectionProxies/validate/action | Überprüft den Aufruf zum Erstellen einer Verbindung mit einem privaten Endpunkt auf NRP-Seite |
> | Action | Microsoft.DBforMySQL/servers/privateEndpointConnectionProxies/write | Erstellt einen Verbindungsproxy eines privaten Endpunkts mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für den angegebenen Verbindungsproxy eines privaten Endpunkts |
> | Action | Microsoft.DBforMySQL/servers/privateEndpointConnections/delete | Löscht eine Verbindung mit einem privaten Endpunkt |
> | Action | Microsoft.DBforMySQL/servers/privateEndpointConnections/read | Gibt eine Liste der Verbindungen mit einem privaten Endpunkt zurück oder ruft die Eigenschaften für die angegebene Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.DBforMySQL/servers/privateEndpointConnections/write | Genehmigt eine vorhandene Verbindung mit einem privaten Endpunkt, oder lehnt diese ab |
> | Action | Microsoft.DBforMySQL/servers/privateLinkResources/read | Ruft die Ressourcen der privaten Verbindung für den entsprechenden MySQL-Server ab. |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für MySQL-Server ab. |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken. |
> | Action | Microsoft.DBforMySQL/servers/queryTexts/action | Gibt die Texte für eine Liste von Abfragen zurück. |
> | Action | Microsoft.DBforMySQL/servers/queryTexts/action | Zurückgeben des Texts einer Abfrage |
> | Action | Microsoft.DBforMySQL/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Action | Microsoft.DBforMySQL/servers/recoverableServers/read | Gibt die wiederherstellbaren MySQL Server-Informationen zurück. |
> | Action | Microsoft.DBforMySQL/servers/replicas/read | Ruft Lesereplikate eines MySQL-Servers ab |
> | Action | Microsoft.DBforMySQL/servers/restart/action | Startet einen bestimmten Server neu |
> | Action | Microsoft.DBforMySQL/servers/securityAlertPolicies/read | Dient zum Abrufen von Details zur Richtlinie für die Serverbedrohungserkennung, die für einen bestimmten Server konfiguriert ist. |
> | Action | Microsoft.DBforMySQL/servers/securityAlertPolicies/write | Dient zum Ändern der Richtlinie für die Serverbedrohungserkennung für einen bestimmten Server. |
> | Action | Microsoft.DBforMySQL/servers/topQueryStatistics/read | Gibt die Liste mit der Abfragestatistik für die obersten Abfragen zurück. |
> | Action | Microsoft.DBforMySQL/servers/topQueryStatistics/read | Gibt eine Abfragestatistik zurück |
> | Action | Microsoft.DBforMySQL/servers/updateConfigurations/action | Aktualisiert Konfigurationen für den angegebenen Server. |
> | Action | Microsoft.DBforMySQL/servers/virtualNetworkRules/delete | Löscht eine vorhandene VNET-Regel. |
> | Action | Microsoft.DBforMySQL/servers/virtualNetworkRules/read | Gibt die Liste der VNET-Regeln zurück oder ruft die Eigenschaften für die angegebene VNET-Regel ab. |
> | Action | Microsoft.DBforMySQL/servers/virtualNetworkRules/write | Erstellt eine VNET-Regel mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene VNET-Regel. |
> | Action | Microsoft.DBforMySQL/servers/waitStatistics/read | Gibt Wartestatistiken für eine Instanz zurück. |
> | Action | Microsoft.DBforMySQL/servers/waitStatistics/read | Gibt eine Wartezeitstatistik zurück |
> | Action | Microsoft.DBforMySQL/servers/write | Erstellt einen Server mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Server. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DBforPostgreSQL/checkNameAvailability/action | Überprüfen Sie, ob der angegebene Servername für die weltweite Bereitstellung bei einem bestimmten Abonnement verfügbar ist. |
> | Action | Microsoft.DBforPostgreSQL/locations/azureAsyncOperation/read | Gibt die Ergebnisse von PostgreSQL-Servervorgängen zurück |
> | Action | Microsoft.DBforPostgreSQL/locations/operationResults/read | Gibt die ResourceGroup basierend auf den Ergebnissen des PostgreSQL-Servervorgangs zurück |
> | Action | Microsoft.DBforPostgreSQL/locations/operationResults/read | Gibt die Ergebnisse von PostgreSQL-Servervorgängen zurück |
> | Action | Microsoft.DBforPostgreSQL/locations/performanceTiers/read | Gibt die Liste der verfügbaren Leistungsstufen zurück. |
> | Action | Microsoft.DBforPostgreSQL/locations/privateEndpointConnectionAzureAsyncOperation/read | Ruft das Ergebnis für einen Vorgang auf der Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.DBforPostgreSQL/locations/privateEndpointConnectionOperationResults/read | Ruft das Ergebnis für einen Vorgang auf der Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.DBforPostgreSQL/locations/privateEndpointConnectionProxyAzureAsyncOperation/read | Ruft das Ergebnis für einen Vorgang auf dem Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.DBforPostgreSQL/locations/privateEndpointConnectionProxyOperationResults/read | Ruft das Ergebnis für einen Vorgang auf dem Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesAzureAsyncOperation/read | Gibt die Liste der Vorgangsergebnisse der Bedrohungserkennung auf dem Server zurück |
> | Action | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesOperationResults/read | Gibt die Liste der Vorgangsergebnisse der Bedrohungserkennung auf dem Server zurück |
> | Action | Microsoft.DBforPostgreSQL/locations/serverKeyAzureAsyncOperation/read | Ruft gegenwärtig ausgeführte Vorgänge für Serverschlüssel von Transparent Data Encryption ab. |
> | Action | Microsoft.DBforPostgreSQL/locations/serverKeyOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für Serverschlüssel von Transparent Data Encryption ab. |
> | Action | Microsoft.DBforPostgreSQL/operations/read | Gibt die Liste der PostgreSQL-Vorgänge zurück |
> | Action | Microsoft.DBforPostgreSQL/performanceTiers/read | Gibt die Liste der verfügbaren Leistungsstufen zurück. |
> | Action | Microsoft.DBforPostgreSQL/register/action | Registriert den PostgreSQL-Ressourcenanbieter |
> | Action | Microsoft.DBforPostgreSQL/servers/administrators/delete | Löscht einen vorhandenen Administrator eines PostgreSQL-Servers |
> | Action | Microsoft.DBforPostgreSQL/servers/administrators/read | Ruft eine Liste der PostgreSQL-Serveradministratoren ab |
> | Action | Microsoft.DBforPostgreSQL/servers/administrators/write | Erstellt oder aktualisiert den PostgreSQL-Serveradministrator mit den angegebenen Parametern |
> | Action | Microsoft.DBforPostgreSQL/servers/advisors/read | Gibt die Liste mit den Ratgebern zurück. |
> | Action | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActions/read | Gibt die Liste der empfohlenen Aktionen zurück. |
> | Action | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActionSessions/action | Dient dazu, Empfehlungen abzugeben. |
> | Action | Microsoft.DBforPostgreSQL/servers/configurations/read | Gibt die Liste mit den Konfigurationen für einen Server zurück oder ruft die Eigenschaften für die angegebene Konfiguration ab. |
> | Action | Microsoft.DBforPostgreSQL/servers/configurations/write | Aktualisiert den Wert für die angegebene Konfiguration. |
> | Action | Microsoft.DBforPostgreSQL/servers/databases/delete | Löscht eine vorhandene PostgreSQL-Datenbank |
> | Action | Microsoft.DBforPostgreSQL/servers/databases/read | Gibt die Liste der PostgreSQL-Datenbanken zurück oder ruft die Eigenschaften für die angegebene Datenbank ab |
> | Action | Microsoft.DBforPostgreSQL/servers/databases/write | Erstellt eine PostgreSQL-Datenbank mit den angegebenen Parametern oder aktualisiert die Eigenschaften für die angegebene Datenbank |
> | Action | Microsoft.DBforPostgreSQL/servers/delete | Löscht einen vorhandenen Server. |
> | Action | Microsoft.DBforPostgreSQL/servers/firewallRules/delete | Löscht eine vorhandene Firewallregel. |
> | Action | Microsoft.DBforPostgreSQL/servers/firewallRules/read | Gibt die Liste der Firewallregeln für einen Server zurück oder ruft die Eigenschaften für die angegebene Firewallregel ab. |
> | Action | Microsoft.DBforPostgreSQL/servers/firewallRules/write | Erstellt eine Firewallregel mit den angegebenen Parametern oder aktualisiert eine vorhandene Regel. |
> | Action | Microsoft.DBforPostgreSQL/servers/keys/delete | Löscht einen vorhandenen Serverschlüssel. |
> | Action | Microsoft.DBforPostgreSQL/servers/keys/read | Gibt die Liste der Serverschlüssel zurück oder ruft die Eigenschaften für den angegebenen Serverschlüssel ab. |
> | Action | Microsoft.DBforPostgreSQL/servers/keys/write | Erstellt einen Schlüssel mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Serverschlüssel. |
> | Action | Microsoft.DBforPostgreSQL/servers/logFiles/read | Gibt die Liste der PostgreSQL-Protokolldateien zurück |
> | Action | Microsoft.DBforPostgreSQL/servers/privateEndpointConnectionProxies/delete | Löscht einen Verbindungsproxy eines privaten Endpunkts |
> | Action | Microsoft.DBforPostgreSQL/servers/privateEndpointConnectionProxies/read | Gibt eine Liste der Verbindungsproxys eines privaten Endpunkts zurück oder ruft die Eigenschaften für den angegebenen Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.DBforPostgreSQL/servers/privateEndpointConnectionProxies/validate/action | Überprüft den Aufruf zum Erstellen einer Verbindung mit einem privaten Endpunkt auf NRP-Seite |
> | Action | Microsoft.DBforPostgreSQL/servers/privateEndpointConnectionProxies/write | Erstellt einen Verbindungsproxy eines privaten Endpunkts mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für den angegebenen Verbindungsproxy eines privaten Endpunkts |
> | Action | Microsoft.DBforPostgreSQL/servers/privateEndpointConnections/delete | Löscht eine Verbindung mit einem privaten Endpunkt |
> | Action | Microsoft.DBforPostgreSQL/servers/privateEndpointConnections/read | Gibt eine Liste der Verbindungen mit einem privaten Endpunkt zurück oder ruft die Eigenschaften für die angegebene Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.DBforPostgreSQL/servers/privateEndpointConnections/write | Genehmigt eine vorhandene Verbindung mit einem privaten Endpunkt, oder lehnt diese ab |
> | Action | Microsoft.DBforPostgreSQL/servers/privateLinkResources/read | Ruft die Ressourcen der privaten Verknüpfung für die entsprechende PostgreSQL Server-Instanz ab |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Postgres-Server ab. |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken. |
> | Action | Microsoft.DBforPostgreSQL/servers/queryTexts/action | Zurückgeben des Texts einer Abfrage |
> | Action | Microsoft.DBforPostgreSQL/servers/queryTexts/read | Gibt die Texte für eine Liste von Abfragen zurück. |
> | Action | Microsoft.DBforPostgreSQL/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Action | Microsoft.DBforPostgreSQL/servers/recoverableServers/read | Gibt die wiederherstellbaren PostgreSQL Server-Informationen zurück. |
> | Action | Microsoft.DBforPostgreSQL/servers/replicas/read | Ruft Lesereplikate eines PostgreSQL-Servers ab |
> | Action | Microsoft.DBforPostgreSQL/servers/restart/action | Startet einen bestimmten Server neu |
> | Action | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/read | Dient zum Abrufen von Details zur Richtlinie für die Serverbedrohungserkennung, die für einen bestimmten Server konfiguriert ist. |
> | Action | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/write | Dient zum Ändern der Richtlinie für die Serverbedrohungserkennung für einen bestimmten Server. |
> | Action | Microsoft.DBforPostgreSQL/servers/topQueryStatistics/read | Gibt die Liste mit der Abfragestatistik für die obersten Abfragen zurück. |
> | Action | Microsoft.DBforPostgreSQL/servers/updateConfigurations/action | Aktualisiert Konfigurationen für den angegebenen Server. |
> | Action | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/delete | Löscht eine vorhandene VNET-Regel. |
> | Action | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/read | Gibt die Liste der VNET-Regeln zurück oder ruft die Eigenschaften für die angegebene VNET-Regel ab. |
> | Action | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/write | Erstellt eine VNET-Regel mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene VNET-Regel. |
> | Action | Microsoft.DBforPostgreSQL/servers/waitStatistics/read | Gibt Wartestatistiken für eine Instanz zurück. |
> | Action | Microsoft.DBforPostgreSQL/servers/write | Erstellt einen Server mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Server. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/configurations/read | Gibt die Liste mit den Konfigurationen für einen Server zurück oder ruft die Eigenschaften für die angegebene Konfiguration ab. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/configurations/write | Aktualisiert den Wert für die angegebene Konfiguration. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/delete | Löscht einen vorhandenen Server. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/firewallRules/delete | Löscht eine vorhandene Firewallregel. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/firewallRules/read | Gibt die Liste der Firewallregeln für einen Server zurück oder ruft die Eigenschaften für die angegebene Firewallregel ab. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/firewallRules/write | Erstellt eine Firewallregel mit den angegebenen Parametern oder aktualisiert eine vorhandene Regel. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Postgres-Server ab. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/updateConfigurations/action | Aktualisiert Konfigurationen für den angegebenen Server. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/write | Erstellt einen Server mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Server. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Devices/Account/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.Devices/Account/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.Devices/Account/logDefinitions/read | Ruft die verfügbaren Protokolldefinitionen für den IotHub-Dienst ab. |
> | Action | Microsoft.Devices/Account/metricDefinitions/read | Ruft die verfügbaren Metriken für den IotHub-Dienst ab. |
> | Action | Microsoft.Devices/checkNameAvailability/Action | Dient zum Prüfen, ob ein IotHub-Name verfügbar ist. |
> | Action | Microsoft.Devices/checkNameAvailability/Action | Dient zum Prüfen, ob ein IotHub-Name verfügbar ist. |
> | Action | Microsoft.Devices/checkProvisioningServiceNameAvailability/Action | Überprüft, ob der Name des Provisioning-Diensts verfügbar ist. |
> | Action | Microsoft.Devices/checkProvisioningServiceNameAvailability/Action | Überprüft, ob der Name des Provisioning-Diensts verfügbar ist. |
> | Action | Microsoft.Devices/digitalTwins/Delete | Löschen eines vorhandenen Digital Twins-Kontos und aller untergeordneten Elemente |
> | Action | Microsoft.Devices/digitalTwins/operationresults/Read | Abrufen des Status eines Vorgangs für ein Digital Twins-Konto |
> | Action | Microsoft.Devices/digitalTwins/Read | Ruft eine Liste der Digital Twins-Konten ab, die einem Abonnement zugeordnet sind |
> | Action | Microsoft.Devices/digitalTwins/skus/Read | Abrufen einer Liste gültiger SKUs für Digital Twins-Konten |
> | Action | Microsoft.Devices/digitalTwins/Write | Erstellen eines neuen Digitial Twins-Kontos |
> | Action | Microsoft.Devices/ElasticPools/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.Devices/ElasticPools/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.Devices/elasticPools/eventGridFilters/Delete | Löscht den Event Grid-Filter eines Pools für elastische Datenbanken. |
> | Action | Microsoft.Devices/elasticPools/eventGridFilters/Read | Ruft den Event Grid-Filter eines Pools für elastische Datenbanken ab. |
> | Action | Microsoft.Devices/elasticPools/eventGridFilters/Write | Erstellt einen neuen Event Grid-Filter eines Pools für elastische Datenbanken oder aktualisiert ihn. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Delete | Löscht Zertifikate. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/generateVerificationCode/Action | Generiert einen Prüfcode. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Read | Ruft das Zertifikat ab. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/verify/Action | Überprüft die Zertifikatsressource. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Write | Erstellt oder aktualisiert das Zertifikat. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/Delete | Löscht die IotHub-Mandantenressource. |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete | Dient zum Löschen von EventHub-Consumergruppen. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read | Dient zum Abrufen von EventHub-Consumergruppen. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write | Dient zum Erstellen von EventHub-Consumergruppen. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/exportDevices/Action | Dient zum Exportieren von Geräten. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/getStats/Read | Ruft die IotHub-Mandantenstatistikressource ab. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/importDevices/Action | Dient zum Importieren von Geräten. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action | Ruft den IotHub-Mandantenschlüssel ab. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/jobs/Read | Dient zum Abrufen von Auftragsdetails, die für eine bestimmte IotHub-Instanz übermittelt wurden. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/listKeys/Action | Ruft die IotHub-Mandantenschlüssel ab. |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/logDefinitions/read | Ruft die verfügbaren Protokolldefinitionen für den IotHub-Dienst ab. |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/metricDefinitions/read | Ruft die verfügbaren Metriken für den IotHub-Dienst ab. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/quotaMetrics/Read | Dient zum Abrufen von Kontingentmetriken. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/Read | Ruft die IotHub-Mandantenressource ab. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testall/Action | Dient zum Testen einer Nachricht mit allen vorhandenen Routen. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testnew/Action | Dient zum Testen einer Nachricht mit einer angegebenen Testroute. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/routingEndpointsHealth/Read | Ruft die Integrität aller Routingendpunkte für eine IotHub-Instanz ab. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/securitySettings/operationResults/Read | Ruft das Ergebnis des asynchronen PUT-Vorgangs für SecuritySettings des IoT-Mandantenhubs ab. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/securitySettings/Read | Ruft die Azure Security Center-Einstellungen auf dem IoT-Mandantenhub ab. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/securitySettings/Write | Aktualisiert die Azure Security Center-Einstellungen auf dem IoT-Mandantenhub. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/Write | Erstellt oder aktualisiert die IotHub-Mandantenressource. |
> | Action | Microsoft.Devices/ElasticPools/metricDefinitions/read | Ruft die verfügbaren Metriken für den IotHub-Dienst ab. |
> | Action | Microsoft.Devices/iotHubs/certificates/Delete | Löscht Zertifikate. |
> | Action | Microsoft.Devices/iotHubs/certificates/generateVerificationCode/Action | Generiert einen Prüfcode. |
> | Action | Microsoft.Devices/iotHubs/certificates/Read | Ruft das Zertifikat ab. |
> | Action | Microsoft.Devices/iotHubs/certificates/verify/Action | Überprüft die Zertifikatsressource. |
> | Action | Microsoft.Devices/iotHubs/certificates/Write | Erstellt oder aktualisiert das Zertifikat. |
> | Action | Microsoft.Devices/iotHubs/Delete | Dient zum Löschen von IotHub-Ressourcen. |
> | Action | Microsoft.Devices/IotHubs/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.Devices/IotHubs/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.Devices/iotHubs/eventGridFilters/Delete | Löscht den Event Grid-Filter. |
> | Action | Microsoft.Devices/iotHubs/eventGridFilters/Read | Ruft den Event Grid-Filter ab. |
> | Action | Microsoft.Devices/iotHubs/eventGridFilters/Write | Erstellt einen neuen Event Grid-Filter oder aktualisiert einen vorhandenen Event Grid-Filter. |
> | Action | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Delete | Dient zum Löschen von EventHub-Consumergruppen. |
> | Action | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Read | Dient zum Abrufen von EventHub-Consumergruppen. |
> | Action | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Write | Dient zum Erstellen von EventHub-Consumergruppen. |
> | Action | Microsoft.Devices/iotHubs/exportDevices/Action | Dient zum Exportieren von Geräten. |
> | Action | Microsoft.Devices/iotHubs/importDevices/Action | Dient zum Importieren von Geräten. |
> | Action | Microsoft.Devices/iotHubs/iotHubKeys/listkeys/Action | Dient zum Abrufen des IotHub-Schlüssels für den angegebenen Namen. |
> | Action | Microsoft.Devices/iotHubs/iotHubStats/Read | Dient zum Abrufen der IotHub-Statistik. |
> | Action | Microsoft.Devices/iotHubs/jobs/Read | Dient zum Abrufen von Auftragsdetails, die für eine bestimmte IotHub-Instanz übermittelt wurden. |
> | Action | Microsoft.Devices/iotHubs/listkeys/Action | Dient zum Abrufen aller IotHub-Schlüssel. |
> | Action | Microsoft.Devices/IotHubs/logDefinitions/read | Ruft die verfügbaren Protokolldefinitionen für den IotHub-Dienst ab. |
> | Action | Microsoft.Devices/IotHubs/metricDefinitions/read | Ruft die verfügbaren Metriken für den IotHub-Dienst ab. |
> | Action | Microsoft.Devices/iotHubs/operationresults/Read | Hiermit rufen Sie das Vorgangsergebnis (veraltete API) ab. |
> | Action | Microsoft.Devices/iotHubs/quotaMetrics/Read | Dient zum Abrufen von Kontingentmetriken. |
> | Action | Microsoft.Devices/iotHubs/Read | Ruft die IotHub-Ressourcen ab. |
> | Action | Microsoft.Devices/iotHubs/routing/$testall/Action | Dient zum Testen einer Nachricht mit allen vorhandenen Routen. |
> | Action | Microsoft.Devices/iotHubs/routing/$testnew/Action | Dient zum Testen einer Nachricht mit einer angegebenen Testroute. |
> | Action | Microsoft.Devices/iotHubs/routingEndpointsHealth/Read | Ruft die Integrität aller Routingendpunkte für eine IotHub-Instanz ab. |
> | Action | Microsoft.Devices/iotHubs/securitySettings/operationResults/Read | Ruft das Ergebnis des asynchronen PUT-Vorgangs für SecuritySettings des IoT-Hubs ab. |
> | Action | Microsoft.Devices/iotHubs/securitySettings/Read | Ruft die Azure Security Center-Einstellungen auf dem IoT-Hub ab. |
> | Action | Microsoft.Devices/iotHubs/securitySettings/Write | Aktualisiert die Azure Security Center-Einstellungen auf dem IoT-Hub. |
> | Action | Microsoft.Devices/iotHubs/skus/Read | Dient zum Abrufen gültiger IotHub-SKUs. |
> | Action | Microsoft.Devices/iotHubs/Write | Dient zum Erstellen oder Aktualisieren von IotHub-Ressourcen. |
> | Action | Microsoft.Devices/locations/operationresults/Read | Ruft das standortbasierte Vorgangsergebnis ab. |
> | Action | Microsoft.Devices/operationresults/Read | Hiermit rufen Sie das Vorgangsergebnis ab. |
> | Action | Microsoft.Devices/operations/Read | Dient zum Abrufen aller ResourceProvider-Vorgänge. |
> | Action | Microsoft.Devices/provisioningServices/certificates/Delete | Löscht Zertifikate. |
> | Action | Microsoft.Devices/provisioningServices/certificates/generateVerificationCode/Action | Generiert einen Prüfcode. |
> | Action | Microsoft.Devices/provisioningServices/certificates/Read | Ruft das Zertifikat ab. |
> | Action | Microsoft.Devices/provisioningServices/certificates/verify/Action | Überprüft die Zertifikatsressource. |
> | Action | Microsoft.Devices/provisioningServices/certificates/Write | Erstellt oder aktualisiert das Zertifikat. |
> | Action | Microsoft.Devices/provisioningServices/Delete | Löscht die IotDps-Ressource. |
> | Action | Microsoft.Devices/provisioningServices/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.Devices/provisioningServices/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.Devices/provisioningServices/keys/listkeys/Action | Ruft IotDps-Schlüssel für Schlüsselnamen ab. |
> | Action | Microsoft.Devices/provisioningServices/listkeys/Action | Ruft alle IotDps-Schlüssel ab. |
> | Action | Microsoft.Devices/provisioningServices/logDefinitions/read | Ruft die verfügbaren Protokolldefinitionen für den Bereitstellungsdienst ab. |
> | Action | Microsoft.Devices/provisioningServices/metricDefinitions/read | Ruft die verfügbaren Metriken für den Bereitstellungsdienst ab. |
> | Action | Microsoft.Devices/provisioningServices/operationresults/Read | Hiermit rufen Sie das DPS-Vorgangsergebnis ab. |
> | Action | Microsoft.Devices/provisioningServices/Read | Ruft die IotDps-Ressource ab. |
> | Action | Microsoft.Devices/provisioningServices/skus/Read | Ruft gültige IotDps-SKUs ab. |
> | Action | Microsoft.Devices/provisioningServices/Write | Erstellt die IotDps-Ressource. |
> | Action | Microsoft.Devices/register/action | Dient zum Registrieren des Abonnements für den IotHub-Ressourcenanbieter und ermöglicht die Erstellung von IotHub-Ressourcen. |
> | Action | Microsoft.Devices/usages/Read | Dient zum Abrufen von Details zur Abonnementnutzung für diesen Anbieter. |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DevSpaces/controllers/delete | Löscht den Azure Dev Spaces-Controller und Dataplane-Dienste. |
> | Action | Microsoft.DevSpaces/controllers/listConnectionDetails/action | Listet Verbindungsdetails für die Infrastruktur des Azure Dev Spaces-Controllers auf. |
> | Action | Microsoft.DevSpaces/controllers/read | Liest Azure Dev Spaces-Controllereigenschaften. |
> | Action | Microsoft.DevSpaces/controllers/write | Erstellt oder aktualisiert Azure Dev Spaces-Controllereigenschaften. |
> | Action | Microsoft.DevSpaces/locations/checkContainerHostMapping/action | Überprüft die bestehende Controllerzuordnung für einen Containerhost |
> | Action | Microsoft.DevSpaces/locations/operationresults/read | Liest den Status eines asynchronen Vorgangs |
> | Action | Microsoft.DevSpaces/register/action | Registriert den Microsoft Dev Spaces-Ressourcenanbieter bei einem Abonnement. |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DevTestLab/labCenters/delete | Löscht die Lab-Center. |
> | Action | Microsoft.DevTestLab/labCenters/read | Liest die Lab-Center. |
> | Action | Microsoft.DevTestLab/labCenters/write | Dient zum Hinzufügen oder Ändern von Lab-Centern. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/armTemplates/read | Dient zum Lesen von Azure Resource Manager-Vorlagen. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/artifacts/GenerateArmTemplate/action | Generiert eine Azure Resource Manager-Vorlage für das angegebene Artefakt, lädt die erforderlichen Dateien in ein Speicherkonto hoch und überprüft das generierte Artefakt. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/artifacts/read | Dient zum Lesen von Artefakten. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/delete | Dient zum Löschen von Artefaktquellen. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/read | Dient zum Lesen von Artefaktquellen. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/write | Dient zum Hinzufügen oder Ändern von Artefaktquellen. |
> | Action | Microsoft.DevTestLab/labs/ClaimAnyVm/action | Dient zum Anfordern eines beliebigen anforderbaren virtuellen Computers im Lab. |
> | Action | Microsoft.DevTestLab/labs/costs/read | Dient zum Lesen von Kosten. |
> | Action | Microsoft.DevTestLab/labs/costs/write | Dient zum Hinzufügen oder Ändern von Kosten. |
> | Action | Microsoft.DevTestLab/labs/CreateEnvironment/action | Dient zum Erstellen von virtuellen Computern in einem Lab. |
> | Action | Microsoft.DevTestLab/labs/customImages/delete | Dient zum Löschen benutzerdefinierter Images. |
> | Action | Microsoft.DevTestLab/labs/customImages/read | Dient zum Lesen benutzerdefinierter Images. |
> | Action | Microsoft.DevTestLab/labs/customImages/write | Dient zum Hinzufügen oder Ändern benutzerdefinierter Images. |
> | Action | Microsoft.DevTestLab/labs/delete | Dient zum Löschen von Labs. |
> | Action | Microsoft.DevTestLab/labs/EnsureCurrentUserProfile/action | Stellt sicher, dass der aktuelle Benutzer über ein gültiges Profil im Lab verfügt |
> | Action | Microsoft.DevTestLab/labs/ExportResourceUsage/action | Exportiert die Labressourcenverwendung in ein Speicherkonto. |
> | Action | Microsoft.DevTestLab/labs/formulas/delete | Dient zum Löschen von Formeln. |
> | Action | Microsoft.DevTestLab/labs/formulas/read | Dient zum Lesen von Formeln. |
> | Action | Microsoft.DevTestLab/labs/formulas/write | Dient zum Hinzufügen oder Ändern von Formeln. |
> | Action | Microsoft.DevTestLab/labs/galleryImages/read | Dient zum Lesen von Katalogimages. |
> | Action | Microsoft.DevTestLab/labs/GenerateUploadUri/action | Dient zum Generieren eines URI für das Hochladen benutzerdefinierter Datenträgerimages in ein Lab. |
> | Action | Microsoft.DevTestLab/labs/ImportVirtualMachine/action | Importiert einen virtuellen Computer in ein anderes Lab. |
> | Action | Microsoft.DevTestLab/labs/ListVhds/action | Dient zum Auflisten von Datenträgerimages, die für die Erstellung benutzerdefinierter Images verfügbar sind. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/delete | Löscht Benachrichtigungskanäle. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/Notify/action | Dient zum Senden einer Benachrichtigung an einen angegebenen Kanal. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/read | Liest Benachrichtigungskanäle. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/write | Dient zum Hinzufügen oder Ändern von Benachrichtigungskanälen. |
> | Action | Microsoft.DevTestLab/labs/policySets/EvaluatePolicies/action | Wertet Labrichtlinien aus. |
> | Action | Microsoft.DevTestLab/labs/policySets/policies/delete | Dient zum Löschen von Richtlinien. |
> | Action | Microsoft.DevTestLab/labs/policySets/policies/read | Dient zum Lesen von Richtlinien. |
> | Action | Microsoft.DevTestLab/labs/policySets/policies/write | Dient zum Hinzufügen oder Ändern von Richtlinien. |
> | Action | Microsoft.DevTestLab/labs/policySets/read | Liest Richtliniensätze. |
> | Action | Microsoft.DevTestLab/labs/read | Dient zum Lesen von Labs. |
> | Action | Microsoft.DevTestLab/labs/schedules/delete | Dient zum Löschen von Zeitplänen. |
> | Action | Microsoft.DevTestLab/labs/schedules/Execute/action | Dient zum Ausführen eines Zeitplans. |
> | Action | Microsoft.DevTestLab/labs/schedules/ListApplicable/action | Listet alle anwendbaren Zeitpläne auf. |
> | Action | Microsoft.DevTestLab/labs/schedules/read | Dient zum Lesen von Zeitplänen. |
> | Action | Microsoft.DevTestLab/labs/schedules/write | Dient zum Hinzufügen oder Ändern von Zeitplänen. |
> | Action | Microsoft.DevTestLab/labs/serviceRunners/delete | Dient zum Löschen von Dienstausführern. |
> | Action | Microsoft.DevTestLab/labs/serviceRunners/read | Dient zum Lesen von Dienstausführern. |
> | Action | Microsoft.DevTestLab/labs/serviceRunners/write | Dient zum Hinzufügen oder Ändern von Dienstausführern. |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/delete | Löscht freigegebene Kataloge |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/read | Liest freigegebene Kataloge |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/delete | Löscht freigegebene Bilder |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/read | Liest freigegebene Bilder |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/write | Fügt freigegebene Bilder hinzu oder ändert sie |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/write | Fügt freigegebene Kataloge hinzu oder ändert sie |
> | Action | Microsoft.DevTestLab/labs/users/delete | Dient zum Löschen von Benutzerprofilen. |
> | Action | Microsoft.DevTestLab/labs/users/disks/Attach/action | Dient zum Anfügen des Datenträgers an den virtuellen Computer sowie zum Erstellen der Datenträgerlease für den virtuellen Computer. |
> | Action | Microsoft.DevTestLab/labs/users/disks/delete | Dient zum Löschen von Datenträgern. |
> | Action | Microsoft.DevTestLab/labs/users/disks/Detach/action | Dient zum Trennen des an den virtuellen Computer angefügten Datenträgers sowie zum Beenden der Datenträgerlease für den virtuellen Computer. |
> | Action | Microsoft.DevTestLab/labs/users/disks/read | Dient zum Lesen von Datenträgern. |
> | Action | Microsoft.DevTestLab/labs/users/disks/write | Dient zum Hinzufügen oder Ändern von Datenträgern. |
> | Action | Microsoft.DevTestLab/labs/users/environments/delete | Dient zum Löschen von Umgebungen. |
> | Action | Microsoft.DevTestLab/labs/users/environments/read | Dient zum Lesen von Umgebungen. |
> | Action | Microsoft.DevTestLab/labs/users/environments/write | Dient zum Hinzufügen oder Ändern von Umgebungen. |
> | Action | Microsoft.DevTestLab/labs/users/read | Dient zum Lesen von Benutzerprofilen. |
> | Action | Microsoft.DevTestLab/labs/users/secrets/delete | Dient zum Löschen von Geheimnissen. |
> | Action | Microsoft.DevTestLab/labs/users/secrets/read | Dient zum Lesen von Geheimnissen. |
> | Action | Microsoft.DevTestLab/labs/users/secrets/write | Dient zum Hinzufügen oder Ändern von Geheimnissen. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/delete | Löscht Service Fabrics. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/ListApplicableSchedules/action | Listet die anwendbaren Zeitpläne zum Starten/Beenden auf, sofern vorhanden. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/read | Liest Service Fabrics. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/delete | Dient zum Löschen von Zeitplänen. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/Execute/action | Dient zum Ausführen eines Zeitplans. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/read | Dient zum Lesen von Zeitplänen. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/write | Dient zum Hinzufügen oder Ändern von Zeitplänen. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/Start/action | Startet ein Service Fabric. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/Stop/action | Beendet ein Service Fabric. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/write | Dient zum Hinzufügen oder Ändern von Service Fabrics. |
> | Action | Microsoft.DevTestLab/labs/users/write | Dient zum Hinzufügen oder Ändern von Benutzerprofilen. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/AddDataDisk/action | Dient zum Anfügen eines neuen oder vorhandenen Datenträgers an virtuelle Computer. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/ApplyArtifacts/action | Dient zum Anwenden von Artefakten auf virtuelle Computer. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Claim/action | Dient dazu, einen vorhandenen virtuellen Computer in Besitz zu nehmen. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/ClearArtifactResults/action | Löscht die Artefaktergebnisse des virtuellen Computers. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/delete | Dient zum Löschen virtueller Computer. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/DetachDataDisk/action | Dient zum Trennen des angegebenen Datenträgers vom virtuellen Computer. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/GetRdpFileContents/action | Ruft eine Zeichenfolge ab, die den Inhalt der RDP-Datei für den virtuellen Computer darstellt. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/ListApplicableSchedules/action | Listet die anwendbaren Zeitpläne zum Starten/Beenden auf, sofern vorhanden. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/read | Dient zum Lesen virtueller Computer. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Redeploy/action | Stellt einen virtuellen Computer erneut bereit. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Resize/action | Ändert die Größe eines virtuellen Computers. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Restart/action | Startet einen virtuellen Computer. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/delete | Dient zum Löschen von Zeitplänen. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/Execute/action | Dient zum Ausführen eines Zeitplans. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/read | Dient zum Lesen von Zeitplänen. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/write | Dient zum Hinzufügen oder Ändern von Zeitplänen. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Start/action | Dient zum Starten eines virtuellen Computers. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Stop/action | Dient zum Beenden eines virtuellen Computers. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/TransferDisks/action | Überträgt den Besitz aller an den virtuellen Computer angefügten Datenträger auf den aktuellen Benutzer. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/UnClaim/action | Hiermit geben Sie den Besitz eines vorhandenen virtuellen Computers frei. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/write | Dient zum Hinzufügen oder Ändern virtueller Computer. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/bastionHosts/delete | Löschen von bastionhosts. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/bastionHosts/read | Lesen von bastionhosts. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/bastionHosts/write | Hinzufügen oder Ändern von bastionhosts. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/delete | Dient zum Löschen virtueller Netzwerke. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/read | Dient zum Lesen virtueller Netzwerke. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/write | Dient zum Hinzufügen oder Ändern virtueller Netzwerke. |
> | Action | Microsoft.DevTestLab/labs/vmPools/delete | Löscht Pools für virtuelle Computer. |
> | Action | Microsoft.DevTestLab/labs/vmPools/read | Liest Pools für virtuelle Computer. |
> | Action | Microsoft.DevTestLab/labs/vmPools/write | Fügt Pools für virtuelle Computer hinzu oder ändert diese. |
> | Action | Microsoft.DevTestLab/labs/write | Dient zum Hinzufügen oder Ändern von Labs. |
> | Action | Microsoft.DevTestLab/locations/operations/read | Dient zum Lesen von Vorgängen. |
> | Action | Microsoft.DevTestLab/register/action | Registriert das Abonnement. |
> | Action | Microsoft.DevTestLab/schedules/delete | Dient zum Löschen von Zeitplänen. |
> | Action | Microsoft.DevTestLab/schedules/Execute/action | Dient zum Ausführen eines Zeitplans. |
> | Action | Microsoft.DevTestLab/schedules/read | Dient zum Lesen von Zeitplänen. |
> | Action | Microsoft.DevTestLab/schedules/Retarget/action | Aktualisiert die Zielressourcen-ID eines Zeitplans. |
> | Action | Microsoft.DevTestLab/schedules/write | Dient zum Hinzufügen oder Ändern von Zeitplänen. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DocumentDB/databaseAccountNames/read | Prüft die Verfügbarkeit des Namens. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/delete | Löscht eine Auflistung. Gilt nur für den API-Typ „mongodb“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „mongodb“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/read | Liest eine Auflistung oder listet alle Auflistungen auf. Gilt nur für den API-Typ „mongodb“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „mongodb“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/read | Liest den Durchsatz einer Auflistung. Gilt nur für den API-Typ „mongodb“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/write | Aktualisiert den Durchsatz einer Auflistung. Gilt nur für den API-Typ „mongodb“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/write | Erstellt oder aktualisiert eine Auflistung. Gilt nur für den API-Typ „mongodb“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/delete | Löschen eines Containers. Gilt nur für den API-Typ „sql“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „sql“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/read | Liest einen Container oder listet alle Container auf. Gilt nur für den API-Typ „sql“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „sql“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/read | Liest den Containerdurchsatz. Gilt nur für den API-Typ „sql“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/write | Aktualisiert einen Containerdurchsatz. Gilt nur für den API-Typ „sql“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/write | Erstellt oder aktualisiert einen Container. Gilt nur für den API-Typ „sql“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/delete | Löscht eine Datenbank. Gilt nur für die API-Typen „sql“, „mongodb“ und „gremlin“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/delete | Löscht einen Graphen. Gilt nur für den API-Typ „gremlin“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „gremlin“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/read | Liest einen Graphen oder listet alle Graphen auf. Gilt nur für den API-Typ „gremlin“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „gremlin“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/read | Liest den Durchsatz eines Graphen. Gilt nur für den API-Typ „gremlin“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/write | Aktualisiert den Durchsatz eines Graphen. Gilt nur für den API-Typ „gremlin“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/write | Erstellt oder aktualisiert einen Graphen. Gilt nur für den API-Typ „gremlin“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für die API-Typen „sql“, „mongodb“ und „gremlin“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/read | Liest eine Datenbank oder listet alle Datenbanken auf. Gilt nur für die API-Typen „sql“, „mongodb“ und „gremlin“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für die API-Typen „sql“, „mongodb“ und „gremlin“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/read | Liest den Durchsatz einer Datenbank. Gilt nur für die API-Typen „sql“, „mongodb“ und „gremlin“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/write | Aktualisiert den Durchsatz einer Datenbank. Gilt nur für die API-Typen „sql“, „mongodb“ und „gremlin“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/write | Erstellen einer Datenbank Gilt nur für die API-Typen „sql“, „mongodb“ und „gremlin“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/delete | Löscht einen Keyspace. Gilt nur für den API-Typ „cassandra“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „cassandra“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/read | Liest einen Keyspace oder listet alle Keyspaces auf. Gilt nur für den API-Typ „cassandra“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „cassandra“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/read | Liest den Durchsatz eines Keyspace. Gilt nur für den API-Typ „cassandra“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/write | Aktualisiert den Durchsatz eines Keyspace. Gilt nur für den API-Typ „cassandra“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/delete | Löscht eine Tabelle. Gilt nur für den API-Typ „cassandra“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „cassandra“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/read | Liest eine Tabelle oder listet alle Tabellen auf. Gilt nur für den API-Typ „cassandra“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „cassandra“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/read | Liest den Durchsatz einer Tabelle. Gilt nur für den API-Typ „cassandra“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/write | Aktualisiert den Durchsatz einer Tabelle. Gilt nur für den API-Typ „cassandra“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/write | Erstellt oder aktualisiert eine Tabelle. Gilt nur für den API-Typ „cassandra“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/write | Erstellt einen Keyspace. Gilt nur für den API-Typ „cassandra“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/delete | Löscht eine Tabelle. Gilt nur für den API-Typ „table“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „table“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/read | Liest eine Tabelle oder listet alle Tabellen auf. Gilt nur für den API-Typ „table“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/operationResults/read | Liest den Status des asynchronen Vorgangs. Gilt nur für den API-Typ „table“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/read | Liest den Durchsatz einer Tabelle. Gilt nur für den API-Typ „table“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/write | Aktualisiert den Durchsatz einer Tabelle. Gilt nur für den API-Typ „table“. Gilt nur für den Einstellungstyp „throughput“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/write | Erstellt oder aktualisiert eine Tabelle. Gilt nur für den API-Typ „table“. |
> | Action | Microsoft.DocumentDB/databaseAccounts/backup/action | Sendet eine Anforderung zum Konfigurieren der Sicherung. |
> | Action | Microsoft.DocumentDB/databaseAccounts/changeResourceGroup/action | Dient zum Ändern der Ressourcengruppe eines Datenbankkontos. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/metricDefinitions/read | Liest die Sammlungsmetrikdefinitionen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/metrics/read | Liest die Sammlungsmetriken. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read | Liest Metriken auf Partitionsschlüsselebene eines Datenbankkontos. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/metrics/read | Liest Metriken auf Partitionsebene eines Datenbankkontos. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/read | Liest die Partitionen eines Datenbankkontos in einer Collection. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/usages/read | Liest Verwendungen auf Partitionsebene eines Datenbankkontos. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/usages/read | Liest Informationen zur Sammlungsverwendung. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/metricDefinitions/read | Liest die Datenbankmetrikdefinitionen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/metrics/read | Liest die Datenbankmetriken. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/usages/read | Liest Informationen zur Datenbankverwendung. |
> | Action | Microsoft.DocumentDB/databaseAccounts/delete | Löscht die Datenbankkonten. |
> | Action | Microsoft.DocumentDB/databaseAccounts/failoverPriorityChange/action | Dient zum Ändern der Failoverprioritäten von Regionen eines Datenbankkontos. Wird für manuelle Failovervorgänge verwendet. |
> | Action | Microsoft.DocumentDB/databaseAccounts/getBackupPolicy/action | Ruft die Sicherungsrichtlinie des Datenbankkontos ab. |
> | Action | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/action | Dient zum Abrufen der Verbindungszeichenfolgen für ein Datenbankkonto. |
> | Action | Microsoft.DocumentDB/databaseAccounts/listKeys/action | Dient zum Auflisten von Schlüsseln eines Datenbankkontos. |
> | Action | Microsoft.DocumentDB/databaseAccounts/metricDefinitions/read | Liest die Metrikdefinitionen für Datenbankkonten. |
> | Action | Microsoft.DocumentDB/databaseAccounts/metrics/read | Liest die Datenbankkontometriken. |
> | Action | Microsoft.DocumentDB/databaseAccounts/offlineRegion/action | Hiermit setzen Sie einen Bereich eines Datenbankkontos offline.  |
> | Action | Microsoft.DocumentDB/databaseAccounts/onlineRegion/action | Hiermit setzen Sie einen Bereich eines Datenbankkontos online. |
> | Action | Microsoft.DocumentDB/databaseAccounts/operationResults/read | Liest den Status eines asynchronen Vorgangs. |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/metrics/read | Liest Latenzmetriken. |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/read | Liest Perzentile von Replikationslatenzen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read | Liest die Latenzmetriken für eine bestimmte Quell- und Zielregion. |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/targetRegion/metrics/read | Liest die Latenzmetriken für eine bestimmte Zielregion. |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/delete | Löscht einen Verbindungsproxy eines privaten Endpunkts eines Datenbankkontos |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/operationResults/read | Liest den Status des asynchronen Vorgangs des Verbindungsproxys eines privaten Endpunkts. |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/read | Liest einen Verbindungsproxy eines privaten Endpunkts eines Datenbankkontos |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/validate/action | Überprüft einen Verbindungsproxy eines privaten Endpunkts eines Datenbankkontos |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/write | Erstellt oder aktualisiert einen Verbindungsproxy eines privaten Endpunkts eines Datenbankkontos |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnections/delete | Löscht für ein Datenbankkonto die Verbindung eines privaten Endpunkts. |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnections/operationResults/read | Liest den Status eines asynchronen Vorgangs von privateEndpointConnections. |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnections/read | Liest die Verbindung eines privaten Endpunkts oder listet für ein Datenbankkonto alle Verbindungen mit privaten Endpunkten auf. |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnections/write | Erstellt oder aktualisiert für ein Datenbankkonto die Verbindung mit einem privaten Endpunkt. |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateLinkResources/read | Liest eine Ressource einer privaten Verbindung oder listet für ein Datenbankkonto alle Ressourcen einer privaten Verbindung auf. |
> | Action | Microsoft.DocumentDB/databaseAccounts/read | Liest ein Datenbankkonto. |
> | Action | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Liest die schreibgeschützten Schlüssel für Datenbankkonten. |
> | Action | Microsoft.DocumentDB/databaseAccounts/readonlykeys/read | Liest die schreibgeschützten Schlüssel für Datenbankkonten. |
> | Action | Microsoft.DocumentDB/databaseAccounts/regenerateKey/action | Dient zum Rotieren von Schlüsseln eines Datenbankkontos. |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/metrics/read | Liest die Metriken von regionalen Collections. |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read | Liest regionale Metriken auf Partitionsschlüsselebene eines Datenbankkontos. |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/metrics/read | Liest regionale Metriken auf Partitionsebene eines Datenbankkontos. |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/read | Liest die Partitionen eines regionalen Datenbankkontos in einer Collection. |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/metrics/read | Liest die Region und die Datenbankkontometriken. |
> | Action | Microsoft.DocumentDB/databaseAccounts/restore/action | Sendet eine Wiederherstellungsanforderung. |
> | Action | Microsoft.DocumentDB/databaseAccounts/usages/read | Liest Informationen zur Datenbankkontoverwendung. |
> | Action | Microsoft.DocumentDB/databaseAccounts/write | Dient zum Aktualisieren von Datenbankkonten. |
> | Action | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/action | Benachrichtigt Microsoft.DocumentDB darüber, dass das virtuelle Netzwerk oder Subnetz gelöscht wird. |
> | Action | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/operationResults/read | Liest den Status des asynchronen Vorgangs „deleteVirtualNetworkOrSubnets“. |
> | Action | Microsoft.DocumentDB/locations/operationsStatus/read | Liest den Status asynchroner Vorgänge |
> | Action | Microsoft.DocumentDB/operationResults/read | Liest den Status eines asynchronen Vorgangs. |
> | Action | Microsoft.DocumentDB/operations/read | Liest verfügbare Vorgänge für Microsoft DocumentDB  |
> | Action | Microsoft.DocumentDB/register/action |  Dient zum Registrieren des Microsoft DocumentDB-Ressourcenanbieters für das Abonnement. |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.DomainRegistration/checkDomainAvailability/Action | Dient zum Prüfen, ob eine Domäne erworben werden kann. |
> | Action | Microsoft.DomainRegistration/domains/Delete | Dient zum Löschen einer vorhandenen Domäne. |
> | Action | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Delete | Löscht die Besitz-ID. |
> | Action | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Listet die Besitz-IDs auf. |
> | Action | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Ruft die Besitz-ID ab. |
> | Action | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Write | Erstellt oder aktualisiert eine ID. |
> | Action | Microsoft.DomainRegistration/domains/operationresults/Read | Dient zum Abrufen eines Domänenvorgangs. |
> | Action | Microsoft.DomainRegistration/domains/Read | Dient zum Abrufen der Domänenliste. |
> | Action | Microsoft.DomainRegistration/domains/Read | Ruft die Domäne ab. |
> | Action | Microsoft.DomainRegistration/domains/renew/Action | Dient zum Verlängern einer vorhandenen Domäne. |
> | Action | Microsoft.DomainRegistration/domains/Write | Dient zum Hinzufügen einer neuen Domäne oder zum Aktualisieren einer bereits vorhandenen. |
> | Action | Microsoft.DomainRegistration/generateSsoRequest/Action | Dient zum Generieren einer Anforderung für die Anmeldung beim Domänensteuerungszentrum. |
> | Action | Microsoft.DomainRegistration/listDomainRecommendations/Action | Dient zum Abrufen der Liste mit Domänenempfehlungen auf der Grundlage von Schlüsselwörtern. |
> | Action | Microsoft.DomainRegistration/operations/Read | Dient zum Auflisten aller Vorgänge über die App Service-Domänenregistrierung. |
> | Action | Microsoft.DomainRegistration/register/action | Dient zum Registrieren des Microsoft-Domänenressourcenanbieters für das Abonnement. |
> | Action | Microsoft.DomainRegistration/topLevelDomains/listAgreements/Action | Listet die Vereinbarungsaktion auf. |
> | Action | Microsoft.DomainRegistration/topLevelDomains/Read | Ruft die Domänen der obersten Domäne ab. |
> | Action | Microsoft.DomainRegistration/topLevelDomains/Read | Ruft die Domäne der obersten Domäne ab. |
> | Action | Microsoft.DomainRegistration/validateDomainRegistrationInformation/Action | Dient zum Überprüfen eines Domänenerwerbsobjekts, ohne es zu übermitteln. |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.EventGrid/domains/delete | Löscht eine Domäne. |
> | Action | Microsoft.EventGrid/domains/listKeys/action | Listet die Schlüssel für eine Domäne auf. |
> | Action | Microsoft.EventGrid/domains/providers/Microsoft.Insights/logDefinitions/read | Ermöglicht den Zugriff auf Diagnoseprotokolle. |
> | Action | Microsoft.EventGrid/domains/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Domänen ab. |
> | Action | Microsoft.EventGrid/domains/read | Liest eine Domäne. |
> | Action | Microsoft.EventGrid/domains/regenerateKey/action | Generiert einen Schlüssel für eine Domäne neu. |
> | Action | Microsoft.EventGrid/domains/topics/delete | Löscht ein Domänenthema |
> | Action | Microsoft.EventGrid/domains/topics/read | Liest ein Domänenthema. |
> | Action | Microsoft.EventGrid/domains/topics/write | Erstellt oder aktualisiert ein Domänenthema |
> | Action | Microsoft.EventGrid/domains/write | Erstellt oder aktualisiert eine Domäne. |
> | Action | Microsoft.EventGrid/eventSubscriptions/delete | Löscht eventSubscription. |
> | Action | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Ruft die vollständige URL für das Ereignisabonnement ab. |
> | Action | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für Ereignisabonnements ab. |
> | Action | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für Ereignisabonnements. |
> | Action | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für eventSubscriptions ab. |
> | Action | Microsoft.EventGrid/eventSubscriptions/read | Liest eventSubscription. |
> | Action | Microsoft.EventGrid/eventSubscriptions/write | Erstellt oder aktualisiert eventSubscription. |
> | Action | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Themen ab. |
> | Action | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für Themen. |
> | Action | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Themen ab. |
> | Action | Microsoft.EventGrid/extensionTopics/read | Liest ein extensionTopic. |
> | Action | Microsoft.EventGrid/locations/eventSubscriptions/read | Listet regionale Ereignisabonnements auf. |
> | Action | Microsoft.EventGrid/locations/operationResults/read | Liest das Ergebnis eines regionalen Vorgangs. |
> | Action | Microsoft.EventGrid/locations/operationsStatus/read | Liest den Status eines regionalen Vorgangs. |
> | Action | Microsoft.EventGrid/locations/topictypes/eventSubscriptions/read | Listet regionale Ereignisabonnements nach Thematyp auf. |
> | Action | Microsoft.EventGrid/operationResults/read | Liest das Ergebnis eines Vorgangs. |
> | Action | Microsoft.EventGrid/operations/read | Listet EventGrid-Vorgänge auf. |
> | Action | Microsoft.EventGrid/operationsStatus/read | Liest den Status eines Vorgangs. |
> | Action | Microsoft.EventGrid/register/action | Registriert das Abonnement für den EventGrid-Ressourcenanbieter. |
> | Action | Microsoft.EventGrid/topics/delete | Löschen eines Themas |
> | Action | Microsoft.EventGrid/topics/listKeys/action | Listet die Schlüssel für ein Thema auf. |
> | Action | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Themen ab. |
> | Action | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für Themen. |
> | Action | Microsoft.EventGrid/topics/providers/Microsoft.Insights/logDefinitions/read | Ermöglicht den Zugriff auf Diagnoseprotokolle. |
> | Action | Microsoft.EventGrid/topics/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Themen ab. |
> | Action | Microsoft.EventGrid/topics/read | Liest ein Thema. |
> | Action | Microsoft.EventGrid/topics/regenerateKey/action | Generiert einen Schlüssel für ein Thema neu. |
> | Action | Microsoft.EventGrid/topics/write | Erstellt oder aktualisiert ein Thema. |
> | Action | Microsoft.EventGrid/topictypes/eventSubscriptions/read | Listet globale Ereignisabonnements nach Thematyp auf. |
> | Action | Microsoft.EventGrid/topictypes/eventtypes/read | Liest die von einem Thementyp unterstützten Ereignistypen. |
> | Action | Microsoft.EventGrid/topictypes/read | Liest einen Thementyp. |
> | Action | Microsoft.EventGrid/unregister/action | Hebt die Registrierung des Abonnements für den EventGrid-Ressourcenanbieter auf. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.EventHub/availableClusterRegions/read | Liest den Vorgang zum Auflisten verfügbarer vorab bereitgestellter Cluster nach Azure-Region |
> | Action | Microsoft.EventHub/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. |
> | Action | Microsoft.EventHub/checkNamespaceAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. Diese API ist veraltet. Verwenden Sie stattdessen CheckNameAvailability. |
> | Action | Microsoft.EventHub/clusters/delete | Löscht eine vorhandene Clusterressource |
> | Action | Microsoft.EventHub/clusters/namespaces/read | Listet Azure Resource Manager-Namespace-IDs für die Namespaces eines Clusters auf. |
> | Action | Microsoft.EventHub/clusters/operationresults/read | Ruft den Status eines asynchronen Clustervorgangs ab |
> | Action | Microsoft.EventHub/clusters/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Liste der Ressourcenbeschreibungen für Clustermetriken ab. |
> | Action | Microsoft.EventHub/clusters/read | Ruft die Beschreibung der Clusterressource ab. |
> | Action | Microsoft.EventHub/clusters/write | Erstellt oder bearbeitet eine vorhandene Clusterressource |
> | Action | Microsoft.EventHub/locations/deleteVirtualNetworkOrSubnets/action | Löscht die VNET-Regeln im EventHub-Ressourcenanbieter für das angegebene VNET. |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/action | Dient zum Aktualisieren von Namespace-Autorisierungsregeln. Diese API ist veraltet. Verwenden Sie stattdessen einen PUT-Aufruf, um die Namespace-Autorisierungsregel zu aktualisieren. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/delete | Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden.  |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/listkeys/action | Dient zum Abrufen der Verbindungszeichenfolge für den Namespace. |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln. |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/write | Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel. |
> | Action | Microsoft.EventHub/namespaces/Delete | Dient zum Löschen von Namespaceressourcen. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Ruft die Schlüssel der Autorisierungsregeln für den primären Namespace für die Notfallwiederherstellung. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/read | Ruft die Autorisierungsregeln des primären Namespace für die Notfallwiederherstellung ab. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/breakPairing/action | Hiermit wird die Notfallwiederherstellung deaktiviert und die Replikation von Änderungen vom primären Namespace zu sekundären Namespaces beendet. |
> | Action | Microsoft.EventHub/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespacealias unter dem angegebenen Abonnement. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/delete | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration gelöscht. Dieser Vorgang kann nur über den primären Namespace aufgerufen werden. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/failover/action | Hiermit wird ein Failover bei der georedundanten Notfallwiederherstellung ausgelöst und der Namespacealias neu konfiguriert, um auf den sekundären Namespace zu zeigen. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/read | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration abgerufen. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/write | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration erstellt oder aktualisiert. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/action | Vorgang zum Aktualisieren von EventHub. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/delete | Vorgang zum Löschen von EventHub-Autorisierungsregeln. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/listkeys/action | Dient zum Abrufen der EventHub-Verbindungszeichenfolge. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/read |  Dient zum Abrufen der Liste mit EventHub-Autorisierungsregeln. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/write | Dient zum Erstellen von EventHub-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden. |
> | Action | Microsoft.EventHub/namespaces/eventHubs/consumergroups/Delete | Vorgang zum Löschen von ConsumerGroup-Ressourcen. |
> | Action | Microsoft.EventHub/namespaces/eventHubs/consumergroups/read | Dient zum Abrufen einer Liste mit ConsumerGroup-Ressourcenbeschreibungen. |
> | Action | Microsoft.EventHub/namespaces/eventHubs/consumergroups/write | Dient zum Erstellen oder Aktualisieren von ConsumerGroup-Eigenschaften. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/Delete | Vorgang zum Löschen von EventHub-Ressourcen. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/read | Dient zum Abrufen einer Liste mit EventHub-Ressourcenbeschreibungen. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/write | Dient zum Erstellen oder Aktualisieren von EventHub-Eigenschaften. |
> | Action | Microsoft.EventHub/namespaces/ipFilterRules/delete | Löscht eine IP-Filterressource. |
> | Action | Microsoft.EventHub/namespaces/ipFilterRules/read | Ruft eine IP-Filterressource ab. |
> | Action | Microsoft.EventHub/namespaces/ipFilterRules/write | Erstellt eine IP-Filterressource. |
> | DataAction | Microsoft.EventHub/namespaces/messages/receive/action | Empfangen von Nachrichten |
> | DataAction | Microsoft.EventHub/namespaces/messages/send/action | Senden von Nachrichten |
> | Action | Microsoft.EventHub/namespaces/messagingPlan/read | Hiermit wird der Messagingplan für einen Namespace abgerufen.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Action | Microsoft.EventHub/namespaces/messagingPlan/write | Hiermit wird der Messagingplan für einen Namespace aktualisiert.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Action | Microsoft.EventHub/namespaces/networkruleset/delete | Löscht eine VNET-Regelressource. |
> | Action | Microsoft.EventHub/namespaces/networkruleset/read | Ruft die NetworkRuleSet-Ressource ab |
> | Action | Microsoft.EventHub/namespaces/networkruleset/write | Erstellt eine VNET-Regelressource. |
> | Action | Microsoft.EventHub/namespaces/networkrulesets/delete | Löscht eine VNET-Regelressource. |
> | Action | Microsoft.EventHub/namespaces/networkrulesets/read | Ruft die NetworkRuleSet-Ressource ab |
> | Action | Microsoft.EventHub/namespaces/networkrulesets/write | Erstellt eine VNET-Regelressource. |
> | Action | Microsoft.EventHub/namespaces/operationresults/read | Hiermit wird der Status des Namespacevorgangs abgerufen. |
> | Action | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen. |
> | Action | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen. |
> | Action | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/logDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespaceprotokolle. |
> | Action | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken. |
> | Action | Microsoft.EventHub/namespaces/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen. |
> | Action | Microsoft.EventHub/namespaces/removeAcsNamepsace/action | Entfernt den ACS-Namespace. |
> | Action | Microsoft.EventHub/namespaces/virtualNetworkRules/delete | Löscht eine VNET-Regelressource. |
> | Action | Microsoft.EventHub/namespaces/virtualNetworkRules/read | Ruft eine VNET-Regelressource ab. |
> | Action | Microsoft.EventHub/namespaces/virtualNetworkRules/write | Erstellt eine VNET-Regelressource. |
> | Action | Microsoft.EventHub/namespaces/write | Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden. |
> | Action | Microsoft.EventHub/operations/read | Dient zum Abrufen von Vorgängen. |
> | Action | Microsoft.EventHub/register/action | Registriert das Abonnement für den EventHub-Ressourcenanbieter und ermöglicht die Erstellung von EventHub-Ressourcen. |
> | Action | Microsoft.EventHub/sku/read | Dient zum Abrufen einer Liste mit SKU-Ressourcenbeschreibungen. |
> | Action | Microsoft.EventHub/sku/regions/read | Dient zum Abrufen einer Liste mit SkuRegions-Ressourcenbeschreibungen. |
> | Action | Microsoft.EventHub/unregister/action | Registriert den EventHub-Ressourcenanbieter. |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Features/features/read | Ruft die Features eines Abonnements ab. |
> | Action | Microsoft.Features/operations/read | Ruft die Liste der Vorgänge ab. |
> | Action | Microsoft.Features/providers/features/read | Ruft das Feature eines Abonnements in einem angegebenen Ressourcenanbieter ab. |
> | Action | Microsoft.Features/providers/features/register/action | Registriert das Feature für ein Abonnement in einem angegebenen Ressourcenanbieter. |
> | Action | Microsoft.Features/providers/features/unregister/action | Hiermit wird die Registrierung des Features für ein Abonnement in einem vorgegebenen Ressourcenanbieter aufgehoben. |
> | Action | Microsoft.Features/register/action | Registriert das Feature eines Abonnements. |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.GuestConfiguration/guestConfigurationAssignments/delete | Löscht die Zuweisung der Gastkonfiguration |
> | Action | Microsoft.GuestConfiguration/guestConfigurationAssignments/read | Abrufen der Zuweisung der Gastkonfiguration |
> | Action | Microsoft.GuestConfiguration/guestConfigurationAssignments/reports/read | Rufen den Gastkonfiguration-Zuweisungsbericht ab. |
> | Action | Microsoft.GuestConfiguration/guestConfigurationAssignments/write | Erstellen der Zuweisung einer neuen Gastkonfiguration |
> | Action | Microsoft.GuestConfiguration/operations/read | Ruft die Vorgänge für den Ressourcenanbieter „Microsoft.GuestConfiguration“ ab. |
> | Action | Microsoft.GuestConfiguration/register/action | Registriert das Abonnement für den Ressourcenanbieter „Microsoft.GuestConfiguration“ |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.HDInsight/clusters/applications/delete | Dient zum Löschen der Anwendung für HDInsight-Cluster. |
> | Action | Microsoft.HDInsight/clusters/applications/read | Dient zum Abrufen der Anwendung für HDInsight-Cluster. |
> | Action | Microsoft.HDInsight/clusters/applications/write | Dient zum Erstellen oder Aktualisieren der Anwendung für HDInsight-Cluster. |
> | Action | Microsoft.HDInsight/clusters/changerdpsetting/action | Dient zum Ändern der RDP-Einstellung für HDInsight-Cluster. |
> | Action | Microsoft.HDInsight/clusters/configurations/action | Dient zum Aktualisieren der Konfiguration von HDInsight-Clustern. |
> | Action | Microsoft.HDInsight/clusters/configurations/action | Dient zum Abrufen der Konfigurationen von HDInsight-Clustern. |
> | Action | Microsoft.HDInsight/clusters/configurations/read | Dient zum Abrufen der Konfigurationen von HDInsight-Clustern. |
> | Action | Microsoft.HDInsight/clusters/delete | Dient zum Löschen eines HDInsight-Clusters. |
> | Action | Microsoft.HDInsight/clusters/extensions/delete | Löscht die Clustererweiterung für HDInsight-Cluster |
> | Action | Microsoft.HDInsight/clusters/extensions/read | Ruft die Clustererweiterung für HDInsight-Cluster ab |
> | Action | Microsoft.HDInsight/clusters/extensions/write | Erstellt die Clustererweiterung für HDInsight-Cluster |
> | Action | Microsoft.HDInsight/clusters/getGatewaySettings/action | Ruft Gatewayeinstellungen für HDInsight-Cluster ab. |
> | Action | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für den HDInsight-Cluster der Ressource ab. |
> | Action | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für den HDInsight-Cluster der Ressource. |
> | Action | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für den HDInsight-Cluster ab. |
> | Action | Microsoft.HDInsight/clusters/read | Dient zum Abrufen von Details zu HDInsight-Clustern. |
> | Action | Microsoft.HDInsight/clusters/roles/resize/action | Dient zum Anpassen der Größe eines HDInsight-Clusters. |
> | Action | Microsoft.HDInsight/clusters/updateGatewaySettings/action | Aktualisiert Gatewayeinstellungen für HDInsight-Cluster. |
> | Action | Microsoft.HDInsight/clusters/write | Dient zum Erstellen oder Aktualisieren von HDInsight-Clustern. |
> | Action | Microsoft.HDInsight/locations/capabilities/read | Dient zum Abrufen von Abonnementfunktionen. |
> | Action | Microsoft.HDInsight/locations/checkNameAvailability/read | Dient zum Prüfen der Verfügbarkeit des Namens. |
> | Action | Microsoft.HDInsight/register/action | Dient zum Registrieren des HDInsight-Ressourcenanbieters für das Abonnement |
> | Action | Microsoft.HDInsight/unregister/action | Dient zum Aufheben der Registrierung des HDInsight-Ressourcenanbieters für das Abonnement |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ImportExport/jobs/delete | Löscht einen vorhandenen Auftrag. |
> | Action | Microsoft.ImportExport/jobs/listBitLockerKeys/action | Ruft die BitLocker-Schlüssel für den angegebenen Auftrag ab. |
> | Action | Microsoft.ImportExport/jobs/read | Ruft die Eigenschaften für den angegebenen Auftrag ab oder gibt die Auftragsliste zurück. |
> | Action | Microsoft.ImportExport/jobs/write | Erstellt einen Auftrag mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für den angegebenen Auftrag. |
> | Action | Microsoft.ImportExport/locations/read | Ruft die Eigenschaften für den angegebenen Standort ab oder gibt die Standortliste zurück. |
> | Action | Microsoft.ImportExport/operations/read | Ruft die vom Ressourcenanbieter unterstützten Vorgänge ab. |
> | Action | Microsoft.ImportExport/register/action | Registriert das Abonnement für den Import/Export-Ressourcenanbieter und ermöglicht die Erstellung von Import/Exportaufträgen. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Insights/ActionGroups/Delete | Löscht eine Aktionsgruppe. |
> | Action | Microsoft.Insights/ActionGroups/Read | Liest eine Aktionsgruppe. |
> | Action | Microsoft.Insights/ActionGroups/Write | Erstellt oder aktualisiert eine Aktionsgruppe. |
> | Action | Microsoft.Insights/ActivityLogAlerts/Activated/Action | Aktivitätsprotokollwarnung aktiviert |
> | Action | Microsoft.Insights/ActivityLogAlerts/Delete | Löscht eine Aktivitätsprotokollwarnung. |
> | Action | Microsoft.Insights/ActivityLogAlerts/Read | Liest eine Aktivitätsprotokollwarnung. |
> | Action | Microsoft.Insights/ActivityLogAlerts/Write | Erstellt oder aktualisiert eine Aktivitätsprotokollwarnung. |
> | Action | Microsoft.Insights/AlertRules/Activated/Action | Klassische Metrikwarnung aktiviert |
> | Action | Microsoft.Insights/AlertRules/Delete | Löscht eine klassische Metrikwarnung. |
> | Action | Microsoft.Insights/AlertRules/Incidents/Read | Liest eine klassische Metrikwarnung zu einem Vorfall. |
> | Action | Microsoft.Insights/AlertRules/Read | Liest eine klassische Metrikwarnung. |
> | Action | Microsoft.Insights/AlertRules/Resolved/Action | Klassische Metrikwarnung gelöst |
> | Action | Microsoft.Insights/AlertRules/Throttled/Action | Klassische Metrikwarnungsregel gedrosselt |
> | Action | Microsoft.Insights/AlertRules/Write | Erstellt oder aktualisiert eine klassische Metrikwarnung. |
> | Action | Microsoft.Insights/AutoscaleSettings/Delete | Löscht eine Einstellung zur automatischen Skalierung. |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Read | Liest eine Diagnoseeinstellung für eine Ressource. |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Write | Erstellt oder aktualisiert eine Diagnoseeinstellung für eine Ressource. |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/logDefinitions/Read | Dient zum Lesen von Protokolldefinitionen. |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Action | Microsoft.Insights/AutoscaleSettings/Read | Liest eine Einstellung zur automatischen Skalierung. |
> | Action | Microsoft.Insights/AutoscaleSettings/Scaledown/Action | Automatisches zentrales Herunterskalieren ausgelöst |
> | Action | Microsoft.Insights/AutoscaleSettings/ScaledownResult/Action | Automatisches zentrales Herunterskalieren abgeschlossen |
> | Action | Microsoft.Insights/AutoscaleSettings/Scaleup/Action | Automatisches zentrales Hochskalieren ausgelöst |
> | Action | Microsoft.Insights/AutoscaleSettings/ScaleupResult/Action | Automatisches zentrales Hochskalieren abgeschlossen |
> | Action | Microsoft.Insights/AutoscaleSettings/Write | Erstellt oder aktualisiert eine Einstellung zur automatischen Skalierung. |
> | Action | Microsoft.Insights/Baseline/Read | Metrikbaseline lesen (Vorschau) |
> | Action | Microsoft.Insights/CalculateBaseline/Read | Baseline für Metrikwerte berechnen (Vorschau) |
> | Action | Microsoft.Insights/Components/AnalyticsItems/Delete | Hiermit wird ein Application Insights-Analyseelement gelöscht. |
> | Action | Microsoft.Insights/Components/AnalyticsItems/Read | Hiermit wird ein Application Insights-Analyseelement gelesen. |
> | Action | Microsoft.Insights/Components/AnalyticsItems/Write | Hiermit wird ein Application Insights-Analyseelement geschrieben. |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Action | Application Insights-Analysetabellenaktion |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Delete | Hiermit wird ein Application Insights-Analysetabellenschema gelöscht. |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Read | Hiermit wird ein Application Insights-Analysetabellenschema gelesen. |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Write | Hiermit wird ein Application Insights-Analysetabellenschema geschrieben. |
> | Action | Microsoft.Insights/Components/Annotations/Delete | Hiermit wird eine Application Insights-Anmerkung gelöscht. |
> | Action | Microsoft.Insights/Components/Annotations/Read | Hiermit wird eine Application Insights-Anmerkung gelesen. |
> | Action | Microsoft.Insights/Components/Annotations/Write | Hiermit wird eine Application Insights-Anmerkung geschrieben. |
> | Action | Microsoft.Insights/Components/Api/Read | Hiermit wird die Daten-API für Application Insights-Komponenten gelesen. |
> | Action | Microsoft.Insights/Components/ApiKeys/Action | Hiermit wird ein Application Insights-API-Schlüssel generiert. |
> | Action | Microsoft.Insights/Components/ApiKeys/Delete | Hiermit wird ein Application Insights-API-Schlüssel gelöscht. |
> | Action | Microsoft.Insights/Components/ApiKeys/Read | Hiermit wird ein Application Insights-API-Schlüssel gelesen. |
> | Action | Microsoft.Insights/Components/BillingPlanForComponent/Read | Hiermit wird ein Abrechnungsplan für eine Application Insights-Komponente gelesen. |
> | Action | Microsoft.Insights/Components/CurrentBillingFeatures/Read | Hiermit werden die aktuellen Abrechnungsfeatures für eine Application Insights-Komponente gelesen. |
> | Action | Microsoft.Insights/Components/CurrentBillingFeatures/Write | Hiermit werden die aktuellen Abrechnungsfeatures für eine Application Insights-Komponente geschrieben. |
> | Action | Microsoft.Insights/Components/DailyCapReached/Action | Das Tageslimit für die Application Insights-Komponente wurde erreicht. |
> | Action | Microsoft.Insights/Components/DailyCapWarningThresholdReached/Action | Der Warnungsschwellenwert für das Tageslimit der Application Insights-Komponente wurde erreicht. |
> | Action | Microsoft.Insights/Components/DefaultWorkItemConfig/Read | Hiermit wird die standardmäßige ALM-Integrationskonfiguration von Application Insights gelesen. |
> | Action | Microsoft.Insights/Components/Delete | Hiermit wird die Konfiguration der Application Insights-Komponente gelöscht. |
> | Action | Microsoft.Insights/Components/Events/Read | Hiermit rufen Sie mithilfe des OData-Abfrageformats Protokolle aus Application Insights ab. |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Action | Application Insights-Exporteinstellungsaktion |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Delete | Hiermit werden die Application Insights-Exporteinstellungen gelöscht. |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Read | Hiermit werden die Application Insights-Exporteinstellungen gelesen. |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Write | Hiermit werden die Application Insights-Exporteinstellungen geschrieben. |
> | Action | Microsoft.Insights/Components/ExtendQueries/Read | Hiermit werden die erweiterten Abfrageergebnisse der Application Insights-Komponenten gelesen. |
> | Action | Microsoft.Insights/Components/Favorites/Delete | Hiermit wird ein Application Insights-Favorit gelöscht. |
> | Action | Microsoft.Insights/Components/Favorites/Read | Hiermit wird ein Application Insights-Favorit gelesen. |
> | Action | Microsoft.Insights/Components/Favorites/Write | Hiermit wird ein Application Insights-Favorit geschrieben. |
> | Action | Microsoft.Insights/Components/FeatureCapabilities/Read | Hiermit werden die Features der Application Insights-Komponente gelesen. |
> | Action | Microsoft.Insights/Components/GetAvailableBillingFeatures/Read | Hiermit werden die verfügbaren Abrechnungsfeatures für eine Application Insights-Komponente gelesen. |
> | Action | Microsoft.Insights/Components/GetToken/Read | Hiermit wird ein Token einer Application Insights-Komponente gelesen. |
> | Action | Microsoft.Insights/Components/MetricDefinitions/Read | Hiermit werden die Metrikdefinitionen für Application Insights-Komponenten gelesen. |
> | Action | Microsoft.Insights/Components/Metrics/Read | Hiermit werden die Metriken für Application Insights-Komponenten gelesen. |
> | Action | Microsoft.Insights/Components/Move/Action | Hiermit wird eine Application Insights-Komponente auf eine andere Ressourcengruppe oder ein anderes Abonnement migriert. |
> | Action | Microsoft.Insights/Components/MyAnalyticsItems/Delete | Hiermit wird ein persönliches Application Insights-Analyseelement gelöscht. |
> | Action | Microsoft.Insights/Components/MyAnalyticsItems/Read | Hiermit wird ein persönliches Application Insights-Analyseelement gelesen. |
> | Action | Microsoft.Insights/Components/MyAnalyticsItems/Write | Hiermit wird ein persönliches Application Insights-Analyseelement geschrieben. |
> | Action | Microsoft.Insights/Components/MyFavorites/Read | Hiermit wird ein persönlicher Application Insights-Favorit gelesen. |
> | Action | Microsoft.Insights/Components/Operations/Read | Hiermit rufen Sie den Status von Vorgängen mit langer Ausführungszeit in Application Insights ab. |
> | Action | Microsoft.Insights/Components/PricingPlans/Read | Hiermit wird ein Tarif für eine Application Insights-Komponente gelesen. |
> | Action | Microsoft.Insights/Components/PricingPlans/Write | Hiermit wird ein Tarif für eine Application Insights-Komponente geschrieben. |
> | Action | Microsoft.Insights/Components/ProactiveDetectionConfigs/Read | Hiermit wird die Konfiguration von Proactive Detection in Application Insights gelesen. |
> | Action | Microsoft.Insights/Components/ProactiveDetectionConfigs/Write | Hiermit wird die Konfiguration von Proactive Detection in Application Insights geschrieben. |
> | Action | Microsoft.Insights/Components/providers/Microsoft.Insights/MetricDefinitions/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Action | Microsoft.Insights/Components/Purge/Action | Daten werden aus Application Insights gelöscht |
> | Action | Microsoft.Insights/Components/Query/Read | Hiermit führen Sie Abfragen für Application Insights-Protokolle aus. |
> | Action | Microsoft.Insights/Components/QuotaStatus/Read | Hiermit wird der Kontingentstatus der Application Insights-Komponente gelesen. |
> | Action | Microsoft.Insights/Components/Read | Hiermit wird die Konfiguration der Application Insights-Komponente gelesen. |
> | Action | Microsoft.Insights/Components/SyntheticMonitorLocations/Read | Hiermit werden die Webteststandorte von Application Insights gelesen. |
> | Action | Microsoft.Insights/Components/Webtests/Read | Hiermit wird eine Webtestkonfiguration gelesen. |
> | Action | Microsoft.Insights/Components/WorkItemConfigs/Delete | Hiermit wird die standardmäßige ALM-Integrationskonfiguration von Application Insights gelöscht. |
> | Action | Microsoft.Insights/Components/WorkItemConfigs/Read | Hiermit wird die ALM-Integrationskonfiguration von Application Insights gelesen. |
> | Action | Microsoft.Insights/Components/WorkItemConfigs/Write | Hiermit wird die ALM-Integrationskonfiguration von Application Insights geschrieben. |
> | Action | Microsoft.Insights/Components/Write | Hiermit wird in eine Application Insights-Komponentenkonfiguration geschrieben. |
> | Action | Microsoft.Insights/DataCollectionRuleAssociations/Delete | Löscht die Zuordnung einer Ressource zu einer Datensammlungsregel. |
> | Action | Microsoft.Insights/DataCollectionRuleAssociations/Read | Liest die Zuordnung einer Ressource zu einer Datensammlungsregel. |
> | Action | Microsoft.Insights/DataCollectionRuleAssociations/Write | Erstellt oder aktualisiert die Zuordnung einer Ressource zu einer Datensammlungsregel. |
> | DataAction | Microsoft.Insights/DataCollectionRules/Data/Write | Sendet Daten an eine Datensammlungsregel. |
> | Action | Microsoft.Insights/DataCollectionRules/Delete | Löscht eine Datensammlungsregel. |
> | Action | Microsoft.Insights/DataCollectionRules/Read | Liest eine Datensammlungsregel. |
> | Action | Microsoft.Insights/DataCollectionRules/Write | Erstellt oder aktualisiert eine Datensammlungsregel. |
> | Action | Microsoft.Insights/DiagnosticSettings/Delete | Löscht eine Diagnoseeinstellung für eine Ressource. |
> | Action | Microsoft.Insights/DiagnosticSettings/Read | Liest eine Diagnoseeinstellung für eine Ressource. |
> | Action | Microsoft.Insights/DiagnosticSettings/Write | Erstellt oder aktualisiert eine Diagnoseeinstellung für eine Ressource. |
> | Action | Microsoft.Insights/EventCategories/Read | Liest im Aktivitätsprotokoll verfügbare Ereigniskategorien. |
> | Action | Microsoft.Insights/eventtypes/digestevents/Read | Dient zum Lesen des Verwaltungsereignistyp-Digests. |
> | Action | Microsoft.Insights/eventtypes/values/Read | Liest Aktivitätsprotokollereignisse. |
> | Action | Microsoft.Insights/ExtendedDiagnosticSettings/Delete | Löscht eine Einstellung für die Netzwerkflussdiagnose. |
> | Action | Microsoft.Insights/ExtendedDiagnosticSettings/Read | Liest eine Einstellung für die Netzwerkflussdiagnose. |
> | Action | Microsoft.Insights/ExtendedDiagnosticSettings/Write | Erstellt oder aktualisiert eine Einstellung für die Netzwerkflussdiagnose. |
> | Action | Microsoft.Insights/ListMigrationDate/Action | Hiermit wird das Migrationsdatum für das Abonnement zurückgegeben. |
> | Action | Microsoft.Insights/ListMigrationDate/Read | Hiermit wird das Migrationsdatum für das Abonnement zurückgegeben. |
> | Action | Microsoft.Insights/LogDefinitions/Read | Dient zum Lesen von Protokolldefinitionen. |
> | Action | Microsoft.Insights/LogProfiles/Delete | Löscht ein Aktivitätsprotokollprofil. |
> | Action | Microsoft.Insights/LogProfiles/Read | Liest ein Aktivitätsprotokollprofil. |
> | Action | Microsoft.Insights/LogProfiles/Write | Erstellt oder aktualisiert ein Aktivitätsprotokollprofil. |
> | Action | Microsoft.Insights/Logs/ADAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der ADAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/ADReplicationResult/Read | Hiermit lesen Sie Daten aus der ADReplicationResult-Tabelle. |
> | Action | Microsoft.Insights/Logs/ADSecurityAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der ADSecurityAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/Alert/Read | Hiermit lesen Sie Daten aus der Alert-Tabelle. |
> | Action | Microsoft.Insights/Logs/AlertHistory/Read | Hiermit lesen Sie Daten aus der AlertHistory-Tabelle. |
> | Action | Microsoft.Insights/Logs/ApplicationInsights/Read | Hiermit lesen Sie Daten aus der ApplicationInsights-Tabelle. |
> | Action | Microsoft.Insights/Logs/AzureActivity/Read | Hiermit lesen Sie Daten aus der AzureActivity-Tabelle. |
> | Action | Microsoft.Insights/Logs/AzureMetrics/Read | Hiermit lesen Sie Daten aus der AzureMetrics-Tabelle. |
> | Action | Microsoft.Insights/Logs/BoundPort/Read | Hiermit lesen Sie Daten aus der BoundPort-Tabelle. |
> | Action | Microsoft.Insights/Logs/CommonSecurityLog/Read | Hiermit lesen Sie Daten aus der CommonSecurityLog-Tabelle. |
> | Action | Microsoft.Insights/Logs/ComputerGroup/Read | Hiermit lesen Sie Daten aus der ComputerGroup-Tabelle. |
> | Action | Microsoft.Insights/Logs/ConfigurationChange/Read | Hiermit lesen Sie Daten aus der ConfigurationChange-Tabelle. |
> | Action | Microsoft.Insights/Logs/ConfigurationData/Read | Hiermit lesen Sie Daten aus der ConfigurationData-Tabelle. |
> | Action | Microsoft.Insights/Logs/ContainerImageInventory/Read | Hiermit lesen Sie Daten aus der ContainerImageInventory-Tabelle. |
> | Action | Microsoft.Insights/Logs/ContainerInventory/Read | Hiermit lesen Sie Daten aus der ContainerInventory-Tabelle. |
> | Action | Microsoft.Insights/Logs/ContainerLog/Read | Hiermit lesen Sie Daten aus der ContainerLog-Tabelle. |
> | Action | Microsoft.Insights/Logs/ContainerServiceLog/Read | Hiermit lesen Sie Daten aus der ContainerServiceLog-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceAppCrash/Read | Hiermit lesen Sie Daten aus der DeviceAppCrash-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceAppLaunch/Read | Hiermit lesen Sie Daten aus der DeviceAppLaunch-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceCalendar/Read | Hiermit lesen Sie Daten aus der DeviceCalendar-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceCleanup/Read | Hiermit lesen Sie Daten aus der DeviceCleanup-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceConnectSession/Read | Hiermit lesen Sie Daten aus der DeviceConnectSession-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceEtw/Read | Hiermit lesen Sie Daten aus der DeviceEtw-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceHardwareHealth/Read | Hiermit lesen Sie Daten aus der DeviceHardwareHealth-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceHealth/Read | Hiermit lesen Sie Daten aus der DeviceHealth-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceHeartbeat/Read | Hiermit lesen Sie Daten aus der DeviceHeartbeat-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceSkypeHeartbeat/Read | Hiermit lesen Sie Daten aus der DeviceSkypeHeartbeat-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceSkypeSignIn/Read | Hiermit lesen Sie Daten aus der DeviceSkypeSignIn-Tabelle. |
> | Action | Microsoft.Insights/Logs/DeviceSleepState/Read | Hiermit lesen Sie Daten aus der DeviceSleepState-Tabelle. |
> | Action | Microsoft.Insights/Logs/DHAppFailure/Read | Hiermit lesen Sie Daten aus der DHAppFailure-Tabelle. |
> | Action | Microsoft.Insights/Logs/DHAppReliability/Read | Hiermit lesen Sie Daten aus der DHAppReliability-Tabelle. |
> | Action | Microsoft.Insights/Logs/DHDriverReliability/Read | Hiermit lesen Sie Daten aus der DHDriverReliability-Tabelle. |
> | Action | Microsoft.Insights/Logs/DHLogonFailures/Read | Hiermit lesen Sie Daten aus der DHLogonFailures-Tabelle. |
> | Action | Microsoft.Insights/Logs/DHLogonMetrics/Read | Hiermit lesen Sie Daten aus der DHLogonMetrics-Tabelle. |
> | Action | Microsoft.Insights/Logs/DHOSCrashData/Read | Hiermit lesen Sie Daten aus der DHOSCrashData-Tabelle. |
> | Action | Microsoft.Insights/Logs/DHOSReliability/Read | Hiermit lesen Sie Daten aus der DHOSReliability-Tabelle. |
> | Action | Microsoft.Insights/Logs/DHWipAppLearning/Read | Hiermit lesen Sie Daten aus der DHWipAppLearning-Tabelle. |
> | Action | Microsoft.Insights/Logs/DnsEvents/Read | Hiermit lesen Sie Daten aus der DnsEvents-Tabelle. |
> | Action | Microsoft.Insights/Logs/DnsInventory/Read | Hiermit lesen Sie Daten aus der DnsInventory-Tabelle. |
> | Action | Microsoft.Insights/Logs/ETWEvent/Read | Hiermit lesen Sie Daten aus der ETWEvent-Tabelle. |
> | Action | Microsoft.Insights/Logs/Event/Read | Hiermit lesen Sie Daten aus der Event-Tabelle. |
> | Action | Microsoft.Insights/Logs/ExchangeAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der ExchangeAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/ExchangeOnlineAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der ExchangeOnlineAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/Heartbeat/Read | Hiermit lesen Sie Daten aus der Heartbeat-Tabelle. |
> | Action | Microsoft.Insights/Logs/IISAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der IISAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/InboundConnection/Read | Hiermit lesen Sie Daten aus der InboundConnection-Tabelle. |
> | Action | Microsoft.Insights/Logs/KubeNodeInventory/Read | Hiermit lesen Sie Daten aus der KubeNodeInventory-Tabelle. |
> | Action | Microsoft.Insights/Logs/KubePodInventory/Read | Hiermit lesen Sie Daten aus der KubePodInventory-Tabelle. |
> | Action | Microsoft.Insights/Logs/LinuxAuditLog/Read | Hiermit lesen Sie Daten aus der LinuxAuditLog-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAApplication/Read | Hiermit lesen Sie Daten aus der MAApplication-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAApplicationHealth/Read | Hiermit lesen Sie Daten aus der MAApplicationHealth-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAApplicationHealthAlternativeVersions/Read | Liest Daten aus der Tabelle „MAApplicationHealthAlternativeVersions“. |
> | Action | Microsoft.Insights/Logs/MAApplicationHealthIssues/Read | Liest Daten aus der Tabelle „MAApplicationHealthIssues“. |
> | Action | Microsoft.Insights/Logs/MAApplicationInstance/Read | Hiermit lesen Sie Daten aus der MAApplicationInstance-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAApplicationInstanceReadiness/Read | Liest Daten aus der Tabelle „MAApplicationInstanceReadiness“. |
> | Action | Microsoft.Insights/Logs/MAApplicationReadiness/Read | Hiermit lesen Sie Daten aus der MAApplicationReadiness-Tabelle. |
> | Action | Microsoft.Insights/Logs/MADeploymentPlan/Read | Hiermit lesen Sie Daten aus der MADeploymentPlan-Tabelle. |
> | Action | Microsoft.Insights/Logs/MADevice/Read | Hiermit lesen Sie Daten aus der MADevice-Tabelle. |
> | Action | Microsoft.Insights/Logs/MADevicePnPHealth/Read | Liest Daten aus der Tabelle „MADevicePnPHealth“. |
> | Action | Microsoft.Insights/Logs/MADevicePnPHealthAlternativeVersions/Read | Liest Daten aus der Tabelle „MADevicePnPHealthAlternativeVersions“. |
> | Action | Microsoft.Insights/Logs/MADevicePnPHealthIssues/Read | Liest Daten aus der Tabelle „MADevicePnPHealthIssues“. |
> | Action | Microsoft.Insights/Logs/MADeviceReadiness/Read | Hiermit lesen Sie Daten aus der MADeviceReadiness-Tabelle. |
> | Action | Microsoft.Insights/Logs/MADriverInstanceReadiness/Read | Liest Daten aus der Tabelle „MADriverInstanceReadiness“. |
> | Action | Microsoft.Insights/Logs/MADriverReadiness/Read | Hiermit lesen Sie Daten aus der MADriverReadiness-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeAddin/Read | Hiermit lesen Sie Daten aus der MAOfficeAddin-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinHealth/Read | Hiermit lesen Sie Daten aus der MAOfficeAddinHealth-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinHealthIssues/Read | Liest Daten aus der Tabelle „MAOfficeAddinHealthIssues“. |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinInstance/Read | Hiermit lesen Sie Daten aus der MAOfficeAddinInstance-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinInstanceReadiness/Read | Liest Daten aus der Tabelle „MAOfficeAddinInstanceReadiness“. |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinReadiness/Read | Hiermit lesen Sie Daten aus der MAOfficeAddinReadiness-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeApp/Read | Hiermit lesen Sie Daten aus der MAOfficeApp-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeAppHealth/Read | Hiermit lesen Sie Daten aus der MAOfficeAppHealth-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeAppInstance/Read | Hiermit lesen Sie Daten aus der MAOfficeAppInstance-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeAppReadiness/Read | Hiermit lesen Sie Daten aus der MAOfficeAppReadiness-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeBuildInfo/Read | Hiermit lesen Sie Daten aus der MAOfficeBuildInfo-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeCurrencyAssessment/Read | Hiermit lesen Sie Daten aus der MAOfficeCurrencyAssessment-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeCurrencyAssessmentDailyCounts/Read | Hiermit lesen Sie Daten aus der MAOfficeCurrencyAssessmentDailyCounts-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeDeploymentStatus/Read | Hiermit lesen Sie Daten aus der MAOfficeDeploymentStatus-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroHealth/Read | Liest Daten aus der Tabelle „MAOfficeMacroHealth“. |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroHealthIssues/Read | Liest Daten aus der Tabelle „MAOfficeMacroHealthIssues“. |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroIssueInstanceReadiness/Read | Liest Daten aus der Tabelle „MAOfficeMacroIssueInstanceReadiness“. |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroIssueReadiness/Read | Hiermit lesen Sie Daten aus der MAOfficeMacroIssueReadiness-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroSummary/Read | Hiermit lesen Sie Daten aus der MAOfficeMacroSummary-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeSuite/Read | Hiermit lesen Sie Daten aus der MAOfficeSuite-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAOfficeSuiteInstance/Read | Hiermit lesen Sie Daten aus der MAOfficeSuiteInstance-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAProposedPilotDevices/Read | Hiermit lesen Sie Daten aus der MAProposedPilotDevices-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAWindowsBuildInfo/Read | Hiermit lesen Sie Daten aus der MAWindowsBuildInfo-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAWindowsCurrencyAssessment/Read | Hiermit lesen Sie Daten aus der MAWindowsCurrencyAssessment-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAWindowsCurrencyAssessmentDailyCounts/Read | Hiermit lesen Sie Daten aus der MAWindowsCurrencyAssessmentDailyCounts-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAWindowsDeploymentStatus/Read | Hiermit lesen Sie Daten aus der MAWindowsDeploymentStatus-Tabelle. |
> | Action | Microsoft.Insights/Logs/MAWindowsSysReqInstanceReadiness/Read | Liest Daten aus der Tabelle „MAWindowsSysReqInstanceReadiness“. |
> | Action | Microsoft.Insights/Logs/NetworkMonitoring/Read | Hiermit lesen Sie Daten aus der NetworkMonitoring-Tabelle. |
> | Action | Microsoft.Insights/Logs/OfficeActivity/Read | Hiermit lesen Sie Daten aus der OfficeActivity-Tabelle. |
> | Action | Microsoft.Insights/Logs/Operation/Read | Hiermit lesen Sie Daten aus der Operation-Tabelle. |
> | Action | Microsoft.Insights/Logs/OutboundConnection/Read | Hiermit lesen Sie Daten aus der OutboundConnection-Tabelle. |
> | Action | Microsoft.Insights/Logs/Perf/Read | Hiermit lesen Sie Daten aus der Perf-Tabelle. |
> | Action | Microsoft.Insights/Logs/ProtectionStatus/Read | Hiermit lesen Sie Daten aus der ProtectionStatus-Tabelle. |
> | Action | Microsoft.Insights/Logs/Read | Hiermit lesen Sie Daten aus allen Ihren Protokollen. |
> | Action | Microsoft.Insights/Logs/ReservedAzureCommonFields/Read | Hiermit lesen Sie Daten aus der ReservedAzureCommonFields-Tabelle. |
> | Action | Microsoft.Insights/Logs/ReservedCommonFields/Read | Hiermit lesen Sie Daten aus der ReservedCommonFields-Tabelle. |
> | Action | Microsoft.Insights/Logs/SCCMAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der SCCMAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/SCOMAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der SCOMAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/SecurityAlert/Read | Hiermit lesen Sie Daten aus der SecurityAlert-Tabelle. |
> | Action | Microsoft.Insights/Logs/SecurityBaseline/Read | Hiermit lesen Sie Daten aus der SecurityBaseline-Tabelle. |
> | Action | Microsoft.Insights/Logs/SecurityBaselineSummary/Read | Hiermit lesen Sie Daten aus der SecurityBaselineSummary-Tabelle. |
> | Action | Microsoft.Insights/Logs/SecurityDetection/Read | Hiermit lesen Sie Daten aus der SecurityDetection-Tabelle. |
> | Action | Microsoft.Insights/Logs/SecurityEvent/Read | Hiermit lesen Sie Daten aus der SecurityEvent-Tabelle. |
> | Action | Microsoft.Insights/Logs/ServiceFabricOperationalEvent/Read | Hiermit lesen Sie Daten aus der ServiceFabricOperationalEvent-Tabelle. |
> | Action | Microsoft.Insights/Logs/ServiceFabricReliableActorEvent/Read | Hiermit lesen Sie Daten aus der ServiceFabricReliableActorEvent-Tabelle. |
> | Action | Microsoft.Insights/Logs/ServiceFabricReliableServiceEvent/Read | Hiermit lesen Sie Daten aus der ServiceFabricReliableServiceEvent-Tabelle. |
> | Action | Microsoft.Insights/Logs/SfBAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der SfBAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/SfBOnlineAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der SfBOnlineAssessmentRecommendatio-Tabelle. |
> | Action | Microsoft.Insights/Logs/SharePointOnlineAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der SharePointOnlineAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/SPAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der SPAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/SQLAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der SQLAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/SQLQueryPerformance/Read | Hiermit lesen Sie Daten aus der SQLQueryPerformance-Tabelle. |
> | Action | Microsoft.Insights/Logs/Syslog/Read | Hiermit lesen Sie Daten aus der Syslog-Tabelle. |
> | Action | Microsoft.Insights/Logs/SysmonEvent/Read | Hiermit lesen Sie Daten aus der SysmonEvent-Tabelle. |
> | Action | Microsoft.Insights/Logs/Tables.Custom/Read | Hiermit lesen Sie Daten aus einem beliebigen benutzerdefinierten Protokoll. |
> | Action | Microsoft.Insights/Logs/UAApp/Read | Hiermit lesen Sie Daten aus der UAApp-Tabelle. |
> | Action | Microsoft.Insights/Logs/UAComputer/Read | Hiermit lesen Sie Daten aus der UAComputer-Tabelle. |
> | Action | Microsoft.Insights/Logs/UAComputerRank/Read | Hiermit lesen Sie Daten aus der UAComputerRank-Tabelle. |
> | Action | Microsoft.Insights/Logs/UADriver/Read | Hiermit lesen Sie Daten aus der UADriver-Tabelle. |
> | Action | Microsoft.Insights/Logs/UADriverProblemCodes/Read | Hiermit lesen Sie Daten aus der UADriverProblemCodes-Tabelle. |
> | Action | Microsoft.Insights/Logs/UAFeedback/Read | Hiermit lesen Sie Daten aus der UAFeedback-Tabelle. |
> | Action | Microsoft.Insights/Logs/UAHardwareSecurity/Read | Hiermit lesen Sie Daten aus der UAHardwareSecurity-Tabelle. |
> | Action | Microsoft.Insights/Logs/UAIESiteDiscovery/Read | Hiermit lesen Sie Daten aus der UAIESiteDiscovery-Tabelle. |
> | Action | Microsoft.Insights/Logs/UAOfficeAddIn/Read | Hiermit lesen Sie Daten aus der UAOfficeAddIn-Tabelle. |
> | Action | Microsoft.Insights/Logs/UAProposedActionPlan/Read | Hiermit lesen Sie Daten aus der UAProposedActionPlan-Tabelle. |
> | Action | Microsoft.Insights/Logs/UASysReqIssue/Read | Hiermit lesen Sie Daten aus der UASysReqIssue-Tabelle. |
> | Action | Microsoft.Insights/Logs/UAUpgradedComputer/Read | Hiermit lesen Sie Daten aus der UAUpgradedComputer-Tabelle. |
> | Action | Microsoft.Insights/Logs/Update/Read | Hiermit lesen Sie Daten aus der Update-Tabelle. |
> | Action | Microsoft.Insights/Logs/UpdateRunProgress/Read | Hiermit lesen Sie Daten aus der UpdateRunProgress-Tabelle. |
> | Action | Microsoft.Insights/Logs/UpdateSummary/Read | Hiermit lesen Sie Daten aus der UpdateSummary-Tabelle. |
> | Action | Microsoft.Insights/Logs/Usage/Read | Hiermit lesen Sie Daten aus der Usage-Tabelle. |
> | Action | Microsoft.Insights/Logs/W3CIISLog/Read | Hiermit lesen Sie Daten aus der W3CIISLog-Tabelle. |
> | Action | Microsoft.Insights/Logs/WaaSDeploymentStatus/Read | Hiermit lesen Sie Daten aus der WaaSDeploymentStatus-Tabelle. |
> | Action | Microsoft.Insights/Logs/WaaSInsiderStatus/Read | Hiermit lesen Sie Daten aus der WaaSInsiderStatus-Tabelle. |
> | Action | Microsoft.Insights/Logs/WaaSUpdateStatus/Read | Hiermit lesen Sie Daten aus der WaaSUpdateStatus-Tabelle. |
> | Action | Microsoft.Insights/Logs/WDAVStatus/Read | Hiermit lesen Sie Daten aus der WDAVStatus-Tabelle. |
> | Action | Microsoft.Insights/Logs/WDAVThreat/Read | Hiermit lesen Sie Daten aus der WDAVThreat-Tabelle. |
> | Action | Microsoft.Insights/Logs/WindowsClientAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der WindowsClientAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/WindowsFirewall/Read | Hiermit lesen Sie Daten aus der WindowsFirewall-Tabelle. |
> | Action | Microsoft.Insights/Logs/WindowsServerAssessmentRecommendation/Read | Hiermit lesen Sie Daten aus der WindowsServerAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.Insights/Logs/WireData/Read | Hiermit lesen Sie Daten aus der WireData-Tabelle. |
> | Action | Microsoft.Insights/Logs/WUDOAggregatedStatus/Read | Hiermit lesen Sie Daten aus der WUDOAggregatedStatus-Tabelle. |
> | Action | Microsoft.Insights/Logs/WUDOStatus/Read | Hiermit lesen Sie Daten aus der WUDOStatus-Tabelle. |
> | Action | Microsoft.Insights/MetricAlerts/Delete | Löscht ein Metrikwarnung. |
> | Action | Microsoft.Insights/MetricAlerts/Read | Liest eine Metrikwarnung. |
> | Action | Microsoft.Insights/MetricAlerts/Status/Read | Liest den Metrikwarnungsstatus. |
> | Action | Microsoft.Insights/MetricAlerts/Write | Erstellt oder aktualisiert eine Metrikwarnung. |
> | Action | Microsoft.Insights/MetricBaselines/Read | Liest Metrikbaselines |
> | Action | Microsoft.Insights/MetricDefinitions/Microsoft.Insights/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Action | Microsoft.Insights/MetricDefinitions/providers/Microsoft.Insights/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Action | Microsoft.Insights/MetricDefinitions/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Action | Microsoft.Insights/Metricnamespaces/Read | Liest die Metriknamespaces |
> | Action | Microsoft.Insights/Metrics/Action | Metrikaktion |
> | Action | Microsoft.Insights/Metrics/Microsoft.Insights/Read | Dient zum Lesen von Metriken. |
> | Action | Microsoft.Insights/Metrics/providers/Metrics/Read | Dient zum Lesen von Metriken. |
> | Action | Microsoft.Insights/Metrics/Read | Dient zum Lesen von Metriken. |
> | DataAction | Microsoft.Insights/Metrics/Write | Hiermit werden Metriken geschrieben. |
> | Action | Microsoft.Insights/MigrateToNewpricingModel/Action | Hiermit wird ein Abonnement auf das neue Preismodell migriert. |
> | Action | Microsoft.Insights/Operations/Read | Dient zum Lesen von Vorgängen. |
> | Action | Microsoft.Insights/Register/Action | Dient zum Registrieren des Microsoft Insights-Anbieters. |
> | Action | Microsoft.Insights/RollbackToLegacyPricingModel/Action | Hiermit wird ein Rollback für das Abonnement auf das Legacypreismodell durchgeführt. |
> | Action | Microsoft.Insights/ScheduledQueryRules/Delete | Löscht eine geplante Abfrageregel. |
> | Action | Microsoft.Insights/ScheduledQueryRules/Read | Liest eine geplante Abfrageregel. |
> | Action | Microsoft.Insights/ScheduledQueryRules/Write | Schreibt eine geplante Abfrageregel. |
> | Action | Microsoft.Insights/Tenants/Register/Action | Hiermit wird der Microsoft Insights-Anbieter initialisiert. |
> | Action | Microsoft.Insights/Unregister/Action | Dient zum Registrieren des Microsoft Insights-Anbieters. |
> | Action | Microsoft.Insights/Webtests/Delete | Hiermit wird eine Webtestkonfiguration gelöscht. |
> | Action | Microsoft.Insights/Webtests/GetToken/Read | Hiermit wird ein Webtesttoken gelesen. |
> | Action | Microsoft.Insights/Webtests/MetricDefinitions/Read | Hiermit werden Webtestmetrikdefinitionen gelesen. |
> | Action | Microsoft.Insights/Webtests/Metrics/Read | Hiermit werden Webtestmetriken gelesen. |
> | Action | Microsoft.Insights/Webtests/Read | Hiermit wird eine Webtestkonfiguration gelesen. |
> | Action | Microsoft.Insights/Webtests/Write | Hiermit wird in eine Webtestkonfiguration geschrieben. |
> | Action | Microsoft.Insights/Workbooks/Delete | Löschen einer Arbeitsmappe |
> | Action | Microsoft.Insights/Workbooks/Read | Lesen einer Arbeitsmappe |
> | Action | Microsoft.Insights/Workbooks/Write | Erstellen oder Aktualisieren einer Arbeitsmappe |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Intune/diagnosticsettings/delete | Löscht eine Diagnoseeinstellung. |
> | Action | Microsoft.Intune/diagnosticsettings/read | Liest eine Diagnoseeinstellung. |
> | Action | Microsoft.Intune/diagnosticsettings/write | Schreibt eine Diagnoseeinstellung. |
> | Action | Microsoft.Intune/diagnosticsettingscategories/read | Liest die Kategorien einer Diagnoseeinstellung. |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.IoTCentral/appTemplates/action | Ruft alle verfügbaren Anwendungsvorlagen in Azure IoT Central ab |
> | Action | Microsoft.IoTCentral/checkNameAvailability/action | Überprüft, ob ein IoT Central-Anwendungsname verfügbar ist. |
> | Action | Microsoft.IoTCentral/checkSubdomainAvailability/action | Überprüft, ob eine IoT Central-Anwendungsunterdomäne verfügbar ist. |
> | Action | Microsoft.IoTCentral/IoTApps/delete | Löscht eine IoT Central-Anwendung. |
> | Action | Microsoft.IoTCentral/IoTApps/read | Ruft eine einzelne IoT Central-Anwendung ab. |
> | Action | Microsoft.IoTCentral/IoTApps/write | Erstellt oder aktualisiert eine IoT Central-Anwendung. |
> | Action | Microsoft.IoTCentral/operations/read | Ruft alle verfügbaren Vorgänge für IoT Central-Anwendungen ab. |
> | Action | Microsoft.IoTCentral/register/action | Registriert das Abonnement für den Azure IoT Central-Ressourcenanbieter. |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.IoTSpaces/Graph/delete | Löscht die Microsoft.IoTSpaces Graph-Ressource. |
> | Action | Microsoft.IoTSpaces/Graph/read | Ruft die Microsoft.IoTSpaces Graph-Ressource(n) ab. |
> | Action | Microsoft.IoTSpaces/Graph/write | Erstellt die Microsoft.IoTSpaces Graph-Ressource. |
> | Action | Microsoft.IoTSpaces/register/action | Dient zum Registrieren des Abonnements für den Microsoft.IoTSpaces Graph-Ressourcenanbieter, um die Erstellung von Ressourcen zu ermöglichen. |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.KeyVault/checkNameAvailability/read | Prüft, ob ein Schlüsseltresorname gültig ist und noch nicht verwendet wird. |
> | Action | Microsoft.KeyVault/deletedVaults/read | Dient zum Anzeigen der Eigenschaften vorläufig gelöschter Schlüsseltresore. |
> | Action | Microsoft.KeyVault/hsmPools/delete | Hiermit wird ein HSM-Pool gelöscht. |
> | Action | Microsoft.KeyVault/hsmPools/joinVault/action | Hiermit wird ein Schlüsseltresor zu einem HSM-Pool hinzugefügt. |
> | Action | Microsoft.KeyVault/hsmPools/read | Hiermit werden die Eigenschaften eines HSM-Pools angezeigt. |
> | Action | Microsoft.KeyVault/hsmPools/write | Hiermit wird durch das Aktualisieren der Eigenschaften eines vorhandenen HSM-Pools ein neuer HSM-Pool erstellt. |
> | Action | Microsoft.KeyVault/locations/deletedVaults/purge/action | Dient zum endgültigen Löschen eines vorläufig gelöschten Schlüsseltresors. |
> | Action | Microsoft.KeyVault/locations/deletedVaults/read | Dient zum Anzeigen der Eigenschaften eines vorläufig gelöschten Schlüsseltresors. |
> | Action | Microsoft.KeyVault/locations/deleteVirtualNetworkOrSubnets/action | Hiermit wird Microsoft.KeyVault darüber benachrichtigt, dass ein virtuelles Netzwerk oder Subnetz gelöscht wird. |
> | Action | Microsoft.KeyVault/locations/operationResults/read | Dient zum Überprüfen des Ergebnisses eines Vorgangs mit langer Ausführungszeit. |
> | Action | Microsoft.KeyVault/operations/read | Listet die verfügbaren Vorgänge für den Microsoft.KeyVault-Ressourcenanbieter auf. |
> | Action | Microsoft.KeyVault/register/action | Registriert ein Abonnement. |
> | Action | Microsoft.KeyVault/unregister/action | Hiermit wird die Registrierung für ein Abonnement aufgehoben. |
> | Action | Microsoft.KeyVault/vaults/accessPolicies/write | Dient zum Aktualisieren einer vorhandenen Zugriffsrichtlinie durch Zusammenführen oder Ersetzen oder zum Hinzufügen einer neuen Zugriffsrichtlinie zu einem Tresor. |
> | Action | Microsoft.KeyVault/vaults/delete | Dient zum Löschen eines Schlüsseltresors. |
> | Action | Microsoft.KeyVault/vaults/deploy/action | Ermöglicht den Zugriff auf Geheimnisse in einem Schlüsseltresor beim Bereitstellen von Azure-Ressourcen. |
> | Action | Microsoft.KeyVault/vaults/eventGridFilters/delete | Hiermit wird „Microsoft.KeyVault“ darüber benachrichtigt, dass ein EventGrid-Abonnement für Key Vault gelöscht wird. |
> | Action | Microsoft.KeyVault/vaults/eventGridFilters/read | Hiermit wird „Microsoft.KeyVault“ darüber benachrichtigt, dass ein EventGrid-Abonnement für Key Vault angezeigt wird. |
> | Action | Microsoft.KeyVault/vaults/eventGridFilters/write | Hiermit wird „Microsoft.KeyVault“ darüber benachrichtigt, dass ein EventGrid-Abonnement für Key Vault erstellt wird. |
> | Action | Microsoft.KeyVault/vaults/read | Dient zum Anzeigen der Eigenschaften eines Schlüsseltresors. |
> | Action | Microsoft.KeyVault/vaults/secrets/read | Zeigt die Eigenschaften (aber nicht den Wert) eines Geheimnisses an |
> | Action | Microsoft.KeyVault/vaults/secrets/write | Erstellt ein neues Geheimnis oder aktualisiert den Wert eines vorhandenen Geheimnisses |
> | Action | Microsoft.KeyVault/vaults/write | Dient zum Erstellen eines neuen Schlüsseltresors oder zum Aktualisieren der Eigenschaften einer vorhandenen Schlüsseltresors. |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Kusto/Clusters/Activate/action | Startet den Cluster. |
> | Action | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/delete | Löscht die resourceCopy einer angefügten Datenbankkonfiguration. |
> | Action | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/read | Liest die resourceCopy einer angefügten Datenbankkonfigurations auf. |
> | Action | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/write | Schreibt die resourceCopy einer angefügten Datenbankkonfiguration. |
> | Action | Microsoft.Kusto/Clusters/CheckNameAvailability/action | Überprüft die Verfügbarkeit des Clusternamens. |
> | Action | Microsoft.Kusto/Clusters/Databases/AddPrincipals/action | Fügt Datenbankprinzipale hinzu. |
> | Action | Microsoft.Kusto/Clusters/Databases/CheckNameAvailability/action | Prüft die Verfügbarkeit von Namen für einen bestimmten Typ. |
> | Action | Microsoft.Kusto/Clusters/Databases/DataConnections/delete | Löscht die resourceCopy einer Datenverbindung. |
> | Action | Microsoft.Kusto/Clusters/Databases/DataConnections/read | Liest die resourceCopy einer Datenverbindung. |
> | Action | Microsoft.Kusto/Clusters/Databases/DataConnections/write | Schreibt die resourceCopy einer Datenverbindung. |
> | Action | Microsoft.Kusto/Clusters/Databases/DataConnectionValidation/action | Überprüft eine Datenbankdatenverbindung. |
> | Action | Microsoft.Kusto/Clusters/Databases/delete | Löscht die resourceCopy einer Datenbank. |
> | Action | Microsoft.Kusto/Clusters/Databases/EventHubConnections/delete | Löscht die resourceCopy einer Event Hub-Verbindung. |
> | Action | Microsoft.Kusto/Clusters/Databases/EventHubConnections/read | Liest die resourceCopy einer Event Hub-Verbindung. |
> | Action | Microsoft.Kusto/Clusters/Databases/EventHubConnections/read | Schreibt die resourceCopy einer Event Hub-Verbindung. |
> | Action | Microsoft.Kusto/Clusters/Databases/EventHubConnectionValidation/action | Überprüft eine Datenbank-Event Hub-Verbindung. |
> | Action | Microsoft.Kusto/Clusters/Databases/ListPrincipals/action | Listet Datenbankprinzipale auf. |
> | Action | Microsoft.Kusto/Clusters/Databases/PrincipalAssignments/delete | Löscht die resourceCopy der Zuweisungen von Datenbankprinzipalen. |
> | Action | Microsoft.Kusto/Clusters/Databases/PrincipalAssignments/read | Liest die resourceCopy der Zuweisungen von Datenbankprinzipalen. |
> | Action | Microsoft.Kusto/Clusters/Databases/PrincipalAssignments/write | Schreibt die resourceCopy der Zuweisungen von Datenbankprinzipalen. |
> | Action | Microsoft.Kusto/Clusters/Databases/read | Liest die resourceCopy einer Datenbank. |
> | Action | Microsoft.Kusto/Clusters/Databases/RemovePrincipals/action | Entfernt Datenbankprinzipale. |
> | Action | Microsoft.Kusto/Clusters/Databases/write | Schreibt die resourceCopy einer Datenbank. |
> | Action | Microsoft.Kusto/Clusters/Deactivate/action | Beendet den Cluster. |
> | Action | Microsoft.Kusto/Clusters/delete | Löscht die resourceCopy eines Clusters. |
> | Action | Microsoft.Kusto/Clusters/DetachFollowerDatabases/action | Trennt die Datenbanken des Followers. |
> | Action | Microsoft.Kusto/Clusters/DiagnoseVirtualNetwork/action | Diagnostiziert den Status der Netzwerkkonnektivität für externe Ressourcen, von denen der Dienst abhängig ist. |
> | Action | Microsoft.Kusto/Clusters/ListFollowerDatabases/action | Listet die Datenbanken des Followers auf. |
> | Action | Microsoft.Kusto/Clusters/read | Liest die resourceCopy eines Clusters. |
> | Action | Microsoft.Kusto/Clusters/SKUs/read | Liest die resourceCopy einer Cluster-SKU. |
> | Action | Microsoft.Kusto/Clusters/Start/action | Startet den Cluster. |
> | Action | Microsoft.Kusto/Clusters/Stop/action | Beendet den Cluster. |
> | Action | Microsoft.Kusto/Clusters/write | Schreibt die resourceCopy eines Clusters. |
> | Action | Microsoft.Kusto/Locations/CheckNameAvailability/action | Überprüft die Verfügbarkeit des resourceCopy-Namens. |
> | Action | Microsoft.Kusto/Locations/GetNetworkPolicies/action | Ruft Netzwerkzielrichtlinien ab. |
> | Action | Microsoft.Kusto/locations/operationresults/read | Liest resourceCopys von Vorgängen. |
> | Action | Microsoft.Kusto/Operations/read | Liest resourceCopys von Vorgängen. |
> | Action | Microsoft.Kusto/register/action | Abonnementregistrierungsaktion |
> | Action | Microsoft.Kusto/Register/action | Registriert das Abonnement beim Kusto-Ressourcenanbieter. |
> | Action | Microsoft.Kusto/SKUs/read | Liest eine SKU-resourceCopy. |
> | Action | Microsoft.Kusto/Unregister/action | Hebt die Registrierung des Abonnements beim Kusto-Ressourcenanbieter auf. |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.LabServices/labAccounts/CreateLab/action | Hiermit wird ein Lab in einem Lab-Konto erstellt. |
> | Action | Microsoft.LabServices/labAccounts/delete | Hiermit werden Lab-Konten gelöscht. |
> | Action | Microsoft.LabServices/labAccounts/galleryImages/delete | Löscht Katalogimages. |
> | Action | Microsoft.LabServices/labAccounts/galleryImages/read | Dient zum Lesen von Katalogimages. |
> | Action | Microsoft.LabServices/labAccounts/galleryImages/write | Fügt Katalogimages hinzu oder ändert diese. |
> | Action | Microsoft.LabServices/labAccounts/GetRegionalAvailability/action | Hiermit werden Informationen zur regionalen Verfügbarkeit jeder Größenkategorie abgerufen, die in einem Labkonto konfiguriert sind. |
> | Action | Microsoft.LabServices/labAccounts/labs/AddUsers/action | Benutzer zu einem Lab hinzufügen |
> | Action | Microsoft.LabServices/labAccounts/labs/delete | Dient zum Löschen von Labs. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/delete | Hiermit wird eine Umgebungseinstellung gelöscht. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/delete | Dient zum Löschen von Umgebungen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/read | Dient zum Lesen von Umgebungen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/ResetPassword/action | Setzt das Kennwort des Benutzers für eine Umgebung zurück. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Start/action | Startet eine Umgebung, indem alle Ressourcen innerhalb der Umgebung gestartet werden. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Stop/action | Beendet eine Umgebung, indem alle Ressourcen innerhalb der Umgebung beendet werden. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/write | Dient zum Hinzufügen oder Ändern von Umgebungen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/Publish/action | Stellt die erforderlichen Ressourcen für eine Umgebungseinstellung anhand des aktuellen Status der Lab- bzw. Umgebungseinstellung bereit oder hebt deren Bereitstellung auf. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/read | Hiermit wird eine Umgebungseinstellung gelesen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/ResetPassword/action | Setzt das Kennwort der Vorlagen-VM zurück |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/SaveImage/action | Hiermit wird das aktuelle Vorlagenimage im freigegebenen Katalog im Labkonto gespeichert. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/delete | Dient zum Löschen von Zeitplänen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/read | Dient zum Lesen von Zeitplänen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/write | Dient zum Hinzufügen oder Ändern von Zeitplänen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/Start/action | Startet eine Vorlage, indem alle Ressourcen innerhalb der Vorlage gestartet werden. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/Stop/action | Beendet eine Vorlage, indem alle in der Vorlage enthaltenen Ressourcen beendet werden. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/write | Dient zum Hinzufügen oder Ändern einer Umgebungseinstellung. |
> | Action | Microsoft.LabServices/labAccounts/labs/read | Dient zum Lesen von Labs. |
> | Action | Microsoft.LabServices/labAccounts/labs/SendEmail/action | Sendet eine E-Mail mit dem Registrierungslink an das Lab. |
> | Action | Microsoft.LabServices/labAccounts/labs/users/delete | Hiermit werden Benutzer gelöscht. |
> | Action | Microsoft.LabServices/labAccounts/labs/users/read | Hiermit werden Benutzer gelesen. |
> | Action | Microsoft.LabServices/labAccounts/labs/users/write | Dient zum Hinzufügen oder Ändern von Benutzern. |
> | Action | Microsoft.LabServices/labAccounts/labs/write | Dient zum Hinzufügen oder Ändern von Labs. |
> | Action | Microsoft.LabServices/labAccounts/read | Hiermit werden Lab-Konten gelesen. |
> | Action | Microsoft.LabServices/labAccounts/sharedGalleries/delete | Löscht sharedgalleries. |
> | Action | Microsoft.LabServices/labAccounts/sharedGalleries/read | Liest sharedgalleries. |
> | Action | Microsoft.LabServices/labAccounts/sharedGalleries/write | Dient zum Hinzufügen oder Ändern von sharedgalleries. |
> | Action | Microsoft.LabServices/labAccounts/sharedImages/delete | Löscht sharedimages. |
> | Action | Microsoft.LabServices/labAccounts/sharedImages/read | Liest sharedimages. |
> | Action | Microsoft.LabServices/labAccounts/sharedImages/write | Dient zum Hinzufügen oder Ändern von sharedimages. |
> | Action | Microsoft.LabServices/labAccounts/write | Dient zum Hinzufügen oder Ändern von Lab-Konten. |
> | Action | Microsoft.LabServices/locations/operations/read | Dient zum Lesen von Vorgängen. |
> | Action | Microsoft.LabServices/register/action | Registriert das Abonnement. |
> | Action | Microsoft.LabServices/users/ListAllEnvironments/action | Listet alle Umgebungen für den Benutzer auf. |
> | Action | Microsoft.LabServices/users/Register/action | Registriert einen Benutzer für ein verwaltetes Lab. |
> | Action | Microsoft.LabServices/users/ResetPassword/action | Setzt das Kennwort des Benutzers für eine Umgebung zurück. |
> | Action | Microsoft.LabServices/users/StartEnvironment/action | Startet eine Umgebung, indem alle Ressourcen innerhalb der Umgebung gestartet werden. |
> | Action | Microsoft.LabServices/users/StopEnvironment/action | Beendet eine Umgebung, indem alle Ressourcen innerhalb der Umgebung beendet werden. |
> | Action | Microsoft.LabServices/users/UserSettings/action | Aktualisiert die persönlichen Benutzereinstellungen und gibt sie zurück. |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Logic/integrationAccounts/agreements/delete | Löscht die Vereinbarung im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/agreements/listContentCallbackUrl/action | Ruft die Rückruf-URL für den Vereinbarungsinhalt im Integrationskonto ab. |
> | Action | Microsoft.Logic/integrationAccounts/agreements/read | Liest die Vereinbarung im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/agreements/write | Erstellt oder aktualisiert die Vereinbarung im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/delete | Löscht die Assembly im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/listContentCallbackUrl/action | Ruft die Rückruf-URL für den Inhalt der Assembly im Integrationskonto ab. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/read | Liest die Assembly im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/write | Erstellt oder aktualisiert die Assembly im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/batchConfigurations/delete | Löscht die Batchkonfiguration im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/batchConfigurations/read | Liest die Batchkonfiguration im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/batchConfigurations/write | Erstellt oder aktualisiert die Batchkonfiguration im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/certificates/delete | Löscht das Zertifikat im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/certificates/read | Liest das Zertifikat im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/certificates/write | Erstellt oder aktualisiert das Zertifikat im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/delete | Löscht das Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/join/action | Verknüpft das Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/listCallbackUrl/action | Ruft die Rückruf-URL für das Integrationskonto ab. |
> | Action | Microsoft.Logic/integrationAccounts/listKeyVaultKeys/action | Ruft die Schlüssel im Schlüsseltresor ab. |
> | Action | Microsoft.Logic/integrationAccounts/logTrackingEvents/action | Protokolliert die Überwachungsereignisse im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/maps/delete | Löscht die Zuordnung im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/maps/listContentCallbackUrl/action | Ruft die Rückruf-URL für den Inhalt der Zuordnung im Integrationskonto ab. |
> | Action | Microsoft.Logic/integrationAccounts/maps/read | Liest die Zuordnung im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/maps/write | Erstellt oder aktualisiert die Zuordnung im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/partners/delete | Löscht den Partner im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/partners/listContentCallbackUrl/action | Ruft die Rückruf-URL für den Partnerinhalt im Integrationskonto ab. |
> | Action | Microsoft.Logic/integrationAccounts/partners/read | Liest den Partner im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/partners/write | Erstellt oder aktualisiert den Partner im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read | Liest die Protokolldefinitionen des Integrationskontos. |
> | Action | Microsoft.Logic/integrationAccounts/read | Liest das Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/regenerateAccessKey/action | Generiert die Zugriffsschlüsselgeheimnisse neu. |
> | Action | Microsoft.Logic/integrationAccounts/rosettaNetProcessConfigurations/delete | Löscht die RosettaNet-Prozesskonfiguration im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/rosettaNetProcessConfigurations/read | Liest die RosettaNet-Prozesskonfiguration im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/rosettaNetProcessConfigurations/write | Erstellt oder aktualisiert die RosettaNet-Prozesskonfiguration im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/delete | Löscht das Schema im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/listContentCallbackUrl/action | Ruft die Rückruf-URL für den Schemainhalt im Integrationskonto ab. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/read | Liest das Schema im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/write | Erstellt oder aktualisiert das Schema im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/sessions/delete | Löscht die Sitzung im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/sessions/read | Liest die Batchkonfiguration im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/sessions/write | Erstellt oder aktualisiert die Sitzung im Integrationskonto. |
> | Action | Microsoft.Logic/integrationAccounts/write | Erstellt oder aktualisiert das Integrationskonto. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/availableManagedApis/read | Liest die in der Integrationsdienstumgebung verfügbaren verwalteten APIs. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/delete | Hiermit wird die Integrationsdienstumgebung gelöscht. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/join/action | Hiermit erfolgt ein Beitritt zur Integrationsdienstumgebung. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/managedApis/apiOperations/read | Liest den Vorgang der in der Integrationsdienstumgebung verwalteten API. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/managedApis/join/action | Bindet die verwaltete API der Integrationsdienstumgebung ein. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/managedApis/operationStatuses/read | Liest die Vorgangsstatus der in der Integrationsdienstumgebung verwalteten API. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/managedApis/read | Liest die in der Integrationsdienstumgebung verwalteten API. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/managedApis/write | Erstellt oder aktualisiert eine in der Integrationsdienstumgebung verwaltete API. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/operationStatuses/read | Liest die Vorgangsstatusangaben zur Integrationsdienstumgebung. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/providers/Microsoft.Insights/metricDefinitions/read | Liest die Metrikdefinitionen in der Integrationsdienstumgebung. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/read | Hiermit wird die Integrationsdienstumgebung gelesen. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/write | Hiermit wird die Integrationsdienstumgebung erstellt oder aktualisiert. |
> | Action | Microsoft.Logic/locations/workflows/recommendOperationGroups/action | Ruft den Workflow der empfohlenen Vorgangsgruppen ab. |
> | Action | Microsoft.Logic/locations/workflows/validate/action | Überprüft den Workflow. |
> | Action | Microsoft.Logic/operations/read | Ruft den Vorgang ab. |
> | Action | Microsoft.Logic/register/action | Registriert den Microsoft.Logic-Ressourcenanbieter für ein angegebenes Abonnement. |
> | Action | Microsoft.Logic/workflows/accessKeys/delete | Löscht den Zugriffsschlüssel. |
> | Action | Microsoft.Logic/workflows/accessKeys/list/action | Listet die Zugriffsschlüsselgeheimnisse auf. |
> | Action | Microsoft.Logic/workflows/accessKeys/read | Liest den Zugriffsschlüssel. |
> | Action | Microsoft.Logic/workflows/accessKeys/regenerate/action | Generiert die Zugriffsschlüsselgeheimnisse neu. |
> | Action | Microsoft.Logic/workflows/accessKeys/write | Erstellt oder aktualisiert den Zugriffsschlüssel. |
> | Action | Microsoft.Logic/workflows/delete | Löscht den Workflow. |
> | Action | Microsoft.Logic/workflows/detectors/read | Liest die Workflowerkennung. |
> | Action | Microsoft.Logic/workflows/disable/action | Deaktiviert den Workflow. |
> | Action | Microsoft.Logic/workflows/enable/action | Aktiviert den Workflow. |
> | Action | Microsoft.Logic/workflows/listCallbackUrl/action | Ruft die Rückruf-URL für Workflows ab. |
> | Action | Microsoft.Logic/workflows/listSwagger/action | Ruft die Swagger-Definitionen für den Workflow ab. |
> | Action | Microsoft.Logic/workflows/move/action | Verschiebt den Workflow von der vorhandenen Abonnement-ID, der Ressourcengruppe und/oder dem Namen in eine andere Abonnement-ID, eine andere Ressourcengruppe und/oder einen anderen Namen. |
> | Action | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/read | Liest die Diagnoseeinstellungen des Workflows. |
> | Action | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung des Workflows. |
> | Action | Microsoft.Logic/workflows/providers/Microsoft.Insights/logDefinitions/read | Liest die Protokolldefinitionen des Workflows. |
> | Action | Microsoft.Logic/workflows/providers/Microsoft.Insights/metricDefinitions/read | Liest die Metrikdefinitionen des Workflows. |
> | Action | Microsoft.Logic/workflows/read | Liest den Workflow. |
> | Action | Microsoft.Logic/workflows/regenerateAccessKey/action | Generiert die Zugriffsschlüsselgeheimnisse neu. |
> | Action | Microsoft.Logic/workflows/run/action | Startet eine Ausführung des Workflows. |
> | Action | Microsoft.Logic/workflows/runs/actions/listExpressionTraces/action | Ruft die Ausdrucksablaufverfolgungen für Workflowausführungsaktionen ab. |
> | Action | Microsoft.Logic/workflows/runs/actions/read | Liest die Workflowausführungsaktion. |
> | Action | Microsoft.Logic/workflows/runs/actions/repetitions/listExpressionTraces/action | Ruft die Ausdrucksablaufverfolgungen für Wiederholungen von Workflowausführungsaktionen ab. |
> | Action | Microsoft.Logic/workflows/runs/actions/repetitions/read | Liest die Wiederholung von Workflowausführungsaktionen. |
> | Action | Microsoft.Logic/workflows/runs/actions/repetitions/requestHistories/read | List den Anforderungsverlauf für wiederholte Workflowausführungsaktionen. |
> | Action | Microsoft.Logic/workflows/runs/actions/requestHistories/read | Liest den Anforderungsverlauf für Workflowausführungsaktionen. |
> | Action | Microsoft.Logic/workflows/runs/actions/scoperepetitions/read | Liest die Bereichswiederholung von Workflowausführungsaktionen. |
> | Action | Microsoft.Logic/workflows/runs/cancel/action | Bricht die Ausführung eines Workflows ab. |
> | Action | Microsoft.Logic/workflows/runs/delete | Löscht eine Ausführung eines Workflows. |
> | Action | Microsoft.Logic/workflows/runs/operations/read | Liest den Status von Workflowausführungsaktionen. |
> | Action | Microsoft.Logic/workflows/runs/read | Liest die Workflowausführung. |
> | Action | Microsoft.Logic/workflows/suspend/action | Hält den Workflow an. |
> | Action | Microsoft.Logic/workflows/triggers/histories/read | Liest die Triggerverläufe. |
> | Action | Microsoft.Logic/workflows/triggers/histories/resubmit/action | Übermittelt den Workflowtrigger erneut. |
> | Action | Microsoft.Logic/workflows/triggers/listCallbackUrl/action | Ruft die Rückruf-URL für Trigger ab. |
> | Action | Microsoft.Logic/workflows/triggers/read | Liest den Trigger. |
> | Action | Microsoft.Logic/workflows/triggers/reset/action | Setzt den Trigger zurück. |
> | Action | Microsoft.Logic/workflows/triggers/run/action | Führt den Trigger aus. |
> | Action | Microsoft.Logic/workflows/triggers/setState/action | Legt den Triggerzustand fest. |
> | Action | Microsoft.Logic/workflows/validate/action | Überprüft den Workflow. |
> | Action | Microsoft.Logic/workflows/versions/read | Liest die Workflowversion. |
> | Action | Microsoft.Logic/workflows/versions/triggers/listCallbackUrl/action | Ruft die Rückruf-URL für Trigger ab. |
> | Action | Microsoft.Logic/workflows/write | Erstellt oder aktualisiert den Workflow. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/move/action | Dient zum Verschieben beliebiger Machine Learning-Vertragsplanzuordnungen. |
> | Action | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/read | Dient zum Lesen beliebiger Machine Learning-Vertragsplanzuordnungen. |
> | Action | Microsoft.MachineLearning/commitmentPlans/delete | Dient zum Löschen beliebiger Machine Learning-Vertragspläne. |
> | Action | Microsoft.MachineLearning/commitmentPlans/join/action | Dient zum Beitreten zu beliebigen Machine Learning-Vertragsplänen. |
> | Action | Microsoft.MachineLearning/commitmentPlans/read | Dient zum Lesen beliebiger Machine Learning-Vertragspläne. |
> | Action | Microsoft.MachineLearning/commitmentPlans/write | Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Vertragspläne. |
> | Action | Microsoft.MachineLearning/locations/operationresults/read | Ruft das Ergebnis eines Machine Learning-Vorgangs ab. |
> | Action | Microsoft.MachineLearning/locations/operationsstatus/read | Ruft den Status eines laufenden Machine Learning-Vorgangs ab. |
> | Action | Microsoft.MachineLearning/operations/read | Ruft Machine Learning-Vorgänge ab. |
> | Action | Microsoft.MachineLearning/register/action | Registriert das Abonnement für den Machine Learning-Webdienst-Ressourcenanbieter und ermöglicht die Erstellung von Webdiensten. |
> | Action | Microsoft.MachineLearning/skus/read | Dient zum Abrufen von SKUs für Machine Learning-Verpflichtungspläne. |
> | Action | Microsoft.MachineLearning/webServices/action | Dient zum Erstellen regionaler Webdiensteigenschaften für unterstützte Regionen. |
> | Action | Microsoft.MachineLearning/webServices/delete | Dient zum Löschen beliebiger Machine Learning-Webdienste. |
> | Action | Microsoft.MachineLearning/webServices/listkeys/read | Ruft Schlüssel für einen Machine Learning-Webdienst ab. |
> | Action | Microsoft.MachineLearning/webServices/read | Dient zum Lesen beliebiger Machine Learning-Webdienste. |
> | Action | Microsoft.MachineLearning/webServices/write | Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Webdienste. |
> | Action | Microsoft.MachineLearning/Workspaces/delete | Dient zum Löschen beliebiger Machine Learning-Arbeitsbereiche. |
> | Action | Microsoft.MachineLearning/Workspaces/listworkspacekeys/action | Dient zum Auflisten von Schlüsseln für einen Machine Learning-Arbeitsbereich. |
> | Action | Microsoft.MachineLearning/Workspaces/read | Dient zum Lesen beliebiger Machine Learning-Arbeitsbereiche. |
> | Action | Microsoft.MachineLearning/Workspaces/resyncstoragekeys/action | Dient zum erneuten Synchronisieren von Schlüsseln eines für einen Machine Learning-Arbeitsbereich konfigurierten Speicherkontos. |
> | Action | Microsoft.MachineLearning/Workspaces/write | Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Arbeitsbereiche. |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.MachineLearningServices/locations/computeoperationsstatus/read | Ruft den Status eines bestimmten Computevorgangs ab. |
> | Action | Microsoft.MachineLearningServices/locations/quotas/read | Ruft die derzeit zugewiesenen Arbeitsbereichskontingente basierend auf VMFamily ab. |
> | Action | Microsoft.MachineLearningServices/locations/updateQuotas/action | Aktualisieren Sie das Kontingent für jede VM-Familie des Arbeitsbereichs. |
> | Action | Microsoft.MachineLearningServices/locations/usages/read | Nutzungsbericht für AML-Computeressourcen in einem Abonnement. |
> | Action | Microsoft.MachineLearningServices/locations/vmsizes/read | Ruft unterstützte VM-Größen ab. |
> | Action | Microsoft.MachineLearningServices/locations/workspaceOperationsStatus/read | Ruft den Status eines bestimmten Arbeitsbereichsvorgangs ab. |
> | Action | Microsoft.MachineLearningServices/register/action | Registriert das Abonnement für den Machine Learning Services-Ressourcenanbieter. |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/delete | Löscht die Computeressourcen in Machine Learning Services-Arbeitsbereichen. |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/listKeys/action | Führt geheime Schlüssel für die Computeressourcen in Machine Learning Services-Arbeitsbereichen auf. |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/listNodes/action | Listet Knoten für die Computeressource im Machine Learning Services-Arbeitsbereich auf. |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/read | Ruft die Computeressourcen in Machine Learning Services-Arbeitsbereichen ab. |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/write | Erstellt oder aktualisiert die Computeressourcen in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datadriftdetectors/read | Ruft Datendrifterkennungen in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datadriftdetectors/write | Erstellt oder aktualisiert Datendrifterkennungen in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/delete | Löscht registrierte Datasets in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/preview/read | Ruft die Datasetvorschau für registrierte Datasets in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/profile/read | Ruft die Datasetprofile für registrierte Datasets in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/profile/write | Erstellt oder aktualisiert Datasetprofile in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/read | Ruft registrierte Datasets in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/write | Erstellt oder aktualisiert registrierte Datasets in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/unregistered/delete | Löscht nicht registrierte Datasets in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/unregistered/preview/read | Ruft die Datasetvorschau für unregistrierte Datasets in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/unregistered/profile/read | Ruft die Datasetprofile für unregistrierte Datasets in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/unregistered/read | Ruft unregistrierte Datasets in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/unregistered/write | Erstellt oder aktualisiert unregistrierte Datasets in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datastores/delete | Löscht Datenspeicher in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datastores/read | Ruft Datenspeicher in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datastores/write | Erstellt oder aktualisiert Datenspeicher in Machine Learning Services-Arbeitsbereichen. |
> | Action | Microsoft.MachineLearningServices/workspaces/delete | Löscht die Machine Learning Services-Arbeitsbereiche. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/endpoints/pipelines/read | Ruft veröffentlichte Pipelines und Pipelineendpunkte in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/endpoints/pipelines/write | Erstellt oder aktualisiert veröffentlichte Pipelines und Pipelineendpunkte in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/environments/build/action | Erstellt Umgebungen in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/environments/read | Ruft Umgebungen in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/environments/readSecrets/action | Ruft Umgebungen mit Geheimnissen in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/environments/write | Erstellt oder aktualisiert Umgebungen in Machine Learning Services-Arbeitsbereichen. |
> | Action | Microsoft.MachineLearningServices/workspaces/eventGridFilters/read | Ruft einen Event Grid-Filter für einen bestimmten Arbeitsbereich ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/delete | Löscht Experimente in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/read | Ruft Experimente in Machine Learning Services-Arbeitsbereiche ab |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/runs/read | Ruft Ausführungen in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/runs/scriptRun/submit/action | Erstellt oder aktualisiert Skriptausführungen in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/runs/write | Erstellt oder aktualisiert Ausführungen in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/write | Erstellt oder aktualisiert Experimente in Machine Learning Services-Arbeitsbereichen |
> | Action | Microsoft.MachineLearningServices/workspaces/listKeys/action | Führt die geheimen Schlüssel für einen Machine Learning Services-Arbeitsbereich auf. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/artifacts/delete | Löscht Artefakte in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/artifacts/read | Ruft Artefakte in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/artifacts/write | Erstellt oder aktualisiert Artefakte in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/snapshots/delete | Löscht Momentaufnahmen in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/snapshots/read | Ruft Momentaufnahmen in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/snapshots/write | Erstellt oder aktualisiert Momentaufnahmen in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/models/delete | Löscht Modelle in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/models/download/action | Lädt Modelle in Machine Learning Services-Arbeitsbereichen herunter. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/models/package/action | Verpackt Modelle in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/models/read | Ruft Modelle in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/models/write | Erstellt oder aktualisiert Modelle in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/modules/read | Ruft Module in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/modules/write | Erstellt oder aktualisiert Module in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/pipelinedrafts/delete | Löscht Pipelineentwürfe in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/pipelinedrafts/read | Ruft Pipelineentwürfe in Machine Learning Services-Arbeitsbereichen ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/pipelinedrafts/write | Erstellt oder aktualisiert Pipelineentwürfe in Machine Learning Services-Arbeitsbereichen. |
> | Action | Microsoft.MachineLearningServices/workspaces/read | Ruft die Machine Learning Services-Arbeitsbereiche ab. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aci/delete | Löscht ACI-Dienste in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aci/listkeys/action | Listet Schlüssel für ACI-Dienste in Machine Learning Services-Arbeitsbereichen auf. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aci/write | Erstellt oder aktualisiert ACI-Dienste in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/devtest/delete | Löscht DevTest-AKS-Dienste in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/devtest/listkeys/action | Listet Schlüssel für DevTest-AKS-Dienste in Machine Learning Services-Arbeitsbereichen auf. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/devtest/score/action | Bewertet DevTest-AKS-Dienste in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/devtest/write | Erstellt oder aktualisiert DevTest-AKS-Dienste in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/prod/delete | Löscht für die Produktion bestimmte AKS-Dienste in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/prod/listkeys/action | Listet Schlüssel für AKS-Dienste, die für die Produktion bestimmt sind, in Machine Learning Services-Arbeitsbereichen auf. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/prod/score/action | Bewertet für die Produktion bestimmte AKS-Dienste in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/prod/write | Erstellt oder aktualisiert für die Produktion bestimmte AKS-Dienste in Machine Learning Services-Arbeitsbereichen. |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/read | Ruft Dienste in Machine Learning Services-Arbeitsbereichen ab. |
> | Action | Microsoft.MachineLearningServices/workspaces/write | Erstellt oder aktualisiert die Machine Learning Services-Arbeitsbereiche. |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ManagedIdentity/identities/read | Ruft eine vorhandene systemseitig zugewiesene Identität ab. |
> | Action | Microsoft.ManagedIdentity/operations/read | Listet verfügbare Vorgänge für den Microsoft.ManagedIdentity-Ressourcenanbieter auf. |
> | Action | Microsoft.ManagedIdentity/register/action | Registriert das Abonnement für den Ressourcenanbieter der verwalteten Identität. |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | RBAC-Aktion für das Zuweisen einer vorhandenen Identität, die einem Benutzer zugewiesen ist, zu einer Ressource |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Löscht eine vorhandene zugewiesene Benutzeridentität. |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/read | Ruft eine vorhandene zugewiesene Benutzeridentität ab. |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/write | Erstellt eine neue zugewiesene Benutzeridentität oder aktualisiert die Markierungen, die einer vorhandenen zugewiesenen Benutzeridentität zugeordnet sind. |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ManagedServices/marketplaceRegistrationDefinitions/read | Ruft eine Liste der Registrierungsdefinitionen für verwaltete Dienste ab. |
> | Action | Microsoft.ManagedServices/operations/read | Ruft eine Liste von Vorgängen verwalteter Dienste ab. |
> | Action | Microsoft.ManagedServices/operationStatuses/read | Liest den Vorgangsstatus für die Ressource. |
> | Action | Microsoft.ManagedServices/register/action | Führt eine Registrierung bei verwalteten Diensten durch. |
> | Action | Microsoft.ManagedServices/registrationAssignments/delete | Entfernt die Registrierungszuweisung für verwaltete Dienste. |
> | Action | Microsoft.ManagedServices/registrationAssignments/read | Ruft eine Liste der Registrierungszuweisungen für verwaltete Dienste ab. |
> | Action | Microsoft.ManagedServices/registrationAssignments/write | Fügt eine Registrierungszuweisung für verwaltete Dienste hinzu oder ändert sie. |
> | Action | Microsoft.ManagedServices/registrationDefinitions/delete | Entfernt die Registrierungsdefinition für verwaltete Dienste. |
> | Action | Microsoft.ManagedServices/registrationDefinitions/read | Ruft eine Liste der Registrierungsdefinitionen für verwaltete Dienste ab. |
> | Action | Microsoft.ManagedServices/registrationDefinitions/write | Fügt eine Registrierungsdefinition für verwaltete Dienste hinzu oder ändert sie. |
> | Action | Microsoft.ManagedServices/unregister/action | Hebt die Registrierung bei verwalteten Diensten auf. |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Management/checkNameAvailability/action | Überprüft, ob der Name der angegebenen Verwaltungsgruppe gültig und eindeutig ist. |
> | Action | Microsoft.Management/getEntities/action | Listet alle Entitäten (Verwaltungsgruppen, Abonnements usw.) für den authentifizierten Benutzer auf. |
> | Action | Microsoft.Management/managementGroups/delete | Löscht eine Verwaltungsgruppe. |
> | Action | Microsoft.Management/managementGroups/descendants/read | Ruft alle Nachfolger (Verwaltungsgruppen, Abonnements) einer Verwaltungsgruppe ab. |
> | Action | Microsoft.Management/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | Action | Microsoft.Management/managementGroups/subscriptions/delete | Hebt die Zuordnung des Abonnements mit der Verwaltungsgruppe auf. |
> | Action | Microsoft.Management/managementGroups/subscriptions/write | Ordnet ein vorhandenes Abonnement der Verwaltungsgruppe zu. |
> | Action | Microsoft.Management/managementGroups/write | Erstellt oder aktualisiert eine Verwaltungsgruppe. |
> | Action | Microsoft.Management/register/action | Registriert das angegebene Abonnement mit Microsoft.Management. |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | DataAction | Microsoft.Maps/accounts/data/read | Gewährt Datenlesezugriff auf ein Maps-Konto. |
> | Action | Microsoft.Maps/accounts/delete | Löscht ein Maps-Konto. |
> | Action | Microsoft.Maps/accounts/eventGridFilters/delete | Löscht einen Event Grid-Filter. |
> | Action | Microsoft.Maps/accounts/eventGridFilters/read | Ruft einen Event Grid-Filter ab. |
> | Action | Microsoft.Maps/accounts/eventGridFilters/write | Erstellt oder aktualisiert einen Event Grid-Filter. |
> | Action | Microsoft.Maps/accounts/listKeys/action | Führt Schlüssel für ein Maps-Konto auf. |
> | Action | Microsoft.Maps/accounts/read | Ruft ein Maps-Konto ab. |
> | Action | Microsoft.Maps/accounts/regenerateKey/action | Generiert einen neuen primären oder sekundären Schlüssel für ein Maps-Konto. |
> | Action | Microsoft.Maps/accounts/write | Erstellt oder aktualisiert ein Maps-Konto. |
> | Action | Microsoft.Maps/operations/read | Liest die Anbietervorgänge. |
> | Action | Microsoft.Maps/register/action | Hiermit wird der Anbieter registriert. |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/read | Gibt eine Vereinbarung zurück. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/write | Akzeptiert eine unterzeichnete Vereinbarung. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/importImage/action | Importiert ein Image in den Access Control-Datensatz des Endbenutzers. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/read | Gibt eine Konfiguration zurück. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/write | Speichert eine Konfiguration. |
> | Action | Microsoft.Marketplace/register/action | Registriert den Microsoft.Marketplace-Ressourcenanbieter im Abonnement. |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/delete | Führt einen DELETE-Vorgang für eine Ressource eines klassischen Entwicklerdiensts durch. |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/listSecrets/action | Ruft die Ressourcenverwaltungsschlüssel eines klassischen Entwicklerdiensts ab. |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/listSingleSignOnToken/action | Ruft die URL für einmaliges Anmelden für einen klassischen Entwicklerdienst ab. |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/read | Führt einen GET-Vorgang für einen klassischen Entwicklerdienst durch. |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/regenerateKey/action | Generiert Ressourcenverwaltungsschlüssel eines klassischen Entwicklerdiensts. |
> | Action | Microsoft.MarketplaceApps/Operations/read | Liest die Vorgänge für alle Ressourcentypen. |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.MarketplaceOrdering/agreements/offers/plans/cancel/action | Dient zum Stornieren einer Vereinbarung für ein angegebenes Marketplace-Element. |
> | Action | Microsoft.MarketplaceOrdering/agreements/offers/plans/read | Dient zum Zurückgeben einer Vereinbarung für ein angegebenes Marketplace-Element. |
> | Action | Microsoft.MarketplaceOrdering/agreements/offers/plans/sign/action | Dient zum Unterzeichnen einer Vereinbarung für ein angegebenes Marketplace-Element. |
> | Action | Microsoft.MarketplaceOrdering/agreements/read | Gibt alle Vereinbarungen eines bestimmten Abonnements zurück. |
> | Action | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/read | Dient zum Abrufen einer Vereinbarung für ein angegebenes Marketplace-VM-Element. |
> | Action | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/write | Dient zum Registrieren oder Stornieren einer Vereinbarung für ein angegebenes Marketplace-VM-Element. |
> | Action | Microsoft.MarketplaceOrdering/operations/read | Listet alle möglichen Vorgänge in der API auf |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Media/checknameavailability/action | Überprüft, ob ein Media Services-Kontoname verfügbar ist. |
> | Action | Microsoft.Media/mediaservices/accountfilters/delete | Löscht Kontofilter. |
> | Action | Microsoft.Media/mediaservices/accountfilters/read | Liest Kontofilter. |
> | Action | Microsoft.Media/mediaservices/accountfilters/write | Dient zum Erstellen oder Aktualisieren von Kontofiltern. |
> | Action | Microsoft.Media/mediaservices/assets/assetfilters/delete | Löscht Medienobjektfilter. |
> | Action | Microsoft.Media/mediaservices/assets/assetfilters/read | Liest Medienobjektfilter. |
> | Action | Microsoft.Media/mediaservices/assets/assetfilters/write | Dient zum Erstellen oder Aktualisieren von Medienobjektfiltern. |
> | Action | Microsoft.Media/mediaservices/assets/delete | Löscht alle Objekte. |
> | Action | Microsoft.Media/mediaservices/assets/getEncryptionKey/action | Ruft Verschlüsselungsschlüssel für ein Objekt ab. |
> | Action | Microsoft.Media/mediaservices/assets/listContainerSas/action | Führt die Container-SAS-URLs für ein Objekt auf. |
> | Action | Microsoft.Media/mediaservices/assets/listStreamingLocators/action | Listet Streaminglocators für ein Medienobjekt auf. |
> | Action | Microsoft.Media/mediaservices/assets/read | Liest alle Objekte. |
> | Action | Microsoft.Media/mediaservices/assets/write | Erstellt oder aktualisiert alle Objekte. |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/delete | Löscht alle Richtlinien für Inhaltsschlüssel. |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Ruft Richtlinieneigenschaften mit geheimen Schlüsseln ab. |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/read | Liest alle Richtlinien für Inhaltsschlüssel. |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/write | Erstellt oder aktualisiert alle Richtlinien für Inhaltsschlüssel. |
> | Action | Microsoft.Media/mediaservices/delete | Löscht ein Media Services-Konto. |
> | Action | Microsoft.Media/mediaservices/eventGridFilters/delete | Löscht alle Event Grid-Filter. |
> | Action | Microsoft.Media/mediaservices/eventGridFilters/read | Liest alle Event Grid-Filter. |
> | Action | Microsoft.Media/mediaservices/eventGridFilters/write | Erstellt oder aktualisiert alle Event Grid-Filter. |
> | Action | Microsoft.Media/mediaservices/liveEventOperations/read | Liest alle Vorgänge für Liveereignisse. |
> | Action | Microsoft.Media/mediaservices/liveEvents/delete | Löscht alle Liveereignisse. |
> | Action | Microsoft.Media/mediaservices/liveEvents/liveOutputs/delete | Löscht alle Liveausgaben. |
> | Action | Microsoft.Media/mediaservices/liveEvents/liveOutputs/read | Liest alle Liveausgaben. |
> | Action | Microsoft.Media/mediaservices/liveEvents/liveOutputs/write | Erstellt oder aktualisiert alle Liveausgaben. |
> | Action | Microsoft.Media/mediaservices/liveEvents/read | Liest alle Liveereignisse. |
> | Action | Microsoft.Media/mediaservices/liveEvents/reset/action | Setzt alle Vorgänge für Liveereignisse zurück. |
> | Action | Microsoft.Media/mediaservices/liveEvents/start/action | Startet alle Vorgänge für Liveereignisse. |
> | Action | Microsoft.Media/mediaservices/liveEvents/stop/action | Beendet alle Vorgänge für Liveereignisse. |
> | Action | Microsoft.Media/mediaservices/liveEvents/write | Erstellt oder aktualisiert alle Liveereignisse. |
> | Action | Microsoft.Media/mediaservices/liveOutputOperations/read | Liest alle Vorgänge für Liveausgaben. |
> | Action | Microsoft.Media/mediaservices/read | Liest ein Media Services-Konto. |
> | Action | Microsoft.Media/mediaservices/streamingEndpointOperations/read | Liest alle Vorgänge des Streamingendpunkts. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/delete | Löscht alle Streamingendpunkte. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/read | Liest alle Streamingendpunkte. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/scale/action | Skaliert alle Vorgänge des Streamingendpunkts. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/start/action | Startet alle Vorgänge des Streamingendpunkts. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/stop/action | Beendet alle Vorgänge des Streamingendpunkts. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/write | Erstellt oder aktualisiert alle Streamingendpunkte. |
> | Action | Microsoft.Media/mediaservices/streamingLocators/delete | Löscht alle Streaminglocators. |
> | Action | Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action | Führt Inhaltsschlüssel auf. |
> | Action | Microsoft.Media/mediaservices/streamingLocators/listPaths/action | Führt Pfade auf. |
> | Action | Microsoft.Media/mediaservices/streamingLocators/read | Liest alle Streaminglocators. |
> | Action | Microsoft.Media/mediaservices/streamingLocators/write | Erstellt oder aktualisiert alle Streaminglocators. |
> | Action | Microsoft.Media/mediaservices/streamingPolicies/delete | Löscht alle Streamingrichtlinien. |
> | Action | Microsoft.Media/mediaservices/streamingPolicies/read | Liest alle Streamingrichtlinien. |
> | Action | Microsoft.Media/mediaservices/streamingPolicies/write | Erstellt oder aktualisiert alle Streamingrichtlinien. |
> | Action | Microsoft.Media/mediaservices/syncStorageKeys/action | Synchronisiert die Speicherschlüssel für ein angefügtes Azure Storage-Konto. |
> | Action | Microsoft.Media/mediaservices/transforms/delete | Löscht alle Transformationen. |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/cancelJob/action | Dient zum Abbrechen von Aufträgen. |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/delete | Löscht alle Aufträge. |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/read | Liest alle Aufträge. |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/write | Erstellt oder aktualisiert alle Aufträge. |
> | Action | Microsoft.Media/mediaservices/transforms/read | Liest alle Transformationen. |
> | Action | Microsoft.Media/mediaservices/transforms/write | Erstellt oder aktualisiert alle Transformationen. |
> | Action | Microsoft.Media/mediaservices/write | Erstellt oder aktualisiert ein Media Services-Konto. |
> | Action | Microsoft.Media/operations/read | Dient zum Abrufen verfügbarer Vorgänge. |
> | Action | Microsoft.Media/register/action | Registriert das Abonnement für den Media Services-Ressourcenanbieter und ermöglicht die Erstellung von Media Services-Konten. |
> | Action | Microsoft.Media/unregister/action | Hebt die Registrierung des Abonnements für den Media Services-Ressourcenanbieter auf. |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Migrate/assessmentprojects/assessmentOptions/read | Ruft die Bewertungsoptionen ab, die am angegebenen Standort verfügbar sind. |
> | Action | Microsoft.Migrate/assessmentprojects/assessments/read | Listet die Bewertungen innerhalb eines Projekts auf. |
> | Action | Microsoft.Migrate/assessmentprojects/delete | Löscht das Bewertungsprojekt |
> | Action | Microsoft.Migrate/assessmentprojects/groups/assessments/assessedmachines/read | Ruft die Eigenschaften eines bewerteten Computers ab. |
> | Action | Microsoft.Migrate/assessmentprojects/groups/assessments/delete | Löscht eine Bewertung. |
> | Action | Microsoft.Migrate/assessmentprojects/groups/assessments/downloadurl/action | Lädt die URL für einen Bewertungsbericht herunter. |
> | Action | Microsoft.Migrate/assessmentprojects/groups/assessments/read | Ruft die Eigenschaften einer Bewertung ab. |
> | Action | Microsoft.Migrate/assessmentprojects/groups/assessments/write | Erstellt eine neue Bewertung oder aktualisiert eine vorhandene Bewertung. |
> | Action | Microsoft.Migrate/assessmentprojects/groups/delete | Löscht eine Gruppe. |
> | Action | Microsoft.Migrate/assessmentprojects/groups/read | Ruft die Eigenschaften einer Gruppe ab. |
> | Action | Microsoft.Migrate/assessmentprojects/groups/updateMachines/action | Aktualisiert eine Gruppe durch Hinzufügen oder Entfernen von Computern |
> | Action | Microsoft.Migrate/assessmentprojects/groups/write | Erstellt eine neue Gruppe oder aktualisiert eine vorhandene Gruppe. |
> | Action | Microsoft.Migrate/assessmentprojects/hypervcollectors/delete | Löscht den Hyper-V-Collector |
> | Action | Microsoft.Migrate/assessmentprojects/hypervcollectors/read | Ruft die Eigenschaften eines Hyper-V-Collectors ab |
> | Action | Microsoft.Migrate/assessmentprojects/hypervcollectors/write | Erstellt einen neuen Hyper-V-Collector oder aktualisiert einen vorhandenen Hyper-V-Collector |
> | Action | Microsoft.Migrate/assessmentprojects/importcollectors/delete | Löscht den Importsammler. |
> | Action | Microsoft.Migrate/assessmentprojects/importcollectors/read | Ruft die Eigenschaften des Importsammlers ab. |
> | Action | Microsoft.Migrate/assessmentprojects/importcollectors/write | Erstellt einen neuen Importsammler oder aktualisiert einen vorhandenen Importsammler. |
> | Action | Microsoft.Migrate/assessmentprojects/machines/read | Ruft die Eigenschaften eines Computers ab. |
> | Action | Microsoft.Migrate/assessmentprojects/read | Ruft die Eigenschaften eines Bewertungsprojekts ab |
> | Action | Microsoft.Migrate/assessmentprojects/servercollectors/read | Ruft die Eigenschaften eines Serversammlers ab. |
> | Action | Microsoft.Migrate/assessmentprojects/servercollectors/write | Erstellt einen neuen Serversammler oder aktualisiert einen vorhandenen Serversammler. |
> | Action | Microsoft.Migrate/assessmentprojects/vmwarecollectors/delete | Löscht den VMware-Collector |
> | Action | Microsoft.Migrate/assessmentprojects/vmwarecollectors/read | Ruft die Eigenschaften eines VMware-Collectors ab |
> | Action | Microsoft.Migrate/assessmentprojects/vmwarecollectors/write | Erstellt einen neuen VMware-Collector oder aktualisiert einen vorhandenen VMware-Collector |
> | Action | Microsoft.Migrate/assessmentprojects/write | Erstellt ein neues Bewertungsprojekt oder aktualisiert ein vorhandenes Bewertungsprojekt |
> | Action | Microsoft.Migrate/locations/assessmentOptions/read | Ruft die Bewertungsoptionen ab, die am angegebenen Standort verfügbar sind. |
> | Action | Microsoft.Migrate/locations/checknameavailability/action | Überprüft die Verfügbarkeit des Ressourcennamens für das angegebene Abonnement am angegebenen Standort. |
> | Action | Microsoft.Migrate/migrateprojects/DatabaseInstances/read | Ruft die Eigenschaften einer Datenbankinstanz ab |
> | Action | Microsoft.Migrate/migrateprojects/Databases/read | Ruft die Eigenschaften einer Datenbank ab |
> | Action | Microsoft.Migrate/migrateprojects/delete | Löscht ein Migrate-Projekt |
> | Action | Microsoft.Migrate/migrateprojects/machines/read | Ruft die Eigenschaften eines Computers ab. |
> | Action | Microsoft.Migrate/migrateprojects/MigrateEvents/Delete | Löscht ein Migrate-Ereignis |
> | Action | Microsoft.Migrate/migrateprojects/MigrateEvents/read | Ruft die Eigenschaften eines Migrate-Ereignisses ab |
> | Action | Microsoft.Migrate/migrateprojects/read | Ruft die Eigenschaften eines Migrate-Projekts ab |
> | Action | Microsoft.Migrate/migrateprojects/RefreshSummary/action | Aktualisiert die Zusammenfassung des Migrationsprojekts |
> | Action | Microsoft.Migrate/migrateprojects/registerTool/action | Registriert ein Tool für ein Migrate-Projekt |
> | Action | Microsoft.Migrate/migrateprojects/solutions/cleanupData/action | Bereinigt die Lösungsdaten des Migrate-Projekts |
> | Action | Microsoft.Migrate/migrateprojects/solutions/Delete | Löscht eine Migrate-Projektlösung |
> | Action | Microsoft.Migrate/migrateprojects/solutions/getconfig/action | Ruft die Lösungskonfiguration für ein Migrationsprojekt ab. |
> | Action | Microsoft.Migrate/migrateprojects/solutions/read | Ruft die Eigenschaften einer Migrate-Projektlösung ab |
> | Action | Microsoft.Migrate/migrateprojects/solutions/write | Erstellt eine neue Migrate-Projektlösung oder aktualisiert eine vorhandene Migrate-Projektlösung |
> | Action | Microsoft.Migrate/migrateprojects/write | Erstellt ein neues Migrate-Projekt oder aktualisiert ein vorhandenes Migrate-Projekt |
> | Action | Microsoft.Migrate/Operations/read | Listet die verfügbaren Vorgänge für den Microsoft.Migrate-Ressourcenanbieter auf. |
> | Action | Microsoft.Migrate/projects/assessments/read | Listet die Bewertungen innerhalb eines Projekts auf. |
> | Action | Microsoft.Migrate/projects/delete | Löscht das Projekt. |
> | Action | Microsoft.Migrate/projects/groups/assessments/assessedmachines/read | Ruft die Eigenschaften eines bewerteten Computers ab. |
> | Action | Microsoft.Migrate/projects/groups/assessments/delete | Löscht eine Bewertung. |
> | Action | Microsoft.Migrate/projects/groups/assessments/downloadurl/action | Lädt die URL für einen Bewertungsbericht herunter. |
> | Action | Microsoft.Migrate/projects/groups/assessments/read | Ruft die Eigenschaften einer Bewertung ab. |
> | Action | Microsoft.Migrate/projects/groups/assessments/write | Erstellt eine neue Bewertung oder aktualisiert eine vorhandene Bewertung. |
> | Action | Microsoft.Migrate/projects/groups/delete | Löscht eine Gruppe. |
> | Action | Microsoft.Migrate/projects/groups/read | Ruft die Eigenschaften einer Gruppe ab. |
> | Action | Microsoft.Migrate/projects/groups/write | Erstellt eine neue Gruppe oder aktualisiert eine vorhandene Gruppe. |
> | Action | Microsoft.Migrate/projects/keys/action | Ruft gemeinsam verwendete Schlüssel für das Projekt ab. |
> | Action | Microsoft.Migrate/projects/machines/read | Ruft die Eigenschaften eines Computers ab. |
> | Action | Microsoft.Migrate/projects/read | Ruft die Eigenschaften eines Projekts ab. |
> | Action | Microsoft.Migrate/projects/write | Erstellt ein neues Projekt oder aktualisiert ein vorhandenes Projekt. |
> | Action | Microsoft.Migrate/register/action | Registriert ein Abonnement bei einem Microsoft.Migrate-Ressourcenanbieter. |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.MixedReality/register/action | Registriert ein Abonnement für den Mixed Reality-Ressourcenanbieter. |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Hiermit werden Raumanker erstellt. |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/delete | Hiermit werden Raumanker gelöscht. |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Hiermit werden Raumanker in räumlicher Nähe ermittelt. |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Hiermit werden die Eigenschaften von Raumankern abgerufen. |
> | Action | Microsoft.MixedReality/spatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für Microsoft.MixedReality/spatialAnchorsAccounts ab |
> | Action | Microsoft.MixedReality/spatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für Microsoft.MixedReality/spatialAnchorsAccounts |
> | Action | Microsoft.MixedReality/spatialAnchorsAccounts/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Microsoft.MixedReality/spatialAnchorsAccounts ab |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Hiermit finden Sie Raumanker. |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Hiermit übermitteln Sie Diagnosedaten, um die Qualität des Azure Spatial Anchors-Diensts zu verbessern. |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Hiermit aktualisieren Sie die Eigenschaften von Raumankern. |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.NetApp/locations/checkfilepathavailability/action | Dient zum Prüfen, ob ein Dateipfad verfügbar ist |
> | Action | Microsoft.NetApp/locations/checknameavailability/action | Prüft, ob ein Ressourcenname verfügbar ist |
> | Action | Microsoft.NetApp/locations/operationresults/read | Liest eine Vorgangsergebnisressource. |
> | Action | Microsoft.NetApp/locations/read | Liest eine Verfügbarkeitsüberprüfungsressource. |
> | Action | Microsoft.NetApp/netAppAccounts/backupPolicies/delete | Löscht die Ressource einer Sicherungsrichtlinie. |
> | Action | Microsoft.NetApp/netAppAccounts/backupPolicies/read | Liest die Ressource einer Sicherungsrichtlinie. |
> | Action | Microsoft.NetApp/netAppAccounts/backupPolicies/write | Schreibt die Ressource einer Sicherungsrichtlinie. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/delete | Löscht eine Poolressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/read | Liest eine Poolressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/backups/delete | Löscht eine Sicherungsressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/backups/read | Liest eine Sicherungsressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/backups/write | Schreibt eine Sicherungsressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/break/action | Unterbricht die Beziehungen in Bezug auf die Volumereplikation. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/delete | Löscht eine Volumeressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/mountTargets/read | Liest eine Einbindungszielressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/read | Liest eine Volumeressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/ReplicationStatus/action | Liest die Status der Volumereplikation. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/revert/action | Setzt das Volume auf eine bestimmte Momentaufnahme zurück. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/delete | Löscht eine Momentaufnahmeressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/read | Liest eine Momentaufnahmeressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/write | Schreibt eine Momentaufnahmeressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/write | Schreibt eine Volumeressource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/write | Schreibt eine Poolressource. |
> | Action | Microsoft.NetApp/netAppAccounts/delete | Löscht eine Kontoressource. |
> | Action | Microsoft.NetApp/netAppAccounts/read | Liest eine Kontoressource. |
> | Action | Microsoft.NetApp/netAppAccounts/vaults/read | Liest eine Tresorressource. |
> | Action | Microsoft.NetApp/netAppAccounts/write | Schreibt eine Kontoressource. |
> | Action | Microsoft.NetApp/Operations/read | Liest eine Vorgangsressource. |
> | Action | Microsoft.NetApp/register/action | Registriert ein Abonnement bei einem Microsoft.NetApp-Ressourcenanbieter |
> | Action | Microsoft.NetApp/unregister/action | Hebt die Registrierung eines Abonnements bei einem Microsoft.NetApp-Ressourcenanbieter auf |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Network/applicationGatewayAvailableRequestHeaders/read | Ruft für Application Gateway verfügbare Anforderungsheader ab. |
> | Action | Microsoft.Network/applicationGatewayAvailableResponseHeaders/read | Ruft für Application Gateway verfügbare Antwortheader ab. |
> | Action | Microsoft.Network/applicationGatewayAvailableServerVariables/read | Ruft für Application Gateway verfügbare Servervariablen ab. |
> | Action | Microsoft.Network/applicationGatewayAvailableSslOptions/predefinedPolicies/read | Vordefinierte SSL-Richtlinie für Application Gateway |
> | Action | Microsoft.Network/applicationGatewayAvailableSslOptions/read | Verfügbare SSL-Optionen für Application Gateway |
> | Action | Microsoft.Network/applicationGatewayAvailableWafRuleSets/read | Ruft verfügbare WAF-Regelsätze für Application Gateway ab. |
> | Action | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für ein Application Gateway. Nicht warnbar. |
> | Action | Microsoft.Network/applicationGateways/backendhealth/action | Ruft die Back-End-Integrität eines Anwendungsgateways ab. |
> | Action | Microsoft.Network/applicationGateways/delete | Löscht ein Anwendungsgateway. |
> | Action | Microsoft.Network/applicationGateways/getBackendHealthOnDemand/action | Ruft die Integrität des Back-Ends eines Anwendungsgateways für eine angegebene HTTP-Einstellung und den Back-End-Pool bedarfsgesteuert ab |
> | Action | Microsoft.Network/applicationGateways/read | Ruft ein Anwendungsgateway ab. |
> | Action | Microsoft.Network/applicationGateways/start/action | Startet ein Anwendungsgateway. |
> | Action | Microsoft.Network/applicationGateways/stop/action | Beendet ein Anwendungsgateway. |
> | Action | Microsoft.Network/applicationGateways/write | Erstellt ein Anwendungsgateway oder aktualisiert ein vorhandenes Anwendungsgateway. |
> | Action | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/delete | Löscht eine WAF-Richtlinie für Application Gateways. |
> | Action | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/read | Ruft eine WAF-Richtlinie für Application Gateways ab. |
> | Action | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/write | Erstellt oder aktualisiert eine WAF-Richtlinie für Application Gateways. |
> | Action | Microsoft.Network/applicationSecurityGroups/delete | Löscht eine Anwendungssicherheitsgruppe. |
> | Action | Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action | Dient zum Verknüpfen der IP-Konfiguration mit Anwendungssicherheitsgruppen. Nicht warnbar. |
> | Action | Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Dient zum Verknüpfen der Sicherheitsregel mit Anwendungssicherheitsgruppen. Nicht warnbar. |
> | Action | Microsoft.Network/applicationSecurityGroups/listIpConfigurations/action | Listet IP-Konfigurationen in der ApplicationSecurityGroup auf. |
> | Action | Microsoft.Network/applicationSecurityGroups/read | Ruft eine Anwendungssicherheitsgruppen-ID ab. |
> | Action | Microsoft.Network/applicationSecurityGroups/write | Erstellt eine Anwendungssicherheitsgruppe oder aktualisiert eine vorhandene Anwendungssicherheitsgruppe. |
> | Action | Microsoft.Network/azureFirewallFqdnTags/read | Ruft die Azure Firewall-FQDN-Tags ab. |
> | Action | Microsoft.Network/azurefirewalls/delete | Hiermit löschen Sie Azure Firewall. |
> | Action | Microsoft.Network/azurefirewalls/read | Hiermit rufen Sie Azure Firewall ab. |
> | Action | Microsoft.Network/azurefirewalls/write | Erstellt oder aktualisiert eine Azure Firewall. |
> | Action | Microsoft.Network/bastionHosts/createbsl/action | Erstellt freigabefähige URLs für die VMs unter einem Bastionhost und gibt die URLs zurück. |
> | Action | Microsoft.Network/bastionHosts/delete | Löscht einen Bastionhost. |
> | Action | Microsoft.Network/bastionHosts/deletebsl/action | Löscht freigabefähige URLs für die bereitgestellten VMs unter einem Bastionhost. |
> | Action | Microsoft.Network/bastionHosts/disconnectactivesessions/action | Trennt angegebene aktive Sitzungen auf dem Bastionhost. |
> | Action | Microsoft.Network/bastionHosts/getactivesessions/action | Ruft aktive Sitzungen auf dem Bastionhost ab. |
> | Action | Microsoft.Network/bastionHosts/getbsl/action | Gibt die freigabefähigen URLs für die angegebenen VMs in einem Bastionhostsubnetz zurück, sofern die zugehörigen URLs erstellt wurden. |
> | Action | Microsoft.Network/bastionHosts/read | Ruft einen Bastionhost ab. |
> | Action | Microsoft.Network/bastionHosts/write | Erstellt oder aktualisiert einen Bastionhost. |
> | Action | Microsoft.Network/bgpServiceCommunities/read | Dient zum Abrufen von BGP Service Communities. |
> | Action | Microsoft.Network/checkFrontDoorNameAvailability/action | Überprüft, ob ein Front Door-Name verfügbar ist. |
> | Action | Microsoft.Network/checkTrafficManagerNameAvailability/action | Prüft die Verfügbarkeit eines relativen Traffic Manager-DNS-Namens. |
> | Action | Microsoft.Network/connections/delete | Löscht VirtualNetworkGatewayConnection-Elemente. |
> | Action | Microsoft.Network/connections/read | Ruft VirtualNetworkGatewayConnection-Elemente ab. |
> | Action | Microsoft.Network/connections/revoke/action | Legt den ExpressRoute-Verbindungsstatus auf „Widerrufen“ fest. |
> | Action | Microsoft.Network/connections/sharedkey/action | Ruft SharedKey-Elemente für VirtualNetworkGatewayConnection ab. |
> | Action | Microsoft.Network/connections/sharedKey/read | Ruft SharedKey-Elemente für „VirtualNetworkGatewayConnection“ ab. |
> | Action | Microsoft.Network/connections/sharedKey/write | Erstellt ein SharedKey-Element für „VirtualNetworkGatewayConnection“ oder aktualisiert ein vorhandenes SharedKey-Element für „VirtualNetworkGatewayConnection“. |
> | Action | Microsoft.Network/connections/startpacketcapture/action | Startet die Erfassung der Verbindungspakete eines Gateways für virtuelle Netzwerke. |
> | Action | Microsoft.Network/connections/stoppacketcapture/action | Beendet die Erfassung der Verbindungspakete eines Gateways für virtuelle Netzwerke. |
> | Action | Microsoft.Network/connections/vpndeviceconfigurationscript/action | Ruft die VPN-Gerätekonfiguration von VirtualNetworkGatewayConnection ab. |
> | Action | Microsoft.Network/connections/write | Erstellt ein VirtualNetworkGatewayConnection-Element oder aktualisiert ein vorhandenes VirtualNetworkGatewayConnection-Element. |
> | Action | Microsoft.Network/ddosCustomPolicies/delete | Löscht eine DDoS-angepasste Richtlinie |
> | Action | Microsoft.Network/ddosCustomPolicies/read | Ruft einen DDoS-angepasste Richtliniendefinition ab. |
> | Action | Microsoft.Network/ddosCustomPolicies/write | Erstellt eine DDoS-angepasste Richtlinie oder aktualisiert eine vorhandene DDoS-angepasste Richtlinie. |
> | Action | Microsoft.Network/ddosProtectionPlans/delete | Löscht einen DDoS Protection-Plan. |
> | Action | Microsoft.Network/ddosProtectionPlans/join/action | Dient zum Verknüpfen eines DDoS Protection-Plans. Nicht warnbar. |
> | Action | Microsoft.Network/ddosProtectionPlans/read | Dient zum Aufrufen eines DDoS Protection-Plans. |
> | Action | Microsoft.Network/ddosProtectionPlans/write | Erstellt oder aktualisiert einen DDoS Protection-Plan.  |
> | Action | Microsoft.Network/dnsoperationresults/read | Ruft Ergebnisse eines DNS-Vorgangs ab. |
> | Action | Microsoft.Network/dnsoperationstatuses/read | Ruft den Status eines DNS-Vorgangs ab.  |
> | Action | Microsoft.Network/dnszones/A/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „A“ aus einer DNS-Zone. |
> | Action | Microsoft.Network/dnszones/A/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „A“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/dnszones/A/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „A“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/dnszones/AAAA/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „AAAA“ aus einer DNS-Zone. |
> | Action | Microsoft.Network/dnszones/AAAA/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „AAAA“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/dnszones/AAAA/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „AAAA“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/dnszones/all/read | Ruft DNS-Ressourceneintragssätze typenübergreifend ab. |
> | Action | Microsoft.Network/dnszones/CAA/delete | Dient zum Entfernen der Datensatzgruppe mit einem bestimmten Namen und dem Typ „CAA“ aus einer DNS-Zone. |
> | Action | Microsoft.Network/dnszones/CAA/read | Dient zum Abrufen der Datensatzgruppe vom Typ „CAA“ im JSON-Format. Der Ressourceneintragssatz enthält Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/dnszones/CAA/write | Dient zum Erstellen oder Aktualisieren einer Datensatzgruppe vom Typ „CAA“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/dnszones/CNAME/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „CNAME“ aus einer DNS-Zone. |
> | Action | Microsoft.Network/dnszones/CNAME/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „CNAME“ im JSON-Format. Der Ressourceneintragssatz enthält Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/dnszones/CNAME/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „CNAME“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/dnszones/delete | Dient zum Löschen der DNS-Zone im JSON-Format. Zu den Zoneneigenschaften zählen „tags“, „etag“, „numberOfRecordSets“ und „maxNumberOfRecordSets“. |
> | Action | Microsoft.Network/dnszones/MX/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „MX“ aus einer DNS-Zone. |
> | Action | Microsoft.Network/dnszones/MX/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „MX“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/dnszones/MX/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „MX“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/dnszones/NS/delete | Löscht den DNS-Ressourceneintragssatz vom Typ „NS“. |
> | Action | Microsoft.Network/dnszones/NS/read | Ruft DNS-Ressourceneintragssätze vom Typ „NS“ ab. |
> | Action | Microsoft.Network/dnszones/NS/write | Erstellt oder aktualisiert DNS-Ressourceneintragssätze vom Typ „NS“. |
> | Action | Microsoft.Network/dnszones/PTR/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „PTR“ aus einer DNS-Zone. |
> | Action | Microsoft.Network/dnszones/PTR/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „PTR“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/dnszones/PTR/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „PTR“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/dnszones/read | Dient zum Abrufen der DNS-Zone im JSON-Format. Zu den Zoneneigenschaften zählen „tags“, „etag“, „numberOfRecordSets“ und „maxNumberOfRecordSets“. Die in der Zone enthaltenen Ressourceneintragssätze werden durch diesen Befehl nicht abgerufen. |
> | Action | Microsoft.Network/dnszones/recordsets/read | Ruft DNS-Ressourceneintragssätze typenübergreifend ab. |
> | Action | Microsoft.Network/dnszones/SOA/read | Ruft DNS-Ressourceneintragssätze vom Typ „SOA“ ab. |
> | Action | Microsoft.Network/dnszones/SOA/write | Erstellt oder aktualisiert DNS-Ressourceneintragssätze vom Typ „SOA“. |
> | Action | Microsoft.Network/dnszones/SRV/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „SRV“ aus einer DNS-Zone. |
> | Action | Microsoft.Network/dnszones/SRV/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „SRV“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/dnszones/SRV/write | Dient zum Erstellen oder Aktualisieren von Ressourceneintragssätzen vom Typ „SRV“. |
> | Action | Microsoft.Network/dnszones/TXT/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „TXT“ aus einer DNS-Zone. |
> | Action | Microsoft.Network/dnszones/TXT/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „TXT“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/dnszones/TXT/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „TXT“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/dnszones/write | Dient zum Erstellen oder Aktualisieren einer DNS-Zone innerhalb einer Ressourcengruppe.  Wird zum Aktualisieren der Tags für eine DNS-Zonenressource verwendet. Mit diesem Befehl können keine Ressourceneintragssätze innerhalb der Zone erstellt oder aktualisiert werden. |
> | Action | Microsoft.Network/expressRouteCircuits/authorizations/delete | Löscht eine ExpressRouteCircuit-Autorisierung. |
> | Action | Microsoft.Network/expressRouteCircuits/authorizations/read | Ruft eine ExpressRouteCircuit-Autorisierung ab. |
> | Action | Microsoft.Network/expressRouteCircuits/authorizations/write | Erstellt eine ExpressRouteCircuit-Autorisierung oder aktualisiert eine vorhandene ExpressRouteCircuit-Autorisierung. |
> | Action | Microsoft.Network/expressRouteCircuits/delete | Löscht ein ExpressRouteCircuit-Element. |
> | Action | Microsoft.Network/expressRouteCircuits/join/action | Verknüpft eine ExpressRoute-Verbindung. Nicht warnbar. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/arpTables/read | Ruft ein ArpTable-Element für ein ExpressRouteCircuit-Peering ab. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/connections/delete | Löscht eine ExpressRouteCircuit-Verbindung. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/connections/read | Ruft eine ExpressRouteCircuit-Verbindung ab. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/connections/write | Erstellt eine ExpressRouteCircuit-Verbindungsressource oder aktualisiert eine vorhandene ExpressRouteCircuit-Verbindungsressource. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/delete | Löscht ein ExpressRouteCircuit-Peering. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/peerConnections/read | Ruft eine Peerverbindung für eine ExpressRoute-Leitung ab. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/read | Ruft ein ExpressRouteCircuit-Peering ab. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/routeTables/read | Ruft ein RouteTable-Element für ein ExpressRouteCircuit-Peering ab. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/routeTablesSummary/read | Ruft eine RouteTable-Zusammenfassung für ein ExpressRouteCircuit-Peering ab. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/stats/read | Ruft eine ExpressRouteCircuit-Peeringstatistik ab. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/write | Erstellt ein ExpressRouteCircuit-Peering oder aktualisiert ein vorhandenes ExpressRouteCircuit-Peering. |
> | Action | Microsoft.Network/expressRouteCircuits/read | Dient zum Abrufen eines ExpressRouteCircuit-Elements. |
> | Action | Microsoft.Network/expressRouteCircuits/stats/read | Ruft eine ExpressRouteCircuit-Statistik ab. |
> | Action | Microsoft.Network/expressRouteCircuits/write | Erstellt ein ExpressRouteCircuit-Element oder aktualisiert ein vorhandenes ExpressRouteCircuit-Element. |
> | Action | Microsoft.Network/expressRouteCrossConnections/join/action | Verknüpft eine ExpressRoute-Querverbindung. Nicht warnbar. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/arpTables/read | Ruft eine ARP-Tabelle zum Peering einer ExpressRoute-Querverbindung ab. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/delete | Löscht ein Peering einer ExpressRoute-Querverbindung. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/read | Ruft ein Peering einer ExpressRoute-Querverbindung ab. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/routeTables/read | Ruft eine Routentabelle zum Peering einer ExpressRoute-Querverbindung ab. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/routeTableSummary/read | Ruft eine Routentabellenzusammenfassung zum Peering einer ExpressRoute-Querverbindung ab. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/write | Erstellt ein Peering für eine ExpressRoute-Querverbindung oder aktualisiert ein Peering für eine vorhandene ExpressRoute-Querverbindung. |
> | Action | Microsoft.Network/expressRouteCrossConnections/read | Ruft eine ExpressRoute-Querverbindung ab. |
> | Action | Microsoft.Network/expressRouteGateways/expressRouteConnections/delete | Löscht eine ExpressRoute-Verbindung. |
> | Action | Microsoft.Network/expressRouteGateways/expressRouteConnections/read | Ruft eine ExpressRoute-Verbindung ab. |
> | Action | Microsoft.Network/expressRouteGateways/expressRouteConnections/write | Erstellt eine ExpressRoute-Verbindung oder aktualisiert eine vorhandene ExpressRoute-Verbindung. |
> | Action | Microsoft.Network/expressRouteGateways/join/action | Verknüpft ein ExpressRoute-Gateway. Nicht warnbar. |
> | Action | Microsoft.Network/expressRouteGateways/read | Ruft ein ExpressRoute-Gateway ab. |
> | Action | Microsoft.Network/expressRoutePorts/delete | Löscht ExpressRoutePorts. |
> | Action | Microsoft.Network/expressRoutePorts/join/action | Verknüpft die Express Route-Ports. Nicht warnbar. |
> | Action | Microsoft.Network/expressRoutePorts/links/read | Hiermit rufen Sie ExpressRouteLink ab. |
> | Action | Microsoft.Network/expressRoutePorts/read | Ruft ExpressRoutePorts ab. |
> | Action | Microsoft.Network/expressRoutePorts/write | Erstellt oder aktualisiert ExpressRoutePorts. |
> | Action | Microsoft.Network/expressRoutePortsLocations/read | Hiermit rufen Sie Standorte von ExpressRoute-Ports ab. |
> | Action | Microsoft.Network/expressRouteServiceProviders/read | Ruft ExpressRoute-Dienstanbieter ab. |
> | Action | Microsoft.Network/firewallPolicies/delete | Löscht eine Firewallregel. |
> | Action | Microsoft.Network/firewallPolicies/join/action | Verknüpft eine Firewallregel. Nicht warnbar. |
> | Action | Microsoft.Network/firewallPolicies/read | Ruft eine Firewallregel ab. |
> | Action | Microsoft.Network/firewallPolicies/ruleGroups/delete | Löscht eine Gruppe von Firewallrichtlinienregeln. |
> | Action | Microsoft.Network/firewallPolicies/ruleGroups/read | Ruft eine Gruppe von Firewallrichtlinienregeln ab. |
> | Action | Microsoft.Network/firewallPolicies/ruleGroups/write | Erstellt eine Gruppe von Firewallrichtlinienregeln oder aktualisiert eine vorhandene Gruppe von Firewallrichtlinienregeln. |
> | Action | Microsoft.Network/firewallPolicies/write | Erstellt eine Firewallrichtlinie oder aktualisiert eine vorhandene Gruppe von Firewallrichtlinien. |
> | Action | Microsoft.Network/frontDoors/backendPools/delete | Löscht einen Back-End-Pool. |
> | Action | Microsoft.Network/frontDoors/backendPools/read | Ruft einen Back-End-Pool ab. |
> | Action | Microsoft.Network/frontDoors/backendPools/read | Erstellt oder aktualisiert einen Back-End-Pool. |
> | Action | Microsoft.Network/frontDoors/delete | Löscht eine „Front Door“. |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/delete | Löscht einen Front-End-Endpunkt. |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/disableHttps/action | Deaktiviert HTTPS für einen Front-End-Endpunkt. |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/enableHttps/action | Aktiviert HTTPS für einen Front-End-Endpunkt. |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/read | Ruft einen Front-End-Endpunkt ab. |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/write | Erstellt oder aktualisiert einen Front-End-Endpunkt. |
> | Action | Microsoft.Network/frontDoors/healthProbeSettings/delete | Löscht Einstellungen für die Integritätsüberprüfung. |
> | Action | Microsoft.Network/frontDoors/healthProbeSettings/read | Ruft Einstellungen für die Integritätsüberprüfung ab. |
> | Action | Microsoft.Network/frontDoors/healthProbeSettings/write | Erstellt oder aktualisiert Einstellungen für die Integritätsüberprüfung. |
> | Action | Microsoft.Network/frontDoors/loadBalancingSettings/delete | Erstellt oder aktualisiert Einstellungen für den Lastenausgleich. |
> | Action | Microsoft.Network/frontDoors/loadBalancingSettings/read | Ruft Lastenausgleichseinstellungen ab. |
> | Action | Microsoft.Network/frontDoors/loadBalancingSettings/write | Erstellt oder aktualisiert Einstellungen für den Lastenausgleich. |
> | Action | Microsoft.Network/frontDoors/purge/action | Bereinigt von zwischengespeicherten Inhalt aus einer „Front Door“. |
> | Action | Microsoft.Network/frontDoors/read | Ruft eine „Front Door“ ab. |
> | Action | Microsoft.Network/frontDoors/routingRules/delete | Löscht eine Routingregel. |
> | Action | Microsoft.Network/frontDoors/routingRules/read | Ruft eine Routingregel ab. |
> | Action | Microsoft.Network/frontDoors/routingRules/write | Erstellt oder aktualisiert eine Routingregel. |
> | Action | Microsoft.Network/frontDoors/validateCustomDomain/action | Überprüft einen Front-End-Endpunkt für eine Front Door. |
> | Action | Microsoft.Network/frontDoors/write | Erstellt oder aktualisiert eine Front Door. |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallManagedRuleSets/read | Ruft verwaltete Web Application Firewall-Regelsätze ab |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/delete | Löscht eine Web Application Firewall-Richtlinie. |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/join/action | Bindet eine Web Application Firewall-Richtlinie ein. Nicht warnbar. |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/read | Ruft eine Web Application Firewall-Richtlinie ab. |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/write | Erstellt oder aktualisiert eine Web Application Firewall-Richtlinie. |
> | Action | Microsoft.Network/ipGroups/delete | Löscht eine IpGroup. |
> | Action | Microsoft.Network/ipGroups/join/action | Bindet eine IpGroup ein. Nicht warnbar. |
> | Action | Microsoft.Network/ipGroups/read | Ruft eine IpGroup ab. |
> | Action | Microsoft.Network/ipGroups/write | Erstellt eine IpGroup oder aktualisiert eine vorhandene IpGroup. |
> | Action | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für den Lastenausgleich. Nicht warnbar. |
> | Action | Microsoft.Network/loadBalancers/backendAddressPools/read | Ruft eine Back-End-Adresspooldefinition für den Lastenausgleich ab. |
> | Action | Microsoft.Network/loadBalancers/delete | Löscht einen Lastenausgleich. |
> | Action | Microsoft.Network/loadBalancers/frontendIPConfigurations/join/action | Verknüpft eine Front-End-IP-Konfiguration für den Lastenausgleich. Nicht warnbar. |
> | Action | Microsoft.Network/loadBalancers/frontendIPConfigurations/read | Ruft eine Front-End-IP-Konfigurationsdefinition für den Lastenausgleich ab. |
> | Action | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Verknüpft einen NAT-Pool für eingehenden Datenverkehr für den Lastenausgleich. Nicht warnbar. |
> | Action | Microsoft.Network/loadBalancers/inboundNatPools/read | Ruft eine eingehende NAT-Pooldefinition für den Lastenausgleich ab. |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/delete | Löscht eine eingehende NAT-Regel für den Lastenausgleich. |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Verknüpft eine NAT-Regel für eingehenden Datenverkehr für den Lastenausgleich. Nicht warnbar. |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/read | Ruft eine eingehende NAT-Regeldefinition für den Lastenausgleich ab. |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/write | Erstellt eine eingehende NAT-Regel für den Lastenausgleich oder aktualisiert eine vorhandene eingehende NAT-Regel für den Lastenausgleich. |
> | Action | Microsoft.Network/loadBalancers/loadBalancingRules/read | Ruft eine Lastenausgleichsregel-Definition für den Lastenausgleich ab. |
> | Action | Microsoft.Network/loadBalancers/networkInterfaces/read | Ruft Verweise auf alle Netzwerkschnittstellen unter einem Lastenausgleich ab. |
> | Action | Microsoft.Network/loadBalancers/outboundRules/read | Ruft eine ausgehende Regeldefinition für den Lastenausgleich ab. |
> | Action | Microsoft.Network/loadBalancers/probes/join/action | Ermöglicht die Verwendung von Prüfpunkten eines Lastenausgleichs. Beispielsweise kann mit dieser Berechtigung die healthProbe-Eigenschaft einer VM-Skalierungsgruppe auf den Prüfpunkt verweisen. Nicht warnbar. |
> | Action | Microsoft.Network/loadBalancers/probes/read | Ruft einen Lastenausgleichstest ab. |
> | Action | Microsoft.Network/loadBalancers/read | Ruft eine Lastenausgleichsdefinition ab. |
> | Action | Microsoft.Network/loadBalancers/virtualMachines/read | Ruft Verweise auf alle virtuellen Computer unter einem Lastenausgleich ab. |
> | Action | Microsoft.Network/loadBalancers/write | Erstellt einen Lastenausgleich oder aktualisiert einen vorhandenen Lastenausgleich. |
> | Action | Microsoft.Network/localnetworkgateways/delete | Löscht lokale Netzwerkgateways. |
> | Action | Microsoft.Network/localnetworkgateways/read | Ruft lokale Netzwerkgateways ab. |
> | Action | Microsoft.Network/localnetworkgateways/write | Erstellt ein lokales Netzwerkgateway oder aktualisiert ein vorhandenes lokales Netzwerkgateway. |
> | Action | Microsoft.Network/locations/autoApprovedPrivateLinkServices/read | Ruft automatisch genehmigte Private Link-Dienste ab |
> | Action | Microsoft.Network/locations/availableDelegations/read | Ruft verfügbare Delegierungen ab. |
> | Action | Microsoft.Network/locations/availablePrivateEndpointTypes/read | Ruft die verfügbaren privaten Endpunktressourcen ab. |
> | Action | Microsoft.Network/locations/availableServiceAliases/read | Ruft verfügbare Dienstaliase ab. |
> | Action | Microsoft.Network/locations/bareMetalTenants/action | Führt die Zuordnung oder Überprüfung eines Bare-Metal-Mandanten durch. |
> | Action | Microsoft.Network/locations/checkAcceleratedNetworkingSupport/action | Überprüft die Unterstützung für beschleunigte Netzwerke. |
> | Action | Microsoft.Network/locations/checkDnsNameAvailability/read | Prüft, ob eine DNS-Bezeichnung am angegebenen Standort verfügbar ist. |
> | Action | Microsoft.Network/locations/checkPrivateLinkServiceVisibility/action | Überprüft die Sichtbarkeit des Private Link-Diensts |
> | Action | Microsoft.Network/locations/operationResults/read | Ruft das Ergebnis eines asynchronen POST- oder DELETE-Vorgangs ab. |
> | Action | Microsoft.Network/locations/operations/read | Ruft die Vorgangsressource ab, die den Status eines asynchronen Vorgangs darstellt. |
> | Action | Microsoft.Network/locations/serviceTags/read | Ruft Diensttags ab |
> | Action | Microsoft.Network/locations/supportedVirtualMachineSizes/read | Ruft die unterstützten Größen für virtuelle Computer ab. |
> | Action | Microsoft.Network/locations/usages/read | Ruft die Ressourcenverwendungsmetriken ab. |
> | Action | Microsoft.Network/locations/virtualNetworkAvailableEndpointServices/read | Ruft eine Liste der verfügbaren Dienstendpunkte für virtuelle Netzwerke ab. |
> | Action | Microsoft.Network/networkIntentPolicies/delete | Löscht eine Netzwerkzielrichtlinie. |
> | Action | Microsoft.Network/networkIntentPolicies/read | Ruft die Beschreibung einer Netzwerkzielrichtlinie ab. |
> | Action | Microsoft.Network/networkIntentPolicies/write | Erstellt eine Netzwerkzielrichtlinie oder aktualisiert eine vorhandene Netzwerkzielrichtlinie. |
> | Action | Microsoft.Network/networkInterfaces/delete | Löscht eine Netzwerkschnittstelle. |
> | Action | Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action | Dient zum Abrufen der Netzwerksicherheitsgruppen, die für die Netzwerkschnittstelle des virtuellen Computers konfiguriert sind. |
> | Action | Microsoft.Network/networkInterfaces/effectiveRouteTable/action | Dient zum Abrufen der Routingtabelle, die für die Netzwerkschnittstelle des virtuellen Computers konfiguriert ist. |
> | Action | Microsoft.Network/networkInterfaces/ipconfigurations/join/action | Verknüpft eine Netzwerkschnittstellen-IP-Konfiguration. Nicht warnbar. |
> | Action | Microsoft.Network/networkInterfaces/ipconfigurations/read | Ruft eine IP-Konfigurationsdefinition für Netzwerkschnittstellen ab.  |
> | Action | Microsoft.Network/networkInterfaces/join/action | Verknüpft einen virtuellen Computer mit einer Netzwerkschnittstelle. Nicht warnbar. |
> | Action | Microsoft.Network/networkInterfaces/loadBalancers/read | Ruft alle Lastenausgleichsmodule ab, denen die Netzwerkschnittstelle angehört. |
> | Action | Microsoft.Network/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | Action | Microsoft.Network/networkInterfaces/tapConfigurations/delete | Löscht eine Netzwerkschnittstellen-TAP-Konfiguration. |
> | Action | Microsoft.Network/networkInterfaces/tapConfigurations/read | Ruft eine Netzwerkschnittstellen-TAP-Konfiguration ab. |
> | Action | Microsoft.Network/networkInterfaces/tapConfigurations/write | Erstellt eine Netzwerkschnittstellen-TAP-Konfiguration oder aktualisiert eine vorhandene Netzwerkschnittstellen-TAP-Konfiguration. |
> | Action | Microsoft.Network/networkInterfaces/write | Erstellt eine Netzwerkschnittstelle oder aktualisiert eine vorhandene Netzwerkschnittstelle.  |
> | Action | Microsoft.Network/networkProfiles/delete | Löscht ein Netzwerkprofil. |
> | Action | Microsoft.Network/networkProfiles/read | Liest ein Netzwerkprofil. |
> | Action | Microsoft.Network/networkProfiles/removeContainers/action | Entfernt Container. |
> | Action | Microsoft.Network/networkProfiles/setContainers/action | Legt Container fest. |
> | Action | Microsoft.Network/networkProfiles/setNetworkInterfaces/action | Legt Netzwerkschnittstellen von Containern fest. |
> | Action | Microsoft.Network/networkProfiles/write | Erstellt oder aktualisiert ein Netzwerkprofil. |
> | Action | Microsoft.Network/networkSecurityGroups/defaultSecurityRules/read | Ruft eine Standardsicherheitsregel-Definition ab. |
> | Action | Microsoft.Network/networkSecurityGroups/delete | Löscht eine Netzwerksicherheitsgruppe. |
> | Action | Microsoft.Network/networkSecurityGroups/join/action | Verknüpft eine Netzwerksicherheitsgruppe. Nicht warnbar. |
> | Action | Microsoft.Network/networkSecurityGroups/read | Ruft eine Netzwerksicherheitsgruppen-Definition ab. |
> | Action | Microsoft.Network/networkSecurityGroups/securityRules/delete | Löscht eine Sicherheitsregel. |
> | Action | Microsoft.Network/networkSecurityGroups/securityRules/read | Ruft eine Sicherheitsregeldefinition ab. |
> | Action | Microsoft.Network/networkSecurityGroups/securityRules/write | Erstellt eine Sicherheitsregel oder aktualisiert eine vorhandene Sicherheitsregel. |
> | Action | Microsoft.Network/networkSecurityGroups/write | Erstellt eine Netzwerksicherheitsgruppe oder aktualisiert eine vorhandene Netzwerksicherheitsgruppe. |
> | Action | Microsoft.Network/networkWatchers/availableProvidersList/action | Gibt alle verfügbaren Internetdienstanbieter für eine bestimmte Azure-Region zurück. |
> | Action | Microsoft.Network/networkWatchers/azureReachabilityReport/action | Gibt das relative Latenzergebnis für Internetdienstanbieter zwischen einem bestimmten Standort und Azure-Regionen zurück. |
> | Action | Microsoft.Network/networkWatchers/configureFlowLog/action | Konfiguriert die Datenflussprotokollierung für eine Zielressource. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/delete | Löscht einen Verbindungsmonitor. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/query/action | Fragt die Überwachungskonnektivität zwischen angegebenen Endpunkten ab. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/read | Ruft Details zum Verbindungsmonitor ab. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/start/action | Startet die Überwachungskonnektivität zwischen angegebenen Endpunkten. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/stop/action | Startet oder beendet die Überwachungskonnektivität zwischen angegebenen Endpunkten. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/write | Erstellt einen Verbindungsmonitor. |
> | Action | Microsoft.Network/networkWatchers/connectivityCheck/action | Überprüft die Möglichkeit, eine direkte TCP-Verbindung zwischen einem virtuellen Computer und einem angegebenen Endpunkt herzustellen, einschließlich mit anderen VMs oder einem freien Remoteserver. |
> | Action | Microsoft.Network/networkWatchers/delete | Löscht eine Network Watcher-Instanz. |
> | Action | Microsoft.Network/networkWatchers/flowLogs/delete | Löscht ein Datenflussprotokoll. |
> | Action | Microsoft.Network/networkWatchers/flowLogs/read | Ruft Details zu Datenflussprotokollen ab. |
> | Action | Microsoft.Network/networkWatchers/flowLogs/write | Erstellt ein Datenflussprotokoll. |
> | Action | Microsoft.Network/networkWatchers/ipFlowVerify/action | Gibt zurück, ob das Paket an oder von einem bestimmten Ziel zugelassen oder abgelehnt wird. |
> | Action | Microsoft.Network/networkWatchers/lenses/delete | Löscht einen Bereich. |
> | Action | Microsoft.Network/networkWatchers/lenses/query/action | Fragt die Überwachung des Netzwerkdatenverkehrs für einen angegebenen Endpunkt ab. |
> | Action | Microsoft.Network/networkWatchers/lenses/read | Ruft Bereichsdetails ab. |
> | Action | Microsoft.Network/networkWatchers/lenses/start/action | Startet die Überwachung des Netzwerkdatenverkehrs für einen angegebenen Endpunkt. |
> | Action | Microsoft.Network/networkWatchers/lenses/stop/action | Beendet die Überwachung des Netzwerkdatenverkehrs für einen angegebenen Endpunkt oder hält diese an. |
> | Action | Microsoft.Network/networkWatchers/lenses/write | Erstellt einen Bereich. |
> | Action | Microsoft.Network/networkWatchers/networkConfigurationDiagnostic/action | Führt eine Diagnose der Netzwerkkonfiguration durch. |
> | Action | Microsoft.Network/networkWatchers/nextHop/action | Gibt für ein angegebenes Ziel und eine Ziel-IP-Adresse die Art des nächsten Hops und die IP-Adresse des nächsten Hops zurück. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/delete | Löscht eine Paketerfassung. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Ruft Informationen zu Eigenschaften und zum Status einer Paketerfassungsressource ab. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/read | Dient zum Abrufen der Paketerfassungsdefinition. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/stop/action | Dient zum Beenden der laufenden Paketerfassungssitzung. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/write | Erstellt eine Paketerfassung. |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/delete | Löscht ein PingMesh-Element. |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/read | Ruft PingMesh-Details ab. |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/start/action | Startet PingMesh zwischen angegebenen VMs. |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/stop/action | Beendet PingMesh zwischen angegebenen VMs. |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/write | Erstellt ein PingMesh-Element. |
> | Action | Microsoft.Network/networkWatchers/queryConnectionMonitors/action | Führt eine Batchabfrage der Überwachungskonnektivität zwischen angegebenen Endpunkten ab. |
> | Action | Microsoft.Network/networkWatchers/queryFlowLogStatus/action | Ruft den Status der Datenflussprotokollierung für eine Ressource ab. |
> | Action | Microsoft.Network/networkWatchers/queryTroubleshootResult/action | Ruft das Ergebnis des zuvor oder momentan ausgeführten Problembehandlungsvorgangs ab. |
> | Action | Microsoft.Network/networkWatchers/read | Dient zum Abrufen der Network Watcher-Definition. |
> | Action | Microsoft.Network/networkWatchers/securityGroupView/action | Dient zum Anzeigen der konfigurierten und effektiven Netzwerksicherheitsgruppen-Regeln, die auf einen virtuellen Computer angewendet wurden. |
> | Action | Microsoft.Network/networkWatchers/topology/action | Ruft eine Netzwerkebenenansicht mit Ressourcen und deren Beziehungen in einer Ressourcengruppe ab. |
> | Action | Microsoft.Network/networkWatchers/troubleshoot/action | Startet die Problembehandlung für eine Netzwerkressource in Azure. |
> | Action | Microsoft.Network/networkWatchers/write | Erstellt eine Network Watcher-Instanz oder aktualisiert eine vorhandene Network Watcher-Instanz. |
> | Action | Microsoft.Network/operations/read | Dient zum Abrufen verfügbarer Vorgänge. |
> | Action | Microsoft.Network/p2sVpnGateways/delete | Löscht ein P2SVpnGateway. |
> | Action | Microsoft.Network/p2sVpnGateways/disconnectp2svpnconnections/action | Trennt P2S-VPN-Verbindungen |
> | Action | Microsoft.Network/p2sVpnGateways/generatevpnprofile/action | Generiert ein VPN-Profil für P2SVpnGateway. |
> | Action | Microsoft.Network/p2sVpnGateways/getp2svpnconnectionhealth/action | Ruft die P2S-VPN-Verbindungsintegrität für P2SVpnGateway ab. |
> | Action | Microsoft.Network/p2sVpnGateways/getp2svpnconnectionhealthdetailed/action | Ruft die P2S-VPN-Verbindungsintegrität für P2SVpnGateway ab |
> | Action | Microsoft.Network/p2sVpnGateways/read | Ruft ein P2SVpnGateway ab. |
> | Action | Microsoft.Network/p2sVpnGateways/write | Legt ein P2SVpnGateway fest. |
> | Action | Microsoft.Network/privateDnsOperationResults/read | Ruft Ergebnisse eines Vorgangs mit Privatem DNS ab |
> | Action | Microsoft.Network/privateDnsOperationStatuses/read | Ruft den Status eines Vorgangs mit Privatem DNS ab |
> | Action | Microsoft.Network/privateDnsZones/A/delete | Entfernt den Ressourceneintragssatz mit einem bestimmten Namen und dem Typ „A“ aus einer Zone mit Privatem DNS |
> | Action | Microsoft.Network/privateDnsZones/A/read | Ruft den Ressourceneintragssatz vom Typ „A“ innerhalb einer Zone mit Privatem DNS im JSON-Format ab. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/privateDnsZones/A/write | Erstellt oder aktualisiert einen Ressourceneintragssatz vom Typ „A“ innerhalb einer Zone mit Privatem DNS. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/privateDnsZones/AAAA/delete | Entfernt den Ressourceneintragssatz mit einem bestimmten Namen und dem Typ „AAAA“ aus einer Zone mit Privatem DNS |
> | Action | Microsoft.Network/privateDnsZones/AAAA/read | Ruft den Ressourceneintragssatz vom Typ „AAAA“ innerhalb einer Zone mit Privatem DNS im JSON-Format ab. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/privateDnsZones/AAAA/write | Erstellt oder aktualisiert einen Ressourceneintragssatz vom Typ „AAAA“ innerhalb einer Zone mit Privatem DNS. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/privateDnsZones/ALL/read | Ruft Ressourceneintragssätze mit Privatem DNS typenübergreifend ab |
> | Action | Microsoft.Network/privateDnsZones/CNAME/delete | Entfernt den Ressourceneintragssatz mit einem bestimmten Namen und dem Typ „CNAME“ aus einer Zone mit Privatem DNS |
> | Action | Microsoft.Network/privateDnsZones/CNAME/read | Ruft den Ressourceneintragssatz vom Typ „CNAME“ innerhalb einer Zone mit Privatem DNS im JSON-Format ab |
> | Action | Microsoft.Network/privateDnsZones/CNAME/write | Erstellt oder aktualisiert einen Ressourceneintragssatz vom Typ „CNAME“ innerhalb einer Zone mit Privatem DNS |
> | Action | Microsoft.Network/privateDnsZones/delete | Löscht eine Zone mit Privatem DNS |
> | Action | Microsoft.Network/privateDnsZones/MX/delete | Entfernt den Ressourceneintragssatz mit einem bestimmten Namen und dem Typ „MX“ aus einer Zone mit Privatem DNS |
> | Action | Microsoft.Network/privateDnsZones/MX/read | Ruft den Ressourceneintragssatz vom Typ „MX“ innerhalb einer Zone mit Privatem DNS im JSON-Format ab. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/privateDnsZones/MX/write | Erstellt oder aktualisiert einen Ressourceneintragssatz vom Typ „MX“ innerhalb einer Zone mit Privatem DNS. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/privateDnsZones/PTR/delete | Entfernt den Ressourceneintragssatz mit einem bestimmten Namen und dem Typ „PTR“ aus einer Zone mit Privatem DNS |
> | Action | Microsoft.Network/privateDnsZones/PTR/read | Ruft den Ressourceneintragssatz vom Typ „PTR“ innerhalb einer Zone mit Privatem DNS im JSON-Format ab. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/privateDnsZones/PTR/write | Erstellt oder aktualisiert einen Ressourceneintragssatz vom Typ „PTR“ innerhalb einer Zone mit Privatem DNS. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/privateDnsZones/read | Ruft die Eigenschaften der Zone mit Privatem DNS im JSON-Format ab. Beachten Sie, dass dieser Befehl weder die virtuellen Netzwerke abruft, mit denen die Zone mit Privatem DNS verknüpft ist, noch die in der Zone enthaltenen Ressourceneintragssätze. |
> | Action | Microsoft.Network/privateDnsZones/recordsets/read | Ruft Ressourceneintragssätze mit Privatem DNS typenübergreifend ab |
> | Action | Microsoft.Network/privateDnsZones/SOA/read | Ruft den Ressourceneintragssatz vom Typ „SOA“ innerhalb einer Zone mit Privatem DNS im JSON-Format ab |
> | Action | Microsoft.Network/privateDnsZones/SOA/write | Aktualisiert einen Ressourceneintragssatz vom Typ „SOA“ innerhalb einer Zone mit Privatem DNS |
> | Action | Microsoft.Network/privateDnsZones/SRV/delete | Entfernt den Ressourceneintragssatz mit einem bestimmten Namen und dem Typ „SRV“ aus einer Zone mit Privatem DNS |
> | Action | Microsoft.Network/privateDnsZones/SRV/read | Ruft den Ressourceneintragssatz vom Typ „SRV“ innerhalb einer Zone mit Privatem DNS im JSON-Format ab. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/privateDnsZones/SRV/write | Erstellt oder aktualisiert einen Ressourceneintragssatz vom Typ „SRV“ innerhalb einer Zone mit Privatem DNS. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/privateDnsZones/TXT/delete | Entfernt den Ressourceneintragssatz mit einem bestimmten Namen und dem Typ „TXT“ aus einer Zone mit Privatem DNS |
> | Action | Microsoft.Network/privateDnsZones/TXT/read | Ruft den Ressourceneintragssatz vom Typ „TXT“ innerhalb einer Zone mit Privatem DNS im JSON-Format ab. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Action | Microsoft.Network/privateDnsZones/TXT/write | Erstellt oder aktualisiert einen Ressourceneintragssatz vom Typ „TXT“ innerhalb einer Zone mit Privatem DNS. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Action | Microsoft.Network/privateDnsZones/virtualNetworkLinks/delete | Löscht die Verknüpfung einer Zone mit Privatem DNS mit dem virtuellen Netzwerk |
> | Action | Microsoft.Network/privateDnsZones/virtualNetworkLinks/read | Ruft die Verknüpfung der Zone mit Privatem DNS mit den Eigenschaften des virtuellen Netzwerks im JSON-Format ab |
> | Action | Microsoft.Network/privateDnsZones/virtualNetworkLinks/write | Erstellt oder aktualisiert die Verknüpfung einer Zone mit Privatem DNS mit dem virtuellen Netzwerk |
> | Action | Microsoft.Network/privateDnsZones/write | Erstellt oder aktualisiert eine Zone mit Privatem DNS innerhalb einer Ressourcengruppe. Beachten Sie, dass dieser Befehl nicht zum Erstellen oder Aktualisieren von Verknüpfungen mit dem virtuellen Netzwerk oder von Ressourceneintragssätzen innerhalb der Zone verwendet werden kann. |
> | Action | Microsoft.Network/privateEndpointRedirectMaps/read | Ruft eine RedirectMap für einen privaten Endpunkt ab |
> | Action | Microsoft.Network/privateEndpointRedirectMaps/write | Erstellt eine RedirectMap für einen privaten Endpunkt oder aktualisiert eine vorhandene RedirectMap für einen privaten Endpunkt |
> | Action | Microsoft.Network/privateEndpoints/delete | Löscht eine private Endpunktressource. |
> | Action | Microsoft.Network/privateEndpoints/read | Ruft eine private Endpunktressource ab. |
> | Action | Microsoft.Network/privateEndpoints/write | Erstellt einen neuen privaten Endpunkt oder aktualisiert einen vorhandenen privaten Endpunkt. |
> | Action | Microsoft.Network/privateLinkServices/delete | Löscht eine private Verknüpfungsdienst-Ressource. |
> | Action | Microsoft.Network/privateLinkServices/privateEndpointConnections/delete | Löscht eine private Endpunktverbindung. |
> | Action | Microsoft.Network/privateLinkServices/privateEndpointConnections/read | Ruft die Definition einer privaten Endpunktverbindung ab. |
> | Action | Microsoft.Network/privateLinkServices/privateEndpointConnections/write | Erstellt eine neue private Endpunktverbindung oder aktualisiert eine vorhandene private Endpunktverbindung. |
> | Action | Microsoft.Network/privateLinkServices/read | Ruft eine private Verknüpfungsdienst-Ressource ab. |
> | Action | Microsoft.Network/privateLinkServices/write | Erstellt einen neuen privaten Verknüpfungsdienst oder aktualisiert einen vorhandenen privaten Verknüpfungsdienst. |
> | Action | Microsoft.Network/publicIPAddresses/delete | Löscht eine öffentliche IP-Adresse. |
> | Action | Microsoft.Network/publicIPAddresses/join/action | Verknüpft eine öffentliche IP-Adresse. Nicht warnbar. |
> | Action | Microsoft.Network/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | Action | Microsoft.Network/publicIPAddresses/write | Erstellt eine öffentliche IP-Adresse oder aktualisiert eine vorhandene öffentliche IP-Adresse.  |
> | Action | Microsoft.Network/publicIPPrefixes/delete | Löscht ein Präfix einer öffentlichen IP-Adresse. |
> | Action | Microsoft.Network/publicIPPrefixes/join/action | Verknüpft ein PublicIPPrefix. Nicht warnbar. |
> | Action | Microsoft.Network/publicIPPrefixes/read | Ruft eine Definition eines Präfix einer öffentlichen IP-Adresse ab. |
> | Action | Microsoft.Network/publicIPPrefixes/write | Erstellt ein Präfix einer öffentlichen IP-Adresse oder aktualisiert ein Präfix einer vorhandenen öffentlichen IP-Adresse. |
> | Action | Microsoft.Network/register/action | Registriert das Abonnement. |
> | Action | Microsoft.Network/routeFilters/delete | Löscht eine Routenfilterdefinition. |
> | Action | Microsoft.Network/routeFilters/join/action | Verknüpft einen Routenfilter. Nicht warnbar. |
> | Action | Microsoft.Network/routeFilters/read | Ruft eine Routenfilterdefinition ab. |
> | Action | Microsoft.Network/routeFilters/routeFilterRules/delete | Löscht eine Routenfilterregel-Definition. |
> | Action | Microsoft.Network/routeFilters/routeFilterRules/read | Ruft eine Routenfilterregel-Definition ab. |
> | Action | Microsoft.Network/routeFilters/routeFilterRules/write | Erstellt eine Routenfilterregel oder aktualisiert eine vorhandene Routenfilterregel. |
> | Action | Microsoft.Network/routeFilters/write | Erstellt einen Routenfilter oder aktualisiert einen vorhandenen Routenfilter. |
> | Action | Microsoft.Network/routeTables/delete | Löscht eine Routingtabellendefinition. |
> | Action | Microsoft.Network/routeTables/join/action | Verknüpft eine Routingtabelle. Nicht warnbar. |
> | Action | Microsoft.Network/routeTables/read | Ruft eine Routingtabellendefinition ab. |
> | Action | Microsoft.Network/routeTables/routes/delete | Löscht eine Routendefinition. |
> | Action | Microsoft.Network/routeTables/routes/read | Ruft eine Routendefinition ab. |
> | Action | Microsoft.Network/routeTables/routes/write | Erstellt eine Route oder aktualisiert eine vorhandene Route. |
> | Action | Microsoft.Network/routeTables/write | Erstellt eine Routingtabelle oder aktualisiert eine vorhandene Routingtabelle. |
> | Action | Microsoft.Network/serviceEndpointPolicies/delete | Löscht eine Dienstendpunktrichtlinie. |
> | Action | Microsoft.Network/serviceEndpointPolicies/join/action | Verknüpft eine Dienstendpunktrichtlinie. Nicht warnbar. |
> | Action | Microsoft.Network/serviceEndpointPolicies/joinSubnet/action | Verknüpft ein Subnetz mit Dienstendpunktrichtlinien. Nicht warnbar. |
> | Action | Microsoft.Network/serviceEndpointPolicies/read | Ruft die Beschreibung einer Dienstendpunktrichtlinie ab. |
> | Action | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete | Löscht die Definition einer Dienstendpunktrichtlinie. |
> | Action | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read | Ruft die Beschreibung für die Definition einer Dienstendpunktrichtlinie ab. |
> | Action | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write | Erstellt eine Definition für eine Dienstendpunktrichtlinie oder aktualisiert eine vorhandene Definition für eine Dienstendpunktrichtlinie. |
> | Action | Microsoft.Network/serviceEndpointPolicies/write | Erstellt eine Dienstendpunktrichtlinie oder aktualisiert eine vorhandene Dienstendpunktrichtlinie. |
> | Action | Microsoft.Network/trafficManagerGeographicHierarchies/read | Ruft die geografische Traffic Manager-Hierarchie mit Regionen ab, die mit der geografischen Routingmethode für Datenverkehr verwendet werden können. |
> | Action | Microsoft.Network/trafficManagerProfiles/azureEndpoints/delete | Löscht einen Azure-Endpunkt aus einem vorhandenen Traffic Manager-Profil an. Der Traffic Manager beendet die Weiterleitung des Datenverkehrs an den gelöschten Azure-Endpunkt. |
> | Action | Microsoft.Network/trafficManagerProfiles/azureEndpoints/read | Ruft einen Azure-Endpunkt ab, der einem Traffic Manager-Profil zugehörig ist, einschließlich aller Eigenschaften dieses Azure-Endpunkts. |
> | Action | Microsoft.Network/trafficManagerProfiles/azureEndpoints/write | Fügt einen neuen Azure-Endpunkt in einem vorhandenen Traffic Manager-Profil hinzu oder aktualisiert die Eigenschaften eines vorhandenen Azure-Endpunkts in diesem Traffic Manager-Profil. |
> | Action | Microsoft.Network/trafficManagerProfiles/delete | Dient zum Löschen des Traffic Manager-Profils. Alle dem Traffic Manager-Profil zugeordneten Einstellungen gehen verloren, und das Profil kann nicht mehr zum Routen von Datenverkehr verwendet werden. |
> | Action | Microsoft.Network/trafficManagerProfiles/externalEndpoints/delete | Löscht einen externen Endpunkt aus einem vorhandenen Traffic Manager-Profil. Der Traffic Manager beendet die Weiterleitung des Datenverkehrs an den gelöschten externen Endpunkt. |
> | Action | Microsoft.Network/trafficManagerProfiles/externalEndpoints/read | Ruft einen externen Endpunkt ab, der einem Traffic Manager-Profil zugehörig ist, einschließlich aller Eigenschaften dieses externen Endpunkts. |
> | Action | Microsoft.Network/trafficManagerProfiles/externalEndpoints/write | Fügt einen neuen externen Endpunkt in einem vorhandenen Traffic Manager-Profil hinzu oder aktualisiert die Eigenschaften eines vorhandenen externen Endpunkts in diesem Traffic Manager-Profil. |
> | Action | Microsoft.Network/trafficManagerProfiles/heatMaps/read | Ruft die Traffic Manager-Heatmap für das angegebene Traffic Manager-Profil ab, das die Anzahl der Abfragen und Latenzdaten nach Standort und Quell-IP enthält. |
> | Action | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/delete | Löscht einen geschachtelten Endpunkt aus einem vorhandenen Traffic Manager-Profil. Der Traffic Manager beendet die Weiterleitung des Datenverkehrs an den gelöschten geschachtelten Endpunkt. |
> | Action | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/read | Ruft einen geschachtelten Endpunkt ab, der einem Traffic Manager-Profil zugehörig ist, einschließlich aller Eigenschaften dieses geschachtelten Endpunkts. |
> | Action | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/write | Fügt einen neuen geschachtelten Endpunkt in einem vorhandenen Traffic Manager-Profil hinzu oder aktualisiert die Eigenschaften eines vorhandenen geschachtelten Endpunkts in diesem Traffic Manager-Profil. |
> | Action | Microsoft.Network/trafficManagerProfiles/read | Dient zum Abrufen der Traffic Manager-Profilkonfiguration. Dazu zählen DNS-Einstellungen, Routingeinstellungen für Datenverkehr, Endpunktüberwachungseinstellungen und die Liste mit den Endpunkten, die durch dieses Traffic Manager-Profil geroutet werden. |
> | Action | Microsoft.Network/trafficManagerProfiles/write | Dient zum Erstellen eines Traffic Manager-Profils oder zum Ändern der Konfiguration eines vorhandenen Traffic Manager-Profils.<br>Dies schließt das Aktivieren oder Deaktivieren eines Profils sowie das Ändern von DNS-Einstellungen, Routingeinstellungen für Datenverkehr oder Endpunktüberwachungseinstellungen ein.<br>Durch das Traffic Manager-Profil geroutete Endpunkte können hinzugefügt, entfernt, aktiviert oder deaktiviert werden. |
> | Action | Microsoft.Network/trafficManagerUserMetricsKeys/delete | Löscht die Schlüssel auf Abonnementebene, die für die Collection für Echtzeitbenutzermetriken verwendet wird. |
> | Action | Microsoft.Network/trafficManagerUserMetricsKeys/read | Ruft die Schlüssel auf Abonnementebene für die Collection mit Echtzeitbenutzermetriken ab. |
> | Action | Microsoft.Network/trafficManagerUserMetricsKeys/write | Erstellt einen neuen Schlüssel auf Abonnementebene, der für die Collection mit Echtzeitbenutzermetriken verwendet werden soll. |
> | Action | Microsoft.Network/unregister/action | Hebt die Registrierung des Abonnements auf. |
> | Action | Microsoft.Network/virtualHubs/delete | Löscht einen virtuellen Hub. |
> | Action | Microsoft.Network/virtualHubs/effectiveRoutes/action | Ruft die effektive Route ab, die für den virtuellen Hub konfiguriert wurde. |
> | Action | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/delete | Löscht HubVirtualNetworkConnection. |
> | Action | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/read | Ruft HubVirtualNetworkConnection ab. |
> | Action | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/write | Erstellt oder aktualisiert HubVirtualNetworkConnection. |
> | Action | Microsoft.Network/virtualHubs/read | Ruft einen virtuellen Hub ab. |
> | Action | Microsoft.Network/virtualHubs/routeTables/delete | Löschen einer VirtualHubRouteTableV2 |
> | Action | Microsoft.Network/virtualHubs/routeTables/read | Abrufen einer VirtualHubRouteTableV2 |
> | Action | Microsoft.Network/virtualHubs/routeTables/write | Erstellen oder Aktualisieren einer VirtualHubRouteTableV2 |
> | Action | Microsoft.Network/virtualHubs/write | Erstellt oder aktualisiert einen virtuellen Hub. |
> | Action | microsoft.network/virtualnetworkgateways/connections/read | Ruft VirtualNetworkGatewayConnection ab. |
> | Action | Microsoft.Network/virtualNetworkGateways/delete | Löscht virtualNetworkGateway. |
> | Action | microsoft.network/virtualnetworkgateways/disconnectvirtualnetworkgatewayvpnconnections/action | Trennt VPN-Verbindungen des Gateways für virtuelle Netzwerke. |
> | Action | microsoft.network/virtualnetworkgateways/generatevpnclientpackage/action | Generiert ein VpnClient-Paket für virtualNetworkGateway. |
> | Action | microsoft.network/virtualnetworkgateways/generatevpnprofile/action | Generiert ein VpnProfile-Paket für VirtualNetworkGateway. |
> | Action | microsoft.network/virtualnetworkgateways/getadvertisedroutes/action | Ruft angekündigte virtualNetworkGateway-Routen ab. |
> | Action | microsoft.network/virtualnetworkgateways/getbgppeerstatus/action | Ruft den Status des BGP-Peerings für virtualNetworkGateway ab. |
> | Action | microsoft.network/virtualnetworkgateways/getlearnedroutes/action | Ruft die erlernten virtualNetworkGateway-Routen ab. |
> | Action | microsoft.network/virtualnetworkgateways/getvpnclientconnectionhealth/action | Ruft für VirtualNetworkGateway die Integrität der VPN-Clientverbindung ab. |
> | Action | microsoft.network/virtualnetworkgateways/getvpnclientipsecparameters/action | Ruft die Vpnclient-IPsec-Parameter für den VirtualNetworkGateway P2S-Client ab. |
> | Action | microsoft.network/virtualnetworkgateways/getvpnprofilepackageurl/action | Ruft die URL eines vorab generierten VPN-Clientprofilpakets ab. |
> | Action | Microsoft.Network/virtualNetworkGateways/read | Ruft VirtualNetworkGateway ab. |
> | Action | microsoft.network/virtualnetworkgateways/reset/action | Setzt virtualNetworkGateway zurück. |
> | Action | microsoft.network/virtualnetworkgateways/resetvpnclientsharedkey/action | Setzt den gemeinsam verwendeten Vpnclient-Schlüssel für den VirtualNetworkGateway P2S-Client zurück. |
> | Action | microsoft.network/virtualnetworkgateways/setvpnclientipsecparameters/action | Legt die Vpnclient-IPsec-Parameter für den VirtualNetworkGateway P2S-Client fest. |
> | Action | microsoft.network/virtualnetworkgateways/startpacketcapture/action | Startet die Erfassung der Pakete eines Gateways für virtuelle Netzwerke. |
> | Action | microsoft.network/virtualnetworkgateways/stoppacketcapture/action | Beendet die Erfassung der Pakete eines Gateways für virtuelle Netzwerke. |
> | Action | Microsoft.Network/virtualnetworkgateways/supportedvpndevices/action | Listet die unterstützten VPN-Geräte auf. |
> | Action | Microsoft.Network/virtualNetworkGateways/write | Erstellt oder aktualisiert VirtualNetworkGateway. |
> | Action | Microsoft.Network/virtualNetworks/BastionHosts/action | Ruft Bastionhostverweise in einem virtuellen Netzwerk ab. |
> | Action | Microsoft.Network/virtualNetworks/bastionHosts/default/action | Ruft Bastionhostverweise in einem virtuellen Netzwerk ab. |
> | Action | Microsoft.Network/virtualNetworks/checkIpAddressAvailability/read | Prüft, ob eine IP-Adresse im angegebenen virtuellen Netzwerk verfügbar ist. |
> | Action | Microsoft.Network/virtualNetworks/delete | Löscht ein virtuelles Netzwerk. |
> | Action | Microsoft.Network/virtualNetworks/join/action | Verknüpft ein virtuelles Netzwerk. Nicht warnbar. |
> | Action | Microsoft.Network/virtualNetworks/peer/action | Richtet ein virtuelles Netzwerk als Peer für ein anderes virtuelles Netzwerk ein. |
> | Action | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Action | Microsoft.Network/virtualNetworks/subnets/contextualServiceEndpointPolicies/delete | Löscht eine kontextbezogene Dienstendpunktrichtlinie. |
> | Action | Microsoft.Network/virtualNetworks/subnets/contextualServiceEndpointPolicies/read | Ruft kontextbezogene Dienstendpunktrichtlinien ab. |
> | Action | Microsoft.Network/virtualNetworks/subnets/contextualServiceEndpointPolicies/write | Erstellt eine kontextbezogene Dienstendpunktrichtlinie oder aktualisiert eine vorhandene kontextbezogene Dienstendpunktrichtlinie. |
> | Action | Microsoft.Network/virtualNetworks/subnets/delete | Löscht ein Subnetz für virtuelle Netzwerke. |
> | Action | Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. Nicht warnbar. |
> | Action | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. Nicht warnbar. |
> | Action | Microsoft.Network/virtualNetworks/subnets/prepareNetworkPolicies/action | Bereitet ein Subnetz vor, indem erforderliche Netzwerkrichtlinien angewendet werden. |
> | Action | Microsoft.Network/virtualNetworks/subnets/read | Ruft eine Subnetzdefinition für virtuelle Netzwerke ab. |
> | Action | Microsoft.Network/virtualNetworks/subnets/unprepareNetworkPolicies/action | Aufheben der Vorbereitung eines Subnetzes durch Entfernen der angewandten Netzwerkrichtlinien |
> | Action | Microsoft.Network/virtualNetworks/subnets/virtualMachines/read | Ruft Verweise auf alle virtuellen Computer im Subnetz eines virtuellen Netzwerks ab. |
> | Action | Microsoft.Network/virtualNetworks/subnets/write | Erstellt ein Subnetz für virtuelle Netzwerke oder aktualisiert ein vorhandenes Subnetz für virtuelle Netzwerke. |
> | Action | Microsoft.Network/virtualNetworks/usages/read | Ruft die IP-Adressenbelegungen für jedes Subnetz des virtuellen Netzwerks ab. |
> | Action | Microsoft.Network/virtualNetworks/virtualMachines/read | Ruft Verweise auf alle virtuellen Computer in einem virtuellen Netzwerk ab. |
> | Action | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Löscht ein Peering virtueller Netzwerke. |
> | Action | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read | Ruft eine Definition für das Peering virtueller Netzwerke ab. |
> | Action | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Erstellt ein Peering virtueller Netzwerke oder aktualisiert ein vorhandenes Peering virtueller Netzwerke. |
> | Action | Microsoft.Network/virtualNetworks/write | Erstellt ein virtuelles Netzwerk oder aktualisiert ein vorhandenes virtuelles Netzwerk. |
> | Action | Microsoft.Network/virtualNetworkTaps/delete | Löscht einen TAP eines virtuellen Netzwerks. |
> | Action | Microsoft.Network/virtualNetworkTaps/join/action | Verknüpft einen TAP für virtuelle Netzwerke. Nicht warnbar. |
> | Action | Microsoft.Network/virtualNetworkTaps/read | Ruft einen TAP eines virtuellen Netzwerks ab. |
> | Action | Microsoft.Network/virtualNetworkTaps/write | Erstellt oder aktualisiert einen TAP eines virtuellen Netzwerks. |
> | Action | Microsoft.Network/virtualRouters/delete | Löscht einen VirtualRouter |
> | Action | Microsoft.Network/virtualRouters/join/action | Verknüpft einen VirtualRouter Nicht warnbar. |
> | Action | Microsoft.Network/virtualRouters/peerings/delete | Löscht ein VirtualRouterPeering |
> | Action | Microsoft.Network/virtualRouters/peerings/read | Ruft ein VirtualRouterPeering ab |
> | Action | Microsoft.Network/virtualRouters/peerings/write | Erstellt ein VirtualRouterPeering oder aktualisiert ein vorhandenes |
> | Action | Microsoft.Network/virtualRouters/read | Ruft einen VirtualRouter ab |
> | Action | Microsoft.Network/virtualRouters/write | Erstellt einen VirtualRouter oder aktualisiert einen vorhandenen |
> | Action | Microsoft.Network/virtualWans/delete | Löscht ein virtuelles WAN. |
> | Action | Microsoft.Network/virtualwans/generateVpnProfile/action | Generieren eines VpnProfile für die VirtualWanVpnServerConfiguration |
> | Action | Microsoft.network/virtualWans/p2sVpnServerConfigurations/delete | Löscht P2SVpnServerConfiguration für ein virtuelles WAN. |
> | Action | Microsoft.Network/virtualWans/p2sVpnServerConfigurations/read | Ruft P2SVpnServerConfiguration für ein virtuelles WAN ab. |
> | Action | Microsoft.network/virtualWans/p2sVpnServerConfigurations/write | Erstellt oder aktualisiert P2SVpnServerConfiguration für ein virtuelles WAN. |
> | Action | Microsoft.Network/virtualWans/read | Ruft ein virtuelles WAN ab. |
> | Action | Microsoft.Network/virtualwans/supportedSecurityProviders/read | Ruft unterstützte VirtualWan-Sicherheitsanbieter ab. |
> | Action | Microsoft.Network/virtualWans/virtualHubs/read | Ruft alle virtuellen Hubs ab, die auf ein virtuelles WAN verweisen. |
> | Action | Microsoft.Network/virtualwans/vpnconfiguration/action | Ruft eine VPN-Konfiguration ab. |
> | Action | Microsoft.Network/virtualwans/vpnServerConfigurations/action | Abrufen von VirtualWanVpnServerConfigurations |
> | Action | Microsoft.Network/virtualWans/vpnSites/read | Ruft alle VPN-Standorte ab, die auf ein virtuelles WAN verweisen. |
> | Action | Microsoft.Network/virtualWans/write | Erstellt oder aktualisiert ein virtuelles WAN. |
> | Action | Microsoft.Network/vpnGateways/delete | Löscht VpnGateway. |
> | Action | microsoft.network/vpngateways/listvpnconnectionshealth/action | Ruft die Verbindungsintegrität für alle oder eine Teilmenge der Verbindungen auf einem VpnGateway ab. |
> | Action | Microsoft.Network/vpnGateways/read | Ruft VpnGateway ab. |
> | Action | microsoft.network/vpngateways/reset/action | Setzt VpnGateway zurück. |
> | Action | microsoft.network/vpnGateways/vpnConnections/delete | Löscht VpnConnection. |
> | Action | microsoft.network/vpnGateways/vpnConnections/read | Ruft VpnConnection ab. |
> | Action | microsoft.network/vpnGateways/vpnConnections/vpnLinkConnections/read | Ruft eine VPN-Linkverbindung ab |
> | Action | microsoft.network/vpnGateways/vpnConnections/write | Legt VpnConnection fest. |
> | Action | Microsoft.Network/vpnGateways/write | Legt VpnGateway fest. |
> | Action | Microsoft.Network/vpnServerConfigurations/delete | Löschen einer VpnServerConfiguration |
> | Action | Microsoft.Network/vpnServerConfigurations/read | Abrufen einer VpnServerConfiguration |
> | Action | Microsoft.Network/vpnServerConfigurations/write | Erstellen oder Aktualisieren einer VpnServerConfiguration |
> | Action | Microsoft.Network/vpnsites/delete | Löscht eine Ressource für einen VPN-Standort. |
> | Action | Microsoft.Network/vpnsites/read | Ruft eine Ressource für einen VPN-Standort ab. |
> | Action | microsoft.network/vpnSites/vpnSiteLinks/read | Ruft einen VPN-Standortlink ab |
> | Action | Microsoft.Network/vpnsites/write | Erstellt oder aktualisiert eine Ressource für einen VPN-Standort. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.NotificationHubs/CheckNamespaceAvailability/action | Prüft, ob ein bestimmter Name einer Namespaceressource innerhalb des NotificationHub-Diensts verfügbar ist. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden.  |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | Dient zum Abrufen der Verbindungszeichenfolge für den Namespace. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | Namespace-Autorisierungsregel zum erneuten Generieren des primären/sekundären Schlüssels. Geben Sie den Schlüssel an, der neu generiert werden soll. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel. |
> | Action | Microsoft.NotificationHubs/Namespaces/CheckNotificationHubAvailability/action | Prüft, ob ein bestimmter NotificationHub-Name innerhalb eines Namespaces verfügbar ist. |
> | Action | Microsoft.NotificationHubs/Namespaces/Delete | Dient zum Löschen von Namespaceressourcen. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | Dient zum Abrufen der Liste mit Notification Hub-Autorisierungsregeln. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Dient zum Löschen von Notification Hub-Autorisierungsregeln. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | Dient zum Abrufen der Notification Hub-Verbindungszeichenfolge. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | Dient zum Abrufen der Liste mit Notification Hub-Autorisierungsregeln. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | Notification Hub-Autorisierungsregel zum erneuten Generieren des primären/sekundären Schlüssels. Geben Sie den Schlüssel an, der neu generiert werden soll. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Dient zum Erstellen von Notification Hub-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/debugSend/action | Dient zum Senden einer Test-Pushbenachrichtigung. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/Delete | Dient zum Löschen von Notification Hub-Ressourcen. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/metricDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Dient zum Abrufen aller Notification Hub-PNS-Anmeldeinformationen. Hierzu zählen Anmeldeinformationen für WNS, MPNS, APNS, GCM und Baidu. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | Dient zum Abrufen einer Liste mit Notification Hub-Ressourcenbeschreibungen. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Dient zum Erstellen eines Notification Hubs und zum Aktualisieren seiner Eigenschaften. Zu diesen Einstellungen zählen in erster Linie PNS-Anmeldeinformationen, Autorisierungsregeln und Gültigkeitsdauer. |
> | Action | Microsoft.NotificationHubs/Namespaces/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen. |
> | Action | Microsoft.NotificationHubs/Namespaces/write | Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden. |
> | Action | Microsoft.NotificationHubs/operationResults/read | Gibt Vorgangsergebnisse für den Notification Hubs-Anbieter zurück. |
> | Action | Microsoft.NotificationHubs/operations/read | Gibt eine Liste unterstützter Vorgänge für den Notification Hubs-Anbieter zurück. |
> | Action | Microsoft.NotificationHubs/register/action | Registriert das Abonnement für den NotificationHubs-Ressourcenanbieter und ermöglicht die Erstellung von Namespaces und Notification Hubs. |
> | Action | Microsoft.NotificationHubs/unregister/action | Hebt die Registrierung des Abonnements für den NotificationHubs-Ressourcenanbieter auf und ermöglicht die Erstellung von Namespaces und Notification Hubs. |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.OffAzure/HyperVSites/clusters/read | Ruft die Eigenschaften eines Hyper-V-Clusters ab. |
> | Action | Microsoft.OffAzure/HyperVSites/clusters/write | Erstellt oder aktualisiert den Hyper-V-Cluster. |
> | Action | Microsoft.OffAzure/HyperVSites/delete | Löscht die Hyper-V-Site. |
> | Action | Microsoft.OffAzure/HyperVSites/healthsummary/read | Ruft die Integritätszusammenfassung für eine Hyper-V-Ressource ab |
> | Action | Microsoft.OffAzure/HyperVSites/hosts/read | Ruft die Eigenschaften eines Hyper-V-Hosts ab. |
> | Action | Microsoft.OffAzure/HyperVSites/hosts/write | Erstellt oder aktualisiert den Hyper-V-Host. |
> | Action | Microsoft.OffAzure/HyperVSites/jobs/read | Ruft die Eigenschaften eines Hyper-V-Auftrags ab. |
> | Action | Microsoft.OffAzure/HyperVSites/machines/read | Ruft die Eigenschaften eines Hyper-V-Computers ab. |
> | Action | Microsoft.OffAzure/HyperVSites/operationsstatus/read | Ruft die Eigenschaften eines Hyper-V-Vorgangsstatus ab. |
> | Action | Microsoft.OffAzure/HyperVSites/read | Ruft die Eigenschaften einer Hyper-V-Site ab. |
> | Action | Microsoft.OffAzure/HyperVSites/refresh/action | Aktualisiert die Objekte in einer Hyper-V-Site. |
> | Action | Microsoft.OffAzure/HyperVSites/runasaccounts/read | Ruft die Eigenschaften eines ausführenden Hyper-V-Kontos ab. |
> | Action | Microsoft.OffAzure/HyperVSites/usage/read | Ruft die Verwendungen einer Hyper-V-Site ab. |
> | Action | Microsoft.OffAzure/HyperVSites/write | Erstellt oder aktualisiert die Hyper-V-Site. |
> | Action | Microsoft.OffAzure/Operations/read | Liest die verfügbar gemachten Vorgänge. |
> | Action | Microsoft.OffAzure/register/action | Registriert ein Abonnement bei einem Microsoft.OffAzure-Ressourcenanbieter. |
> | Action | Microsoft.OffAzure/ServerSites/jobs/read | Ruft die Eigenschaften eines Serverauftrags ab. |
> | Action | Microsoft.OffAzure/ServerSites/machines/read | Ruft die Eigenschaften eines Servercomputers ab. |
> | Action | Microsoft.OffAzure/ServerSites/machines/write | Schreibt die Eigenschaften eines Servercomputers. |
> | Action | Microsoft.OffAzure/ServerSites/operationsstatus/read | Ruft die Eigenschaften eines Servervorgangsstatus ab. |
> | Action | Microsoft.OffAzure/ServerSites/read | Ruft die Eigenschaften einer Serversite ab. |
> | Action | Microsoft.OffAzure/ServerSites/refresh/action | Aktualisiert die Objekte in einer Serversite. |
> | Action | Microsoft.OffAzure/ServerSites/runasaccounts/read | Ruft die Eigenschaften eines ausführenden Serverkontos ab. |
> | Action | Microsoft.OffAzure/ServerSites/usage/read | Ruft die Verwendungen einer Serversite ab. |
> | Action | Microsoft.OffAzure/ServerSites/write | Erstellt oder aktualisiert die Serversite. |
> | Action | Microsoft.OffAzure/VMwareSites/delete | Löscht die VMware-Site. |
> | Action | Microsoft.OffAzure/VMwareSites/healthsummary/read | Ruft die Integritätszusammenfassung für eine VMware-Ressource ab |
> | Action | Microsoft.OffAzure/VMwareSites/jobs/read | Ruft die Eigenschaften eines VMware-Auftrags ab. |
> | Action | Microsoft.OffAzure/VMwareSites/machines/read | Ruft die Eigenschaften eines VMware-Computers ab. |
> | Action | Microsoft.OffAzure/VMwareSites/machines/start/action | Startet VMware-Computer. |
> | Action | Microsoft.OffAzure/VMwareSites/machines/stop/action | Beendet die VMware-Computer. |
> | Action | Microsoft.OffAzure/VMwareSites/operationsstatus/read | Ruft die Eigenschaften eines VMware-Vorgangsstatus ab. |
> | Action | Microsoft.OffAzure/VMwareSites/read | Ruft die Eigenschaften einer VMware-Site ab. |
> | Action | Microsoft.OffAzure/VMwareSites/refresh/action | Aktualisiert die Objekte in einer VMware-Site. |
> | Action | Microsoft.OffAzure/VMwareSites/runasaccounts/read | Ruft die Eigenschaften eines ausführenden VMware-Kontos ab. |
> | Action | Microsoft.OffAzure/VMwareSites/usage/read | Ruft die Verwendungen einer VMware-Site ab. |
> | Action | Microsoft.OffAzure/VMwareSites/vcenters/read | Ruft die Eigenschaften für eine VMware vCenter-Instanz ab. |
> | Action | Microsoft.OffAzure/VMwareSites/vcenters/write | Erstellt oder aktualisiert die VMware vCenter-Instanz. |
> | Action | Microsoft.OffAzure/VMwareSites/write | Erstellt oder aktualisiert die VMware-Site. |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.OperationalInsights/linkTargets/read | Listet vorhandene Konten auf, die keinem Azure-Abonnement zugeordnet sind. Verwenden Sie eine Kunden-ID, die von diesem Vorgang in der Eigenschaft für die Kunden-ID des Vorgangs „Arbeitsbereich erstellen“ zurückgegeben wird, um dieses Azure-Abonnement mit einem Arbeitsbereich zu verknüpfen. |
> | Action | microsoft.operationalinsights/operations/read | Listet alle verfügbaren OperationalInsights-REST-API-Vorgänge auf. |
> | Action | microsoft.operationalinsights/register/action | Registriert das Abonnement |
> | Action | Microsoft.OperationalInsights/register/action | Dient zum Registrieren eines Abonnements bei einem Ressourcenanbieter. |
> | Action | microsoft.operationalinsights/unregister/action | Hebt die Registrierung des Abonnements auf |
> | Action | Microsoft.OperationalInsights/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
> | Action | Microsoft.OperationalInsights/workspaces/analytics/query/schema/read | Rufen Sie das Suchschema V2 ab. |
> | Action | Microsoft.OperationalInsights/workspaces/api/query/action | Führt eine Suche mit der neuen Engine aus. |
> | Action | Microsoft.OperationalInsights/workspaces/api/query/schema/read | Rufen Sie das Suchschema V2 ab. |
> | Action | Microsoft.OperationalInsights/workspaces/configurationScopes/delete | Dient zum Löschen von Konfigurationsbereichen. |
> | Action | Microsoft.OperationalInsights/workspaces/configurationScopes/read | Dient zum Abrufen von Konfigurationsbereichen. |
> | Action | Microsoft.OperationalInsights/workspaces/configurationScopes/write | Dient zum Festlegen von Konfigurationsbereichen. |
> | Action | Microsoft.OperationalInsights/workspaces/datasources/delete | Dient zum Löschen von Datenquellen unter einem Arbeitsbereich. |
> | Action | Microsoft.OperationalInsights/workspaces/datasources/read | Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich. |
> | Action | Microsoft.OperationalInsights/workspaces/datasources/write | Dient zum Erstellen/Aktualisieren von Datenquellen unter einem Arbeitsbereich. |
> | Action | Microsoft.OperationalInsights/workspaces/delete | Löscht einen Arbeitsbereich. Wenn der Arbeitsbereich bei seiner Erstellung mit einem vorhandenen Arbeitsbereich verknüpft war, wird der Arbeitsbereich, mit dem er verknüpft war, nicht gelöscht. |
> | Action | Microsoft.OperationalInsights/workspaces/gateways/delete | Entfernt ein für den Arbeitsbereich konfiguriertes Gateway. |
> | Action | Microsoft.OperationalInsights/workspaces/generateregistrationcertificate/action | Generiert ein Registrierungszertifikat für den Arbeitsbereich. Dieses Zertifikat wird verwendet, um Microsoft System Center Operation Manager mit dem Arbeitsbereich zu verbinden. |
> | Action | Microsoft.OperationalInsights/workspaces/intelligencepacks/disable/action | Deaktiviert ein Intelligence Pack für einen bestimmten Arbeitsbereich. |
> | Action | Microsoft.OperationalInsights/workspaces/intelligencepacks/enable/action | Aktiviert ein Intelligence Pack für einen bestimmten Arbeitsbereich. |
> | Action | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Listet alle für einen angegebenen Arbeitsbereich sichtbaren Intelligence Packs auf und gibt an, ob das Pack für diesen Arbeitsbereich aktiviert oder deaktiviert ist. |
> | Action | Microsoft.OperationalInsights/workspaces/linkedServices/delete | Löscht verknüpfte Dienste im angegebenen Arbeitsbereich. |
> | Action | Microsoft.OperationalInsights/workspaces/linkedServices/read | Ruft verknüpfte Dienste im angegebenen Arbeitsbereich ab. |
> | Action | Microsoft.OperationalInsights/workspaces/linkedServices/write | Erstellt oder aktualisiert verknüpfte Dienste im angegebenen Arbeitsbereich. |
> | Action | Microsoft.OperationalInsights/workspaces/listKeys/action | Ruft die Listenschlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | Action | Microsoft.OperationalInsights/workspaces/listKeys/read | Ruft die Listenschlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | Action | Microsoft.OperationalInsights/workspaces/managementGroups/read | Ruft die Namen und Metadaten für über System Center Operations Manager verwaltete Gruppen ab, die mit diesem Arbeitsbereich verbunden sind. |
> | Action | Microsoft.OperationalInsights/workspaces/metricDefinitions/read | Ruft Metrikdefinitionen im Arbeitsbereich ab. |
> | Action | Microsoft.OperationalInsights/workspaces/notificationSettings/delete | Löscht die Benachrichtigungseinstellungen für den Arbeitsbereich des Benutzers. |
> | Action | Microsoft.OperationalInsights/workspaces/notificationSettings/read | Ruft die Benachrichtigungseinstellungen für den Arbeitsbereich des Benutzers ab. |
> | Action | Microsoft.OperationalInsights/workspaces/notificationSettings/write | Legt die Benachrichtigungseinstellungen für den Arbeitsbereich des Benutzers fest. |
> | Action | microsoft.operationalinsights/workspaces/operations/read | Ruft den Status eines OperationalInsights-Arbeitsbereichsvorgangs ab. |
> | Action | Microsoft.OperationalInsights/workspaces/purge/action | Löscht die angegebenen Daten aus dem Arbeitsbereich. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesAccountLogon/read | Liest Daten aus der Tabelle AADDomainServicesAccountLogon |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesAccountManagement/read | Liest Daten aus der Tabelle „AADDomainServicesAccountManagement“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesDirectoryServiceAccess/read | Liest Daten aus der Tabelle „AADDomainServicesDirectoryServiceAccess“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesLogonLogoff/read | Liest Daten aus der Tabelle AADDomainServicesLogonLogoff |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesPolicyChange/read | Liest Daten aus der Tabelle „AADDomainServicesPolicyChange“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesPrivilegeUse/read | Liest Daten aus der Tabelle „AADDomainServicesPrivilegeUse“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesSystemSecurity/read | Liest Daten aus der Tabelle „AADDomainServicesSystemSecurity“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der ADAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AddonAzureBackupAlerts/read | Liest Daten aus der Tabelle AddonAzureBackupAlerts |
> | Action | Microsoft.OperationalInsights/workspaces/query/AddonAzureBackupJobs/read | Liest Daten aus der Tabelle AddonAzureBackupJobs |
> | Action | Microsoft.OperationalInsights/workspaces/query/AddonAzureBackupPolicy/read | Liest Daten aus der Tabelle AddonAzureBackupPolicy |
> | Action | Microsoft.OperationalInsights/workspaces/query/AddonAzureBackupProtectedInstance/read | Liest Daten aus der Tabelle AddonAzureBackupProtectedInstance |
> | Action | Microsoft.OperationalInsights/workspaces/query/AddonAzureBackupStorage/read | Liest Daten aus der Tabelle AddonAzureBackupStorage |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADFActivityRun/read | Liest Daten aus der Tabelle „ADFActivityRun“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADFPipelineRun/read | Liest Daten aus der Tabelle „ADFPipelineRun“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADFTriggerRun/read | Liest Daten aus der Tabelle „ADFTriggerRun“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADReplicationResult/read | Hiermit lesen Sie Daten aus der ADReplicationResult-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADSecurityAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der ADSecurityAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AegDeliveryFailureLogs/read | Liest Daten aus der Tabelle „AegDeliveryFailureLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AegPublishFailureLogs/read | Liest Daten aus der Tabelle „AegPublishFailureLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/Alert/read | Hiermit lesen Sie Daten aus der Alert-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AlertHistory/read | Hiermit lesen Sie Daten aus der AlertHistory-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AmlComputeClusterEvent/read | Lesen von Daten aus der Tabelle „AmlComputeClusterEvent“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/AmlComputeClusterNodeEvent/read | Lesen von Daten aus der Tabelle „AmlComputeClusterNodeEvent“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/AmlComputeJobEvent/read | Lesen von Daten aus der Tabelle „AmlComputeJobEvent“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/ApiManagementGatewayLogs/read | Liest Daten aus der Tabelle ApiManagementGatewayLogs |
> | Action | Microsoft.OperationalInsights/workspaces/query/AppCenterError/read | Liest Daten aus der AppCenterError-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ApplicationInsights/read | Hiermit lesen Sie Daten aus der ApplicationInsights-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AppPlatformLogsforSpring/read | Lesen von Daten aus der Tabelle „AppPlatformLogsforSpring“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/AppPlatformSystemLogs/read | Liest Daten aus der Tabelle „AppPlatformSystemLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AppServiceAppLogs/read | Liest Daten aus der Tabelle „AppServiceAppLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AppServiceAuditLogs/read | Liest Daten aus der Tabelle „AppServiceAuditLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AppServiceConsoleLogs/read | Liest Daten aus der Tabelle „AppServiceConsoleLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AppServiceEnvironmentPlatformLogs/read | Lesen von Daten aus der Tabelle „AppServiceEnvironmentPlatformLogs“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/AppServiceHTTPLogs/read | Liest Daten aus der Tabelle „AppServiceHTTPLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AuditLogs/read | Dient zum Lesen von Daten aus der AuditLogs-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AutoscaleEvaluationsLog/read | Liest Daten aus der AutoscaleEvaluationsLog-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AutoscaleScaleActionsLog/read | Liest Daten aus der AutoscaleScaleActionsLog-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AWSCloudTrail/read | Liest Daten aus der AWSCloudTrail-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AzureActivity/read | Hiermit lesen Sie Daten aus der AzureActivity-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/AzureAssessmentRecommendation/read | Liest Daten aus der Tabelle „AzureAssessmentRecommendation“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/AzureMetrics/read | Hiermit lesen Sie Daten aus der AzureMetrics-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/BaiClusterEvent/read | Lesen von Daten aus der Tabelle „BaiClusterEvent“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/BaiClusterNodeEvent/read | Lesen von Daten aus der Tabelle „BaiClusterNodeEvent“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/BaiJobEvent/read | Lesen von Daten aus der Tabelle „BaiJobEvent“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/BlockchainApplicationLog/read | Liest Daten aus der Tabelle „BlockchainApplicationLog“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/BlockchainProxyLog/read | Liest Daten aus der Tabelle „BlockchainProxyLog“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/BoundPort/read | Hiermit lesen Sie Daten aus der BoundPort-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/CommonSecurityLog/read | Hiermit lesen Sie Daten aus der CommonSecurityLog-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ComputerGroup/read | Hiermit lesen Sie Daten aus der ComputerGroup-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ConfigurationChange/read | Hiermit lesen Sie Daten aus der ConfigurationChange-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ConfigurationData/read | Hiermit lesen Sie Daten aus der ConfigurationData-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerImageInventory/read | Hiermit lesen Sie Daten aus der ContainerImageInventory-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerInventory/read | Hiermit lesen Sie Daten aus der ContainerInventory-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerLog/read | Hiermit lesen Sie Daten aus der ContainerLog-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerNodeInventory/read | Liest Daten aus der ContainerNodeInventory-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerRegistryLoginEvents/read | Liest Daten aus der Tabelle ContainerRegistryLoginEvents |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerRegistryRepositoryEvents/read | Liest Daten aus der Tabelle ContainerRegistryRepositoryEvents |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerServiceLog/read | Hiermit lesen Sie Daten aus der ContainerServiceLog-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/CoreAzureBackup/read | Liest Daten aus der Tabelle CoreAzureBackup |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksAccounts/read | Liest Daten aus der Tabelle DatabricksAccounts |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksClusters/read | Liest Daten aus der Tabelle DatabricksClusters |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksDBFS/read | Liest Daten aus der Tabelle DatabricksDBFS |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksInstancePools/read | Lesen von Daten aus der Tabelle „DatabricksInstancePools“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksJobs/read | Liest Daten aus der Tabelle DatabricksJobs |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksNotebook/read | Liest Daten aus der Tabelle DatabricksNotebook |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksSecrets/read | Liest Daten aus der Tabelle DatabricksSecrets |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksSQLPermissions/read | Liest Daten aus der Tabelle DatabricksSQLPermissions |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksSSH/read | Liest Daten aus der Tabelle DatabricksSSH |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksTables/read | Liest Daten aus der Tabelle DatabricksTables |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksWorkspace/read | Liest Daten aus der Tabelle DatabricksWorkspace |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceAppCrash/read | Hiermit lesen Sie Daten aus der DeviceAppCrash-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceAppLaunch/read | Hiermit lesen Sie Daten aus der DeviceAppLaunch-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceCalendar/read | Hiermit lesen Sie Daten aus der DeviceCalendar-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceCleanup/read | Hiermit lesen Sie Daten aus der DeviceCleanup-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceConnectSession/read | Hiermit lesen Sie Daten aus der DeviceConnectSession-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceEtw/read | Hiermit lesen Sie Daten aus der DeviceEtw-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceHardwareHealth/read | Hiermit lesen Sie Daten aus der DeviceHardwareHealth-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceHealth/read | Hiermit lesen Sie Daten aus der DeviceHealth-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceHeartbeat/read | Hiermit lesen Sie Daten aus der DeviceHeartbeat-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeHeartbeat/read | Hiermit lesen Sie Daten aus der DeviceSkypeHeartbeat-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeSignIn/read | Hiermit lesen Sie Daten aus der DeviceSkypeSignIn-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceSleepState/read | Hiermit lesen Sie Daten aus der DeviceSleepState-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHAppFailure/read | Hiermit lesen Sie Daten aus der DHAppFailure-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHAppReliability/read | Hiermit lesen Sie Daten aus der DHAppReliability-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHCPActivity/read | Liest Daten aus der Tabelle „DHCPActivity“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHDriverReliability/read | Hiermit lesen Sie Daten aus der DHDriverReliability-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHLogonFailures/read | Hiermit lesen Sie Daten aus der DHLogonFailures-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHLogonMetrics/read | Hiermit lesen Sie Daten aus der DHLogonMetrics-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHOSCrashData/read | Hiermit lesen Sie Daten aus der DHOSCrashData-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHOSReliability/read | Hiermit lesen Sie Daten aus der DHOSReliability-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHWipAppLearning/read | Hiermit lesen Sie Daten aus der DHWipAppLearning-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DnsEvents/read | Hiermit lesen Sie Daten aus der DnsEvents-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/DnsInventory/read | Hiermit lesen Sie Daten aus der DnsInventory-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ETWEvent/read | Hiermit lesen Sie Daten aus der ETWEvent-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/Event/read | Hiermit lesen Sie Daten aus der Event-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ExchangeAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der ExchangeAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ExchangeOnlineAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der ExchangeOnlineAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/FailedIngestion/read | Lesen von Daten aus der Tabelle „FailedIngestion“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/FunctionAppLogs/read | Liest Daten aus der Tabelle „FunctionAppLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/Heartbeat/read | Hiermit lesen Sie Daten aus der Heartbeat-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/HuntingBookmark/read | Liest Daten aus der HuntingBookmark-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/IISAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der IISAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/InboundConnection/read | Hiermit lesen Sie Daten aus der InboundConnection-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/InsightsMetrics/read | Liest Daten aus der Tabelle InsightsMetrics |
> | Action | Microsoft.OperationalInsights/workspaces/query/IntuneAuditLogs/read | Liest Daten aus der Tabelle „IntuneAuditLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/IntuneDeviceComplianceOrg/read | Liest Daten aus der Tabelle „IntuneDeviceComplianceOrg“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/IntuneOperationalLogs/read | Liest Daten aus der Tabelle „IntuneOperationalLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubeEvents/read | Liest Daten aus der Tabelle „KubeEvents“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubeHealth/read | Lesen von Daten aus der Tabelle „KubeHealth“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubeMonAgentEvents/read | Liest Daten aus der Tabelle „KubeMonAgentEvents“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubeNodeInventory/read | Hiermit lesen Sie Daten aus der KubeNodeInventory-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubePodInventory/read | Hiermit lesen Sie Daten aus der KubePodInventory-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubeServices/read | Liest Daten aus der Tabelle „KubeServices“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/LinuxAuditLog/read | Hiermit lesen Sie Daten aus der LinuxAuditLog-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplication/read | Hiermit lesen Sie Daten aus der MAApplication-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealth/read | Hiermit lesen Sie Daten aus der MAApplicationHealth-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthAlternativeVersions/read | Liest Daten aus der Tabelle „MAApplicationHealthAlternativeVersions“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthIssues/read | Liest Daten aus der Tabelle „MAApplicationHealthIssues“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstance/read | Hiermit lesen Sie Daten aus der MAApplicationInstance-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstanceReadiness/read | Liest Daten aus der Tabelle „MAApplicationInstanceReadiness“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationReadiness/read | Hiermit lesen Sie Daten aus der MAApplicationReadiness-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADeploymentPlan/read | Hiermit lesen Sie Daten aus der MADeploymentPlan-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevice/read | Hiermit lesen Sie Daten aus der MADevice-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADeviceNotEnrolled/read | Liest Daten aus der MADeviceNotEnrolled-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADeviceNRT/read | Liest Daten aus der Tabelle MADeviceNRT |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealth/read | Liest Daten aus der Tabelle „MADevicePnPHealth“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthAlternativeVersions/read | Liest Daten aus der Tabelle „MADevicePnPHealthAlternativeVersions“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthIssues/read | Liest Daten aus der Tabelle „MADevicePnPHealthIssues“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADeviceReadiness/read | Hiermit lesen Sie Daten aus der MADeviceReadiness-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADriverInstanceReadiness/read | Liest Daten aus der Tabelle „MADriverInstanceReadiness“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADriverReadiness/read | Hiermit lesen Sie Daten aus der MADriverReadiness-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddin/read | Hiermit lesen Sie Daten aus der MAOfficeAddin-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinEntityHealth/read | Liest Daten aus der Tabelle „MAOfficeAddinEntityHealth“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealth/read | Hiermit lesen Sie Daten aus der MAOfficeAddinHealth-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthEventNRT/read | Liest Daten aus der Tabelle „MAOfficeAddinHealthEventNRT“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthIssues/read | Liest Daten aus der Tabelle „MAOfficeAddinHealthIssues“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstance/read | Hiermit lesen Sie Daten aus der MAOfficeAddinInstance-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstanceReadiness/read | Liest Daten aus der Tabelle „MAOfficeAddinInstanceReadiness“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinReadiness/read | Hiermit lesen Sie Daten aus der MAOfficeAddinReadiness-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeApp/read | Hiermit lesen Sie Daten aus der MAOfficeApp-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppCrashesNRT/read | Liest Daten aus der Tabelle „MAOfficeAppCrashesNRT“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppHealth/read | Hiermit lesen Sie Daten aus der MAOfficeAppHealth-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstance/read | Hiermit lesen Sie Daten aus der MAOfficeAppInstance-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstanceHealth/read | Liest Daten aus der Tabelle „MAOfficeAppInstanceHealth“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppReadiness/read | Hiermit lesen Sie Daten aus der MAOfficeAppReadiness-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppSessionsNRT/read | Liest Daten aus der Tabelle „MAOfficeAppSessionsNRT“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeBuildInfo/read | Hiermit lesen Sie Daten aus der MAOfficeBuildInfo-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessment/read | Hiermit lesen Sie Daten aus der MAOfficeCurrencyAssessment-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessmentDailyCounts/read | Hiermit lesen Sie Daten aus der MAOfficeCurrencyAssessmentDailyCounts-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatus/read | Hiermit lesen Sie Daten aus der MAOfficeDeploymentStatus-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatusNRT/read | Liest Daten aus der Tabelle MAOfficeDeploymentStatusNRT |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroErrorNRT/read | Liest Daten aus der Tabelle „MAOfficeMacroErrorNRT“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroGlobalHealth/read | Liest Daten aus der Tabelle MAOfficeMacroGlobalHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealth/read | Liest Daten aus der Tabelle „MAOfficeMacroHealth“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealthIssues/read | Liest Daten aus der Tabelle „MAOfficeMacroHealthIssues“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueInstanceReadiness/read | Liest Daten aus der Tabelle „MAOfficeMacroIssueInstanceReadiness“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueReadiness/read | Hiermit lesen Sie Daten aus der MAOfficeMacroIssueReadiness-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroSummary/read | Hiermit lesen Sie Daten aus der MAOfficeMacroSummary-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuite/read | Hiermit lesen Sie Daten aus der MAOfficeSuite-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuiteInstance/read | Hiermit lesen Sie Daten aus der MAOfficeSuiteInstance-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAProposedPilotDevices/read | Hiermit lesen Sie Daten aus der MAProposedPilotDevices-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsBuildInfo/read | Hiermit lesen Sie Daten aus der MAWindowsBuildInfo-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessment/read | Hiermit lesen Sie Daten aus der MAWindowsCurrencyAssessment-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessmentDailyCounts/read | Hiermit lesen Sie Daten aus der MAWindowsCurrencyAssessmentDailyCounts-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatus/read | Hiermit lesen Sie Daten aus der MAWindowsDeploymentStatus-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatusNRT/read | Liest Daten aus der Tabelle MAWindowsDeploymentStatusNRT |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsSysReqInstanceReadiness/read | Liest Daten aus der Tabelle „MAWindowsSysReqInstanceReadiness“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/McasShadowItReporting/read | Liest Daten aus der Tabelle McasShadowItReporting |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftAzureBastionAuditLogs/read | Liest Daten aus der Tabelle „MicrosoftAzureBastionAuditLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftDataShareReceivedSnapshotLog/read | Liest Daten aus der Tabelle „MicrosoftDataShareReceivedSnapshotLog“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftDataShareSentSnapshotLog/read | Liest Daten aus der Tabelle „MicrosoftDataShareSentSnapshotLog“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftDataShareShareLog/read | Lesen von Daten aus der Tabelle „MicrosoftDataShareShareLog“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftDynamicsTelemetryPerformanceLogs/read | Liest Daten aus der Tabelle „MicrosoftDynamicsTelemetryPerformanceLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftDynamicsTelemetrySystemMetricsLogs/read | Liest Daten aus der Tabelle „MicrosoftDynamicsTelemetrySystemMetricsLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftHealthcareApisAuditLogs/read | Lesen von Daten aus der Tabelle „MicrosoftHealthcareApisAuditLogs“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftInsightsAzureActivityLog/read | Liest Daten aus der Tabelle MicrosoftInsightsAzureActivityLog |
> | Action | Microsoft.OperationalInsights/workspaces/query/NetworkMonitoring/read | Hiermit lesen Sie Daten aus der NetworkMonitoring-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/OfficeActivity/read | Hiermit lesen Sie Daten aus der OfficeActivity-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/Operation/read | Hiermit lesen Sie Daten aus der Operation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/OutboundConnection/read | Hiermit lesen Sie Daten aus der OutboundConnection-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/Perf/read | Hiermit lesen Sie Daten aus der Perf-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ProtectionStatus/read | Hiermit lesen Sie Daten aus der ProtectionStatus-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/read | Dient zum Ausführen von Abfragen für die Daten im Arbeitsbereich. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ReservedCommonFields/read | Hiermit lesen Sie Daten aus der ReservedCommonFields-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SCCMAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der SCCMAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SCOMAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der SCOMAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityAlert/read | Hiermit lesen Sie Daten aus der SecurityAlert-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read | Hiermit lesen Sie Daten aus der SecurityBaseline-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityBaselineSummary/read | Hiermit lesen Sie Daten aus der SecurityBaselineSummary-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityDetection/read | Hiermit lesen Sie Daten aus der SecurityDetection-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityEvent/read | Hiermit lesen Sie Daten aus der SecurityEvent-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityIoTRawEvent/read | Liest Daten aus der SecurityIoTRawEvent-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityRecommendation/read | Liest Daten aus der SecurityRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ServiceFabricOperationalEvent/read | Hiermit lesen Sie Daten aus der ServiceFabricOperationalEvent-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableActorEvent/read | Hiermit lesen Sie Daten aus der ServiceFabricReliableActorEvent-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableServiceEvent/read | Hiermit lesen Sie Daten aus der ServiceFabricReliableServiceEvent-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SfBAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der SfBAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SfBOnlineAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der SfBOnlineAssessmentRecommendatio-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SharePointOnlineAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der SharePointOnlineAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SignalRServiceDiagnosticLogs/read | Liest Daten aus der Tabelle „SignalRServiceDiagnosticLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SigninLogs/read | Dient zum Lesen von Daten aus der SigninLogs-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SPAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der SPAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SQLAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der SQLAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SQLQueryPerformance/read | Hiermit lesen Sie Daten aus der SQLQueryPerformance-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SqlThreatProtectionLoginAudits/read | Hiermit lesen Sie Daten aus der Tabelle „SqlThreatProtectionLoginAudits“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SqlVulnerabilityAssessmentResult/read | Hiermit lesen Sie Daten aus der Tabelle „SqlVulnerabilityAssessmentResult“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/StorageBlobLogs/read | Liest Daten aus der Tabelle „StorageBlobLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/StorageFileLogs/read | Liest Daten aus der Tabelle „StorageFileLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/StorageQueueLogs/read | Liest Daten aus der Tabelle „StorageQueueLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/StorageTableLogs/read | Liest Daten aus der Tabelle „StorageTableLogs“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SucceededIngestion/read | Lesen von Daten aus der Tabelle „SucceededIngestion“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/Syslog/read | Hiermit lesen Sie Daten aus der Syslog-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/SysmonEvent/read | Hiermit lesen Sie Daten aus der SysmonEvent-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read | Hiermit lesen Sie Daten aus einem beliebigen benutzerdefinierten Protokoll. |
> | Action | Microsoft.OperationalInsights/workspaces/query/ThreatIntelligenceIndicator/read | Liest Daten aus der ThreatIntelligenceIndicator-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAApp/read | Hiermit lesen Sie Daten aus der UAApp-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAComputer/read | Hiermit lesen Sie Daten aus der UAComputer-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAComputerRank/read | Hiermit lesen Sie Daten aus der UAComputerRank-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UADriver/read | Hiermit lesen Sie Daten aus der UADriver-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UADriverProblemCodes/read | Hiermit lesen Sie Daten aus der UADriverProblemCodes-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAFeedback/read | Hiermit lesen Sie Daten aus der UAFeedback-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAHardwareSecurity/read | Hiermit lesen Sie Daten aus der UAHardwareSecurity-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAIESiteDiscovery/read | Hiermit lesen Sie Daten aus der UAIESiteDiscovery-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAOfficeAddIn/read | Hiermit lesen Sie Daten aus der UAOfficeAddIn-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAProposedActionPlan/read | Hiermit lesen Sie Daten aus der UAProposedActionPlan-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UASysReqIssue/read | Hiermit lesen Sie Daten aus der UASysReqIssue-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAUpgradedComputer/read | Hiermit lesen Sie Daten aus der UAUpgradedComputer-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/Update/read | Hiermit lesen Sie Daten aus der Update-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UpdateRunProgress/read | Hiermit lesen Sie Daten aus der UpdateRunProgress-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/UpdateSummary/read | Hiermit lesen Sie Daten aus der UpdateSummary-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/Usage/read | Hiermit lesen Sie Daten aus der Usage-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/VMBoundPort/read | Liest Daten aus der Tabelle „VMBoundPort“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/VMComputer/read | Liest Daten aus der Tabelle „VMComputer“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/VMConnection/read | Liest Daten aus der Tabelle „VMConnection“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/VMProcess/read | Liest Daten aus der Tabelle „VMProcess“. |
> | Action | Microsoft.OperationalInsights/workspaces/query/W3CIISLog/read | Hiermit lesen Sie Daten aus der W3CIISLog-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WaaSDeploymentStatus/read | Hiermit lesen Sie Daten aus der WaaSDeploymentStatus-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WaaSInsiderStatus/read | Hiermit lesen Sie Daten aus der WaaSInsiderStatus-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WaaSUpdateStatus/read | Hiermit lesen Sie Daten aus der WaaSUpdateStatus-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WDAVStatus/read | Hiermit lesen Sie Daten aus der WDAVStatus-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WDAVThreat/read | Hiermit lesen Sie Daten aus der WDAVThreat-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsClientAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der WindowsClientAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsEvent/read | Dient zum Lesen von Daten aus der WindowsEvent-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsFirewall/read | Hiermit lesen Sie Daten aus der WindowsFirewall-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsServerAssessmentRecommendation/read | Hiermit lesen Sie Daten aus der WindowsServerAssessmentRecommendation-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WireData/read | Hiermit lesen Sie Daten aus der WireData-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WorkloadMonitoringPerf/read | Lesen von Daten aus der Tabelle „WorkloadMonitoringPerf“ |
> | Action | Microsoft.OperationalInsights/workspaces/query/WUDOAggregatedStatus/read | Hiermit lesen Sie Daten aus der WUDOAggregatedStatus-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WUDOStatus/read | Hiermit lesen Sie Daten aus der WUDOStatus-Tabelle. |
> | Action | Microsoft.OperationalInsights/workspaces/read | Ruft einen vorhandenen Arbeitsbereich ab. |
> | Action | Microsoft.OperationalInsights/workspaces/regeneratesharedkey/action | Generiert den angegebenen gemeinsam verwendeten Schlüssel des Arbeitsbereichs neu. |
> | Action | microsoft.operationalinsights/workspaces/rules/read | Ruft alle Warnungsregeln ab. |
> | Action | Microsoft.OperationalInsights/workspaces/savedSearches/delete | Löscht eine gespeicherte Suchabfrage. |
> | Action | Microsoft.OperationalInsights/workspaces/savedSearches/read | Ruft eine gespeicherte Suchabfrage ab. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/results/read | Ruft die Ergebnisse gespeicherter Suchvorgänge ab. Als veraltet markiert |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/delete | Löscht geplante Suchaktionen. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/read | Ruft geplante Suchaktionen ab. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/write | Erstellt oder aktualisiert geplante Suchaktionen. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/delete | Löscht geplante Suchvorgänge. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/read | Ruft geplante Suchvorgänge ab. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/write | Erstellt oder aktualisiert geplante Suchvorgänge. |
> | Action | Microsoft.OperationalInsights/workspaces/savedSearches/write | Erstellt eine gespeicherte Suchabfrage. |
> | Action | Microsoft.OperationalInsights/workspaces/schema/read | Ruft das Suchschema für den Arbeitsbereich ab.  Ein Suchschema umfasst die verfügbar gemachten Felder und die dazugehörigen Typen. |
> | Action | Microsoft.OperationalInsights/workspaces/search/action | Führt eine Suchabfrage aus. |
> | Action | microsoft.operationalinsights/workspaces/search/read | Ruft Suchergebnisse ab. Veraltet. |
> | Action | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | Action | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | Action | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/delete | Löscht eine Speicherkonfiguration. Daraufhin werden von Microsoft Operational Insights keine Daten mehr aus dem Speicherkonto gelesen. |
> | Action | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/read | Ruft eine Speicherkonfiguration ab. |
> | Action | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/write | Erstellt eine neue Speicherkonfiguration. Diese Konfigurationen werden dazu verwendet, Daten von einem Speicherort in einem vorhandenen Speicherkonto abzurufen. |
> | Action | Microsoft.OperationalInsights/workspaces/upgradetranslationfailures/read | Ruft das Übersetzungsfehlerprotokoll für das Upgrade der Suche für den Arbeitsbereich ab. |
> | Action | Microsoft.OperationalInsights/workspaces/usages/read | Ruft Nutzungsdaten für einen Arbeitsbereich ab, einschließlich der durch den Arbeitsbereich gelesenen Datenmenge. |
> | Action | Microsoft.OperationalInsights/workspaces/write | Erstellt einen neuen Arbeitsbereich oder stellt eine Verknüpfung mit einem vorhandenen Arbeitsbereich her, indem die Kunden-ID für den vorhandenen Arbeitsbereich bereitgestellt wird. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.OperationsManagement/managementAssociations/delete | Löscht eine vorhandene Verwaltungszuordnung. |
> | Action | Microsoft.OperationsManagement/managementAssociations/read | Ruft eine vorhandene Verwaltungszuordnung ab. |
> | Action | Microsoft.OperationsManagement/managementAssociations/write | Erstellt eine neue Verwaltungszuordnung. |
> | Action | Microsoft.OperationsManagement/managementConfigurations/delete | Löscht eine vorhandene Verwaltungskonfiguration. |
> | Action | Microsoft.OperationsManagement/managementConfigurations/read | Ruft eine vorhandene Verwaltungskonfiguration ab. |
> | Action | Microsoft.OperationsManagement/managementConfigurations/write | Erstellt eine vorhandene Verwaltungskonfiguration. |
> | Action | Microsoft.OperationsManagement/register/action | Dient zum Registrieren eines Abonnements bei einem Ressourcenanbieter. |
> | Action | Microsoft.OperationsManagement/solutions/delete | Dient zum Löschen vorhandener OMS-Lösungen. |
> | Action | Microsoft.OperationsManagement/solutions/read | Dient zum Abrufen vorhandener OMS-Lösungen. |
> | Action | Microsoft.OperationsManagement/solutions/write | Dient zum Erstellen neuer OMS-Lösungen. |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.PolicyInsights/asyncOperationResults/read | Hiermit wird das Ergebnis des asynchronen Vorgangs abgerufen. |
> | DataAction | Microsoft.PolicyInsights/checkDataPolicyCompliance/action | Überprüft den Compliancestatus einer angegebenen Komponente anhand von Datenrichtlinien. |
> | Action | Microsoft.PolicyInsights/operations/read | Ruft unterstützte Vorgänge im Microsoft.PolicyInsights-Namespace ab |
> | DataAction | Microsoft.PolicyInsights/policyEvents/logDataEvents/action | Protokollieren der Ressourcenkomponenten-Richtlinienereignisse. |
> | Action | Microsoft.PolicyInsights/policyEvents/queryResults/action | Fragt Informationen zu Richtlinienereignissen ab. |
> | Action | Microsoft.PolicyInsights/policyEvents/queryResults/read | Fragt Informationen zu Richtlinienereignissen ab. |
> | Action | Microsoft.PolicyInsights/policyMetadata/read | Ruft Ressourcen für Richtlinienmetadaten ab. |
> | Action | Microsoft.PolicyInsights/policyStates/queryResults/action | Fragt Informationen zu Richtlinienzuständen ab. |
> | Action | Microsoft.PolicyInsights/policyStates/queryResults/read | Fragt Informationen zu Richtlinienzuständen ab. |
> | Action | Microsoft.PolicyInsights/policyStates/summarize/action | Fragt zusammenfassende Informationen zu den letzten Richtlinienzuständen ab. |
> | Action | Microsoft.PolicyInsights/policyStates/summarize/read | Fragt zusammenfassende Informationen zu den letzten Richtlinienzuständen ab. |
> | Action | Microsoft.PolicyInsights/policyStates/triggerEvaluation/action | Hiermit wird eine neue Konformitätsauswertung für den ausgewählten Bereich ausgelöst. |
> | Action | Microsoft.PolicyInsights/policyTrackedResources/queryResults/read | Hiermit werden Informationen zu Ressourcen abgefragt, die gemäß DeployIfNotExists-Richtlinien erforderlich sind. |
> | Action | Microsoft.PolicyInsights/register/action | Registriert den Ressourcenanbieter für Microsoft Policy Insights und ermöglicht entsprechende Aktionen |
> | Action | Microsoft.PolicyInsights/remediations/cancel/action | Bricht laufende Microsoft Policy-Wartungen ab |
> | Action | Microsoft.PolicyInsights/remediations/delete | Hiermit werden Richtlinienwartungen gelöscht. |
> | Action | Microsoft.PolicyInsights/remediations/listDeployments/read | Listet die für eine Richtlinienwartung erforderlichen Bereitstellungen auf. |
> | Action | Microsoft.PolicyInsights/remediations/read | Hiermit werden Richtlinienwartungen abgerufen. |
> | Action | Microsoft.PolicyInsights/remediations/write | Erstellt oder aktualisiert Microsoft Policy-Wartungen |
> | Action | Microsoft.PolicyInsights/unregister/action | Hebt die Registrierung des Ressourcenanbieters für Microsoft Policy Insights auf |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Portal/consoles/delete | Entfernt die Cloud Shell-Instanz. |
> | Action | Microsoft.Portal/consoles/read | Hiermit wird die Cloud Shell-Instanz gelesen. |
> | Action | Microsoft.Portal/consoles/write | Hiermit erstellen oder aktualisieren Sie eine Cloud Shell-Instanz. |
> | Action | Microsoft.Portal/dashboards/delete | Entfernt das Dashboard aus dem Abonnement. |
> | Action | Microsoft.Portal/dashboards/read | Liest die Dashboards für das Abonnement. |
> | Action | Microsoft.Portal/dashboards/write | Fügt ein Dashboard zu einem Abonnement hinzu oder ändert es. |
> | Action | Microsoft.Portal/register/action | Führt die Registrierung bei einem Portal durch. |
> | Action | Microsoft.Portal/usersettings/delete | Entfernt die Cloud Shell-Benutzereinstellungen. |
> | Action | Microsoft.Portal/usersettings/read | Liest die Cloud Shell-Benutzereinstellungen. |
> | Action | Microsoft.Portal/usersettings/write | Dient zum Erstellen oder Aktualisieren einer Cloud Shell-Benutzereinstellung. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.PowerBIDedicated/capacities/delete | Löscht die dedizierte Power BI-Kapazität. |
> | Action | Microsoft.PowerBIDedicated/capacities/read | Ruft die Informationen der angegebenen dedizierten Power BI-Kapazität ab. |
> | Action | Microsoft.PowerBIDedicated/capacities/resume/action | Setzt die Kapazität fort. |
> | Action | Microsoft.PowerBIDedicated/capacities/skus/read | Ruft die verfügbaren SKU-Informationen für die Kapazität ab. |
> | Action | Microsoft.PowerBIDedicated/capacities/suspend/action | Hält die Kapazität an. |
> | Action | Microsoft.PowerBIDedicated/capacities/write | Erstellt oder aktualisiert die angegebene dedizierte Power BI-Kapazität. |
> | Action | Microsoft.PowerBIDedicated/locations/checkNameAvailability/action | Überprüft, ob der Name für die angegebene Power BI-Kapazität gültig ist und nicht verwendet wird. |
> | Action | Microsoft.PowerBIDedicated/locations/operationresults/read | Ruft die Informationen des angegebenen Vorgangsergebnisses ab. |
> | Action | Microsoft.PowerBIDedicated/locations/operationstatuses/read | Ruft die Informationen des angegebenen Vorgangsstatus ab. |
> | Action | Microsoft.PowerBIDedicated/operations/read | Ruft die Informationen von Vorgängen ab. |
> | Action | Microsoft.PowerBIDedicated/register/action | Registriert den dedizierten Power BI-Ressourcenanbieter. |
> | Action | Microsoft.PowerBIDedicated/skus/read | Ruft die Informationen der SKUs ab. |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Action | Microsoft.RecoveryServices/locations/allocateStamp/action | „AllocateStamp“ ist ein interner Vorgang des Diensts. |
> | Action | Microsoft.RecoveryServices/Locations/backupPreValidateProtection/action |  |
> | Action | Microsoft.RecoveryServices/Locations/backupProtectedItem/write | Dient zum Erstellen eines geschützten Elements für die Sicherung. |
> | Action | Microsoft.RecoveryServices/Locations/backupProtectedItems/read | Gibt die Liste mit allen geschützten Elementen zurück. |
> | Action | Microsoft.RecoveryServices/Locations/backupStatus/action | Überprüft den Sicherungsstatus für Recovery Services-Tresore. |
> | Action | Microsoft.RecoveryServices/Locations/backupValidateFeatures/action | Überprüft Features. |
> | Action | Microsoft.RecoveryServices/locations/checkNameAvailability/action | Überprüft die Verfügbarkeit des Ressourcennamens als API, um sicherzustellen, dass der Ressourcenname verfügbar ist. |
> | Action | Microsoft.RecoveryServices/locations/operationStatus/read | Ruft den Vorgangsstatus eines angegebenen Vorgangs ab. |
> | Action | Microsoft.RecoveryServices/operations/read | Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück. |
> | Action | Microsoft.RecoveryServices/register/action | Registriert das Abonnement für den angegebenen Ressourcenanbieter. |
> | Action | Microsoft.RecoveryServices/Vaults/backupconfig/read | Gibt die Konfiguration für Recovery Services-Tresore zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupconfig/write | Aktualisiert die Konfiguration für Recovery Services-Tresore. |
> | Action | Microsoft.RecoveryServices/Vaults/backupEncryptionConfigs/read | Ruft die Verschlüsselungskonfiguration für Sicherungsressourcen ab. |
> | Action | Microsoft.RecoveryServices/Vaults/backupEncryptionConfigs/write | Aktualisiert die Verschlüsselungskonfiguration für Sicherungsressourcen. |
> | Action | Microsoft.RecoveryServices/Vaults/backupEngines/read | Gibt alle Sicherungsverwaltungsserver zurück, die beim Tresor registriert sind. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/delete | Löscht einen beabsichtigten Sicherungsschutz. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Ruft einen beabsichtigten Sicherungsschutz ab. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Erstellt einen beabsichtigten Sicherungsschutz. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Gibt den Status des Vorgangs zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/operationsStatus/read | Gibt den Status des Vorgangs zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Ruft alle schützbaren Container ab. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/delete | Löscht den registrierten Container. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Führt die Abfrage für Workloads innerhalb eines Containers durch. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Ruft alle Elemente in einem Container ab. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für den Schutzcontainer ausgeführt wurde. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationsStatus/read | Ruft den Status eines Vorgangs ab, der für den Schutzcontainer ausgeführt wurde. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Führt eine Sicherung für geschützte Elemente aus. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/delete | Löscht geschützte Elemente. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für geschützte Elemente ausgeführt wurde. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Dient zum Bereitstellen der sofortigen Elementwiederherstellung für geschützte Elemente. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Dient zum Abrufen von Wiederherstellungspunkten für geschützte Elemente. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Dient zum Wiederherstellen von Wiederherstellungspunkten für geschützte Elemente. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Dient zum Widerrufen der sofortigen Elementwiederherstellung für geschützte Elemente. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Dient zum Erstellen eines geschützten Elements für die Sicherung. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Gibt alle registrierten Container zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Erstellt einen registrierten Container. |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Aktualisiert die Containerliste. |
> | Action | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Dient zum Abbrechen des Auftrags. |
> | Action | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Gibt das Ergebnis von Auftragsvorgängen zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupJobs/operationsStatus/read | Gibt den Status des Auftragsvorgangs zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupJobs/read | Gibt alle Auftragsobjekte zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dient zum Exportieren von Aufträgen. |
> | Action | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Gibt das Ergebnis eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupOperations/read | Gibt den Status eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupPolicies/delete | Dient zum Löschen einer Schutzrichtlinie. |
> | Action | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Dient zum Abrufen der Ergebnisse von Richtlinienvorgängen. |
> | Action | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Dient zum Abrufen des Status von Richtlinienvorgängen. |
> | Action | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Erstellt Schutzrichtlinien. |
> | Action | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Gibt die Liste mit allen schützbaren Elementen zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Gibt die Liste mit allen geschützten Elementen zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Gibt alle zum Abonnement gehörenden Container zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Listet den gesamten beabsichtigten Sicherungsschutz auf. |
> | Action | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/action | Gibt Informationen zur Sicherheits-PIN für Recovery Services-Tresore zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Gibt die Speicherkonfiguration für Recovery Services-Tresore zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupstorageconfig/write | Aktualisiert Speicherkonfiguration für Recovery Services-Tresore. |
> | Action | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Überprüft Vorgang für geschütztes Element. |
> | Action | Microsoft.RecoveryServices/Vaults/certificates/write | Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen. |
> | Action | Microsoft.RecoveryServices/Vaults/delete | Der Vorgang „Tresor löschen“ löscht die angegebene Azure-Ressource vom Typ „Tresor“. |
> | Action | Microsoft.RecoveryServices/Vaults/extendedInformation/delete | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Action | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Action | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Löst die Warnung auf. |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/read | Ruft die Konfiguration von Recovery Services-Tresorbenachrichtigungen ab. |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/write | Konfiguriert E-Mail-Benachrichtigungen für den Recovery Services-Tresor. |
> | Action | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/delete | Der Vorgang „Registrierung des Containers aufheben“ kann zum Aufheben der Registrierung eines Containers verwendet werden. |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Der Vorgang „Dienstcontainer registrieren“ kann zum Registrieren eines Containers beim Wiederherstellungsdienst verwendet werden. |
> | Action | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Dient zum Lesen beliebiger Warnungseinstellungen |
> | Action | Microsoft.RecoveryServices/vaults/replicationAlertSettings/write | Erstellt oder aktualisiert Warnungseinstellungen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationEvents/read | Dient zum Lesen beliebiger Ereignisse |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Prüft die Konsistenz des Fabrics. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/delete | Löscht Fabrics. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/deployProcessServerImage/action | Dient zum Bereitstellen von Prozessserverimages. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/migratetoaad/action | Migriert Fabric zu AAD. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für Fabrics von Ressourcen nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Dient zum Lesen beliebiger Fabrics |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Dient zum Neuzuordnen des Gateways. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/remove/action | Dient zum Entfernen von Fabrics. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Verlängert das Zertifikat für Fabrics. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationLogicalNetworks/read | Liest logische Netzwerke. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Dient zum Lesen beliebiger Netzwerke |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete | Löscht Netzwerkzuordnungen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Dient zum Lesen beliebiger Netzwerkzuordnungen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write | Erstellt oder aktualisiert Netzwerkzuordnungen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/action | Dient zum Ermitteln des schützbaren Elements. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für Schutzcontainer der Ressource nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Dient zum Lesen beliebiger Schutzcontainer |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/remove/action | Dient zum Entfernen von Schutzcontainern. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/delete | Löscht die Migrationselemente. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrate/action | Migriert ein Element. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrationRecoveryPoints/read | Liest die Wiederherstellungspunkte für die Migration. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für Migrationselemente der Ressource nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/read | Liest die Migrationselemente. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrate/action | Testet die Migration. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrateCleanup/action | Testet die Bereinigung der Migration. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/write | Dient zum Erstellen oder Aktualisieren von Migrationselementen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Dient zum Lesen beliebiger schützbarer Elemente |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/addDisks/action | Fügt Datenträger hinzu |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Dient zum Anwenden des Wiederherstellungspunkts. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/delete | Löscht geschützte Elemente. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failovercommit |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für geschützte Elemente der Ressource nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplantes Failover |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Dient zum Lesen beliebiger geschützter Elemente |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Dient zum Lesen beliebiger Wiederherstellungspunkte für die Replikation |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/remove/action | Dient zum Entfernen geschützter Elemente. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/removeDisks/action | Entfernt Datenträger |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Dient zum Reparieren der Replikation. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Dient zum erneuten Schützen geschützter Elemente. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/ResolveHealthErrors/action |  |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/submitFeedback/action | Sendet Feedback. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/targetComputeSizes/read | Liest Zielcomputegrößen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Testfailover |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Testfailoverbereinigung |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Dient zum Aktualisieren von Mobility Service. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/write | Erstellt oder aktualisiert geschützte Elemente. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/delete | Löscht Schutzcontainerzuordnungen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für Schutzcontainerzuordnungen der Ressource nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Dient zum Lesen beliebiger Schutzcontainerzuordnungen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/remove/action | Dient zum Entfernen von Schutzcontainerzuordnungen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/write | Erstellt oder aktualisiert Schutzcontainerzuordnungen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Dient zum Wechseln von Schutzcontainern. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/write | Erstellt oder aktualisiert Schutzcontainer. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete | Löscht Recovery Services-Anbieter. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für Recovery Services-Anbieter der Ressource nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Dient zum Lesen beliebiger Recovery Services-Anbieter |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Dient zum Aktualisieren des Anbieters. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action | Dient zum Entfernen von Recovery Services-Anbietern. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/write | Erstellt oder aktualisiert Recovery Services-Anbieter. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Dient zum Lesen beliebiger Speicherklassifizierungen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/delete | Löscht Speicherklassifizierungszuordnungen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für Speicherklassifizierungszuordnungen der Ressource nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Dient zum Lesen beliebiger Speicherklassifizierungszuordnungen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/write | Erstellt oder aktualisiert Speicherklassifizierungszuordnungen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/delete | Löscht vCenters. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für vCenters der Ressource nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Dienst zum Lesen beliebiger vCenter |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/write | Erstellt oder aktualisiert vCenters. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/write | Erstellt oder aktualisiert Fabrics. |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/cancel/action | Dient zum Abbrechen von Aufträgen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für Aufträge der Ressource nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/read | Dient zum Lesen beliebiger Aufträge |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/restart/action | Dient zum Neustarten von Aufträgen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/resume/action | Dient zum Fortsetzen von Aufträgen. |
> | Action | Microsoft.RecoveryServices/vaults/replicationMigrationItems/read | Liest die Migrationselemente. |
> | Action | Microsoft.RecoveryServices/vaults/replicationNetworkMappings/read | Dient zum Lesen beliebiger Netzwerkzuordnungen |
> | Action | Microsoft.RecoveryServices/vaults/replicationNetworks/read | Dient zum Lesen beliebiger Netzwerke |
> | Action | Microsoft.RecoveryServices/vaults/replicationOperationStatus/read | Liest alle Status des Tresorreplikationsvorgangs |
> | Action | Microsoft.RecoveryServices/vaults/replicationPolicies/delete | Löscht Richtlinien. |
> | Action | Microsoft.RecoveryServices/vaults/replicationPolicies/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für Richtlinien der Ressource nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Dient zum Lesen beliebiger Richtlinien |
> | Action | Microsoft.RecoveryServices/vaults/replicationPolicies/write | Erstellt oder aktualisiert Richtlinien. |
> | Action | Microsoft.RecoveryServices/vaults/replicationProtectedItems/read | Dient zum Lesen beliebiger geschützter Elemente |
> | Action | Microsoft.RecoveryServices/vaults/replicationProtectionContainerMappings/read | Dient zum Lesen beliebiger Schutzcontainerzuordnungen |
> | Action | Microsoft.RecoveryServices/vaults/replicationProtectionContainers/read | Dient zum Lesen beliebiger Schutzcontainer |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/delete | Löscht Wiederherstellungspläne. |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Wiederherstellungsplan für Failovercommit |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für Wiederherstellungspläne der Ressource nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Wiederherstellungsplan für geplantes Failover |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Dient zum Lesen beliebiger Wiederherstellungspläne |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Wiederherstellungsplan für erneutes Schützen |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Wiederherstellungsplan für Testfailover |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Wiederherstellungsplan für Testfailoverbereinigung |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Wiederherstellungsplan für Failover |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/write | Erstellt oder aktualisiert Wiederherstellungspläne. |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryServicesProviders/read | Dient zum Lesen beliebiger Recovery Services-Anbieter |
> | Action | Microsoft.RecoveryServices/vaults/replicationStorageClassificationMappings/read | Dient zum Lesen beliebiger Speicherklassifizierungszuordnungen |
> | Action | Microsoft.RecoveryServices/vaults/replicationStorageClassifications/read | Dient zum Lesen beliebiger Speicherklassifizierungen |
> | Action | Microsoft.RecoveryServices/vaults/replicationSupportedOperatingSystems/read | Liest alles.  |
> | Action | Microsoft.RecoveryServices/vaults/replicationUsages/read | Liest Verwendungen der Tresorreplikation. |
> | Action | Microsoft.RecoveryServices/vaults/replicationVaultHealth/operationresults/read | Verfolgt die Ergebnisse eines asynchronen Vorgangs für die Integrität der Tresorreplikation der Ressource nach |
> | Action | Microsoft.RecoveryServices/vaults/replicationVaultHealth/read | Liest die Integrität der Tresorreplikation. |
> | Action | Microsoft.RecoveryServices/vaults/replicationVaultHealth/refresh/action | Aktualisiert die Tresorintegrität. |
> | Action | Microsoft.RecoveryServices/vaults/replicationVaultSettings/read | Liest alles.  |
> | Action | Microsoft.RecoveryServices/vaults/replicationVaultSettings/write | Erstellt oder aktualisiert  |
> | Action | Microsoft.RecoveryServices/vaults/replicationvCenters/read | Dienst zum Lesen beliebiger vCenter |
> | Action | Microsoft.RecoveryServices/vaults/usages/read | Liest Tresorverwendungen. |
> | Action | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Action | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
> | Action | Microsoft.RecoveryServices/Vaults/write | Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“. |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Relay/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. |
> | Action | Microsoft.Relay/checkNamespaceAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. Diese API ist veraltet. Verwenden Sie stattdessen CheckNameAvailability. |
> | Action | Microsoft.Relay/namespaces/authorizationRules/action | Dient zum Aktualisieren von Namespace-Autorisierungsregeln. Diese API ist veraltet. Verwenden Sie stattdessen einen PUT-Aufruf, um die Namespace-Autorisierungsregel zu aktualisieren. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Action | Microsoft.Relay/namespaces/authorizationRules/delete | Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden.  |
> | Action | Microsoft.Relay/namespaces/authorizationRules/listkeys/action | Dient zum Abrufen der Verbindungszeichenfolge für den Namespace. |
> | Action | Microsoft.Relay/namespaces/authorizationRules/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln. |
> | Action | Microsoft.Relay/namespaces/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Action | Microsoft.Relay/namespaces/authorizationRules/write | Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel. |
> | Action | Microsoft.Relay/namespaces/Delete | Dient zum Löschen von Namespaceressourcen. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Ruft die Schlüssel der Autorisierungsregeln für den primären Namespace für die Notfallwiederherstellung. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/read | Ruft die Autorisierungsregeln des primären Namespace für die Notfallwiederherstellung ab. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/breakPairing/action | Hiermit wird die Notfallwiederherstellung deaktiviert und die Replikation von Änderungen vom primären Namespace zu sekundären Namespaces beendet. |
> | Action | Microsoft.Relay/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespacealias unter dem angegebenen Abonnement. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/delete | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration gelöscht. Dieser Vorgang kann nur über den primären Namespace aufgerufen werden. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/failover/action | Hiermit wird ein Failover bei der georedundanten Notfallwiederherstellung ausgelöst und der Namespacealias neu konfiguriert, um auf den sekundären Namespace zu zeigen. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/read | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration abgerufen. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/write | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration erstellt oder aktualisiert. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/action | Vorgang zum Aktualisieren von HybridConnection. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/delete | Vorgang zum Löschen von HybridConnection-Autorisierungsregeln. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/listkeys/action | Dient zum Abrufen der HybridConnection-Verbindungszeichenfolge. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/read |  Ruft die Liste der HybridConnection-Autorisierungsregeln ab. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/regeneratekeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/write | Dient zum Erstellen von HybridConnection-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/Delete | Vorgang zum Löschen von HybridConnection-Ressourcen. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/read | Dient zum Abrufen einer Liste mit HybridConnection-Ressourcenbeschreibungen. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/write | Dient zum Erstellen oder Aktualisieren von HybridConnection-Eigenschaften. |
> | Action | Microsoft.Relay/namespaces/messagingPlan/read | Hiermit wird der Messagingplan für einen Namespace abgerufen.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Action | Microsoft.Relay/namespaces/messagingPlan/write | Hiermit wird der Messagingplan für einen Namespace aktualisiert.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Action | Microsoft.Relay/namespaces/operationresults/read | Hiermit wird der Status des Namespacevorgangs abgerufen. |
> | Action | Microsoft.Relay/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen. |
> | Action | Microsoft.Relay/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen. |
> | Action | Microsoft.Relay/namespaces/providers/Microsoft.Insights/logDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespaceprotokolle. |
> | Action | Microsoft.Relay/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken. |
> | Action | Microsoft.Relay/namespaces/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen. |
> | Action | Microsoft.Relay/namespaces/removeAcsNamepsace/action | Entfernt den ACS-Namespace. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/action | Vorgang zum Aktualisieren von WcfRelay. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/delete | Vorgang zum Löschen von WcfRelay-Autorisierungsregeln. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/listkeys/action | Dient zum Abrufen der WcfRelay-Verbindungszeichenfolge. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/read |  Dient zum Abrufen der WcfRelay-Autorisierungsregeln. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/regeneratekeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/write | Dient zum Erstellen von WcfRelay-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/Delete | Vorgang zum Löschen von WcfRelay-Ressourcen. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/read | Dient zum Abrufen einer Liste mit WcfRelay-Ressourcenbeschreibungen. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/write | Dient zum Erstellen oder Aktualisieren von WcfRelay-Eigenschaften. |
> | Action | Microsoft.Relay/namespaces/write | Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden. |
> | Action | Microsoft.Relay/operations/read | Dient zum Abrufen von Vorgängen. |
> | Action | Microsoft.Relay/register/action | Registriert das Abonnement für den Relayressourcenanbieter und ermöglicht die Erstellung von Relayressourcen. |
> | Action | Microsoft.Relay/unregister/action | Registriert das Abonnement für den Relayressourcenanbieter und ermöglicht die Erstellung von Relayressourcen. |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ResourceHealth/AvailabilityStatuses/current/read | Ruft den Verfügbarkeitsstatus für die angegebene Ressource ab. |
> | Action | Microsoft.ResourceHealth/AvailabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Action | Microsoft.ResourceHealth/events/read | Ruft Service Health-Ereignisse für ein angegebenes Abonnement ab. |
> | Action | Microsoft.Resourcehealth/healthevent/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Action | Microsoft.Resourcehealth/healthevent/Activated/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Action | Microsoft.Resourcehealth/healthevent/InProgress/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Action | Microsoft.Resourcehealth/healthevent/Pending/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Action | Microsoft.Resourcehealth/healthevent/Resolved/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Action | Microsoft.Resourcehealth/healthevent/Updated/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Action | Microsoft.ResourceHealth/impactedResources/read | Ruft betroffene Ressourcen für das angegebene Abonnement ab. |
> | Action | Microsoft.ResourceHealth/metadata/read | Ruft Metadaten ab |
> | Action | Microsoft.ResourceHealth/Notifications/read | Empfängt Azure Resource Manager-Benachrichtigungen. |
> | Action | Microsoft.ResourceHealth/Operations/read | Ruft die verfügbaren Vorgänge für Microsoft ResourceHealth ab. |
> | Action | Microsoft.ResourceHealth/register/action | Hiermit wird das Abonnement für Microsoft ResourceHealth registriert. |
> | Action | Microsoft.ResourceHealth/unregister/action | Hebt die Registrierung des Abonnements für Microsoft ResourceHealth auf. |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Resources/calculateTemplateHash/action | Berechnen des Hashs der bereitgestellten Vorlage |
> | Action | Microsoft.Resources/checkPolicyCompliance/read | Überprüft den Konformitätsstatus einer angegebenen Ressource anhand von Ressourcenrichtlinien. |
> | Action | Microsoft.Resources/checkResourceName/action | Dient zum Überprüfen der Gültigkeit des Ressourcennamens. |
> | Action | Microsoft.Resources/deployments/cancel/action | Bricht eine Bereitstellung ab. |
> | Action | Microsoft.Resources/deployments/delete | Löscht eine Bereitstellung. |
> | Action | Microsoft.Resources/deployments/exportTemplate/action | Exportieren der Vorlage für eine Bereitstellung |
> | Action | Microsoft.Resources/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | Action | Microsoft.Resources/deployments/operationstatuses/read | Ruft den Status von Bereitstellungsvorgängen ab oder listet ihn auf. |
> | Action | Microsoft.Resources/deployments/read | Ruft Bereitstellungen ab oder listet sie auf. |
> | Action | Microsoft.Resources/deployments/validate/action | Überprüft eine Bereitstellung. |
> | Action | Microsoft.Resources/deployments/whatIf/action | Sagt Änderungen an der Vorlagenbereitstellung voraus |
> | Action | Microsoft.Resources/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | Action | Microsoft.Resources/deploymentScripts/delete | Löscht ein Bereitstellungsskript. |
> | Action | Microsoft.Resources/deploymentScripts/logs/read | Ruft Bereitstellungsskriptprotokolle ab oder listet diese auf. |
> | Action | Microsoft.Resources/deploymentScripts/read | Ruft Bereitstellungsskripts ab oder listet diese auf. |
> | Action | Microsoft.Resources/deploymentScripts/write | Erstellt oder aktualisiert ein Bereitstellungsskript. |
> | Action | Microsoft.Resources/links/delete | Löscht einen Ressourcenlink. |
> | Action | Microsoft.Resources/links/read | Ruft Ressourcenlinks ab oder listet sie auf. |
> | Action | Microsoft.Resources/links/write | Erstellt oder aktualisiert einen Ressourcenlink. |
> | Action | Microsoft.Resources/marketplace/purchase/action | Führt zum Erwerb einer Ressource im Marketplace. |
> | Action | Microsoft.Resources/providers/read | Dient zum Abrufen der Anbieterliste. |
> | Action | Microsoft.Resources/resources/read | Dient zum Abrufen einer gefilterten Ressourcenliste. |
> | Action | Microsoft.Resources/subscriptions/locations/read | Ruft die Liste mit den unterstützten Standorten ab. |
> | Action | Microsoft.Resources/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | Action | Microsoft.Resources/subscriptions/providers/read | Ruft Ressourcenanbieter ab oder listet sie auf. |
> | Action | Microsoft.Resources/subscriptions/read | Ruft die Abonnementliste ab. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/delete | Löscht eine Ressourcengruppe und alle dazugehörigen Ressourcen. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read | Ruft den Status von Bereitstellungsvorgängen ab oder listet ihn auf. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/read | Ruft Bereitstellungen ab oder listet sie auf. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/moveResources/action | Verschiebt Ressourcen aus einer Ressourcengruppe in eine andere. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/resources/read | Ruft die Ressourcen für die Ressourcengruppe ab. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/validateMoveResources/action | Überprüft das Verschieben von Ressourcen aus einer Ressourcengruppe in eine andere. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/write | Erstellt oder aktualisiert eine Ressourcengruppe. |
> | Action | Microsoft.Resources/subscriptions/resources/read | Ruft die Ressourcen eines Abonnements ab. |
> | Action | Microsoft.Resources/subscriptions/tagNames/delete | Löscht ein Abonnementtag. |
> | Action | Microsoft.Resources/subscriptions/tagNames/read | Ruft Abonnementtags ab oder listet sie auf. |
> | Action | Microsoft.Resources/subscriptions/tagNames/tagValues/delete | Löscht einen Abonnementtagwert. |
> | Action | Microsoft.Resources/subscriptions/tagNames/tagValues/read | Ruft Abonnementtagwerte ab oder listet sie auf. |
> | Action | Microsoft.Resources/subscriptions/tagNames/tagValues/write | Fügt einen Abonnementtagwert hinzu. |
> | Action | Microsoft.Resources/subscriptions/tagNames/write | Fügt ein Abonnementtag hinzu. |
> | Action | Microsoft.Resources/tags/delete | Entfernt alle Tags einer Ressource |
> | Action | Microsoft.Resources/tags/read | Ruft alle Tags für eine Ressource ab |
> | Action | Microsoft.Resources/tags/write | Aktualisiert die Tags einer Ressource durch Ersetzen oder Zusammenführen vorhandener Tags mit einem neuen Satz von Tags oder entfernt die vorhandenen Tags |
> | Action | Microsoft.Resources/tenants/read | Ruft die Mandantenliste ab. |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Scheduler/jobcollections/delete | Löscht Auftragssammlungen. |
> | Action | Microsoft.Scheduler/jobcollections/disable/action | Deaktiviert Auftragssammlungen. |
> | Action | Microsoft.Scheduler/jobcollections/enable/action | Aktiviert Auftragssammlungen. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/delete | Löscht Aufträge. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/generateLogicAppDefinition/action | Generiert Logik-App-Definitionen auf der Grundlage eines Scheduler-Auftrags. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/jobhistories/read | Ruft den Auftragsverlauf ab. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/read | Ruft Aufträge ab. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/run/action | Führt Aufträge aus. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/write | Erstellt oder aktualisiert Aufträge. |
> | Action | Microsoft.Scheduler/jobcollections/read | Dient zum Abrufen von Auftragssammlungen. |
> | Action | Microsoft.Scheduler/jobcollections/write | Erstellt oder aktualisiert Auftragssammlungen. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Search/checkNameAvailability/action | Prüft die Verfügbarkeit des Dienstnamens. |
> | Action | Microsoft.Search/operations/read | Führt alle verfügbaren Vorgänge des Anbieters Microsoft.Search auf. |
> | Action | Microsoft.Search/register/action | Registriert das Abonnement für den Search-Ressourcenanbieter und ermöglicht die Erstellung von Suchdiensten. |
> | Action | Microsoft.Search/searchServices/createQueryKey/action | Erstellt den Abfrageschlüssel. |
> | Action | Microsoft.Search/searchServices/delete | Löscht den Suchdienst. |
> | Action | Microsoft.Search/searchServices/deleteQueryKey/delete | Löscht den Abfrageschlüssel. |
> | Action | Microsoft.Search/searchServices/listAdminKeys/action | Liest die Administratorschlüssel. |
> | Action | Microsoft.Search/searchServices/listQueryKeys/action | Gibt die Liste der API-Abfrageschlüssel für den angegebenen Azure Search-Dienst zurück. |
> | Action | Microsoft.Search/searchServices/listQueryKeys/read | Gibt die Liste der API-Abfrageschlüssel für den angegebenen Azure Search-Dienst zurück. |
> | Action | Microsoft.Search/searchServices/privateEndpointConnectionProxies/delete | Löscht einen Verbindungsproxy eines privaten Endpunkts |
> | Action | Microsoft.Search/searchServices/privateEndpointConnectionProxies/read | Gibt eine Liste der Verbindungsproxys eines privaten Endpunkts zurück oder ruft die Eigenschaften für den angegebenen Verbindungsproxy eines privaten Endpunkts ab. |
> | Action | Microsoft.Search/searchServices/privateEndpointConnectionProxies/validate/action | Überprüft den Aufruf zum Erstellen einer Verbindung mit einem privaten Endpunkt auf NRP-Seite |
> | Action | Microsoft.Search/searchServices/privateEndpointConnectionProxies/write | Erstellt einen Verbindungsproxy eines privaten Endpunkts mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für den angegebenen Verbindungsproxy eines privaten Endpunkts. |
> | Action | Microsoft.Search/searchServices/read | Liest den Suchdienst. |
> | Action | Microsoft.Search/searchServices/regenerateAdminKey/action | Generiert die Administratorschlüssel neu. |
> | Action | Microsoft.Search/searchServices/start/action | Startet den Suchdienst. |
> | Action | Microsoft.Search/searchServices/stop/action | Beendet den Suchdienst. |
> | Action | Microsoft.Search/searchServices/write | Erstellt oder aktualisiert den Suchdienst. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Security/adaptiveNetworkHardenings/enforce/action | Erzwingt die angegebenen Härtungsregeln für den Datenverkehr durch Erstellen passender Sicherheitsregeln in den angegebenen Netzwerksicherheitsgruppen |
> | Action | Microsoft.Security/adaptiveNetworkHardenings/read | Ruft Empfehlungen für das adaptive Erhöhen des Netzwerkschutzes für eine durch Azure geschützte Ressource ab |
> | Action | Microsoft.Security/advancedThreatProtectionSettings/read | Ruft die Einstellungen des erweiterten Schutzes vor Bedrohungen für die Ressource ab. |
> | Action | Microsoft.Security/advancedThreatProtectionSettings/write | Aktualisiert die Einstellungen des erweiterten Schutzes vor Bedrohungen für die Ressource. |
> | Action | Microsoft.Security/alerts/read | Ruft alle verfügbaren Sicherheitswarnungen ab. |
> | Action | Microsoft.Security/applicationWhitelistings/read | Ruft die Anwendungswhitelists ab. |
> | Action | Microsoft.Security/applicationWhitelistings/write | Erstellt eine neue Anwendungswhitelist oder aktualisiert eine bereits vorhandene. |
> | Action | Microsoft.Security/assessmentMetadata/read | Abrufen der verfügbaren Sicherheitsbewertungs-Metadaten für Ihr Abonnement |
> | Action | Microsoft.Security/assessmentMetadata/write | Erstellen oder Aktualisieren von Sicherheitsbewertungs-Metadaten |
> | Action | Microsoft.Security/assessments/read | Abrufen von Sicherheitsbewertungen für Ihr Abonnement |
> | Action | Microsoft.Security/assessments/write | Erstellen oder Aktualisieren von Sicherheitsbewertungen für Ihr Abonnement |
> | Action | Microsoft.Security/complianceResults/read | Ruft die Konformitätsergebnisse für die Ressource ab. |
> | Action | Microsoft.Security/informationProtectionPolicies/read | Ruft die Information Protection-Richtlinien für die Ressource ab. |
> | Action | Microsoft.Security/informationProtectionPolicies/write | Aktualisiert die Information Protection-Richtlinien für die Ressource. |
> | Action | Microsoft.Security/locations/alerts/activate/action | Aktiviert eine Sicherheitswarnung. |
> | Action | Microsoft.Security/locations/alerts/dismiss/action | Schließt eine Sicherheitswarnung. |
> | Action | Microsoft.Security/locations/alerts/read | Ruft alle verfügbaren Sicherheitswarnungen ab. |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/delete | Löscht die JIT-Netzwerkzugriffsrichtlinien. |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action | Löst eine Anforderung einer JIT-Netzwerkzugriffsrichtlinie aus. |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/read | Ruft die JIT-Netzwerkzugriffsrichtlinien ab. |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/write | Erstellt eine neue JIT-Netzwerkzugriffsrichtlinie oder aktualisiert eine bereits vorhandene. |
> | Action | Microsoft.Security/locations/read | Ruft den Speicherort der Sicherheitsdaten ab. |
> | Action | Microsoft.Security/locations/tasks/activate/action | Dient zum Aktivieren einer Sicherheitsempfehlung. |
> | Action | Microsoft.Security/locations/tasks/dismiss/action | Dient zum Verwerfen einer Sicherheitsempfehlung. |
> | Action | Microsoft.Security/locations/tasks/read | Ruft alle verfügbaren Sicherheitsempfehlungen ab. |
> | Action | Microsoft.Security/locations/tasks/resolve/action | Dient zum Auflösen einer Sicherheitsempfehlung. |
> | Action | Microsoft.Security/locations/tasks/start/action | Dient zum Starten einer Sicherheitsempfehlung. |
> | Action | Microsoft.Security/policies/read | Ruft die Sicherheitsrichtlinie ab. |
> | Action | Microsoft.Security/policies/write | Aktualisiert die Sicherheitsrichtlinie. |
> | Action | Microsoft.Security/pricings/delete | Löscht die Tarifeinstellungen für den Bereich. |
> | Action | Microsoft.Security/pricings/read | Ruft die Tarifeinstellungen für den Bereich ab. |
> | Action | Microsoft.Security/pricings/write | Aktualisiert die Tarifeinstellungen für den Bereich. |
> | Action | Microsoft.Security/register/action | Registriert das Abonnement für das Azure Security Center. |
> | Action | Microsoft.Security/securityContacts/delete | Löscht den Sicherheitskontakt. |
> | Action | Microsoft.Security/securityContacts/read | Ruft den Sicherheitskontakt ab. |
> | Action | Microsoft.Security/securityContacts/write | Aktualisiert den Sicherheitskontakt. |
> | Action | Microsoft.Security/securitySolutions/delete | Löscht eine Sicherheitslösung. |
> | Action | Microsoft.Security/securitySolutions/read | Ruft die Sicherheitslösungen ab. |
> | Action | Microsoft.Security/securitySolutions/write | Erstellt eine neue Sicherheitslösung oder aktualisiert eine bereits vorhandene. |
> | Action | Microsoft.Security/securitySolutionsReferenceData/read | Ruft die Verweisdaten für Sicherheitslösungen ab. |
> | Action | Microsoft.Security/securityStatuses/read | Ruft den Sicherheitsintegritätsstatus für Azure-Ressourcen ab. |
> | Action | Microsoft.Security/securityStatusesSummaries/read | Ruft die Zusammenfassungen der Sicherheitsstatus für den Bereich ab. |
> | Action | Microsoft.Security/settings/read | Ruft die Einstellungen für den Bereich ab. |
> | Action | Microsoft.Security/settings/write | Aktualisiert die Einstellungen für den Bereich. |
> | Action | Microsoft.Security/tasks/read | Ruft alle verfügbaren Sicherheitsempfehlungen ab. |
> | Action | Microsoft.Security/unregister/action | Hebt die Registrierung des Abonnements für das Azure Security Center auf. |
> | Action | Microsoft.Security/webApplicationFirewalls/delete | Löscht eine Web Application Firewall. |
> | Action | Microsoft.Security/webApplicationFirewalls/read | Ruft die Web Application Firewalls ab. |
> | Action | Microsoft.Security/webApplicationFirewalls/write | Erstellt eine neue Web Application Firewall oder aktualisiert eine bereits vorhandene. |
> | Action | Microsoft.Security/workspaceSettings/connect/action | Ändert die Einstellungen zum erneuten Herstellen einer Verbindung in den Arbeitsbereichseinstellungen. |
> | Action | Microsoft.Security/workspaceSettings/delete | Löscht die Arbeitsbereichseinstellungen. |
> | Action | Microsoft.Security/workspaceSettings/read | Ruft die Arbeitsbereichseinstellungen ab. |
> | Action | Microsoft.Security/workspaceSettings/write | Aktualisiert die Arbeitsbereichseinstellungen. |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.SecurityGraph/diagnosticsettings/delete | Löscht eine Diagnoseeinstellung. |
> | Action | Microsoft.SecurityGraph/diagnosticsettings/read | Liest eine Diagnoseeinstellung. |
> | Action | Microsoft.SecurityGraph/diagnosticsettings/write | Schreibt eine Diagnoseeinstellung. |
> | Action | Microsoft.SecurityGraph/diagnosticsettingscategories/read | Liest die Kategorien einer Diagnoseeinstellung. |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.SecurityInsights/Aggregations/read | Ruft aggregierte Informationen ab. |
> | Action | Microsoft.SecurityInsights/alertRules/actions/delete | Löscht die Antwortaktionen einer Warnungsregel. |
> | Action | Microsoft.SecurityInsights/alertRules/actions/read | Ruft die Antwortaktionen einer Warnungsregel ab. |
> | Action | Microsoft.SecurityInsights/alertRules/actions/write | Aktualisiert die Antwortaktionen einer Warnungsregel. |
> | Action | Microsoft.SecurityInsights/alertRules/delete | Löscht Warnungsregeln. |
> | Action | Microsoft.SecurityInsights/alertRules/read | Ruft die Warnungsregeln ab. |
> | Action | Microsoft.SecurityInsights/alertRules/write | Aktualisiert Warnungsregeln. |
> | Action | Microsoft.SecurityInsights/Bookmarks/delete | Löscht Lesezeichen. |
> | Action | Microsoft.SecurityInsights/Bookmarks/expand/action | Ruft verwandte Entitäten einer Entität über eine bestimmte Erweiterung ab. |
> | Action | Microsoft.SecurityInsights/Bookmarks/read | Ruft Lesezeichen ab. |
> | Action | Microsoft.SecurityInsights/Bookmarks/write | Aktualisiert Lesezeichen. |
> | Action | Microsoft.SecurityInsights/cases/comments/read | Ruft die Kommentare zu Fällen ab. |
> | Action | Microsoft.SecurityInsights/cases/comments/write | Erstellt die Kommentare zu Fällen. |
> | Action | Microsoft.SecurityInsights/cases/delete | Löscht einen Fall. |
> | Action | Microsoft.SecurityInsights/cases/investigations/read | Ruft die Falluntersuchungen ab. |
> | Action | Microsoft.SecurityInsights/cases/investigations/write | Aktualisiert die Metadaten eines Falls. |
> | Action | Microsoft.SecurityInsights/cases/read | Ruft einen Fall ab. |
> | Action | Microsoft.SecurityInsights/cases/write | Aktualisiert einen Fall. |
> | Action | Microsoft.SecurityInsights/dataConnectors/delete | Löscht einen Datenconnector. |
> | Action | Microsoft.SecurityInsights/dataConnectors/read | Ruft die Datenconnectors ab. |
> | Action | Microsoft.SecurityInsights/dataConnectors/write | Aktualisiert einen Datenconnector. |
> | Action | Microsoft.SecurityInsights/register/action | Registriert das Abonnement für Azure Sentinel. |
> | Action | Microsoft.SecurityInsights/settings/read | Ruft Einstellungen ab. |
> | Action | Microsoft.SecurityInsights/settings/write | Aktualisiert Einstellungen. |
> | Action | Microsoft.SecurityInsights/unregister/action | Hebt die Registrierung des Abonnements für Azure Sentinel auf. |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ServiceBus/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. |
> | Action | Microsoft.ServiceBus/checkNamespaceAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. Diese API ist veraltet. Verwenden Sie stattdessen CheckNameAvailability. |
> | Action | Microsoft.ServiceBus/locations/deleteVirtualNetworkOrSubnets/action | Löscht die VNET-Regeln im ServiceBus-Ressourcenanbieter für das angegebene VNET. |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/action | Dient zum Aktualisieren von Namespace-Autorisierungsregeln. Diese API ist veraltet. Verwenden Sie stattdessen einen PUT-Aufruf, um die Namespace-Autorisierungsregel zu aktualisieren. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/delete | Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden.  |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/listkeys/action | Dient zum Abrufen der Verbindungszeichenfolge für den Namespace. |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln. |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/write | Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel. |
> | Action | Microsoft.ServiceBus/namespaces/Delete | Dient zum Löschen von Namespaceressourcen. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Ruft die Schlüssel der Autorisierungsregeln für den primären Namespace für die Notfallwiederherstellung. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/read | Ruft die Autorisierungsregeln des primären Namespace für die Notfallwiederherstellung ab. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/breakPairing/action | Hiermit wird die Notfallwiederherstellung deaktiviert und die Replikation von Änderungen vom primären Namespace zu sekundären Namespaces beendet. |
> | Action | Microsoft.ServiceBus/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespacealias unter dem angegebenen Abonnement. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/delete | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration gelöscht. Dieser Vorgang kann nur über den primären Namespace aufgerufen werden. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/failover/action | Hiermit wird ein Failover bei der georedundanten Notfallwiederherstellung ausgelöst und der Namespacealias neu konfiguriert, um auf den sekundären Namespace zu zeigen. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/read | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration abgerufen. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/write | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration erstellt oder aktualisiert. |
> | Action | Microsoft.ServiceBus/namespaces/eventGridFilters/delete | Löscht den Event Grid-Filter, der dem Namespace zugeordnet ist. |
> | Action | Microsoft.ServiceBus/namespaces/eventGridFilters/read | Ruft den Event Grid-Filter ab, der dem Namespace zugeordnet ist. |
> | Action | Microsoft.ServiceBus/namespaces/eventGridFilters/write | Erstellt oder aktualisiert den Event Grid-Filter, der dem Namespace zugeordnet ist. |
> | Action | Microsoft.ServiceBus/namespaces/eventhubs/read | Dient zum Abrufen einer Liste mit EventHub-Ressourcenbeschreibungen. |
> | Action | Microsoft.ServiceBus/namespaces/ipFilterRules/delete | Löscht eine IP-Filterressource. |
> | Action | Microsoft.ServiceBus/namespaces/ipFilterRules/read | Ruft eine IP-Filterressource ab. |
> | Action | Microsoft.ServiceBus/namespaces/ipFilterRules/write | Erstellt eine IP-Filterressource. |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/receive/action | Empfangen von Nachrichten |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/send/action | Senden von Nachrichten |
> | Action | Microsoft.ServiceBus/namespaces/messagingPlan/read | Hiermit wird der Messagingplan für einen Namespace abgerufen.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Action | Microsoft.ServiceBus/namespaces/messagingPlan/write | Hiermit wird der Messagingplan für einen Namespace aktualisiert.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Action | Microsoft.ServiceBus/namespaces/migrate/action | Migriert den Namespacevorgang. |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/delete | Löscht die Migrationskonfiguration. |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/read | Ruft die Migrationskonfiguration ab, die den Zustand der Migration und ausstehende Replikationsvorgänge angibt. |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/revert/action | Setzt die Migration vom Standard- zum Premium-Namespace zurück. |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/upgrade/action | Weist den dem Standard-Namespace zugeordneten DNS-Wert dem Premium-Namespace zu, wodurch die Migration abgeschlossen und die Ressourcensynchronisierung zwischen dem Standard- und dem Premium-Namespace beendet wird. |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/write | Erstellt oder aktualisiert die Migrationskonfiguration. Dadurch wird die Synchronisierung der Ressourcen zwischen dem Standard- und dem Premium-Namespace ausgelöst. |
> | Action | Microsoft.ServiceBus/namespaces/networkruleset/delete | Löscht eine VNET-Regelressource. |
> | Action | Microsoft.ServiceBus/namespaces/networkruleset/read | Ruft die NetworkRuleSet-Ressource ab |
> | Action | Microsoft.ServiceBus/namespaces/networkruleset/write | Erstellt eine VNET-Regelressource. |
> | Action | Microsoft.ServiceBus/namespaces/networkrulesets/delete | Löscht eine VNET-Regelressource. |
> | Action | Microsoft.ServiceBus/namespaces/networkrulesets/read | Ruft die NetworkRuleSet-Ressource ab |
> | Action | Microsoft.ServiceBus/namespaces/networkrulesets/write | Erstellt eine VNET-Regelressource. |
> | Action | Microsoft.ServiceBus/namespaces/operationresults/read | Hiermit wird der Status des Namespacevorgangs abgerufen. |
> | Action | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen. |
> | Action | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen. |
> | Action | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/logDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespaceprotokolle. |
> | Action | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/action | Vorgang zum Aktualisieren von Queue. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/delete | Vorgang zum Löschen von Warteschlangen-Autorisierungsregeln. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/listkeys/action | Dient zum Abrufen der Warteschlangen-Verbindungszeichenfolge. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/read |  Dient zum Abrufen der Liste mit Warteschlangen-Autorisierungsregeln. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/write | Dient zum Erstellen von Warteschlangen-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden. |
> | Action | Microsoft.ServiceBus/namespaces/queues/Delete | Vorgang zum Löschen von Warteschlangenressourcen. |
> | Action | Microsoft.ServiceBus/namespaces/queues/read | Dient zum Abrufen einer Liste mit Warteschlangen-Ressourcenbeschreibungen. |
> | Action | Microsoft.ServiceBus/namespaces/queues/write | Dient zum Erstellen oder Aktualisieren von Warteschlangeneigenschaften. |
> | Action | Microsoft.ServiceBus/namespaces/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen. |
> | Action | Microsoft.ServiceBus/namespaces/removeAcsNamepsace/action | Entfernt den ACS-Namespace. |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/action | Vorgang zum Aktualisieren von Topic. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren. |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/delete | Vorgang zum Löschen von Themaautorisierungsregeln. |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/listkeys/action | Dient zum Abrufen der Themaverbindungszeichenfolge. |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/read |  Dient zum Abrufen der Liste mit Themaautorisierungsregeln. |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/write | Dient zum Erstellen von Themaautorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden. |
> | Action | Microsoft.ServiceBus/namespaces/topics/Delete | Vorgang zum Löschen von Themaressourcen. |
> | Action | Microsoft.ServiceBus/namespaces/topics/read | Dient zum Abrufen einer Liste mit Themaressourcenbeschreibungen. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/Delete | Vorgang zum Löschen von TopicSubscription-Ressourcen. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/read | Dient zum Abrufen einer Liste mit TopicSubscription-Ressourcenbeschreibungen. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/Delete | Vorgang zum Löschen von Regelressourcen. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/read | Dient zum Abrufen einer Liste mit Regelressourcenbeschreibungen. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/write | Dient zum Erstellen oder Aktualisieren von Aktualisierungsregeleigenschaften. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/write | Dient zum Erstellen oder Aktualisieren von TopicSubscription-Eigenschaften. |
> | Action | Microsoft.ServiceBus/namespaces/topics/write | Dient zum Erstellen oder Aktualisieren von Themaeigenschaften. |
> | Action | Microsoft.ServiceBus/namespaces/virtualNetworkRules/delete | Löscht eine VNET-Regelressource. |
> | Action | Microsoft.ServiceBus/namespaces/virtualNetworkRules/read | Ruft eine VNET-Regelressource ab. |
> | Action | Microsoft.ServiceBus/namespaces/virtualNetworkRules/write | Erstellt eine VNET-Regelressource. |
> | Action | Microsoft.ServiceBus/namespaces/write | Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden. |
> | Action | Microsoft.ServiceBus/operations/read | Dient zum Abrufen von Vorgängen. |
> | Action | Microsoft.ServiceBus/register/action | Registriert das Abonnement für den ServiceBus-Ressourcenanbieter und ermöglicht die Erstellung von ServiceBus-Ressourcen. |
> | Action | Microsoft.ServiceBus/sku/read | Dient zum Abrufen einer Liste mit SKU-Ressourcenbeschreibungen. |
> | Action | Microsoft.ServiceBus/sku/regions/read | Dient zum Abrufen einer Liste mit SkuRegions-Ressourcenbeschreibungen. |
> | Action | Microsoft.ServiceBus/unregister/action | Registriert das Abonnement für den ServiceBus-Ressourcenanbieter und ermöglicht die Erstellung von ServiceBus-Ressourcen. |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.ServiceFabric/clusters/applications/delete | Hiermit löschen Sie eine Anwendung. |
> | Action | Microsoft.ServiceFabric/clusters/applications/read | Liest eine Anwendung. |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/delete | Hiermit löschen Sie einen Dienst. |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/partitions/read | Liest eine Partition. |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/partitions/replicas/read | Liest ein Replikat. |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/read | Liest einen Dienst. |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/statuses/read | Liest einen Dienststatus. |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/write | Hiermit erstellen oder aktualisieren Sie einen Dienst. |
> | Action | Microsoft.ServiceFabric/clusters/applications/write | Hiermit erstellen oder aktualisieren Sie eine Anwendung. |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/delete | Hiermit löschen Sie einen Anwendungstyp. |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/read | Hiermit lesen Sie einen Anwendungstyp. |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/versions/delete | Hiermit löschen Sie eine Anwendungstypversion. |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/versions/read | Hiermit lesen Sie eine Anwendungstypversion. |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/versions/write | Hiermit erstellen oder aktualisieren Sie eine Anwendungstypversion. |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/write | Hiermit erstellen oder aktualisieren Sie einen Anwendungstyp. |
> | Action | Microsoft.ServiceFabric/clusters/delete | Löscht einen Cluster. |
> | Action | Microsoft.ServiceFabric/clusters/nodes/read | Liest einen Knoten. |
> | Action | Microsoft.ServiceFabric/clusters/read | Liest einen Cluster. |
> | Action | Microsoft.ServiceFabric/clusters/statuses/read | Liest einen Clusterstatus. |
> | Action | Microsoft.ServiceFabric/clusters/write | Erstellt oder aktualisiert einen Cluster. |
> | Action | Microsoft.ServiceFabric/locations/clusterVersions/read | Liest Clusterversionen. |
> | Action | Microsoft.ServiceFabric/locations/environments/clusterVersions/read | Hiermit lesen Sie eine Clusterversion für eine bestimmte Umgebung. |
> | Action | Microsoft.ServiceFabric/locations/operationresults/read | Hiermit lesen Sie Vorgangsergebnisse. |
> | Action | Microsoft.ServiceFabric/locations/operations/read | Hiermit lesen Sie Vorgänge nach Standort. |
> | Action | Microsoft.ServiceFabric/operations/read | Hiermit lesen Sie verfügbare Vorgänge. |
> | Action | Microsoft.ServiceFabric/register/action | Hiermit registrieren Sie eine Aktion. |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.SignalRService/locations/checknameavailability/action | Überprüft, ob ein Name für die Verwendung mit einem neuen SignalR-Dienst verfügbar ist. |
> | Action | Microsoft.SignalRService/locations/operationresults/signalr/read | Fragt den Status eines asynchronen Vorgangs ab |
> | Action | Microsoft.SignalRService/locations/operationStatuses/operationId/read | Fragt den Status eines asynchronen Vorgangs ab |
> | Action | Microsoft.SignalRService/locations/usages/read | Ruft den Kontingentbedarf für den Azure SignalR Service ab. |
> | Action | Microsoft.SignalRService/operationresults/read | Fragt den Status eines asynchronen Vorgangs ab |
> | Action | Microsoft.SignalRService/operations/read | Auflisten der Vorgänge für Azure SignalR Service. |
> | Action | Microsoft.SignalRService/operationstatus/read | Fragt den Status eines asynchronen Vorgangs ab |
> | Action | Microsoft.SignalRService/register/action | Registriert den Ressourcenanbieter „Microsoft.SignalRService“ bei einem Abonnement. |
> | Action | Microsoft.SignalRService/SignalR/delete | Löscht den gesamten SignalR Service. |
> | Action | Microsoft.SignalRService/SignalR/eventGridFilters/delete | Löscht einen Event Grid-Filter aus einer SignalR-Instanz |
> | Action | Microsoft.SignalRService/SignalR/eventGridFilters/read | Ruft die Eigenschaften des angegebenen Event Grid-Filters ab oder listet alle Event Grid-Filter für die angegebene SignalR-Instanz auf |
> | Action | Microsoft.SignalRService/SignalR/eventGridFilters/write | Erstellt oder aktualisiert einen Event Grid-Filter für eine SignalR-Instanz mit den angegebenen Parametern |
> | Action | Microsoft.SignalRService/SignalR/listkeys/action | Zeigt den Wert von SignalR-Zugriffsschlüsseln im Verwaltungsportal oder über die API an. |
> | Action | Microsoft.SignalRService/SignalR/privateEndpointConnectionProxies/delete | Löscht den Verbindungsproxy für einen privaten Endpunkt. |
> | Action | Microsoft.SignalRService/SignalR/privateEndpointConnectionProxies/read | Liest den Verbindungsproxy für einen privaten Endpunkt. |
> | Action | Microsoft.SignalRService/SignalR/privateEndpointConnectionProxies/validate/action | Überprüft den Verbindungsproxy für einen privaten Endpunkt. |
> | Action | Microsoft.SignalRService/SignalR/privateEndpointConnectionProxies/write | Erstellt einen Verbindungsproxy für einen privaten Endpunkt. |
> | Action | Microsoft.SignalRService/SignalR/privateEndpointConnections/read | Liest die Verbindung eines privaten Endpunkts. |
> | Action | Microsoft.SignalRService/SignalR/privateEndpointConnections/write | Genehmigt die Verbindung eines privaten Endpunkts oder lehnt sie ab. |
> | Action | Microsoft.SignalRService/SignalR/privateLinkResources/read | Listet alle Ressourcen einer privaten SignalR-Verbindung auf. |
> | Action | Microsoft.SignalRService/SignalR/read | Zeigt die SignalR-Einstellungen und Konfigurationen im Verwaltungsportal oder über die API an. |
> | Action | Microsoft.SignalRService/SignalR/regeneratekey/action | Ändert den Wert von SignalR-Zugriffsschlüsseln im Verwaltungsportal oder über die API. |
> | Action | Microsoft.SignalRService/SignalR/restart/action | Dient zum Neustarten eines Azure SignalR Service im Verwaltungsportal oder über die API. Hierbei kommt es zu einer Ausfallzeit. |
> | Action | Microsoft.SignalRService/SignalR/write | Bearbeitet die SignalR-Einstellungen und Konfigurationen im Verwaltungsportal oder über die API. |
> | Action | Microsoft.SignalRService/unregister/action | Hebt die Registrierung des Ressourcenanbieters „Microsoft.SignalRService“ bei einem Abonnement auf. |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Solutions/applicationDefinitions/applicationArtifacts/read | Hiermit werden die Anwendungsartefakte der Anwendungsdefinition aufgelistet. |
> | Action | Microsoft.Solutions/applicationDefinitions/delete | Hiermit wird eine Anwendungsdefinition entfernt. |
> | Action | Microsoft.Solutions/applicationDefinitions/read | Hiermit wird eine Liste mit Anwendungsdefinitionen abgerufen. |
> | Action | Microsoft.Solutions/applicationDefinitions/write | Hiermit wird eine Anwendungsdefinition hinzugefügt oder geändert. |
> | Action | Microsoft.Solutions/applications/applicationArtifacts/read | Hiermit werden Anwendungsartefakte aufgelistet. |
> | Action | Microsoft.Solutions/applications/delete | Hiermit wird eine Anwendung entfernt. |
> | Action | Microsoft.Solutions/applications/read | Hiermit wird eine Liste mit Anwendungen abgerufen. |
> | Action | Microsoft.Solutions/applications/refreshPermissions/action | Hiermit werden die Anwendungsberechtigungen aktualisiert. |
> | Action | Microsoft.Solutions/applications/updateAccess/action | Hiermit wird der Anwendungszugriff aktualisiert. |
> | Action | Microsoft.Solutions/applications/write | Erstellt eine Anwendung |
> | Action | Microsoft.Solutions/jitRequests/delete | Hiermit wird eine JitRequest entfernt. |
> | Action | Microsoft.Solutions/jitRequests/read | Hiermit wird eine Liste mit JitRequests abgerufen. |
> | Action | Microsoft.Solutions/jitRequests/write | Hiermit wird eine JitRequest erstellt. |
> | Action | Microsoft.Solutions/locations/operationStatuses/read | Liest den Vorgangsstatus für die Ressource. |
> | Action | Microsoft.Solutions/operations/read | Ruft die Liste der Vorgänge ab. |
> | Action | Microsoft.Solutions/register/action | Hiermit registrieren Sie sich bei Lösungen. |
> | Action | Microsoft.Solutions/unregister/action | Hiermit wird die Registrierung von Lösungen aufgehoben. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Sql/checkNameAvailability/action | Überprüfen Sie, ob der angegebene Servername für die weltweite Bereitstellung bei einem bestimmten Abonnement verfügbar ist. |
> | Action | Microsoft.Sql/instancePools/delete | Löscht einen Instanzenpool. |
> | Action | Microsoft.Sql/instancePools/read | Ruft einen Instanzenpool ab. |
> | Action | Microsoft.Sql/instancePools/usages/read | Ruft die Nutzungsinformationen für einen Instanzenpool ab. |
> | Action | Microsoft.Sql/instancePools/write | Dient zum Erstellen oder Aktualisieren eines Instanzenpools. |
> | Action | Microsoft.Sql/locations/auditingSettingsAzureAsyncOperation/read | Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die erweiterte Serverblobüberwachung. |
> | Action | Microsoft.Sql/locations/auditingSettingsOperationResults/read | Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die Serverblobüberwachung. |
> | Action | Microsoft.Sql/locations/capabilities/read | Ruft die Funktionen für dieses Abonnement in einem bestimmten Standort ab. |
> | Action | Microsoft.Sql/locations/databaseAzureAsyncOperation/read | Ruft den Status eines Datenbankvorgangs ab. |
> | Action | Microsoft.Sql/locations/databaseOperationResults/read | Ruft den Status eines Datenbankvorgangs ab. |
> | Action | Microsoft.Sql/locations/deletedServerAsyncOperation/read | Ruft gegenwärtig ausgeführte Vorgänge für gelöschte Server ab. |
> | Action | Microsoft.Sql/locations/deletedServerOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für gelöschte Server ab. |
> | Action | Microsoft.Sql/locations/deletedServers/read | Gibt die Liste der gelöschten Server zurück oder ruft die Eigenschaften für den angegebenen gelöschten Server ab. |
> | Action | Microsoft.Sql/locations/deletedServers/recover/action | Stellt einen gelöschten Server wieder her. |
> | Action | Microsoft.Sql/locations/elasticPoolAzureAsyncOperation/read | Ruft den asynchronen Vorgang in Azure für einen asynchronen Vorgang in einem Pool für elastische Datenbank ab. |
> | Action | Microsoft.Sql/locations/elasticPoolOperationResults/read | Ruft das Ergebnis eines Vorgangs eines Pools für elastische Datenbanken ab. |
> | Action | Microsoft.Sql/locations/encryptionProtectorAzureAsyncOperation/read | Ruft gegenwärtig ausgeführte Vorgänge für die Verschlüsselungsschutzvorrichtung von Transparent Data Encryption ab. |
> | Action | Microsoft.Sql/locations/encryptionProtectorOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für die Verschlüsselungsschutzvorrichtung von Transparent Data Encryption ab. |
> | Action | Microsoft.Sql/locations/extendedAuditingSettingsAzureAsyncOperation/read | Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die erweiterte Serverblobüberwachung. |
> | Action | Microsoft.Sql/locations/extendedAuditingSettingsOperationResults/read | Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die erweiterte Serverblobüberwachung. |
> | Action | Microsoft.Sql/locations/firewallRulesAzureAsyncOperation/read | Ruft den Status eines Firewallregelvorgangs ab. |
> | Action | Microsoft.Sql/locations/firewallRulesOperationResults/read | Ruft den Status eines Firewallregelvorgangs ab. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/delete | Löscht eine vorhandene Failovergruppe einer Instanz. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/failover/action | Führt das geplante Failover in einer vorhandenen Failovergruppe einer Instanz durch. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/forceFailoverAllowDataLoss/action | Führt das erzwungene Failover in einer vorhandenen Failovergruppe einer Instanz durch. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/read | Gibt die Liste der Failovergruppen einer Instanz zurück oder ruft die Eigenschaften für die angegebene Failovergruppe einer Instanz ab. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/write | Erstellt eine Failovergruppe einer Instanz mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für die angegebene Failovergruppe einer Instanz. |
> | Action | Microsoft.Sql/locations/instancePoolAzureAsyncOperation/read | Ruft den Status eines Instanzenpoolvorgangs ab. |
> | Action | Microsoft.Sql/locations/instancePoolOperationResults/read | Ruft das Ergebnis eines Instanzenpoolvorgangs ab. |
> | Action | Microsoft.Sql/locations/interfaceEndpointProfileAzureAsyncOperation/read | Gibt die Details eines asynchronen Azure-Vorgangs für einen bestimmten Schnittstellenendpunkt zurück. |
> | Action | Microsoft.Sql/locations/interfaceEndpointProfileOperationResults/read | Gibt die Details des angegebenen Schnittstellenendpunkt-Vorgangs zurück. |
> | Action | Microsoft.Sql/locations/jobAgentAzureAsyncOperation/read | Ruft den Status eines Auftrags-Agent-Vorgangs ab. |
> | Action | Microsoft.Sql/locations/jobAgentOperationResults/read | Ruft die Ergebnisse eines Auftrags-Agent-Vorgangs ab. |
> | Action | Microsoft.Sql/locations/longTermRetentionBackups/read | Listet die Sicherungen zur langfristigen Aufbewahrung für jede Datenbank auf jedem Server an einem Speicherort auf. |
> | Action | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read | Listet die Sicherungen zur langfristigen Aufbewahrung für jede Datenbank auf jedem Server auf. |
> | Action | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete | Löscht eine Sicherung zur langfristigen Aufbewahrung. |
> | Action | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read | Listet die Sicherungen zur langfristigen Aufbewahrung für eine Datenbank auf. |
> | Action | Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action | Führt den Vorgang zur Wiederherstellung einer verwalteten Datenbank durch. |
> | Action | Microsoft.Sql/locations/managedInstanceEncryptionProtectorAzureAsyncOperation/read | Ruft gegenwärtig ausgeführte Vorgänge für die Verschlüsselungsschutzvorrichtung der verwalteten Instanz von Transparent Data Encryption ab. |
> | Action | Microsoft.Sql/locations/managedInstanceEncryptionProtectorOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für die Verschlüsselungsschutzvorrichtung der verwalteten Instanz von Transparent Data Encryption ab. |
> | Action | Microsoft.Sql/locations/managedInstanceKeyAzureAsyncOperation/read | Ruft gegenwärtig ausgeführte Vorgänge für verwaltete Instanzenschlüssel von Transparent Data Encryption ab. |
> | Action | Microsoft.Sql/locations/managedInstanceKeyOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für verwaltete Instanzenschlüssel von Transparent Data Encryption ab. |
> | Action | Microsoft.Sql/locations/managedInstanceLongTermRetentionPolicyAzureAsyncOperation/read | Ruft den Status des Vorgangs für eine Richtlinie zur langfristigen Aufbewahrung für eine verwaltete Datenbank ab. |
> | Action | Microsoft.Sql/locations/managedInstanceLongTermRetentionPolicyOperationResults/read | Ruft den Status des Vorgangs für eine Richtlinie zur langfristigen Aufbewahrung für eine verwaltete Datenbank ab. |
> | Action | Microsoft.Sql/locations/managedShortTermRetentionPolicyOperationResults/read | Ruft den Status des Vorgangs für eine Richtlinie zur kurzfristigen Aufbewahrung ab. |
> | Action | Microsoft.Sql/locations/managedTransparentDataEncryptionAzureAsyncOperation/read | Ruft gegenwärtig ausgeführte Vorgänge für Transparent Data Encryption einer verwalteten Datenbank ab. |
> | Action | Microsoft.Sql/locations/managedTransparentDataEncryptionOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für Transparent Data Encryption einer verwalteten Datenbank ab. |
> | Action | Microsoft.Sql/locations/privateEndpointConnectionAzureAsyncOperation/read | Ruft das Ergebnis für einen Vorgang auf der Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.Sql/locations/privateEndpointConnectionOperationResults/read | Ruft das Ergebnis für einen Vorgang auf der Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.Sql/locations/privateEndpointConnectionProxyAzureAsyncOperation/read | Ruft das Ergebnis für einen Vorgang auf dem Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.Sql/locations/privateEndpointConnectionProxyOperationResults/read | Ruft das Ergebnis für einen Vorgang auf dem Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.Sql/locations/read | Ruft die verfügbaren Standorte für ein bestimmtes Abonnement ab. |
> | Action | Microsoft.Sql/locations/serverKeyAzureAsyncOperation/read | Ruft gegenwärtig ausgeführte Vorgänge für Serverschlüssel von Transparent Data Encryption ab. |
> | Action | Microsoft.Sql/locations/serverKeyOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für Serverschlüssel von Transparent Data Encryption ab. |
> | Action | Microsoft.Sql/locations/shortTermRetentionPolicyOperationResults/read | Ruft den Status des Vorgangs für eine Richtlinie zur kurzfristigen Aufbewahrung ab. |
> | Action | Microsoft.Sql/locations/syncAgentOperationResults/read | Ruft das Ergebnis des Ressourcenvorgangs des Synchronisierungs-Agents ab. |
> | Action | Microsoft.Sql/locations/syncDatabaseIds/read | Ruft die Synchronisierungsdatenbank-IDs für eine bestimmte Region und ein bestimmtes Abonnement ab. |
> | Action | Microsoft.Sql/locations/syncGroupOperationResults/read | Ruft das Ergebnis des Ressourcenvorgangs der Synchronisierungsgruppe ab. |
> | Action | Microsoft.Sql/locations/syncMemberOperationResults/read | Ruft das Ergebnis des Ressourcenvorgangs des Synchronisierungsmitglieds ab. |
> | Action | Microsoft.Sql/locations/usages/read | Ruft eine Collection von Nutzungsmetriken für dieses Abonnement an einem Standort ab. |
> | Action | Microsoft.Sql/locations/virtualNetworkRulesAzureAsyncOperation/read | Gibt die Details des asynchronen Vorgangs in Azure für die angegebene VNET-Regel zurück.  |
> | Action | Microsoft.Sql/locations/virtualNetworkRulesOperationResults/read | Gibt die Details der VNET-Regeln zurück.  |
> | Action | Microsoft.Sql/managedInstances/administrators/delete | Löscht einen vorhandenen Administrator einer verwalteten Instanz. |
> | Action | Microsoft.Sql/managedInstances/administrators/read | Ruft eine Liste der Administratoren für verwaltete Instanzen ab. |
> | Action | Microsoft.Sql/managedInstances/administrators/write | Erstellt oder aktualisiert den Administrator für verwaltete Instanzen mit den angegebenen Parametern. |
> | Action | Microsoft.Sql/managedInstances/databases/backupLongTermRetentionPolicies/read | Ruft eine Richtlinie zur langfristigen Aufbewahrung für eine verwaltete Datenbank ab. |
> | Action | Microsoft.Sql/managedInstances/databases/backupLongTermRetentionPolicies/write | Aktualisiert eine Richtlinie zur langfristigen Aufbewahrung für eine verwaltete Datenbank. |
> | Action | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/read | Ruft eine kurzfristige Aufbewahrungsrichtlinie für eine verwaltete Datenbank ab. |
> | Action | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/write | Aktualisiert eine kurzfristige Aufbewahrungsrichtlinie für eine verwaltete Datenbank. |
> | Action | Microsoft.Sql/managedInstances/databases/columns/read | Zurückgeben einer Liste von Spalten für eine verwaltete Datenbank |
> | Action | Microsoft.Sql/managedInstances/databases/completeRestore/action | Führt den Vorgang zur Wiederherstellung einer verwalteten Datenbank durch. |
> | Action | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/read | Listet die Vertraulichkeitsbezeichnungen einer bestimmten Datenbank auf. |
> | Action | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/write | Batchupdate von Vertraulichkeitsbezeichnungen |
> | Action | Microsoft.Sql/managedInstances/databases/delete | Löscht eine vorhandene verwaltete Datenbank. |
> | Action | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Datenbanken der verwalteten Instanz ab. |
> | Action | Microsoft.Sql/managedInstances/databases/read | Ruft eine vorhandene verwaltete Datenbank ab. |
> | Action | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/read | Listet die Vertraulichkeitsbezeichnungen einer bestimmten Datenbank auf. |
> | Action | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/write | Batchupdate von empfohlenen Vertraulichkeitsbezeichnungen |
> | Action | Microsoft.Sql/managedInstances/databases/restoreDetails/read | Gibt Details zur Wiederherstellung einer verwalteten Datenbank während der Wiederherstellung zurück. |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/read | Ruft ein verwaltetes Datenbankschema ab. (Nur Schema) |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/read | Ruft eine verwaltete Datenbankspalte ab (nur Schema). |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/delete | Löscht die Vertraulichkeitsbezeichnung einer bestimmten Spalte. |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/disable/action | Deaktiviert die Vertraulichkeitsempfehlungen für eine bestimmte Spalte. |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/enable/action | Aktiviert die Vertraulichkeitsempfehlungen für eine bestimmte Spalte. |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/read | Ruft die Vertraulichkeitsbezeichnung einer bestimmten Spalte ab. |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/write | Erstellt oder aktualisiert die Vertraulichkeitsbezeichnung einer bestimmten Spalte. |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/read | Ruft eine verwaltete Datenbanktabelle ab (nur Schema). |
> | Action | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/read | Ruft eine Liste der Bedrohungserkennungrichtlinien für verwaltete Datenbanken ab, die für einen bestimmten Server konfiguriert sind. |
> | Action | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/write | Dient zum Ändern der Bedrohungserkennungsrichtlinie für eine bestimmte verwaltete Datenbank. |
> | Action | Microsoft.Sql/managedInstances/databases/securityEvents/read | Ruft die Sicherheitsereignisse für die verwaltete Datenbank ab. |
> | Action | Microsoft.Sql/managedInstances/databases/sensitivityLabels/read | Listet die Vertraulichkeitsbezeichnungen einer bestimmten Datenbank auf. |
> | Action | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/read | Ruft Details zu Datenbank-TDE (Transparent Data Encryption) für eine bestimmte verwaltete Datenbank ab. |
> | Action | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/write | Ändert Transparent Data Encryption für Datenbanken für eine bestimmte verwaltete Datenbank. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/delete | Entfernt die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/read | Ruft die Sicherheitsrisikobewertungs-Richtlinien für eine angegebene Datenbank ab. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/delete | Entfernt die Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/read | Ruft das Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank ab. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/write | Ändert die Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/export/action | Konvertiert ein vorhandenes Überprüfungsergebnis in ein lesbares Format. Wenn dies bereits der Fall ist, werden keine Vorgänge ausgeführt. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/initiateScan/action | Dient zum Ausführen einer Datenbanküberprüfung zur Bewertung von Sicherheitsrisiken. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/read | Gibt die Liste der Überprüfungsdatensätze für eine Datenbank-Sicherheitsrisikobewertung oder den Überprüfungsdatensatz für die angegebene Überprüfungs-ID zurück. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/write | Ändert die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/managedInstances/databases/write | Erstellt eine neue Datenbank oder aktualisiert eine bereits vorhandene Datenbank |
> | Action | Microsoft.Sql/managedInstances/delete | Löscht eine vorhandene verwaltete Instanz. |
> | Action | Microsoft.Sql/managedInstances/encryptionProtector/read | Gibt eine Liste von Schutzvorrichtungen zur Serververschlüsselung zurück oder ruft die Eigenschaften für die angegebene Schutzvorrichtung zur Serververschlüsselung ab. |
> | Action | Microsoft.Sql/managedInstances/encryptionProtector/revalidate/action | Aktualisiert die Eigenschaften für die angegebene Schutzvorrichtung zur Serververschlüsselung. |
> | Action | Microsoft.Sql/managedInstances/encryptionProtector/write | Aktualisiert die Eigenschaften für die angegebene Schutzvorrichtung zur Serververschlüsselung. |
> | Action | Microsoft.Sql/managedInstances/inaccessibleManagedDatabases/read | Ruft eine Liste mit den unzugänglichen verwalteten Datenbank einer verwalteten Instanz ab. |
> | Action | Microsoft.Sql/managedInstances/keys/delete | Löscht einen vorhandenen Schlüssel für die verwaltete Azure SQL-Datenbank-Instanz. |
> | Action | Microsoft.Sql/managedInstances/keys/read | Gibt die Liste mit den verwalteten Instanzschlüsseln zurück oder ruft die Eigenschaften für den angegebenen Schlüssel der verwalteten Instanz ab. |
> | Action | Microsoft.Sql/managedInstances/keys/write | Erstellt einen Schlüssel mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den Schlüssel der verwalteten Instanz. |
> | Action | Microsoft.Sql/managedInstances/metricDefinitions/read | Ruft Metrikdefinitionen der verwalteten Instanz ab. |
> | Action | Microsoft.Sql/managedInstances/metrics/read | Ruft Metriken der verwalteten Instanz ab. |
> | Action | Microsoft.Sql/managedInstances/operations/cancel/action | Bricht einen ausstehenden asynchronen Vorgang einer verwaltete Azure SQL-Datenbank-Instanz ab, der noch nicht abgeschlossen ist. |
> | Action | Microsoft.Sql/managedInstances/operations/read | Ruft Vorgänge einer verwalteten Instanz ab. |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für verwaltete Instanzen ab. |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriken für verwaltete Instanzen. |
> | Action | Microsoft.Sql/managedInstances/read | Gibt die Liste der verwalteten Instanzen zurück oder ruft die Eigenschaften für die angegebene verwaltete Instanz ab. |
> | Action | Microsoft.Sql/managedInstances/recoverableDatabases/read | Gibt eine Liste mit wiederherstellbaren verwalteten Datenbanken zurück. |
> | Action | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/read | Ruft eine kurzfristige Aufbewahrungsrichtlinie für eine gelöschte verwaltete Datenbank ab. |
> | Action | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/write | Aktualisiert eine kurzfristige Aufbewahrungsrichtlinie für eine gelöschte verwaltete Datenbank. |
> | Action | Microsoft.Sql/managedInstances/restorableDroppedDatabases/read | Gibt eine Liste der wiederherstellbaren gelöschten verwalteten Datenbanken zurück. |
> | Action | Microsoft.Sql/managedInstances/securityAlertPolicies/read | Ruft eine Liste der Bedrohungserkennungrichtlinien für verwaltete Server ab, die für einen bestimmten Server konfiguriert sind. |
> | Action | Microsoft.Sql/managedInstances/securityAlertPolicies/write | Dient zum Ändern der Richtlinie zur Bedrohungserkennung bei verwalteten Servern für einen bestimmten verwalteten Server. |
> | Action | Microsoft.Sql/managedInstances/tdeCertificates/action | Erstellt oder aktualisiert das TDE-Zertifikat. |
> | Action | Microsoft.Sql/managedInstances/vulnerabilityAssessments/delete | Entfernt die Sicherheitsrisikobewertung für eine bestimmte verwaltete Instanz. |
> | Action | Microsoft.Sql/managedInstances/vulnerabilityAssessments/read | Ruft die Sicherheitsrisikobewertungs-Richtlinien für eine angegebene verwaltete Instanz ab. |
> | Action | Microsoft.Sql/managedInstances/vulnerabilityAssessments/write | Ändert die Sicherheitsrisikobewertung für eine bestimmte verwaltete Instanz. |
> | Action | Microsoft.Sql/managedInstances/write | Erstellt eine verwaltete Instanz mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die verwaltete Instanz. |
> | Action | Microsoft.Sql/operations/read | Ruft die verfügbaren REST-Vorgänge ab. |
> | Action | Microsoft.Sql/privateEndpointConnectionsApproval/action | Bestimmt, ob der Benutzer eine private Endpunktverbindung genehmigen darf. |
> | Action | Microsoft.Sql/register/action | Registriert das Abonnement für den Microsoft SQL-Datenbankressourcenanbieter und ermöglicht die Erstellung von Microsoft SQL-Datenbanken. |
> | Action | Microsoft.Sql/servers/administrators/delete | Löscht ein bestimmtes Azure Active Directory-Administratorobjekt. |
> | Action | Microsoft.Sql/servers/administrators/read | Ruft ein bestimmtes Azure Active Directory-Administratorobjekt ab. |
> | Action | Microsoft.Sql/servers/administrators/write | Fügt ein bestimmtes Azure Active Directory-Administratorobjekt hinzu, oder aktualisiert es. |
> | Action | Microsoft.Sql/servers/advisors/read | Gibt eine Liste mit verfügbaren Ratgebern für den Server zurück. |
> | Action | Microsoft.Sql/servers/advisors/recommendedActions/read | Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für den Server zurück. |
> | Action | Microsoft.Sql/servers/advisors/recommendedActions/write | Dient zum Anwenden der empfohlenen Aktion auf den Server. |
> | Action | Microsoft.Sql/servers/advisors/write | Aktualisiert den Status der automatischen Ausführung eines Ratgebers auf der Serverebene. |
> | Action | Microsoft.Sql/servers/auditingPolicies/read | Dient zum Abrufen von Details zur Richtlinie für die Standardservertabellen-Überwachung, die für einen bestimmten Server konfiguriert ist. |
> | Action | Microsoft.Sql/servers/auditingPolicies/write | Dient zum Ändern der Standardservertabellen-Überwachung für einen bestimmten Server. |
> | Action | Microsoft.Sql/servers/auditingSettings/operationResults/read | Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die Serverblobüberwachung. |
> | Action | Microsoft.Sql/servers/auditingSettings/read | Dient zum Abrufen von Details zur Richtlinie für die Serverblobüberwachung, die für einen bestimmten Server konfiguriert ist. |
> | Action | Microsoft.Sql/servers/auditingSettings/write | Dient zum Ändern der Serverblobüberwachung für einen bestimmten Server. |
> | Action | Microsoft.Sql/servers/automaticTuning/read | Gibt die Einstellungen zur automatischen Optimierung für den Server zurück. |
> | Action | Microsoft.Sql/servers/automaticTuning/write | Aktualisiert die Einstellungen zur automatischen Optimierung für den Server und gibt die aktualisierten Einstellungen zurück. |
> | Action | Microsoft.Sql/servers/backupLongTermRetentionVaults/delete | Löscht einen vorhandenen Sicherungsarchivtresor. |
> | Action | Microsoft.Sql/servers/backupLongTermRetentionVaults/read | Vorgang zum Abrufen eines Sicherungstresors für die langfristige Aufbewahrung. Gibt Informationen zu dem Tresor zurück, der bei diesem Server registriert ist. |
> | Action | Microsoft.Sql/servers/backupLongTermRetentionVaults/write | Dieser Vorgang wird zum Registrieren eines Sicherungstresors für die langfristige Aufbewahrung bei einem Server verwendet. |
> | Action | Microsoft.Sql/servers/communicationLinks/delete | Löscht eine vorhandene Serverkommunikationsverbindung. |
> | Action | Microsoft.Sql/servers/communicationLinks/read | Gibt die Liste der Kommunikationsverbindungen eines bestimmten Servers zurück. |
> | Action | Microsoft.Sql/servers/communicationLinks/write | Erstellt oder aktualisiert eine Serverkommunikationsverbindung. |
> | Action | Microsoft.Sql/servers/connectionPolicies/read | Gibt die Liste der Serververbindungsrichtlinien eines angegebenen Servers zurück. |
> | Action | Microsoft.Sql/servers/connectionPolicies/write | Erstellt oder aktualisiert eine Serververbindungsrichtlinie. |
> | Action | Microsoft.Sql/servers/databases/advisors/read | Gibt eine Liste mit verfügbaren Ratgebern für die Datenbank zurück. |
> | Action | Microsoft.Sql/servers/databases/advisors/recommendedActions/read | Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für die Datenbank zurück. |
> | Action | Microsoft.Sql/servers/databases/advisors/recommendedActions/write | Dient zum Anwenden der empfohlenen Aktion auf die Datenbank. |
> | Action | Microsoft.Sql/servers/databases/advisors/write | Dient zum Aktualisieren des Status der automatischen Ausführung eines Ratgebers auf der Datenbankebene. |
> | Action | Microsoft.Sql/servers/databases/auditingPolicies/read | Dient zum Abrufen von Details zur Tabellenüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist. |
> | Action | Microsoft.Sql/servers/databases/auditingPolicies/write | Dient zum Ändern der Tabellenüberwachungsrichtlinie für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/auditingSettings/read | Dient zum Abrufen von Details zur Blobüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist. |
> | Action | Microsoft.Sql/servers/databases/auditingSettings/write | Dient zum Ändern der Blobüberwachungsrichtlinie für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/auditRecords/read | Dient zum Abrufen der Datensätze für die Datenbankblobüberwachung. |
> | Action | Microsoft.Sql/servers/databases/automaticTuning/read | Gibt die Einstellungen zur automatischen Optimierung für eine Datenbank zurück. |
> | Action | Microsoft.Sql/servers/databases/automaticTuning/write | Aktualisiert die Einstellungen zur automatischen Optimierung für eine Datenbank und gibt die aktualisierten Einstellungen zurück. |
> | Action | Microsoft.Sql/servers/databases/azureAsyncOperation/read | Ruft den Status eines Datenbankvorgangs ab. |
> | Action | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/read | Gibt die Liste der Sicherungsarchivierungsrichtlinien einer angegebenen Datenbank zurück. |
> | Action | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/write | Erstellt oder aktualisiert eine Datenbanksicherungs-Archivierungsrichtlinie. |
> | Action | Microsoft.Sql/servers/databases/backupShortTermRetentionPolicies/read | Ruft eine kurzfristige Aufbewahrungsrichtlinie für eine Datenbank ab |
> | Action | Microsoft.Sql/servers/databases/backupShortTermRetentionPolicies/write | Aktualisiert eine kurzfristige Aufbewahrungsrichtlinie für eine Datenbank |
> | Action | Microsoft.Sql/servers/databases/columns/read | Zurückgeben einer Liste von Spalten für eine Datenbank |
> | Action | Microsoft.Sql/servers/databases/connectionPolicies/read | Dient zum Abrufen von Details zur Verbindungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist. |
> | Action | Microsoft.Sql/servers/databases/connectionPolicies/write | Dient zum Ändern der Verbindungsrichtlinie für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/currentSensitivityLabels/read | Listet die Vertraulichkeitsbezeichnungen einer bestimmten Datenbank auf. |
> | Action | Microsoft.Sql/servers/databases/currentSensitivityLabels/write | Batchupdate von Vertraulichkeitsbezeichnungen |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/read | Gibt die Liste der Richtlinien zur Maskierung von Datenbankdaten zurück. |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/delete | Dient zum Löschen von Datenmaskierungsrichtlinien-Regeln für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/read | Dient zum Abrufen von Details zur Datenmaskierungsrichtlinien-Regel, die für eine bestimmte Datenbank konfiguriert ist. |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/write | Dient zum Ändern von Datenmaskierungsrichtlinen-Regeln für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/write | Dient zum Ändern der Datenmaskierungsrichtlinie für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read | Gibt die verteilten Abfrageschrittinformationen von Data Warehouse-Abfragen für die ausgewählte Schritt-ID zurück. |
> | Action | Microsoft.Sql/servers/databases/dataWarehouseQueries/read | Gibt die Data Warehouse-Verteilungsabfrageinformationen für die ausgewählte Abfrage-ID zurück. |
> | Action | Microsoft.Sql/servers/databases/dataWarehouseUserActivities/read | Ruft die Benutzeraktivitäten einer SQL Data Warehouse-Instanz ab, einschließlich ausgeführte und angehaltene Abfragen. |
> | Action | Microsoft.Sql/servers/databases/delete | Löscht eine vorhandene Datenbank. |
> | Action | Microsoft.Sql/servers/databases/export/action | Exportiert Azure SQL-Datenbank. |
> | Action | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Dient zum Abrufen von Details zur erweiterten Blobüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist. |
> | Action | Microsoft.Sql/servers/databases/extendedAuditingSettings/write | Dient zum Ändern der erweiterten Blobüberwachungsrichtlinie für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/extensions/read | Ruft eine Collection von Erweiterungen für die Datenbank ab. |
> | Action | Microsoft.Sql/servers/databases/extensions/write | Ändert die Erweiterung für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/failover/action | Vom Kunden initiiertes Datenbankfailover |
> | Action | Microsoft.Sql/servers/databases/geoBackupPolicies/read | Ruft Richtlinien für die Geosicherung einer bestimmten Datenbank ab. |
> | Action | Microsoft.Sql/servers/databases/geoBackupPolicies/write | Erstellt oder aktualisiert eine Richtlinie für die Geosicherung einer Datenbank. |
> | Action | Microsoft.Sql/servers/databases/importExportOperationResults/read | Ruft gegenwärtig ausgeführte Import-/Exportvorgänge ab. |
> | Action | Microsoft.Sql/servers/databases/maintenanceWindowOptions/read | Ruft eine Liste mit den verfügbaren Wartungsfenstern für eine ausgewählte Datenbank ab. |
> | Action | Microsoft.Sql/servers/databases/maintenanceWindows/read | Ruft Wartungsfenstereinstellungen für eine ausgewählte Datenbank ab. |
> | Action | Microsoft.Sql/servers/databases/maintenanceWindows/write | Legt Wartungsfenstereinstellungen für eine ausgewählte Datenbank fest. |
> | Action | Microsoft.Sql/servers/databases/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken. |
> | Action | Microsoft.Sql/servers/databases/metrics/read | Gibt Metriken für Datenbanken zurück. |
> | Action | Microsoft.Sql/servers/databases/move/action | Ändert den Namen einer vorhandenen Datenbank. |
> | Action | Microsoft.Sql/servers/databases/operationResults/read | Ruft den Status eines Datenbankvorgangs ab. |
> | Action | Microsoft.Sql/servers/databases/operations/cancel/action | Bricht einen ausstehenden asynchronen Vorgang in Azure SQL-Datenbank ab, der noch nicht abgeschlossen ist. |
> | Action | Microsoft.Sql/servers/databases/operations/read | Gibt die Liste der Vorgänge zurück, die für die Datenbank ausgeführt werden. |
> | Action | Microsoft.Sql/servers/databases/pause/action | Hält die Azure SQL Data Warehouse-Datenbank an. |
> | Action | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Datenbanken ab. |
> | Action | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken. |
> | Action | Microsoft.Sql/servers/databases/queryStore/queryTexts/read | Gibt die Collection von Abfragetexten zurück, die den angegebenen Parametern entsprechen. |
> | Action | Microsoft.Sql/servers/databases/queryStore/read | Gibt die aktuellen Werte von Abfragespeichereinstellungen für die Datenbank zurück. |
> | Action | Microsoft.Sql/servers/databases/queryStore/write | Aktualisiert die Abfragespeichereinstellung für die Datenbank. |
> | Action | Microsoft.Sql/servers/databases/read | Gibt die Liste der Datenbanken zurück oder ruft die Eigenschaften für die angegebene Datenbank ab. |
> | Action | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/read | Listet die Vertraulichkeitsbezeichnungen einer bestimmten Datenbank auf. |
> | Action | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/write | Batchupdate von empfohlenen Vertraulichkeitsbezeichnungen |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/delete | Dient zum Erzwingen der Beendigung der Replikationsbeziehung mit potenziellem Datenverlust. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/failover/action | Dient zum Ausführen eines Failovers nach der Synchronisierung aller Änderungen der primären Datenbank. Dadurch wird diese Datenbank zur primären Datenbank der Replikationsbeziehung, und die primäre Remotedatenbank wird zur sekundären Datenbank. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action | Dient zum Ausführen eines sofortigen Failovers mit potenziellem Datenverlust. Dadurch wird diese Datenbank zur primären Datenbank der Replikationsbeziehung, und die primäre Remotedatenbank wird zur sekundären Datenbank. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/read | Gibt die Liste mit den Replikationslinks zurück oder ruft die Eigenschaften für die angegebenen Replikationslinks ab. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/unlink/action | Dient zum Beenden der Replikationsbeziehung – entweder erzwungen oder nach der Synchronisierung mit dem Partner. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/updateReplicationMode/action | Dient zum Aktualisieren des Replikationsmodus für die Verknüpfung auf den synchronen oder asynchronen Modus. |
> | Action | Microsoft.Sql/servers/databases/restorePoints/action | Erstellt einen neuen Wiederherstellungspunkt. |
> | Action | Microsoft.Sql/servers/databases/restorePoints/delete | Löscht den Wiederherstellungspunkt der Datenbank. |
> | Action | Microsoft.Sql/servers/databases/restorePoints/read | Gibt Wiederherstellungspunkte für die Datenbank zurück. |
> | Action | Microsoft.Sql/servers/databases/resume/action | Setzt die Azure SQL Data Warehouse-Datenbank fort. |
> | Action | Microsoft.Sql/servers/databases/schemas/read | Ruft ein Datenbankschema ab (nur Schema). |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Ruft eine Datenbankspalte ab (nur Schema). |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/delete | Löscht die Vertraulichkeitsbezeichnung einer bestimmten Spalte. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/disable/action | Deaktiviert die Vertraulichkeitsempfehlungen für eine bestimmte Spalte. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/enable/action | Aktiviert die Vertraulichkeitsempfehlungen für eine bestimmte Spalte. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/read | Ruft die Vertraulichkeitsbezeichnung einer bestimmten Spalte ab. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/write | Erstellt oder aktualisiert die Vertraulichkeitsbezeichnung einer bestimmten Spalte. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/read | Ruft eine Datenbanktabelle ab (nur Schema). |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/read | Dient zum Abrufen einer Liste mit Indexempfehlungen für eine Datenbank. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/write | Dient zum Anwenden von Indexempfehlungen. |
> | Action | Microsoft.Sql/servers/databases/securityAlertPolicies/read | Ruft eine Liste der Bedrohungserkennungrichtlinien für Datenbanken ab, die für einen bestimmten Server konfiguriert sind. |
> | Action | Microsoft.Sql/servers/databases/securityAlertPolicies/write | Ändert die Bedrohungserkennungsrichtlinie für eine angegebene Datenbank. |
> | Action | Microsoft.Sql/servers/databases/securityMetrics/read | Ruft eine Collection von Metriken für Datenbanksicherheit ab. |
> | Action | Microsoft.Sql/servers/databases/sensitivityLabels/read | Listet die Vertraulichkeitsbezeichnungen einer bestimmten Datenbank auf. |
> | Action | Microsoft.Sql/servers/databases/serviceTierAdvisors/read | Gibt Vorschläge zum zentralen Hoch- oder Herunterskalieren von Datenbanken auf der Grundlage von Abfrageausführungsstatistiken zurück, um die Leistung zu verbessern oder die Kosten zu verringern. |
> | Action | Microsoft.Sql/servers/databases/skus/read | Ruft eine Collection von SKUs ab, die für eine Datenbank verfügbar ist. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/cancelSync/action | Bricht die Synchronisierung von Synchronisierungsgruppen ab. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/delete | Löscht eine vorhandene Synchronisierungsgruppe. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/hubSchemas/read | Gibt die Liste der Datenbankschemas für Synchronisierungshubs zurück. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/logs/read | Gibt die Liste der Synchronisierungsgruppenprotokolle zurück. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/read | Gibt die Liste der Synchronisierungsgruppen zurück oder ruft die Eigenschaften für die angegebene Synchronisierungsgruppe ab. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchema/action | Aktualisiert das Datenbankschema des Synchronisierungshubs. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchemaOperationResults/read | Ruft das Ergebnis des Aktualisierungsvorgangs für das Schema des Synchronisierungshubs ab. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/delete | Löscht ein vorhandenes Synchronisierungsmitglied. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/read | Gibt die Liste der Synchronisierungsmitglieder zurück oder ruft die Eigenschaften für das angegebene Synchronisierungsmitglied ab. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchema/action | Aktualisiert das Schema für das Synchronisierungsmitglied. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read | Ruft das Ergebnis des Aktualisierungsvorgangs für das Schema für das Synchronisierungsmitglied ab. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/schemas/read | Gibt die Liste von Schemas für Synchronisierungsmitglieder zurück. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/write | Erstellt ein Synchronisierungsmitglied mit den angegebenen Parametern oder aktualisiert die Eigenschaften für das angegebene Synchronisierungsmitglied. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/triggerSync/action | Löst die Synchronisierung für die Synchronisierungsgruppe aus. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/write | Erstellt eine Synchronisierungsgruppe mit den angegebenen Parametern oder aktualisiert die Eigenschaften für die angegebene Synchronisierungsgruppe. |
> | Action | Microsoft.Sql/servers/databases/topQueries/queryText/action | Gibt den Transact-SQL-Text für die ausgewählte Abfrage-ID zurück. |
> | Action | Microsoft.Sql/servers/databases/topQueries/read | Gibt aggregierte Laufzeitstatistiken für die ausgewählte Abfrage im ausgewählten Zeitraum zurück. |
> | Action | Microsoft.Sql/servers/databases/topQueries/statistics/read | Gibt aggregierte Laufzeitstatistiken für die ausgewählte Abfrage im ausgewählten Zeitraum zurück. |
> | Action | Microsoft.Sql/servers/databases/transparentDataEncryption/operationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für Transparent Data Encryption ab. |
> | Action | Microsoft.Sql/servers/databases/transparentDataEncryption/read | Dient zum Abrufen von Status und Details des Transparent Data Encryption-Sicherheitsfeatures für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/transparentDataEncryption/write | Ändert den Status von Transparent Data Encryption. |
> | Action | Microsoft.Sql/servers/databases/upgradeDataWarehouse/action | Führt ein Upgrade für die Azure SQL Data Warehouse-Datenbank durch. |
> | Action | Microsoft.Sql/servers/databases/usages/read | Ruft Informationen zur Nutzung von Azure SQL-Datenbank ab. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/delete | Entfernt die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/read | Ruft die Sicherheitsrisikobewertungs-Richtlinien für eine angegebene Datenbank ab. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/delete | Entfernt die Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/read | Ruft das Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank ab. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/write | Ändert die Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/export/action | Konvertiert ein vorhandenes Überprüfungsergebnis in ein lesbares Format. Wenn dies bereits der Fall ist, werden keine Vorgänge ausgeführt. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/initiateScan/action | Dient zum Ausführen einer Datenbanküberprüfung zur Bewertung von Sicherheitsrisiken. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/read | Gibt die Liste der Überprüfungsdatensätze für eine Datenbank-Sicherheitsrisikobewertung oder den Überprüfungsdatensatz für die angegebene Überprüfungs-ID zurück. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/write | Ändert die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/action | Dient zum Ausführen einer Datenbanküberprüfung zur Bewertung von Sicherheitsrisiken. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/operationResults/read | Ruft das Ergebnis des Ausführungsvorgangs für die Datenbank-Sicherheitsrisikobewertung ab. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/read | Dient zum Abrufen von Details zur Sicherheitsrisikobewertung, die für eine bestimmte Datenbank konfiguriert ist. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/write | Ändert die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Action | Microsoft.Sql/servers/databases/workloadGroups/delete | Löscht eine bestimmte Arbeitsauslastungsgruppe. |
> | Action | Microsoft.Sql/servers/databases/workloadGroups/read | Listet die Arbeitsauslastungsgruppen für eine ausgewählte Datenbank auf. |
> | Action | Microsoft.Sql/servers/databases/workloadGroups/workloadClassifiers/delete | Löscht einen bestimmten Arbeitsauslastungsklassifizierer. |
> | Action | Microsoft.Sql/servers/databases/workloadGroups/workloadClassifiers/read | Listet die Arbeitsauslastungsklassifizierer für eine ausgewählte Datenbank auf. |
> | Action | Microsoft.Sql/servers/databases/workloadGroups/workloadClassifiers/write | Legt die Eigenschaften für einen bestimmten Arbeitsauslastungsklassifizierer fest. |
> | Action | Microsoft.Sql/servers/databases/workloadGroups/write | Legt die Eigenschaften für eine bestimmte Arbeitsauslastungsgruppe fest. |
> | Action | Microsoft.Sql/servers/databases/write | Erstellt eine Datenbank mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Datenbank. |
> | Action | Microsoft.Sql/servers/delete | Löscht einen vorhandenen Server. |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/delete | Löscht vorhandene Notfallwiederherstellungskonfigurationen für einen bestimmten Server. |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/failover/action | Führt ein Failover für DisasterRecoveryConfiguration durch. |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action | Erzwingen Sie ein Failover für DisasterRecoveryConfiguration. |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/read | Ruft eine Collection von Notfallwiederherstellungskonfigurationen ab, die diesen Server umfassen. |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/write | Ändert die Notfallwiederherstellungskonfiguration des Servers. |
> | Action | Microsoft.Sql/servers/elasticPoolEstimates/read | Gibt die Liste mit Schätzwerten für den Pool für elastische Datenbanken zurück, die bereits für diesen Server erstellt wurden. |
> | Action | Microsoft.Sql/servers/elasticPoolEstimates/write | Erstellt für die Liste der bereitgestellten Datenbanken neue Schätzwerte für den Pool für elastische Datenbanken. |
> | Action | Microsoft.Sql/servers/elasticPools/advisors/read | Gibt eine Liste mit verfügbaren Ratgebern für den Pool für elastische Datenbanken zurück. |
> | Action | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/read | Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für den Pool für elastische Datenbanken zurück. |
> | Action | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/write | Dient zum Anwenden der empfohlenen Aktion auf den Pool für elastische Datenbanken. |
> | Action | Microsoft.Sql/servers/elasticPools/advisors/write | Dient zum Aktualisieren des Status der automatischen Ausführung eines Ratgebers auf der Ebene von Pools für elastische Datenbanken. |
> | Action | Microsoft.Sql/servers/elasticPools/databases/read | Ruft eine Liste der Datenbanken für einen Pool für elastische Datenbanken zurück. |
> | Action | Microsoft.Sql/servers/elasticPools/delete | Löscht einen vorhandenen Pool für elastische Datenbanken. |
> | Action | Microsoft.Sql/servers/elasticPools/elasticPoolActivity/read | Dient zum Abrufen von Aktivitäten und Details zu einem bestimmten Pool für elastische Datenbanken. |
> | Action | Microsoft.Sql/servers/elasticPools/elasticPoolDatabaseActivity/read | Dient zum Abrufen von Aktivitäten und Details zu einer bestimmten Datenbank, die einem Pool für elastische Datenbanken angehört. |
> | Action | Microsoft.Sql/servers/elasticPools/failover/action | Vom Kunden initiiertes Failover eines Pools für elastische Datenbanken |
> | Action | Microsoft.Sql/servers/elasticPools/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Pools für elastische Datenbanken. |
> | Action | Microsoft.Sql/servers/elasticPools/metrics/read | Gibt Metriken für Pools für elastische Datenbanken zurück. |
> | Action | Microsoft.Sql/servers/elasticPools/operations/cancel/action | Bricht einen ausstehenden asynchronen Vorgang in einem Pool für elastische Azure SQL-Datenbanken ab, der noch nicht abgeschlossen ist. |
> | Action | Microsoft.Sql/servers/elasticPools/operations/read | Gibt die Liste der Vorgänge zurück, die für den Pool für elastische Datenbanken ausgeführt werden. |
> | Action | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Action | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Action | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Pools für elastische Datenbanken. |
> | Action | Microsoft.Sql/servers/elasticPools/read | Dient zum Abrufen von Details zu Pools für elastische Datenbanken auf einem bestimmten Server. |
> | Action | Microsoft.Sql/servers/elasticPools/skus/read | Ruft eine Collection von SKUs ab, die für einen Pool für elastische Datenbanken verfügbar ist. |
> | Action | Microsoft.Sql/servers/elasticPools/write | Dient zum Erstellen eines neuen Pools für elastische Datenbanken oder zum Ändern von Eigenschaften vorhandener Pools für elastische Datenbanken. |
> | Action | Microsoft.Sql/servers/encryptionProtector/read | Gibt eine Liste von Schutzvorrichtungen zur Serververschlüsselung zurück oder ruft die Eigenschaften für die angegebene Schutzvorrichtung zur Serververschlüsselung ab. |
> | Action | Microsoft.Sql/servers/encryptionProtector/revalidate/action | Aktualisiert die Eigenschaften für die angegebene Schutzvorrichtung zur Serververschlüsselung. |
> | Action | Microsoft.Sql/servers/encryptionProtector/write | Aktualisiert die Eigenschaften für die angegebene Schutzvorrichtung zur Serververschlüsselung. |
> | Action | Microsoft.Sql/servers/extendedAuditingSettings/read | Dient zum Abrufen von Details zur Richtlinie für die erweiterte Serverblobüberwachung, die für einen bestimmten Server konfiguriert ist. |
> | Action | Microsoft.Sql/servers/extendedAuditingSettings/write | Dient zum Ändern der erweiterten Serverblobüberwachung für einen bestimmten Server. |
> | Action | Microsoft.Sql/servers/failoverGroups/delete | Löscht eine vorhandene Failovergruppe. |
> | Action | Microsoft.Sql/servers/failoverGroups/failover/action | Führt das geplante Failover in einer vorhandenen Failovergruppe durch. |
> | Action | Microsoft.Sql/servers/failoverGroups/forceFailoverAllowDataLoss/action | Führt ein erzwungenes Failover in einer vorhandenen Failovergruppe aus. |
> | Action | Microsoft.Sql/servers/failoverGroups/read | Gibt die Liste der Failovergruppen zurück oder ruft die Eigenschaften für die angegebene Failovergruppe ab. |
> | Action | Microsoft.Sql/servers/failoverGroups/write | Erstellt eine Failovergruppe mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Failovergruppe. |
> | Action | Microsoft.Sql/servers/firewallRules/delete | Löscht eine vorhandene Serverfirewallregel. |
> | Action | Microsoft.Sql/servers/firewallRules/read | Gibt die Liste der Serverfirewallregeln zurück oder ruft die Eigenschaften für die angegebene Serverfirewallregel ab. |
> | Action | Microsoft.Sql/servers/firewallRules/write | Erstellt eine Serverfirewallregel mit den angegebenen Parametern, aktualisiert die Eigenschaften für die angegebene Regel oder überschreibt alle vorhandenen Regeln durch neue Serverfirewallregel. |
> | Action | Microsoft.Sql/servers/import/action | Dient zum Erstellen einer neuen Datenbank auf dem Server und zum Bereitstellen von Schema und Daten aus einem DACPAC-Paket. |
> | Action | Microsoft.Sql/servers/importExportOperationResults/read | Ruft gegenwärtig ausgeführte Import-/Exportvorgänge ab. |
> | Action | Microsoft.Sql/servers/inaccessibleDatabases/read | Gibt eine Liste mit den nicht zugänglichen Datenbanken auf einem logischen Server zurück. |
> | Action | Microsoft.Sql/servers/interfaceEndpointProfiles/delete | Löscht das angegebene Schnittstellenendpunktprofil. |
> | Action | Microsoft.Sql/servers/interfaceEndpointProfiles/read | Gibt die Eigenschaften für das angegebene Schnittstellenendpunktprofil zurück. |
> | Action | Microsoft.Sql/servers/interfaceEndpointProfiles/write | Erstellt ein Schnittstellenendpunktprofil mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für das angegebene Schnittstellenendpunktprofil. |
> | Action | Microsoft.Sql/servers/jobAgents/delete | Löscht einen Auftrags-Agent von Azure SQL-Datenbank. |
> | Action | Microsoft.Sql/servers/jobAgents/read | Ruft einen Auftrags-Agent von Azure SQL-Datenbank ab. |
> | Action | Microsoft.Sql/servers/jobAgents/write | Erstellt oder aktualisiert einen Auftrags-Agent von Azure SQL-Datenbank. |
> | Action | Microsoft.Sql/servers/keys/delete | Löscht einen vorhandenen Serverschlüssel. |
> | Action | Microsoft.Sql/servers/keys/read | Gibt die Liste der Serverschlüssel zurück oder ruft die Eigenschaften für den angegebenen Serverschlüssel ab. |
> | Action | Microsoft.Sql/servers/keys/write | Erstellt einen Schlüssel mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Serverschlüssel. |
> | Action | Microsoft.Sql/servers/operationResults/read | Ruft gegenwärtig ausgeführte Servervorgänge ab. |
> | Action | Microsoft.Sql/servers/operations/read | Gibt die Liste mit den Vorgängen zurück, die auf dem Server ausgeführt werden. |
> | Action | Microsoft.Sql/servers/privateEndpointConnectionProxies/delete | Löscht einen Verbindungsproxy eines privaten Endpunkts |
> | Action | Microsoft.Sql/servers/privateEndpointConnectionProxies/read | Gibt eine Liste der Verbindungsproxys eines privaten Endpunkts zurück oder ruft die Eigenschaften für den angegebenen Verbindungsproxy eines privaten Endpunkts ab |
> | Action | Microsoft.Sql/servers/privateEndpointConnectionProxies/validate/action | Überprüft den Aufruf zum Erstellen einer Verbindung mit einem privaten Endpunkt auf NRP-Seite |
> | Action | Microsoft.Sql/servers/privateEndpointConnectionProxies/write | Erstellt einen Verbindungsproxy eines privaten Endpunkts mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für den angegebenen Verbindungsproxy eines privaten Endpunkts |
> | Action | Microsoft.Sql/servers/privateEndpointConnections/delete | Löscht eine Verbindung mit einem privaten Endpunkt |
> | Action | Microsoft.Sql/servers/privateEndpointConnections/read | Gibt eine Liste der Verbindungen mit einem privaten Endpunkt zurück oder ruft die Eigenschaften für die angegebene Verbindung eines privaten Endpunkts ab |
> | Action | Microsoft.Sql/servers/privateEndpointConnections/write | Genehmigt eine vorhandene Verbindung mit einem privaten Endpunkt, oder lehnt diese ab |
> | Action | Microsoft.Sql/servers/privateEndpointConnectionsApproval/action | Bestimmt, ob der Benutzer eine private Endpunktverbindung genehmigen darf. |
> | Action | Microsoft.Sql/servers/privateLinkResources/read | Ruft die Ressourcen der privaten Verknüpfung für die entsprechende SQL Server-Instanz ab |
> | Action | Microsoft.Sql/servers/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriken für Server. |
> | Action | Microsoft.Sql/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Action | Microsoft.Sql/servers/recommendedElasticPools/databases/read | Dient zum Abrufen von Metriken für empfohlene Pools für elastische Datenbanken für einen bestimmten Server. |
> | Action | Microsoft.Sql/servers/recommendedElasticPools/read | Dient zum Abrufen von Empfehlungen für Pools für elastische Datenbanken, um Kosten zu senken oder die Leistung zu verbessern. Die Empfehlungen basieren auf der Ressourcenverwendung in der Vergangenheit. |
> | Action | Microsoft.Sql/servers/recoverableDatabases/read | Vorgang für die Notfallwiederherstellung der Livedatenbank, um die Datenbank auf der Grundlage des letzten als funktionierend bekannten Sicherungspunkts wiederherzustellen. Gibt Informationen zur letzten als funktionierend bekannten Sicherung zurück, stellt die Datenbank aber nicht wieder her. |
> | Action | Microsoft.Sql/servers/replicationLinks/read | Gibt die Liste mit den Replikationslinks zurück oder ruft die Eigenschaften für die angegebenen Replikationslinks ab. |
> | Action | Microsoft.Sql/servers/restorableDroppedDatabases/read | Dient zum Abrufen einer Liste mit Datenbanken, die auf einem bestimmten Server gelöscht wurden und weiterhin in einer Aufbewahrungsrichtlinie enthalten sind. |
> | Action | Microsoft.Sql/servers/securityAlertPolicies/operationResults/read | Dient zum Abrufen von Ergebnissen des Schreibvorgangs der Richtlinie für die Serverbedrohungserkennung. |
> | Action | Microsoft.Sql/servers/securityAlertPolicies/read | Ruft eine Liste der Bedrohungserkennungrichtlinien für Server ab, die für einen bestimmten Server konfiguriert sind. |
> | Action | Microsoft.Sql/servers/securityAlertPolicies/write | Dient zum Ändern der Richtlinie für die Serverbedrohungserkennung für einen bestimmten Server. |
> | Action | Microsoft.Sql/servers/serviceObjectives/read | Dient zum Abrufen einer Liste mit verfügbaren Servicelevelzielen (auch Leistungsstufen genannt) für einen bestimmten Server. |
> | Action | Microsoft.Sql/servers/syncAgents/delete | Löscht einen vorhandenen Synchronisierungs-Agent. |
> | Action | Microsoft.Sql/servers/syncAgents/generateKey/action | Generiert einen Registrierungsschlüssel für den Synchronisierungs-Agent. |
> | Action | Microsoft.Sql/servers/syncAgents/linkedDatabases/read | Gibt die Liste der mit dem Synchronisierungs-Agent verknüpften Datenbanken zurück. |
> | Action | Microsoft.Sql/servers/syncAgents/read | Gibt die Liste der Synchronisierungs-Agents zurück oder ruft die Eigenschaften für den angegebenen Synchronisierungs-Agent ab. |
> | Action | Microsoft.Sql/servers/syncAgents/write | Erstellt einen Synchronisierungs-Agent mit den angegebenen Parametern oder aktualisiert die Eigenschaften für den angegebenen Synchronisierungs-Agent. |
> | Action | Microsoft.Sql/servers/tdeCertificates/action | Erstellt oder aktualisiert das TDE-Zertifikat. |
> | Action | Microsoft.Sql/servers/usages/read | Dient zum Zurückgeben des Server-DTU-Kontingents und des aktuellen DTU-Verbrauchs durch alle Datenbanken auf dem Server. |
> | Action | Microsoft.Sql/servers/virtualNetworkRules/delete | Löscht eine vorhandene VNET-Regel. |
> | Action | Microsoft.Sql/servers/virtualNetworkRules/read | Gibt die Liste der VNET-Regeln zurück oder ruft die Eigenschaften für die angegebene VNET-Regel ab. |
> | Action | Microsoft.Sql/servers/virtualNetworkRules/write | Erstellt eine VNET-Regel mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene VNET-Regel. |
> | Action | Microsoft.Sql/servers/vulnerabilityAssessments/delete | Entfernt die Sicherheitsrisikobewertung für einen bestimmten Server. |
> | Action | Microsoft.Sql/servers/vulnerabilityAssessments/read | Ruft die Sicherheitsrisikobewertungs-Richtlinien für einen angegebenen Server ab. |
> | Action | Microsoft.Sql/servers/vulnerabilityAssessments/write | Ändert die Sicherheitsrisikobewertung für einen bestimmten Server. |
> | Action | Microsoft.Sql/servers/write | Erstellt einen Server mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Server. |
> | Action | Microsoft.Sql/unregister/action | Hebt die Registrierung für das Abonnement für den Microsoft SQL-Datenbankressourcenanbieter auf und ermöglicht die Erstellung von Microsoft SQL-Datenbanken. |
> | Action | Microsoft.Sql/virtualClusters/delete | Löscht einen vorhandenen virtuellen Cluster. |
> | Action | Microsoft.Sql/virtualClusters/read | Gibt die Liste der Regeln für virtuelle Cluster zurück oder ruft die Eigenschaften für das angegebene virtuelle Cluster ab. |
> | Action | Microsoft.Sql/virtualClusters/write | Aktualisiert die Markierungen virtueller Cluster. |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.SqlVirtualMachine/locations/availabilityGroupListenerOperationResults/read | Ruft das Ergebnis des Vorgangs eines Verfügbarkeitsgruppenlisteners ab. |
> | Action | Microsoft.SqlVirtualMachine/locations/sqlVirtualMachineGroupOperationResults/read | Ruft das Ergebnis des Vorgangs einer Gruppe mit virtuellen SQL-Computern ab. |
> | Action | Microsoft.SqlVirtualMachine/locations/sqlVirtualMachineOperationResults/read | Ruft das Ergebnis des Vorgangs eines virtuellen SQL-Computers ab. |
> | Action | Microsoft.SqlVirtualMachine/operations/read |  |
> | Action | Microsoft.SqlVirtualMachine/register/action | Registriert das Abonnement beim Microsoft.SqlVirtualMachine-Ressourcenanbieter. |
> | Action | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/availabilityGroupListeners/delete | Löscht einen vorhandenen Verfügbarkeitsgruppenlistener. |
> | Action | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/availabilityGroupListeners/read | Ruft die Details eines SQL-Verfügbarkeitsgruppenlisteners für eine bestimmte Gruppe mit virtuellen SQL-Computern ab. |
> | Action | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/availabilityGroupListeners/write | Erstellt einen neuen SQL-Verfügbarkeitsgruppenlistener oder ändert dessen Eigenschaften. |
> | Action | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/delete | Löscht eine vorhandene Gruppe mit virtuellen SQL-Computern. |
> | Action | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/read | Ruft die Details einer Gruppe mit virtuellen SQL-Computern ab. |
> | Action | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/sqlVirtualMachines/read | Listet die virtuellen SQL-Computer einer bestimmten Gruppe mit virtuellen SQL-Computern auf. |
> | Action | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/write | Erstellt eine neue Gruppe mit virtuellen SQL-Computern oder ändert deren Eigenschaften. |
> | Action | Microsoft.SqlVirtualMachine/sqlVirtualMachines/delete | Löscht einen vorhandenen virtuellen SQL-Computer. |
> | Action | Microsoft.SqlVirtualMachine/sqlVirtualMachines/read | Ruft die Details eines virtuellen SQL-Computers ab. |
> | Action | Microsoft.SqlVirtualMachine/sqlVirtualMachines/write | Erstellt einen neuen virtuellen SQL-Computer oder ändert dessen Eigenschaften. |
> | Action | Microsoft.SqlVirtualMachine/unregister/action | Hebt die Registrierung des Abonnements beim Microsoft.SqlVirtualMachine-Ressourcenanbieter auf. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Storage/checknameavailability/read | Prüft, ob der Kontoname gültig ist und noch nicht verwendet wird. |
> | Action | Microsoft.Storage/locations/checknameavailability/read | Prüft, ob der Kontoname gültig ist und noch nicht verwendet wird. |
> | Action | Microsoft.Storage/locations/deleteVirtualNetworkOrSubnets/action | Benachrichtigt Microsoft.Storage darüber, dass das virtuelle Netzwerk oder Subnetz gelöscht wird. |
> | Action | Microsoft.Storage/locations/usages/read | Gibt den Grenzwert und den aktuellen Verwendungszähler für Ressourcen im angegebenen Abonnement zurück. |
> | Action | Microsoft.Storage/operations/read | Fragt den Status eines asynchronen Vorgangs ab. |
> | Action | Microsoft.Storage/register/action | Registriert das Abonnement für den Speicherressourcenanbieter und ermöglicht die Erstellung von Speicherkonten. |
> | Action | Microsoft.Storage/skus/read | Listet die von Microsoft.Storage unterstützten SKUs auf. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/add/action | Gibt das Ergebnis beim Hinzufügen von Blobinhalten zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Gibt das Ergebnis beim Löschen eines Blobs zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/deleteBlobVersion/action | Gibt das Ergebnis der Löschung einer Blobversion zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/filter/action | Gibt eine Liste der Blobs unter einem Konto mit entsprechendem Tagfilter zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Gibt ein Blob oder eine Liste von Blobs zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/runAsSuperUser/action | Gibt das Ergebnis des Blobbefehls zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/read | Gibt das Ergebnis beim Lesen von Blobtags zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/write | Gibt das Ergebnis beim Schreiben von Blobtags zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Gibt das Ergebnis beim Schreiben eines Blobs zurück. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/clearLegalHold/action | Löscht die rechtliche Aufbewahrungspflicht für Blobcontainer. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Gibt das Ergebnis beim Löschen eines Containers zurück. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/delete | Löscht die Unveränderlichkeitsrichtlinie für Blobcontainer. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action | Erweitert die Unveränderlichkeitsrichtlinie für Blobcontainer. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action | Sperrt die Unveränderlichkeitsrichtlinie für Blobcontainer. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/read | Ruft die Unveränderlichkeitsrichtlinie für Blobcontainer ab. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/write | Legt die Unveränderlichkeitsrichtlinie für Blobcontainer fest. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/lease/action | Gibt das Ergebnis des Vorgangs zum Leasen eines Blobcontainers zurück. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/read | Gibt einen Container zurück. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/read | Hiermit wird eine Liste von Containern zurückgegeben. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/setLegalHold/action | Legt die rechtliche Aufbewahrungspflicht für Blobcontainer fest. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/write | Gibt das Ergebnis des Patchvorgangs für den Blobcontainer zurück. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/write | Gibt das Ergebnis des PUT-Vorgangs für den Blobcontainer zurück. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Gibt einen Benutzerdelegierungsschlüssel für den Blobdienst zurück. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/read |  |
> | Action | Microsoft.Storage/storageAccounts/blobServices/read | Gibt Eigenschaften oder Statistiken des Blob-Diensts zurück. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/write | Hiermit wird das Ergebnis des Vorgangs zum Festlegen von Eigenschaften des Blob-Diensts zurückgegeben. |
> | Action | Microsoft.Storage/storageAccounts/delete | Löscht ein vorhandenes Speicherkonto. |
> | Action | Microsoft.Storage/storageAccounts/encryptionScopes/read |  |
> | Action | Microsoft.Storage/storageAccounts/encryptionScopes/read |  |
> | Action | Microsoft.Storage/storageAccounts/encryptionScopes/write |  |
> | Action | Microsoft.Storage/storageAccounts/encryptionScopes/write |  |
> | Action | Microsoft.Storage/storageAccounts/failover/action | Der Kunde kann bei Verfügbarkeitsproblemen das Failover steuern. |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/actassuperuser/action | Abrufen von Dateiadministratorberechtigungen |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete | Gibt das Ergebnis des Löschens einer Datei oder eines Ordners zurück |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action | Gibt das Ergebnis der Berechtigungsänderung für die Datei oder den Ordner zurück |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Gibt eine Datei oder einen Ordner oder eine Liste mit Dateien/Ordnern zurück |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write | Gibt das Ergebnis des Schreibens einer Datei oder des Erstellens eines Ordners zurück |
> | Action | Microsoft.Storage/storageAccounts/fileServices/read |  |
> | Action | Microsoft.Storage/storageAccounts/fileServices/read | Ruft Dateidiensteigenschaften ab. |
> | Action | Microsoft.Storage/storageAccounts/fileServices/shares/delete |  |
> | Action | Microsoft.Storage/storageAccounts/fileServices/shares/read |  |
> | Action | Microsoft.Storage/storageAccounts/fileServices/shares/read |  |
> | Action | Microsoft.Storage/storageAccounts/fileServices/shares/write |  |
> | Action | Microsoft.Storage/storageAccounts/fileServices/write |  |
> | Action | Microsoft.Storage/storageAccounts/listAccountSas/action | Gibt das Konto-SAS-Token für das angegebene Speicherkonto zurück. |
> | Action | Microsoft.Storage/storageAccounts/listkeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Action | Microsoft.Storage/storageAccounts/listServiceSas/action | Gibt das Dienst-SAS-Token für das angegebene Speicherkonto zurück. |
> | Action | Microsoft.Storage/storageAccounts/managementPolicies/delete | Löscht Verwaltungsrichtlinien für Speicherkonten. |
> | Action | Microsoft.Storage/storageAccounts/managementPolicies/read | Ruft Verwaltungsrichtlinien für Speicherkonten ab. |
> | Action | Microsoft.Storage/storageAccounts/managementPolicies/write | Legt Verwaltungsrichtlinien für Speicherkonten fest. |
> | Action | Microsoft.Storage/storageAccounts/objectReplicationPolicies/delete |  |
> | Action | Microsoft.Storage/storageAccounts/objectReplicationPolicies/read |  |
> | Action | Microsoft.Storage/storageAccounts/objectReplicationPolicies/write |  |
> | Action | Microsoft.Storage/storageAccounts/privateEndpointConnectionProxies/delete | Löscht die Proxys der Verbindung mit privatem Endpunkt |
> | Action | Microsoft.Storage/storageAccounts/privateEndpointConnectionProxies/read | Abrufen des Verbindungsproxys des privaten Endpunkts |
> | Action | Microsoft.Storage/storageAccounts/privateEndpointConnectionProxies/write | Gibt die Proxys der Verbindung mit privatem Endpunkt an |
> | Action | Microsoft.Storage/storageAccounts/privateEndpointConnections/delete | Löscht eine Verbindung mit privatem Endpunkt |
> | Action | Microsoft.Storage/storageAccounts/privateEndpointConnections/read | Ruft eine Verbindung mit privatem Endpunkt ab |
> | Action | Microsoft.Storage/storageAccounts/privateEndpointConnections/write | Platziert eine Verbindung mit privatem Endpunkt |
> | Action | Microsoft.Storage/storageAccounts/PrivateEndpointConnectionsApproval/action | Bestätigt Verbindungen mit privaten Endpunkten |
> | Action | Microsoft.Storage/storageAccounts/privateLinkResources/read | Ruft die StorageAccount-Gruppen-IDs ab |
> | Action | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Gibt das Ergebnis beim Löschen einer Warteschlange zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Gibt das Ergebnis beim Hinzufügen einer Nachricht zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Gibt das Ergebnis beim Löschen einer Nachricht zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Gibt das Ergebnis beim Verarbeiten einer Nachricht zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Gibt eine Nachricht zurück. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Gibt das Ergebnis beim Schreiben einer Nachricht zurück. |
> | Action | Microsoft.Storage/storageAccounts/queueServices/queues/read | Gibt eine Warteschlange oder eine Liste von Warteschlangen zurück. |
> | Action | Microsoft.Storage/storageAccounts/queueServices/queues/write | Gibt das Ergebnis beim Schreiben einer Warteschlange zurück. |
> | Action | Microsoft.Storage/storageAccounts/queueServices/read | Ruft Eigenschaften des Warteschlangendiensts ab. |
> | Action | Microsoft.Storage/storageAccounts/queueServices/read | Gibt Eigenschaften oder Statistiken des Warteschlangendiensts zurück. |
> | Action | Microsoft.Storage/storageAccounts/queueServices/write | Gibt das Ergebnis beim Festlegen von Eigenschaften des Warteschlangendiensts zurück. |
> | Action | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Action | Microsoft.Storage/storageAccounts/regeneratekey/action | Generiert die Zugriffsschlüssel für das angegebene Speicherkonto neu. |
> | Action | Microsoft.Storage/storageAccounts/restoreBlobRanges/action | Stellt Blobbereiche auf den Status der angegebenen Zeit wieder her |
> | Action | Microsoft.Storage/storageAccounts/revokeUserDelegationKeys/action | Widerruft alle Benutzerdelegierungsschlüssel für das angegebene Speicherkonto. |
> | Action | Microsoft.Storage/storageAccounts/services/diagnosticSettings/write | Dient zum Erstellen/Aktualisieren von Speicherkonto-Diagnoseeinstellungen. |
> | Action | Microsoft.Storage/storageAccounts/tableServices/read | Ruft Eigenschaften des Tabellenspeicherdiensts ab. |
> | Action | Microsoft.Storage/storageAccounts/write | Erstellt ein Speicherkonto mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags oder fügt eine benutzerdefinierte Domäne zum angegebenen Speicherkonto hinzu. |
> | Action | Microsoft.Storage/usages/read | Gibt den Grenzwert und den aktuellen Verwendungszähler für Ressourcen im angegebenen Abonnement zurück. |

## <a name="microsoftstoragesync"></a>microsoft.storagesync

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | microsoft.storagesync/locations/checkNameAvailability/action | Prüft, ob der Name des Speichersynchronisierungsdiensts gültig ist und noch nicht verwendet wird. |
> | Action | microsoft.storagesync/locations/workflows/operations/read | Ruft den Status eines asynchronen Vorgangs ab. |
> | Action | microsoft.storagesync/operations/read | Ruft eine Liste der unterstützten Vorgänge ab. |
> | Action | microsoft.storagesync/register/action | Registriert das Abonnement für den Speichersynchronisierungsanbieter. |
> | Action | microsoft.storagesync/storageSyncServices/delete | Löscht Speichersynchronisierungsdienste. |
> | Action | microsoft.storagesync/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Speichersynchronisierungsdienste ab. |
> | Action | microsoft.storagesync/storageSyncServices/read | Liest Speichersynchronisierungsdienste. |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/delete | Löscht registrierte Server. |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für den registrierten Server ab. |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/read | Liest registrierte Server. |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/write | Erstellt oder aktualisiert registrierte Server. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/delete | Löscht Cloudendpunkte. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read | Ruft den Status eines asynchronen Sicherungs-/Wiederherstellungsvorgangs ab. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action | Ruft diese Aktion nach der Sicherung auf. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action | Ruft diese Aktion nach der Wiederherstellung auf. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action | Ruft diese Aktion vor der Sicherung auf. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action | Ruft diese Aktion vor der Wiederherstellung auf. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/read | Liest Cloudendpunkte. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action | Stellt den Heartbeat wieder her. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/triggerChangeDetection/action | Rufen Sie diese Aktion auf, um die Erkennung von Änderungen auf der Dateifreigabe eines Cloudendpunkts auszulösen. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/write | Hiermit erstellen oder aktualisieren Sie Cloudendpunkte. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/delete | Löscht Synchronisierungsgruppen. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Synchronisierungsgruppen ab. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/read | Liest Synchronisierungsgruppen. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/delete | Löscht Serverendpunkte. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Serverendpunkte ab. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/read | Liest Serverendpunkte. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/recallAction/action | Ruft diese Aktion auf, um Dateien auf einem Server abzurufen. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/write | Hiermit erstellen oder aktualisieren Sie Serverendpunkte. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/write | Hiermit erstellen oder aktualisieren Sie Synchronisierungsgruppen. |
> | Action | microsoft.storagesync/storageSyncServices/workflows/operationresults/read | Ruft den Status eines asynchronen Vorgangs ab. |
> | Action | microsoft.storagesync/storageSyncServices/workflows/operations/read | Ruft den Status eines asynchronen Vorgangs ab. |
> | Action | microsoft.storagesync/storageSyncServices/workflows/read | Dient zum Lesen von Workflows. |
> | Action | microsoft.storagesync/storageSyncServices/write | Erstellt oder aktualisiert Speichersynchronisierungsdienste. |
> | Action | microsoft.storagesync/unregister/action | Hebt die Registrierung des Abonnements für den Speichersynchronisierungsanbieter auf. |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.StorSimple/managers/accessControlRecords/delete | Löscht die Access Control-Datensätze. |
> | Action | Microsoft.StorSimple/managers/accessControlRecords/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/accessControlRecords/read | Listet die Access Control-Datensätze auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/accessControlRecords/write | Dient zum Erstellen oder Aktualisieren der Access Control-Datensätze. |
> | Action | Microsoft.StorSimple/managers/alerts/read | Listet die Warnungen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/backups/read | Listet den Sicherungssatz auf oder ruft ihn ab. |
> | Action | Microsoft.StorSimple/managers/bandwidthSettings/delete | Löscht vorhandene Bandbreiteneinstellungen (nur 8000er Serie). |
> | Action | Microsoft.StorSimple/managers/bandwidthSettings/operationResults/read | Dient zum Auflisten der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/bandwidthSettings/read | Listet die Bandbreiteneinstellungen auf (nur 8000er Serie). |
> | Action | Microsoft.StorSimple/managers/bandwidthSettings/write | Erstellt eine neue Bandbreiteneinstellung oder aktualisiert Bandbreiteneinstellungen (nur 8000er Serie). |
> | Action | Microsoft.StorSimple/managers/certificates/write | Dient zum Erstellen oder Aktualisieren der Zertifikate. |
> | Action | Microsoft.StorSimple/Managers/certificates/write | Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen. |
> | Action | Microsoft.StorSimple/managers/clearAlerts/action | Dient zum Deaktivieren aller Warnungen im Zusammenhang mit dem Geräte-Manager. |
> | Action | Microsoft.StorSimple/managers/cloudApplianceConfigurations/read | Dient zum Auflisten der vom Cloudgerät unterstützten Konfigurationen. |
> | Action | Microsoft.StorSimple/managers/configureDevice/action | Konfiguriert ein Gerät. |
> | Action | Microsoft.StorSimple/managers/delete | Löscht die Geräte-Manager. |
> | Action | Microsoft.StorSimple/Managers/delete | Der Vorgang „Tresor löschen“ löscht die angegebene Azure-Ressource vom Typ „Tresor“. |
> | Action | Microsoft.StorSimple/managers/devices/alertSettings/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/alertSettings/read | Listet die Warnungseinstellungen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/alertSettings/write | Dient zum Erstellen oder Aktualisieren der Warnungseinstellungen. |
> | Action | Microsoft.StorSimple/managers/devices/authorizeForServiceEncryptionKeyRollover/action | Dient zum Autorisieren des Rollovers des Dienstverschlüsselungsschlüssels von Geräten. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/backup/action | Dient zum Erstellen einer manuellen Sicherung, um eine bedarfsgesteuerte Sicherung aller durch die Richtlinie geschützten Volumes zu erstellen. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/delete | Löscht vorhandene Sicherungsrichtlinien (nur 8000er Serie). |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/operationResults/read | Dient zum Auflisten der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/read | Dient zum Auflisten der Sicherungsrichtlinien (nur 8000er Serie). |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/delete | Löscht vorhandene Zeitpläne. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/operationResults/read | Dient zum Auflisten der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/read | Dient zum Auflisten der Zeitpläne. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/write | Erstellt einen neuen Zeitplan oder aktualisiert Zeitpläne. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/write | Erstellt eine neue Sicherungsrichtlinie oder aktualisiert vorhandene Sicherungsrichtlinien (nur 8000er Serie). |
> | Action | Microsoft.StorSimple/managers/devices/backups/delete | Löscht den Sicherungssatz. |
> | Action | Microsoft.StorSimple/managers/devices/backups/elements/clone/action | Dient zum Klonen einer Freigabe oder eines Volumes unter Verwendung eines Sicherungselements. |
> | Action | Microsoft.StorSimple/managers/devices/backups/elements/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/backups/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/backups/read | Listet den Sicherungssatz auf oder ruft ihn ab. |
> | Action | Microsoft.StorSimple/managers/devices/backups/restore/action | Dient zum Wiederherstellen aller Volumes aus einem Sicherungssatz. |
> | Action | Microsoft.StorSimple/managers/devices/backupScheduleGroups/delete | Löscht die Sicherungszeitplangruppen. |
> | Action | Microsoft.StorSimple/managers/devices/backupScheduleGroups/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/backupScheduleGroups/read | Listet die Sicherungszeitplangruppen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/backupScheduleGroups/write | Dient zum Erstellen oder Aktualisieren der Sicherungszeitplangruppen. |
> | Action | Microsoft.StorSimple/managers/devices/chapSettings/delete | Löscht die CHAP-Einstellungen. |
> | Action | Microsoft.StorSimple/managers/devices/chapSettings/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/chapSettings/read | Listet die CHAP-Einstellungen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/chapSettings/write | Dient zum Erstellen oder Aktualisieren der CHAP-Einstellungen. |
> | Action | Microsoft.StorSimple/managers/devices/deactivate/action | Deaktiviert ein Gerät. |
> | Action | Microsoft.StorSimple/managers/devices/delete | Löscht die Geräte. |
> | Action | Microsoft.StorSimple/managers/devices/disks/read | Listet die Datenträger auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/download/action | Dient zum Herunterladen von Updates für ein Gerät. |
> | Action | Microsoft.StorSimple/managers/devices/failover/action | Dient zum Ausführen eines Failovers für das Gerät. |
> | Action | Microsoft.StorSimple/managers/devices/failover/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/failoverTargets/read | Dient zum Auflisten oder Abrufen von Failoverzielen der Geräte. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/backup/action | Dient zum Erstellen einer Sicherung eines Dateiservers. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/delete | Löscht die Dateiserver. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/read | Listet die Dateiserver auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/delete | Löscht die Freigaben. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/read | Listet die Freigaben auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/write | Dient zum Erstellen oder Aktualisieren der Freigaben. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/write | Dient zum Erstellen oder Aktualisieren der Dateiserver. |
> | Action | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/changeControllerPowerState/action | Dient zum Ändern des Controllerenergiezustands von Hardwarekomponentengruppen. |
> | Action | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/operationResults/read | Dient zum Auflisten der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/read | Dient zum Auflisten der Hardwarekomponentengruppen. |
> | Action | Microsoft.StorSimple/managers/devices/install/action | Dient zum Installieren von Updates auf einem Gerät. |
> | Action | Microsoft.StorSimple/managers/devices/installUpdates/action | Installiert Updates auf den Geräten (nur 8000er Serie). |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/backup/action | Dient zum Erstellen einer Sicherung eines iSCSI-Servers. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/delete | Löscht die iSCSI-Server. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/delete | Löscht die Datenträger. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/read | Listet die Datenträger auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/write | Dient zum Erstellen oder Aktualisieren der Datenträger. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/read | Listet die iSCSI-Server auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/write | Dient zum Erstellen oder Aktualisieren der iSCSI-Server. |
> | Action | Microsoft.StorSimple/managers/devices/jobs/cancel/action | Dient zum Abbrechen eines ausgeführten Auftrags. |
> | Action | Microsoft.StorSimple/managers/devices/jobs/operationResults/read | Dient zum Auflisten der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/jobs/read | Listet die Aufträge auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/listFailoverSets/action | Dient zum Auflisten der Failovergruppen für ein vorhandenes Gerät (nur 8000-er Serie). |
> | Action | Microsoft.StorSimple/managers/devices/listFailoverTargets/action | Dient zum Auflisten von Failoverzielen der Geräte (nur 8000-er Serie). |
> | Action | Microsoft.StorSimple/managers/devices/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigration/action | Bestätigt eine erfolgreiche Migration und committet sie. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigrationStatus/read | Dient zum Auflisten der Bestätigung des Migrationsstatus. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action | Dient zum Abrufen des Migrationsbestätigungsstatus. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action | Dient zum Abrufen des Status für den Migrationsschätzauftrag. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action | Dient zum Abrufen des Status für die Migration. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/import/action | Dient zum Importieren von Quellkonfigurationen für die Migration. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationEstimate/read | Dient zum Auflisten der Schätzung für die Migration. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationStatus/read | Dient zum Auflisten des Migrationstatus. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/operationResults/read | Dient zum Auflisten der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigration/action | Dient zum Starten der Migration unter Verwendung von Quellkonfigurationen. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action | Dient zum Starten eines Auftrags zur Ermittlung der voraussichtlichen Dauer des Migrationsvorgangs. |
> | Action | Microsoft.StorSimple/managers/devices/networkSettings/operationResults/read | Dient zum Auflisten der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/networkSettings/read | Listet die Netzwerkeinstellungen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/networkSettings/write | Erstellt eine neue Netzwerkeinstellung oder aktualisiert Netzwerkeinstellungen. |
> | Action | Microsoft.StorSimple/managers/devices/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/publicEncryptionKey/action | Dient zum Auflisten öffentlicher Verschlüsselungsschlüssel des Geräte-Managers. |
> | Action | Microsoft.StorSimple/managers/devices/publishSupportPackage/action | Veröffentlicht das Supportpaket für ein vorhandenes Gerät. Ein StorSimple-Unterstützungspaket ist ein einfach zu verwendender Mechanismus, der alle relevanten Protokolle zur Unterstützung des Microsoft Supports bei der Behandlung von Problemen mit StorSimple-Geräten erfasst. |
> | Action | Microsoft.StorSimple/managers/devices/read | Listet die Geräte auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/scanForUpdates/action | Dient zum Suchen nach Updates auf einem Gerät. |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/read | Dient zum Auflisten der Sicherheitseinstellungen. |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/syncRemoteManagementCertificate/action | Dient zum Synchronisieren des Remoteverwaltungszertifikats für ein Gerät. |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/update/action | Dient zum Aktualisieren der Sicherheitseinstellungen. |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/write | Erstellt eine neue Sicherheitseinstellung oder aktualisiert Sicherheitseinstellungen. |
> | Action | Microsoft.StorSimple/managers/devices/sendTestAlertEmail/action | Dient zum Senden einer Testwarnungs-E-Mail an konfigurierte E-Mail-Empfänger. |
> | Action | Microsoft.StorSimple/managers/devices/shares/read | Listet die Freigaben auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/timeSettings/operationResults/read | Dient zum Auflisten der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/timeSettings/read | Listet die Uhrzeiteinstellungen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/timeSettings/write | Erstellt eine neue Uhrzeiteinstellung oder aktualisiert Uhrzeiteinstellungen. |
> | Action | Microsoft.StorSimple/managers/devices/updates/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/updateSummary/read | Listet die Updatezusammenfassung auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/delete | Löscht vorhandene Volumecontainer (nur 8000er Serie). |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/metrics/read | Dient zum Auflisten der Metriken. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/metricsDefinitions/read | Dient zum Auflisten der Metrikdefinitionen. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/operationResults/read | Dient zum Auflisten der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/read | Dient zum Auflisten der Volumecontainer (nur 8000er Serie). |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/delete | Löscht vorhandene Volumes. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metrics/read | Dient zum Auflisten der Metriken. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metricsDefinitions/read | Dient zum Auflisten der Metrikdefinitionen. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/operationResults/read | Dient zum Auflisten der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/read | Dient zum Auflisten der Volumes. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/write | Erstellt ein neues Volume oder aktualisiert Volumes. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/write | Erstellt einen neuen Volumecontainer oder aktualisiert Volumecontainer (nur 8000er Serie). |
> | Action | Microsoft.StorSimple/managers/devices/volumes/read | Dient zum Auflisten der Volumes. |
> | Action | Microsoft.StorSimple/managers/devices/write | Dient zum Erstellen oder Aktualisieren der Geräte. |
> | Action | Microsoft.StorSimple/managers/encryptionSettings/read | Listet die Verschlüsselungseinstellungen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/extendedInformation/delete | Löscht die erweiterten Tresorinformationen. |
> | Action | Microsoft.StorSimple/Managers/extendedInformation/delete | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Action | Microsoft.StorSimple/managers/extendedInformation/read | Dient zum Auflisten oder Abrufen der erweiterten Tresorinformationen. |
> | Action | Microsoft.StorSimple/Managers/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Action | Microsoft.StorSimple/managers/extendedInformation/write | Dient zum Erstellen oder Aktualisieren der erweiterten Tresorinformationen. |
> | Action | Microsoft.StorSimple/Managers/extendedInformation/write | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Action | Microsoft.StorSimple/managers/features/read | Listet die Features auf. |
> | Action | Microsoft.StorSimple/managers/fileservers/read | Listet die Dateiserver auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/getEncryptionKey/action | Dient zum Abrufen des Verschlüsselungsschlüssels für den Geräte-Manager. |
> | Action | Microsoft.StorSimple/managers/iscsiservers/read | Listet die iSCSI-Server auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/jobs/read | Listet die Aufträge auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/listActivationKey/action | Ruft den Aktivierungsschlüssel des StorSimple-Geräte-Managers ab. |
> | Action | Microsoft.StorSimple/managers/listPublicEncryptionKey/action | Dient zum Auflisten öffentlicher Verschlüsselungsschlüssel eines StorSimple-Geräte-Managers. |
> | Action | Microsoft.StorSimple/managers/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/migrateClassicToResourceManager/action | Migrieren von klassischen Bereitstellungen zu Resource Manager-Bereitstellungen |
> | Action | Microsoft.StorSimple/managers/migrationSourceConfigurations/read | Listet die Migrationsquellkonfigurationen auf (nur 8000er Serie). |
> | Action | Microsoft.StorSimple/managers/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/provisionCloudAppliance/action | Dient zum Erstellen eines neuen Cloudgeräts. |
> | Action | Microsoft.StorSimple/managers/read | Listet die Geräte-Manager auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/Managers/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Action | Microsoft.StorSimple/managers/regenerateActivationKey/action | Dient zum erneuten Generieren des Aktivierungsschlüssels für einen vorhandenen StorSimple-Geräte-Manager. |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/delete | Löscht die Speicherkonto-Anmeldedaten. |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/read | Listet die Speicherkonto-Anmeldedaten auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/write | Dient zum Erstellen oder Aktualisieren der Speicherkonto-Anmeldedaten. |
> | Action | Microsoft.StorSimple/managers/storageDomains/delete | Löscht die Speicherdomänen. |
> | Action | Microsoft.StorSimple/managers/storageDomains/operationResults/read | Dient zum Auflisten oder Abrufen der Vorgangsergebnisse. |
> | Action | Microsoft.StorSimple/managers/storageDomains/read | Listet die Speicherdomänen auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/managers/storageDomains/write | Dient zum Erstellen oder Aktualisieren der Speicherdomänen. |
> | Action | Microsoft.StorSimple/managers/write | Dient zum Erstellen oder Aktualisieren der Geräte-Manager. |
> | Action | Microsoft.StorSimple/Managers/write | Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“. |
> | Action | Microsoft.StorSimple/operations/read | Listet die Vorgänge auf oder ruft sie ab. |
> | Action | Microsoft.StorSimple/register/action | Registriert den Anbieter Microsoft.StorSimple |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.StreamAnalytics/locations/quotas/Read | Liest das Stream Analytics-Abonnementkontingent. |
> | Action | Microsoft.StreamAnalytics/operations/Read | Liest Stream Analytics-Vorgänge. |
> | Action | Microsoft.StreamAnalytics/Register/action | Registriert das Abonnement beim Stream Analytics-Ressourcenanbieter. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Delete | Dient zum Löschen von Stream Analytics-Aufträgen. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Delete | Löscht die Stream Analytics-Auftragsfunktion. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/operationresults/Read | Liest die Vorgangsergebnisse für die Stream Analytics-Auftragsfunktion. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Read | Liest die Stream Analytics-Auftragsfunktion. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/RetrieveDefaultDefinition/action | Ruft die Standarddefinition einer Stream Analytics-Auftragsfunktion ab. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Test/action | Testet die Stream Analytics-Auftragsfunktion. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Write | Schreibt die Stream Analytics-Auftragsfunktion. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Delete | Dient zum Löschen von Stream Analytics-Auftragseingaben. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/operationresults/Read | Liest die Vorgangsergebnisse für die Stream Analytics-Auftragseingabe. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Read | Dient zum Lesen von Stream Analytics-Auftragseingaben. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Sample/action | Stream Analytics-Auftragseingaben |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Test/action | Dient zum Testen von Stream Analytics-Auftragseingaben. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Write | Dient zum Schreiben von Stream Analytics-Auftragseingaben. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/metricdefinitions/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/operationresults/Read | Liest die Vorgangsergebnisse für den Stream Analytics-Auftrag. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/Delete | Dient zum Löschen von Stream Analytics-Auftragsausgaben. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/operationresults/Read | Liest die Vorgangsergebnisse für die Stream Analytics-Auftragsausgabe. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/Read | Dient zum Lesen von Stream Analytics-Auftragsausgaben. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/Test/action | Dient zum Testen der Stream Analytics-Auftragsausgabe. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/Write | Dient zum Schreiben von Stream Analytics-Auftragsausgaben. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Lesen der Diagnoseeinstellung. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Schreiben der Diagnoseeinstellung. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Streamingaufträge ab. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Streamingaufträge ab. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/PublishEdgePackage/action | Veröffentlicht ein Edgepaket für einen Stream Analytics-Auftrag. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Read | Dient zum Lesen von Stream Analytics-Aufträgen. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Scale/action | Skaliert einen Stream Analytics-Auftrag. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Start/action | Dient zum Starten von Stream Analytics-Aufträgen. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Stop/action | Dient zum Beenden von Stream Analytics-Aufträgen. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/transformations/Delete | Dient zum Löschen von Stream Analytics-Auftragstransformationen. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/transformations/Read | Dient zum Lesen von Stream Analytics-Auftragstransformationen. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/transformations/Write | Dient zum Schreiben von Stream Analytics-Auftragstransformationen. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Write | Dient zum Schreiben von Stream Analytics-Aufträgen. |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Subscription/cancel/action | Kündigt das Abonnement. |
> | Action | Microsoft.Subscription/CreateSubscription/action | Erstellt ein Azure-Abonnement. |
> | Action | Microsoft.Subscription/register/action | Registriert ein Abonnement bei einem Microsoft.Subscription-Ressourcenanbieter. |
> | Action | Microsoft.Subscription/rename/action | Benennt das Abonnement um. |
> | Action | Microsoft.Subscription/SubscriptionDefinitions/read | Ruft die Definition eines Azure-Abonnements in einer Verwaltungsgruppe ab. |
> | Action | Microsoft.Subscription/SubscriptionDefinitions/write | Erstellt die Definition für ein Azure-Abonnement. |

## <a name="microsoftsupport"></a>Microsoft.Support

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.Support/register/action | Führt die Registrierung beim Supportressourcenanbieter durch. |
> | Action | Microsoft.Support/supportTickets/read | Ruft Details zu Supporttickets (einschließlich Status, Schweregrad, Kontaktdetails und Kommunikation) oder die Liste mit Supporttickets (abonnementübergreifend) ab. |
> | Action | Microsoft.Support/supportTickets/write | Erstellt oder aktualisiert ein Supportticket. Sie können ein Supportticket zu technischen Problemen bzw. zu Problemen im Zusammenhang mit Abrechnung, Kontingenten oder Abonnementverwaltung erstellen. Der Schweregrad, die Kontaktinformationen und die Kommunikation für vorhandene Supporttickets können aktualisiert werden. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.TimeSeriesInsights/environments/accesspolicies/delete | Löscht die Zugriffsrichtlinie. |
> | Action | Microsoft.TimeSeriesInsights/environments/accesspolicies/read | Dient zum Abrufen der Eigenschaften einer Zugriffsrichtlinie. |
> | Action | Microsoft.TimeSeriesInsights/environments/accesspolicies/write | Erstellt eine neue Zugriffsrichtlinie für eine Umgebung oder aktualisiert eine vorhandene Zugriffsrichtlinie. |
> | Action | Microsoft.TimeSeriesInsights/environments/delete | Löscht die Umgebung. |
> | Action | Microsoft.TimeSeriesInsights/environments/eventsources/delete | Löscht die Ereignisquelle. |
> | Action | Microsoft.TimeSeriesInsights/environments/eventsources/read | Dient zum Abrufen der Eigenschaften einer Ereignisquelle. |
> | Action | Microsoft.TimeSeriesInsights/environments/eventsources/write | Erstellt eine neue Ereignisquelle für eine Umgebung oder aktualisiert eine vorhandene Ereignisquelle. |
> | Action | Microsoft.TimeSeriesInsights/environments/read | Dient zum Abrufen der Eigenschaften einer Umgebung. |
> | Action | Microsoft.TimeSeriesInsights/environments/referencedatasets/delete | Löscht das Verweisdataset. |
> | Action | Microsoft.TimeSeriesInsights/environments/referencedatasets/read | Dient zum Abrufen der Eigenschaften eines Verweisdatasets. |
> | Action | Microsoft.TimeSeriesInsights/environments/referencedatasets/write | Erstellt ein neues Verweisdataset für eine Umgebung oder aktualisiert ein vorhandenes Verweisdataset. |
> | Action | Microsoft.TimeSeriesInsights/environments/status/read | Ruft den Status der Umgebung und den Status der zugehörigen Vorgänge wie eingehende Daten ab. |
> | Action | Microsoft.TimeSeriesInsights/environments/write | Erstellt eine neue Umgebung oder aktualisiert eine vorhandene Umgebung. |
> | Action | Microsoft.TimeSeriesInsights/register/action | Registriert das Abonnement für den Time Series Insights-Ressourcenanbieter und ermöglicht die Erstellung von Time Series Insights-Umgebungen. |

## <a name="microsoftvisualstudio"></a>Microsoft.VisualStudio

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.VisualStudio/Account/Delete | Löscht ein Konto. |
> | Action | Microsoft.VisualStudio/Account/Extension/Read | Liest ein Konto bzw. eine Erweiterung. |
> | Action | Microsoft.VisualStudio/Account/Project/Read | Liest ein Konto bzw. Projekt. |
> | Action | Microsoft.VisualStudio/Account/Project/Write | Legt das Konto bzw. das Projekt fest. |
> | Action | Microsoft.VisualStudio/Account/Read | Liest ein Konto. |
> | Action | Microsoft.VisualStudio/Account/Write | Legt ein Konto fest. |
> | Action | Microsoft.VisualStudio/Extension/Delete | Löscht eine Erweiterung. |
> | Action | Microsoft.VisualStudio/Extension/Read | Liest eine Erweiterung. |
> | Action | Microsoft.VisualStudio/Extension/Write | Legt eine Erweiterung fest. |
> | Action | Microsoft.VisualStudio/Project/Delete | Löscht ein Projekt. |
> | Action | Microsoft.VisualStudio/Project/Read | Liest ein Projekt. |
> | Action | Microsoft.VisualStudio/Project/Write | Legt ein Projekt fest. |
> | Action | Microsoft.VisualStudio/Register/Action | Registriert das Azure-Abonnement beim Microsoft.VisualStudio-Anbieter. |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | microsoft.web/apimanagementaccounts/apiacls/read | Ruft die API-ACLs der API Management-Konten ab. |
> | Action | microsoft.web/apimanagementaccounts/apis/apiacls/delete | Löscht die API-ACLs für die APIs der API Management-Konten. |
> | Action | microsoft.web/apimanagementaccounts/apis/apiacls/read | Ruft die API-ACLs für die APIs der API Management-Konten ab. |
> | Action | microsoft.web/apimanagementaccounts/apis/apiacls/write | Aktualisiert die API-ACLs für die APIs der API Management-Konten. |
> | Action | microsoft.web/apimanagementaccounts/apis/connectionacls/read | Ruft die Verbindungs-ACLs für die APIs der API Management-Konten ab. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/confirmconsentcode/action | Bestätigt den Zustimmungscode für die API-Verbindungen der API Management-Konten. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/delete | Löscht die Verbindungs-ACLs für die API-Verbindungen der API Management-Konten. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/read | Ruft die Verbindungs-ACLs für die API-Verbindungen der API Management-Konten ab. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/write | Aktualisiert die Verbindungs-ACLs für die API-Verbindungen der API Management-Konten. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/delete | Löscht die API-Verbindungen der API Management-Konten. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/getconsentlinks/action | Ruft die Zustimmungslinks für die API-Verbindungen der API Management-Konten ab. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/listconnectionkeys/action | Listet die Verbindungsschlüssel für die API-Verbindungen der API Management-Konten auf. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/listsecrets/action | Listet die Geheimnisse für die API-Verbindungen der API Management-Konten auf. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/read | Ruft die API-Verbindungen der API Management-Konten ab. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/write | Aktualisiert die API-Verbindungen der API Management-Konten. |
> | Action | microsoft.web/apimanagementaccounts/apis/delete | Löscht die APIs der API Management-Konten. |
> | Action | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/delete | Löscht die lokalisierten Definitionen für die APIs der API Management-Konten. |
> | Action | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/read | Ruft die lokalisierten Definitionen für die APIs der API Management-Konten ab. |
> | Action | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/write | Aktualisiert die lokalisierten Definitionen für die APIs der API Management-Konten. |
> | Action | microsoft.web/apimanagementaccounts/apis/read | Ruft die APIs der API Management-Konten ab. |
> | Action | microsoft.web/apimanagementaccounts/apis/write | Aktualisiert die APIs der API Management-Konten. |
> | Action | microsoft.web/apimanagementaccounts/connectionacls/read | Ruft die Verbindungs-ACLs der API Management-Konten ab. |
> | Action | microsoft.web/availablestacks/read | Dient zum Abrufen verfügbarer Stapel. |
> | Action | Microsoft.Web/certificates/Delete | Dient zum Löschen eines vorhandenen Zertifikats. |
> | Action | Microsoft.Web/certificates/Read | Dient zum Abrufen der Zertifikatliste. |
> | Action | Microsoft.Web/certificates/Write | Dient zum Hinzufügen eines neuen Zertifikats oder zum Aktualisieren eines bereits vorhandenen. |
> | Action | microsoft.web/checknameavailability/read | Dient zum Prüfen, ob ein Ressourcenname verfügbar ist. |
> | Action | microsoft.web/classicmobileservices/read | Dient zum Abrufen von Mobile Services (klassisch). |
> | Action | Microsoft.Web/connectionGateways/Delete | Löscht ein Verbindungsgateway. |
> | Action | Microsoft.Web/connectionGateways/Join/Action | Verknüpft ein Verbindungsgateway. |
> | Action | Microsoft.Web/connectionGateways/ListStatus/Action | Listet den Status eines Verbindungsgateways auf. |
> | Action | Microsoft.Web/connectionGateways/Move/Action | Verschiebt ein Verbindungsgateway. |
> | Action | Microsoft.Web/connectionGateways/Read | Dient zum Abrufen der Verbindungsgatewayliste. |
> | Action | Microsoft.Web/connectionGateways/Write | Erstellt oder aktualisiert ein Verbindungsgateway. |
> | Action | microsoft.web/connections/confirmconsentcode/action | Dient zum Bestätigen des Genehmigungscodes für Verbindungen. |
> | Action | Microsoft.Web/connections/Delete | Löscht eine Verbindung. |
> | Action | Microsoft.Web/connections/Join/Action | Verknüpft eine Verbindung. |
> | Action | microsoft.web/connections/listconsentlinks/action | Dient zum Auflisten von Genehmigungslinks für Verbindungen. |
> | Action | Microsoft.Web/connections/Move/Action | Verschiebt eine Verbindung. |
> | Action | Microsoft.Web/connections/Read | Dient zum Abrufen der Verbindungsliste. |
> | Action | Microsoft.Web/connections/Write | Erstellt oder aktualisiert eine Verbindung. |
> | Action | Microsoft.Web/customApis/Delete | Löscht eine benutzerdefinierte API. |
> | Action | Microsoft.Web/customApis/extractApiDefinitionFromWsdl/Action | Extrahiert eine API-Definition aus einer WSDL-Schnittstelle. |
> | Action | Microsoft.Web/customApis/Join/Action | Verknüpft eine benutzerdefinierte API. |
> | Action | Microsoft.Web/customApis/listWsdlInterfaces/Action | Listet WSDL-Schnittstellen für eine benutzerdefinierte API auf. |
> | Action | Microsoft.Web/customApis/Move/Action | Verschiebt eine benutzerdefinierte API |
> | Action | Microsoft.Web/customApis/Read | Dient zum Abrufen einer benutzerdefinierten API. |
> | Action | Microsoft.Web/customApis/Write | Erstellt oder aktualisiert eine Verbindungs-API. |
> | Action | Microsoft.Web/deletedSites/Read | Hiermit rufen Sie die Eigenschaften einer gelöschten Web-App ab. |
> | Action | microsoft.web/deploymentlocations/read | Dient zum Abrufen von Bereitstellungsorten. |
> | Action | Microsoft.Web/geoRegions/Read | Ruft die Liste mit geografischen Regionen ab. |
> | Action | microsoft.web/hostingenvironments/capacities/read | Dient zum Abrufen von Hostingumgebungskapazitäten. |
> | Action | Microsoft.Web/hostingEnvironments/Delete | Dient zum Löschen einer App Service-Umgebung. |
> | Action | microsoft.web/hostingenvironments/detectors/read | Hiermit rufen Sie Erkennungsmodule für Hostingumgebungen ab. |
> | Action | microsoft.web/hostingenvironments/diagnostics/read | Dient zum Abrufen von Hostingumgebungsdiagnosen. |
> | Action | microsoft.web/hostingenvironments/inboundnetworkdependenciesendpoints/read | Ruft die Netzwerkendpunkte aller eingehenden Abhängigkeiten ab. |
> | Action | Microsoft.Web/hostingEnvironments/Join/Action | Tritt einer App Service-Umgebung bei. |
> | Action | microsoft.web/hostingenvironments/metricdefinitions/read | Dient zum Abrufen von Metrikdefinitionen für Hostingumgebungen. |
> | Action | microsoft.web/hostingenvironments/multirolepools/metricdefinitions/read | Dient zum Abrufen von MultiRole-Poolmetrikdefinitionen für Hostingumgebungen. |
> | Action | microsoft.web/hostingenvironments/multirolepools/metrics/read | Dient zum Abrufen von MultiRole-Poolmetriken für Hostingumgebungen. |
> | Action | Microsoft.Web/hostingEnvironments/multiRolePools/Read | Dient zum Abrufen der Eigenschaften eines FrontEnd-Pools in einer App Service-Umgebung. |
> | Action | microsoft.web/hostingenvironments/multirolepools/skus/read | Dient zum Abrufen von MultiRole-Pool-SKUs für Hostingumgebungen. |
> | Action | microsoft.web/hostingenvironments/multirolepools/usages/read | Dient zum Abrufen von MultiRole-Poolverwendungen für Hostingumgebungen. |
> | Action | Microsoft.Web/hostingEnvironments/multiRolePools/Write | Dient zum Erstellen eines neuen FrontEnd-Pools in einer App Service-Umgebung oder zum Aktualisieren eines bereits vorhandenen. |
> | Action | microsoft.web/hostingenvironments/operations/read | Dient zum Abrufen von Hostingumgebungsvorgängen. |
> | Action | microsoft.web/hostingenvironments/outboundnetworkdependenciesendpoints/read | Ruft die Netzwerkendpunkte aller ausgehenden Abhängigkeiten ab. |
> | Action | Microsoft.Web/hostingEnvironments/PrivateEndpointConnectionsApproval/action | Bestätigt Verbindungen mit privaten Endpunkten |
> | Action | Microsoft.Web/hostingEnvironments/Read | Dient zum Abrufen der Eigenschaften einer App Service-Umgebung. |
> | Action | Microsoft.Web/hostingEnvironments/reboot/Action | Dient zum Neustarten aller Computer in einer App Service-Umgebung. |
> | Action | microsoft.web/hostingenvironments/resume/action | Dient zum Fortsetzen von Hostingumgebungen. |
> | Action | microsoft.web/hostingenvironments/serverfarms/read | Dient zum Abrufen von App Service-Plänen für Hostingumgebungen. |
> | Action | microsoft.web/hostingenvironments/sites/read | Dient zum Abrufen von Web-Apps für Hostingumgebungen. |
> | Action | microsoft.web/hostingenvironments/suspend/action | Dient zum Anhalten von Hostingumgebungen. |
> | Action | microsoft.web/hostingenvironments/usages/read | Dient zum Abrufen von Hostingumgebungsverwendungen. |
> | Action | microsoft.web/hostingenvironments/workerpools/metricdefinitions/read | Dient zum Abrufen von Workerpool-Metrikdefinitionen für Hostingumgebungen. |
> | Action | microsoft.web/hostingenvironments/workerpools/metrics/read | Dient zum Abrufen von Workerpoolmetriken für Hostingumgebungen. |
> | Action | Microsoft.Web/hostingEnvironments/workerPools/Read | Dient zum Abrufen der Eigenschaften eines Workerpools in einer App Service-Umgebung. |
> | Action | microsoft.web/hostingenvironments/workerpools/skus/read | Dient zum Abrufen von Workerpool-SKUs für Hostingumgebungen. |
> | Action | microsoft.web/hostingenvironments/workerpools/usages/read | Dient zum Abrufen von Workerpoolverwendungen für Hostingumgebungen. |
> | Action | Microsoft.Web/hostingEnvironments/workerPools/Write | Dient zum Erstellen eines neuen Workerpools in einer App Service-Umgebung oder zum Aktualisieren eines bereits vorhandenen. |
> | Action | Microsoft.Web/hostingEnvironments/Write | Dient zum Erstellen einer neuen App Service-Umgebung oder zum Aktualisieren einer bereits vorhandenen. |
> | Action | microsoft.web/ishostingenvironmentnameavailable/read | Dient zum Abrufen, ob der Hostingumgebungsname verfügbar ist. |
> | Action | microsoft.web/ishostnameavailable/read | Dient zum Prüfen, ob der Hostname verfügbar ist. |
> | Action | microsoft.web/isusernameavailable/read | Dient zum Prüfen, ob der Benutzername verfügbar ist. |
> | Action | Microsoft.Web/listSitesAssignedToHostName/Read | Dient zum Abrufen der Namen von Websites, die dem Hostnamen zugewiesen sind. |
> | Action | microsoft.web/locations/apioperations/read | Dient zum Abrufen von Standorten für API-Vorgänge. |
> | Action | microsoft.web/locations/connectiongatewayinstallations/read | Dient zum Abrufen von Standorten für Verbindungsgatewayinstallationen. |
> | Action | microsoft.web/locations/deleteVirtualNetworkOrSubnets/action | VNET- oder-Subnetzlöschbenachrichtigung für Standorte. |
> | Action | microsoft.web/locations/extractapidefinitionfromwsdl/action | Extrahiert eine API-Definition aus einer WSDL-Schnittstelle für Speicherorte. |
> | Action | microsoft.web/locations/listwsdlinterfaces/action | Listet WSDL-Schnittstellen für Speicherorte auf. |
> | Action | microsoft.web/locations/managedapis/apioperations/read | Dient zum Abrufen verwalteter API-Vorgänge. |
> | Action | Microsoft.Web/locations/managedapis/Join/Action | Verknüpft eine verwaltete API. |
> | Action | microsoft.web/locations/managedapis/read | Dient zum Abrufen verwalteter APIs für Standorte. |
> | Action | microsoft.web/locations/operationResults/read | Dient zum Abrufen von Vorgängen. |
> | Action | microsoft.web/locations/operations/read | Dient zum Abrufen von Vorgängen. |
> | Action | microsoft.web/operations/read | Dient zum Abrufen von Vorgängen. |
> | Action | microsoft.web/publishingusers/read | Dient zum Abrufen von Veröffentlichungsbenutzern. |
> | Action | microsoft.web/publishingusers/write | Dient zum Aktualisieren von Veröffentlichungsbenutzern. |
> | Action | Microsoft.Web/recommendations/Read | Dient zum Abrufen der Liste mit Empfehlungen für Abonnements. |
> | Action | microsoft.web/register/action | Dient zum Registrieren des Microsoft.Web-Ressourcenanbieters für das Abonnement. |
> | Action | microsoft.web/resourcehealthmetadata/read | Ruft Metadaten zur Ressourcenintegrität ab. |
> | Action | microsoft.web/serverfarms/capabilities/read | Dient zum Abrufen von Funktionen für App Service-Pläne. |
> | Action | Microsoft.Web/serverfarms/Delete | Löschen eines vorhandenen App Service-Plans |
> | Action | Microsoft.Web/serverfarms/eventGridFilters/delete | Löschen des Event Grid-Filters für eine Serverfarm. |
> | Action | Microsoft.Web/serverfarms/eventGridFilters/read | Abrufen des Event Grid-Filters für eine Serverfarm. |
> | Action | Microsoft.Web/serverfarms/eventGridFilters/write | Festlegen des Event Grid-Filters für eine Serverfarm. |
> | Action | microsoft.web/serverfarms/firstpartyapps/settings/delete | Dient zum Löschen von Erstanbieter-App-Einstellungen für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/firstpartyapps/settings/read | Dient zum Abrufen von Erstanbieter-App-Einstellungen für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/firstpartyapps/settings/write | Dient zum Aktualisieren von Erstanbieter-App-Einstellungen für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/delete | Dient zum Löschen von Relaynamespace-Agents für Hybridverbindungen für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/read | Dient zum Abrufen von Relaynamespace-Agents für Hybridverbindungen für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/sites/read | Dient zum Abrufen von Hybridverbindungsnamespace-Relay-Web-Apps für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/hybridconnectionplanlimits/read | Dient zum Abrufen von Hybridverbindungsplan-Grenzwerten für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/hybridconnectionrelays/read | Dient zum Abrufen von Hybridverbindungsrelays für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/metricdefinitions/read | Dient zum Abrufen von Metrikdefinitionen für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/metrics/read | Dient zum Abrufen von Metriken für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/operationresults/read | Dient zum Abrufen von Vorgangsergebnissen für App Service-Pläne. |
> | Action | Microsoft.Web/serverfarms/Read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |
> | Action | Microsoft.Web/serverfarms/restartSites/Action | Dient zum Neustarten aller Web-Apps in einem App Service-Plan. |
> | Action | microsoft.web/serverfarms/sites/read | Dient zum Abrufen von Web-Apps für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/skus/read | Dient zum Abrufen von SKUs für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/usages/read | Dient zum Abrufen von Verwendungen für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/gateways/write | Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/read | Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/routes/delete | Dient zum Löschen von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/routes/read | Dient zum Abrufen von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/routes/write | Dient zum Aktualisieren von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne. |
> | Action | microsoft.web/serverfarms/workers/reboot/action | Dient zum Neustarten von Workern für App Service-Pläne. |
> | Action | Microsoft.Web/serverfarms/Write | Dient zum Erstellen eines neuen App Service-Plans oder zum Aktualisieren eines bereits vorhandenen. |
> | Action | microsoft.web/sites/analyzecustomhostname/read | Dient zum Analysieren des benutzerdefinierten Hostnamens. |
> | Action | Microsoft.Web/sites/applySlotConfig/Action | Dient zum Anwenden der Web-App-Slotkonfiguration aus dem Zielslot auf die aktuelle Web-App. |
> | Action | Microsoft.Web/sites/backup/Action | Dient zum Erstellen einer neuen Web-App-Sicherung. |
> | Action | microsoft.web/sites/backup/read | Dient zum Abrufen der Web-App-Sicherung. |
> | Action | microsoft.web/sites/backup/write | Dient zum Aktualisieren der Web-App-Sicherung. |
> | Action | microsoft.web/sites/backups/action | Ermittelt eine vorhandene App-Sicherung, die aus einem Blob im Azure-Speicher wiederhergestellt werden kann. |
> | Action | microsoft.web/sites/backups/delete | Dient zum Löschen von Web-App-Sicherungen. |
> | Action | microsoft.web/sites/backups/list/action | Dient zum Auflisten von Web-App-Sicherungen. |
> | Action | Microsoft.Web/sites/backups/Read | Dient zum Abrufen der Eigenschaften einer Web-App-Sicherung. |
> | Action | microsoft.web/sites/backups/restore/action | Dient zum Wiederherstellen von Web-App-Sicherungen. |
> | Action | microsoft.web/sites/backups/write | Hiermit aktualisieren Sie Web-App-Sicherungen. |
> | Action | microsoft.web/sites/config/delete | Dient zum Löschen der Web-App-Konfiguration. |
> | Action | Microsoft.Web/sites/config/list/Action | Dient zum Auflisten der sicherheitsrelevanten Einstellungen der Web-App. Hierzu zählen etwa Veröffentlichungsanmeldeinformationen, App-Einstellungen und Verbindungszeichenfolgen. |
> | Action | Microsoft.Web/sites/config/Read | Dient zum Abrufen von Web-App-Konfigurationseinstellungen. |
> | Action | microsoft.web/sites/config/snapshots/read | Dient zum Abrufen von Momentaufnahmen der Web-Apps-Konfiguration. |
> | Action | Microsoft.Web/sites/config/Write | Dient zum Aktualisieren der Konfigurationseinstellungen der Web-App. |
> | Action | microsoft.web/sites/containerlogs/action | Hiermit rufen Sie gezippte Containerprotokolle für die Web-App auf. |
> | Action | microsoft.web/sites/containerlogs/download/action | Lädt Containerprotokolle von Web-Apps herunter. |
> | Action | microsoft.web/sites/continuouswebjobs/delete | Dient zum Löschen fortlaufender Webaufträge für Web-Apps. |
> | Action | microsoft.web/sites/continuouswebjobs/read | Dient zum Abrufen fortlaufender Webaufträge für Web-Apps. |
> | Action | microsoft.web/sites/continuouswebjobs/start/action | Dient zum Starten fortlaufender Webaufträge für Web-Apps. |
> | Action | microsoft.web/sites/continuouswebjobs/stop/action | Dient zum Beenden fortlaufender Webaufträge für Web-Apps. |
> | Action | Microsoft.Web/sites/Delete | Dient zum Löschen einer vorhandenen Web-App. |
> | Action | microsoft.web/sites/deployments/delete | Dient zum Löschen von Web-App-Bereitstellungen. |
> | Action | microsoft.web/sites/deployments/log/read | Dient zum Abrufen des Web-App-Bereitstellungsprotokolls. |
> | Action | microsoft.web/sites/deployments/read | Dient zum Abrufen von Web-App-Bereitstellungen. |
> | Action | microsoft.web/sites/deployments/write | Dient zum Aktualisieren von Web-App-Bereitstellungen. |
> | Action | microsoft.web/sites/detectors/read | Hiermit rufen Sie Erkennungsmodule für die Web-App auf. |
> | Action | microsoft.web/sites/diagnostics/analyses/execute/Action | Dient zum Ausführen der Analyse von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/analyses/read | Dient zum Abrufen der Analyse von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/aspnetcore/read | Ruft Web-Apps-Diagnosen für die ASP.NET Core-App ab. |
> | Action | microsoft.web/sites/diagnostics/autoheal/read | Dient zum Abrufen von AutoHeal für Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/deployment/read | Dient zum Abrufen einer Bereitstellung für Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/deployments/read | Dient zum Abrufen von Bereitstellungen für Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/detectors/execute/Action | Dient zum Ausführen der Erkennung von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/detectors/read | Dient zum Abrufen der Erkennung von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/failedrequestsperuri/read | Dient zum Abrufen fehlerhafter Anforderungen bei Web-Apps-Diagnosen pro URI. |
> | Action | microsoft.web/sites/diagnostics/frebanalysis/read | Dient zum Abrufen der FREB-Analyse von Web-App-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/loganalyzer/read | Dient zum Abrufen der Protokollanalyse von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/read | Dient zum Abrufen der Kategorien von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/runtimeavailability/read | Dient zum Abrufen der Laufzeitverfügbarkeit für Web-App-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/servicehealth/read | Dient zum Abrufen der Dienstintegrität von Web-App-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/sitecpuanalysis/read | Dient zum Abrufen der CPU-Analyse von Websites von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/sitecrashes/read | Dient zum Abrufen von Websiteabstürzen von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/sitelatency/read | Dient zum Abrufen von Websitelatenzen von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/sitememoryanalysis/read | Dient zum Abrufen der Websitespeicheranalyse von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/siterestartsettingupdate/read | Dient zum Abrufen des Websiteupdates für die Einstellungen zum Neustarten der Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/siterestartuserinitiated/read | Dient zum Abrufen des vom Benutzer initiierten Websiteneustarts der Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/siteswap/read | Dient zum Abrufen des Websiteaustauschs von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/threadcount/read | Dient zum Abrufen der Threadanzahl von Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/workeravailability/read | Dient zum Abrufen der Workerverfügbarkeit für Web-App-Diagnosen. |
> | Action | microsoft.web/sites/diagnostics/workerprocessrecycle/read | Dient zum Abrufen der Workerprozesswiederverwendung für Web-App-Diagnosen. |
> | Action | microsoft.web/sites/domainownershipidentifiers/read | Dient zum Abrufen von Domänenbesitz-IDs für Web-Apps. |
> | Action | microsoft.web/sites/domainownershipidentifiers/write | Dient zum Aktualisieren von Domänenbesitz-IDs für Web-Apps. |
> | Action | Microsoft.Web/sites/eventGridFilters/delete | Löschen des Event Grid-Filters für eine Web-App. |
> | Action | Microsoft.Web/sites/eventGridFilters/read | Abrufen des Event Grid-Filters für eine Web-App. |
> | Action | Microsoft.Web/sites/eventGridFilters/write | Festlegen des Event Grid-Filters für eine Web-App. |
> | Action | microsoft.web/sites/extensions/delete | Dient zum Löschen von Web-Apps-Websiteerweiterungen. |
> | Action | microsoft.web/sites/extensions/read | Dient zum Abrufen von Web-Apps-Websiteerweiterungen. |
> | Action | microsoft.web/sites/extensions/write | Dient zum Aktualisieren von Web-Apps-Websiteerweiterungen. |
> | Action | microsoft.web/sites/functions/action | Functions-Web-Apps |
> | Action | microsoft.web/sites/functions/delete | Dient zum Löschen von Web-App-Funktionen. |
> | Action | microsoft.web/sites/functions/keys/delete | Löschen von Funktionsschlüsseln. |
> | Action | microsoft.web/sites/functions/keys/write | Aktualisieren von Funktionsschlüsseln. |
> | Action | microsoft.web/sites/functions/listkeys/action | Auflisten von Funktionsschlüsseln. |
> | Action | microsoft.web/sites/functions/listsecrets/action | Auflisten von Funktionsgeheimnissen. |
> | Action | microsoft.web/sites/functions/masterkey/read | Dient zum Abrufen des Hauptschlüssels von Web-Apps-Funktionen. |
> | Action | microsoft.web/sites/functions/properties/read | Lesen von Web-Apps-Funktionseigenschaften. |
> | Action | microsoft.web/sites/functions/properties/write | Aktualisieren von Web-Apps-Funktionseigenschaften. |
> | Action | microsoft.web/sites/functions/read | Dient zum Abrufen von Web-App-Funktionen. |
> | Action | microsoft.web/sites/functions/token/read | Dient zum Abrufen des Funktionstokens von Web-Apps. |
> | Action | microsoft.web/sites/functions/write | Dient zum Aktualisieren von Web-App-Funktionen. |
> | Action | microsoft.web/sites/host/functionkeys/delete | Löschen von Functions-Hostfunktionsschlüsseln. |
> | Action | microsoft.web/sites/host/functionkeys/write | Aktualisieren von Functions-Hostfunktionsschlüsseln. |
> | Action | microsoft.web/sites/host/listkeys/action | Auflisten von Functions-Hostschlüsseln. |
> | Action | microsoft.web/sites/host/listsyncstatus/action | Auflisten des Status von Synchronisierungsfunktionstriggern. |
> | Action | microsoft.web/sites/host/properties/read | Lesen von Web-Apps-Functions-Hosteigenschaften. |
> | Action | microsoft.web/sites/host/sync/action | Synchronisieren von Funktionstriggern. |
> | Action | microsoft.web/sites/host/systemkeys/delete | Löschen von Functions-Hostsystemschlüsseln. |
> | Action | microsoft.web/sites/host/systemkeys/write | Aktualisieren von Functions-Hostsystemschlüsseln. |
> | Action | microsoft.web/sites/hostnamebindings/delete | Dient zum Löschen von Hostnamenbindungen für Web-Apps. |
> | Action | microsoft.web/sites/hostnamebindings/read | Dient zum Abrufen von Hostnamenbindungen für Web-Apps. |
> | Action | microsoft.web/sites/hostnamebindings/write | Dient zum Aktualisieren von Hostnamenbindungen für Web-Apps. |
> | Action | microsoft.web/sites/hostruntime/functions/keys/read | Ruft Funktionsschlüssel der Hostruntime für Web-Apps ab. |
> | Action | Microsoft.Web/sites/hostruntime/host/_master/read | Abrufen des Hauptschlüssels der Funktionen-App für Aministratorvorgänge |
> | Action | Microsoft.Web/sites/hostruntime/host/action | Durchführen von Laufzeitaktionen für die Funktionen-App wie Synchronisieren von Triggern, Hinzufügen von Funktionen, Aufrufen von Funktionen, Löschen von Funktionen usw. |
> | Action | microsoft.web/sites/hostruntime/host/read | Ruft den Hostruntime-Host für Web-Apps ab. |
> | Action | microsoft.web/sites/hybridconnection/delete | Dient zum Löschen der Hybridverbindung für Web-Apps. |
> | Action | microsoft.web/sites/hybridconnection/read | Dient zum Abrufen der Hybridverbindung für Web-Apps. |
> | Action | microsoft.web/sites/hybridconnection/write | Dient zum Aktualisieren der Hybridverbindung für Web-Apps. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/delete | Dient zum Löschen der Relaynamespace-Agents für Hybridverbindungen für Web-Apps. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/listkeys/action | Dient zum Auflisten der Schlüssel von Relaynamespace-Agents für Hybridverbindungen für Web-Apps. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/read | Dient zum Abrufen von Relaynamespace-Agents für Hybridverbindungen für Web-Apps. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/write | Dient zum Aktualisieren des Relaynamespace-Agents für Hybridverbindungen für Web-Apps. |
> | Action | microsoft.web/sites/hybridconnectionrelays/read | Dient zum Abrufen von Hybridverbindungsrelays für Web-Apps. |
> | Action | microsoft.web/sites/instances/deployments/delete | Dient zum Löschen von Web-Apps-Instanzbereitstellungen. |
> | Action | microsoft.web/sites/instances/deployments/read | Dient zum Abrufen von Prozessen für Web-App-Bereitstellungen. |
> | Action | microsoft.web/sites/instances/extensions/log/read | Dient zum Abrufen des Web-App-Instanzerweiterungsprotokolls. |
> | Action | microsoft.web/sites/instances/extensions/processes/read | Ruft Prozesse der Erweiterungen von Web-App-Instanzen ab. |
> | Action | microsoft.web/sites/instances/extensions/read | Dient zum Abrufen der Erweiterungen von Web-App-Instanzen. |
> | Action | microsoft.web/sites/instances/processes/delete | Dient zum Löschen von Prozessen für Web-App-Instanzen. |
> | Action | microsoft.web/sites/instances/processes/modules/read | Ruft Module der Prozesse für Web-App-Instanzen ab. |
> | Action | microsoft.web/sites/instances/processes/read | Dient zum Abrufen von Prozessen für Web-App-Instanzen. |
> | Action | microsoft.web/sites/instances/processes/threads/read | Dient zum Abrufen von Prozess-Threads für Web-App-Instanzen. |
> | Action | microsoft.web/sites/instances/read | Dient zum Abrufen von Web-App-Instanzen. |
> | Action | microsoft.web/sites/listsyncfunctiontriggerstatus/action | Auflisten des Status von Synchronisierungsfunktionstriggern. |
> | Action | microsoft.web/sites/metricdefinitions/read | Dient zum Abrufen von Metrikdefinitionen für Web-Apps. |
> | Action | microsoft.web/sites/metrics/read | Dient zum Abrufen von Web-App-Metriken. |
> | Action | microsoft.web/sites/metricsdefinitions/read | Dient zum Abrufen von Metrikdefinitionen für Web-Apps. |
> | Action | microsoft.web/sites/migratemysql/action | Dient zum Durchführen von Migrationen mithilfe von MySql und Web-Apps. |
> | Action | microsoft.web/sites/migratemysql/read | Dient zum Abrufen von Web-Apps für die Migration mit MySql. |
> | Action | microsoft.web/sites/networktrace/action | Netzwerkablaufverfolgungs-Web-Apps |
> | Action | microsoft.web/sites/networktraces/operationresults/read | Ruft Vorgangsergebnisse für die Web-Apps-Netzwerkablaufverfolgung ab. |
> | Action | microsoft.web/sites/newpassword/action | NewPassword-Web-Apps |
> | Action | microsoft.web/sites/operationresults/read | Dient zum Abrufen der Ergebnisse von Web-App-Vorgängen. |
> | Action | microsoft.web/sites/operations/read | Dient zum Abrufen von Web-Apps-Vorgängen. |
> | Action | microsoft.web/sites/perfcounters/read | Dient zum Abrufen von Leistungsindikatoren für Web-Apps. |
> | Action | microsoft.web/sites/premieraddons/delete | Dient zum Löschen von Premier-Add-Ons für Web-Apps. |
> | Action | microsoft.web/sites/premieraddons/read | Dient zum Abrufen von Premier-Add-Ons für Web-Apps. |
> | Action | microsoft.web/sites/premieraddons/write | Dient zum Aktualisieren von Premier-Add-Ons für Web-Apps. |
> | Action | microsoft.web/sites/privateaccess/read | Abrufen von Daten für die Aktivierung des privaten Websitezugriffs und autorisierter virtueller Netzwerke, die auf die Website zugreifen können |
> | Action | Microsoft.Web/sites/PrivateEndpointConnectionsApproval/action | Bestätigt Verbindungen mit privaten Endpunkten |
> | Action | microsoft.web/sites/processes/modules/read | Dient zum Abrufen von Web-Apps-Prozessmodulen. |
> | Action | microsoft.web/sites/processes/read | Dient zum Abrufen von Web-Apps-Prozessen. |
> | Action | microsoft.web/sites/processes/threads/read | Dient zum Abrufen von Web-Apps-Prozessthreads. |
> | Action | microsoft.web/sites/publiccertificates/delete | Löscht die öffentlichen Zertifikate für Web-Apps. |
> | Action | microsoft.web/sites/publiccertificates/read | Ruft die öffentlichen Zertifikate für Web-Apps ab. |
> | Action | microsoft.web/sites/publiccertificates/write | Aktualisiert die öffentlichen Zertifikate für Web-Apps. |
> | Action | Microsoft.Web/sites/publish/Action | Dient zum Veröffentlichen einer Web-App. |
> | Action | Microsoft.Web/sites/publishxml/Action | Dient zum Abrufen des Veröffentlichungsprofils (XML) für eine Web-App. |
> | Action | microsoft.web/sites/publishxml/read | Dient zum Abrufen des XML-Codes für die Veröffentlichung von Web-Apps. |
> | Action | Microsoft.Web/sites/Read | Dient zum Abrufen der Eigenschaften einer Web-App. |
> | Action | microsoft.web/sites/recommendationhistory/read | Dient zum Abrufen des Empfehlungsverlaufs für Web-Apps. |
> | Action | microsoft.web/sites/recommendations/disable/action | Dient zum Deaktivieren von Web-App-Empfehlungen. |
> | Action | Microsoft.Web/sites/recommendations/Read | Dient zum Abrufen der Liste mit Empfehlungen für Web-Apps. |
> | Action | microsoft.web/sites/recover/action | Stellt Web-Apps wieder her. |
> | Action | Microsoft.Web/sites/resetSlotConfig/Action | Dient zum Zurücksetzen der Web-App-Konfiguration. |
> | Action | microsoft.web/sites/resourcehealthmetadata/read | Ruft Metadaten zur Ressourcenintegrität von Web-Apps ab. |
> | Action | Microsoft.Web/sites/restart/Action | Dient zum Neustarten einer Web-App. |
> | Action | microsoft.web/sites/restore/read | Dient zum Abrufen der Web-App-Wiederherstellung. |
> | Action | microsoft.web/sites/restore/write | Dient zum Wiederherstellen von Web-Apps. |
> | Action | microsoft.web/sites/restorefrombackupblob/action | Wiederherstellen einer Web-App aus dem Sicherungsblob |
> | Action | microsoft.web/sites/restorefromdeletedapp/action | Hiermit stellen Sie Web-Apps aus einer gelöschten App wieder her. |
> | Action | microsoft.web/sites/restoresnapshot/action | Stellt Momentaufnahmen von Web-Apps wieder her. |
> | Action | microsoft.web/sites/siteextensions/delete | Dient zum Löschen von Web-Apps-Websiteerweiterungen. |
> | Action | microsoft.web/sites/siteextensions/read | Dient zum Abrufen von Web-Apps-Websiteerweiterungen. |
> | Action | microsoft.web/sites/siteextensions/write | Dient zum Aktualisieren von Web-Apps-Websiteerweiterungen. |
> | Action | microsoft.web/sites/slots/analyzecustomhostname/read | Dient zum Abrufen des benutzerdefinierten Hostnamens für die Analyse von Web-Apps-Slots. |
> | Action | Microsoft.Web/sites/slots/applySlotConfig/Action | Dient zum Anwenden der Web-App-Slotkonfiguration aus dem Zielslot auf den aktuellen Slot. |
> | Action | Microsoft.Web/sites/slots/backup/Action | Dient zum Erstellen einer neuen Web-App-Slotsicherung. |
> | Action | microsoft.web/sites/slots/backup/read | Dient zum Abrufen von Sicherungen von Web-Apps-Slots. |
> | Action | microsoft.web/sites/slots/backup/write | Dient zum Aktualisieren der Web-App-Slotsicherung. |
> | Action | microsoft.web/sites/slots/backups/action | Dient zum Erkennen von Sicherungen von Web-Apps-Slots. |
> | Action | microsoft.web/sites/slots/backups/delete | Dient zum Löschen von Sicherungen von Web-Apps-Slots. |
> | Action | microsoft.web/sites/slots/backups/list/action | Dient zum Auflisten von Web-App-Slotsicherungen. |
> | Action | Microsoft.Web/sites/slots/backups/Read | Dient zum Abrufen der Eigenschaften einer Sicherung von Web-App-Slots. |
> | Action | microsoft.web/sites/slots/backups/restore/action | Dient zum Wiederherstellen von Web-App-Slotsicherungen. |
> | Action | microsoft.web/sites/slots/config/delete | Dient zum Löschen der Konfiguration von Web-App-Slots. |
> | Action | Microsoft.Web/sites/slots/config/list/Action | Dient zum Auflisten der sicherheitsrelevanten Einstellungen des Web-App-Slots. Hierzu zählen etwa Veröffentlichungsanmeldeinformationen, App-Einstellungen und Verbindungszeichenfolgen. |
> | Action | Microsoft.Web/sites/slots/config/Read | Dient zum Abrufen der Konfigurationseinstellungen des Web-App-Slots. |
> | Action | Microsoft.Web/sites/slots/config/Write | Dient zum Aktualisieren der Konfigurationseinstellungen des Web-App-Slots. |
> | Action | microsoft.web/sites/slots/containerlogs/action | Hiermit rufen Sie gezippte Containerprotokolle für den Web-App-Slot auf. |
> | Action | microsoft.web/sites/slots/containerlogs/download/action | Lädt Containerprotokolle von Web-App-Slots herunter. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/delete | Dient zum Löschen fortlaufender Webaufträge für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/read | Dient zum Abrufen fortlaufender Webaufträge für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/start/action | Dient zum Starten fortlaufender Webaufträge für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/stop/action | Dient zum Beenden fortlaufender Webaufträge für Web-App-Slots. |
> | Action | Microsoft.Web/sites/slots/Delete | Dient zum Löschen eines vorhandenen Web-App-Slots. |
> | Action | microsoft.web/sites/slots/deployments/delete | Dient zum Löschen von Web-App-Slotbereitstellungen. |
> | Action | microsoft.web/sites/slots/deployments/log/read | Dient zum Abrufen des Protokolls für Web-App-Slotbereitstellungen. |
> | Action | microsoft.web/sites/slots/deployments/read | Dient zum Abrufen von Web-App-Slotbereitstellungen. |
> | Action | microsoft.web/sites/slots/deployments/write | Dient zum Aktualisieren von Web-App-Slotbereitstellungen. |
> | Action | microsoft.web/sites/slots/detectors/read | Dient zum Abrufen von Web-App-Sloterkennungen. |
> | Action | microsoft.web/sites/slots/diagnostics/analyses/execute/Action | Dient zum Ausführen der Analyse von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/analyses/read | Dient zum Abrufen der Analyse von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/aspnetcore/read | Ruft Web-Apps-Slotdiagnosen für die ASP.NET Core-App ab. |
> | Action | microsoft.web/sites/slots/diagnostics/autoheal/read | Dient zum Abrufen von AutoHeal für Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/deployment/read | Dient zum Abrufen einer Bereitstellung für Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/deployments/read | Dient zum Abrufen von Bereitstellungen für Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/detectors/execute/Action | Dient zum Ausführen der Erkennung von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/detectors/read | Dient zum Abrufen der Erkennung von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/frebanalysis/read | Dient zum Abrufen der FREB-Analyse von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/loganalyzer/read | Dient zum Abrufen der Protokollanalyse von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/read | Dient zum Abrufen der Web-Apps-Slotdiagnose. |
> | Action | microsoft.web/sites/slots/diagnostics/runtimeavailability/read | Dient zum Abrufen der Laufzeitverfügbarkeit für Web-Apps-Diagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/servicehealth/read | Dient zum Abrufen der Dienstintegrität von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/sitecpuanalysis/read | Dient zum Abrufen der CPU-Analyse von Websites von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/sitecrashes/read | Dient zum Abrufen der Siteabstürze von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/sitelatency/read | Dient zum Abrufen der Websitelatenzen von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/sitememoryanalysis/read | Dient zum Abrufen der Websitespeicheranalyse von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/siterestartsettingupdate/read | Dient zum Abrufen des Websiteupdates für die Einstellungen zum Neustarten der Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/siterestartuserinitiated/read | Dient zum Abrufen des vom Benutzer initiierten Websiteneustarts der Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/siteswap/read | Dient zum Abrufen des Websiteaustauschs von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/threadcount/read | Dient zum Abrufen der Threadanzahl von Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/workeravailability/read | Dient zum Abrufen der Workerverfügbarkeit für Web-App-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/diagnostics/workerprocessrecycle/read | Dient zum Abrufen der Workerprozesswiederverwendung für Web-Apps-Slotdiagnosen. |
> | Action | microsoft.web/sites/slots/domainownershipidentifiers/read | Dient zum Abrufen von Domänenbesitz-IDs für Web-Apps-Slots. |
> | Action | microsoft.web/sites/slots/functions/listsecrets/action | Dient zum Auflisten der Geheimnisse für Web-App-Slot-Funktionen. |
> | Action | microsoft.web/sites/slots/functions/read | Dient zum Abrufen von Web-App-Slotfunktionen. |
> | Action | microsoft.web/sites/slots/hostnamebindings/delete | Dient zum Löschen von Hostnamenbindungen für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/hostnamebindings/read | Dient zum Abrufen von Hostnamenbindungen für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/hostnamebindings/write | Dient zum Aktualisieren von Hostnamenbindungen für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/hybridconnection/delete | Dient zum Löschen der Hybridverbindung für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/hybridconnection/read | Dient zum Abrufen der Hybridverbindung für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/hybridconnection/write | Dient zum Aktualisieren der Hybridverbindung für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/delete | Dient zum Löschen der Relaynamespace-Agents für Hybridverbindungen für Web-Apps-Slots. |
> | Action | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/write | Dient zum Aktualisieren der Relaynamespace-Agents für Hybridverbindungen für Web-Apps-Slots. |
> | Action | microsoft.web/sites/slots/hybridconnectionrelays/read | Dient zum Abrufen der Relay-Agents für Hybridverbindungen für Web-Apps-Slots. |
> | Action | microsoft.web/sites/slots/instances/deployments/read | Dient zum Abrufen von Bereitstellungen für Web-App-Slotinstanzen. |
> | Action | microsoft.web/sites/slots/instances/processes/delete | Dient zum Löschen von Prozessen für Web-Apps-Slotinstanzen. |
> | Action | microsoft.web/sites/slots/instances/processes/read | Dient zum Abrufen von Prozessen für Web-App-Slotinstanzen. |
> | Action | microsoft.web/sites/slots/instances/read | Dient zum Abrufen von Web-App-Slotinstanzen. |
> | Action | microsoft.web/sites/slots/metricdefinitions/read | Dient zum Abrufen von Metrikdefinitionen für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/metrics/read | Dient zum Abrufen von Metriken für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/migratemysql/read | Dient zum Abrufen von Web-Apps-Slots für die Migration mit MySql. |
> | Action | microsoft.web/sites/slots/networktrace/action | Web-Apps-Slots für die Netzwerkablaufverfolgung |
> | Action | microsoft.web/sites/slots/networktraces/operationresults/read | Ruft Vorgangsergebnisse für die Netzwerkablaufverfolgung von Web-Apps-Slots ab. |
> | Action | microsoft.web/sites/slots/newpassword/action | NewPassword-Web-App-Slots |
> | Action | microsoft.web/sites/slots/operationresults/read | Dient zum Abrufen der Ergebnisse von Web-App-Slotvorgängen. |
> | Action | microsoft.web/sites/slots/operations/read | Dient zum Abrufen von Web-Apps-Slotvorgängen. |
> | Action | microsoft.web/sites/slots/perfcounters/read | Dient zum Abrufen von Leistungsindikatoren für Web-Apps-Slots. |
> | Action | microsoft.web/sites/slots/phplogging/read | Dient zum Abrufen der PHP-Protokollierung für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/premieraddons/delete | Dient zum Löschen von Premier-Add-Ons für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/premieraddons/read | Dient zum Abrufen von Premier-Add-Ons für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/premieraddons/write | Dient zum Aktualisieren von Premier-Add-Ons für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/processes/read | Ruft Prozesse von Web-App-Slots ab. |
> | Action | microsoft.web/sites/slots/publiccertificates/delete | Hiermit löschen Sie öffentliche Zertifikate für Web-Apps-Slots ab. |
> | Action | microsoft.web/sites/slots/publiccertificates/read | Ruft die öffentlichen Zertifikate für Web-Apps-Slots ab. |
> | Action | microsoft.web/sites/slots/publiccertificates/write | Erstellt oder aktualisiert die öffentlichen Zertifikate für Web-Apps-Slots. |
> | Action | Microsoft.Web/sites/slots/publish/Action | Dient zum Veröffentlichen eines Web-App-Slots. |
> | Action | Microsoft.Web/sites/slots/publishxml/Action | Dient zum Abrufen des Veröffentlichungsprofils (XML) für einen Web-App-Slot. |
> | Action | Microsoft.Web/sites/slots/Read | Dient zum Abrufen der Eigenschaften eines Web-App-Bereitstellungsslots. |
> | Action | microsoft.web/sites/slots/recover/action | Stellt Web-App-Slots wieder her. |
> | Action | Microsoft.Web/sites/slots/resetSlotConfig/Action | Dient zum Zurücksetzen der Web-App-Slotkonfiguration. |
> | Action | microsoft.web/sites/slots/resourcehealthmetadata/read | Ruft Metadaten zur Ressourcenintegrität von Web-Apps-Slots ab. |
> | Action | Microsoft.Web/sites/slots/restart/Action | Dient zum Neustarten eines Web-App-Slots. |
> | Action | microsoft.web/sites/slots/restore/read | Dient zum Abrufen der Wiederherstellung für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/restore/write | Dient zum Wiederherstellen von Web-Apps-Slots. |
> | Action | microsoft.web/sites/slots/restorefrombackupblob/action | Wiederherstellen eines Web-App-Slots aus dem Sicherungsblob |
> | Action | microsoft.web/sites/slots/restorefromdeletedapp/action | Hiermit stellen Sie Web-App-Slots aus einer gelöschten App wieder her. |
> | Action | microsoft.web/sites/slots/restoresnapshot/action | Stellt Momentaufnahme von Web-App-Slots wieder her. |
> | Action | microsoft.web/sites/slots/siteextensions/delete | Dient zum Löschen von Websiteerweiterungen von Web-Apps-Slots. |
> | Action | microsoft.web/sites/slots/siteextensions/read | Dient zum Abrufen von Websiteerweiterungen von Web-Apps-Slots. |
> | Action | microsoft.web/sites/slots/siteextensions/write | Dient zum Aktualisieren von Websiteerweiterungen von Web-Apps-Slots. |
> | Action | Microsoft.Web/sites/slots/slotsdiffs/Action | Dient zum Abrufen von Konfigurationsunterschieden zwischen Web-App und Slots. |
> | Action | Microsoft.Web/sites/slots/slotsswap/Action | Dient zum Austauschen von Web-App-Bereitstellungsslots. |
> | Action | microsoft.web/sites/slots/snapshots/read | Dient zum Abrufen von Web-Apps-Slotmomentaufnahmen. |
> | Action | Microsoft.Web/sites/slots/sourcecontrols/Delete | Dient zum Löschen der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots. |
> | Action | Microsoft.Web/sites/slots/sourcecontrols/Read | Dient zum Abrufen der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots. |
> | Action | Microsoft.Web/sites/slots/sourcecontrols/Write | Dient zum Aktualisieren der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots. |
> | Action | Microsoft.Web/sites/slots/start/Action | Dient zum Starten eines Web-App-Slots. |
> | Action | Microsoft.Web/sites/slots/stop/Action | Dient zum Beenden eines Web-App-Slots. |
> | Action | microsoft.web/sites/slots/sync/action | Dient zum Synchronisieren von Web-App-Slots. |
> | Action | microsoft.web/sites/slots/triggeredwebjobs/delete | Dient zum Löschen ausgelöster Webaufträge für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/triggeredwebjobs/read | Dient zum Abrufen ausgelöster Webaufträge für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/triggeredwebjobs/run/action | Dient zum Ausführen ausgelöster Webaufträge für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/usages/read | Dient zum Abrufen von Verwendungen für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/delete | Dient zum Löschen von Verbindungen mit virtuellen Netzwerken für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/gateways/write | Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/read | Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/write | Dient zum Aktualisieren von Verbindungen mit virtuellen Netzwerken für Web-App-Slots. |
> | Action | microsoft.web/sites/slots/webjobs/read | Dient zum Abrufen von Webaufträgen für Web-App-Slots. |
> | Action | Microsoft.Web/sites/slots/Write | Dient zum Erstellen eines neuen Web-App-Slots oder zum Aktualisieren eines bereits vorhandenen. |
> | Action | Microsoft.Web/sites/slotsdiffs/Action | Dient zum Abrufen von Konfigurationsunterschieden zwischen Web-App und Slots. |
> | Action | Microsoft.Web/sites/slotsswap/Action | Dient zum Austauschen von Web-App-Bereitstellungsslots. |
> | Action | microsoft.web/sites/snapshots/read | Dient zum Abrufen von Web-App-Momentaufnahmen. |
> | Action | Microsoft.Web/sites/sourcecontrols/Delete | Dient zum Löschen der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App. |
> | Action | Microsoft.Web/sites/sourcecontrols/Read | Dient zum Abrufen der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App. |
> | Action | Microsoft.Web/sites/sourcecontrols/Write | Dient zum Aktualisieren der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App. |
> | Action | Microsoft.Web/sites/start/Action | Dient zum Starten einer Web-App. |
> | Action | Microsoft.Web/sites/stop/Action | Dient zum Beenden einer Web-App. |
> | Action | microsoft.web/sites/sync/action | Synchronisierungs-Web-Apps |
> | Action | microsoft.web/sites/syncfunctiontriggers/action | Synchronisieren von Funktionstriggern. |
> | Action | microsoft.web/sites/triggeredwebjobs/delete | Dient zum Löschen ausgelöster Webaufträge für Web-Apps. |
> | Action | microsoft.web/sites/triggeredwebjobs/history/read | Dient zum Abrufen des ausgelösten WebJobs-Verlaufs für Web-Apps. |
> | Action | microsoft.web/sites/triggeredwebjobs/read | Dient zum Abrufen ausgelöster Webaufträge für Web-Apps. |
> | Action | microsoft.web/sites/triggeredwebjobs/run/action | Dient zum Ausführen ausgelöster Webaufträge für Web-Apps. |
> | Action | microsoft.web/sites/usages/read | Dient zum Abrufen von Web-App-Verwendungen. |
> | Action | microsoft.web/sites/virtualnetworkconnections/delete | Dient zum Löschen von Verbindungen mit virtuellen Netzwerken für Web-Apps. |
> | Action | microsoft.web/sites/virtualnetworkconnections/gateways/read | Dient zum Abrufen von Gateways für Verbindungen mit virtuellen Netzwerken für Web-Apps. |
> | Action | microsoft.web/sites/virtualnetworkconnections/gateways/write | Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für Web-Apps. |
> | Action | microsoft.web/sites/virtualnetworkconnections/read | Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für Web-Apps. |
> | Action | microsoft.web/sites/virtualnetworkconnections/write | Dient zum Aktualisieren von Verbindungen mit virtuellen Netzwerken für Web-Apps. |
> | Action | microsoft.web/sites/webjobs/read | Dient zum Abrufen von Webaufträgen für Web-Apps. |
> | Action | Microsoft.Web/sites/Write | Dient zum Erstellen einer neuen Web-App oder zum Aktualisieren einer bereits vorhandenen. |
> | Action | microsoft.web/skus/read | Dient zum Abrufen von SKUs. |
> | Action | microsoft.web/sourcecontrols/read | Dient zum Abrufen von Quellcodeverwaltungen. |
> | Action | microsoft.web/sourcecontrols/write | Dient zum Aktualisieren von Quellcodeverwaltungen. |
> | Action | microsoft.web/unregister/action | Dient zum Aufheben der Registrierung des Microsoft.Web-Ressourcenanbieters für das Abonnement. |
> | Action | microsoft.web/validate/action | Dient zur Überprüfung. |
> | Action | microsoft.web/verifyhostingenvironmentvnet/action | Überprüft das VNET der Hostingumgebung. |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Action | Microsoft.WorkloadMonitor/components/read | Ruft Komponenten für die Ressource ab |
> | Action | Microsoft.WorkloadMonitor/componentsSummary/read | Ruft eine Zusammenfassung von Komponenten ab |
> | Action | Microsoft.WorkloadMonitor/monitorInstances/read | Ruft Überwachungsinstanzen für die Ressource ab |
> | Action | Microsoft.WorkloadMonitor/monitorInstancesSummary/read | Ruft eine Zusammenfassung von Überwachungsinstanzen ab |
> | Action | Microsoft.WorkloadMonitor/monitors/read | Ruft Überwachungen für die Ressource ab |
> | Action | Microsoft.WorkloadMonitor/monitors/write | Überwachung für die Ressource konfigurieren |
> | Action | Microsoft.WorkloadMonitor/notificationSettings/read | Ruft Benachrichtigungseinstellungen für die Ressource ab |
> | Action | Microsoft.WorkloadMonitor/notificationSettings/write | Benachrichtigungseinstellungen für die Ressource konfigurieren |
> | Action | Microsoft.WorkloadMonitor/operations/read | Ruft die unterstützten Vorgänge ab |

## <a name="next-steps"></a>Nächste Schritte

- [Zuordnung von Ressourcenanbieter zu Dienst](../azure-resource-manager/management/azure-services-resource-providers.md)
- [Benutzerdefinierte Rollen für Azure-Ressourcen](custom-roles.md)
- [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](built-in-roles.md)
