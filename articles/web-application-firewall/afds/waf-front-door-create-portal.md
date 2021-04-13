---
title: 'Tutorial: Erstellen von WAF-Richtlinien für Azure Front Door – Azure-Portal'
description: In diesem Tutorial erfahren Sie, wie Sie im Azure-Portal eine Web Application Firewall-Richtlinie (WAF) erstellen.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 03/31/2021
ms.author: victorh
ms.openlocfilehash: e7b4544530dc9c0c894ae7a0f2b1d2830f895928
ms.sourcegitcommit: 9f4510cb67e566d8dad9a7908fd8b58ade9da3b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122255"
---
# <a name="tutorial-create-a-web-application-firewall-policy-on-azure-front-door-using-the-azure-portal"></a>Tutorial: Erstellen einer Web Application Firewall-Richtlinie auf Azure Front Door im Azure-Portal

Dieses Tutorial zeigt Ihnen, wie Sie eine einfache Azure Web Application Firewall-Richtlinie (WAF) erstellen und auf einen Front-End-Host in Azure Front Door anwenden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer WAF-Richtlinie
> * Deren Zuordnung zu einem Front-End-Host
> * Konfigurieren von WAF-Regeln

## <a name="prerequisites"></a>Voraussetzungen

Erstellen eines Profils für [Front Door](../../frontdoor/quickstart-create-front-door.md) oder [Front Door Standard/Premium](../../frontdoor/standard-premium/create-front-door-portal.md) 

## <a name="create-a-web-application-firewall-policy"></a>Erstellen einer Web Application Firewall-Richtlinie

Erstellen Sie zuerst im Portal mithilfe des verwalteten Standardregelsatzes eine einfache WAF-Richtlinie. 

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** aus. Suchen Sie nach **WAF**, und wählen Sie **Web Application Firewall (WAF)** und dann **Erstellen** aus.

1. Geben Sie auf der Seite **WAF-Richtlinie erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus. Übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Überprüfen + erstellen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Richtlinie für              | Wählen Sie **Globale WAF (Front Door)** aus. |
    | Front Door-SKU          | Wählen Sie zwischen den SKUs „Basic“, „Standard“ und „Premium“. |
    | Subscription            | Wählen Sie den Namen Ihres Front Door-Abonnements aus.|
    | Resource group          | Wählen Sie den Namen Ihrer Front Door-Ressourcengruppe aus.|
    | Richtlinienname             | Geben Sie einen eindeutigen Namen für Ihre WAF-Richtlinie ein.|
    | Richtlinienstatus            | Legen Sie **Aktiviert** fest. | 

   :::image type="content" source="../media/waf-front-door-create-portal/basic.png" alt-text="Screenshot: Seite „WAF-Richtlinie erstellen“ mit der Schaltfläche „Überprüfen + erstellen“ und Listenfeldern für Abonnement, Ressourcengruppe und Richtlinienname":::

1. Wählen Sie auf der Seite **WAF-Richtlinie erstellen** auf der Registerkarte **Zuordnung** die Option **+ Ein Front Door-Profil zuordnen** aus. Geben Sie die folgenden Einstellungen ein, und wählen Sie dann **Hinzufügen** aus:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Front Door-Profil              | Wählen Sie Ihren Front Door-Profilnamen aus. |
    | Domänen          | Wählen Sie die Domänen aus, denen Sie die WAF-Richtlinie zuordnen möchten, und wählen Sie dann **Hinzufügen** aus. |

    :::image type="content" source="../media/waf-front-door-create-portal/associate-profile.png" alt-text="Screenshot: Seite zum Zuordnen eines Front Door-Profils":::
    
    > [!NOTE]
    > Wenn die Domäne einer WAF-Richtlinie zugeordnet ist, wird sie abgeblendet angezeigt. Sie müssen zuerst die Domäne aus der zugehörigen Richtlinie entfernen und sie dann wieder einer neuen WAF-Richtlinie zuordnen.

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

## <a name="configure-web-application-firewall-rules-optional"></a>Konfigurieren von Web Application Firewall-Regeln (optional)

### <a name="change-mode"></a>Ändern des Modus

Wenn Sie eine WAF-Richtlinie erstellen, wird diese standardmäßig im Modus **Erkennung** betrieben. Im Modus **Erkennung** blockiert WAF keine Anforderungen. Stattdessen werden Anforderungen, die den WAF-Regeln entsprechen, in WAF-Protokollen aufgezeichnet.
Um WAF in Aktion zu sehen, können Sie den Modus von **Erkennung** in **Prävention** ändern. Im Modus **Prävention** werden Anforderungen, die mit Regeln übereinstimmen, die im Standardregelsatz definiert sind, blockiert und in WAF-Protokollen aufgezeichnet.

 :::image type="content" source="../media/waf-front-door-create-portal/policy.png" alt-text="Screenshot: Abschnitt „Richtlinieneinstellungen“. Die Umschaltfläche für den Modus ist auf „Prävention“ festgelegt.":::

### <a name="custom-rules"></a>Benutzerdefinierte Regeln

Sie können eine benutzerdefinierte Regel erstellen, indem Sie im Abschnitt **Benutzerdefinierte Regeln** die Option **Benutzerdefinierte Regel hinzufügen** wählen. Die Seite für die Konfiguration einer benutzerdefinierten Regel wird geöffnet. 

:::image type="content" source="../media/waf-front-door-create-portal/custom-rules.png" alt-text="Screenshot: Seite für benutzerdefinierte Regeln":::

Unten ist ein Beispiel für die Konfiguration einer benutzerdefinierten Regel zum Blockieren einer Anforderung für den Fall angegeben, in dem die Abfragezeichenfolge **blockme** enthält.

:::image type="content" source="../media/waf-front-door-create-portal/customquerystring2.png" alt-text="Screenshot: Seite für die Konfiguration einer benutzerdefinierten Regel mit Einstellungen für eine Regel zum Überprüfen, ob die Variable „QueryString“ den Wert „blockme“ enthält":::

### <a name="default-rule-set-drs"></a>Standardregelsatz

Der von Azure verwaltete Standardregelsatz ist standardmäßig aktiviert. Die aktuelle Standardversion ist DefaultRuleSet_1.0. Unter WAF **verwaltete Regeln**, **Zuweisung** ist der neueste verfügbare Regelsatz Microsoft_DefaultRuleSet_1 1 in der Dropdown-Liste verfügbar.

Um eine einzelne Regel zu deaktivieren, aktivieren Sie das **Kontrollkästchen** vor der Regelnummer, und wählen Sie oben auf der Seite **Deaktivieren** aus. Um Aktionstypen für einzelne Regeln zu ändern, aktivieren Sie das Kontrollkästchen vor der Regelnummer, und wählen Sie dann oben auf der Seite **Aktion ändern** aus.

:::image type="content" source="../media/waf-front-door-create-portal/managed-rules.png" alt-text="Screenshot: Seite „Verwaltete Regeln“ mit einem Regelsatz, Regelgruppen, Regeln und Schaltflächen für das Aktivieren, Deaktivieren und Ändern von Aktionen" lightbox="../media/waf-front-door-create-portal/managed-rules-expanded.png":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Entfernen Sie die Ressourcengruppe und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zu Azure Front Door](../../frontdoor/front-door-overview.md)
> [Weitere Informationen zu Azure Front Door Standard/Premium](../../frontdoor/standard-premium/overview.md)
