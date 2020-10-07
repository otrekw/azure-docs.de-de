---
title: 'Schnellstart: Erstellen eines Synapse-Arbeitsbereichs'
description: Erstellen Sie anhand der Schritte in diesem Leitfaden einen Synapse-Arbeitsbereich.
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 09/03/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: 04bbab477f20ec25ccb0e975dad2b30a511346d2
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91667757"
---
# <a name="quickstart-create-a-synapse-workspace"></a>Schnellstart: Erstellen eines Synapse-Arbeitsbereichs
In dieser Schnellstartanleitung werden die Schritte zum Erstellen eines Azure Synapse-Arbeitsbereichs über das Azure-Portal erläutert.

## <a name="create-a-synapse-workspace"></a>Erstellen eines Synapse-Arbeitsbereichs

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und suchen Sie oben nach **Synapse**.
1. Wählen Sie in den Suchergebnissen unter **Dienste** den Eintrag **Azure Synapse Analytics (Arbeitsbereichsvorschau)** aus.
1. Wählen Sie **Hinzufügen** aus, um einen Arbeitsbereich mit diesen Einstellungen zu erstellen:

    |Registerkarte|Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
    |---|---|---|---|
    |Grundlagen|**Arbeitsbereichsname**|Sie können einen beliebigen Namen eingeben.| In diesem Dokument wird **myworkspace** verwendet.|
    |Grundlagen|**Region**|Wählen Sie dieselbe Region wie für das Speicherkonto aus.|

1. Zum Erstellen eines Arbeitsbereichs benötigen Sie ein ADLSGEN2-Konto. Am einfachsten ist es, ein solches Konto neu zu erstellen. Wenn Sie ein vorhandenes Konto verwenden möchten, müssen Sie einige zusätzliche Konfigurationsschritte ausführen. 
1. OPTION 1: Erstellen eines neuen ADLSGEN2-Kontos 
    1. Klicken Sie unter **Data Lake Storage Gen 2 auswählen** auf **Neu erstellen**, und geben Sie **contosolake** als Namen ein.
    1. Klicken Sie unter **Data Lake Storage Gen 2 auswählen** auf **Dateisystem**, und geben Sie **users** als Namen ein.
1. OPTION 2: Anweisungen finden Sie unter **Vorbereiten eines Speicherkontos** am Ende dieses Dokuments.
1. Ihr Azure Synapse-Arbeitsbereich verwendet dieses Speicherkonto als „primäres“ Speicherkonto und den Container zum Speichern von Arbeitsbereichsdaten. Im Arbeitsbereich werden Daten in Apache Spark-Tabellen gespeichert. Die Spark-Anwendungsprotokolle werden im Ordner **/synapse/workspacename** gespeichert.
1. Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr Arbeitsbereich steht nach wenigen Minuten zur Verfügung.

## <a name="open-synapse-studio"></a>Öffnen von Synapse Studio

Nachdem Ihr Azure Synapse-Arbeitsbereich erstellt wurde, haben Sie zwei Möglichkeiten zum Öffnen von Synapse Studio:

* Öffnen Sie Ihren Synapse-Arbeitsbereich im [Azure-Portal](https://portal.azure.com). Wählen Sie oben im Abschnitt **Übersicht** die Option **Synapse Studio starten** aus.
* Navigieren Sie zu `https://web.azuresynapse.net`, und melden Sie sich bei Ihrem Arbeitsbereich an.

## <a name="prepare-an-existing-storage-account-for-use-with-synapse-analytics"></a>Vorbereiten eines vorhandenen Speicherkontos für die Verwendung mit Synapse Analytics

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Navigieren Sie zu einem vorhandenen ADLSGEN2-Speicherkonto.
1. Wählen Sie im linken Bereich **Zugriffssteuerung (IAM)** aus. Weisen Sie anschließend die folgenden Rollen zu, bzw. stellen Sie sicher, dass sie bereits zugewiesen sind:
    * Weisen Sie sich selbst die Rolle **Besitzer** zu.
    * Weisen Sie sich selbst die Rolle **Besitzer von Speicherblobdaten** zu.
1. Wählen Sie im linken Bereich die Option **Container** aus, und erstellen Sie einen Container.
1. Sie können dem Container einen beliebigen Namen geben. In diesem Dokument hat der Container den Namen **users**.
1. Übernehmen Sie die Standardeinstellung **Öffentliche Zugriffsebene**, und wählen Sie anschließend **Erstellen** aus.

### <a name="configure-access-to-the-storage-account-from-your-workspace"></a>Konfigurieren des Zugriffs auf das Speicherkonto vom Arbeitsbereich

Verwaltete Identitäten für Ihren Azure Synapse-Arbeitsbereich verfügen unter Umständen bereits über Zugriff auf das Speicherkonto. Führen Sie die folgenden Schritte aus, um dies zu überprüfen:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) und dann das primäre Speicherkonto, das Sie für Ihren Arbeitsbereich ausgewählt haben.
1. Wählen Sie im linken Bereich **Zugriffssteuerung (IAM)** aus.
1. Weisen Sie die folgenden Rollen zu, bzw. stellen Sie sicher, dass sie bereits zugewiesen sind. Wir verwenden für die Identität und den Namen des Arbeitsbereichs den gleichen Namen.
    * Weisen Sie für die Rolle **Mitwirkender an Storage-Blobdaten** des Speicherkontos **myworkspace** als Identität des Arbeitsbereichs zu.
    * Weisen Sie **myworkspace** als Namen für den Arbeitsbereich zu.

1. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines SQL-Pools](quickstart-create-sql-pool-studio.md) 
* [Erstellen eines Apache Spark-Pools](quickstart-create-apache-spark-pool-portal.md)
* [Verwenden von SQL On-Demand](quickstart-sql-on-demand.md)
