---
title: 'Häufig gestellte Fragen: Azure-VM-Größen ohne lokalen temporären Datenträger'
description: In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zu Microsoft Azure-VM-Größen ohne lokalen temporären Datenträger.
author: brbell
ms.service: virtual-machines
ms.topic: article
ms.author: brbell
ms.reviewer: mimckitt
ms.date: 06/15/2020
ms.openlocfilehash: 413f53feedc4fee0877694e3f3a3a509c4d38001
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84783539"
---
# <a name="azure-vm-sizes-with-no-local-temporary-disk"></a>Azure-VM-Größen ohne lokalen temporären Datenträger 
In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zu Azure-VM-Größen, die nicht über einen lokalen temporären Datenträger verfügen. Weitere Informationen zu diesen VM-Größen finden Sie in den [Spezifikationen für die Dv4- und Dsv4-Serie (universelle Workloads)](dv4-dsv4-series.md) oder den [Spezifikationen für die Ev4- und Esv4-Serie (arbeitsspeicheroptimierte Workloads)](ev4-esv4-series.md).

> [!IMPORTANT]
> Die VM-Größen der Serien Dv4, Dsv4, Ev4 und Esv4 sind jetzt als Public Preview verfügbar. Füllen Sie dieses [Formular](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRURE1ZSkdDUzg1VzJDN0cwWUlKTkcyUlo5Mi4u) aus, wenn Sie sich für die Public Preview registrieren möchten. 

## <a name="what-does-no-local-temp-disk-mean"></a>Was bedeutet es, wenn eine VM keinen lokalen temporären Datenträger hat? 
Bisher hatten unsere VM-Größen (z. B. „Standard_D2s_v3“ oder „Standard_E48_v3“) einen kleinen lokalen Datenträger (d. h. ein Laufwerk „D:“). Bei den neuen VM-Größen gibt es diesen kleinen lokalen Datenträger nicht mehr, Sie können jedoch weiterhin HDD Standard-, SSD Premium- oder SSD Ultra-Datenträger anfügen.

## <a name="what-if-i-still-want-local-temp-disk"></a>Was kann ich tun, wenn ich weiterhin einen lokalen temporären Datenträger haben möchte?
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

> [!NOTE]
> Wenn ein Image vom Ressourcendatenträger abhängig ist oder eine Auslagerungsdatei auf dem lokalen temporären Datenträger vorhanden ist, funktionieren die Images ohne Datenträger nicht. Verwenden Sie in diesem Fall stattdessen die Alternative mit Datenträger. 

## <a name="do-these-vm-sizes-support-both-linux-and-windows-operating-systems-os"></a>Unterstützen diese VM-Größen sowohl Linux- als auch Windows-Betriebssysteme?
Ja.

## <a name="will-this-break-my-custom-scripts-custom-images-or-os-images-that-have-scratch-files-or-page-files-on-a-local-temp-disk"></a>Hat dies Auswirkungen auf meine benutzerdefinierten Skripts, benutzerdefinierten Images oder Betriebssystemimages, die über Scratchdateien oder Auslagerungsdateien auf einem lokalen temporären Datenträger verfügen?
Wenn das benutzerdefinierte Betriebssystemimage auf den lokalen temporären Datenträger verweist, funktioniert es bei dieser Größe ohne Datenträger möglicherweise nicht richtig.

## <a name="have-questions-or-feedback"></a>Haben Sie Fragen oder Feedback?
Füllen Sie das [Feedbackformular]( https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRUMzdCQkw0OVVRTldFUUtXSTlLQVBPUkVHSy4u) aus. 

## <a name="next-steps"></a>Nächste Schritte 
In diesem Artikel haben Sie Antworten auf die am häufigsten gestellten Fragen im Zusammenhang mit Azure-VMs mit lokalem temporärem Datenträger erhalten. Weitere Informationen zu diesen VM-Größen finden Sie in den folgenden Artikeln:

- [Spezifikationen für die Dv4- und Dsv4-Serie (universelle Workload)](dv4-dsv4-series.md)
- [Spezifikationen für die Ev4- und Esv4-Serie (arbeitsspeicheroptimierte Workload)](ev4-esv4-series.md)
