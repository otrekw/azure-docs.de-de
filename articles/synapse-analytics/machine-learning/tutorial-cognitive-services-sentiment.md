---
title: 'Tutorial: Stimmungsanalyse mit Cognitive Services'
description: Hier erfahren Sie, wie Sie Cognitive Services zur Stimmungsanalyse in Azure Synapse Analytics verwenden.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 08d5e53facce172c2287c2e341895f0ee38571f0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98943703"
---
# <a name="tutorial-sentiment-analysis-with-cognitive-services-preview"></a>Tutorial: Stimmungsanalyse mit Cognitive Services (Vorschau)

In diesem Tutorial erfahren Sie, wie Sie Ihre Daten in Azure Synapse Analytics problemlos mit [Azure Cognitive Services](../../cognitive-services/index.yml) anreichern können. Hier werden die Funktionen der [Textanalyse](../../cognitive-services/text-analytics/index.yml) für eine Stimmungsanalyse verwendet. 

Ein Benutzer in Azure Synapse kann einfach eine Tabelle mit einer Textspalte auswählen, um sie mit Stimmungen anzureichern. Diese Stimmungen können positiv, negativ, gemischt oder neutral sein. Auch eine Wahrscheinlichkeit wird zurückgegeben.

In diesem Lernprogramm wird Folgendes behandelt:

> [!div class="checklist"]
> - Schritte zum Abrufen eines Spark-Tabellendatasets mit einer Textspalte für die Stimmungsanalyse
> - Verwenden eines Assistenten in Azure Synapse, um Daten mithilfe der Textanalyse in Cognitive Services anzureichern

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- [Azure Synapse Analytics-Arbeitsbereich](../get-started-create-workspace.md) mit einem als Standardspeicher konfigurierten Azure Data Lake Storage Gen2-Speicherkonto. Für das hier verwendete Data Lake Storage Gen2-Dateisystem müssen Sie über die Rolle *Mitwirkender an Storage-Blobdaten* verfügen.
- Spark-Pool in Ihrem Azure Synapse Analytics-Arbeitsbereich. Ausführliche Informationen finden Sie unter [Erstellen eines Spark-Pools in Azure Synapse](../quickstart-create-sql-pool-studio.md).
- Vorkonfigurationsschritte im Tutorial [Konfigurieren von Cognitive Services in Azure Synapse](tutorial-configure-cognitive-services-synapse.md)

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-spark-table"></a>Erstellen einer Spark-Tabelle

Für dieses Tutorial wird eine Spark-Tabelle benötigt.

1. Laden Sie die Datei [FabrikamComments.csv](https://github.com/Kaiqb/KaiqbRepo0731190208/blob/master/CognitiveServices/TextAnalytics/FabrikamComments.csv) herunter. Sie enthält ein Dataset für die Textanalyse. 

1. Laden Sie die Datei in Ihr Azure Synapse-Speicherkonto in Data Lake Storage Gen2 hoch.
  
   ![Screenshot: Auswahl für das Hochladen von Daten](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00a.png)

1. Erstellen Sie auf der Grundlage der CSV-Datei eine Spark-Tabelle. Klicken Sie hierzu mit der rechten Maustaste auf die Datei, und wählen Sie **Neues Notebook**  > **Spark-Tabelle erstellen** aus.

   ![Screenshot: Auswahl für die Erstellung einer Spark-Tabelle](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00b.png)

1. Benennen Sie die Tabelle in der Codezelle, und führen Sie das Notebook in einem Spark-Pool aus. Vergessen Sie nicht, `header=True` festzulegen.

   ![Screenshot: Ausführen eines Notebooks](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00c.png)

   ```python
   %%pyspark
   df = spark.read.load('abfss://default@azuresynapsesa.dfs.core.windows.net/data/FabrikamComments.csv', format='csv'
   ## If a header exists, uncomment the line below
   , header=True
   )
   df.write.mode("overwrite").saveAsTable("default.YourTableName")
   ```

## <a name="open-the-cognitive-services-wizard"></a>Öffnen des Cognitive Services-Assistenten

1. Klicken Sie mit der rechten Maustaste auf die im vorherigen Verfahren erstellte Spark-Tabelle. Wählen Sie **Machine Learning** > **Mit vorhandenem Modell anreichern** aus.

   ![Screenshot: Auswahl für das Öffnen des Bewertungs-Assistenten](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00d.png)

2. Ein Konfigurationsbereich wird angezeigt, und Sie werden aufgefordert, ein Cognitive Services-Modell auszuwählen. Wählen Sie **Textanalyse – Standpunktanalyse**“ aus.

   ![Screenshot: Auswählen eines Cognitive Services-Modells](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00e.png)

## <a name="provide-authentication-details"></a>Angeben von Authentifizierungsdetails

Für die Authentifizierung bei Cognitive Services müssen Sie auf das Geheimnis für Ihren Schlüsseltresor verweisen. Die folgenden Eingaben sind abhängig von [erforderlichen Schritten](tutorial-configure-cognitive-services-synapse.md), die Sie zu diesem Zeitpunkt bereits abgeschlossen haben sollten.

- **Azure-Abonnement**: Wählen Sie das Abonnement aus, zu dem Ihre Key Vault-Instanz gehört.
- **Cognitive Services-Konto:** Geben Sie die Textanalyseressource ein, mit der Sie eine Verbindung herstellen.
- **Mit Azure Key Vault verknüpfter Dienst:** Im Rahmen der erforderlichen Schritte haben Sie einen verknüpften Dienst für Ihre Textanalyseressource erstellt. Wählen Sie ihn hier aus.
- **Geheimnisname**: Geben Sie den Namen des Geheimnisses in Ihrem Schlüsseltresor ein, der den Schlüssel zum Authentifizieren bei Ihrer Cognitive Services-Ressource enthält.

![Screenshot: Authentifizierungsdetails für einen Schlüsseltresor](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00f.png)

## <a name="configure-sentiment-analysis"></a>Konfigurieren der Stimmungsanalyse

Konfigurieren Sie als Nächstes die Stimmungsanalyse. Wählen Sie die folgenden Details aus:
- **Sprache:** Wählen Sie **Englisch** als Sprache des Texts aus, für den Sie eine Stimmungsanalyse durchführen möchten.
- **Textspalte**: Wählen Sie die Spalte **comment (string)** (Kommentar (Zeichenfolge)) als die Textspalte in Ihrem Dataset aus, die Sie analysieren möchten, um die Stimmung zu ermitteln.

Wenn Sie fertig sind, wählen Sie **Notebook öffnen** aus. Dadurch wird ein Notebook mit PySpark-Code generiert, der die Stimmungsanalyse mithilfe von Azure Cognitive Services durchführt.

![Screenshot: Auswahl für die Konfiguration der Stimmungsanalyse](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00g.png)

## <a name="run-the-notebook"></a>Ausführen des Notebooks

Das soeben geöffnete Notebook verwendet die [mmlspark-Bibliothek](https://github.com/Azure/mmlspark), um eine Verbindung mit Cognitive Services herzustellen. Mit den von Ihnen bereitgestellten Azure Key Vault-Details können Sie auf sichere Weise auf Ihre Geheimnisse verweisen, ohne sie offenzulegen.

Nun können Sie alle Zellen ausführen, um Ihre Daten mit Stimmungen anzureichern. Wählen Sie **Alle ausführen** aus. 

Die Stimmungen werden als **positive** (positiv), **negative** (negativ), **neutral** (neutral) oder **mixed** (gemischt) zurückgegeben. Des Weiteren erhalten Sie Wahrscheinlichkeiten pro Stimmung. Weitere Informationen zur Stimmungsanalyse in Cognitive Services finden Sie [hier](../../cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis.md).

![Screenshot: Stimmungsanalyse](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00h.png)

## <a name="next-steps"></a>Nächste Schritte
- [Tutorial: Anomalieerkennung mit Azure Cognitive Services](tutorial-cognitive-services-sentiment.md)
- [Tutorial: Assistent für die Modellbewertung in dedizierten SQL-Pools von Azure Synapse](tutorial-sql-pool-model-scoring-wizard.md)
- [Machine Learning-Funktionen in Azure Synapse Analytics](what-is-machine-learning.md)