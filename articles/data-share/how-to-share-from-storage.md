---
title: Freigeben und Empfangen von Daten aus Azure Blob Storage und Azure Data Lake Storage
description: Erfahren Sie, wie Sie Daten aus Azure Blob Storage und Azure Data Lake Storage freigeben und empfangen.
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 12/16/2020
ms.openlocfilehash: 9dfc8be54fc55842440e376916b2eb9bb04a4610
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617084"
---
# <a name="share-and-receive-data-from-azure-blob-storage-and-azure-data-lake-storage"></a>Freigeben und Empfangen von Daten aus Azure Blob Storage und Azure Data Lake Storage

[!INCLUDE[appliesto-storage](includes/appliesto-storage.md)]

Azure Data Share unterstützt das Freigeben aus dem Speicherkonto über Momentaufnahmen. In diesem Artikel wird erläutert, wie Sie Daten aus den folgenden Quellen freigeben und empfangen: Azure Blob Storage, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2.

Azure Data Share unterstützt die Freigabe von Dateien, Ordnern und Dateisystemen über Azure Data Lake Gen1 und Azure Data Lake Gen2. Außerdem unterstützt es die Freigabe von Blobs, Ordnern und Containern über Azure Blob Storage. Derzeit wird nur das Blockblob unterstützt. Freigegebene Daten aus diesen Quellen können in Azure Data Lake Gen2 oder Azure Blob Storage empfangen werden.

Wenn Dateisysteme, Container oder Ordner in einer auf Momentaufnahmen basierenden Freigabe freigegeben werden, kann der Datenconsumer wahlweise eine vollständige Kopie der Freigabedaten erstellen oder die Funktion der inkrementellen Momentaufnahme nutzen, um nur neue oder aktualisierte Dateien zu kopieren. Die inkrementelle Momentaufnahme basiert auf dem Zeitpunkt der letzten Änderung der Dateien. Vorhandene Dateien mit demselben Namen werden während der Momentaufnahme überschrieben. Die aus der Quelle gelöschte Datei wird nicht im Ziel gelöscht. Leere Unterordner in der Quelle werden nicht in das Ziel kopiert. 

## <a name="share-data"></a>Freigeben von Daten

### <a name="prerequisites-to-share-data"></a>Voraussetzungen für das Freigeben von Daten

* Azure-Abonnement: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* E-Mail-Adresse für die Azure-Anmeldung Ihrer Empfänger (Ein E-Mail-Alias funktioniert nicht.)
* Befindet sich der Azure-Quelldatenspeicher nicht in dem Azure-Abonnement, in dem Sie die Data Share-Ressource erstellen, registrieren Sie [Microsoft.DataShare resource provider](concepts-roles-permissions.md#resource-provider-registration) in dem Abonnement, in dem sich der Azure-Datenspeicher befindet. 

### <a name="prerequisites-for-source-storage-account"></a>Voraussetzungen für das Quellspeicherkonto

* Ein Azure Storage-Konto: Falls Sie noch nicht über ein Konto verfügen, können Sie [hier ein Azure Storage-Konto erstellen](../storage/common/storage-account-create.md).
* Berechtigung zum Schreiben in das Speicherkonto (unter *Microsoft.Storage/storageAccounts/write*). Diese Berechtigung ist in der Rolle „Mitwirkender“ vorhanden.
* Berechtigung zum Hinzufügen einer Rollenzuweisung zum Speicherkonto (unter *Microsoft.Authorization/role assignments/write*). Diese Berechtigung ist in der Rolle „Besitzer“ vorhanden. 

### <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

### <a name="create-a-data-share-account"></a>Erstellen eines Data Share-Kontos

Erstellen Sie eine Azure Data Share-Ressource in einer Azure-Ressourcengruppe.

1. Wählen Sie im Portal links oben die Menüschaltfläche und dann **Ressource erstellen** (+) aus.

1. Suchen Sie nach *Data Share*.

1. Wählen Sie „Data Share“ und dann die Option **Erstellen** aus.

1. Geben Sie die folgenden Informationen als grundlegende Details Ihrer Azure Data Share-Ressource ein. 

     **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**
    |---|---|---|
    | Subscription | Ihr Abonnement | Wählen Sie das Azure-Abonnement aus, das Sie für Ihr Data Share-Konto verwenden möchten.|
    | Ressourcengruppe | *test-resource-group* | Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. |
    | Standort | *USA (Ost) 2* | Wählen Sie eine Region für Ihr Data Share-Konto aus.
    | Name | *datashareaccount* | Geben Sie einen Namen für Ihr Data Share-Konto an. |
    | | |

1. Wählen Sie **Überprüfen + erstellen** und dann **Erstellen** aus, um Ihr Data Share-Konto bereitzustellen. Die Bereitstellung eines neuen Data Share-Kontos dauert normalerweise maximal ca. 2 Minuten. 

1. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressourcengruppe wechseln**.

### <a name="create-a-share"></a>Erstellen einer Freigabe

1. Navigieren Sie zu Ihrer Seite mit der Data Share-Übersicht.

    ![Freigeben von Daten](./media/share-receive-data.png "Freigeben von Daten") 

1. Wählen Sie **Start sharing your data** (Mit Freigabe der Daten beginnen).

1. Klicken Sie auf **Erstellen**.   

1. Geben Sie die Details für Ihre Freigabe ein. Geben Sie einen Namen, die Art der Freigabe, eine Beschreibung der Freigabeinhalte und Nutzungsbedingungen (optional) an. 

    ![EnterShareDetails](./media/enter-share-details.png "Eingeben der Details zur Freigabe") 

1. Wählen Sie **Weiter**.

1. Wählen Sie zum Hinzufügen von Datasets zu Ihrer Freigabe **Datasets hinzufügen** aus. 

    ![Hinzufügen von Datasets zu Ihrer Freigabe](./media/datasets.png "Datasets")

1. Wählen Sie den gewünschten Datasettyp für das Hinzufügen aus. Die angezeigte Liste der Datasettypen ist abhängig von der Art der Freigabe (Momentaufnahme oder direkt), die Sie im vorherigen Schritt ausgewählt haben. 

    ![AddDatasets](./media/add-datasets.png "Hinzufügen von Datasets")    

1. Navigieren Sie zum Objekt, das Sie freigeben möchten, und wählen Sie „Datasets hinzufügen“. 

    ![SelectDatasets](./media/select-datasets.png "Auswählen der Datasets")    

1. Geben Sie auf der Registerkarte „Empfänger“ die E-Mail-Adressen Ihrer Datenconsumer ein, indem Sie die Option „+ Empfänger hinzufügen“ wählen. 

    ![AddRecipients](./media/add-recipient.png "Hinzufügen von Empfängern") 

1. Wählen Sie **Weiter**.

1. Wenn Sie eine Momentaufnahmefreigabe ausgewählt haben, können Sie einen Momentaufnahmezeitplan konfigurieren, um Aktualisierungen Ihrer Daten für Ihren Datenconsumer bereitzustellen. 

    ![EnableSnapshots](./media/enable-snapshots.png "Aktivieren von Momentaufnahmen") 

1. Wählen Sie eine Startzeit und ein Wiederholungsintervall aus. 

1. Wählen Sie **Weiter**.

1. Überprüfen Sie auf der Registerkarte „Bewerten + erstellen“ die Angaben für Paketinhalt, Einstellungen, Empfänger und Synchronisierungseinstellungen. Klicken Sie auf **Erstellen**.

Ihre Azure Data Share-Instanz wurde jetzt erstellt, und der Empfänger Ihrer Datenfreigabe kann Ihre Einladung nun akzeptieren. 

## <a name="receive-data"></a>Empfangen von Daten

### <a name="prerequisites-to-receive-data"></a>Voraussetzungen für den Empfang von Daten
Bevor Sie eine Einladung zu einer Datenfreigabe annehmen können, müssen Sie einige Azure-Ressourcen bereitstellen. Diese sind unten aufgeführt. 

Stellen Sie sicher, dass alle Voraussetzungen erfüllt sind, bevor Sie die Einladung zu einer Datenfreigabe annehmen. 

* Azure-Abonnement: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Eine Data Share-Einladung: Eine Einladung von Microsoft Azure mit dem Betreff „Azure Data Share-Einladung von **<yourdataprovider@domain.com>** “.
* Registrieren Sie den [Ressourcenanbieter „Microsoft.DataShare“](concepts-roles-permissions.md#resource-provider-registration) in dem Azure-Abonnement, in dem Sie eine Data Share-Ressource erstellen möchten, und in dem Azure-Abonnement, in dem sich die Azure-Zieldatenspeicher befinden.

### <a name="prerequisites-for-target-storage-account"></a>Voraussetzungen für das Zielspeicherkonto

* Ein Azure Storage-Konto: Falls Sie noch nicht über ein Konto verfügen, können Sie [hier ein Azure Storage-Konto erstellen](../storage/common/storage-account-create.md). 
* Berechtigung zum Schreiben in das Speicherkonto (unter *Microsoft.Storage/storageAccounts/write*). Diese Berechtigung ist in der Rolle „Mitwirkender“ vorhanden. 
* Berechtigung zum Hinzufügen einer Rollenzuweisung zum Speicherkonto (unter *Microsoft.Authorization/role assignments/write*). Diese Berechtigung ist in der Rolle „Besitzer“ vorhanden.  

### <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

### <a name="open-invitation"></a>Öffnen der Einladung

1. Sie können die Einladung über die E-Mail oder direkt im Azure-Portal öffnen. 

   Wenn Sie die Einladung über die E-Mail öffnen möchten, überprüfen Sie Ihren Posteingang auf eine Einladung von Ihrem Datenanbieter. Die Einladung stammt von Microsoft Azure und hat die Bezeichnung **Azure Data Share-Einladung von <yourdataprovider@domain.com>** . Klicken Sie auf **Einladung anzeigen**, um Ihre Einladung in Azure anzuzeigen. 

   Wenn Sie eine Einladung direkt im Azure-Portal öffnen möchten, suchen Sie im Azure-Portal nach **Einladungen zu Data Share**. Dadurch gelangen Sie zur Liste der Data Share-Einladungen.

   ![Liste mit Einladungen](./media/invitations.png "Liste mit Einladungen") 

1. Wählen Sie die Freigabe aus, die Sie anzeigen möchten. 

### <a name="accept-invitation"></a>Annehmen der Einladung
1. Stellen Sie sicher, dass Sie alle Felder überprüfen – auch die **Nutzungsbedingungen**. Wenn Sie den Nutzungsbedingungen zustimmen, müssen Sie das entsprechende Kontrollkästchen aktivieren, um dies anzugeben. 

   ![Nutzungsbedingungen](./media/terms-of-use.png "Nutzungsbedingungen") 

1. Wählen Sie unter *Target Data Share Account* (Data Share-Zielkonto) das Abonnement und die Ressourcengruppe für die Bereitstellung Ihrer Data Share-Instanz aus. 

   Wählen Sie für das Feld **Data Share Account** (Data Share-Konto) die Option **Neue erstellen**, falls Sie nicht über ein vorhandenes Data Share-Konto verfügen. Wählen Sie andernfalls ein vorhandenes Data Share-Konto für Ihre Datenfreigabe aus. 

   Im Feld **Received Share Name** (Name der empfangenen Freigabe) können Sie den vom Datenanbieter angegebenen Standardnamen übernehmen oder einen neuen Namen für die empfangene Freigabe angeben. 

   Nachdem Sie den Nutzungsbedingungen zugestimmt und ein Data Share-Konto für die Verwaltung der empfangenen Freigabe angegeben haben, wählen Sie **Accept and configure** (Akzeptieren und konfigurieren) aus. Ein Freigabeabonnement wird erstellt. 

   ![Optionen zum Akzeptieren](./media/accept-options.png "Optionen zum Akzeptieren") 

   Dadurch gelangen Sie zu der empfangenen Freigabe in Ihrem Data Share-Konto. 

   Wählen Sie *Ablehnen*, falls Sie die Einladung nicht annehmen möchten. 

### <a name="configure-received-share"></a>Konfigurieren der empfangenen Freigabe
Führen Sie die folgenden Schritte aus, um zu konfigurieren, wo Sie Daten empfangen möchten.

1. Wählen Sie die Registerkarte **Datasets** aus. Aktivieren Sie das Kontrollkästchen neben dem Dataset, dem Sie ein Ziel zuweisen möchten. Wählen Sie **+ Dem Ziel zuordnen** aus, um einen Zieldatenspeicher auszuwählen. 

   ![Dem Ziel zuordnen](./media/dataset-map-target.png "Dem Ziel zuordnen") 

1. Wählen Sie einen Zieldatenspeicher aus, in dem die empfangenen Daten gespeichert werden sollen. Datendateien im Zieldatenspeicher mit demselben Pfad und Namen werden überschrieben. 

   ![Zielspeicherkonto](./media/map-target.png "Zielspeicher") 

1. Wenn der Datenanbieter für die momentaufnahmebasierte Freigabe einen Momentaufnahmezeitplan erstellt hat, um die Daten regelmäßig zu aktualisieren, können Sie auch den Momentaufnahmezeitplan aktivieren, indem Sie die Registerkarte **Momentaufnahmezeitplan** auswählen. Aktivieren Sie das Kontrollkästchen neben „Momentaufnahmezeitplan“, und wählen Sie **+ Aktivieren** aus.

   ![Aktivieren von „Momentaufnahmezeitplan“](./media/enable-snapshot-schedule.png "Aktivieren des Momentaufnahmezeitplans")

### <a name="trigger-a-snapshot"></a>Auslösen einer Momentaufnahme
Diese Schritte sind nur für die momentaufnahmebasierte Freigabe relevant.

1. Sie können eine Momentaufnahme auslösen, indem Sie die Registerkarte **Details** und anschließend **Trigger snapshot** (Momentaufnahme auslösen) auswählen. Hier können Sie eine vollständige oder inkrementelle Momentaufnahme Ihrer Daten auslösen. Wählen Sie die Option für das vollständige Kopieren, falls Sie zum ersten Mal Daten von Ihrem Datenanbieter erhalten. 

   ![Auslösen der Momentaufnahme](./media/trigger-snapshot.png "Auslösen der Momentaufnahme") 

1. Wenn der Status der letzten Ausführung *Erfolgreich* lautet, navigieren Sie zum Zieldatenspeicher, um die empfangenen Daten anzuzeigen. Wählen Sie **Datasets** aus, und klicken Sie auf den Link im Zielpfad. 

   ![Consumerdatasets](./media/consumer-datasets.png "Consumerdatasetzuordnung") 

### <a name="view-history"></a>Anzeigen des Verlaufs
Dieser Schritt ist nur für die momentaufnahmebasierte Freigabe relevant. Wählen Sie zum Anzeigen des Verlaufs der Momentaufnahmen die Registerkarte **Verlauf** aus. Hier ist ein Verlauf aller Momentaufnahmen angegeben, die in den letzten 30 Tagen generiert wurden. 

## <a name="next-steps"></a>Nächste Schritte
Sie haben erfahren, wie Sie Daten aus Speicherkonten mithilfe des Azure Data Share-Diensts freigeben und empfangen. Weitere Informationen zum Freigeben von Daten aus anderen Datenquellen finden Sie unter [Unterstützte Datenspeicher](supported-data-stores.md).