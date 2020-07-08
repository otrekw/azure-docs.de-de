---
title: Lokales Testen eines Azure Stream Analytics-Auftrags mit Beispieldaten mithilfe von Visual Studio Code
description: In diesem Artikel wird beschrieben, wie Abfragen lokal mit Beispieldaten mithilfe von Azure Stream Analytics-Tools für Visual Studio Code getestet werden.
ms.service: stream-analytics
author: su-jie
ms.author: sujie
ms.date: 11/10/2019
ms.topic: how-to
ms.openlocfilehash: a47b641f4f5049beb605490122a33d407ac3e222
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86044361"
---
# <a name="test-stream-analytics-queries-locally-with-sample-data-using-visual-studio-code"></a>Lokales Testen von Stream Analytics-Abfragen mit Beispieldaten mithilfe von Visual Studio Code

Mithilfe von Azure Stream Analytics-Tools für Visual Studio Code können Sie Ihre Stream Analytics-Aufträge lokal mit Beispieldaten testen. Sie finden die Abfrageergebnisse in JSON-Dateien im Ordner **LocalRunOutputs** des Projekts.

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie [.NET Core SDK](https://dotnet.microsoft.com/download), und starten Sie Visual Studio Code neu.

* In diesem [Schnellstarttutorial](quick-create-vs-code.md) erfahren Sie, wie ein Stream Analytics-Auftrags mithilfe von Visual Studio Code erstellt wird.

## <a name="prepare-sample-data"></a>Vorbereiten der Beispieldaten

Bereiten Sie zuerst Dateien mit Beispieldaten für die Eingabe vor. Wenn auf Ihrem Computer bereits Dateien mit Beispieldaten vorhanden sind, können Sie diesen Schritt überspringen und mit dem nächsten Schritt fortfahren.

1. Klicken Sie in der Eingabekonfigurationsdatei in der Zeile oben auf **Vorschau der Daten**. Es werden einige Eingabedaten aus dem IoT Hub abgerufen und im Vorschaufenster angezeigt. Dieser Vorgang kann einige Zeit in Anspruch nehmen.

2. Sobald die Daten angezeigt werden, klicken Sie auf **Speichern unter**, um die Daten in einer lokalen Datei zu speichern.

 ![Vorschau der Liveeingabe](./media/quick-create-vs-code/preview-live-input.png)

## <a name="define-a-local-input"></a>Definieren einer lokalen Eingabe

1. Klicken Sie im Stream Analytics-Projekt im Ordner „Eingaben“ auf **input.json**. Wählen Sie dann aus der oberen Zeile **Lokale Eingabe hinzufügen** aus.

    ![Hinzufügen einer lokalen Eingabe aus Projekt](./media/quick-create-vs-code/add-input-from-project.png)

    Sie können auch **STRG+UMSCHALT+P** auswählen, um die Befehlspalette zu öffnen und **ASA einzugeben: Eingabe hinzufügen** ein.

   ![Hinzufügen von Stream Analytics-Eingaben in VS Code](./media/quick-create-vs-code/add-input.png)

2. Wählen Sie **Local Input** (Lokale Eingabe) aus.

    ![Hinzufügen von lokalen ASA-Eingaben in Visual Studio Code](./media/vscode-local-run/add-local-input.png)

3. Wählen Sie **+ New Local Input** (+ Neue lokale Eingabe) aus.

    ![Hinzufügen einer neuen lokalen ASA-Eingabe in Visual Studio Code](./media/vscode-local-run/add-new-local-input.png)

4. Geben Sie die gleichen Eingabealias ein, die Sie in Ihrer Abfrage verwendet haben.

    ![Hinzufügen eines neuen lokalen ASA-Eingabealias](./media/vscode-local-run/new-local-input-alias.png)

5. Geben Sie in der neu generierten Datei **LocalInput_Input.json** den Dateipfad ein, in dem sich Ihre lokale Datendatei befindet.

    ![Eingeben von lokalen Dateipfaden in Visual Studio](./media/vscode-local-run/local-file-path.png)

6. Wählen Sie **Datenvorschau** aus, um eine Vorschau der Eingabedaten anzuzeigen. Der Serialisierungstyp für Ihre Daten wird automatisch erkannt, wenn es sich um JSON oder CSV handelt. Verwenden Sie die Auswahl, um Ihre Daten im **Tabellen**- oder **Roh**format anzuzeigen. Die folgende Tabelle ist ein Beispiel für Daten im **Tabellenformat**:

     ![Vorschau lokaler Daten im Tabellenformat](./media/vscode-local-run/local-file-preview-table.png)

    Die folgende Tabelle ist ein Beispiel für Daten im **Rohformat**:

    ![Vorschau lokaler Daten im Rohformat](./media/vscode-local-run/local-file-preview-raw.png)

## <a name="run-queries-locally"></a>Lokales Ausführen von Abfragen

Gehen Sie zurück zum Abfrage-Editor, und wählen Sie **Lokal ausführen** aus. Wählen Sie dann in der Dropdownliste **Lokale Eingabe verwenden** aus.

![Auswählen von „Lokal ausführen“ im Abfrage-Editor](./media/vscode-local-run/run-locally.png)

![Verwenden von lokalen Eingaben](./media/vscode-local-run/run-locally-use-local-input.png)

Das Ergebnis wird im Fenster rechts dargestellt. Klicken Sie zum erneuten Testen auf **Ausführen**. Sie können auch **In Ordner öffnen** auswählen, um die Ergebnisdateien im Datei-Explorer anzuzeigen und Sie mit anderen Tools zu öffnen. Beachten Sie, dass die Ergebnisdateien nur im JSON-Format verfügbar sind.

![Anzeigen des Ergebnisses der lokalen Ausführung](./media/vscode-local-run/run-locally-result.png)

## <a name="next-steps"></a>Nächste Schritte

* [Lokales Testen von Azure Stream Analytics-Aufträgen mit Liveeingabe unter Verwendung von Visual Studio Code](visual-studio-code-local-run-live-input.md)

* [Erkunden von Azure Stream Analytics-Aufträgen mit Visual Studio Code (Vorschauversion)](visual-studio-code-explore-jobs.md)
