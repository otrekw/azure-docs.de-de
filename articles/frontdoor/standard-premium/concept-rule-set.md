---
title: 'Azure Front Door: Regelsatz'
description: Dieser Artikel enthält eine Übersicht über das Feature „Regelsatz“ in Azure Front Door Standard/Premium.
services: front-door
author: duongau
ms.service: frontdoor
ms.topic: conceptual
ms.date: 03/31/2021
ms.author: yuajia
ms.openlocfilehash: e3e5333b339101676582cec03dbb960148d59b56
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106067553"
---
# <a name="what-is-a-rule-set-for-azure-front-door-standardpremium-preview"></a>Was ist ein Regelsatz für Azure Front Door Standard/Premium (Vorschau)?

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

Ein Regelsatz ist eine benutzerdefinierte Regel-Engine, die eine Kombination von Regeln in einem einzelnen Satz gruppiert. Sie können einem Regelsatz mehrere Routen zuordnen. Mithilfe des Regelsatzes können Sie anpassen, wie Anforderungen am Edge verarbeitet und von Azure Front Door behandelt werden.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="common-supported-scenarios"></a>Häufige unterstützte Szenarien

* Implementieren von Sicherheitsheadern wie HTTP Strict-Transport-Security (HSTS), X-XSS-Protection, Content-Security-Policy, X-Frame-Options und Access-Control-Allow-Origin für CORS-Szenarien (Cross-Origin Resource Sharing), um browserbasierte Sicherheitsrisiken zu verhindern. Sicherheitsbasierte Attribute können auch mit Cookies definiert werden.

* Weiterleiten von Anforderungen an die mobile Version oder Desktopversion Ihrer Anwendung auf Grundlage des Clientgerätetyps

* Verwenden von Umleitungsfunktionen, um die Umleitungen 301, 302, 307 und 308 an den Client zurückzugeben und so an neue Hostnamen, Pfade, Abfragezeichenfolgen oder Protokolle zu leiten

* Dynamisches Ändern der Zwischenspeicherkonfiguration Ihrer Route, basierend auf den eingehenden Anforderungen.

* Umschreiben des URL-Pfads der Anforderung und Weiterleiten der Anforderung an den entsprechenden Ursprung in der konfigurierten Ursprungsgruppe

* Hinzufügen, Ändern oder Entfernen des Anforderungs-/Antwortheaders, um vertrauliche Informationen zu verbergen oder über Header wichtige Informationen zu erfassen

* Unterstützen von Servervariablen zum dynamischen Ändern von Anforderungs-/Antwortheadern oder URL-Rewrite-Pfaden/Abfragezeichenfolgen, z. B. beim Laden einer neuen Seite oder beim Veröffentlichen eines Formulars. Servervariablen werden derzeit nur für **[Regelsatzaktionen](concept-rule-set-actions.md)** unterstützt.

## <a name="architecture"></a>Aufbau

Der Regelsatz behandelt Anforderungen am Edge. Wenn eine Anforderung beim Azure Front Door Standard/Premium-Endpunkt eintrifft, wird zuerst WAF ausgeführt, gefolgt von den für die Route konfigurierten Einstellungen. Diese Einstellungen umfassen den Regelsatz, der der Route zugeordnet ist. Regelsätze werden in der Route von oben nach unten verarbeitet. Das Gleiche gilt für Regeln innerhalb eines Regelsatzes. Damit alle Aktionen in jeder Regel ausgeführt werden, müssen alle Übereinstimmungsbedingungen innerhalb einer Regel erfüllt sein. Wenn eine Anforderung mit keiner der Bedingungen in der Regelsatzkonfiguration übereinstimmt, werden nur die Konfigurationen in der Route ausgeführt.

Wenn **Auswertung der verbleibenden Regeln beenden** aktiviert ist, wird keiner der verbleibenden Regelsätze, die der Route zugeordnet sind, ausgeführt.  

### <a name="example"></a>Beispiel

Im folgenden Diagramm werden zuerst WAF-Richtlinien ausgeführt. Ein Regelsatz wird so konfiguriert, dass ein Antwortheader angefügt wird. Der Header ändert dann das maximale Alter (Max-age) der Cachesteuerung, wenn die Übereinstimmungsbedingung erfüllt wird.

:::image type="content" source="../media/concept-rule-set/front-door-rule-set-architecture-1.png" alt-text="Diagramm der Architektur des Regelsatzes" lightbox="../media/concept-rule-set/front-door-rule-set-architecture-1-expanded.png":::

## <a name="terminology"></a>Begriff

Mit dem Azure Front Door-Regelsatz können Sie eine Kombination von Regelsatzkonfigurationen erstellen, die jeweils aus einem Satz von Regeln bestehen. Im Folgenden finden Sie einige hilfreiche Begriffe, auf die Sie bei der Konfiguration eines Regelsatzes stoßen werden.

Weitere Informationen zu Kontingentgrenzen finden Sie unter [Grenzwerte für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../../azure-resource-manager/management/azure-subscription-service-limits.md).

* *Regelsatz*: Ein Satz von Regeln, der einer oder mehreren [Routen](concept-route.md) zugeordnet wird.

* *Regelsatzregel*: Eine Regel, die bis zu 10 Vergleichsbedingungen und 5 Aktionen umfasst. Regeln sind für einen Regelsatz lokal und können nicht für die Verwendung in mehreren Regelsätzen exportiert werden. Benutzer können dieselbe Regel in mehreren Regelsätzen erstellen.

* *Vergleichsbedingung*: Es gibt eine Vielzahl von Vergleichsbedingungen, anhand derer Ihre eingehenden Anforderungen analysiert werden können. Eine Regel kann bis zu 10 Übereinstimmungsbedingungen enthalten. Übereinstimmungsbedingungen werden mit einem **UND**-Operator ausgewertet. *Reguläre Ausdrücke werden in Bedingungen unterstützt*. Eine vollständige Liste der Vergleichsbedingungen finden Sie unter [Regelsatz für Vergleichsbedingungen](concept-rule-set-match-conditions.md).

* *Aktion*: Aktionen legen fest, wie AFD die eingehenden Anforderungen auf Grundlage der Vergleichsbedingungen behandelt. Sie können das Verhalten beim Zwischenspeichern ändern, Anforderungsheader/Antwortheader ändern sowie URL-Rewrite und URL-Umleitung durchführen. *Servervariablen werden für Aktionen unterstützt*. Eine Regel kann bis zu 10 Übereinstimmungsbedingungen enthalten. Eine vollständige Liste der Aktionen finden Sie unter [Regelsatzaktionen](concept-rule-set-actions.md).

## <a name="arm-template-support"></a>ARM-Vorlagen-Support

Regelsätze können mit Azure Resource Manager-Vorlagen konfiguriert werden. [Eine Beispielvorlage finden Sie hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-rule-set). Sie können das Verhalten anpassen, indem Sie die JSON-oder Bicep-Ausschnitte in den Dokumentationsbeispielen für [Vergleichsbedingungen](concept-rule-set-match-conditions.md) und [Aktionen](concept-rule-set-actions.md)verwenden.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Erstellen einer Instanz von Front Door Standard/Premium](create-front-door-portal.md).
* Erfahren Sie, wie Sie Ihren ersten [Regelsatz](how-to-configure-rule-set.md) konfigurieren.
