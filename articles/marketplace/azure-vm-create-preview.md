---
title: Hinzufügen einer Vorschauzielgruppe für ein VM-Angebot im Azure Marketplace
description: Hinzufügen einer Vorschauzielgruppe für ein VM-Angebot im Azure Marketplace
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 10/19/2020
ms.openlocfilehash: 5ca53a950aeaaf888dead6c1e134f63fe7cea04c
ms.sourcegitcommit: 70ce9237435df04b03dd0f739f23d34930059fef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2021
ms.locfileid: "111524956"
---
# <a name="add-a-preview-audience-for-a-virtual-machine-offer"></a>Hinzufügen einer Vorschauzielgruppe für ein VM-Angebot

Wählen Sie auf der Seite **Vorschau** (Auswahl in Partner Center im linken Navigationsmenü) eine eingeschränkte **Vorschauzielgruppe** zur Überprüfung Ihres Angebots aus, bevor Sie es live für die breitere Zielgruppe des kommerziellen Marketplace veröffentlichen.

> [!IMPORTANT]
> Nachdem Sie Ihr Angebot im Bereich **Vorschau** überprüft haben, wählen Sie **Live schalten** aus, um es für die öffentliche Zielgruppe des kommerziellen Marketplace zu veröffentlichen.

Ihre Vorschauzielgruppe wird durch **Azure-Abonnement-ID**-GUIDs zusammen mit einer optionalen **Beschreibung** für jede Gruppe identifiziert. Keines dieser Felder ist für Kunden sichtbar. Sie finden Ihre Azure-Abonnement-ID auf der Seite **Abonnement** im Azure-Portal.

Fügen Sie mindestens eine Azure-Abonnement-ID hinzu, entweder einzeln (bis zu 10) oder durch Hochladen einer CSV-Datei (bis zu 100). Durch Hinzufügen dieser Abonnement-IDs definieren Sie, wer Ihr Angebot als Vorschau anzeigen kann, bevor es live veröffentlicht wird. Wenn Ihr Angebot bereits live geschaltet ist, können Sie trotzdem eine Vorschauzielgruppe für das Testen von Änderungen oder Aktualisierungen Ihres Angebots definieren.

> [!NOTE]
> Eine Vorschauzielgruppe unterscheidet sich von einer privaten Zielgruppe. Eine Vorschauzielgruppe kann auf das Angebot zugreifen, *bevor* es im Azure Marketplace live veröffentlicht wird. Die Vorschauzielgruppe kann alle Pläne sehen und überprüfen, einschließlich der Pläne, die nach der vollständigen Veröffentlichung Ihres Angebots im Azure Marketplace nur für eine private Zielgruppe verfügbar sind. Eine private Zielgruppe (definiert jeweils im Bereich **Preise und Verfügbarkeit**) hat exklusiven Zugriff auf einen bestimmten Plan.

Wenn Sie Änderungen vorgenommen haben, wählen Sie **Entwurf speichern** aus, bevor Sie mit der nächsten Registerkarte im linken Navigationsmenü (**Planübersicht**) fortfahren.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen von Plänen](azure-vm-create-plans.md)
