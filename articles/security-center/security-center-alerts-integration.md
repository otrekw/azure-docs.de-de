---
title: Integration von Azure-Sicherheitsprodukten in Azure Security Center
description: In diesem Thema werden Azure-Sicherheitsprodukte vorgestellt, die in Azure Security Center integriert sind.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ad4b0373-08ee-46ca-a573-638ed93a647c
ms.service: security-center
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: memildin
ms.openlocfilehash: 693e7d35a0bb4c7dfbb3e033690a5e86e2c398a8
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74278332"
---
# <a name="integration-of-azure-security-products-in-azure-security-center"></a>Integration von Azure-Sicherheitsprodukten in Azure Security Center

Azure Security Center bietet Ihnen zusätzliche Microsoft-Lizenzen für die Arbeit mit den folgenden Sicherheitsprodukten:

* [Azure WAF](#azure-waf)
* [Azure DDoS Protection](#azure-ddos)

## Azure WAF <a name="azure-waf"></a>

Azure Application Gateway verfügt über eine Web Application Firewall (WAF), die den zentralisierten Schutz Ihrer Webanwendungen vor allgemeinen Exploits und Sicherheitsrisiken ermöglicht.

Webanwendungen sind zunehmend Ziele böswilliger Angriffe, die allgemein bekannte Sicherheitslücken ausnutzen. Die Application Gateway-WAF basiert auf Version 3.0 oder 2.2.9 des Kernregelsatzes aus dem Open Web Application Security Project. Die WAF wird automatisch aktualisiert und bietet ganz ohne zusätzliche Konfiguration Schutz vor neuen Sicherheitsrisiken. WAF-Warnungen werden an das Security Center gestreamt. Weitere Informationen zu den von WAF generierten Warnungen finden Sie unter [CRS-Regelgruppen und -Regeln der Web Application Firewall](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md?tabs=owasp31#crs911-31).

## Azure DDoS Protection<a name="azure-ddos"></a>

Verteilte Denial-of-Service-Angriffe (Distributed Denial of Service, DDoS) sind bekanntlich einfach durchführbar. Sie haben sich zu einem wichtigen Sicherheitsthema entwickelt, insbesondere wenn Sie Ihre Anwendungen zur Cloud migrieren. 

Ein DDoS-Angriff hat das Ziel, die Ressourcen einer Anwendung zu verbrauchen, damit sie für berechtigte Benutzer nicht mehr verfügbar ist. DDoS-Angriffe können jeden beliebigen Endpunkt ins Visier nehmen, der über das Internet erreichbar sind.

Azure DDoS Protection trägt in Kombination mit bewährten Anwendungsentwurfsmethoden zum Schutz vor DDoS-Angriffen bei. Azure DDoS Protection bietet verschiedene Dienstebenen. Weitere Informationen finden Sie in der [Übersicht über Azure DDoS Protection Standard](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

Die folgenden Arten von Angriffen können mit DDoS Protection Standard abgewehrt werden:

> [!div class="mx-tableFixed"]

|Warnung|BESCHREIBUNG|
|---|---|
|**Volumetric attack detected** (Volumetrischen Angriff erkannt)|Ziel dieses Angriffs ist die Überflutung der Netzwerkebene mit einer beträchtlichen Menge scheinbar legitimen Datenverkehrs. Dazu zählen UDP-Überflutungen, Verstärkungsüberflutungen und andere Überflutungen mit gefälschten Paketen. DDoS Protection Standard wehrt diese bis zu mehreren Gigabytes großen Angriffe ab, indem sie mithilfe des weltweiten Netzwerks automatisch absorbiert und bereinigt werden.|
|**Protocol attack detected** (Protokollangriff erkannt)|Diese Angriffe machen den Zugriff auf ein Ziel unmöglich, indem sie eine Schwachstelle in den Schichten 3 und 4 des Protokollstapels ausnutzen. Dazu gehören SYN-Flutangriffe, Reflexionsangriffe und andere Protokollangriffe. DDoS Protection Standard wehrt diese Angriffe ab und unterscheidet dabei zwischen schädlichem und berechtigtem Datenverkehr. Nach Interaktion mit dem Client wird der schädliche Datenverkehr gesperrt.|
|**Resource (application) layer attack detected** (Angriff in der Ressourcenschicht (Anwendungsschicht) erkannt)|Das Ziel dieser Art von Angriffen sind Webanwendungspakete, um die Datenübertragung zwischen Hosts zu unterbrechen. Zu diesen Angriffen zählen Verletzungen des HTTP-Protokolls, die Einschleusung von SQL-Befehlen, XSS-Angriffe (Cross-Site Scripting) und andere Angriffe auf Schicht 7. Verwenden Sie die WAF (Web Application Firewall) von Azure Application Gateway in Kombination mit DDoS Protection Standard, um sich vor diesen Angriffen zu schützen. Im Azure Marketplace finden Sie auch WAF-Angebote von Drittanbietern.|
