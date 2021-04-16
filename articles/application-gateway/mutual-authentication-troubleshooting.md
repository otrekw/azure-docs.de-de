---
title: Behandeln von Problemen bei der gegenseitigen Authentifizierung in Azure Application Gateway
description: Es wird beschrieben, wie Sie Probleme beheben, die bei der gegenseitigen Authentifizierung in Application Gateway auftreten.
services: application-gateway
author: mscatyao
ms.service: application-gateway
ms.topic: troubleshooting
ms.date: 04/02/2021
ms.author: caya
ms.openlocfilehash: 623addd253b3eb28bdf70db02ddfbe4b320cbae7
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106230858"
---
# <a name="troubleshooting-mutual-authentication-errors-in-application-gateway-preview"></a>Beheben von Fehlern bei der gegenseitigen Authentifizierung in Application Gateway (Vorschau)

In diesem Artikel wird beschrieben, wie Sie Probleme bei der gegenseitigen Authentifizierung in Application Gateway beheben. 

## <a name="overview"></a>Übersicht 

Nach dem Konfigurieren der gegenseitigen Authentifizierung auf einer Application Gateway-Instanz können einige Fehler auftreten, wenn versucht wird, die gegenseitige Authentifizierung zu nutzen. Häufige Gründe für Fehler dieser Art:

* Upload eines Zertifikats oder einer Zertifikatkette ohne Zertifikat der Stammzertifizierungsstelle
* Upload einer Zertifikatkette mit mehreren Zertifikaten der Stammzertifizierungsstelle
* Upload einer Zertifikatkette, die nur ein untergeordnetes Zertifikat ohne Zertifikat der Zertifizierungsstelle enthalten hat 
* Überprüfungsfehler aufgrund eines Konflikts beim Aussteller-DN  

Es werden verschiedene Szenarien, zu denen es ggf. kommen kann, und die entsprechenden Schritte zur Problembehandlung beschrieben. Anschließend werden die Fehlercodes behandelt und die wahrscheinlichen Ursachen der Fehlercodes beschrieben, die bei der gegenseitigen Authentifizierung unter Umständen angezeigt werden. 

## <a name="scenario-troubleshooting---configuration-problems"></a>Problembehandlung für verschiedene Szenarien: Konfigurationsprobleme
Es gibt einige Szenarien, denen Sie ggf. begegnen, wenn Sie versuchen, die gegenseitige Authentifizierung zu konfigurieren. In den folgenden Abschnitten ist beschrieben, wie Sie einige der am häufigsten auftretenden Probleme beheben. 

### <a name="self-signed-certificate"></a>Selbstsigniertes Zertifikat

#### <a name="problem"></a>Problem 

Das von Ihnen hochgeladene Clientzertifikat ist ein selbstsigniertes Zertifikat, das zum folgenden Fehlercode führt: „ApplicationGatewayTrustedClientCertificateDoesNotContainAnyCACertificate“.

#### <a name="solution"></a>Lösung 

Vergewissern Sie sich, dass das von Ihnen genutzte selbstsignierte Zertifikat über die folgende Erweiterung verfügt: *BasicConstraintsOid* = "2.5.29.19". Hiermit wird angegeben, dass der Antragsteller als Zertifizierungsstelle fungieren kann. Auf diese Weise wird sichergestellt, dass es sich beim verwendeten Zertifikat um ein Zertifikat der Zertifizierungsstelle handelt. Weitere Informationen zum Generieren von selbstsignierten Clientzertifikaten finden Sie unter [Vertrauenswürdige Clientzertifikate](./mutual-authentication-certificate-management.md).

## <a name="scenario-troubleshooting---connectivity-problems"></a>Problembehandlung für verschiedene Szenarien: Konnektivitätsprobleme

Es kann vorkommen, dass Sie die gegenseitige Authentifizierung ohne Probleme konfigurieren können, aber beim Senden von Anforderungen an Ihre Application Gateway-Instanz Probleme auftreten. Im folgenden Abschnitt sind einige häufige Probleme und Lösungen beschrieben. Sie finden die *sslClientVerify*-Eigenschaft in den Zugriffsprotokollen Ihrer Application Gateway-Instanz. 

### <a name="sslclientverify-is-none"></a>„SslClientVerify“ auf „NONE“ festgelegt

#### <a name="problem"></a>Problem 

Für die *sslClientVerify*-Eigenschaft wird in Ihren Zugriffsprotokollen „NONE“ angezeigt. 

#### <a name="solution"></a>Lösung 

Dies tritt auf, wenn vom Client beim Senden einer Anforderung an die Application Gateway-Instanz kein Clientzertifikat gesendet wird. Die Ursache besteht ggf. darin, dass der Client, von dem die Anforderung an die Application Gateway-Instanz gesendet wird, nicht richtig für die Nutzung von Clientzertifikaten konfiguriert ist. Eine Möglichkeit zur Sicherstellung, dass die Clientauthentifizierung auf der Application Gateway-Instanz wie erwartet funktioniert, ist die Verwendung des folgenden OpenSSL-Befehls:

```
openssl s_client -connect <hostname:port> -cert <path-to-certificate> -key <client-private-key-file> 
```

Das Flag `-cert` ist das untergeordnete Zertifikat, und das Flag `-key` steht für die Datei mit dem privaten Schlüssel des Clients. 

Weitere Informationen zur Verwendung des OpenSSL-Befehls `s_client` finden Sie auf der zugehörigen [Manpage](https://www.openssl.org/docs/man1.0.2/man1/openssl-s_client.html).

### <a name="sslclientverify-is-failed"></a>„SslClientVerify“ auf „FAILED“ festgelegt

#### <a name="problem"></a>Problem

Für die *sslClientVerify*-Eigenschaft wird in Ihren Zugriffsprotokollen „FAILED“ angezeigt. 

#### <a name="solution"></a>Lösung

Die Zugriffsprotokolle können verschiedene potenzielle Ursachen für Fehler enthalten. Hier ist eine Liste mit häufigen Fehlerursachen angegeben:
* **Ausstellerzertifikat kann nicht abgerufen werden:** Das Ausstellerzertifikat des Clientzertifikats wurde nicht gefunden. Normalerweise bedeutet dies, dass die vertrauenswürdige ZS-Clientzertifikatkette auf der Application Gateway-Instanz unvollständig ist. Überprüfen Sie, ob die vertrauenswürdige ZS-Clientzertifikatkette, die auf die Application Gateway-Instanz hochgeladen wurde, vollständig ist.  
* **Lokales Ausstellerzertifikat konnte nicht abgerufen werden:** Dies ähnelt dem obigen Fall, in dem das Ausstellerzertifikat nicht abgerufen werden konnte und das Ausstellerzertifikat des Clientzertifikats nicht gefunden wurde. Normalerweise bedeutet dies, dass die vertrauenswürdige ZS-Clientzertifikatkette auf der Application Gateway-Instanz unvollständig ist. Überprüfen Sie, ob die vertrauenswürdige ZS-Clientzertifikatkette, die auf die Application Gateway-Instanz hochgeladen wurde, vollständig ist.
* **Erstes Zertifikat kann nicht überprüft werden:** Das Clientzertifikat kann nicht überprüft werden. Dieser Fehler tritt vor allem dann auf, wenn vom Client nur das untergeordnete Zertifikat angegeben wird und dessen Aussteller nicht vertrauenswürdig ist. Überprüfen Sie, ob die vertrauenswürdige ZS-Clientzertifikatkette, die auf die Application Gateway-Instanz hochgeladen wurde, vollständig ist.
* **Aussteller des Clientzertifikats kann nicht überprüft werden:** Dieser Fehler tritt auf, wenn *VerifyClientCertIssuerDN* in der Konfiguration auf „true“ festgelegt ist. Dies ist meist der Fall, wenn der Austeller-DN des Clientzertifikats nicht mit dem *ClientCertificateIssuerDN* übereinstimmt, der aus der vom Kunden hochgeladenen vertrauenswürdigen ZS-Clientzertifikatkette extrahiert wurde. Weitere Informationen dazu, wie der *ClientCertificateIssuerDN* von Application Gateway extrahiert wird, finden Sie im Abschnitt mit der Beschreibung, [wie der Aussteller-DN von Application Gateway extrahiert wird](./mutual-authentication-overview.md#verify-client-certificate-dn). Die bewährte Methode besteht darin sicherzustellen, dass Sie eine Zertifikatkette pro Datei auf die Application Gateway-Instanz hochladen. 

Weitere Informationen zum Extrahieren der gesamten vertrauenswürdigen ZS-Clientzertifikatkette für den Upload auf die Application Gateway-Instanz finden Sie unter [Exportieren einer vertrauenswürdigen ZS-Clientzertifikatkette für die Clientauthentifizierung](./mutual-authentication-certificate-management.md).

## <a name="error-code-troubleshooting"></a>Problembehandlung für Fehlercodes
Wenn bei Ihnen einer der folgenden Fehlercodes angezeigt wird, helfen Ihnen die unten angegebenen empfohlenen Lösungen beim Beheben des Problems weiter. 

### <a name="error-code-applicationgatewaytrustedclientcertificatemustspecifydata"></a>Fehlercode: ApplicationGatewayTrustedClientCertificateMustSpecifyData

#### <a name="cause"></a>Ursache

Es fehlen Zertifikatdaten. Das hochgeladene Zertifikat kann beispielsweise eine leere Datei ohne Zertifikatdaten sein. 

#### <a name="solution"></a>Lösung

Vergewissern Sie sich, dass für die hochgeladene Zertifikatdatei keine Daten fehlen. 

### <a name="error-code-applicationgatewaytrustedclientcertificatemustnothaveprivatekey"></a>Fehlercode: ApplicationGatewayTrustedClientCertificateMustNotHavePrivateKey

#### <a name="cause"></a>Ursache

Die Zertifikatkette enthält einen privaten Schlüssel. In der Zertifikatkette darf kein privater Schlüssel enthalten sein. 

#### <a name="solution"></a>Lösung

Überprüfen Sie die Zertifikatkette, die hochgeladen wurde, und entfernen Sie den privaten Schlüssel, der in der Kette enthalten war. Laden Sie die Kette ohne den privaten Schlüssel erneut hoch. 

### <a name="error-code-applicationgatewaytrustedclientcertificateinvaliddata"></a>Fehlercode: ApplicationGatewayTrustedClientCertificateInvalidData

#### <a name="cause"></a>Ursache

Es gibt zwei mögliche Ursachen für diesen Fehlercode.
1. Bei der Analyse ist ein Fehler aufgetreten, weil die Kette nicht im richtigen Format angegeben wurde. Von der Application Gateway-Instanz wird erwartet, dass eine Zertifikatkette im PEM-Format vorliegt und dass die Zertifikatdaten durch Trennzeichen voneinander getrennt sind. 
2. Der Parser hat keine Daten für die Analyse gefunden. Unter Umständen hat die hochgeladene Datei nur die Trennzeichen enthalten, aber keine Zertifikatdaten. 

#### <a name="solution"></a>Lösung

Je nach Ursache dieses Fehlers gibt es zwei mögliche Lösungen. 
* Vergewissern Sie sich, dass die hochgeladene Zertifikatkette das richtige Format (PEM) hat und dass die Zertifikatdaten korrekt mit Trennzeichen voneinander getrennt sind. 
* Überprüfen Sie, ob die hochgeladene Zertifikatsdatei nicht nur die Trennzeichen, sondern auch die Zertifikatdaten enthält. 

### <a name="error-code-applicationgatewaytrustedclientcertificatedoesnotcontainanycacertificate"></a>Fehlercode: ApplicationGatewayTrustedClientCertificateDoesNotContainAnyCACertificate

#### <a name="cause"></a>Ursache

Das hochgeladene Zertifikat hat nur ein untergeordnetes Zertifikat ohne Zertifikat der Zertifizierungsstelle enthalten. Das Hochladen einer Zertifikatkette mit Zertifizierungsstellenzertifikaten und einem untergeordneten Zertifikat ist akzeptabel, da das untergeordnete Zertifikat einfach ignoriert wird. Ein Zertifikat muss aber über eine Zertifizierungsstelle verfügen. 

#### <a name="solution"></a>Lösung 

Vergewissern Sie sich, dass die hochgeladene Zertifikatkette mehr als nur das untergeordnete Zertifikat enthält. Die Erweiterung *BasicConstraintsOid* = "2.5.29.19" sollte vorhanden sein und darauf hinweisen, dass der Antragsteller als Zertifizierungsstelle fungieren kann.

### <a name="error-code-applicationgatewayonlyonerootcaallowedintrustedclientcertificate"></a>Fehlercode: ApplicationGatewayOnlyOneRootCAAllowedInTrustedClientCertificate

#### <a name="cause"></a>Ursache

Die Zertifikatkette hat mehrere Zertifikate der Stammzertifizierungsstelle *oder* keine Zertifikate der Stammzertifizierungsstelle enthalten. 

#### <a name="solution"></a>Lösung

Die hochgeladenen Zertifikate dürfen nur ein Zertifikat der Stammzertifizierungsstelle enthalten (und können so viele Zwischenzertifizierungsstellen-Zertifikate wie nötig enthalten).


