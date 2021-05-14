---
title: Verfügbarkeit des Azure-Dienstcloudfeatures für US-Regierungskunden
description: Listet die Featureverfügbarkeit für Azure-Sicherheitsdienste auf, z. B. Azure Sentinel für US-Regierungskunden.
author: batami
ms.author: bagol
ms.service: security
ms.topic: reference
ms.date: 04/29/2021
ms.openlocfilehash: 6cead1e9dfb8cf1d12ef09079e97779c08523e36
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108290576"
---
# <a name="cloud-feature-availability-for-us-government-customers"></a>Verfügbarkeit von Cloudfeatures für US-Regierungskunden

In diesem Artikel wird die Featureverfügbarkeit in den Microsoft Azure und Azure Government Clouds für die folgenden Sicherheitsdienste beschrieben:

- Azure Sentinel

> [!NOTE]
> Diesem Artikel werden in Kürze zusätzliche Sicherheitsdienste hinzugefügt.
> 

## <a name="azure-government"></a>Azure Government

Azure Government verwendet die gleichen zugrunde liegenden Technologien wie Azure (manchmal auch als Azure Commercial oder Azure Public bezeichnet), das die Kernkomponenten von Infrastructure-as-a-Service (IaaS), Platform-as-a-Service (PaaS) und Software-as-a-Service (SaaS) umfasst. Sowohl Azure als auch Azure Government verfügen über umfassende Sicherheitskontrollen und die Microsoft-Verpflichtung zum Schutz von Kundendaten.

Azure Government ist eine physisch isolierte Cloudumgebung, die dediziert für US-Behörden auf Bundes-, Staats-, lokaler und Stammesebene sowie für deren Partner bestimmt ist. Zwar sind beide Cloudumgebungen auf der FedRAMP High-Auswirkungsstufe bewertet und autorisiert, doch Azure Government bietet eine zusätzliche Schutzebene für Kunden durch vertragliche Verpflichtungen hinsichtlich der Speicherung von Kundendaten in den USA und der Beschränkung des potenziellen Zugriffs auf Systeme, die Kundendaten verarbeiten, auf sicherheitsüberprüfte US-Bürger. Diese Verpflichtungen können für Kunden von Interesse sein, die die Cloud verwenden, um Daten zu speichern oder zu verarbeiten, die den US-Exportkontrollbestimmungen wie EAR, ITAR und DoE 10 CFR Part 810 unterliegen.

Weitere Informationen zu Azure Government finden Sie unter [Was ist Azure Government?](/azure/azure-government/documentation-government-welcome).

## <a name="microsoft-365-integration"></a>Microsoft 365-Integration

Integrationen zwischen Produkten basieren auf der Interoperabilität zwischen Azure- und Office-Plattformen. Auf Angebote, die in der Azure-Umgebung gehostet werden, kann über die Plattformen Microsoft 365 Enterprise und Microsoft 365 Government zugegriffen werden. Office 365 und Office 365 GCC sind mit Azure Active Directory (Azure AD) in Azure gekoppelt. Office 365 GCC High und Office 365 DoD sind mit Azure AD in Azure Government gekoppelt.

Das folgende Diagramm zeigt die Hierarchie von Microsoft Clouds und deren Beziehungen zueinander.

:::image type="content" source="media/microsoft-365-cloud-integration.png" alt-text="Microsoft 365-Cloudintegration.":::

Die Office 365 GCC-Umgebung hilft Kunden, die Anforderungen der US-Regierung zu erfüllen, einschließlich FedRAMP High, CJIS und IRS 1075. Die Office 365 GCC High- und DoD-Umgebungen unterstützen Kunden, die Compliance mit DoD IL4/5, DFARS 7012, NIST 800-171 und ITAR benötigen.

Weitere Informationen zu Office 365 US Government-Umgebungen finden Sie unter:

- [Office 365 GCC](/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc)
- [Office 365 GCC High und DoD](/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc-high-and-dod)


In den folgenden Abschnitten wird beschrieben, wann ein Dienst über eine Integration in Microsoft 365 und die Featureverfügbarkeit für Office 365 GCC, Office 365 High und Office 365 DoD verfügt.
## <a name="azure-sentinel"></a>Azure Sentinel

Microsoft Azure Sentinel ist eine skalierbare, cloudnative Lösung für Security Information & Event Management (SIEM) und die Sicherheitsorchestrierung mit automatisierter Reaktion (Security Orchestration Automated Response, SOAR). Azure Sentinel bietet intelligente Sicherheits- und Bedrohungsanalysen für das ganze Unternehmen und stellt eine zentrale Lösung für die Warnungs- und Bedrohungserkennung, die proaktive Suche sowie die Reaktion auf Bedrohungen bereit.

Weitere Informationen finden Sie in der [Azure Sentinel-Produktdokumentation](/azure/sentinel/overview).

In den folgenden Tabellen wird die aktuelle Azure Sentinel-Featureverfügbarkeit in Azure und Azure Government gezeigt.


| Funktion | Azure | Azure Government  |
| ----- | ----- | ---- |
|- [Bring Your Own ML (BYO-ML)](/azure/sentinel/bring-your-own-ml) | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Mandantenübergreifende/arbeitsbereichsübergreifende Incidentansicht](/azure/sentinel/multiple-workspace-view) |Öffentliche Vorschau | Öffentliche Vorschau |
| - [Erkenntnisse über Entitäten](/azure/sentinel/enable-entity-behavior-analytics) | Öffentliche Vorschau | Nicht verfügbar |
| - [Fusion](/azure/sentinel/fusion)<br>Erweiterte Erkennung von mehrstufigen Angriffen<sup>[1](#footnote1)</sup> | Allgemein verfügbar | Nicht verfügbar |
|- [Notebooks](/azure/sentinel/notebooks) | Allgemein verfügbar | Allgemein verfügbar |
|- [Überwachungsmetriken für SOC-Incidents](/azure/sentinel/manage-soc-with-incident-metrics) | Allgemein verfügbar | Allgemein verfügbar |
|- [Watchlists](https://techcommunity.microsoft.com/t5/azure-sentinel/what-s-new-watchlist-is-now-in-public-preview/ba-p/1765887) | Öffentliche Vorschau | Nicht verfügbar |
| **Threat Intelligence-Unterstützung** | | |
| - [Threat Intelligence – TAXII-Datenconnector](/azure/sentinel/import-threat-intelligence)  | Öffentliche Vorschau | Nicht verfügbar |
| - [Threat Intelligence-Plattform-Datenconnector](/azure/sentinel/import-threat-intelligence)  | Öffentliche Vorschau | Nicht verfügbar |
| - [Blatt „Threat Intelligence-Forschung“](https://techcommunity.microsoft.com/t5/azure-sentinel/what-s-new-threat-intelligence-menu-item-in-public-preview/ba-p/1646597)  | Öffentliche Vorschau | Nicht verfügbar |
| - [URL-Detonation](https://techcommunity.microsoft.com/t5/azure-sentinel/using-the-new-built-in-url-detonation-in-azure-sentinel/ba-p/996229) | Allgemein verfügbar | Nicht verfügbar |
| - [Arbeitsmappe „Threat Intelligence“](/azure/architecture/example-scenario/data/sentinel-threat-intelligence)  | Allgemein verfügbar | Nicht verfügbar |
|**Erkennungsunterstützung** | | |
| - [Erkennung anomaler Zugriffe auf Windows-Dateifreigaben](/azure/sentinel/fusion)  | Öffentliche Vorschau | Nicht verfügbar |
| - [Erkennung anomaler RDP-Anmeldungen](/azure/sentinel/connect-windows-security-events#configure-the-security-events-connector-for-anomalous-rdp-login-detection)<br>Integrierte ML-Erkennung | Öffentliche Vorschau | Nicht verfügbar |
| - [Erkennung anomaler SSH-Anmeldungen](/azure/sentinel/connect-syslog#configure-the-syslog-connector-for-anomalous-ssh-login-detection)<br>Integrierte ML-Erkennung | Öffentliche Vorschau | Nicht verfügbar |
| **Azure-Dienstconnectors** | | |
| - [Azure-Aktivitätsprotokolle](/azure/sentinel/connect-azure-activity)                                  |   Allgemein verfügbar           |    Allgemein verfügbar         |
| - [Azure Active Directory](/azure/sentinel/connect-azure-active-directory)                |      Allgemein verfügbar        |       Allgemein verfügbar        |
| - [Azure ADIP](/azure/sentinel/connect-azure-ad-identity-protection)                         |  Allgemein verfügbar            |        Allgemein verfügbar              |
| - [Azure DDoS Protection](/azure/sentinel/connect-azure-ddos-protection)                |     Allgemein verfügbar         |       Allgemein verfügbar               |
| - [Azure Defender](/azure/sentinel/connect-azure-security-center)                  |    Allgemein verfügbar          |        Allgemein verfügbar              |
| - [Azure Defender für IoT](/azure/sentinel/connect-asc-iot)           |       Allgemein verfügbar       |  Nicht verfügbar           |
| - [Azure Firewall ](/azure/sentinel/connect-azure-firewall)                        |   Allgemein verfügbar           |        Allgemein verfügbar              |
| - [Azure Information Protection](/azure/sentinel/connect-azure-information-protection)              |     Öffentliche Vorschau         |         Nicht verfügbar             |
| - [Azure Key Vault](/azure/sentinel/connect-azure-key-vault)                           |       Öffentliche Vorschau         |         Nicht verfügbar                       |
| - [Azure Kubernetes Services (AKS)](/azure/sentinel/connect-azure-kubernetes-service)           |       Öffentliche Vorschau         |         Nicht verfügbar                |
| - [Azure SQL-Datenbanken](/azure/sentinel/connect-azure-sql-logs)                        |     Allgemein verfügbar         |         Allgemein verfügbar             |
| - [Azure WAF](/azure/sentinel/connect-azure-waf)                                  |      Allgemein verfügbar        |      Allgemein verfügbar                |
| **Windows-Connectors** | | |
| - [Windows-Firewall](/azure/sentinel/connect-windows-firewall)                                 |     Allgemein verfügbar         |   Allgemein verfügbar           |
| - [Windows-Sicherheitsereignisse](/azure/sentinel/connect-windows-security-events)                                  |      Allgemein verfügbar        |         Allgemein verfügbar     |
| **Externe Connectors**| | |
| - [Agari Phishing Defense and Brand Protection](/azure/sentinel/connect-agari-phishing-defense)       | Öffentliche Vorschau | Öffentliche Vorschau |
| - [AI Analyst Darktrace](/azure/sentinel/connect-data-sources)                            | Öffentliche Vorschau | Öffentliche Vorschau |
| - [AI Vectra Detect](/azure/sentinel/connect-ai-vectra-detect)                                 | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Akamai Security Events](/azure/sentinel/connect-akamai-security-events)                           | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Alcide kAudit](/azure/sentinel/connect-alcide-kaudit)                                   | Öffentliche Vorschau | Nicht verfügbar      |
| - [Alsid für Active Directory](/azure/sentinel/connect-alsid-active-directory)                      | Öffentliche Vorschau | Nicht verfügbar      |
| - [Apache HHTP-Server](/azure/sentinel/connect-apache-http-server)                               | Öffentliche Vorschau | Nicht verfügbar      |
| - [Aruba ClearPass](/azure/sentinel/connect-aruba-clearpass)                                  | Öffentliche Vorschau | Öffentliche Vorschau |
| - [AWS](/azure/sentinel/connect-data-sources)                                             | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Barracuda CloudGen Firewall](/azure/sentinel/connect-barracuda-cloudgen-firewall)                      | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Barracuda Web App Firewall](/azure/sentinel/connect-barracuda)                       | Allgemein verfügbar             | Allgemein verfügbar             |
| - [BETTER Mobile Threat Defense MTD](/azure/sentinel/connect-better-mtd)                 | Öffentliche Vorschau | Nicht verfügbar      |
| - [Beyond Security beSECURE](/azure/sentinel/connect-besecure)                        | Öffentliche Vorschau | Nicht verfügbar      |
| - [Blackberry CylancePROTECT](/azure/sentinel/connect-data-sources)                        | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Broadcom Symantec DLP](/azure/sentinel/connect-broadcom-symantec-dlp)                            | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Check Point](/azure/sentinel/connect-checkpoint)                                      | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Cisco ASA](/azure/sentinel/connect-cisco)                                        | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Cisco Meraki](/azure/sentinel/connect-cisco-meraki)                                     | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Cisco Umbrella](/azure/sentinel/connect-cisco-umbrella)                                   | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Cisco UCS](/azure/sentinel/connect-cisco-ucs)                                        | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Cisco Firepower EStreamer](/azure/sentinel/connect-data-sources)                          | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Citrix Analytics WAF](/azure/sentinel/connect-citrix-waf)                             | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Common Event Format (CEF)](/azure/sentinel/connect-common-event-format)                        | Allgemein verfügbar             | Allgemein verfügbar             |
| - [CyberArk EPV-Ereignisse (Enterprise Password Vault)](/azure/sentinel/connect-cyberark) | Öffentliche Vorschau | Öffentliche Vorschau |
| - [ESET Enterprise Inspector](/azure/sentinel/connect-data-sources)                       | Öffentliche Vorschau | Nicht verfügbar      |
| - [Eset Security Management Center](/azure/sentinel/connect-data-sources)                  | Öffentliche Vorschau | Nicht verfügbar      |
| - [ExtraHop Reveal(x)](/azure/sentinel/connect-extrahop)                               | Allgemein verfügbar             | Allgemein verfügbar             |
| - [F5 BIG-IP ](/azure/sentinel/connect-f5-big-ip)                                       | Allgemein verfügbar             | Allgemein verfügbar             |
| - [F5 Networks](/azure/sentinel/connect-f5)                                     | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Forcepoint NGFW](/azure/sentinel/connect-forcepoint-casb-ngfw)                                  | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Forepoint CASB](/azure/sentinel/connect-forcepoint-casb-ngfw)                                  | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Forepoint DLP ](/azure/sentinel/connect-forcepoint-dlp)                                   | Öffentliche Vorschau | Nicht verfügbar      |
| - [ForgeRock Common Audit for CEF](/azure/sentinel/connect-data-sources)                  | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Fortinet](/azure/sentinel/connect-fortinet)                                         | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Google Workspace (G Suite) ](/azure/sentinel/connect-google-workspace)                      | Öffentliche Vorschau | Nicht verfügbar      |
| - [Illusive Attack Management System](/azure/sentinel/connect-illusive-attack-management-system)                | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Imperva WAF Gateway](/azure/sentinel/connect-imperva-waf-gateway)                             | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Infoblox NIOS](/azure/sentinel/connect-infoblox)                                    | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Juniper SRX](/azure/sentinel/connect-juniper-srx)                                      | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Morphisec UTPP](/azure/sentinel/connect-data-sources)                                   | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Netskope](/azure/sentinel/connect-data-sources)                                         | Öffentliche Vorschau | Öffentliche Vorschau |
| - [NXLog Windows DNS](/azure/sentinel/connect-nxlog-dns)                                             | Öffentliche Vorschau | Nicht verfügbar      |
| - [NXLog LinuxAudit](/azure/sentinel/connect-nxlog-linuxaudit)                                 | Öffentliche Vorschau | Nicht verfügbar      |
| - [Okta Single Sign On](/azure/sentinel/connect-okta-single-sign-on)                              | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Onapsis Platform](/azure/sentinel/connect-data-sources)                                 | Öffentliche Vorschau | Öffentliche Vorschau |
| - [One Identity Safeguard](/azure/sentinel/connect-one-identity)                          | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Orca Security Alerts](/azure/sentinel/connect-orca-security-alerts)                            | Öffentliche Vorschau | Nicht verfügbar      |
| - [Palo Alto Networks](/azure/sentinel/connect-paloalto)                               | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Perimeter 81 Activity Logs](/azure/sentinel/connect-perimeter-81-logs)                      | Allgemein verfügbar             | Nicht verfügbar      |
| - [Proofpoint On Demand Email Security](/azure/sentinel/connect-proofpoint-pod)             | Öffentliche Vorschau | Nicht verfügbar      |
| - [Proofpoint TAP](/azure/sentinel/connect-proofpoint-tap)                                   | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Pulse Connect Secure](/azure/sentinel/connect-proofpoint-tap)                             | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Qualys Vulnerability Management](/azure/sentinel/connect-qualys-vm)                  | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Salesforce Service Cloud](/azure/sentinel/connect-salesforce-service-cloud)                         | Öffentliche Vorschau | Nicht verfügbar      |
| - [SonicWall Firewall ](/azure/sentinel/connect-sophos-cloud-optix)                              | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Sophos Cloud Optix](/azure/sentinel/connect-sophos-cloud-optix)                               | Öffentliche Vorschau | Nicht verfügbar      |
| - [Sophos XG Firewall](/azure/sentinel/connect-sophos-xg-firewall)                               | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Squadra Technologies secRMM](/azure/sentinel/connect-squadra-secrmm)               | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Squid Proxy](/azure/sentinel/connect-squid-proxy)                                      | Öffentliche Vorschau | Nicht verfügbar      |
| - [Symantec Integrated Cyber Defense Exchange](/azure/sentinel/connect-symantec)       | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Symantec ProxySG](/azure/sentinel/connect-symantec-proxy-sg)                                | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Symantec VIP](/azure/sentinel/connect-symantec-vip)                                     | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Syslog](/azure/sentinel/connect-syslog)                                           | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Threat Intelligence-Plattform](/azure/sentinel/connect-threat-intelligence)en                   | Öffentliche Vorschau | Nicht verfügbar      |
| - [Threat Intelligence TAXII](/azure/sentinel/connect-threat-intelligence)                       | Öffentliche Vorschau | Nicht verfügbar      |
| - [Thycotic Secret Server](/azure/sentinel/connect-thycotic-secret-server)                          | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Trend Micro Deep Security](/azure/sentinel/connect-trend-micro)                       | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Trend Micro TippingPoint](/azure/sentinel/connect-trend-micro-tippingpoint)                         | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Trend Micro XDR](/azure/sentinel/connect-data-sources)                                  | Öffentliche Vorschau | Nicht verfügbar      |
| - [VMware Carbon Black Endpoint Standard](/azure/sentinel/connect-vmware-carbon-black)           | Öffentliche Vorschau | Öffentliche Vorschau |
| - [VMware ESXi](/azure/sentinel/connect-vmware-esxi)                                      | Öffentliche Vorschau | Öffentliche Vorschau |
| - [WireX Network Forensics Platform](/azure/sentinel/connect-wirex-systems)                | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Zimperium Mobile Threat Defense](/azure/sentinel/connect-zimperium-mtd)                  | Öffentliche Vorschau | Nicht verfügbar      |
| - [Zscaler](/azure/sentinel/connect-zscaler)                                         | Allgemein verfügbar             | Allgemein verfügbar             |
| | | |


<sup><a name="footnote1" /></a>1</sup> SSH- und RDP-Erkennungen werden für Sovereign Clouds nicht unterstützt, da die Databricks ML-Plattform nicht verfügbar ist.

### <a name="microsoft-365-data-connectors"></a>Microsoft 365-Datenconnectors

Office 365 GCC ist mit Azure Active Directory (Azure AD) in Azure gekoppelt. Office 365 GCC High und Office 365 DoD sind mit Azure AD in Azure Government gekoppelt.

> [!TIP]
> Achten Sie unbedingt auf die Azure-Umgebung, um zu verstehen, wo [Interoperabilität möglich ist](#microsoft-365-integration). In der folgenden Tabelle wird die Interoperabilität, die *nicht* möglich ist, mit einem Bindestrich (-) markiert, um anzugeben, dass die Unterstützung nicht relevant ist.
>

| Connector | Azure  | Azure Government  |
| ------------------------------------ | -------------- | ---------------------- |
| **[Dynamics365](/azure/sentinel/connect-dynamics-365)**                               |              |                      |
| - Office 365 GCC |Öffentliche Vorschau | -|
| - Office 365 GCC High | -|Nicht verfügbar |
| - Office 365 DoD |- | Nicht verfügbar|
| **[Microsoft 365 Defender](/azure/sentinel/connect-microsoft-365-defender)**                             |              |                      |
| - Office 365 GCC | Öffentliche Vorschau| -|
| - Office 365 GCC High |- |Nicht verfügbar |
| - Office 365 DoD |- | Nicht verfügbar|
| **[Microsoft Cloud App Security (MCAS)](/azure/sentinel/connect-cloud-app-security)**                                      |              |                      |
| - Office 365 GCC | Allgemein verfügbar| -|
| - Office 365 GCC High |-|Allgemein verfügbar |
| - Office 365 DoD |- |Allgemein verfügbar |
| **[Microsoft Cloud App Security (MCAS)](/azure/sentinel/connect-cloud-app-security)** <br>Schatten-IT-Protokolle                                  |              |                      |
| - Office 365 GCC | Öffentliche Vorschau| -|
| - Office 365 GCC High |-|Öffentliche Vorschau |
| - Office 365 DoD |- |Öffentliche Vorschau |
| **[Microsoft Cloud App Security (MCAS)](/azure/sentinel/connect-cloud-app-security)**                  <br>Alerts                    |              |                      |
| - Office 365 GCC | Öffentliche Vorschau| -|
| - Office 365 GCC High |-|Öffentliche Vorschau |
| - Office 365 DoD |- |Öffentliche Vorschau |
| **[Microsoft Defender für den Endpunkt](/azure/sentinel/connect-microsoft-defender-advanced-threat-protection)**                                       |              |                      |
| - Office 365 GCC | Allgemein verfügbar|- |
| - Office 365 GCC High |- |Nicht verfügbar |
| - Office 365 DoD |- | Nicht verfügbar|
| **[Microsoft Defender for Identity](/azure/sentinel/connect-azure-atp)**                                        |              |                      |
| - Office 365 GCC |Öffentliche Vorschau | -|
| - Office 365 GCC High |- | Nicht verfügbar |
| - Office 365 DoD |- |Nicht verfügbar |
| **[Microsoft Defender für Office 365](/azure/sentinel/connect-office-365-advanced-threat-protection)**               |              |                      |
| - Office 365 GCC |Öffentliche Vorschau |- |
| - Office 365 GCC High |- |Nicht verfügbar |
| - Office 365 DoD | -|Nicht verfügbar |
| **[Office 365](/azure/sentinel/connect-office-365)**                                      |              |                      |
| - Office 365 GCC | Allgemein verfügbar|- |
| - Office 365 GCC High |- |Allgemein verfügbar |
| - Office 365 DoD |- |Allgemein verfügbar |
| | |


## <a name="next-steps"></a>Nächste Schritte

- Grundlegendes zum Modell [gemeinsamer Verantwortung](https://docs.microsoft.com/azure/security/fundamentals/shared-responsibility) sowie welche Sicherheitsaufgaben vom Cloudanbieter und welche Aufgaben von Ihnen verarbeitet werden.
- Grundlegendes zu den Funktionen der [Azure Government Cloud](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) und dem vertrauenswürdigen Design und der Sicherheit, die zur Unterstützung der Compliance für Bundes-, Staats- und lokale Behörden und deren Partner verwendet werden.
- Grundlegendes zum [Office 365 Government-Plan](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/office-365-us-government#about-office-365-government-environments).
- Grundlegendes zur [Compliance in Azure](/azure/compliance/) für rechtliche und gesetzliche Standards.
