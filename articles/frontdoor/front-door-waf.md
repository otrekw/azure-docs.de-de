---
title: Skalieren und Schützen einer Web-App mithilfe von Azure Front Door und WAF
description: In diesem Tutorial erfahren Sie, wie Sie Azure Web Application Firewall mit Azure Front Door Service verwenden.
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2020
ms.author: duau
ms.openlocfilehash: 2d531289a1d6e8c484b0334e570d943acdb82268
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91276253"
---
# <a name="tutorial-quickly-scale-and-protect-a-web-application-by-using-azure-front-door-and-azure-web-application-firewall-waf"></a>Tutorial: Schnelles Skalieren und Schützen einer Webanwendung mithilfe von Azure Front Door und Azure Web Application Firewall (WAF)

Bei vielen Webanwendungen hat sich das Datenverkehrsaufkommen aufgrund von COVID-19 in den letzten Wochen schnell erhöht. Darüber hinaus ist bei diesen Webanwendungen eine starke Zunahme von schädlichem Datenverkehr zu beobachten, etwa in Form von Denial-of-Service-Angriffen. Es gibt eine effektive Möglichkeit, um Ihre Ressourcen zur Bewältigung der Datenverkehrszunahme aufzuskalieren und sich gleichzeitig vor Angriffen zu schützen: Richten Sie Azure Front Door mit Azure WAF als Beschleunigungs-, Caching- und Sicherheitsebene vor Ihrer Web-App ein. In diesem Artikel erfahren Sie, wie Sie Azure Front Door mit Azure WAF schnell für eine beliebige Web-App einrichten, die innerhalb oder außerhalb von Azure ausgeführt wird. 

In diesem Tutorial wird die WAF mithilfe der Azure-Befehlszeilenschnittstelle eingerichtet. Sie können dazu aber auch das Azure-Portal, Azure PowerShell, Azure Resource Manager oder die Azure-REST-APIs verwenden. 

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> - Erstellen einer Front Door-Instanz
> - Erstellen einer Azure WAF-Richtlinie
> - Konfigurieren von Regelsätzen für eine WAF-Richtlinie
> - Zuordnen einer WAF-Richtlinie zu Front Door
> - Konfigurieren einer benutzerdefinierten Domäne

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

- In den Schritten dieses Tutorials wird die Azure-Befehlszeilenschnittstelle verwendet. Informationen zu den ersten Schritten mit der Azure-Befehlszeilenschnittstelle finden Sie in [dieser Anleitung](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).

  > [!TIP] 
  > [Bash in Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) ermöglicht einen schnellen und einfachen Einstieg in die Verwendung der Azure-Befehlszeilenschnittstelle.

- Stellen Sie sicher, dass der Azure-Befehlszeilenschnittstelle die Erweiterung `front-door` hinzugefügt wurde:

   ```azurecli-interactive 
   az extension add --name front-door
   ```

> [!NOTE] 
> Weitere Informationen zu den in diesem Tutorial verwendeten Befehlen finden Sie in der [Azure CLI-Referenz für Front Door](https://docs.microsoft.com/cli/azure/ext/front-door/?view=azure-cli-latest).

## <a name="create-an-azure-front-door-resource"></a>Erstellen einer Azure Front Door-Ressource

```azurecli-interactive 
az network front-door create --backend-address <>  --accepted-protocols <> --name <> --resource-group <>
```

`--backend-address`: Der vollqualifizierte Domänenname (Fully Qualified Domain Name, FQDN) der zu schützenden Anwendung. Beispiel: `myapplication.contoso.com`.

`--accepted-protocols`: Dient zum Angeben der Protokolle, die von Azure Front Door für Ihre Webanwendung unterstützt werden sollen. Beispiel: `--accepted-protocols Http Https`.

`--name`: Der Name Ihrer Azure Front Door-Ressource.

`--resource-group`: Die Ressourcengruppe, in der die Azure Front Door-Ressource platziert werden soll. Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwalten von Azure Resource Manager-Gruppen mithilfe des Azure-Portals](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal).

Suchen Sie in der Antwort des ausgeführten Befehls nach dem Schlüssel `hostName`. Dieser Wert wird in einem späteren Schritt benötigt. Der Hostname (`hostName`) ist der DNS-Name der von Ihnen erstellten Azure Front Door-Ressource.

## <a name="create-an-azure-waf-profile-to-use-with-azure-front-door-resources"></a>Erstellen eines Azure WAF-Profils zur Verwendung mit Azure Front Door-Ressourcen

```azurecli-interactive 
az network front-door waf-policy create --name <>  --resource-group <>  --disabled false --mode Prevention
```

`--name`: Der Name der neuen Azure WAF-Richtlinie.

`--resource-group`: Die Ressourcengruppe, in der diese WAF-Ressource platziert werden soll. 

Durch den obigen CLI-Code wird eine aktivierte WAF-Richtlinie im Schutzmodus erstellt. 

> [!NOTE] 
> Es empfiehlt sich unter Umständen, die WAF-Richtlinie im Erkennungsmodus zu erstellen und zu beobachten, wie sie schädliche Anforderungen erkennt und protokolliert (ohne sie zu blockieren), bevor Sie sich für die Verwendung des Schutzmodus entscheiden.

Suchen Sie in der Antwort des ausgeführten Befehls nach dem Schlüssel `ID`. Dieser Wert wird in einem späteren Schritt benötigt. 

Für das Feld `ID` muss das folgende Format verwendet werden:

/subscriptions/**Abonnement-ID**/resourcegroups/**Ressourcengruppenname**/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/**Name der WAF-Richtlinie**

## <a name="add-managed-rule-sets-to-the-waf-policy"></a>Hinzufügen verwalteter Regelsätze zur WAF-Richtlinie

Einer WAF-Richtlinie können verwaltete Regelsätze hinzugefügt werden. Bei einem verwalteten Regelsatz handelt es sich um eine Reihe von Regeln, die von Microsoft erstellt und verwaltet werden und zum Schutz vor einer Bedrohungsklasse beitragen. In diesem Beispiel werden zwei Regelsätze hinzugefügt:
- Der Standardregelsatz zum Schutz vor gängigen Webbedrohungen 
- Der Bot-Schutzregelsatz zum Schutz vor schädlichen Bots

Fügen Sie den Standardregelsatz hinzu:

   ```azurecli-interactive 
   az network front-door waf-policy managed-rules add --policy-name <> --resource-group <> --type DefaultRuleSet --version 1.0
   ```

Fügen Sie den Bot-Schutzregelsatz hinzu:

   ```azurecli-interactive 
   az network front-door waf-policy managed-rules add --policy-name <> --resource-group <> --type Microsoft_BotManagerRuleSet --version 1.0
   ```

`--policy-name`: Der Name, den Sie für Ihre Azure WAF-Ressource angegeben haben.

`--resource-group`: Die Ressourcengruppe, in der Sie die WAF-Ressource platziert haben.

## <a name="associate-the-waf-policy-with-the-azure-front-door-resource"></a>Zuordnen der WAF-Richtlinie zur Azure Front Door-Ressource

In diesem Schritt wird die erstellte WAF-Richtlinie der Azure Front Door-Ressource zugeordnet, die sich vor Ihrer Webanwendung befindet:

```azurecli-interactive 
az network front-door update --name <> --resource-group <> --set frontendEndpoints[0].webApplicationFirewallPolicyLink='{"id":"<>"}'
```

`--name`: Der Name, den Sie für Ihre Azure Front Door-Ressource angegeben haben.

`--resource-group`: Die Ressourcengruppe, in der Sie die Azure Front Door-Ressource platziert haben.

`--set`: Aktualisieren Sie hier das Attribut `WebApplicationFirewallPolicyLink` für den Front-End-Endpunkt (`frontendEndpoint`), der Ihrer Azure Front Door-Ressource zugeordnet ist, mit der neuen WAF-Richtlinie. Verwenden Sie die ID der WAF-Richtlinie, die Sie weiter oben in diesem Tutorial beim Erstellen des WAF-Profils erhalten haben.

 > [!NOTE] 
> Das vorherige Beispiel ist anwendbar, wenn Sie keine benutzerdefinierte Domäne verwenden. Wenn Sie keine benutzerdefinierten Domänen verwenden, um auf Ihre Webanwendungen zuzugreifen, können Sie den nächsten Abschnitt überspringen. In diesem Fall geben Sie den Hostnamen (`hostName`), den Sie beim Erstellen der Azure Front Door-Ressource erhalten haben, an Ihre Kunden weiter. Diese verwenden dann den Hostnamen (`hostName`), um zu Ihrer Webanwendung zu gelangen.

## <a name="configure-the-custom-domain-for-your-web-application"></a>Konfigurieren der benutzerdefinierten Domäne für Ihre Webanwendung

Der Name der benutzerdefinierten Domäne Ihrer Webanwendung wird von Benutzern verwendet, um auf Ihre Anwendung zu verweisen. Beispielsweise www.contoso.com. Von diesem Namen der benutzerdefinierten Domäne wurde ursprünglich auf den Ausführungsort vor der Einführung von Azure Front Door verwiesen. Nachdem Sie Azure Front Door und WAF vor der Anwendung hinzugefügt haben, muss der DNS-Eintrag, der dieser benutzerdefinierten Domäne entspricht, auf die Azure Front Door-Ressource verweisen. Um diese Änderung vorzunehmen, können Sie den Eintrag in Ihrem DNS-Server dem Hostnamen (`hostName`) von Azure Front Door zuordnen, den Sie sich beim Erstellen der Azure Front Door-Ressource notiert haben.

Die genauen Schritte zum Aktualisieren Ihrer DNS-Einträge hängen von Ihrem DNS-Dienstanbieter ab. Wenn Sie Azure DNS zum Hosten Ihres DNS-Namens verwenden, erfahren Sie in der zugehörigen Dokumentation, wie Sie [einen DNS-Eintrag aktualisieren](https://docs.microsoft.com/azure/dns/dns-operations-recordsets-cli) und auf den Hostnamen (`hostName`) von Azure Front Door verweisen. 

Wichtiger Hinweis für den Fall, dass Ihre Kunden den Zonen-Apex (beispielsweise „contoso.com“) verwenden müssen, um zu Ihrer Website zu gelangen: In diesem Fall müssen Sie Azure DNS und den zugehörigen [Aliaseintragstyp](https://docs.microsoft.com/azure/dns/dns-alias) verwenden, um Ihren DNS-Namen zu hosten. 

Außerdem müssen Sie die Azure Front Door-Konfiguration aktualisieren und ihr [die benutzerdefinierte Domäne hinzufügen](https://docs.microsoft.com/azure/frontdoor/front-door-custom-domain), damit diese Zuordnung erkannt wird.

Des Weiteren gilt: Wenn Sie eine benutzerdefinierte Domäne für die Navigation zu Ihrer Webanwendung verwenden und das HTTPS-Protokoll aktivieren möchten, müssen Sie die [Zertifikate für Ihre benutzerdefinierte Domäne in Azure Front Door einrichten](https://docs.microsoft.com/azure/frontdoor/front-door-custom-domain-https). 

## <a name="lock-down-your-web-application"></a>Sperren der Webanwendung

Es empfiehlt sich, dafür zu sorgen, dass nur Azure Front Door-Edges mit Ihrer Webanwendung kommunizieren können. Dadurch wird sichergestellt, dass niemand den Azure Front Door-Schutz umgehen und direkt auf Ihre Anwendung zugreifen kann. Informationen zur Implementierung dieser Sperre finden Sie unter [Wie kann ich den Zugriff auf mein Back-End nur auf Azure Front Door beschränken?](https://docs.microsoft.com/azure/frontdoor/front-door-faq#how-do-i-lock-down-the-access-to-my-backend-to-only-azure-front-door).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die in diesem Tutorial verwendeten Ressourcen nicht mehr benötigen, entfernen Sie die Ressourcengruppe, Front Door und die WAF-Richtlinie mithilfe des Befehls [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete):

```azurecli-interactive
  az group delete \
    --name <>
```
`--name`: Der Name der Ressourcengruppe für alle in diesem Tutorial verwendeten Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Behandlung von Problemen mit Front Door finden Sie in den Leitfäden zur Problembehandlung:

> [!div class="nextstepaction"]
> [Behandeln von häufigen Routingproblemen](front-door-troubleshoot-routing.md)

> [!div class="nextstepaction"]
> [Zulässige Zertifizierungsstellen](https://docs.microsoft.com/azure/frontdoor/front-door-troubleshoot-allowed-ca)
