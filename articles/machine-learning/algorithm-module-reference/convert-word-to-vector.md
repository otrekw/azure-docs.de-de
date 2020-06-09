---
title: Konvertieren eines Word-Dokuments in das PDF-Format
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit den drei mitgelieferten Word2Vec-Modellen ein Vokabular und die entsprechenden Worteinbettungen aus einem Textkorpus extrahieren können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/19/2020
ms.openlocfilehash: e0e796b75690bcacc6be8ef29b8b490c7faa40af
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83853652"
---
# <a name="convert-word-to-vector"></a>Konvertieren eines Word-Dokuments in das PDF-Format

In diesem Artikel wird beschrieben, wie Sie das Modul **Konvertieren eines Word-Dokuments in das PDF-Format** im Azure Machine Learning-Designer (Vorschauversion) verwenden, um verschiedene Word2Vec-Modelle (mit Word2Vec, FastText, Glove vortrainiertes Modell) auf den Textkorpus anzuwenden, den Sie als Eingabe angegeben haben, und ein Vokabular mit Worteinbettungen zu generieren.

Dieses Modul verwendet die Gensim-Bibliothek. Weitere Informationen zu Gensim finden Sie auf dessen [offizieller Website](https://radimrehurek.com/gensim/apiref.html), die Tutorials und eine Erläuterung der Algorithmen enthält.

### <a name="more-about-convert-word-to-vector"></a>Weitere Informationen zum Konvertieren von Wörtern in Vektoren

Im Allgemeinen ist die Konvertierung von Wörtern in Vektoren oder die Wortvektorisierung ein natürlicher Sprachverarbeitungsprozess, bei dem Sprachmodelle oder -verfahren verwendet werden, um Wörter im Vektorraum abzubilden, d. h. jedes Wort durch einen Vektor reeller Zahlen darzustellen, und bei dem Wörter mit ähnlichen Bedeutungen ähnliche Darstellungen aufweisen können.

Worteinbettungen können als anfängliche Eingabe für NLP-Downstreamaufgaben wie Textklassifizierung, Standpunktanalyse usw. verwendet werden.

Unter den verschiedenen Technologien zur Worteinbettung haben wir in diesem Modul drei weit verbreitete Methoden implementiert, darunter zwei Onlinetrainingsmodelle, Word2Vec und FastText, und ein vortrainiertes Modell, glove-wiki-gigaword-100. Onlinetrainingsmodelle werden anhand Ihrer Eingabedaten trainiert, während vortrainierte Modelle offline an einem größeren Textkorpus (z. B. Wikipedia, Google News) trainiert werden, der in der Regel etwa 100 Milliarden Wörter enthält, sodass die Worteinbettung während der Wortvektorisierung konstant bleibt. Vortrainierte Wortmodelle bieten Vorteile, z. B. kürzere Trainingszeit, bessere codierte Wortvektoren und eine verbesserte Gesamtleistung.

+ Word2Vec ist eine der beliebtesten Techniken zum Erlernen von Worteinbettungen mit flachen neuronalen Netzen. Die Theorie wird in diesem Dokument behandelt, das als PDF-Download zur Verfügung steht: [Efficient Estimation of Word Representations in Vector Space, Mikolov, Tomas, et al](https://arxiv.org/pdf/1301.3781.pdf). Die Implementierung in diesem Modul basiert auf der [gensim-Bibliothek für Word2Vec](https://radimrehurek.com/gensim/models/word2vec.html).

+ Die FastText-Theorie wird in diesem Dokument erläutert, das als PDF-Download zur Verfügung steht: [Enriching Word Vectors with Subword Information, Bojanowski, Piotr, et al](https://arxiv.org/pdf/1607.04606.pdf). Die Implementierung in diesem Modul basiert auf der [gensim-Bibliothek für FastText](https://radimrehurek.com/gensim/models/fasttext.html).

+ Mit Glove vortrainiertes Modell: glove-wiki-gigaword-100 ist eine Sammlung von vortrainierten Vektoren, die auf dem Wikipedia-Textkorpus basiert, der 5,6 Milliarden Token und 400K kasusfreies Vokabular enthält, PDF-Datei ist verfügbar: [GloVe: Globale Vektoren für die Wortdarstellung](https://nlp.stanford.edu/pubs/glove.pdf).

## <a name="how-to-configure-convert-word-to-vector"></a>Konfigurieren der Konvertierung von Wörtern in Vektoren

Dieses Modul erfordert ein Dataset mit einer Textspalte, besser ist vorverarbeiteter Text.

1. Fügen Sie das Modul zum **Konvertieren von Wörtern in Vektoren** Ihrer Pipeline hinzu.

2. Stellen Sie als Eingabe für das Modul ein Dataset mit einer oder mehreren Textspalten bereit.

3. Wählen Sie für **Zielspalte** nur eine Spalte aus, die zu verarbeitenden Text enthält.

    Da dieses Modul im Allgemeinen ein Vokabular aus Text erstellt, unterscheidet sich der Inhalt der verschiedenen Spalten, was zu unterschiedlichen Vokabularinhalten führt, daher akzeptiert das Modul nur eine Zielspalte.

4. Wählen Sie für die **Word2Vec-Strategie** zwischen `GloVe pretrained English Model`, `Gensim Word2Vec` und `Gensim FastText`.

5. Wenn die **Wort2Vec-Strategie** entweder `Gensim Word2Vec` oder `Gensim FastText` entspricht:

    + **Word2Vec-Trainingsalgorithmus**. Wählen Sie zwischen `Skip_gram` und `CBOW`. Der Unterschied wird in dem ursprünglichen [Dokument](https://arxiv.org/pdf/1301.3781.pdf) eingeführt.

        Die Standardmethode ist `Skip_gram`.

    + **Länge der Worteinbettung**. Geben Sie die Dimensionalität der Wortvektoren an. Entspricht dem Parameter `size` in gensim.

        Der Standardwert für die Einbettungsgröße ist 100.

    + **Größe des Kontextfensters**. Geben Sie den maximalen Abstand zwischen dem vorhergesagten Wort und dem aktuellen Wort an. Entspricht dem Parameter `window` in gensim.

        Die Standardfenstergröße ist 5.

    + **Anzahl der Epochen**. Geben Sie die Anzahl der über den Korpus verlaufenden Epochen (Iterationen) an. Entspricht dem Parameter `iter` in gensim.

        Die Standardanzahl der Epochen ist 5.

6. Geben Sie für die **maximale Vokabulargröße** die maximale Anzahl der Wörter im generierten Vokabular an.

    Wenn es weitere eindeutige Wörter als nur dieses gibt, dann löschen Sie die unregelmäßigen Wörter.

    Die standardmäßige Vokabulargröße ist 10000.

7. Geben Sie für die **Mindestwortzahl** eine Mindestwortzahl an, wodurch das Modul alle Wörter ignoriert, deren Häufigkeit niedriger als dieser Wert ist.

    Der Standardwert ist 5.

8. Übermitteln Sie die Pipeline.

## <a name="examples"></a>Beispiele

Das Modul hat eine Ausgabe:

+ **Vokabular mit Einbettungen**: Enthält das generierte Vokabular. Zusammen mit der Einbettung jedes Worts nimmt eine Dimension eine Spalte ein.

### <a name="result-examples"></a>Beispiele für Ergebnisse

Um zu veranschaulichen, wie das Modul zum **Konvertieren von Wörtern in Vektoren** funktioniert, wendet das folgende Beispiel dieses Modul mit den Standardeinstellungen auf das vorverarbeitete Wikipedia SP 500-Dataset an, das in Azure Machine Learning bereitgestellt wird (Vorschau).

#### <a name="source-dataset"></a>Quelldataset

Das Dataset enthält eine Kategoriespalte sowie den vollständigen Text, der von Wikipedia abgerufen wird. In dieser Tabelle werden nur einige repräsentative Beispiele angezeigt.

|text|
|----------|
|nasdaq 100 component s p 500 component foundation founder location city apple campus 1 infinite loop street infinite loop cupertino california cupertino california location country united states...|
|br nasdaq 100 nasdaq 100 component br s p 500 s p 500 component industry computer software foundation br founder charles geschke br john warnock location adobe systems...|
|s p 500 s p 500 component industry automotive industry automotive predecessor general motors corporation 1908 2009 successor...|
|s p 500 s p 500 component industry conglomerate company conglomerate foundation founder location city fairfield connecticut fairfield connecticut location country usa area...|
|br s p 500 s p 500 component foundation 1903 founder william s harley br arthur davidson harley davidson founder arthur davidson br walter davidson br william a davidson location...|

#### <a name="output-vocabulary-with-embeddings"></a>Ausgabevokabular mit Einbettungen

Die folgende Tabelle enthält die Ausgabe dieses Moduls, das das Wikipedia SP 500-Dataset als Eingabe übernimmt. Die äußerste linke Spalte gibt das Vokabular an, dessen Einbettungsvektor wird durch die Werte der übrigen Spalten in derselben Zeile dargestellt.

|Vokabular|Embedding dim 0|Embedding dim 1|Embedding dim 2|Embedding dim 3|Embedding dim 4|Embedding dim 5|...|Embedding dim 99|
|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|
|nasdaq|-0,375865|0,609234|0,812797|-0,002236|0,319071|-0,591986|...|0,364276
|Komponente|0,081302|0,40001|0,121803|0,108181|0,043651|-0,091452|...|0,636587
|s|-0,34355|-0,037092|-0,012167|0,151542|0,601019|0,084501|...|0,149419
|p|-0,133407|0,073244|0,170396|0,326706|0,213463|-0,700355|...|0,530901
foundation|-0,166819|0,10883|-0,07933|-0,073753|0,262137|0,045725|...|0,27487
founder|-0,297408|0,493067|0,316709|-0,031651|0,455416|-0,284208|...|0,22798
location|-0,375213|0,461229|0,310698|0,213465|0,200092|0,314288|...|0,14228
city|-0,460828|0,505516|-0,074294|-0,00639|0,116545|0,494368|...|-0,2403
apple|0,05779|0,672657|0,597267|-0,898889|0,099901|0,11833|...|0,4636
campus|-0,281835|0,29312|0,106966|-0,031385|0,100777|0,061452|...|0,05978
infinite|-0,263074|0,245753|0,07058|-0,164666|0,162857|-0,027345|...|-0,0525
loop|-0,391421|0,52366|0,141503|-0,105423|0,084503|-0,018424|...|-0,0521

In diesem Beispiel haben wir die Voreinstellung `Gensim Word2Vec` als **Word2Vec-Strategie** verwendet. Der **Trainingsalgorithmus** ist `Skip-gram`, die **Länge der Worteinbettung** ist 100 und somit haben 100 Einbettungsspalten.

## <a name="technical-notes"></a>Technische Hinweise

Dieser Abschnitt enthält Tipps und Antworten auf häufig gestellte Fragen.

+ Unterschied zwischen Onlinetraining und vortrainiertem Modell

    In diesem Modul zum **Konvertieren von Wörtern in Vektoren** wurden drei verschiedene Strategien, zwei Onlinetrainingsmodelle und ein vortrainiertes Modell bereitgestellt. Das Onlinetrainingsmodell verwendet Ihr Eingabedataset als Trainingsdaten, generiert während des Trainings Vokabular und Wortvektoren, während das vortrainierte Modell bereits durch viel größere Textkorpusse wie Wikipedia- oder Twitter-Text trainiert wird, sodass das vortrainierte Modell eigentlich eine Sammlung von (Wort, Einbettung)-Paaren ist.  

    Wenn das mit Glove vortrainierte Modell als Wortvektorisierungsstrategie gewählt wird, fasst es ein Vokabular aus dem Eingabedataset zusammen und erzeugt einen Einbettungsvektor für jedes Wort aus dem vortrainierten Modell. Ohne Onlinetraining könnte die Verwendung des vortrainierten Modells entsprechende Trainingszeit einsparen und eine bessere Leistung aufweisen, insbesondere wenn die Größe des Eingabedatasets relativ klein ist.

+ Größe der Einbettung

    Im Allgemeinen wird die Länge der Worteinbettung auf einige Hundert (z. B. 100, 200, 300) festgelegt, um eine angemessene Leistung zu erzielen, da eine kleine Einbettungsgröße einen geringen Vektorraum bedeutet, was zu Konflikten bei der Worteinbettung führen kann.  

    Bei vortrainierten Modellen ist die Länge der Worteinbettungen festgelegt, bei dieser Implementierung beträgt die Einbettungsgröße von „glove-wiki-gigaword-100“ 100.


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 

Eine Liste von Fehlern, die spezifisch für die Designer-Module (Vorschau) sind, finden Sie unter [Machine Learning-Fehlercodes](designer-error-codes.md).
