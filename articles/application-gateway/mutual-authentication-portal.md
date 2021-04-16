---
title: Konfigurieren der gegenseitigen Authentifizierung in Azure Application Gateway über das Portal
description: Es wird beschrieben, wie Sie eine Application Gateway-Instanz so konfigurieren, dass die gegenseitige Authentifizierung über das Portal möglich ist.
services: application-gateway
author: mscatyao
ms.service: application-gateway
ms.topic: how-to
ms.date: 04/02/2021
ms.author: caya
ms.openlocfilehash: b2118a257f029a63445b09dfa618d6e3ceacddda
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106230862"
---
# <a name="configure-mutual-authentication-with-application-gateway-through-portal-preview"></a>Konfigurieren der gegenseitigen Authentifizierung in Application Gateway über das Portal (Vorschau)

In diesem Artikel wird beschrieben, wie Sie das Azure-Portal verwenden, um die gegenseitige Authentifizierung auf Ihrer Application Gateway-Instanz zu konfigurieren. Gegenseitige Authentifizierung bedeutet Folgendes: Application Gateway authentifiziert den Client, der die Anforderung sendet, indem das Clientzertifikat verwendet wird, das Sie auf die Application Gateway-Instanz hochladen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="before-you-begin"></a>Voraussetzungen

Zum Konfigurieren der gegenseitigen Authentifizierung über eine Application Gateway-Instanz benötigen Sie ein Clientzertifikat zum Hochladen auf das Gateway. Das Clientzertifikat wird verwendet, um das Zertifikat zu überprüfen, das vom Client für Application Gateway angegeben wird. Sie können zu Testzwecken ein selbstsigniertes Zertifikat verwenden. Dies wird jedoch nicht für Produktionsworkloads empfohlen, da sie schwieriger zu verwalten und nicht vollständig sicher sind. 

Weitere Informationen, vor allem zur Art der Clientzertifikate, die Sie hochladen können, finden Sie unter [Übersicht über die gegenseitige Authentifizierung mit Application Gateway](./mutual-authentication-overview.md#certificates-supported-for-mutual-authentication).

## <a name="create-a-new-application-gateway"></a>Erstellen eines neuen Application Gateway

Erstellen Sie zunächst im Portal wie üblich eine neue Application Gateway-Instanz. Bei der Erstellung sind keine zusätzlichen Schritte erforderlich, um die gegenseitige Authentifizierung zu aktivieren. Weitere Informationen zur Erstellung einer Application Gateway-Instanz im Portal finden Sie in unserer [Schnellstartanleitung für die Erstellung im Portal](./quick-create-portal.md).

## <a name="configure-mutual-authentication"></a>Konfigurieren der gegenseitigen Authentifizierung 

Um eine vorhandene Application Gateway-Instanz mit gegenseitiger Authentifizierung zu konfigurieren, müssen Sie zunächst im Portal zur Registerkarte **SSL-Einstellungen (Vorschau)** navigieren und ein neues SSL-Profil erstellen. Wenn Sie ein SSL-Profil erstellen, werden zwei Registerkarten angezeigt: **Clientauthentifizierung** und **SSL-Richtlinie**. Auf der Registerkarte **Clientauthentifizierung** laden Sie Ihre Clientzertifikate hoch. Die Registerkarte **SSL-Richtlinie** dient zum Konfigurieren einer listenerspezifischen SSL-Richtlinie. Weitere Informationen finden Sie im Artikel zum [Konfigurieren einer listenerspezifischen SSL-Richtlinie](./application-gateway-configure-listener-specific-ssl-policy.md).

> [!IMPORTANT]
> Achten Sie darauf, dass Sie die gesamte ZS-Clientzertifikatkette in einer Datei und nur eine Kette pro Datei hochladen.

1. Suchen Sie im Portal nach **Application Gateway**, wählen Sie die Option **Anwendungsgateways** aus, und klicken Sie auf Ihre vorhandene Application Gateway-Instanz.

2. Wählen Sie im Menü auf der linken Seite die Option **SSL-Einstellungen (Vorschau)** aus.

3. Klicken Sie oben auf das Pluszeichen neben **SSL-Profile**, um ein neues SSL-Profil zu erstellen.

4. Geben Sie unter **SSL Profile Name** (Name des SSL-Profils) einen Namen ein. In diesem Beispiel geben wir dem SSL-Profil den Namen *applicationGatewaySSLProfile*. 

5. Bleiben Sie auf der Registerkarte **Clientauthentifizierung**. Laden Sie das PEM-Zertifikat, das Sie für die gegenseitige Authentifizierung zwischen dem Client und der Application Gateway-Instanz verwenden möchten, mit der Schaltfläche **Neues Zertifikat hochladen** hoch. 

    Weitere Informationen zum Extrahieren von vertrauenswürdigen ZS-Clientzertifikatketten für den Upload finden Sie unter [Exportieren einer vertrauenswürdigen ZS-Clientzertifikatkette für die Clientauthentifizierung](./mutual-authentication-certificate-management.md).

   > [!NOTE]
   > Falls dies nicht Ihr erstes SSL-Profil ist und Sie bereits andere Clientzertifikate auf Ihre Application Gateway-Instanz hochgeladen haben, können Sie über das Dropdownmenü ein vorhandenes Zertifikat für die Wiederverwendung auf Ihrem Gateway auswählen. 

6. Aktivieren Sie das Kontrollkästchen **DN des Clientzertifikatausstellers verifizieren** nur, falls über die Application Gateway-Instanz der Distinguished Name des direkten Clientzertifikatausstellers verifiziert werden soll. 

7. Erwägen Sie, eine listenerspezifische Richtlinie hinzuzufügen. Sehen Sie sich die Anleitung zum [Einrichten von listenerspezifischen SSL-Richtlinien](./application-gateway-configure-listener-specific-ssl-policy.md) an.

8. Wählen Sie die Option **Hinzufügen** aus, um die Änderungen zu speichern.
    > [!div class="mx-imgBorder"]
    > ![Hinzufügen einer Clientauthentifizierung zum SSL-Profil](./media/mutual-authentication-portal/mutual-authentication-portal.png)

## <a name="associate-the-ssl-profile-with-a-listener"></a>Zuordnen eines Listeners zum SSL-Profil

Nachdem wir nun ein SSL-Profil mit konfigurierter gegenseitiger Authentifizierung erstellt haben, müssen wir das SSL-Profil dem Listener zuordnen, um die Einrichtung der gegenseitigen Authentifizierung abzuschließen. 

1. Navigieren Sie zu Ihrer vorhandenen Application Gateway-Instanz. Nachdem Sie die obigen Schritte abgeschlossen haben, müssen Sie hier keine weiteren Schritte ausführen. 

2. Wählen Sie im linken Menü **Listener** aus. 

3. Klicken Sie auf **Listener hinzufügen**, falls Sie noch keinen HTTPS-Listener eingerichtet haben. Wenn Sie bereits über einen HTTPS-Listener verfügen, können Sie in der Liste darauf klicken. 

4. Füllen Sie die Felder **Listenername**, **Front-End-IP-Adresse**, **Port**, **Protokoll** und **HTTPS-Einstellungen** gemäß Ihren Anforderungen aus.

5. Aktivieren Sie das Kontrollkästchen **SSL-Profil aktivieren**, damit Sie auswählen können, welches SSL-Profil dem Listener zugeordnet werden soll. 

6. Wählen Sie das soeben erstellte SSL-Profil in der Dropdownliste aus. In diesem Beispiel wählen wir das SSL-Profil aus, das wir in den obigen Schritten erstellt haben: *applicationGatewaySSLProfile*. 

7. Konfigurieren Sie die restlichen Einstellungen für den Listener gemäß Ihren Anforderungen. 

8. Klicken Sie auf **Hinzufügen**, um Ihren neuen Listener mit dem zugeordneten SSL-Profil zu speichern. 

    > [!div class="mx-imgBorder"]
    > ![Zuordnen des SSL-Profils zum neuen Listener](./media/mutual-authentication-portal/mutual-authentication-listener-portal.png)

## <a name="renew-expired-client-ca-certificates"></a>Verlängern abgelaufener ZS-Clientzertifikate

Falls Ihr ZS-Clientzertifikat abgelaufen ist, können Sie das Zertifikat auf Ihrem Gateway mit den folgenden Schritten aktualisieren: 

1. Navigieren Sie zu Ihrer Application Gateway-Instanz und dann im linken Menü auf die Registerkarte **SSL-Einstellungen (Vorschau)** . 
 
1. Wählen Sie das bzw. die vorhandenen SSL-Profile mit dem abgelaufenen Clientzertifikat aus. 
 
1. Wählen Sie auf der Registerkarte **Clientauthentifizierung** die Option **Neues Zertifikat hochladen** aus, und laden Sie Ihr neues Clientzertifikat hoch. 
 
1. Wählen Sie neben dem abgelaufenen Zertifikat das Papierkorbsymbol aus. Hierdurch wird die Zuordnung dieses Zertifikats aus dem SSL-Profil entfernt. 

1. Wiederholen Sie die obigen Schritte 2 bis 4 für alle anderen SSL-Profile, für die dasselbe abgelaufene Clientzertifikat verwendet wurde. Sie können das neue Zertifikat, das Sie in Schritt 3 hochgeladen haben, in den anderen SSL-Profilen dann im Dropdownmenü auswählen.

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Webdatenverkehr mit einem Anwendungsgateway per Azure CLI](./tutorial-manage-web-traffic-cli.md)