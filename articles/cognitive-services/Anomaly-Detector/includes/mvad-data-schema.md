---
title: MVAD-Datenschema
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: quying
manager: tonyxin
ms.service: cognitive-services
ms.topic: include
ms.date: 7/1/2021
ms.author: yingqunpku
ms.openlocfilehash: 3be9d5d50d34eee35de38b7b28dc1d42217ace24
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114292660"
---
### <a name="input-data-schema"></a>Eingabe des Datenschemas

MVAD erkennt Anomalien aus einer Gruppe von Metriken, und wir bezeichnen jede Metrik als **Variable** oder Zeitreihe.

* Sie können die Beispieldatendatei von Microsoft herunterladen, um das akzeptierte Schema zu überprüfen: [https://aka.ms/AnomalyDetector/MVADSampleData](https://aka.ms/AnomalyDetector/MVADSampleData)
* Jede Variable muss zwei und nur zwei Felder haben (`timestamp` und `value`) und sollte in einer CSV-Datei (durch Trennzeichen getrennte Werte) gespeichert werden.
* Die Spaltennamen der CSV-Datei sollten genau `timestamp` und `value` sein, und die Groß-/Kleinschreibung muss dabei beachtet werden.
* Die `timestamp`-Werte müssen ISO 8601 entsprechen. Für`value` können ganze Zahlen oder Dezimalzahlen mit einer beliebigen Anzahl von Dezimalstellen verwendet werden.
    Ein gutes Beispiel für den Inhalt einer CSV-Datei ist:

    |timestamp | value|
    |-------|-------|
    |2019-04-01T00:00:00Z| 5|
    |2019-04-01T00:01:00Z| 3.6|
    |2019-04-01T00:02:00Z| 4|
    |...| ...|

    > [!NOTE]
    > Wenn Ihre Zeitstempel Stunden, Minuten und/oder Sekunden enthalten, stellen Sie sicher, dass sie ordnungsgemäß aufgerundet werden, bevor Sie die APIs aufrufen.
    >
    > Wenn Ihre Datenfrequenz beispielsweise bei einem Datenpunkt aller 30 Sekunden liegt, Ihnen aber Zeitstempel wie „12:00:01“ und „12:00:28“ angezeigt werden, ist dies ein deutlicher Hinweis darauf, dass Sie die Zeitstempel vorab in neue Werte wie „12:00:00“ und „12:00:30“ einstellen sollten.
    >
    > Weitere Informationen finden Sie im Abschnitt [„Aufrunden von Zeitstempeln“](../concepts/best-practices-multivariate.md#timestamp-round-up) des Dokuments zu bewährten Methoden.
* Der Name der CSV-Datei wird als Variablenname verwendet und sollte eindeutig sein. Beispiel: „temperature.csv“ und „humidity.csv“.
* Variablen für Training und Variablen für Rückschlüsse sollten konsistent sein. Wenn Sie beispielsweise `series_1`, `series_2`, `series_3`, `series_4` und `series_5` für das Training verwenden, sollten Sie genau die gleichen Variablen für Rückschlüsse bereitstellen.
* CSV-Dateien sollten in eine ZIP-Datei komprimiert und in einen Azure-Blobcontainer hochgeladen werden. Die ZIP-Datei kann einen beliebigen Namen haben.

#### <a name="folder-structure"></a>Ordnerstruktur

Ein häufiger Fehler bei der Datenvorbereitung sind zusätzliche Ordner in der ZIP-Datei. Angenommen, der Name der ZIP-Datei lautet `series.zip`: Nach dem Dekomprimieren der Dateien in einen neuen Ordner `./series` lautet der **richtige** Pfad zu den CSV-Dateien `./series/series_1.csv`, und ein **falscher** Pfad könnte bspw. `./series/foo/bar/series_1.csv` sein.

Das richtige Beispiel für die Verzeichnisstruktur nach dem Dekomprimieren der ZIP-Datei in Windows

```bash
.
└── series
    ├── series_1.csv
    ├── series_2.csv
    ├── series_3.csv
    ├── series_4.csv
    └── series_5.csv
```

Ein falsches Beispiel für die Verzeichnisstruktur nach dem Dekomprimieren der ZIP-Datei in Windows

```bash
.
└── series
    └── series
        ├── series_1.csv
        ├── series_2.csv
        ├── series_3.csv
        ├── series_4.csv
        └── series_5.csv
```