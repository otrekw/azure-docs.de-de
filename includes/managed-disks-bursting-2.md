---
title: include file
description: include file
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 04/27/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: cc0dd5e76227c0814659b74afc2ac46c8ca4de73
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87102674"
---
## <a name="common-scenarios"></a>Häufige Szenarios
Die folgenden Szenarien können von einem Bursting stark profitieren:
- **Verbesserung der Startzeiten**: Mit Bursting wird Ihre Instanz deutlich schneller starten. Beispielsweise ist der standardmäßige Betriebssystemdatenträger für Premium-fähige virtuelle Computer der P4-Datenträger, der eine bereitgestellte Leistung von bis zu 120 IOPS und 25 MB/s bietet. Mit Bursting kann der P4 bis zu 3500 IOPS und 170 MB/s erreichen, wodurch die Startzeit um das Sechsfache beschleunigt werden kann.
- **Behandlung von Batchaufträgen**: Die Workloads einiger Anwendungen besitzen eine zyklische Natur und erfordern für die meiste Zeit eine Basisleistung und für kurze Zeit eine höhere Leistung. Ein Beispiel hierfür ist ein Buchhaltungsprogramm, das täglich Transaktionen verarbeitet, die eine geringe Menge an Datenverkehr auf dem Datenträger erfordern. Am Ende des Monats erfolgt dann der Abgleich von Berichten, die eine viel höhere Menge an Datenverkehr auf dem Datenträger erfordern.
- **Bereitschaft für Datenverkehrsspitzen**: Webserver und ihre Anwendungen können jederzeit einen Anstieg des Datenverkehrs erfahren. Wenn Ihr Webserver durch virtuelle Computer oder Datenträger mittels Bursting gesichert ist, sind die Server besser für die Bewältigung von Datenverkehrsspitzen gerüstet. 

## <a name="bursting-flow"></a>Bursting-Flow
Das Bursting-Guthabensystem gilt in gleicher Weise sowohl auf der Ebene virtueller Computer als auch auf Datenträgerebene. Ihre Ressource, entweder ein virtueller Computer oder ein Datenträger, startet mit vollständig aufgefüllten Guthaben. Mit diesen Guthaben können Sie das Bursting 30 Minuten lang mit der maximalen Burstrate nutzen. Das Guthaben für das Bursting akkumuliert sich, wenn Ihre Ressource unter ihren Leistungsgrenzwerten für den Datenträgerspeicher arbeitet. Für alle IOPS und MB/s, die Ihre Ressource unterhalb der Leistungsgrenzwerte nutzt, beginnen Sie Guthaben zu akkumulieren. Wenn Ihre Ressource Guthaben angesammelt hat, die Sie für das Bursting verwenden können, und Ihre Workload die zusätzliche Leistung benötigt, kann Ihre Ressource diese Guthaben verwenden, um über Ihre Leistungsgrenzwerte hinauszugehen und ihr die E/A-Datenträgerleistung zu bieten, die sie zum Erfüllen der Anforderungen benötigt.



![Diagramm für Bursting-Bucket](media/managed-disks-bursting/bucket-diagram.jpg)

Wie Sie das 30-minütige Bursting nutzen möchten, bleibt Ihnen überlassen. Sie können es fortlaufend 30 Minuten oder sporadisch im Laufe des Tages verwenden. Bei der Bereitstellung des Produkts steht das vollständige Guthaben zur Verfügung, und wenn das Guthaben erschöpft ist, dauert es weniger als einen Tag, bis das Guthaben wieder vollständig aufgefüllt ist. Sie können Ihre Bursting-Guthaben ansammeln und nach eigenem Ermessen ausgeben, und um das Bursting nutzen zu können, muss der 30-Minuten-Eimer nicht wieder voll sein. Beachten Sie bei der Burstakkumulation, dass sie für jede Ressource unterschiedlich ist, da sie auf den nicht genutzten IOPS und MB/s unterhalb ihrer Leistungswerte basiert. Dies bedeutet, dass Produkte mit einer höheren Basisleistung schneller ihre Bursting-Guthaben ansammeln können als Produkte mit einer niedrigeren Basisleistung. Bei einem P1-Datenträger im Leerlauf ohne Aktivität fallen z. B. 120 IOPS pro Sekunde an, während bei einem P20-Datenträger 2.300 IOPS pro Sekunde im Leerlauf ohne Aktivität anfallen.

## <a name="bursting-states"></a>Bursting-Zustände
Es gibt drei Zustände, in denen sich Ihre Ressource mit aktiviertem Bursting befinden kann:
- **Ansammeln**: Der E/A-Datenverkehr der Ressource verbraucht weniger als das Leistungsziel. Das Akkumulieren von Bursting-Guthaben für IOPS und MB/s erfolgt separat. Ihre Ressource kann IOPS-Guthaben ansammeln und MB/s-Guthaben ausgeben oder umgekehrt.
- **Bursting**: Der Datenverkehr der Ressource verbraucht mehr als das Leistungsziel. Der Burstdatenverkehr beansprucht das Guthaben unabhängig von IOPS und Bandbreite.
- **Konstant**: Der Datenverkehr der Ressource entspricht exakt dem Leistungsziel.

## <a name="examples-of-bursting"></a>Beispiele für Bursting
Die folgenden Beispiele zeigen, wie das Bursting mit verschiedenen Kombinationen aus virtuellem Computer und Datenträger funktioniert. Damit die Beispiele leicht nachvollziehbar sind, konzentrieren wir uns auf MB/s, doch dieselbe Logik wird unabhängig davon auch auf IOPS angewandt.

### <a name="non-burstable-virtual-machine-with-burstable-disks"></a>Nicht burstfähiger virtueller Computer mit burstfähigen Datenträgern
**Kombination aus virtuellem Computer und Datenträger:** 
- Standard_D8as_v4 
    - Nicht zwischengespeicherte MB/s: 192
- P4-Betriebssystemdatenträger
    - Bereitgestellte MB/s: 25
    - MB/s bei max. Burst: 170 
- 2 P10-Datenträger für Daten 
    - Bereitgestellte MB/s: 25
    - MB/s bei max. Burst: 170

 Wenn der virtuelle Computer gestartet wird, werden Daten vom Betriebssystemdatenträger abgerufen. Da der Betriebssystemdatenträger Teil eines virtuellen Computers ist, der gerade gestartet wird, ist der Betriebssystemdatenträger mit Bursting-Guthaben gefüllt. Diese Guthaben ermöglichen es dem Betriebssystemdatenträger, seinen Start mit einem Burst von 170 MB/s auszuführen, wie unten gezeigt:

![Virtueller Computer ohne Bursting und Startdatenträger mit Bursting](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-startup.jpg)

Nachdem der Startvorgang abgeschlossen ist, wird eine Anwendung auf dem virtuellen Computer ausgeführt, die über eine nicht kritische Workload verfügt. Diese Workload erfordert 15 MB/s, die gleichmäßig auf alle Datenträger verteilt werden:

![Virtueller Computer ohne Bursting und Datenträger mit Bursting im Leerlauf](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-idling.jpg)

Anschließend muss die Anwendung einen Batchauftrag verarbeiten, der 192 MB/s erfordert. 2 MB/s werden vom Betriebssystemdatenträger verwendet, und die restlichen Daten werden gleichmäßig zwischen den Datenträgern für Daten aufgeteilt:

![Virtueller Computer ohne Bursting und Datenträger mit Bursting beim Bursting](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-bursting.jpg)

### <a name="burstable-virtual-machine-with-non-burstable-disks"></a>Burstfähiger virtueller Computer mit nicht burstfähigen Datenträgern
**Kombination aus virtuellem Computer und Datenträger:** 
- Standard_L8s_v2 
    - Nicht zwischengespeicherte MB/s: 160
    - MB/s bei max. Burst: 1.280
- P50-Betriebssystemdatenträger
    - Bereitgestellte MB/s: 250 
- 2 P10-Datenträger für Daten 
    - Bereitgestellte MB/s: 250

 Nach dem anfänglichen Start wird eine Anwendung auf dem virtuellen Computer ausgeführt, die eine nicht kritische Workload aufweist. Diese Workload erfordert 30 MB/s, die gleichmäßig auf alle Datenträger verteilt werden: ![Virtueller Computer mit Bursting und Datenträger ohne Bursting im Leerlauf](media/managed-disks-bursting/bursting-vm-nonbursting-disk/burst-vm-nonbursting-disk-normal.jpg)

Anschließend muss die Anwendung einen Batchauftrag verarbeiten, der 600 MB/s erfordert. Der Standard_L8s_v2 nutzt das Bursting, um diese Anforderung zu erfüllen, und dann werden Anforderungen an die Datenträger gleichmäßig auf die P50-Datenträger verteilt:

![Virtueller Computer mit Bursting und Datenträger ohne Bursting beim Bursting](media/managed-disks-bursting/bursting-vm-nonbursting-disk/burst-vm-nonbursting-disk-bursting.jpg)
### <a name="burstable-virtual-machine-with-burstable-disks"></a>Burstfähiger virtueller Computer mit burstfähigen Datenträgern
**Kombination aus virtuellem Computer und Datenträger:** 
- Standard_L8s_v2 
    - Nicht zwischengespeicherte MB/s: 160
    - MB/s bei max. Burst: 1.280
- P4-Betriebssystemdatenträger
    - Bereitgestellte MB/s: 25
    - MB/s bei max. Burst: 170 
- 2 P4-Datenträger für Daten 
    - Bereitgestellte MB/s: 25
    - MB/s bei max. Burst: 170 

Wenn der virtuelle Computer gestartet wird, nutzt er das Bursting, um seine Burstgrenze von 1.280 MB/s vom Betriebssystemdatenträger anzufordern, und der Betriebssystemdatenträger wird mit seiner Burstleistung von 170 MB/s antworten:

![Virtueller Computer mit Bursting und Startdatenträger mit Bursting](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-startup.jpg)

Nachdem der Startvorgang abgeschlossen wurde, wird anschließend eine Anwendung auf dem virtuellen Computer ausgeführt. Die Anwendung verfügt über eine nicht kritische Workload, die 15 MB/s erfordert, die gleichmäßig auf alle Datenträger verteilt werden:

![Virtueller Computer mit Bursting und Datenträger mit Bursting im Leerlauf](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-idling.jpg)

Anschließend muss die Anwendung einen Batchauftrag verarbeiten, der 360 MB/s erfordert. Der Standard_L8s_v2 nutzt das Bursting, um diese Anforderung zu erfüllen, und stellt dann Anforderungen. Es werden nur 20 MB/s vom Betriebssystemdatenträger benötigt. Die verbleibenden 340 MB/s werden von den P4-Datenträgern mit Bursting verarbeitet:  

![Virtueller Computer mit Bursting und Datenträger mit Bursting beim Bursting](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-bursting.jpg)
