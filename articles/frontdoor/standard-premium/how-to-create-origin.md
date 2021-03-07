---
title: Einrichten eines Ursprungs für Azure Front Door Standard/Premium (Vorschau)
description: In diesem Artikel wird gezeigt, wie Sie einen Ursprung mit Endpoint Manager konfigurieren.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: ebc71ea2d354caf0c8f31b1231ecc1487237dd29
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101741887"
---
# <a name="set-up-an-azure-front-door-standardpremium-preview-origin"></a>Einrichten eines Ursprungs für Azure Front Door Standard/Premium (Vorschau)

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

In diesem Artikel erfahren Sie, wie Sie einen Ursprung für Azure Front Door Standard/Premium in einer vorhandenen Ursprungsgruppe erstellen können. 

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie einen Ursprung für Azure Front Door Standard/Premium erstellen können, müssen Sie mindestens eine Ursprungsgruppe erstellt haben.

## <a name="create-a-new-azure-front-door-standardpremium-origin"></a>Erstellen eines Ursprungs für Azure Front Door Standard/Premium

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem Azure Front Door Standard/Premium-Profil.

1. Wählen Sie **Ursprungsgruppe** aus. Wählen Sie dann **+Hinzufügen** aus, um eine neue Ursprungsgruppe zu erstellen.
   
    :::image type="content" source="../media/how-to-create-origin/select-add-origin.png" alt-text="Screenshot der Landing Page der Ursprungsgruppe":::

1. Geben Sie auf der Seite **Ursprungsgruppe hinzufügen** einen eindeutigen **Namen** für die neue Ursprungsgruppe ein.

1. Wählen Sie dann **+Ursprung hinzufügen** aus, um dieser Ursprungsgruppe einen neuen Ursprung hinzuzufügen. 

    :::image type="content" source="../media/how-to-create-origin/add-origin-view.png" alt-text="Screenshot der Seite zum Hinzufügen des Ursprungs":::
  
    | Einstellung | Wert |
    | --- | --- |
    | Name | Geben Sie einen eindeutigen Namen für den neuen Azure Front Door-Ursprung ein. |   
    | Ursprungstyp | Der Typ der Ressource, die hinzugefügt werden soll. Azure Front Door Standard/Premium unterstützt die automatische Ermittlung des Ursprungs Ihrer App über den App-Dienst, Clouddienst oder Speicher. Wenn Sie eine andere Ressource in Azure oder ein Nicht-Azure-Back-End hinzufügen möchten, wählen Sie **Benutzerdefinierter Host** aus. |
    | Hostname  | Wenn Sie nicht **Benutzerdefinierter Host** für den Hosttyp des Ursprungs aktiviert haben, wählen Sie Ihr Back-End aus, indem Sie in der Dropdownliste den Hostnamen des Ursprungs auswählen. |
    | Header des Ursprungshosts | Geben Sie den Hostheaderwert ein, der für jede Anforderung an das Back-End gesendet wird. Weitere Informationen finden Sie unter [Header des Ursprungshosts](concept-origin.md#hostheader). |
    | HTTP-Port | Geben Sie den Wert für den Port ein, den der Ursprung für das HTTP-Protokoll unterstützt. |
    | HTTPS-Anschluss | Geben Sie den Wert für den Port ein, den der Ursprung für das HTTPS-Protokoll unterstützt. |
    | Priorität | Weisen Sie den verschiedenen Ursprüngen Prioritäten zu, wenn Sie einen primären Dienstursprung für den gesamten Datenverkehr verwenden möchten. Stellen Sie außerdem Sicherungen bereit, wenn der primäre oder Sicherungsursprung nicht verfügbar sind. Weitere Informationen finden Sie unter [Priorität](concept-origin.md#priority). |
    | Weight | Weisen Sie Ihren verschiedenen Ursprüngen Gewichtungen zu, um den Datenverkehr entweder gleichmäßig oder anhand von Gewichtungskoeffizienten auf eine Gruppe von Ursprüngen zu verteilen. Weitere Informationen finden Sie unter [Gewichtung](concept-origin.md#weighted). |
    | Status | Wählen Sie diese Option aus, um den Ursprung zu aktivieren. |
    | Regel | Wählen Sie Regelsätze aus, die auf diese Route angewendet werden. Weitere Informationen zum Konfigurieren von Regeln finden Sie unter [Konfigurieren eines Regelsatzes für Azure Front Door](how-to-configure-rule-set.md). | 

    > [!IMPORTANT]
    > Bei der Konfiguration überprüfen die APIs nicht, ob aus Front Door-Umgebungen auf den Ursprung zugegriffen werden kann. Stellen Sie daher sicher, dass Front Door Ihren Ursprung erreichen kann.

1. Wählen Sie **Hinzufügen** aus, um den neuen Ursprung zu erstellen. Der erstellte Ursprung sollte in der Ursprungsliste mit der Gruppe angezeigt werden.
  
    :::image type="content" source="../media/how-to-create-origin/origin-list-view.png" alt-text="Screenshot des Ursprungs in der Listenansicht":::

1. Wählen Sie **Hinzufügen** aus, um die Ursprungsgruppe dem aktuellen Endpunkt hinzuzufügen. Die Ursprungsgruppe sollte im Bereich „Ursprungsgruppe“ angezeigt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie eine Ursprungsgruppe löschen möchten, wenn Sie sie nicht mehr benötigen, klicken Sie auf **...** , und wählen Sie dann aus der Dropdownliste **Löschen** aus.

:::image type="content" source="../media/how-to-create-origin/delete-origin-group.png" alt-text="Screenshot zum Löschen einer Ursprungsgruppe":::

Wenn Sie einen Ursprung löschen möchten, wenn Sie ihn nicht mehr benötigen, klicken Sie auf **...** , und wählen Sie dann aus der Dropdownliste **Löschen** aus. 

:::image type="content" source="../media/how-to-create-origin/delete-origin-view.png" alt-text="Screenshot zum Löschen eines Ursprungs":::

## <a name="next-steps"></a>Nächste Schritte

Informationen zu benutzerdefinierten Domänen finden Sie unter [Hinzufügen einer benutzerdefinierten Domäne](how-to-add-custom-domain.md) zu Ihrem Azure Front Door Standard/Premium-Endpunkt.
