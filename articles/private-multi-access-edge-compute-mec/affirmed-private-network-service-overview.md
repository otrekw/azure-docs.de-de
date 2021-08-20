---
title: Was ist Affirmed Private Network Service in Azure?
description: Hier erfahren Sie mehr über die Affirmed Private Network Service-Lösungen in Azure für private LTE- und 5G-Netzwerke.
author: KumudD
ms.service: private-multi-access-edge-compute-mec
ms.topic: overview
ms.date: 06/16/2021
ms.author: hollycl
ms.openlocfilehash: ca2327ee8e9245698dc8d9b57b2c5bf2bf8e6881
ms.sourcegitcommit: 30e3eaaa8852a2fe9c454c0dd1967d824e5d6f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2021
ms.locfileid: "112458522"
---
# <a name="what-is-affirmed-private-network-service-on-azure"></a>Was ist Affirmed Private Network Service in Azure?

Der Affirmed Private Network Service (APNS) ist ein verwalteter Netzwerkdienst, der für Managed Service Providers und Mobilfunkanbieter entwickelt wurde, um private LTE- und 5G-Lösungen für Unternehmen zur Verfügung zu stellen.

Affirmed hat seine Mobile-Core-Technologie mit Azure kombiniert, um eine einsatzbereite Lösung für private LTE- und 5G-Netzwerke zu entwickeln, die es Netzbetreibern und Unternehmen ermöglicht, die Vorteile von verwalteten Netzwerken und dem mobilen Edge zu nutzen. Durch die Kombination aus Cloudverwaltung und Automatisierung können Managed Service Providers eine vollständig verwaltete Infrastruktur bereitstellen. Außerdem profitieren Betreiber von einer umfassenden Lösung mit der besten Auswahl an RAN- (Radio Access Network), SIM- und Azure-Diensten, die von unserem vielfältigen Partnernetzwerk im Azure Marketplace angeboten werden. Die Lösung besteht aus fünf Komponenten:

- **Cloudnativer Mobile Core:** Diese Komponente ist mit den 3GPP-Standards konform, unterstützt die Netzwerkfunktionen für 4G und 5G und enthält native Integritätstests für virtuelle Netzwerke auf dem Mobile Core. Der Mobile Core kann auf VMs, physischen Servern oder in der Cloud des Betreibers bereitgestellt werden. Es ist also keine dedizierte Hardware mehr erforderlich.

- **Private Network Service Manager für Affirmed-Netzwerke:** Der Private Network Service Manager ist eine Anwendung, mit der Betreiber private Mobile-Core-Netzwerke auf der Azure-Plattform bereitstellen, überwachen und verwalten. Er enthält alle benötigten Verwaltungsfunktionen, unter anderem eine einfache Selbstaktivierung und die Verwaltung privater Netzwerkressourcen über ein programmgesteuertes Portal mit grafischer Benutzeroberfläche.

- **Azure Network Functions Manager:** Der Azure Network Functions Manager (NFM) ist ein vollständig verwalteter, cloudnativer Orchestrierungsdienst, mit dem Kunden Netzwerkfunktionen auf Azure Stack Edge Pro mit GPU bereitstellen können, um eine konsistente Hybridnutzung über das Azure-Portal zu ermöglichen.

- **Azure-Cloud:** Diese öffentliche Cloud-Computing-Plattform enthält beispielsweise IaaS-, PaaS- und SaaS-Lösungen (Infrastructure-as-a-Service, Platform-as-a-Service und Software-as-a-Service), die für Analysedienste, Virtual-Computing-Dienste, Speicherdienste, Netzwerkdienste und vieles mehr eingesetzt werden können.

- **Azure Stack Edge:** Diese Hardware-as-a-Service-Lösung von Microsoft wird in der Cloud verwaltet. So kann die Leistungsfähigkeit der Azure-Cloud auch auf lokalen, robusten Servern genutzt werden, die überall bereitgestellt werden können, wo lokale KI und komplexe Computingtasks erforderlich sind.


:::image type="content" source="./media/affirmed-overview/affirmed-private-network-service-solution.png" alt-text="Affirmed Private Network Service-Lösung":::

## <a name="why-use-the-affirmed-private-network-solution"></a>Vorteile der Affirmed Private Network Service-Lösung
APNS bietet für Betreiber und deren Kunden die folgenden Hauptvorteile:

- **Flexible Bereitstellung:** APNS nutzt eine Technologie zur Trennung der Steuerungsebene von der Benutzerebene und unterstützt drei Bereitstellungsmodi. So können verschiedenste vom Betreiber gewünschte Szenarios für Unternehmensangebote umgesetzt werden. Mit dem Private Network Service Manager können Betreiber die folgenden Bereitstellungsmodelle konfigurieren:

    - Das eigenständige Modell ermöglicht es Betreibern, ein komplett eigenständiges privates Netzwerk lokal bereitzustellen, indem das RAN und 5G Core in Azure Stack Edge und die Verwaltungsschicht in der zentralisierten Cloud bereitgestellt werden.

    - Das verteilte Modell ermöglicht eine schnellere Datenverarbeitung durch die nähere Verteilung der Benutzerebene am Unternehmensedge in Azure Stack Edge, während die Steuerungsebene in der Cloud verbleibt. Ein Anwendungsfall für ein solches Modell wäre beispielsweise eine Fertigungsanlage.

    - Das Cloudmodell ermöglicht es, sämtliche 5G Core-Funktionen in der Cloud bereitzustellen, während das RAN am Edge ausgeführt wird. So können Cloudressourcen dynamisch zugeteilt werden, um sich ändernden Workloadanforderungen gerecht zu werden.

- **MNO-Integration:** APNS ist beim Mobilfunkanbieter integriert. Das bedeutet, dass durch den verteilten Abonnentenkern eine vollständige Mobilität für private und öffentliche Betreibernetzwerke möglich ist. Betreiber können die privaten Mobilfunknetze auf Tausende Edgestandorte von Unternehmen skalieren.

    - Unterstützt alle Spectrum-Optionen: MNO Licensed, Private Licensed, CBRS, Shared, Unlicensed

    - Unterstützt isolierte und eigenständige private Netzwerke, Roaming für mehrere Standorte und Makroroaming durch die MNO-Integration

    - Bis zu 99,999 % Dienstverfügbarkeit und Interoperabilität mit sämtlichen 3GPP-konformen LTE- und 5G NR-Funknetzen Carrier-Grade-Resilienz für Unternehmen

- **Automatisierung und erleichterte Verwaltung:** Die APNS-Lösung kann über den Service Manager in der Azure-Cloud vollständig remote verwaltet werden. Über den Service Manager können Endbenutzer auf ein personalisiertes Dashboard zugreifen und Geräte im privaten Mobilfunknetz anzeigen, verwalten, aktivieren oder deaktivieren. Betreiber können den Status der Netzwerke auf Netzwerkprobleme und wichtige Parameter überwachen, um eine optimale Leistung sicherzustellen.

    - Sicherer, zuverlässiger, latenzarmer Dienst mit hoher Bandbreite für private Mobilfunknetze, der mit privatem Multi-Access Edge Computing in Azure ausgeführt wird

    - Vollständige Remoteverwaltung ohne Wartungsservice vor Ort

    - Cloudautomatisierung für Angebot verwalteter Dienste an Unternehmen oder Partner durch Betreiber über MSPs, die wiederum verwaltete Dienste anbieten

- **Smarte Network und Business Insights:** Der Mobile Core von Affirmed beinhaltet Integritätstests für virtuelle Netzwerke und Paketbrokerfunktionen, aus denen Erkenntnisse zu Netzwerken gewonnen werden können. Mithilfe dieser Erkenntnisse kann der Betreiber bessere Entscheidungen für seine Netzwerke treffen, und Kunden können diese für Entscheidungen zur Monetarisierung nutzen.

- **Datenschutz und Sicherheit:** APNS setzt auf Azure, um Sicherheit und Compliance für private Netzwerke und Unternehmensanwendungen zu erzielen. Betreiber können die Lösung zuverlässig für Anwendungsfälle in Branchen bereitstellen, in denen strenge Auflagen für den Datenschutz gelten, zum Beispiel für das Gesundheitswesen, Regierungsbehörden, die öffentliche Sicherheit und das Verteidigungswesen.

## <a name="next-steps"></a>Nächste Schritte
- [Bereitstellen der Affirmed Private Network Service-Lösung in Azure](deploy-affirmed-private-network-service-solution.md)



