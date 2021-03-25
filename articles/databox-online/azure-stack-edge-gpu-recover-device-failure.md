---
title: Wiederherstellung nach Ausfall eines Azure Stack Edge Pro-Geräts
description: Hier wird die Wiederherstellung nach Ausfall eines Azure Stack Edge Pro-Geräts beschrieben.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: b1bfbda007619bf5bd94d47297845881758037bc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102636639"
---
# <a name="recover-from-a-failed-azure-stack-edge-pro-gpu-device"></a>Wiederherstellung nach Ausfall eines „Azure Stack Edge Pro mit GPU“-Geräts 

[!INCLUDE [applies-to-GPU-and-pro-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-sku.md)]

In diesem Artikel wird die Wiederherstellung nach einem nicht tolerierbaren Fehler auf Ihrem „Azure Stack Edge Pro mit GPU“-Gerät beschrieben. Ein nicht tolerierbarer Fehler auf einem „Azure Stack Edge Pro mit GPU“-Gerät erfordert einen Geräteaustausch.

## <a name="before-you-begin"></a>Vorbereitung

Stellen Sie sicher, dass Sie über Folgendes verfügen:

- Sie haben den Microsoft-Support bezüglich des Geräteausfalls kontaktiert, und Ihnen wurde ein Geräteaustausch empfohlen. 
- Sichern Sie Ihre Gerätekonfiguration, wie unter [Vorbereitung auf den Ausfall eines Azure Stack Edge Pro-GPU-Geräts](azure-stack-edge-gpu-prepare-device-failure.md) beschrieben.


## <a name="configure-replacement-device"></a>Konfigurieren eines Ersatzgeräts

Wenn auf Ihrem Gerät ein nicht tolerierbarer Fehler auftritt, müssen Sie ein Ersatzgerät bestellen. Für das Ersatzgerät sind dieselben Konfigurationsschritte erforderlich. 

Rufen Sie die Gerätekonfigurationsinformationen ab, die Sie auf dem ausgefallenen Gerät gesichert haben. Mit diesen Informationen konfigurieren Sie das Ersatzgerät.  

Führen Sie zum Konfigurieren des Ersatzgeräts diese Schritte aus:

1. Sammeln Sie die erforderlichen Informationen in der [Bereitstellungsprüfliste für Ihr Azure Stack Edge Pro-GPU-Gerät](azure-stack-edge-gpu-deploy-checklist.md). Sie können die Informationen verwenden, die Sie von der vorherigen Gerätekonfiguration gespeichert haben. 
1. Bestellen Sie ein neues Gerät, das genau so konfiguriert ist wie das ausgefallene.  Um eine Bestellung durchzuführen, [erstellen Sie eine neue Azure Stack Edge-Ressource](azure-stack-edge-gpu-deploy-prep.md#) im Azure-Portal.
1. [Packen](azure-stack-edge-gpu-deploy-install.md#unpack-the-device) Sie Ihr Gerät aus, [montieren](azure-stack-edge-gpu-deploy-install.md#rack-the-device) Sie es in einem Rack und [verkabeln](azure-stack-edge-gpu-deploy-install.md#cable-the-device) sie es. 
1. [Stellen Sie eine Verbindung mit der lokalen Benutzeroberfläche des Geräts her](azure-stack-edge-gpu-deploy-connect.md).
1. Konfigurieren Sie das Netzwerk mit den gleichen IP-Adressen, die Sie für das alte Gerät verwendet haben. Die Verwendung der gleichen IP-Adressen minimiert die Auswirkungen auf Clientcomputer, die in Ihrer Umgebung verwendet werden. Informieren Sie sich über das [Konfigurieren der Netzwerkeinstellungen](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md).
1. Weisen Sie den gleichen Gerätenamen und die gleiche DNS-Domäne wie beim alten Gerät zu. So können Ihre Clients für die Kommunikation mit dem neuen Gerät denselben Gerätenamen verwenden. Weitere Informationen finden Sie unter [Tutorial: Konfigurieren der Geräteeinstellungen für Azure Stack Edge Pro-Geräte mit GPU](azure-stack-edge-gpu-deploy-set-up-device-update-time.md).
1. Konfigurieren Sie Zertifikate auf dem neuen Gerät in gleicher Weise wie auf dem alten Gerät. Beachten Sie, dass das neue Gerät über eine neue Knotenseriennummer verfügt. Wenn Sie auf dem alten Gerät Ihre eigenen Zertifikate verwendet haben, benötigen Sie ein neues Knotenzertifikat. Informieren Sie sich über das [Konfigurieren von Zertifikaten für Ihr Azure Stack Edge Pro-Gerät mit GPU](azure-stack-edge-gpu-deploy-configure-certificates.md).
1. Rufen Sie den Aktivierungsschlüssel im Azure-Portal ab, und aktivieren Sie das neue Gerät. Informieren Sie sich über das [Aktivieren des Geräts](azure-stack-edge-gpu-deploy-activate.md).

Sie können nun die Workloads bereitstellen, die Sie auf dem alten Gerät ausgeführt haben.

## <a name="restore-edge-cloud-shares"></a>Wiederherstellen von Edge-Cloudfreigaben

Führen Sie die folgenden Schritte aus, um die Daten auf den Edge-Cloudfreigaben auf Ihrem Gerät wiederherzustellen:

1. [Fügen Sie Freigaben](azure-stack-edge-gpu-manage-shares.md#add-a-share) mit denselben Freigabenamen hinzu, die zuvor auf dem ausgefallenen Gerät erstellt wurden. Stellen Sie sicher, dass **Blobcontainer auswählen** beim Erstellen von Freigaben auf **Vorhandene verwenden** festgelegt ist, und wählen Sie dann den Container aus, der mit dem vorherigen Gerät verwendet wurde.
1. [Fügen Sie Benutzer hinzu](azure-stack-edge-gpu-manage-users.md#add-a-user), die Zugriff auf das vorherige Gerät hatten.
1. [Fügen Sie Speicherkonten hinzu](azure-stack-edge-gpu-manage-storage-accounts.md#add-an-edge-storage-account), die den zuvor auf dem Gerät befindlichen Freigaben zugeordnet sind. Wählen Sie beim Erstellen von Edge-Speicherkonten einen vorhandenen Container aus, und zeigen Sie auf den Container, der auf dem vorherigen Gerät dem Azure Storage-Konto zugeordnet war. Alle Daten des Geräts, die auf dem vorherigen Gerät in das Edge-Speicherkonto geschrieben wurden, wurden in den ausgewählten Speichercontainer im zugeordneten Azure Storage-Konto hochgeladen.
1. [Aktualisieren Sie die Freigabedaten](azure-stack-edge-gpu-manage-shares.md#refresh-shares) aus Azure. Dadurch werden alle Clouddaten aus dem vorhandenen Container in die Freigaben gepullt.

## <a name="restore-edge-local-shares"></a>Wiederherstellen lokaler Edge-Freigaben

Zur Vorbereitung auf einen möglichen Geräteausfall sollten Sie eine der folgenden Sicherungslösungen bereitstellen, um die Daten lokaler Freigaben aus ihren Kubernetes- oder IoT-Workloads zu schützen:

| Drittanbietersoftware           | Verweis auf die Lösung                               |
|--------------------------------|---------------------------------------------------------|
| Cohesity                       | [https://www.cohesity.com/solution/cloud/azure/](https://www.cohesity.com/solution/cloud/azure/) <br> Weitere Informationen erhalten Sie bei Cohesity.          |
| Commvault                      | [https://www.commvault.com/azure](https://www.commvault.com/azure) <br> Weitere Informationen erhalten Sie bei Commvault. |
| Veritas                        | [http://veritas.com/azure](http://veritas.com/azure) <br> Weitere Informationen erhalten Sie bei Veritas.   |
| Veeam                          | [https://www.veeam.com/kb4041](https://www.veeam.com/kb4041) <br> Weitere Informationen erhalten Sie bei Veeam. |

Nach vollständiger Konfiguration des Ersatzgeräts aktivieren Sie das Gerät für die lokale Speicherung. 

Führen Sie diese Schritte aus, um die Daten aus lokalen Freigaben wiederherzustellen:

1. [Konfigurieren Sie die Computerolle auf einem Azure Stack Edge Pro-Gerät mit GPU](azure-stack-edge-gpu-deploy-configure-compute.md).
1. [Fügen Sie wieder eine lokale Freigabe hinzu](azure-stack-edge-gpu-manage-shares.md#add-a-local-share).
1. Führen Sie das Wiederherstellungsverfahren der Datenschutzlösung Ihrer Wahl aus. Beachten Sie die Verweise in der obigen Tabelle.

## <a name="restore-vm-files-and-folders"></a>Wiederherstellen von VM-Dateien und -Ordnern

Zur Vorbereitung auf einen möglichen Geräteausfall sollten Sie eine der folgenden Sicherungslösungen bereitstellen, um die Daten auf VMs zu schützen:



| Sicherungslösungen        | Unterstütztes Betriebssystem   | Referenz                                                                |
|-------------------------|----------------|--------------------------------------------------------------------------|
| MARS-Agent (Microsoft Azure Recovery Services) für Azure Backup | Windows        | [Informationen zum MARS-Agent](../backup/backup-azure-about-mars.md)    |
| Cohesity                | Windows, Linux | [Lösungsübersicht zur Integration, Sicherung und Wiederherstellung in Microsoft Azure](https://www.cohesity.com/solution/cloud/azure) <br>Weitere Informationen erhalten Sie bei Cohesity.                          |
| Commvault               | Windows, Linux | [https://www.commvault.com/azure](https://www.commvault.com/azure) <br> Weitere Informationen erhalten Sie bei Commvault.
| Veritas                 | Windows, Linux | [https://vox.veritas.com/t5/Protection/Protecting-Azure-Stack-edge-with-NetBackup/ba-p/883370](https://vox.veritas.com/t5/Protection/Protecting-Azure-Stack-edge-with-NetBackup/ba-p/883370) <br> Weitere Informationen erhalten Sie bei Veritas.                    |
| Veeam                   | Windows, Linux | [https://www.veeam.com/kb4041](https://www.veeam.com/kb4041) <br> Weitere Informationen erhalten Sie bei Veeam. |

Nach vollständiger Konfiguration des Ersatzgeräts können Sie die VMs mit dem zuvor verwendeten VM-Image erneut bereitstellen. 

Führen Sie diese Schritte aus, um die Daten in den VMs wiederherzustellen:
 
1. [Stellen Sie eine VM aus einem VM-Image auf dem Gerät bereit](azure-stack-edge-gpu-deploy-virtual-machine-templates.md). 
1. Installieren Sie die Datenschutzlösung Ihrer Wahl auf dem virtuellen Computer.
1. Führen Sie das Wiederherstellungsverfahren der Datenschutzlösung Ihrer Wahl aus. Beachten Sie die Verweise in der obigen Tabelle.

## <a name="restore-a-kubernetes-deployment"></a>Wiederherstellen einer Kubernetes-Bereitstellung

Wenn Sie Ihre Kubernetes-Bereitstellung über Azure Arc durchgeführt haben, können Sie die Bereitstellung nach einem nicht tolerierbaren Geräteausfall wiederherstellen. Sie müssen die Kundenanwendung/die Container von dem `git`-Repository aus, in dem die Anwendungsdefinition gespeichert ist, erneut bereitstellen. [Informationen zum Bereitstellen von Kubernetes mit Azure Arc](./azure-stack-edge-gpu-deploy-stateless-application-git-ops-guestbook.md)<!--Original text: Kubernetes deployments can be restored from a non-tolerated failure with the device when deployed with Azure Arc. Customer application/containers deployed onto a Kubernetes on Azure Stack Edge via Azure Arc can be redeployed from the git repository where the application definition is. Here is a link to the article to deploy Kubernetes with Arc -->
 
## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich darüber, wie Sie [ein Azure Stack Edge Pro-Gerät zurückgeben](azure-stack-edge-return-device.md) können.
