---
title: 'Tutorial: Erstellen von WAF-Richtlinien für Azure CDN – Azure-Portal'
description: In diesem Tutorial erfahren Sie, wie Sie im Azure-Portal eine Web Application Firewall-Richtlinie (WAF) für Azure CDN erstellen.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 03/18/2020
ms.author: victorh
ms.openlocfilehash: 738be1361bfbd944575abceb08781b241336f6e8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "79472542"
---
# <a name="tutorial-create-a-waf-policy-on-azure-cdn-using-the-azure-portal"></a>Tutorial: Erstellen einer WAF-Richtlinie für Azure CDN über das Azure-Portal

In diesem Tutorial wird gezeigt, wie Sie eine einfache Azure Web Application Firewall-Richtlinie (WAF) erstellen und auf einen Endpunkt in Azure Content Delivery Network (CDN) anwenden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer WAF-Richtlinie
> * Zuordnen zu einem CDN-Endpunkt
> * Konfigurieren von WAF-Regeln

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie anhand der Anweisungen im folgenden Artikel ein Azure CDN-Profil und einen Endpunkt: [Schnellstart: Erstellen eines Azure CDN-Profils und -Endpunkts](../../cdn/cdn-create-new-endpoint.md). 

## <a name="create-a-web-application-firewall-policy"></a>Erstellen einer Web Application Firewall-Richtlinie

Erstellen Sie zuerst im Portal mithilfe des verwalteten Standardregelsatzes eine einfache WAF-Richtlinie.

1. Wählen Sie links auf dem Bildschirm **Ressource erstellen** aus. Suchen Sie nach **WAF**, und wählen Sie **Web Application Firewall** und dann **Erstellen** aus.
2. Geben Sie auf der Seite **WAF-Richtlinie erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus. Übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Überprüfen + erstellen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Richtlinie für            |Wählen Sie Azure CDN (Vorschau) aus.|
    | Subscription            |Wählen Sie den Namen Ihres Front Door-Abonnements aus.|
    | Resource group          |Wählen Sie den Namen Ihrer Front Door-Ressourcengruppe aus.|
    | Richtlinienname             |Geben Sie einen eindeutigen Namen für Ihre WAF-Richtlinie ein.|

   ![Erstellen einer WAF-Richtlinie](../media/waf-cdn-create-portal/basic.png)

3. Wählen Sie auf der Seite **WAF-Richtlinie erstellen** auf der Registerkarte **Zuordnung** die Option **CDN-Endpunkt hinzufügen** aus. Geben Sie die folgenden Einstellungen ein, und wählen Sie dann **Hinzufügen** aus:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | CDN-Profil              | Wählen Sie den Namen Ihres CDN-Profils aus.|
    | Endpunkt           | Wählen Sie den Namen Ihres Endpunkts und dann **Hinzufügen** aus.|
    
    > [!NOTE]
    > Ist der Endpunkt einer WAF-Richtlinie zugeordnet, wird er abgeblendet dargestellt. Sie müssen zuerst den Endpunkt aus der zugehörigen Richtlinie entfernen und ihn dann wieder einer neuen WAF-Richtlinie zuordnen.
1. Klicken Sie auf**Überprüfen + erstellen** und dann auf **Erstellen**.

## <a name="configure-web-application-firewall-policy-optional"></a>Konfigurieren einer Web Application Firewall-Richtlinie (optional)

### <a name="change-mode"></a>Ändern des Modus

Wenn Sie eine WAF-Richtlinie erstellen, befindet sich diese standardmäßig im Modus *Erkennung*. Im Modus *Erkennung* blockiert WAF keine Anforderungen. Stattdessen werden die mit WAF-Regeln übereinstimmenden Anforderungen in WAF-Protokollen aufgezeichnet.

Um WAF in Aktion zu sehen, können Sie den Modus von *Erkennung* in *Prävention* ändern. Im Modus *Prävention* werden Anforderungen, die mit Regeln übereinstimmen, die im Standardregelsatz definiert sind, blockiert und in WAF-Protokollen aufgezeichnet.

 ![Ändern des WAF-Richtlinienmodus](../media/waf-cdn-create-portal/policy.png)

### <a name="custom-rules"></a>Benutzerdefinierte Regeln

Wählen Sie zum Erstellen einer benutzerdefinierten Regel im Abschnitt **Benutzerdefinierte Regeln** die Option **Benutzerdefinierte Regel hinzufügen** aus. Die Seite für die Konfiguration einer benutzerdefinierten Regel wird geöffnet. Es gibt zwei Arten von benutzerdefinierten Regeln: **Abgleichsregeln** und **Ratenbegrenzungsregeln**.

Der folgende Screenshot zeigt eine benutzerdefinierte Abgleichsregel zum Blockieren einer Anforderung, wenn die Abfragezeichenfolge den Wert **blockme** enthält.

![Ändern des WAF-Richtlinienmodus](../media/waf-cdn-create-portal/custommatch.png)

Bei Ratenbegrenzungsregeln sind zwei zusätzliche Felder erforderlich: **Dauer der Ratenbegrenzung** und **Schwellenwert für die Ratenbegrenzung (Anforderungen)** wie im folgenden Beispiel gezeigt:

![Ändern des WAF-Richtlinienmodus](../media/waf-cdn-create-portal/customrate.png)

### <a name="default-rule-set-drs"></a>Standardregelsatz

Der von Azure verwaltete Standardregelsatz ist standardmäßig aktiviert. Um eine einzelne Regel innerhalb einer Regelgruppe zu deaktivieren, erweitern Sie die Regeln innerhalb dieser Regelgruppe. Aktivieren Sie das Kontrollkästchen vor der Regelnummer, und wählen Sie oben auf der Registerkarte **Deaktivieren** aus. Um Aktionstypen für einzelne Regeln innerhalb des Regelsatzes zu ändern, aktivieren Sie das Kontrollkästchen vor der Regelnummer, und wählen Sie dann oben die Registerkarte **Aktion ändern** aus.

 ![Ändern des WAF-Regelsatzes](../media/waf-cdn-create-portal/managed2.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über Azure Web Application Firewall.](../overview.md)
