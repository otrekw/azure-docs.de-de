---
title: Installieren von Paketen in Jupyter-Notebooks – Azure Notebooks (Vorschau)
description: Erfahren Sie, wie Python-, R- und F#-Pakete aus einem Jupyter-Notebook installiert werden, das auf Azure ausgeführt wird.
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: 43d38c9f3a4b0095967ab3e103ea729ec86ea2bd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85831215"
---
# <a name="install-packages-from-within-azure-notebooks-preview"></a>Installieren von Paketen aus Azure Notebooks (Vorschau)

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

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

- [Vorgehensweise: Konfigurieren und Verwalten von Projekten](configure-manage-azure-notebooks-projects.md)
- [Vorgehensweise: Zeigen einer Bildschirmpräsentation](present-jupyter-notebooks-slideshow.md)
