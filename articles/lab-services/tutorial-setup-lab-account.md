---
title: Einrichten eines Lab-Kontos mit Azure Lab Services | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein Lab-Konto mit Azure Lab Services einrichten, einen Lab-Ersteller hinzufügen und Marketplace-Images für die Verwendung durch Labs im Lab-Konto angeben.
ms.topic: tutorial
ms.date: 06/26/2020
ms.openlocfilehash: fba4dbc5386407bd796606d86a5b7bdc7c10fd61
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85445065"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Tutorial: Einrichten eines Labkontos mit Azure Lab Services
In Azure Lab Services fungiert ein Lab-Konto als zentrales Konto, unter dem die Labs Ihrer Organisation verwaltet werden. In Ihrem Lab-Konto können Sie anderen Benutzern die Berechtigung zum Erstellen von Labs erteilen und Richtlinien festlegen, die für alle Labs unter dem Lab-Konto gelten. In diesem Tutorial erfahren Sie, wie Sie ein Labkonto erstellen. 

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Erstellen eines Lab-Kontos
> * Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="create-a-lab-account"></a>Erstellen eines Lab-Kontos
Die folgenden Schritte veranschaulichen, wie Sie Azure-Portal verwenden, um ein Lab-Konto in Azure Lab Services zu erstellen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü links **Alle Dienste** aus. Wählen Sie unter **Kategorien** die Option **DevOps** aus. Wählen Sie anschließend **Lab Services** aus. Wenn Sie das Sternchen (`*`) neben **Lab-Dienste** auswählen, wird die Option im linken Menü dem Abschnitt **FAVORITEN** hinzugefügt. Beim nächsten Mal wählen Sie **Lab-Dienste** unter **FAVORITEN** aus.

    ![Alle Dienste -> Lab-Dienste](./media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Wählen Sie auf der Seite **Lab Services** auf der Symbolleiste **Hinzufügen** aus. Wählen Sie alternativ die Schaltfläche **Labkonto erstellen** auf der Seite aus. 

    ![Auswählen von „Hinzufügen“ auf der Seite „Labkonten“](./media/tutorial-setup-lab-account/add-lab-account-button.png)
4. Führen Sie auf der Seite **Labkonto erstellen** auf der Registerkarte **Grundlagen** die folgenden Schritte aus: 
    1. Geben Sie einen **Lab-Kontonamen** ein. 
    2. Wählen Sie das **Azure-Abonnement** aus, in dem Sie das Lab-Konto erstellen möchten.
    3. Wählen Sie unter **Ressourcengruppe** eine vorhandene Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein.
    4. Wählen Sie unter **Standort** einen Standort oder eine Region aus, an dem bzw. in der das Labkonto erstellt werden soll. 

        ![Labkonto: Seite „Grundlagen“](./media/tutorial-setup-lab-account/lab-account-basics-page.png)
    5. Klicken Sie auf **Überprüfen + erstellen**.
    6. Überprüfen Sie die Zusammenfassung, und wählen Sie **Erstellen** aus. 

        ![Überprüfen und erstellen > Erstellen](./media/tutorial-setup-lab-account/create-button.png)    
5. Erweitern Sie nach Abschluss der Bereitstellung die Option **Nächste Schritte**, und wählen Sie **Zu Ressource wechseln** aus. 

    ![Seite „Zu Ressource wechseln“](./media/tutorial-setup-lab-account/go-to-lab-account.png)
6. Vergewissern Sie sich, dass die Seite **Labkonto** angezeigt wird. 

    ![Seite des Lab-Kontos](./media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“
Zum Einrichten eines Classroom-Labs in einem Labkonto muss der Benutzer Mitglied der Rolle **Ersteller des Labs** für das Labkonto sein. Um Lehrkräften die Berechtigung zum Erstellen von Labs für ihre Klassen zu erteilen, fügen Sie sie zur Rolle **Lab-Ersteller** hinzu:

> [!NOTE]
> Das zum Erstellen des Labkontos verwendete Konto wird dieser Rolle automatisch hinzugefügt. Wenn Sie in diesem Tutorial zum Erstellen eines Classroom-Labs das gleiche Benutzerkonto verwenden möchten, überspringen Sie diesen Schritt. 

1. Wählen Sie auf der Seite **Lab-Konto** die Option **Zugriffssteuerung (IAM)** aus, und wählen Sie auf der Symbolleiste **+Hinzufügen** und dann **+ Rollenzuweisung hinzufügen** aus. 

    ![Zugriffssteuerung > Schaltfläche „Rollenzuweisung hinzufügen“](./media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. Wählen Sie auf der Seite **Rollenzuweisung hinzufügen** als **Rolle** die Option **Ersteller des Labs** aus. Wählen Sie den Benutzer aus, den Sie der Rolle „Ersteller des Labs“ hinzufügen möchten, und klicken Sie auf **Speichern**. 

    ![Hinzufügen des Lab-Erstellers](./media/tutorial-setup-lab-account/add-lab-creator.png)


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein Lab-Konto erstellt. Im nächsten Tutorial erfahren Sie, wie Sie als Lehrkraft ein Classroom-Lab erstellen:

> [!div class="nextstepaction"]
> [Einrichten eines Classroom-Labs](tutorial-setup-classroom-lab.md)

