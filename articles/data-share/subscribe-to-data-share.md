---
title: 'Tutorial: Akzeptieren und Empfangen von Daten: Azure Data Share'
description: 'Tutorial: Akzeptieren und Empfangen von Daten per Azure Data Share'
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: tutorial
ms.date: 08/14/2020
ms.openlocfilehash: 4d4ef9f1d80604373463450adf597f8202611175
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92216571"
---
# <a name="tutorial-accept-and-receive-data-using-azure-data-share"></a>Tutorial: Akzeptieren und Empfangen von Daten mithilfe von Azure Data Share  

In diesem Tutorial wird beschrieben, wie Sie mit Azure Data Share eine Einladung für eine Datenfreigabe annehmen. Sie erfahren, wie Sie die für Sie freigegebenen Daten empfangen und ein Intervall für die regelmäßige Aktualisierung einrichten. So können Sie sicherstellen, dass Sie immer über die aktuellste Momentaufnahme der Daten verfügen, die für Sie freigegeben werden. 

> [!div class="checklist"]
> * Annehmen einer Azure Data Share-Einladung
> * Erstellen eines Azure Data Share-Kontos
> * Angeben eines Ziels für Ihre Daten
> * Erstellen eines Abonnements Ihrer Datenfreigabe für die geplante Aktualisierung

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie eine Einladung zu einer Datenfreigabe annehmen können, müssen Sie einige Azure-Ressourcen bereitstellen. Diese sind unten aufgeführt. 

Stellen Sie sicher, dass alle Voraussetzungen erfüllt sind, bevor Sie die Einladung zu einer Datenfreigabe annehmen. 

* Azure-Abonnement: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Eine Data Share-Einladung: Eine Einladung von Microsoft Azure mit dem Betreff „Azure Data Share-Einladung von **<yourdataprovider@domain.com>** “.
* Registrieren Sie den [Ressourcenanbieter „Microsoft.DataShare“](concepts-roles-permissions.md#resource-provider-registration) in dem Azure-Abonnement, in dem Sie eine Data Share-Ressource erstellen möchten, und in dem Azure-Abonnement, in dem sich die Azure-Zieldatenspeicher befinden.

### <a name="receive-data-into-a-storage-account"></a>Empfangen von Daten in einem Speicherkonto: 

* Ein Azure Storage-Konto: Falls Sie noch nicht über ein Konto verfügen, können Sie [hier ein Azure Storage-Konto erstellen](../storage/common/storage-account-create.md). 
* Berechtigung zum Schreiben in das Speicherkonto (unter *Microsoft.Storage/storageAccounts/write*). Diese Berechtigung ist in der Rolle „Mitwirkender“ vorhanden. 
* Berechtigung zum Hinzufügen einer Rollenzuweisung zum Speicherkonto (unter *Microsoft.Authorization/role assignments/write*). Diese Berechtigung ist in der Rolle „Besitzer“ vorhanden.  

### <a name="receive-data-into-a-sql-based-target"></a>Empfangen von Daten in einem SQL-basierten Ziel:

* Berechtigung zum Schreiben in Datenbanken auf dem SQL-Server (unter *Microsoft.Sql/servers/databases/write*). Diese Berechtigung ist in der Rolle „Mitwirkender“ vorhanden. 
* Berechtigung zum Zugreifen auf die Azure SQL-Datenbank- oder Azure Synapse Analytics-Instanz für die verwaltete Identität der Datenfreigaberessource. Die Berechtigung kann mit folgenden Schritten gewährt werden: 
    1. Legen Sie sich selbst als Azure Active Directory-Administrator für den SQL-Server fest.
    1. Stellen Sie mithilfe von Azure Active Directory eine Verbindung mit der Azure SQL-Datenbank-/Data Warehouse-Instanz her.
    1. Führen Sie mithilfe des Abfrage-Editors (Vorschauversion) das folgende Skript aus, um die verwaltete Data Share-Identität als „db_datareader, db_datawriter, db_ddladmin“ hinzuzufügen. Sie müssen mithilfe von Active Directory und nicht über die SQL Server-Authentifizierung eine Verbindung herstellen. 

        ```sql
        create user "<share_acc_name>" from external provider; 
        exec sp_addrolemember db_datareader, "<share_acc_name>"; 
        exec sp_addrolemember db_datawriter, "<share_acc_name>"; 
        exec sp_addrolemember db_ddladmin, "<share_acc_name>";
        ```      
        Beachten Sie, dass *<share_acc_name>* der Name Ihrer Data Share-Ressource ist. Falls Sie noch keine Data Share-Ressource erstellt haben, können Sie später zu dieser Voraussetzung zurückkehren.         

* Client-IP-SQL Server-Firewallzugriff. Die Berechtigung kann mit folgenden Schritten gewährt werden: 
    1. Navigieren Sie in SQL Server im Azure-Portal zu *Firewalls und virtuelle Netzwerke*.
    1. Legen Sie die Umschaltfläche auf **Ein** fest, um den Zugriff auf Azure-Dienste zuzulassen.
    1. Klicken Sie auf **+ Client-IP-Adresse hinzufügen** und anschließend auf **Speichern**. Die IP-Adresse kann sich ggf. ändern. Dieser Prozess muss unter Umständen beim nächsten Dateneingang im SQL-Ziel über das Azure-Portal wiederholt werden. Sie können auch einen IP-Adressbereich hinzufügen. 


### <a name="receive-data-into-an-azure-data-explorer-cluster"></a>Empfangen von Daten in einem Azure Data Explorer-Cluster: 

* Ein Azure Data Explorer-Cluster im gleichen Azure-Rechenzentrum wie der Data Explorer-Cluster des Datenanbieters: Sollten Sie über keinen solchen Cluster verfügen, können Sie einen [Azure Data Explorer-Cluster erstellen](/azure/data-explorer/create-cluster-database-portal). Falls Ihnen das Azure-Rechenzentrum des Datenanbieterclusters nicht bekannt ist, können Sie den Cluster später in diesem Prozess erstellen.
* Berechtigung zum Schreiben in den Azure Data Explorer-Cluster (unter *Microsoft.Kusto/clusters/write*). Diese Berechtigung ist in der Rolle „Mitwirkender“ vorhanden. 
* Berechtigung zum Hinzufügen einer Rollenzuweisung zum Azure Data Explorer-Cluster (unter *Microsoft.Authorization/role assignments/write*). Diese Berechtigung ist in der Rolle „Besitzer“ vorhanden. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="open-invitation"></a>Öffnen der Einladung

1. Sie können die Einladung über die E-Mail oder direkt im Azure-Portal öffnen. 

   Wenn Sie die Einladung über die E-Mail öffnen möchten, überprüfen Sie Ihren Posteingang auf eine Einladung von Ihrem Datenanbieter. Die Einladung stammt von Microsoft Azure und hat die Bezeichnung **Azure Data Share-Einladung von <yourdataprovider@domain.com>** . Klicken Sie auf **Einladung anzeigen**, um Ihre Einladung in Azure anzuzeigen. 

   Wenn Sie eine Einladung direkt im Azure-Portal öffnen möchten, suchen Sie im Azure-Portal nach **Einladungen zu Data Share**. Dadurch gelangen Sie zur Liste der Data Share-Einladungen.

   ![Liste mit Einladungen](./media/invitations.png "Liste mit Einladungen") 

1. Wählen Sie die Freigabe aus, die Sie anzeigen möchten. 

## <a name="accept-invitation"></a>Annehmen der Einladung
1. Stellen Sie sicher, dass Sie alle Felder überprüfen – auch die **Nutzungsbedingungen**. Wenn Sie den Nutzungsbedingungen zustimmen, müssen Sie das entsprechende Kontrollkästchen aktivieren, um dies anzugeben. 

   ![Nutzungsbedingungen](./media/terms-of-use.png "Nutzungsbedingungen") 

1. Wählen Sie unter *Target Data Share Account* (Data Share-Zielkonto) das Abonnement und die Ressourcengruppe für die Bereitstellung Ihrer Data Share-Instanz aus. 

   Wählen Sie für das Feld **Data Share Account** (Data Share-Konto) die Option **Neue erstellen**, falls Sie nicht über ein vorhandenes Data Share-Konto verfügen. Wählen Sie andernfalls ein vorhandenes Data Share-Konto für Ihre Datenfreigabe aus. 

   Im Feld **Received Share Name** (Name der empfangenen Freigabe) können Sie den vom Datenanbieter angegebenen Standardnamen übernehmen oder einen neuen Namen für die empfangene Freigabe angeben. 

   Nachdem Sie den Nutzungsbedingungen zugestimmt und ein Data Share-Konto für die Verwaltung der empfangenen Freigabe angegeben haben, wählen Sie **Accept and configure** (Akzeptieren und konfigurieren) aus. Ein Freigabeabonnement wird erstellt. 

   ![Optionen zum Akzeptieren](./media/accept-options.png "Optionen zum Akzeptieren") 

   Dadurch gelangen Sie zu der empfangenen Freigabe in Ihrem Data Share-Konto. 

   Wählen Sie *Ablehnen*, falls Sie die Einladung nicht annehmen möchten. 

## <a name="configure-received-share"></a>Konfigurieren der empfangenen Freigabe
Führen Sie die folgenden Schritte aus, um zu konfigurieren, wo Sie Daten empfangen möchten.

1. Wählen Sie die Registerkarte **Datasets** aus. Aktivieren Sie das Kontrollkästchen neben dem Dataset, dem Sie ein Ziel zuweisen möchten. Wählen Sie **+ Dem Ziel zuordnen** aus, um einen Zieldatenspeicher auszuwählen. 

   ![Dem Ziel zuordnen](./media/dataset-map-target.png "Dem Ziel zuordnen") 

1. Wählen Sie einen Zieldatenspeichertyp aus, in dem die Daten gespeichert werden sollen. Alle Datendateien oder -tabellen im Zieldatenspeicher mit demselben Pfad und Namen werden überschrieben. 

   Wählen Sie für die direkte Freigabe einen Datenspeicher am angegebenen Speicherort aus. Der Speicherort ist das Azure-Rechenzentrum, in dem sich der Quelldatenspeicher des Datenanbieters befindet. Nach der Zuordnung des Datasets können Sie über den Link im Zielpfad auf die Daten zugreifen.

   ![Zielspeicherkonto](./media/dataset-map-target-sql.png "Zielspeicher") 

1. Wenn der Datenanbieter für die momentaufnahmebasierte Freigabe einen Momentaufnahmezeitplan erstellt hat, um die Daten regelmäßig zu aktualisieren, können Sie auch den Momentaufnahmezeitplan aktivieren, indem Sie die Registerkarte **Momentaufnahmezeitplan** auswählen. Aktivieren Sie das Kontrollkästchen neben „Momentaufnahmezeitplan“, und wählen Sie **+ Aktivieren** aus.

   ![Aktivieren von „Momentaufnahmezeitplan“](./media/enable-snapshot-schedule.png "Aktivieren des Momentaufnahmezeitplans")

## <a name="trigger-a-snapshot"></a>Auslösen einer Momentaufnahme
Diese Schritte sind nur für die momentaufnahmebasierte Freigabe relevant.

1. Sie können eine Momentaufnahme auslösen, indem Sie die Registerkarte **Details** und anschließend **Trigger snapshot** (Momentaufnahme auslösen) auswählen. Hier können Sie eine vollständige oder inkrementelle Momentaufnahme Ihrer Daten auslösen. Wählen Sie die Option für das vollständige Kopieren, falls Sie zum ersten Mal Daten von Ihrem Datenanbieter erhalten. 

   ![Auslösen der Momentaufnahme](./media/trigger-snapshot.png "Auslösen der Momentaufnahme") 

1. Wenn der Status der letzten Ausführung *Erfolgreich* lautet, navigieren Sie zum Zieldatenspeicher, um die empfangenen Daten anzuzeigen. Wählen Sie **Datasets** aus, und klicken Sie auf den Link im Zielpfad. 

   ![Consumerdatasets](./media/consumer-datasets.png "Consumerdatasetzuordnung") 

## <a name="view-history"></a>Anzeigen des Verlaufs
Dieser Schritt ist nur für die momentaufnahmebasierte Freigabe relevant. Wählen Sie zum Anzeigen des Verlaufs der Momentaufnahmen die Registerkarte **Verlauf** aus. Hier ist ein Verlauf aller Momentaufnahmen angegeben, die in den letzten 30 Tagen generiert wurden. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Sie eine Azure Data Share-Instanz akzeptieren und empfangen. Weitere Informationen zu den Konzepten von Azure Data Share finden Sie unter [Konzepte: Azure Data Share-Terminologie](terminology.md).