---
title: Was ist die Personalisierung?
description: Die Personalisierung ist ein cloudbasierter API-Dienst, mit dem Sie die beste Benutzeroberfläche für Ihre Benutzer auswählen und dabei in Echtzeit von deren Verhalten lernen können.
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 3ae425479d764c0a6bf6c63bdd54a964c48af8b6
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687264"
---
# <a name="what-is-personalizer"></a>Was ist die Personalisierung?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Die Azure-Personalisierung ist ein cloudbasierter API-Dienst, mit dessen Hilfe Ihre Clientanwendung für jeden Benutzer jeweils den am besten geeigneten _Inhalt_ anzeigen kann. Der Dienst wählt basierend auf den gesamten Echtzeitinformationen, die Sie für den Inhalt und zum Kontext angeben, das beste Element aus den Inhaltselementen aus.

Nachdem Sie das Inhaltselement für den Benutzer bereitgestellt haben, überwacht Ihr System das Benutzerverhalten und meldet eine Relevanzbewertung an die Personalisierung, damit basierend auf den erhaltenen Kontextinformationen noch besser der bestmögliche Inhalt ausgewählt werden kann.

Der **Inhalt** kann eine beliebige Informationseinheit sein, z. B. Text, Bilder, URLs oder E-Mails, die zum Treffen einer Auswahl für Ihren Benutzer genutzt werden soll.

<!--
![What is personalizer animation](./media/what-is-personalizer.gif)
-->

## <a name="how-does-personalizer-select-the-best-content-item"></a>Wie wählt die Personalisierung das beste Inhaltselement aus?

Für die Personalisierung wird das **vertiefende Lernen** verwendet, um anhand des gesamten Verhaltens und der Relevanzbewertungen aller Benutzer das beste Element (_Aktion_) auswählen zu können. Aktionen sind die Inhaltselemente, z. B. Nachrichtenartikel, bestimmte Filme oder Produkte, die zur Wahl stehen.

Beim **Rangfolge**-Aufruf werden das Aktionselement, einschließlich der zugehörigen Features, und die Kontextfeatures verwendet, um das oberste Aktionselement auszuwählen:

* **Aktionen mit Features**: Inhaltselemente mit spezifischen Features für jedes Element.
* **Kontextfeatures**: Features Ihrer Benutzer, des Kontexts oder der Umgebung bei der Nutzung Ihrer App.

Im Feld für die **Relevanzaktion-ID** gibt der Rangfolge-Aufruf die ID dazu zurück, welches Inhaltselement (__Aktion__) dem Benutzer angezeigt werden soll.
Die dem Benutzer angezeigte __Aktion__ wird mithilfe von Machine Learning-Modellen ausgewählt, um zu versuchen, die Relevanz im Laufe der Zeit möglichst stark zu erhöhen.

Beispielszenarien:

|Inhaltstyp|**Aktionen (mit Features)**|**Kontextfeatures**|Zurückgegebene Relevanzaktion-ID<br>(anzuzeigender Inhalt)|
|--|--|--|--|
|Liste mit News|a. `The president...` (Inland, Politik, [Text])<br>b. `Premier League ...` (Weltweit, Sport, [Text, Bild, Video])<br> c. `Hurricane in the ...` (Regional, Wetter, [Text, Bild]|Gerät, von dem News gelesen werden<br>Monat oder Saison<br>|a `The president...`|
|Liste mit Filmen|1. `Star Wars` (1977, [Aktion, Abenteuer, Fantasy], George Lucas)<br>2. `Hoop Dreams` (1994, [Dokumentation, Sport], Steve James<br>3. `Casablanca` (1942, [Romanze, Drama, Krieg], Michael Curtiz)|Gerät, auf dem der Film angeschaut wird<br>Displaygröße<br>Typ des Benutzers<br>|3. `Casablanca`|
|Produktliste|i. `Product A` (3 kg, $$$$, Lieferung innerhalb von 24 Stunden)<br>ii. `Product B` (20 kg, $$, 2 Wochen Lieferzeit mit Zoll)<br>iii. `Product C` (3 kg, $$$, Lieferung innerhalb von 48 Stunden)|Gerät, über das der Einkauf erfolgt<br>Ausgabenebene des Benutzers<br>Monat oder Saison|ii. `Product B`|

Für die Personalisierung wird das vertiefende Lernen genutzt, um die beste Aktion auszuwählen. Es wird eine _Relevanzaktion-ID_ verwendet, die auf einer Kombination der folgenden Elemente basiert:
* Trainiertes Modell: Vom Personalisierungsdienst empfangene Informationen
* Aktuelle Daten: Spezifische Aktionen mit Features und Kontextfeatures

## <a name="when-to-call-personalizer"></a>Zeitpunkt zum Aufrufen der Personalisierung

Die **Rangfolge**-[API](https://go.microsoft.com/fwlink/?linkid=2092082) der Personalisierung wird _immer_ in Echtzeit aufgerufen, wenn Sie Inhalt anzeigen. Dies wird als **Ereignis** bezeichnet und durch eine _Ereignis-ID_ gekennzeichnet.

Die **Relevanz**-[API](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Reward) der Personalisierung kann in Echtzeit oder verzögert aufgerufen werden, um die jeweils am besten geeignete Anpassung an Ihre Infrastruktur zu erzielen. Sie bestimmen die Relevanzbewertung basierend auf Ihren Geschäftsanforderungen. Die Relevanzbewertung liegt zwischen 0 und 1. Dies kann ein einzelner Wert sein, z. B. „1“ für „Gut“ und „0“ für „Schlecht“. Sie können auch eine Zahl verwenden, die von einem Algorithmus ermittelt wird, den Sie anhand Ihrer Geschäftsziele und Metriken erstellt haben.

## <a name="personalizer-content-requirements"></a>Inhaltsanforderungen für die Personalisierung

Verwenden Sie die Personalisierung, wenn für Ihren Inhalt Folgendes gilt:

* Verfügt über eine begrenzte Anzahl von Elementen (max. 50), die ausgewählt werden können. Bei einer längeren Liste sollten Sie eine [Empfehlungs-Engine verwenden](where-can-you-use-personalizer.md#how-to-use-personalizer-with-a-recommendation-solution), um die Liste auf 50 Elemente zu reduzieren.
* Enthält Informationen zur Beschreibung des Inhalts, für den die Rangfolge nach Relevanz erstellt werden soll: _Aktionen mit Features_ und _Kontextfeatures_.
* Verfügt über mindestens 1.000 inhaltsbezogene Ereignisse pro Tag, damit die Personalisierung effektiv ist. Wenn die Personalisierung nicht den mindestens benötigten Datenverkehr erhält, dauert es länger, bis der Dienst das beste Inhaltselement ermittelt hat.

Da für die Personalisierung die gesamten Informationen nahezu in Echtzeit genutzt werden, um das beste Inhaltselement zurückgeben zu können, wird vom Dienst Folgendes nicht durchgeführt:
* Speichern und Verwalten von Informationen zum Benutzerprofil
* Protokollieren von Einstellungen oder des Verlaufs einzelner Benutzer
* Erzwingen von bereinigtem Inhalt mit Bezeichnungen

## <a name="how-to-design-and-implement-personalizer-for-your-client-application"></a>Entwerfen und Implementieren der Personalisierung für Ihre Clientanwendung

1. Führen Sie das [Entwerfen](concepts-features.md) und Planen für Inhalt, **_Aktionen_** und **_Kontext_** durch. Ermitteln Sie den Relevanzalgorithmus für die Bewertung der **_Relevanz_** .
1. Jede von Ihnen erstellte [Personalisierungsressource](how-to-settings.md) wird als gesonderte Lernschleife angesehen. Die Schleife empfängt sowohl jeweils den Rangfolge- als auch den Relevanz-Aufruf für diesen Inhalt bzw. die Benutzererfahrung.
1. Fügen Sie die Personalisierung Ihrer Website oder dem Inhaltssystem hinzu:
    1. Fügen Sie der Personalisierung in Ihrer Anwendung, der Website oder dem System einen **Rangfolge**-Aufruf hinzu, um das am besten geeignete _Inhaltselement_ zu ermitteln, bevor der Inhalt dem Benutzer angezeigt wird.
    1. Zeigen Sie dem Benutzer das am besten geeignete _Inhaltselement_ an, das anhand der zurückgegebenen _Relevanzaktion-ID_ gekennzeichnet ist.
    1. Wenden Sie den _Algorithmus_ auf die gesammelten Informationen an, die das Verhalten des Benutzers angeben, um die **Relevanzbewertung** zu ermitteln. Beispiel:

        |Verhalten|Berechnete Relevanzbewertung|
        |--|--|
        |Benutzer hat das am besten geeignete _Inhaltselement_ (Relevanzaktion-ID) ausgewählt|**1**|
        |Benutzer hat anderen Inhalt ausgewählt|**0**|
        |Benutzer hat innegehalten und zunächst gescrollt, bevor er das am besten geeignete _Inhaltselement_ (Relevanzaktion-ID) ausgewählt hat|**0,5**|

    1. Fügen Sie einen **Relevanz**-Aufruf hinzu, bei dem eine Relevanzbewertung zwischen 0 und 1 gesendet wird.
        * Unmittelbar nach dem Anzeigen Ihrer Inhalte
        * Oder später in einem Offlinesystem
    1. [Werten Sie Ihre Schleife aus](concepts-offline-evaluation.md), indem Sie nach einer gewissen Nutzungsdauer eine Offlineauswertung durchführen. Bei der Offlineauswertung können Sie die Wirksamkeit des Personalisierungsdiensts testen und bewerten, ohne Ihren Code zu ändern oder die Benutzererfahrung zu beeinträchtigen.

## <a name="next-steps"></a>Nächste Schritte


* [Funktionsweise der Personalisierung](how-personalizer-works.md)
* [Was ist vertiefendes Lernen?](concepts-reinforcement-learning.md)
* [Erfahren Sie mehr über Features und Aktionen für die Rangfolgeanforderung.](concepts-features.md)
* [Erfahren Sie mehr über das Festlegen der Bewertung für die Relevanzanforderung.](concept-rewards.md)
* [Schnellstarts](sdk-learning-loop.md)
* [Tutorial](tutorial-use-azure-notebook-generate-loop-data.md)
* [Verwenden der interaktiven Demo](https://personalizationdemo.azurewebsites.net/)
