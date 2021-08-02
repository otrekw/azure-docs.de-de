---
title: Sichern von APIs über eine Clientzertifikatauthentifizierung in API Management
titleSuffix: Azure API Management
description: In diesem Artikel erfahren Sie, wie Sie den Zugriff auf APIs mithilfe von Clientzertifikaten sichern. Eingehende Zertifikate können mit Richtlinienausdrücken überprüft werden.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/01/2021
ms.author: apimpm
ms.openlocfilehash: d000b9db658c76b5d7cdb586599f04d9078dde5d
ms.sourcegitcommit: a434cfeee5f4ed01d6df897d01e569e213ad1e6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111812155"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>Sichern von APIs über eine Clientzertifikatauthentifizierung in API Management

API Management bietet die Möglichkeit, den Zugriff auf APIs (d.h. vom Client auf API Management) mithilfe von Clientzertifikaten zu sichern. Mithilfe von Richtlinienausdrücken können Sie die von dem Client, der eine Verbindung herstellen möchte, angegebenen Zertifikate überprüfen, und Sie können Zertifikateigenschaften anhand von gewünschten Werten überprüfen.

Informationen zum Schützen des Zugriffs auf den Back-End-Dienst einer API mithilfe von Clientzertifikaten (von API Management auf das Back-End) finden Sie unter [Sichern von Back-End-Diensten über eine Clientzertifikatauthentifizierung in Azure API Management](./api-management-howto-mutual-certificates.md).

> [!IMPORTANT]
> Um Clientzertifikate im Tarif „Entwickler“, „Basic“, „Standard“ oder „Premium“ über HTTP/2 empfangen und überprüfen zu können, müssen Sie wie unten gezeigt auf dem Blatt „Benutzerdefinierte Domänen“ die Einstellung „Clientzertifikat aushandeln“ aktivieren.

![Clientzertifikat aushandeln](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

> [!IMPORTANT]
> Um Clientzertifikate im Tarif „Verbrauch“ empfangen und überprüfen zu können, müssen Sie wie unten gezeigt auf dem Blatt „Benutzerdefinierte Domänen“ die Einstellung „Clientzertifikat anfordern“ aktivieren.

![Anfordern des Clientzertifikats](./media/api-management-howto-mutual-certificates-for-clients/request-client-certificate.png)

## <a name="policy-to-validate-client-certificates"></a>Richtlinie zum Überprüfen von Clientzertifikaten

Verwenden Sie die Richtlinie [validate-client-certificate](api-management-access-restriction-policies.md#validate-client-certificate), um ein oder mehrere Attribute eines Clientzertifikats zu überprüfen, das für den Zugriff auf in Ihrer API Management-Instanz gehostete APIs verwendet wird.

Konfigurieren Sie die Richtlinie zum Überprüfen eines oder mehrerer Attribute, wie z. B.: Zertifikataussteller, Antragsteller, Fingerabdruck, Informationen dazu, ob das Zertifikat anhand einer Onlinesperrliste überprüft wird, sowie weitere Attribute.

Weitere Informationen finden Sie unter [API Management-Richtlinien für die Zugriffsbeschränkung](api-management-access-restriction-policies.md).

## <a name="certificate-validation-with-context-variables"></a>Zertifikatüberprüfung mit Kontextvariablen

Sie können auch Richtlinienausdrücke mit der [`context`-Variable](api-management-policy-expressions.md#ContextVariables) erstellen, um Clientzertifikate zu überprüfen. Die Beispiele in den folgenden Abschnitten zeigen Ausdrücke, die die Eigenschaft `context.Request.Certificate` sowie weitere `context`-Eigenschaften verwenden.

> [!IMPORTANT]
> Ab Mai 2021 fordert die `context.Request.Certificate`-Eigenschaft nur dann das Zertifikat an, wenn die [`hostnameConfiguration`](/rest/api/apimanagement/2019-12-01/apimanagementservice/createorupdate#hostnameconfiguration) der API Management-Instanz die `negotiateClientCertificate`-Eigenschaft auf TRUE festlegt. Standardmäßig ist `negotiateClientCertificate` auf FALSE festgelegt.

### <a name="checking-the-issuer-and-subject"></a>Prüfen des Ausstellers und des Antragstellers

Die unten stehenden Richtlinien konfiguriert werden, um den Aussteller und den Antragsteller eines Clientzertifikats zu prüfen:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName.Name != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Verwenden Sie `context.Request.Certificate.VerifyNoRevocation()` anstelle von `context.Request.Certificate.Verify()`, um die Überprüfung der Zertifikatssperrliste zu deaktivieren.
> Wenn das Clientzertifikat selbstsigniert ist, müssen Stamm- oder Zwischen-Zertifizierungsstellenzertifikate in API Management [hochgeladen](api-management-howto-ca-certificates.md) werden, damit `context.Request.Certificate.Verify()` und `context.Request.Certificate.VerifyNoRevocation()` funktionieren.

### <a name="checking-the-thumbprint"></a>Prüfen des Fingerabdrucks

Die folgenden Richtlinien können zum Prüfen des Fingerabdrucks eines Clientzertifikats konfiguriert werden:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Thumbprint != "DESIRED-THUMBPRINT-IN-UPPER-CASE")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Verwenden Sie `context.Request.Certificate.VerifyNoRevocation()` anstelle von `context.Request.Certificate.Verify()`, um die Überprüfung der Zertifikatssperrliste zu deaktivieren.
> Wenn das Clientzertifikat selbstsigniert ist, müssen Stamm- oder Zwischen-Zertifizierungsstellenzertifikate in API Management [hochgeladen](api-management-howto-ca-certificates.md) werden, damit `context.Request.Certificate.Verify()` und `context.Request.Certificate.VerifyNoRevocation()` funktionieren.

### <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>Prüfen eines Fingerabdrucks anhand von auf API Management hochgeladenen Zertifikaten

Das folgende Beispiel zeigt, wie Sie den Fingerabdruck eines Clientzertifikats anhand von auf API Management hochgeladenen Zertifikaten prüfen können:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify()  || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

> [!NOTE]
> Verwenden Sie `context.Request.Certificate.VerifyNoRevocation()` anstelle von `context.Request.Certificate.Verify()`, um die Überprüfung der Zertifikatssperrliste zu deaktivieren.
> Wenn das Clientzertifikat selbstsigniert ist, müssen Stamm- oder Zwischen-Zertifizierungsstellenzertifikate in API Management [hochgeladen](api-management-howto-ca-certificates.md) werden, damit `context.Request.Certificate.Verify()` und `context.Request.Certificate.VerifyNoRevocation()` funktionieren.

> [!TIP]
> Das Problem aufgrund eines Clientzertifikat-Deadlocks, das in [diesem Artikel](https://techcommunity.microsoft.com/t5/Networking-Blog/HTTPS-Client-Certificate-Request-freezes-when-the-Server-is/ba-p/339672) beschrieben ist, kann auf unterschiedliche Art und Weise auftreten, z. B. Einfrieren von Anforderungen, Anforderungen führen nach dem Timeout zum Status `403 Forbidden` oder `context.Request.Certificate` ist `null`. Dieses Problem wirkt sich normalerweise auf Anforderungen vom Typ `POST` und `PUT` mit einer Inhaltslänge von ca. 60 KB oder mehr aus.
> Um dieses Problem zu verhindern, aktivieren Sie auf dem Blatt „Benutzerdefinierte Domänen“ die Einstellung „Clientzertifikat aushandeln“ für gewünschte Hostnamen, wie auf der ersten Abbildung dieses Dokuments gezeigt. Dieses Feature ist im Tarif „Verbrauch“ nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte

-   [Sichern von Back-End-Diensten über eine Clientzertifikatauthentifizierung](./api-management-howto-mutual-certificates.md)
-   [Hochladen von Zertifikaten](./api-management-howto-mutual-certificates.md)
