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
ms.date: 07/12/2020
ms.author: memildin
ms.openlocfilehash: 95b514999d7c38140bd3dd275b8ba103ee9af20e
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86519230"
---
# <a name="feature-coverage-for-machines"></a>Funktionsabdeckung für Computer

Die beiden folgenden Registerkarten enthalten die Azure Security Center-Features, die für virtuelle Computer und Server unter Windows und Linux verfügbar sind.

## <a name="supported-features-for-virtual-machines-and-servers"></a>Unterstützte Funktionen für virtuelle Computer und Server <a name="vm-server-features"></a>

### <a name="windows-machines"></a>[**Windows-Computer**](#tab/features-windows)

|**Feature**|**Dokumentation zu virtuellen Computern**|**Azure Virtual Machine Scale Sets**|**Azure-fremde Computer**|**Preise**
|----|:----:|:----:|:----:|:----:|
|[Microsoft Defender ATP-Integration](security-center-wdatp.md)|✔</br>(für unterstützte Versionen)|✔</br>(für unterstützte Versionen)|✔|Standard|
|[Virtual Machine-Verhaltensanalysen (und Sicherheitswarnungen)](threat-protection.md)|✔|✔|✔|Standard|
|[Dateilose Sicherheitswarnungen](alerts-reference.md#alerts-windows)|✔|✔|✔|Standard|
|[Netzwerkbasierte Sicherheitswarnungen](threat-protection.md#network-layer)|✔|✔|-|Standard|
|[Just-in-Time-VM-Zugriff](security-center-just-in-time.md)|✔|-|-|Standard|
|[Native Sicherheitsrisikobewertung](built-in-vulnerability-assessment.md)|✔|-|-|Standard|
|[Dateiintegritätsüberwachung](security-center-file-integrity-monitoring.md)|✔|✔|✔|Standard|
|[Adaptive Anwendungssteuerungen](security-center-adaptive-application.md)|✔|-|✔|Standard|
|[Netzwerkübersicht](security-center-network-recommendations.md#network-map)|✔|✔|-|Standard|
|[Adaptives Erhöhen des Netzwerkschutzes](security-center-adaptive-network-hardening.md)|✔|-|-|Standard|
|[Dashboard und Berichte für die Einhaltung gesetzlicher Bestimmungen](security-center-compliance-dashboard.md)|✔|✔|✔|Standard|
|Empfehlungen und Bedrohungsschutz für in Docker gehostete IaaS-Container|-|-|-|Standard|
|Fehlende Bewertung von BS-Patches|✔|✔|✔|Azure: Kostenlos<br><br>Nicht-Azure: Standard|
|Bewertung von Sicherheitsfehlkonfigurationen|✔|✔|✔|Azure: Kostenlos<br><br>Nicht-Azure: Standard|
|[Endpoint Protection-Bewertung](security-center-services.md#supported-endpoint-protection-solutions-)|✔|✔|✔|Azure: Kostenlos<br><br>Nicht-Azure: Standard|
|Bewertung der Datenträgerverschlüsselung|✔|✔|-|Kostenlos|
|Sicherheitsrisikobewertung durch Drittanbieter|✔|-|-|Kostenlos|
|[Netzwerksicherheitsbewertung](security-center-network-recommendations.md)|✔|✔|-|Kostenlos|


### <a name="linux-machines"></a>[**Linux-Computer**](#tab/features-linux)

|**Feature**|**Dokumentation zu virtuellen Computern**|**Azure Virtual Machine Scale Sets**|**Azure-fremde Computer**|**Preise**
|----|:----:|:----:|:----:|:----:|
|[Microsoft Defender ATP-Integration](security-center-wdatp.md)|-|-|-|Standard|
|[Virtual Machine-Verhaltensanalysen (und Sicherheitswarnungen)](security-center-alerts-iaas.md)|✔</br>(für unterstützte Versionen)|✔</br>(für unterstützte Versionen)|✔|Standard|
|[Dateilose Sicherheitswarnungen](alerts-reference.md#alerts-windows)|-|-|-|Standard|
|[Netzwerkbasierte Sicherheitswarnungen](threat-protection.md#network-layer)|✔|✔|-|Standard|
|[Just-in-Time-VM-Zugriff](security-center-just-in-time.md)|✔|-|-|Standard|
|[Native Sicherheitsrisikobewertung](built-in-vulnerability-assessment.md)|✔|-|-|Standard|
|[Dateiintegritätsüberwachung](security-center-file-integrity-monitoring.md)|✔|✔|✔|Standard|
|[Adaptive Anwendungssteuerungen](security-center-adaptive-application.md)|✔|-|✔|Standard|
|[Netzwerkübersicht](security-center-network-recommendations.md#network-map)|✔|✔|-|Standard|
|[Adaptives Erhöhen des Netzwerkschutzes](security-center-adaptive-network-hardening.md)|✔|-|-|Standard|
|[Dashboard und Berichte für die Einhaltung gesetzlicher Bestimmungen](security-center-compliance-dashboard.md)|✔|✔|✔|Standard|
|Empfehlungen und Bedrohungsschutz für in Docker gehostete IaaS-Container|✔|✔|✔|Standard|
|Fehlende Bewertung von BS-Patches|✔|✔|✔|Azure: Kostenlos<br><br>Nicht-Azure: Standard|
|Bewertung von Sicherheitsfehlkonfigurationen|✔|✔|✔|Azure: Kostenlos<br><br>Nicht-Azure: Standard|
|[Endpoint Protection-Bewertung](security-center-services.md#supported-endpoint-protection-solutions-)|-|-|-|Kostenlos|
|Bewertung der Datenträgerverschlüsselung|✔|✔|-|Kostenlos|
|Sicherheitsrisikobewertung durch Drittanbieter|✔|-|-|Kostenlos|
|[Netzwerksicherheitsbewertung](security-center-network-recommendations.md)|✔|✔|-|Kostenlos|

--- 


> [!TIP]
>Um mit Features zu experimentieren, die nur für den Tarif "Standard" verfügbar sind, können Benutzer im Free-Tarif sich für eine 30-tägige Testversion registrieren. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/security-center/).


## <a name="supported-endpoint-protection-solutions"></a>Unterstützte Endpoint Protection-Lösungen <a name="endpoint-supported"></a>

In der folgenden Tabelle finden Sie eine Matrix zu folgenden Fragen:

 - Ob Sie mit Azure Security Center jede Lösung für sich installieren können.
 - Welche Endpunktschutz-Lösungen Security Center erkennen kann. Wenn eine Endpoint Protection-Lösungen aus dieser Liste ermittelt wird, empfiehlt Security Center nicht deren Installation.

Informationen darüber, wann Empfehlungen für die einzelnen Schutzfunktionen generiert werden, finden Sie unter [Endpoint Protection: Bewertung und Empfehlungen](security-center-endpoint-protection.md).

| Endpoint Protection| Plattformen | Security Center-Installation | Security Center-Ermittlung |
|------|------|-----|-----|
| Windows Defender (Microsoft Antimalware)| Windows Server 2016| Nein, in Betriebssystem integriert| Ja |
| System Center Endpoint Protection (Microsoft Antimalware) | Windows Server 2012 R2, 2012, 2008 R2 (siehe Hinweis unten) | Per Erweiterung | Ja |
| Trend Micro – Deep Security | Windows Server-Familie  | Nein | Ja |
| Symantec v12.1.1100+| Windows Server-Familie  | Nein | Ja |
| McAfee v10+ | Windows Server-Familie  | Nein | Ja |
| McAfee v10+ | Linux-Serverfamilie  | Nein | Ja **\*** |
| Sophos V9+| Linux-Serverfamilie  | Nein | Ja **\***  |

 **\*** Der Abdeckungsstand und die unterstützenden Daten sind zurzeit nur im Log Analytics-Arbeitsbereich verfügbar, der Ihren geschützten Abonnements zugeordnet ist. Sie spiegeln sich nicht im Azure Security Center-Portal wider.

> [!NOTE]
> Für die Erkennung von System Center Endpoint Protection (SCEP) auf einem virtuellen Computer mit Windows Server 2008 R2 muss SCEP nach PowerShell (ab Version 3.0) installiert werden.


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Datenerfassung in Security Center und den Log Analytics-Agent](security-center-enable-data-collection.md).
- Erfahren Sie, wie [Daten von Security Center verwaltet und geschützt werden](security-center-data-security.md).
- Informieren Sie sich über die [Plattformen, die Security Center unterstützen](security-center-os-coverage.md).