---
title: Übersicht über Azure HPC Cache
description: Beschreibt Azure HPC Cache, eine Dateizugriffsbeschleuniger-Lösung für High Performance Computing.
author: ekpgh
ms.service: hpc-cache
ms.topic: overview
ms.date: 05/29/2020
ms.author: v-erkel
ms.openlocfilehash: c0d32beeedba61bc2ecdc444685f7e81073459be
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2020
ms.locfileid: "84433694"
---
# <a name="what-is-azure-hpc-cache"></a>Was ist Azure HPC Cache?

Azure HPC Cache beschleunigt den Zugriff auf Ihre Daten für High Performance Computing-Aufgaben (HPC). Aufgrund der Zwischenspeicherung von Dateien in Azure ermöglicht Azure HPC Cache für Ihren vorhandenen Workflow die Skalierbarkeit des Cloud Computing. Dieser Dienst kann auch für Workflows verwendet werden, bei denen Ihre Daten über WAN-Links gespeichert werden, z. B. in der NAS-Umgebung (Network Attached Storage) Ihres lokalen Rechenzentrums.

Azure HPC Cache lässt sich problemlos über das Azure-Portal starten und überwachen. Vorhandene NFS-Speicher oder neue BLOB-Container können Teil des aggregierten Namespace werden. Dies vereinfacht den Clientzugriff, auch wenn Sie das Back-End-Speicherziel ändern.

## <a name="overview-video"></a>Übersichtsvideo

[![Videominiaturansicht: Übersicht über Azure HPC Cache: zum Besuchen der Videoseite klicken](media/video-1-overview.png)](https://azure.microsoft.com/resources/videos/hpc-cache-overview/)

Klicken Sie auf das obige Bild, um sich eine [Kurzübersicht über Azure HPC Cache](https://azure.microsoft.com/resources/videos/hpc-cache-overview/) anzusehen.

## <a name="use-cases"></a>Anwendungsfälle

Azure HPC Cache steigert die Produktivität am besten für Workflows wie diese:

* Workflow mit vielen Lesedateizugriffen
* Daten, die in über NFS zugänglichem Speicher, Azure Blob oder beidem gespeichert sind
* Computefarmen mit bis zu 75.000 CPU-Kernen

Azure HPC Cache kann einer Vielzahl von Workflows in vielen Branchen hinzugefügt werden. Jedes System, in dem eine große Anzahl von Computern bedarfsorientiert und mit geringer Latenzzeit auf einen Satz von Dateien zugreifen muss, profitiert von diesem Dienst. In den folgenden Abschnitten finden Sie spezifische Beispiele.

### <a name="visual-effects-vfx-rendering"></a>Rendern visueller Effekte (VFX)

Im Medien- und Unterhaltungsbereich kann Azure HPC Cache den Datenzugriff für zeitkritische Renderingprojekte beschleunigen. VFX-Renderingworkflows erfordern häufig eine Verarbeitung in letzter Minute durch eine große Anzahl von Computeknoten. Die Daten für diese Workflows befinden sich in der Regel in einer lokalen NAS-Umgebung. Azure HPC Cache kann diese Dateidaten in der Cloud zwischenspeichern, um die Latenzzeit zu verringern und die Flexibilität für das bedarfsgesteuerte Rendering zu erhöhen.

### <a name="life-sciences"></a>Biowissenschaften

Zahlreiche biowissenschaftliche Workflows können von der Zwischenspeicherung von Dateien mit horizontaler Skalierung profitieren.

Ein Forschungsinstitut, das seine genomischen Analyseworkflows zu Azure portieren möchte, kann sie mithilfe von Azure HPC Cache problemlos verschieben. Da der Cache den POSIX-Dateizugriff ermöglicht, kann der vorhandene Clientworkflow in der Cloud ausgeführt werden, ohne dass clientseitige Änderungen vorgenommen werden müssen.

Mit Azure HPC Cache kann auch die Effizienz von Aufgaben wie sekundärer Analyse, pharmakologischer Simulation oder KI-gesteuerter Bildanalyse verbessert werden.

### <a name="financial-services-analytics"></a>Analyse von Finanzdienstleistungen

Eine Azure HPC Cache-Bereitstellung kann zur Beschleunigung von quantitativen Analyseberechnungen, Risikoanalyse-Workloads und Monte-Carlo-Simulationen beitragen und Finanzdienstleistern so eine bessere Grundlage für strategische Entscheidungen bieten.

## <a name="region-availability"></a>Regionale Verfügbarkeit

Azure HPC Cache ist in diesen Azure-Regionen verfügbar:

| Nordamerika      | Europa         | Asia            | Australien      |
|--------------------|----------------|-----------------|----------------|
| East US            | Nordeuropa   | Korea, Mitte   | Australien (Osten) |
| USA (Ost) 2          | Europa, Westen    | Asien, Südosten  |               |
| USA Süd Mitte | | | |
| USA, Westen 2        | | | |

[Kundenseitig verwaltete Schlüssel](customer-keys.md) werden nur in folgenden Regionen unterstützt:

* East US
* USA Süd Mitte
* USA, Westen 2

Aktuelle Informationen zur Verfügbarkeit finden Sie auf der [Produktseite für Azure HPC Cache](https://azure.microsoft.com/services/hpc-cache).

## <a name="service-availability"></a>Dienstverfügbarkeit

Sie müssen für jedes Abonnement, das Sie mit Azure HPC Cache verwenden möchten, Zugriff anfordern. Diese Einschränkung trägt zur Sicherstellung der Dienstqualität bei.

Fordern Sie den Zugriff an, indem Sie [dieses Formular](https://aka.ms/onboard-hpc-cache) ausfüllen. Nachdem Ihr Abonnement der Zugriffsliste hinzugefügt wurde, können Sie Caches erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den Funktionen finden Sie auf der [Produktseite für Azure HPC Cache](https://azure.microsoft.com/services/hpc-cache).
* Informationen zu [Produktvoraussetzungen](hpc-cache-prereqs.md)
* [Erstellen einer Azure HPC Cache-Instanz](hpc-cache-create.md) über das Azure-Portal
