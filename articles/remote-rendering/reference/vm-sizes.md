---
title: VM-Größen
description: Beschreibt die unterschiedlichen VM-Größen, die zugeordnet werden können
author: florianborn71
ms.author: flborn
ms.date: 05/28/2020
ms.topic: reference
ms.openlocfilehash: e8e439a055b71ed291573965c561ee31610e3ed4
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88121611"
---
# <a name="vm-sizes"></a>VM-Größen

Der Renderingdienst kann auf zwei verschiedenen Arten von Computern in Azure ausgeführt werden, die als `Standard` und `Premium` bezeichnet werden.

## <a name="polygon-limits"></a>Maximale Polygonzahl

Bei der `Standard`-VM-Größe gibt es eine feste Beschränkung auf **20 Millionen Polygone**. Bezüglich der `Premium`-Größe gibt es keinerlei Einschränkung.

Wenn der Renderer auf einer Standard-VM diesen Grenzwert erreicht, schaltet er das Rendering in einen Schachbretthintergrund um:

![Schachbrett](media/checkerboard.png)

## <a name="allocate-the-vm"></a>Zuordnen der VM

Der gewünschte VM-Typ muss bei der Initialisierung der Renderingsitzung angegeben werden. Er kann nicht während einer laufenden Sitzung geändert werden. Die folgenden Codebeispiele zeigen, wo die VM-Größe angegeben werden muss:

```cs
async void CreateRenderingSession(AzureFrontend frontend)
{
    RenderingSessionCreationParams sessionCreationParams = new RenderingSessionCreationParams();
    sessionCreationParams.Size = RenderingSessionVmSize.Standard; // or  RenderingSessionVmSize.Premium

    AzureSession session = await frontend.CreateNewRenderingSessionAsync(sessionCreationParams).AsTask();
}
```

```cpp
void CreateRenderingSession(ApiHandle<AzureFrontend> frontend)
{
    RenderingSessionCreationParams sessionCreationParams;
    sessionCreationParams.Size = RenderingSessionVmSize::Standard; // or  RenderingSessionVmSize::Premium

    if (auto createSessionAsync = frontend->CreateNewRenderingSessionAsync(sessionCreationParams))
    {
        // ...
    }
}
```

Für die [PowerShell-Beispielskripts](../samples/powershell-example-scripts.md) muss die VM-Größe in der Datei `arrconfig.json` angegeben werden:

```json
{
  "accountSettings": {
    ...
  },
  "renderingSessionSettings": {
    "vmSize": "<standard or premium>",
    ...
  },
```

### <a name="how-the-renderer-evaluates-the-number-of-polygons"></a>Auswerten der Anzahl der Polygone durch den Renderer

Die Anzahl von Polygonen, die für den Einschränkungstest berücksichtigt werden, entspricht der Anzahl der Polygone, die tatsächlich an den Renderer übermittelt werden. Diese Geometrie ist in der Regel die Summe aller instanziierten Modelle, es gibt jedoch auch Ausnahmen. Folgende Geometrie ist **nicht eingeschlossen**:
* Geladene Modellinstanzen, die vollständig außerhalb des Ansichtsfrustums liegen.
* Modelle oder Modellteile, die mithilfe der [Komponente der hierarchischen Zustandsüberschreibung](../overview/features/override-hierarchical-state.md) unsichtbar geschaltet werden.

Dementsprechend ist es möglich, eine Anwendung zu schreiben, die auf die `standard`-Größe abzielt und mehrere Modelle mit einer Polygonzahl nahe des Limits für das einzelne Modell lädt. Wenn die Anwendung jeweils nur ein Modell anzeigt, wird das Schachbrett nicht ausgelöst.

### <a name="how-to-determine-the-number-of-polygons"></a>Bestimmen der Anzahl der Polygone

Es gibt zwei Möglichkeiten, die Anzahl der Polygone eines Modells oder einer Szene zu ermitteln, die zum Budgetlimit der VM mit `standard`-Größe beitragen:
* Rufen Sie auf der Modellkonvertierungsseite die [JSON-Datei der Konvertierungsausgabe](../how-tos/conversion/get-information.md) ab, und überprüfen Sie den `numFaces`-Eintrag im Abschnitt [*inputStatistics*](../how-tos/conversion/get-information.md#the-inputstatistics-section) (Eingabestatistik).
* Wenn die Anwendung dynamische Inhalte verarbeitet, kann die Anzahl gerenderter Polygone während der Runtime dynamisch abgefragt werden. Verwenden Sie eine [Leistungsbewertungsabfrage](../overview/features/performance-queries.md#performance-assessment-queries), und suchen Sie in der `FrameStatistics`-Struktur nach dem Element `polygonsRendered`. Das Feld `polygonsRendered` wird auf `bad` festgelegt, wenn der Renderer den Grenzwert für Polygone erreicht. Der Schachbretthintergrund wird stets mit einiger Verzögerung ausgeblendet, um sicherzustellen, dass nach dieser asynchronen Abfrage eine Benutzeraktion durchgeführt werden kann. Eine Benutzeraktion kann beispielsweise Modellinstanzen ausblenden oder löschen.

## <a name="pricing"></a>Preise

Eine detaillierte Aufschlüsselung der Preise für die einzelnen VM-Typen finden Sie auf der Seite [Remote Rendering – Preise](https://azure.microsoft.com/pricing/details/remote-rendering).

## <a name="next-steps"></a>Nächste Schritte
* [PowerShell-Beispielskripts](../samples/powershell-example-scripts.md)
* [Modellkonvertierung](../how-tos/conversion/model-conversion.md)

