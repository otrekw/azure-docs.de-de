---
title: Konfigurieren der gegenseitigen TLS-Authentifizierung
description: Erfahren Sie, wie Clientzertifikate mit TLS authentifiziert werden. Azure App Service kann das Clientzertifikat für den App-Code zur Überprüfung verfügbar machen.
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.topic: article
ms.date: 12/11/2020
ms.custom: devx-track-csharp, seodec18
ms.openlocfilehash: 6ceeb3d31652c04eb9a69c1c8bb4b114e6f38d52
ms.sourcegitcommit: fa807e40d729bf066b9b81c76a0e8c5b1c03b536
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "97347722"
---
# <a name="configure-tls-mutual-authentication-for-azure-app-service"></a>Konfigurieren der gegenseitigen TLS-Authentifizierung für Azure App Service

Sie können den Zugriff auf Ihre Azure App Service-App einschränken, indem Sie verschiedene Authentifizierungstypen für sie aktivieren. Eine Möglichkeit dafür ist die Anforderung eines Clientzertifikats, wenn die Clientanforderung über TLS/SSL erfolgt, und die Überprüfung des Zertifikats. Dieser Mechanismus wird als „gegenseitige TLS-Authentifizierung“ oder „Clientzertifikatauthentifizierung“ bezeichnet. In diesem Artikel wird gezeigt, wie Sie Ihre App für die Verwendung der Clientzertifikatauthentifizierung einrichten.

> [!NOTE]
> Wenn Sie über HTTP und nicht HTTPS auf Ihre Website zugreifen, erhalten Sie kein Clientzertifikat. Wenn Ihre Anwendung also Clientzertifikate erfordert, sollten Sie keine Anforderungen an Ihre Anwendung über HTTP zulassen.
>

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

## <a name="enable-client-certificates"></a>Aktivieren von Clientzertifikaten

So richten Sie Ihre App für das Erzwingen von Clientzertifikaten ein

1. Wählen Sie auf der Verwaltungsseite Ihrer App im linken Navigationsbereich **Konfiguration** > **Allgemeine Einstellungen** aus.

1. Legen Sie **Client certificate mode** (Clientzertifikatmodus) auf **Erforderlich** fest. Klicken Sie oben auf der Seite auf **Speichern**.

Wenn Sie den Vorgang an der Azure-Befehlszeilenschnittstelle ausführen möchten, führen Sie den folgenden Befehl in der [Cloud Shell](https://shell.azure.com) aus:

```azurecli-interactive
az webapp update --set clientCertEnabled=true --name <app-name> --resource-group <group-name>
```

## <a name="exclude-paths-from-requiring-authentication"></a>Ausschließen von Pfaden von der Anforderung der Authentifizierung

Wenn Sie die gegenseitige Authentifizierung für Ihre Anwendung aktivieren, erfordern alle Pfade unter dem Stammverzeichnis Ihrer App ein Clientzertifikat für den Zugriff. Um diese Anforderung für bestimmte Pfade aufzuheben, definieren Sie in Ihrer Anwendungskonfiguration Ausschlusspfade.

1. Wählen Sie auf der Verwaltungsseite Ihrer App im linken Navigationsbereich **Konfiguration** > **Allgemeine Einstellungen** aus.

1. Klicken Sie neben **Client exclusion paths** (Clientausschlusspfade) auf das Bearbeitungssymbol.

1. Klicken Sie auf **Neuer Pfad**, geben Sie einen Pfad an, und klicken Sie auf **OK**.

1. Klicken Sie oben auf der Seite auf **Speichern**.

Im folgenden Screenshot wird für alle Verzeichnisse unter dem Pfad `/public` für Ihre App kein Clientzertifikat angefordert.

![Zertifikatausschlusspfade][exclusion-paths]

## <a name="access-client-certificate"></a>Zugreifen auf das Clientzertifikat

In App Service erfolgt die TLS-Terminierung der Anforderung auf dem Front-End-Load Balancer. Bei der Weiterleitung der Anforderung an Ihren App-Code mit [aktivierten Clientzertifikaten](#enable-client-certificates) fügt App Service einen `X-ARR-ClientCert`-Anforderungsheader mit dem Clientzertifikat ein. App Service verwendet dieses Clientzertifikat nur für dessen Weiterleitung an Ihre App. Ihr App-Code ist für die Überprüfung des Clientzertifikats zuständig.

Für ASP.NET steht das Clientzertifikat über die Eigenschaft **HttpRequest.ClientCertificate** zur Verfügung.

Für andere Anwendungsstapel (Node.js, PHP usw.) steht das Clientzertifikat in Ihrer App über einen mit Base64 codierten Wert im Anforderungsheader `X-ARR-ClientCert` zur Verfügung.

## <a name="aspnet-sample"></a>Beispiel für ASP.NET

```csharp
    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class Cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                return true;
            }
        }
    }
```

## <a name="nodejs-sample"></a>Beispiel für Node.js

Der folgende Node.js-Beispielcode ruft den Header `X-ARR-ClientCert` ab und verwendet [node-forge](https://github.com/digitalbazaar/forge) zum Konvertieren der mit Base64 codierten PEM-Zeichenfolge in ein Zertifikatobjekt sowie dessen Überprüfung:

```javascript
import { NextFunction, Request, Response } from 'express';
import { pki, md, asn1 } from 'node-forge';

export class AuthorizationHandler {
    public static authorizeClientCertificate(req: Request, res: Response, next: NextFunction): void {
        try {
            // Get header
            const header = req.get('X-ARR-ClientCert');
            if (!header) throw new Error('UNAUTHORIZED');

            // Convert from PEM to pki.CERT
            const pem = `-----BEGIN CERTIFICATE-----${header}-----END CERTIFICATE-----`;
            const incomingCert: pki.Certificate = pki.certificateFromPem(pem);

            // Validate certificate thumbprint
            const fingerPrint = md.sha1.create().update(asn1.toDer(pki.certificateToAsn1(incomingCert)).getBytes()).digest().toHex();
            if (fingerPrint.toLowerCase() !== 'abcdef1234567890abcdef1234567890abcdef12') throw new Error('UNAUTHORIZED');

            // Validate time validity
            const currentDate = new Date();
            if (currentDate < incomingCert.validity.notBefore || currentDate > incomingCert.validity.notAfter) throw new Error('UNAUTHORIZED');

            // Validate issuer
            if (incomingCert.issuer.hash.toLowerCase() !== 'abcdef1234567890abcdef1234567890abcdef12') throw new Error('UNAUTHORIZED');

            // Validate subject
            if (incomingCert.subject.hash.toLowerCase() !== 'abcdef1234567890abcdef1234567890abcdef12') throw new Error('UNAUTHORIZED');

            next();
        } catch (e) {
            if (e instanceof Error && e.message === 'UNAUTHORIZED') {
                res.status(401).send();
            } else {
                next(e);
            }
        }
    }
}
```

## <a name="java-sample"></a>Java-Beispiel

Mit der folgenden Java-Klasse wird das Zertifikat aus `X-ARR-ClientCert` in eine `X509Certificate`-Instanz codiert. `certificateIsValid()` überprüft, ob der Fingerabdruck des Zertifikats mit dem im Konstruktor angegebenen übereinstimmt und das Zertifikat nicht abgelaufen ist.


```java
import java.io.ByteArrayInputStream;
import java.security.NoSuchAlgorithmException;
import java.security.cert.*;
import java.security.MessageDigest;

import sun.security.provider.X509Factory;

import javax.xml.bind.DatatypeConverter;
import java.util.Base64;
import java.util.Date;

public class ClientCertValidator { 

    private String thumbprint;
    private X509Certificate certificate;

    /**
     * Constructor.
     * @param certificate The certificate from the "X-ARR-ClientCert" HTTP header
     * @param thumbprint The thumbprint to check against
     * @throws CertificateException If the certificate factory cannot be created.
     */
    public ClientCertValidator(String certificate, String thumbprint) throws CertificateException {
        certificate = certificate
                .replaceAll(X509Factory.BEGIN_CERT, "")
                .replaceAll(X509Factory.END_CERT, "");
        CertificateFactory cf = CertificateFactory.getInstance("X.509");
        byte [] base64Bytes = Base64.getDecoder().decode(certificate);
        X509Certificate X509cert =  (X509Certificate) cf.generateCertificate(new ByteArrayInputStream(base64Bytes));

        this.setCertificate(X509cert);
        this.setThumbprint(thumbprint);
    }

    /**
     * Check that the certificate's thumbprint matches the one given in the constructor, and that the
     * certificate has not expired.
     * @return True if the certificate's thumbprint matches and has not expired. False otherwise.
     */
    public boolean certificateIsValid() throws NoSuchAlgorithmException, CertificateEncodingException {
        return certificateHasNotExpired() && thumbprintIsValid();
    }

    /**
     * Check certificate's timestamp.
     * @return Returns true if the certificate has not expired. Returns false if it has expired.
     */
    private boolean certificateHasNotExpired() {
        Date currentTime = new java.util.Date();
        try {
            this.getCertificate().checkValidity(currentTime);
        } catch (CertificateExpiredException | CertificateNotYetValidException e) {
            return false;
        }
        return true;
    }

    /**
     * Check the certificate's thumbprint matches the given one.
     * @return Returns true if the thumbprints match. False otherwise.
     */
    private boolean thumbprintIsValid() throws NoSuchAlgorithmException, CertificateEncodingException {
        MessageDigest md = MessageDigest.getInstance("SHA-1");
        byte[] der = this.getCertificate().getEncoded();
        md.update(der);
        byte[] digest = md.digest();
        String digestHex = DatatypeConverter.printHexBinary(digest);
        return digestHex.toLowerCase().equals(this.getThumbprint().toLowerCase());
    }

    // Getters and setters

    public void setThumbprint(String thumbprint) {
        this.thumbprint = thumbprint;
    }

    public String getThumbprint() {
        return this.thumbprint;
    }

    public X509Certificate getCertificate() {
        return certificate;
    }

    public void setCertificate(X509Certificate certificate) {
        this.certificate = certificate;
    }
}
```

[exclusion-paths]: ./media/app-service-web-configure-tls-mutual-auth/exclusion-paths.png
