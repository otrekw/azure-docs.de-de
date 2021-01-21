---
title: 'Tutorial: Stimmungsanalyse mit Cognitive Services'
description: Tutorial zur Verwendung von Cognitive Services zur Stimmungsanalyse in Synapse
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 6a4833cf0d73939e01fd3e3e7263c6cba3c0a28a
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98222189"
---
# <a name="tutorial-sentiment-analysis-with-cognitive-services-preview"></a>Tutorial: Stimmungsanalyse mit Cognitive Services (Vorschau)

In diesem Tutorial erfahren Sie, wie Sie Ihre Daten in Azure Synapse mit [Cognitive Services](../../cognitive-services/index.yml) problemlos anreichern können. Wir verwenden die Funktionen der [Textanalyse](../../cognitive-services/text-analytics/index.yml) zum Durchführen einer Stimmungsanalyse. Ein Benutzer in Azure Synapse kann einfach eine Tabelle auswählen, die eine Textspalte enthält, um sie mit Stimmungen anzureichern. Diese Stimmungen können positiv, negativ, gemischt oder neutral sein, und es wird auch eine Wahrscheinlichkeit zurückgegeben.

In diesem Lernprogramm wird Folgendes behandelt:

> [!div class="checklist"]
> - Schritte zum Abrufen eines Spark-Tabellendatasets, das Textspalten für die Stimmungsanalyse enthält.
> - Verwenden eines Assistenten in Azure Synapse, um Daten mithilfe von Cognitive Services für Textanalyse anzureichern

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- Als Standardspeicher konfigurierter [Azure Synapse Analytics-Arbeitsbereich](../get-started-create-workspace.md) mit einem ADLS Gen2-Speicherkonto. Sie müssen **Mitwirkender an Storage-Blobdaten** des ADLS Gen2-Dateisystems sein, mit dem Sie arbeiten.
- Spark-Pool in Ihrem Azure Synapse Analytics-Arbeitsbereich. Ausführliche Informationen finden Sie unter [Erstellen eines Spark-Pools in Azure Synapse](../quickstart-create-sql-pool-studio.md).
- Bevor Sie dieses Tutorial verwenden können, müssen Sie auch die in diesem Tutorial beschriebenen Schritte vor der Konfiguration ausführen. [Konfigurieren von Cognitive Services in Azure Synapse](tutorial-configure-cognitive-services-synapse.md).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)

## <a name="create-a-spark-table"></a>Erstellen einer Spark-Tabelle

Für dieses Tutorial benötigen Sie eine Spark-Tabelle.

1. Laden Sie die folgende CSV-Datei mit einem Dataset für die Textanalyse herunter: [FabrikamComments.csv](https://github.com/Kaiqb/KaiqbRepo0731190208/blob/master/CognitiveServices/TextAnalytics/FabrikamComments.csv)

1. Laden Sie die Datei in Ihr Azure Synapse ADLSGen2-Speicherkonto hoch.
![Hochladen von Daten](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00a.png)

1. Erstellen Sie eine Spark-Tabelle aus der CSV-Datei, indem Sie mit der rechten Maustaste auf die Datei klicken und **Neues Notebook > Spark-Tabelle erstellen** auswählen.
![Spark-Tabelle erstellen](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00b.png)

1. Benennen Sie die Tabelle in der Codezelle, und führen Sie das Notebook in einem Spark-Pool aus. Denken Sie daran, „header = True“ festzulegen.
![Ausführen des Notebooks](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00c.png)

```python
%%pyspark
df = spark.read.load('abfss://default@azuresynapsesa.dfs.core.windows.net/data/FabrikamComments.csv', format='csv'
## If header exists uncomment line below
, header=True
)
df.write.mode("overwrite").saveAsTable("default.YourTableName")
```

## <a name="launch-cognitive-services-wizard"></a>Starten des Cognitive Services-Assistenten

1. Klicken Sie mit der rechten Maustaste auf die im vorherigen Schritt erstellte Spark-Tabelle. Wählen Sie „Machine Learning“ -> „Mit vorhandenem Modell anreichern“ aus.
![Starten des Bewertungsassistenten](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00d.png)

2. Ein Konfigurationsbereich wird angezeigt, und Sie werden aufgefordert, ein Cognitive Services-Modell auszuwählen. Wählen Sie „Textanalyse – Standpunktanalyse“ aus.

![Starten des Bewertungsassistenten – Schritt 1](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00e.png)

## <a name="provide-authentication-details"></a>Angeben von Authentifizierungsdetails

Für die Authentifizierung bei Cognitive Services müssen Sie auf das Geheimnis verweisen, das in Ihrer Key Vault-Instanz verwendet werden soll. Die folgenden Eingaben sind abhängig von [erforderlichen Schritten](tutorial-configure-cognitive-services-synapse.md), die Sie vor diesem Schritt abgeschlossen haben sollten.

- **Azure-Abonnement**: Wählen Sie das Abonnement aus, zu dem Ihre Key Vault-Instanz gehört.
- **Cognitive Services-Konto**: Dies ist die Textanalyse-Ressource, mit der Sie eine Verbindung herstellen möchten.
- **Mit Azure Key Vault verknüpfter Dienst**: Im Rahmen der erforderlichen Schritte haben Sie einen verknüpften Dienst für Ihre Textanalyse Ressource erstellt. Wählen Sie ihn hier aus.
- **Geheimnisname**: Dies ist der Name des Geheimnisses in Ihrem Schlüsseltresor, der den Schlüssel zum Authentifizieren bei ihrer Cognitive Services-Ressource enthält.

![Angeben von Azure Key Vault-Details](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00f.png)

## <a name="configure-sentiment-analysis"></a>Konfigurieren der Stimmungsanalyse

Als nächstes müssen Sie die Stimmungsanalyse konfigurieren. Wählen Sie die folgenden Details aus:
- **Sprache:** Wählen Sie die Sprache des Texts aus, für den Sie eine Stimmungsanalyse durchführen möchten. Wählen Sie **DE** aus.
- **Textspalte**: Dies ist die Textspalte in Ihrem Dataset, die Sie analysieren möchten, um die Stimmung zu ermitteln. Wählen Sie die Tabellenspalte **Kommentar** aus.

Wenn Sie fertig sind, klicken Sie **Notebook öffnen** aus. Dadurch wird ein Notebook für Sie mit PySpark-Code generiert, der die Stimmungsanalyse mithilfe von Azure Cognitive Services ausführt.

![Konfigurieren der Stimmungsanalyse](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00g.png)

## <a name="open-notebook-and-run"></a>Öffnen und Ausführen des Notebooks

Das soeben geöffnete Notebook verwendet die [mmlspark-Bibliothek](https://github.com/Azure/mmlspark), um eine Verbindung mit Cognitive Services herzustellen.

Mit den von Ihnen bereitgestellten Azure Key Vault-Details können Sie auf sichere Weise auf Ihre Geheimnisse verweisen, ohne sie offenzulegen.

Sie können jetzt **Alle ausführen** für die Zellen wählen, um Ihre Daten mit Stimmungen anzureichern. Die Stimmungen werden als positiv/negativ/neutral/gemischt zurückgegeben, und Sie erhalten auch eine Wahrscheinlichkeit pro Stimmung. Weitere Informationen zu [Cognitive Services – Stimmungsanalyse](../../cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis.md)

![Ausführen der Stimmungsanalyse](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00h.png)

## <a name="next-steps"></a>Nächste Schritte
- [Tutorial: Anomalieerkennung mit Azure Cognitive Services](tutorial-cognitive-services-sentiment.md)
- [Tutorial: Assistent für die Modellbewertung in dedizierten SQL-Pools von Azure Synapse](tutorial-sql-pool-model-scoring-wizard.md)
- [Machine Learning-Funktionen in Azure Synapse Analytics](what-is-machine-learning.md)