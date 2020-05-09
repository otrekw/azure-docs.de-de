---
title: 'Tutorial: Akzeptieren und Empfangen von Daten: Azure Data Share'
description: 'Tutorial: Akzeptieren und Empfangen von Daten per Azure Data Share'
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.openlocfilehash: 4dff48f909cd3febbbb7e92dcf96070020b8f57c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82145138"
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

* Ein Azure Storage-Konto: Falls Sie noch nicht über ein Konto verfügen, können Sie [hier ein Azure Storage-Konto erstellen](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account). 
* Berechtigung zum Schreiben in das Speicherkonto (unter *Microsoft.Storage/storageAccounts/write*). Diese Berechtigung ist in der Rolle „Mitwirkender“ vorhanden. 
* Berechtigung zum Hinzufügen einer Rollenzuweisung zum Speicherkonto (unter *Microsoft.Authorization/role assignments/write*). Diese Berechtigung ist in der Rolle „Besitzer“ vorhanden.  

### <a name="receive-data-into-a-sql-based-source"></a>Empfangen von Daten in einer SQL-basierten Quelle:

* Berechtigung zum Schreiben in Datenbanken auf dem SQL-Server (unter *Microsoft.Sql/servers/databases/write*). Diese Berechtigung ist in der Rolle „Mitwirkender“ vorhanden. 
* Berechtigung zum Zugreifen auf die Azure SQL-Datenbank- oder Azure SQL Data Warehouse-Instanz für die verwaltete Identität der Datenfreigaberessource. Die Berechtigung kann mit folgenden Schritten gewährt werden: 
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

* Ein Azure Data Explorer-Cluster im gleichen Azure-Rechenzentrum wie der Data Explorer-Cluster des Datenanbieters: Sollten Sie über keinen solchen Cluster verfügen, können Sie einen [Azure Data Explorer-Cluster erstellen](https://docs.microsoft.com/azure/data-explorer/create-cluster-database-portal). Falls Ihnen das Azure-Rechenzentrum des Datenanbieterclusters nicht bekannt ist, können Sie den Cluster später in diesem Prozess erstellen.
* Berechtigung zum Schreiben in den Azure Data Explorer-Cluster (unter *Microsoft.Kusto/clusters/write*). Diese Berechtigung ist in der Rolle „Mitwirkender“ vorhanden. 
* Berechtigung zum Hinzufügen einer Rollenzuweisung zum Azure Data Explorer-Cluster (unter *Microsoft.Authorization/role assignments/write*). Diese Berechtigung ist in der Rolle „Besitzer“ vorhanden. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="open-invitation"></a>Öffnen der Einladung

1. Sie können die Einladung über die E-Mail oder direkt im Azure-Portal öffnen. 

   Wenn Sie die Einladung über die E-Mail öffnen möchten, überprüfen Sie Ihren Posteingang auf eine Einladung von Ihrem Datenanbieter. Die Einladung stammt von Microsoft Azure und hat die Bezeichnung **Azure Data Share-Einladung von <yourdataprovider@domain.com>** . Klicken Sie auf **Einladung anzeigen**, um Ihre Einladung in Azure anzuzeigen. 

   Wenn Sie eine Einladung direkt im Azure-Portal öffnen möchten, suchen Sie im Azure-Portal nach **Einladungen zu Data Share**. Dadurch gelangen Sie zur Liste der Data Share-Einladungen.

   ![Einladungen](./media/invitations.png "Liste mit Einladungen") 

1. Wählen Sie die Freigabe aus, die Sie anzeigen möchten. 

## <a name="accept-invitation"></a>Annehmen der Einladung
1. Stellen Sie sicher, dass Sie alle Felder überprüfen – auch die **Nutzungsbedingungen**. Wenn Sie den Nutzungsbedingungen zustimmen, müssen Sie das entsprechende Kontrollkästchen aktivieren, um dies anzugeben. 

   ![Nutzungsbedingungen](./media/terms-of-use.png "Nutzungsbedingungen") 

1. Wählen Sie unter *Target Data Share Account* (Data Share-Zielkonto) das Abonnement und die Ressourcengruppe für die Bereitstellung Ihrer Data Share-Instanz aus. 

   Wählen Sie für das Feld **Data Share Account** (Data Share-Konto) die Option **Neue erstellen**, falls Sie nicht über ein vorhandenes Data Share-Konto verfügen. Wählen Sie andernfalls ein vorhandenes Data Share-Konto für Ihre Datenfreigabe aus. 

   Im Feld **Received Share Name** (Name der empfangenen Freigabe) können Sie den vom Datenanbieter angegebenen Standardnamen übernehmen oder einen neuen Namen für die empfangene Freigabe angeben. 

   ![Datenfreigabe-Zielkonto](./media/target-data-share.png "Datenfreigabe-Zielkonto") 

1. Nachdem Sie den Nutzungsbedingungen zugestimmt und einen Speicherort für Ihre Freigabe angegeben haben, wählen Sie *Accept and configure* (Akzeptieren und konfigurieren) aus. Ein Freigabeabonnement wird erstellt.

   Bei einer momentaufnahmebasierten Freigabe werden Sie im nächsten Bildschirm zum Auswählen eines Zielspeicherkontos aufgefordert, in das Ihre Daten kopiert werden sollen. 

   ![Optionen zum Akzeptieren](./media/accept-options.png "Optionen zum Akzeptieren") 

   Wenn Sie die Einladung jetzt annehmen, Ihren Zieldatenspeicher aber erst später konfigurieren möchten, wählen Sie *Accept and configure later* (Akzeptieren und später konfigurieren) aus. Informationen zum späteren Konfigurieren Ihres Speichers finden Sie auf der Seite [Konfigurieren einer Datasetzuordnung für eine empfangene Freigabe in Azure Data Share](how-to-configure-mapping.md). Dort wird ausführlich beschrieben, wie Sie die Konfiguration Ihrer Datenfreigabe fortsetzen. 

   Informationen zum direkten Freigeben finden Sie auf der Seite [Konfigurieren einer Datasetzuordnung für eine empfangene Freigabe in Azure Data Share](how-to-configure-mapping.md). Dort wird ausführlich beschrieben, wie Sie die Konfiguration Ihrer Datenfreigabe fortsetzen. 

   Wählen Sie *Ablehnen*, falls Sie die Einladung nicht annehmen möchten. 

## <a name="configure-storage"></a>Konfigurieren des Speichers
1. Wählen Sie unter *Target Storage Settings* (Einstellungen für Zielspeicher) das Abonnement, die Ressourcengruppe und das Speicherkonto für den Empfang der Daten aus. 

   ![Zielspeichereinstellungen](./media/target-storage-settings.png "Zielspeicher") 

1. Aktivieren Sie die Einstellungen für Momentaufnahmen, um regelmäßige Aktualisierungen Ihrer Daten zu erhalten. Beachten Sie, dass nur dann ein Zeitplan für die Momentaufnahmeneinstellungen angezeigt wird, wenn Ihr Datenanbieter diesen in die Datenfreigabe einbezogen hat. 

   ![Momentaufnahmeeinstellungen](./media/snapshot-settings.png "Momentaufnahmeeinstellungen") 

1. Wählen Sie *Speichern* aus. 

> [!IMPORTANT]
> Wenn Sie SQL-basierte Daten in einer SQL-basierten Quelle empfangen möchten, lesen Sie die [Anleitung zum Konfigurieren einer Datasetzuordnung](how-to-configure-mapping.md), um zu erfahren, wie Sie eine SQL Server-Instanz als Ziel für das Dataset konfigurieren. 

## <a name="trigger-a-snapshot"></a>Auslösen einer Momentaufnahme
Diese Schritte sind nur für die momentaufnahmebasierte Freigabe relevant.

1. Sie können eine Momentaufnahme auf der Registerkarte „Empfangene Freigaben“ > „Details“ auslösen, indem Sie **Trigger snapshot** (Momentaufnahme auslösen) wählen. Hier können Sie eine vollständige oder inkrementelle Momentaufnahme Ihrer Daten auslösen. Wählen Sie die Option für das vollständige Kopieren, falls Sie zum ersten Mal Daten von Ihrem Datenanbieter erhalten. 

   ![Auslösen der Momentaufnahme](./media/trigger-snapshot.png "Auslösen der Momentaufnahme") 

1. Wenn der Status der letzten Ausführung *Erfolgreich* lautet, navigieren Sie zum Zieldatenspeicher, um die empfangenen Daten anzuzeigen. Wählen Sie **Datasets** aus, und klicken Sie auf den Link im Zielpfad. 

   ![Consumerdatasets](./media/consumer-datasets.png "Consumerdatasetzuordnung") 

## <a name="view-history"></a>Anzeigen des Verlaufs
Navigieren Sie zum Anzeigen eines Verlaufs Ihrer Momentaufnahmen zu „Empfangene Freigaben“ > „Verlauf“. Hier ist ein Verlauf aller Momentaufnahmen angegeben, die in den letzten 60 Tagen generiert wurden. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Sie eine Azure Data Share-Instanz akzeptieren und empfangen. Weitere Informationen zu den Konzepten von Azure Data Share finden Sie unter [Konzepte: Azure Data Share-Terminologie](terminology.md).