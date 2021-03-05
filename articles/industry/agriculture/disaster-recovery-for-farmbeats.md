---
title: Notfallwiederherstellung für FarmBeats
description: In diesem Artikel wird beschrieben, wie die Datenwiederherstellung vor dem Verlust Ihrer Daten schützt.
author: uhabiba04
ms.topic: article
ms.date: 04/13/2020
ms.author: v-ummehabiba
ms.openlocfilehash: 9ece624546cab1b8b6fab8c19f4401bd050f6267
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102179883"
---
# <a name="disaster-recovery-for-farmbeats"></a>Notfallwiederherstellung für FarmBeats

Die Datenwiederherstellung schützt Sie vor dem Verlust Ihrer Daten bei einem Vorfall wie dem Ausfall einer Azure-Region. In einem solchen Fall können Sie ein Failover starten und die in der FarmBeats-Bereitstellung gespeicherten Daten wiederherstellen.

Die Datenwiederherstellung ist kein Standardfeature in Azure-FarmBeats. Sie können dieses Feature manuell konfigurieren, indem Sie die erforderlichen Azure-Ressourcen konfigurieren, die von FarmBeats zum Speichern von Daten in einer gekoppelten Azure-Region verwendet werden. Verwenden Sie zum Aktivieren der Wiederherstellung einen Aktiv-Passiv-Ansatz.

In den folgenden Abschnitten finden Sie Informationen zum Konfigurieren der Datenwiederherstellung in Azure FarmBeats:

- [Aktivieren von Datenredundanz](#enable-data-redundancy)
- [Wiederherstellen des Diensts aus einer Onlinesicherung](#restore-service-from-online-backup)


## <a name="enable-data-redundancy"></a>Aktivieren von Datenredundanz

FarmBeats speichert Daten in drei Azure-Erstanbieterdiensten: **Azure Storage**, **Cosmos DB** und **Time Series Insights**. Führen Sie die folgenden Schritte aus, um Datenredundanz für diese Dienste in einer gekoppelten Azure-Region zu aktivieren:

1.  **Azure Storage**: Befolgen Sie diese Anleitung, um Datenredundanz für jedes Speicherkonto in Ihrer FarmBeats-Bereitstellung zu aktivieren.
2.  **Azure Cosmos DB:** Befolgen Sie diese Anleitung, um Datenredundanz für ein Cosmos DB-Konto in Ihrer FarmBeats-Bereitstellung zu aktivieren.
3.  **Azure Time Series Insights (TSI):** TSI bietet derzeit keine Datenredundanz. Um Time Series Insights-Daten wiederherzustellen, wechseln Sie zu Ihrem Sensor-/Wetterpartner, und pushen Sie die Daten erneut an die FarmBeats-Bereitstellung.

## <a name="restore-service-from-online-backup"></a>Wiederherstellen des Diensts aus einer Onlinesicherung

Sie können ein Failover initiieren und gespeicherte Daten für jeden der oben erwähnten Datenspeicher für Ihre FarmBeats-Bereitstellung wiederherstellen. Nachdem Sie die Daten für Azure Storage und Cosmos DB wiederhergestellt haben, erstellen Sie eine weitere FarmBeats-Bereitstellung in der gekoppelten Azure-Region, und konfigurieren Sie diese anhand der folgenden Schritte für die Verwendung von Daten aus wiederhergestellten Datenspeichern (z. B. Azure Storage und Cosmos DB):

1. [Konfigurieren von Cosmos DB](#configure-cosmos-db)
2. [Konfigurieren des Speicherkontos](#configure-storage-account)


### <a name="configure-cosmos-db"></a>Konfigurieren von Cosmos DB

Kopieren Sie den Zugriffsschlüssel der wiederhergestellten Cosmos DB-Datenbank, und aktualisieren Sie den Schlüsseltresor für den neuen FarmBeats-Datenhub.


  ![Screenshot, auf dem hervorgehoben ist, wo der Zugriffsschlüssel kopiert wird](./media/disaster-recovery-for-farmbeats/key-vault-secrets.png)

> [!NOTE]
> Kopieren Sie die URL der wiederhergestellten Cosmos DB-Datenbank, und aktualisieren Sie sie in der App Service-Konfiguration für den neuen FarmBeats-Datenhub. Sie können jetzt das Cosmos DB-Konto in der neuen FarmBeats-Bereitstellung löschen.

  ![Screenshot, der zeigt, wo die URL des wiederhergestellten Cosmos DB-Kontos kopiert wird](./media/disaster-recovery-for-farmbeats/configuration.png)

### <a name="configure-storage-account"></a>Konfigurieren des Speicherkontos

Kopieren Sie den Zugriffsschlüssel des wiederhergestellten Speicherkontos, und aktualisieren Sie ihn im Schlüsseltresor für den neuen FarmBeats-Datenhub.

![Screenshot, der zeigt, wo der Zugriffsschlüssel des wiederhergestellten Speicherkontos kopiert wird](./media/disaster-recovery-for-farmbeats/key-vault-7-secrets.png)

>[!NOTE]
> Sie müssen unbedingt den Speicherkontonamen in der Batch-VM-Konfigurationsdatei für die neue FarmBeats-Bereitstellung aktualisieren.

![Notfallwiederherstellung](./media/disaster-recovery-for-farmbeats/batch-prep-files.png)

Wenn Sie die Datenwiederherstellung für Ihr Startpaket-Speicherkonto aktiviert haben, führen Sie Schritt 2 aus, um den Zugriffsschlüssel und den Namen des Startpaket-Speicherkontos in der neuen FarmBeats-Instanz zu aktualisieren.
