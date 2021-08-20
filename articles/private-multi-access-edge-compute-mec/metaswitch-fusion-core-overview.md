---
title: Fusion Core in Azure
description: Eine Übersicht über Fusion Core, eine cloudnative Implementierung des durch den 3GPP-Standard definierten 5G Next Generation Core-Netzwerks (5G NGC oder 5GC), mit dem 5G-Netzwerkbetreiber Datenverkehr von allen Endgeräten über mehrere Drahtlos- und Festzugriffstechnologien aggregieren können.
ms.service: private-multi-access-edge-compute-mec
author: djrmetaswitch
ms.author: drichards
ms.topic: overview
ms.date: 06/16/2021
ms.openlocfilehash: dd813b08301da960fb13e6047e51046ac2d95aed
ms.sourcegitcommit: 30e3eaaa8852a2fe9c454c0dd1967d824e5d6f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2021
ms.locfileid: "112464555"
---
# <a name="what-is-fusion-core"></a>Was ist Fusion Core?

Fusion Core ist eine cloudnative Implementierung des durch den 3GPP-Standard definierten 5G Next Generation Core-Netzwerks (5G NGC oder 5GC), mit dem 5G-Netzwerkbetreiber Datenverkehr von allen Endgeräten über mehrere Drahtlos- und Festzugriffstechnologien aggregieren können. Fusion Core umfasst eine hochgradig leistungsstarke und programmierbare 5G-User-Plane-Function (UPF), Kernfunktionen der Steuerungsebene, ein Portfolio aus dienstbasierten Architekturelementen und Verwaltungskomponenten für die Netzwerküberwachung.

Fusion Core kann in reinen 5G-Szenarios bereitgestellt werden oder mit 4G-Netzwerken zusammenarbeiten.

Fusion Core implementiert die folgenden 5G-Netzwerkfunktionen.

|Netzwerkfunktion  |Beschreibung  |
|---------|---------|
|**AMF**<br><br>Access and Mobility Management Function     |Diese Funktion unterstützt Folgendes:<br><ul> <li>Beendigung von NAS-Signalen des gNB</li><li>NAS-Verschlüsselung und -Integritätsschutz</li><li>Registrierungsverwaltung</li><li>Verbindungsverwaltung</li><li>Mobility Management</li><li>Zugriffsauthentifizierung und -autorisierung</li><li>Verwaltung des Sicherheitskontexts</li></ul>         |
|**SMF**<br><br>Session Management Function     |SMF unterstützt das Einrichten, Ändern und Veröffentlichen von Sitzungen.<br><br>In diesem Rahmen stellt die Funktion UE-IP-Adresszuordnung und -verwaltung, einschließlich DHCP, sowie die Beendigung von NAS-Signalen im Zusammenhang mit der Sitzungsverwaltung bereit.<br><br>Darüber hinaus bietet sie Unterstützung für das Paging von Downlinkdatenverkehr zu Geräten und eine Konfiguration der Datenverkehrsteuerung im Rahmen der Sitzungsverwaltung.         |
|**UPF**<br><br>User Plane Function     |UPF ist für die Verarbeitung des Paketdatenverkehrs verantwortlich. Dies umfasst die Verarbeitung des Routings, der Weiterleitung, der Überprüfung und der QoS für den Paketdatenverkehr.<br><br>Die Funktion fungiert als Ankerpunkt für den Datenverkehr an das Datennetzwerk (DN) und für die Übergabe zwischen verschiedenen Funknetzwerken.         |
|**PCF**<br><br>Policy Control Function     |PCF stellt ein zentrales Richtlinienframework für Datenverkehr und Richtlinienregeln für die Steuerungsebenenfunktion bereit und dient als Quelle für Abonnementinformationen.         |
|**AUSF**<br><br>Authentication Server Function     |AUSF ist der Authentifizierungsserver für 5G-Abonnenten.         |
|**UDM**<br><br>Unified Data Management     |UDM unterstützt die Generierung der AKA-Anmeldeinformationen, die Benutzeridentifizierung, die Zugriffsautorisierung und die Abonnentenverwaltung.         |
|**UDR**<br><br>Unified Data Repository     |UDR ist ein zusammengeführtes Repository aller Abonnenteninformationen.         |
|**NRF**<br><br>Network Repository Function     |NRF stellt die Dienstermittlung für NFs bereit.         |

Außerdem implementiert die Funktion in der Zusammenarbeit mit einem 4G-Netzwerk die folgenden Netzwerkfunktionen.

|Netzwerkfunktion  |Beschreibung  |
|---------|---------|
|**MME**<br><br>Mobility Management Entity     |MME ist der wichtigste Steuerknoten für das LTE-Zugriffsnetzwerk und ist für die umfassende Steuerung des UE-Zugriffs auf das Netzwerk verantwortlich.         |
|**S11-IWF**<br><br>S11 Interworking Function |S11-IWF macht eine S11-Schnittstelle für 4G Mobility Management Entities (MMEs) verfügbar und stellt Protokollübersetzungen für unterstützende Funktionen bereit.         |
|**UDR**<br><br>Unified Data Repository     |UDR ist ein zusammengeführtes Repository aller Abonnenteninformationen. UDR von Fusion Core übernimmt die Rolle, die in einem 4G-Szenario normalerweise von einem Home Subscriber Store (HSS) ausgeführt wird. Das Repository interagiert mit der MME über die Schnittstelle S6a.         |

Die folgende Abbildung zeigt jede dieser Netzwerkfunktionen und die Schnittstellen, die sie für die Zusammenarbeit mit Drittanbieterkomponenten verwenden.

:::image type="content" source="./media/metaswitch-overview/fusion-core-architecture.png" alt-text="Fusion Core-Architektur":::

Fusion Core wird auf einer VM bereitgestellt, die als Fusion Core Base VM bezeichnet wird. Die Fusion Core Base VM ist dafür konzipiert, als von Azure verwaltete Anwendung in Azure Stack Edge bereitgestellt zu werden. Die Netzwerkfunktionen und Infrastrukturkomponenten, die für die Bereitstellung der Fusion Core-Funktionen erforderlich sind, werden als Container auf der Fusion Core Base VM bereitgestellt und von Kubernetes orchestriert.

:::image type="content" source="./media/metaswitch-overview/fusion-core-base-vm-azure-stack-edge-with-networking.png" alt-text="Bereitstellen von Fusion Core in Azure Stack Edge":::

## <a name="why-use-fusion-core"></a>Warum Fusion Core?

### <a name="deployment-in-private-networks"></a>Bereitstellung in privaten Netzwerken

Mithilfe der Funktionen des privaten Multi-Access Edge Computings von Azure bietet Fusion Core eine 5G-Lösung für Unternehmen, die die Leistung und geringe Latenz von Edgecomputingressourcen, die für Anwendungsfälle der Industrie 4.0 erforderlich sind, mit einer konsistenten, zentralisierten Verwaltung in Azure kombiniert. Ausführliche Informationen zu privatem Multi-Access Edge Computing finden Sie unter [Was ist privates Multi-Access Edge Computing in Azure](overview.md).

Die Bereitstellung von Fusion Core am Unternehmensedge stellt sicher, dass sich der Dienst so nah wie möglich an den Geräten befindet, die er bedient. Außerdem ermöglicht diese Bereitstellung durch die lokale Datenverarbeitung niedrige Latenzzeiten und einen geringeren Backhaul, wenn sie am gleichen Standort mit Anwendungslogik kombiniert wird. Dies bietet Unternehmen u. a. die folgenden wertvollen Vorteile.

- **Automatisierung:** Befehls- und Steuerungsmeldungen von automatisierten Systemen (z. B. Robotern) können in Echtzeit verarbeitet werden. So werden Verzögerungen verhindert und eine höhere Produktivität gewährleistet.
- **Telemetrie:** Telemetriedaten für die Integritätsbewertung und den Betrieb automatisierter Systeme können in Echtzeit verarbeitet werden, um Unfälle zu verhindern und die Sicherheit vor Ort zu gewährleisten.
- **Analysen:** Große Mengen von Betriebs- und Diagnosedaten können zu minimalen Kosten transportiert werden. Dies stellt sicher, dass kritische Aktionen nicht verzögert werden.

:::image type="content" source="./media/metaswitch-overview/enterprise-edge-latency.png" alt-text="Fusion Core am Edge von privaten 5G-Netzen":::

Fusion Core kann diese geringe Latenz zusammen mit der Sicherheit und hohen Bandbreite privater 5G-Netzwerke nutzen. Dies macht es zur idealen Lösung, um Anwendungsfälle der Industrie 4.0 wie die folgenden zu unterstützen.

- **Fertigung:** Analysen von Produktionslinien und Lagerautomatisierung mit Robotern
- **Öffentliche Sicherheit:** Mobilität und Konnektivität für Nothelfer und Notfallwiederherstellungsexperten
- **Energie- und Versorgungswirtschaft:** Backhaulnetzwerke für Smart Meter und Netzwerksegmentierung/-steuerung
- **Verteidigung:** Vernetzte Befehlsposten und Gefechtsfelder mit Echtzeitanalysen
- **Smart Farming:** Vernetzte Geräte für landwirtschaftliche Betriebe

### <a name="pure-5g-and-4g-interworking-support"></a>Unterstützung für reines 5G- und 4G-Interworking

Fusion Core kann in reinen 5G-Szenarios bereitgestellt werden. In diesem Fall wird Fusion Core im **eigenständigen 5G-Modus** ausgeführt.

Fusion Core unterstützt auch 4G-Interworking und stellt so über einen cloudnativen Kern Dienste für 4G-UEs bereit. Dies wird als **4G-Modus** bezeichnet. Für Greenfield-Netzbetreiber und etablierte Netzbetreiber bietet der 4G-Modus einen direkten Zugang zu 5G, ohne weiterhin in die Wartung eines 4G-Kerns zu investieren.

### <a name="service-assurance-and-kpi-metrics"></a>Service Assurance und KPI-Metriken

Fusion Core ist mit **Service Assurance Server (SAS) von Metaswitch** integriert und bietet eine proaktive Echtzeitanalyse des gesamten Nachrichtendatenverkehrs, einschließlich NGAP/NAS-Nachrichten sowie HTTP-Anforderungen und -Antworten.

Service Assurance Server bietet eine Web-GUI, mit der Sie detaillierte Ablaufverfolgungen für Signalflüsse im Zusammenhang mit Fusion Core erfassen können. Damit können Sie viele allgemeine Konfigurations-, Netzwerk- und Interoperabilitätsprobleme diagnostizieren, die den Benutzerdienst beeinträchtigen.

:::image type="content" source="./media/metaswitch-overview/service-assurance-server-detailed-timeline.png" alt-text="Detaillierte Zeitachsenansicht auf der Web-GUI von Service Assurance Server":::

Fusion Core ist auch mit **Metaswitch ServiceIQ Monitoring** integriert – eine Lösung, die Branchenstandards setzende cloudnative Überwachungstools wie Prometheus und Grafana bietet und eine Echtzeitanalyse der Systemleistung, Fehleridentifikation und Problembehandlung ermöglicht.

ServiceIQ Monitoring wird auf der Fusion Core-VM ausgeführt und ermöglicht Ihnen den Zugriff auf eine Reihe von Fusion Core-Dashboards, über die Sie wichtige Metriken zu Ihrer Fusion Core-Bereitstellung flexibel überwachen können. Auf den Dashboards können Sie außerdem Informationen zum Auslösen von Warnungen anzeigen und so sicherstellen, dass Sie schnell auf neue Probleme reagieren können.


[![Übersichtsdashboard von Fusion Core](media/metaswitch-overview/fusion-core-overview-dashboard.png)](media/metaswitch-overview/fusion-core-overview-dashboard.png#lightbox)

### <a name="other-features"></a>Andere Funktionen

|Feature  |Beschreibung  |
|---------|---------|
|**Netzwerksegmentierung**     |Fusion Core-Bereitstellungen können so konfiguriert werden, dass sie mehrere Netzwerksegmente für multiplexunabhängige logische Netzwerke bereitstellen. Alle Netzwerkfunktionen innerhalb einer einzelnen Fusion Core-Bereitstellung bedienen alle konfigurierten Segmente.<br><br>Sie können die zulässigen und standardmäßig verfügbaren Segmente für Abonnenten bereitstellen und eine segmentspezifische Richtlinie (QoS) definieren. So stellen Sie einen einfachen und allgegenwärtigen Mechanismus bereit, um die administrative Trennung von Abonnentengruppen aufrechtzuerhalten.         |
|**Unterstützte 5G-Verfahren**     |Fusion Core erfüllt die 3GPP TS 23.502-Vorgaben für die folgenden Verfahren, wenn es im Rahmen einer umfassenderen privaten 5G-Lösung verwendet wird.<ul><li>UE-Registrierung/Aufhebung der UE-Registrierung</li><li>Mobilitätsregistrierungsupdate/periodisches Registrierungsupdate</li><li>UE-initiierte Dienstanforderung (Signale/Daten)</li><li>AN/netzwerkinitiiertes UE-Kontextrelease</li><li>PDU-Sitzungseinrichtung</li><li>PDU-Sitzungsfreigabe</li><li>N2-basiertes Handover zwischen NG-RAN-Knoten</li><li>Xn-basiertes Handover NG-RAN-Knoten</li><li>Vom Netzwerk initiierte Downlinkdatenbenachrichtigung oder vom Netzwerk initiiertes Downlinkdatenpaging</li>        |
|**UE-Authentifizierung**     |<ul><li>Unterstützung für Security Anchor Function (SEAF), um Authentifizierungsfunktionen im Bereitstellungsnetzwerk bereitzustellen</li><li>Authentifizierung mit permanenten Abonnementbezeichnern (Subscription Permanent Identifiers, SUPI), das Abonnement verbergenden Bezeichnern (Subscription Concealed Identifiers, SUCI) und global eindeutigen temporären Identitäten (Globally Unique Temporary Identities, 5G-GUTI)</li><li>Zuweisung oder Neuzuordnung einer 5G-GUTI zu einer UE-Instanz</li><li>Authentifizierung und Schlüsselvereinbarung der dritten Generation (EAP-AKA) und 5G-Authentifizierung und Schlüsselvereinbarung (5G-AKA) für die gegenseitige Authentifizierung zwischen UE-Instanzen und dem Netzwerk</li><li>Auf Evolved Packet System basierende Authentifizierung und Schlüsselvereinbarung (EPS-AKA) für Bereitstellungen, die im 4G-Modus ausgeführt werden</li>         |
|**Verwaltung des UE-Sicherheitskontexts**     |AMF von Fusion Core führt Verschlüsselung und Integritätsschutz für 5G NAS aus. Während der UE-Registrierung fügt die UE-Instanz ihre Sicherheitsfunktionen für 5G NAS mit 128-Bit-Schlüsseln hinzu.<br><br>Die von Fusion Core unterstützten Algorithmen für die Verschlüsselung und den Integritätsschutz sind z. B.:<ul><li>5GS-NULL-Verschlüsselungsalgorithmus</li><li>128-Bit Snow3G</li><li>128-Bit AES</li><li>128-Bit ZUC</li>        |
|**UE-MTU-Konfiguration**     |Auf Anforderung signalisiert SMF von Fusion Core UEs im Rahmen der PDU-Sitzungseinrichtungsverfahren die MTU eines Datennetzwerks, um Fragmentierung zu vermeiden.         |
|**Richtliniensteuerung**     |Fusion Core wendet umfassende Richtliniensteuerungsentscheidungen und -erzwingung, die auf Basis von PDU-Sitzungen und Flows angewendet werden. Dies bietet die Flexibilität, die Sie benötigen, um unterschiedliche QoS-Ebenen auf bestimmte Flows oder UEs anzuwenden. Außerdem ermöglicht es Ihnen, Dienste oder Datennetzwerke selektiv für UE-Gruppen verfügbar zu machen und die Datenverkehrstypen zu steuern, die über das Netzwerk übertragen werden.         |
|**UPF-Zugriffssteuerungslisten**     |Zugriffssteuerungslisten (Access Control Lists, ACLs) können verwendet werden, um Datenverkehr, der von oder an bestimmte IPv4-Adressbereiche adressiert ist, zuzulassen oder zu blockieren. Sie können ACLs in den Zugriffs- und Kernschnittstellen (N3 und N6) von Fusion Core konfigurieren.         |
|**Index zu RAT/Frequency Selection Priority (RFSP)**     |AMF von Fusion Core kann ein RAN mit einem RFSP-Index bereitstellen, den das RAN seiner lokalen Konfiguration zuordnen kann, um bestimmte Richtlinien für die Verwaltung von Funkressourcen anzuwenden, z. B. die erneute Auswahl von Zellen oder die Frequenzebenenweiterleitung.         |

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur [Bereitstellung von Fusion Core](deploy-metaswitch-fusion-core-solution.md)

