---
title: 'Export Data (Daten exportieren): Modulreferenz'
titleSuffix: Azure Machine Learning
description: Verwenden Sie das Modul Export Data im Azure Machine Learning-Designer, um Ergebnisse und Zwischendaten außerhalb von Azure Machine Learning zu speichern.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/19/2021
ms.openlocfilehash: 90755aef66fa51084d83d036722187b61449a6fc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104656905"
---
# <a name="export-data-module"></a>Modul „Export Data“ (Daten exportieren)

In diesem Artikel wird ein Modul im Azure Machine Learning-Designer beschrieben.

Verwenden Sie dieses Modul, um Ergebnisse, Zwischendaten und Arbeitsdaten aus Ihren Pipelines in Cloudspeicherzielen zu speichern. 

Dieses Modul unterstützt das Exportieren von Daten in folgende Clouddatendiensten:

- Azure-Blobcontainer
- Azure-Dateifreigabe
- Azure Data Lake Storage Gen1
- Azure Data Lake Storage Gen2
- Azure SQL-Datenbank

Bevor Sie Ihre Daten exportieren können, müssen Sie einen Datenspeicher in Ihrem Azure Machine Learning-Arbeitsbereich registrieren. Weitere Informationen finden Sie unter [Zugreifen auf Daten in Azure Storage-Diensten](../how-to-access-data.md).

## <a name="how-to-configure-export-data"></a>Gewusst wie: Konfigurieren von „Export Data“

1. Fügen Sie das Modul **Export Data** zu Ihrer Pipeline im Designer hinzu. Sie finden dieses Modul in der Kategorie **Input and Output** (Eingabe und Ausgabe).

1. Verbinden Sie **Export Data** mit dem Modul, das die zu exportierenden Daten enthält.

1. Wählen Sie **Export Data** aus, um den Bereich **Properties** (Eigenschaften) zu öffnen.

1. Wählen Sie für **Datastore** (Datenspeicher) in der Dropdownliste einen vorhandenen Datenspeicher aus. Sie können auch einen neuen Datenspeicher erstellen. Wie dies funktioniert, erfahren Sie unter [Zugreifen auf Daten in Azure Storage-Diensten](../how-to-access-data.md).

    > [!NOTE]
    > Das Exportieren von Daten eines bestimmten Datentyps in eine SQL-Datenbankspalte, für die ein anderer Datentyp angegeben ist, wird nicht unterstützt. Die Zieltabelle muss nicht vorab existieren.

1. Das Kontrollkästchen **Ausgabe erneut generieren** legt fest, ob das Modul zum erneuten Generieren der Ausgabe zur Laufzeit ausgeführt werden soll. 

    Die Option ist standardmäßig deaktiviert. Dies bedeutet, dass das System die Ausgabe der letzten Ausführung wiederverwendet, um die Laufzeit zu verkürzen, wenn das Modul bereits mit denselben Parametern ausgeführt wurde. 

    Wenn sie ausgewählt ist, führt das System das Modul erneut aus, um die Ausgabe neu zu generieren.

1. Geben Sie den Pfad im Datenspeicher an, an dem sich die Daten befinden. Bei dem Pfad handelt es sich um einen relativen Pfad. Am Beispiel `data/testoutput` wird ersichtlich, dass die bei **Daten exportieren** eingegebenen Daten in `data/testoutput` in dem Datenspeicher gespeichert werden, den Sie bei den **Ausgabeeinstellungen** des Moduls festlegen.

    > [!NOTE]
    > Leere Pfade oder **URL-Pfade** sind nicht zulässig.


1. Wählen Sie für **File format** (Dateiformat) das Format aus, in dem die Daten gespeichert werden sollen.
 
1. Übermitteln Sie die Pipeline.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
