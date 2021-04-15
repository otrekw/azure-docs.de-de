---
title: Tutorial - Konfigurieren Sie Ihr Azure Active Directory so, dass es überprüfbare Anmeldedaten ausgibt (Vorschau)
description: In diesem Lernprogramm bauen Sie die Umgebung auf, die zur Bereitstellung von überprüfbaren Berechtigungsnachweisen in Ihrem Mandant erforderlich ist
documentationCenter: ''
author: barclayn
manager: daveba
ms.service: identity
ms.topic: tutorial
ms.subservice: verifiable-credentials
ms.date: 04/01/2021
ms.author: barclayn
ms.reviewer: ''
ms.openlocfilehash: cd39f6c484ebe116918611bb1d543c1919a3cb0a
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106222944"
---
# <a name="tutorial---configure-your-azure-active-directory-to-issue-verifiable-credentials-preview"></a>Tutorial - Konfigurieren Sie Ihr Azure Active Directory so, dass es überprüfbare Anmeldedaten ausgibt (Vorschau)

In diesem Tutorial bauen wir auf der Arbeit, die im Rahmen des Artikels [Erste Schritte](get-started-verifiable-credentials.md) ausgeführt wurde, und richten ihre Azure Active Directory (Azure AD) mit einem eigenen [dezentralen Bezeichner](https://www.microsoft.com/security/business/identity-access-management/decentralized-identity-blockchain?rtc=1#:~:text=Decentralized%20identity%20is%20a%20trust,protect%20privacy%20and%20secure%20transactions.) (DID) ein. Wir verwenden den dezentralen Bezeichner, um mit der Beispiel-App und Ihrem Zertifikataussteller überprüfbare Anmeldeinformationen auszustellen; in diesem Tutorial benutzen wir jedoch weiterhin den Beispiel-Azure-B2C-Mandanten zur Authentifizierung.  In unserem nächsten Tutorial werden wir weitere Schritte durchführen, um die App so zu konfigurieren, dass sie mit Ihrem Azure AD funktioniert.

> [!IMPORTANT]
> Azure Active Directory überprüfbare Anmeldedaten ist derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Artikel:

> [!div class="checklist"]
> * Sie erstellen die notwendigen Dienste, um Ihre Azure AD für überprüfbare Anmeldeinformationen einzubinden 
> * Ihre DID wird erstellt
> * Die Regeln und Anzeige Dateien werden angepasst
> * Konfigurieren Sie die überprüfbaren Anmeldeinformationen in Azure AD.


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie dieses Tutorial erfolgreich abschließen können, müssen Sie zuerst:

- Vervollständigen Sie [Erste Schritte](get-started-verifiable-credentials.md).
- Sie benötigen ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Azure AD mit einer P2- [Lizenz](https://azure.microsoft.com/pricing/details/active-directory/). Befolgen Sie unter [Erstellen eines kostenlosen Entwickler Kontos](how-to-create-a-free-developer-account.md) Wenn Sie nicht über eines verfügen.
- Eine Instanz von [Azure Key Vault](../../key-vault/general/overview.md), in der Sie über das Recht zum Erstellen von Schlüsseln und Geheimnissen verfügen.

## <a name="azure-active-directory"></a>Azure Active Directory

Bevor wir beginnen können, benötigen wir einen Azure AD Mandanten. Wenn Ihr Mandant für überprüfbare Anmeldeinformationen aktiviert ist, wird ihm ein dezentralisierter Bezeichner (DID) zugewiesen, und er wird mit einem Aussteller Dienst zum Ausstellen überprüfbarer Anmelde Informationen aktiviert. Alle überprüfbaren Anmeldeinformationen, die Sie ausgeben, werden von Ihrem Mandanten und seiner DID ausgegeben. Der DID wird auch verwendet, wenn überprüfbare Anmeldeinformationen überprüft werden.
Wenn Sie gerade ein Test-Azure-Abonnement erstellt haben, muss Ihr Mandant nicht mit Benutzerkonten gefüllt werden, aber Sie benötigen mindestens ein Benutzer Testkonto, um spätere Tutorials zu vervollständigen.

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Wenn Sie mit überprüfbaren Anmeldeinformationen arbeiten, haben Sie die komplette Kontrolle und Verwaltung der kryptografischen Schlüssel, die Ihr Mandant zum digitalen Signieren überprüfbarer Anmelde Informationen verwendet. Zum Ausstellen und Überprüfen von Anmelde Informationen müssen Sie Azure AD mit Zugriff auf Ihre eigene Instanz von Azure Key Vault bereitstellen.

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.
2. Geben Sie **Key Vault** in das Suchfeld ein.
3. Wählen Sie in der Ergebnisliste **Key Vault** aus.
4. Klicken Sie im Abschnitt „Key Vault“ auf **Erstellen**.
5. Geben Sie im Abschnitt **Schlüsseltresor erstellen** folgende Informationen ein:
    - **Abonnement**: Wählen Sie ein Abonnement aus.
    - Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie einen Ressourcengruppennamen wie beispielsweise **vc-resource-group** ein. Wir verwenden denselben Ressourcengruppen Namen in mehreren Artikeln.
    - **Name**: Es ist ein eindeutiger Name erforderlich. Wir verwenden " **woodgroup-VC-KV** ", um diesen Wert durch ihren eigenen eindeutigen Namen zu ersetzen.
    - Wählen Sie im Pulldownmenü **Speicherort** einen Speicherort aus.
    - Behalten Sie bei den anderen Optionen die Standardeinstellungen bei.
6. Klicken Sie nach der Angabe der obigen Informationen auf **Zugriffsrichtlinie**

    ![Erstellen Sie eine Key Vault-Seite.](media/enable-your-tenant-verifiable-credentials/create-key-vault.png)

7. Wählen Sie im Bildschirm Zugriffsrichtlinie die Option **Zugriffsrichtlinie hinzufügen** aus

    >[!NOTE]
    > Standardmäßig ist das Konto, mit dem der Key Vault erstellt wird, der einzige mit Access. Der überprüfbare Anmeldeinformationsdienst benötigt Zugriff auf Key Vault. Der Key Vault muss über eine Zugriffsrichtlinie verfügen, die es dem Administrator ermöglicht,**Schlüssel zu erstellen**,**Schlüssel zu löschen**,wenn Sie sich abmelden, und **anzumelden**, um die Dämonen Bindung für überprüfbare Anmeldeinformationen zu erstellen. Wenn Sie beim Testen dasselbe Konto verwenden, stellen Sie sicher, dass Sie die Standardrichtlinie ändern, um die Konto **Anmeldung** zusätzlich zu den Standard Berechtigungen zu erteilen, die für Vault-Ersteller erteilt wurden.

8. Stellen Sie für den Benutzer Administrator sicher, dass im Abschnitt Schlüsselberechtigungen die Berechtigungen **Erstellen**, **Löschen** und **anmelden** aktiviert sind. Standardmäßig sind Erstellen und Löschen bereits aktiviert, und Anmelden sollte die einzige Key Vault-Berechtigung sein, die aktualisiert werden muss. 

    ![Key Vault-Berechtigungen](media/enable-your-tenant-verifiable-credentials/keyvault-access.png)

9. Klicken Sie auf **Überprüfen + erstellen**.
10. Klicken Sie auf **Erstellen**.
11. Gehen Sie zum Tresor, und notieren Sie sich den Tresornamen und den URI

Beachten Sie die beiden folgenden Eigenschaften:

- **Tresorname**: im Beispiel lautet der Value-Name **Woodgrove-VC-KV**. Sie verwenden diesen Namen noch für andere Schritte.
- **Tresor-URI**: in diesem Beispiel, die Value ist https://woodgrove-vc-kv.vault.azure.net/. Anwendungen, die Ihren Tresor über die zugehörige REST-API nutzen, müssen diesen URI verwenden.

>[!NOTE]
> Jede Key Vault-Transaktion führt zu zusätzlichen Kosten für Azure-Abonnements. Weitere Informationen finden Sie auf der [Seite Key Vault-Preis](https://azure.microsoft.com/pricing/details/key-vault/).

>[!IMPORTANT]
> Während der Vorschau der Azure Active Directory überprüfbaren Anmeldeinformationen sollten die im Tresor erstellten Schlüssel und Geheimnisse nicht geändert werden, nachdem Sie erstellt wurden. Durch das Löschen, deaktivieren oder Aktualisieren Ihrer Schlüssel und Geheimnisse werden alle ausgestellten Anmeldeinformationen ungültig. Ändern Sie Ihre Schlüssel oder Geheimnisse während der Vorschau nicht.

## <a name="create-a-modified-rules-and-display-file"></a>Geänderte Regeln und Anzeige Datei erstellen

In diesem Abschnitt verwenden wir die Regeln und Anzeige Dateien aus der Beispiel Aussteller-APP und ändern Sie leicht, um die ersten überprüfbaren Anmeldeinformationen Ihres Mandanten zu erstellen.

1. Kopieren Sie beide  Regeln, und zeigen sie JSON-Dateien in einem temporären Ordner an, und benennen Sie sie **MyFirstVC-display.json** bzw. **MyFirstVC-rules.json**. Beide Dateien finden Sie unter **Issuer \ issuer_config**

   ![Anzeigen von und Regel Dateien als Teil des Beispiel-App-Verzeichnisses](media/enable-your-tenant-verifiable-credentials/sample-app-rules-display.png)

   ![Anzeigen von und Regel Dateien in einem temporären Ordner](media/enable-your-tenant-verifiable-credentials/display-rules-files-temp.png)

2. Öffnen Sie die Datei MyFirstVC-rules.json in Ihrem Code-Editor. 

    ```json
         {
          "attestations": {
            "idTokens": [
              {
                "mapping": {
                  "firstName": { "claim": "given_name" },
                  "lastName": { "claim": "family_name" }
                },
                "configuration": "https://didplayground.b2clogin.com/didplayground.onmicrosoft.com/B2C_1_sisu/v2.0/.well-known/openid-configuration",
                "client_id": "8d5b446e-22b2-4e01-bb2e-9070f6b20c90",
                "redirect_uri": "vcclient://openid/"
              }
            ]
          },
          "validityInterval": 2592000,
          "vc": {
            "type": ["VerifiedCredentialExpert"]
          }
        }
      
    ```

Nun ändern wir das Typfeld in "myfirstvc". 

  ```json
   "type": ["MyFirstVC"]
  
  ```

Diese Änderung speichern.

 >[!NOTE]
   > An dieser Stelle im Tutorial ändern wir die **"Konfiguration"** oder **"client_id"** nicht. Wir verwenden weiterhin den Microsoft B2C-Mandanten, den wir in den[ Ersten Schritten ](get-started-verifiable-credentials.md) verwendet haben. Wir verwenden Ihre Azure AD im nächsten Tutorial.

3. Öffnen Sie die Datei MyFirstVC-rules.json in Ihrem Code-Editor.

   ```json
       {
          "default": {
           "locale": "en-US",
           "card": {
             "title": "Verified Credential Expert",
             "issuedBy": "Microsoft",
             "backgroundColor": "#000000",
             "textColor": "#ffffff",
             "logo": {
               "uri": "https://didcustomerplayground.blob.core.windows.net/public/VerifiedCredentialExpert_icon.png",
               "description": "Verified Credential Expert Logo"
             },
             "description": "Use your verified credential to prove to anyone that you know all about verifiable credentials."
           },
           "consent": {
             "title": "Do you want to get your Verified Credential?",
             "instructions": "Sign in with your account to get your card."
           },
           "claims": {
             "vc.credentialSubject.firstName": {
               "type": "String",
               "label": "First name"
             },
             "vc.credentialSubject.lastName": {
               "type": "String",
               "label": "Last name"
             }
           }
         }
      }
   ```

Wir nehmen einige Änderungen vor, sodass sich diese überprüfbaren Anmeldeinformationen von der Version des Beispielcodes unterscheiden. 
    
```json
     "card": {
        "title": "My First VC",
        "issuedBy": "Your Issuer Name",
        "backgroundColor": "#ffffff",
        "textColor": "#000000",
```

Speichern der Änderungen.
## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Vor dem Erstellen der ersten überprüfbaren Anmeldeinformationen müssen wir einen BLOB Storage Container erstellen, der die Konfigurations-und Regel Dateien enthalten kann.

1. Erstellen Sie mithilfe der unten gezeigten Optionen ein Speicherkonto. Ausführliche Schritte finden Sie im Artikel [Erstellen eines Speicherkontos](../../storage/common/storage-account-create.md?tabs=azure-portal).

   - **Abonnement:** Wählen Sie das Abonnement aus, das wir für diese Tutorials verwenden.
   - **Ressourcengruppe:** Wählen Sie dieselbe Ressourcengruppe aus, die wir in früheren Tutorials verwendet haben (**VC-Resource-Group**).
   - **Name**: Einen eindeutigen Namen.
   - **Region**(USA) USA, Osten.
   - Leistung: **Standard**
   - **Kontoart:** Speicher v2.
   - **Replication:** Lokal redundante.
 
   ![Erstellen eines neuen Speicherkontos](media/enable-your-tenant-verifiable-credentials/create-storage-account.png)

2. Nachdem Sie das Speicherkonto erstellt haben, müssen wir einen Container erstellen. Wählen Sie unter **BLOB Storage** die Option **Container** aus, und erstellen Sie mithilfe der unten angegebenen Werte einen Container:

    - **Name:** VC-Container
    - **Öffentliche Zugriffsebene:** Privat (kein anonymer Zugriff)

   ![Erstellen eines Containers](media/enable-your-tenant-verifiable-credentials/new-container.png)

3. Wählen Sie nun den neuen Container **MyFirstVC-rules.js** aus, und laden Sie beide **MyFirstVC-display.js** "neue Regeln" und "Dateien anzeigen" ein, die Sie zuvor erstellt haben.

   ![Regeldatei hochladen](media/enable-your-tenant-verifiable-credentials/blob-storage-upload-rules-display-files.png)

## <a name="assign-blob-role"></a>BLOB-Rolle zuweisen

Vor dem Erstellen der Anmeldeinformationen müssen wir dem angemeldeten Benutzer zuerst die richtige Rollenzuweisung zuweisen, damit sie auf die Dateien im Speicherblob zugreifen können.

1. Navigieren Sie zu **Speicher**  >  **Container**.
2. Wählen Sie im linken Menü die Option **Zugriffssteuerung (IAM)** aus.
3. Wählen Sie **Rollenzuweisung**.
4. Wählen Sie **Hinzufügen**.
5. Wählen Sie im Abschnitt **Rolle** die Option **Speicher-BLOB-Daten Leser** aus.
6. Wählen Sie unter **Zugriff zuweisen zu** die Option **Benutzer, Gruppe oder Dienstprinzipal** aus.
7. Unter **Auswählen**: Wählen Sie das Konto aus, das Sie zum Ausführen dieser Schritte verwenden.
8. Wählen Sie **Speichern** aus, um den Vorgang der Rollenzuweisung abzuschließen.


   ![Hinzufügen einer Rollenzuweisung](media/enable-your-tenant-verifiable-credentials/role-assignment.png)

  >[!IMPORTANT]
  >Standardmäßig wird die Rolle " **Besitzer** " dem Container Ersteller zugewiesen. Die Rolle " **Besitzer** " ist allein nicht ausreichend. Ihr Konto benötigt die Rolle **Speicher-BLOB-Daten Leser**. Weitere Informationen finden Sie unter [Benutzen Sie das Azure-Portal zum Zuweisen einer Azure-Rolle für den Zugriff auf Blob- und Warteschlangendaten](../../storage/common/storage-auth-aad-rbac-portal.md).

## <a name="set-up-verifiable-credentials-preview"></a>Richten Sie die überprüfbare Anmeldeinformationen (Vorschau) ein.

Nun müssen wir den letzten Schritt zum Einrichten Ihres Mandanten für überprüfbare Anmeldeinformationen ausführen.

1. Suchen Sie im Azure-Portal nach **überprüfbaren Anmeldeinformationen**.
2. Wählen Sie **überprüfbare Anmeldeinformationen (Vorschau)** .
3. Wählen Sie **Erste Schritte** aus
4. Wir müssen Ihre Organisation einrichten und den Organisationsnamen, die Domäne und den Key Vault bereitstellen. Sehen wir uns jede dieser Werte genauer an.

      - **Organisationsname**: Dieser Name ist der Verweis auf Ihr Unternehmen innerhalb des überprüfbaren Anmeldeinformations dienstes. Dieser Wert ist nicht Kunden basiert.
      - **Domäne:** Die eingegebene Domäne wird einem Dienst Endpunkt in Ihrem DID-Dokument hinzugefügt. [Microsoft Authenticator](../user-help/user-help-auth-app-download-install.md) und andere Geldbörsen verwenden diese Informationen, um zu überprüfen, ob Ihre DID mit [Ihrer Domäne verknüpft](how-to-dnsbind.md)ist. Wenn das Wallet die DID überprüfen kann, wird ein verifiziertes Symbol angezeigt. Wenn das Wallet die DID nicht überprüfen kann, wird der Benutzer darüber informiert, dass die Anmeldeinformationen von einer Organisation ausgestellt wurden, die Sie nicht überprüfen konnte. Die Domäne bindet Ihre DID an etwas konkretes, das der Benutzer möglicherweise über Ihr Unternehmen kennt.
      - **Key Vault:** Geben Sie den Namen des Key Vault an, den wir zuvor erstellt haben.
 
   >[!IMPORTANT]
   > Die Domäne kann keine Umleitung sein; andernfalls können die DID-und die-Domäne nicht verknüpft werden. Stellen Sie sicher, dass Sie das https://www.domain.com Format verwenden.
    
5. Wählen Sie **speichern, und erstellen** aus.

    ![Einrichten Ihrer Organisations Identität](media/enable-your-tenant-verifiable-credentials/save-create.png)

Herzlichen Glückwunsch, Ihr Mandant ist jetzt für die überprüfbare Anmeldeinformationen-Vorschau aktiviert!

## <a name="create-your-vc-in-the-portal"></a>Erstellen Ihres VC im Portal

Im vorherigen Schritt haben Sie die Seite **Erstellen von neuen Anmeldeinformationen**  erstellt. 

   ![Überprüfbare Anmeldeinformationen starten](media/enable-your-tenant-verifiable-credentials/create-credential-after-enable-did.png)

1. Fügen Sie unter Anmeldeinformationen den Namen **myfirstvc** hinzu. Dieser Name wird im Portal verwendet, um Ihre überprüfbaren Anmeldeinformationen zu identifizieren, und er ist als Teil des überprüfbare Anmeldeinformationen-Vertrags enthalten.
2. Wählen Sie im Abschnitt Anzeige Datei die Option **Anzeige Datei konfigurieren** aus
3. Wählen Sie im Abschnitt " **Speicherkonten** " die Option **woodgrovevcstorage** aus.
4. Wählen Sie in der Liste der verfügbaren Container **VC-Container** aus.
5. Wählen Sie die **MyFirstVC-display.js Datei** aus, die Sie zuvor erstellt haben.
6. Wählen Sie im Abschnitt **Neue Anmeldeinformationen erstellen** in der **Regel Datei** die Option **Regel Datei konfigurieren**
7. Wählen Sie in den **Speicherkonten** die Option **woodgrovevcstorage** aus
8. Wählen Sie **VC-Container** aus.
9. Wählen Sie **MyFirstVC-rules.json** aus
10. Wählen Sie aus **neue Anmeldeinformationen erstellen** die Option **Erstellen** aus.

   ![Erstellen von neuen Anmeldeinformationen](media/enable-your-tenant-verifiable-credentials/create-my-first-vc.png)

### <a name="credential-url"></a>Anmeldeinformation-URL

Nachdem Sie nun über neue Anmeldeinformationen verfügen, kopieren Sie die Anmeldeinformationen-URL.

   ![Die Ausgabe der Anmeldeinformationen-URL](media/enable-your-tenant-verifiable-credentials/issue-credential-url.png)

>[!NOTE]
>Die Anmeldeinformationen-URL ist die Kombination der Regeln und Anzeige Dateien. Dies ist die URL, die vom Authenticator ausgewertet wird, bevor den Benutzer überprüfbare Anmeldeinformationen Ausgabeanforderungen angezeigt werden.  

## <a name="update-the-sample-app"></a>Aktualisieren der Beispiel-App

Nun nehmen wir Änderungen am Issuer-Code der Beispiel-App vor, um ihn mit Ihrer überprüfbaren Anmeldeinformationen-URL zu aktualisieren. Dies ermöglicht es Ihnen, überprüfbare Anmeldeinformationen mit Ihrem eigenen Mandanten auszugeben.

1. Gehen Sie zu dem Ordner, in den Sie die Dateien der Beispiel-App eingefügt haben.
2. Öffnen Sie den Issuer-Ordner, und öffnen Sie dann app.js in Visual Studio Code.
3. Aktualisieren Sie die Konstante "Anmeldeinformationen" mit ihrer neuen Anmeldeinformationen-URL, und setzen Sie die Konstante Anmeldeinformationsart auf 'MyFirstVC' und speichern Sie die Datei.

    ![Abbildung von Visual Studio Code mit hervorgehobenen relevanten Bereichen](media/enable-your-tenant-verifiable-credentials/sample-app-vscode.png)

4. Öffnen Sie eine Eingabeaufforderung und navigieren Sie zum Ordner .
5. Führen Sie das aktualisierte Skript aus.

    ```terminal
    node app.js
    ```

6. Führen Sie mit einer anderen Eingabeaufforderung ngrok aus, um eine URL auf 8081 einzurichten

    ```terminal
    ngrok http 8081
    ```
    
    >[!IMPORTANT]
    > Möglicherweise bemerken Sie auch eine Warnung, dass diese APP oder Website riskant sein kann. Die Meldung wird zu diesem Zeitpunkt erwartet, da Ihr DID noch nicht mit Ihrer Domäne verknüpft wurde. Befolgen Sie die Anweisungen der [DNS-Bindung](how-to-dnsbind.md), um dies zu konfigurieren.

    
7. Öffnen Sie die HTTPS-URL, die von ngrok generiert wird.

    ![Ngrok-Weiterleitungs Endpunkte](media/enable-your-tenant-verifiable-credentials/ngrok-url-screen.png)

8. Wählen Sie  **ANMELDEINFORMATIONEN ERHALTEN**
9. Scannen Sie in den Authentifikator den QR-Code.
10. Bei **dieser APP oder Website ist möglicherweise eine riskant** Warnmeldung, wählen Sie **erweitert** aus.

  ![Anfängliche Warnung](media/enable-your-tenant-verifiable-credentials/site-warning.png)

11. Wählen Sie unter der Warnung riskante Website die Option **trotzdem fortsetzen (unsicher)**

  ![Zweite Warnung zum Aussteller](media/enable-your-tenant-verifiable-credentials/site-warning-proceed.png)


12. Beachten Sie auf dem Bildschirm **Anmeldeinformationen Hinzufügen** einige Dinge: 
    1. Am oberen Bildschirmrand sehen Sie eine rote **nicht verifizierte** Meldung
    1. Die Anmeldeinformationen werden auf der Grundlage der Änderungen angepasst, die wir an der Anzeige Datei vorgenommen haben.
    1. Die Option **Anmeldung zu Ihrem Konto** verweist auf **didplayground.b2clogin.com**.
    
   ![Bildschirm-Anmeldeinformationen mit Warnung hinzufügen](media/enable-your-tenant-verifiable-credentials/add-credential-not-verified.png)

13. Wählen Sie **Anmeldung zu Ihrem Konto aus,** und Authentifizieren Sie sich mit den Anmeldeinformationen, die Sie im [Tutorial Erste Schritte](get-started-verifiable-credentials.md) angegeben haben.
14. Nach der erfolgreichen Authentifizierung ist die Schaltfläche **Hinzufügen** nicht mehr abgeblendet. Wählen Sie **Hinzufügen**.

  ![Bildschirm-Anmeldeinformationen nach der Authentifizierung hinzufügen](media/enable-your-tenant-verifiable-credentials/add-credential-not-verified-authenticated.png)

Wir haben jetzt überprüfbare Anmeldeinformationen mit unserem Mandanten zum Generieren unserer VC ausgestellt, während wir weiterhin unseren B2C-Mandanten für die Authentifizierung verwenden.

  ![VC ausgestellt von Ihrer Azure AD und authentifiziert von unserer Azure B2C-Instanz](media/enable-your-tenant-verifiable-credentials/my-vc-b2c.png)


## <a name="test-verifying-the-vc-using-the-sample-app"></a>Testen der VC-Überprüfung mithilfe der Beispiel-App

Nachdem wir nun die überprüfbaren Anmeldeinformationen aus unserem eigenen Mandanten ausgegeben haben, überprüfen wir sie mit unserer Beispiel-App.

>[!IMPORTANT]
> Verwenden Sie beim Testen dieselbe e-Mail-Adresse und dasselbe Kennwort wie während des [Erste Schritte](get-started-verifiable-credentials.md) Artikel. Während Ihr Mandant den VC ausgibt, stammt die Eingabe von einem ID-Token, das vom Microsoft B2C-Mandanten ausgegeben wurde. In Tutorial 2 wird die Tokenausstellung auf ihren Mandanten umgestellt

1. Öffnen Sie die **Einstellungen** auf dem Blatt überprüfbare Anmeldeinformationen im Azure-Portal. Kopieren Sie den dezentralisierten Bezeichner (DID).

   ![Kopieren Sie den DID](media/enable-your-tenant-verifiable-credentials/issuer-identifier.png)

2. Öffnen Sie jetzt den Überprüfungsordner Teil der Beispiel-APP-Dateien. Wir müssen die app.js Datei im Verifizierungs-Beispielcode aktualisieren und die folgenden Änderungen vornehmen:

    - **Anmeldeinformation**: Wechseln Sie zu Ihrer Anmeldeinformationen-URL
    - **Anmeldeinformationsart**: 'MyFirstVC'
    - **issuerDid**: Kopieren Sie diesen Wert aus dem Azure-Portal>Überprüfbare Anmeldeinformationen (Vorschau)>Einstellungen>Dezentralisierter Bezeichner (DID)
    
   ![Aktualisieren Sie die Konstante issuerDid, damit sie mit Ihrer Emittentenkennung übereinstimmt](media/enable-your-tenant-verifiable-credentials/editing-verifier.png)

3. Starten Sie den ngrok-Dienst für den Aussteller nicht mehr.

    ```cmd
    control-c
    ```

4. Führen Sie nun ngrok mit dem Überprüfungs-Port 8082 aus.

    ```cmd
    ngrok http 8082
    ```

5. Navigieren Sie in einem anderen Terminalfenster zur Verifizierzungs-App, und führen Sie es ähnlich wie wir die Aussteller-App ausgeführt haben.

    ```cmd
    cd ..
    cd verifier
    node app.js
    ```

6. Öffnen Sie die ngrok-URL in Ihrem Browser, und Scannen Sie den QR-Code mithilfe des Authentifikators auf Ihrem mobilen Gerät.
7. Wählen Sie auf Ihrem mobilen Gerät auf dem Bildschirm die **neue Berechtigungsanforderung** Option **zulassen** aus.

    >[!NOTE]
    > Der DID der diese VC zeichnet ist weiterhin Microsoft B2C. Der Verifier ist noch immer vom Mandanten der Microsoft-Beispiel-App. Da Microsoft´s DID mit einer Domäne verknüpft ist, die wir besitzen, sehen Sie die Warnung nicht wie bei der Ausgabe des Ausstellungsflusses. Dies wird im nächsten Abschnitt aktualisiert.
    
   ![Neue Berechtigungsanforderung](media/enable-your-tenant-verifiable-credentials/new-permission-request.png)

8. Sie haben ihre Anmeldeinformationen nicht erfolgreich überprüft.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun über den Beispielcode verfügen, der einen VC von Ihrem Aussteller ausgibt, können Sie mit dem nächsten Abschnitt fortfahren, in dem Sie Ihren eigenen Identitäts Anbieter zum Authentifizieren von Benutzern verwenden, die versuchen, überprüfbare Anmeldeinformationen zu erhalten.

> [!div class="nextstepaction"]
> [Tutorial: Ausstellen und Überprüfen überprüfbarer Anmeldeinformationen mithilfe Ihres Mandanten](issue-verify-verifiable-credentials-your-tenant.md)


