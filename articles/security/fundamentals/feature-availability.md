---
title: Verfügbarkeit des Azure-Dienstcloudfeatures für US-Regierungskunden
description: Listet die Featureverfügbarkeit für Azure-Sicherheitsdienste auf, z. B. Azure Sentinel für US-Regierungskunden.
author: TerryLanfear
ms.author: terrylan
ms.service: security
ms.topic: reference
ms.date: 05/23/2021
ms.openlocfilehash: 817e23bfe21aeabde51064cd2606fa447ee6de22
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112060010"
---
# <a name="cloud-feature-availability-for-us-government-customers"></a>Verfügbarkeit von Cloudfeatures für US-Regierungskunden


In diesem Artikel wird die Featureverfügbarkeit in den Microsoft Azure und Azure Government Clouds für die folgenden Sicherheitsdienste beschrieben:

- [Azure Sentinel](#azure-sentinel)

> [!NOTE]
> Diesem Artikel werden in Kürze zusätzliche Sicherheitsdienste hinzugefügt.
> 

## <a name="azure-government"></a>Azure Government

Azure Government verwendet die gleichen zugrunde liegenden Technologien wie Azure (manchmal auch als Azure Commercial oder Azure Public bezeichnet), das die Kernkomponenten von Infrastructure-as-a-Service (IaaS), Platform-as-a-Service (PaaS) und Software-as-a-Service (SaaS) umfasst. Sowohl Azure als auch Azure Government verfügen über umfassende Sicherheitskontrollen und die Microsoft-Verpflichtung zum Schutz von Kundendaten.

Azure Government ist eine physisch isolierte Cloudumgebung, die dediziert für US-Behörden auf Bundes-, Staats-, lokaler und Stammesebene sowie für deren Partner bestimmt ist. Zwar sind beide Cloudumgebungen auf der FedRAMP High-Auswirkungsstufe bewertet und autorisiert, doch Azure Government bietet eine zusätzliche Schutzebene für Kunden durch vertragliche Verpflichtungen hinsichtlich der Speicherung von Kundendaten in den USA und der Beschränkung des potenziellen Zugriffs auf Systeme, die Kundendaten verarbeiten, auf sicherheitsüberprüfte US-Bürger. Diese Verpflichtungen können für Kunden von Interesse sein, die die Cloud verwenden, um Daten zu speichern oder zu verarbeiten, die den US-Exportkontrollbestimmungen wie EAR, ITAR und DoE 10 CFR Part 810 unterliegen.

Weitere Informationen zu Azure Government finden Sie unter [Was ist Azure Government?](../../azure-government/documentation-government-welcome.md).

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

Weitere Informationen finden Sie in der [Azure Sentinel-Produktdokumentation](../../sentinel/overview.md).

In den folgenden Tabellen wird die aktuelle Azure Sentinel-Featureverfügbarkeit in Azure und Azure Government gezeigt.


| Funktion | Azure | Azure Government  |
| ----- | ----- | ---- |
|- [Bring Your Own ML (BYO-ML)](../../sentinel/bring-your-own-ml.md) | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Mandantenübergreifende/arbeitsbereichsübergreifende Incidentansicht](../../sentinel/multiple-workspace-view.md) |Öffentliche Vorschau | Öffentliche Vorschau |
| - [Erkenntnisse über Entitäten](../../sentinel/enable-entity-behavior-analytics.md) | Allgemein verfügbar | Öffentliche Vorschau |
| - [Fusion](../../sentinel/fusion.md)<br>Erweiterte Erkennung von mehrstufigen Angriffen<sup>[1](#footnote1)</sup> | Allgemein verfügbar | Allgemein verfügbar |
| - [Hunting](../../sentinel/hunting.md) | Allgemein verfügbar | Allgemein verfügbar |
|- [Notebooks](../../sentinel/notebooks.md) | Allgemein verfügbar | Allgemein verfügbar |
|- [Überwachungsmetriken für SOC-Incidents](../../sentinel/manage-soc-with-incident-metrics.md) | Allgemein verfügbar | Allgemein verfügbar |
|- [Watchlists](https://techcommunity.microsoft.com/t5/azure-sentinel/what-s-new-watchlist-is-now-in-public-preview/ba-p/1765887) | Öffentliche Vorschau | Nicht verfügbar |
| **Threat Intelligence-Unterstützung** | | |
| - [Threat Intelligence – TAXII-Datenconnector](../../sentinel/import-threat-intelligence.md)  | Öffentliche Vorschau | Nicht verfügbar |
| - [Threat Intelligence-Plattform-Datenconnector](../../sentinel/import-threat-intelligence.md)  | Öffentliche Vorschau | Nicht verfügbar |
| - [Blatt „Threat Intelligence-Forschung“](https://techcommunity.microsoft.com/t5/azure-sentinel/what-s-new-threat-intelligence-menu-item-in-public-preview/ba-p/1646597)  | Öffentliche Vorschau | Nicht verfügbar |
| - [URL-Detonation](https://techcommunity.microsoft.com/t5/azure-sentinel/using-the-new-built-in-url-detonation-in-azure-sentinel/ba-p/996229) | Allgemein verfügbar | Nicht verfügbar |
| - [Arbeitsmappe „Threat Intelligence“](/azure/architecture/example-scenario/data/sentinel-threat-intelligence)  | Allgemein verfügbar | Nicht verfügbar |
|**Erkennungsunterstützung** | | |
| - [Erkennung anomaler Zugriffe auf Windows-Dateifreigaben](../../sentinel/fusion.md)  | Öffentliche Vorschau | Nicht verfügbar |
| - [Erkennung anomaler RDP-Anmeldungen](../../sentinel/connect-windows-security-events.md#configure-the-security-events--windows-security-events-connector-for-anomalous-rdp-login-detection)<br>Integrierte ML-Erkennung | Öffentliche Vorschau | Nicht verfügbar |
| - [Erkennung anomaler SSH-Anmeldungen](../../sentinel/connect-syslog.md#configure-the-syslog-connector-for-anomalous-ssh-login-detection)<br>Integrierte ML-Erkennung | Öffentliche Vorschau | Nicht verfügbar |
| **Azure-Dienstconnectors** | | |
| - [Azure-Aktivitätsprotokolle](../../sentinel/connect-azure-activity.md)                                  |   Allgemein verfügbar           |    Allgemein verfügbar         |
| - [Azure Active Directory](../../sentinel/connect-azure-active-directory.md)                |      Allgemein verfügbar        |       Allgemein verfügbar        |
| - [Azure ADIP](../../sentinel/connect-azure-ad-identity-protection.md)                         |  Allgemein verfügbar            |        Allgemein verfügbar              |
| - [Azure DDoS Protection](../../sentinel/connect-azure-ddos-protection.md)                |     Allgemein verfügbar         |       Allgemein verfügbar               |
| - [Azure Defender](../../sentinel/connect-azure-security-center.md)                  |    Allgemein verfügbar          |        Allgemein verfügbar              |
| - [Azure Defender für IoT](../../sentinel/connect-asc-iot.md)           |       Allgemein verfügbar       |  Nicht verfügbar           |
| - [Azure Firewall ](../../sentinel/connect-azure-firewall.md)                        |   Allgemein verfügbar           |        Allgemein verfügbar              |
| - [Azure Information Protection](../../sentinel/connect-azure-information-protection.md)              |     Öffentliche Vorschau         |         Nicht verfügbar             |
| - [Azure Key Vault](../../sentinel/connect-azure-key-vault.md)                           |       Öffentliche Vorschau         |         Nicht verfügbar                       |
| - [Azure Kubernetes Services (AKS)](../../sentinel/connect-azure-kubernetes-service.md)           |       Öffentliche Vorschau         |         Nicht verfügbar                |
| - [Azure SQL-Datenbanken](../../sentinel/connect-azure-sql-logs.md)                        |     Allgemein verfügbar         |         Allgemein verfügbar             |
| - [Azure WAF](../../sentinel/connect-azure-waf.md)                                  |      Allgemein verfügbar        |      Allgemein verfügbar                |
| **Windows-Connectors** | | |
| - [Windows-Firewall](../../sentinel/connect-windows-firewall.md)                                 |     Allgemein verfügbar         |   Allgemein verfügbar           |
| - [Windows-Sicherheitsereignisse](../../sentinel/connect-windows-security-events.md)                                  |      Allgemein verfügbar        |         Allgemein verfügbar     |
| **Externe Connectors**| | |
| - [Agari Phishing Defense and Brand Protection](../../sentinel/connect-agari-phishing-defense.md)       | Öffentliche Vorschau | Öffentliche Vorschau |
| - [AI Analyst Darktrace](../../sentinel/connect-data-sources.md)                            | Öffentliche Vorschau | Öffentliche Vorschau |
| - [AI Vectra Detect](../../sentinel/connect-ai-vectra-detect.md)                                 | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Akamai Security Events](../../sentinel/connect-akamai-security-events.md)                           | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Alcide kAudit](../../sentinel/connect-alcide-kaudit.md)                                   | Öffentliche Vorschau | Nicht verfügbar      |
| - [Alsid für Active Directory](../../sentinel/connect-alsid-active-directory.md)                      | Öffentliche Vorschau | Nicht verfügbar      |
| - [Apache HTTP Server](../../sentinel/connect-apache-http-server.md)                               | Öffentliche Vorschau | Nicht verfügbar      |
| - [Aruba ClearPass](../../sentinel/connect-aruba-clearpass.md)                                  | Öffentliche Vorschau | Öffentliche Vorschau |
| - [AWS](../../sentinel/connect-data-sources.md)                                             | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Barracuda CloudGen Firewall](../../sentinel/connect-barracuda-cloudgen-firewall.md)                      | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Barracuda Web App Firewall](../../sentinel/connect-barracuda.md)                       | Allgemein verfügbar             | Allgemein verfügbar             |
| - [BETTER Mobile Threat Defense MTD](../../sentinel/connect-better-mtd.md)                 | Öffentliche Vorschau | Nicht verfügbar      |
| - [Beyond Security beSECURE](../../sentinel/connect-besecure.md)                        | Öffentliche Vorschau | Nicht verfügbar      |
| - [Blackberry CylancePROTECT](../../sentinel/connect-data-sources.md)                        | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Broadcom Symantec DLP](../../sentinel/connect-broadcom-symantec-dlp.md)                            | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Check Point](../../sentinel/connect-checkpoint.md)                                      | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Cisco ASA](../../sentinel/connect-cisco.md)                                        | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Cisco Meraki](../../sentinel/connect-cisco-meraki.md)                                     | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Cisco Umbrella](../../sentinel/connect-cisco-umbrella.md)                                   | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Cisco UCS](../../sentinel/connect-cisco-ucs.md)                                        | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Cisco Firepower EStreamer](../../sentinel/connect-data-sources.md)                          | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Citrix Analytics WAF](../../sentinel/connect-citrix-waf.md)                             | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Common Event Format (CEF)](../../sentinel/connect-common-event-format.md)                        | Allgemein verfügbar             | Allgemein verfügbar             |
| - [CyberArk EPV-Ereignisse (Enterprise Password Vault)](../../sentinel/connect-cyberark.md) | Öffentliche Vorschau | Öffentliche Vorschau |
| - [ESET Enterprise Inspector](../../sentinel/connect-data-sources.md)                       | Öffentliche Vorschau | Nicht verfügbar      |
| - [Eset Security Management Center](../../sentinel/connect-data-sources.md)                  | Öffentliche Vorschau | Nicht verfügbar      |
| - [ExtraHop Reveal(x)](../../sentinel/connect-extrahop.md)                               | Allgemein verfügbar             | Allgemein verfügbar             |
| - [F5 BIG-IP ](../../sentinel/connect-f5-big-ip.md)                                       | Allgemein verfügbar             | Allgemein verfügbar             |
| - [F5 Networks](../../sentinel/connect-f5.md)                                     | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Forcepoint NGFW](../../sentinel/connect-forcepoint-casb-ngfw.md)                                  | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Forepoint CASB](../../sentinel/connect-forcepoint-casb-ngfw.md)                                  | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Forepoint DLP ](../../sentinel/connect-forcepoint-dlp.md)                                   | Öffentliche Vorschau | Nicht verfügbar      |
| - [ForgeRock Common Audit for CEF](../../sentinel/connect-data-sources.md)                  | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Fortinet](../../sentinel/connect-fortinet.md)                                         | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Google Workspace (G Suite) ](../../sentinel/connect-google-workspace.md)                      | Öffentliche Vorschau | Nicht verfügbar      |
| - [Illusive Attack Management System](../../sentinel/connect-illusive-attack-management-system.md)                | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Imperva WAF Gateway](../../sentinel/connect-imperva-waf-gateway.md)                             | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Infoblox NIOS](../../sentinel/connect-infoblox.md)                                    | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Juniper SRX](../../sentinel/connect-juniper-srx.md)                                      | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Morphisec UTPP](../../sentinel/connect-data-sources.md)                                   | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Netskope](../../sentinel/connect-data-sources.md)                                         | Öffentliche Vorschau | Öffentliche Vorschau |
| - [NXLog Windows DNS](../../sentinel/connect-nxlog-dns.md)                                             | Öffentliche Vorschau | Nicht verfügbar      |
| - [NXLog LinuxAudit](../../sentinel/connect-nxlog-linuxaudit.md)                                 | Öffentliche Vorschau | Nicht verfügbar      |
| - [Okta Single Sign On](../../sentinel/connect-okta-single-sign-on.md)                              | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Onapsis Platform](../../sentinel/connect-data-sources.md)                                 | Öffentliche Vorschau | Öffentliche Vorschau |
| - [One Identity Safeguard](../../sentinel/connect-one-identity.md)                          | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Orca Security Alerts](../../sentinel/connect-orca-security-alerts.md)                            | Öffentliche Vorschau | Nicht verfügbar      |
| - [Palo Alto Networks](../../sentinel/connect-paloalto.md)                               | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Perimeter 81 Activity Logs](../../sentinel/connect-perimeter-81-logs.md)                      | Allgemein verfügbar             | Nicht verfügbar      |
| - [Proofpoint On Demand Email Security](../../sentinel/connect-proofpoint-pod.md)             | Öffentliche Vorschau | Nicht verfügbar      |
| - [Proofpoint TAP](../../sentinel/connect-proofpoint-tap.md)                                   | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Pulse Connect Secure](../../sentinel/connect-proofpoint-tap.md)                             | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Qualys Vulnerability Management](../../sentinel/connect-qualys-vm.md)                  | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Salesforce Service Cloud](../../sentinel/connect-salesforce-service-cloud.md)                         | Öffentliche Vorschau | Nicht verfügbar      |
| - [SonicWall Firewall ](../../sentinel/connect-sophos-cloud-optix.md)                              | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Sophos Cloud Optix](../../sentinel/connect-sophos-cloud-optix.md)                               | Öffentliche Vorschau | Nicht verfügbar      |
| - [Sophos XG Firewall](../../sentinel/connect-sophos-xg-firewall.md)                               | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Squadra Technologies secRMM](../../sentinel/connect-squadra-secrmm.md)               | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Squid Proxy](../../sentinel/connect-squid-proxy.md)                                      | Öffentliche Vorschau | Nicht verfügbar      |
| - [Symantec Integrated Cyber Defense Exchange](../../sentinel/connect-symantec.md)       | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Symantec ProxySG](../../sentinel/connect-symantec-proxy-sg.md)                                | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Symantec VIP](../../sentinel/connect-symantec-vip.md)                                     | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Syslog](../../sentinel/connect-syslog.md)                                           | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Threat Intelligence-Plattform](../../sentinel/connect-threat-intelligence.md)en                   | Öffentliche Vorschau | Nicht verfügbar      |
| - [Threat Intelligence TAXII](../../sentinel/connect-threat-intelligence.md)                       | Öffentliche Vorschau | Nicht verfügbar      |
| - [Thycotic Secret Server](../../sentinel/connect-thycotic-secret-server.md)                          | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Trend Micro Deep Security](../../sentinel/connect-trend-micro.md)                       | Allgemein verfügbar             | Allgemein verfügbar             |
| - [Trend Micro TippingPoint](../../sentinel/connect-trend-micro-tippingpoint.md)                         | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Trend Micro XDR](../../sentinel/connect-data-sources.md)                                  | Öffentliche Vorschau | Nicht verfügbar      |
| - [VMware Carbon Black Endpoint Standard](../../sentinel/connect-vmware-carbon-black.md)           | Öffentliche Vorschau | Öffentliche Vorschau |
| - [VMware ESXi](../../sentinel/connect-vmware-esxi.md)                                      | Öffentliche Vorschau | Öffentliche Vorschau |
| - [WireX Network Forensics Platform](../../sentinel/connect-wirex-systems.md)                | Öffentliche Vorschau | Öffentliche Vorschau |
| - [Zimperium Mobile Threat Defense](../../sentinel/connect-zimperium-mtd.md)                  | Öffentliche Vorschau | Nicht verfügbar      |
| - [Zscaler](../../sentinel/connect-zscaler.md)                                         | Allgemein verfügbar             | Allgemein verfügbar             |
| | | |


<sup><a name="footnote1" /></a>1</sup> SSH- und RDP-Erkennungen werden für Sovereign Clouds nicht unterstützt, da die Databricks ML-Plattform nicht verfügbar ist.

### <a name="microsoft-365-data-connectors"></a>Microsoft 365-Datenconnectors

Office 365 GCC ist mit Azure Active Directory (Azure AD) in Azure gekoppelt. Office 365 GCC High und Office 365 DoD sind mit Azure AD in Azure Government gekoppelt.

> [!TIP]
> Achten Sie unbedingt auf die Azure-Umgebung, um zu verstehen, wo [Interoperabilität möglich ist](#microsoft-365-integration). In der folgenden Tabelle wird die Interoperabilität, die *nicht* möglich ist, mit einem Bindestrich (-) markiert, um anzugeben, dass die Unterstützung nicht relevant ist.
>

| Connector | Azure  | Azure Government  |
| ------------------------------------ | -------------- | ---------------------- |
| **[Dynamics365](../../sentinel/connect-dynamics-365.md)**                               |              |                      |
| - Office 365 GCC |Öffentliche Vorschau | -|
| - Office 365 GCC High | -|Nicht verfügbar |
| - Office 365 DoD |- | Nicht verfügbar|
| **[Microsoft 365 Defender](../../sentinel/connect-microsoft-365-defender.md)**                             |              |                      |
| - Office 365 GCC | Öffentliche Vorschau| -|
| - Office 365 GCC High |- |Nicht verfügbar |
| - Office 365 DoD |- | Nicht verfügbar|
| **[Microsoft Cloud App Security (MCAS)](../../sentinel/connect-cloud-app-security.md)**                                      |              |                      |
| - Office 365 GCC | Allgemein verfügbar| -|
| - Office 365 GCC High |-|Allgemein verfügbar |
| - Office 365 DoD |- |Allgemein verfügbar |
| **[Microsoft Cloud App Security (MCAS)](../../sentinel/connect-cloud-app-security.md)** <br>Schatten-IT-Protokolle                                  |              |                      |
| - Office 365 GCC | Öffentliche Vorschau| -|
| - Office 365 GCC High |-|Öffentliche Vorschau |
| - Office 365 DoD |- |Öffentliche Vorschau |
| **[Microsoft Cloud App Security (MCAS)](../../sentinel/connect-cloud-app-security.md)**                  <br>Alerts                    |              |                      |
| - Office 365 GCC | Öffentliche Vorschau| -|
| - Office 365 GCC High |-|Öffentliche Vorschau |
| - Office 365 DoD |- |Öffentliche Vorschau |
| **[Microsoft Defender für den Endpunkt](../../sentinel/connect-microsoft-defender-advanced-threat-protection.md)**                                       |              |                      |
| - Office 365 GCC | Allgemein verfügbar|- |
| - Office 365 GCC High |- |Nicht verfügbar |
| - Office 365 DoD |- | Nicht verfügbar|
| **[Microsoft Defender for Identity](../../sentinel/connect-azure-atp.md)**                                        |              |                      |
| - Office 365 GCC |Öffentliche Vorschau | -|
| - Office 365 GCC High |- | Nicht verfügbar |
| - Office 365 DoD |- |Nicht verfügbar |
| **[Microsoft Defender für Office 365](../../sentinel/connect-office-365-advanced-threat-protection.md)**               |              |                      |
| - Office 365 GCC |Öffentliche Vorschau |- |
| - Office 365 GCC High |- |Nicht verfügbar |
| - Office 365 DoD | -|Nicht verfügbar |
| **[Office 365](../../sentinel/connect-office-365.md)**                                      |              |                      |
| - Office 365 GCC | Allgemein verfügbar|- |
| - Office 365 GCC High |- |Allgemein verfügbar |
| - Office 365 DoD |- |Allgemein verfügbar |
| | |


## <a name="next-steps"></a>Nächste Schritte

- Grundlegendes zum Modell [gemeinsamer Verantwortung](shared-responsibility.md) sowie welche Sicherheitsaufgaben vom Cloudanbieter und welche Aufgaben von Ihnen verarbeitet werden.
- Grundlegendes zu den Funktionen der [Azure Government Cloud](../../azure-government/documentation-government-welcome.md) und dem vertrauenswürdigen Design und der Sicherheit, die zur Unterstützung der Compliance für Bundes-, Staats- und lokale Behörden und deren Partner verwendet werden.
- Grundlegendes zum [Office 365 Government-Plan](/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/office-365-us-government#about-office-365-government-environments).
- Grundlegendes zur [Compliance in Azure](../../compliance/index.yml) für rechtliche und gesetzliche Standards.
