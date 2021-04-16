---
title: Zurücksetzen von vSphere-Anmeldeinformationen für Azure VMware Solution
description: Hier erfahren Sie, wie Sie vSphere-Anmeldeinformationen für Ihre private Cloud in Azure VMware Solution zurücksetzen und sicherstellen, dass der HCX-Connector über die neuesten vSphere-Anmeldeinformationen verfügt.
ms.topic: how-to
ms.date: 03/31/2021
ms.openlocfilehash: 793b79e42a0adbca54804d1b66102736aff22d7a
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106109100"
---
# <a name="reset-vsphere-credentials-for-azure-vmware-solution"></a>Zurücksetzen von vSphere-Anmeldeinformationen für Azure VMware Solution

In diesem Artikel erfahren Sie, wie Sie die vCenter Server- und NSX-T Manager-Anmeldeinformationen für Ihre private Cloud in Azure VMware Solution zurücksetzen. Auf diese Weise können Sie sicherstellen, dass der HCX-Connector über die neuesten vCenter Server-Anmeldeinformationen verfügt.

Zusätzlich zu dieser Vorgehensweise können Sie auch das Video zum [Zurücksetzen des vCenter CloudAdmin- und NSX-T-Administrator-Kennworts](https://youtu.be/cK1qY3knj88)anzeigen.

## <a name="reset-your-azure-vmware-solution-credentials"></a>Zurücksetzen der Anmeldeinformationen für Azure VMware Solution

 Setzen Sie zunächst die Anmeldeinformationen für die Komponenten von Azure VMware Solution zurück. Ihre CloudAdmin-Anmeldeinformationen für vCenter und Ihre Administratoranmeldeinformationen für NSX-T laufen nicht ab. Mithilfe der folgenden Schritte können Sie jedoch neue Kennwörter für diese Konten generieren.

> [!NOTE]
> Wenn Sie Ihre CloudAdmin-Anmeldeinformationen für verbundene Dienste wie HCX, vCenter Orchestrator, vRealize, Operations Manager oder VMware Horizon verwenden, werden Ihre Verbindungen unterbrochen, sobald Sie Ihr Kennwort aktualisieren.  Daher sollten diese Dienste beendet werden, bevor die Kennwortrotation initiiert wird.  Andernfalls kann dies zu vorübergehenden Sperren Ihres vCenter-CloudAdmin-Kontos und Ihres NSX-T-Administratorkontos führen, da diese Dienste kontinuierlich Aufrufe mit Ihren alten Anmeldeinformationen senden.  Weitere Informationen zur Einrichtung separater Konten für verbundene Dienste finden Sie unter [Zugriffs- und Identitätskonzepte](https://docs.microsoft.com/azure/azure-vmware/concepts-identity).

1. Öffnen Sie im Azure-Portal eine Azure Cloud Shell-Sitzung.

2. Führen Sie den folgenden Befehl aus, um das vCenter-CloudAdmin-Kennwort zu aktualisieren.  Sie müssen {SubscriptionID}, {ResourceGroup} und {PrivateCloudName} durch die tatsächlichen Werte der private Cloud ersetzen, zu der das CloudAdmin-Konto gehört.

```
az resource invoke-action --action rotateVcenterPassword --ids "/subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroup}/providers/Microsoft.AVS/privateClouds/{PrivateCloudName}" --api-version "2020-07-17-preview"
```
          
3. Führen Sie den folgenden Befehl aus, um das NSX-T-Administratorkennwort zu aktualisieren. Sie müssen {SubscriptionID}, {ResourceGroup} und {PrivateCloudName} durch die tatsächlichen Werte der privaten Cloud ersetzen, zu der das NSX-T-Administratorkonto gehört.

```
az resource invoke-action --action rotateNSXTPassword --ids "/subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroup}/providers/Microsoft.AVS/privateClouds/{PrivateCloudName}" --api-version "2020-07-17-preview"
```

## <a name="ensure-the-hcx-connector-has-your-latest-vcenter-server-credentials"></a>Sicherstellen, dass der HCX-Connector über die neuesten vCenter Server-Anmeldeinformationen verfügt

Nachdem Sie Ihre Anmeldeinformationen zurückgesetzt haben, führen Sie die folgenden Schritte aus, um sicherzustellen, dass der HCX-Connector Ihre aktualisierten Anmeldeinformationen hat.

1. Nachdem Ihr Kennwort geändert wurde, wechseln Sie mit https://{IP-Adresse der HCX-Connectorappliance}:443 zur Web-Schnittstelle des lokalen HCX-Connectors. Achten Sie darauf, Port 443 zu verwenden. Melden Sie sich mit Ihren neuen Anmeldeinformationen an.

2. Wählen Sie auf dem VMware-HCX-Dashboard **Site Pairing** (Websitekopplung) aus.
    
    :::image type="content" source="media/reset-vsphere-credentials/hcx-site-pairing.png" alt-text="Screenshot des VMware-HCX-Dashboards mit hervorgehobener Websitekopplung.":::
 
3. Wählen Sie die korrekte Verbindung mit Azure VMware Solution (sofern vorhanden) aus, und klicken Sie auf **Verbindung bearbeiten**.
 
4. Geben Sie die neuen vCenter Server CloudAdmin-Benutzer-Anmeldeinformationen an und wählen Sie **Bearbeiten** aus. Dadurch werden die Anmeldeinformationen gespeichert. Das Speichern sollte erfolgreich verlaufen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit dem Zurücksetzen von vCenter Server- und NSX-T Manager-Anmeldeinformationen in Azure VMware Solution vertraut gemacht haben, sollten Sie sich den folgenden Themen zuwenden:

- [Konfigurieren von NSX-Netzwerkkomponenten in Azure VMware Solution](configure-nsx-network-components-azure-portal.md)
- [Lebenszyklusverwaltung von Azure VMware Solution-VMs](lifecycle-management-of-azure-vmware-solution-vms.md)
- [Bereitstellen der Notfallwiederherstellung virtueller Computer mithilfe von Azure VMware Solution](disaster-recovery-for-virtual-machines.md)
