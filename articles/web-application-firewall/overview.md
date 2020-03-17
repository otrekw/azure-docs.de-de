---
title: Einführung zu Azure Web Application Firewall
description: Dieser Artikel enthält eine Übersicht über die Azure Web Application Firewall (WAF)
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 03/06/2020
ms.author: victorh
ms.topic: overview
ms.openlocfilehash: 44bc8db5d8ada0378f8f9d0911ed398ba491d289
ms.sourcegitcommit: f5e4d0466b417fa511b942fd3bd206aeae0055bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78851185"
---
# <a name="what-is-azure-web-application-firewall"></a>Was ist die Azure Web Application Firewall?

Die Web Application Firewall (WAF) bietet zentralisierten Schutz Ihrer Webanwendungen vor verbreiteten Exploits und Sicherheitsrisiken. Webanwendungen sind zunehmend Ziele böswilliger Angriffe, die allgemein bekannte Sicherheitslücken ausnutzen. Einschleusung von SQL-Befehlen und websiteübergreifendes Skripting gehören zu den häufigsten Angriffen.

![WAF-Übersicht](media/overview/wafoverview.png)

Es ist schwierig, solche Angriffe im Anwendungscode zu verhindern. Hierzu können rigorose Wartungs-, Patch- und Überwachungsmaßnahmen auf mehreren Ebenen der Anwendungstopologie erforderlich sein. Eine zentrale Web Application Firewall vereinfacht die Sicherheitsverwaltung erheblich. Eine WAF bietet auch Anwendungsadministratoren eine bessere Gewähr des Schutzes vor Bedrohungen und Angriffen.

Mit einer WAF-Lösung können Sie schneller auf ein Sicherheitsrisiko reagieren, da eine bekannte Schwachstelle an einem zentralen Ort gepatcht wird, statt jede einzelne Webanwendung separat zu sichern.

## <a name="supported-services"></a>Unterstützte Dienste

WAF kann mit [Azure Application Gateway](../application-gateway/overview.md) und [Azure Front Door Service](../frontdoor/front-door-overview.md) bereitgestellt werden. Bei beiden Diensten handelt es sich um Layer-7-Lastenausgleichsmodule (HTTP/S), Application Gateway ist jedoch ein regionaler Dienst und Front Door ein globaler Dienst. WAF verfügt über Features, die für jeden spezifischen Dienst angepasst werden.

Weitere Informationen finden Sie in der WAF-Übersicht für die einzelnen Dienste.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Web Application Firewall auf Application Gateway finden Sie unter [Web Application Firewall auf Azure Application Gateway](./ag/ag-overview.md).
- Weitere Informationen zu Web Application Firewall in Azure Front Door Service finden Sie unter [Azure Web Application Firewall für Azure Front Door](./afds/afds-overview.md).
