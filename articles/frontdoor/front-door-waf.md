---
title: Schnelles Skalieren und Schützen einer Webanwendung mithilfe von Azure Front Door und Azure Web Application Firewall (WAF) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Web Application Firewall mit Ihrem Azure Front Door-Dienst verwenden.
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/06/2020
ms.author: duau
ms.openlocfilehash: 7d2978b34d4c1e6cf85c65be2e9c3292ec704be4
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89398884"
---
# <a name="quickly-scale-and-protect-a-web-application-using-azure-front-door-and-azure-web-application-firewall-waf"></a>Schnelles Skalieren und Schützen einer Webanwendung mithilfe von Azure Front Door und Azure Web Application Firewall (WAF)

Bei vielen Webanwendungen hat der Datenverkehr im Zusammenhang mit COVID-19 in den letzten Wochen schnell zugenommen. Darüber hinaus lässt sich bei diesen Webanwendungen auch ein Anstieg des schädlichen Datenverkehrs beobachten, einschließlich Denial-of-Service-Angriffen. Eine effektive Möglichkeit zur Bewältigung beider Anforderungen, Aufskalieren für den Anstieg des Datenverkehrs einerseits und Schutz vor Angriffen andererseits, besteht darin, Azure Front Door mit Azure WAF als Beschleunigungs-, Caching- und Sicherheitsebene vor der Webanwendung einzurichten. Dieser Artikel enthält eine Anleitung dazu, wie Azure Front Door mit Azure WAF schnell für alle in oder außerhalb von Azure ausgeführten Webanwendungen eingerichtet werden kann. 

WAF wird in diesem Tutorial über die Azure-Befehlszeilenschnittstelle eingerichtet, alle diese Schritte werden jedoch auch vollständig im Azure-Portal, über Azure PowerShell, Azure ARM und Azure-REST-APIs unterstützt. 

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

Bei den Anweisungen in diesem Blog wird die Azure-Befehlszeilenschnittstelle (CLI) verwendet. Lesen Sie [diese Anleitung](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) für Ihre ersten Schritte mit der Azure-Befehlszeilenschnittstelle.

*Tipp: Mit dem [Schnellstart für Bash in Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart)* gelingt der Einstieg in die Azure-Befehlszeilenschnittstelle schnell und einfach.

Stellen Sie sicher, dass die Front Door-Erweiterung in der Azure-Befehlszeilenschnittstelle hinzugefügt ist.

```azurecli-interactive 
az extension add --name front-door
```

Hinweis: Weitere Informationen zu den im Folgenden aufgeführten Befehlen finden Sie in der [Referenz zur Azure-Befehlszeilenschnittstelle für Front Door](https://docs.microsoft.com/cli/azure/ext/front-door/?view=azure-cli-latest).

## <a name="step-1-create-an-azure-front-door-afd-resource"></a>Schritt 1: Erstellen einer Azure Front Door-Ressource (AFD-Ressource)


```azurecli-interactive 
az network front-door create --backend-address <>  --accepted-protocols <> --name <> --resource-group <>
```

**--backend-address:** Die Back-End-Adresse ist der vollqualifizierte Domänenname (FQDN) der Anwendung, die geschützt werden soll. Beispiel: myapplication.contoso.com

**--accepted-protocols:** Die zulässigen Protokolle geben an, welche Protokolle in AFD für die Webanwendung unterstützt werden sollen. Beispiel: --accepted-protocols Http Https

**--name**: Geben Sie einen Namen für die AFD-Ressource an.

**--resource-group:** Die Ressourcengruppe, in die die AFD-Ressource eingefügt werden soll.  Weitere Informationen zu Ressourcengruppen finden Sie im Thema zum Verwalten von Ressourcengruppen in Azure.

Suchen Sie in der Antwort, die bei erfolgreicher Ausführung dieses Befehls zurückgegeben wird, den Schlüssel „hostName“, und notieren Sie den zugehörigen Wert zur Verwendung in einem späteren Schritt. hostName ist der DNS-Name der erstellten AFD-Ressource.

## <a name="step-2-create-an-azure-waf-profile-to-use-with-azure-front-door-resources"></a>Schritt 2: Erstellen eines Azure WAF-Profils zur Verwendung mit Azure Front Door-Ressourcen

```azurecli-interactive 
az network front-door waf-policy create --name <>  --resource-group <>  --disabled false --mode Prevention
```

--name: Geben Sie einen Namen für die Azure WAF-Richtlinie an.

--resource-group: die Ressourcengruppe, in die die WAF-Ressource eingefügt werden soll. 

Mit dem CLI-Code oben wird eine WAF-Richtlinie erstellt, die aktiviert ist und sich im Schutzmodus befindet. 

Hinweis: Sie können WAF auch im Erkennungsmodus erstellen und damit testen, wie bösartige Anforderungen erkannt und protokolliert (aber nicht blockiert) werden, bevor Sie den Schutzmodus festlegen.

Suchen Sie in der Antwort, die bei erfolgreicher Ausführung dieses Befehls zurückgegeben wird, den Schlüssel „ID“, und notieren Sie den zugehörigen Wert zur Verwendung in einem späteren Schritt. Das Feld „ID“ sollte das folgende Format aufweisen:

/subscriptions/**Abonnement-ID**/resourcegroups/**Ressourcengruppenname**/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/**Name der WAF-Richtlinie**

## <a name="step-3-add-managed-rulesets-to-this-waf-policy"></a>Schritt 3: Hinzufügen verwalteter Regelsätze zur WAF-Richtlinie

Sie können einer WAF-Richtlinie verwaltete Regelsätze hinzufügen, bei denen es sich um eine Reihe von Regeln handelt, die von Microsoft erstellt und verwaltet werden und einen vorkonfigurierten Schutz für ganze Bedrohungsklassen bieten. In diesem Beispiel fügen wir zwei solcher Regelsätze hinzu: (1) DefaultRuleSet (Standardregelsatz) zum Schutz vor gängigen Webbedrohungen und (2) BotManagerRuleSet (Regelsatz für Bot-Schutz) zum Schutz vor schädlichen Bots

(1) Hinzufügen von DefaultRuleSet

```azurecli-interactive 
az network front-door waf-policy managed-rules add --policy-name <> --resource-group <> --type DefaultRuleSet --version 1.0
```

(2) Hinzufügen von BotManagerRuleSet

```azurecli-interactive 
az network front-door waf-policy managed-rules add --policy-name <> --resource-group <> --type Microsoft_BotManagerRuleSet --version 1.0
```

--policy-name: der Name, den Sie für die Azure WAF-Ressource angegeben haben

--resource-group: die Ressourcengruppe, in die die WAF-Ressource eingefügt wurde

## <a name="step-4-associate-the-waf-policy-with-the-afd-resource"></a>Schritt 4: Zuordnen der WAF-Richtlinie zur AFD-Ressource

In diesem Schritt ordnen Sie die erstellte WAF-Richtlinie der AFD-Ressource zu, die sich vor der Webanwendung befindet.

```azurecli-interactive 
az network front-door update --name <> --resource-group <> --set frontendEndpoints[0].webApplicationFirewallPolicyLink='{"id":"<>"}'
```

--name: der Name, den Sie für die AFD-Ressource angegeben haben

--resource-group: die Ressourcengruppe, in die die Azure Front Door-Ressource eingefügt wurde

--set: Hier aktualisieren Sie das Attribut „WebApplicationFirewallPolicyLink“ für den „frontendEndpoint“, der der AFD-Ressource zugeordnet ist, mit der neu erstellten WAF-Richtlinie. Die ID der WAF-Richtlinie ist in der Antwort angegeben, die in Schritt 2 oben zurückgegeben wurde.

Hinweis: Das Beispiel oben bezieht sich auf den Fall, dass Sie keine benutzerdefinierte Domäne verwenden.

Wenn Sie keine benutzerdefinierten Domänen verwenden, um auf Ihre Webanwendungen zuzugreifen, können Sie Schritt 5 überspringen. In diesem Fall stellen Sie Ihren Endbenutzern für die Navigation zu Ihrer Webanwendung den Hostnamen zur Verfügung, den Sie in Schritt 1 erhalten haben.

## <a name="step-5-configure-custom-domain-for-your-web-application"></a>Schritt 5: Konfigurieren der benutzerdefinierten Domäne für die Webanwendung

Anfänglich verwies der Name der benutzerdefinierten Domäne Ihrer Webanwendung (der Domänenname, mit dem Benutzer sich auf Ihre Anwendung beziehen, z. B. www.contoso.com) auf den Ort, an dem sie vor dem Hinzufügen von AFD ausgeführt wurde. Nach der Änderung der Architektur durch Hinzufügen von AFD und WAF vor der Anwendung, muss der DNS-Eintrag, der der benutzerdefinierten Domäne entspricht, nun auf die AFD-Ressource verweisen. Zu diesem Zweck können Sie diesen Eintrag im DNS-Server dem in Schritt 1 notierten AFD-Hostnamen neu zuordnen.

Die spezifischen Schritte zum Aktualisieren der DNS-Einträge hängen vom verwendeten DNS-Dienstanbieter ab. Wenn Sie den DNS-Namen jedoch mithilfe von Azure DNS hosten, finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/dns/dns-operations-recordsets-cli) entsprechende Schritte zum Aktualisieren eines DNS-Eintrags, um auf den AFD-Hostnamen zu verweisen. 

Hierbei ist Folgendes zu beachten: Wenn Ihre Benutzer über den Zonen-Apex zu Ihrer Website navigieren sollen, z. B. contoso.com, müssen Sie Azure DNS und den zugehörigen [Aliaseintragstyp](https://docs.microsoft.com/azure/dns/dns-alias) zum Hosten des DNS-Namens verwenden. 

Außerdem müssen Sie auch die AFD-Konfiguration aktualisieren und ihr [die benutzerdefinierte Domäne](https://docs.microsoft.com/azure/frontdoor/front-door-custom-domain) hinzufügen, sodass die Zuordnung in AFD erkannt wird.

Wenn Sie letztendlich eine benutzerdefinierte Domäne für die Navigation zur Webanwendung verwenden und das HTTPS-Protokoll aktivieren möchten, müssen Sie über die [Zertifikate für die Einrichtung der benutzerdefinierten Domäne in AFD](https://docs.microsoft.com/azure/frontdoor/front-door-custom-domain-https) verfügen. 

## <a name="step-6-lock-down-your-web-application"></a>Schritt 6: Sperren der Webanwendung

Eine optionale bewährte Methode besteht darin sicherzustellen, dass nur AFD-Edges mit der Webanwendung kommunizieren können. Dadurch wird sichergestellt, dass kein Benutzer den AFD-Schutz umgehen und direkt auf Ihre Anwendungen zugreifen kann. Informationen zum Einrichten dieser Sperre finden Sie unter [Häufig gestellte Fragen zu Azure Front Door](https://docs.microsoft.com/azure/frontdoor/front-door-faq) unter der Frage „Wie kann ich den Zugriff auf mein Back-End nur auf Azure Front Door beschränken?“.
