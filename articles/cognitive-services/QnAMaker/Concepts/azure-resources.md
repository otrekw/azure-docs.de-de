---
title: 'Azure-Ressourcen: QnA Maker'
description: In QnA Maker werden verschiedene Azure-Quellen verwendet, von denen jede einen anderen Zweck erfüllt. Wenn Sie wissen, wie sie einzeln verwendet werden, können Sie den richtigen Tarif planen und auswählen oder bestimmen, wann ein Tarifwechsel sinnvoll ist. Das Verständnis dafür, wie sie kombiniert verwendet werden, ermöglicht es Ihnen, auftretende Probleme zu finden und zu beheben.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: 77aced459c7731a2cac432538cfc66ed9ce83f9d
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76902068"
---
# <a name="azure-resources-for-qna-maker"></a>Azure-Ressourcen für QnA Maker

In QnA Maker werden verschiedene Azure-Quellen verwendet, von denen jede einen anderen Zweck erfüllt. Wenn Sie wissen, wie sie einzeln verwendet werden, können Sie den richtigen Tarif planen und auswählen oder bestimmen, wann ein Tarifwechsel sinnvoll ist. Das Verständnis dafür, wie sie _kombiniert_ verwendet werden, ermöglicht es Ihnen, auftretende Probleme zu finden und zu beheben.

## <a name="resource-planning"></a>Ressourcenplanung

Wenn Sie zum ersten Mal eine QnA Maker Knowledge Base entwickeln, ist es in der Prototypenphase üblich, sowohl für Tests als auch für die Produktion eine einzelne QnA Maker-Ressource zu verwenden.

Beim Einstieg in die Entwicklungsphase des Projekts sollten Sie die folgenden Punkte berücksichtigen:

* wie viele Sprachen Ihr Wissensdatenbank-System enthalten soll
* in wie vielen Regionen Ihre Wissensdatenbank verfügbar sein muss, bzw. aus wie vielen Regionen sie zur Verfügung gestellt wird
* wie viele Dokumente die einzelnen Domänen in Ihrem System enthalten werden

Sie sollten so planen, dass eine einzelne QnA Maker-Ressource alle Wissensdatenbanken enthält, die die gleiche Sprache, die gleiche Region und die gleiche Kombination aus Thema und Domäne aufweisen.

## <a name="pricing-tier-considerations"></a>Tarifstufeninformationen

Es gibt in der Regel drei Parameter, die Sie berücksichtigen müssen:

* **Den Durchsatz, den Sie vom Dienst benötigen:** Wählen Sie den passenden [App-Plan](https://azure.microsoft.com/pricing/details/app-service/plans/) für Ihren App Service auf der Grundlage Ihrer Anforderungen aus. Sie können die App [zentral hochskalieren](https://docs.microsoft.com/azure/app-service/manage-scale-up) oder zentral herunterskalieren. Dies sollte auch Einfluss auf Ihre Wahl der Azure Cognitive Search SKU haben; weitere Details dazu finden Sie [hier](https://docs.microsoft.com/azure/search/search-sku-tier).

* **Größe und Anzahl von Wissensdatenbanken:** Wählen Sie eine geeignete [Azure Search-SKU](https://azure.microsoft.com/pricing/details/search/) für Ihr Szenario aus. Normalerweise entscheiden Sie über die Anzahl der erforderlichen Wissensdatenbanken anhand der Anzahl der verschiedenen Themendomänen. Eine Themendomäne (für eine einzelne Sprache) sollte in einer Wissensdatenbank vorliegen.

    Sie können N-1 Wissensdatenbanken in einem bestimmten Tarif veröffentlichen, wobei N die Anzahl der im Tarif maximal zulässigen Indizes ist. Überprüfen Sie außerdem die maximale Größe und Anzahl der in den einzelnen Tarifen zulässigen Dokumente.

    Wenn Ihr Tarif beispielsweise 15 zulässige Indizes aufweist, können Sie 14 Wissensdatenbanken veröffentlichen (1 Index pro veröffentlichter Wissensdatenbank). Der fünfzehnte Index wird für alle Wissensdatenbanken zum Erstellen und Testen verwendet.

* **Anzahl von Dokumenten als Quellen:** Die kostenlose SKU des QnA Maker-Verwaltungsdiensts begrenzt die Anzahl der Dokumente, die über das Portal und die APIs verwaltet werden können, auf 3 (bei einer Größe von jeweils 1 MB). Die Standard-SKU ist in der Anzahl der Dokumente, die Sie verwalten können, nicht begrenzt. Ausführlichere Informationen finden Sie [hier](https://aka.ms/qnamaker-pricing).

Die folgende Tabelle gibt Ihnen einige allgemeine Richtlinien.

|                        | QnA Maker Management | App Service | Azure Cognitive Search | Einschränkungen                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Experimentieren        | Kostenlose SKU             | Free-Tarif   | Free-Tarif    | Veröffentlichen von bis zu 2 KB bei einer Größe von 50 MB  |
| Entwicklungs-/Testumgebung   | Standard-SKU         | Shared      | Basic        | Veröffentlichen von bis zu 14 KB bei einer Größe von 2 GB    |
| Produktionsumgebung | Standard-SKU         | Basic       | Standard     | Veröffentlichen von bis zu 49 KB bei einer Größe von 25 GB |

## <a name="when-to-change-a-pricing-tier"></a>Anlässe zum Ändern eines Tarifs

|Aktualisieren|`Reason`|
|--|--|
|[Upgrade](../How-to/set-up-qnamaker-service-azure.md#upgrade-qna-maker-sku) der QnA Maker Management-SKU|Sie möchten mehr QnA-Sätze oder Dokumentquellen in Ihre Wissensdatenbank aufnehmen.|
|[Upgrade](../How-to/set-up-qnamaker-service-azure.md#upgrade-app-service) der App Service-SKU|Ihre Wissensdatenbank muss mehr Anforderungen aus Ihrer Client-App verarbeiten, z.B. einen Chatbot.|
|[Upgrade](../How-to/set-up-qnamaker-service-azure.md#upgrade-the-azure-cognitive-search-service) des Azure Cognitive Search-Diensts|Sie benötigen voraussichtlich eine Vielzahl von Wissensdatenbanken.|

Rufen Sie die aktuellsten Runtime-Updates ab, indem Sie [Ihren App Service im Azure-Portal aktualisieren](../how-to/set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates).

## <a name="resource-naming-considerations"></a>Überlegungen zu Ressourcennamen

Der Ressourcenname für die QnA Maker-Ressource, wie etwa `qna-westus-f0-b`, wird auch zum Benennen der weiteren Ressourcen verwendet.

Im Erstellen-Fenster des Azure-Portals können Sie eine QnA Maker-Ressource erstellen und die Tarife für die weiteren Ressourcen auswählen.

> [!div class="mx-imgBorder"]
> ![Screenshot des Azure-Portals für die Erstellung von QnA Maker-Ressourcen](../media/concept-azure-resource/create-blade.png)

Nach dem Erstellen haben die Ressourcen den gleichen Namen, mit Ausnahme der optionalen Application Insights-Ressource, die dem Namen Zeichen nachstellt.

> [!div class="mx-imgBorder"]
> ![Screenshot der Ressourcenauflistung im Azure-Portal](../media/concept-azure-resource/all-azure-resources-created-qnamaker.png)

> [!TIP]
> Erstellen Sie eine neue Ressourcengruppe, wenn Sie eine QnA Maker Ressource erstellen. Auf diese Weise können Sie beim Suchen nach der Ressourcengruppe alle Ressourcen anzeigen, die der QnA Maker Ressource zugeordnet sind.

> [!TIP]
> Verwenden Sie eine Benennungskonvention, die Tarifstufen im Namen der Ressource oder Ressourcengruppe anzeigt. Wenn beim Erstellen einer neuen Wissensdatenbank oder beim Hinzufügen neuer Dokumente Fehler angezeigt werden, ist der Grenzwert des Cognitive Search-Tarifs ein häufiges Problem.

## <a name="resource-purposes"></a>Zwecke von Ressourcen

Jede Azure-Ressource, die mit QnA Maker erstellt wird, hat einen bestimmten Zweck:

* QnA Maker-Ressource
* Cognitive Search-Ressource
* App Service
* App-Plandienst
* Application Insights-Dienst


### <a name="cognitive-search-resource"></a>Cognitive Search-Ressource

Die [Cognitive Search](../../../search/index.yml)-Ressource wird für diese Zwecke verwendet:

* Speichern der QnA-Sätze
* Angeben der anfänglichen Rangfolge (Bewerter Nr. 1) der QnA-Sätze zur Laufzeit

#### <a name="index-usage"></a>Indexnutzung

Die Ressource pflegt einen Index, der als Testindex fungiert, und die verbleibenden Indizes korrelieren jeweils zu einer veröffentlichten Wissensdatenbank.

Eine Ressource, die nach ihrem Preis 15 Indizes unterhalten kann, enthält 14 veröffentlichte Wissensdatenbanken, während ein Index zum Testen aller Wissensdatenbanken verwendet wird. Dieser Testindex ist nach Wissensdatenbanken partitioniert, sodass eine Abfrage, die den interaktiven Testbereich verwendet, den Testindex verwendet, jedoch nur Ergebnisse aus der spezifischen Partition zurückgibt, die der spezifischen Wissensdatenbank zugeordnet ist.

#### <a name="language-usage"></a>Sprachverwendung

Die erste Wissensdatenbank, die in der QnA Maker-Ressource erstellt wird, wird verwendet, um den _einzelnen_ Sprachsatz zu bestimmen, der für die Cognitive Search-Ressource und alle zugehörigen Indizes festgelegt ist. Es kann nur _ein Sprachsatz_ für einen QnA Maker-Dienst festgelegt werden.

### <a name="qna-maker-resource"></a>QnA Maker-Ressource

Die QnA Maker-Ressource bietet zur Laufzeit Zugriff auf die Erstellungs- und Veröffentlichungs-APIs sowie auf die zweite Bewertungsschicht (Bewerter Nr. 2) der QnA-Sätze, die auf natürlichsprachlicher Verarbeitung (Natural Language Processing, NLP) beruht.

Die zweite Bewertung wendet intelligente Filter an, die Metadaten und Folgeaufforderungen beinhalten können.

#### <a name="qna-maker-resource-configuration-settings"></a>Ressourcenkonfigurationseinstellungen für QnA Maker

Wenn Sie eine neue Wissensdatenbank im [QnA Maker-Portal](https://qnamaker.ai) erstellen, ist die Einstellung für **Sprache** die einzige Einstellung, die auf der Ressourcenebene angewendet wird. Sie wählen die Sprache aus, wenn Sie die erste Wissensdatenbank für die Ressource erstellen. 

### <a name="app-service-and-app-service-plan"></a>App Service und App Service-Plan

Der [App Service](../../../app-service/index.yml) wird von Ihrer Clientanwendung verwendet, um über den Laufzeitendpunkt auf die veröffentlichten Wissensdatenbanken zuzugreifen.

Um die veröffentlichte Wissensdatenbank abzufragen, verwenden alle veröffentlichten Wissensdatenbanken denselben URL-Endpunkt, geben aber die **Wissensdatenbank-ID** in der Route an.

`{RuntimeEndpoint}/qnamaker/knowledgebases/{kbId}/generateAnswer`

### <a name="application-insights"></a>Application Insights

[Application Insights](../../../azure-monitor/app/app-insights-overview.md) wird verwendet, um Chatprotokolle und Telemetriedaten zu erfassen. Verwenden Sie die allgemeinen [Kusto-Abfragen](../how-to/get-analytics-knowledge-base.md), um Informationen zu Ihrem Dienst zu erhalten.

## <a name="share-services-with-qna-maker"></a>Freigeben von Diensten mit QnA Maker

QnA Maker erstellt verschiedene Azure-Ressourcen. Wenn Sie den Verwaltungsaufwand reduzieren und von der Kostenteilung profitieren möchten, informieren Sie sich anhand der folgenden Tabelle, was Sie freigeben können und was nicht:

|Dienst|Freigabe|`Reason`|
|--|--|--|
|Cognitive Services|X|Programmbedingt nicht möglich.|
|App Service-Plan|✔|Der für einen App Service-Plan zugewiesene Speicherplatz auf dem Datenträger. Wenn andere Apps, die denselben App Service-Plan gemeinsam nutzen, eine erhebliche Menge an Speicherplatz belegen, treten bei der QnAMaker App Service-Instanz Probleme auf.|
|App Service|X|Programmbedingt nicht möglich.|
|Application Insights|✔|Gemeinsame Nutzung möglich.|
|Suchdienst|✔|1. `testkb` ist ein reservierter Name für den QnAMaker-Dienst, der von anderen Benutzern nicht verwendet werden kann.<br>2. Eine Synonymzuordnung nach dem Namen `synonym-map` ist für den QnAMaker-Dienst reserviert.<br>3. Die Anzahl von veröffentlichten Wissensdatenbanken wird durch den Suchdiensttarif begrenzt. Wenn freie Indizes verfügbar sind, können sie von anderen Diensten genutzt werden.|

### <a name="using-a-single-cognitive-search-service"></a>Verwenden eines einzelnen Cognitive Search-Diensts

Wenn Sie einen QnA-Dienst und dessen Abhängigkeiten (z.B. Search) über das Portal erstellen, wird automatisch ein Suchdienst erstellt und mit dem QnA Maker-Dienst verknüpft. Nachdem diese Ressourcen erstellt wurden, können Sie die App Service-Einstellung aktualisieren, um einen bereits vorhandenen Suchdienst zu nutzen und den soeben erstellten Suchdienst zu entfernen.

Erfahren Sie, wie Sie QnA Maker [so konfigurieren](../How-To/set-up-qnamaker-service-azure.md#configure-qna-maker-to-use-different-cognitive-search-resource), dass er eine andere Cognitive Service-Ressource als die verwendet, die im Rahmen des Ressourcenerstellungsvorgangs für QnA Maker erstellt wurde.

## <a name="management-service-region"></a>Verwaltungsdienstregion

Der Verwaltungsdienst von QnA Maker wird nur für das QnA Maker-Portal und für die anfängliche Datenverarbeitung genutzt. Dieser Dienst ist nur in der Region **USA, Westen** verfügbar. In diesem Dienst (USA, Westen) werden keine Kundendaten gespeichert.

## <a name="keys-in-qna-maker"></a>Schlüssel in QnA Maker

Ihr QnA Maker-Dienst befasst sich mit zwei Arten von Schlüsseln: **Erstellungsschlüsseln** und **Anfrageendpunktschlüsseln**, die zusammen mit der im App Service gehosteten Runtime verwendet werden.

Wenn Sie nach Ihrem **Abonnementschlüssel** suchen, nehmen Sie zur Kenntnis, dass sich [die Terminologie geändert hat](#subscription-keys).

Verwenden Sie diese Schlüssel, wenn Sie Anforderungen an den Dienst über APIs senden.

![Schlüsselverwaltung](../media/qnamaker-how-to-key-management/key-management.png)

|Name|Location|Zweck|
|--|--|--|
|Erstellungsschlüssel|[Azure portal](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)|Diese Schlüssel werden verwendet, um auf die [QnA Maker-Verwaltungsdienst-APIs](https://go.microsoft.com/fwlink/?linkid=2092179) zuzugreifen. Mit diesen APIs können Sie die Fragen und Antworten in Ihrer Wissensdatenbank bearbeiten und Ihre Wissensdatenbank veröffentlichen. Diese Schlüssel werden erstellt, wenn Sie einen neuen QnA Maker-Dienst erstellen.<br><br>Sie finden diese Schlüssel in der Ressource **Cognitive Services** auf der Seite **Schlüssel**.|
|Abfrageendpunktschlüssel|[QnA Maker-Portal](https://www.qnamaker.ai)|Diese Schlüssel werden zum Abfragen des Endpunkts der veröffentlichten Wissensdatenbank verwendet, um eine Antwort auf eine Benutzerfrage abzurufen. In der Regel verwenden Sie diesen Abfrageendpunkt in Ihrem Chatbot oder im Clientanwendungscode, der eine Verbindung mit dem QnA Maker-Dienst herstellt. Diese Schlüssel werden erstellt, wenn Sie Ihre QnA Maker-Wissensdatenbank veröffentlichen.<br><br>Sie finden diese Schlüssel auf der Seite **Diensteinstellungen**. Suchen Sie diese Seite im Menü des Benutzers in der oberen rechten Ecke der Seite im Dropdownmenü.|

### <a name="subscription-keys"></a>Abonnementschlüssel

Die Begriffe „Erstellungsschlüssel“ und „Abfrageendpunktschlüssel“ dienen der Berichtigung. Der vorherig verwendete Begriff lautete **Abonnementschlüssel**. Wenn in anderen Dokumenten der Dokumentation auf Abonnementschlüssel verwiesen wird, sind diese gleichbedeutend mit Erstellungs- und Abfrageendpunktschlüsseln (die in der Runtime verwendet werden).

Sie müssen wissen, welchen Zweck der Zugriff mit dem Schlüssel verfolgt, die Verwaltung der Wissensdatenbank oder die Abfrage der Wissensdatenbank, damit Sie wissen, welcher Schlüssel gesucht werden muss.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die QnA Maker-[Wissensdatenbank](knowledge-base.md)
* Grundlegendes zum [Lebenszyklus von Wissensdatenbanken](development-lifecycle-knowledge-base.md)
* [Grenzwerte](../limits.md) von Überprüfungsdienst und Wissensdatenbanken

