---
title: 'Erneutes Generieren einer URL und Abfragezeichenfolge mit Azure Application Gateway: Azure-Portal'
description: Erfahren Sie, wie Sie mit dem Azure-Portal eine Azure Application Gateway-Instanz für das erneute Generieren einer URL und Abfragezeichenfolge konfigurieren.
services: application-gateway
author: surajmb
ms.service: application-gateway
ms.topic: how-to
ms.date: 7/16/2020
ms.author: surmb
ms.openlocfilehash: 160d056447bd53ea01437acd372b5efeb15b4773
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87083156"
---
# <a name="rewrite-url-with-azure-application-gateway---azure-portal-preview"></a>Erneutes Generieren einer URL mit Azure Application Gateway: Azure-Portal (Vorschau)

In diesem Artikel erfahren Sie, wie Sie über das Azure-Portal eine [Application Gateway v2-SKU](application-gateway-autoscaling-zone-redundant.md)-Instanz für das erneute Generieren einer URL konfigurieren.

>[!NOTE]
> Das Feature zum Umschreiben einer URL befindet sich in der Vorschauphase und ist nur für die Application Gateway-SKUs Standard_v2 und WAF_v2 verfügbar. Von der Verwendung dieser Option in Produktionsumgebungen wird abgeraten. Weitere Informationen zu Vorschauversionen finden Sie unter [Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="before-you-begin"></a>Voraussetzungen

Sie benötigen eine Application Gateway v2-SKU-Instanz, um die Schritte in diesem Artikel durchzuführen. Das erneute Generieren der URLs wird in der v1-SKU nicht unterstützt. Wenn Sie nicht über die v2-SKU verfügen, erstellen Sie eine [Application Gateway v2-SKU](tutorial-autoscale-ps.md)-Instanz, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="configure-url-rewrite"></a>Konfigurieren von URL-Rewrite

Wenn die Anforderungs-URL */article* enthält, werden im folgenden Beispiel die URL-Pfade und die URL-Abfragezeichenfolge erneut generiert.

`contoso.com/article/123/fabrikam` -> `contoso.com/article.aspx?id=123&title=fabrikam`

1. Wählen Sie die Option **Alle Ressourcen** und dann Ihr Anwendungsgateway aus.

2. Wählen Sie im linken Bereich **Umschreibungen** aus.

3. Wählen Sie **Umschreibungssatz** aus:

    :::image type="content" source="./media/rewrite-url-portal/rewrite-url-portal-1.png" alt-text="Hinzufügen eines Satzes zum erneuten Generieren":::

4. Geben Sie einen Namen für den Satz zum erneuten Generieren an, und ordnen Sie ihm eine Routingregel zu:

    a. Geben Sie den Namen für den Satz zum erneuten Generieren in das Feld **Name** ein.
    
    b. Wählen Sie eine oder mehrere Regeln aus, die in der Liste **Zugeordnete Routingregeln** aufgeführt sind. Damit wird die Konfiguration der erneuten Generierung anhand der Routingregel dem Quelllistener zugeordnet. Sie können nur Routingregeln auswählen, die keinen anderen Sätzen zum erneuten Generieren zugeordnet sind. Die Regeln, die bereits anderen Sätzen zum erneuten Generieren zugeordnet sind, sind abgeblendet.
    
    c. Wählen Sie **Weiter** aus.
    
    :::image type="content" source="./media/rewrite-url-portal/rewrite-url-portal-2.png" alt-text="Hinzufügen eines Satzes zum erneuten Generieren":::

5. Erstellen einer Regel zum erneuten Generieren:

    a. Wählen Sie **Neuschreibungsregel hinzufügen** aus.
    
    :::image type="content" source="./media/rewrite-url-portal/rewrite-url-portal-3.png" alt-text="Hinzufügen eines Satzes zum erneuten Generieren":::
    
    b. Geben Sie einen Namen für die Regel zum erneuten Generieren in das Feld **Umschreibungsregelname** ein. Geben Sie eine Zahl in das Feld **Regelsequenz** ein.

6. In diesem Beispiel generieren wir den URL-Pfad und die URL-Abfragezeichenfolge nur dann neu, wenn der Pfad */article* enthält. Fügen Sie zu diesem Zweck eine Bedingung hinzu, um auszuwerten, ob der URL-Pfad */article* enthält.

    a. Wählen Sie auf **Bedingung hinzufügen** und dann das Feld aus, das die **If**-Anweisungen enthält, um es zu erweitern.
    
    b. Da in diesem Beispiel das Muster */article* im URL-Pfad überprüft werden soll, wählen Sie in der Liste **Typ der zu überprüfenden Variablen** die Option **Servervariable** aus.
    
    c. Wählen Sie in der Liste **Servervariable** die Option „uri_path“ aus.
    
    d. Wählen Sie unter **Groß-/Kleinschreibung** den Eintrag **Keine** aus.
    
    e. Wählen Sie in der Liste **Operator** den Eintrag **gleich (=)** aus.
    
    f. Geben Sie ein reguläres Ausdrucksmuster ein. In diesem Beispiel verwenden wir das Muster `.*article/(.*)/(.*)`.
    
      () wird verwendet, um die substring-Funktion für die spätere Verwendung bei der Erstellung des Ausdrucks zum erneuten Generieren des URL-Pfads zu erfassen. Weitere Informationen finden Sie [hier](rewrite-http-headers-url.md#capturing).

    g. Klicken Sie auf **OK**.

    :::image type="content" source="./media/rewrite-url-portal/rewrite-url-portal-4.png" alt-text="Hinzufügen eines Satzes zum erneuten Generieren":::

 

7. Hinzufügen einer Aktion zum erneuten Generieren der URL und des URL-Pfads

   a. Wählen Sie in der Liste **Typ umschreiben** die Option **URL** aus.

   b. Wählen Sie in der Liste **Aktionstyp** den Eintrag **Satz** aus.

   c. Wählen Sie unter **Komponenten** sowohl den **URL-Pfad als auch die URL-Abfragezeichenfolge** aus.

   d. Geben Sie als **URL-Pfadwert** den neuen Wert des Pfads ein. In diesem Beispiel verwenden wir **/article.aspx**. 

   e. Geben Sie als **URL-Abfragezeichenfolgenwert** den neuen Wert der URL-Abfragezeichenfolge ein. In diesem Beispiel verwenden wir **id={var_uri_path_1}&title={var_uri_path_2}** .
    
    `{var_uri_path_1}` und `{var_uri_path_1}` werden zum Abrufen der substring-Funktionen verwendet, die während der Auswertung der Bedingung in diesem Ausdruck aufgezeichnet wurden: `.*article/(.*)/(.*)`.
    
   f. Klicken Sie auf **OK**.

    :::image type="content" source="./media/rewrite-url-portal/rewrite-url-portal-5.png" alt-text="Hinzufügen eines Satzes zum erneuten Generieren":::

8. Klicken Sie auf **Erstellen**, um den Satz zum erneuten Generieren zu erstellen.

9. Sicherstellen, dass der neue Satz zum erneuten Generieren in der Liste der Sätze zum erneuten Generieren angezeigt wird

    :::image type="content" source="./media/rewrite-url-portal/rewrite-url-portal-6.png" alt-text="Hinzufügen eines Satzes zum erneuten Generieren":::

## <a name="verify-url-rewrite-through-access-logs"></a>Überprüfen der erneuten URL-Generierung durch Zugriffsprotokolle

Beachten Sie die unten aufgeführten Felder in Zugriffsprotokollen, um zu überprüfen, ob die erneuten URL-Generierung gemäß ihrer Erwartung erfolgt ist.

* **originalRequestUriWithArgs**: Dieses Feld enthält die ursprüngliche Anforderungs-URL.
* **requestUri**: Dieses Feld enthält die URL nach dem Vorgang zur erneuten Generierung in Application Gateway.

Weitere Informationen zu allen Feldern in den Zugriffsprotokollen finden Sie [hier](application-gateway-diagnostics.md#for-application-gateway-and-waf-v2-sku).

##  <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Einrichten der erneuten Generierung für einige gängige Anwendungsfälle finden Sie unter [Allgemeine Szenarien zum erneuten Generieren](rewrite-http-headers.md).
