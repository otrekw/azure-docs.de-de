---
title: 'Häufig gestellte Fragen: Azure-VM-Größen ohne lokalen temporären Datenträger'
description: In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zu Microsoft Azure-VM-Größen ohne lokalen temporären Datenträger.
author: brbell
ms.service: virtual-machines
ms.topic: conceptual
ms.subservice: sizes
ms.author: brbell
ms.reviewer: mimckitt
ms.date: 06/15/2020
ms.openlocfilehash: bd4dcbdc7ab13d18ef7f2d7102c56d1bd8d8758d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104582101"
---
# <a name="azure-vm-sizes-with-no-local-temporary-disk"></a>Azure-VM-Größen ohne lokalen temporären Datenträger 
In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zu Azure-VM-Größen, die nicht über einen lokalen temporären Datenträger verfügen. Weitere Informationen zu diesen VM-Größen finden Sie in den [Spezifikationen für die Dv4- und Dsv4-Serie (universelle Workloads)](dv4-dsv4-series.md) oder den [Spezifikationen für die Ev4- und Esv4-Serie (arbeitsspeicheroptimierte Workloads)](ev4-esv4-series.md).

## <a name="what-does-no-local-temp-disk-mean"></a>Was bedeutet es, wenn eine VM keinen lokalen temporären Datenträger hat? 
Bisher hatten unsere VM-Größen (z. B. „Standard_D2s_v3“ oder „Standard_E48_v3“) einen kleinen lokalen Datenträger (d. h. ein Laufwerk „D:“). Bei den neuen VM-Größen gibt es diesen kleinen lokalen Datenträger nicht mehr, Sie können jedoch weiterhin HDD Standard-, SSD Premium- oder SSD Ultra-Datenträger anfügen.

## <a name="what-if-i-still-want-a-local-temp-disk"></a>Was kann ich tun, wenn ich weiterhin einen lokalen temporären Datenträger haben möchte?
Für Workloads, die einen lokalen temporären Datenträger erfordern, stehen Ihnen unsere neuen VM-Größen [Ddv4 und Ddsv4](ddv4-ddsv4-series.md) bzw. [Edv4 und Edsv4](edv4-edsv4-series.md) zur Verfügung. Diese Größen bieten im Vergleich zu den vorherigen v3-Größen einen um 50 % größeren temporären Datenträger.

> [!NOTE]
> Der lokale temporäre Datenträger ist nicht persistent. Verwenden Sie die Optionen „HDD Standard“, „SSD Premium“ oder „SSD Ultra“, um die Persistenz Ihrer Daten sicherzustellen. 

## <a name="what-are-the-differences-between-these-new-vm-sizes-and-the-general-purpose-dv3dsv3-or-the-memory-optimized-ev3esv3-vm-sizes-that-i-am-used-to"></a>Wie unterscheiden sich diese neuen VM-Größen von den universellen VM-Größen Dv3/Dsv3 oder den arbeitsspeicheroptimierten VM-Größen Ev3/Esv3, die ich kenne? 
| v3-VM-Familien mit lokalem temporärem Datenträger   | Neue v4-Familien mit lokalem temporärem Datenträger | Neue v4-Familien ohne lokalen temporären Datenträger |
|---|---|---|
| Dv3   | Ddv4 | Dv4 |
| Dsv3 | Ddsv4  | Dsv4 |
| Ev3   | Edv4  | Ev4 |
| Esv3 | Edsv4 |    Esv4 | 

## <a name="can-i-resize-a-vm-size-that-has-a-local-temp-disk-to-a-vm-size-with-no-local-temp-disk"></a>Kann ich eine VM-Größe mit lokalem temporärem Datenträger in eine VM-Größe ohne lokalen temporären Datenträger ändern?  
Nein. Eine Größenänderung ist nur für die folgenden Kombinationen zulässig: 

1. VM (mit lokalem temporärem Datenträger) -> VM (mit lokalem temporärem Datenträger) und 
2. VM (ohne lokalen temporären Datenträger) -> VM (ohne lokalen temporären Datenträger) 

Eine Problemumgehungsmöglichkeit finden Sie in der Antwort auf die nächste Frage.

> [!NOTE]
> Wenn ein Image vom Ressourcendatenträger abhängig ist oder eine Auslagerungsdatei auf dem lokalen temporären Datenträger vorhanden ist, funktionieren die Images ohne Datenträger nicht. Verwenden Sie in diesem Fall stattdessen die Alternative mit Datenträger. 

## <a name="how-do-i-migrate-from-a-vm-size-with-local-temp-disk-to-a-vm-size-with-no-local-temp-disk"></a>Wie migriere ich von einer VM-Größe mit lokalem temporärem Datenträger zu einer ohne?  
Eine solche Migration kann wie folgt durchgeführt werden: 

1. Stellen Sie als lokaler Administrator eine Verbindung mit dem virtuellen Computer her, der über einen lokalen temporären Datenträger verfügt (z. B. ein Laufwerk D:).
2. Befolgen Sie die Anweisungen im Abschnitt „Temporäres Verschieben von „pagefile.sys“ zu Laufwerk C“ des Artikels [Verwenden des Laufwerks D: als Datenlaufwerk auf einem virtuellen Windows-Computer](./windows/change-drive-letter.md), um die Auslagerungsdatei vom lokalen temporären Datenträger (Laufwerk D:) auf das Laufwerk C: zu verschieben.

   > [!NOTE]
   > Befolgen Sie die Anweisungen im Abschnitt „Temporäres Verschieben von „pagefile.sys“ zu Laufwerk C“ des Artikels „Verwenden des Laufwerks D: als Datenlaufwerk auf einem virtuellen Windows-Computer“, um die Auslagerungsdatei vom lokalen temporären Datenträger (Laufwerk D:) auf das Laufwerk C: zu verschieben. **Ein Abweichen von den beschriebenen Schritten führt zu der Fehlermeldung: „Die Größe der VM kann nicht geändert werden, weil der Wechsel von einer VM-Größe mit Ressourcendatenträger zu einer VM-Größe ohne Ressourcendatenträger und umgekehrt unzulässig ist.“**

3. Erstellen Sie eine Momentaufnahme der VM, indem Sie die im Artikel [Erstellen einer Momentaufnahme mit dem Portal oder der Azure CLI](./linux/snapshot-copy-managed-disk.md) beschriebenen Schritte ausführen. 
4. Verwenden Sie diese Momentaufnahme, um mit den unter [Erstellen eines virtuellen Computers aus einer Momentaufnahme mit der CLI](/previous-versions/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot) beschriebenen Schritten eine neue datenträgerlose VM zu erstellen (z. B. eine der Dv4-, Dsv4-, Ev4- oder Esv4-Serie). 

## <a name="do-these-vm-sizes-support-both-linux-and-windows-operating-systems-os"></a>Unterstützen diese VM-Größen sowohl Linux- als auch Windows-Betriebssysteme?
Ja.

## <a name="will-this-break-my-custom-scripts-custom-images-or-os-images-that-have-scratch-files-or-page-files-on-a-local-temp-disk"></a>Hat dies Auswirkungen auf meine benutzerdefinierten Skripts, benutzerdefinierten Images oder Betriebssystemimages, die über Scratchdateien oder Auslagerungsdateien auf einem lokalen temporären Datenträger verfügen?
Wenn das benutzerdefinierte Betriebssystemimage auf den lokalen temporären Datenträger verweist, funktioniert es bei dieser Größe ohne Datenträger möglicherweise nicht richtig.

## <a name="have-questions-or-feedback"></a>Haben Sie Fragen oder Feedback?
Füllen Sie das [Feedbackformular]( https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRUMzdCQkw0OVVRTldFUUtXSTlLQVBPUkVHSy4u) aus. 

## <a name="next-steps"></a>Nächste Schritte 
In diesem Dokument haben Sie Antworten auf die am häufigsten gestellten Fragen im Zusammenhang mit Azure-VMs ohne lokalen temporären Datenträger erhalten. Weitere Informationen zu diesen VM-Größen finden Sie in den folgenden Artikeln:

- [Spezifikationen für die Dv4- und Dsv4-Serie (universelle Workload)](dv4-dsv4-series.md)
- [Spezifikationen für die Ev4- und Esv4-Serie (arbeitsspeicheroptimierte Workload)](ev4-esv4-series.md)