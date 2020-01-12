---
title: Daten- und Speicherempfehlungen – Azure Security Center
description: Dieses Dokument behandelt Empfehlungen in Azure Security Center, die zum Schutz Ihrer Daten und des Azure SQL-Diensts sowie zur Einhaltung von Sicherheitsrichtlinien beitragen.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2019
ms.author: memildin
ms.openlocfilehash: 74ed55e1d460495bfa8d3d4c00bd37bb7f05260e
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552864"
---
# <a name="protect-azure-data-and-storage-services"></a>Schützen von Azure-Daten und Speicherdiensten
Werden potenzielle Sicherheitslücken erkannt, erstellt Azure Security Center Empfehlungen, die Sie beim Konfigurieren der erforderlichen Steuerelemente zum Härten und Schützen Ihrer Ressourcen unterstützen.

In diesem Artikel wird die Seite **Datensicherheit** des Abschnitts „Ressourcensicherheit“ von Azure Security Center erläutert.

Eine vollständige Liste der Empfehlungen, die auf dieser Seite angezeigt werden können, finden Sie unter [Daten- und Speicherempfehlungen](recommendations-reference.md#recs-datastorage).


## <a name="view-your-data-security-information"></a>Anzeigen der Informationen zur Sicherheit Ihrer Daten

1. Klicken Sie im Abschnitt **Resource security hygiene** (Ressourcensicherheit) auf **Data and storage resources** (Daten- und Speicherressourcen).

    ![Daten- und Speicherressourcen](./media/security-center-monitoring/click-data.png)

    Die Seite **Data security** (Datensicherheit) wird mit Empfehlungen für Datenressourcen geöffnet.

    [![Datenressourcen](./media/security-center-monitoring/sql-overview.png)](./media/security-center-monitoring/sql-overview.png#lightbox)

    Auf der Seite haben Sie folgende Möglichkeiten:

    * Wenn Sie auf die Registerkarte **Übersicht** klicken, werden alle zu bereinigenden Empfehlungen zu Datenressourcen aufgelistet. 
    * Klicken Sie auf jede Registerkarte, und lassen Sie die Empfehlungen je nach Ressourcentyp anzeigen.

    > [!NOTE]
    > Weitere Informationen zur Speicherverschlüsselung finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](../storage/common/storage-service-encryption.md).


## <a name="remediate-a-recommendation-on-a-data-resource"></a>Bereinigen einer Empfehlung für eine Datenressource

1. Klicken Sie auf einer der Registerkarten auf eine Ressource. Die Informationsseite wird geöffnet und die zu bereinigenden Empfehlungen aufgelistet.

    ![Ressourceninformationen](./media/security-center-monitoring/sql-recommendations.png)

2. Klicken Sie auf eine Empfehlung. Die Seite „Empfehlungen“ wird geöffnet und zeigt die **Schritte zur Bereinigung** an, um die Empfehlung zu implementieren.

   ![Schritte zur Bereinigung](./media/security-center-monitoring/remediate1.png)

3. Klicken Sie auf **Aktion ausführen**. Die Seite für die Ressourceneinstellungen wird angezeigt.

    ![Aktivieren der Empfehlung](./media/security-center-monitoring/remediate2.png)

4. Führen Sie die **Schritte zur Bereinigung** aus, und klicken Sie auf **Speichern**.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Artikeln:

* [Vollständige Referenzliste der Sicherheitsempfehlungen von Azure Security Center](recommendations-reference.md)
* [Schützen von Computern und Anwendungen im Azure Security Center](security-center-virtual-machine-protection.md)
* [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md)