---
title: Erstellen eines Azure-Containerangebots in Azure Marketplace
description: Erstellen eines Azure-Containerangebots in Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: keferna
ms.author: keferna
ms.date: 04/21/2021
ms.openlocfilehash: a8c10ea5bdfab20ed5a250fba656527cd7935527
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108129403"
---
# <a name="create-an-azure-container-offer"></a>Erstellen eines Azure-Containerangebots

In diesem Artikel wird beschrieben, wie Sie ein Azure Container-Angebot erstellen. Alle Angebote durchlaufen unseren Zertifizierungsprozess, der Ihre Lösung auf Standardanforderungen, Kompatibilität und korrekte Praktiken prüft.

Erstellen Sie, bevor Sie beginnen, ein Konto für den kommerziellen Marketplace in [Partner Center](./create-account.md), und stellen Sie sicher, dass es für das kommerzielle Marketplace-Programm registriert ist.

## <a name="before-you-begin"></a>Bevor Sie beginnen

Planen[eines Azure-Containerangebots](marketplace-containers.md). Dort werden die technischen Anforderungen für dieses Angebot erläutert sowie die Informationen und Assets aufgelistet, die Sie bei der Erstellung des Angebots benötigen.

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Wählen Sie im linken Navigationsmenü **Kommerzieller Marketplace** > **Übersicht** aus.
3. Wählen Sie auf der Übersichtsseite die Option **+ Neues Angebot** > **Azure-Container** aus.

    :::image type="content" source="media/azure-container/new-offer-azure-container.png" alt-text="Die Menüoptionen im linken Fensterbereich und die Schaltfläche „Neues Angebot“.":::

> [!IMPORTANT]
> Nachdem Ihr Angebot veröffentlicht wurde, werden Änderungen, die Sie in Partner Center vornehmen, im Azure Marketplace erst angezeigt, nachdem Sie das Angebot erneut veröffentlicht haben. Achten Sie darauf, ein Angebot nach einer Änderung stets neu zu veröffentlichen.

## <a name="new-offer"></a>Neues Angebot

Geben Sie eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

- Diese ID ist in der Webadresse für das Angebot und in Azure Resource Manager-Vorlagen für Kunden sichtbar, falls zutreffend.
- Verwenden Sie nur Kleinbuchstaben und Zahlen. Die ID kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt. Wenn die Herausgeber-ID z. B. `testpublisherid` lautet und Sie **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://appsource.microsoft.com/product/dynamics-365/testpublisherid.test-offer-1`.
- Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

Geben Sie einen **Angebotsalias** ein. Dies ist der Name, der für das Angebot im Partner Center verwendet wird.

- Dieser Name wird nicht im Azure Marketplace verwendet. Er unterscheidet sich vom Angebotsnamen und anderen Werten, die Kunden präsentiert werden.

Wählen Sie **Erstellen** aus, um das Angebot zu generieren. In Partner Center wird die Seite **Angebotseinrichtung** geöffnet.

## <a name="alias"></a>Alias

Geben Sie einen beschreibenden Namen ein, mit dem ausschließlich in Partner Center auf dieses Angebot verwiesen wird. Der Angebotsalias (vorausgefüllt mit dem, was Sie beim Erstellen des Angebots eingegeben haben) wird auf dem Marktplatz nicht verwendet und unterscheidet sich von dem Angebotsnamen, der den Kunden angezeigt wird. Wenn Sie den Angebotsnamen später ändern möchten, wechseln Sie zur Seite [Angebotsliste](azure-container-offer-listing.md).

## <a name="customer-leads"></a>Kundenleads

[!INCLUDE [Connect lead management](includes/customer-leads.md)]

Weitere Informationen finden Sie unter [Kundenleads aus Ihrem Marketplace-Angebot](partner-center-portal/commercial-marketplace-get-customer-leads.md).

Wählen Sie **Entwurf speichern** aus, bevor Sie mit der nächsten Registerkarte im linken Navigationsmenü (**Eigenschaften**) fortfahren.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Angebotseigenschaften](azure-container-properties.md)
- [Bewährte Methoden für Angebotslistung](gtm-offer-listing-best-practices.md)