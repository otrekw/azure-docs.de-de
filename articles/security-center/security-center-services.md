---
title: Verfügbare unterstützte Features in Azure Security Center | Microsoft-Dokumentation
description: Dieses Dokument enthält eine Liste der von Azure Security Center unterstützten Dienste.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 870ebc8d-1fad-435b-9bf9-c477f472ab17
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: 8d905da733ea0573b91b289da43684ed8083c804
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77616383"
---
# <a name="supported-features-available-in-azure-security-center"></a>Verfügbare unterstützte Features in Azure Security Center

> [!NOTE]
>Einige Features sind nur beim Standardtarif verfügbar. Wenn Sie sich noch nicht für den Security Center-Standardtarif registriert haben, können Sie einen kostenlosen Testzeitraum nutzen. Weitere Informationen finden Sie auf der Seite mit den [Preisen für Security Center](https://azure.microsoft.com/pricing/details/security-center/).

Die folgenden Abschnitte enthalten Security Center-Features, die für die [unterstützten Plattformen](security-center-os-coverage.md) verfügbar sind.

* [Virtuelle Computer/Server](#vm-server-features)
* [PaaS-Dienste](#paas-services)


## Virtuelle Computer/Server – unterstützte Features <a name="vm-server-features"></a>

### <a name="windows"></a>[Windows](#tab/features-windows)

|||||||||
|----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
||**Dokumentation zu virtuellen Computern**|**Azure Virtual Machine Scale Sets**|**Azure-fremde Computer**|**Preise**
|[Microsoft Defender ATP-Integration](security-center-wdatp.md)|✔ (für unterstützte Versionen)|✔ (für unterstützte Versionen)|✔|Standard|
|[Warnungen zur Bedrohungserkennung für Virtual Machine-Verhaltensanalysen](threat-protection.md)|✔|✔|✔|Empfehlungen (kostenlos) Bedrohungserkennung (Standard)|
|[Warnungen der Erkennung von Fileless-Bedrohungen](alerts-reference.md#alerts-windows)|✔|✔|✔|Standard|
|[Warnungen der netzwerkbasierten Bedrohungserkennung](threat-protection.md#network-layer)|✔|✔|-|Standard|
|[Just-in-Time-VM-Zugriff](security-center-just-in-time.md)|✔|-|-|Standard|
|[Native Sicherheitsrisikobewertung](built-in-vulnerability-assessment.md)|✔|-|-|Standard|
|[Dateiintegritätsüberwachung](security-center-file-integrity-monitoring.md)|✔|✔|✔|Standard|
|[Adaptive Anwendungssteuerungen](security-center-adaptive-application.md)|✔|-|✔|Standard|
|[Netzwerkübersicht](security-center-network-recommendations.md#network-map)|✔|✔|-|Standard|
|[Adaptives Erhöhen des Netzwerkschutzes](security-center-adaptive-network-hardening.md)|✔|-|-|Standard|
|Adaptive Netzwerksteuerungen|✔|✔|-|Standard|
|[Dashboard und Berichte für die Einhaltung gesetzlicher Bestimmungen](security-center-compliance-dashboard.md)|✔|✔|✔|Standard|
|Empfehlungen und Bedrohungserkennung für in Docker gehostete IaaS-Container|-|-|-|Standard|
|Fehlende Bewertung von BS-Patches|✔|✔|✔|Kostenlos|
|Bewertung von Sicherheitsfehlkonfigurationen|✔|✔|✔|Kostenlos|
|[Endpoint Protection-Bewertung](security-center-services.md#supported-endpoint-protection-solutions-)|✔|✔|✔|Kostenlos|
|Bewertung der Datenträgerverschlüsselung|✔|✔|-|Kostenlos|
|Sicherheitsrisikobewertung durch Drittanbieter|✔|-|-|Kostenlos|
|[Netzwerksicherheitsbewertung](security-center-network-recommendations.md)|✔|✔|-|Kostenlos|


### <a name="linux"></a>[Linux](#tab/features-linux)

|||||||||
|----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
||**Dokumentation zu virtuellen Computern**|**Azure Virtual Machine Scale Sets**|**Azure-fremde Computer**|**Preise**
|[Microsoft Defender ATP-Integration](security-center-wdatp.md)|-|-|-|Standard|
|[Warnungen zur Bedrohungserkennung für Virtual Machine-Verhaltensanalysen](security-center-alerts-iaas.md)|✔ (für unterstützte Versionen)|✔ (für unterstützte Versionen)|✔|Empfehlungen (kostenlos) Bedrohungserkennung (Standard)|
|[Warnungen der Erkennung von Fileless-Bedrohungen](alerts-reference.md#alerts-windows)|-|-|-|Standard|
|[Warnungen der netzwerkbasierten Bedrohungserkennung](threat-protection.md#network-layer)|✔|✔|-|Standard|
|[Just-in-Time-VM-Zugriff](security-center-just-in-time.md)|✔|-|-|Standard|
|[Native Sicherheitsrisikobewertung](built-in-vulnerability-assessment.md)|✔|-|-|Standard|
|[Dateiintegritätsüberwachung](security-center-file-integrity-monitoring.md)|✔|✔|✔|Standard|
|[Adaptive Anwendungssteuerungen](security-center-adaptive-application.md)|✔|-|✔|Standard|
|[Netzwerkübersicht](security-center-network-recommendations.md#network-map)|✔|✔|-|Standard|
|[Adaptives Erhöhen des Netzwerkschutzes](security-center-adaptive-network-hardening.md)|✔|-|-|Standard|
|Adaptive Netzwerksteuerungen|✔|✔|-|Standard|
|[Dashboard und Berichte für die Einhaltung gesetzlicher Bestimmungen](security-center-compliance-dashboard.md)|✔|✔|✔|Standard|
|Empfehlungen und Bedrohungserkennung für in Docker gehostete IaaS-Container|✔|✔|✔|Standard|
|Fehlende Bewertung von BS-Patches|✔|✔|✔|Kostenlos|
|Bewertung von Sicherheitsfehlkonfigurationen|✔|✔|✔|Kostenlos|
|[Endpoint Protection-Bewertung](security-center-services.md#supported-endpoint-protection-solutions-)|-|-|-|Kostenlos|
|Bewertung der Datenträgerverschlüsselung|✔|✔|-|Kostenlos|
|Sicherheitsrisikobewertung durch Drittanbieter|✔|-|-|Kostenlos|
|[Netzwerksicherheitsbewertung](security-center-network-recommendations.md)|✔|✔|-|Kostenlos|

--- 

## Unterstützte Endpoint Protection-Lösungen <a name="endpoint-supported"></a>

In der folgenden Tabelle finden Sie eine Matrix zu folgenden Fragen:

 - Ob Sie mit Azure Security Center jede Lösung für sich installieren können.
 - Welche Endpunktschutz-Lösungen Security Center erkennen kann. Wenn eine Endpoint Protection-Lösungen aus dieser Liste ermittelt wird, empfiehlt Security Center nicht deren Installation.

Informationen darüber, wann Empfehlungen für die einzelnen Schutzfunktionen generiert werden, finden Sie unter [Endpoint Protection: Bewertung und Empfehlungen](security-center-endpoint-protection.md).

| Endpoint Protection| Plattformen | Security Center-Installation | Security Center-Ermittlung |
|------|------|-----|-----|
| Windows Defender (Microsoft Antimalware)| Windows Server 2016| Nein, in Betriebssystem integriert| Ja |
| System Center Endpoint Protection (Microsoft Antimalware) | Windows Server 2012 R2, 2012, 2008 R2 (siehe Hinweis unten) | Per Erweiterung | Ja |
| Trend Micro – Alle Versionen* | Windows Server-Familie  | Nein | Ja |
| Symantec v12.1.1100+| Windows Server-Familie  | Nein | Ja |
| McAfee v10+ | Windows Server-Familie  | Nein | Ja |
| McAfee v10+ | Linux-Serverfamilie  | Nein | Ja **\*** |
| Sophos V9+| Linux-Serverfamilie  | Nein | Ja **\***  |

 **\*** Der Abdeckungsstand und die unterstützenden Daten sind zurzeit nur im Log Analytics-Arbeitsbereich verfügbar, der Ihren geschützten Abonnements zugeordnet ist. Sie spiegeln sich nicht im Azure Security Center-Portal wider.

> [!NOTE]
> - Für die Erkennung von System Center Endpoint Protection (SCEP) auf einem virtuellen Computer mit Windows Server 2008 R2 muss SCEP nach PowerShell 3.0 (oder einer höheren Version) installiert werden.
> - Die Erkennung von Trend Micro-Schutz wird für Deep Security-Agents unterstützt.  OfficeScan-Agents werden nicht unterstützt.


## PaaS-Dienste – unterstützte Features <a name="paas-services"> </a>

Die folgenden PaaS-Ressourcen werden von Azure Security Center unterstützt:

|Dienst|Empfehlungen (kostenlos)|Warnungen der Bedrohungserkennung (Standard)|Sicherheitsrisikobewertung (Standard)|
|----|:----:|:----:|:----:|
|SQL-DATENBANKEN|✔|✔|✔|
|Azure Container Registry|-|-|✔|
|Azure Kubernetes Service|✔|✔|-|
|Azure Database for PostgreSQL*|✔|✔|-|
|Azure Database for MySQL*|✔|✔|-|
|Azure CosmosDB*|-|✔|-|
|Speicherkonten|✔|-|-|
|Blob Storage|✔|✔|-|
|App Service|✔|✔|-|
|Funktionen-App|✔|-|-|
|Cloud Services|✔|-|-|
|Virtual Network|✔|-|-|
|Subnet|✔|-|-|
|NIC|✔|-|-|
|Netzwerksicherheitsgruppen|✔|-|-|
|Subscription|✔ **|✔|-|
|Batch-Konto|✔|-|-|
|Service Fabric-Konto|✔|-|-|
|Automation-Konto|✔|-|-|
|Load Balancer|✔|-|-|
|Cognitive Search|✔|-|-|
|Service Bus-Namespace|✔|-|-|
|Stream Analytics|✔|-|-|
|Event Hub-Namespace|✔|-|-|
|Logik-Apps|✔|-|-|
|Cache for Redis|✔|-|-|
|Data Lake Analytics|✔|-|-|
|Azure Data Lake Storage|✔|-|-|
|Key Vault|✔|✔ *|-|

\* Diese Features werden derzeit in der Vorschau unterstützt.

\*\* Empfehlungen zu Azure Active Directory (Azure AD) sind nur für Standardabonnements verfügbar.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Datenerfassung in Security Center und den Log Analytics-Agent](security-center-enable-data-collection.md).
- Erfahren Sie, wie [Daten von Security Center verwaltet und geschützt werden](security-center-data-security.md).
- Hier erfahren Sie, wie Sie [die Entwurfsaspekte in Bezug auf die Einführung von Azure Security Center planen und verstehen](security-center-planning-and-operations-guide.md).
- Informieren Sie sich über die [Plattformen, die Security Center unterstützen](security-center-os-coverage.md).
- Erfahren Sie mehr über die [Bedrohungserkennung für virtuelle Computer und Server in Azure Security Center](security-center-alerts-iaas.md).
- Lesen Sie [Häufig gestellte Fragen zu Azure Security Center](faq-general.md).
- Lesen Sie [Blogbeiträge zur Sicherheit und Compliance von Azure](https://blogs.msdn.com/b/azuresecurity/).