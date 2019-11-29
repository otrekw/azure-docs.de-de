---
title: Installieren von Paketen in einem Jupyter-Notebook auf Azure
description: Installieren von Python-, R- und F#-Paketen aus einem Jupyter-Notebook, das auf Azure ausgeführt wird.
ms.topic: article
ms.date: 12/04/2018
ms.openlocfilehash: 5d85c8e936ce7c8bf38ec7bc9c27d9066cc8b155
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277537"
---
# <a name="install-packages-from-within-a-notebook"></a>Installieren von Paketen aus einem Notebook

Zwar können Sie die [Umgebung für Ihr Notebook auf Projektebene konfigurieren](configure-manage-azure-notebooks-projects.md#configure-the-project-environment), es kann aber sinnvoll sein, Pakete direkt innerhalb eines einzelnen Notebooks zu installieren.

Aus dem Notebook installierte Pakete betreffen nur die aktuelle Serversitzung. Paketinstallationen bleiben beim Herunterfahren des Servers nicht erhalten.

## <a name="python"></a>Python

Pakete können in Python wahlweise mit Pip oder mit Conda mithilfe von Befehlen innerhalb von Codezellen installiert werden:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Wenn die Befehlsausgabe darauf schließen lässt, dass der Anforderung bereits entsprochen wird, enthält Azure Notebooks das fragliche Paket möglicherweise bereits standardmäßig. Möglicherweise wurde das Paket auch über einen [Einrichtungsschritt der Projektumgebung](configure-manage-azure-notebooks-projects.md#configure-the-project-environment) installiert.

## <a name="r"></a>R

Pakete können in R von CRAN oder GitHub mithilfe der Funktion `install.packages` in einer Codezelle installiert werden:

```r
install.packages("package_name")
```

Mithilfe der devtools-Bibliothek können auch Vorabversionen oder sonstige Entwicklungspakete von GitHub installiert werden:

```r
options(unzip = 'internal')
library(devtools)
install_github('<user>/<repo>')
```

## <a name="f"></a>F#

Pakete können in F# von [nuget.org](https://www.nuget.org) durch Aufrufen des Paket-Abhängigkeits-Managers aus Codezellen installiert werden. Laden Sie zunächst den Paket-Manager:

```fsharp
#load "Paket.fsx"
```

Installieren Sie anschließend Pakete:

```fsharp
Paket.Package
  [ "MathNet.Numerics"
    "MathNet.Numerics.FSharp"
  ]
```

Laden Sie dann den Paket-Generator:
```fsharp
#load "Paket.Generated.Refs.fsx"
```

Öffnen Sie die Bibliothek:
```fsharp
open MathNet.Numerics
```

## <a name="next-steps"></a>Nächste Schritte

- [Gewusst wie: Konfigurieren und Verwalten von Projekten](configure-manage-azure-notebooks-projects.md)
- [Gewusst wie: Zeigen einer Bildschirmpräsentation](present-jupyter-notebooks-slideshow.md)
