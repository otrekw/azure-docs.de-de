---
title: 'Web Application Firewall-Ausschlusslisten in Azure Front Door: Azure-Portal'
description: Dieser Artikel enthält Informationen zur Konfiguration von Ausschlusslisten in Azure Front Door mit dem Azure-Portal.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 02/25/2020
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 43565e812abcf0b7dbb992ac4d25a62a4d08df2b
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92018643"
---
# <a name="web-application-firewall-waf-with-front-door-service-exclusion-lists"></a>Web Application Firewall (WAF) mit Ausschlusslisten des Front Door-Diensts 

Manchmal blockiert Web Application Firewall (WAF) eine Anforderung, die Sie für Ihre Anwendung zulassen möchten. Beispielsweise fügt Active Directory Token ein, die für die Authentifizierung verwendet werden. Diese Token können Sonderzeichen enthalten, die ein falsch positives Ergebnis von den WAF-Regeln auslösen können. Mit WAF-Ausschlusslisten können Sie bestimmte Anforderungsattribute in einer WAF-Auswertung weglassen.  Eine Ausschlussliste kann mit [PowserShell](https://docs.microsoft.com/powershell/module/az.frontdoor/New-AzFrontDoorWafManagedRuleExclusionObject?view=azps-3.5.0), der [Azure CLI](https://docs.microsoft.com/cli/azure/ext/front-door/network/front-door/waf-policy/managed-rules/exclusion?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-managed-rules-exclusion-add), der [REST-API](https://docs.microsoft.com/rest/api/frontdoorservice/webapplicationfirewall/policies/createorupdate) oder dem Azure-Portal konfiguriert werden. Im folgenden Beispiel wird die Konfiguration im Azure-Portal gezeigt. 
## <a name="configure-exclusion-lists-using-the-azure-portal"></a>Konfigurieren von Ausschlusslisten mithilfe des Azure-Portals
Auf **Ausschlüsse verwalten** kann über das WAF-Portal unter **Verwaltete Regeln** zugegriffen werden.

![Ausschluss verwalten](../media/waf-front-door-exclusion/exclusion1.png)
![Manage exclusion_add](../media/waf-front-door-exclusion/exclusion2.png)

 Ein Beispiel für eine Ausschlussliste: ![Manage exclusion_define](../media/waf-front-door-exclusion/exclusion3.png)

In diesem Beispiel wird der Wert im Headerfeld *user* ausgeschlossen. Eine gültige Anforderung kann das Feld *user* enthalten, das eine Zeichenfolge enthält, die eine SQL-Einschleusungsregel auslöst. Sie können in diesem Fall den Parameter *user* ausschließen, sodass die WAF-Regel in dem Feld keine Auswertung vornimmt.

Die folgenden Attribute können Ausschlusslisten nach dem Namen hinzugefügt werden. Die Werte der Felder, die Sie verwenden, werden nicht anhand von WAF-Regeln ausgewertet, aber ihre Namen werden ausgewertet. Die Ausschlusslisten entfernen die Überprüfung des Feldwerts.

* Name des Anforderungsheaders
* Name des Anforderungscookies
* Name der Abfragezeichenfolgenargumente
* Name des Anforderungstexts nach Argumenten

Sie können eine exakte Übereinstimmung für Anforderungsheader, Test, Cookies oder Abfragezeichenfolgenattribute angeben.  Oder Sie können optional teilweise Übereinstimmungen angeben. Für Übereinstimmungskriterien werden die folgenden Operatoren unterstützt:

- **Equals** (gleich):  Dieser Operator wird für exakte Übereinstimmungen verwendet. Verwenden Sie beispielsweise zum Auswählen eines Headers namens **bearerToken** den Operator „equals“ mit dem Selektor **bearerToken**.
- **Starts with** (Beginnt mit): Dieser Operator gleicht alle Felder ab, die mit dem angegebenen Selektorwert beginnen.
- **Ends with** (Endet mit):  Dieser Operator gleicht alle Anforderungsfelder ab, die auf den angegebenen Selektorwert enden.
- **Contains** (Enthält): Dieser Operator gleicht alle Anforderungsfelder ab, die den angegebenen Selektorwert enthalten.
- **Gleich beliebige:** Dieser Operator entspricht allen angeforderten Feldern. * ist der Selektorwert.

Bei Header- und Cookienamen wird Groß-/Kleinschreibung nicht berücksichtigt.

Sie können eine Ausschlussliste auf alle Regeln innerhalb des verwalteten Regelsatzes, auf Regeln für eine bestimmte Regelgruppe oder auf eine einzelne Regel anwenden, wie im vorherigen Beispiel gezeigt. 

## <a name="define-exclusion-based-on-web-application-firewall-logs"></a>Definieren von Ausschlüssen auf Basis von Web Application Firewall-Protokollen
 [Überwachung und Protokollierung von Azure Web Application Firewall](waf-front-door-monitor.md) zeigt übereinstimmende Details einer blockierten Anforderung. Wenn ein Headerwert, ein Cookiewert, ein post-Argumentwert oder ein Abfrageargumentwert für manche Regeln falsch positive Ergebnisse erzeugt, können Sie diesen Teil der Anforderung von der Berücksichtigung in der Regel ausschließen. Die folgende Tabelle zeigt Beispielwerte aus WAF-Protokollen und die entsprechenden Ausschlussbedingungen.

|matchVariableName aus WAF-Protokollen    |Regelausschluss im Portal|
|--------|------|
|CookieValue:SOME_NAME  |Name des Anforderungscookies entspricht SOME_NAME|
|HeaderValue:SOME_NAME  |Name des Anforderungsheaders entspricht SOME_NAME|
|PostParamValue:SOME_NAME|  Name des Anforderungstext-post-Arguments entspricht SOME_NAME|
|QueryParamValue:SOME_NAME| Argumentname der Abfragezeichenfolge entspricht SOME_NAME|


## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren Ihrer WAF-Einstellungen informieren Sie sich darüber, wie Sie WAF-Protokolle anzeigen. Weitere Informationen finden Sie unter [Front Door-Diagnose](../afds/waf-front-door-monitor.md).
