---
title: Erstellen eines Supporttickets
description: Sie erfahren, wie Sie in Azure Synapse Analytics ein Supportticket erstellen.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 9623a878ea009ec7363501c09c324baa6d0fa7ad
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78195678"
---
# <a name="how-to-create-a-support-ticket-for-azure-synapse-analytics"></a>Sie erfahren, wie Sie in Azure Synapse Analytics ein Supportticket erstellen.
Bei Problemen mit Azure Synapse Analytics ist es ratsam, ein Supportticket zu erstellen, damit Sie Hilfe von unserem Entwicklungssupportteam erhalten können.

## <a name="create-a-support-ticket"></a>Erstellen ein Supporttickets
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/).
1. Klicken Sie auf der Startseite auf die Registerkarte **Hilfe und Support**.
   
    ![Hilfe und Support](./media/sql-data-warehouse-get-started-create-support-ticket/main-page.png)

1. Überprüfen Sie Ihren [Azure-Supportplan](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/).
   
   * **Verwaltung von Abrechnung, Kontingenten und Abonnements** ist in allen Supportstufen verfügbar.
   * **Problemlösungssupport** wird in den Supportstufen [Entwickler](https://azure.microsoft.com/support/plans/developer/), [Standard](https://azure.microsoft.com/support/plans/standard/), [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) oder [Premier](https://azure.microsoft.com/support/plans/premier/) bereitgestellt. Problemlösungssupport betrifft Probleme, die bei der Verwendung von Azure auftreten und bei denen Grund zu der Annahme besteht, dass das Problem durch Microsoft verursacht wurde.
   * **Anleitung für Entwickler** und **beratende Dienstleistungen** sind für die Supportstufen [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) und [Premier](https://azure.microsoft.com/support/plans/premier/) verfügbar. 
     
     Wenn Sie über den Supportplan „Premier“ verfügen, können Sie Probleme im Zusammenhang mit SQL Data Warehouse auch im [Microsoft Premier-Onlineportal](https://premier.microsoft.com/) melden. Weitere Informationen zu den unterschiedlichen Supportplänen, z.B. zu Umfang, Reaktionszeiten, Preisen usw., finden Sie auf der Seite mit den [Azure-Supportplänen](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/).  Häufig gestellte Fragen zum Azure-Support finden Sie unter [Häufig gestellte Fragen zum Azure-Support](https://azure.microsoft.com/support/faq/).
1. Wählen Sie auf der Seite **Hilfe und Support** die Option **Neue Supportanfrage** aus. Wählen Sie im Dropdownmenü einen Problemtyp aus. Füllen Sie dann die Informationen auf der Registerkarte **Grundlagen** aus. Geben Sie eine **Zusammenfassung** Ihres Problems ein, wählen Sie im Menü einen **Problemtyp** und dann „Speichern“ aus.

    ![Hilfe und Support](./media/sql-data-warehouse-get-started-create-support-ticket/issue-type.png)

   > [!NOTE]
   > Standardmäßig verfügt jeder SQL-Server (z.B. myserver.database.windows.net) über ein **DTU-Kontingent** in Höhe von 45.000. Bei diesem Kontingentwert handelt es sich einfach um ein Sicherheitslimit. Sie können Ihr Kontingent erhöhen, indem Sie ein Supportticket erstellen und als Anfragetyp *Kontingent* wählen. Multiplizieren Sie zum Berechnen Ihrer DTU-Anforderungen die Anzahl der insgesamt benötigten [DWUs](sql-data-warehouse-overview-what-is.md) mit 7,5. Wenn Sie beispielsweise zwei DW6000-Einheiten auf einer SQL Server-Instanz hosten möchten, sollten Sie ein DTU-Kontingent in Höhe von 90.000 anfordern.  Sie können den aktuellen DTU-Verbrauch im Portal auf dem Blatt „SQL-Server“ anzeigen. Sowohl angehaltene als auch nicht angehaltene Datenbanken werden in das DTU-Kontingent eingerechnet. 
   > 

1. Möglicherweise werden Ihnen Lösungen zum Beheben Ihres Problems angezeigt. Wenn das Problem mit den vorgestellten Lösungen nicht behoben werden kann, wählen Sie **Weiter: Details** aus. Übermitteln Sie die Details Ihres Problems und Ihre Kontaktinformationen. Wählen Sie **Weiter: Überprüfen + erstellen**
![Details](./media/sql-data-warehouse-get-started-create-support-ticket/details.png) aus.

    
1. Überprüfen Sie Ihre Informationen, und wählen Sie unten im Formular **Erstellen** aus, um die Supportanfrage zu übermitteln.

## <a name="monitor-a-support-ticket"></a>Überwachen eines Supporttickets
Nachdem Sie die Supportanfrage übermittelt haben, nimmt das Azure-Supportteam Kontakt mit Ihnen auf. Klicken Sie zum Überprüfen des Anfragestatus und der Details auf dem Dashboard auf **Alle Supportanfragen**.

![Status überprüfen](./media/sql-data-warehouse-get-started-create-support-ticket/monitor-ticket.png)

## <a name="other-resources"></a>Weitere Ressourcen
Sie können sich außerdem an der SQL Data Warehouse-Community unter [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw/) oder über das [Azure SQL Data Warehouse-MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/) beteiligen.

 
