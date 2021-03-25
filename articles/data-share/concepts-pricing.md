---
title: Grundlagen zu Azure Data Share-Preisen
description: Erfahren Sie mehr über die Preise für Azure Data Share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: 1c2c58e206a70a3801c712df3b5582409712d3c8
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88136640"
---
# <a name="understand-azure-data-share-pricing"></a>Grundlagen zu Azure Data Share-Preisen

Für die auf momentaufnahmebasierte Freigabe erhebt Azure Data Share Gebühren für die Datasetmomentaufnahme und die Momentaufnahmeausführung. In diesem Artikel wird erläutert, wie Sie die Datasetmomentaufnahme und die Momentaufnahmeausführung anhand von Informationen zum Momentaufnahmeverlauf schätzen können. Gegenwärtig werden dem Datenanbieter die Datasetmomentaufnahme und die Momentaufnahmeausführung in Rechnung gestellt.

## <a name="dataset-snapshot"></a>Datasetmomentaufnahme

Datasetmomentaufnahme ist der Vorgang der Verschiebung des Datasets aus der Quelle in das Ziel. Wenn für eine Freigabe eine Momentaufnahme erstellt wird, ist die Momentaufnahme jedes Datasets innerhalb der Freigabe ein Datasetmomentaufnahme-Vorgang. Führen Sie die folgenden Schritte aus, um eine Liste von Datasetmomentaufnahmen anzuzeigen. 

1. Navigieren Sie im Azure-Portal zu Ihrer Data Share-Ressource.

1. Wählen Sie eine Freigabe aus „Gesendete Freigabe“ oder „Empfangene Freigabe“ aus.

1. Klicken Sie auf die Registerkarte **Verlauf**.

1. Klicken Sie auf die Startzeit einer Momentaufnahme.
 
    ![Momentaufnahmeverlauf](./media/concepts/concepts-pricing/pricing-snapshot-history.png "Momentaufnahmeverlauf") 

1. Zeigen Sie die Liste der Datasetmomentaufnahme-Vorgänge an. Jedes Zeilenelement entspricht einem Datasetmomentaufnahme-Vorgang. In diesem Beispiel sind zwei Datasetmomentaufnahmen vorhanden.

    ![Datasetmomentaufnahme-Vorgang](./media/concepts/concepts-pricing/pricing-dataset-snapshot.png "Datasetmomentaufnahme-Vorgang")

## <a name="snapshot-execution"></a>Momentaufnahmeausführung

Die Momentaufnahmeausführung umfasst die Ressourcen, die zum Verschieben eines Datasets von der Quelle zum Ziel erforderlich sind. Für jeden Datasetmomentaufnahme-Vorgang wird die Momentaufnahmeausführung folgendermaßen berechnet: Anzahl der virtuellen Kerne, multipliziert mit der Momentaufnahmedauer. Gebühren werden anteilmäßig auf Minutenbasis berechnet und aufgerundet. Die Anzahl der virtuellen Kerne wird dynamisch auf der Grundlage des Quelle-Ziel-Paares und des Datenmusters ausgewählt. Führen Sie die folgenden Schritte aus, um die Startzeit und die Endzeit der Momentaufnahme sowie die für diesen Datasetmomentaufnahme-Vorgang verwendeten virtuellen Kerne anzuzeigen.

1. Navigieren Sie im Azure-Portal zu Ihrer Data Share-Ressource.

1. Wählen Sie eine Freigabe aus „Gesendete Freigabe“ oder „Empfangene Freigabe“ aus.

1. Klicken Sie auf die Registerkarte **Verlauf**.

1. Klicken Sie auf die Startzeit einer Momentaufnahme.

    ![Momentaufnahmeverlauf](./media/concepts/concepts-pricing/pricing-snapshot-history.png "Momentaufnahmeverlauf") 

1. Klicken Sie auf den Status eines Datasetmomentaufnahme-Vorgangs.

    ![Datasetmomentaufnahme-Status](./media/concepts/concepts-pricing/pricing-snapshot-status.png "Datasetmomentaufnahme-Status")

1. Zeigen Sie die Startzeit, die Endzeit und die für diesen Datasetmomentaufnahme-Vorgang verwendeten virtuellen Kerne an. 

    ![Momentaufnahmeausführung](./media/concepts/concepts-pricing/pricing-snapshot-execution.png "Momentaufnahmeausführung")

## <a name="next-steps"></a>Nächste Schritte

- Abrufen der neuesten Preisinformationen: [Azure Data Share-Preise](https://azure.microsoft.com/pricing/details/data-share/)
- Verwenden des Azure-Preisrechners, um die voraussichtlichen Kosten zu ermitteln: [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/)
