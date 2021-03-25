---
title: Avere vFXT für Azure
description: Hier erfahren Sie mehr über Avere vFXT for Azure, eine cloudbasierte Dateisystem-Zwischenspeicherungslösung für datenintensive HPC-Aufgaben (High Performance Computing).
author: ekpgh
ms.service: avere-vfxt
ms.topic: overview
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: 5095f05b0c9ce9061781c3558ca6c7091ef37aa5
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88270955"
---
# <a name="what-is-avere-vfxt-for-azure"></a>Was ist Avere vFXT für Azure?

Avere vFXT für Azure ist eine Dateisystem-Zwischenspeicherungslösung für datenintensive HPC-Aufgaben (High Performance Computing). Mit dieser Lösung können Sie Ihre Daten nach Bedarf zugänglich machen und dabei von der Skalierbarkeit des Cloud Computings profitieren. Das gilt sogar für Daten, die auf Ihrer eigenen lokalen Hardware gespeichert sind.

Avere vFXT unterstützt folgende gängige IT-Szenarien:

* Hybrid Cloud-Architektur: Avere vFXT for Azure kann mit einem Hardwarespeichersystem kombiniert werden, um von den Vorteilen des Cloud Computings zu profitieren, ohne Dateien verschieben zu müssen.

* Cloudbursting: Avere vFXT for Azure kann Sie dabei unterstützen, Daten für ein einzelnes Projekt in die Cloud zu verschieben oder eine dauerhafte Lift & Shift-Migration des gesamten Workflows durchzuführen.

![Diagramm mit Details des vFXT Avere-Systems in einem Azure-Abonnement, das mit Blobspeicher und einem lokalen Datencenter verbunden ist](media/avere-vfxt-hybrid.png)

Avere vFXT for Azure eignet sich optimal für folgende Situationen:

* Leseintensive Vorgänge für HPC-Workloads
* Anwendungen, die das gängige NFS-Protokoll verwenden
* Computefarmen mit 1.000 bis 40.000 CPU-Kernen
* Integration in lokale NAS-Hardware, Azure-Blobspeicher oder beides

Weitere Informationen finden Sie unter <https://azure.microsoft.com/services/storage/avere-vfxt/>.

## <a name="who-uses-avere-vfxt-for-azure"></a>Wer verwendet Avere vFXT für Azure?

Avere vFXT kann bei verschiedensten leseintensiven Computingaufgaben hilfreich sein:

### <a name="visual-effects-rendering"></a>Rendern visueller Effekte

Im Medien- und Unterhaltungsbereich kann der Avere vFXT-Cluster den Datenzugriff für zeitkritische Renderingprojekte beschleunigen. Die Möglichkeit zum Hinzufügen von mehr Zwischenspeicher und mehr Computeknoten in Azure gibt Ihnen die nötige Flexibilität für eine effiziente Bewältigung umfangreicher Projekte.

### <a name="life-sciences"></a>Biowissenschaften

Mit Avere vFXT führen Forscher ihre sekundären Analyseworkflows in Azure Compute aus und greifen an jedem beliebigen Ort auf Genomdaten zu.

In der Arzneimittelforschung können Avere vFXT-Cluster die Entdeckung neuer Medikamente beschleunigen, indem sie Forscher bei der Prognostizierung der Interaktion zwischen Arzneimittel und Ziel sowie bei der Analyse von Forschungsdaten unterstützen.

### <a name="financial-services-analytics"></a>Analyse von Finanzdienstleistungen

Ein Avere vFXT-Cluster kann zur Beschleunigung der Berechnungen für quantitative Analysen beitragen und Finanzdienstleistern so eine bessere Grundlage für strategische Entscheidungen bieten.

## <a name="features-and-specifications"></a>Funktionen und Spezifikationen

Das Avere vFXT-System umfasst mindestens drei virtuelle Edgeknoten für die Dateiverwaltung, konfiguriert in einem Cluster. Es kann in der Nähe der Clientcomputer platziert werden, die den Cluster einbinden, anstatt den Speicher direkt einzubinden.

Der vFXT Avere-Cluster speichert Dateien zwischen, wenn diese angefordert werden. Wiederholte Anforderungen in mehr als 80 Prozent der Fälle über den Cache bedient werden.

### <a name="compatibility"></a>Kompatibilität

* Kompatibel mit NAS-Hardwaresystemen von NetApp oder Dell EMC Isilon
* Kompatibel mit Azure-Blobs
* Verwendetes Protokoll: NFSv3 oder SMB2

Avere vFXT for Azure verwendet folgende Azure-Ressourcen:

|Azure-Komponente| Resource |
|----------|-----------|
|Virtuelle Computer|Mindestens drei E32s_v3|
|SSD Premium-Speicher|200 GB Speicherplatz für das Betriebssystem plus 1 bis 4 TB Cachespeicherplatz pro Knoten |
|Speicherkonto (optional) |V2|
|Back-End-Datenspeicher (optional) | Ein leerer LRS-Blobcontainer |

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie diese Artikel, um Ihre eigene Avere vFXT for Azure-Bereitstellung zu planen und zu erstellen.

* [Planen Ihres Systems](avere-vfxt-deploy-plan.md)
* [Übersicht über die Bereitstellung](avere-vfxt-deploy-overview.md)
* [Erstellen der vFXT-Bereitstellung](avere-vfxt-deploy.md)
