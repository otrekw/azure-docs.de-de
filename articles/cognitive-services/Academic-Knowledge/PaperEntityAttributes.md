---
title: Attribute der Artikelentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Artikelentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: DarrinEide
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: fc3bb5987c31fe718951a3cae02ed7c4ddc29957
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74123079"
---
# <a name="paper-entity"></a>Artikelentität

> [!NOTE]
> Die folgenden Attribute sind spezifisch für die Artikelentität. (Ty = '0')

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
AA.AfId | ID der Zugehörigkeit des Autors | Int64 | Equals
AA.AfN | Name der Zugehörigkeit des Autors | Zeichenfolge | Equals, StartsWith
AA.AuId | ID des Autors | Int64 | Equals
AA.AuN | Normalisierter Autorname | Zeichenfolge | Equals, StartsWith
AA.DAuN | Ursprünglicher Autorname | Zeichenfolge | Keine
AA.DAfN | Ursprünglicher Name der Zugehörigkeit | Zeichenfolge | Keine
AA.S | Numerische Position in der Autorenliste | Int32 | Equals
BT | BibTeX-Dokumenttyp („a“: Journalartikel, „b“: Buch, „c“: Buchkapitel, „p“: Konferenzbeitrag) | Zeichenfolge | Keine
BV | Name des BibTeX-Veranstaltungsorts | Zeichenfolge | Keine
C.CId | ID der Konferenzreihe | Int64 | Equals
C.CN | Name der Konferenzreihe | Zeichenfolge | Equals, StartsWith
CC | Anzahl der Zitate | Int32 | Keine  
CitCon | Zitatkontexte</br></br>Liste der IDs der referenzierten Artikel und des entsprechenden Kontexts in dem Artikel (z. B. [{123: [„braune Füchse sind für ihre Sprünge bekannt, wie in Dokument 123 referenziert“, „die faulen Hunde sind historisch irreführend, wie in Artikel 123 gezeigt“]}) | Benutzerdefiniert | Keine
D | Veröffentlichungsdatum im Format JJJJ-MM-TT | Date | Equals, IsBetween
DN | Ursprünglicher Titel des Artikels | Zeichenfolge | Keine
DOI | Digitaler Objektbezeichner | Zeichenfolge | Equals, StartsWith
E | Erweiterte Metadaten</br></br>**WICHTIG:** Dieses Attribut ist veraltet und wird nur für ältere Anwendungen unterstützt. Die individuelle Abfrage dieses Attributs (z. B. attributes=Id,Ti,E) führt dazu, dass alle erweiterten Metadatenattribute in einer *serialisierten JSON-Zeichenfolge* zurückgegeben werden.</br></br>Alle in den erweiterten Metadaten enthaltenen Attribute stehen jetzt als Attribut der obersten Ebene zur Verfügung und können als solche angefordert werden (z. B. attributes=Id,Ti,DOI,IA). | [Erweitert](#extended) | Keine
ECC | Geschätzte Zitatanzahl | Int32 | Keine
F.DFN | Ursprünglicher Name für den Forschungsbereich | Zeichenfolge | Keine
F.FId | ID des Forschungsbereichs | Int64 | Equals
F.FN | Normalisierter Name für den Forschungsbereich | Zeichenfolge | Equals, StartsWith
FP | Erste Seite des Artikels in der Veröffentlichung | Zeichenfolge | Equals
I | Veröffentlichungsausgabe | Zeichenfolge | Equals
IA | Umgekehrte Zusammenfassung | [InvertedAbstract](#invertedabstract) | Keine
Id | Artikel-ID | Int64 | Equals
J.JId | ID der Publikation | Int64 | Equals
J.JN | Name der Publikation | Zeichenfolge | Equals, StartsWith
LP | Letzte Seite des Artikels in der Veröffentlichung | Zeichenfolge | Equals
PB | Herausgeber | Zeichenfolge | Keine
Pt | Veröffentlichungstyp (0: unbekannt, 1: Journalartikel, 2: Patent, 3: Konferenzbeitrag, 4: Buchkapitel, 5: Buch, 6: Verweiseintrag in Buch, 7: Dataset, 8: Repository) | Zeichenfolge | Equals
RId | Liste der referenzierten Artikel-IDs | Int64[] | Equals
S | Liste der Quell-URLs des Papiers, sortiert nach Relevanz | [Quelle](#source)[] | Keine
Ti | Normalisierter Titel | Zeichenfolge | Equals, StartsWith
V | Veröffentlichung | Zeichenfolge | Equals
VFN | Vollständiger Name des Journals oder Veranstaltungsorts der Konferenz | Zeichenfolge | Keine
VSN | Kurzname des Journals oder Veranstaltungsorts der Konferenz | Zeichenfolge | Keine
W | Eindeutige Wörter im Titel | String[] | Equals
J | Veröffentlichungsjahr | Int32 | Equals, IsBetween

## <a name="extended"></a>Erweitert
> [!IMPORTANT]
> Dieses Attribut ist veraltet und wird nur für ältere Anwendungen unterstützt. Die individuelle Abfrage dieses Attributs (z. B. attributes=Id,Ti,E) führt dazu, dass alle erweiterten Metadatenattribute in einer *serialisierten JSON-Zeichenfolge* zurückgegeben werden.</br></br>Alle in den erweiterten Metadaten enthaltenen Attribute stehen jetzt als Attribut der obersten Ebene zur Verfügung und können als solche angefordert werden (z. B. attributes=Id,Ti,DOI,IA).

> [!IMPORTANT]
> Die Unterstützung für die Anforderung einzelner erweiterter Attribute mithilfe des Bereichs „E.“ d. h. „E.DN“, ist veraltet. Obwohl dies technisch noch unterstützt wird, führt die Anforderung einzelner erweiterter Attribute mit dem Bereich „E.“ dazu, dass der Attributwert an zwei Stellen in der JSON-Antwort, als Teil des Objekts „E“ und als Attribut der obersten Ebene zurückgegeben wird.

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
BT | BibTeX-Dokumenttyp („a“: Journalartikel, „b“: Buch, „c“: Buchkapitel, „p“: Konferenzbeitrag) | Zeichenfolge | Keine
BV | Name des BibTeX-Veranstaltungsorts | Zeichenfolge | Keine
CC | Zitatkontexte</br></br>Liste der IDs der referenzierten Artikel und des entsprechenden Kontexts in dem Artikel (z. B. [{123: [„braune Füchse sind für ihre Sprünge bekannt, wie in Dokument 123 referenziert“, „die faulen Hunde sind historisch irreführend, wie in Artikel 123 gezeigt“]}) | Benutzerdefiniert | Keine
DN | Ursprünglicher Titel des Artikels | Zeichenfolge | Keine
DOI | Digitaler Objektbezeichner | Zeichenfolge | Keine
FP | Erste Seite des Artikels in der Veröffentlichung | Zeichenfolge | Keine
I | Veröffentlichungsausgabe | Zeichenfolge | Keine
IA | Umgekehrte Zusammenfassung | [InvertedAbstract](#invertedabstract) | Keine
LP | Letzte Seite des Artikels in der Veröffentlichung | Zeichenfolge | Keine
PB | Herausgeber | Zeichenfolge | Keine
S | Liste der Quell-URLs des Papiers, sortiert nach Relevanz | [Quelle](#source)[] | Keine
V | Veröffentlichung | Zeichenfolge | Keine
VFN | Vollständiger Name des Journals oder Veranstaltungsorts der Konferenz | Zeichenfolge | Keine
VSN | Kurzname des Journals oder Veranstaltungsorts der Konferenz | Zeichenfolge | Keine

## <a name="invertedabstract"></a>InvertedAbstract

> [!IMPORTANT]
> InvertedAbstract-Attribute können nicht direkt als Rückgabeattribut angefordert werden. Wenn Sie ein individuelles Attribut benötigen, müssen Sie das „IA“-Attribut der obersten Ebene anfordern, d. h. um „IA.IndexLength“ zu erhalten, fordern Sie „attributes=IA“ an.

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
IndexLength | Anzahl der Elemente im Index (Anzahl der Wörter in der Zusammenfassung) | Int32 | Keine
InvertedIndex | Liste der Wörter in der Zusammenfassung und ihre entsprechende Position in der ursprünglichen Zusammenfassung (z. B. [{„der“:[0, 15, 30]}, {„braune“:[1]}, {„Fuchs“:[2]}]) | Benutzerdefiniert | Keine

## <a name="source"></a>`Source`

> [!IMPORTANT]
> Source-Attribute können nicht direkt als Rückgabeattribut angefordert werden. Wenn Sie ein individuelles Attribut benötigen, müssen Sie das „S“-Attribut der obersten Ebene anfordern, d. h. um „S.U“ zu erhalten, fordern Sie „attributes=S“ an.

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
Ty | Typ der Quell-URL (1:HTML, 2:Text, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS) | Zeichenfolge | Keine
U | Quell-URL | Zeichenfolge | Keine
