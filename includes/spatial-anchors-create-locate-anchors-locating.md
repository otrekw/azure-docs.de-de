---
ms.openlocfilehash: b5fec8bbc0db78454b080a411702014bd96f7db9
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76887539"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Lokalisieren eines cloudbasierten Raumankers

Die Möglichkeit, einen zuvor gespeicherten cloudbasierten Raumanker zu lokalisieren, ist einer der Hauptgründe für die Nutzung von Azure Spatial Anchors. Ein cloudbasierter Raumanker kann auf verschiedene Arten lokalisiert werden. Sie können jeweils eine Strategie pro Watcher verwenden.
- Lokalisieren von Ankern anhand des Bezeichners
- Lokalisieren von Ankern, die mit einem zuvor lokalisierten Anker verbunden sind. Informationen zu Ankerbeziehungen finden Sie [hier](/azure/spatial-anchors/concepts/anchor-relationships-way-finding/).
- Lokalisieren eines Ankers unter Verwendung der [ungefähren Standortbestimmung](/azure/spatial-anchors/concepts/coarse-reloc/)

Wenn Sie cloudbasierte Raumanker anhand des Bezeichners lokalisieren, sollten Sie den Bezeichner des cloudbasierten Raumankers im Back-End-Dienst Ihrer Anwendung speichern und für alle Geräte verfügbar machen, die sich ordnungsgemäß authentifizieren können. Ein entsprechendes Beispiel finden Sie unter [Tutorial: Freigeben von Raumankern für Geräte](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Instanziieren Sie ein `AnchorLocateCriteria`-Objekt, legen Sie die zu suchenden IDs fest, und rufen Sie die `CreateWatcher`-Methode für die Sitzung auf, indem Sie Ihr `AnchorLocateCriteria`-Objekt angeben.