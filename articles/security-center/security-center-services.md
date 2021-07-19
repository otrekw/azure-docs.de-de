---
title: Azure Security Center-Funktionen nach Betriebssystem, Computertyp und Cloud
description: Hier erfahren Sie, welche Azure Security Center-Funktionen abhängig von Betriebssystem, Typ und Cloudbereitstellung verfügbar sind.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: overview
ms.date: 06/17/2021
ms.custom: references_regions
ms.author: memildin
ms.openlocfilehash: 05b535a0aabe0108c0a9abcedef166293541d407
ms.sourcegitcommit: a038863c0a99dfda16133bcb08b172b6b4c86db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113005472"
---
# <a name="feature-coverage-for-machines"></a>Funktionsabdeckung für Computer

Die beiden folgenden Registerkarten enthalten die Azure Security Center-Features, die für virtuelle Computer und Server unter Windows und Linux verfügbar sind.

## <a name="supported-features-for-virtual-machines-and-servers"></a>Unterstützte Funktionen für virtuelle Computer und Server <a name="vm-server-features"></a>

### <a name="windows-machines"></a>[**Windows-Computer**](#tab/features-windows)

|**Feature**|**Dokumentation zu virtuellen Computern**|**Azure Virtual Machine Scale Sets**|**Computer mit Azure Arc-Unterstützung**|**Azure Defender erforderlich**
|----|:----:|:----:|:----:|:----:|
|[Integration in Microsoft Defender für Endpunkt](security-center-wdatp.md)|✔</br>(für unterstützte Versionen)|✔</br>(für unterstützte Versionen)|✔|Ja|
|[Virtual Machine-Verhaltensanalysen (und Sicherheitswarnungen)](alerts-reference.md)|✔|✔|✔|Ja|
|[Dateilose Sicherheitswarnungen](alerts-reference.md#alerts-windows)|✔|✔|✔|Ja|
|[Netzwerkbasierte Sicherheitswarnungen](other-threat-protections.md#network-layer)|✔|✔|-|Ja|
|[Just-in-Time-VM-Zugriff](security-center-just-in-time.md)|✔|-|-|Ja|
|[Native Sicherheitsrisikobewertung](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner)|✔|-|✔|Ja|
|[Dateiintegritätsüberwachung](security-center-file-integrity-monitoring.md)|✔|✔|✔|Ja|
|[Adaptive Anwendungssteuerungen](security-center-adaptive-application.md)|✔|-|✔|Ja|
|[Netzwerkübersicht](security-center-network-recommendations.md#network-map)|✔|✔|-|Ja|
|[Adaptives Erhöhen des Netzwerkschutzes](security-center-adaptive-network-hardening.md)|✔|-|-|Ja|
|[Dashboard und Berichte für die Einhaltung gesetzlicher Bestimmungen](security-center-compliance-dashboard.md)|✔|✔|✔|Ja|
|Empfehlungen und Bedrohungsschutz für in Docker gehostete IaaS-Container|-|-|-|Ja|
|Fehlende Bewertung von BS-Patches|✔|✔|✔|Azure: Nein<br><br>Arc-Unterstützung: Ja|
|Bewertung von Sicherheitsfehlkonfigurationen|✔|✔|✔|Azure: Nein<br><br>Arc-Unterstützung: Ja|
|[Endpoint Protection-Bewertung](security-center-services.md#supported-endpoint-protection-solutions-)|✔|✔|✔|Azure: Nein<br><br>Arc-Unterstützung: Ja|
|Bewertung der Datenträgerverschlüsselung|✔</br>(für [unterstützte Szenarien](../virtual-machines/windows/disk-encryption-windows.md#unsupported-scenarios))|✔|-|Nein|
|Sicherheitsrisikobewertung durch Drittanbieter|✔|-|✔|Nein|
|[Netzwerksicherheitsbewertung](security-center-network-recommendations.md)|✔|✔|-|Nein|


### <a name="linux-machines"></a>[**Linux-Computer**](#tab/features-linux)

|**Feature**|**Dokumentation zu virtuellen Computern**|**Azure Virtual Machine Scale Sets**|**Computer mit Azure Arc-Unterstützung**|**Azure Defender erforderlich**
|----|:----:|:----:|:----:|:----:|
|[Integration in Microsoft Defender für Endpunkt](security-center-wdatp.md)|-|-|-|-|
|[Virtual Machine-Verhaltensanalysen (und Sicherheitswarnungen)](./azure-defender.md)|✔</br>(für unterstützte Versionen)|✔</br>(für unterstützte Versionen)|✔|Ja|
|[Dateilose Sicherheitswarnungen](alerts-reference.md#alerts-windows)|-|-|-|Ja|
|[Netzwerkbasierte Sicherheitswarnungen](other-threat-protections.md#network-layer)|✔|✔|-|Ja|
|[Just-in-Time-VM-Zugriff](security-center-just-in-time.md)|✔|-|-|Ja|
|[Native Sicherheitsrisikobewertung](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner)|✔|-|✔|Ja|
|[Dateiintegritätsüberwachung](security-center-file-integrity-monitoring.md)|✔|✔|✔|Ja|
|[Adaptive Anwendungssteuerungen](security-center-adaptive-application.md)|✔|-|✔|Ja|
|[Netzwerkübersicht](security-center-network-recommendations.md#network-map)|✔|✔|-|Ja|
|[Adaptives Erhöhen des Netzwerkschutzes](security-center-adaptive-network-hardening.md)|✔|-|-|Ja|
|[Dashboard und Berichte für die Einhaltung gesetzlicher Bestimmungen](security-center-compliance-dashboard.md)|✔|✔|✔|Ja|
|Empfehlungen und Bedrohungsschutz für in Docker gehostete IaaS-Container|✔|✔|✔|Ja|
|Fehlende Bewertung von BS-Patches|✔|✔|✔|Azure: Nein<br><br>Arc-Unterstützung: Ja|
|Bewertung von Sicherheitsfehlkonfigurationen|✔|✔|✔|Azure: Nein<br><br>Arc-Unterstützung: Ja|
|[Endpoint Protection-Bewertung](security-center-services.md#supported-endpoint-protection-solutions-)|-|-|-|Nein|
|Bewertung der Datenträgerverschlüsselung|✔</br>(für [unterstützte Szenarien](../virtual-machines/windows/disk-encryption-windows.md#unsupported-scenarios))|✔|-|Nein|
|Sicherheitsrisikobewertung durch Drittanbieter|✔|-|✔|Nein|
|[Netzwerksicherheitsbewertung](security-center-network-recommendations.md)|✔|✔|-|Nein|

--- 


> [!TIP]
>Wenn Sie mit Features experimentieren möchten, die nur in Azure Defender verfügbar sind, können Sie sich für eine 30-tägige Testversion registrieren. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/security-center/).


## <a name="supported-endpoint-protection-solutions"></a>Unterstützte Endpoint Protection-Lösungen <a name="endpoint-supported"></a>

In der folgenden Tabelle finden Sie eine Matrix zu folgenden Fragen:

 - Ob Sie mit Azure Security Center jede Lösung für sich installieren können.
 - Welche Endpunktschutz-Lösungen Security Center erkennen kann. Wenn eine Endpoint Protection-Lösungen aus dieser Liste ermittelt wird, empfiehlt Security Center nicht deren Installation.

Informationen darüber, wann Empfehlungen für die einzelnen Schutzfunktionen generiert werden, finden Sie unter [Endpoint Protection: Bewertung und Empfehlungen](security-center-endpoint-protection.md).

| Endpoint Protection| Plattformen | Security Center-Installation | Security Center-Ermittlung |
|------|------|-----|-----|
| Microsoft Defender Antivirus| Windows Server 2016 oder höher| Nein, in Betriebssystem integriert| Ja |
| System Center Endpoint Protection (Microsoft Antimalware) | Windows Server 2012 R2, 2012, 2008 R2 (siehe Hinweis unten) | Per Erweiterung | Ja |
| Trend Micro – Deep Security | Windows Server-Familie  | Nein | Ja |
| Symantec v12.1.1100+| Windows Server-Familie  | Nein | Ja |
| McAfee v10+ | Windows Server-Familie  | Nein | Ja |
| McAfee v10+ | Linux-Serverfamilie  | Nein | Ja |
| Sophos V9+| Linux-Serverfamilie  | Nein | Ja |

> [!NOTE]
> Für die Erkennung von System Center Endpoint Protection (SCEP) auf einem virtuellen Computer mit Windows Server 2008 R2 muss SCEP nach PowerShell (ab Version 3.0) installiert werden.



## <a name="feature-support-in-government-and-sovereign-clouds"></a>Funktionsunterstützung in Government- und Sovereign Clouds

| Funktion/Dienst                                                                                                                                                             | Azure          | US Government                  | Azure China   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|--------------------------------|---------------|
| **Security Center: kostenlose Funktionen**                                                                                                                                           |                |                                |               |
| - [Fortlaufender Export](/azure/security-center/continuous-export)                                                                                                             | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Workflowautomatisierung](/azure/security-center/continuous-export)                                                                                                           | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Empfehlungsausnahmeregeln](/azure/security-center/exempt-resource)                                                                                                  | Öffentliche Vorschau | Nicht verfügbar                  | Nicht verfügbar. |
| - [Warnungsunterdrückungsregeln](/azure/security-center/alerts-suppression-rules)                                                                                                | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [E-Mail-Benachrichtigungen für Sicherheitswarnungen](/azure/security-center/security-center-provide-security-contact-details)                                                        | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Automatische Bereitstellung für Agents und Erweiterungen](/azure/security-center/security-center-enable-data-collection)                                                              | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Ressourcenbestand](/azure/security-center/asset-inventory)                                                                                                                 | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Azure Monitor-Arbeitsmappenberichte im Azure Security Center-Arbeitsmappenkatalog](/azure/security-center/custom-dashboards-azure-workbooks)                                  | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| **Azure Defender-Pläne und Erweiterungen**                                                                                                                                     |                |                                |               |
| - [Azure Defender für Server](/azure/security-center/defender-for-servers-introduction)                                                                                    | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Azure Defender für App Service](/azure/security-center/defender-for-app-service-introduction)                                                                            | Allgemein verfügbar             | Nicht verfügbar                  | Nicht verfügbar. |
| - [Azure Defender für DNS](/azure/security-center/defender-for-dns-introduction)                                                                                            | Allgemein verfügbar             | Nicht verfügbar                  | Nicht verfügbar. |
| - [Azure Defender für Containerregistrierungen](/azure/security-center/defender-for-container-registries-introduction) <sup>[1](#footnote1)</sup>                               | Allgemein verfügbar             | Allgemein verfügbar <sup>[2](#footnote2)</sup> | Allgemein verfügbar <sup>[2](#footnote2)</sup> |
| - [Azure Defender für Containerregistrierungen zum Überprüfen von Images in CI/CD-Workflows](/azure/security-center/defender-for-container-registries-cicd) <sup>[3](#footnote3)</sup> | Öffentliche Vorschau | Nicht verfügbar                  | Nicht verfügbar. |
| - [Azure Defender für Kubernetes](/azure/security-center/defender-for-kubernetes-introduction) <sup>[4](#footnote4)</sup>                                                   | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Azure Defender-Erweiterung für Azure Arc-fähige Kubernetes-Cluster ](/azure/security-center/defender-for-kubernetes-azure-arc) <sup>[5](#footnote5)</sup>                 | Öffentliche Vorschau | Nicht verfügbar                  | Nicht verfügbar. |
| - [Azure Defender für Azure SQL-Datenbank-Server](/azure/security-center/defender-for-sql-introduction)                                                                     | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar <sup>[9](#footnote9)</sup> |
| - [Azure Defender für SQL-Server auf Computern](/azure/security-center/defender-for-sql-introduction)                                                                        | Allgemein verfügbar             | Allgemein verfügbar                             | Nicht verfügbar |
| - [Azure Defender für relationale Open-Source-Datenbanken](/azure/security-center/defender-for-databases-introduction)                                                         | Allgemein verfügbar             | Nicht verfügbar                  | Nicht verfügbar. |
| - [Azure Defender für Key Vault](/azure/security-center/defender-for-key-vault-introduction)                                                                                | Allgemein verfügbar             | Nicht verfügbar                  | Nicht verfügbar. |
| - [Azure Defender für Resource Manager](/azure/security-center/defender-for-resource-manager-introduction)                                                                  | Allgemein verfügbar             | Öffentliche Vorschau                 | Nicht verfügbar |
| - [Azure Defender für Storage](/azure/security-center/defender-for-storage-introduction) <sup>[6](#footnote6)</sup>                                                         | Allgemein verfügbar             | Allgemein verfügbar                             | Nicht verfügbar |
| - [Bedrohungsschutz für Cosmos DB](/azure/security-center/other-threat-protections.md#threat-protection-for-azure-cosmos-db-preview)                                       | Öffentliche Vorschau | Nicht verfügbar                  | Nicht verfügbar. |
| - [Kubernetes-Workloadschutz](/azure/security-center/kubernetes-workload-protections)                                                                                  | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| **Azure Defender für Serverfunktionen** <sup>[7](#footnote7)</sup>                                                                                                          |                |                                |               |
| - [Just-In-Time-VM-Zugriff](/azure/security-center/security-center-just-in-time)                                                                                             | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Dateiintegritätsüberwachung](/azure/security-center/security-center-file-integrity-monitoring)                                                                             | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Adaptive Anwendungssteuerungen](/azure/security-center/security-center-adaptive-application)                                                                              | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Adaptives Erhöhen des Netzwerkschutzes](/azure/security-center/security-center-adaptive-network-hardening)                                                                           | Allgemein verfügbar             | Nicht verfügbar                  | Nicht verfügbar. |
| - [Docker-Hosthärtung](/azure/security-center/harden-docker-hosts)                                                                                                       | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Integrierte Sicherheitsrisikobewertung für Computer](/azure/security-center/deploy-vulnerability-assessment-vm)                                                             | Allgemein verfügbar             | Nicht verfügbar                  | Nicht verfügbar. |
| - [Dashboard und Berichte für die Einhaltung gesetzlicher Bestimmungen](/azure/security-center/security-center-compliance-dashboard) <sup>[8](#footnote8)</sup>                                       | Allgemein verfügbar             | Allgemein verfügbar                             | Allgemein verfügbar            |
| - [Microsoft Defender für Endpunkt: Bereitstellung und integrierte Lizenz](/azure/security-center/security-center-wdatp)                                                         | Allgemein verfügbar             | Allgemein verfügbar                             | Nicht verfügbar |
| - [Verbinden von AWS-Konten](/azure/security-center/quickstart-onboard-aws)                                                                                                      | Allgemein verfügbar             | Nicht verfügbar                  | Nicht verfügbar. |
| - [Verbinden von GCP-Konten](/azure/security-center/quickstart-onboard-gcp)                                                                                                      | Allgemein verfügbar             | Nicht verfügbar                  | Nicht verfügbar. |
|                                                                                                                                                                             |                |                                |

<sup><a name="footnote1" /></a>1</sup> Teilweise allgemeine Verfügbarkeit: Die Möglichkeit, bestimmte Ergebnisse von Sicherheitsrisikoüberprüfungen zu deaktivieren, befindet sich in der öffentlichen Vorschau.

<sup><a name="footnote2" /></a>2</sup> Sicherheitsrisikoüberprüfungen von Containerregistrierungen in Azure Gov können nur mit der Funktion zur Überprüfung beim Pushen durchgeführt werden.

<sup><a name="footnote3" /></a>3</sup> Erfordert Azure Defender für Containerregistrierungen

<sup><a name="footnote4" /></a>4</sup> Teilweise allgemeine Verfügbarkeit: Die Unterstützung für Arc-fähige Cluster befindet sich in der öffentlichen Vorschau und ist für Azure Government nicht verfügbar.

<sup><a name="footnote5" /></a>5</sup> Erfordert Azure Defender für Kubernetes

<sup><a name="footnote6" /></a>6</sup> Teilweise allgemeine Verfügbarkeit: Einige Warnungen zum Bedrohungsschutz von Azure Defender für Storage befinden sich in der öffentlichen Vorschau.

<sup><a name="footnote7" /></a>7</sup> Diese Funktionen erfordern [Azure Defender für Server](/azure/security-center/defender-for-servers-introduction).

<sup><a name="footnote8" /></a>8</sup> Es gibt möglicherweise Unterschiede in den Standards, die für die einzelnen Cloudtypen angeboten werden.
 
<sup><a name="footnote9" /></a>9</sup> Teilweise allgemeine Verfügbarkeit: Teilmenge von Warnungen und Sicherheitsrisikobewertungen für SQL-Server. Verhaltensbedingte Bedrohungsschutzmaßnahmen sind nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Datenerfassung in Security Center mit dem Log Analytics-Agent](security-center-enable-data-collection.md).
- Erfahren Sie, wie [Daten von Security Center verwaltet und geschützt werden](security-center-data-security.md).
- Informieren Sie sich über die [Plattformen, die Security Center unterstützen](security-center-os-coverage.md).