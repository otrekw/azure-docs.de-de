---
ms.openlocfilehash: 4df49b4e68769a907423c4edef9f8820df4d20b7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83006427"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Lokalisieren eines cloudbasierten Raumankers

Die Möglichkeit, einen zuvor gespeicherten cloudbasierten Raumanker zu lokalisieren, ist einer der Hauptgründe für die Nutzung von Azure Spatial Anchors. Ein cloudbasierter Raumanker kann auf verschiedene Arten lokalisiert werden. Sie können jeweils eine Strategie pro Watcher verwenden.
- Lokalisieren von Ankern anhand des Bezeichners
- Lokalisieren von Ankern, die mit einem zuvor lokalisierten Anker verbunden sind. Informationen zu Ankerbeziehungen finden Sie [hier](/azure/spatial-anchors/concepts/anchor-relationships-way-finding/).
- Lokalisieren eines Ankers unter Verwendung der [ungefähren Standortbestimmung](/azure/spatial-anchors/concepts/coarse-reloc/)

> [!NOTE]
> Bei jeder Lokalisierung eines Ankers wird von Azure Spatial Anchors versucht, die erfassten Umgebungsdaten zu verwenden, um die visuellen Informationen des Ankers zu erweitern. Falls Sie beim Lokalisieren eines Ankers Probleme haben, kann es hilfreich sein, einen Anker zu erstellen und diesen dann mehrfach aus unterschiedlichen Winkeln und bei unterschiedlichen Lichtverhältnissen zu lokalisieren.

Wenn Sie cloudbasierte Raumanker anhand des Bezeichners lokalisieren, sollten Sie den Bezeichner des cloudbasierten Raumankers im Back-End-Dienst Ihrer Anwendung speichern und für alle Geräte verfügbar machen, die sich ordnungsgemäß authentifizieren können. Ein entsprechendes Beispiel finden Sie unter [Tutorial: Freigeben von Raumankern für Geräte](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Instanziieren Sie ein `AnchorLocateCriteria`-Objekt, legen Sie die zu suchenden IDs fest, und rufen Sie die `CreateWatcher`-Methode für die Sitzung auf, indem Sie Ihr `AnchorLocateCriteria`-Objekt angeben.