---
title: Ausgangsregeln – Azure Load Balancer
description: In diesem Artikel wird erläutert, wie Sie Ausgangsregeln konfigurieren, um den ausgehenden Internetdatenverkehr mit Azure Load Balancer zu steuern.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: conceptual
ms.custom: contperfq1
ms.date: 10/13/2020
ms.author: allensu
ms.openlocfilehash: 51810876e3636b7023ce9c9318a071636bb00c4c
ms.sourcegitcommit: 090ea6e8811663941827d1104b4593e29774fa19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "92002607"
---
# <a name="outbound-rules-azure-load-balancer"></a><a name="outboundrules"></a>Ausgangsregeln – Azure Load Balancer

Mit Ausgangsregeln können Sie die SNAT (Quell-Netzwerkadressenübersetzung) einer öffentlichen Load Balancer Standard-Instanz für ausgehenden Datenverkehr konfigurieren. Mit dieser Konfiguration können Sie die öffentlichen IP-Adressen Ihrer Load Balancer-Instanz als Proxy verwenden.

Diese Konfiguration ermöglicht Folgendes:

* IP-Maskierung
* Vereinfachen ihrer Zulassungslisten.
* Verringern der Anzahl öffentlicher IP-Ressourcen für die Bereitstellung.

Mit Ausgangsregeln besitzen Sie vollständige deklarative Kontrolle über ausgehende Internetkonnektivität. Ausgangsregeln ermöglichen Ihnen das Skalieren und Optimieren dieser Fähigkeit gemäß Ihren speziellen Anforderungen. 

Ausgangsregeln werden nur befolgt, wenn die Back-End-VM nicht über eine öffentliche IP-Adresse (ILPIP) auf Instanzebene verfügt.

![Load Balancer-Ausgangsregeln](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

Mit Ausgangsregeln können Sie das Verhalten von ausgehenden **SNAT**-Verbindungen explizit definieren.

Mit Ausgangsregeln können Sie steuern:

* **Welchen IP-Adressen von VMs in welche öffentlichen IP-Adressen übersetzt werden.**
     * Zwei Regeln,durch die Back-End-Pool A die IP-Adressen A und B verwendet und Back-End-Pool B die IP-Adressen C und D.
* **Wie ausgehende SNAT-Ports zugewiesen werden.**
     * Back-End-Pool B ist der einzige Pool, der ausgehende Verbindungen ermöglicht, alle SNAT-Ports werden Back-End-Pool B und kein Port Back-End-Pool A zugewiesen.
* **Welche Protokolle zum Übersetzen ausgehenden Datenverkehrs verwendet werden.**
     * Back-End-Pool B benötigt UDP-Ports für ausgehenden Datenverkehr. Back-End-Pool A benötigt TCP. Weisen Sie TCP-Ports A und UDP-Ports B zu.
* **Wie lange das Leerlauftimeout für ausgehende Verbindungen dauert (4 bis 120 Minuten).**
     * Wenn zeitintensive Verbindungen mit Keepalives vorhanden sind, reservieren Sie für zeitintensive Verbindungen bis zu 120 Minuten lang Ports mit Leerlauf. Gehen Sie davon aus, dass veraltete Verbindungen eingestellt werden, und geben Sie Ports innerhalb von 4 Minuten für neue Verbindungen frei. 
* **Ob eine TCP-Zurücksetzung bei Leerlauftimeout gesendet wird.**
     * Wenn bei Verbindungen im Leerlauf ein Timeout auftritt, senden wir TCP RST an den Client und Server, damit diese wissen, dass der Datenfluss eingestellt wird?

## <a name="outbound-rule-definition"></a>Definition der Ausgangsregel

Ausgangsregeln folgen der gleichen vertrauten Syntax wie Lastenausgleichsregeln und NAT-Eingangsregeln: **Front-End** + **Parameter** + **Back-End-Pool**. 

Eine Ausgangsregel konfiguriert die NAT für ausgehenden Datenverkehr für _alle VMs, die vom Back-End-Pool_ für die _Front-End_-Übersetzung identifiziert wurden.  

Die _Parameter_ ermöglichen eine zusätzliche differenzierte Steuerung des NAT-Algorithmus für ausgehenden Datenverkehr.

## <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a> Skalieren der NAT für ausgehenden Datenverkehr mit mehreren IP-Adressen

Jede zusätzliche IP-Adresse, die von einem Front-End bereitgestellt wird, stellt zusätzliche 64.000 kurzlebige Ports zur Verfügung, die Load Balancer als SNAT-Ports verwenden kann. 

Verwenden Sie mehrere IP-Adressen, um umfangreiche Szenarien zu planen. Verwenden Sie Ausgangsregeln, um [SNAT-Erschöpfung](troubleshoot-outbound-connection.md#snatexhaust) zu vermeiden. 

Sie können auch ein [öffentliches IP-Präfix](https://aka.ms/lbpublicipprefix) direkt mit einer Ausgangsregel verwenden. 

Ein öffentliches IP-Präfix erhöht die Skalierung Ihrer Bereitstellung. Das Präfix kann der Positivliste der Datenflüsse hinzugefügt werden, die aus ihren Azure-Ressourcen stammen. Sie können eine Front-End-IP-Konfiguration innerhalb der Load Balancer-Instanz konfigurieren, um auf das Präfix einer öffentlichen IP-Adresse zu verweisen.  

Die Load Balancer-Instanz besitzt die Kontrolle über das öffentliche IP-Präfix. Die Ausgangsregel verwendet automatisch alle öffentlichen IP-Adressen, die im Präfix für öffentliche IP-Adressen für ausgehende Verbindungen enthalten sind. 

Jede IP-Adresse innerhalb des öffentlichen IP-Präfixes bietet zusätzliche 64.000 kurzlebige Ports pro IP-Adresse, die Load Balancer als SNAT-Ports verwenden kann.

## <a name="outbound-flow-idle-timeout-and-tcp-reset"></a><a name="idletimeout"></a> Leerlauftimeout für ausgehenden Datenfluss und TCP-Zurücksetzung

Ausgangsregeln stellen einen Konfigurationsparameter bereit, um das Leerlauftimeout für den ausgehenden Datenfluss zu steuern und ihn an die Anforderungen Ihrer Anwendung anzupassen. Das Leerlauftimeout für ausgehenden Datenverkehr tritt standardmäßig nach 4 Minuten ein. Weitere Informationen finden Sie unter [Konfigurieren von Leerlauftimeouts](load-balancer-tcp-idle-timeout.md). 

Das Standardverhalten von Load Balancer besteht darin, den Datenfluss allmählich zu entfernen, wenn das Leerlauftimeout für ausgehenden Datenverkehr erreicht wurde. Der `enableTCPReset`-Parameter ermöglicht vorhersagbares Anwendungsverhalten und Kontrolle. Der Parameter gibt vor, ob beim Timeout des ausgehenden Leerlauftimeouts bidirektionales TCP Reset (TCP RST) gesendet werden soll. 

Unter [TCP-Zurücksetzung bei Leerlauftimeout](https://aka.ms/lbtcpreset) finden Sie weitere Informationen, unter anderem zur regionalen Verfügbarkeit.

## <a name="securing-and-controlling-outbound-connectivity-explicitly"></a><a name="preventoutbound"></a>Explizites Sichern und Steuern der ausgehenden Konnektivität

Lastenausgleichsregeln bieten automatische Programmierung der NAT für ausgehenden Datenverkehr. In einigen Szenarien müssen Sie die automatische Programmierung der NAT für ausgehenden Datenverkehr durch die Lastenausgleichsregel deaktivieren. Durch die Deaktivierung über die Regel können Sie das Verhalten steuern oder verfeinern.  

Sie haben zwei Möglichkeiten, diesen Parameter zu verwenden:

1. Verhinderung der eingehenden IP-Adresse für ausgehende SNAT-Verbindungen. Deaktivieren Sie SNAT für ausgehenden Datenverkehr in der Lastenausgleichsregel.
  
2. Passen Sie die **SNAT**-Parameter für ausgehenden Datenverkehr einer IP-Adresse an, die zugleich für eingehenden und ausgehenden Datenverkehr verwendet wird. Automatische NAT für ausgehenden Datenverkehr muss deaktiviert werden, damit eine Ausgangsregel die Kontrolle übernehmen kann. Um die SNAT-Portzuordnung einer Adresse zu ändern, die auch für den eingehenden Datenverkehr verwendet wird, muss der Parameter `disableOutboundSnat` auf TRUE festgelegt werden. 

Der Vorgang zum Konfigurieren einer Ausgangsregel schlägt fehl, wenn Sie versuchen, eine IP-Adresse neu zu definieren, die für eingehenden Datenverkehr verwendet wird.  Deaktivieren Sie zuerst die ausgehende NAT der Lastenausgleichsregel.

>[!IMPORTANT]
> Ihre VM hat keine ausgehenden Verbindungen, wenn Sie diesen Parameter auf TRUE festlegen und keine Ausgangsregel zum Definieren ausgehender Verbindungen haben.  Einige Vorgänge Ihrer VM oder Ihrer Anwendung können davon abhängen, ob ausgehende Verbindungen verfügbar sind. Stellen Sie sicher, dass Sie die Abhängigkeiten Ihres Szenarios verstehen und die Auswirkungen von Änderungen berücksichtigt haben.

Manchmal ist es nicht wünschenswert, dass einem VM einen ausgehenden Datenflusses erstellt. Möglicherweise müssen Sie auch verwalten, welche Ziele mit ausgehende Datenflüsse empfangen bzw. welche Ziele eingehende Datenflüsse beginnen. Verwenden Sie [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md) zum Verwalten der Ziele,die die VM erreicht. Verwenden Sie Netzwerksicherheitsgruppen, um zu verwalten, welche öffentlichen Ziele eingehende Datenflüsse starten.

Wenn Sie eine Netzwerksicherheitsgruppe einem virtuellen Computer mit Lastenausgleich zuordnen, müssen Sie auf die [Diensttags](../virtual-network/security-overview.md#service-tags) und [Standardsicherheitsregeln](../virtual-network/security-overview.md#default-security-rules) achten. 

Stellen Sie sicher, dass die VM Integritätstestanforderungen von Azure Load Balancer empfangen kann.

Wenn eine Netzwerksicherheitsgruppe Anforderungen von Integritätstests vom Standardtag AZURE_LOADBALANCER blockiert, misslingt Ihr VM-Integritätstests, weshalb die VM mit „Nicht verfügbar“ markiert wird. Der Lastenausgleich beendet das Senden neuer Datenflüsse an diese VM.

## <a name="limitations"></a>Einschränkungen

- Die maximale Anzahl von verwendbaren kurzlebigen Ports pro Front-End-IP-Adresse beträgt 64.000.
- Das konfigurierbare Leerlauftimeout für ausgehenden Datenverkehr beträgt ist 4 bis 120 Minuten (240 bis 7200 Sekunden).
- Load Balancer unterstützt kein ICMP für die NAT ausgehenden Datenverkehrs.
- Ausgangsregeln können nur auf die primäre IP-Konfiguration einer NIC angewandt werden.  Sie können keine Ausgangsregel für die sekundäre IP-Adresse einer VM oder virtuellen Netzwerkappliance erstellen. Es werden mehrere NICs unterstützt.
- Alle virtuellen Computer innerhalb einer **Verfügbarkeitsgruppe** müssen dem Back-End-Pool für ausgehende Verbindungen hinzugefügt werden. 
- Alle virtuellen Computer in einer **VM-Skalierungsgruppe** müssen dem Back-End-Pool für ausgehende Verbindungen hinzugefügt werden.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Azure Load Balancer Standard](load-balancer-overview.md)
- Weitere Informationen finden Sie unter den [häufig gestellten Fragen zu Azure Load Balancer](load-balancer-faqs.md)

