---
title: Informationen zur Azure Key Vault-Zertifikaterneuerung
description: In diesem Artikel wird beschrieben, wie Sie Azure Key Vault-Zertifikate verlängern.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: overview
ms.date: 07/20/2020
ms.author: sebansal
ms.openlocfilehash: 3809fa9e1ce17a5a0c3cf333ac20ef543db4b5a7
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88588802"
---
# <a name="renew-your-azure-key-vault-certificates"></a>Verlängern Ihrer Azure Key Vault-Zertifikate

Azure Key Vault ermöglicht Ihnen das einfache Bereitstellen und Verwalten von digitalen Zertifikaten für Ihr Netzwerk und das Aktivieren der sicheren Kommunikation für Ihre Anwendungen. Weitere Informationen zu Zertifikaten finden Sie unter [Informationen zu Azure Key Vault-Zertifikaten](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates).

Indem Sie Zertifikate mit kurzer Lebensdauer verwenden oder die Häufigkeit der Zertifikatrotation erhöhen, können Sie verhindern, dass nicht autorisierte Benutzer auf Ihre Anwendungen zugreifen.

In diesem Artikel wird beschrieben, wie Sie Ihre Azure Key Vault-Zertifikate verlängern.

## <a name="get-notified-about-certificate-expirations"></a>Erhalten von Benachrichtigungen zum Zertifikatablauf
Gehen Sie wie folgt vor, um benachrichtigt zu werden, wenn Ihre Zertifikate in Kürze ablaufen:

Fügen Sie Ihrem Schlüsseltresor zunächst eine Kontaktperson für Zertifikate hinzu, indem Sie das PowerShell-Cmdlet [Add-AzureKeyVaultCertificateContact](https://docs.microsoft.com/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificatecontact?view=azurermps-6.13.0) verwenden.

Konfigurieren Sie als Nächstes, wann Sie über den Ablauf des Zertifikats benachrichtigt werden möchten. Informationen zum Konfigurieren der Lebenszyklusattribute des Zertifikats finden Sie unter [Konfigurieren der automatischen Zertifikatrotation in Key Vault](https://docs.microsoft.com/azure/key-vault/certificates/tutorial-rotate-certificates#update-lifecycle-attributes-of-a-stored-certificate).

In Key Vault gibt es drei Kategorien von Zertifikaten:
-   Zertifikate, die mit einer integrierten Zertifizierungsstelle (ZS) erstellt werden, z. B. DigiCert oder GlobalSign
-   Zertifikate, die mit einer nicht integrierten Zertifizierungsstelle erstellt werden
-   Selbstsignierte Zertifikate

## <a name="renew-an-integrated-ca-certificate"></a>Verlängern eines Zertifikats mit einer integrierten Zertifizierungsstelle 
Azure Key Vault führt die End-to-End-Wartung von Zertifikaten durch, die von den vertrauenswürdigen Microsoft-Zertifizierungsstellen DigiCert und GlobalSign ausgestellt werden. Erfahren Sie, wie Sie eine [vertrauenswürdige Zertifizierungsstelle in Key Vault integrieren](https://docs.microsoft.com/azure/key-vault/certificates/how-to-integrate-certificate-authority).

## <a name="renew-a-nonintegrated-ca-certificate"></a>Verlängern eines Zertifikats mit einer nicht integrierten Zertifizierungsstelle 
Mit Azure Key Vault können Sie Zertifikate von einer beliebigen Zertifizierungsstelle importieren. Dieser Vorteil ermöglicht Ihnen die Integration in mehrere Azure-Ressourcen und eine einfache Bereitstellung. Falls Sie Bedenken haben, dass Sie den Überblick über Ihre Ablaufdaten der Zertifikate verlieren, oder ein Zertifikat bereits abgelaufen ist, können Sie die Schlüsseltresorfunktionen nutzen, um auf dem Laufenden gehalten zu werden. Bei Zertifikaten mit einer nicht integrierten Zertifizierungsstelle können Sie über den Schlüsseltresor E-Mail-Benachrichtigungen einrichten, die kurz vor dem Ablauf gesendet werden. Diese Benachrichtigungen können auch für mehrere Benutzer festgelegt werden.

> [!IMPORTANT]
> Ein Zertifikat ist ein Objekt mit Versionsangabe. Wenn die aktuelle Version abläuft, müssen Sie eine neue Version erstellen. Vom Konzept her handelt es sich bei jeder neuen Version um ein neues Zertifikat, das aus einem Schlüssel und einem Blob besteht, mit dem der Schlüssel mit einer Identität verknüpft wird. Bei Verwendung einer Zertifizierungsstelle, mit der keine Partnerschaft besteht, wird vom Schlüsseltresor ein Schlüssel-Wert-Paar generiert und eine Zertifikatsignieranforderung (Certificate Signing Request, CSR) zurückgegeben.

Gehen Sie wie folgt vor, um ein Zertifikat mit einer nicht integrierten Zertifizierungsstelle zu verlängern:

1. Melden Sie sich beim Azure-Portal an, und öffnen Sie dann das Zertifikat, das Sie verlängern möchten.
1. Wählen Sie im Zertifikatbereich die Option **Neue Version** aus.
1. Wählen Sie **Zertifikatvorgang** aus.
1. Wählen Sie die Option **CSR herunterladen** aus, um eine CSR-Datei auf Ihr lokales Laufwerk herunterzuladen.
1. Senden Sie die CSR an die Zertifizierungsstelle Ihrer Wahl, um die Anforderung zu signieren.
1. Verwenden Sie die signierte Anforderung, und wählen Sie auf demselben Bildschirm „Zertifikatvorgang“ die Option **CSR mergen** aus.

> [!NOTE]
> Es ist wichtig, dass Sie die signierte CSR mit derselben von Ihnen erstellten CSR-Anforderung mergen. Andernfalls ergibt sich für den Schlüssel keine Übereinstimmung.

Weitere Informationen zum Erstellen einer neuen CSR finden Sie unter [Erstellen und Zusammenführen einer Zertifikatsignieranforderung in Key Vault]( https://docs.microsoft.com/azure/key-vault/certificates/create-certificate-signing-request#azure-portal).

## <a name="renew-a-self-signed-certificate"></a>Verlängern eines selbstsignierten Zertifikats

Von Azure Key Vault wird auch die automatische Verlängerung von selbstsignierten Zertifikaten durchgeführt. Weitere Informationen zur Änderung der Ausstellungsrichtlinie und Aktualisierung der Lebenszyklusattribute eines Zertifikats finden Sie unter [Konfigurieren der automatischen Zertifikatrotation in Key Vault](https://docs.microsoft.com/azure/key-vault/certificates/tutorial-rotate-certificates#update-lifecycle-attributes-of-a-stored-certificate).

## <a name="troubleshoot"></a>Problembehandlung
Falls sich das ausgestellte Zertifikat im Azure-Portal im Status *Deaktiviert* befindet, sollten Sie unter **Zertifikatvorgang** die Fehlermeldung für das Zertifikat anzeigen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wie kann ich das Feature für die automatische Rotation des Zertifikats testen?**

Erstellen Sie ein Zertifikat mit einer Gültigkeitsdauer von **1 Monat**, und legen Sie die Lebensdaueraktion für die Rotation dann auf **1 %** fest. Bei dieser Einstellung wird das Zertifikat alle 7,2 Stunden rotiert.
  
**Werden die Tags nach der automatischen Verlängerung des Zertifikats repliziert?**

Ja. Die Tags werden nach der automatischen Verlängerung repliziert.

## <a name="next-steps"></a>Nächste Schritte
*   [Integrieren von Key Vault in die DigiCert-Zertifizierungsstelle](how-to-integrate-certificate-authority.md)
*   [Tutorial: Konfigurieren der automatischen Zertifikatrotation in Key Vault](tutorial-rotate-certificates.md)
