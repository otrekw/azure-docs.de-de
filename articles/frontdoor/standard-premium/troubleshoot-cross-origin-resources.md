---
title: Verwendung von Azure Front Door Standard/Premium mit Cross-Origin Resource Sharing
description: Erfahren Sie, wie Sie Azure Front Door Standard/Premium mit Cross-Origin Resource Sharing (CORS) verwenden.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: ee8f19aca62d2e331fcf59551d47c2dac93783b1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101097729"
---
# <a name="using-azure-front-door-standardpremium-with-cross-origin-resource-sharing-cors"></a>Verwendung von Azure Front Door Standard/Premium mit Cross-Origin Resource Sharing (CORS)

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

## <a name="what-is-cors"></a>Was ist CORS?

CORS (Cross Origin Resource Sharing; Ressourcenfreigabe zwischen verschiedenen Ursprüngen) ist eine HTTP-Funktion, die einer Webanwendung, die in einer Domäne ausgeführt wird, den Zugriff auf Ressourcen in einer anderen Domäne ermöglicht. Alle modernen Webbrowser implementieren eine Sicherheitseinschränkung, die als [Same Origin Policy](https://www.w3.org/Security/wiki/Same_Origin_Policy) bezeichnet wird, um die Möglichkeiten für Cross-Site Scripting-Angriffe zu verringern. Dies hindert eine Website daran, APIs in einer anderen Domäne aufzurufen.  CORS bietet eine sichere Methode, um einem Ursprung (der Ursprungsdomäne) das Aufrufen von APIs in einem anderen Ursprung zu ermöglichen.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-it-works"></a>Funktionsweise

Es gibt zwei Arten von CORS-Anforderungen: *einfache Anforderungen* und *komplexe Anforderungen*.

### <a name="for-simple-requests"></a>Für einfache Anforderungen gilt Folgendes:

1. Der Browser sendet die CORS-Anforderung mit einem zusätzlichen **Origin**-HTTP-Anforderungsheader. Der Wert dieses Headers ist der Ursprung, der die übergeordneten Seite bereitgestellt hat. Dabei handelt es sich um eine Kombination aus *Protokoll,* *Domäne* und *Port*.  Wenn eine Seite aus „https\://www.contoso.com“ versucht, auf die Benutzerdaten im Ursprung „fabrikam.com“ zuzugreifen, wird der folgende Anforderungsheader an „fabrikam.com“ gesendet:

   `Origin: https://www.contoso.com`

2. Der Server reagiert kann wie folgt reagieren:

   * Mit einem **Access-Control-Allow-Origin**-Header in der Antwort, der die zulässige Ursprungswebsites angibt. Beispiele:

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * Mit einem HTTP-Fehlercode (z. B. 403), falls der Server die ursprungsübergreifende Anforderung nach der Überprüfung des Origin-Headers nicht zulässt.

   * Mit einem **Access-Control-Allow-Origin**-Header mit einem Platzhalter, der alle Ursprünge zulässt:

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a>Für komplexe Anforderungen gilt Folgendes:

Eine komplexe-Anforderung ist eine CORS-Anforderung, bei der der Browser vor dem Senden der eigentlichen CORS-Anforderung eine *Preflight-Anforderung* (also einen Vorabtest) senden muss. Die Preflight-Anforderung ist eine `OPTIONS`-Anforderung an die gleiche URL und fordert vom Server eine Berechtigung zum Ausführen der ursprünglichen CORS-Anforderung an.

> [!TIP]
> Weitere Informationen zu CORS-Abläufen und häufigen Problemen finden Sie im [CORS-Leitfaden für REST-APIs](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).
>
>

## <a name="wildcard-or-single-origin-scenarios"></a>Platzhalter oder Szenarien mit nur einem Ursprung
CORS für Azure Front Door funktioniert automatisch ohne zusätzliche Konfiguration, wenn der **Access-Control-Allow-Origin** -Header auf Platzhalter (*) oder einen einzelnen Ursprung festgelegt ist.  Azure Front Door führt eine Zwischenspeicherung der ersten Antwort aus, und nachfolgende Anforderungen verwenden den gleichen Header.

Wenn Anforderungen schon an Azure Front Door gesendet wurden, bevor CORS auf den Ursprung festgelegt wurde, müssen Sie den Inhalt im Inhalt Ihres Endgeräts entfernen, um den Inhalt mit dem **Access-Control-Allow-Origin**-Header erneut zu laden.

## <a name="multiple-origin-scenarios"></a>Szenarien mit mehreren Ursprüngen
Wenn Sie eine bestimmte Liste von Ursprüngen für CORS zulassen müssen, wird es etwas komplizierter. Das Problem tritt auf, wenn CDN den **Access-Control-Allow-Origin** -Header für den ersten CORS-Ursprung zwischenspeichert.  Bei einer Folgeanforderung durch einen anderen CORS-Ursprung stellt CDN den zwischengespeicherten **Access-Control-Allow-Origin**-Header bereit, dieser stimmt jedoch nicht überein. Es gibt mehrere Möglichkeiten, dieses Problem zu korrigieren.

### <a name="azure-front-door-rule-set"></a>Azure Front Door-Regelsatz

Sei können in Azure Front Door im Azure Front Door-[Regelsatz](concept-rule-set.md) eine Regel erstellen, um den **Origin**-Header in der Anforderung zu überprüfen. Wenn es sich hierbei um einen gültigen Ursprung handelt, legt die Regel den **Access-Control-Allow-Origin**-Header auf den richtigen Wert fest. In diesem Fall wird der **Access-Control-Allow-Origin**-Header des Ursprungsservers der Datei ignoriert, und die Regel-Engine von AFD verwaltet vollständig die zulässigen CORS-Ursprünge.

:::image type="content" source="../media/troubleshooting-cross-origin-resource-sharing/cross-origin-resource.png" alt-text="Screenshot: Beispiel für Regeln mit Regelsatz":::

> [!TIP]
> Sie können Ihrer Regel zusätzliche Aktionen hinzufügen, um zusätzliche Antwortheader zu ändern, z. B. **Access-Control-Allow-Methods**.
> 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](create-front-door-portal.md).
