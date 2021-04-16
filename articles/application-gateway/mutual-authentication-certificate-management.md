---
title: Exportieren einer vertrauenswürdigen ZS-Clientzertifikatkette für die Clientauthentifizierung
titleSuffix: Azure Application Gateway
description: Es wird beschrieben, wie Sie eine vertrauenswürdige ZS-Clientzertifikatkette für die Clientauthentifizierung unter Azure Application Gateway exportieren.
services: application-gateway
author: mscatyao
ms.service: application-gateway
ms.topic: how-to
ms.date: 03/31/2021
ms.author: caya
ms.openlocfilehash: 2329dc7426b223ef2c81dd0e2e607bccf73192e6
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106230883"
---
# <a name="export-a-trusted-client-ca-certificate-chain-to-use-with-client-authentication"></a>Exportieren einer vertrauenswürdigen ZS-Clientzertifikatkette für die Clientauthentifizierung
Zum Konfigurieren der gegenseitigen Authentifizierung für den Client oder der Clientauthentifizierung muss für Application Gateway eine vertrauenswürdige ZS-Clientzertifikatkette auf das Gateway hochgeladen werden. Falls Sie über mehrere Zertifikatketten verfügen, müssen Sie die Ketten separat erstellen und als einzelne Dateien auf die Application Gateway-Instanz hochladen. In diesem Artikel wird beschrieben, wie Sie eine vertrauenswürdige ZS-Clientzertifikatkette exportieren, die Sie für die Konfiguration der Clientauthentifizierung auf Ihrem Gateway verwenden können.  

## <a name="prerequisites"></a>Voraussetzungen

Es muss ein Clientzertifikat vorhanden sein, um die vertrauenswürdige ZS-Clientzertifikatkette generieren zu können. 

## <a name="export-trusted-client-ca-certificate"></a>Exportieren eines vertrauenswürdigen ZS-Clientzertifikats

Ein vertrauenswürdiges ZS-Clientzertifikat wird benötigt, um die Clientauthentifizierung auf der Application Gateway-Instanz zu ermöglichen. In diesem Beispiel verwenden wir ein TLS/SSL-Zertifikat für das Clientzertifikat, exportieren den zugehörigen öffentlichen Schlüssel und exportieren dann die ZS-Zertifikate aus dem öffentlichen Schlüssel, um die vertrauenswürdigen ZS-Clientzertifikate zu erhalten. Anschließend verketten wir alle ZS-Clientzertifikate zu einer vertrauenswürdigen ZS-Clientzertifikatkette. 

Mit den folgenden Schritten können Sie die PEM- oder CER-Datei für Ihr Zertifikat exportieren:

### <a name="export-public-certificate"></a>Exportieren eines öffentlichen Zertifikats 

1. Öffnen Sie **Benutzerzertifikate verwalten**, um eine CER-Datei für das Zertifikat zu erhalten. Suchen Sie das Zertifikat (in der Regel befindet es sich unter „Certificates > Aktueller Benutzer\Personal\Certificates“), und klicken Sie mit der rechten Maustaste darauf. Klicken Sie auf **Alle Aufgaben** und anschließend auf **Exportieren**. Dadurch wird der **Zertifikatexport-Assistent** geöffnet. Wenn Sie das Zertifikat unter „Aktueller Benutzer\Eigene Zertifikate\Zertifikate“ nicht finden, haben Sie unter Umständen versehentlich „Zertifikate – Lokaler Benutzer“ anstelle von „Zertifikate – Aktueller Benutzer“ geöffnet. Wenn Sie den Zertifikat-Manager im Bereich für den aktuellen Benutzer mithilfe von PowerShell öffnen möchten, geben Sie *certmgr* ins Konsolenfenster ein.

    > [!div class="mx-imgBorder"]
    > ![Screenshot des Zertifikat-Managers, in dem „Zertifikate“ und in einem Kontextmenü „Alle Aufgaben“ und „Exportieren“ ausgewählt sind](./media/certificates-for-backend-authentication/export.png)

2. Klicken Sie im Assistenten auf **Weiter**.
    > [!div class="mx-imgBorder"]
    > ![Exportieren eines Zertifikats](./media/certificates-for-backend-authentication/exportwizard.png)

3. Wählen Sie **Nein, privaten Schlüssel nicht exportieren** aus, und klicken Sie dann auf **Weiter**.
    > [!div class="mx-imgBorder"]
    > ![Nein, privaten Schlüssel nicht exportieren](./media/certificates-for-backend-authentication/notprivatekey.png)

4. Wählen Sie auf der Seite **Dateiformat für den Export** die Option **Base-64-codiert X.509 (.CER)** aus, und klicken Sie dann auf **Weiter**.
    > [!div class="mx-imgBorder"]
    > ![Base64-codiert](./media/certificates-for-backend-authentication/base64.png)

5. Wählen Sie unter **Zu exportierende Datei** die Option **Durchsuchen** aus, um zu dem Speicherort zu wechseln, an den das Zertifikat exportiert werden soll. Geben Sie unter **Dateiname** einen Namen für die Zertifikatdatei ein. Klicken Sie auf **Weiter**.

    > [!div class="mx-imgBorder"]
   > ![Screenshot des Zertifikatexport-Assistenten, in dem Sie eine Datei für den Export angeben können](./media/certificates-for-backend-authentication/browse.png)

6. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren.

    > [!div class="mx-imgBorder"]
    > ![Screenshot des Zertifikatexport-Assistenten, nachdem Sie den Dateiexport abgeschlossen haben](./media/certificates-for-backend-authentication/finish.png)

7. Das Zertifikat wurde erfolgreich exportiert.

    > [!div class="mx-imgBorder"]
    > ![Screenshot einer Meldung über den Erfolg im Zertifikatexport-Assistenten](./media/certificates-for-backend-authentication/success.png)

   Das exportierte Zertifikat sieht in etwa wie folgt aus:

    > [!div class="mx-imgBorder"]
    > ![Screenshot eines Zertifikatsymbols](./media/certificates-for-backend-authentication/exported.png)

### <a name="export-ca-certificates-from-the-public-certificate"></a>Exportieren von ZS-Zertifikaten aus dem öffentlichen Zertifikat

Nachdem Sie Ihr öffentliches Zertifikat nun exportiert haben, exportieren Sie das bzw. die ZS-Zertifikate aus Ihrem öffentlichen Zertifikat. Falls Sie nicht mehr als eine Stammzertifizierungsstelle nutzen, müssen Sie nur das entsprechende zugehörige Zertifikat exportieren. Wenn Sie aber über mehr als eine Zwischenzertifizierungsstelle verfügen, müssen Sie auch dafür jeweils den Exportschritt ausführen. 

1. Öffnen Sie die Datei, wenn der öffentliche Schlüssel exportiert wurde.

    > [!div class="mx-imgBorder"]
    > ![Authentifizierungszertifikat öffnen](./media/certificates-for-backend-authentication/openAuthcert.png)

    > [!div class="mx-imgBorder"]
    > ![Zertifikatinformationen](./media/mutual-authentication-certificate-management/general.png)

1. Wählen Sie die Registerkarte „Zertifizierungspfad“ aus, um die Zertifizierungsstelle anzuzeigen.

    > [!div class="mx-imgBorder"]
    > ![Zertifikatdetails](./media/mutual-authentication-certificate-management/cert-details.png) 

1. Wählen Sie das Sicherheitszertifikat aus, und klicken Sie auf **Zertifikat anzeigen**.

    > [!div class="mx-imgBorder"]
    > ![Zertifikatpfad](./media/mutual-authentication-certificate-management/root-cert.png) 

   Die Details des Stammzertifikats sollten angezeigt werden.

    > [!div class="mx-imgBorder"]
    > ![Zertifikatinformationen](./media/mutual-authentication-certificate-management/root-cert-details.png)

1. Wählen Sie die Registerkarte **Details** aus, und klicken Sie auf **In Datei kopieren...** .

    > [!div class="mx-imgBorder"]
    > ![Stammzertifikat kopieren](./media/mutual-authentication-certificate-management/root-cert-copy-to-file.png)

1. Sie haben die Details des Zertifikats der Stammzertifizierungsstelle nun aus dem öffentlichen Zertifikat extrahiert. Der **Zertifikatexport-Assistent** wird angezeigt. Führen Sie die Schritte 2 bis 7 aus dem vorherigen Abschnitt ([Exportieren eines öffentlichen Zertifikats](./mutual-authentication-certificate-management.md#export-public-certificate)) aus, um den Zertifikatexport-Assistent abzuschließen. 

1. Wiederholen Sie nun die Schritte 2 bis 6 des aktuellen Abschnitts ([Exportieren von ZS-Zertifikaten aus dem öffentlichen Zertifikat](./mutual-authentication-certificate-management.md#export-ca-certificates-from-the-public-certificate)) für alle Zwischenzertifizierungsstellen, um alle Zertifikate der Zwischenzertifizierungsstellen im Base64-codierten X.509-Format (.CER) zu exportieren.

    > [!div class="mx-imgBorder"]
    > ![Zwischenzertifizierungsstelle](./media/mutual-authentication-certificate-management/intermediate-cert.png)

    Sie können die Schritte 2 bis 6 aus diesem Abschnitt beispielsweise für die Zwischenzertifizierungsstelle *MSIT CAZ2* wiederholen, um die Extraktion als eigenes Zertifikat durchzuführen. 

### <a name="concatenate-all-your-ca-certificates-into-one-file"></a>Verketten Ihrer gesamten Zertifizierungsstellenzertifikate in einer Datei

1. Führen Sie den folgenden Befehl mit allen ZS-Zertifikaten aus, die Sie zuvor extrahiert haben. 

    Windows:
    ```console
    type intermediateCA.cer rootCA.cer > combined.cer
    ```
    
    Linux:
    ```console
    cat intermediateCA.cer rootCA.cer >> combined.cer
    ```

    Das sich ergebende kombinierte Zertifikat sollte in etwa wie folgt aussehen:
    
    > [!div class="mx-imgBorder"]
    > ![Kombiniertes Zertifikat](./media/mutual-authentication-certificate-management/combined-cert.png)

## <a name="next-steps"></a>Nächste Schritte

Sie verfügen nun über die vertrauenswürdige ZS-Clientzertifikatkette. Diese können Sie der Konfiguration Ihrer Clientauthentifizierung auf der Application Gateway-Instanz hinzufügen, um die gegenseitige Authentifizierung für Ihr Gateway zu ermöglichen. Weitere Informationen finden Sie unter [Konfigurieren der gegenseitigen Authentifizierung in Application Gateway über das Portal (Vorschau)](./mutual-authentication-portal.md) oder [Konfigurieren der gegenseitigen Authentifizierung in Application Gateway mit PowerShell (Vorschau)](./mutual-authentication-powershell.md).

