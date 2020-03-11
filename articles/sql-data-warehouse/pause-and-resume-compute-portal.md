---
title: 'Schnellstart: Anhalten und Fortsetzen von Computeressourcen – Azure-Portal '
description: Verwenden Sie das Azure-Portal, um Computeressourcen für den SQL-Pool anzuhalten und Kosten zu sparen. Setzen Sie die Computeressourcen fort, wenn Sie das Data Warehouse verwenden möchten.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 04/18/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 49684e6844c2d9f58e9b750b12991428218e4426
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78200612"
---
# <a name="quickstart-pause-and-resume-compute-in-azure-synapse-analytics-sql-pool-in-the-azure-portal"></a>Schnellstart: Anhalten und Fortsetzen von Computeressourcen im SQL-Pool von Azure Synapse Analytics über das Azure-Portal

Verwenden Sie das Azure-Portal, um Computeressourcen für den SQL-Pool anzuhalten und Kosten zu sparen. [Setzen Sie die Computeressourcen fort](sql-data-warehouse-manage-compute-overview.md), wenn Sie das Data Warehouse verwenden möchten.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="before-you-begin"></a>Voraussetzungen

Verwenden Sie [Erstellen und Verbinden – Portal](create-data-warehouse-portal.md), um einen SQL-Pool namens **mySampleDataWarehouse** zu erstellen. 

## <a name="pause-compute"></a>Anhalten von Computeressourcen

Um Kosten zu sparen, können Sie Computeressourcen bei Bedarf anhalten und fortsetzen. Wenn Sie die Datenbank z.B. nachts oder am Wochenende nicht verwenden, können Sie sie während dieser Zeiträume anhalten und während des Tages wieder fortsetzen. Computeressourcen werden Ihnen nicht in Rechnung gestellt, während die Datenbank angehalten ist. Allerdings wird Ihnen der Speicherplatz weiter in Rechnung gestellt. 

Führen Sie die folgenden Schritte aus, um einen SQL-Pool anzuhalten:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie auf der linken Navigationsseite des Azure-Portals auf **Azure Synapse Analytics (vormals SQL DW)** .
2. Wählen Sie auf der Seite **Azure Synapse Analytics (vormals SQL DW)** die Option **mySampleDataWarehouse** aus, um den SQL-Pool zu öffnen. 
3. Beachten Sie, dass auf der Seite **mySampleDataWarehouse** für **Status** der Wert **Online** angezeigt wird.

    ![Computerressourcen online](media/pause-and-resume-compute-portal/compute-online.png)

4. Klicken Sie zum Anhalten des SQL-Pools auf die Schaltfläche **Anhalten**. 
5. Sie werden in einer Meldung aufgefordert, das Fortsetzen des Vorgangs zu bestätigen. Klicken Sie auf **Ja**.
6. Warten Sie einen Moment, und beachten Sie, dass der **Status** als **Wird angehalten** angezeigt wird.

    ![Status „Wird angehalten“](media/pause-and-resume-compute-portal/pausing.png)

7. Nach Abschluss des Anhaltevorgangs lautet der Status **Angehalten**, und für die Optionsschaltfläche wird **Starten** angezeigt.
8. Die Computeressourcen für den SQL-Pool sind nun offline. Ihnen werden erst wieder Computeressourcen in Rechnung gestellt, wenn Sie den Dienst fortsetzen.

    ![Computeressourcen offline](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>Fortsetzen von Computeressourcen

Führen Sie die folgenden Schritte aus, um einen SQL-Pool fortzusetzen:

1. Klicken Sie auf der linken Seite des Azure-Portals auf **Azure Synapse Analytics (vormals SQL DW)** .
2. Wählen Sie auf der Seite **Azure Synapse Analytics (vormals SQL DW)** die Option **mySampleDataWarehouse** aus, um die Seite des SQL-Pools zu öffnen. 
3. Beachten Sie, dass auf der Seite **mySampleDataWarehouse** für **Status** der Wert **Angehalten** angezeigt wird.

    ![Computeressourcen offline](media/pause-and-resume-compute-portal/compute-offline.png)

4. Klicken Sie zum Fortsetzen des SQL-Pools auf **Fortsetzen**. 
5. Sie werden in einer Meldung aufgefordert, den Startvorgang zu bestätigen. Klicken Sie auf **Ja**.
6. Beachten Sie, dass der **Status** als **Wird fortgesetzt** angezeigt wird.

    ![Wird fortgesetzt](media/pause-and-resume-compute-portal/resuming.png)

7. Wenn der SQL-Pool wieder online ist, lautet der Status **Online**, und für die Optionsschaltfläche wird **Anhalten** angezeigt.
8. Die Computeressourcen für den SQL-Pool sind nun online, und Sie können den Dienst verwenden. Es fallen wieder Kosten für die Computeressourcen an.

    ![Computerressourcen online](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Ihnen werden Gebühren für Ihre Data Warehouse-Einheiten und die in Ihrem Data Warehouse gespeicherte Daten in Rechnung gestellt. Diese Compute- und Speicherressourcen werden separat in Rechnung gestellt. 

- Wenn Sie die Daten im Speicher beibehalten möchten, halten Sie die Computeressourcen an.
- Wenn künftig keine Gebühren mehr anfallen sollen, können Sie den SQL-Pool löschen. 

Führen Sie die folgenden Schritte aus, um Ressourcen nach Wunsch zu bereinigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf Ihren SQL-Pool.

    ![Bereinigen von Ressourcen](media/pause-and-resume-compute-portal/clean-up-resources.png)

1. Zum Anhalten von Computeressourcen klicken Sie auf die Schaltfläche **Anhalten**. 

2. Wenn Sie den SQL-Pool entfernen möchten, damit keine Gebühren für Compute- oder Speicherressourcen anfallen, klicken Sie auf **Löschen**.

3. Wenn Sie die von Ihnen erstellte SQL Server-Instanz entfernen möchten, klicken Sie auf **sqlpoolservername.database.windows.net** und anschließend auf **Löschen**.  Seien Sie bei diesem Löschvorgang vorsichtig, da beim Löschen des Servers auch alle Datenbanken gelöscht werden, die dem Server zugewiesen sind.

4. Zum Entfernen der Ressourcengruppe klicken Sie auf **myResourceGroup**, und klicken Sie dann auf **Ressourcengruppe löschen**.


## <a name="next-steps"></a>Nächste Schritte

Sie haben die Computeressourcen für Ihren SQL-Pool angehalten und fortgesetzt. Informationen zu den nächsten Schritten finden Sie im Tutorial zum Laden von Daten.

> [!div class="nextstepaction"]
> [Tutorial: Laden des Datasets „New York Taxis“](load-data-from-azure-blob-storage-using-polybase.md)
