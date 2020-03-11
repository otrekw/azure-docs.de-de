---
title: 'Problembehandlung: Azure Virtual Network NAT-Konnektivität'
titleSuffix: Azure Virtual Network NAT troubleshooting
description: Es wird beschrieben, wie Sie Virtual Network NAT-Probleme beheben.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
Customer intent: As an IT administrator, I want to troubleshoot Virtual Network NAT.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/04/2020
ms.author: allensu
ms.openlocfilehash: d56cd3b3d286d69a51d8cc14eb8020343cf7295a
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302983"
---
# <a name="troubleshoot-azure-virtual-network-nat-connectivity-problems"></a>Problembehandlung: Azure Virtual Network NAT-Konnektivität

In diesem Artikel erhalten Administratoren Hilfe beim Diagnostizieren und Beheben von Konnektivitätsproblemen mit Virtual Network NAT.

>[!NOTE] 
>Virtual Network NAT ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar. Dieser Dienst ist bisher nur in einer begrenzten Zahl von [Regionen](nat-overview.md#region-availability) erhältlich. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms).

## <a name="problems"></a>Probleme

- [SNAT-Auslastung](#snat-exhaustion)
- [Fehler bei ICMP-Ping](#icmp-ping-is-failing)

Führen Sie die Schritte im folgenden Abschnitt aus, um diese Probleme zu beheben.

## <a name="resolution"></a>Lösung

### <a name="snat-exhaustion"></a>SNAT-Auslastung

Für eine einzelne [NAT-Gatewayressource](nat-gateway-resource.md) werden jeweils 64.000 bis maximal 1 Million gleichzeitige Datenflüsse unterstützt.  Jede IP-Adresse stellt 64.000 SNAT-Ports für den verfügbaren Bestand bereit. Sie können bis zu 16 IP-Adressen pro NAT-Gatewayressource nutzen.  Eine ausführlichere Beschreibung des SNAT-Mechanismus finden Sie [hier](nat-gateway-resource.md#source-network-address-translation).

#### <a name="steps"></a>Schritte:

1. Ermitteln Sie, wie von Ihrer Anwendung ausgehende Konnektivität erstellt wird (z. B. Code Review oder Paketerfassung). 
2. Ermitteln Sie, ob es sich bei dieser Aktivität um ein erwartetes Verhalten handelt oder die Anwendung ein Fehlverhalten aufweist.  Verwenden Sie [Metriken](nat-metrics.md) in Azure Monitor, um Ihre Erkenntnisse zu untermauern.
3. Prüfen Sie, ob die richtigen Muster eingehalten werden.
4. Prüfen Sie, ob die hohe SNAT-Portauslastung verringert werden sollte, indem der NAT-Gatewayressource weitere IP-Adressen zugewiesen werden.

#### <a name="design-pattern"></a>Entwurfsmuster:

Nutzen Sie nach Möglichkeit immer die Wiederverwendung von Verbindungen und Verbindungspools.  Bei diesem Muster werden Probleme aufgrund einer hohen Ressourcenauslastung ganz vermieden, und es ergibt sich ein vorhersagbares Verhalten. Primitive für diese Muster finden Sie in vielen Entwicklungsbibliotheken und Frameworks.
- Erwägen Sie die Nutzung von [asynchronen Abrufmustern](https://docs.microsoft.com/azure/architecture/patterns/async-request-reply) für zeitintensive Vorgänge, um Verbindungsressourcen für andere Vorgänge freizuhalten.
- Für langlebige Datenflüsse (z. B. wiederverwendete TCP-Verbindungen) sollten TCP-Keepalives oder Anwendungsschicht-Keepalives verwendet werden, um zu verhindern, dass es für zwischengeschaltete Systeme zu Zeitüberschreitungen kommt.
- Es sollten ordnungsgemäße [Wiederholungsmuster](https://docs.microsoft.com/azure/architecture/patterns/retry) verwendet werden, um bei vorübergehenden Fehlern oder der Wiederherstellung nach Fehlern aggressive Wiederholungsversuche oder Bursts zu vermeiden.
Die Erstellung einer neuen TCP-Verbindung für jeden HTTP-Vorgang (auch als „atomische Verbindungen“ bezeichnet) ist ein Antimuster.  Mit atomischen Verbindungen wird verhindert, dass für Ihre Anwendung eine gute Skalierung erzielt wird, und es werden Ressourcen verschwendet.  Fassen Sie immer mehrere Vorgänge per Pipeline in derselben Verbindung zusammen.  Ihre Anwendung profitiert von der Transaktionsgeschwindigkeit und den Ressourcenkosten.  Wenn Ihre Anwendung die Verschlüsselung auf der Transportschicht nutzt (z. B. TLS), ist die Verarbeitung neuer Verbindungen mit erheblichen Kosten verbunden.  Informationen zu weiteren bewährten Mustern finden Sie unter [Cloudentwurfsmuster](https://docs.microsoft.com/azure/architecture/patterns/).

#### <a name="mitigations"></a>Gegenmaßnahmen

Sie können die Konnektivität in ausgehender Richtung wie folgt skalieren:

| Szenario | Entschärfung |
|---|---|
| In Zeiten mit hoher Auslastung kommt es bei SNAT-Ports zu Konflikten und zu einer Überlastung. | Ermitteln Sie, ob Sie zusätzliche öffentliche Ressourcen bzw. Präfixressourcen für öffentliche IP-Adressen hinzufügen können. Durch diese Hinzufügung können für Ihr NAT-Gateway maximal 16 IP-Adressen genutzt werden. Aufgrund dieser Maßnahme ist ein größerer Bestand für die verfügbaren SNAT-Ports vorhanden (64.000 pro IP-Adresse), und Sie können Ihr Szenario weiter skalieren.|
| Sie haben bereits 16 IP-Adressen bereitgestellt, und es kommt trotzdem weiterhin zu einer Überlastung bei den SNAT-Ports. | Verteilen Sie Ihre Anwendungsumgebung auf mehrere Subnetze, und stellen Sie für jedes Subnetz eine NAT-Gatewayressource bereit. |

>[!NOTE]
>Es ist wichtig, dass Sie verstehen, warum es zur SNAT-Überlastung kommt. Stellen Sie sicher, dass Sie die richtigen Muster verwenden, um skalierbare und zuverlässige Szenarien zu erhalten.  Das Hinzufügen von weiteren SNAT-Ports, ohne dass Ihnen die Ursache des Bedarfs bekannt ist, sollte nur im Notfall erfolgen. Wenn Sie nicht verstehen, warum es bei Ihnen zu einer hohen Auslastung beim SNAT-Portbestand kommt, führt die Erweiterung auf eine größere Zahl von SNAT-Ports durch das Hinzufügen von mehr IP-Adressen nur dazu, dass sich dieser Überlastungsfehler bei der weiteren Skalierung Ihrer Anwendung fortsetzt.  Es kann sein, dass hierdurch andere Ineffizienzen und Antimuster verdeckt werden.

### <a name="icmp-ping-is-failing"></a>Fehler bei ICMP-Ping

[Virtual Network NAT](nat-overview.md) unterstützt IPv4-UDP- und TCP-Protokolle. ICMP wird nicht unterstützt, sodass hierfür das Auftreten von Fehlern zu erwarten ist.  Verwenden Sie stattdessen TCP-Verbindungstests (z. B. „TCP-Ping“) und UDP-spezifische Anwendungsschichttests, um die End-to-End-Konnektivität zu überprüfen.

Die folgende Tabelle kann als Ausgangspunkt dafür dienen, welche Tools für den Einstieg in die Tests verwendet werden können.

| Betriebssystem | Allgemeiner TCP-Verbindungstest | TCP-Anwendungsschichttest | UDP |
|---|---|---|---|
| Linux | nc (allgemeiner Verbindungstest) | curl (TCP-Anwendungsschichttest) | Anwendungsspezifisch |
| Windows | [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) | PowerShell [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) | Anwendungsspezifisch |

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Virtual Network NAT](nat-overview.md).
- Informieren Sie sich über die [NAT-Gatewayressource](nat-gateway-resource.md).
- Informieren Sie sich über [Metriken und Warnungen für NAT-Gatewayressourcen](nat-metrics.md).
