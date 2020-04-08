---
title: Latent Dirichlet Allocation
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul Latent Dirichlet Allocation verwenden, um andernfalls nicht klassifizierten Text in eine Reihe von Kategorien zu gruppieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/11/2020
ms.openlocfilehash: 1384491489c175ffc338f80a99aa8d5050f835d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80089070"
---
# <a name="latent-dirichlet-allocation"></a>Latent Dirichlet Allocation

In diesem Artikel wird beschrieben, wie Sie das Modul **Latent Dirichlet Allocation** im Azure Machine Learning-Designer (Vorschauversion) verwenden, um andernfalls nicht klassifizierten Text in eine Reihe von Kategorien zu gruppieren. 

Latent Dirichlet Allocation (LDA) wird häufig bei der Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) verwendet, um ähnliche Texte zu finden. Ein anderer häufig verwendeter Begriff ist *Themenmodellierung*.

Dieses Modul nimmt eine Textspalte entgegen und generiert diese Ausgaben:

+ Den Quelltext mit einem Score für jede Kategorie

+ Eine Featurematrix mit extrahierten Begriffen und Koeffizienten für jede Kategorie

+ Eine Transformation, die Sie speichern und erneut auf neuen Eingabetext anwenden können

Dieses Modul verwendet die Bibliothek scikit-learn. Weitere Informationen zu scikit-learn finden Sie im GitHub-Repository, das Tutorials und eine Erläuterung des Algorithmus enthält.

### <a name="more-about-latent-dirichlet-allocation-lda"></a>Weitere Informationen zu Latent Dirichlet Allocation (LDA)

Grundsätzlich ist LDA keine spezifische Methode für die Klassifizierung, sondern verwendet einen generativen Ansatz. Dies bedeutet, dass Sie keine bekannten Klassenbezeichnungen bereitstellen und dann die Muster ableiten müssen.  Stattdessen generiert der Algorithmus ein Wahrscheinlichkeitsmodell, das verwendet wird, um Gruppen von Themen zu identifizieren. Sie können das Wahrscheinlichkeitsmodell zum Klassifizieren von vorhandenen Trainingsfällen oder von neuen Fällen verwenden, die Sie als Eingabe für das Modell bereitstellen.

Ein generatives Modell kann vorteilhaft sein, da es starke Annahmen über die Beziehung zwischen dem Text und den Kategorien vermeidet und nur die Verteilung von Wörtern für die mathematische Modellierung von Themen nutzt.

+ Die Theorie wird in diesem Dokument erläutert, das als PDF-Download zur Verfügung steht: [Latent Dirichlet Allocation: Blei, Ng, and Jordan](https://ai.stanford.edu/~ang/papers/nips01-lda.pdf)

+ Die Implementierung in diesem Modul basiert auf der Bibliothek [scikit-learn](https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/decomposition/_lda.py) für LDA.

Weitere Informationen finden Sie im Abschnitt mit den [technischen Hinweisen](#technical-notes).

## <a name="how-to-configure-latent-dirichlet-allocation"></a>Konfigurieren der Latent Dirichlet Allocation

Dieses Modul erfordert ein Dataset mit einer Textspalte, die unformatiert oder vorverarbeitet sein kann.

1. Fügen Sie Ihrer Pipeline das Modul **Latent Dirichlet Allocation** hinzu.

2. Stellen Sie als Eingabe für das Modul ein Dataset mit einer oder mehreren Textspalten bereit.

3. Wählen Sie als **Zielspalten** mindestens eine Spalte aus, die zu analysierenden Text enthält.

    Sie können mehrere Spalten auswählen, diese müssen jedoch den Datentyp String aufweisen.

    Da LDA eine große Featurematrix aus dem Text erstellt, analysieren Sie in der Regel eine einzelne Textspalte.

4. Geben Sie für **Number of topics to model** (Anzahl der zu modellierenden Themen) einen Integer zwischen 1 und 1.000 ein, der angibt, wie viele Kategorien oder Themen Sie aus dem Eingabetext ableiten möchten.

    Standardmäßig werden fünf Themen erstellt.

5. Geben Sie für **N-grams** die maximale Länge von N-Grammen an, die während der Hasherstellung generiert werden sollen.

    Der Standardwert beträgt 2, sodass sowohl Bigramme als auch Unigramme generiert werden.

6. Wählen Sie die Option **Normalize** (Normalisieren) aus, um Ausgabewerte in Wahrscheinlichkeiten zu konvertieren. Anstatt die transformierten Werte als Integer darzustellen, werden die Werte im Ausgabe- und Featuredataset somit wie folgt transformiert:

    + Werte im Dataset werden als Wahrscheinlichkeit dargestellt durch `P(topic|document)`.

    + Werte in der Feature-Themen-Matrix werden als Wahrscheinlichkeit dargestellt durch `P(word|topic)`.

    > [!NOTE] 
    > Da im Azure Machine Learning-Designer (Vorschauversion) die Bibliothek scikit-learn, auf der wir aufbauen, die nicht normalisierte Ausgabe *doc_topic_distr* von Version 0.19 nicht mehr unterstützt, kann der Parameter **Normalize** nur auf die Ausgabe der **Feature-Themen-Matrix** angewandt werden, die Ausgabe als **transformiertes Dataset** ist immer normalisiert.

7. Wählen Sie die Option **Show all options** (Alle Optionen anzeigen) aus, und legen Sie sie auf TRUE fest, wenn Sie zusätzliche erweiterte Parameter anzeigen und dann festlegen möchten.

    Diese Parameter sind spezifisch für die scikit-learn-Implementierung von LDA. Es gibt einige gute Tutorials zu LDA in scikit-learn sowie das offizielle [Dokument zu scikit-learn](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.LatentDirichletAllocation.html).

    + **Parameter „Rho“** . Geben Sie eine vorherige Wahrscheinlichkeit für eine geringe Datendichte von Themenverteilungen an. Entspricht dem Parameter `topic_word_prior` in scikit-learn. Verwenden Sie den Wert 1, wenn Sie davon ausgehen, dass die Verteilung der Wörter flach ist, d. h., dass alle Wörter als gleich wahrscheinlich angesehen werden. Wenn Sie davon ausgehen, dass die meisten Wörter nur eine geringe Datendichte aufweisen, können Sie einen wesentlich niedrigeren Wert festlegen.

    + **Parameter „Alpha“** . Geben Sie eine vorherige Wahrscheinlichkeit für eine geringe Datendichte von Themengewichtungen pro Dokument an.  Entspricht dem Parameter `doc_topic_prior` in scikit-learn.

    + **Geschätzte Anzahl von Dokumenten**. Geben Sie eine Zahl für Ihre beste Schätzung der Anzahl der zu verarbeitenden Dokumente (Zeilen) ein. Dadurch kann das Modul eine Hashtabelle mit ausreichender Größe zuordnen.  Entspricht dem Parameter `total_samples` in scikit-learn.

    + **Größe des Batches**. Geben Sie eine Zahl ein, die angibt, wie viele Zeilen in jeden Textbatch eingeschlossen werden sollen, der an das LDA-Modell gesendet wird. Entspricht dem Parameter `batch_size` in scikit-learn.

    + **Anfänglicher Wert der Iterationen im Aktualisierungszeitplan für das Lernen**. Geben Sie den Startwert an, mit dem die Lernrate für frühe Iterationen beim Onlinelernen herabgestuft wird. Entspricht dem Parameter `learning_offset` in scikit-learn.

    + **Während der Aktualisierung auf die Iteration aufgewandte Leistung**. Geben Sie den Leistungsgrad an, der auf Anzahl von Iterationen aufgewandt wird, um die Lernrate während Onlineupdates zu steuern. Entspricht dem Parameter `learning_decay` in scikit-learn.

    + **Anzahl der Durchgänge über die Daten**. Geben Sie an, wie oft der Algorithmus die Daten maximal durchlaufen soll. Entspricht dem Parameter `max_iter` in scikit-learn.

8. Wählen Sie eine der Optionen **Build dictionary of ngrams** (Wörterbuch aus N-Grammen erstellen) oder **Build dictionary of ngrams prior to LDA** (Vor LDA Wörterbuch aus N-Grammen erstellen) aus, wenn Sie die Liste der N-Gramme in einem anfänglichen Durchlauf erstellen möchten, bevor Text klassifiziert wird.

    Wenn Sie das erste Wörterbuch vorab erstellen, können Sie es später beim Überprüfen des Modells verwenden. Zuordnungen von Ergebnissen zu Text anstelle von numerischen Indizes sind in der Regel einfacher zu interpretieren. Das Speichern des Wörterbuchs dauert jedoch länger und verbraucht zusätzlichen Speicher.

9. Geben Sie für **Maximum size of ngram dictionary** (Maximale Größe des N-Gramm-Wörterbuchs) die Gesamtanzahl von Zeilen ein, die im N-Gramm-Wörterbuch erstellt werden können.

    Diese Option ist nützlich, um die Größe des Wörterbuchs zu steuern. Wenn jedoch die Anzahl der N-Gramme in der Eingabe diese Größe überschreitet, können Konflikte auftreten.

10. Übermitteln Sie die Pipeline. Das LDA-Modul nutzt das Bayes-Theorem, um zu ermitteln, welche Themen mit einzelnen Wörtern verknüpft sein können. Wörter sind Themen oder Gruppen nicht exklusiv zugeordnet. Stattdessen weist jedes N-Gramm eine erlernte Wahrscheinlichkeit auf, mit einer der ermittelten Klassen verknüpft zu sein.

## <a name="results"></a>Ergebnisse

Das Modul liefert zwei Ausgaben:

+ **Transformiertes Dataset:** Enthält den Eingabetext und eine angegebene Anzahl von ermittelten Kategorien sowie die Scores für jedes Textbeispiel und jede Kategorie.

+ **Feature-Themen-Matrix:** Die ganz linke Spalte enthält das extrahierte Textfeature, und für jede Kategorie ist eine Spalte mit dem Score für dieses Feature in dieser Kategorie enthalten.


### <a name="lda-transformation"></a>LDA-Transformation

Dieses Modul gibt auch die *LDA-Transformation* aus, die LDA auf das Dataset anwendet.

Sie können diese Transformation speichern, indem Sie ein Dataset auf der Registerkarte **Outputs+logs** (Ausgaben und Protokolle) im rechten Bereich des Moduls registrieren, und für andere Datasets wiederverwenden. Dies kann nützlich sein, wenn Sie mit einem großen Korpus trainiert haben und die Koeffizienten oder Kategorien wiederverwenden möchten.

### <a name="refining-an-lda-model-or-results"></a>Optimieren von LDA-Modellen oder -Ergebnissen

In der Regel ist es nicht möglich, ein einzelnes LDA-Modell zu erstellen, das alle Anforderungen erfüllt, und auch ein für eine Aufgabe entworfenes Modell erfordert eventuell viele Iterationen, um die Genauigkeit zu verbessern. Es wird empfohlen, alle diese Methoden zur Verbesserung des Modells auszuprobieren:

+ Ändern der Modellparameter
+ Analysieren der Ergebnisse mithilfe von Visualisierung
+ Rücksprache mit Fachleuten, um zu ermitteln, ob die generierten Themen nützlich sind

Qualitative Maßnahmen können für eine Auswertung der Ergebnisse ebenfalls hilfreich sein. Zum Auswerten der Ergebnisse der Themenmodellierung sollten Sie Folgendes berücksichtigen:

+ Genauigkeit: Sind ähnliche Elemente wirklich ähnlich?
+ Vielfalt: Kann das Modell zwischen ähnlichen Elementen unterscheiden, wenn dies für das Geschäftsproblem erforderlich ist?
+ Skalierbarkeit: Funktioniert es für eine breite Streuung von Textkategorien oder nur für eine eng gefasste Zieldomäne?

Die Genauigkeit von Modellen, die auf LDA basieren, kann häufig durch die Verarbeitung natürlicher Sprache verbessert werden, um Text zu bereinigen, zusammenzufassen und zu vereinfachen oder zu kategorisieren. Beispielsweise können die folgenden Techniken, die alle in Azure Machine Learning unterstützt werden, die Klassifizierungsgenauigkeit verbessern:

+ Stoppwortentfernung

+ Kasusnormalisierung

+ Lemmatisierung oder Wortstammerkennung

+ Erkennung benannter Entitäten

Weitere Informationen finden Sie unter [Vorverarbeiten von Text](preprocess-text.md).

Sie können im Designer für die Textverarbeitung auch R- oder Python-Bibliotheken verwenden: [R-Skript ausführen](execute-r-script.md), [Python-Skript ausführen](execute-python-script.md)



## <a name="technical-notes"></a>Technische Hinweise

Dieser Abschnitt enthält Implementierungsdetails, Tipps und Antworten auf häufig gestellte Fragen.

### <a name="implementation-details"></a>Details zur Implementierung

Standardmäßig werden die Verteilungen von Ausgaben für transformierte Datasets und Feature-Themen-Matrizen als Wahrscheinlichkeiten normalisiert.

+ Das transformierte Dataset wird als bedingte Wahrscheinlichkeit von Themen in einem Dokument normalisiert. In diesem Fall ist die Summe der einzelnen Zeilen gleich 1.

+ Die Feature-Themen-Matrix wird als bedingte Wahrscheinlichkeit von Wörtern in einem Thema normalisiert. In diesem Fall ist die Summe der einzelnen Spalten gleich 1.

> [!TIP]
> Gelegentlich gibt das Modul möglicherweise ein leeres Thema zurück. Dies wird meistens durch die pseudozufällige Initialisierung des Algorithmus verursacht.  In diesem Fall können Sie versuchen, zugehörige Parameter zu ändern, z. B. die maximale Größe des N-Gramm-Wörterbuchs oder die Anzahl von Bits für die Featurehasherstellung.

### <a name="lda-and-topic-modeling"></a>LDA und Themenmodellierung

Latent Dirichlet Allocation (LDA) wird für die *inhaltsbasierte Themenmodellierung* verwendet. Dies bedeutet grundsätzlich das Erlernen von Kategorien aus nicht klassifiziertem Text. Bei der inhaltsbasierten Themenmodellierung ist ein Thema eine Verteilung von Wörtern.

Angenommen, Sie haben einen Korpus von Kundenrezensionen bereitgestellt, der sehr viele Produkte enthält. Der Text der Rezensionen, die von vielen Kunden im Lauf der Zeit übermittelt wurden, würde viele Begriffe enthalten, die zum Teil in mehreren Themen verwendet werden.

Ein **Thema**, das durch den LDA-Prozess identifiziert wird, kann eine Rezension für ein einzelnes Produkt A darstellen, oder es kann eine Gruppe von Produktrezensionen darstellen. Für LDA ist das Thema lediglich eine Wahrscheinlichkeitsverteilung über die Zeit für eine Gruppe von Wörtern.

Begriffe betreffen selten exklusiv ein Produkt, sondern können auch auf andere Produkte verweisen, oder es handelt sich um allgemeine Begriffe, die für alles gelten („großartig“, „schrecklich“). Andere Begriffe können Füllwörter sein.  Es ist jedoch wichtig zu verstehen, dass die LDA-Methode nicht zum Erfassen aller Wörter der Welt und zum Verständnis der Beziehung zwischen Wörtern verwendet werden kann (abgesehen von den Wahrscheinlichkeiten zusammen auftretender Wörter). Sie kann nur Wörter gruppieren, die in der Zieldomäne verwendet wurden.

Nachdem die Begriffsindizes berechnet wurden, werden einzelne Textzeilen mit einem entfernungsbasierten Ähnlichkeitsmeasure verglichen, um zu ermitteln, ob zwei Textteile einander ähneln.  Beispielsweise können Sie feststellen, dass das Produkt mehrere Namen aufweist, die stark korreliert sind. Möglicherweise werden Sie auch feststellen, dass stark negative Begriffe normalerweise einem bestimmten Produkt zugeordnet sind. Sie können das Ähnlichkeitsmeasure sowohl zum Identifizieren verwandter Begriffe als auch zum Erstellen von Empfehlungen verwenden.

###  <a name="module-parameters"></a>Modulparameter

|Name|type|Range|Optional|Standard|BESCHREIBUNG|  
|----------|----------|-----------|--------------|-------------|-----------------|  
|Target column(s)|Spaltenauswahl||Erforderlich|StringFeature|Name oder Index der Zielspalte|  
|Number of topics to model|Integer|[1;1000]|Erforderlich|5|Modellieren der Dokumentverteilung anhand von N Themen|  
|N-grams|Integer|[1;10]|Erforderlich|2|Anzahl der N-Gramme an, die während der Hasherstellung generiert werden|  
|Normalize|Boolean|„true“ oder „false“|Erforderlich|true|Normalisieren der Ausgabe in Wahrscheinlichkeiten.  Das transformierte Dataset ist P(Thema&#124;Dokument), und die Feature-Themen-Matrix ist P(Wort&#124;Thema).|  
|Show all options|Boolean|„true“ oder „false“|Erforderlich|False|Zeigt zusätzliche Parameter an, die speziell für die Online-LDA mit scikit-learn gelten|  
|Rho parameter|Float|[0.00001;1.0]|Gilt, wenn das Kontrollkästchen **Show all options** aktiviert ist|0.01|Wort im Thema vor der Verteilung|  
|Alpha parameter|Float|[0.00001;1.0]|Gilt, wenn das Kontrollkästchen **Show all options** aktiviert ist|0.01|Thema im Dokument vor der Verteilung|  
|Estimated number of documents|Integer|[1;int.MaxValue]|Gilt, wenn das Kontrollkästchen **Show all options** aktiviert ist|1000|Geschätzte Anzahl von Dokumenten (entspricht dem Parameter total_samples)|  
|Size of the batch|Integer|[1;1024]|Gilt, wenn das Kontrollkästchen **Show all options** aktiviert ist|32|Größe des Batches|  
|Initial value of iteration used in learning rate update schedule|Integer|[0;int.MaxValue]|Gilt, wenn das Kontrollkästchen **Show all options** aktiviert ist|0|Anfangswert, der die Lernrate für frühe Iterationen herabstuft. Entspricht dem Parameter learning_offset|  
|Power applied to the iteration during updates|Float|[0.0;1.0]|Gilt, wenn das Kontrollkästchen **Show all options** aktiviert ist|0.5|Die auf die Iterationsanzahl aufgewendete Leistung zum Steuern der Lernrate. Entspricht dem Parameter learning_decay |  
|Number of training iterations|Integer|[1;1024]|Gilt, wenn das Kontrollkästchen **Show all options** aktiviert ist|25|Anzahl der Trainingsiterationen|  
|Build dictionary of ngrams|Boolean|„true“ oder „false“|Gilt, wenn das Kontrollkästchen **Show all options** *nicht* aktiviert ist|True|Erstellt vor dem Berechnen der LDA ein Wörterbuch von N-Grammen. Nützlich für die Untersuchung und Interpretation des Modells|  
|Maximum size of ngram dictionary|Integer|[1;int.MaxValue]|Gilt, wenn die Option **Build dictionary of ngrams** den Wert TRUE aufweist|20000|Maximale Größe des N-Gramm-Wörterbuchs. Wenn die Anzahl der Token in der Eingabe diese Größe überschreitet, können Konflikte auftreten.|  
|Number of bits to use for feature hashing|Integer|[1;31]|Gilt, wenn das Kontrollkästchen **Show all options** *nicht* aktiviert ist und **Build dictionary of ngrams** den Wert FALSE aufweist|12|Anzahl von Bits für die Featurehasherstellung| 
|Build dictionary of ngrams prior to LDA|Boolean|„true“ oder „false“|Gilt, wenn das Kontrollkästchen **Show all options** aktiviert ist|True|Erstellt vor der LDA ein Wörterbuch von N-Grammen. Nützlich für die Untersuchung und Interpretation des Modells|  
|Maximum number of ngrams in dictionary|Integer|[1;int.MaxValue]|Gilt, wenn das Kontrollkästchen **Show all options** aktiviert ist und **Build dictionary of ngrams** den Wert TRUE aufweist|20000|Maximale Größe des Wörterbuchs. Wenn die Anzahl der Token in der Eingabe diese Größe überschreitet, können Konflikte auftreten.|  
|Number of hash bits|Integer|[1;31]|Gilt, wenn das Kontrollkästchen **Show all options** aktiviert ist und **Build dictionary of ngrams** den Wert FALSE aufweist|12|Anzahl von Bits für die Featurehasherstellung|   


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an.   
Eine Liste mit Fehlern der jeweiligen Module finden Sie unter [Ausnahmen und Fehlercodes für den Designer](designer-error-codes.md).
