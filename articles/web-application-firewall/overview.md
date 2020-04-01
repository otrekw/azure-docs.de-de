---
title: Einführung zu Azure Web Application Firewall
description: Dieser Artikel enthält eine Übersicht über die Azure Web Application Firewall (WAF)
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 03/18/2020
ms.author: victorh
ms.topic: overview
ms.openlocfilehash: 49024c86d09f5cdd9e8b04d5a49f60021660b0c4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "79475786"
---
# <a name="what-is-azure-web-application-firewall"></a>Was ist die Azure Web Application Firewall?

Die Web Application Firewall (WAF) bietet zentralisierten Schutz Ihrer Webanwendungen vor verbreiteten Exploits und Sicherheitsrisiken. Webanwendungen sind zunehmend Ziele böswilliger Angriffe, die allgemein bekannte Sicherheitslücken ausnutzen. Einschleusung von SQL-Befehlen und websiteübergreifendes Skripting gehören zu den häufigsten Angriffen.

![WAF-Übersicht](media/overview/wafoverview.png)

Es ist schwierig, solche Angriffe im Anwendungscode zu verhindern. Hierzu können rigorose Wartungs-, Patch- und Überwachungsmaßnahmen auf mehreren Ebenen der Anwendungstopologie erforderlich sein. Eine zentrale Web Application Firewall vereinfacht die Sicherheitsverwaltung erheblich. Eine WAF bietet auch Anwendungsadministratoren eine bessere Gewähr des Schutzes vor Bedrohungen und Angriffen.

Mit einer WAF-Lösung können Sie schneller auf ein Sicherheitsrisiko reagieren, da eine bekannte Schwachstelle an einem zentralen Ort gepatcht wird, statt jede einzelne Webanwendung separat zu sichern.

## <a name="supported-service"></a>Unterstützter Dienst

WAF kann mit Azure Application Gateway, Azure Front Door und dem Azure CDN-Dienst (Content Delivery Network) von Microsoft bereitgestellt werden. WAF für Azure CDN befindet sich derzeit in der öffentlichen Vorschauphase.  WAF verfügt über Features, die für jeden spezifischen Dienst angepasst werden. Weitere Informationen zu den WAF-Features für die einzelnen Dienste finden Sie in der Übersicht zu den einzelnen Diensten.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Web Application Firewall auf Application Gateway finden Sie unter [Web Application Firewall auf Azure Application Gateway](./ag/ag-overview.md).
- Weitere Informationen zu Web Application Firewall in Azure Front Door Service finden Sie unter [Azure Web Application Firewall für Azure Front Door](./afds/afds-overview.md).
- Weitere Informationen zu Web Application Firewall unter dem Azure CDN-Dienst finden Sie unter [Azure Web Application Firewall für den Azure CDN-Dienst](./cdn/cdn-overview.md).
