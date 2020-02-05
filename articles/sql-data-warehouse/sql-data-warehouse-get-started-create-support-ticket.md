---
title: Erstellen eines Supporttickets
description: Sie erfahren, wie Sie in Azure SQL Data Warehouse ein Supportticket erstellen.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 04fd6267782ab89e12288dadb64d28d1ef1746b5
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76717840"
---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Gewusst wie: Erstellen eines Supporttickets für SQL Data Warehouse
Bei Problemen mit Ihrer Instanz von SQL Data Warehouse ist es ratsam, ein Supportticket zu erstellen, damit Sie Hilfe von unserem Entwicklungssupportteam erhalten können.

## <a name="create-a-support-ticket"></a>Erstellen ein Supporttickets
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/).
2. Klicken Sie auf der Startseite auf die Registerkarte **Hilfe und Support**.
   
    ![Hilfe und Support](./media/sql-data-warehouse-get-started-create-support-ticket/MainPage.PNG)
3. Klicken Sie auf dem Blatt „Hilfe und Support“ auf **Neue Supportanfrage**, und füllen Sie das Blatt **Grundlagen** aus.

   Wählen Sie Ihren [Azure-Supportplan](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/).
   
   * **Verwaltung von Abrechnung, Kontingenten und Abonnements** ist in allen Supportstufen verfügbar.
   * **Problemlösungssupport** wird in den Supportstufen [Entwickler](https://azure.microsoft.com/support/plans/developer/), [Standard](https://azure.microsoft.com/support/plans/standard/), [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) oder [Premier](https://azure.microsoft.com/support/plans/premier/) bereitgestellt. Problemlösungssupport betrifft Probleme, die bei der Verwendung von Azure auftreten und bei denen Grund zu der Annahme besteht, dass das Problem durch Microsoft verursacht wurde.
   * **Anleitung für Entwickler** und **beratende Dienstleistungen** sind für die Supportstufen [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) und [Premier](https://azure.microsoft.com/support/plans/premier/) verfügbar. 
     
     Wenn Sie über den Supportplan „Premier“ verfügen, können Sie Probleme im Zusammenhang mit SQL Data Warehouse auch im [Microsoft Premier-Onlineportal](https://premier.microsoft.com/) melden. Weitere Informationen zu den unterschiedlichen Supportplänen, z.B. zu Umfang, Reaktionszeiten, Preisen usw., finden Sie auf der Seite mit den [Azure-Supportplänen](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/).  Häufig gestellte Fragen zum Azure-Support finden Sie unter [Häufig gestellte Fragen zum Azure-Support](https://azure.microsoft.com/support/faq/).  
        
     ![Blatt „Grundlagen“](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_1.PNG)
     ![Blatt1 „Grundlagen“](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_2.PNG)
4. Füllen Sie das Blatt **Problem** aus.

    ![Problem_blade](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_3.PNG)
   
   > [!NOTE]
   > Standardmäßig verfügt jeder SQL-Server (z.B. myserver.database.windows.net) über ein **DTU-Kontingent** in Höhe von 45.000. Bei diesem Kontingentwert handelt es sich einfach um ein Sicherheitslimit. Sie können Ihr Kontingent erhöhen, indem Sie ein Supportticket erstellen und als Anfragetyp *Kontingent* wählen. Multiplizieren Sie zum Berechnen Ihrer DTU-Anforderungen die Anzahl der insgesamt benötigten [DWUs](sql-data-warehouse-overview-what-is.md) mit 7,5. Wenn Sie beispielsweise zwei DW6000-Einheiten auf einer SQL Server-Instanz hosten möchten, sollten Sie ein DTU-Kontingent in Höhe von 90.000 anfordern.  Sie können den aktuellen DTU-Verbrauch im Portal auf dem Blatt „SQL-Server“ anzeigen. Sowohl angehaltene als auch nicht angehaltene Datenbanken werden in das DTU-Kontingent eingerechnet. 
   > 
   > 
   
5. Geben Sie Ihre **Kontaktinformationen** ein.

   ![Contact_information](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_4.PNG)

    
6. Klicken Sie auf **Erstellen** , um die Supportanfrage zu übermitteln.

## <a name="monitor-a-support-ticket"></a>Überwachen eines Supporttickets
Nachdem Sie die Supportanfrage übermittelt haben, nimmt das Azure-Supportteam Kontakt mit Ihnen auf. Klicken Sie zum Überprüfen des Anfragestatus und der Details auf dem Dashboard auf **Alle Supportanfragen**.

![Status überprüfen](./media/sql-data-warehouse-get-started-create-support-ticket/Monitor_ticket.PNG)

## <a name="other-resources"></a>Weitere Ressourcen
Sie können sich außerdem an der SQL Data Warehouse-Community unter [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw/) oder über das [Azure SQL Data Warehouse-MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/) beteiligen.

 
