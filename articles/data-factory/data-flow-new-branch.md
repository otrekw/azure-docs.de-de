---
title: Mehrere Verzweigungen im Zuordnungsdatenfluss
description: Replizieren von Datenströmen im Zuordnungsdatenfluss mit mehreren Verzweigungen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 01/08/2020
ms.openlocfilehash: daebd6a4a00d82706ceda68f94736f21b8fe5a84
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "81606392"
---
# <a name="creating-a-new-branch-in-mapping-data-flow"></a>Erstellen einer neuen Verzweigung im Zuordnungsdatenfluss

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Fügen Sie eine neue Verzweigung hinzu, um mehrere Vorgänge und Transformationen für denselben Datenstrom auszuführen. Das Hinzufügen einer neuen Verzweigung ist nützlich, wenn Sie dieselbe Quelle für mehrere Senken oder für Selbstverknüpfungsdaten zusammen verwenden möchten.

Eine neue Verzweigung kann ähnlich wie andere Transformationen aus der Transformationsliste hinzugefügt werden. **Neue Verzweigung** ist als Aktion nur dann verfügbar, wenn es eine Transformation gibt, die auf die zu verzweigende Transformation folgt.

![Hinzufügen einer neuen Verzweigung](media/data-flow/new-branch2.png "Hinzufügen einer neuen Verzweigung")

Im nachstehenden Beispiel liest der Datenfluss die Daten einer Taxifahrt. Dazu ist die aus „Day“ (Tag )und „Vendor“ (Anbieter) aggregierte Ausgabe erforderlich. Statt zwei separate Datenflüsse zu erstellen, die aus derselben Quelle gelesen werden, kann eine neue Verzweigung hinzugefügt werden. Auf diese Weise können beide Aggregationen als Teil desselben Datenflusses ausgeführt werden. 

![Hinzufügen einer neuen Verzweigung](media/data-flow/new-branch.png "Hinzufügen einer neuen Verzweigung")
