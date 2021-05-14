---
title: Konfigurieren von Azure Active Directory, um die FedRAMP High-Auswirkungsstufe zu erfüllen
description: Übersicht darüber, wie Sie mithilfe von Azure Active Directory eine FedRAMP High-Auswirkungsstufe für Ihre Organisation erreichen können.
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: barbaraselden
ms.author: baselden
manager: mtillman
ms.reviewer: martinco
ms.date: 4/26/2021
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 965fbe7ec1f46e006e30d721bf306a0debaf8473
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108294030"
---
# <a name="configure-azure-active-directory-to-meet-fedramp-high-impact-level"></a>Konfigurieren von Azure Active Directory, um die FedRAMP High-Auswirkungsstufe zu erfüllen

Das [Federal Risk and Authorization Management Program](https://www.fedramp.gov/) (FedRAMP) ist ein Bewertungs- und Autorisierungsprozess für Clouddienstanbieter (Cloud Service Providers, CSPs), die Cloudlösungsangebote (Cloud Solution Offerings, CSOs) für die Verwendung von Bundesbehörden erstellen. Azure und Azure Government haben vom Joint Authorization Board, dem höchsten Organ für die FedRAMP-Zulassung, eine [vorübergehende Betriebsgenehmigung (Provisional Authority to Operate, P-ATO)](https://docs.microsoft.com/compliance/regulatory/offering-fedramp) auf der High-Auswirkungsebene erhalten.

Azure bietet die Möglichkeit, alle Steuerungsanforderungen zu erfüllen, um eine hohe FedRAMP-Bewertung für Ihr CSO oder als Bundesorganisation zu erzielen. Es liegt in der Verantwortung Ihrer Organisation, zusätzliche Konfigurationen oder Prozesse zu erfüllen, um konform zu sein. Diese Verantwortung gilt sowohl für CSPs, die eine hohe FedRAMP-Autorisierung für ihr CSO beantragen, als auch für Bundesbehörden, die eine Autorität für den Betrieb (Authority to Operate, ATO) beantragen. 

## <a name="microsoft-and-fedramp"></a>Microsoft und FedRAMP 

Microsoft Azure unterstützt mehr Dienste auf [FedRAMP High](https://docs.microsoft.com/azure/azure-government/compliance/azure-services-in-fedramp-auditscope)-Auswirkungsebenen als jeder andere CSP. Und während FedRAMP High in der öffentlichen Azure-Cloud die Anforderungen vieler US-Behördenkunden erfüllt, können Behörden mit strengeren Anforderungen auf die Azure Government-Cloud zurückgreifen. Die Azure Government-Cloud bietet zusätzliche Sicherheitsvorkehrungen, z. B. die verstärkte Personalüberprüfung. 

Microsoft muss seine Clouddienste jedes Jahr neu zertifizieren, um seine Autorisierungen zu verwalten. Zu diesem Ziel überwacht und bewertet Microsoft kontinuierlich seine Sicherheitskontrollen und zeigt, dass die Sicherheit seiner Dienste konform bleibt.

* [FedRAMP-Autorisierungen für Microsoft-Clouddienste](https://marketplace.fedramp.gov/)

* [Microsoft FedRAMP-Überwachungsberichte](https://aka.ms/MicrosoftFedRAMPAuditDocuments)

Um andere FedRAMP-Berichte zu erhalten, senden Sie eine E-Mail an [Azure Federal Documentation](mailto:AzFedDoc@microsoft.com).

Es gibt mehrere Pfade zur FedRAMP-Autorisierung. Sie können das vorhandene Autorisierungspaket von Microsoft Azure und die hier enthaltenen Leitfäden wiederverwenden, um Zeit und Aufwand für den Erhalt eines ATO oder P-ATO erheblich zu reduzieren. Weitere Informationen zu FedRAMP finden Sie auf der [FedRAMP-Website.](https://www.fedramp.gov/)

 ## <a name="scope-of-guidance"></a>Umfang der Leitfäden

Die FedRAMP High Baseline besteht aus 421 Steuerelementen und Kontrollerweiterungen von [NIST 800-53 Security Controls Catalog Revision 4](https://csrc.nist.gov/publications/detail/sp/800-53/rev-4/final). Sofern zutreffend, haben wir klarstellende Informationen aus der [800-53 Revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) hinzugefügt. Dieser Artikelsatz behandelt eine Teilmenge dieser Steuerelemente, die sich auf die Identität beziehen und die Sie konfigurieren müssen. Wir bieten einen ausführlichen Leitfaden, der Sie bei der Einhaltung von Kontrollen unterstützt, für die Sie für die Konfiguration in Azure Active Directory (Azure AD) verantwortlich sind. Um einige Anforderungen an die Identitätssteuerung vollständig zu erfüllen, müssen Sie möglicherweise andere Systeme verwenden. Andere Systeme können ein SIEM-Tool (Security Information and Event Management) enthalten, z. B. Azure Sentinel. Wenn Sie Azure-Dienste außerhalb von Azure Active Directory verwenden, gibt es weitere Steuerelemente, die Sie berücksichtigen müssen, und Sie können die Funktionen nutzen, über die Azure bereits verfügt, um die Steuerelemente zu erfüllen.

FedRAMP-Ressourcen

* [Federal Risk and Authorization Management Program (FedRAMP)](https://www.fedramp.gov/)

* [FedRAMP Security Assessment Framework](https://www.fedramp.gov/assets/resources/documents/FedRAMP_Security_Assessment_Framework.pdf)

* [Agency Guide for FedRAMP Authorizations](https://www.fedramp.gov/assets/resources/documents/Agency_Guide_for_Reuse_of_FedRAMP_Authorizations.pdf)

* [Verwalten von Compliance in der Cloud bei Microsoft](https://www.microsoft.com/trustcenter/common-controls-hub)

* [Microsoft Government Cloud](https://go.microsoft.com/fwlink/p/?linkid=2087246)

* [Complianceangebote von Azure](https://aka.ms/azurecompliance)

* [Blaupausenbeispiel „FedRAMP High“ – Übersicht](https://docs.microsoft.com/azure/governance/blueprints/samples/fedramp-h/)

* [Microsoft 365 Compliance Center](https://docs.microsoft.com///microsoft-365/compliance/microsoft-365-compliance-center)

* [Microsoft Compliance Manager](https://docs.microsoft.com///microsoft-365/compliance/compliance-manager)

 

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren der Zugriffssteuerung](fedramp-access-controls.md)

[Konfigurieren von Identifikations- und Authentifizierungskontrollen](fedramp-identification-and-authentication-controls.md)

[Konfigurieren anderer Steuerelemente](fedramp-other-controls.md)

 
