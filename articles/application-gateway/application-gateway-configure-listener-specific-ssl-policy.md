---
title: Konfigurieren listenerspezifischer SSL-Richtlinien für Azure Application Gateway über das Portal
description: Hier erfahren Sie, wie Sie listenerspezifische SSL-Richtlinien für Application Gateway über das Portal konfigurieren.
services: application-gateway
author: mscatyao
ms.service: application-gateway
ms.topic: how-to
ms.date: 03/30/2021
ms.author: caya
ms.openlocfilehash: b0bda1921c63b4e353365c3b391e17f620740396
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106231001"
---
# <a name="configure-listener-specific-ssl-policies-on-application-gateway-through-portal-preview"></a>Konfigurieren listenerspezifischer SSL-Richtlinien für Application Gateway über das Portal (Vorschau)

In diesem Artikel erfahren Sie, wie Sie das Azure-Portal verwenden, um listenerspezifische SSL-Richtlinien für Ihre Application Gateway-Instanz zu konfigurieren. Mithilfe listenerspezifischer SSL-Richtlinien können Sie bestimmte Listener für die Verwendung jeweils unterschiedlicher SSL-Richtlinien konfigurieren. Sie können weiterhin eine SSL-Standardrichtlinie festlegen, die von allen Listenern verwendet wird, solange sie nicht durch eine listenerspezifische SSL-Richtlinie überschrieben wird. 

> [!NOTE]
> Da listenerspezifische Richtlinien Teil von SSL-Profilen sind und SSL-Profile nur für v2-Gateways unterstützt werden, werden listenerspezifische Richtlinien nur von den SKUs „Standard_v2“ und „WAF_v2“ unterstützt. 



Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-new-application-gateway"></a>Erstellen eines neuen Application Gateway

Erstellen Sie im Portal zunächst wie gewohnt eine neue Application Gateway-Instanz. Bei der Erstellung sind keine zusätzlichen Schritte erforderlich, um listenerspezifische SSL-Richtlinien konfigurieren zu können. Weitere Informationen zur Erstellung einer Application Gateway-Instanz im Portal finden Sie in unserer [Schnellstartanleitung für die Erstellung im Portal](./quick-create-portal.md).

## <a name="set-up-a-listener-specific-ssl-policy"></a>Einrichten einer listenerspezifischen SSL-Richtlinie

Wenn Sie eine listenerspezifische SSL-Richtlinie einrichten möchten, müssen Sie im Portal zunächst zur Registerkarte **SSL-Einstellungen (Vorschau)** navigieren und ein neues SSL-Profil erstellen. Bei der SSL-Profilerstellung werden zwei Registerkarten angezeigt: **Clientauthentifizierung** und **SSL-Richtlinie**. Die Registerkarte **SSL-Richtlinie** dient zum Konfigurieren einer listenerspezifischen SSL-Richtlinie. Die Registerkarte **Clientauthentifizierung** dient zum Hochladen von Clientzertifikaten für die gegenseitige Authentifizierung. Weitere Informationen finden Sie unter [Konfigurieren der gegenseitigen Authentifizierung in Application Gateway über das Portal (Vorschau)](./mutual-authentication-portal.md).

> [!NOTE]
> Wir empfehlen die Verwendung von TLS 1.2, da TLS 1.2 in Zukunft obligatorisch sein wird. 

1. Suchen Sie im Portal nach **Application Gateway**, wählen Sie die Option **Anwendungsgateways** aus, und klicken Sie auf Ihre vorhandene Application Gateway-Instanz.

2. Wählen Sie im Menü auf der linken Seite die Option **SSL-Einstellungen (Vorschau)** aus.

3. Klicken Sie oben auf das Pluszeichen neben **SSL-Profile**, um ein neues SSL-Profil zu erstellen.

4. Geben Sie unter **SSL Profile Name** (Name des SSL-Profils) einen Namen ein. In diesem Beispiel geben wir dem SSL-Profil den Namen *applicationGatewaySSLProfile*. 

5. Wechseln Sie zur Registerkarte **SSL-Richtlinie**, und aktivieren Sie das Kontrollkästchen **Enable listener-specific SSL Policy** (Listenerspezifische SSL-Richtlinie aktivieren). 

6. Richten Sie Ihre listenerspezifische SSL-Richtlinie gemäß Ihren Anforderungen ein. Sie können entweder vordefinierte SSL-Richtlinien verwenden oder Ihre eigene SSL-Richtlinie anpassen. Weitere Informationen zu SSL-Richtlinien finden Sie in der [Übersicht über SSL-Richtlinien](./application-gateway-ssl-policy-overview.md). Wir empfehlen die Verwendung von TLS 1.2.

7. Wählen Sie die Option **Hinzufügen** aus, um die Angaben zu speichern.

    > [!NOTE]
    > Sie müssen keine Clientauthentifizierung für ein SSL-Profil konfigurieren, um es einem Listener zuzuordnen. Clientauthentifizierung und listenerspezifische SSL-Richtlinie können entweder jeweils einzeln oder auch gemeinsam in Ihrem SSL-Profil konfiguriert sein.  

    ![Hinzufügen einer listenerspezifischen SSL-Richtlinie zum SSL-Profil](./media/application-gateway-configure-listener-specific-ssl-policy/listener-specific-ssl-policy-ssl-profile.png)
    
## <a name="associate-the-ssl-profile-with-a-listener"></a>Zuordnen des SSL-Profils zu einem Listener

Nachdem wir nun über ein SSL-Profil mit einer listenerspezifischen SSL-Richtlinie verfügen, müssen wir das SSL-Profil dem Listener zuordnen, um die listenerspezifische Richtlinie zu aktivieren. 

1. Navigieren Sie zu Ihrer vorhandenen Application Gateway-Instanz. Wenn Sie die obigen Schritte gerade eben abgeschlossen haben, sind hier keine weiteren Aktionen erforderlich. 

2. Wählen Sie im linken Menü **Listener** aus. 

3. Klicken Sie auf **Listener hinzufügen**, falls Sie noch keinen HTTPS-Listener eingerichtet haben. Wenn Sie bereits über einen HTTPS-Listener verfügen, können Sie in der Liste darauf klicken. 

4. Füllen Sie die Felder **Listenername**, **Front-End-IP-Adresse**, **Port**, **Protokoll** und **HTTPS-Einstellungen** gemäß Ihren Anforderungen aus.

5. Aktivieren Sie das Kontrollkästchen **SSL-Profil aktivieren**, damit Sie auswählen können, welches SSL-Profil dem Listener zugeordnet werden soll. 

6. Wählen Sie das von Ihnen erstellte SSL-Profil in der Dropdownliste aus. In diesem Beispiel wählen wir das SSL-Profil aus, das wir in den obigen Schritten erstellt haben: *applicationGatewaySSLProfile*. 

7. Konfigurieren Sie die restlichen Einstellungen für den Listener gemäß Ihren Anforderungen. 

8. Klicken Sie auf **Hinzufügen**, um Ihren neuen Listener mit dem zugeordneten SSL-Profil zu speichern. 

    ![Zuordnen des SSL-Profils zum neuen Listener](./media/mutual-authentication-portal/mutual-authentication-listener-portal.png)        

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten von Webdatenverkehr mit einem Anwendungsgateway per Azure CLI](./tutorial-manage-web-traffic-cli.md)