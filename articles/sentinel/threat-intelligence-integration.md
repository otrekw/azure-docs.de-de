---
title: Threat Intelligence-Integration in Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie mehr über die verschiedenen Möglichkeiten, wie Threat Intelligence-Feeds in Azure Sentinel integriert und von Azure Sentinel verwendet wird.
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2021
ms.author: yelevin
ms.openlocfilehash: d8f7fff97a92a98fbcd0198ce601c88df914d9e2
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110471482"
---
# <a name="threat-intelligence-integration-in-azure-sentinel"></a>Threat Intelligence-Integration in Azure Sentinel

> [!IMPORTANT]
> Die Threat Intelligence-Datenconnectors in Azure Sentinel befinden sich aktuell in der Public Preview-Phase.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel bietet Ihnen verschiedene Möglichkeiten zur [Verwendung von Threat Intelligence-Feeds](import-threat-intelligence.md), damit Ihre Sicherheitsanalysten besser in der Lage sind, bekannte Bedrohungen zu erkennen und zu priorisieren. 

Sie können eines von vielen verfügbaren Produkten der integrierten Threat Intelligence Platform (TIP) verwenden, eine Verbindung mit TAXII-Servern herstellen, um von STIX-kompatiblen Threat Intelligence-Quellen zu profitieren, sowie benutzerdefinierte Lösungen nutzen, die direkt mit der [Microsoft Graph-Sicherheits-API „tiIndicators“](/graph/api/resources/tiindicator) kommunizieren können.

Sie können auch über Playbooks eine Verbindung mit Threat Intelligence-Quellen herstellen, um Incidents mit TI-Informationen anzureichern, die direkte Aktionen für Untersuchung und Reaktion unterstützen können.

## <a name="taxii-threat-intelligence-feeds"></a>TAXII-Threat Intelligence-Feeds

Verwenden Sie zum Herstellen einer Verbindung mit TAXII-Threat Intelligence-Feeds den Datenconnector [Threat Intelligence – TAXII](connect-threat-intelligence.md#connect-azure-sentinel-to-taxii-servers) zusammen mit den von den unten verlinkten Anbietern bereitgestellten Daten. Möglicherweise müssen Sie sich direkt an den Anbieter wenden, um die erforderlichen Daten für die Verwendung mit dem Connector zu erhalten.

### <a name="anomali-limo"></a>Anomali Limo

- [Erfahren Sie, was Sie benötigen, um eine Verbindung mit dem Anomali Limo-Feed herzustellen.](https://www.anomali.com/resources/limo)

### <a name="cybersixgill-darkfeed"></a>Cybersixgill Darkfeed

- [Erfahren Sie mehr über die Cybersixgill-Integration mit Azure Sentinel @Cybersixgill](https://www.cybersixgill.com/partners/azure-sentinel/)
- Um Azure Sentinel mit Cybersixgill TAXII-Server zu verbinden und Zugriff auf Darkfeed zu erhalten, [kontaktieren Sie Cybersixgill](mailto://azuresentinel@cybersixgill.com), um die API-Stammadresse, die Sammlungs-ID, den Benutzernamen und das Kennwort zu erhalten.

### <a name="financial-services-information-sharing-and-analysis-center-fs-isac"></a>Financial Services Information Sharing and Analysis Center (FS-ISAC)

- Treten Sie [FS-ISAC](https://www.fsisac.com/membership?utm_campaign=ThirdParty&utm_source=MSFT&utm_medium=ThreatFeed-Join) bei, um die Anmeldeinformationen für den Zugriff auf diesen Feed zu erhalten.

### <a name="health-intelligence-sharing-community-h-isac"></a>Health Intelligence Sharing Community (H-ISAC)

- [Treten Sie H-ISAC bei](https://h-isac.org/soltra/), um die Anmeldeinformationen für den Zugriff auf diesen Feed zu erhalten.

### <a name="ibm-x-force"></a>IBM X-Force

- [Weitere Informationen zur IBM X-Force-Integration](https://www.ibm.com/security/xforce)

### <a name="intsights"></a>IntSights

- [Weitere Informationen zur IntSights-Integration mit Azure Sentinel @IntSights](https://intsights.com/resources/intsights-microsoft-azure-sentinel)
- Um Azure Sentinel mit dem IntSights TAXII-Server zu verbinden, rufen Sie die API-Stammadresse, die Sammlungs-ID, den Benutzernamen und das Kennwort aus dem IntSights-Portal ab, nachdem Sie eine Richtlinie der Daten konfiguriert haben, die Sie an Azure Sentinel senden möchten.

### <a name="threatconnect"></a>ThreatConnect

- [Weitere Informationen zu STIX und TAXII @ThreatConnect](https://threatconnect.com/stix-taxii/)
- [Dokumentation zu TAXII-Diensten @ThreatConnect](https://docs.threatconnect.com/en/latest/rest_api/taxii/taxii.html)

## <a name="integrated-threat-intelligence-platform-products"></a>Produkte der integrierten Threat Intelligence Platform

Um eine Verbindung mit TIP-Feeds (Threat Intelligence Platform) herzustellen, befolgen Sie die Anweisungen zum [Herstellen einer Verbindung von Threat Intelligence Platform-Feeds](connect-threat-intelligence.md#connect-azure-sentinel-to-your-threat-intelligence-platform) zu Azure Sentinel. Im zweiten Teil dieser Anweisungen müssen Sie Informationen in Ihre TIP-Lösung eingeben. Unter den folgenden Links finden Sie weitere Informationen.

### <a name="agari-phishing-defense-and-brand-protection"></a>Agari Phishing Defense und Brand Protection

- Verwenden Sie zum Verbinden von [Agari Phishing Defense and Brand Protection](https://agari.com/products/phishing-defense/) den integrierten [Agari-Datenconnector](connect-agari-phishing-defense.md) in Azure Sentinel.

### <a name="anomali-threatstream"></a>Anomali ThreatStream

- Auf der Seite für [ThreatStream-Downloads](https://ui.threatstream.com/downloads) können Sie [ThreatStream Integrator und Erweiterungen](https://www.anomali.com/products/threatstream) herunterladen. Außerdem finden Sie dort Anweisungen dazu, wie Sie ThreatStream-Daten mit der Microsoft Graph-Sicherheits-API verbinden.

### <a name="alienvault-open-threat-exchange-otx-from-att-cybersecurity"></a>AlienVault Open Threat Exchange (OTX) von AT&T Cybersecurity

- [AlienVault OTX](https://otx.alienvault.com/) nutzt Azure Logic Apps (Playbooks), um die Verbindung zu Azure Sentinel herzustellen. Sehen Sie sich die [speziellen Anweisungen](https://techcommunity.microsoft.com/t5/azure-sentinel/ingesting-alien-vault-otx-threat-indicators-into-azure-sentinel/ba-p/1086566) an, die erforderlich sind, um das gesamte Angebot in vollem Umfang zu nutzen.

### <a name="eclecticiq-platform"></a>EclecticIQ Platform

- Erfahren Sie mehr über [EclecticIQ Platform](https://www.eclecticiq.com/platform/).

### <a name="groupib-threat-intelligence-and-attribution"></a>GroupIB Threat Intelligence and Attribution

- GroupIB nutzt Azure Logic Apps, um [GroupIB Threat Intelligence and Attribution](https://www.group-ib.com/intelligence-attribution.html) mit Azure Sentinel zu verbinden. Sehen Sie sich die [speziellen Anweisungen](https://techcommunity.microsoft.com/t5/azure-sentinel/group-ib-threat-intelligence-and-attribution-connector-azure/ba-p/2252904) an, die erforderlich sind, um das gesamte Angebot in vollem Umfang zu nutzen.

### <a name="misp-open-source-threat-intelligence-platform"></a>MISP-Open-Source-Threat Intelligence Platform

- Ein Beispielskript, das Clients mit MISP-Instanzen zur Migration von Bedrohungsindikatoren in die Microsoft Graph-Sicherheits-API bereitstellt, finden Sie unter [MISP zu Microsoft Graph-Sicherheitsskript](https://github.com/microsoftgraph/security-api-solutions/tree/master/Samples/MISP).
- [Erfahren Sie mehr über das MISP-Projekt](https://www.misp-project.org/).

### <a name="palo-alto-networks-minemeld"></a>Palo Alto Networks MineMeld

- Informationen zum Konfigurieren von [Palo Alto MineMeld](https://www.paloaltonetworks.com/products/secure-the-network/subscriptions/minemeld) mit den Verbindungsinformationen zu Azure Sentinel finden Sie unter [Senden von IOCs an die Sicherheits-API von Microsoft Graph mithilfe von MineMeld](https://live.paloaltonetworks.com/t5/MineMeld-Articles/Sending-IOCs-to-the-Microsoft-Graph-Security-API-using-MineMeld/ta-p/258540), und fahren Sie mit der Überschrift **MiningMeld-Konfiguration** fort.

### <a name="recorded-future-security-intelligence-platform"></a>Recorded Future Security Intelligence Platform

- [Recorded Future](https://www.recordedfuture.com/integrations/microsoft-azure/) nutzt Azure Logic Apps (Playbooks), um die Verbindung zu Azure Sentinel herzustellen. Sehen Sie sich die [speziellen Anweisungen](https://go.recordedfuture.com/hubfs/partners/microsoft-azure-installation-guide.pdf) an, die erforderlich sind, um das gesamte Angebot in vollem Umfang zu nutzen.

### <a name="threatconnect-platform"></a>ThreatConnect-Plattform

- Anweisungen zum Verbinden von [ThreatConnect](https://threatconnect.com/solution/) mit Azure Sentinel finden Sie im [Konfigurationshandbuch zur Integration von Microsoft Graph-Indikatoren für Sicherheitsbedrohungen](https://training.threatconnect.com/learn/article/microsoft-graph-security-threat-indicators-integration-configuration-guide-kb-article).

### <a name="threatquotient-threat-intelligence-platform"></a>ThreatQuotient Threat Intelligence Platform

- Unter [Microsoft Sentinel-Connector für ThreatQ-Integration](https://appsource.microsoft.com/product/web-apps/threatquotientinc1595345895602.microsoft-sentinel-connector-threatq?src=health&tab=DetailsAndSupport) finden Sie Supportinformationen und Anweisungen zum Verbinden von [ThreatQuotient TIP](https://www.threatq.com/) mit Azure Sentinel.

## <a name="incident-enrichment-sources"></a>Quellen für die Incidentanreicherung

Neben dem Import von Bedrohungsindikatoren können Threat-Intelligence-Feeds auch als Quelle zur Anreicherung der Informationen in Ihren Incidents verwendet werden und Ihren Untersuchungen mehr Kontext verleihen. Die folgenden Feeds dienen diesem Zweck und stellen Azure Logic Apps-Playbooks zur Verwendung in Ihrer [automatisierten Reaktion auf Incidents ](automate-responses-with-playbooks.md) zur Verfügung.

### <a name="hyas-insight"></a>HYAS Insight

- Suchen und aktivieren Sie Playbooks zur Incidentanreicherung für [HYAS Insight](https://www.hyas.com/hyas-insight) im [GitHub-Repository](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks) von Azure Sentinel. Suchen Sie nach Unterordnern, die mit „Enrich-Sentinel-Incident-HYAS-Insight-“ beginnen.
- Weitere Informationen finden Sie in der [Dokumentation zum HYAS Insight Logic App-Connector](/connectors/hyasinsight/).

### <a name="recorded-future-security-intelligence-platform"></a>Recorded Future Security Intelligence Platform

- Suchen und aktivieren Sie Playbooks zur Incidentanreicherung für [Recorded Future](https://www.recordedfuture.com/integrations/microsoft-azure/) im [GitHub-Repository](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks) von Azure Sentinel. Suchen Sie nach Unterordnern, die mit „RecordedFuture_“ beginnen.
- Weitere Informationen finden Sie in der [Dokumentation zum Recorded Future Logic App-Connector](/connectors/recordedfuture/).

### <a name="reversinglabs-titaniumcloud"></a>ReversingLabs TitaniumCloud

- Suchen und aktivieren Sie Playbooks zur Incidentanreicherung für [ReversingLabs](https://www.reversinglabs.com/products/file-reputation-service) im [GitHub-Repository](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks/Enrich-SentinelIncident-ReversingLabs-File-Information) von Azure Sentinel.
- Weitere Informationen finden Sie in der [Dokumentation zum ReversingLabs Intelligence Logic App-Connector](/connectors/reversinglabsintelligence/).

### <a name="riskiq-passive-total"></a>RiskIQ Passive Total

- Suchen und aktivieren Sie Playbooks zur Incidentanreicherung für [RiskIQ Passive Total](https://www.riskiq.com/products/passivetotal/) im [GitHub-Repository](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks) von Azure Sentinel. Suchen Sie nach Unterordnern, die mit „Enrich-SentinelIncident-RiskIQ-“ beginnen.
- Weitere Informationen zum Arbeiten mit RiskIQ-Playbooks finden Sie [hier](https://techcommunity.microsoft.com/t5/azure-sentinel/enrich-azure-sentinel-security-incidents-with-the-riskiq/ba-p/1534412).
- Weitere Informationen finden Sie in der [Dokumentation zum RiskIQ PassiveTotal Logic App-Connector](/connectors/riskiqpassivetotal/).

### <a name="virus-total"></a>Virus Total

- Suchen und aktivieren Sie Playbooks zur Incidentanreicherung für [Virus Total](https://developers.virustotal.com/v3.0/reference) im [GitHub-Repository](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks) von Azure Sentinel. Suchen Sie nach Unterordnern, die mit „Get-VirusTotal“ und „Get-VTURL“ beginnen.
- Weitere Informationen finden Sie in der [Dokumentation zum Virus Total Logic App-Connector](/connectors/virustotal/).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie Ihren Threat Intelligence-Anbieter mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln.

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](./tutorial-detect-threats-built-in.md).
