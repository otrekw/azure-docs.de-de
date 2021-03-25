---
title: Anfordern von Kontingenterhöhungen und Erhalten von Unterstützung
description: Informationen zum Erstellen einer Supportanfrage im Azure-Portal für Azure Synapse Analytics. Anfordern von Kontingenterhöhungen oder Erhalten von Unterstützung bei der Problemlösung.
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 03/10/2020
author: gaursa
ms.author: gaursa
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: b5b7563b49c6f0eb155223026797f8306e642f5a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104585654"
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

1. Wählen Sie im Fenster **Details** die Option **Details eingeben** aus, um zusätzliche Informationen einzugeben.

   ![Der Link „Details angeben“](./media/sql-data-warehouse-get-started-create-support-ticket/provide-details-link.png)

## <a name="quota-request-types"></a>Kontingentanforderungstypen

Wenn Sie **Details eingeben** auswählen, wird das Fenster **Kontingentdetails** angezeigt, in dem Sie zusätzliche Informationen hinzufügen können. In den folgenden Abschnitten werden die verschiedenen Kontingentanforderungen beschrieben, die für Azure Synapse Analytics zur Verfügung stehen.

### <a name="synapse-sql-pool-data-warehouse-units-dwus-per-server"></a>Synapse SQL-Pool: Data Warehouse-Einheiten (DWUs) pro Server

Führen Sie die folgenden Schritte aus, um eine Erhöhung der DWUs pro Server anzufordern.

1. Wählen Sie den Kontingenttyp **Synapse SQL pool DWUs per server** (Synapse SQL-Pool: DWUs pro Server) aus.

1. Wählen Sie in der Dropdownliste **Ressource** die Ressource für die Kontingenterhöhung aus.

1. Geben Sie im Abschnitt **Kontingent anfordern** das neue Kontingent ein.

1. Wählen Sie **Speichern und fortfahren** aus.

   ![DWU-Kontingentdetails](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-dwus.png)


### <a name="servers-per-subscription"></a>Server pro Abonnement

Wenn Sie eine Erhöhung der Serveranzahl pro Abonnement anfordern möchten, gehen Sie wie folgt vor:

1. Wählen Sie den Kontingenttyp **SQL Servers per subscription** (SQL Server-Instanzen pro Abonnement) aus.

1. Wählen Sie in der Liste **Standort** die zu verwendende Azure-Region aus. Das Kontingent gilt pro Abonnement in jeder Region.

1. Geben Sie im Feld **Kontingent anfordern** Ihre Anforderung für die maximale Anzahl von Servern in dieser Region ein.

   ![Serverkontingentdetails](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-servers.png)



1. Wählen Sie **Speichern und fortfahren** aus.

Einige Angebotstypen sind nicht in allen Regionen verfügbar. Es wird möglicherweise der folgende Fehler angezeigt:

![Regionszugriffsfehler](./media/sql-data-warehouse-get-started-create-support-ticket/region-access-error.png)

### <a name="enable-subscription-access-to-a-region"></a>Aktivieren des Abonnementzugriffs auf eine Region

Gehen Sie wie folgt vor, um den Regionszugriff für ein Abonnement zu aktivieren:  

1. Wählen Sie den Kontingenttyp **Synapse SQL pool (data warehouse) region access** (Synapse SQL-Pool: Regionszugriff (Data Warehouse)) aus.

1. Wählen Sie in der Dropdownliste **Standort** einen Standort aus, um die Region auszuwählen.

1. Geben Sie im Abschnitt **DWU required** (DWU-Bedarf) Ihren DWU-Leistungsbedarf an.

1. Geben Sie unter **Description of business requirements** (Beschreibung der Geschäftsanforderungen) eine entsprechende Beschreibung ein. 

1. Wählen Sie **Speichern und fortfahren** aus.

![Regionszugriff](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-region.png)


### <a name="for-other-quota-requests"></a>Sonstige Kontingentanforderungen

Wählen Sie für andere Arten von Kontingentanforderungen im Dropdownmenü für den Kontingenttyp die Option **Andere Kontingentanforderung** aus:

![Weitere Kontingentdetails](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details.png)

## <a name="submit-your-request"></a>Senden der Anforderung

Der letzte Schritt besteht darin, die verbleibenden Details der Supportanfrage für die SQL-Datenbank einzugeben. Wählen Sie anschließend **Weiter: Überprüfen + erstellen >>** aus.

![„Überprüfen + erstellen“: Details](./media/sql-data-warehouse-get-started-create-support-ticket/review-create-details.png)

Wählen Sie nach dem Überprüfen der Anforderungsdetails **Erstellen** aus, um die Anforderung zu übermitteln.

![Erstellen eines Tickets](./media/sql-data-warehouse-get-started-create-support-ticket/create-ticket.png)

## <a name="monitor-a-support-ticket"></a>Überwachen eines Supporttickets

Nachdem Sie die Supportanfrage übermittelt haben, nimmt das Azure-Supportteam Kontakt mit Ihnen auf. Wählen Sie zum Überprüfen des Anfragestatus und der Details auf dem Dashboard die Option **Alle Supportanfragen** aus.

![Status überprüfen](./media/sql-data-warehouse-get-started-create-support-ticket/monitor-ticket.png)

## <a name="other-resources"></a>Weitere Ressourcen

Sie können auch Kontakt mit der Azure Synapse Analytics-Community aufnehmen – entweder über [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-synapse+or+azure-sql-data-warehouse) oder über die [Q&A-Seite von Microsoft für Azure Synapse Analytics](/answers/topics/azure-synapse-analytics.html).