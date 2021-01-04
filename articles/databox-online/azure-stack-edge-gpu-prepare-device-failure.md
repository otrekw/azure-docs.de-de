---
title: Vorbereitung auf den Ausfall eines Azure Stack Edge Pro-Geräts
description: Hier wird beschrieben, wie Sie Ersatz für ein ausgefallenes Azure Stack Edge Pro-Gerät erhalten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 12/11/2020
ms.author: alkohli
ms.openlocfilehash: b437ce7b6894ebefe38b32f27d370d9f8c4bfe80
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2020
ms.locfileid: "97369020"
---
# <a name="prepare-for-an-azure-stack-edge-pro-gpu-device-failure"></a>Vorbereitung auf den Ausfall eines Azure Stack Edge Pro-GPU-Geräts

Dieser Artikel hilft Ihnen bei der Vorbereitung auf einen Geräteausfall und erläutert, wie die Gerätekonfiguration und Daten auf Ihrem Azure Stack Edge Pro-GPU-Gerät gespeichert und gesichert werden. 

Der Artikel enthält keine Schritte zum Sichern von Kubernetes- und IoT-Containern, die auf Ihrem Azure Stack Edge Pro-GPU-Gerät bereitgestellt werden. 

## <a name="understand-device-failures"></a>Grundlegendes zu Geräteausfällen

Bei Ihrem Azure Stack Edge Pro-GPU-Gerät können zwei Arten von Hardwareausfällen auftreten.

- Tolerierbare Ausfälle, bei denen Sie eine Hardwarekomponente austauschen müssen. Bei diesen Ausfällen können Sie das Gerät in beeinträchtigtem Zustand betreiben. Beispiele für solche Ausfälle sind eine einzelne ausgefallene Stromversorgung oder ein einzelner ausgefallener Datenträger auf dem Gerät. In jedem dieser Fälle kann das Gerät weiterhin betrieben werden. Wenden Sie sich schnellstmöglich an Microsoft-Support, um die ausgefallenen Komponenten zu ersetzen.

- Nicht tolerierbare Fehler, bei denen Sie das gesamte Gerät ersetzen müssen – beispielsweise wenn auf dem Gerät zwei Datenträger ausgefallen sind. Wenden Sie sich in diesen Fällen sofort an Microsoft-Support. Nachdem der Support festgestellt hat, dass ein Geräteersatz erforderlich ist, unterstützt er die Ersetzung Ihres Azure Stack Edge-Geräts.

Zur Vorbereitung auf nicht tolerierbare Ausfälle müssen Sie auf Ihrem Gerät Folgendes sichern:

- Informationen zur Gerätekonfiguration
- Daten in lokalen Edge-Freigaben und Edge-Cloudfreigaben
- Dateien und Ordner, die den auf Ihrem Gerät ausgeführten VMs zugeordnet sind


## <a name="back-up-device-configuration"></a>Sichern der Gerätekonfiguration

Während der Erstkonfiguration des Geräts ist es wichtig, eine Kopie der Informationen zur Gerätekonfiguration aufzubewahren, wie in der [Bereitstellungsprüfliste](azure-stack-edge-gpu-deploy-checklist.md) beschrieben wird. Während der Wiederherstellung werden diese Konfigurationsinformationen auf das neue Ersatzgerät angewendet. 

## <a name="protect-device-data"></a>Schützen von Gerätedaten

Bei den Gerätedaten kann es sich um einen der folgenden Typen handeln:

- Daten in Edge-Cloudfreigaben
- Daten in lokalen Freigaben
- Dateien und Ordner auf VMs

In den folgenden Abschnitten werden die Schritte und Empfehlungen zum Schutz der einzelnen Datentypen auf Ihrem Gerät erläutert.

## <a name="protect-data-in-edge-cloud-shares"></a>Schützen von Daten in Edge-Cloudfreigaben

Sie können Edge-Cloudfreigaben erstellen, mit denen Daten von Ihrem Gerät in Azure ausgelagert werden. Konfigurieren Sie abhängig von der verfügbaren Netzwerkbandbreite Bandbreitenvorlagen auf Ihrem Gerät, um Datenverluste bei einem nicht tolerierbaren Ausfall zu minimieren.

> [!IMPORTANT]
> Wenn bei dem Gerät ein nicht tolerierbarer Ausfall auftritt, können lokale Daten, die nicht vom Gerät nach Azure verschoben wurden, verloren gehen. 

## <a name="protect-data-in-edge-local-shares"></a>Schützen von Daten in lokalen Edge-Freigaben

Wenn Sie Kubernetes oder IoT Edge bereitstellen, nehmen Sie die Konfiguration so vor, dass die Anwendungsdaten auf dem Gerät lokal gespeichert und mit Azure Storage nicht synchronisiert werden. Die Daten werden in einer Freigabe auf dem Gerät gespeichert. Möglicherweise ist es wichtig, die Daten in diesen Freigaben zu sichern.

Die folgenden Datenschutzlösungen von Drittanbietern können als Sicherungslösung für die Daten in lokalen SMB- oder NFS-Freigaben dienen. 

| Drittanbietersoftware           | Verweis auf die Lösung                               |
|--------------------------------|---------------------------------------------------------|
| Cohesity                       | [https://www.cohesity.com/solution/cloud/azure/](https://www.cohesity.com/solution/cloud/azure/) <br> Weitere Informationen erhalten Sie bei Cohesity.          |
| Commvault                      | [https://www.commvault.com/azure](https://www.commvault.com/azure) <br> Weitere Informationen erhalten Sie bei Commvault.          |
| Veritas                        | [http://veritas.com/azure](http://veritas.com/azure) <br> Weitere Informationen erhalten Sie bei Veritas.   |
| Veeam                          | [https://www.veeam.com/kb4041](https://www.veeam.com/kb4041) <br> Weitere Informationen erhalten Sie bei Veeam. |


## <a name="protect-files-and-folders-on-vms"></a>Schützen von Dateien und Ordnern auf VMs

Azure Stack Edge arbeitet mit Azure Backup und anderen Datenschutzlösungen von Drittanbietern, um eine Sicherungslösung zum Schutz von Daten in den auf dem Gerät bereitgestellten VMs bereitzustellen. In der folgenden Tabelle sind Verweise auf verfügbare Lösungen aufgeführt, aus denen Sie eine Auswahl treffen können.


| Sicherungslösungen        | Unterstütztes Betriebssystem   | Referenz                                                                |
|-------------------------|----------------|--------------------------------------------------------------------------|
| MARS-Agent (Microsoft Azure Recovery Services) für Azure Backup | Windows        | [Informationen zum MARS-Agent](../backup/backup-azure-about-mars.md)    |
| Cohesity                | Windows, Linux | [Lösungsübersicht zur Integration, Sicherung und Wiederherstellung in Microsoft Azure](https://www.cohesity.com/solution/cloud/azure) <br>Weitere Informationen erhalten Sie bei Cohesity.                          |
| Commvault               | Windows, Linux | [https://www.commvault.com/azure](https://www.commvault.com/azure) <br>Weitere Informationen erhalten Sie bei Commvault.                          |
| Veritas                 | Windows, Linux | [https://vox.veritas.com/t5/Protection/Protecting-Azure-Stack-Edge-with-NetBackup/ba-p/883370](https://vox.veritas.com/t5/Protection/Protecting-Azure-Stack-Edge-with-NetBackup/ba-p/883370) <br> Weitere Informationen erhalten Sie bei Veritas.                    |
| Veeam                   | Windows, Linux | [https://www.veeam.com/kb4041](https://www.veeam.com/kb4041) <br> Weitere Informationen erhalten Sie bei Veeam. |


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Wiederherstellung nach Ausfall eines Azure Stack Edge Pro-GPU-Geräts](azure-stack-edge-gpu-recover-device-failure.md).