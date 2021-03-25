---
title: Wiederherstellen von Azure-Blobs
description: Hier erfahren Sie, wie Sie Azure-Blobs wiederherstellen (Vorschau).
ms.topic: conceptual
ms.date: 02/16/2021
ms.openlocfilehash: 4cbd47ea654115f00095e30f7d5114aec0f2c85a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743395"
---
# <a name="restore-azure-blobs-in-preview"></a>Wiederherstellen von Azure-Blobs (Vorschau)

Blockblobs in Speicherkonten, für die eine operationale Sicherung konfiguriert sind, können auf einen beliebigen Zeitpunkt innerhalb des Aufbewahrungszeitraums wiederhergestellt werden. Außerdem können Sie die Wiederherstellung aller Blockblobs im Speicherkonto oder einer Teilmenge von Blobs festlegen.

## <a name="before-you-start"></a>Vorbereitung

- Blobs werden im gleichen Speicherkonto wiederhergestellt. Blobs, die seit dem Zeitpunkt, auf den Sie die Wiederherstellung durchführen, geändert wurden, werden daher überschrieben.
- Nur Blockblobs in einem Standardspeicherkonto vom Typ „Allgemein v2“ können im Rahmen eines Wiederherstellungsvorgangs wiederhergestellt werden. Anfügeblobs, Seitenblobs und Premium-Blockblobs werden nicht wiederhergestellt.
- Während ein Wiederherstellungsauftrag läuft, Blobs im Speicher nicht gelesen oder geschrieben werden.
- Ein Blob mit einer aktiven Leasedauer kann nicht wiederhergestellt werden. Wenn ein Blob mit einer aktiven Leasedauer in den Bereich der wiederherzustellenden Blobs eingeschlossen ist, schlägt der Wiederherstellungsvorgang automatisch fehl. Unterbrechen Sie alle aktiven Leases, bevor Sie den Wiederherstellungsvorgang starten.
- Momentaufnahmen werden im Rahmen eines Wiederherstellungsvorgangs nicht erstellt oder gelöscht. Nur das Basisblob wird in seinem vorherigen Zustand wiederhergestellt.
- Wenn Sie einen Container aus dem Speicherkonto löschen, indem Sie den Vorgang **Container löschen** aufrufen, kann dieser Container nicht mit einem Wiederherstellungsvorgang wiederhergestellt werden. Löschen Sie die einzelnen Blobs, anstatt einen ganzen Container zu löschen, wenn Sie sie möglicherweise später wiederherstellen möchten. Außerdem empfiehlt Microsoft, zusätzlich zur operativen Sicherung das vorläufige Löschen für Container zu aktivieren, um ein versehentliches Löschen von Containern zu verhindern.
- Alle Einschränkungen und unterstützten Szenarien finden Sie in der [Unterstützungsmatrix](blob-backup-support-matrix.md).

## <a name="restore-blobs"></a>Wiederherstellen von Blobs

Sie können eine Wiederherstellung über das Backup Center veranlassen.

1. Gehen Sie im Backup Center auf der oberen Leiste zu **Wiederherstellen**.

    ![Wiederherstellen in Backup Center](./media/blob-restore/backup-center-restore.png)

1. Wählen Sie auf der Registerkarte **Wiederherstellung initiieren** als Datenquellentyp **Azure-Blobs (Azure Storage)** aus, und wählen Sie die **Sicherungsinstanz** aus, die Sie wiederherstellen möchten. Die Sicherungsinstanz ist hierbei das Speicherkonto, das die Blobs enthält, die Sie wiederherstellen möchten.

     ![Auswählen der Sicherungsinstanz](./media/blob-restore/select-backup-instance.png)

1. Wählen Sie auf der Registerkarte **Wiederherstellungspunkt auswählen** das Datum und die Uhrzeit der wiederherzustellenden Daten aus. Sie können auch den Schieberegler verwenden, um den Zeitpunkt für die Wiederherstellung auszuwählen. Die Infoblase neben dem Datum zeigt den gültigen Zeitraum an, aus dem Sie Ihre Daten wiederherstellen können. Die operative Sicherung für fortlaufend gesicherte Blobs ermöglicht eine Feinsteuerung der Zeitpunkte, bis zu denen Daten wiederhergestellt werden können.

    >[!NOTE]
    > Die Zeit ist hier in Ortszeit angegeben.

    ![Datum und Uhrzeit für die Wiederherstellung](./media/blob-restore/date-and-time.png)

1. Wählen Sie auf der Registerkarte **Wiederherstellungsparameter** aus, ob Sie alle Blobs im Speicherkonto, bestimmte Container oder mithilfe der Präfixübereinstimmung eine Teilmenge der Blobs wiederherstellen möchten. Wenn Sie die Präfixübereinstimmung verwenden, können Sie bis zu 10 Präfix- oder Dateipfadbereiche angeben. Weitere Informationen zur Präfixübereinstimmung finden Sie [hier](#use-prefix-match-for-restoring-blobs).

    ![Wiederherstellungsparameter](./media/blob-restore/restore-parameters.png)

    Wählen Sie eine der Optionen aus:

    - **Restore all blobs in the storage account** (Alle Blobs im Speicherkonto wiederherstellen): Bei Verwendung dieser Option werden alle Blockblobs im Speicherkonto wiederhergestellt, indem ein Rollback zum ausgewählten Zeitpunkt erfolgt. Bei Speicherkonten, die große Datenmengen enthalten oder eine hohe Änderungsrate aufweisen, kann der Vorgang längere Zeit in Anspruch nehmen.

    - **Browse and restore selected containers** (Ausgewählte Container durchsuchen und wiederherstellen): Mit dieser Option können Sie bis zu 10 wiederherzustellende Container durchsuchen und auswählen. Sie müssen über ausreichende Berechtigungen zum Anzeigen der Container im Speicherkonto verfügen. Andernfalls wird Ihnen der Inhalt des Speicherkontos möglicherweise nicht angezeigt.

    - **Select blobs to restore using prefix match** (Wiederherzustellende Blobs über Präfixübereinstimmung auswählen): Mit dieser Option können Sie eine Teilmenge von Blobs über eine Präfixübereinstimmung wiederherstellen. Sie können bis zu 10 lexikografische Bereiche von Blobs innerhalb eines einzelnen Containers oder aus mehreren Containern wiederherstellen, um diese Blobs auf einen Zustand zu einem bestimmten Zeitpunkt zurückzusetzen. Beachten Sie dabei Folgendes:

        - Sie können einen Schrägstrich (/) verwenden, um den Containernamen vom Blobpräfix zu trennen.
        - Der Anfang des angegebenen Bereichs wird mit eingeschlossen, das Ende des angegebenen Bereichs wird jedoch ausgeschlossen.

    Weitere Informationen zum Verwenden von Präfixen für die Wiederherstellung von Blobbereichen finden Sie in [diesem Abschnitt](#use-prefix-match-for-restoring-blobs).

1. Wenn Sie die wiederherzustellenden Blobs angegeben haben, fahren Sie mit der Registerkarte **Überprüfen und wiederherstellen** fort, und wählen Sie **Wiederherstellen** aus, um die Wiederherstellung zu starten.

1. **Track restore** (Wiederherstellung nachverfolgen): Mit der Ansicht **Sicherungsaufträge** können Sie die Details und den Status der Wiederherstellungsvorgänge nachverfolgen. Navigieren Sie hierzu zu **Backup Center** > **Sicherungsaufträge**. Der Status lautet **In Bearbeitung**, während die Wiederherstellung läuft.

    ![Registerkarte „Sicherungsaufträge“](./media/blob-restore/backup-jobs.png)

    Wenn der Wiederherstellungsvorgang erfolgreich abgeschlossen wurde, ändert sich der Status in **Abgeschlossen**. Nachdem die Wiederherstellung erfolgreich abgeschlossen wurde, können Sie wieder Blobs im Speicherkonto lesen und schreiben.

## <a name="additional-topics"></a>Weitere Themen

### <a name="use-prefix-match-for-restoring-blobs"></a>Verwenden der Präfixübereinstimmung zum Wiederherstellen von Blobs

Betrachten Sie das folgenden Beispiel:

![Wiederherstellen mit der Präfixübereinstimmung](./media/blob-restore/prefix-match.png)

In dem in der Abbildung gezeigten Wiederherstellungsvorgang werden die folgenden Aktionen ausgeführt:

- Der gesamte Inhalt von *container1* wird wiederhergestellt.
- Blobs im lexikografischen Bereich von *blob1* bis *blob5* in *container2* werden wiederhergestellt. Mit diesem Bereich werden Blobs mit Namen wie *blob1*, *blob11*, *blob100*, *blob2* usw. wiederhergestellt. Da das Ende des Bereichs ausgeschlossen wird, werden Blobs, deren Name mit *blob4* beginnt, wiederhergestellt, Blobs, deren Name mit *blob5* beginnt, jedoch nicht.
- Alle Blobs in *container3* und *container4* werden wiederhergestellt. Da das Ende des Bereichs ausgeschlossen wird, wird *container5* bei diesem Bereich nicht wiederhergestellt.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über die operative Sicherung für Azure-Blobs (Vorschau)](blob-backup-overview.md)
