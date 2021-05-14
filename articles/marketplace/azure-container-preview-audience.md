---
title: Bestimmen Sie die Vorschauzielgruppe für ein Azure Container-Angebot in Microsoft AppSource.
description: Bestimmen Sie die Vorschauzielgruppe für ein Azure Container-Angebot in Microsoft AppSource.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: keferna
ms.author: keferna
ms.date: 03/30/2021
ms.openlocfilehash: 0d8bd02e247a3db0429f227e396abbfbc7cdfd91
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107892379"
---
# <a name="set-the-preview-audience-for-an-azure-container-offer"></a>Bestimmen Sie die Vorschauzielgruppe für ein Azure Container-Angebot

Dieser Artikel beschreibt, wie Sie mit dem Partner-Center eine Vorschauzielgruppe für ein Azure Container-Angebot im kommerziellen Marktplatz einrichten. Die Vorschauzielgruppe kann Ihr Angebot überprüfen, bevor es online geschaltet wird.

## <a name="define-a-preview-audience"></a>Definieren einer Vorschauzielgruppe

Auf der Seite **Vorschauzielgruppe** erstellen Sie eine begrenzte Zielgruppe, die Ihr Container-Angebot prüfen kann, bevor Sie es live für die breitere Zielgruppe des Marktplatzes veröffentlichen. Sie legen Ihre Vorschauzielgruppe mithilfe von Azure-Abonnement-IDs fest, begleitet jeweils von einer optionalen Beschreibung. Keines dieser Felder ist für Kunden sichtbar. Sie finden Ihre Azure-Abonnement-ID auf der Seite **Abonnements** im Azure-Portal.

Fügen Sie mindestens eine Azure-Abonnement-ID hinzu, entweder einzeln (bis zu 10) oder mittels Hochladen einer CSV-Datei (bis zu 100), um zu definieren, wer eine Vorschau Ihres Angebots anzeigen kann. Wenn Ihr Angebot bereits live geschaltet ist, können Sie dennoch eine Vorschauzielgruppe für das Testen von Aktualisierungen Ihres Angebots definieren.

## <a name="add-email-addresses-manually"></a>Manuelles Hinzufügen von E-Mail-Adressen

1. Fügen Sie auf der Seite **Vorschauzielgruppe** eine einzelne Azure-Abonnement-ID und optional eine Beschreibung in den entsprechenden Feldern hinzu.
1. Um eine weitere E-Mail-Adresse hinzuzufügen, wählen Sie den Link **ID hinzufügen (max. 10)** aus.
1. Wählen Sie **Entwurf speichern**, bevor Sie mit der nächsten Registerkarte fortfahren, um Pläne zu erstellen.

## <a name="add-email-addresses-using-a-csv-file"></a>Hinzufügen von E-Mail-Adressen über eine CSV-Datei

1. Wählen Sie auf der Seite **Vorschauzielgruppe** den Link **Zielgruppe exportieren (CSV)** aus.
1. Öffnen Sie die CSV-Datei. Geben Sie in der Spalte **Id** die Azure-Abonnement-IDs ein, die Sie der Vorschauzielgruppe hinzufügen möchten.
1. In der Spalte **Description** können Sie optional eine Beschreibung für jeden Eintrag hinzufügen.
1. Fügen Sie in der Spalte **Type** jeder Zeile mit einer ID die Zeichenfolge **SubscriptionID** hinzu.
1. Speichern Sie die Datei als CSV-Datei.
1. Wählen Sie auf der Seite **Vorschauzielgruppe Audience** den Link **Zielgruppe importieren (CSV)** aus.
1. Wählen Sie im Dialogfeld **Bestätigen** die Option **Ja** aus, und laden Sie dann die CSV-Datei hoch.
1. Wählen Sie **Entwurf speichern**, bevor Sie mit der nächsten Registerkarte fortfahren, um Pläne zu erstellen.

> [!IMPORTANT]
> Nachdem Sie Ihr Angebot in der Vorschau angezeigt haben, müssen Sie **Live schalten** auswählen, um Ihr Angebot für die Öffentlichkeit zu veröffentlichen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen und verwalten Sie Pläne](azure-container-plan-overview.md)
