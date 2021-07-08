---
title: Illumina Platinum Genomes
titleSuffix: Azure Open Datasets
description: Hier erfahren Sie, wie Sie das „Illumina Platinum Genomes“-Dataset in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 03e6bf1e30410092be9665053d808d13eb7f4c28
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038746"
---
# <a name="illumina-platinum-genomes"></a>Illumina Platinum Genomes

Mithilfe der vollständigen Genomsequenzierung können Wissenschaftler weltweit das menschliche Genom besser und genauer charakterisieren. Dafür ist ein umfassender, genomweiter Katalog hochwahrscheinlicher Varianten in einer Genomgruppe als Maßstab nötig. Illumina hat vollständige Genomsequenzdaten von 17 Individuen aus drei Generationen generiert und die Varianten in jedem Genom mithilfe verschiedener, derzeit verfügbarer Algorithmen ermittelt.

Weitere Informationen zu den Daten finden Sie auf der offiziellen [Illumina-Website.](https://www.illumina.com/platinumgenomes.html)

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="data-source"></a>Datenquelle

Dieses Dataset ist eine Spiegelung von ftp://ussd-ftp.illumina.com/.

## <a name="data-volumes-and-update-frequency"></a>Datenvolumes und Aktualisierungshäufigkeit

Dieses Dataset enthält etwa 2 GB Daten und wird täglich aktualisiert.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in den Azure-Regionen „USA, Westen 2“ und „USA, Westen-Mitte“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in den Regionen „USA, Westen 2“ oder „USA, Westen-Mitte“ empfohlen.

## <a name="data-access"></a>Datenzugriff

USA, Westen 2: https://datasetplatinumgenomes.blob.core.windows.net/dataset

USA, Westen-Mitte: https://datasetplatinumgenomes-secondary.blob.core.windows.net/dataset

[SAS Token](/azure/storage/common/storage-sas-overview): sv=2019-02-02&se=2050-01-01T08%3A00%3A00Z&si=prod&sr=c&sig=FFfZ0QaDcnEPQmWsshtpoYOjbzd4jtwIWeK%2Fc4i9MqM%3D

## <a name="use-terms"></a>Nutzungsbedingungen

Die Daten stehen uneingeschränkt zur Verfügung. Weitere Informationen und Zitatdetails finden Sie auf der [offiziellen Illumina-Website](https://www.illumina.com/platinumgenomes.html).

## <a name="contact"></a>Contact

Bei Fragen oder Feedback zum Dataset wenden Sie sich an platinumgenomes@illumina.com.

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=genomics-platinum-genomes -->

> [!TIP]
> **[Laden Sie stattdessen das Dataset herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=genomics-platinum-genomes)** .

## <a name="getting-the-illumina-platinum-genomes-from-azure-open-datasets-and-doing-initial-analysis"></a>Abrufen von „Illumina Platinum Genomes“ aus Azure Open Datasets und Durchführen der ersten Analyse 

Verwenden Sie Jupyter Notebooks, GATK und Picard für folgende Zwecke:

1. Kommentieren von Genotypen mit VariantFiltration
2. Auswählen bestimmter Varianten
3. Filtern der relevanten Varianten – keine Aufrufe ODER bestimmte Regionen
4. Durchführen einer Übereinstimmungsanalyse
5. Konvertieren der endgültigen VCF-Dateien in eine Tabelle 

**Abhängigkeiten**:

Für dieses Notebook sind die folgenden Bibliotheken erforderlich:

- Azure Storage `pip install azure-storage-blob`

- numpy `pip install numpy`

- Genome Analysis Toolkit (GATK) (*Benutzer müssen GATK von der Webseite des Broad Institute mit diesem Notebook in dieselbe Compute-Umgebung herunterladen: https://github.com/broadinstitute/gatk/releases* )

**Wichtige Information: Dieses Notebook verwendet den Python 3.6-Kernel.**

## <a name="getting-the-genomics-data-from-azure-open-datasets"></a>Abrufen der Genomics-Daten aus Azure Open Datasets

[Hier](https://azure.microsoft.com/services/open-datasets/catalog/) finden Sie eine Reihe von öffentlichen Genomikdaten, die in Azure Open Dataset hochgeladen wurden. Es wurde ein Blob-Dienst erstellt, der mit dieser Open Dataset-Instanz verknüpft ist. Beispiele zur Vorgehensweise beim Aufrufen von Daten aus Azure Open Dataset für das Dataset `Illumina Platinum Genomes` finden Sie unten:

### <a name="downloading-the-specific-illumina-platinum-genomes"></a>Herunterladen des spezifischen „Illumina Platinum Genomes“

```python
import os
import uuid
import sys
from azure.storage.blob import BlockBlobService, PublicAccess

blob_service_client = BlockBlobService(account_name='datasetplatinumgenomes', sas_token='sv=2019-02-02&se=2050-01-01T08%3A00%3A00Z&si=prod&sr=c&sig=FFfZ0QaDcnEPQmWsshtpoYOjbzd4jtwIWeK%2Fc4i9MqM%3D')     
blob_service_client.get_blob_to_path('dataset/2017-1.0/hg38/small_variants/NA12877', 'NA12877.vcf.gz', './NA12877.vcf.gz')
```

### <a name="1-annotate-genotypes-using-variantfiltration"></a>1. Kommentieren von Genotypen mit VariantFiltration

**Wichtiger Hinweis: Überprüfen Sie, ob Ihr GATK auf Ihrem System ausgeführt wird.**

Wenn wir heterozygote Genotypen filtern möchten, verwenden wir die VariantFiltration-Option `--genotype-filter-expression isHet == 1`. Wir können den Anmerkungswert für das Tool angeben, um die heterozygoten Genotypen mit der Option `--genotype-filter-name` zu bezeichnen. Hier wird der Wert dieses Parameters auf `isHetFilter` festgelegt. In unserem ersten Beispiel haben wir `NA12877.vcf.gz` aus „Illumina Platinum Genomes“ verwendet, aber Benutzer können beliebige VCF-Dateien aus anderen Datasets verwenden:`Platinum Genomes`

```python
run gatk VariantFiltration -V NA12877.vcf.gz -O outputannot.vcf --genotype-filter-expression "isHet == 1" --genotype-filter-name "isHetFilter"
```

### <a name="2-select-specific-variants"></a>2. Auswählen bestimmter Varianten

Wählen Sie eine Teilmenge von Varianten aus einer VCF-Datei aus. Dieses Tool ermöglicht es, eine Teilmenge von Varianten basierend auf verschiedenen Kriterien auszuwählen, um bestimmte Analysen zu unterstützen. Beispiele für solche Analysen sind das Vergleichen und Gegenüberstellen von Fällen und Kontrollen, das Extrahieren von varianten oder nicht varianten Loci, die bestimmte Anforderungen erfüllen, oder die Problembehandlung einiger unerwarteter Ergebnisse, um nur einige zu nennen.

Es gibt viele verschiedene Optionen zum Auswählen von Teilmengen von Varianten aus einem größeren Aufrufsatz:

Extrahieren Sie eine oder mehrere Stichproben aus einem Aufrufsatz basierend auf einem vollständigen Stichprobennamen oder einem Musterabgleich.
Geben Sie Kriterien für die Aufnahme an, die Schwellenwerte für Anmerkungswerte festlegen, **z. B. „DP > 1000“ (Abdeckungstiefe größer als 1000x), „AF < 0,25“ (Stellen mit einer Allelehäufigkeit kleiner als 0,25)** . Diese Kriterien werden als „JEXL-Ausdrücke“ geschrieben, die im Artikel zur Verwendung von JEXL-Ausdrücken dokumentiert sind.
Stellen Sie Übereinstimmungs- oder Nichtübereinstimmungs-Tracks bereit, um Varianten einzuschließen oder auszuschließen, die auch in anderen angegebenen Aufrufsätzen vorhanden sind.
Wählen Sie Varianten basierend auf Kriterien wie ihrem Typ (z. B. nur INDELs), Anzeichen für mendelsche Verletzungen, Filterstatus, Allelizität usw. aus. Es gibt auch mehrere Optionen zum Aufzeichnen der ursprünglichen Werte bestimmter Anmerkungen, die neu berechnet werden, wenn eine Teilmenge des neuen Aufrufsatzes erstellt wird, Allele gekürzt werden usw.

Eingabe: Ein Varianten-Aufrufsatz im VCF-Format, aus dem eine Teilmenge ausgewählt werden kann.

Ausgabe: Eine neue VCF-Datei, die die ausgewählte Teilmenge von Varianten enthält.

```python
run gatk SelectVariants -R Homo_sapiens_assembly38.fasta -V outputannot.vcf --select-type-to-include SNP --select-type-to-include INDEL -O selective.vcf
```

### <a name="3-transform-filtered-genotypes-to-no-call"></a>3. Transformieren gefilterter Genotypen in „No call“ 

Wenn Sie SelectVariants mit „--set-filtered-gt-to-nocall“ ausführen, werden die gekennzeichneten Genotypen weiter mit einem Null-Genotyp-Aufruf weiter transformiert. 

Diese Konvertierung ist erforderlich, da Downstreamtools das Filterfeld auf FORMAT-Ebene nicht analysieren.

Wie können wir die Varianten mit **„No call“** filtern?

```python
run gatk SelectVariants -V outputannot.vcf --set-filtered-gt-to-nocall -O outputnocall.vcf
```

### <a name="4-check-the-concordance-of-vcf-file-with-ground-truth"></a>4. Überprüfen der Übereinstimmung der VCF-Datei mit Ground Truth

Werten Sie die Übereinstimmung auf der Ebene der Stelle einer Eingabe-VCF anhand einer Truth-VCF aus.
Dieses Tool wertet zwei variante Aufrufsätze gegeneinander aus und erzeugt eine sechsspaltige Tabelle mit zusammenfassenden Metriken. 

**Diese Funktion führt Folgendes aus:**

1. Stratifiziert SNP- und INDEL-Aufrufe
2. Meldet richtig positive, falsch positive und falsch negative Aufrufe
3. Berechnet Empfindlichkeit und Genauigkeit

Das Tool geht davon aus, dass alle Datensätze in der --truth-VCF Truth-Varianten übergeben. Für die -eval-VCF verwendet das Tool nur ungefilterte Übergabeaufrufe.

Optional kann das Tool so eingestellt werden, dass VCFs der folgenden Variantendatensätze erzeugt werden, die mit dem Status der einzelnen Varianten versehen sind:

Richtig positive und falsch negative Ergebnisse (d. h. alle Varianten in der Truth-VCF): nützlich zum Berechnen der Empfindlichkeit

Richtig positive und falsch positive Ergebnisse (d. h. alle Varianten in der Eval-VCF): nützlich zum Abrufen eines Trainingsdatensets für Machine Learning-Klassifizierer von Artefakten

**Diese Ausgabe-VCFs können an VariantsToTable übergeben werden, um eine TSV-Datei für die statistische Analyse in R oder Python zu erstellen.**

```python
 run gatk Concordance -R Homo_sapiens_assembly38.fasta -eval outputannot.vcf --truth outputnocall.vcf  --summary summary.tsv 
```

### <a name="5-variantstotable"></a>5. VariantsToTable

Extrahieren Sie Felder aus einer VCF-Datei in eine durch Tabstopps getrennte Tabelle. Dieses Tool extrahiert angegebene Felder für jede Variante in einer VCF-Datei in eine durch Tabstopps getrennte Tabelle, die möglicherweise einfacher zu verwenden ist als eine VCF. Standardmäßig extrahiert das Tool nur PASS- oder (ungefilterte) Varianten in der VCF-Datei. Gefilterte Varianten können in die Ausgabe eingeschlossen werden, indem das Flag „--show-filtered“ hinzugefügt wird. Das Tool kann sowohl INFO-Felder (d. h. Felder auf der Ebene der Stelle) als auch FORMAT-Felder (d. h. Felder auf der Ebene der Stichprobe) extrahieren.

INFO-Felder/Felder auf der Ebene der Stelle:

Verwenden Sie das Argument `-F`, um INFO-Felder zu extrahieren. Jedes Feld belegt eine einzelne Spalte in der Ausgabedatei. Das Feld kann eine beliebige VCF-Standardspalte (z. B. CHROM, ID, QUAL) oder ein beliebiger Anmerkungsname im INFO-Feld (z. B. AC, AF) sein. Das Tool unterstützt auch die folgenden Felder:

EVENTLENGTH (Länge des Ereignisses) TRANSITION (1 für eine biallelische Transition (SNP), 0 für eine biallelische Transversion (SNP), -1 für INDELs und multiallelische) HET (Anzahl heterozygoter Genotypen) HOM-REF (Anzahl homozygoter Referenzgenotypen) HOM-VAR (Anzahl homozygoter Variantengenotypen) NO-CALL (Anzahl von No-call-Genotypen) TYPE (Variantentyp, mögliche Werte sind NO_VARIATION, SNP, MNP, INDEL, SYMBOLIC und MIXED VAR (Anzahl der Nicht-Referenz-Genotypen) NSAMPLES (Anzahl der Stichproben) NCALLED (Anzahl der aufgerufenen Stichproben) MULTI-ALLELIC (ist diese Variante multiallelisch? wahr/falsch)

FORMAT-Felder/Felder auf der Ebene der Stichprobe:

Verwenden Sie das Argument `-GF`, um FORMAT-Felder/Felder auf der Ebene der Stichprobe zu extrahieren. Das Tool erstellt pro Stichprobe eine neue Spalte mit dem Namen „SAMPLE_NAME. FORMAT_FIELD_NAME“ z. B. NA12877.GQ, NA12878.GQ.

Eingabe:

Eine VCF-Datei, die in eine Tabelle konvertiert werden soll

Ausgabe:

Eine Datei mit Tabstopptrennzeichen, die die Werte der angeforderten Felder in der VCF-Datei enthält.

```python
run gatk VariantsToTable -V NA12877.vcf.gz -F CHROM -F POS -F TYPE -F AC -F AD -F AF -GF DP -GF AD -O outputtable.table
```

## <a name="references"></a>Referenzen

1. VariantFiltration: https://gatk.broadinstitute.org/hc/en-us/articles/360036827111-VariantFiltration 
2. Auswählen von Varianten: https://gatk.broadinstitute.org/hc/en-us/articles/360037052272-SelectVariants
3. Übereinstimmung: https://gatk.broadinstitute.org/hc/en-us/articles/360041851651-Concordance
4. Varianten in Tabelle: https://gatk.broadinstitute.org/hc/en-us/articles/360036882811-VariantsToTable 
5. Illumina Platinum Genomes: https://www.illumina.com/platinumgenomes.html 

<!-- nbend -->

---

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.