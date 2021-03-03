---
title: Verwaltetes Datenträgerbursting
description: Erfahren Sie mehr über Datenträgerbursting für Azure-Datenträger und virtuelle Azure-Computer.
author: albecker1
ms.author: albecker
ms.date: 03/02/2021
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 4024d2b1357f3dda8216e9ebdd2055b28b064d33
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101677486"
---
# <a name="managed-disk-bursting"></a>Verwaltetes Datenträgerbursting
[!INCLUDE [managed-disks-bursting](../../includes/managed-disks-bursting.md)]

Azure [SSD Premium](disks-types.md#premium-ssd)-Datenträger bieten zwei Modelle für das Bursting:

- Ein bedarfsgesteuertes Burstingmodell (Vorschau), bei dem der Datenträger immer dann das Bursting vornimmt, wenn seine Anforderungen seine aktuelle Kapazität übersteigen. Bei diesem Modell fallen zusätzliche Gebühren beim Bursting auf dem Datenträger an. Nicht auf Guthaben basierendes Bursting ist nur auf Datenträgern mit einer Größe von mehr als 512 GiB verfügbar.
- Ein auf Guthaben basierendes Modell, bei dem das Bursting auf dem Datenträger nur erfolgt, wenn Guthaben für das Bursting im Guthaben-Bucket akkumuliert wurde. Bei diesem Modell fallen keine zusätzlichen Gebühren an, wenn das Bursting auf dem Datenträger erfolgt. Das auf Guthaben basierende Bursting ist nur auf Datenträgern mit maximal 512 GiB verfügbar.

Zusätzlich kann die [Leistungsstufe der verwalteten Datenträger geändert werden](disks-change-performance.md), was ideal sein kann, wenn Ihr Workload sonst im Burstmodus ausgeführt wird.

|  |Guthabenbasiertes Bursting  |Bedarfsgesteuertes Bursting  |Ändern der Leistungsstufe  |
|---------|---------|---------|---------|
| Szenarien|Ideal für die kurzfristige Skalierung (30 Minuten oder weniger).|Ideal für die kurzfristige Skalierung (keine Zeitbeschränkung).|Ideal, wenn Ihre Workload andernfalls ständig im Burstmodus ausgeführt werden würde.|
|Kosten     |Kostenlos         |Die Kosten sind variabel. Weitere Informationen finden Sie im Abschnitt [Abrechnung](#billing).        |Die Kosten für die einzelnen Leistungsstufen sind festgelegt. Details finden Sie unter [Preise für verwaltete Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/).         |
|Verfügbarkeit     |Nur verfügbar für SSD Premium-Datenträger mit 512 GiB und weniger.         |Nur verfügbar für SSD Premium-Datenträger, die größer als 512 GiB sind.         |Verfügbar für alle SSD Premium-Größen.         |
|Aktivierung     |Standardmäßig auf berechtigten Datenträgern aktiviert.         |Muss vom Benutzer aktiviert werden.         |Der Benutzer muss seine Leistungsstufe manuell ändern.         |

## <a name="common-scenarios"></a>Häufige Szenarios
Die folgenden Szenarien können von einem Bursting stark profitieren:
- **Verbesserung der Startzeiten**: Mit Bursting wird Ihre Instanz deutlich schneller starten. Beispielsweise ist der standardmäßige Betriebssystemdatenträger für Premium-fähige virtuelle Computer der P4-Datenträger, der eine bereitgestellte Leistung von bis zu 120 IOPS und 25 MB/s bietet. Mit Bursting kann der P4 bis zu 3500 IOPS und 170 MB/s erreichen, wodurch der Start maximal um das Sechsfache beschleunigt werden kann.
- **Verarbeiten von Batchaufträgen**: Einige Anwendungsworkloads sind zyklischer Natur. Sie benötigen die meiste Zeit eine Baselineleistung und für kurze Zeiträume eine höhere Leistung. Ein Beispiel hierfür ist ein Buchhaltungsprogramm, das tägliche Transaktionen verarbeitet, die eine geringe Menge an Datenverkehr auf dem Datenträger erfordern. Am Ende des Monats würde dieses Programm den Abgleich von Berichten abschließen, die einen viel höheren Datenverkehr benötigen.
- **Datenverkehrsspitzen**: Webserver und ihre Anwendungen können jederzeit einen Anstieg des Datenverkehrs erfahren. Wenn Ihr Webserver durch virtuelle Computer oder Datenträger gesichert ist, die Bursting verwenden, wären die Server besser für die Bewältigung von Datenverkehrsspitzen gerüstet. 

[!INCLUDE [managed-disks-bursting](../../includes/managed-disks-bursting-2.md)]

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Aktivieren des bedarfsgesteuerten Burstings finden Sie unter [Aktivieren des bedarfsgesteuerten Burstings](disks-enable-bursting.md).
Wie Sie einen Einblick in Ihre Burstingressourcen erhalten, erfahren Sie unter [Metriken zum Datenträgerbursting](disks-metrics.md).
