---
title: 'Freigeben außerhalb Ihrer Organisation (Azure-Portal): Schnellstartanleitung zu Azure Data Share'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure Data Share Daten für Kunden und Partner freigeben.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: quickstart
ms.date: 08/19/2020
ms.openlocfilehash: 41598c04af78d4366435259357d8f897ac178942
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89489934"
---
# <a name="quickstart-share-data-using-azure-data-share-in-the-azure-portal"></a>Schnellstart: Freigeben von Daten mithilfe von Azure Data Share über das Azure-Portal

In dieser Schnellstartanleitung wird beschrieben, wie Sie mit dem Azure-Portal eine neue Azure Data Share-Instanz einrichten.

## <a name="prerequisites"></a>Voraussetzungen

Azure-Abonnement: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.


## <a name="create-a-data-share-account"></a>Erstellen eines Data Share-Kontos

Erstellen Sie eine Azure Data Share-Ressource in einer Azure-Ressourcengruppe.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Klicken Sie in der linken oberen Ecke des Portals auf die Schaltfläche **Ressource erstellen** (+).

1. Suchen Sie nach *Data Share*.

1. Wählen Sie **Data Share** und dann die Option **Erstellen** aus.

1. Geben Sie die folgenden Informationen als grundlegende Details Ihrer Azure Data Share-Ressource ein. 

   **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**
   |---|---|---|
   | Subscription | Ihr Abonnement | Wählen Sie das Azure-Abonnement aus, das Sie für Ihr Data Share-Konto verwenden möchten.|
   | Ressourcengruppe | *test-resource-group* | Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. |
   | Standort | *USA (Ost) 2* | Wählen Sie eine Region für Ihr Data Share-Konto aus.
   | Name | *datashareaccount* | Geben Sie einen Namen für Ihr Data Share-Konto an. |

1. Wählen Sie **Überprüfen + erstellen** und dann **Erstellen** aus, um Ihr Data Share-Konto bereitzustellen. Die Bereitstellung eines neuen Data Share-Kontos dauert normalerweise maximal ca. 2 Minuten.

1. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressourcengruppe wechseln**.

## <a name="create-a-share"></a>Erstellen einer Freigabe

1. Navigieren Sie zu Ihrer Seite mit der Data Share-Übersicht.

   ![Freigeben von Daten](./media/share-receive-data.png "Freigeben von Daten") 

1. Wählen Sie **Start sharing your data** (Mit Freigabe der Daten beginnen).

1. Klicken Sie auf **Erstellen**.

1. Geben Sie die Details für Ihre Freigabe ein. Geben Sie einen Namen, die Art der Freigabe, eine Beschreibung der Freigabeinhalte und Nutzungsbedingungen (optional) an. 

   ![EnterShareDetails](./media/enter-share-details.png "Eingeben der Details zur Freigabe") 

1. Wählen Sie **Weiter**.

1. Wählen Sie zum Hinzufügen von Datasets zu Ihrer Freigabe **Datasets hinzufügen** aus. 

   ![Hinzufügen von Datasets zu Ihrer Freigabe](./media/datasets.png "Datasets")

1. Wählen Sie den gewünschten Datasettyp für das Hinzufügen aus. Die angezeigte Liste der Datasettypen ist abhängig von der Art der Freigabe (Momentaufnahme oder direkt), die Sie im vorherigen Schritt ausgewählt haben. Bei der Freigabe über eine Azure SQL-Datenbank- oder Azure Synapse Analytics-Instanz werden Sie zur Eingabe einiger SQL-Anmeldeinformationen aufgefordert. Authentifizieren Sie sich unter Verwendung des Benutzers, den Sie im Rahmen der Voraussetzungen erstellt haben.

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

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressource nicht mehr benötigt wird, navigieren Sie zur Seite mit der **Data Share-Übersicht**, und wählen Sie **Löschen** aus, um die Ressource zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie eine Azure Data Share-Instanz erstellen. Fahren Sie mit dem Tutorial [Akzeptieren und Empfangen von Daten](subscribe-to-data-share.md) fort, um sich darüber zu informieren, wie ein Datenconsumer eine Datenfreigabe akzeptieren und empfangen kann. 
