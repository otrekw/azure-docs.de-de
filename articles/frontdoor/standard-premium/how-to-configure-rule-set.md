---
title: 'Azure Front Door: Konfigurieren eines Front Door-Regelsatzes'
description: Dieser Artikel bietet eine Anleitung zum Konfigurieren eines Regelsatzes.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: 6863c492059ccee152ecf3d03a09e61793576bcb
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101715597"
---
# <a name="configure-a-rule-set-with-azure-front-door-standardpremium-preview"></a>Konfigurieren eines Regelsatzes mit Azure Front Door Standard/Premium (Vorschau)

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

In diesem Artikel wird erläutert, wie Sie einen Regelsatz sowie die ersten Regeln im Azure-Portal erstellen. Sie erfahren, wie Sie den Regelsatz über die Regelsatzseite oder den Endpoint Manager einer Route zuordnen.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

* Bevor Sie einen Regelsatz konfigurieren können, müssen Sie zuerst eine Azure Front Door-Instanz im Standard- oder Premium-Tarif erstellen. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Azure Front Door Standard/Premium-Profils](create-front-door-portal.md).

## <a name="configure-rule-set-in-azure-portal"></a>Konfigurieren eines Regelsatzes im Azure-Portal

1. Wählen Sie in Ihrem Front Door-Profil unter **Einstellungen** die Option **Regelsatz** aus. Wählen Sie **Hinzufügen** aus, und benennen Sie den Regelsatz.

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-create-rule-set-1.png" alt-text="Screenshot: Regelsatz-Landing Page":::
    
1. Wählen Sie **Regel hinzufügen** aus, um die erste Regel zu erstellen. Vergeben Sie einen Regelnamen. Klicken Sie dann auf **Bedingung hinzufügen** oder **Aktion hinzufügen**, um die Regel zu definieren. Sie können einer einzelnen Regel bis zu zehn Bedingungen und fünf Aktionen hinzufügen. In diesem Beispiel verwenden wir „servervariable“, um den Antwortheader „*Geo-country*“ für Anforderungen hinzuzufügen, deren URL *contoso* enthält.

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-create-rule-set.png" alt-text="Screenshot: Regelsatz-Konfigurationsseite":::
    
    > [!NOTE]
    > * Zum Löschen einer Bedingung oder Aktion aus einer Regel verwenden Sie den Papierkorb auf der rechten Seite der jeweiligen Bedingung oder Aktion.
    > * Geben Sie keine Bedingungen an, um eine Regel zu erstellen, die für den gesamten eingehenden Datenverkehr gilt.
    > * Wenn nach Erfüllung einer bestimmten Regel die verbleibenden Regeln nicht mehr ausgewertet werden sollen, aktivieren Sie **Auswertung der verbleibenden Regeln beenden**. Wenn diese Option aktiviert ist, werden alle noch verbleibenden Regeln im Regelsatz nicht ausgeführt, unabhängig davon, ob die entsprechenden Bedingungen erfüllt waren.  

1. Sie können die Priorität der Regeln in Ihrem Regelsatz festlegen, indem Sie die Regeln mithilfe der Pfeilschaltflächen nach oben oder unten verschieben. Die Liste ist in aufsteigender Reihenfolge angeordnet, die wichtigste Regel wird also zuerst aufgeführt.

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-rule-set-change-orders.png" alt-text="Screenshot: Regelsatzpriorität" lightbox="../media/how-to-configure-rule-set/front-door-rule-set-change-orders-expanded.png":::

1. Nachdem Sie eine oder mehrere Regeln erstellt haben, klicken Sie auf **Speichern**, um die Erstellung des Regelsatzes abzuschließen.

1. Ordnen Sie den Regelsatz jetzt einer Route zu, damit er wirksam wird. Sie können den Regelsatz über die Seite des Regelsatzes zuordnen oder zum Endpoint Manager wechseln, um die Zuordnung zu erstellen.
 
    **Seite des Regelsatzes**: 
    
    1. Wählen Sie den Regelsatz aus, den Sie zuordnen möchten.
    
    1. Klicken Sie auf den Link *Nicht zugeordnet*.
     

    1. Wählen Sie auf der Seite **Eine Route zuordnen** den Endpunkt und die Route aus, den bzw. die Sie dem Regelsatz zuordnen möchten. 
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set.png" alt-text="Screenshot: Seite zum Erstellen einer Route":::    
        
    1. Klicken Sie auf *Weiter*, um die Regelsatzreihenfolge zu ändern, falls sich in der ausgewählten Route mehrere Regelsätze befinden. Die Regelsätze werden von oben nach unten ausgeführt. Sie können die Reihenfolge ändern, indem Sie einen Regelsatz auswählen und nach oben oder unten verschieben. Wählen Sie dann *Zuordnen* aus.
    
        > [!Note]
        > Auf dieser Seite können Sie einen Regelsatz nur einer Route zuordnen. Wenn Sie einen Regelsatz mehreren Routen zuordnen möchten, verwenden Sie den Endpoint Manager.
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-2.png" alt-text="Screenshot: Regelsatzreihenfolge":::
    
    1. Der Regelsatz ist jetzt einer Route zugeordnet. Sie werden feststellen, dass dem Antwortheader der Wert von „Geo-country“ hinzugefügt wurde.
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-3.png" alt-text="Screenshot: Regel, die einer Route zugeordnet wurde":::

   **Endpoint Manager**: 
    
    1. Wechseln Sie zum Endpoint Manager, und wählen Sie den Endpunkt aus, den Sie dem Regelsatz zuordnen möchten.
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-1.png" alt-text="Screenshot: Auswählen des Endpunkts im Endpoint Manager" lightbox="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-1-expanded.png":::

    1. Klicken Sie auf *Endpunkt bearbeiten*.  
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-2.png" alt-text="Screenshot: Auswählen von „Endpunkt bearbeiten“ im Endpoint Manager" lightbox="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-2-expanded.png":::

    1. Wählen Sie die Route aus. 
    
         :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-3.png" alt-text="Screenshot: Auswählen einer Route":::
    
    1. Wählen Sie auf der Seite *Route aktualisieren* unter *Regeln* aus der Dropdownliste die Regelsätze aus, die Sie der Route zuordnen möchten. Sie können die Reihenfolge ändern, indem Sie Regelsätze nach oben oder unten verschieben. 
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-4.png" alt-text="Screenshot: Seite zum Aktualisieren einer Route":::
    
    1. Klicken Sie dann auf *Aktualisieren* oder *Hinzufügen*, um die Zuordnung abzuschließen.

## <a name="delete-a-rule-set-from-your-azure-front-door-profile"></a>Löschen eines Regelsatzes aus dem Azure Front Door-Profil

In den vorherigen Schritten haben Sie einen Regelsatz konfiguriert und einer Route zugeordnet. Wenn der Regelsatz Ihrer Front Door-Instanz nicht mehr zugeordnet sein soll, können Sie den Regelsatz mithilfe der folgenden Schritte entfernen:

1. Wechseln Sie zur **Seite des Regelsatzes**, und heben Sie unter **Einstellungen** die Zuordnung des Regelsatzes zu allen Routen auf.

1. Erweitern Sie die Route, und klicken Sie auf die drei Punkte. Wählen Sie dann *Route bearbeiten* aus.

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-disassociate-rule-set-1.png" alt-text="Screenshot: erweiterte Route im Regelsatz":::

1. Wechseln Sie auf der Seite der Route zum Abschnitt „Regeln“, und klicken Sie auf die Schaltfläche *Löschen*. 

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-disassociate-rule-set-2.png" alt-text="Screenshot: Aktualisieren der Routenseite zum Löschen eines Regelsatzes" lightbox="../media/how-to-configure-rule-set/front-door-disassociate-rule-set-2-expanded.png":::

1. Wählen Sie *Aktualisieren* aus. Die Zuordnung des Regelsatzes zur Route wird aufgehoben.

1. Wiederholen Sie die Schritte 2–5, um die Zuordnung weiterer Routen zu diesem Regelsatz aufzuheben, bis der Routenstatus *Nicht zugeordnet* anzeigt.

1. Sie können einen Regelsatz mit dem Status *Nicht zugeordnet* löschen, indem Sie auf die drei Punkte rechts klicken und *Löschen auswählen*. 

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-disassociate-rule-set-3.png" alt-text="Screenshot: Löschen eines Regelsatzes":::

1. Der Regelsatz ist jetzt gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Sicherheitsheader mit Regelsätzen](how-to-add-security-headers.md) hinzufügen.
