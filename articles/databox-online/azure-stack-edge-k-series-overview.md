---
title: Azure Stack Edge Mini R – Übersicht | Microsoft-Dokumentation
description: Informationen zu Azure Stack Edge Mini R, einer Speicherlösung für militärische Anwendungen, die ein tragbares physisches Gerät mit einem Akku für die Datenübertragung über WLAN in Azure nutzt.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 09/22/2020
ms.author: alkohli
ms.openlocfilehash: 20efae411ae4d2fae9bf3b5e69dbfdd98da1603a
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90985640"
---
# <a name="what-is-the-azure-stack-edge-mini-r"></a>Was ist Azure Stack Edge Mini R?

Azure Stack Edge Mini R ist eine HaaS-Lösung (Hardware-as-a-Service). Microsoft liefert Ihnen ein robustes, in der Cloud verwaltetes, sehr einfach zu transportierendes Gerät mit einer integrierten Bildverarbeitungseinheit (Vision Processing Unit, VPU), die eine beschleunigte KI-Interferenz ermöglicht und alle Funktionen eines Netzwerkspeicher-Gateways bietet. Diese Geräte eignen sich für den Einsatz in den widrigsten Umgebungen und sind für KI, Analysen und serverloses Computing optimiert.

Dieser Artikel bietet Ihnen einen Überblick über die Azure Stack Edge Mini R-Lösung, ihre wichtigsten Funktionen und die Szenarien, in denen Sie dieses Gerät bereitstellen können.


## <a name="key-capabilities"></a>Wichtige Funktionen

Azure Stack Edge Mini R weist die folgenden Merkmale auf:

|Funktion |Beschreibung  |
|---------|---------|
|Robuste Hardware| Robuste Hardware für den Einsatz in den widrigsten Umgebungen.|
|Sehr einfacher Transport| Sehr einfacher Transport, Formfaktor mit Akkubetrieb.|
|Von der Cloud verwaltet|Das Gerät und der Dienst werden über das Azure-Portal verwaltet.|
|Workloads für Edgecomputing|Ermöglicht das Analysieren, Verarbeiten und Filtern von Daten.<br>Unterstützt VMs und Workloads in Containern. |
|Beschleunigte KI-Rückschlüsse| Ausgestattet mit der Intel Movidius Myriad X-VPU.|
|Verdrahtet und drahtlos | Ermöglicht verdrahtete und drahtlose Datenübertragungen.|
|Datenzugriff     | Direkter Datenzugriff aus Azure Storage-Blobs und Azure Files über Cloud-APIs zur zusätzlichen Datenverarbeitung in der Cloud. Lokaler Cache auf dem Gerät wird für schnellen Zugriff auf zuletzt verwendete Dateien verwendet.|
|Modus „Getrennt“|  Gerät und Dienst können optional über Azure Stack Hub verwaltet werden. Bereitstellen, Ausführen und Verwalten von Anwendungen im Offlinemodus. <br> Der Modus „Getrennt“ unterstützt Szenarien für das Hochladen von Daten im Offlinemodus.|
|Unterstützte Protokolle     |Unterstützt für die Datenerfassung die Standardprotokolle SMB, NFS und REST. <br> Weitere Informationen zu unterstützten Versionen finden Sie unter [Systemanforderungen für Azure Stack Edge Mini R](azure-stack-edge-gpu-system-requirements.md).|
|Datenaktualisierung     | Möglichkeit, lokale Dateien mit den neuesten Informationen aus der Cloud zu aktualisieren.|
|Doppelte Verschlüsselung    | Das sich selbst verschlüsselnde Laufwerk stellt die erste Verschlüsselungsebene dar. VPN bietet die zweite Verschlüsselungsebene. BitLocker-Unterstützung zur lokalen Verschlüsselung von Daten und zur sicheren Datenübertragung in die Cloud über *HTTPS*.|
|Bandbreiteneinschränkung| Drosselung, um die Bandbreitennutzung während der Spitzenzeiten einzuschränken.|

## <a name="use-cases"></a>Anwendungsfälle

Dies sind die verschiedenen Szenarien, in denen Azure Stack Edge Mini R für schnelle Machine Learning-Rückschlüsse (ML) am Edge sowie für die Vorverarbeitung von Daten verwendet werden kann, bevor diese an Azure gesendet werden.

- **Rückschluss mit Azure Machine Learning**: Mit Azure Stack Edge Mini R können Sie ML-Modelle ausführen, um schnell Ergebnisse zu erzielen, auf die Sie reagieren können, bevor die Daten in die Cloud übertragen werden. Das gesamte Dataset kann optional übertragen werden, um Ihre ML-Modelle weiter zu trainieren und zu verbessern. Weitere Informationen zur Verwendung der Azure ML-Modelle mit Hardwarebeschleunigung auf dem Azure Stack Edge Mini R-Gerät finden Sie unter [Bereitstellen von Azure ML-Modellen mit Hardwarebeschleunigung auf Azure Stack Edge Mini R](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- **Vorverarbeiten von Daten**: Transformieren Sie Daten mithilfe von Computeoptionen wie Containern oder virtuellen Computern, ehe Sie sie an Azure senden, um ein besser verwertbares Dataset zu erstellen. Die Vorverarbeitung ermöglicht Folgendes:

    - Aggregieren von Daten.
    - Ändern von Daten, um beispielsweise persönliche Daten zu entfernen.
    - Teilmengen von Daten zur Optimierung von Speicher und Bandbreite oder für die weitere Analyse.
    - Analysieren von und Reagieren auf IoT-Ereignisse.

- **Übertragen von Daten über das Netzwerk an Azure**: Verwenden Sie Azure Stack Edge Mini R, um Daten einfach und schnell in Azure zu übertragen. Dadurch werden weitere Compute- und Analysenvorgänge oder eine Archivierung ermöglicht.

## <a name="components"></a>Komponenten

Die Azure Stack Edge Mini R-Lösung umfasst eine Azure Stack Edge Pro R-Ressource, ein robustes, physisches, sehr einfach zu transportierendes Azure Stack Edge Mini R-Gerät und eine lokale Webbenutzeroberfläche.

* **Physisches Azure Stack Edge Mini R-Gerät**: Ein sehr einfach zu transportierendes Gerät mit eingebautem Akku und einem robusten, von Microsoft gelieferten Gehäuse, das zum Senden von Daten an Azure konfiguriert werden kann. Das Gerät mit Akku wiegt etwas mehr als 3 kg.

    ![Azure Stack Edge Mini R-Gerät](media/azure-stack-edge-k-series-overview/perspective-view-1.png)

* **Azure Stack Edge-Ressource:** Eine Ressource im Azure-Portal, mit der Sie ein robustes Azure Stack Edge Mini R-Gerät über eine zentrale Webbenutzeroberfläche verwalten können, auf die von verschiedenen geografischen Standorten aus zugegriffen werden kann. Verwenden Sie die Azure Stack Edge-Ressource, um Ressourcen zu erstellen und zu verwalten, Geräte und Warnungen anzuzeigen und zu verwalten sowie Freigaben zu verwalten.  

* **Lokale Webbenutzeroberfläche für Azure Stack Edge Mini R**: Mithilfe der lokalen Webbenutzeroberfläche für die anfängliche Gerätekonfiguration können Sie Diagnosen ausführen, das Azure Stack Edge Mini R-Gerät herunterfahren und neu starten, Kopierprotokolle anzeigen und den Microsoft-Support kontaktieren, um eine Serviceanfrage zu stellen.


## <a name="region-availability"></a>Regionale Verfügbarkeit

Das physische Azure Stack Edge Mini R-Gerät, die Azure-Ressource und das Zielspeicherkonto, an das Sie Daten übertragen, müssen sich nicht in derselben Region befinden.

- **Ressourcenverfügbarkeit**: Eine Liste aller Regionen, in denen die Azure Stack Edge-Ressource verfügbar ist, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). 

- **Zielspeicherkonten**: Die Speicherkonten, in denen die Daten gespeichert werden, sind in allen Azure-Regionen verfügbar. Die Regionen, in denen die Speicherkonten Azure Stack Edge Mini R-Daten speichern, sollten sich in der Nähe des Gerätestandorts befinden, um optimale Leistung zu erzielen. Ein weit entfernt vom Gerät befindliches Speicherkonto führt zu langen Wartezeiten und einer langsameren Leistung.


## <a name="next-steps"></a>Nächste Schritte

- Prüfen Sie die [Systemanforderungen für Azure Stack Edge Mini R](azure-stack-edge-gpu-system-requirements.md).


