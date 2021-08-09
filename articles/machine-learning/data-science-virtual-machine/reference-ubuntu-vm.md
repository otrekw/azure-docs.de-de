---
title: 'Referenz: Data Science Virtual Machine für Ubuntu'
titleSuffix: Azure Data Science Virtual Machine
description: Details zu den Tools, die in der Data Science Virtual Machine für Ubuntu enthalten sind
author: timoklimmer
services: machine-learning
ms.service: data-science-vm
ms.custom: devx-track-python
ms.author: tklimmer
ms.date: 05/12/2021
ms.topic: reference
ms.openlocfilehash: d2a960b2a924d06b7ecc2a2f613fdd5a5695071a
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110076382"
---
# <a name="reference-ubuntu-linux-data-science-virtual-machine"></a>Referenz: Data Science Virtual Machine für Ubuntu (Linux)

Nachstehend finden Sie eine Liste der Tools, die auf Ihrer Data Science Virtual Machine für Ubuntu verfügbar sind. 

## <a name="deep-learning-libraries"></a>Deep Learning-Bibliotheken

### <a name="pytorch"></a>PyTorch

[PyTorch](https://pytorch.org/) ist ein beliebtes Framework für wissenschaftliche Berechnungen mit weit reichender Unterstützung für Machine Learning-Algorithmen. Wenn ihr Computer über eine integrierte GPU verfügt, kann er diese GPU nutzen, um das Deep Learning zu beschleunigen. PyTorch ist in der `py38_pytorch`-Umgebung verfügbar.

### <a name="h2o"></a>H2O

H2O ist eine schnelle, verteilte In-Memory-Plattform für Machine Learning und Predictive Analytics. Ein Python-Paket ist sowohl in der Stammumgebung als auch der py35 Anaconda-Umgebung installiert. Ein R-Paket ist ebenfalls installiert. 

Führen Sie `java -jar /dsvm/tools/h2o/current/h2o.jar` aus, um H2O über die Befehlszeile zu öffnen. Es gibt verschiedene [Befehlszeilenoptionen](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/starting-h2o.html#from-the-command-line), die Sie bei Bedarf konfigurieren können. Sie können zunächst auf die Flow-Webbenutzeroberfläche zugreifen, indem Sie zu `http://localhost:54321` navigieren. Beispiel-Notebooks stehen auch in JupyterHub zur Verfügung.

### <a name="tensorflow"></a>TensorFlow

[TensorFlow](https://tensorflow.org) ist die Deep Learning-Bibliothek von Google. Es handelt sich dabei um eine Open-Source-Softwarebibliothek für numerische Berechnungen unter Verwendung von Datenflussdiagrammen. Wenn Ihr Computer über eine integrierte GPU verfügt, kann er diese verwenden, um das Deep Learning zu beschleunigen. TensorFlow ist in der Conda-Umgebung `py38_tensorflow` verfügbar.


## <a name="python"></a>Python

Die DSVM verfügt über mehrere vorinstallierte Python-Umgebungen, wobei die Python-Version entweder Python 3.8 oder Python 3.6 ist.
Um die vollständige Liste der installierten Umgebungen anzuzeigen, führen Sie `conda env list` über die Befehlszeile aus.


## <a name="jupyter"></a>Jupyter

Die DSVM wird auch mit Jupyter ausgeliefert, einer Umgebung zum Freigeben von Code und Analysen. Jupyter wird auf der DSVM in verschiedenen Varianten installiert:
 - Jupyter Lab
 - Jupyter Notebook
 - Jupyter Hub

Öffnen Sie Jupyter über das Anwendungsmenü, oder klicken Sie auf das Desktopsymbol, um Jupyter Lab zu öffnen. Alternativ können Sie Jupyter Lab öffnen, indem Sie `jupyter lab` über eine Befehlszeile ausführen.

Öffnen Sie eine Befehlszeile, und führen Sie `jupyter notebook` aus, um ein Jupyter Notebook zu öffnen.

Öffnen Sie **https://\<VM DNS name or IP address\>:8000/** , um Jupyter Hub zu öffnen. Anschließend werden Sie zur Angabe Ihres lokalen Linux-Benutzernamens und -Kennworts aufgefordert.

> [!NOTE]
> Fahren Sie fort, falls Sie Zertifikatwarnungen erhalten.

> [!NOTE]
> Für die Ubuntu-Images wird Port 8000 in der Firewall standardmäßig geöffnet, wenn der virtuelle Computer bereitgestellt wird.


## <a name="apache-spark-standalone"></a>Apache Spark – eigenständige Instanz

Eine eigenständige Instanz von Apache Spark ist auf der Linux-DSVM vorinstalliert, damit Sie Spark-Anwendungen zunächst lokal entwickeln können, bevor Sie sie in großen Clustern testen und bereitstellen. 

Sie können PySpark-Programme über den Jupyter-Kernel ausführen. Wenn Sie Jupyter öffnen und die Schaltfläche **New** (Neu) auswählen, wird eine Liste mit verfügbaren Kernels angezeigt. **Spark – Python** ist der PySpark-Kernel, mit dem Sie Spark-Anwendungen in der Sprache Python erstellen können. Sie können auch eine Python-IDE wie VS.Code oder PyCharm verwenden, um Spark-Programme zu erstellen. 

In dieser eigenständigen Instanz wird der Spark-Stapel im aufrufenden Clientprogramm ausgeführt. Dieses Feature führt dazu, dass sich Probleme schneller und einfacher als beim Entwickeln in einem Spark-Cluster beheben lassen.


## <a name="ides-and-editors"></a>IDEs und Editoren

Sie haben die Wahl zwischen mehreren Code-Editoren, z. B. VS.Code, PyCharm, RStudio, IntelliJ, vi/Vim, Emacs. 

VS.Code, PyCharm, RStudio und IntelliJ sind grafische Editoren. Sie müssen bei einem grafischen Desktop angemeldet sein, um sie verwenden zu können. Verwenden Sie zum Öffnen die Desktop- und Anwendungsmenüverknüpfungen.

Vim und Emacs sind textbasierte Editoren. Bei Emacs erleichtert das ESS-Add-On-Paket die Arbeit mit R im Emacs-Editor. Weitere Informationen finden Sie auf der [ESS-Website](https://ess.r-project.org/).


## <a name="databases"></a>Datenbanken

### <a name="graphical-sql-client"></a>Grafischer SQL-Client

SQuirreL SQL, ein grafischer SQL-Client, kann eine Verbindung mit mehreren Datenbanken herstellen (z. B. Microsoft SQL Server und MySQL) und SQL-Abfragen ausführen. Die schnellste Möglichkeit zum Öffnen von SQuirrel SQL ist die Verwendung des Anwendungsmenüs aus einer grafischen Desktopsitzung (z. B. über den X2Go-Client).

Vor der ersten Verwendung müssen Sie Ihre Treiber und Datenbankaliase einrichten. Die JDBC-Treiber befinden sich unter „/usr/share/Java/jdbcdrivers“.

Weitere Informationen finden Sie unter [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Befehlszeilentools für den Zugriff auf Microsoft SQL Server

Das ODBC-Treiberpaket für SQL Server verfügt auch über zwei Befehlszeilentools:

- **bcp**: Mit dem Tool bcp werden Daten per Massenkopiervorgang zwischen einer Instanz von Microsoft SQL Server und einer Datendatei in einem vom Benutzer angegebenen Format kopiert. Das Tool bcp kann zum Importieren großer Mengen von neuen Zeilen in SQL Server-Tabellen oder zum Exportieren von Daten aus Tabellen in Datendateien verwendet werden. Zum Importieren von Daten in eine Tabelle müssen Sie eine für diese Tabelle erstellte Formatdatei verwenden. Alternativ müssen Sie mit der Struktur der Tabelle und den Datentypen, die für ihre Spalten gelten, vertraut sein.

Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit bcp](/sql/connect/odbc/linux-mac/connecting-with-bcp).

- **sqlcmd**: Sie können Transact-SQL-Anweisungen mit dem Tool sqlcmd eingeben. Sie haben auch die Möglichkeit, Systemprozeduren und Skriptdateien an der Eingabeaufforderung einzugeben. Bei diesem Tool wird ODBC genutzt, um Transact-SQL-Batches auszuführen.

  Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit sqlcmd](/sql/connect/odbc/linux-mac/connecting-with-sqlcmd).

  > [!NOTE]
  > Für dieses Tool gelten einige Unterschiede zwischen Linux- und Windows-Plattformen. Ausführliche Informationen dazu finden Sie in der -Dokumentation.

### <a name="database-access-libraries"></a>Bibliotheken für den Datenbankzugriff

Bibliotheken für den Datenbankzugriff sind in R und Python verfügbar:

* In R können Sie mit den Paketen RODBC oder dplyr SQL-Anweisungen auf dem Datenbankserver abfragen oder ausführen.
* In Python ermöglicht die Bibliothek pyodbc den Datenbankzugriff mit ODBC als zugrunde liegender Schicht.  

## <a name="azure-tools"></a>Azure-Tools

Die folgenden Azure-Tools werden auf dem virtuellen Computer installiert:

* **Azure CLI**: Mit der Befehlszeilenschnittstelle in Azure können Sie Azure-Ressourcen über Shellbefehle erstellen und verwalten. Geben Sie **azure help** ein, um die Azure-Tools zu öffnen. Weitere Informationen finden Sie auf der [Dokumentationsseite zur Azure-Befehlszeilenschnittstelle](/cli/azure/get-started-with-az-cli2).
* **Azure Storage-Explorer**: Azure Storage-Explorer ist ein grafisches Tool zum Navigieren durch die Objekte, die Sie in Ihrem Azure-Speicherkonto gespeichert haben, sowie zum Hoch- und Herunterladen von Daten in und aus Azure-Blobs. Sie können über das Symbol der Desktopverknüpfung auf den Storage-Explorer zugreifen. Sie können ihn auch über eine Eingabeaufforderung der Shell öffnen, indem Sie **StorageExplorer** eingeben. Sie müssen über einen X2Go-Client angemeldet sein oder die X11-Weiterleitung eingerichtet haben.
* **Azure-Bibliotheken**: Im Folgenden finden Sie einige der vorinstallierten Bibliotheken.
  
  * **Python**: Die zu Azure gehörenden Bibliotheken in Python sind *azure*, *azureml*, *pydocumentdb* und *pyodbc*. Mit den ersten drei Bibliotheken können Sie auf Azure-Speicherdienste, Azure Machine Learning und Azure Cosmos DB (eine NoSQL-Datenbank in Azure) zugreifen. Mit der vierten Bibliothek, pyodbc (zusammen mit dem Microsoft ODBC-Treiber für SQL Server), können Sie unter Verwendung einer ODBC-Schnittstelle über Python auf SQL Server, Azure SQL-Datenbank und Azure Synapse Analytics zugreifen. Geben Sie **pip list** ein, um alle aufgeführten Bibliotheken anzuzeigen. Achten Sie darauf, dass dieser Befehl sowohl in der Python 2.7- als auch in der Python 3.5-Umgebung ausgeführt wird.
  * **R:** Die zu Azure gehörenden Bibliotheken in R sind AzureML und RODBC.
  * **Java**: Sie finden die Liste mit den Azure Java-Bibliotheken im Verzeichnis „/dsvm/sdk/AzureSDKJava“ auf der VM. Die wichtigsten Bibliotheken sind Azure-Speicher- und -Verwaltungs-APIs, Azure Cosmos DB und JDBC-Treiber für SQL Server.  

## <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning ist ein vollständig verwalteter Clouddienst, mit dem Sie Predictive Analytics-Lösungen erstellen, bereitstellen und freigeben können. Sie können Ihre Experimente und Modelle in Azure Machine Learning Studio (Vorschau) erstellen. Sie können mit einem Webbrowser auf der Data Science Virtual Machine darauf zugreifen, indem Sie [Microsoft Azure Machine Learning](https://ml.azure.com) nutzen.

Nachdem Sie sich bei Azure Machine Learning Studio angemeldet haben, haben Sie Zugriff auf einen Zeichenbereich zum Experimentieren, in dem Sie einen logischen Ablauf für die Machine Learning-Algorithmen erstellen können. Außerdem haben Sie Zugriff auf ein Jupyter Notebook, das in Azure Machine Learning gehostet wird, und können nahtlos mit den Experimenten in Azure Machine Learning Studio arbeiten. 

Operationalisieren Sie die von Ihnen erstellten Machine Learning-Modelle, indem Sie sie mit einer Webdienst-Schnittstelle umschließen. Durch das Operationalisieren von Machine Learning-Modellen können Clients, die in einer beliebigen Sprache geschrieben sind, Vorhersagen aus diesen Modellen aufrufen. Weitere Informationen finden Sie in der [Dokumentation zu Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).

Sie können Ihre Modelle auch in R oder Python auf der VM erstellen und diese dann in der Produktion in Azure Machine Learning bereitstellen. Wir haben Bibliotheken in R (**AzureML**) und Python (**azureml**) installiert, um diese Funktionalität zu ermöglichen.

> [!NOTE]
> Diese Anweisungen wurden für die Windows-Version von Data Science Virtual Machine geschrieben. Die Informationen zum Bereitstellen von Modellen für Azure Machine Learning gelten allerdings für die Linux-VM.

## <a name="machine-learning-tools"></a>Machine Learning-Tools

Die VM enthält Machine Learning-Tools und -Algorithmen, die vorkompiliert und lokal vorinstalliert wurden. Dazu gehören:

* **Vowpal Wabbit**: Ein Algorithmus für schnelles Onlinelernen.
* **xgboost**: Ein Tool, das optimierte Boosted Tree-Algorithmen bereitstellt.
* **Rattle**: R-basiertes grafisches Tool zum einfachen Durchsuchen und Modellieren von Daten.
* **Python**: Anaconda Python wird als Paket mit Machine Learning-Algorithmen für Bibliotheken wie Scikit-learn bereitgestellt. Sie können andere Bibliotheken installieren, indem Sie den `pip install` -Befehl verwenden.
* **LightGBM**: Ein schnelles, verteiltes Gradient-Boosted-Hochleistungsframework auf der Grundlage von Entscheidungsstrukturalgorithmen.
* **R:** Für R ist eine umfassende Bibliothek mit Machine Learning-Funktionen verfügbar. Zu den vorinstallierten Bibliotheken zählen u. a. lm, glm, randomForest und rpart. Sie können andere Bibliotheken installieren, indem Sie diesen Befehl ausführen:

    ```r
    install.packages(<lib name>)
    ```

Im Folgenden finden Sie einige zusätzliche Informationen zu den ersten drei Machine Learning-Tools in der Liste.

### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit ist ein Machine Learning-System, das verschiedene Verfahren einsetzt, wie z.B. Online, Hashing, Allreduce, Reductions, Learning2Search, Active und Interactive Learning.

Verwenden Sie die folgenden Befehle, um das Tool für ein einfaches Beispiel auszuführen:

```bash
cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
cd vwdemo
vw house_dataset
```

In diesem Verzeichnis sind noch weitere, größere Demos enthalten. Informationen zu Vowpal Wabbit finden Sie in [diesem Abschnitt auf GitHub](https://github.com/JohnLangford/vowpal_wabbit) und im [Vowpal Wabbit-Wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

### <a name="xgboost"></a>Xgboost

Die xgboost-Bibliothek wurde für Boosted (Tree)-Algorithmen entworfen und optimiert. Das Ziel dieser Bibliothek besteht darin, die Rechenleistung von Computern erheblich zu verbessern, um Struktur-Boosting-Vorgänge in großem Umfang zu ermöglichen, die skalierbar, portabel und präzise sind.

Sie wird per Befehlszeile und als R-Bibliothek bereitgestellt. Um diese Bibliothek in R zu verwenden, können Sie eine interaktive R-Sitzung starten (indem Sie in der Shell **R** eingeben) und die Bibliothek laden.

Hier ist ein einfaches Beispiel angegeben, das Sie an einer R-Eingabeaufforderung ausführen können:

```R
library(xgboost)

data(agaricus.train, package='xgboost')
data(agaricus.test, package='xgboost')
train <- agaricus.train
test <- agaricus.test
bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
pred <- predict(bst, test$data)
```

Hier sind die Befehle für die Shell zum Ausführen der xgboost-Befehlszeile angegeben:

```bash
cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
cd xgboostdemo
xgboost mushroom.conf
```

Eine MODEL-Datei wird in das angegebene Verzeichnis geschrieben. Informationen zu diesem Demonstrationsbeispiel finden Sie [auf GitHub](https://github.com/dmlc/xgboost/tree/master/demo/CLI/binary_classification).

Weitere Informationen zu xgboost finden Sie auf der [xgboost-Dokumentationsseite](https://xgboost.readthedocs.org/en/latest/) und im zugehörigen [GitHub-Repository](https://github.com/dmlc/xgboost).

### <a name="rattle"></a>Rattle

Rattle (**R** **A** nalytical **T** ool **T** o **L** earn **E** asily) verwendet GUI-basierte Durchsuchungs- und Modellierungsvorgänge für Daten. Mit dem Tool werden statistische und visuelle Zusammenfassungen von Daten dargestellt, Daten transformiert, die leicht modelliert werden können, nicht überwachte und überwachte Modelle aus den Daten erstellt, Leistungsdaten von Modellen grafisch dargestellt und neue Datasets bewertet. Außerdem wird R-Code generiert, mit dem die Vorgänge in der UI repliziert werden, die direkt in R ausgeführt oder als Ausgangspunkt für weitere Analysen verwendet werden können.

Um Rattle ausführen zu können, müssen Sie in einer grafischen Desktopsitzung angemeldet sein. Geben Sie im Terminal **R** ein, um die R-Umgebung zu öffnen. Geben Sie an der R-Eingabeaufforderung die folgenden Befehle ein:

```R
library(rattle)
rattle()
```

Eine grafische Benutzeroberfläche mit einer Reihe von Registerkarten wird geöffnet. Verwenden Sie die folgenden Schnellstartschritte in Rattle, um ein Beispiel-Wetterdataset zu verwenden und ein Modell zu erstellen. In einigen Schritten werden Sie zum automatischen Installieren und Laden einiger erforderlicher R-Pakete aufgefordert, die sich nicht bereits im System befinden.

> [!NOTE]
> Falls Sie für die Installation des Pakets im Systemverzeichnis (Standardeinstellung) keinen Zugriff haben, wird unter Umständen in Ihrem R-Konsolenfenster eine Aufforderung mit der Frage angezeigt, ob Pakete in Ihrer persönlichen Bibliothek installiert werden sollen. Wählen Sie **y** , wenn diese Aufforderungen angezeigt werden.

1. Wählen Sie **Execute**(Ausführen).
1. Es wird ein Dialogfeld mit der Frage angezeigt, ob Sie das Beispiel-Wetterdataset verwenden möchten. Wählen Sie **Yes** (Ja) aus, um das Beispiel zu laden.
1. Wählen Sie die Registerkarte **Model** (Modell) aus.
1. Wählen Sie **Execute** (Ausführen) aus, um eine Entscheidungsstruktur zu erstellen.
1. Wählen Sie **Draw** (Zeichnen) aus, um die Entscheidungsstruktur anzuzeigen.
1. Wählen Sie die Option **Forest** (Gesamtstruktur) und **Execute** (Ausführen) aus, um eine zufällige Gesamtstruktur zu erstellen.
1. Wählen Sie die Registerkarte **Evaluate** (Auswerten) aus.
1. Wählen Sie die Option **Risk** (Risiko) und **Execute** (Ausführen) aus, um zwei Leistungsdarstellungen zu **Risk (Cumulative)** (Risiko (Kumulativ)) anzuzeigen.
1. Wählen Sie die Registerkarte **Log** (Protokoll) aus, um den generierten R-Code für die obigen Vorgänge anzuzeigen.
   (Aufgrund eines Fehlers in der aktuellen Version von Rattle müssen Sie im Text des Protokolls vor **Export this log** (Protokoll exportieren) das Zeichen **#** eingeben.)
1. Wählen Sie die Schaltfläche **Export** (Exportieren) aus, um die R-Skriptdatei *weather_script.R* im Basisordner zu speichern.

Sie können Rattle und R beenden. Nun können Sie das generierte R-Skript bearbeiten. Sie können das Skript auch unverändert verwenden und es jederzeit ausführen, um alle Aktionen auf der Rattle-Benutzeroberfläche zu wiederholen. Dies ist besonders für Einsteiger in R eine Möglichkeit, auf einer einfachen grafischen Benutzeroberfläche schnell Analysen und maschinelles Lernen durchzuführen, während in R automatisch Code zum Ändern bzw. Lernen generiert wird.

## <a name="next-steps"></a>Nächste Schritte

Haben Sie weitere Fragen? Erstellen Sie ggf. ein [Supportticket](https://azure.microsoft.com/support/create-ticket/).