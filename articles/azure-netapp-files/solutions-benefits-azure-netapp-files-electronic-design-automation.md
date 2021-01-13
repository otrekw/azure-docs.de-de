---
title: Vorteile der Verwendung von Azure NetApp Files für die elektronische Entwurfsautomatisierung | Microsoft-Dokumentation
description: Erläutert die Lösung, die Azure NetApp Files bereitstellt, um die Anforderungen der Halbleiter- und Chip-Designbranche zu erfüllen. Präsentiert Testszenarien, in denen ein Standard-Branchenbenchmark für die elektronische Entwurfsautomatisierung (Electronic Design Automation, EDA) mithilfe von Azure NetApp Files ausgeführt wird.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: b-juche
ms.openlocfilehash: fcede16619e8488796adc6f4c60af30643c1aadf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82160152"
---
# <a name="benefits-of-using-azure-netapp-files-for-electronic-design-automation"></a>Vorteile der Verwendung von Azure NetApp Files für die elektronische Entwurfsautomatisierung

Die Markteinführungszeit (Time-to-Market, TTM) ist ein wichtiger Aspekt für die Halbleiter- und Chip-Designbranche. Die Branche benötigt bei Speichern hohe Bandbreiten und niedrige Wartezeiten. In diesem Artikel wir die Lösung erläutert, die Azure NetApp Files bereitstellt, um die Anforderungen der Branche zu erfüllen. Er präsentiert Testszenarien, in denen ein Standard-Branchenbenchmark für die elektronische Entwurfsautomatisierung (Electronic Design Automation, EDA) mithilfe von Azure NetApp Files ausgeführt wird. 

## <a name="test-scenario-configurations"></a>Testszenariokonfigurationen

Die Tests umfassen drei Szenarien mit den folgenden Konfigurationen. 

|    Szenario    |    Volumes    |    Clients<br> SLES15 D16s_v3  |
|----------------|---------------|--------------------------------|
|    Eine         |    1          |    1                           |
|    Zwei         |    6          |    24                          |
|    drei       |    12         |    24                          |

Im ersten Szenario wird behandelt, bis zu welcher Grenze ein Volume verwendet werden kann.  

Im zweiten und dritten Szenario werden die Grenzwerte eines einzelnen Azure NetApp Files-Endpunkts bewertet. Diese Szenarien untersuchen die potenziellen Vorteile von E/A-Obergrenzen und -Wartezeiten.

## <a name="test-scenario-results"></a>Testszenarioergebnisse

In der folgenden Tabelle werden die Ergebnisse der Testszenarien zusammengefasst.

|    Szenario       |    E/A-Rate<br>  bei 2 ms     |    E/A-Rate<br>  an der Belastungsgrenze     |    Throughput<br>  bei 2 ms     |    Throughput<br>  an der Belastungsgrenze     |
|-------------------|---------------------------|--------------------------------|-----------------------------|----------------------------------|
|    1 Volume       |    39.601                 |    49.502                      |    692 MiB/s                 |    866 MiB/s                      |
|    6 Volumes      |    255.613                |    317.000                     |    4\.577 MiB/s               |    5\.568 MiB/s                    |
|    12 Volumes     |    256.612                |    319.196                     |    4\.577 MiB/s               |    5\.709 MiB/s                    |

Das Szenario mit einem einzelnen Volume stellt die grundlegende Anwendungskonfiguration dar. Es ist das Baselineszenario für nachfolgende Testszenarien.  

Das Szenario mit sechs Volumes zeigt einen linearen Anstieg (600 %). relativ zur Workload bei einem einzelnen Volume.  Der Zugriff auf alle Volumes innerhalb eines einzelnen virtuellen Netzwerks erfolgt über eine einzige IP-Adresse.  

Das Szenario mit 12 Volumes zeigt eine allgemeine Abnahme der Wartezeit im Vergleich zum Szenario mit sechs Volumes. Es verfügt aber über keinen entsprechenden Anstieg beim erreichbaren Durchsatz.   

Das folgende Diagramm veranschaulicht die Wartezeit und Betriebsrate für die EDA-Workload bei Azure NetApp Files.  

![Wartezeit und Betriebsrate der EDA-Workload bei Azure NetApp Files](../media/azure-netapp-files/solutions-electronic-design-automation-workload-latency-operation-rate.png)   

Das folgende Diagramm veranschaulicht die Wartezeit und den Durchsatz für die EDA-Workload bei Azure NetApp Files.  

![Wartezeit und Durchsatz für die EDA-Workload bei Azure NetApp Files](../media/azure-netapp-files/solutions-electronic-design-automation-workload-latency-throughput.png) 

## <a name="layout-of-test-scenarios"></a>Layout der Testszenarien 

In der folgenden Tabelle wird das Layout der Testszenarien zusammengefasst.

|    Testszenario     |    Gesamtzahl der Verzeichnisse     |    Gesamtanzahl de Dateien     |
|----------------------|------------------------------------|------------------------------|
|    1 Volume          |    88.000                          |    880.000                   |
|    6 Volumes         |    568.000                         |    5\.680.000                 |
|    12 Volumes        |    568.000                         |    5\.680.000                 |

Die gesamte Workload ist eine Mischung aus gleichzeitig ausgeführten funktionalen und physischen Phasen. Sie stellt einen typischen Fluss von einem Satz von EDA-Tools zu einem anderen dar.   

Die funktionale Phase besteht aus anfänglichen Spezifikationen und einem logischen Design. Die physische Phase findet statt, wenn das logische Design in einen physischen Chip umgewandelt wird. Während der Abschluss- und Tape-Out-Phasen werden abschließende Tests vorgenommen, und das Design wird an einen Hersteller (Foundry) zur Fertigung übermittelt.  

Die funktionale Phase umfasst eine Mischung aus sequenziellen und zufälligen Lese- und Schreib-E/As. Die funktionale Phase ist metadatenintensiv, wie „file stat“- und „file access“-Aufrufe. Obwohl Metadatenvorgänge effektiv keine Größe besitzen, liegen die Lese- und Schreibvorgänge zwischen weniger als 1 K und 16 K. Die meisten Lesevorgänge liegen zwischen 4 K und 16 K. Die meisten Schreibvorgänge liegen bei 4 K oder weniger. Die physische Phase besteht vollständig aus sequenziellen Lese-und Schreibvorgängen mit einer Mischung aus Vorgangsgrößen von 32 K und 64 K.  

In den obigen Diagrammen ergibt sich der größte Teil des Durchsatzes aus der sequenziellen physischen Phase des Workloads. Die E/As ergeben sich aus der kleinen zufälligen und metadatenintensiven funktionalen Phase. Beide Phasen werden parallel ausgeführt. 

Hieraus folgt, dass Sie Azure Compute mit Azure NetApp Files für das EDA-Design kombinieren können, um eine skalierbare Bandbreite zu erhalten. 

## <a name="next-steps"></a>Nächste Schritte

- [Lösungsarchitekturen mit Azure NetApp Files](azure-netapp-files-solution-architectures.md)
