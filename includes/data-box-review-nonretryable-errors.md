---
author: v-dalc
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 05/07/2021
ms.author: alkohli
ms.openlocfilehash: 6ccf37d96da5b52111fd08a51de4949185891193
ms.sourcegitcommit: eda26a142f1d3b5a9253176e16b5cbaefe3e31b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109736120"
---
Gehen Sie wie folgt vor, um nicht rückgängig zu machende Fehler zu überprüfen und mit Ihrem Auftrag fortzufahren:

1. Öffnen Sie Ihres Auftrags im Azure-Portal.  

   Wenn nicht rückgängig zu machende Fehler das Hochladen von Dateien verhindert haben, wird die folgende Benachrichtigung angezeigt. Der aktuelle Auftragsstatus lautet **Datenkopie abgeschlossen mit Fehlern. Gerät mit ausstehender Datenlöschung.**

   ![Benachrichtigung bei Kopierfehlern während eines Uploads in Azure-Portal](media/data-box-review-nonretryable-errors/copy-errors-in-upload-01.png)

   Notieren Sie sich den **COPY LOG PATH** in **DATA COPY DETAILS**. Sie überprüfen die Fehler im Datenkopierprotokoll.

2. Wählen Sie **Gerätelöschung bestätigen** aus, um einen Überprüfungsbereich zu öffnen.

   ![Überprüfen und Fortfahren des Panels für Uploadfehler im Azure-Portal](media/data-box-review-nonretryable-errors/copy-errors-in-upload-02.png)

3. Überprüfen Sie die Fehler im Datenkopierprotokoll mithilfe des Kopierprotokollpfads, den Sie sich zuvor notiert haben. Bei Bedarf können Sie **Schließen** auswählen, um den Pfad erneut anzuzeigen. 

   Sie müssen alle Konfigurationsprobleme beheben, bevor Sie einen weiteren Upload über eine Netzwerkübertragung oder einen neuen Importauftrag versuchen. <!--For guidance, see [Review copy errors in uploads from Azure Data Box and Azure Data Box Heavy devices](../articles/databox/data-box-troubleshoot-data-upload.md). - To make the Include, I needed to move this reference out of the main procedure.-->

4. Nachdem Sie die Fehler überprüft haben, aktivieren Sie das Kontrollkästchen, um zu bestätigen, dass Sie bereit sind, mit dem Löschen von Daten fortzufahren. Wählen Sie anschließend **Fortfahren** aus.

   ![Vergewissern Sie sich, dass Sie bereit sind, mit dem Löschen von Daten fortzufahren](media/data-box-review-nonretryable-errors/copy-errors-in-upload-03.png)

   Nachdem die Daten sicher vom Gerät gelöscht wurden, wird der Auftragsstatus auf **Kopie abgeschlossen mit Fehlern** aktualisiert.

   ![Statusanzeige für einen Data Box-Importauftrag, der mit Fehlern abgeschlossen wurde](media/data-box-review-nonretryable-errors/copy-errors-in-upload-04.png)

   Wenn Sie keine Aktion durchführen, wird der Auftrag nach 14 Tagen automatisch abgeschlossen.

