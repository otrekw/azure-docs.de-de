---
title: Datei einfügen
description: include file
services: backup
author: v-amallick
manager: carmonm
ms.service: backup
ms.topic: include
ms.date: 06/01/2021
ms.author: v-amallick
ms.custom: include file
ms.openlocfilehash: 7d354ce3e69f075d2f523a75ba96f9a6efb7d797
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110783575"
---
## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Ein Recovery Services-Tresor ist eine Verwaltungsentität, unter der die im Laufe der Zeit erstellten Wiederherstellungspunkte gespeichert werden. Darüber hinaus verfügt er über eine Benutzeroberfläche zum Durchführen von sicherungsbezogenen Vorgängen. Hierzu gehören das Erstellen von bedarfsgesteuerten Sicherungen, Durchführen von Wiederherstellungen und Erstellen von Sicherungsrichtlinien.

Führen Sie die folgenden Schritte aus, um einen Recovery Services-Tresor zu erstellen:

1. Melden Sie sich bei Ihrem Abonnement im [Azure-Portal](https://portal.azure.com/) an.

1. Suchen Sie im Azure-Portal nach **Backup Center**, und navigieren Sie zum Dashboard **Backup Center**.

    ![Auswählen von „Backup Center“](../includes/media/backup-create-rs-vault/backup-center-search-backup-center.png)

1. Wählen Sie auf der Registerkarte **Übersicht** die Option **+Tresor** aus.

    ![Erstellen eines Tresors](./media/backup-create-rs-vault/backup-center-create-vault.png) 

1. Wählen Sie **Recovery Services-Tresor** aus, und klicken Sie auf **Weiter**.

    ![Auswählen des Recovery Services-Tresors](./media/backup-create-rs-vault/backup-center-select-recovery-services-vault.png) 

1. Das Dialogfeld **Recovery Services-Tresor** wird geöffnet. Legen Sie Werte für **Name**, **Abonnement**, **Ressourcengruppe** und **Speicherort** fest.

    ![Konfigurieren des Recovery Services-Tresors](./media/backup-create-rs-vault/backup-center-add-vault-details.png)

   - **Name**: Geben Sie einen Anzeigenamen ein, über den der Tresor identifiziert wird. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der mindestens zwei, aber nicht mehr als 50 Zeichen enthält. Der Name muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.
   - **Abonnement**: Wählen Sie das zu verwendende Abonnement aus. Wenn Sie nur in einem Abonnement Mitglied sind, wird dessen Name angezeigt. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Geschäfts-, Schul- oder Unikonto mehreren Azure-Abonnements zugeordnet ist.
   - **Ressourcengruppe**: Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. Um eine Liste der verfügbaren Ressourcengruppen in Ihrem Abonnement anzuzeigen, wählen Sie **Vorhandene verwenden** und dann eine Ressource in der Dropdownliste aus. Wählen Sie zum Erstellen einer neuen Ressourcengruppe **Neu erstellen** aus, und geben Sie den Namen ein. Weitere Informationen zu Ressourcengruppen finden Sie unter [Azure Resource Manager – Übersicht](../articles/azure-resource-manager/management/overview.md).
   - **Standort**: Wählen Sie die geografische Region für den Tresor aus. Um einen Tresor zum Schutz von Datenquellen zu erstellen, *muss* sich dieser in derselben Region wie die Datenquelle befinden.

      > [!IMPORTANT]
      > Wenn Sie den Speicherort der Datenquelle nicht kennen, schließen Sie das Dialogfeld. Navigieren Sie zur Liste Ihrer Ressourcen im Portal. Falls Sie über Datenquellen in mehreren Regionen verfügen, erstellen Sie für jede Region einen Recovery Services-Tresor. Erstellen Sie den Tresor am ersten Speicherort, bevor Sie den Tresor für einen anderen Speicherort erstellen. Das Angeben von Speicherkonten zum Speichern der Sicherungsdaten ist nicht erforderlich. Der Recovery Services-Tresor und Azure Backup führen diesen Schritt automatisch aus.
      >
      >

1. Wählen Sie nach dem Angeben der Werte die Option **Überprüfen + erstellen** aus.

    ![Screenshot, der die Schaltfläche „Überprüfen und erstellen“ im Prozess zum Erstellen eines Recovery Services-Tresors zeigt.](./media/backup-create-rs-vault/review-and-create.png)

1. Wenn Sie den Recovery Services-Tresor erstellen möchten, wählen Sie **Erstellen** aus.

    ![Erstellen des Recovery Services-Tresors](./media/backup-create-rs-vault/click-create-button.png)

    Es kann einige Zeit dauern, denn Recovery Services-Tresor zu erstellen. Verfolgen Sie die Statusbenachrichtigungen im **Benachrichtigungsbereich** in der oberen rechten Ecke im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor nicht angezeigt werden, wählen Sie **Aktualisieren**.

     ![Aktualisieren der Liste der Sicherungstresore](./media/backup-create-rs-vault/refresh-button.png)

>[!IMPORTANT]
> Es wird dringend empfohlen, vor dem Konfigurieren von Sicherungen im Tresor die Standardeinstellungen für **Speicherreplikationstyp** und **Sicherheitseinstellungen** zu überprüfen. Weitere Informationen finden Sie im Abschnitt [Festlegen der Speicherredundanz](../articles/backup/backup-create-rs-vault.md#set-storage-redundancy).