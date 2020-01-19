---
title: Bedrohungserkennungswarnungen von Azure-Sicherheitsprodukten in Azure Security Center
description: In diesem Thema werden die Azure-Sicherheitsprodukte vorgestellt, für die Azure Security Center Bedrohungswarnungen anzeigen kann.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ad4b0373-08ee-46ca-a573-638ed93a647c
ms.service: security-center
ms.topic: conceptual
ms.date: 01/12/2020
ms.author: memildin
ms.openlocfilehash: 16cae819b1714c2b410cfa311a3602e0f4ed968a
ms.sourcegitcommit: e9776e6574c0819296f28b43c9647aa749d1f5a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2020
ms.locfileid: "75913037"
---
# <a name="threat-detection-alerts-from-azure-waf-and-azure-ddos-protection"></a>Warnungen der Bedrohungserkennung von Azure WAF und Azure DDoS Protection

Azure Security Center kann Bedrohungserkennungswarnungen anzeigen und erfassen, die von den folgenden Azure-Sicherheitsprodukten generiert werden (eine separate Lizenz für jedes Produkt ist erforderlich):

* [Azure WAF](#azure-waf)
* [Azure DDoS Protection](#azure-ddos)

## Azure WAF <a name="azure-waf"></a>

Azure Application Gateway verfügt über eine Web Application Firewall (WAF), die den zentralisierten Schutz Ihrer Webanwendungen vor allgemeinen Exploits und Sicherheitsrisiken ermöglicht.

Webanwendungen sind zunehmend Ziele böswilliger Angriffe, die allgemein bekannte Sicherheitslücken ausnutzen. Die Application Gateway-WAF basiert auf Version 3.0 oder 2.2.9 des Kernregelsatzes aus dem Open Web Application Security Project. Die WAF wird automatisch aktualisiert und bietet ganz ohne zusätzliche Konfiguration Schutz vor neuen Sicherheitsrisiken. WAF-Warnungen werden an das Security Center gestreamt. Weitere Informationen zu den von WAF generierten Warnungen finden Sie unter [CRS-Regelgruppen und -Regeln der Web Application Firewall](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md?tabs=owasp31#crs911-31).

## Azure DDoS Protection <a name="azure-ddos"></a>

Verteilte Denial-of-Service-Angriffe (Distributed Denial of Service, DDoS) sind bekanntlich einfach durchführbar. Sie haben sich zu einem wichtigen Sicherheitsthema entwickelt, insbesondere wenn Sie Ihre Anwendungen zur Cloud migrieren. 

Ein DDoS-Angriff hat das Ziel, die Ressourcen einer Anwendung zu verbrauchen, damit sie für berechtigte Benutzer nicht mehr verfügbar ist. DDoS-Angriffe können jeden beliebigen Endpunkt ins Visier nehmen, der über das Internet erreichbar sind.

Azure DDoS Protection trägt in Kombination mit bewährten Anwendungsentwurfsmethoden zum Schutz vor DDoS-Angriffen bei. Azure DDoS Protection bietet verschiedene Dienstebenen. Weitere Informationen finden Sie in der [Übersicht über Azure DDoS Protection Standard](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

Eine Liste der Azure DDoS Protection-Warnungen finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-azureddos).