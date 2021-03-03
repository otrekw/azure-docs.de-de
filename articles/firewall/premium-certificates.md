---
title: Zertifikate der Vorschauversion von Azure Firewall Premium
description: Zum ordnungsgemäßen Konfigurieren der TLS-Inspektion für die Vorschauversion von Azure Firewall Premium müssen Sie Zertifikate von Zwischenzertifizierungsstellen konfigurieren und installieren.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 02/16/2021
ms.author: victorh
ms.openlocfilehash: 3914a82903c293cf1a8306b5ecc1f542fef83e72
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549459"
---
# <a name="azure-firewall-premium-preview-certificates"></a>Zertifikate der Vorschauversion von Azure Firewall Premium 

> [!IMPORTANT]
> Azure Firewall Premium ist zurzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

 Um die TLS-Inspektion für die Vorschauversion von Azure Firewall Premium ordnungsgemäßen zu konfigurieren, müssen Sie ein Zertifikat der Zwischenzertifizierungsstelle bereitstellen und es in Azure Key Vault speichern.

## <a name="certificates-used-by-azure-firewall-premium-preview"></a>Von der Vorschauversion von Azure Firewall Premium verwendete Zertifikate

Es gibt drei Arten von Zertifikaten, die in einer typischen Bereitstellung verwendet werden:

- **Zertifikat der Zwischenzertifizierungsstelle (CA-Zertifikat)**

   Eine Zertifizierungsstelle (Certificate Authority, CA) ist eine vertrauenswürdige Organisation zum Signieren von digitalen Zertifikaten. Eine Zertifizierungsstelle überprüft die Identität und Legitimität eines Unternehmens oder einer Person, von denen ein Zertifikat angefordert wird. Ist die Überprüfung erfolgreich, stellt die Zertifizierungsstelle ein signiertes Zertifikat aus. Wenn der Server dem Client (z. B. Ihrem Webbrowser) das Zertifikat während eines SSL/TLS-Handshakes vorlegt, versucht der Client, die Signatur anhand einer Liste von *bekannten geeigneten* Signaturgebern zu überprüfen. Webbrowser verfügen normalerweise über Listen von Zertifizierungsstellen, denen sie implizit vertrauen, um Hosts zu identifizieren. Wenn die Zertifizierungsstelle nicht in der Liste enthalten ist, wie bei einigen Websites, die ihre eigenen Zertifikate signieren, warnt der Browser den Benutzer, dass das Zertifikat nicht von einer anerkannten Zertifizierungsstelle signiert ist, und fragt den Benutzer, ob er die Kommunikation mit der nicht verifizierten Website fortsetzen möchte.

- **Serverzertifikat (Websitezertifikat)**

   Ein Zertifikat, das mit einem bestimmten Domänennamen verknüpft ist. Wenn eine Website ein gültiges Zertifikat aufweist, bedeutet dies, dass eine Zertifizierungsstelle Schritte unternommen hat, um zu überprüfen, dass die Webadresse tatsächlich zu dieser Organisation gehört. Wenn Sie eine URL eingeben oder einem Link zu einer sicheren Website folgen, überprüft Ihr Browser das Zertifikat auf die folgenden Merkmale:
   - Die Adresse der Website stimmt mit der Adresse auf dem Zertifikat überein.
   - Das Zertifikat ist von einer Zertifizierungsstelle signiert, die der Browser als *vertrauenswürdig* erkennt.
   
   Gelegentlich können Benutzer eine Verbindung mit einem Server mit einem nicht vertrauenswürdigen Zertifikat herstellen. Azure Firewall trennt die Verbindung, als ob der Server die Verbindung beendet hätte.

- **Zertifikat der Stammzertifizierungsstelle (Stammzertifikat)**

   Eine Zertifizierungsstelle kann mehrere Zertifikate in Form einer Baumstruktur ausstellen. Ein Stammzertifikat ist das oberste Zertifikat der Baumstruktur.

Die Vorschauversion von Azure Firewall Premium kann ausgehenden HTTP/S-Datenverkehr abfangen und automatisch ein Serverzertifikat für `www.website.com` generieren. Dieses Zertifikat wird mit dem von Ihnen bereitgestellten Zertifikat der Zwischenzertifizierungsstelle generiert. Browser- und Clientanwendungen der Endbenutzer müssen dem Zertifikat der Stammzertifizierungsstelle oder dem Zertifikat der Zwischenzertifizierungsstelle Ihrer Organisation vertrauen, damit dieses Verfahren funktioniert. 

:::image type="content" source="media/premium-certificates/certificate-process.png" alt-text="Zertifikatprozess":::

## <a name="intermediate-ca-certificate-requirements"></a>Anforderungen an das Zertifikat der Zwischenzertifizierungsstelle

Stellen Sie sicher, dass Ihr Zertifikat der Zertifizierungsstelle die folgenden Anforderungen erfüllt:

- Bei der Bereitstellung als Key Vault-Geheimnis müssen Sie PFX (Pkcs12) ohne Kennwort mit einem Zertifikat und einem privaten Schlüssel verwenden.

- Es muss ein einzelnes Zertifikat sein und sollte nicht die gesamte Zertifikatskette umfassen.  

- Es muss ein Jahr lang gültig sein.  

- Es muss ein privater RSA-Schlüssel mit einer Mindestgröße von 4096 Bytes sein.  

- Er muss die `KeyUsage`-Erweiterung aufweisen, die mit dem `KeyCertSign`-Flag als „Kritisch“ gekennzeichnet ist (RFC 5280; 4.2.1.3 Schlüsselverwendung).

- Es muss mit der `BasicContraints`-Erweiterung als „Kritisch“ gekennzeichnet sein (RFC 5280; 4.2.1.9 Basiseinschränkungen).  

- Das Flag `CA` muss auf TRUE festgelegt werden.

- Die Pfadlänge muss größer als oder gleich 1 sein.

## <a name="azure-key-vault"></a>Azure-Schlüsseltresor

[Azure Key Vault](../key-vault/general/overview.md) ist ein als Plattform verwalteter Geheimnisspeicher, mit dem Sie Geheimnisse, Schlüssel und TLS/SSL-Zertifikate schützen können. Azure Firewall Premium unterstützt die Integration mit Key Vault für Serverzertifikate, die an eine Firewallrichtlinie angefügt sind.
 
So konfigurieren Sie Ihren Schlüsseltresor

- Sie müssen ein vorhandenes Zertifikat mit seinem Schlüsselpaar in Ihren Schlüsseltresor importieren. 
- Alternativ kann auch ein Key Vault-Geheimnis verwendet werden, das als Base64-codierte PFX-Datei ohne Kennwort gespeichert ist.  Eine PFX-Datei ist ein digitales Zertifikat, das sowohl den privaten als auch den öffentlichen Schlüssel enthält.
- Es wird empfohlen, einen Zertifikatsimport einer Zertifizierungsstelle zu verwenden, da Sie damit eine Warnung basierend auf dem Ablaufdatum des Zertifikats konfigurieren können.
- Nachdem Sie ein Zertifikat oder ein Geheimnis importiert haben, müssen Sie im Schlüsseltresor Zugriffsrichtlinien definieren, damit die zugewiesene Identität Zugriff auf das Zertifikat/Geheimnis erhält.
- Das bereitgestellte Zertifikat der Zertifizierungsstelle muss von Ihrer Azure-Workload als vertrauenswürdig eingestuft werden. Stellen Sie sicher, dass sie ordnungsgemäß bereitgestellt werden.

Sie können entweder eine benutzerseitig zugewiesene verwaltete Identität erstellen oder eine bereits vorhandene erneut verwenden, über die Azure Firewall in Ihrem Namen Zertifikate von Key Vault abruft. Weitere Informationen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md). 

## <a name="configure-a-certificate-in-your-policy"></a>Konfigurieren eines Zertifikats in der Richtlinie

Um ein Zertifikat der Zertifizierungsstelle in Ihrer Firewall Premium-Richtlinie zu konfigurieren, wählen Sie Ihre Richtlinie und dann **TLS-Inspektion (Vorschau)** aus. Wählen Sie **Aktiviert** auf der Seite **TLS-Inspektion** aus. Wählen Sie dann Ihr Zertifizierungsstellenzertifikat in Azure Key Vault aus, wie in der folgenden Abbildung gezeigt:

:::image type="content" source="media/premium-certificates/tls-inspection.png" alt-text="Azure Firewall Premium: Übersichtsdiagramm":::
 
> [!IMPORTANT]
> Wenn Sie ein Zertifikat aus dem Azure-Portal anzeigen und konfigurieren möchten, müssen Sie Ihr Azure-Benutzerkonto zur Key Vault-Zugriffsrichtlinie hinzufügen. Weisen Sie Ihrem Benutzerkonto die Rechte zum **Abrufen** und **Auflisten** unter **Geheimnisberechtigungen** zu.
   :::image type="content" source="media/premium-certificates/secret-permissions.png" alt-text="Azure Key Vault-Zugriffsrichtlinie":::


## <a name="troubleshooting"></a>Problembehandlung

Wenn Ihr Zertifizierungsstellenzertifikat gültig ist, aber bei der TLS-Überprüfung der Zugriff auf FQDNs oder URLs nicht möglich ist, sollten Sie folgende Punkte überprüfen:

- Stellen Sie sicher, dass das Webserverzertifikat gültig ist.  

- Stellen Sie sicher, dass das Zertifikat der Stammzertifizierungsstelle auf dem Clientbetriebssystem installiert ist.  

- Stellen Sie sicher, dass der Browser oder HTTPS-Client ein gültiges Stammzertifikat enthält. Firefox und einige andere Browser weisen möglicherweise spezielle Zertifizierungsrichtlinien auf.  

- Stellen Sie sicher, dass der URL-Zieltyp in Ihrer Anwendungsregel den richtigen Pfad und alle anderen auf der HTML-Zielseite eingebetteten Links abdeckt. Sie können Platzhalter verwenden, um den vollständigen erforderlichen URL-Pfad auf einfache Weise abzudecken.  


## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über die Features von Azure Firewall Premium](premium-features.md)
