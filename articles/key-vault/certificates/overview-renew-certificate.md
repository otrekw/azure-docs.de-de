---
title: Informationen zur Azure Key Vault-Zertifikaterneuerung
description: Informationen zur Azure Key Vault-Zertifikaterneuerung
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: conceptual
ms.date: 07/20/2020
ms.author: sebansal
ms.openlocfilehash: 41085ee629189c32c1bc7196f23805c9c48d154a
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056277"
---
# <a name="about-azure-key-vault-certificate-renewal"></a>Informationen zur Azure Key Vault-Zertifikaterneuerung

Azure Key Vault ermöglicht das einfache Bereitstellen und Verwalten von digitalen Zertifikaten für Ihr Netzwerk und das Aktivieren der sicheren Kommunikation für Anwendungen. Weitere allgemeine Informationen zu Zertifikaten finden Sie unter [Azure Key Vault-Zertifikate](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates).

Die Verwendung von kurzlebigen Zertifikaten oder die Erhöhung der Häufigkeit der Zertifikatrotation schränkt den Schadensspielraum eines potenziellen Gegners ein.

## <a name="certificate-expiration-notifications"></a>Benachrichtigungen zum Zertifikatablauf
Fügen Sie Ihrem Schlüsseltresor unbedingt Kontaktinformationen für das Zertifikat hinzu, damit Sie benachrichtigt werden, wenn Zertifikate ablaufen (z. B. mit dem PowerShell-Befehl [Add-AzureKeyVaultCertificateContact](https://docs.microsoft.com/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificatecontact?view=azurermps-6.13.0)). Konfigurieren Sie außerdem, wann Sie über den Zertifikatablauf informiert werden möchten. Informationen zum Konfigurieren eines Lebensdauer-Aktionstyps finden Sie [hier](https://docs.microsoft.com/azure/key-vault/certificates/tutorial-rotate-certificates#update-lifecycle-attributes-of-a-stored-certificate).

In Key Vault sind drei Kategorien der Zertifikaterstellung verfügbar. Dieser Leitfaden hilft Ihnen zu verstehen, wie die Erneuerung von Zertifikaten erreicht werden kann.
-   Mit integrierter Zertifizierungsstelle erstellte Zertifikate (DigiCert oder GlobalSign)
-   Zertifikate, die mit einer nicht integrierten Zertifizierungsstelle erstellt wurden
-   Selbstsignierte Zertifikate

## <a name="renewal-of-integrated-ca-certificate"></a>Erneuerung des integrierten Zertifizierungsstellenzertifikats 
Gute Nachrichten! Azure Key Vault übernimmt die End-to-End-Verwaltung von Zertifikaten, die von Zertifizierungsstellen ausgegeben werden, denen Microsoft vertraut, d. h. DigiCert und GlobalSign. Erfahren Sie, wie Sie eine [vertrauenswürdige Zertifizierungsstelle in Key Vault integrieren](https://docs.microsoft.com/azure/key-vault/certificates/how-to-integrate-certificate-authority).

## <a name="renewal-of-non-integrated-ca-certificate"></a>Erneuerung des nicht integrierten Zertifizierungsstellenzertifikats 
Azure Key Vault bietet Benutzern den Vorteil, Zertifikate von jeder Zertifizierungsstelle (CA) zu importieren, um die Integration in verschiedene Azure-Ressourcen zu ermöglichen und die Bereitstellung zu erleichtern. Wenn Sie befürchten, den Überblick über den Ablauf Ihres Zertifikats zu verlieren, oder noch schlimmer, wenn Sie feststellen, dass Ihr Zertifikat bereits abgelaufen ist, kann Key Vault Ihnen helfen, auf dem neuesten Stand zu bleiben. Bei einem Zertifikat einer nicht integrierten Zertifizierungsstelle ermöglicht Key Vault dem Benutzer das Einrichten von E-Mail-Benachrichtigungen vor dem Ablauf. Diese Benachrichtigungen können auch für mehrere Benutzer festgelegt werden.

Um ein Zertifikat zu erneuern, ist es wichtig zu verstehen, dass ein Azure Key Vault-Zertifikat ein Objekt mit Versionsangabe ist. Wenn die aktuelle Version abläuft, müssen Sie eine neue Version erstellen. Konzeptionell wäre jede neue Version ein neues Zertifikat, das aus einem Schlüssel und einem Blob besteht, das diesen Schlüssel an eine Identität bindet. Wenn Sie eine Zertifizierungsstelle verwenden, die kein Partner ist, generiert Key Vault ein Schlüssel-Wert-Paar und gibt die CSR-Datei zurück.

**Schritte, die in Azure-Portal ausgeführt werden müssen:**
1.  Öffnen Sie das Zertifikat, das Sie erneuern möchten.
2.  Wählen Sie auf dem Bildschirm „Zertifikat“ die Schaltfläche **+ Neue Version** aus.
3.  Wählen Sie **Zertifikatvorgang** aus.
4.  Wählen Sie **CSR herunterladen** aus. Dadurch wird eine CSR-Datei auf das lokale Laufwerk heruntergeladen.
5.  Aktivieren Sie die CSR-Datei für die Zertifizierungsstelle Ihrer Wahl, um die Anforderung zu signieren.
6.  Verwenden Sie die signierte Anforderung, und wählen Sie **CSR mergen** auf demselben Bildschirm „Zertifikatvorgang“ aus.

> [!NOTE]
> Es ist wichtig, die signierte CSR-Datei mit der gleichen CSR-Anforderung zu mergen, die erstellt wurde. Andernfalls würde der Schlüssel nicht übereinstimmen.

Die Schritte ähneln dem Erstellen eines neuen Zertifikats und werden [hier]( https://docs.microsoft.com/azure/key-vault/certificates/create-certificate-signing-request#azure-portal) ausführlicher dokumentiert.

## <a name="renewal-of-self-signed-certificate"></a>Erneuerung eines selbstsignierten Zertifikats

Schon wieder gute Nachrichten! Azure Key Vault übernimmt auch die automatische Erneuerung von selbstsignierten Zertifikaten für Benutzer. Weitere Informationen zum Ändern der Ausstellungsrichtlinie und zum Aktualisieren der Lebensdauer-Aktionsattribute des Zertifikats finden Sie [hier](https://docs.microsoft.com/azure/key-vault/certificates/tutorial-rotate-certificates#update-lifecycle-attributes-of-a-stored-certificate).

### <a name="troubleshoot"></a>Problembehandlung
Wenn das ausgestellte Zertifikat im Azure-Portal den Status „Deaktiviert“ aufweist, fahren Sie mit der Anzeige des Zertifikatvorgangs fort, um die Fehlermeldung für dieses Zertifikat zu überprüfen.

### <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
Werden die Tags nach der automatischen Verlängerung des Zertifikats repliziert?
Nein, Tags werden nicht repliziert, es sei denn, Benutzer kopieren sie manuell.

### <a name="see-also"></a>Weitere Informationen
*   [Integrieren von Key Vault in die DigiCert-Zertifizierungsstelle](how-to-integrate-certificate-authority.md)
*   [Tutorial: Konfigurieren der automatischen Zertifikatrotation in Key Vault](tutorial-rotate-certificates.md)
