---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: 10599b47d55a0b4ea1eb79cda3c31a2479efb7c0
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92755566"
---
Sie können eine Reihe von Datenträgern an einen virtuellen Azure-Computer anfügen. Basierend auf den Skalierbarkeits- und Leistungszielen für Datenträger eines virtuellen Computers können Sie die Anzahl und den Typ der Datenträger festlegen, die Sie benötigen, um Ihre Anforderungen an Leistung und Kapazität zu erfüllen.

> [!IMPORTANT]
> Begrenzen Sie zur Optimierung der Leistung die Anzahl stark ausgelasteter Datenträger, die an den virtuellen Computer angefügt sind, um eine mögliche Drosselung zu vermeiden. Wenn nicht alle angefügten Datenträger zum gleichen Zeitpunkt stark ausgelastet sind, kann der virtuelle Computer eine größere Anzahl von Datenträgern unterstützen.

**Verwaltete Azure-Datenträger:**

In der folgenden Tabelle sind die standardmäßigen und maximal zulässigen Limits für die Anzahl von Ressourcen pro Region und Abonnement aufgeführt. Die Grenzwerte gelten unabhängig davon, ob Datenträger mit plattformseitig verwalteten Schlüsseln oder kundenseitig verwalteten Schlüsseln verschlüsselt werden. Es gilt kein Limit für die Anzahl von verwalteten Datenträgern, Momentaufnahmen und Images pro Ressourcengruppe.  

> | Resource | Begrenzung |
> | --- | --- |
> | Verwaltete Standarddatenträger | 50.000 |
> | Verwaltete SSD Standard-Datenträger | 50.000 |
> | Verwaltete Premium-Datenträger | 50.000 |
> | Standard_LRS-Momentaufnahmen | 50.000 |
> | Standard_ZRS-Momentaufnahmen | 50.000 |
> | Verwaltetes Image | 50.000 |

**Standardspeicherkonten:** Ein Standardspeicherkonto hat eine maximale Gesamtanforderungsrate von 20.000 IOPS. Die gesamten IOPS auf allen Datenträgern eines virtuellen Computers in einem Standardspeicherkonto dürfen dieses Limit nicht überschreiten.
  
    You can roughly calculate the number of highly utilized disks supported by a single Standard storage account based on the request rate limit. For example, for a Basic tier VM, the maximum number of highly utilized disks is about 66, which is 20,000/300 IOPS per disk. The maximum number of highly utilized disks for a Standard tier VM is about 40, which is 20,000/500 IOPS per disk. 

**Storage Premium-Konten:** Ein Storage Premium-Konto hat eine maximale Gesamtdurchsatzrate von 50 GBit/s. Der Gesamtdurchsatz aller Ihrer VM-Datenträger darf dieses Limit nicht überschreiten.

