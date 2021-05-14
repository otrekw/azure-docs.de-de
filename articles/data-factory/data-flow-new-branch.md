---
title: Mehrere Verzweigungen im Zuordnungsdatenfluss
description: Replizieren von Datenströmen im Zuordnungsdatenfluss mit mehreren Verzweigungen
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 04/16/2021
ms.openlocfilehash: f9f2bf2e2204e6b74bb8a31ac856dbe276a6e983
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107588750"
---
# <a name="creating-a-new-branch-in-mapping-data-flow"></a>Erstellen einer neuen Verzweigung im Zuordnungsdatenfluss

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Fügen Sie eine neue Verzweigung hinzu, um mehrere Vorgänge und Transformationen für denselben Datenstrom auszuführen. Das Hinzufügen einer neuen Verzweigung ist nützlich, wenn Sie dieselbe Quelle für mehrere Senken oder für Selbstverknüpfungsdaten zusammen verwenden möchten.

Eine neue Verzweigung kann ähnlich wie andere Transformationen aus der Transformationsliste hinzugefügt werden. **Neue Verzweigung** ist als Aktion nur dann verfügbar, wenn es eine Transformation gibt, die auf die zu verzweigende Transformation folgt.

![Screenshot: Option „Neuer Branch“ im Menü „Multiple inputs/outputs“ (Mehrere Eingaben/Ausgaben)](media/data-flow/new-branch2.png "Hinzufügen einer neuen Verzweigung")

Im nachstehenden Beispiel liest der Datenfluss die Daten einer Taxifahrt. Dazu ist die aus „Day“ (Tag )und „Vendor“ (Anbieter) aggregierte Ausgabe erforderlich. Statt zwei separate Datenflüsse zu erstellen, die aus derselben Quelle gelesen werden, kann eine neue Verzweigung hinzugefügt werden. Auf diese Weise können beide Aggregationen als Teil desselben Datenflusses ausgeführt werden. 

![Screenshot: Datenfluss mit zwei Branches aus der Quelle](media/data-flow/new-branch.png "Hinzufügen einer neuen Verzweigung")

> [!NOTE]
> Wenn Sie auf das Pluszeichen (+) klicken, um Ihrem Graphen Transformationen hinzuzufügen, wird die Option „Neuer Branch“ nur angezeigt, wenn nachfolgende Transformationsblöcke vorhanden sind. Das liegt daran, dass durch „Neuer Branch“ ein Verweis auf den vorhandenen Stream erstellt wird und eine weitere Upstreamverarbeitung erforderlich ist. Sollte die Option „Neuer Branch“ nicht angezeigt werden, fügen Sie zuerst eine abgeleitete Spalte oder eine andere Transformation hinzu. Wenn Sie nun zum vorherigen Block zurückkehren, wird „Neuer Branch“ als Option angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Branchen können Sie die [Datenflusstransformationen](data-flow-transformation-overview.md) verwenden.
