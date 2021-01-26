---
title: Renderingübersicht
description: Enthält eine Einführung in die Verwendung von Azure für das Rendering und eine Übersicht über Azure Batch-Renderingfunktionen.
author: mscurrell
ms.author: markscu
ms.date: 01/14/2021
ms.topic: how-to
ms.openlocfilehash: 1cd07f9322837c03e15aaeabec993820deb3170a
ms.sourcegitcommit: c7153bb48ce003a158e83a1174e1ee7e4b1a5461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2021
ms.locfileid: "98232113"
---
# <a name="rendering-using-azure"></a>Rendern mit Azure

Das „Rendering“ ist der Prozess, bei dem 3D-Modelle in 2D-Bilder konvertiert werden. Dateien mit 3D-Szenen werden in Anwendungen wie Autodesk 3ds Max, Autodesk Maya und Blender erstellt.  Mit Renderinganwendungen, z.B. Autodesk Maya, Autodesk Arnold, Chaos Group V-Ray und Blender Cycles, werden 2D-Bilder erstellt.  In einigen Fällen werden aus den Szenendateien einzelne Bilder erstellt. Es ist aber eher üblich, mehrere Bilder zu modellieren und zu rendern und dann in einer Animation zu kombinieren.

Die Renderingworkload wird häufig für Spezialeffekte (VFX) in der Medien- und Unterhaltungsbranche eingesetzt. Rendering wird auch in vielen anderen Branchen verwendet, z. B. in der Werbebranche, im Einzelhandel, in der Öl-und Gasindustrie sowie in der Fertigungsindustrie.

Der Renderingprozess ist rechenintensiv. Es kann sein, das viele Frames bzw. Bilder erstellt werden müssen, und das Rendern jedes Bilds kann viele Stunden dauern.  Das Rendering ist daher als Workload perfekt für die Batchverarbeitung geeignet, indem Azure genutzt wird, um viele Rendervorgänge parallel auszuführen und dabei eine riesige Palette an Hardware (einschließlich GPUs) einzusetzen.

## <a name="why-use-azure-for-rendering"></a>Warum sollte Azure für das Rendering verwendet werden?

Das Rendering ist aus vielen Gründen eine Workload, die für Azure perfekt geeignet ist:

* Renderingaufträge können in viele Teile zerlegt werden, die mithilfe von mehreren VMs parallel ausgeführt werden können:
  * Animationen bestehen aus vielen Frames, und jeder Frame kann parallel gerendert werden.  Je mehr VMs für die Verarbeitung der einzelnen Frames verfügbar sind, desto schneller können alle Frames und die Animation erstellt werden.
  * Bei einigen Softwareanwendungen für das Rendering können einzelne Frames in mehrere Teile zerlegt werden, z.B. Kacheln oder Segmente.  Die Teile können jeweils separat gerendert und dann zum fertigen Bild kombiniert werden, wenn alle Vorgänge abgeschlossen sind.  Je mehr VMs verfügbar sind, desto schneller kann ein Frame gerendert werden.
* Für Renderingprojekte kann ein großer Skalierungsaufwand anfallen:
  * Einzelne Frames können komplex sein, und auch bei Verwendung von Highend-Hardware kann das Rendern viele Stunden dauern. Animationen können aus mehreren Hunderttausend Frames bestehen.  Ein riesiger Computeaufwand ist erforderlich, um in einem angemessenen Zeitraum Animationen von hoher Qualität zu rendern.  In einigen Fällen wurden mehr als 100.000 Kerne genutzt, um Tausende Frames parallel zu rendern.
* Renderingprojekte sind projektbasiert, und der Computeaufwand variiert jeweils:
  * Ordnen Sie je nach Bedarf Compute- und Speicherkapazität zu, führen Sie in Abhängigkeit der Auslastung während eines Projekts das vertikale Hoch- oder Herunterskalieren durch, und heben Sie die Zuordnung auf, wenn ein Projekt beendet ist.
  * Hierbei zahlen Sie für die Kapazität, wenn diese zugeordnet ist, aber es fallen keine Kosten an, wenn keine Auslastung vorhanden ist, z.B. zwischen Projekten.
  * Seien Sie auf Auslastungsspitzen aufgrund von unerwarteten Änderungen vorbereitet. Erhöhen Sie die Skalierung, wenn es in einer späten Phase eines Projekts zu unerwarteten Änderungen kommt und diese Änderungen nach einem knappen Zeitplan verarbeitet werden müssen.
* Wählen Sie basierend auf Anwendung, Workload und Zeitraum aus einem großen Hardwareangebot:
  * Für Azure besteht ein großes Angebot an Hardware, die mit Batch zugeordnet und verwaltet werden kann.
  * Je nach Projekt kann das beste Preis-Leistungs-Verhältnis oder auch die beste Gesamtleistung am wichtigsten sein.  Unterschiedliche Szenen bzw. Renderinganwendungen verfügen über unterschiedliche Arbeitsspeicheranforderungen.  Bei einigen Renderinganwendungen können GPUs genutzt werden, um die beste Leistung oder bestimmte Features zu erhalten. 
* Kostenreduzierung durch VMs mit niedriger Priorität oder [Spot-VMs](https://azure.microsoft.com/pricing/spot/):
  * VMs mit niedriger Priorität und Spot-VMs sind im Vergleich zu Standard-VMs zu einem günstigen Rabattpreis erhältlich und für einige Auftragsarten geeignet.
  
## <a name="existing-on-premises-rendering-environment"></a>Vorhandene lokale Renderingumgebung

Im Normalfall wird eine vorhandene lokale Renderfarm mit einer Anwendung für die Renderverwaltung verwaltet, z. B. PipelineFX Qube, Royal Render oder Thinkbox Deadline oder eine benutzerdefinierte Anwendung.  Die Anforderung besteht darin, die Kapazität der lokalen Renderfarm mit Azure-VMs zu erweitern.

Mithilfe der Azure-Infrastruktur und -Dienste wird eine Hybridumgebung erstellt, in der Azure als Ergänzung der lokalen Kapazität verwendet wird. Beispiel:

* Verwenden Sie ein [virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md), um die Azure-Ressourcen im selben Netzwerk wie die lokale Renderfarm zu platzieren.
* Verwenden Sie [Avere vFXT for Azure](../avere-vfxt/avere-vfxt-overview.md) oder [Azure HPC Cache](../hpc-cache/hpc-cache-overview.md) zum Zwischenspeichern von Quelldateien in Azure, um die Bandbreitennutzung und Latenz zu verringern und die Leistung zu maximieren.
* Stellen Sie sicher, dass sich der Lizenzserver im virtuellen Netzwerk befindet, und erwerben Sie die weiteren Lizenzen, die für die zusätzliche Azure-basierte Kapazität erforderlich sind.

## <a name="no-existing-render-farm"></a>Keine vorhandene Renderfarm

Mit Clientarbeitsstationen kann das Rendering zwar durchgeführt werden, aber die Renderinglast nimmt zu, und der Vorgang dauert zu lange, wenn nur die Kapazität der Arbeitsstationen verwendet wird.

Sie haben zwei Möglichkeiten:

* Stellen Sie einen lokalen Rendering-Manager bereit, z. B. „Royal Render“, und konfigurieren Sie eine Hybridumgebung, in der Azure eingesetzt wird, wenn zusätzliche Kapazität oder Leistung erforderlich ist. Ein Render-Manager ist speziell auf das Rendern von Workloads zugeschnitten und umfasst Plug-Ins für gängige Clientanwendungen, sodass Renderingaufträge problemlos übermittelt werden können.

* Setzen Sie eine benutzerdefinierte Lösung mit Azure Batch ein, um die Computekapazität zuzuordnen und zu verwalten und um die Auftragsplanung zum Ausführen der Renderingaufträge durchzuführen.

## <a name="next-steps"></a>Nächste Schritte

 Erfahren Sie, wie Sie [lokale Renderfarmen mit der Azure-Infrastruktur und Azure-Diensten erweitern](https://azure.microsoft.com/solutions/high-performance-computing/rendering/).

Erfahren Sie mehr über die [Azure Batch-Renderingfunktionen](batch-rendering-functionality.md).
