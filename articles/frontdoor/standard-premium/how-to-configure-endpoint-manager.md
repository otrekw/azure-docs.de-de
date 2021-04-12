---
title: Konfigurieren eines Azure Front Door Standard/Premium-Endpunkts mit Endpoint Manager
description: In diesem Artikel wird gezeigt, wie Sie einen Endpunkt mit Endpoint Manager konfigurieren.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: 241f4a61e8d8c8de7a5573e8de534cb927a71b30
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101097779"
---
# <a name="configure-an-azure-front-door-standardpremium-preview-endpoint-with-endpoint-manager"></a>Konfigurieren eines Endpunkts in Azure Front Door Standard/Premium (Vorschau) mit Endpoint Manager

> [!NOTE]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Lesen Sie die **[Azure Front Door-Dokumente](../front-door-overview.md)** .

In diesem Artikel erfahren Sie, wie Sie mit Endpoint Manager einen Endpunkt für ein vorhandenes Azure Front Door Standard/Premium-Profil erstellen.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit Endpoint Manager einen Azure Front Door Standard/Premium-Endpunkt erstellen können, müssen Sie mindestens ein Azure Front Door-Profil erstellt haben. Das Profil muss mindestens einen Azure Front Door Standard/Premium-Endpunkt aufweisen. Sie können mehrere Profile verwenden, um Ihre Azure Front Door Standard/Premium-Endpunkte nach Internetdomäne, Webanwendung oder anderen Kriterien zu organisieren. 

Informationen zum Erstellen eines Azure Front Door-Profils finden Sie unter [Erstellen eines Profils für Front Door Standard/Premium](create-front-door-portal.md).

## <a name="create-a-new-azure-front-door-standardpremium-endpoint"></a>Erstellen eines neuen Azure Front Door Standard/Premium-Endpunkts

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem Azure Front Door Standard/Premium-Profil.

1. Wählen Sie **Endpunkt-Manager** aus. Wählen Sie dann **Endpunkt hinzufügen** aus, um einen neuen Endpunkt zu erstellen.
   
    :::image type="content" source="../media/how-to-configure-endpoint-manager/select-create-endpoint.png" alt-text="Screenshot: Hinzufügen eines Endpunkts über Endpoint Manager":::

1. Geben Sie auf der Seite **Endpunkt hinzufügen** die folgenden Einstellungen ein, und wählen Sie sie aus.
    
    :::image type="content" source="../media/how-to-configure-endpoint-manager/create-endpoint-page.png" alt-text="Screenshot der Seite „Endpunkt hinzufügen“":::

    | Einstellungen | Wert |
    | -------- | ----- |
    | Name | Geben Sie einen eindeutigen Namen für den neuen Azure Front Door Standard-/Premium-Endpunkt ein. Dieser Name wird für den Zugriff auf die zwischengespeicherten Ressourcen in der Domäne `<endpointname>.az01.azurefd.net` verwendet. |
    | Timeout für Ursprungsantwort (in Sekunden) | Geben Sie einen Timeoutwert in Sekunden ein, nach dessen Ablauf die Zeit für das Herstellen der Verbindung von Azure Front Door mit dem Ursprung überschritten wurde. |
    | Status | Aktivieren Sie das Kontrollkästchen zum Aktivieren des Endpunkts. |

## <a name="add-domains-origin-group-routes-and-security"></a>Hinzufügen von Domänen, Routen, Sicherheit und einer Ursprungsgruppe

1. Wählen Sie am Endpunkt **Endpunkt bearbeiten** aus, um die Route zu konfigurieren.

1. Wählen Sie auf der Seite **Endpunkt bearbeiten** unter „Domänen“ die Option **+ Hinzufügen** aus.
   
    :::image type="content" source="../media/how-to-configure-endpoint-manager/select-add-domain.png" alt-text="Screenshot zum Auswählen einer Domäne auf der Seite „Endpunkt bearbeiten“":::

### <a name="add-domain"></a>Hinzufügen einer Domäne

1. Wählen Sie auf der Seite **Domäne hinzufügen** die Option zum Zuordnen einer Domäne *Von Ihrem Azure Front Door-Profil* oder die Option *Neue Domäne hinzuzufügen* aus. Weitere Informationen zum Erstellen einer neuen Domäne finden Sie unter [Create a custom domain on Azure Front Door Standard/Premium SKU (Preview) using the Azure portal](how-to-add-custom-domain.md) (Erstellen einer neuen benutzerdefinierten Domäne in der Azure Front Door Standard/Premium-SKU (Vorschau) mit dem Azure-Portal, in englischer Sprache).

    :::image type="content" source="../media/how-to-configure-endpoint-manager/add-domain-page.png" alt-text="Screenshot der Seite „Domäne hinzufügen“":::

1. Wählen Sie **Hinzufügen** aus, um die Domäne dem aktuellen Endpunkt hinzuzufügen. Die ausgewählte Domäne sollte im Bereich „Domänen“ angezeigt werden.

    :::image type="content" source="../media/how-to-configure-endpoint-manager/domain-in-domainview.png" alt-text="Screenshot der Domänen in der Ansicht „Domänen“":::

### <a name="add-origin-group"></a>Hinzufügen einer Ursprungsgruppe

1. Wählen Sie in der Ansicht „Ursprungsgruppen“ die Option **Hinzufügen** aus. Die Seite **Ursprungsgruppe hinzufügen** wird angezeigt. 

    :::image type="content" source="../media/how-to-configure-endpoint-manager/add-origin-group-view.png" alt-text="Screenshot der Seite „Ursprungsgruppe hinzufügen“":::

1. Geben Sie unter **Name** einen eindeutigen Namen für die neue Ursprungsgruppe ein.

1. Wählen Sie **Ursprung hinzufügen** aus, um der aktuellen Gruppe einen neuen Ursprung hinzuzufügen.
 
#### <a name="health-probes"></a>Integritätstests
Front Door sendet regelmäßige HTTP/HTTPS-Testanforderungen an jeden Ursprung. Anhand der Testanforderungen werden Näherung und Integrität jedes Ursprungs für den Lastenausgleich der Endbenutzeranforderungen bestimmt. Mit den Integritätstesteinstellungen für eine Ursprungsgruppe wird festgelegt, wie der Integritätsstatus des App-Ursprungs abgefragt wird. Die folgenden Einstellungen sind für die Konfiguration des Lastenausgleichs verfügbar:

> [!WARNING]
> Da Front Door weltweit viele Edge-Umgebungen hat, kann das Volumen der Integritätstests für den Ursprung recht hoch sein. Es variiert abhängig von der konfigurierten Häufigkeit von Integritätstests zwischen 25 Anforderungen pro Minute und bis zu 1200 Anforderungen pro Minute. Bei der standardmäßigen Testfrequenz von 30 Sekunden sollte das Testvolumen im Ursprung etwa 200 Anforderungen pro Minute betragen.

* **Status**: Legen Sie fest, ob Integritätstests aktiviert werden sollen. Wenn die Ursprungsgruppe einen einzelnen Ursprung enthält, können Sie die Integritätstests deaktivieren, um die Last des Anwendungs-Back-Ends zu verringern. Selbst wenn die Ursprungsgruppe mehrere Ursprünge enthält, jedoch nur eine davon aktiviert ist, können Sie Integritätstests deaktivieren.
   
* **Pfad**: Die URL, die für Testanforderungen für alle Ursprünge in der Ursprungsgruppe verwendet wird. Beispiel: Wenn ein Ursprung auf „contoso-westus.azurewebsites.net“ und der Pfad auf „/probe/test.aspx“ festgelegt ist, senden Front Door-Umgebungen Integritätstestanforderungen an „`http://contoso-westus.azurewebsites.net/probe/test.aspx`“ (sofern HTTP als Protokoll festgelegt ist). 
   
* **Protokoll**: Legt fest, ob die Integritätstestanforderungen von Front Door an den Ursprung über das HTTP-Protokoll oder das HTTPS-Protokoll gesendet werden.
   
* **Testmethode**: Die HTTP-Methode, die zum Senden von Integritätstests verwendet werden soll. Mögliche Optionen sind GET oder HEAD (Standardeinstellung).

    > [!NOTE]
    > Um die Belastung und Kosten des Ursprungs zu senken, empfiehlt Front Door die Verwendung von HEAD-Anforderungen für Integritätstests.

* **Intervall (Sekunden):** Definiert die Häufigkeit der Integritätstests für den Ursprung bzw. die Intervalle, in denen die einzelnen Front Door-Umgebungen einen Test senden.
   
    >[!NOTE]
    >Für schnellere Failover sollten Sie das Intervall auf einen niedrigeren Wert festlegen. Je niedriger der Wert, desto größer ist jedoch das Integritätstestvolumen, das der Ursprung empfängt. Wenn beispielsweise bei 100 globalen Front Door-POPs das Intervall auf 30 Sekunden festgelegt ist, empfängt jedes Back-End etwa 200 Testanforderungen pro Minute.

#### <a name="load-balancing"></a>Lastenausgleich
Anhand der Einstellungen für den Lastenausgleich für die Ursprungsgruppe wird definiert, wie Integritätstests ausgewertet werden. Diese Einstellungen bestimmen, ob das Back-End fehlerfrei oder fehlerhaft ist. Dabei wird auch überprüft, wie der Lastenausgleich für den Datenverkehr zwischen den verschiedenen Ursprüngen in der Ursprungsgruppe erfolgen soll. Die folgenden Einstellungen sind für die Konfiguration des Lastenausgleichs verfügbar:

- **Stichprobengröße**: Gibt an, wie viele Stichproben von Integritätstests bei der Auswertung der Integrität der Ursprünge zu berücksichtigen sind.

- **Erfolgreiche Stichprobengröße**: Definiert die zuvor erwähnte Stichprobengröße, d. h. die Anzahl der erfolgreichen Stichproben, die erforderlich sind, um den Ursprung als fehlerfrei zu bezeichnen. Angenommen, das Intervall für Front Door-Integritätstests ist auf 30 Sekunden, die Stichprobengröße auf 5 und die erfolgreiche Stichprobengröße auf 3 festgelegt. Jedes Mal, wenn Integritätstests für den Ursprung ausgewertet werden, werden die letzten fünf Stichproben innerhalb von 150 Sekunden (5 x 30) betrachtet. Mindestens drei erfolgreiche Stichproben sind erforderlich, um Ihr Back-End als fehlerfrei zu deklarieren.

- **Wartezeitenaktivität (zusätzliche Wartezeit)** . Legt fest, ob Front Door die Anforderung an einen Ursprung innerhalb des Aktivitätsbereichs der Wartezeitmessung senden oder die Anforderung an das nächstgelegene Back-End weiterleiten soll.

Wählen Sie **Hinzufügen** aus, um die Ursprungsgruppe dem aktuellen Endpunkt hinzuzufügen. Die Ursprungsgruppe sollte im Bereich „Ursprungsgruppe“ angezeigt werden.

:::image type="content" source="../media/how-to-configure-endpoint-manager/origin-in-origin-group.png" alt-text="Screenshot der Ursprünge in der Ursprungsgruppe":::

### <a name="add-route"></a>Hinzufügen einer Route

Wählen Sie in der Ansicht „Routen“ **Hinzufügen** aus. Die Seite **Route hinzufügen** wird angezeigt. Informationen über das Zuordnen der Domäne und der Ursprungsgruppe finden Sie unter [Configure an Azure Front Door Standard/Premium Route](how-to-configure-route.md) (Konfigurieren einer Route in Azure Front Door Standard/Premium, in englischer Sprachen).

### <a name="add-security"></a>Hinzufügen von Sicherheit

1. Wählen Sie in der Ansicht „Sicherheit“ **Hinzufügen** aus. Die Seite **WAF-Richtlinie hinzufügen** wird angezeigt.
 
    :::image type="content" source="../media/how-to-configure-endpoint-manager/add-waf-policy-page.png" alt-text="Screenshot der Seite „WAF-Richtlinie hinzufügen“":::

1. **WAF-Richtlinie**: Wählen Sie eine WAF-Richtlinie aus, die auf die ausgewählte Domäne in diesem Endpunkt angewendet werden soll. 
  
   Wählen Sie **Neu erstellen** aus, um eine neue WAF-Richtlinie zu erstellen.

    :::image type="content" source="../media/how-to-configure-endpoint-manager/create-new-waf-policy.png" alt-text="Screenshot: Erstellen einer neuen WAF-Richtlinie":::
   
    **Name**: Geben Sie einen eindeutigen Name für die neue WAF-Richtlinie ein. Sie können auf der Seite „Web Application Firewall“ weitere Konfigurationseinstellungen dieser Richtlinie bearbeiten.

    **Domänen**: Wählen Sie die Domäne aus, auf die die WAF-Richtlinie angewendet werden soll.

1. Wählen Sie die Schaltfläche **Hinzufügen** aus. Die WAF-Richtlinie sollte im Bereich „Sicherheit“ angezeigt werden.

    :::image type="content" source="../media/how-to-configure-endpoint-manager/waf-in-security-view.png" alt-text="Screenshot der WAF-Richtlinie in der Ansicht „Sicherheit“":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Um einen Endpunkt zu löschen, wenn er nicht mehr benötigt wird, wählen Sie am Ende der Endpunktzeile **Endpunkt löschen** aus. 

:::image type="content" source="../media/how-to-configure-endpoint-manager/delete-endpoint.png" alt-text="Screenshot: Löschen eines Endpunkts":::

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie den Artikel [Create a custom domain on Azure Front Door Standard/Premium SKU (Preview) using the Azure portal](how-to-add-custom-domain.md) (Erstellen einer benutzerdefinierten Domäne in der Azure Front Door Standard/Premium-SKU (Vorschau) mit dem Azure-Portal, in englischer Sprache).
