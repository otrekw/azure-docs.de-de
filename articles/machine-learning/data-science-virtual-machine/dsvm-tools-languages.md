---
title: Unterstützte Sprachen
titleSuffix: Azure Data Science Virtual Machine
description: Die unterstützten Programmsprachen und zugehörigen Tools, die auf der Data Science Virtual Machine vorinstalliert sind.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
ms.service: data-science-vm
ms.custom: devx-track-python
author: timoklimmer
ms.author: tklimmer
ms.topic: conceptual
ms.date: 05/12/2021
ms.openlocfilehash: fcb2a4d9860687fb18409666f81839002329d54e
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110070896"
---
# <a name="languages-supported-on-the-data-science-virtual-machine"></a>Unterstützte Sprachen der Data Science-VM 

Die Data Science Virtual Machine (DSVM) verfügt über mehrere vorgefertigte Sprachen und Entwicklungstools für die Erstellung von Anwendungen, die künstliche Intelligenz (KI) nutzen. In Folgenden werden einige der wichtigsten aufgeführt.

## <a name="python"></a>Python

| Category | Wert |
|--|--|
| Unterstützte Sprachversionen | Python 3.8 |
| Unterstützte DSVM-Editionen | Windows Server 2019, Ubuntu 18.04 |
| Konfiguration/Installation auf der DSVM | Es gibt mehrere `conda`-Umgebungen, in denen für jede dieser Umgebungen unterschiedliche Python-Pakete vorinstalliert sind. Führen Sie `conda env list` aus, um alle verfügbaren Umgebungen auf Ihrem Computer aufzulisten. |

### <a name="how-to-use-and-run-it"></a>Verwenden und Ausführen

* Geben Sie an der Eingabeaufforderung Folgendes ein:

  Öffnen Sie eine Eingabeaufforderung, und verwenden Sie abhängig von Python-Version, die ausgeführt werden soll, eine der folgenden Methoden:

    ```
    conda activate <conda_environment_name>
    python --version
    ```
    
* Verwenden in einer IDE:

  Auf den DSVM-Images sind mehrere integrierte Entwicklungsumgebungen (IDE) installiert, z. B. VS.Code oder PyCharm. Sie können sie zum Bearbeiten, Ausführen und Debuggen Ihrer Python-Skripts verwenden.

* Verwendung in Jupyter Lab:

  Öffnen Sie eine Launcher-Registerkarte in Jupyter Lab, und wählen Sie den Typ und Kernel Ihres neuen Dokuments aus. Wenn Ihr Dokument in einem bestimmten Ordner abgelegt werden soll, navigieren Sie zuerst im Dateibrowser auf der linken Seite zu diesem Ordner.

* Installieren von Python-Paketen:

  Zum Installieren eines neuen Pakets müssen Sie zunächst die richtige Umgebung aktivieren. Die Umgebung ist der Ort, an dem Ihr neues Paket installiert wird, und das Paket ist dann nur in dieser Umgebung verfügbar.

  Führen Sie `conda activate <environment_name>` aus, um eine Umgebung zu aktivieren. Nachdem die Umgebung aktiviert wurde, können Sie einen Paket-Manager wie `conda` oder `pip` verwenden, um ein Paket zu installieren oder zu aktualisieren.

  Wenn Sie Jupyter verwenden, können Sie alternativ Pakete auch direkt installieren, indem Sie `!pip install --upgrade <package_name>` in einer Zelle ausführen.

## <a name="r"></a>R

| Category | Wert |
|--|--|
| Unterstützte Sprachversionen | CRAN R 4.0.5 |
| Unterstützte DSVM-Editionen | Linux, Windows |

### <a name="how-to-use-and-run-it"></a>Verwenden und Ausführen

* Geben Sie an der Eingabeaufforderung Folgendes ein:

  Öffnen Sie eine Eingabeaufforderung, und geben Sie `R` ein.

* Verwenden in einer IDE:

  Zum Bearbeiten von R-Skripts in einer IDE können Sie RStudio verwenden, das standardmäßig auf den DSVM-Images installiert ist.

* Verwendung in Jupyter Lab

  Öffnen Sie eine Launcher-Registerkarte in Jupyter Lab, und wählen Sie den Typ und Kernel Ihres neuen Dokuments aus. Wenn Ihr Dokument in einem bestimmten Ordner abgelegt werden soll, navigieren Sie zuerst im Dateibrowser auf der linken Seite zu diesem Ordner.

* Installieren von R-Paketen:

  Sie können neue R-Pakete entweder mithilfe der `install.packages()`-Funktion oder mithilfe von RStudio installieren.

## <a name="julia"></a>Julia

| Category | Wert |
| ------------- | ------------- |
| Unterstützte Sprachversionen | 1.0.5 |
| Unterstützte DSVM-Editionen      | Linux, Windows     |


### <a name="how-to-use-and-run-it"></a>Verwenden und Ausführen    

* Ausführen an einer Eingabeaufforderung

  Öffnen Sie eine Eingabeaufforderung, und führen Sie `julia` aus.

* Verwenden in Jupyter:

  Öffnen Sie eine Launcher-Registerkarte in Jupyter, und wählen Sie den Typ und Kernel Ihres neuen Dokuments aus. Wenn Ihr Dokument in einem bestimmten Ordner abgelegt werden soll, navigieren Sie zuerst im Dateibrowser auf der linken Seite zu diesem Ordner.

* Installieren von Julia-Paketen:

  Sie können den Julia-Paket-Manager mit Befehlen wie `Pkg.add()` verwenden, um Pakete zu installieren oder zu aktualisieren.


## <a name="other-languages"></a>Andere Sprachen

**C#** : Verfügbar unter Windows, wobei Zugriff darauf über die Visual Studio Community Edition oder an der `Developer Command Prompt for Visual Studio` möglich ist, wo Sie den Befehl `csc` ausführen können.

**Java**: OpenJDK ist sowohl in der Linux- als auch in der Windows-Edition der DSVM verfügbar und ist im Pfad festgelegt. Wenn Sie Java verwenden möchten, geben Sie den Befehl `javac` oder `java` an einer Eingabeaufforderung in Windows oder in der bash-Shell in Linux ein.

**Node.js**: Node.js ist sowohl in der Linux- als auch in der Windows-Edition der DSVM verfügbar und ist im Pfad festgelegt. Wenn Sie auf Node.js zugreifen möchten, geben Sie den Befehl `node` oder `npm` an einer Eingabeaufforderung in Windows oder in der bash-Shell in Linux ein. Unter Windows ist die Visual Studio-Erweiterung für die Node.js-Tools installiert, um eine grafische IDE bereitzustellen, mit der Sie Ihre Node.js-Anwendung entwickeln können.

**F#** : Verfügbar unter Windows, wobei Zugriff darauf über die Visual Studio Community Edition oder an einer `Developer Command Prompt for Visual Studio` möglich ist, wo Sie den Befehl `fsc` ausführen können.
