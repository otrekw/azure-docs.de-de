---
title: Verfügbarkeitszonen
description: Media Services unterstützt Verfügbarkeitszonen, die Fehlerisolation bereitstellen.
services: media-services
author: johndeu
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 05/27/2021
ms.author: johndeu
ms.openlocfilehash: d50af7acadb17fe060ede90ad93896e465d3bad6
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111746949"
---
# <a name="availability-zones"></a>Verfügbarkeitszonen

Azure Media Services nutzt [Verfügbarkeitszonen](../../availability-zones/az-overview.md), die fehlerisolierte Standorte innerhalb derselben Azure-Region bieten. Media Services ist an den [verfügbaren Standorten](../../availability-zones/az-region.md#azure-regions-with-availability-zones) standardmäßig zonenredundant, und es ist keine zusätzliche Konfiguration für das Konto erforderlich, um diese Funktion zu aktivieren.  Media Services speichert Mediendaten in den zugeordneten Speicherkonten.  Diese Speicherkonten sollten als zonenredundanter Speicher (ZRS) oder geozonenredundanter Speicher (GZRS) erstellt werden, um das gleiche Maß an Redundanz wie das Media Services-Konto bereitzustellen. Ausführliche Informationen zum Konfigurieren der Replikation für die zugeordneten Speicherkonten finden Sie im Artikel [Ändern der Replikation eines Speicherkontos](../../storage/common/redundancy-migration.md).

## <a name="how-media-services-components-handle-an-availability-zone-fault"></a>Behandeln eines Verfügbarkeitszonenfehlers durch Media Services-Komponenten

| Komponente             | Verhalten bei Verfügbarkeitszonenfehlern |
|-----------            |----------------------|
| Steuerungsebene (Azure-Ressourcenverwaltung) | Funktioniert weiterhin wie gewohnt. |
| Schlüsselübermittlung            | Funktioniert weiterhin wie gewohnt. |
| Aufträge                    | Aufträge werden in einer anderen Verfügbarkeitszone neu geplant. Bei der Verarbeitung kommt es zu einer Verzögerung, da aktuell verarbeitete Aufträge neu geplant und in der Verfügbarkeitszone neu gestartet werden. |
| Liveereignisse             | Streaming und Erfassung für das Liveereignis funktionieren weiterhin wie gewohnt. Das Aufrufen von „reset“ für ein Liveereignis wird während eines Verfügbarkeitszonenfehlers derzeit nicht unterstützt. Es wird empfohlen, das Liveereignis zuerst zu beenden und neu zu starten, statt einen Zurücksetzungsvorgang auszuführen. Wenn ein Liveereignis während eines Zonenausfalleignisses gerade in den Zustand „Wird ausgeführt“ versetzt wurde, wird es für Kunden unter Umständen weiterhin als Ereignis mit dem Zustand „Wird gestartet“ angezeigt, und der Zustand wird nicht mehr aktualisiert. In diesem Fall wird empfohlen, ein neues Liveereignis zu starten und die Liveereignisse im Zustand „Wird gestartet“ zu bereinigen, nachdem die Zone wiederhergestellt wurde.  |
| Streamingendpunkte     | Funktioniert weiterhin wie gewohnt. |


## <a name="high-availability-streaming-and-encoding-for-vod"></a>Hochverfügbarkeitsstreaming und -codierung für VOD

Verfügbarkeitszonen erhöhen die Fehlerisolation in einer einzelnen Region. Wenn Sie Hochverfügbarkeit für On-Demand-Streaming und -Codierung bereitstellen möchten, können Sie andere Azure-Dienste verwenden, um eine Architektur zu erstellen, die Aspekte wie Redundanz, Integritätsüberwachung, Lastenausgleich sowie Datensicherung und -wiederherstellung abdeckt. Eine solche Architektur finden Sie im Artikel [Hochverfügbarkeit bei Media Services und Video on Demand (VoD)](architecture-high-availability-encoding-concept.md).
Der Artikel und der Beispielcode bieten eine Lösung dafür, wie einzelne regionale Media Services-Konten verwendet werden können, um eine Hochverfügbarkeitsarchitektur für Ihre VOD-Anwendung zu erstellen.

## <a name="media-services-support-for-availability-zones-by-region"></a>Media Services-Unterstützung für Verfügbarkeitszonen nach Region

Verfügbarkeitszonen werden derzeit nur in bestimmten Azure-Regionen unterstützt. Weitere Informationen zur Regionsunterstützung für Verfügbarkeitszonen finden Sie unter [Azure-Regionen mit Verfügbarkeitszonen](../../availability-zones/az-region.md#azure-regions-with-availability-zones).

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Regionen und Verfügbarkeitszonen in Azure](../../availability-zones/az-overview.md).

Weitere Informationen zur Codierung und zum Streaming mit Hochverfügbarkeit finden Sie unter [Hochverfügbarkeit bei Media Services und Video on Demand (VoD)](architecture-high-availability-encoding-concept.md).

Informationen zum ordnungsgemäßen Konfigurieren der Speicherkontoreplikation zur Unterstützung von Verfügbarkeitszonen finden Sie unter [Ändern der Replikation eines Speicherkontos](../../storage/common/redundancy-migration.md).
