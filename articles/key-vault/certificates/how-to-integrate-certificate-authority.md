---
title: Integrieren von Key Vault in die Zertifizierungsstelle DigiCert
description: In diesem Artikel wird beschrieben, wie Sie Key Vault und die Zertifizierungsstelle DigiCert integrieren, damit Sie Zertifikate für Ihr Netzwerk bereitstellen, verwalten und bereitstellen können.
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: how-to
ms.date: 06/02/2020
ms.author: sebansal
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 9cec61df061c7a504da412851349064273507173
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110678854"
---
# <a name="integrating-key-vault-with-digicert-certificate-authority"></a>Integrieren von Key Vault in die Zertifizierungsstelle DigiCert

Azure Key Vault ermöglicht das einfache Bereitstellen und Verwalten von digitalen Zertifikaten für Ihr Netzwerk und das Aktivieren der sicheren Kommunikation für Anwendungen. Ein digitales Zertifikat ist eine elektronische Anmeldeinformation, um bei einer elektronischen Transaktion einen Identitätsnachweis zu erbringen. 

Azure Key Vault-Benutzer können DigiCert-Zertifikate direkt aus ihren Schlüsseltresoren generieren. Key Vault hat eine vertrauenswürdige Partnerschaft mit der Zertifizierungsstelle DigiCert. Diese Partnerschaft stellt die End-to-End-Verwaltung des Lebenszyklus für von DigiCert ausgestellte Zertifikate sicher.

Weitere allgemeine Informationen zu Zertifikaten finden Sie unter [Informationen zu Azure Key Vault-Zertifikaten](./about-certificates.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Die in diesem Artikel aufgeführten Verfahren setzen Folgendes voraus:
* Einen Schlüsseltresor. Sie können einen vorhandenen Schlüsseltresor verwenden oder einen erstellen, indem Sie die Schritte in einem dieser Schnellstarts ausführen:
   - [Erstellen eines Schlüsseltresors über die Azure CLI](../general/quick-create-cli.md)
   - [Erstellen eines Schlüsseltresors mit Azure PowerShell](../general/quick-create-powershell.md)
   - [Erstellen eines Schlüsseltresors über das Azure-Portal](../general/quick-create-portal.md)
*   Ein aktiviertes DigiCert CertCentral-Konto. [Registrieren Sie sich](https://www.digicert.com/account/signup/) für Ihr CertCentral-Konto.
*   Berechtigungen auf Administratorebene in Ihren Konten.


### <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie sicher, dass Ihnen die folgenden Informationen in Ihrem DigiCert CertCentral-Konto vorliegen:
-   CertCentral-Konto-ID
-   Organisations-ID
-   API-Schlüssel

## <a name="add-the-certificate-authority-in-key-vault"></a>Hinzufügen der Zertifizierungsstelle in Key Vault 
Nachdem Sie die vorstehenden Informationen aus Ihrem DigiCert CertCentral-Konto abgerufen haben, können Sie DigiCert zur Liste der Zertifizierungsstellen im Schlüsseltresor hinzufügen.

### <a name="azure-portal"></a>Azure-Portal

1.  Um die Zertifizierungsstelle DigiCert hinzuzufügen, navigieren Sie zum Schlüsseltresor, dem Sie sie hinzufügen möchten. 
2.  Wählen Sie auf der Eigenschaftenseite von Key Vault **Zertifikate** aus.
3.  Wählen Sie die Registerkarte **Zertifizierungsstellen** aus: :::image type="content" source="../media/certificates/how-to-integrate-certificate-authority/select-certificate-authorities.png" alt-text="Screenshot der Auswahl der Registerkarte „Zertifizierungsstellen“.":::
4.  Wählen Sie **Hinzufügen** aus: :::image type="content" source="../media/certificates/how-to-integrate-certificate-authority/add-certificate-authority.png" alt-text="Screenshot der Schaltfläche „Hinzufügen“ auf der Registerkarte „Zertifizierungsstellen“.":::
5.  Geben Sie unter **Zertifizierungsstelle erstellen** diese Werte ein:
    -   **Name**: ein identifizierbarer Ausstellername. Beispiel: **DigicertCA**.
    -   **Anbieter**: **DigiCert**.
    -   **Konto-ID:** Ihre DigiCert CertCentral-Konto-ID.
    -   **Kontokennwort**: der API-Schlüssel, den Sie in Ihrem DigiCert CertCentral-Konto erstellt haben.
    -   **Organisations-ID**: die Organisations-ID Ihres DigiCert CertCentral-Kontos. 

1. Klicken Sie auf **Erstellen**.
   
DigicertCA ist jetzt in der Liste der Zertifizierungsstelle enthalten.


### <a name="azure-powershell"></a>Azure PowerShell

Sie können mit Azure PowerShell sowie Befehlen oder Skripts Azure-Ressourcen erstellen und verwalten. Azure hostet Azure Cloud Shell, eine interaktive Shellumgebung, die Sie über das Azure-Portal in einem Browser nutzen können.

Wenn Sie sich für die lokale Installation und Verwendung von PowerShell entscheiden, benötigen Sie mindestens das Azure AZ PowerShell-Modul 1.0.0, um die hier beschriebenen Schritte durchzuführen. Geben Sie `$PSVersionTable.PSVersion` ein, um die Version zu bestimmen. Wenn ein Upgrade erforderlich ist, finden Sie weitere Informationen unter [Installieren des Azure AZ PowerShell-Moduls](/powershell/azure/install-az-ps). Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen:

```azurepowershell-interactive
Login-AzAccount
```

1.  Erstellen Sie mit [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

    ```azurepowershell-interactive
    New-AzResourceGroup -Name ContosoResourceGroup -Location EastUS
    ```

2. Erstellen Sie einen Schlüsseltresor mit einem eindeutigen Namen. Hier ist `Contoso-Vaultname` der Name des Schlüsseltresors.

   - **Tresorname**: `Contoso-Vaultname`
   - **Ressourcengruppenname**: `ContosoResourceGroup`
   - **Standort**: `EastUS`

    ```azurepowershell-interactive
    New-AzKeyVault -Name 'Contoso-Vaultname' -ResourceGroupName 'ContosoResourceGroup' -Location 'EastUS'
   ```

3. Legen Sie für die folgenden Werte in Ihrem DigiCert CertCentral-Konto Variablen fest:

   - **Konto-ID** 
   - **Organisations-ID** 
   - **API-Schlüssel** 

   ```azurepowershell-interactive
   $accountId = "myDigiCertCertCentralAccountID"
   $org = New-AzKeyVaultCertificateOrganizationDetail -Id OrganizationIDfromDigiCertAccount
   $secureApiKey = ConvertTo-SecureString DigiCertCertCentralAPIKey -AsPlainText –Force
   ```

4. Legen Sie den Zertifikataussteller fest. Dadurch wird DigiCert als Zertifizierungsstelle im Schlüsseltresor hinzugefügt. [Erfahren Sie mehr über die Parameter.](/powershell/module/az.keyvault/Set-AzKeyVaultCertificateIssuer)
   ```azurepowershell-interactive
   Set-AzKeyVaultCertificateIssuer -VaultName "Contoso-Vaultname" -Name "TestIssuer01" -IssuerProvider DigiCert -AccountId $accountId -ApiKey $secureApiKey -OrganizationDetails $org -PassThru
   ```

5. Legen Sie die Richtlinie für das Zertifikat und das Ausstellen des Zertifikats von DigiCert direkt in Key Vault fest:

   ```azurepowershell-interactive
   $Policy = New-AzKeyVaultCertificatePolicy -SecretContentType "application/x-pkcs12" -SubjectName "CN=contoso.com" -IssuerName "TestIssuer01" -ValidityInMonths 12 -RenewAtNumberOfDaysBeforeExpiry 60
   Add-AzKeyVaultCertificate -VaultName "Contoso-Vaultname" -Name "ExampleCertificate" -CertificatePolicy $Policy
   ```

Das Zertifikat wird nun von der Zertifizierungsstelle DigiCert im angegebenen Schlüsseltresor ausgestellt.

## <a name="troubleshoot"></a>Problembehandlung

Wenn das ausgestellte Zertifikat im Azure-Portal den Status „Deaktiviert“ hat, zeigen Sie den Zertifikatvorgang an, um die DigiCert-Fehlermeldung für dieses Zertifikat zu überprüfen:

:::image type="content" source="../media/certificates/how-to-integrate-certificate-authority/certificate-operation-select.png" alt-text="Screenshot der Registerkarte „Zertifikatvorgang“.":::

Fehlermeldung: „Führen Sie eine Zusammenführung aus, um diese Zertifikatanforderung abzuschließen.“
   
Führen Sie die von der Zertifizierungsstelle signierte Zertifikatsignieranforderung (CSR) zusammen. Weitere Informationen zum Zusammenführen einer Zertifikatsignieranforderung finden Sie unter [Erstellen und Zusammenführen einer Zertifikatsignieranforderung in Key Vault](./create-certificate-signing-request.md).

Weitere Informationen finden Sie unter den [Zertifikatvorgängen in der Referenz zur REST-API für Azure Key Vault](/rest/api/keyvault). Informationen zum Einrichten von Berechtigungen finden Sie unter [Tresore – Erstellen oder Aktualisieren](/rest/api/keyvault/vaults/createorupdate) und [Tresore – Aktualisieren der Zugriffsrichtlinie](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

- **Kann ich mit Key Vault ein DigiCert-Platzhalterzertifikat generieren?** 
   
  Ja, dies hängt jedoch davon ab, wie Sie Ihr DigiCert-Konto konfiguriert haben.
- **Wie kann ich ein OV SSL- oder EV SSL-Zertifikat mit DigiCert erstellen?**
 
   Key Vault unterstützt die Erstellung von OV- und EV SSL-Zertifikaten. Wenn Sie ein Zertifikat erstellen, wählen Sie **Erweiterte Richtlinienkonfiguration** aus, und geben Sie dann den Zertifikatstyp an. Unterstützte Werte: OV SSL, EV SSL
   
   Sie können dieser Typ von Zertifikat in Key Vault erstellen, sofern Ihr DigiCert-Konto dies zulässt. Bei diesem Zertifikatstyp erfolgt die Validierung durch DigiCert. Wenn die Validierung fehlschlägt, kann das DigiCert-Supportteam helfen. Sie können beim Erstellen eines Zertifikats Informationen hinzufügen, indem Sie die Informationen in `subjectName` festlegen.

  Beispiel: `SubjectName="CN = docs.microsoft.com, OU = Microsoft Corporation, O = Microsoft Corporation, L = Redmond, S = WA, C = US"`.
   
- **Dauert es länger, ein DigiCert-Zertifikat über die Integration zu erstellen, als es direkt bei DigiCert zu beziehen?**
   
   Nein. Wenn Sie ein Zertifikat erstellen, kann der Überprüfungsprozess Zeit in Anspruch nehmen. DigiCert steuert diesen Prozess.


## <a name="next-steps"></a>Nächste Schritte

- [Authentifizierung, Anforderungen und Antworten](../general/authentication-requests-and-responses.md)
- [Entwicklerhandbuch für Key Vault](../general/developers-guide.md)
