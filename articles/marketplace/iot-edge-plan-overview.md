---
title: Erstellen und Bearbeiten von Plänen für ein IoT Edge-Modulangebot auf Azure Marketplace
description: Erstellen und Bearbeiten von Plänen für ein IoT Edge-Modulangebot auf Azure Marketplace
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: aarathin
ms.author: aarathin
ms.date: 05/21/2021
ms.openlocfilehash: 65284852a0e52c79339e31a54b829d63bb78adbf
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111955635"
---
# <a name="create-and-edit-plans-for-an-iot-edge-module-offer"></a>Erstellen und Bearbeiten von Plänen für ein IoT Edge-Modulangebot

Auf der Seite „Planübersicht“ können Sie verschiedene Planoptionen innerhalb desselben Angebots erstellen. Pläne (früher als SKUs bezeichnet) können sich in Bezug darauf unterscheiden, wo sie verfügbar sind („Azure Global“ oder „Azure Government“) und in Bezug auf das Image, auf das der Plan verweist. Ihr Angebot muss mindestens einen Plan enthalten.

Sie können bis zu 100 Pläne für jedes Angebot erstellen. Bis zu 45 davon können privat sein. Erfahren Sie unter [Private Angebote im kommerziellen Microsoft-Marketplace](private-offers.md) mehr über private Pläne.

Nachdem Sie einen Plan erstellt haben, wird auf der Seite **Planübersicht** Folgendes angezeigt:

- Plannamen
- Preismodell
- Azure-Regionen (global oder Government)
- Aktueller Veröffentlichungsstatus
- Eventuell verfügbare Aktionen

Die für einen Plan verfügbaren Aktionen unterscheiden sich abhängig vom aktuellen Status Ihres Plans. Dazu gehören:

- **Entwurf löschen**: Wenn es sich beim Plan um einen Entwurf handelt.
- **Planverkauf beenden**: Wenn der Plan live veröffentlicht wurde.

## <a name="edit-a-plan"></a>Bearbeiten eines Plans

Wählen Sie den **Namen** eines Plans aus, um die zugehörigen Details zu bearbeiten.

## <a name="create-a-plan"></a>Erstellen von Plänen

Wählen Sie **+ Neuen Plan erstellen** aus, um einen neuen Plan einzurichten.

Geben Sie für jeden Plan eine eindeutige **Plan-ID** ein. Diese ID ist für Kunden unter der Webadresse des Produkts sichtbar. Verwenden Sie nur Kleinbuchstaben und Zahlen, Bindestriche oder Unterstriche bei maximal 50 Zeichen. Die Plan-ID kann nicht mehr geändert werden, nachdem Sie **Erstellen** ausgewählt haben.

Geben Sie einen **Plannamen** ein. Kunden sehen diesen Namen, wenn sie sich für einen der Pläne in Ihrem Angebot interessieren. Jeder Plan in diesem Angebot muss einen eindeutigen Namen aufweisen. Beispielsweise können Sie den Angebotsnamen **Windows Server** für die Pläne **Windows Server 2016** und **Windows Server 2019** verwenden.

Klicken Sie auf **Erstellen**, und fahren Sie unten fort.

## <a name="next-steps"></a>Nächste Schritte

- Klicken Sie auf [+ Neuen Plan erstellen](iot-edge-plan-setup.md), oder
- Beenden Sie die Planeinrichtung, und fahren Sie optional mit [Co-Selling mit Microsoft](./co-sell-overview.md) fort, oder
- [Überprüfen und veröffentlichen Sie Ihr Angebot](review-publish-offer.md)