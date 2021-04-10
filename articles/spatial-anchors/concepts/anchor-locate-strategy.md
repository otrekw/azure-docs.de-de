---
title: Strategie zum Suchen von Ankern
description: Informationen zu den verschiedenen Strategien beim Aufrufen der Such-API
author: pamistel
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: pamistel
ms.date: 02/11/2021
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 13aa12be5a336363bbe3bcbf3e3fb354a8fa3074
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105048475"
---
# <a name="understanding-the-anchorlocatecriteria-class"></a>Grundlegende Informationen zur AnchorLocateCriteria-Klasse
In diesem Artikel erfahren Sie mehr über die verschiedenen Optionen, die Sie beim Abfragen eines Ankers verwenden können. Dabei werden die AnchorLocateCriteria-Klasse, die entsprechenden Optionen sowie gültige Optionskombinationen behandelt.

## <a name="anchor-locate-criteria"></a>Suchkriterien für Anker
Die [AnchorLocateCriteria-Klasse](/dotnet/api/microsoft.azure.spatialanchors.anchorlocatecriteria) unterstützt Sie beim Abfragen des Diensts nach zuvor erstellten Ankern. Ein AnchorLocateCriteria-Objekt kann jederzeit über einen Watcher verwendet werden. Jedes AnchorLocateCriteria-Objekt muss **genau eine** der folgenden Eigenschaften enthalten: [Identifiers](#identifiers), [NearAnchor](#nearanchor) oder [NearDevice](#neardevice). Bei Bedarf können auch zusätzliche Eigenschaften wie [Strategy](#strategy), [BypassCache](#bypasscache) und [RequestedCategories](#requestedcategories) festgelegt werden. 

### <a name="properties"></a>Eigenschaften
Definieren Sie **genau eine** der folgenden Eigenschaften in Ihrem Watcher:
#### <a name="identifiers"></a>Bezeichner
*Standardwert: Leeres Zeichenfolgenarray*

Mithilfe von „Identifiers“ können Sie eine Liste von Anker-IDs für Anker definieren, die Sie suchen möchten. Anker-IDs werden anfänglich nach der erfolgreichen Ankererstellung zurückgegeben. Wenn „Identifiers“ festgelegt ist, schränkt „AnchorLocateCriteria“ die angeforderten Anker auf die Anker mit übereinstimmenden Anker-IDs ein. Diese Eigenschaft wird mithilfe eines Zeichenfolgenarrays angegeben. 

#### <a name="nearanchor"></a>NearAnchor
*Standardwert: Nicht festgelegt*

Mithilfe von „NearAnchor“ können Sie festlegen, dass „AnchorLocateCriteria“ die angeforderten Anker auf die Anker beschränkt, die sich innerhalb einer gewünschten Entfernung von einem ausgewählten Anker befinden. Sie müssen diesen ausgewählten Anker als Quellanker bereitstellen. Sie können auch die gewünschte Entfernung vom Quellanker und die maximale Anzahl der zurückgegebenen Anker festlegen, um die Suche weiter einzuschränken.
Diese Eigenschaft wird mithilfe eines NearAnchorCriteria-Objekts angegeben.

#### <a name="neardevice"></a>NearDevice
*Standardwert: Nicht festgelegt*

Mithilfe von „NearDevice“ können Sie angeben, dass „AnchorLocateCriteria“ die angeforderten Anker auf die Anker beschränkt, die sich in der Nähe des physischen Standorts des Geräts befinden. Alle aktivierten Sensoren werden verwendet, um Anker in der Nähe Ihres Geräts zu ermitteln. Sie sollten „SensorCapabilities“ so konfigurieren, dass die Sitzung auf alle geeigneten Sensoren zugreifen kann, um die besten Bedingungen für die Suche nach Ankern zu schaffen. Weitere Informationen zum Einrichten und Verwenden dieser Eigenschaft in [C#](../how-tos/set-up-coarse-reloc-unity.md), [Objective-C](../how-tos/set-up-coarse-reloc-unity.md), [Swift](../how-tos/set-up-coarse-reloc-swift.md), [Java](../how-tos/set-up-coarse-reloc-java.md), [C++/NDK](../how-tos/set-up-coarse-reloc-cpp-ndk.md) und [C++/WinRT](../how-tos/set-up-coarse-reloc-cpp-winrt.md) finden Sie unter [Ungefähre Standortbestimmung](./coarse-reloc.md) und *Erstellen von und Suchen nach Ankern mithilfe der ungefähren Standortbestimmung*.
Diese Eigenschaft wird mithilfe eines NearDeviceCriteria-Objekts angegeben.

### <a name="additional-properties"></a>Zusätzliche Eigenschaften
#### <a name="bypasscache"></a>BypassCache
*Standardwert: „false“*

Wenn ein Anker erstellt oder in einer Sitzung gefunden wurde, wird er ebenfalls im Cache gespeichert.  Wenn diese Eigenschaft auf „false“ festgelegt ist, wird bei jeder nachfolgenden Abfrage in derselben Sitzung der zwischengespeicherte Wert zurückgegeben. Es erfolgt keine Anforderung an den ASA-Dienst.

#### <a name="requestedcategories"></a>RequestedCategories
*Standardwert: Properties | Spatial*

Diese Eigenschaft wird verwendet, um zu bestimmen, welche Daten von einer Abfrage mithilfe von „AnchorLocateCriteria“ zurückgegeben werden. Der Standardwert gibt sowohl Eigenschaften als auch räumliche Daten zurück. Dieser Wert sollte nicht geändert werden, wenn sowohl Eigenschaften als auch räumliche Daten zurückgegeben werden sollen. Diese Eigenschaft kann mithilfe der AnchorDataCategory-Enumeration angegeben werden.

AnchorDataCategory-Enumerationswert | Zurückgegebene Daten
-----|------------
Keiner | Es werden keine Daten zurückgegeben.
Eigenschaften| Es werden Ankereigenschaften einschließlich „AppProperties“ zurückgegeben.
Spatial| Es werden räumliche Informationen zu einem Anker zurückgegeben.

#### <a name="strategy"></a>Strategie
*Standardwert: „AnyStrategy“*

„Strategy“ definiert, wie nach Ankern gesucht werden soll. Die Eigenschaft „Strategy“ kann mithilfe einer LocateStrategy-Enumeration angegeben werden.

LocateStrategy-Enumerationswert | Beschreibung
---------------|------------
AnyStrategy | Diese Strategie ermöglicht dem System die Verwendung von Kombinationen der VisualInformation- und Relationship-Strategien für die Suche nach Ankern. 
VisualInformation|Bei dieser Strategie wird versucht, nach Ankern zu suchen, indem visuelle Informationen aus der aktuellen Umgebung mit den Informationen zum visuellen Speicherbedarf des Ankers abgeglichen werden. Der visuelle Speicherbedarf eines Ankers bezieht sich auf die visuellen Informationen, die dem Anker derzeit zugeordnet sind. Diese visuellen Informationen werden normalerweise bei der Ankererstellung gesammelt, allerdings nicht ausschließlich bei diesem Vorgang. Diese Strategie ist zurzeit nur in Verbindung mit den Eigenschaften „NearDevice“ und „Identifiers“ zulässig.
Relationship|Bei dieser Strategie werden Anker gesucht, indem vorhandene [verbundene Anker](./anchor-relationships-way-finding.md#connect-anchors) verwendet werden. Diese Strategie ist zurzeit nur in Verbindung mit den Eigenschaften „NearAnchor“ und „Identifiers“ zulässig. Bei Verwendung mit der Identifiers-Eigenschaft ist es erforderlich, dass der Benutzer in derselben Sitzung bereits einen oder mehrere Anker mit bereits eingerichteten Verbindungsbeziehungen zu den Ankern gefunden hat, deren IDs im Identifiers-Array angegeben sind. 


### <a name="valid-combinations-of-locatestrategy-and-anchorlocatecriteria-properties"></a>Gültige Kombinationen der Eigenschaften „LocateStrategy“ und „AnchorLocateCriteria“ 

Nicht alle Kombinationen der Eigenschaften „Strategy“ und „AnchorLocateCriteria“ werden derzeit vom System zugelassen. In der folgenden Tabelle sind die zulässigen Kombinationen aufgeführt:



Eigenschaft | AnyStrategy | Relationship | VisualInformation
-------- | ------------|--------------|-------------------
Bezeichner | &check;    | &check;     | &check;
NearAnchor  | &check; (wird standardmäßig auf „Relationship“ festgelegt) | &check;    | 
NearDevice  | &check;    |   | &check;




## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele zur Verwendung der AnchorLocateCriteria-Klasse finden Sie unter [Erstellen von und Suchen nach Ankern mit Azure Spatial Anchors](../create-locate-anchors-overview.md).