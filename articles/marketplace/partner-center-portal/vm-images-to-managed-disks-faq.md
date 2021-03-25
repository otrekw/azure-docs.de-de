---
title: VM-Images im Azure Marketplace werden in verwalteten Datenträgerspeicher verschoben
description: Um einen schnelleren und zuverlässigeren Speicher sowie Unterstützung für neue Marketplace-Features und -Funktionen bereitzustellen, werden Marketplace-VM-Images in verwalteten Datenträgerspeicher verschoben.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/22/2019
author: kriti-ms
ms.author: krsh
ms.openlocfilehash: e3d20b5e9206cf05d749f75982a07937a8caf078
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102201089"
---
# <a name="were-moving-virtual-machine-vm-images-on-azure-marketplace-to-managed-disk-storage"></a>VM-Images im Azure Marketplace werden in verwalteten Datenträgerspeicher verschoben

Um einen schnelleren und zuverlässigeren Speicher sowie Unterstützung für neue Marketplace-Features und -Funktionen bereitzustellen, werden Marketplace-VM-Images in verwalteten Datenträgerspeicher verschoben.

Ab dem 2. Januar 2020 werden VM-Images phasenweise in verwalteten Datenträgerspeicher verschoben. In der ersten Phase werden nur Images verschoben, für die in den letzten 90 Tagen keine neuen Bereitstellungen erfolgt sind und auf denen keine virtuellen Computer ausgeführt wurden. Bevor Images verschoben werden, senden wir E-Mails an den Herausgeber, um diesen zu informieren, welche Images wann verschoben werden.

Herausgeber oder Consumer müssen keine Maßnahmen ergreifen, und die Benutzer sind nicht betroffen. Die Marketplace-Angebote bleiben verfügbar, und Kunden können verwaltete VMs aus diesen Images während und nach der Verschiebung weiterhin bereitstellen.

Wenn Sie Fragen haben, [kontaktieren Sie uns](https://support.microsoft.com/supportforbusiness/productselection?sapId=48734891-ee9a-5d77-bf29-82bf8d8111ff).

## <a name="faqs"></a>Häufig gestellte Fragen

### <a name="would-the-users-of-my-vm-images-experience-an-outage"></a>Treten für die Benutzer meiner VM-Images Ausfälle auf?

Für die Benutzer meiner VM-Images treten keine Ausfälle auf. 

### <a name="how-long-does-it-take-for-the-process-to-complete"></a>Wie lange dauert es, bis der Prozess abgeschlossen ist?

Es kann bis zu 24 Stunden dauern, bis die Migration abgeschlossen ist.

### <a name="do-i-need-to-take-any-action"></a>Muss ich Maßnahmen ergreifen?

Nein. Herausgeber oder Consumer brauchen nichts zu tun.

### <a name="do-i-have-to-update-my-system-to-call-the-cloud-portal-apis-in-a-different-way-after-they-are-moved-to-managed-disk-storage"></a>Muss ich mein System aktualisieren, um die APIs des Cloudportals auf andere Weise aufzurufen, nachdem sie in den verwalteten Datenträgerspeicher verschoben wurden?

Nein. Ihre vorhandenen API-Aufrufe funktionieren weiterhin.

### <a name="would-all-my-vm-images-be-moved-to-managed-disk-at-the-same-time"></a>Werden alle meine VM-Images gleichzeitig in den verwalteten Datenträger verschoben?

Alle Ihre VM-Images werden am gleichen Tag verschoben. Sie werden benachrichtigt, sobald die Verschiebung erfolgt ist.

### <a name="can-i-request-to-schedule-the-move-of-my-vm-images-to-a-later-time"></a>Kann ich anfordern, die Verschiebung meiner VM-Images für einen späteren Zeitpunkt zu planen?

Es wird empfohlen, die Images am geplanten Datum verschieben zu lassen. Wenn Sie jedoch Bedenken haben, wenden Sie sich an uns, um die Verschiebung umzuplanen.

### <a name="can-i-publish-updates-to-my-vm-images-during-the-move"></a>Kann ich während der Verschiebung Aktualisierungen für meine VM-Images veröffentlichen?

Aktualisierungen für die VM-Images können während der Verschiebung nicht durchgeführt werden.

### <a name="will-the-publishing-process-change-after-my-vm-image-is-moved-to-managed-disk"></a>Wird der Veröffentlichungsvorgang geändert, nachdem mein VM-Image auf den verwalteten Datenträger verschoben wurde?

Nein, der Veröffentlichungsvorgang bleibt unverändert. 

### <a name="can-the-publisher-move-their-offers-to-managed-disk"></a>Kann der Herausgeber seine Angebote auf verwaltete Datenträger verschieben?

Nein, die Herausgeber können ihre Angebote nicht auf verwaltete Datenträger verschieben. Sie müssen warten und ihre Images werden automatisch verschoben. Wir werden den Herausgeber benachrichtigen, bevor wir Änderungen vornehmen.
