---
title: Verwalten zonenredundanter Hochverfügbarkeit – Azure-Portal – Azure Database for MySQL Flexible Server
description: In diesem Artikel wird beschrieben, wie Sie die zonenredundante Hochverfügbarkeit in Azure Database for MySQL Flexible Server im Azure-Portal aktivieren oder deaktivieren.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.custom: references_regions
ms.openlocfilehash: e217dcaeafd553803f5c9699ab6d7779ed755b67
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "107818282"
---
# <a name="manage-zone-redundant-high-availability-in-azure-database-for-mysql-flexible-server-preview"></a>Verwalten von zonenredundanter Hochverfügbarkeit für Azure Database for MySQL Flexible Server (Vorschau)

In diesem Artikel wird beschrieben, wie Sie die zonenredundante Hochverfügbarkeit in Azure Database for PostgreSQL – Flexible Server im Azure-Portal aktivieren oder deaktivieren.

Die Hochverfügbarkeitsfunktion stellt ein physisch getrenntes primäres und Standbyreplikat in unterschiedlichen Zonen bereit. Weitere Detailinformationen finden Sie unter [Dokumentation zu Hochverfügbarkeitskonzepten](./concepts/../concepts-high-availability.md). 

> [!IMPORTANT]
> Sie können die zonenredundante Hochverfügbarkeit nur während der Erstellung flexibler Server aktivieren.

Auf dieser Seite finden Sie Richtlinien zum Aktivieren oder Deaktivieren der Hochverfügbarkeit. Durch diesen Vorgang werden keine Ihrer anderen Einstellungen geändert, einschließlich der VNET-Konfiguration, der Firewalleinstellungen und der Sicherungsaufbewahrung. Analog dazu ist das Deaktivieren der Hochverfügbarkeit ein Onlinevorgang, der sich nicht auf die Konnektivität und die Vorgänge Ihrer Anwendung auswirkt.

> [!IMPORTANT]
> Zonenredundante Hochverfügbarkeit ist in einer begrenzten Anzahl von Regionen verfügbar: Asien (Südosten), USA (Westen 2), Europa (Westen) und USA (Osten).  

## <a name="enable-high-availability-during-server-creation"></a>Aktivieren von Hochverfügbarkeit während der Servererstellung

In diesem Abschnitt finden Sie spezifische Informationen für auf Hochverfügbarkeit bezogene Felder. Sie können diese Schritte ausführen, um Hochverfügbarkeit während der Erstellung Ihres flexiblen Servers bereitzustellen.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option „Flexibler Server“ aus, und klicken Sie auf **Erstellen**.  Ausführliche Informationen zum Ausfüllen von Details wie **Abonnement**, **Ressourcengruppe**, **Servername**, **Region** und anderen Feldern finden Sie in der Dokumentation zur Vorgehensweise beim Erstellen des Servers.

2.  Aktivieren Sie das Kontrollkästchen für **Zonenredundante Hochverfügbarkeit** in der Option „Verfügbarkeit“.

3.  Wenn Sie die Standardwerte für „Compute“ und „Speicher“ ändern möchten, klicken Sie auf  **Server konfigurieren**.

4.  Wenn die Option „Hochverfügbarkeit“ aktiviert ist, steht die Ebene „Burstfähig“ nicht zur Auswahl. Sie können die Computeebene **Allgemeiner Zweck** oder **Arbeitsspeicheroptimiert** auswählen.

    > [!IMPORTANT]
    > Wir unterstützen zonenredundante Hochverfügbarkeit nur für die Tarife ***Universell** _ und _ *_Arbeitsspeicheroptimiert_**.

5.  Wählen Sie die **Computegröße** für Ihre Option aus der Dropdownliste aus.

6.  Wählen Sie die **Speichergröße** in GiB mithilfe der Schiebereglerleiste aus, und wählen Sie den **Aufbewahrungszeitraum für Sicherungen** zwischen 7 Tagen und 35 Tagen aus.   

## <a name="disable-high-availability"></a>Deaktivieren der Hochverfügbarkeit

Führen Sie diese Schritte aus, um die Hochverfügbarkeit für Ihren flexiblen Server zu deaktivieren, der bereits mit Zonenredundanz konfiguriert ist.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihren vorhandenen flexiblen Azure Database for MySQL-Server aus.

2.  Klicken Sie auf der Seite „Flexibler Server“ im vorderen Bereich auf **Hochverfügbarkeit**, um die Seite „Hochverfügbarkeit“ zu öffnen.

3.  Klicken Sie auf das Kontrollkästchen **Zonenredundante Hochverfügbarkeit**, um die Option zu deaktivieren, und klicken Sie auf **Speichern**, um die Änderung zu speichern.

4.  Ein Bestätigungsdialogfeld wird angezeigt, in dem Sie die Deaktivierung der Hochverfügbarkeit bestätigen können.

5.  Klicken Sie auf die Schaltfläche **Hochverfügbarkeit deaktivieren**, um die Hochverfügbarkeit zu deaktivieren.

6.  Es wird eine Benachrichtigung angezeigt, die besagt, dass die Außerbetriebnahme der Bereitstellung der Hochverfügbarkeit ausgeführt wird.


## <a name="forced-failover"></a>erzwungenes Failover

Führen Sie die folgenden Schritte aus, um ein Failover von Ihrem primären Server auf den flexiblen Standbyserver zu erzwingen.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihren flexiblen Azure Database for MySQL-Server aus, bei dem das Feature „Hochverfügbarkeit“ aktiviert wurde.

2.  Klicken Sie auf der Seite „Flexibler Server“ im vorderen Bereich auf **Hochverfügbarkeit**, um die Seite „Hochverfügbarkeit“ zu öffnen.

3.  Überprüfen Sie die **Primäre Verfügbarkeitszone** und die **Standbyverfügbarkeitszone**.

4.  Klicken Sie auf **Erzwungenes Failover**, um das manuelle Failoververfahren einzuleiten. Ein Popupfenster informiert Sie über die erwartete Failoverzeit – abhängig von der aktuellen Workload auf dem primären Server – und die Aktualität des letzten Prüfpunkts. Lesen Sie diese Meldung, und klicken Sie auf „OK“.
 
5. In einer Benachrichtigung wird mitgeteilt, dass ein Failover ausgeführt wird.

6. Sobald das Failover auf den Standbyserver erfolgreich war, wird eine entsprechende Benachrichtigung angezeigt.

7. Überprüfen Sie die neue **Primäre Verfügbarkeitszone** und die **Standbyverfügbarkeitszone**.

![Erzwingen eines Failovers](media/how-to-configure-high-availability/how-to-forced-failover.png) 

## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zur [Geschäftskontinuität](./concepts-business-continuity.md)
-   Weitere Informationen zur [zonenredundanten Hochverfügbarkeit](./concepts-high-availability.md)
