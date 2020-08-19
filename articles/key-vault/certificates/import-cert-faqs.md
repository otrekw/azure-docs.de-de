---
title: 'Häufig gestellte Fragen: Azure Key Vault-Zertifikatimport'
description: 'Häufig gestellte Fragen: Azure Key Vault-Zertifikatimport'
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: conceptual
ms.date: 07/20/2020
ms.author: sebansal
ms.openlocfilehash: 402672d8eeaae8a5097e2ab2905997eb1f646ad6
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056345"
---
# <a name="frequently-asked-questions---azure-key-vault-certificate-import"></a>Häufig gestellte Fragen: Azure Key Vault-Zertifikatimport

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="how-can-i-import-a-certificate-in-azure-key-vault"></a>Wie kann ich ein Zertifikat in Azure Key Vault importieren?

Zertifikat importieren: Für den Importvorgang akzeptiert Azure Key Vault zwei Zertifikatformate: PEM und PFX. Obwohl es PEM-Dateien gibt, die nur den öffentlichen Teil enthalten, erfordert und akzeptiert Azure Key Tresor nur PEM- oder PFX-Dateien im Dateiordner und mit einem privaten Schlüssel. Befolgen Sie das [Tutorial zum Importieren eines Zertifikats](https://docs.microsoft.com/azure/key-vault/certificates/tutorial-import-certificate#import-a-certificate-to-key-vault).

### <a name="after-importing-password-protected-certificate-into-the-key-vault-and-then-downloading-it-i-am-not-able-to-see-the-password-associated-with-the-certificate"></a>Nachdem ich ein durch ein Kennwort geschütztes Zertifikat in Key Vault importiert und es dann heruntergeladen habe, kann ich das dem Zertifikat zugeordnete Kennwort nicht anzeigen.
    
Das dem hochgeladenen geschützten Zertifikat zugeordnete Kennwort wird nach dem Speichern in Key Vault nicht gespeichert. Er wird nur während des Importvorgangs benötigt. Obwohl es sich hierbei um ein entwurfsbedingtes Konzept handelt, können Sie das Zertifikat immer als Geheimnis abrufen und aus Base64 in PFX konvertieren, indem Sie das vorherige Kennwort über [Azure PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/37431.exporting-azure-app-service-certificates.aspx) hinzufügen.

### <a name="how-can-i-resolve-bad-parameter-error-what-are-the-supported-certificate-formats-for-importing-in-key-vault"></a>Wie kann ich den Fehler „Bad Parameter“ (Ungültiger Parameter) beheben? Welche Zertifikatformate werden beim Importieren in Key Vault unterstützt?

Wenn Sie das Zertifikat importieren, müssen Sie sicherstellen, dass der Schlüssel in der Datei selbst enthalten ist. Wenn der private Schlüssel separat in einem anderen Format vorliegt, müssen Sie den Schlüssel mit dem Zertifikat kombinieren. Einige Zertifizierungsstellen bieten Zertifikate in verschiedenen Formaten an. Stellen Sie daher vor dem Importieren des Zertifikats sicher, dass es im PEM- oder PFX-Format vorliegt und dass der verwendete Schlüssel ein RSA- oder ECC-Schlüssel ist. Diese können zur Überprüfung der [Zertifikatanforderungen](https://docs.microsoft.com/azure/key-vault/certificates/certificate-scenarios#formats-of-import-we-support) und [Zertifikatschlüsselanforderungen](https://docs.microsoft.com/azure/key-vault/keys/about-keys#cryptographic-protection) herangezogen werden.

###  <a name="can-i-import-certificate-using-arm-template"></a>Kann ich ein Zertifikat mithilfe einer ARM-Vorlage importieren?

Nein, es können keine Zertifikatvorgänge mithilfe von ARM-Vorlagen ausgeführt werden. Die empfohlene Problemumgehung ist die Verwendung der Methoden für den Zertifikatimport in der API, der CLI oder in PowerShell. Wenn Sie bereits über ein Zertifikat verfügen, können Sie es als Geheimnis importieren.

### <a name="error-when-importing-certificate-via-portal-something-went-wrong-how-can-i-investigate-further"></a>Fehler beim Importieren eines Zertifikats über das Portal. „Es ist ein Problem aufgetreten.“ Wie kann ich weitere Untersuchungen durchführen?
    
Wenn Sie einen aussagekräftigeren Fehler anzeigen möchten, importieren Sie die Zertifikatsdatei über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-import) oder [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate?view=azurermps-6.13.0).

### <a name="how-can-i-resolve-error-type-access-denied-or-user-is-unauthorized-to-import-certificate"></a>Wie kann ich den folgenden Fehlertyp beheben: „Access denied or user is unauthorized to import certificate“ (Zugriff verweigert, oder der Benutzer ist nicht autorisiert, das Zertifikat zu importieren)?
    
Für diesen Vorgang ist die Berechtigung für Zertifikate bzw. den Import erforderlich. Navigieren Sie zum Speicherort des Schlüsseltresors. Sie müssen dem Benutzer unter Zugriffsrichtlinien die entsprechenden Berechtigungen erteilen. Navigieren Sie zu Key Vault > Zugriffsrichtlinien > Zugriffsrichtlinie hinzufügen“, wählen Sie „Zertifikatberechtigungen“ (oder die gewünschten Berechtigungen) > Prinzipal > aus, und suchen Sie nach der E-Mail-Adresse des Benutzers, und fügen Sie sie hinzu. [Weitere Informationen zu zertifikatbezogenen Zugriffsrichtlinien](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates#certificate-access-control)


### <a name="how-can-i-resolve-error-type-conflict-when-creating-a-certificate"></a>Wie kann ich den folgenden Fehlertyp beheben: „Conflict when creating a certificate“ (Konflikt beim Erstellen eines Zertifikats)?
    
Der Zertifikatname muss eindeutig sein. Ein Zertifikat mit dem gleichen Namen befindet sich ggf. im vorläufig gelöschten Zustand. Gemäß der [Zusammensetzung eines Zertifikats](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates#composition-of-a-certificate) in Azure Key Vault schlägt der Vorgang fehl, wenn es in Key Vault einen weiteren Schlüssel oder Geheimnis mit dem gleichen Namen gibt, den bzw. das Sie für Ihr Zertifikat angeben möchten, und Sie müssen entweder diesen Schlüssel oder das Geheimnis entfernen oder einen anderen Namen für Ihr Zertifikat verwenden. [Anzeigen eines gelöschten Zertifikats](https://docs.microsoft.com/rest/api/keyvault/getdeletedcertificate/getdeletedcertificate)

### <a name="why-am-i-getting-the-error-type-char-length-is-too-long"></a>Warum erhalte ich den Fehlertyp „char length is too long“ (Zeichenlänge ist zu lang)?
    
* Die Länge des Antragstellernamens des Zertifikats ist auf 200 Zeichen beschränkt.
* Die Länge des Kennworts des Zertifikats ist auf 200 Zeichen beschränkt.

### <a name="can-i-import-an-expired-certificate-in-azure-key-vault"></a>Kann ich ein abgelaufenes Zertifikat in Azure Key Vault importieren?
    
Nein, abgelaufene PFX-Zertifikate können nicht in Azure Key Vault importiert werden.

### <a name="how-can-i-convert-my-certificate-to-proper-format"></a>Wie kann ich mein Zertifikat in das richtige Format konvertieren?

Sie können Ihre Zertifizierungsstelle bitten, das Zertifikat im erforderlichen Format bereitzustellen. Es gibt auch Tools von Drittanbietern, mit denen Sie das Zertifikat in das richtige Format konvertieren können. Microsoft wird keine weiteren Informationen dazu bereitstellen, wie Sie das Zertifikat im gewünschten Format erhalten.

### <a name="can-i-import-certificates-from-non-partner-cas"></a>Kann ich Zertifikate von Zertifizierungsstellen importieren, die keine Partner sind?
Ja, Sie können Zertifikate von einer beliebigen Zertifizierungsstelle importieren, aber Key Vault kann diese Zertifikate nicht automatisch erneuern. Sie können E-Mail-Benachrichtigungen festlegen, um vor dem Ablauf des Zertifikats benachrichtigt zu werden.

### <a name="if-i-import-a-certificate-from-a-partner-ca-will-the-auto-renew-feature-still-work"></a>Wenn ich ein Zertifikat von einer Partnerzertifizierungsstelle importiere, funktioniert die Funktion zur automatischen Erneuerung weiterhin?
Ja, Sie müssen sicherstellen, dass Sie nach dem Hochladen die automatische Rotation in der Ausstellungsrichtlinie des Zertifikats angeben. Außerdem werden die Änderungen bis zum nächsten Zyklus oder der nächsten Zertifikatversion widergespiegelt.

### <a name="unable-to-see-the-app-service-certificate-imported-to-key-vault"></a>Wird das in Key Vault importierte App Service Certificate nicht angezeigt? 
Wenn das Zertifikat erfolgreich importiert wurde, können Sie es auf dem Blatt „Geheimnisse“ überprüfen.


## <a name="next-steps"></a>Nächste Schritte

- [Azure Key Vault-Zertifikate](/azure/key-vault/certificates/about-certificates)
