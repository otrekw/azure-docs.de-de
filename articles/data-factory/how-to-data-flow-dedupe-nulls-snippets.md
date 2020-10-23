---
title: Deduplizieren von Zeilen und Suchen nach NULL-Werten mit Datenfluss-Codeausschnitten
description: Es wird beschrieben, wie Sie problemlos Zeilen deduplizieren und nach NULL-Werten suchen, indem Sie Codeausschnitte in Datenflüssen verwenden.
services: data-factory
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/30/2020
ms.author: makromer
ms.openlocfilehash: 841484a647d2737d621c75ebe63f65f2de829a26
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91666499"
---
# <a name="dedupe-rows-and-find-nulls-using-data-flow-snippets"></a>Deduplizieren von Zeilen und Suchen nach NULL-Werten mit Datenfluss-Codeausschnitten

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Indem Sie Codeausschnitte in Zuordnungsdatenflüssen verwenden, können Sie häufige Aufgaben wie die Datendeduplizierung oder Filterung nach NULL-Werten leicht durchführen. In dieser Anleitung wird beschrieben, wie Sie diese Funktionen mit sehr wenig Aufwand Ihren Pipelines hinzufügen können, indem Sie Codeausschnitte von Datenflussskripts verwenden.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4GnhH]

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

1. Wählen Sie **+Neue Pipeline** aus, um eine neue Pipeline zu erstellen.

2. Fügen Sie eine Datenflussaktivität hinzu.

3. Fügen Sie eine Quelltransformation hinzu, und verbinden Sie sie mit einem Ihrer Datasets.

    ![Quellcodeausschnitt 2](media/data-flow/snippet-adf-2.png)

4. Für die Codeausschnitte für die Deduplizierung und die Überprüfung auf NULL-Werte werden generische Muster verwendet, für die Datenfluss-Schemaabweichungen genutzt werden. Sie funktionieren dann mit allen Schemas Ihres Datasets oder mit Datasets, die nicht über ein vordefiniertes Schema verfügen.

5. [Navigieren Sie zur entsprechenden Seite in der Datenflussskript-Dokumentation, und kopieren Sie den Codeausschnitt für „Distinct Rows“](https://docs.microsoft.com/azure/data-factory/data-flow-script#distinct-row-using-all-columns).

6. Klicken Sie auf der Benutzeroberfläche des Datenfluss-Designers oben rechts auf die Schaltfläche „Skript“, um den Skript-Editor hinter dem Datenflussgraphen zu öffnen.

    ![Quellcodeausschnitt 3](media/data-flow/snippet-adf-3.png)

7. Drücken Sie nach der Definition für ```source1``` in Ihrem Skript die EINGABETASTE, und fügen Sie den Codeausschnitt ein.

8. Sie verbinden diesen eingefügten Codeausschnitt mit der vorherigen Quelltransformation, die Sie im Graphen erstellt haben, indem Sie vor dem eingefügten Code „source1“ eingeben.

9. Alternativ können Sie für die neue Transformation im Designer eine Verbindung herstellen, indem Sie über den neuen Transformationsknoten im Graphen den eingehenden Datenstrom auswählen.

    ![Quellcodeausschnitt 4](media/data-flow/snippet-adf-4.png)

10. In Ihrem Datenfluss werden nun doppelte Zeilen aus Ihrer Quelle entfernt, indem die Transformation für das Aggregieren verwendet wird. Hiermit wird nach allen Zeilen gruppiert, indem übergreifend für alle Spaltenwerte ein allgemeiner Hash genutzt wird.
    
11. Als Nächstes fügen wir einen Codeausschnitt zum Aufteilen Ihrer Daten in einen Datenstrom mit Zeilen mit NULL-Werten und einen Datenstrom ohne NULL-Werte hinzu.

12. [Navigieren Sie zurück zur Bibliothek mit den Codeausschnitten, und kopieren Sie nun den Code für die Überprüfung auf NULL-Werte](https://docs.microsoft.com/azure/data-factory/data-flow-script#check-for-nulls-in-all-columns).

13. Klicken Sie in Ihrem Datenfluss-Designer erneut auf „Skript“, und fügen Sie diesen neuen Transformationscode unten ein. Stellen Sie die Verbindung mit Ihrer vorherigen Transformation her, indem Sie den Namen dieser Transformation vor dem eingefügten Codeausschnitt eingeben.

14. Ihr Datenflussgraph sollte jetzt in etwa wie folgt aussehen:

    ![Quellcodeausschnitt 1](media/data-flow/snippet-adf-1.png)

  Sie verfügen nun über einen funktionierenden Datenfluss mit generischer Deduplizierung und einer Überprüfung auf NULL-Werte, indem Sie vorhandene Codeausschnitte aus der Datenflussskript-Bibliothek in Ihren vorhandenen Entwurf eingefügt haben.

## <a name="next-steps"></a>Nächste Schritte

* Erstellen Sie die restliche Datenflusslogik mithilfe von Mapping Data Flow-[Transformationen](concepts-data-flow-overview.md).
