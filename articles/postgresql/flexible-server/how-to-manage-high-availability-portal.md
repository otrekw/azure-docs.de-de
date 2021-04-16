---
title: Verwalten zonenredundanter Hochverfügbarkeit – Azure-Portal – Azure Database for PostgreSQL – Flexible Server
description: In diesem Artikel wird beschrieben, wie Sie die zonenredundante Hochverfügbarkeit in Azure Database for PostgreSQL – Flexible Server im Azure-Portal aktivieren oder deaktivieren.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: fc1bca1265139a438fad86bfce770026866d9a2f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "90930315"
---
# <a name="manage-zone-redundant-high-availability-in-flexible-server"></a>Verwalten zonenredundanter Hochverfügbarkeit in flexiblen Servern

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

In diesem Artikel wird beschrieben, wie Sie die zonenredundante Hochverfügbarkeit in Azure Database for PostgreSQL – Flexible Server im Azure-Portal aktivieren oder deaktivieren.

Die Hochverfügbarkeitsfunktion stellt ein physisch getrenntes primäres und Standbyreplikat in unterschiedlichen Zonen bereit. Weitere Detailinformationen finden Sie unter [Dokumentation zu Hochverfügbarkeitskonzepten](./concepts-high-availability.md). Sie können die Hochverfügbarkeit zum Zeitpunkt der Erstellung eines flexiblen Servers oder nach dessen Erstellung aktivieren. Auf dieser Seite finden Sie Richtlinien zum Aktivieren oder Deaktivieren der Hochverfügbarkeit. Durch diesen Vorgang werden keine Ihrer anderen Einstellungen geändert, einschließlich der VNET-Konfiguration, der Firewalleinstellungen und der Sicherungsaufbewahrung. Analog dazu ist das Aktivieren und Deaktivieren der Hochverfügbarkeit ein Onlinevorgang, der sich nicht auf die Konnektivität und die Vorgänge Ihrer Anwendung auswirkt.

## <a name="pre-requisites"></a>Voraussetzungen

Zonenredundante Hochverfügbarkeit ist nur in Regionen verfügbar, in denen mehrere Zonen unterstützt werden. 

## <a name="enable-high-availability-during-server-creation"></a>Aktivieren von Hochverfügbarkeit während der Servererstellung

In diesem Abschnitt finden Sie spezifische Informationen für auf Hochverfügbarkeit bezogene Felder. Sie können diese Schritte ausführen, um Hochverfügbarkeit während der Erstellung Ihres flexiblen Servers bereitzustellen.

1.  Wählen Sie im[Azure-Portal](https://portal.azure.com/) die Option „Flexibler Server“ aus, und klicken Sie auf „Erstellen“.  Ausführliche Informationen zum Ausfüllen von Details wie **Abonnement**, **Ressourcengruppe**, **Servername**, **Region** und anderen Feldern finden Sie in der Dokumentation zur Vorgehensweise beim Erstellen des Servers.
   
    :::image type="content" source="./media/how-to-manage-high-availability-portal/subscription-region.png" alt-text="Anzeigen von Abonnement und der Region":::

2.  Wählen Sie Ihre **Verfügbarkeitszone** aus. Dies ist hilfreich, wenn Sie Ihre Anwendung in derselben Verfügbarkeitszone wie der der Datenbank platzieren möchten, um die Wartezeit zu verringern. Wählen Sie **Keine Präferenz** aus, wenn der flexible Server in einer beliebigen Verfügbarkeitszone bereitgestellt werden soll.
    ![AZ Selection]() :::image type="content" source="./media/how-to-manage-high-availability-portal/zone-selection.png" alt-text="Verfügbarkeitszonenauswahl":::  

3.  Aktivieren Sie das Kontrollkästchen für **Zonenredundante Hochverfügbarkeit** in der Option „Verfügbarkeit“.

    :::image type="content" source="./media/how-to-manage-high-availability-portal/high-availability-checkbox.png" alt-text="Kontrollkästchen „Hochverfügbarkeit“":::

4.  Wenn Sie die Standardwerte für „Compute“ und „Speicher“ ändern möchten, klicken Sie auf  **Server konfigurieren**.
 
    :::image type="content" source="./media/how-to-manage-high-availability-portal/configure-server.png" alt-text="Server konfigurieren: Compute und Speicher":::  

5.  Wenn die Option „Hochverfügbarkeit“ aktiviert ist, steht die Ebene „Burstfähig“ nicht zur Auswahl. Sie können die Computeebene **Allgemeiner Zweck** oder **Arbeitsspeicheroptimiert** auswählen. Anschließend können Sie in der Dropdownliste für Ihre Auswahl **Computegröße** auswählen.

    :::image type="content" source="./media/how-to-manage-high-availability-portal/select-compute.png" alt-text="Auswahl der Computeebene":::  


6.  Wählen Sie die **Speichergröße** in GiB mithilfe der Schiebereglerleiste aus, und wählen Sie den **Aufbewahrungszeitraum für Sicherungen** zwischen 7 Tagen und 35 Tagen aus.
   
    :::image type="content" source="./media/how-to-manage-high-availability-portal/storage-backup.png" alt-text="Speichersicherung"::: 

7. Klicken Sie auf **Speichern**. 

## <a name="enable-high-availability-post-server-creation"></a>Aktivieren von Hochverfügbarkeit nach der Servererstellung

Führen Sie diese Schritte aus, um Hochverfügbarkeit für Ihren vorhandenen flexiblen Server zu aktivieren.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihren vorhandenen flexiblen PostgreSQL-Server aus.

2.  Klicken Sie auf der Seite „Flexibler Server“ im linken Bereich auf **Hochverfügbarkeit**, um die Seite „Hochverfügbarkeit“ zu öffnen.
   
     :::image type="content" source="./media/how-to-manage-high-availability-portal/high-availability-left-panel.png" alt-text="Auswahl des linken Bereichs"::: 

3.  Klicken Sie auf das Kontrollkästchen **Zonenredundante Hochverfügbarkeit**, um die Option zu **aktivieren**, und klicken Sie auf **Speichern**, um die Änderung zu speichern.

     :::image type="content" source="./media/how-to-manage-high-availability-portal/enable-high-availability.png" alt-text="Aktivieren der Hochverfügbarkeit"::: 

4.  Es wird ein Bestätigungsdialogfeld angezeigt, das besagt, dass sich durch die Aktivierung der Hochverfügbarkeit Ihre Kosten aufgrund zusätzlicher Server- und Speicherbereitstellungen erhöhen werden.

5.  Klicken Sie auf die Schaltfläche **Hochverfügbarkeit aktivieren**, um die Hochverfügbarkeit zu aktivieren.

6.  Es wird eine Benachrichtigung angezeigt, die besagt, dass die Bereitstellung der Hochverfügbarkeit ausgeführt wird.

## <a name="disable-high-availability"></a>Deaktivieren der Hochverfügbarkeit

Führen Sie diese Schritte aus, um die Hochverfügbarkeit für Ihren flexiblen Server zu deaktivieren, der bereits mit Zonenredundanz konfiguriert ist.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre vorhandene Azure Database for PostgreSQL – Flexible Server-Instanz aus.

2.  Klicken Sie auf der Seite „Flexibler Server“ im vorderen Bereich auf **Hochverfügbarkeit**, um die Seite „Hochverfügbarkeit“ zu öffnen.
   
    :::image type="content" source="./media/how-to-manage-high-availability-portal/high-availability-left-panel.png" alt-text="Auswahl des linken Bereichs"::: 

3.  Klicken Sie auf das Kontrollkästchen **Zonenredundante Hochverfügbarkeit**, um die Option zu **deaktivieren**. Klicken Sie dann auf **Speichern**, um die Änderung zu speichern.

     :::image type="content" source="./media/how-to-manage-high-availability-portal/disable-high-availability.png" alt-text="Deaktivieren der Hochverfügbarkeit"::: 

4.  Ein Bestätigungsdialogfeld wird angezeigt, in dem Sie die Deaktivierung der Hochverfügbarkeit bestätigen können.

5.  Klicken Sie auf die Schaltfläche **Hochverfügbarkeit deaktivieren**, um die Hochverfügbarkeit zu deaktivieren.

6.  Es wird eine Benachrichtigung angezeigt, die besagt, dass die Außerbetriebnahme der Bereitstellung der Hochverfügbarkeit ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zur [Geschäftskontinuität](./concepts-business-continuity.md)
-   Weitere Informationen zur [zonenredundanten Hochverfügbarkeit](./concepts-high-availability.md)
