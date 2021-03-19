---
title: 'Azure-Schnellstart: Ausführen Ihres ersten Batch-Auftrags im Azure-Portal'
description: In dieser Schnellstartanleitung wird gezeigt, wie Sie das Azure-Portal verwenden, um ein Batch-Konto, einen Pool von Computeknoten und einen Auftrag zu erstellen, der grundlegende Aufgaben im Pool ausführt.
ms.topic: quickstart
ms.date: 08/17/2020
ms.custom: mvc
ms.openlocfilehash: 1234a932a732cdb6fda1c412a423ae0b1ea089e9
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102184014"
---
# <a name="quickstart-run-your-first-batch-job-in-the-azure-portal"></a>Schnellstart: Ausführen Ihres ersten Batch-Auftrags im Azure-Portal

Führen Sie erste Schritte mit Azure Batch im Azure-Portal aus, um ein Batch-Konto, einen Pool mit Computeknoten (virtuelle Computer) und einen Auftrag zu erstellen, mit dem Aufgaben im Pool ausgeführt werden. Nach Abschluss dieser Schnellstartanleitung sind Sie mit den wichtigsten Konzepten des Batch-Diensts vertraut und können Batch mit realistischeren Workloads und in größerem Umfang ausprobieren.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-batch-account"></a>Erstellen eines Batch-Kontos

Führen Sie diese Schritte aus, um ein Batch-Beispielkonto für Testzwecke zu erstellen. Zum Erstellen von Pools und Aufträgen benötigen Sie ein Batch-Konto. Wie hier gezeigt, können Sie ein Azure-Speicherkonto mit dem Batch-Konto verknüpfen. Auch wenn es für diese Schnellstartanleitung nicht erforderlich ist, ist das Speicherkonto nützlich zum Bereitstellen von Anwendungen und Speichern von Eingabe- und Ausgabedaten für die meisten Workloads in der Praxis.

1. Wählen Sie [im Azure-Portal](https://portal.azure.com) die Optionen **Ressource erstellen** > **Compute** > **Batch-Dienst** aus. 

   :::image type="content" source="media/quick-create-portal/marketplace-batch.png" alt-text="Screenshot: Batch-Dienst in Azure Marketplace":::

1. Wählen Sie im Feld **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein.

1. Geben Sie einen Wert für **Kontoname** ein. Dieser Name muss innerhalb des ausgewählten Azure-**Standorts** eindeutig sein. Er darf nur Kleinbuchstaben und Ziffern enthalten und muss 3 bis 24 Zeichen lang sein.

1. Wählen Sie unter **Speicherkonto** ein vorhandenes Speicherkonto aus, oder erstellen Sie ein neues.

1. Ändern Sie keine anderen Einstellungen. Wählen Sie **Überprüfen und erstellen** und dann **Erstellen** aus, um das Batch-Konto zu erstellen.

Navigieren Sie zu dem erstellten Batch-Konto, wenn die Meldung **Bereitstellung erfolgreich** angezeigt wird.

## <a name="create-a-pool-of-compute-nodes"></a>Erstellen eines Pools mit Computeknoten

Nachdem Sie nun über ein Batch-Konto verfügen, können Sie einen Beispielpool mit Windows-Computeknoten für Testzwecke erstellen. Der Pool für dieses kurze Beispiel umfasst zwei Knoten, auf denen ein Windows Server 2019-Image aus dem Azure Marketplace ausgeführt wird.

1. Klicken Sie im Batch-Konto auf **Pools** > **Hinzufügen**.

1. Geben Sie eine **Pool-ID** mit dem Namen *mypool* ein.

1. Wählen Sie unter **Betriebssystem** die folgenden Einstellungen. (Sie können auch weitere Optionen ausprobieren.)
  
   |Einstellung  |Wert  |
   |---------|---------|
   |**Imagetyp**|Marketplace|
   |**Publisher**     |microsoftwindowsserver|
   |**Angebot**     |windowsserver|
   |**sku**     |2019-datacenter-core-smalldisk|

1. Scrollen Sie nach unten, um Werte für die Einstellungen **Knotengröße** und **Skalieren** einzugeben. Die vorgeschlagene Knotengröße bietet für dieses kurze Beispiel eine gute Balance zwischen Leistung und Kosten.
  
   |Einstellung  |Wert  |
   |---------|---------|
   |**Knotentarif**     |Standard A1|
   |**Ziel für dedizierte Knoten**     |2|

1. Behalten Sie die Standardwerte für die übrigen Einstellungen bei, und klicken Sie auf **OK**, um den Pool zu erstellen.

Batch erstellt den Pool sofort, aber es dauert einige Minuten, bis die Computeknoten zugeordnet und gestartet wurden. Während dieses Zeitraums lautet der **Zuweisungsstatus** des Pools **Größe ändern**. Während für den Pool die Größenänderung durchgeführt wird, können Sie einen Auftrag und Aufgaben erstellen.

Nach einigen Minuten ändert sich der Zuordnungsstatus in **Bereit**, und die Knoten werden gestartet. Wählen Sie zum Überprüfen des Knotenstatus den Pool und dann **Knoten** aus. Wenn der Status eines Knotens **Leerlauf** lautet, ist er zum Ausführen von Aufgaben bereit.

## <a name="create-a-job"></a>Erstellen eines Auftrags

Da Sie jetzt über einen Pool verfügen, können Sie einen Auftrag erstellen, um ihn darin auszuführen. Ein Batchauftrag ist eine logische Gruppe für einen oder mehrere Tasks. Ein Auftrag enthält gemeinsame Einstellungen für Aufgaben, z.B. die Priorität und den Pool zum Ausführen von Aufgaben. Der Auftrag enthält ursprünglich keine Aufgaben.

1. Klicken Sie in der Batch-Kontoansicht auf **Aufträge** > **Hinzufügen**.

1. Geben Sie eine **Auftrags-ID** mit dem Namen *myjob* ein. Wählen Sie unter **Pool** die Option *mypool*. Behalten Sie die Standardwerte für die übrigen Einstellungen bei, und klicken Sie auf **OK**.

## <a name="create-tasks"></a>Erstellen von Tasks

Wählen Sie nun den Auftrag aus, um die Seite **Aufgaben** zu öffnen. Auf dieser Seite erstellen Sie Beispielaufgaben für die Ausführung im Auftrag. Normalerweise erstellen Sie mehrere Aufgaben, die von Batch zur Ausführung auf Computeknoten in die Warteschlange eingereiht und verteilt werden. In diesem Beispiel erstellen Sie zwei identische Aufgaben. Für jede Aufgabe wird eine Befehlszeile zum Anzeigen der Batch-Umgebungsvariablen auf einem Computeknoten ausgeführt, und anschließend wird 90 Sekunden gewartet.

Bei Verwendung von Batch befindet sich die Befehlszeile dort, wo Sie Ihre App bzw. Ihr Skript angeben. In Batch gibt es mehrere Möglichkeiten, Apps und Skripts auf Computeknoten bereitzustellen.

Gehen Sie wie folgt vor, um die erste Aufgabe zu erstellen:

1. Wählen Sie **Hinzufügen**.

1. Geben Sie *mytask* als **Task-ID** ein.

1. Geben Sie in der **Befehlszeile** die Zeichenfolge `cmd /c "set AZ_BATCH & timeout /t 90 > NUL"` ein. Behalten Sie die Standardwerte für die übrigen Einstellungen bei, und wählen Sie **Übermitteln** aus.

Nachdem Sie eine Aufgabe erstellt haben, wird sie von Batch zur Ausführung im Pool in die Warteschlange eingereiht. Wenn ein Knoten für die Ausführung verfügbar ist, wird die Aufgabe ausgeführt.

Wiederholen Sie die obigen Schritte, um eine zweite Aufgabe zu erstellen. Geben Sie eine andere **Task-ID** ein, aber verwenden Sie eine identische Befehlszeile. Falls die erste Aufgabe noch ausgeführt wird, startet Batch die zweite Aufgabe auf dem anderen Knoten im Pool.

## <a name="view-task-output"></a>Anzeigen der Aufgabenausgabe

Die von Ihnen erstellten Beispielaufgaben werden in wenigen Minuten abgeschlossen. Wählen Sie zum Anzeigen der Ausgabe einer abgeschlossenen Aufgabe die Aufgabe und dann **Dateien auf Knoten** aus. Wählen Sie die Datei `stdout.txt` aus, um die Standardausgabe der Aufgabe anzuzeigen. Der Inhalt lautet in etwa wie folgt:

:::image type="content" source="media/quick-create-portal/task-output.png" alt-text="Screenshot: Ausgabe einer abgeschlossenen Aufgabe":::

Darin sind die Azure Batch-Umgebungsvariablen zu sehen, die auf dem Knoten festgelegt sind. Beim Erstellen Ihrer eigenen Batch-Aufträge und -Aufgaben können Sie auf diese Umgebungsvariablen in Aufgabenbefehlszeilen sowie in den Apps und Skripts verweisen, die über die Befehlszeilen ausgeführt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit den Batch-Tutorials und -Beispielen fortfahren möchten, können Sie das erstellte Batch-Konto und das verknüpfte Speicherkonto aus dieser Schnellstartanleitung verwenden. Für das Batch-Konto selbst fallen keine Gebühren an.

Ihnen werden während der Ausführung der Knoten auch dann Gebühren für den Pool berechnet, wenn keine Aufträge geplant sind. Es ist ratsam, den Pool zu löschen, wenn Sie ihn nicht mehr benötigen. Klicken Sie in der Kontoansicht auf **Pools** und auf den Namen des Pools. Wählen Sie anschließend die Option **Löschen**.  Beim Löschen des Pools werden alle Aufgabenausgaben auf den Knoten gelöscht.

Löschen Sie die Ressourcengruppe, das Batch-Konto und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe für das Batch-Konto aus, und klicken Sie auf **Ressourcengruppe löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Batch-Konto, einen Batch-Pool und einen Batch-Auftrag erstellt. Mit dem Auftrag wurden Beispielaufgaben ausgeführt, und Sie haben die Ausgabe angezeigt, die auf einem der Knoten erstellt wurde. Da Sie sich jetzt mit den wichtigsten Konzepten des Batch-Diensts vertraut gemacht haben, können Sie Batch mit realistischeren Workloads und in größerem Umfang ausprobieren. Weitere Informationen zu Azure Batch finden Sie in den Azure Batch-Tutorials.

> [!div class="nextstepaction"]
> [Azure Batch-Tutorials](./tutorial-parallel-dotnet.md)
