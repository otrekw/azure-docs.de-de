---
title: Azure Front Door – Unterstützung für Platzhalterdomänen
description: In diesem Artikel erfahren Sie, wie Azure Front Door die Zuordnung und Verwaltung von Platzhalterdomänen in der Liste benutzerdefinierter Domänen unterstützt.
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2020
ms.author: sharadag
ms.openlocfilehash: c568c9cc5c57098385cc7399459ec656cdbfc305
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79540962"
---
# <a name="wildcard-domains"></a>Platzhalterdomänen

Abgesehen von Apex-Domänen und Unterdomänen können Sie der Liste von Front-End-Hosts oder benutzerdefinierten Domänen Ihres Front Door-Profils auch einen Platzhalter-Domänennamen zuordnen. Platzhalterdomänen in der Front Door-Konfiguration vereinfachen das Routingverhalten von Datenverkehr für mehrere Unterdomänen für eine API, Anwendung oder Website mit derselben Routingregel, ohne dass die Konfiguration geändert werden muss, um die einzelnen Unterdomänen separat hinzuzufügen und/oder anzugeben. Beispielsweise können Sie das Routing für `customer1.contoso.com`, `customer2.contoso.com` und `customerN.contoso.com` mithilfe derselben Routingregel definieren, indem Sie eine Platzhalterdomäne `*.contoso.com` hinzufügen.

Zu den wichtigsten Szenarien, die mit Unterstützung für Platzhalterdomänen gelöst werden, gehören folgende:

- Es ist nicht mehr erforderlich, ein Onboarding der einzelnen Unterdomänen in Ihrer Front Door-Instanz durchzuführen und anschließend HTTPS zu aktivieren, um ein Zertifikat an jede Unterdomäne zu binden.
- Wenn eine Anwendung eine neue Unterdomäne hinzufügt, muss die Front Door-Produktionskonfiguration nicht mehr geändert werden. Zuvor war es erforderlich, die Unterdomäne hinzuzufügen, ein Zertifikat daran zu binden, eine Web Application Firewall-Richtlinie (WAF) anzufügen und die Domäne zu verschiedenen Routingregeln hinzuzufügen.

> [!NOTE]
> Platzhalterdomänen werden derzeit nur über die API, PowerShell und die CLI unterstützt. Unterstützung für das Hinzufügen und Verwalten von Platzhalterdomänen über das Azure-Portal ist nicht verfügbar.

## <a name="adding-wildcard-domains"></a>Hinzufügen von Platzhalterdomänen

Sie können das Onboarding für eine Platzhalterdomäne im Abschnitt „Front-End-Hosts“ oder „Domänen“ durchführen. Ähnlich wie bei Unterdomänen überprüft die Front Door-Instanz auch, ob eine CNAME-Zuordnung für Ihre Platzhalterdomäne vorhanden ist. Diese DNS-Zuordnung kann eine direkte CNAME-Zuordnung sein, z. B. `*.contoso.com`, das `contoso.azurefd.net` zugeordnet ist, oder über die temporäre afdverify-Zuordnung wie `afdverify.contoso.com`, das `afdverify.contoso.azurefd.net` zugeordnet ist. Die CNAME-Zuordnung wird auch für Platzhalter überprüft (Azure DNS unterstützt Platzhalter-Datensätze).

Sie können auch beliebig viele Unterdomänen mit einer Ebene der Platzhalterdomäne zu Front-End-Hosts hinzufügen, wenn Sie den maximalen Wert von Front-End-Hosts nicht überschreiten. Diese Funktion ist unter Umständen erforderlich, um eine andere Route für eine Unterdomäne zu definieren als für die übrigen Domänen (ausgehend von der Platzhalterdomäne) oder um eine andere WAF-Richtlinie für eine bestimmte Unterdomäne festzulegen. `*.contoso.com` erlaubt somit das Hinzufügen von `foo.contoso.com`, ohne dass der Domänenbesitz erneut nachgewiesen werden muss, aber nicht von `foo.bar.contoso.com`, da dies keine Unterdomäne von `*.contoso.com` mit einer Ebene ist. Damit Sie `foo.bar.contoso.com` ohne eine zusätzliche Überprüfung des Domänenbesitzes hinzufügen können, muss `*.bar.contosonews.com` hinzugefügt werden.

### <a name="limitations"></a>Einschränkungen

1. Wenn eine Platzhalterdomäne zu einem bestimmten Front Door-Profil hinzugefügt wird, kann sie zu keinem anderen Front Door-Profil hinzugefügt werden. 
2. Wenn eine Platzhalterdomäne zu einem bestimmten Front Door-Profil hinzugefügt wird, können alle Unterdomänen dieser Platzhalterdomäne zu keinem anderen Front Door-Profil und nicht zu einem Profil von Azure CDN von Microsoft hinzugefügt werden.
3. Wenn eine Unterdomäne einer Platzhalterdomäne entweder zu einem Front Door-Profil oder zu einem Profil von Azure CDN von Microsoft hinzugefügt wird, kann die Platzhalterdomäne zu keinem anderen Front Door-Profil hinzugefügt werden. 
4. Wenn zwei Profile (Front Door oder Azure CDN von Microsoft) über verschiedene Unterdomänen einer Stammdomäne verfügen, können Platzhalterdomänen zu keinem der Profile hinzugefügt werden.

## <a name="certificate-binding-for-wildcard-domains-and-its-subdomains"></a>Zertifikatbindung für Platzhalterdomänen und deren Unterdomänen

Um HTTPS-Datenverkehr für Ihre Platzhalterdomäne zu akzeptieren, müssen Sie HTTPS für die Platzhalterdomäne aktivieren. Die Zertifikatbindung für die Platzhalterdomäne erfordert ein Platzhalterzertifikat. Das heißt, der Name des Zertifikatantragstellers muss auch über die Platzhalterdomäne verfügen.

> [!NOTE]
> Derzeit ist nur die Verwendung Ihrer eigenen benutzerdefinierten SSL-Zertifikatoption zum Aktivieren von HTTPS für Platzhalterdomänen verfügbar. Von Front Door verwaltete Zertifikate können nicht für Platzhalterdomänen verwendet werden. 

Sie können das Platzhalterzertifikat aus Ihrem Key Vault für die Unterdomänen verwenden; die Verwendung von Zertifikaten, die von Front Door verwaltet werden, für Unterdomänen wird aber ebenfalls unterstützt.
Wenn eine Unterdomäne für eine Platzhalterdomäne hinzugefügt wird und der Platzhalterdomäne bereits ein Zertifikat zugeordnet ist, kann HTTPS für diese Unterdomäne nicht deaktiviert werden. Die Unterdomäne verwendet standardmäßig die Zertifikatbindung der Platzhalterdomäne, sofern dies nicht von einem anderen Key Vault-Zertifikat oder einem von Front Door verwalteten Zertifikat überschrieben wird.

## <a name="web-application-firewall-for-wildcard-domains-and-its-subdomains"></a>Web Application Firewall für Platzhalterdomänen und deren Unterdomänen

WAF-Richtlinien können einer Platzhalterdomäne zugeordnet werden, wie es bei anderen Domänen der Fall ist. Eine andere WAF-Richtlinie kann auf eine Unterdomäne einer Platzhalterdomäne angewendet werden. Für die Unterdomänen müssen Sie die zu verwendende WAF-Richtlinie explizit angeben, auch wenn es sich um dieselbe Richtlinie wie die Platzhalterdomäne handelt. Unterdomänen erben **nicht** automatisch die WAF-Richtlinie von der Platzhalterdomäne.

Wenn Sie ein Szenario haben, in dem WAF für eine Unterdomäne nicht ausgeführt werden soll, können Sie eine leere WAF-Richtlinie ohne verwaltete oder benutzerdefinierte RuleSets erstellen.

## <a name="routing-rules-for-wildcard-domains-and-its-subdomains"></a>Routingregeln für Platzhalterdomänen und deren Unterdomänen

Beim Konfigurieren einer Routingregel können Sie eine Platzhalterdomäne als Front-End-Host auswählen. Sie können für Platzhalterdomänen auch ein anderes Routingverhalten als für Unterdomänen festlegen. Wie unter [Abgleich von Anforderungen mit Routingregeln durch Front Door](front-door-route-matching.md) beschrieben, wird der genaueste Treffer für die Domäne bei unterschiedlichen Routingregeln zur Laufzeit ausgewählt.

> [!WARNING]
> Wenn Sie über zwei Routingregeln verfügen, z. B. über **Route 1**: `*.foo.com/*` ist dem Back-End-Pool A zugeordnet und **Route 2**: `bar.foo.com/somePath/*` ist dem Back-End-Pool B zugeordnet und wenn eine Anforderung für `bar.foo.com/anotherPath/*`eingeht, erhalten die Clients Fehler, da Front Door keine Treffer für beide Routen findet. Dies liegt daran, dass Front Door dem [Algorithmus für den Routingabgleich](front-door-route-matching.md) entsprechend Route 2 basierend auf einer spezifischeren Domänenübereinstimmung auswählt, aber nur um zu festzustellen, dass keine übereinstimmenden Pfadmuster vorhanden sind. 


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](quickstart-create-front-door.md).
- Erfahren Sie mehr über das [Hinzufügen einer benutzerdefinierten Domäne in Front Door](front-door-custom-domain.md).
- Erfahren Sie mehr über das [Aktivieren von HTTPS für eine benutzerdefinierte Domäne](front-door-custom-domain-https.md).
