---
title: 'Azure VMware Solution: Anfordern von Hilfe zu Bereitstellungen oder Bereitstellungsfehlern'
description: In diesem Artikel erfahren Sie, wie Sie die Informationen in Ihrer privaten Azure VMware Solution-Cloud finden, die für das Erstellen einer Serviceanfrage zu einer Azure VMware Solution-Bereitstellung oder zu Azure VMware Solution-Bereitstellungsfehlern erforderlich sind.
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: 1f46dde895db417fd2b488a6203d5482e73d3c5e
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92779491"
---
# <a name="get-help-with-azure-vmware-solution-deployment-or-provisioning-failures"></a>Azure VMware Solution: Anfordern von Hilfe zu Bereitstellungen oder Bereitstellungsfehlern

Dieser Artikel unterstützt Sie bei der Azure VMware Solution-Bereitstellung und bei -Bereitstellungsfehlern. Wenn Fehler in Ihrer privaten Cloud auftreten, müssen Sie eine [Supportanfrage](https://rc.portal.azure.com/#create/Microsoft.Support) im Azure-Portal öffnen. 

Zunächst müssen Sie jedoch einige wichtige Informationen aus dem Azure-Portal zusammentragen:

- Korrelations-ID
- ID der ExpressRoute-Leitung

## <a name="collect-the-correlation-id"></a>Erfassen der Korrelations-ID
 
Eine Korrelations-ID wird generiert, wenn Sie eine private Cloud oder beliebige Ressource in Azure erstellen. Auch die Bereitstellungen von Azure Resource Manager generieren jeweils eine Korrelations-ID. Mithilfe dieser ID können Serviceanfragen schneller erstellt und gelöst werden. 
 
Nachstehend finden Sie die Beispielausgabe einer fehlgeschlagenen Bereitstellung einer privaten Cloud. Die Korrelations-ID ist hervorgehoben.

:::image type="content" source="media/fix-deployment-provisioning-failures/failed-private-cloud-deployment.png" alt-text="Fehlgeschlagene Bereitstellung einer privaten Cloud mit Korrelations-ID":::

Kopieren und speichern Sie diese Korrelations-ID, um sie später in der Serviceanfrage verwenden zu können. Weitere Informationen finden Sie am Ende dieses Artikels unter [Erstellen einer Supportanfrage](#create-your-support-request).

Wenn der Fehler in den Schritten vor der Validierung auftritt, wird keine Korrelations-ID generiert. In diesem Fall können Sie die Informationen angeben, die Sie beim Erstellen der privaten Azure VMware Solution-Cloud verwendet haben, einschließlich:

- Standort
- Resource group
- Ressourcenname
 
### <a name="collect-a-summary-of-errors"></a>Sammeln einer Fehlerzusammenfassung

Die Fehlerdetails können beim Beheben des Problems ebenfalls helfen. Klicken Sie in der vorherigen Anzeige auf die Warnungsmeldung, um eine Zusammenfassung der Fehler anzuzeigen.
 
 :::image type="content" source="media/fix-deployment-provisioning-failures/summary-of-errors.png" alt-text="Fehlerzusammenfassung":::

Kopieren und speichern Sie wieder diese Zusammenfassung, um sie später in der Serviceanfrage zu verwenden.
 
### <a name="retrieve-past-deployments"></a>Abrufen früherer Bereitstellungen

Sie können frühere Bereitstellungen (einschließlich fehlerhafter Bereitstellungen) abrufen, indem Sie im Aktivitätsprotokoll für Bereitstellungen danach suchen. Sie öffnen das Protokoll, indem Sie auf das Benachrichtigungssymbol klicken.

:::image type="content" source="media/fix-deployment-provisioning-failures/open-notifications.png" alt-text="Benachrichtigungen öffnen":::

Klicken Sie unter „Benachrichtigungen“ auf **Weitere Ereignisse im Aktivitätsprotokoll**.

:::image type="content" source="media/fix-deployment-provisioning-failures/more-events-in-activity-log.png" alt-text="Link: Weitere Ereignisse im Aktivitätsprotokoll":::

Suchen Sie anschließend nach dem Namen der Ressource oder anderen Informationen, die zum Erstellen der Ressource verwendet wurden, um die fehlerhafte Bereitstellung und ihre Korrelations-ID zu ermitteln. Im folgenden Beispiel werden die Suchergebnisse für eine private Cloudressource (pc03) angezeigt.
 
:::image type="content" source="media/fix-deployment-provisioning-failures/find-past-deployments.png" alt-text="Suchen nach früheren fehlerhaften Azure VMware Solution-Bereitstellungen":::
 
Wenn Sie auf den Vorgangsnamen der fehlgeschlagenen Bereitstellung klicken, wird ein Detailfenster geöffnet. Klicken Sie auf die Registerkarte „JSON“, und suchen Sie nach „correlationId“. Kopieren Sie den entsprechenden Wert, und nennen Sie ihn in der Serviceanfrage. 
 
## <a name="collect-the-expressroute-id-uri"></a>Erfassen der ExpressRoute-ID (URI)
 
Angenommen, Sie versuchen, eine Skalierung oder ein Peering für eine vorhandene private Cloud mit der ExpressRoute-Leitung für die private Cloud durchzuführen, was jedoch fehlschlägt. In diesem Fall benötigen Sie die ExpressRoute-ID. 

Klicken Sie im Azure-Portal auf **Konnektivität > ExpressRoute** , und kopieren Sie die **ExpressRoute-ID** in die Zwischenablage.
 
:::image type="content" source="media/fix-deployment-provisioning-failures/expressroute-id.png" alt-text="Kopieren Sie die ExpressRoute-ID in die Zwischenablage."::: 
 
> [!NOTE]
> Gelegentlich schlagen Vorabüberprüfungen vor der Bereitstellung fehl, und die einzigen verfügbaren Informationen sind die Fehlermeldungen. Diese können bei vielen Fehlern hilfreich sein, zum Beispiel bei Problemen mit dem Instanzkontingent. Daher ist es wichtig, diese Nachrichten in die Supportanfrage aufzunehmen. Weitere Informationen dazu, wie diese Informationen abgerufen werden, finden Sie im vorherigen Abschnitt [Sammeln einer Fehlerzusammenfassung](#collect-a-summary-of-errors).

## <a name="create-your-support-request"></a>Erstellen einer Supportanfrage

Einen allgemeinen Leitfaden zum Erstellen einer Supportanfrage finden Sie unter [Erstellen einer Azure-Supportanfrage](../azure-portal/supportability/how-to-create-azure-support-request.md). 

Im Folgenden finden Sie eine explizite Anleitung zum Erstellen einer Supportanfrage für die Azure VMware Solution-Bereitstellung oder -Bereitstellungsfehler.

1. Klicken Sie auf das **Hilfesymbol** und dann auf **+ New support request** (+ Neue Supportanfrage).

    :::image type="content" source="media/fix-deployment-provisioning-failures/open-sr-on-avs.png" alt-text="Erfassen Sie eine ExpressRoute-ID für Ihre Supportanfrage.":::

2. Füllen Sie alle Pflichtfelder aus, und klicken Sie auf die Registerkarte **Grundlegende Einstellungen** :

    - Wählen Sie bei **Problemtyp** die Option **Configuration and Setup Issues** (Konfigurations- und Setupprobleme) aus.

    - Wählen Sie bei **Problemuntertyp** die Option **Private Cloud bereitstellen** aus.

3. Auf der Registerkarte **Details** :

    - Füllen Sie alle erforderlichen Felder aus.

    - Fügen Sie Ihre Korrelations-ID oder eine ExpressRoute-ID in das entsprechende Feld ein. Wenn Ihnen kein spezifisches Feld angezeigt wird, können Sie diese in das Textfeld unter **Provide details about the issue** (Geben Sie Details zum Problem an).

    - Fügen Sie alle Fehlerdetails, einschließlich der Zusammenfassung der kopierten Fehler, in das Textfeld unter **Provide details about the issue** (Weitere Details zum Problem) ein.

4. Lesen Sie sich Ihre Supportanfrage noch einmal durch, und klicken Sie auf **Erstellen** , um sie zu erstellen.
