---
title: Empfehlungen
description: Empfohlene bewährte Methoden zum Erzielen besserer Ergebnisse
author: ariye
ms.author: crtreasu
ms.date: 03/12/2021
ms.topic: best-practice
ms.service: azure-object-anchors
ms.openlocfilehash: 6b9546843d88a5a7329120cca86d685d8abf3460
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061926"
---
# <a name="best-practices"></a>Bewährte Methoden

Wir empfehlen Ihnen, einige dieser Schritte auszuprobieren, um die besten Ergebnisse zu erzielen.

## <a name="ingestion"></a>Erfassung

- Überprüfen Sie die Dimensionen Ihrer physischen Objekte. Azure Object Anchors funktionieren am besten für Objekte, deren kleinste Dimension im Bereich der empfohlenen 1-10 m liegt.
- Untersuchen Sie Ihr 3D-Modell in Software wie [**MeshLab**](https://www.meshlab.net/) auf die folgenden Details.
  - Stellen Sie sicher, dass das 3D-Modell über ein Dreiecksgittermodell verfügt, und das die Dreiecke auf der äußeren Oberfläche nach außen ausgerichtet sind. Das heißt, dass die Scheitelpunkte (Vertices) so ausgerichtet sein sollten, dass die Lotrechten (Normalen) in ihrer Ausrichtung nach außen der Rechte-Hand-Regel folgen.
  - Stellen Sie sicher, dass das 3D-Modell mit den richtigen Skalierungseinheiten in Bezug auf die physischen Objekte angegeben wird. Die Einheiten sollten sich aus den folgenden rekrutieren: ***Zentimeter, Dezimeter, Fuß, Zoll, Kilometer, Meter, Millimeter, Yard***.
  - Bestätigen Sie die nominale Schwerkraftrichtung, die der vertikalen Ausrichtung des Objekts in der realen Welt entspricht. Wenn die nach unten gerichtete Vertikale/Schwerkraft des Objekts -Y ist, verwenden Sie * **(0, -1, 0)** _ oder _*_ (0, 0, -1)_** für -Z und ebenso für jede andere Richtung.
  - Stellen Sie sicher, dass das 3D-Modell in einem der unterstützten Formate codiert ist: `.glb`, `.gltf`, `.ply`, `.fbx`, `.obj`.
- Die Verarbeitung eines Modells mit hoher Detailgenauigkeit (LOD, Level-of-Detail) durch unseren Modellkonvertierungsdienst kann viel Zeit in Anspruch nehmen. Zur Effizienzsteigerung können Sie Ihr 3D-Modell vorverarbeiten, um die Innenflächen zu entfernen.

## <a name="detection"></a>Erkennung

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Mixed-Reality/Azure-Object-Anchors-Detection-and-Alignment-Best-Practices/player]

- Das bereitgestellte Runtime-SDK benötigt einen benutzerseitig bereitgestellten Suchbereich, um nach den physischen Objekten zu suchen und diese zu erkennen. Der Suchbereich kann ein Begrenzungsrahmen, eine Kugel, ein Pyramidenstumpf (Frustum) oder eine beliebige Kombination daraus sein. Um eine falsche Erkennung zu vermeiden, empfiehlt es sich, einen Suchbereich festzulegen, der groß genug ist, um das Objekt abzudecken. Wenn Sie die bereitgestellten Beispiel-Apps verwenden, können Sie auf einer Seite des Objekts, ungefähr 2 Meter von der nächstgelegenen Oberfläche entfernt, stehen und die App starten.
- Bevor Sie die App „Azure Object Anchors“ auf einem HoloLens 2-Gerät starten, entfernen Sie die Hologramme in der Nähe Ihres Arbeitsplatzes in den Haupteinstellungen Ihrer Geräte über ***Einstellungen->System->Hologramme***.

  Mit diesem Schritt wird sichergestellt, dass alte und nicht mehr relevante Hologramme nicht dauerhaft bleiben und so verwirrende Visualisierungen für das aktuell in der Ansicht befindliche Objekt erzeugen, wenn ein neues Objekt, wie z. B. ein Auto, im selben Bereich vorhanden ist, der zuvor von einem anderen Objekt belegt wurde, oder wenn sich das Objekt aus dem Zielbereich hinausbewegt hat.
- Scannen Sie das Objekt, z. B. ein Auto, nach dem Entfernen der Hologramme und vor dem Starten der App, indem Sie das Objekt aus einer Entfernung von ca. 1–2 Metern ansehen, während Sie das Gerät tragen, und ein- oder zweimal langsam und vollständig um das Objekt herumgehen.

  Durch diesen Schritt wird sichergestellt, dass jegliche Restoberflächenschätzungen, die von früheren Objekten und Scans in Ihrem Bereich erzeugt wurden, durch die Oberflächen des aktuellen Zielobjekts aktualisiert werden, mit dem Sie arbeiten möchten. Andernfalls sieht die App möglicherweise doppelte Geisteroberflächen, die zu einer ungenauen Ausrichtung Ihres 3D-Modells und der zugeordneten Hologramme führen. Durch das Vorscannen des Objekts wird auch die Erkennungswartezeit von Azure Object Anchors erheblich reduziert, beispielsweise von 30  auf 5 Sekunden.
- Bei dunklen und hochgradig reflektierenden Objekten müssen Sie das Objekt möglicherweise aus einem geringeren Abstand scannen und dabei außerdem den Kopf auf und ab bewegen sowie von links nach rechts und umgekehrt, damit das Gerät Oberflächen aus mehreren Winkeln und mehreren Abständen sehen kann.
- Wenn Sie eine falsche Objekterkennung sehen, z. B. eine gespiegelte Ausrichtung oder eine fehlerhafte Haltung wie bei einem gekippten Modell, sollten Sie die räumliche Zuordnung visualisieren. Häufig resultieren die falschen Ergebnisse aus einer schlechten oder unvollständigen Oberflächenrekonstruktion. Sie können die Hologramme entfernen, das Objekt scannen und die Objekterkennung in der App erneut ausführen.
- Das bereitgestellte Runtime-SDK bietet ein paar Parameter, die Benutzern eine Feinabstimmung der Erkennung erlauben, wie in unseren Beispiel-Apps veranschaulicht. Die Standardparameter funktionieren für die meisten Objekte gut. Wenn Sie feststellen, dass Sie sie für bestimmte Objekte anpassen müssen, finden Sie hier einige Empfehlungen:
  - Verwenden Sie einen niedrigeren Schwellenwert für die Oberflächenabdeckung, wenn das physische Objekt groß, dunkel oder glänzend ist.
  - Lassen Sie eine kleine Skalierungsänderung (z. B. 0,1) für ein großes Objekt wie ein Auto zu.
  - Lassen Sie eine gewisse graduelle Abweichung zwischen der lokalen vertikalen Richtung des Objekts und der Schwerkraft zu, wenn sich das Objekt auf einer Schrägen befindet.
