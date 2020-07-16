---
title: 'Azure VMware Solution: Anfordern von Hilfe zu Bereitstellungen oder Bereitstellungsfehlern'
description: In diesem Artikel erfahren Sie, wie Sie die Informationen in Ihrer privaten Azure VMware Solution-Cloud (AVS) finden, die für das Erstellen einer Serviceanfrage zu einer AVS-Bereitstellung oder zu AVS-Bereitstellungsfehlern erforderlich sind.
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: 3032c01d48617347c454d71498571b0e6b789e32
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84986931"
---
# <a name="get-help-with-azure-vmware-solution-deployment-or-provisioning-failures"></a>Azure VMware Solution: Anfordern von Hilfe zu Bereitstellungen oder Bereitstellungsfehlern

In diesem Artikel erfahren Sie, wie Sie Hilfe zur Bereitstellung von Azure VMware Solution (AVS) oder zu AVS-Bereitstellungsfehlern in Ihrer privaten Cloud erhalten, indem Sie eine Serviceanfrage im Azure-Portal erstellen. Zunächst müssen Sie jedoch einige wichtige Informationen aus dem Azure-Portal zusammentragen. In den meisten Fällen benötigen Sie Folgendes:

- Die Korrelations-ID (der fehlgeschlagenen Bereitstellung)
- Die ID der ExpressRoute-Verbindung (die beim fehlgeschlagenen Versuch verwendet wurde, eine vorhandene private Cloud über die ExpressRoute-Verbindung zu skalieren oder ein Peering für sie zu erstellen)

## <a name="collect-the-correlation-id"></a>Erfassen der Korrelations-ID
 
Zunächst werden Sie sich die Korrelations-ID genauer ansehen. Wenn Sie eine private Cloud erstellen (oder eine beliebige andere Azure-Ressource), wird eine dazugehörige Korrelations-ID generiert. Auch die Bereitstellungen von Azure Resource Manager generieren jeweils eine eindeutige Korrelations-ID. Mithilfe dieser ID können Serviceanfragen schneller erstellt und gelöst werden. 
 
Nachstehend finden Sie die Beispielausgabe einer fehlgeschlagenen Bereitstellung einer privaten Cloud. Die Korrelations-ID ist hervorgehoben.

:::image type="content" source="media/fix-deployment-provisioning-failures/failed-private-cloud-deployment.png" alt-text="Fehlgeschlagene Bereitstellung einer privaten Cloud mit Korrelations-ID":::

Kopieren und speichern Sie diese Korrelations-ID, um sie später in der Serviceanfrage verwenden zu können. Weitere Informationen finden Sie am Ende dieses Artikels unter [Erstellen einer Supportanfrage](#create-your-support-request).

Wenn der Fehler in einer der Phasen der Vorabüberprüfung auftritt, bevor die private Cloud bereitgestellt wird, wird keine Korrelations-ID generiert. In diesem Fall können Sie einfach die Informationen angeben, die Sie beim Erstellen der privaten AVS-Cloud verwendet haben, einschließlich:

- Standort
- Resource group
- Ressourcenname
 
### <a name="collect-a-summary-of-errors"></a>Sammeln einer Fehlerzusammenfassung

Die Fehlerdetails können beim Beheben des Problems ebenfalls sehr hilfreich sein. Klicken Sie auf dem vorherigen Bildschirm auf **Klicken Sie hier, um Details anzuzeigen.** (hervorgehoben), und eine Fehlerzusammenfassung (folgender Screenshot) wird geöffnet.
 
 :::image type="content" source="media/fix-deployment-provisioning-failures/summary-of-errors.png" alt-text="Fehlerzusammenfassung":::

Kopieren und speichern Sie wieder diese Zusammenfassung, um sie später in der Serviceanfrage zu verwenden.
 
### <a name="retrieve-past-deployments"></a>Abrufen früherer Bereitstellungen

Sie können frühere Bereitstellungen (einschließlich fehlerhafter Bereitstellungen) abrufen, indem Sie im Aktivitätsprotokoll für Bereitstellungen danach suchen. Sie öffnen das Protokoll, indem Sie auf das Benachrichtigungssymbol klicken.

:::image type="content" source="media/fix-deployment-provisioning-failures/open-notifications.png" alt-text="Benachrichtigungen öffnen":::

Klicken Sie unter „Benachrichtigungen“ auf **Weitere Ereignisse im Aktivitätsprotokoll**.

:::image type="content" source="media/fix-deployment-provisioning-failures/more-events-in-activity-log.png" alt-text="Link: Weitere Ereignisse im Aktivitätsprotokoll":::

Suchen Sie anschließend nach dem Namen der Ressource oder einer anderen eindeutigen Information, die Sie für die Erstellung der Ressource verwendet haben, um die fehlgeschlagene Bereitstellung und die entsprechende Korrelations-ID zu ermitteln. Im folgenden Beispiel werden die Suchergebnisse für eine private Cloudressource (pc03) angezeigt.
 
:::image type="content" source="media/fix-deployment-provisioning-failures/find-past-deployments.png" alt-text="Suchen nach früheren fehlerhaften AVS-Bereitstellungen":::
 
Wenn Sie auf den Vorgangsnamen der fehlgeschlagenen Bereitstellung klicken, wird ein Detailfenster geöffnet. Klicken Sie auf die Registerkarte „JSON“, und suchen Sie nach „correlationId“. Kopieren Sie den entsprechenden Wert, und nennen Sie ihn in der Serviceanfrage. 
 
## <a name="collect-the-expressroute-id-uri"></a>Erfassen der ExpressRoute-ID (URI)
 
Möglicherweise verfügen Sie bereits über eine private Cloud, und es tritt ein Fehler auf, wenn Sie versuchen, mithilfe der ExpressRoute-Verbindung der privaten Cloud zu skalieren oder ein Peering herzustellen. In diesem Fall können Sie sich in einer Serviceanfrage mithilfe der ExpressRoute-ID der privaten Cloud auf die fehlerhafte Verbindung beziehen.

Wenn Sie eine private Cloud im Portal anzeigen möchten, klicken Sie auf **Konnektivität > ExpressRoute**, und kopieren Sie die **ExpressRoute-ID** in die Zwischenablage.
 
:::image type="content" source="media/fix-deployment-provisioning-failures/expressroute-id.png" alt-text="ExpressRoute-Verbindung erfassen"::: 
 
Fügen Sie die ExpressRoute-ID in das entsprechende Feld in der neuen Supportanfrage ein. Weitere Informationen finden Sie im folgenden Abschnitt [Erstellen einer Supportanfrage](#create-your-support-request).
 
> [!NOTE]
> Gelegentlich schlagen Vorabüberprüfungen vor einer Bereitstellung fehl, und die einzigen verfügbaren Informationen sind die Fehlermeldungen. Diese können bei verschiedenen Fehlern hilfreich sein, zum Beispiel bei Problemen mit dem Instanzkontingent. Daher ist es wichtig, diese Nachrichten in die Supportanfrage aufzunehmen. Weitere Informationen dazu, wie diese Informationen abgerufen werden, finden Sie im vorherigen Abschnitt [Sammeln einer Fehlerzusammenfassung](#collect-a-summary-of-errors).

## <a name="create-your-support-request"></a>Erstellen einer Supportanfrage

Einen allgemeinen Leitfaden zum Erstellen einer Supportanfrage finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). 

Dieser Artikel enthält zusätzliche Anleitungen für das Erstellen einer Supportanfrage zu AVS-Bereitstellungen oder AVS-Bereitstellungsfehlern.

1. Klicken Sie auf das **Hilfesymbol** und dann auf **+ New support request** (+ Neue Supportanfrage).

    :::image type="content" source="media/fix-deployment-provisioning-failures/open-sr-on-avs.png" alt-text="ExpressRoute-Verbindung erfassen":::

2. Füllen Sie alle Pflichtfelder aus, und klicken Sie auf die Registerkarte **Grundlegende Einstellungen**:

    - Wählen Sie bei **Problemtyp** die Option **Configuration and Setup Issues** (Konfigurations- und Setupprobleme) aus.

    - Wählen Sie bei **Problemuntertyp** die Option **Private Cloud bereitstellen** aus.

3. Auf der Registerkarte **Details**:

    - Füllen Sie alle erforderlichen Felder aus.

    - Fügen Sie Ihre Korrelations-ID oder eine ExpressRoute-ID in das entsprechende Feld ein. Wenn kein eigenes Feld für diese Werte vorhanden ist, können Sie sie in das Textfeld unter **Provide details about the issue** (Weitere Details zum Problem) einfügen.

    - Fügen Sie alle Fehlerdetails, einschließlich der Zusammenfassung der kopierten Fehler, in das Textfeld unter **Provide details about the issue** (Weitere Details zum Problem) ein.

4. Lesen Sie sich Ihre Supportanfrage noch einmal durch, und klicken Sie auf **Erstellen**, um sie zu erstellen.
