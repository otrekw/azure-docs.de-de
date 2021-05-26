---
title: Rotieren der Anmeldeinformationen von Cloudadministratoren für Azure VMware Solution
description: Erfahren Sie, wie Sie die vCenter Server- und NSX-T Manager-Anmeldeinformationen für Ihre private Cloud in Azure VMware Solution rotieren.
ms.topic: how-to
ms.date: 05/11/2021
ms.openlocfilehash: fbfed495fd904b67ce283934791cee516349dfa6
ms.sourcegitcommit: 0ce834cd348bb8b28a5f7f612c2807084cde8e8f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109814950"
---
# <a name="rotate-the-cloudadmin-credentials-for-azure-vmware-solution"></a>Rotieren der Anmeldeinformationen von Cloudadministratoren für Azure VMware Solution

In diesem Artikel werden die Schritte zum Rotieren der Anmeldeinformationen von Cloudadministratoren für Ihre private Cloud in Azure VMware Solution beschrieben.  Ihre Anmeldeinformationen für den vCenter-Cloudadministrator und den NSX-T Manager-Administrator laufen nicht ab. Sie können jedoch neue Kennwörter für diese Konten generieren. Nachdem Sie die Anmeldeinformationen rotiert haben, vergewissern Sie sich, dass der HCX-Connector über die neuesten Anmeldeinformationen für vCenter Server verfügt.

Sie können sich auch ein Video zum [Zurücksetzen der Kennwörter des vCenter-Cloudadministrators und des NSX-T-Administrators](https://youtu.be/cK1qY3knj88) ansehen. 

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie Ihre Anmeldeinformationen für den Cloudadministrator für verbundene Dienste wie HCX, vRealize Orchestrator, vRealize, Operations Manager oder VMware Horizon verwenden, werden Ihre Verbindungen unterbrochen, sobald Sie Ihr Kennwort aktualisieren.  Beenden Sie diese Dienste, bevor Sie die Kennwortrotation starten. Andernfalls kann dies zu vorübergehendem Sperren Ihrer Konten für den vCenter-Cloudadministrator und den NSX-T-Administrator führen, da diese Dienste kontinuierlich Aufrufe mit Ihren alten Anmeldeinformationen übermitteln.  Weitere Informationen zur Einrichtung separater Konten für verbundene Dienste finden Sie unter [Zugriffs- und Identitätskonzepte](./concepts-identity.md).

## <a name="reset-your-azure-vmware-solution-credentials"></a>Zurücksetzen der Anmeldeinformationen für Azure VMware Solution

In diesem Schritt rotieren Sie die Anmeldeinformationen für den Cloudadministrator Ihrer Azure VMware Solution-Komponenten. 

>[!NOTE]
>Denken Sie daran, **{SubscriptionID}** , **{ResourceGroup}** und **{PrivateCloudName}** durch die Daten Ihrer privaten Cloud zu ersetzen.

1. Öffnen Sie im Azure-Portal eine Azure Cloud Shell-Sitzung.

2. Aktualisieren Sie das Kennwort Ihres vCenter-Cloudadministrators.  

   ```azurecli-interactive
   az resource invoke-action --action rotateVcenterPassword --ids "/subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroup}/providers/Microsoft.AVS/privateClouds/{PrivateCloudName}" --api-version "2020-07-17-preview"
   ```
          
3. Aktualisieren Sie das Kennwort Ihres NSX-T-Administrators. 

   ```azurecli-interactive
   az resource invoke-action --action rotateNSXTPassword --ids "/subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroup}/providers/Microsoft.AVS/privateClouds/{PrivateCloudName}" --api-version "2020-07-17-preview"
   ```

## <a name="verify-hcx-connector-has-the-latest-credentials"></a>Vergewissern, dass der HCX-Connector über die neuesten Anmeldeinformationen verfügt

In diesem Schritt überprüfen Sie, ob der HCX-Connector über die aktualisierten Anmeldeinformationen verfügt.

1. Wechseln Sie unter https://{IP-Adresse_der_HCX-Connectorappliance}:443 zum lokalen HCX-Connector, und melden Sie sich mit den neuen Anmeldeinformationen an.

   Achten Sie darauf, Port 443 zu verwenden. 

2. Wählen Sie auf dem VMware-HCX-Dashboard **Site Pairing** (Websitekopplung) aus.
    
   :::image type="content" source="media/rotate-cloudadmin-credentials/hcx-site-pairing.png" alt-text="Screenshot des VMware-HCX-Dashboards mit hervorgehobener Websitekopplung.":::
 
3. Wählen Sie die richtige Verbindung mit Azure VMware Solution und dann **Verbindung bearbeiten** aus.
 
4. Geben Sie die neuen vCenter Server CloudAdmin-Benutzer-Anmeldeinformationen an und wählen Sie **Bearbeiten** aus. Dadurch werden die Anmeldeinformationen gespeichert. Das Speichern sollte erfolgreich verlaufen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit dem Zurücksetzen von vCenter Server- und NSX-T Manager-Anmeldeinformationen in Azure VMware Solution vertraut gemacht haben, sollten Sie sich den folgenden Themen zuwenden:

- [Konfigurieren von NSX-Netzwerkkomponenten in Azure VMware Solution](configure-nsx-network-components-azure-portal.md)
- [Überwachen und Verwalten von Azure VMware Solution-VMs](lifecycle-management-of-azure-vmware-solution-vms.md)
- [Bereitstellen der Notfallwiederherstellung für VMs mithilfe von Azure VMware Solution](disaster-recovery-for-virtual-machines.md)
