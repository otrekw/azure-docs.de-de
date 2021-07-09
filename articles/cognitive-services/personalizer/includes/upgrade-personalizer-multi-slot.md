---
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 03/23/2021
ms.openlocfilehash: ce9bc2807708375847594674e5c28330cbe7c23f
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110487177"
---
### <a name="upgrade-personalizer-instance-to-multislot"></a>Upgraden der Personalisierungsinstanz zur Verwendung mehrerer Slots

> [!NOTE]
> Die Personalisierung mit mehreren Slots (Vorschau) wirkt sich auf weitere Funktionen des Personalisierungsdiensts aus. Diese Änderung kann nicht rückgängig gemacht werden. Bevor Sie die Personalisierung mit mehreren Slots aktivieren, sollten Sie die Informationen zur [Personalisierung mit mehreren Slots (Vorschau)](../concept-multi-slot-personalization.md) lesen. 

Klicken Sie im Azure-Portal in der Personalisierungsressource auf der Seite **Modell- und Lerneinstellungen** auf **Lerneinstellungen exportieren**. Das **Argumentfeld** in der heruntergeladenen JSON-Datei beginnt mit **--cb_explore_adf**. Ändern Sie dies in **--ccb_explore_adf**, und lassen Sie den Rest der Datei unverändert. Speichern Sie die Datei . 

![Lerneinstellungen vor der Änderung](../media/settings/learning-settings-pre-upgrade.png)

![Lerneinstellungen nach der Änderung](../media/settings/learning-settings-post-upgrade.png)

Navigieren Sie auf der gleichen Registerkarte im Portal unter **Lerneinstellungen importieren** zu Ihrer zuletzt geänderten JSON-Datei, und laden Sie die Datei hoch. Durch diesen Vorgang wird Ihre Personalisierungsinstanz für die Unterstützung mehrerer Slots aktualisiert, sodass ab diesem Zeitpunkt das Einstufen von Aufrufen nach Rangfolge und Relevanz mit mehreren Slots unterstützt wird.
