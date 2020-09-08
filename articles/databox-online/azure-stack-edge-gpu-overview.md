---
title: Microsoft Azure Stack Edge with GPU – Übersicht | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Azure Stack Edge with GPU, eine Speicherlösung, die ein physisches Gerät für die netzwerkbasierte Übertragung in Azure verwendet.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 08/27/2020
ms.author: alkohli
ms.openlocfilehash: a8e1c83573de53962b3646304389023d91ab6dd3
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89256241"
---
# <a name="what-is-azure-stack-edge-with-gpu-preview"></a>Was ist Azure Stack Edge with GPU (Vorschau)?

Azure Stack Edge with GPU ist ein KI-fähiges Edgecomputinggerät mit Netzwerkfunktionen für die Datenübertragung. Dieser Artikel bietet Ihnen einen Überblick über die Azure Stack Edge-Lösung, ihre Vorteile, ihre wichtigsten Funktionen und die Szenarien, in denen Sie dieses Gerät bereitstellen können.

Azure Stack Edge with GPU ist eine HaaS-Lösung (Hardware-as-a-Service). Microsoft liefert Ihnen ein von der Cloud verwaltetes Gerät, das als Netzwerkspeichergateway fungiert und über einen Grafikprozessor (Graphical Processing Unit, GPU) verfügt, der beschleunigte KI-Rückschlüsse ermöglicht. 

> [!IMPORTANT]
> Azure Stack Edge with GPU befindet sich derzeit in der Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="use-cases"></a>Anwendungsfälle

Dies sind die verschiedenen Szenarien, in denen Azure Stack Edge für schnelle Machine Learning-Rückschlüsse (ML) am Edge sowie für die Vorverarbeitung von Daten verwendet werden kann, bevor diese an Azure gesendet werden.

- **Rückschluss mit Azure Machine Learning**: Mit Azure Stack Edge können Sie ML-Modelle ausführen, um schnell Ergebnisse zu erzielen, auf die Sie reagieren können, bevor die Daten in die Cloud übertragen werden. Das gesamte Dataset kann optional übertragen werden, um Ihre ML-Modelle weiter zu trainieren und zu verbessern. Weitere Informationen zur Verwendung der Azure ML-Modelle mit Hardwarebeschleunigung auf dem Azure Stack Edge-Gerät finden Sie unter [Bereitstellen von Azure ML-Modellen mit Hardwarebeschleunigung auf Azure Stack Edge](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- **Vorverarbeiten von Daten**: Transformieren Sie Daten vor dem Senden an Azure, um einen aussagekräftigeren Datensatz zu erstellen. Die Vorverarbeitung ermöglicht Folgendes: 

    - Aggregieren von Daten.
    - Ändern von Daten, um beispielsweise persönliche Daten zu entfernen.
    - Teilmengen von Daten zur Optimierung von Speicher und Bandbreite oder für die weitere Analyse.
    - Analysieren von und Reagieren auf IoT-Ereignisse. 

- **Übertragen von Daten über das Netzwerk an Azure**: Verwenden Sie Azure Stack Edge, um Daten einfach und schnell nach Azure zu übertragen. Dadurch werden weitere Berechnungen und Analysen oder eine Archivierung ermöglicht. 

## <a name="key-capabilities"></a>Wichtige Funktionen

Azure Stack Edge bietet die folgenden Funktionen:

|Funktion |BESCHREIBUNG  |
|---------|---------|
|Beschleunigte KI-Rückschlüsse| Ermöglicht durch die integrierte GPU (eine oder zwei, je nach Modell)|
|Edgecomputing      |Ermöglicht das Analysieren, Verarbeiten und Filtern von Daten. Unterstützt VMs und Kubernetes-Cluster.|
|Hohe Leistung | Hochleistungscompute und -datenübertragungen.|
|Datenzugriff     | Direkter Datenzugriff aus Azure Storage-Blobs und Azure Files über Cloud-APIs zur zusätzlichen Datenverarbeitung in der Cloud. Lokaler Cache auf dem Gerät wird für schnellen Zugriff auf zuletzt verwendete Dateien verwendet.|
|Von der Cloud verwaltet     |Das Gerät und der Dienst werden über das Azure-Portal verwaltet.  |
|Hochladen von Daten im Offlinemodus     | Der Modus „Getrennt“ unterstützt Szenarien für das Hochladen von Daten im Offlinemodus.|
|Unterstützte Protokolle     | Unterstützung der standardmäßigen SMB-, NFS- und REST-Protokolle für die Datenerfassung. <br> Weitere Informationen zu den unterstützten Versionen finden Sie unter [Systemanforderungen für Azure Stack Edge](azure-stack-edge-system-requirements.md).|
|Datenaktualisierung     | Möglichkeit, lokale Dateien mit den neuesten Informationen aus der Cloud zu aktualisieren.|
|Verschlüsselung    | BitLocker-Unterstützung zur lokalen Verschlüsselung von Daten und zur sicheren Datenübertragung in die Cloud über *HTTPS*.|
|Bandbreiteneinschränkung| Drosselung, um die Bandbreitennutzung während der Spitzenzeiten einzuschränken.|
|ExpressRoute | Sicherheit durch ExpressRoute hinzugefügt. Verwenden Sie die Peeringkonfiguration, wenn Datenverkehr von lokalen Geräten an die Cloudspeicherendpunkte über ExpressRoute geleitet wird. Weitere Informationen finden Sie unter [ExpressRoute-Übersicht](../expressroute/expressroute-introduction.md).

## <a name="components"></a>Komponenten

Die Azure Stack Edge-Lösung umfasst die Azure Stack Edge-Ressource, ein physisches Azure Stack Edge-Gerät und eine lokale Webbenutzeroberfläche.

* **Physisches Azure Stack Edge-Gerät**: Ein von Microsoft bereitgestellter 1U-Rackserver, der konfiguriert werden kann, um Daten an Azure zu senden.
    
* **Azure Stack Edge-Ressource:** Eine Ressource im Azure-Portal, mit der Sie ein Azure Stack Edge-Gerät über eine zentrale Webbenutzeroberfläche verwalten können, auf die von verschiedenen geografischen Standorten aus zugegriffen werden kann. Verwenden Sie die Azure Stack Edge-Ressource, um Ressourcen zu erstellen und zu verwalten, Geräte und Warnungen anzuzeigen und zu verwalten sowie Freigaben zu verwalten.  

    Weitere Informationen finden Sie unter [Erstellen eines Auftrags für ein Azure Stack Edge-Gerät](azure-stack-edge-gpu-deploy-prep.md#create-a-new-resource).

* **Lokale Webbenutzeroberfläche für Azure Stack Edge**: Mithilfe der lokalen Webbenutzeroberfläche können Sie Diagnosen ausführen, das Azure Stack Edge-Gerät herunterfahren und neu starten, Kopierprotokolle anzeigen und den Microsoft-Support kontaktieren, um eine Serviceanfrage zu stellen.

    Informationen zur Verwendung der webbasierten Benutzeroberfläche finden Sie unter [Verwenden der webbasierten Benutzeroberfläche zum Verwalten Ihres Azure Stack Edge-Geräts](azure-stack-edge-manage-access-power-connectivity-mode.md).

## <a name="region-availability"></a>Regionale Verfügbarkeit

Das physische Azure Stack Edge-Gerät, die Azure-Ressource und das Zielspeicherkonto, an das Sie Daten übertragen, müssen sich nicht in derselben Region befinden.

- **Ressourcenverfügbarkeit:** : Für diese Vorschauversion ist die Ressource in den Regionen „USA, Osten“, „Europa, Westen“, und „Asien, Südosten“ verfügbar.
    
- **Zielspeicherkonten**: Die Speicherkonten, in denen die Daten gespeichert werden, sind in allen Azure-Regionen verfügbar. Die Regionen, in denen die Speicherkonten Azure Stack Edge-Daten speichern, sollten sich in der Nähe des Gerätestandorts befinden, um optimale Leistung zu erzielen. Ein weit entfernt vom Gerät befindliches Speicherkonto führt zu langen Wartezeiten und einer langsameren Leistung.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Azure Stack Edge-Systemanforderungen](azure-stack-edge-gpu-system-requirements.md) an.
- Machen Sie sich mit den [Einschränkungen für Azure Stack Edge](azure-stack-edge-limits.md) vertraut.
- Stellen Sie [Azure Stack Edge](azure-stack-edge-gpu-deploy-prep.md) im Azure-Portal bereit.
