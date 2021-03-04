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
ms.openlocfilehash: 801f0f03b49d20c84a4531bd0daad7630a0ed01d
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100585055"
---
## <a name="common-scenarios"></a>Häufige Szenarios
Die folgenden Szenarien können von einem Bursting stark profitieren:
- **Verbesserung der Startzeiten**: Mit Bursting wird Ihre Instanz deutlich schneller starten. Beispielsweise ist der standardmäßige Betriebssystemdatenträger für Premium-fähige virtuelle Computer der P4-Datenträger, der eine bereitgestellte Leistung von bis zu 120 IOPS und 25 MB/s bietet. Mit Bursting kann der P4 bis zu 3500 IOPS und 170 MB/s erreichen, wodurch die Startzeit um das Sechsfache beschleunigt werden kann.
- **Behandlung von Batchaufträgen**: Die Workloads einiger Anwendungen besitzen eine zyklische Natur und erfordern für die meiste Zeit eine Basisleistung und für kurze Zeit eine höhere Leistung. Ein Beispiel hierfür ist ein Buchhaltungsprogramm, das täglich Transaktionen verarbeitet, die eine geringe Menge an Datenverkehr auf dem Datenträger erfordern. Am Ende des Monats erfolgt dann der Abgleich von Berichten, die eine viel höhere Menge an Datenverkehr auf dem Datenträger erfordern.
- **Bereitschaft für Datenverkehrsspitzen**: Webserver und ihre Anwendungen können jederzeit einen Anstieg des Datenverkehrs erfahren. Wenn Ihr Webserver durch virtuelle Computer oder Datenträger mittels Bursting gesichert ist, sind die Server besser für die Bewältigung von Datenverkehrsspitzen gerüstet. 

## <a name="bursting-flow"></a>Bursting-Flow
Das Bursting-Guthabensystem gilt in gleicher Weise sowohl auf der Ebene virtueller Computer als auch auf Datenträgerebene. Ihre Ressource, entweder ein virtueller Computer oder ein Datenträger, startet mit vollständig aufgefüllten Guthaben. Mit diesen Guthaben können Sie das Bursting 30 Minuten lang mit der maximalen Burstrate nutzen. Das Guthaben für das Bursting akkumuliert sich, wenn Ihre Ressource unter ihren Leistungsgrenzwerten für den Datenträgerspeicher arbeitet. Für alle IOPS und MB/s, die Ihre Ressource unterhalb der Leistungsgrenzwerte nutzt, beginnen Sie Guthaben zu akkumulieren. Wenn Ihre Ressource Guthaben angesammelt hat, die Sie für das Bursting verwenden können, und Ihre Workload die zusätzliche Leistung benötigt, kann Ihre Ressource diese Guthaben verwenden, um über Ihre Leistungsgrenzwerte hinauszugehen und ihr die E/A-Datenträgerleistung zu bieten, die sie zum Erfüllen der Anforderungen benötigt.



![Diagramm für Bursting-Bucket](media/managed-disks-bursting/bucket-diagram.jpg)

Wie Sie das 30-minütige Bursting nutzen möchten, bleibt vollkommen Ihnen überlassen. Sie können es fortlaufend 30 Minuten oder sporadisch im Laufe des Tages verwenden. Bei der Bereitstellung des Produkts steht das vollständige Guthaben zur Verfügung, und wenn das Guthaben erschöpft ist, dauert es weniger als einen Tag, bis das Guthaben wieder vollständig aufgefüllt ist. Sie können Ihre Bursting-Guthaben ansammeln und nach eigenem Ermessen ausgeben, und um das Bursting nutzen zu können, muss der 30-Minuten-Eimer nicht wieder voll sein. Beachten Sie bei der Burstakkumulation, dass sie für jede Ressource unterschiedlich ist, da sie auf den nicht genutzten IOPS und MB/s unterhalb ihrer Leistungswerte basiert. Dies bedeutet, dass Produkte mit einer höheren Basisleistung schneller ihre Bursting-Guthaben ansammeln können als Produkte mit einer niedrigeren Basisleistung. Bei einem P1-Datenträger im Leerlauf ohne Aktivität fallen z. B. 120 IOPS pro Sekunde an, während bei einem P20-Datenträger 2.300 IOPS pro Sekunde im Leerlauf ohne Aktivität anfallen.

## <a name="bursting-states"></a>Bursting-Zustände
Es gibt drei Zustände, in denen sich Ihre Ressource mit aktiviertem Bursting befinden kann:
- **Ansammeln**: Der E/A-Datenverkehr der Ressource verbraucht weniger als das Leistungsziel. Das Akkumulieren von Bursting-Guthaben für IOPS und MB/s erfolgt separat. Ihre Ressource kann IOPS-Guthaben ansammeln und MB/s-Guthaben ausgeben oder umgekehrt.
- **Bursting**: Der Datenverkehr der Ressource verbraucht mehr als das Leistungsziel. Der Burstdatenverkehr beansprucht das Guthaben unabhängig von IOPS und Bandbreite.
- **Konstant**: Der Datenverkehr der Ressource entspricht exakt dem Leistungsziel.

## <a name="examples-of-bursting"></a>Beispiele für Bursting

Die folgenden Beispiele zeigen, wie das Bursting mit verschiedenen Kombinationen aus VM und Datenträger funktioniert. Damit die Beispiele leicht nachvollziehbar sind, konzentrieren wir uns auf MB/s, doch dieselbe Logik wird unabhängig davon auch auf IOPS angewandt.

### <a name="non-burstable-virtual-machine-with-burstable-disks"></a>Nicht burstfähige VM mit burstfähigen Datenträgern
**Kombination aus virtuellem Computer und Datenträger:** 
- Standard_D8as_v4 
    - Nicht zwischengespeicherte MB/s: 192
- P4-Betriebssystemdatenträger
    - Bereitgestellte MB/s: 25
    - MB/s bei max. Burst: 170 
- 2 P10-Datenträger für Daten 
    - Bereitgestellte MB/s:
    - MB/s bei max. Burst: 170

 Wenn die VM gestartet wird, werden Daten vom Betriebssystemdatenträger abgerufen. Da der Betriebssystemdatenträger Teil einer VM ist, die gerade gestartet wird, ist der Betriebssystemdatenträger mit Bursting-Guthaben gefüllt. Diese Guthaben ermöglichen es dem Betriebssystemdatenträger, seinen Start mit einem Burst von 170 MB/s auszuführen.

![Die VM sendet eine Anforderung für einen Durchsatz von 192 MB/s an den Betriebssystemdatenträger, der mit einem Datendurchsatz von 170 MB/s antwortet.](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-startup.jpg)

Nachdem der Startvorgang abgeschlossen ist, wird eine Anwendung auf dem virtuellen Computer ausgeführt, die über eine nicht kritische Workload verfügt. Diese Workload erfordert 15 MB/s, die gleichmäßig auf alle Datenträger verteilt werden.

![Die Anwendung sendet eine Anforderung für einen Durchsatz von 15 MB/s an die VM, die anhand dieser Anforderung von allen Datenträgern 5 MB/s anfordert. Jeder Datenträger gibt 5 MB/s zurück, und die VM gibt 15 MB/s an die Anwendung zurück.](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-idling.jpg)

Anschließend muss die Anwendung einen Batchauftrag verarbeiten, der 192 MB/s erfordert. 2 MB/s werden vom Betriebssystemdatenträger verwendet, und die restlichen Daten werden gleichmäßig zwischen den Datenträgern für Daten aufgeteilt.

![Die Anwendung sendet eine Anforderung für einen Durchsatz von 192 MB/s an die VM, die aufgrund dieser Anforderung selbst mehrere Anforderungen an die Datenträger (für jeweils 95 MB/s) sowie an den Betriebssystemdatenträger (für 2 MB/s) übermittelt. Die Datenträger führen das Bursting durch, um die Nachfrage zu erfüllen, und alle Datenträger geben den angeforderten Durchsatz an die VM zurück, die ihn an die Anwendung zurückgibt.](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-bursting.jpg)

### <a name="burstable-virtual-machine-with-non-burstable-disks"></a>Burstfähiger virtueller Computer mit nicht burstfähigen Datenträgern
**Kombination aus virtuellem Computer und Datenträger:** 
- Standard_L8s_v2 
    - Nicht zwischengespeicherte MB/s: 160
    - MB/s bei max. Burst: 1.280
- P50-Betriebssystemdatenträger
    - Bereitgestellte MB/s: 250 
- 2 P10-Datenträger für Daten 
    - Bereitgestellte MB/s: 250

 Nach dem anfänglichen Start wird eine Anwendung auf dem virtuellen Computer ausgeführt, die eine nicht kritische Workload aufweist. Diese Workload erfordert 30 MB/s, die gleichmäßig auf alle Datenträger verteilt werden.
![Die Anwendung sendet eine Anforderung für einen Durchsatz von 30 MB/s an die VM, die anhand dieser Anforderung von allen Datenträgern 10 MB/s anfordert. Jeder Datenträger gibt 10 MB/s zurück, und die VM gibt 30 MB/s an die Anwendung zurück.](media/managed-disks-bursting/bursting-vm-nonbursting-disk/burst-vm-nonbursting-disk-normal.jpg)

Anschließend muss die Anwendung einen Batchauftrag verarbeiten, der 600 MB/s erfordert. Bei Standard_L8s_v2 wird Bursting genutzt, um diese Anforderung zu erfüllen. Die Anforderungen an die Datenträger werden dabei gleichmäßig auf P50-Datenträger verteilt.

![Die Anwendung sendet eine Anforderung für einen Durchsatz von 600 MB/s an die VM, die anhand dieser Anforderung von allen Datenträgern 200 MB/s anfordert. Jeder Datenträger gibt 200 MB/s zurück, und die VM gibt per Bursting 600 MB/s an die Anwendung zurück.](media/managed-disks-bursting/bursting-vm-nonbursting-disk/burst-vm-nonbursting-disk-bursting.jpg)
### <a name="burstable-virtual-machine-with-burstable-disks"></a>Burstfähige VM mit burstfähigen Datenträgern
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

Wenn die VM gestartet wird, nutzt sie das Bursting, um ihre Burstgrenze von 1.280 MB/s vom Betriebssystemdatenträger anzufordern, und der Betriebssystemdatenträger wird mit seiner Burstleistung von 170 MB/s antworten.

![Beim Start führt die VM das Bursting durch, um 1.280 MB/s vom Betriebssystemdatenträger anzufordern, der ebenfalls per Bursting 1.280 MB/s zurückgibt.](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-startup.jpg)

Nach dem Start starten Sie eine Anwendung mit einer nicht kritischen Workload. Diese Anwendung erfordert 15 MB/s, die gleichmäßig auf alle Datenträger verteilt werden.

![Die Anwendung sendet eine Anforderung für einen Durchsatz von 15 MB/s an die VM, die anhand dieser Anforderung von allen Datenträgern 5 MB/s anfordert. Jeder Datenträger gibt 5 MB/s zurück, und die VM gibt 15 MB/s an die Anwendung zurück.](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-idling.jpg)

Anschließend muss die Anwendung einen Batchauftrag verarbeiten, der 360 MB/s erfordert. Der Standard_L8s_v2 nutzt das Bursting, um diese Anforderung zu erfüllen, und stellt dann Anforderungen. Es werden nur 20 MB/s vom Betriebssystemdatenträger benötigt. Die verbleibenden 340 MB/s werden per Bursting von den P4-Datenträgern verarbeitet.

![Die Anwendung sendet eine Anforderung für einen Durchsatz von 360 MB/s an die VM, die anhand dieser Anforderung ein Bursting durchführt und von allen Datenträgern 170 MB/s und vom Betriebssystemdatenträger 20 MB/s anfordert. Jeder Datenträger gibt die angeforderten MB/s zurück, und die VM gibt per Bursting 360 MB/s an die Anwendung zurück.](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-bursting.jpg)