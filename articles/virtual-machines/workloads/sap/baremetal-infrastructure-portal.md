---
title: BareMetal-Instanzeinheiten in Azure
description: Hier erfahren Sie, wie Sie BareMetal-Instanzeinheiten über das Azure-Portal identifizieren und mit ihnen interagieren.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: shortpatti
manager: gwallace
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/18/2020
ms.author: v-patsho
ms.openlocfilehash: 8d94b54d909b6d0528150e6a678dd6e819ea78cd
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97675728"
---
# <a name="manage-baremetal-instances-through-the-azure-portal"></a>Verwalten von BareMetal-Instanzen über das Azure-Portal
 
In diesem Artikel wird die Darstellung von BareMetal-Instanzen im [Azure-Portal](https://portal.azure.com/) erläutert. Zudem werden die Aktivitäten beschrieben, die Sie im Azure-Portal mit den bereitgestellten BareMetal-Instanzeinheiten durchführen können. 
 
## <a name="register-the-resource-provider"></a>Registrieren des Ressourcenanbieters
Die Sichtbarkeit von Instanzen im Azure-Portal wird über einen Azure-Ressourcenanbieter gesteuert. Diese Funktion befindet sich derzeit in der öffentlichen Vorschau. Im Azure-Abonnement, das Sie für BareMetal-Instanzbereitstellungen verwenden, ist der *BareMetalInfrastructure*-Ressourcenanbieter standardmäßig registriert. Wenn die bereitgestellten BareMetal-Instanzeinheiten nicht angezeigt werden, müssen Sie den Ressourcenanbieter in Ihrem Abonnement registrieren. Der BareMetal-Instanzressourcenanbieter kann auf zwei Arten registriert werden:
 
* [Azure-Befehlszeilenschnittstelle](#azure-cli)
 
* [Azure portal](#azure-portal)
 
### <a name="azure-cli"></a>Azure CLI
 
Melden Sie sich über die Azure CLI bei dem Azure-Abonnement an, das Sie für die Bereitstellung von BareMetal-Instanzen verwenden. Sie können den BareMetalInfrastructure-Ressourcenanbieter wie folgt registrieren:

```azurecli-interactive
az provider register --namespace Microsoft.BareMetalInfrastructure
```
 
Weitere Informationen finden Sie im Artikel [Ressourcenanbieter und -typen](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-powershell).
 
### <a name="azure-portal"></a>Azure-Portal
 
Sie können den BareMetalInfrastructure-Ressourcenanbieter über das Azure-Portal registrieren.
 
Sie müssen Ihr Abonnement, das für die Bereitstellung der BareMetal-Instanzeinheiten verwendet wurde, im Azure-Portal anzeigen und darauf doppelklicken.
 
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im Menü des Azure-Portals die Option **Alle Dienste** aus.

1. Geben Sie im Feld **Alle Dienste****Abonnement** ein, und wählen Sie dann **Abonnements** aus.

1. Wählen Sie das Abonnement aus der Abonnentenliste aus, um es anzuzeigen.

1. Wählen Sie die Option **Ressourcenanbieter** aus, und geben Sie **BareMetalInfrastructure** im Suchfenster ein. Der Ressourcenanbieter sollte wie in der Abbildung als **registriert** angezeigt werden.
 
>[!NOTE]
>Wählen Sie **Registrieren** aus, falls der Ressourcenanbieter nicht registriert ist.
 
:::image type="content" source="media/baremetal-infrastructure-portal/register-resource-provider-azure-portal.png" alt-text="Screenshot: Registrierte BareMetal-Instanzeinheit":::
 
## <a name="baremetal-instance-units-in-the-azure-portal"></a>BareMetal-Instanzeinheiten im Azure-Portal
 
Geben Sie beim Übermitteln einer Bereitstellungsanforderung für BareMetal-Instanzen das Azure-Abonnement an, das mit den BareMetal-Instanzen verknüpft werden soll. Verwenden Sie dasselbe Abonnement, das Sie zum Bereitstellen der Anwendungsschicht einsetzen, die den BareMetal-Instanzeinheiten entgegenwirkt.
 
Während der Bereitstellung Ihrer BareMetal-Instanzen wird eine neue [Azure-Ressourcengruppe](../../../azure-resource-manager/management/manage-resources-portal.md) im Azure-Abonnement erstellt, das Sie in der Bereitstellungsanforderung verwendet haben. In der neuen Ressourcengruppe werden alle im jeweiligen Abonnement bereitgestellten BareMetal-Instanzeinheiten aufgelistet.

1. Wählen Sie im Azure-Portal für das BareMetal-Abonnement **Ressourcengruppen** aus.
 
   :::image type="content" source="media/baremetal-infrastructure-portal/view-baremetal-instance-units-azure-portal.png" alt-text="Screenshot: Liste der Ressourcengruppen":::

1. Suchen Sie in der Liste nach der neuen Ressourcengruppe.
 
   :::image type="content" source="media/baremetal-infrastructure-portal/filter-resource-groups.png" alt-text="Screenshot: BareMetal-Instanzeinheit in einer gefilterten Ressourcengruppenliste" lightbox="media/baremetal-infrastructure-portal/filter-resource-groups.png":::
   
   >[!TIP]
   >Sie können nach dem Abonnement filtern, das Sie zur Bereitstellung der BareMetal-Instanz verwendet haben. Nach dem Filtern nach dem richtigen Abonnement wird Ihnen möglicherweise eine lange Liste von Ressourcengruppen angezeigt. Suchen Sie nach einer mit dem Postfix **-Txxx**, wobei „xxx“ drei Ziffern sind, z. B. **-T250**.

1. Wählen Sie die neue Ressourcengruppe aus, um die Details anzuzeigen. Das Bild zeigt eine bereitgestellte BareMetal-Instanzeinheit.
   
   >[!NOTE]
   >Wenn Sie mehrere Mandanten von BareMetal-Instanzen unter demselben Azure-Abonnement bereitgestellt haben, werden mehrere Azure-Ressourcengruppen angezeigt.
 
## <a name="view-the-attributes-of-a-single-instance"></a>Anzeigen von Attributen einer einzelnen Instanz
 
Sie können die Details einer einzelnen Einheit anzeigen. Wählen Sie in der Liste der BareMetal-Instanz die einzelne Instanz aus, die Sie anzeigen möchten.
 
:::image type="content" source="media/baremetal-infrastructure-portal/view-attributes-single-baremetal-instance.png" alt-text="Screenshot: Attribute der BareMetal-Instanzeinheit einer einzelnen Instanz" lightbox="media/baremetal-infrastructure-portal/view-attributes-single-baremetal-instance.png":::
 
Die dargestellten Attribute unterscheiden sich nicht wesentlich von den Azure-VM-Attributen. Auf der linken Seite werden die Ressourcengruppe, die Azure-Region, der Abonnementname und die ID angezeigt. Wenn Sie Tags zugewiesen haben, werden sie hier ebenfalls angezeigt. Standardmäßig sind den BareMetal-Instanzeinheiten keine Tags zugewiesen.
 
Auf der rechten Seite werden der Name, das Betriebssystem, die IP-Adresse und die SKU des Geräts mit der Anzahl der CPU-Threads und dem Arbeitsspeicher angezeigt. Außerdem sehen Sie den Energiezustand und die Hardwareversion (Revision des BareMetal-Instanzstamps). Der Energiezustand gibt an, ob die Hardwareeinheit ein- oder ausgeschaltet ist. Die Details des Betriebssystems geben jedoch keinen Aufschluss darüber, ob es ausgeführt wird.
 
Mögliche Hardwarerevisionen:
 
* Revision 3
 
* Revision 4
 
* Revision 4.2
 
>[!NOTE]
>Revision 4.2 ist die neueste BareMetal-Infrastruktur-Revision mit neuem Branding, die die Architektur von Revision 4 verwendet. Sie bietet wesentliche Verbesserungen in der Netzwerklatenz zwischen Azure-VMs und BareMetal-Instanzeinheiten, die in Revision 4-Stamps oder -Reihen bereitgestellt werden.
 
Auf der rechten Seite sehen Sie den Namen der [Azure-Näherungsplatzierungsgruppe](../../../virtual-machines/linux/co-location.md), die automatisch für jede bereitgestellte BareMetal-Instanzeinheit erstellt wird. Referenzieren Sie die Näherungsplatzierungsgruppe bei der Bereitstellung der Azure-VMs, die die Anwendungsschicht hosten. Durch die Verwendung der Näherungsplatzierungsgruppe, die der BareMetal-Instanzeinheit zugeordnet ist, stellen Sie sicher, dass die Azure-VMs in unmittelbarer Nähe der BareMetal-Instanzeinheit bereitgestellt werden.
 
>[!TIP]
>Informationen darüber, wie Sie die Anwendungsschicht im gleichen Azure-Rechenzentrum wie Revision 4.x unterbringen, finden Sie unter [Azure-Näherungsplatzierungsgruppen für optimale Netzwerklatenz](../../../virtual-machines/workloads/sap/sap-proximity-placement-scenarios.md).
 
## <a name="check-activities-of-a-single-instance"></a>Überprüfen von Aktivitäten einer einzelnen Instanz
 
Sie können die Aktivitäten einer einzelnen Einheit überprüfen. Eine der wichtigsten aufgezeichneten Aktivitäten ist ein Neustart der Einheit. Die aufgelisteten Daten umfassen den Status der Aktivität, den Zeitstempel der ausgelösten Aktivität, die Abonnement-ID und den Azure-Benutzer, der die Aktivität ausgelöst hat.
 
:::image type="content" source="media/baremetal-infrastructure-portal/check-activities-single-baremetal-instance.png" alt-text="Screenshot: Aktivitäten der BareMetal-Instanzeinheit" lightbox="media/baremetal-infrastructure-portal/check-activities-single-baremetal-instance.png":::
 
Änderungen an den Azure-Metadaten der Einheit werden ebenfalls im Aktivitätsprotokoll aufgezeichnet. Neben dem initiierten Neustart sehen Sie die Aktivität **Write BareMetallnstances**. Bei dieser Aktivität werden an der BareMetal-Instanzeinheit selbst keine Änderungen vorgenommen, aber es werden Änderungen an den Azure-Metadaten der Einheit dokumentiert.
 
Eine weitere Aktivität, die aufgezeichnet wird, ist das Hinzufügen oder Löschen eines [Tags](../../../azure-resource-manager/management/tag-resources.md) zu bzw. aus einer Instanz.
 
## <a name="add-and-delete-an-azure-tag-to-an-instance"></a>Hinzufügen und Löschen eines Azure-Tags zu bzw. aus einer Instanz
 
Sie können Azure-Tags zu einer BareMetal-Instanzeinheit hinzufügen oder aus dieser löschen. Diese Art der Tagzuweisung unterscheidet sich nicht von der Zuweisung von Tags zu VMs. Wie bei VMs befinden sich die Tags in den Azure-Metadaten, und für BareMetal-Instanzen gelten dieselben Einschränkungen wie für die Tags für VMs.
 
Das Löschen von Tags funktioniert auf dieselbe Weise wie bei VMs. Das Anwenden und Löschen eines Tags wird im Aktivitätsprotokoll der BareMetal-Instanzeinheit aufgeführt.
 
## <a name="check-properties-of-an-instance"></a>Überprüfen von Eigenschaften einer Instanz
 
Beim Abrufen der Instanzen können Sie zum Abschnitt „Eigenschaften“ navigieren, um die gesammelten Daten zu den Instanzen anzuzeigen. Die gesammelten Daten umfassen die Azure-Konnektivität, das Speicher-Back-End, die ID der ExpressRoute-Verbindung, die eindeutige Ressourcen-ID und die Abonnement-ID. Diese Informationen werden in Supportanfragen oder beim Einrichten einer Speichermomentaufnahme-Konfiguration benötigt.
 
Die IP-Adresse des Speicher-NFS ist eine weitere wichtige Information. Sie isoliert Ihren Speicher auf Ihren **Mandanten** im BareMetal-Instanzstapel. Sie benötigen diese IP-Adresse auch, wenn Sie die [Konfigurationsdatei für Sicherungen von Speichermomentaufnahmen](../../../virtual-machines/workloads/sap/hana-backup-restore.md#set-up-storage-snapshots) bearbeiten.
 
:::image type="content" source="media/baremetal-infrastructure-portal/baremetal-instance-properties.png" alt-text="Screenshot: Eigenschafteneinstellung der BareMetal-Instanz" lightbox="media/baremetal-infrastructure-portal/baremetal-instance-properties.png":::
 
## <a name="restart-a-unit-through-the-azure-portal"></a>Neustart einer Einheit über das Azure-Portal
 
Es gibt verschiedene Situationen, in denen das Betriebssystem einen Neustart nicht abschließen kann und die BareMetal-Instanzeinheit deshalb neu gestartet werden muss. Sie können einen Neustart der Einheit direkt über das Azure-Portal durchführen:
 
Klicken Sie auf **Neu starten** und anschließend auf **Ja**, um den Neustart der Einheit zu bestätigen.
 
:::image type="content" source="media/baremetal-infrastructure-portal/baremetal-instance-restart.png" alt-text="Screenshot: Vorgehensweise beim Neustarten der BareMetal-Instanzeinheit":::
 
Wenn Sie eine BareMetal-Instanzeinheit neu starten, dauert es einen Augenblick, bis Sie fortfahren können. Während dieser Verzögerung wechselt der Energiezustand von **Wird gestartet** zu **Gestartet**, was darauf hinweist, dass das Betriebssystem vollständig gestartet wurde. Daher können Sie sich nach einem Neustart der Einheit nicht bei der Einheit anmelden, sobald der Zustand zu **Gestartet** wechselt.
 
>[!IMPORTANT]
>Abhängig von der Speichermenge in Ihrer BareMetal-Instanzeinheit kann der Neustart der Hardware und des Betriebssystems bis zu einer Stunde dauern.
 
## <a name="open-a-support-request-for-baremetal-instances"></a>Öffnen einer Supportanfrage für BareMetal-Instanzen
 
Sie können Supportanfragen speziell für eine BareMetal-Instanzeinheit übermitteln.
1. Erstellen Sie im Azure-Portal unter **Hilfe und Support** eine **[Neue Supportanfrage](https://rc.portal.azure.com/#create/Microsoft.Support)** , und geben Sie die folgenden Informationen für das Ticket an:
 
   - **Problemtyp:** Auswählen eines Problemtyps
 
   - **Abonnement:** Wählen Sie Ihr Abonnement aus.
 
   - **Dienst:** BareMetal-Infrastruktur
 
   - **Ressource:** Geben Sie den Namen der Instanz an.
 
   - **Zusammenfassung:** Geben Sie eine Zusammenfassung Ihrer Anfrage ein.
 
   - **Problemtyp:** Wählen Sie einen Problemtyp aus.
 
   - **Problemuntertyp:** Wählen Sie einen Untertyp für das Problem aus.

1. Klicken Sie auf die Registerkarte **Lösungen**, um nach einer Lösung für Ihr Problem zu suchen. Wenn Sie keine Lösung finden können, fahren Sie mit dem nächsten Schritt fort.

1. Klicken Sie auf die Registerkarte **Details**, und wählen Sie aus, ob es sich um ein Problem mit VMs oder BareMetal-Instanzeinheiten handelt. Anhand dieser Angaben wird die Supportanfrage an die richtigen Experten weitergeleitet.

1. Geben Sie an, wann das Problem aufgetreten ist, und wählen Sie den Instanzbereich aus.

1. Geben Sie bei Bedarf weitere Details zur Anfrage ein, und laden Sie ggf. eine Datei hoch.

1. Wählen Sie **Überprüfen + erstellen** aus, um die Anforderung zu übermitteln.
 
Es dauert bis zu fünf Werktage, bis ein Supportmitarbeiter Ihre Anfrage bestätigt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu BareMetal finden Sie unter [BareMetal-Workloadtypen](../../../virtual-machines/workloads/sap/get-started.md).