---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 07/10/2019
ms.author: danlep
ms.openlocfilehash: 2b28cc742bb981e0db535d39cb0c7fbf027470ea
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "83673763"
---
### <a name="identity-tier"></a>Identitätsschicht 

Die Partnerschaft zwischen Microsoft und Oracle ermöglicht die Einrichtung einer einheitlichen Identität in Azure, OCI und Ihrer Oracle-Anwendung. Für die JD Edwards EnterpriseOne- und die PeopleSoft-Anwendungssuite wird eine OHS-Instanz (Oracle-HTTP-Server) benötigt, um das einmalige Anmelden zwischen Azure AD und Oracle IDCS einzurichten.

OHS fungiert als Reverseproxy für die Logikschicht. Das bedeutet, dass alle an die Endanwendungen gerichteten Anforderungen OHS durchlaufen. Oracle Access Manager WebGate ist ein OHS-Webserver-Plug-In, das jede an die Endanwendung gerichtete Anforderung abfängt. Handelt es sich bei einer Ressource, auf die zugegriffen wird, um eine geschützte Ressource, für die eine authentifizierte Sitzung erforderlich ist, initiiert WebGate den OIDC-Authentifizierungsablauf mit Identity Cloud Service über den Browser des Benutzers. Weitere Informationen zu den Abläufen, die von OpenID Connect WebGate unterstützt werden, finden Sie in der [Oracle Access Manager-Dokumentation](https://docs.oracle.com/cd/E52734_01/oam/AIAAG/GUID-1E927D1B-FB83-425B-8768-85DB441821A4.htm#AIAAG7327).

Mit diesem Setup kann ein bereits bei Azure AD angemeldeter Benutzer zur JD Edwards EnterpriseOne- oder PeopleSoft-Anwendung navigieren, ohne sich erneut über Oracle Identity Cloud Service anzumelden. Kunden, die diese Lösung bereitstellen, profitieren von den Vorteilen des einmaligen Anmeldens. Hierzu zählen etwa ein einzelner Satz von Anmeldeinformationen, ein verbessertes Anmeldeverfahren, höhere Sicherheit und geringere Helpdeskkosten.

Weitere Informationen zur Einrichtung des einmaligen Anmeldens für JD Edwards EnterpriseOne oder PeopleSoft mit Azure AD finden Sie im entsprechenden [Oracle-Whitepaper](https://cloud.oracle.com/iaas/whitepapers/deploy_peoplesoft_jdedwards_across_oci_azure.pdf).