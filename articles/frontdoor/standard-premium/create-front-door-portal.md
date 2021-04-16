---
title: 'Schnellstart: Erstellen eines Azure Front Door Standard/Premium-Profils im Azure-Portal'
description: In dieser Schnellstartanleitung wird die Vorgehensweise zum Verwenden des Azure Front Door Standard/Premium-Diensts für globale Webanwendungen mit Hochverfügbarkeit und hoher Leistung über das Azure-Portal veranschaulicht.
services: frontdoor
author: duongau
manager: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/16/2021
ms.author: duau
ms.openlocfilehash: 8c6fe355621b4f096814268dea8fd2fc8e1166a7
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106552868"
---
# <a name="quickstart-create-an-azure-front-door-standardpremium-profile---azure-portal"></a>Schnellstart: Erstellen eines Azure Front Door Standard/Premium-Profils im Azure-Portal

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Lesen Sie die [Azure Front Door-Dokumente](../front-door-overview.md).

In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Azure Front Door Standard/Premium-Profil im Azure-Portal erstellen. Sie können das Azure Front Door Standard/Premium-Profil über *Schnell erstellen* mit grundlegenden Konfigurationen oder über *Benutzerdefiniert erstellen* mit erweiterten Konfigurationen erstellen. Mit *Benutzerdefiniert erstellen* stellen Sie zwei Web-Apps bereit. Als Nächstes erstellen Sie das Azure Front Door Standard/Premium-Profil mithilfe der beiden Web-Apps als Ursprung. Anschließend überprüfen Sie die Konnektivität mit Ihren Web-Apps mithilfe des Frontendhostnamens von Azure Front Door Standard/Premium.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-front-door-profile---quick-create"></a>Erstellen eines Front Door-Profils: Schnellerfassung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie auf der Startseite oder im Azure-Menü die Option  **+ Ressource erstellen** aus. Suchen Sie nach  *Front Door Standard/Premium (Vorschau)* . Klicken Sie anschließend auf **Erstellen**.

1. Wählen Sie auf der Seite **Angebote vergleichen** die Option **Schnell erstellen** aus. Wählen Sie dann **Zum Erstellen einer Front Door-Instanz fortfahren** aus.

   :::image type="content" source="../media/create-front-door-portal/front-door-quick-create.png" alt-text="Screenshot der Seite „Angebote vergleichen“.":::

1. Geben Sie auf der Seite **Front Door-Profil erstellen** die folgenden Einstellungen ein, oder wählen Sie sie aus.

    :::image type="content" source="../media/create-front-door-portal/front-door-quick-create-2.png" alt-text="Screenshot der Seite zum schnellen Erstellen des Front Door-Profils.":::    

    | Einstellungen | Wert |
    | --- | --- |
    | **Abonnement**  | Wählen Sie Ihr Abonnement aus. |
    | **Ressourcengruppe**  | Wählen Sie **Neu erstellen** aus, und geben Sie *contoso-appservice* in das Textfeld ein.|
    | **Name** | Geben Sie dem Profil einen Namen. In diesem Beispiel wird **contoso-afd-quickcreate** verwendet. |
    | **Tier** | Wählen Sie entweder Standard- oder Premium-SKU aus. Die Standard-SKU ist eine optimierte Inhaltsbereitstellung. Die Premium-SKU baut auf der Standard-SKU auf und ist sicherheitskonzentriert. Siehe [Vergleich der Dienstebenen](tier-comparison.md).  |
    | **Endpunktname** | Geben Sie einen global eindeutigen Namen für Ihren Endpunkt ein. |
    | **Ursprungstyp** | Wählen Sie den Ressourcentyp für den Ursprung aus. In diesem Beispiel wählen wir einen App-Dienst als Ursprung aus, für den Private Link aktiviert ist. |
    | **Ursprungshostname** | Geben Sie den Hostnamen für Ihren Ursprung ein. |
    | **Aktivieren von Private Link** | Wenn Sie eine private Verbindung zwischen Ihrer Azure Front Door-Instanz und Ihrem Ursprung wünschen. Weitere Informationen finden Sie unter [Sichern ihres Ursprungs mit Private Link in Azure Front Door Standard/Premium (Vorschau)](concept-private-link.md) und [Herstellen einer Verbindung von Azure Front Door Premium mit einem Web-App-Ursprung mit Private Link](./how-to-enable-private-link-web-app.md).
    | **Zwischenspeichern** | Aktivieren Sie dieses Kontrollkästchen, wenn Sie mit den Edge-POPs von Azure Front Door und dem Microsoft-Netzwerk global Inhalte näher bei den Benutzern zwischenspeichern möchten. |
    | **WAF-Richtlinie** | Wählen Sie **Neu erstellen** aus, oder wählen Sie in der Dropdownliste eine vorhandene WAF-Richtlinie aus, wenn Sie dieses Feature aktivieren möchten. |

    > [!NOTE]
    > Wenn Sie ein Azure Front Door Standard/Premium-Profil erstellen, müssen Sie einen Ursprung aus demselben Abonnement auswählen, in dem die Front Door-Instanz erstellt ist.

1. Wählen Sie **Überprüfen und erstellen** aus, um das Front-Door-Profil in Betrieb zu nehmen.

   > [!NOTE]
   > Es kann einige Minuten dauern, bis die Konfigurationen an alle Edge-POPs weitergegeben werden.

1. Klicken Sie dann auf **Erstellen**, um das Front Door-Profil bereitzustellen und zu starten.

1. Wenn Sie Private Link aktiviert haben, navigieren Sie zu Ihrem Ursprung (in diesem Beispiel App-Dienst). Wählen Sie **Netzwerk** > **Private Link konfigurieren** aus. Wählen Sie dann die ausstehende Anforderung von Azure Front Door aus, und klicken Sie auf „Genehmigen“. Nach einigen Sekunden kann auf sichere Weise über Azure Front Door auf Ihre Anwendung zugegriffen werden.

## <a name="create-front-door-profile---custom-create"></a>Erstellen eines Front Door-Profils: Benutzerdefiniert erstellen

### <a name="create-a-web-app-with-two-instances-as-the-origin"></a>Erstellen einer Web-App mit zwei Instanzen als Ursprung

Wenn Sie bereits einen Ursprung oder eine Ursprungsgruppe konfiguriert haben, fahren Sie mit „Erstellen einer Front Door Standard-/Premium-Instanz (Vorschau) für Ihre Anwendung“ fort.

In diesem Beispiel erstellen wir eine Webanwendung mit zwei Instanzen, die in unterschiedlichen Azure-Regionen ausgeführt werden. Beide Webanwendungsinstanzen werden im *Aktiv/Aktiv*-Modus ausgeführt, sodass jede von ihnen Datenverkehr annehmen kann. Diese Konfiguration unterscheidet sich von einer *Aktiv/Standby*-Konfiguration, bei der eine Instanz als Failover fungiert.

Wenn Sie noch nicht über eine Web-App verfügen, richten Sie anhand der folgenden Schritte eine Beispielwebanwendung ein.

1. Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Web-App**.

1. Geben Sie auf der Registerkarte **Grundlagen** der Seite **Web-App erstellen** die folgenden Informationen ein, bzw. wählen Sie sie aus.

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | **Abonnement**               | Wählen Sie Ihr Abonnement aus. |
    | **Ressourcengruppe**       | Wählen Sie die Option **Neu erstellen** aus, und geben Sie im Textfeld den Text *FrontDoorQS_rg1* ein.|
    | **Name**                   | Geben Sie unter **Name** einen eindeutigen Namen für Ihre Web-App ein. In diesem Beispiel wird *WebAppContoso-001* verwendet. |
    | **Veröffentlichen** | Wählen Sie **Code** aus. |
    | **Runtimestapel**         | Wählen Sie **.NET Core 2.1 (LTS)** aus. |
    | **Betriebssystem**          | Wählen Sie **Windows** aus. |
    | **Region**           | Wählen Sie **USA, Mitte** aus. |
    | **Windows-Plan** | Wählen Sie **Neu erstellen** aus, und geben Sie im Textfeld den Text *myAppServicePlanCentralUS* ein. |
    | **SKU und Größe** | Wählen Sie **Standard S1, 100 ACU insgesamt, 1,75 GB Arbeitsspeicher** aus. |

     :::image type="content" source="../media/create-front-door-portal/create-web-app.png" alt-text="Schnelles Erstellen der Front Door Premium-SKU im Azure-Portal":::

1. Wählen Sie **Überprüfen und erstellen** aus, überprüfen Sie die Zusammenfassung, und wählen Sie dann die Option **Erstellen** aus. Die Bereitstellung kann ein paar Minuten dauern.

Erstellen Sie nach Abschluss der Bereitstellung eine zweite Web-App. Verwenden Sie mit Ausnahme der folgenden Einstellungen die gleichen Einstellungen wie oben:

| Einstellung          | Wert     |
| ---              | ---  |
| **Ressourcengruppe**   | Wählen Sie **Neu erstellen** aus, und geben Sie *FrontDoorQS_rg2* ein. |
| **Name**             | Geben Sie einen eindeutigen Namen für Ihre Web-App ein. In diesem Beispiel wird *WebAppContoso-002* verwendet.  |
| **Region**           | Geben Sie eine andere Region ein. In diesem Beispiel wird *USA, Süden-Mitte* verwendet. |
| **App Service-Plan** > **Windows-Plan**         | Wählen Sie **Neu** aus, geben Sie *myAppServicePlanSouthCentralUS* ein, und wählen Sie dann **OK** aus. |

### <a name="create-a-front-door-standardpremium-preview-for-your-application"></a>Erstellen einer Front Door Standard-/Premium-Instanz (Vorschau) für Ihre Anwendung

Konfigurieren Sie Azure Front Door Standard/Premium (Vorschau) zum Weiterleiten des Benutzerdatenverkehrs basierend auf der geringsten Wartezeit zwischen den beiden Web-App-Servern. Schützen Sie auch Front Door mit Web Application Firewall. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
 
1. Wählen Sie auf der Startseite oder im Azure-Menü die Option  **+ Ressource erstellen** aus. Suchen Sie nach  *Front Door Standard/Premium (Vorschau)* . Klicken Sie anschließend auf **Erstellen**.

1. Wählen Sie auf der Seite **Angebote vergleichen** die Option **Benutzerdefiniert erstellen** aus. Wählen Sie dann **Zum Erstellen einer Front Door-Instanz fortfahren** aus.

1. Geben Sie auf der Registerkarte **Grundlagen**  die folgenden Informationen ein, oder wählen Sie sie aus, und wählen Sie dann  **Weiter: Geheim** aus. 

    | Einstellung | Wert |
    | --- | --- |
    | **Abonnement** | Wählen Sie Ihr Abonnement aus. |
    | **Ressourcengruppe** | Wählen Sie die Option **Neu erstellen** aus, und geben Sie im Textfeld den Text *FrontDoorQS_rg0* ein. |
    | **Ressourcengruppenstandort** | Wählen Sie **USA, Osten** aus. |
    | **Profilname** | Geben Sie einen eindeutigen Namen in diesem Abonnement **Webapp-Contoso-AFD** ein. |
    | **Tier** | Wählen Sie **Premium** aus. |
    
    :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-2.png" alt-text="Erstellen eines Front Door-Profils":::

1. *Optional*: **Geheimnisse**. Wenn Sie verwaltete Zertifikate verwenden möchten, ist dieser Schritt optional. Wenn Sie bereits über eine Key Vault-Instanz in Azure verfügen, die Sie verwenden möchten, um ein eigenes Zertifikat (Bring Your Own Certificate, BYOC) für die benutzerdefinierte Domäne zu verwenden, wählen Sie **Zertifikat hinzufügen** aus. Sie können das Zertifikat auch nach der Erstellung in der Verwaltungsbenutzeroberfläche hinzufügen.

    > [!NOTE]
    > Sie benötigen die richtige Berechtigung, um das Zertifikat aus Azure Key Vault als Benutzer hinzuzufügen. 

    :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-secret.png" alt-text="Screenshot: Hinzufügen eines Geheimnisses beim benutzerdefinierten Erstellen.":::

1. Wählen Sie auf der Registerkarte **Endpunkt** die Option **Endpunkt hinzufügen** aus, und geben Sie Ihrem Endpunkt einen global eindeutigen Namen. Sie können mehrere Endpunkte in Ihrem Azure Front Door Standard/Premium-Profil erstellen, nachdem Sie die Erstellung abgeschlossen haben. In diesem Beispiel wird *contoso-frontend* verwendet. Behalten Sie für „Timeout für Ursprungsantwort (in Sekunden)“ und „Status“ die Standardwerte bei. Wählen Sie **Hinzufügen** aus, um den Endpunkt hinzuzufügen.
    
    :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-endpoint.png" alt-text="Screenshot des Hinzufügens eines Endpunkts.":::

1. Fügen Sie als Nächstes eine Ursprungsgruppe hinzu, die ihre zwei Web-Apps enthält. Wählen Sie  **+ Hinzufügen**  aus, um die Seite  **Ursprungsgruppe hinzufügen** zu öffnen. Geben Sie unter „Name“  *myOrignGroup* ein, und wählen Sie dann  **+ Ursprung hinzufügen** aus.
 
     :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-origin-group.png" alt-text="Screenshot des Hinzufügens eines Ursprungs.":::

1. Geben Sie auf der Seite  **Ursprung hinzufügen**  die folgenden Informationen ein, oder wählen Sie sie aus. Wählen Sie anschließend **Hinzufügen**.

    | Einstellung | Wert |
    | --- | --- |
    | **Name** | Geben Sie **webapp1** ein. |
    | **Ursprungstyp** | Wählen Sie **App-Dienste** aus. |
    | **Hostname** | Wählen Sie `WebAppContoso-001.azurewebsites.net` aus. |
    | **Header des Ursprungshosts** | Wählen Sie `WebAppContoso-001.azurewebsites.net` aus. |
    | **Weitere Felder** | Übernehmen Sie für alle anderen Felder die Standardeinstellungen. |

    > [!NOTE]
    > Wenn Sie ein Azure Front Door Standard/Premium-Profil erstellen, müssen Sie einen Ursprung aus demselben Abonnement auswählen, in dem die Azure Front Door Standard/Premium-Instanz erstellt ist.
   
   :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-origin-1.png" alt-text="Screenshot: Hinzufügen von weiteren Ursprüngen.":::

1. Wiederholen Sie Schritt 8, um den zweiten Ursprung „webapp002“ hinzuzufügen. Wählen Sie `webappcontoso-002.azurewebsite.net` als **Ursprungshostnamen** und **Hostheader für Ursprung** aus.

1. Auf der Seite **Ursprungsgruppe hinzufügen** werden zwei Ursprünge hinzugefügt, bei allen anderen Felder werden die Standardeinstellungen beibehalten.
  
   :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-origin-group-2.png" alt-text="Screenshot der Seite zum Hinzufügen einer Ursprungsgruppe.":::

1. Fügen Sie als Nächstes eine Route hinzu, um den Front-End-Endpunkt der Ursprungsgruppe zuzuordnen. Diese Route leitet Anforderungen vom Endpunkt an myOriginGroup weiter. Wählen Sie **+ Hinzufügen** für „Route“ aus, um eine Route zu konfigurieren.  

1. Geben Sie auf der Seite **Route hinzufügen** Informationen ein, oder wählen Sie die folgenden Informationen aus. Wählen Sie anschließend **Hinzufügen**.
  
    :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-route-without-caching.png" alt-text="Route ohne Zwischenspeichern hinzufügen":::

    | Einstellung | Wert |
    | --- | --- |
    | **Name** | Geben Sie **MyRoute** ein. |    
    | **Domäne** | Wählen Sie `contoso-frontend.z01.azurefd.net` aus. | 
    | **Hostname** | Wählen Sie `WebAppContoso-001.azurewebsites.net` aus. |
    | **Abzugleichende Muster** | Behalten Sie den Standard bei. |
    | **Akzeptierte Protokolle** | Behalten Sie den Standard bei. | 
    | **Umleiten** | Belassen Sie den Standardwert für **Gesamten Datenverkehr zur Verwendung von HTTPS umleiten** bei. | 
    | **Ursprungsgruppe** | Wählen Sie **MyOriginGroup** aus. | 
    | **Ursprungspfad** | Behalten Sie den Standard bei. | 
    | **Weiterleitungsprotokoll** | Wählen Sie **Eingehende Anforderung abgleichen** aus. | 
    | **Zwischenspeichern** | Lassen Sie diese Option in diesem Schnellstart deaktiviert. Wenn Sie möchten, dass Ihre Inhalte in Edges zwischengespeichert werden, aktivieren Sie das Kontrollkästchen **Zwischenspeicherung aktivieren**. |
    | **Regeln** | Behalten Sie den Standard bei. Nachdem Sie Ihr Front Door-Profil erstellt haben, können Sie benutzerdefinierte Regeln erstellen und auf Routen anwenden. |  
       
    >[!WARNING]
    > **Stellen Sie sicher**, dass für jeden Endpunkt eine Route vorhanden ist. Wenn keine Route vorhanden ist, kann bei einem Endpunkt ein Fehler auftreten.

1. Wählen Sie als Nächstes **+ Hinzufügen** für „Sicherheit“ aus, um eine WAF-Richtlinie hinzuzufügen. Wählen Sie **Neue hinzufügen** aus, und geben Sie Ihrer Richtlinie einen eindeutigen Namen. Aktivieren Sie das Kontrollkästchen für **Bot-Schutz hinzufügen**. Wählen Sie den Endpunkt in **Domänen** und dann **Hinzufügen** aus.

   :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-waf-policy-2.png" alt-text="WAF-Richtlinie hinzufügen":::

1. Wählen Sie  **Überprüfen und erstellen** und dann  **Erstellen** aus. Es kann einige Minuten dauern, bis die Konfigurationen an alle Edge-POPs weitergegeben werden. Nun verfügen Sie über das erste Front End-Profil und den Endpunkt.

   :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-review.png" alt-text="Benutzerdefinierte Erstellung prüfen":::

## <a name="verify-azure-front-door"></a>Überprüfen von Azure Front Door

Nachdem Sie ein Azure Front Door Standard/Premium-Profil erstellt haben, dauert es einige Minuten, bis die Konfiguration global bereitgestellt ist. Nach Abschluss des Vorgangs können Sie auf den von Ihnen erstellten Front-End-Host zugreifen. Navigieren Sie in einem Browser zu `contoso-frontend.z01.azurefd.net`. Ihre Anforderung wird automatisch an den nächstgelegenen Server aus den angegebenen Servern in der Ursprungsgruppe weitergeleitet.

Wenn Sie diese Apps in dieser Schnellstartanleitung erstellt haben, wird eine Seite mit Informationen angezeigt.

Führen Sie die folgenden Schritte aus, um das sofortige globale Failover zu testen:

1. Öffnen Sie wie oben beschrieben einen Browser, und navigieren Sie zur Front-End-Adresse: `contoso-frontend.azurefd.net`.

1. Suchen Sie im Azure-Portal nach *App Services*, und wählen Sie den Eintrag aus. Scrollen Sie nach unten zu einer Ihrer Web-Apps. In diesem Beispiel wird **WebAppContoso-001** verwendet.

1. Wählen Sie Ihre Web-App und anschließend **Beenden** und zum Bestätigen **Ja** aus.

1. Aktualisieren Sie Ihren Browser. Die gleiche Seite mit Informationen sollte angezeigt werden.

   >[!TIP]
   >Bei diesen Aktionen tritt eine kleine Verzögerung auf. Möglicherweise müssen Sie den Browser erneut aktualisieren.

1. Navigieren Sie zur anderen Web-App, und beenden Sie sie ebenfalls.

1. Aktualisieren Sie Ihren Browser. Dieses Mal sollte eine Fehlermeldung angezeigt werden.

    :::image type="content" source="../media/create-front-door-portal/web-app-stopped-message.png" alt-text="Beide Instanzen der Web-App beendet":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie fertig sind, können Sie alle erstellten Elemente entfernen. Wenn eine Ressourcengruppe gelöscht wird, werden auch die darin befindlichen Inhalte gelöscht. Wenn Sie diese Front Door-Instanz nicht weiter verwenden möchten, sollten Sie Ressourcen entfernen, um unnötige Gebühren zu vermeiden.

1. Suchen Sie im Azure-Portal nach **Ressourcengruppen**, und wählen Sie den Eintrag aus, oder wählen Sie im Menü des Azure-Portals die Option **Ressourcengruppen** aus.

1. Zum Suchen einer Ressourcengruppe (etwa **FrontDoorQS_rg0**) können Sie filtern oder nach unten scrollen.

1. Wählen Sie die Ressourcengruppe und dann **Ressourcengruppe löschen** aus.

   >[!WARNING]
   >Diese Aktion kann nicht rückgängig gemacht werden.

1. Geben Sie zur Bestätigung den Ressourcengruppennamen ein, und wählen Sie dann **Löschen** aus.

Wiederholen Sie den Vorgang für die anderen beiden Gruppen.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie Ihrer Front Door-Instanz eine benutzerdefinierte Domäne hinzufügen.
> [!div class="nextstepaction"]
> [Hinzufügen einer benutzerdefinierten Domäne](how-to-add-custom-domain.md)