---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 05/07/2021
ms.author: alkohli
ms.openlocfilehash: a46552639b9cdea135216e544ec4c51f4d9dda3d
ms.sourcegitcommit: eda26a142f1d3b5a9253176e16b5cbaefe3e31b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109736167"
---
Nachdem das Gerät bei Microsoft eingegangen ist und gescannt wurde, wird der Status der Bestellung in **Empfangen** geändert. Das Gerät wird dann physisch auf Schäden oder Anzeichen einer Manipulation überprüft.

Nach der Überprüfung wird die Data Box mit dem Netzwerk im Azure-Datencenter verbunden. Das Kopieren der Daten wird automatisch gestartet. Je nach Datengröße kann der Kopiervorgang einige Stunden oder auch einige Tage dauern. Sie können den Status des Kopiervorgangs im Portal verfolgen.

### <a name="review-copy-errors-from-upload"></a>Überprüfen von Kopierfehlern beim Hochladen

Wenn Dateien aufgrund eines nicht wiederholbaren Fehlers nicht hochgeladen werden können, werden Sie benachrichtigt, um die Fehler zu überprüfen, bevor Sie fortfahren. Die Fehler werden im Datenkopierprotokoll aufgeführt.

Sie können diese Fehler nicht beheben. Der Upload wurde mit Fehlern abgeschlossen. Die Benachrichtigung informiert Sie über alle Konfigurationsprobleme, die Sie beheben müssen, bevor Sie einen weiteren Upload über eine Netzwerkübertragung oder einen neuen Importauftrag versuchen. Eine Leitfaden finden Sie unter [Überprüfen von Kopierfehlern bei Uploads von Azure Data Box und Azure Data Box Heavy Geräten](../articles/databox/data-box-troubleshoot-data-upload.md).

Wenn Sie bestätigen, dass Sie die Fehler überprüft haben und bereit zum Fortfahren sind, werden die Daten sicher vom Gerät gelöscht. Der Auftrag wird nach 14 Tagen automatisch abgeschlossen. Wenn Sie auf die Benachrichtigung reagieren, können Sie den Vorgang beschleunigen.

[!INCLUDE [data-box-review-nonretryable-errors](data-box-review-nonretryable-errors.md)]


### <a name="verify-data-in-completed-upload"></a>Überprüfen der Daten beim abgeschlossenen Upload

Nachdem der Datenupload abgeschlossen ist, wird der Auftragsstatus in **Abgeschlossen** geändert.

Stellen Sie sicher, dass Ihre Daten in Azure hochgeladen wurden, bevor Sie sie aus der Quelle löschen. Ihre Daten können sich an folgenden Orten befinden:

- In Ihren Azure Storage-Konten. Wenn Sie die Daten in Data Box kopieren, werden die Daten in einen der folgenden Pfade in Ihrem Azure Storage-Konto hochgeladen:

  - Blockblobs und Seitenblobs: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
  - Azure Files: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

    Alternativ hierzu können Sie auch im Azure-Portal auf Ihr Azure-Speicherkonto zugreifen und von dort aus entsprechend navigieren.

- In Ihren Ressourcengruppen für verwaltete Datenträger. Beim Erstellen von verwalteten Datenträgern werden die VHDs als Seitenblobs hochgeladen und dann in verwaltete Datenträger konvertiert. Die verwalteten Datenträger werden an die Ressourcengruppen angefügt, die zum Zeitpunkt der Auftragserstellung angegeben waren. 

    - Wenn der Kopiervorgang auf verwaltete Datenträger in Azure erfolgreich war, können Sie im Azure-Portal zu **Auftragsdetails** navigieren und sich die Ressourcengruppen notieren, die für verwaltete Datenträger angegeben sind.

        ![Identifizieren von Ressourcengruppen für verwaltete Datenträger](media/data-box-verify-upload-return/order-details-managed-disk-resource-groups.png)

        Wechseln Sie zu der Ressourcengruppe, die Sie notiert haben, und suchen Sie Ihre verwalteten Datenträger.

        ![An Ressourcengruppen angefügter verwalteter Datenträger](media/data-box-verify-upload-return/managed-disks-resource-group.png)

    - Wenn Sie eine VHDX oder eine dynamische bzw. differenzierende VHD kopiert haben, wird die VHDX bzw. VHD als Seitenblob in das Stagingspeicherkonto hochgeladen, die VHD kann jedoch nicht in einen verwalteten Datenträger konvertiert werden. Wechseln Sie zu Ihrem **Stagingspeicherkonto > Blobs**, und wählen Sie den geeigneten Container aus: SSD Standard, HDD Standard oder SSD Premium. Die VHDs werden als Seitenblobs in Ihr Stagingspeicherkonto hochgeladen, wobei Gebühren anfallen.


## <a name="erasure-of-data-from-data-box"></a>Löschen von Daten von der Data Box
 
Nachdem die Daten in Azure hochgeladen wurden, löscht die Data Box die Daten auf den Datenträgern gemäß den [NIST-Richtlinien (SP 800-88 Revision 1)](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Themen erhalten, darunter die folgenden:

> [!div class="checklist"]
> * Voraussetzungen
> * Vorbereiten des Versands
> * Senden der Data Box an Microsoft
> * Überprüfen des Datenuploads in Azure
> * Löschen von Daten von der Data Box

Im folgenden Artikel erfahren Sie, wie Sie Data Box über die lokale Webbenutzeroberfläche verwalten.

> [!div class="nextstepaction"]
> [Verwenden der lokalen Webbenutzeroberfläche zum Verwalten von Azure Data Box](../articles/databox/data-box-local-web-ui-admin.md)