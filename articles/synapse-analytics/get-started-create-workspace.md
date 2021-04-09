---
title: 'Schnellstart: Erste Schritte zum Erstellen eines Synapse-Arbeitsbereichs'
description: In diesem Tutorial erfahren Sie, wie Sie einen Synapse-Arbeitsbereich, einen dedizierten SQL-Pool und einen serverlosen Apache Spark-Pool erstellen.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 03/17/2021
ms.openlocfilehash: ee8a192bcfe20c77e47ee9c89dfc3286a979e358
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "104722292"
---
# <a name="creating-a-synapse-workspace"></a>Erstellen eines Synapse-Arbeitsbereichs

In diesem Tutorial erfahren Sie, wie Sie einen Synapse-Arbeitsbereich, einen dedizierten SQL-Pool und einen serverlosen Apache Spark-Pool erstellen. 

## <a name="prerequisites"></a>Voraussetzungen

Um die Schritte dieses Tutorials ausführen zu können, benötigen Sie Zugriff auf eine Ressourcengruppe, für die Ihnen die Rolle **Besitzer** zugewiesen wurde. Erstellen Sie den Synapse-Arbeitsbereich in dieser Ressourcengruppe.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Erstellen eines Synapse-Arbeitsbereichs im Azure-Portal

### <a name="start-the-process"></a>Starten des Prozesses
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Geben Sie in der Suchleiste **Synapse** ein, und drücken Sie nicht die EINGABETASTE.
1. Wählen Sie in den Suchergebnissen unter **Dienste** den Eintrag **Azure Synapse Analytics** aus.
1. Wählen Sie **Hinzufügen** aus, um einen Arbeitsbereich zu erstellen.

## <a name="basics-tab--project-details"></a>Registerkarte „Grundlegende Einstellungen“ > „Projektdetails“
1. Füllen Sie auf der Registerkarte **Grundlagen** unter **Projektdetails** die folgenden Felder aus:
      1. **Abonnement**: Wählen Sie ein beliebiges Abonnement.
      2. **Ressourcengruppe**: Wählen Sie eine beliebige Ressourcengruppe.
      3. **Ressourcengruppe**: Lassen Sie dieses Feld leer.


## <a name="basics-tab--workspace-details"></a>Registerkarte „Grundlegende Einstellungen“ > „Details zum Arbeitsbereich“
1. Füllen Sie auf der Registerkarte **Grundlagen** unter **Arbeitsbereichsdetails** die folgenden Felder aus:
      1. **Arbeitsbereichsname**: Wählen Sie einen beliebigen global eindeutigen Namen. In diesem Tutorial wird **myworkspace** verwendet.
      1. **Region**: Wählen Sie eine beliebige Region.
      1. **Auswählen von Data Lake Storage Gen 2**
        1. Klicken Sie auf die Schaltfläche **Aus Abonnement**.
        1. Klicken Sie unter **Kontoname** auf **Neu erstellen**, und geben Sie für das neue Speicherkonto den Namen **contosolake** oder einen ähnlichen Namen ein. Dieser Name muss eindeutig sein.
        1. Klicken Sie unter **Dateisystemname** auf **Neu erstellen**, und geben Sie den Namen **Benutzer** ein. Dadurch wird ein Speichercontainer namens **Benutzer** erstellt. Der Arbeitsbereich verwendet dieses Speicherkonto als primäres Speicherkonto für Spark-Tabellen und Spark-Anwendungsprotokolle.
        1. Aktivieren Sie das Kästchen „Mir die Rolle "Mitwirkender an Storage-Blobdaten" für das Data Lake Storage Gen2-Konto zuweisen“. 

### <a name="completing-the-process"></a>Abschließen des Vorgangs
5. Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr Arbeitsbereich steht nach wenigen Minuten zur Verfügung.

> [!NOTE]
> Wie Sie Arbeitsbereichsfunktionen für einen vorhandenen dedizierten SQL-Pool (ehemals SQL DW) aktivieren, erfahren Sie unter [Aktivieren von Synapse-Arbeitsbereichsfunktionen für einen dedizierten SQL-Pool (ehemals SQL DW)](./sql-data-warehouse/workspace-connected-create.md).


## <a name="open-synapse-studio"></a>Öffnen von Synapse Studio

Nachdem Ihr Azure Synapse-Arbeitsbereich erstellt wurde, haben Sie zwei Möglichkeiten zum Öffnen von Synapse Studio:

* Öffnen Sie Ihren Synapse-Arbeitsbereich im [Azure-Portal](https://portal.azure.com), und wählen Sie im Abschnitt **Übersicht** des Synapse-Arbeitsbereichs im Feld „Synapse Studio öffnen“ den Eintrag **Öffnen** aus.
* Navigieren Sie zu `https://web.azuresynapse.net`, und melden Sie sich bei Ihrem Arbeitsbereich an.











## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren mithilfe eines serverlosen SQL-Pools](get-started-analyze-sql-on-demand.md)
