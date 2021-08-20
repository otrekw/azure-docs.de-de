---
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 03/23/2021
ms.openlocfilehash: 0f181a28d1522fb433a274361c96e532c6a0b708
ms.sourcegitcommit: f3b930eeacdaebe5a5f25471bc10014a36e52e5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/16/2021
ms.locfileid: "112255787"
---
### <a name="upgrade-personalizer-instance-to-multi-slot"></a>Upgraden der Instanz für die Personalisierung zur Verwendung mehrerer Slots

> [!NOTE]
> Die Personalisierung mit mehreren Slots (Vorschau) wirkt sich auf weitere Funktionen des Personalisierungsdiensts aus. Diese Änderung kann nicht rückgängig gemacht werden. Bevor Sie die Personalisierung mit mehreren Slots aktivieren, sollten Sie die Informationen zur [Personalisierung mit mehreren Slots (Vorschau)](../concept-multi-slot-personalization.md) lesen. 


1. Deaktivieren der automatischen Optimierung im Microsoft Azure-Portal. Deaktivieren Sie in der Ressource „Personalisierung“ unter **Ressourcenverwaltung** auf der Seite **Modell- und Lerneinstellungen** die automatische Optimierung und speichern Sie.

> [!NOTE]
> Die Personalisierung mit mehreren Slots funktioniert nur, wenn Sie die automatische Optimierung deaktivieren. Die automatische Optimierung für die Personalisierung mit mehreren Slots wird in der Zukunft unterstützt. 

2. Aktualisieren Sie die Personalisierung auf mehrere Slots im Azure-Portal in der Ressource „Personalisierung“, indem Sie auf der Seite **Modell- und Lerneinstellungen** unter der **Ressourcenverwaltung** auf die Option **Lerneinstellungen exportieren** klicken. Das **Argumentfeld** in der heruntergeladenen JSON-Datei beginnt mit **--cb_explore_adf**. Ändern Sie dies in **--ccb_explore_adf** und speichern Sie die Datei. CB (kontextbezogene Banditen) und CCB (bedingt kontextbezogene Banditen) sind die Algorithmen, die von der Personalisierung für Vorgänge mit einem Slot bzw. mehreren Slots verwendet werden. ADF (Aktionsabhängige Funktionen) bedeutet, dass die Aktionen mit Funktionen ausgedrückt/identifiziert werden.

![Lerneinstellungen vor der Änderung](../media/settings/learning-settings-pre-upgrade.png)

![Lerneinstellungen nach der Änderung](../media/settings/learning-settings-post-upgrade.png)

Navigieren Sie auf der gleichen Registerkarte im Portal unter **Lerneinstellungen importieren** zu Ihrer zuletzt geänderten JSON-Datei, und laden Sie die Datei hoch. Durch diesen Vorgang wird Ihre Personalisierungsinstanz für die Unterstützung mehrerer Slots aktualisiert, sodass ab diesem Zeitpunkt das Einstufen von Aufrufen nach Rangfolge und Relevanz mit mehreren Slots unterstützt wird.


