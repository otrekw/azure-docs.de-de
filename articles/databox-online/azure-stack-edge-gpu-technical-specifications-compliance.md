---
title: 'Microsoft Azure Stack Edge Pro mit GPU: Technische Spezifikationen und Compliance | Microsoft-Dokumentation'
description: Dieser Artikel enthält Informationen zu technischen Spezifikationen und Compliance für Ihr Azure Stack Edge Pro-Gerät mit GPU
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 03/01/2021
ms.author: alkohli
ms.openlocfilehash: 937f0d1feb0c45c1c158b5e88daf268aeb383509
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102034743"
---
# <a name="technical-specifications-and-compliance-for-azure-stack-edge-pro-with-gpu"></a>Technische Spezifikationen und Compliance für Azure Stack Edge Pro mit GPU 

Die Hardwarekomponenten Ihres Azure Stack Edge Pro-Geräts mit integriertem Grafikprozessor (Graphics Processing Unit, GPU) entsprechen den in diesem Artikel beschriebenen technischen Spezifikationen und gesetzlichen Richtlinien. In den technischen Spezifikationen werden die Hardware, Netzteile, Speicherkapazität, Gehäuse und Umgebungsstandards beschrieben.

## <a name="compute-and-memory-specifications"></a>Spezifikationen zu Compute- und Arbeitsspeicherressourcen

Das Azure Stack Edge Pro-Gerät verfügt über die folgenden Spezifikationen für Compute- und Arbeitsspeicherressourcen:

| Spezifikation           | Wert                  |
|-------------------------|----------------------------|
| CPU                     | 2 Intel Xeon Silver 4214-CPUs (Cascade Lake)<br> 24 physische Kerne (12 pro CPU)<br>48 logische Kerne (vCPUs) (24 pro CPU)       |
| Arbeitsspeicher                  | 128 GB RAM (8x16 GB) <br> Dell-kompatibles RDIMM mit 16 GB (PC4-23400 DDR4-2933Mhz 2Rx8 1.2v ECC Registered)       |


## <a name="compute-acceleration-specifications"></a>Spezifikationen zur Computebeschleunigung

Jedes Azure Stack Edge Pro-Gerät verfügt über einen Grafikprozessor (GPU), der die Implementierung von Kubernetes-, Deep Learning- und Machine Learning-Szenarien ermöglicht.

| Spezifikation           | Wert                  |
|-------------------------|----------------------------|
| GPU   | Eine oder zwei nVidia T4-GPUs <br> Weitere Informationen finden Sie unter [NVIDIA T4](https://www.nvidia.com/en-us/data-center/tesla-t4/).| 


## <a name="power-supply-unit-specifications"></a>Spezifikationen für Netzteile

Das Azure Stack Edge Pro-Gerät verfügt über zwei Netzteile mit 100-240 V und leistungsstarken Lüftern. Dadurch wird eine redundante Stromversorgungskonfiguration gewährleistet. Beim Ausfall eines Netzteils wird das Gerät mit dem anderen Netzteil normal weiterbetrieben, bis die fehlerhafte Einheit ausgetauscht wird. In der folgenden Tabelle sind die technischen Spezifikationen der Netzteile aufgeführt:

| Spezifikation           | 750 Watt-Netzteil                  |
|-------------------------|----------------------------|
| Maximale Ausgangsleistung    | 750 Watt                     |
| Häufigkeit               | 50/60 Hz                   |
| Spannungsbereichsauswahl | Automatischer Spannungsbereich: 100-240 V AC |
| Hot-Plug-fähig           | Ja                        |


## <a name="network-interface-specifications"></a>Netzwerkschnittstellenspezifikationen

Ihr Azure Stack Edge Pro-Gerät verfügt über sechs Netzwerkschnittstellen: PORT1 – PORT6.

| Spezifikation           | BESCHREIBUNG                 |
|-------------------------|----------------------------|
|  Netzwerkschnittstellen    | **2 1-GbE-Schnittstellen**: Eine Verwaltungsschnittstelle (Port 1) wird für die anfängliche Einrichtung verwendet. Dies ist standardmäßig ein statischer Port. Nach Abschluss der anfänglichen Einrichtung kann die Schnittstelle für Daten mit einer beliebigen IP-Adresse verwendet werden. Beim Zurücksetzen wird jedoch erneut eine statische IP-Adresse verwendet. <br>Die zweite Schnittstelle (Port 2) kann vom Benutzer konfiguriert und für die Datenübertragung verwendet werden. Dieser Port ist standardmäßig als DHCP-Schnittstelle konfiguriert. <br>**4 25-GbE-Schnittstellen**: Diese Datenschnittstellen (Port 3 bis Port 6) können vom Benutzer als DHCP- (Standard) oder statische Schnittstelle konfiguriert werden. Sie können auch als 10-GbE-Schnittstellen genutzt werden.  | 

Ihr Azure Stack Edge Pro-Gerät enthält folgende Netzwerkhardware:

* **Benutzerdefinierter Microsoft QLogic Cavium 25G NDC-Adapter**: Port 1 bis Port 4.
* **Mellanox Dual Port 25G ConnectX-4-Kanal-Netzwerkadapter**: Port 5 und Port 6.

Nachfolgend sind die Details für die Mellanox-Karte aufgeführt:

| Parameter           | BESCHREIBUNG                 |
|-------------------------|----------------------------|
| Modell    | ConnectX®-4 Lx EN-Netzwerkschnittstellenkarte                      |
| Modellbeschreibung               | 25 GbE-SFP28 (Dual Port); PCIe3.0 x8; ROHS R6                    |
| Teilenummer (R640) | MCX4121A-ACAT  |
| PSID (R640)           | MT_2420110034                         |

Eine vollständige Liste der unterstützten Kabel, Switches und Transceiver für diese Netzwerkkarten finden Sie unter:

- [Interoperabilitätsmatrix für Qlogic Cavium 25G NDC-Adapter](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).
- [Mit Mellanox Dual Port 25G ConnectX-4-Kanal-Netzwerkadapter kompatible Produkte](https://docs.mellanox.com/display/ConnectX4LxFirmwarev14271016/Firmware+Compatible+Products).  

## <a name="storage-specifications"></a>Speicherspezifikation

Die Azure Stack Edge Pro-Geräte verfügen über fünf NVMe DC P4610-SSDs mit 2,5 Zoll und einer Kapazität von jeweils 1,6 TB. Das Startlaufwerk ist ein SATA-SSD-Datenträger mit 240 GB. Die insgesamt verwendbare Kapazität für das Gerät beträgt etwa 4,19 TB. In der folgenden Tabelle ist die Speicherkapazität des Geräts aufgeführt.

|     Spezifikation                          |     Wert             |
|--------------------------------------------|-----------------------|
|    Anzahl von NVMe-SSDs                     |    5                  |
|    Kapazität je NVMe-SSD                |    1,6 TB             |
|    SATA-SSD-Startlaufwerke      |    1                  |
|    Kapazität des SSD-Startlaufwerks                       |    240 GB             |
|    Gesamtkapazität                          |    8,0 TB             |
|    Insgesamt nutzbare Kapazität                   |    ~ 4,19 TB          |
|    RAID-Konfiguration                      |    Direkte Speicherplätze mit einer Kombination aus Spiegelung und Parität  |
|    SAS-Controller                          |    HBA330 mit 12 GBit/s     |

<!--Remove based on feedback from Ravi
## Other hardware specifications

Your Azure Stack Edge Pro device also contains the following hardware:

* iDRAC baseboard management
* Performance fans
* Custom ID module-->

## <a name="enclosure-dimensions-and-weight-specifications"></a>Gehäuseabmessungen und Gewicht

In den folgenden Tabellen sind die verschiedenen Gehäusespezifikationen (Abmessungen und Gewicht) aufgeführt.

### <a name="enclosure-dimensions"></a>Gehäuseabmessungen

In der folgenden Tabelle sind die Abmessungen des Gehäuses eines 1U-Geräts in Millimetern und Zoll aufgeführt.

|     Gehäuse     |     Millimeter     |     Zoll     |
|-------------------|---------------------|----------------|
|    Höhe         |    44,45            |    1,75 Zoll       |
|    Breite          |    434,1            |    17,09 Zoll      |
|    Länge         |    740,4            |    29,15 Zoll      |

Die folgenden Tabellen enthalten die Abmessungen der Versandverpackung in Millimetern und Zoll.

|     Paket     |     Millimeter     |     Zoll     |
|-------------------|---------------------|----------------|
|    Höhe         |    311,2            |    12,25 Zoll          |
|    Breite          |    642,8            |    25,31 Zoll          |
|    Länge         |    1\.051,1          |    41,38 Zoll          |

### <a name="enclosure-weight"></a>Gehäusegewicht

Das Gerätepaket wiegt ca. 30 kg und sollte von zwei Personen getragen werden. Das Gewicht des Geräts hängt von der Konfiguration des Gehäuses ab.

|     Gehäuse                                 |     Weight          |
|-----------------------------------------------|---------------------|
|    Gesamtgewicht einschließlich Verpackung:       |    27,7 kg.          |
|    Gerätegewicht:                       |    16 kg.          |

## <a name="enclosure-environment-specifications"></a>Spezifikationen der Gehäuseumgebung

Dieser Abschnitt enthält die Spezifikationen im Zusammenhang mit der Gehäuseumgebung, z. B. Temperatur, Luftfeuchtigkeit und Höhenlage.

### <a name="temperature-and-humidity"></a>Temperatur und Feuchtigkeit

|     Gehäuse         |     Umgebungstemperaturspanne     |     Relative Luftfeuchtigkeit der Umgebung     |     Maximaler Kondensationspunkt     |
|-----------------------|--------------------------------------|--------------------------------------|---------------------------|
|    Bei Betrieb        |    10°C bis 35°C (50° F bis 86 F)         |    10–80 % nicht kondensierend         |    29 °C            |
|    Außerhalb des Betriebs    |    -40°C bis 65°C (-40°F bis 149°F)     |    5–95 % nicht kondensierend          |    33°C (91°F)            |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Luftströmung, Höhe, Stöße, Schwingungen, Ausrichtung, Sicherheit und EMC

|     Gehäuse                           |     Betriebsspezifikationen                                                                                                                                                                                         |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Luftströmung                              |    Die Luftströmung im System verläuft von vorne nach hinten. Das System muss mit einem hinteren Luftstromauslass mit niedrigem Druck betrieben werden. <!--Back pressure created by rack doors and obstacles should not exceed 5 pascals (0.5 mm water gauge).-->    |
| IP-Schutzart                 |    Dieser Gerätetyp für die Rackmontage im Innenbereich wird in der Regel nicht auf die Schutzart getestet (Schutz gegen Fremdkörper und Wasser für elektrische Betriebsmittel). Die Sicherheitsbewertung des Herstellers zeigt daher IPXO (kein Eindringschutz).  |
|    Maximale Betriebshöhe        |    3\.048 m (10.000 Fuß) mit einer maximalen Betriebstemperatur bei heruntergesetzter Leistung, die Sie der Tabelle unter [Spezifikationen für Betriebstemperatur bei heruntergesetzter Leistung](#operating-temperature-de-rating-specifications) entnehmen.                                                                                |
|    Maximal Höhenlage, außerhalb des Betriebs    |    12.000 Meter (39.370 Fuß)                                                                                                                                                                                         |
|    Stoßfestigkeit, bei Betrieb                   |    6 G für eine Dauer von 11 Millisekunden in sechs verschiedenen Ausrichtungen                                                                                                                                                                         |
|    Stoßfestigkeit, außerhalb des Betriebs               |    71 G für eine Dauer von zwei Millisekunden in sechs verschiedenen Ausrichtungen                                                                                                                                                                           |
|    Schwingungen, bei Betrieb               |    0,26 G<sub>QMW</sub> bei 5 Hertz bis 350 Hertz zufälliger Vibration                                                                                                                                                                                     |
|    Schwingungen, außerhalb des Betriebs           |    1,88 G<sub>QMW</sub> bei 10 Hertz bis 500 Hertz für die Dauer von 15 Minuten (von allen sechs Seiten getestet)                                                                                                                                                  |
|    Ausrichtung und Montage             |    Montage im 19-Zoll-Rack (Standard) für 1U-Geräte                                                                                                                                                                                       |
|    Sicherheit und Genehmigungen                 |    EN 60950-1:2006 +A1:2010 +A2:2013 +A11:2009 +A12:2011/IEC 60950-1:2005 ed2 +A1:2009 +A2:2013 EN 62311:2008                                                                                                                                                                       |
|    EMC                                  |    FCC A, ICES-003 <br>EN 55032:2012/CISPR 32:2012  <br>EN 55032:2015/CISPR 32:2015  <br>EN 55024:2010 +A1:2015/CISPR 24:2010 +A1:2015  <br>EN 61000-3-2:2014/IEC 61000-3-2:2014 (Klasse D)   <br>EN 61000-3-3:2013/IEC 61000-3-3:2013                                                                                                                                                                                         |
|    Energieversorgung             |    Kommissionsregelung (EU) Nr. 617/2013.                                                                                                                                                                                        |
|    RoHS-Richtlinien           |    EN 50581:2012                                                                                                                                                                                        |

### <a name="operating-temperature-de-rating-specifications"></a>Spezifikationen für Betriebstemperatur bei heruntergesetzter Leistung

|     Betriebstemperatur bei heruntergesetzter Leistung     |     Umgebungstemperaturspanne                                                         |
|--------------------------------------------|------------------------------------------------------------------------------------------|
|    Bis zu 35°C (95°F)                       |    Die maximale Temperatur reduziert sich auf über 950 Metern (3.117 Fuß) alle 300 Meter um 1°C (1°F/547 Fuß).    |
|    35°C bis 40°C (95°F bis 104°F)            |    Die maximale Temperatur reduziert sich auf über 950 Metern (3.117 Fuß) alle 175 Meter um 1°C (1°F/319 Fuß).    |
|    40°C bis 45°C (104°F bis 113°F)           |    Die maximale Temperatur reduziert sich auf über 950 Metern (3.117 Fuß) alle 125 Meter um 1°C (1°F/228 Fuß).    |

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen Ihres Azure Stack Edge Pro-Geräts](azure-stack-edge-gpu-deploy-prep.md)
