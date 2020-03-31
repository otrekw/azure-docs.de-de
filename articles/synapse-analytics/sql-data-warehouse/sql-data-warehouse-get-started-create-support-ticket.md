---
title: Anfordern von Kontingenterhöhungen und Erhalten von Unterstützung
description: Informationen zum Erstellen einer Supportanfrage im Azure-Portal für Azure Synapse Analytics. Anfordern von Kontingenterhöhungen oder Erhalten von Unterstützung bei der Problemlösung.
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/10/2020
author: kevinvngo
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: eaa6ceded9893b278b28d517cbddb303f8469e10
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350893"
---
# <a name="request-quota-increases-and-get-support-for-azure-synapse-analytics"></a>Anfordern von Kontingenterhöhungen und Erhalten von Unterstützung für Azure Synapse Analytics

In diesem Artikel wird beschrieben, wie Sie ein Supportticket für Azure Synapse Analytics im Azure-Portal übermitteln. Mit diesem Vorgang können Sie eine Kontingenterhöhung anfordern oder eine technische Supportanfrage an das Engineeringsupportteam senden.

## <a name="create-a-support-ticket"></a>Erstellen ein Supporttickets

Führen Sie die folgenden Schritte aus, um im Azure-Portal eine neue Supportanfrage Azure Synapse Analytics zu erstellen.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Menü die Option **Hilfe und Support** aus.

   ![Link „Hilfe und Support“](./media/sql-data-warehouse-get-started-create-support-ticket/help-plus-support.png)


1. Wählen Sie unter **Hilfe und Support** die Option **Neue Supportanfrage** aus.

    ![Erstellen einer neuen Supportanfrage](./media/sql-data-warehouse-get-started-create-support-ticket/new-support-request.png)

1. Überprüfen Sie Ihren [Azure-Supportplan](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/).

   * **Verwaltung von Abrechnung, Kontingenten und Abonnements** ist in allen Supportstufen verfügbar.
   * **Problemlösungssupport** wird in den Supportstufen [Entwickler](https://azure.microsoft.com/support/plans/developer/), [Standard](https://azure.microsoft.com/support/plans/standard/), [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) oder [Premier](https://azure.microsoft.com/support/plans/premier/) bereitgestellt. Problemlösungssupport betrifft Probleme, die bei der Verwendung von Azure auftreten und bei denen Grund zu der Annahme besteht, dass das Problem durch Microsoft verursacht wurde.
   * **Anleitung für Entwickler** und **beratende Dienstleistungen** sind für die Supportstufen [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) und [Premier](https://azure.microsoft.com/support/plans/premier/) verfügbar.

   Wenn Sie über den Supportplan „Premier“ verfügen, können Sie Probleme im Zusammenhang mit Azure Synapse Analytics auch im [Microsoft Premier-Onlineportal](https://premier.microsoft.com/) melden. Weitere Informationen zu den unterschiedlichen Supportplänen, z.B. zu Umfang, Reaktionszeiten, Preisen usw., finden Sie auf der Seite mit den [Azure-Supportplänen](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/).  Häufig gestellte Fragen zum Azure-Support finden Sie unter [Häufig gestellte Fragen zum Azure-Support](https://azure.microsoft.com/support/faq/).

1. Wählen Sie als **Problemtyp** den entsprechenden Problemtyp aus. Wählen Sie **Technisch** für Problemlösungs-Anfragen aus. Wählen Sie für Anforderungen zur Kontingenterhöhung **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.

   ![Auswählen eines Problemtyps](./media/sql-data-warehouse-get-started-create-support-ticket/select-quota-issue-type.png)  

   > [!NOTE]
   > Im weiteren Verlauf dieses Artikels werden Anforderungen zur Kontingenterhöhung behandelt. Sie können jedoch auch hier **Technisch** auswählen, um Supportanfragen zur Problemlösung zu übermitteln. Wenn Sie **Technisch** auswählen, werden Sie aufgefordert, eine Zusammenfassung anzugeben und dann einen Problemtyp zu identifizieren, indem Sie **Problemtyp auswählen** auswählen. Möglicherweise werden Ihnen Lösungen zum Beheben Ihres Problems angezeigt. Wenn das Problem mit den angegebenen Lösungen nicht behoben werden kann, wählen Sie **Next:Details** aus, und füllen Sie das Formular zum Übermitteln des Supporttickets aus.

1. Wählen Sie zum Anfordern einer Kontingenterhöhung **Azure Synapse Analytics** als **Kontingenttyp** aus. Wählen Sie anschließend **Next: Lösungen >>** .

   ![Auswählen eines Kontingenttyps](./media/sql-data-warehouse-get-started-create-support-ticket/select-quota-type.png)

1. Wählen Sie im Fenster **Details** die Option **Details angeben** aus, um zusätzliche Informationen einzugeben.

   ![Der Link „Details angeben“](./media/sql-data-warehouse-get-started-create-support-ticket/provide-details-link.png)

## <a name="quota-request-types"></a>Kontingentanforderungstypen

Wenn Sie auf **Details angeben** klicken, wird das Fenster **Kontingentdetails** angezeigt, in dem Sie zusätzliche Informationen hinzufügen können. In den folgenden Abschnitten werden die verschiedenen Kontingentanforderungen beschrieben, die für Azure Synapse Analytics zur Verfügung stehen.

### <a name="data-warehouse-units-dwus-per-server"></a>Data Warehouse-Einheiten (DWUs) pro Server

Führen Sie die folgenden Schritte aus, um eine Erhöhung der DWUs pro Server anzufordern.

1. Wählen Sie den Kontingenttyp **Data Warehouse-Einheiten (DWUs) pro Server** aus.

1. Wählen Sie in der Liste **Ressourcen** die Zielressource aus.

1. Geben Sie im Feld **Kontingent anfordern** den neuen DTU-Grenzwert ein, den Sie anfordern möchten.

   ![DWU-Kontingentdetails](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-dwus.png)

### <a name="servers-per-subscription"></a>Server pro Abonnement

Führen Sie die folgenden Schritte aus, um eine Erhöhung der Anzahl der Server pro Abonnement anzufordern.

1. Wählen Sie den Kontingenttyp **Server pro Abonnement** aus.

1. Wählen Sie in der Liste **Standort** die zu verwendende Azure-Region aus. Das Kontingent gilt pro Abonnement in jeder Region.

1. Geben Sie im Feld **Neues Kontingent** Ihre Anforderung für die maximale Anzahl von Servern in dieser Region ein.

   ![Serverkontingentdetails](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-servers.png)

### <a name="enable-subscription-access-to-a-region"></a>Aktivieren des Abonnementzugriffs auf eine Region

Einige Angebotstypen sind nicht in allen Regionen verfügbar. Unter Umständen wird beispielsweise folgender Fehler angezeigt:

`This location is not available for subscription`

Wenn Ihr Abonnement Zugriff auf eine bestimmte Region benötigt, verwenden Sie die Option **Andere Kontingentanforderung**, um den Zugriff anzufordern. Geben Sie in Ihrer Anforderung die Details zu dem Angebot und der SKU an, die Sie für die Region aktivieren möchten. Informationen zu den Angebots- und SKU-Optionen finden Sie unter [Azure Synapse Analytics – Preise](https://azure.microsoft.com/pricing/details/synapse-analytics/).

![Weitere Kontingentdetails](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-whitelisting.png)

## <a name="submit-your-request"></a>Senden der Anforderung

Der letzte Schritt besteht darin, die verbleibenden Details der Supportanfrage für die SQL-Datenbank einzugeben. Wählen Sie anschließend **Next: Bewerten + erstellen>>** aus, und klicken Sie nach dem Überprüfen der Anforderungsdetails auf **Erstellen**, um die Anforderung zu übermitteln.

## <a name="monitor-a-support-ticket"></a>Überwachen eines Supporttickets

Nachdem Sie die Supportanfrage übermittelt haben, nimmt das Azure-Supportteam Kontakt mit Ihnen auf. Klicken Sie zum Überprüfen des Anfragestatus und der Details auf dem Dashboard auf **Alle Supportanfragen**.

![Status überprüfen](./media/sql-data-warehouse-get-started-create-support-ticket/monitor-ticket.png)

## <a name="other-resources"></a>Weitere Ressourcen

Sie können sich außerdem an der Azure Synapse Analytics-Community unter [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-synapse+or+azure-sql-data-warehouse) oder über das [Azure SQL Data Warehouse-MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/) beteiligen.

