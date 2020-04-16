---
title: 'Geschäftskontinuitätsplan: QnA Maker'
description: Das Hauptziel des Geschäftskontinuitätsplans besteht darin, einen ausfallsicheren Wissensdatenbank-Endpunkt zu erstellen, der sicherstellen kann, dass keine Ausfallzeiten für den Bot oder die ihn verwendende Anwendung entstehen.
ms.topic: conceptual
ms.date: 04/07/2020
ms.openlocfilehash: 5d1501ecc42fe948959075cec7d728f6c9df908a
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887066"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Erstellen eines Geschäftskontinuitätsplans für Ihren QnA Maker-Dienst

Das Hauptziel des Geschäftskontinuitätsplans besteht darin, einen ausfallsicheren Wissensdatenbank-Endpunkt zu erstellen, der sicherstellen kann, dass keine Ausfallzeiten für den Bot oder die ihn verwendende Anwendung entstehen.

## <a name="business-continuity-with-traffic-manager"></a>Geschäftskontinuität mit Traffic Manager

> [!div class="mx-imgBorder"]
> ![QnA Maker-Geschäftskontinuitätsplan](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Das oben dargestellte allgemeine Konzept lautet wie folgt:

1. Richten Sie zwei parallele [QnA Maker-Dienste](set-up-qnamaker-service-azure.md) in [Azure-Regionspaaren](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) ein.

2. [Sichern](../../../app-service/manage-backup.md) Sie Ihren primären QnA Maker-App-Dienst, und [stellen](../../../app-service/web-sites-restore.md) Sie ihn bei der Sekundäreinrichtung wieder her. Dadurch wird sichergestellt, dass beide Setups mit demselben Hostnamen und denselben Schlüsseln arbeiten.

3. Halten Sie die primären und sekundären Azure-Suchindizes synchron. Verwenden Sie das [hier](https://github.com/pchoudhari/QnAMakerBackupRestore) aufgeführte GitHub-Beispiel, um zu erfahren, wie Azure-Indizes gesichert und wiederhergestellt werden.

4. Sichern Sie die Application Insights-Daten mithilfe des [fortlaufenden Exports](../../../application-insights/app-insights-export-telemetry.md).

5. Verwenden Sie [Traffic Manager](../../../traffic-manager/traffic-manager-overview.md) nach der Einrichtung der primären und sekundären Stapel, um die zwei Endpunkte zu konfigurieren und eine Routingmethode einzurichten.

6. Sie müssten ein TLS-Zertifikat (Transport Layer Security), früher SSL-Zertifikat (Secure Sockets Layer) genannt, für Ihren Traffic Manager-Endpunkt erstellen. [Binden Sie das TLS/SSL-Zertifikat](../../../app-service/configure-ssl-bindings.md) an Ihre App-Dienste.

7. Verwenden Sie dann den Traffic Manager-Endpunkt in Ihrem Bot oder in Ihrer App.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Auswählen der Kapazität](./improve-knowledge-base.md)