---
title: Konfigurieren des einmaligen Anmeldens mit AD FS für Azure Virtual Desktop – Azure
description: Hier erfahren Sie, wie Sie das einmalige Anmelden mit AD FS für eine Azure Virtual Desktop-Umgebung konfigurieren.
services: virtual-desktop
author: Heidilohr
manager: lizross
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 06/30/2021
ms.author: helohr
ms.openlocfilehash: 3c6e61754d9332cbdfbea6b971363c1b0d6cb4fe
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114289420"
---
# <a name="configure-ad-fs-single-sign-on-for-azure-virtual-desktop"></a>Konfigurieren des einmaligen Anmeldens mit AD FS für Azure Virtual Desktop

Dieser Artikel führt Sie durch die Konfiguration des einmaligen Anmeldens (SSO) mit AD FS (Active Directory-Verbunddienst) für Azure Virtual Desktop.

> [!NOTE]
> Dieses Feature wird von Azure Virtual Desktop (Classic) nicht unterstützt.

## <a name="requirements"></a>Requirements (Anforderungen)

Bevor Sie das einmalige Anmelden mit AD FS konfigurieren können, muss in Ihrer Umgebung das folgende Setup ausgeführt werden:

* Sie müssen die Rolle **Active Directory-Zertifikatdienste (CA)** bereitstellen. Alle Server, auf denen die Rolle ausgeführt wird, müssen in die Domäne eingebunden und als [Unternehmenszertifizierungsstellen](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731183%28v%3dws.10%29) konfiguriert sein. Darüber hinaus müssen auf ihnen die neuesten Windows-Updates installiert sein.
* Sie müssen die Rolle **Active Directory-Verbunddienste (AD FS)** bereitstellen. Alle Server, auf denen diese Rolle ausgeführt wird, müssen in die Domäne eingebunden sein. Darüber hinaus müssen auf ihnen die neuesten Windows-Updates installiert sein und Windows Server 2016 oder höher ausgeführt werden. Informationen über die ersten Schritte beim Einrichten dieser Rolle finden Sie in unserem [Verbundtutorial](../active-directory/hybrid/tutorial-federation.md).
* Es wird empfohlen, die Rolle **Web-Anwendungsproxy** einzurichten, um die Verbindung Ihrer Umgebung mit den AD FS-Servern zu schützen. Auf allen Servern, auf denen diese Rolle ausgeführt wird, müssen die neuesten Windows-Updates installiert sein und Windows Server 2016 oder höher ausgeführt werden. Informationen über die ersten Schritte beim Einrichten dieser Rolle finden Sie in diesem [Web-Anwendungsproxy-Leitfaden](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383662(v=ws.11)).
* Sie müssen **Azure AD Connect** bereitstellen, um Benutzer mit Azure AD synchronisieren zu können. Azure AD Connect muss im [Verbundmodus](../active-directory/hybrid/how-to-connect-install-custom.md) konfiguriert werden.
* [Richten Sie Ihre PowerShell-Umgebung](powershell-module.md) für Azure Virtual Desktop auf dem AD FS-Server ein.
* Wenn Sie Windows 10 20H1 oder 20H2 verwenden, um eine Verbindung mit Azure Virtual Desktop herzustellen, müssen Sie das **kumulative Update 2021-04 für Windows 10 (KB5001330)** oder höher installieren, damit das einmalige Anmelden ordnungsgemäß funktioniert.

> [!NOTE]
> Diese Lösung wird von Azure AD Domain Services nicht unterstützt. Sie müssen einen Domänencontroller mit einer Active Directory-Domäne verwenden.

## <a name="supported-clients"></a>Unterstützte Clients

Die folgenden Azure Virtual Desktop-Clients unterstützen dieses Feature:

* [Windows Desktop-Client](./user-documentation/connect-windows-7-10.md)
* [Webclient](./user-documentation/connect-web.md)

## <a name="configure-the-certificate-authority-to-issue-certificates"></a>Konfigurieren der Zertifizierungsstelle für das Ausstellen von Zertifikaten

Die folgenden Zertifikatvorlagen müssen ordnungsgemäß erstellt werden, damit AD FS das einmalige Anmelden verwenden kann:

* Zunächst müssen Sie die Zertifikatvorlage für den **Exchange-Registrierungs-Agent (Offline-Request)** erstellen. AD FS verwendet die Zertifikatvorlage für den Exchange-Registrierungs-Agent, um im Namen des Benutzers Zertifikate anzufordern.
* Darüber hinaus müssen Sie die Zertifikatvorlage für die **Smartcard-Anmeldung** erstellen. Diese wird von AD FS zum Erstellen des Anmeldezertifikats verwendet.

Nachdem Sie diese Zertifikatvorlagen erstellt haben, müssen Sie die Vorlagen für die Zertifizierungsstelle aktivieren, damit AD FS sie anfordern kann.

> [!NOTE]
> Durch diese Lösung werden neue kurzfristige Zertifikate für jede Benutzeranmeldung generiert. Wenn Sie viele Benutzer haben, kann dadurch die Datenbank der Zertifizierungsstelle im Laufe der Zeit überfüllt werden. Sie können dies vermeiden, indem Sie [eine Zertifizierungsstelle für die Verarbeitung nicht persistenter Zertifikate einrichten](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff934598(v=ws.10)).

### <a name="create-the-enrollment-agent-certificate-template"></a>Erstellen der Zertifikatvorlage für den Registrierungs-Agent

Je nach Umgebung haben Sie möglicherweise bereits eine Zertifikatvorlage für den Registrierungs-Agent konfiguriert, die für andere Zwecke wie z. B. Windows Hello for Business, Anmeldezertifikate oder VPN-Zertifikate genutzt wird. In diesem Fall müssen Sie diese Vorlage anpassen, damit sie das einmalige Anmelden unterstützt. Andernfalls können Sie auch eine neue Vorlage erstellen.

Um zu ermitteln, ob Sie bereits eine Zertifikatvorlage für den Registrierungs-Agent verwenden, führen Sie den folgenden PowerShell-Befehl auf dem AD FS-Server aus, und überprüfen Sie, ob ein Wert zurückgegeben wird. Erstellen Sie eine neue Zertifikatvorlage für den Registrierungs-Agent, wenn kein Wert zurückgegeben wird. Wenn ein Wert zurückgegeben wird, sollten Sie sich den Namen des Agents merken, um die entsprechende vorhandene Zertifikatvorlage zu aktualisieren.

```powershell
Import-Module adfs
(Get-AdfsCertificateAuthority).EnrollmentAgentCertificateTemplateName
```

So erstellen Sie eine neue Zertifikatvorlage für den Registrierungs-Agent:

1. Führen Sie **mmc.exe** aus dem Startmenü der Zertifizierungsstelle aus, um die **Microsoft Management Console** zu starten.
2. Wählen Sie **Datei...**  > **Hinzufügen/Entfernen Snap-In...**  > **Zertifikatvorlagen** > **Hinzufügen >**  > **OK** aus, um die Liste der Zertifikatvorlagen anzuzeigen.
3. Erweitern Sie die **Zertifikatvorlagen**, klicken Sie mit der rechten Maustaste auf **Exchange-Registrierungs-Agent (Offline-Request)** und wählen Sie **Vorlage duplizieren** aus.
4. Wählen Sie die Registerkarte **Allgemein** aus, und geben Sie dann „AD FS-Registrierungs-Agent“ in das Feld **Anzeigename der Vorlage** ein. Dadurch wird als Name der Vorlage automatisch „ADFSRegistrierungsAgent“ festgelegt.
5. Wählen Sie zunächst die Registerkarte **Sicherheit** und anschließend **Bearbeiten** aus.
6. Wählen Sie als Nächstes **Objekttypen...** , dann **Dienstkonten** und dann **OK** aus.
7. Geben Sie den Dienstkontonamen für AD FS ein und wählen Sie **OK** aus.
   * In einem isolierten AD FS-Setup erhält das Dienstkonto den Namen „adfssvc$“.
   * Wenn Sie AD FS über Azure AD Connect einrichten, erhält das Dienstkonto den Namen „aadcsvc$“.
8. Nachdem das Dienstkonto hinzugefügt wurde und auf der Registerkarte **Sicherheit** angezeigt wird, wählen Sie es im Bereich **Gruppen- oder Benutzernamen** aus. Wählen Sie dann sowohl für „Registrieren“ als auch für „Automatische Registrierung“ im Bereich **Berechtigungen für das AD FS Dienstkonto** die Option **Zulassen** aus, und klicken Sie abschließend auf **OK**, um das Konto zu speichern.

   :::image type="content" source="media/adfs-enrollment-properties-security.png" alt-text="Screenshot: Registerkarte „Sicherheit“ der Zertifikatvorlage des Registrierungs-Agents, nachdem dieser ordnungsgemäß konfiguriert wurde.":::

So aktualisieren Sie eine vorhandene Zertifikatvorlage für den Registrierungs-Agent:

1. Führen Sie **mmc.exe** aus dem Startmenü der Zertifizierungsstelle aus, um die **Microsoft Management Console** zu starten.
2. Wählen Sie **Datei...**  > **Hinzufügen/Entfernen Snap-In...**  > **Zertifikatvorlagen** > **Hinzufügen >**  > **OK** aus, um die Liste der Zertifikatvorlagen anzuzeigen.
3. Erweitern Sie die **Zertifikatvorlagen** und klicken Sie doppelt auf die Vorlage, die der auf dem AD FS-Server konfigurierten Vorlage entspricht. In der Registerkarte **Allgemein** sollte der Name der Vorlage mit dem Namen übereinstimmen, den Sie wie oben beschrieben ermittelt haben.
4. Wählen Sie zunächst die Registerkarte **Sicherheit** und anschließend **Bearbeiten** aus.
5. Wählen Sie als Nächstes **Objekttypen...** , dann **Dienstkonten** und dann **OK** aus.
6. Geben Sie den Dienstkontonamen für AD FS ein und wählen Sie **OK** aus.
   * In einem isolierten AD FS-Setup erhält das Dienstkonto den Namen „adfssvc$“.
   * Wenn Sie AD FS über Azure AD Connect einrichten, erhält das Dienstkonto den Namen „aadcsvc$“.
7. Nachdem das Dienstkonto hinzugefügt wurde und auf der Registerkarte **Sicherheit** angezeigt wird, wählen Sie es im Bereich **Gruppen- oder Benutzernamen** aus. Wählen Sie dann sowohl für „Registrieren“ als auch für „Automatische Registrierung“ im Bereich **Berechtigungen für das AD FS Dienstkonto** die Option **Zulassen** aus, und klicken Sie abschließend auf **OK**, um das Konto zu speichern.

### <a name="create-the-smartcard-logon-certificate-template"></a>Erstellen der Zertifikatvorlage für die Smartcard-Anmeldung

So erstellen Sie die Zertifikatvorlage für die Smartcard-Anmeldung:

1. Führen Sie **mmc.exe** aus dem Startmenü der Zertifizierungsstelle aus, um die **Microsoft Management Console** zu starten.
2. Wählen Sie **Datei...**  > **Hinzufügen/Entfernen Snap-In...**  > **Zertifikatvorlagen** > **Hinzufügen** > **OK** aus, um die Liste der Zertifikatvorlagen anzuzeigen.
3. Erweitern Sie die **Zertifikatvorlagen**, klicken Sie mit der rechten Maustaste auf **Smartcard-Anmeldung** und wählen Sie **Vorlage duplizieren** aus.
4. Wählen Sie die Registerkarte **Allgemein** aus, und geben Sie dann „AD FS SSO“ in das Feld **Anzeigename der Vorlage** ein. Dadurch wird als Name der Vorlage automatisch „ADFSSSO“ festgelegt.
   > [!NOTE]
   > Da dieses Zertifikat nur bei Bedarf angefordert wird, wird empfohlen, die Gültigkeitsdauer auf 8 Stunden und den Verlängerungszeitraum auf 1 Stunde zu verkürzen.

5. Wählen Sie zunächst die Registerkarte **Antragstellername** und dann **Informationen werden in der Anforderung angegeben** aus. Wenn eine Warnmeldung angezeigt wird, klicken Sie auf **OK**.

   :::image type="content" source="media/adfs-sso-properties-subject-inline.png" alt-text="Screenshot: Registerkarte „Antragstellername“ der SSO-Zertifikatvorlage mit ordnungsgemäßer Konfiguration" lightbox="media/adfs-sso-properties-subject-expanded.png":::.

6. Wählen Sie die Registerkarte **Ausstellungsvoraussetzungen** aus.
7. Wählen Sie **Diese Anzahl an autorisierten Signaturen** aus und geben Sie den Wert **1** ein.

   :::image type="content" source="media/adfs-sso-properties-issuance-inline.png" alt-text="Screenshot: Registerkarte „Ausstellungsvoraussetzungen“ der SSO-Zertifikatvorlage mit ordnungsgemäßer Konfiguration" lightbox="media/adfs-sso-properties-issuance-expanded.png":::.

8. Wählen Sie als **Anwendungsrichtlinie** die Option **Zertifikatanforderungs-Agent** aus.
9.  Wählen Sie zunächst die Registerkarte **Sicherheit** und anschließend **Bearbeiten** aus.
10. Wählen Sie **Objekttypen...** , dann **Dienstkonten** und dann **OK** aus.
11. Geben Sie wie im Abschnitt [Erstellen der Zertifikatvorlage für den Registrierungs-Agent](#create-the-enrollment-agent-certificate-template) den Dienstkontonamen für AD FS ein.
    * In einem isolierten AD FS-Setup erhält das Dienstkonto den Namen „adfssvc$“.
    * Wenn Sie AD FS über Azure AD Connect einrichten, erhält das Dienstkonto den Namen „aadcsvc$“.
12. Nachdem das Dienstkonto hinzugefügt wurde und auf der Registerkarte **Sicherheit** angezeigt wird, wählen Sie es im Bereich **Gruppen- oder Benutzernamen** aus. Wählen Sie dann sowohl für „Registrieren“ als auch für „Automatische Registrierung“ die Option **Zulassen** aus, und klicken Sie abschließend auf **OK**, um das Konto zu speichern.

   :::image type="content" source="media/adfs-sso-properties-security.png" alt-text="Screenshot: Registerkarte „Sicherheit“ der SSO-Zertifikatvorlage, nachdem dieser ordnungsgemäß konfiguriert wurde.":::

### <a name="enable-the-new-certificate-templates"></a>Aktivieren Sie die neuen Zertifikatvorlagen:

So aktivieren Sie die neuen Zertifikatvorlagen:

1. Führen Sie **mmc.exe** aus dem Startmenü der Zertifizierungsstelle aus, um die **Microsoft Management Console** zu starten.
2. Wählen Sie **Datei...**  > **Hinzufügen/Entfernen Snap-In...**  > **Zertifizierungsstelle** > **Hinzufügen >**  > **Fertigstellen** > und dann **OK** aus, um die Zertifizierungsstelle anzuzeigen.
3. Erweitern Sie die Zertifizierungsstelle im linken Bereich und öffnen Sie die **Zertifikatvorlagen**.
4. Klicken Sie mit der rechten Maustaste in den mittleren Bereich, in dem die Liste der Zertifikatvorlagen angezeigt wird, wählen Sie **Neu** und dann **Auszustellende Zertifikatvorlage** aus.
5. Wählen Sie sowohl **AD FS-Registrierungs-Agent** als auch **AD FS SSO** aus, und klicken Sie dann auf **OK**. Beide Vorlagen sollten im mittleren Bereich angezeigt werden.

   :::image type="content" source="media/adfs-certificate-templates.png" alt-text="Screenshot: Liste der Zertifikatvorlagen, die ausgestellt werden können, einschließlich des neuen AD FS-Registrierungs-Agents und AD FS SSO.":::

   > [!NOTE]
   > Wenn Sie bereits eine Zertifikatvorlage für den Registrierungs-Agent konfiguriert haben, müssen Sie nur die AD FS SSO-Vorlage hinzufügen.

## <a name="configure-the-ad-fs-servers"></a>Konfigurieren der AD FS-Server

Sie müssen die AD FS-Server (Active Directory-Verbunddienste) für die Verwendung der neuen Zertifikatvorlagen konfigurieren und die Vertrauensstellung der vertrauenden Seite so festlegen, dass das einmalige Anmelden unterstützt wird.

Die Vertrauensstellung der vertrauenden Seite zwischen Ihrem AD FS-Server und dem Azure Virtual Desktop-Dienst ermöglicht eine ordnungsgemäße Weiterleitung von SSO-Zertifikatanforderungen an Ihre Domänenumgebung.

Beim Konfigurieren des einmaligen Anmeldens mit AD FS müssen Sie einen gemeinsam verwendeten Schlüssel oder ein Zertifikat auswählen:

* Wenn Sie über einen einzelnen AD FS-Server verfügen, können Sie zwischen dem gemeinsam verwendeten Schlüssel und dem Zertifikat wählen.
* Wenn Sie über mehrere AD FS-Server verfügen, müssen Sie das Zertifikat wählen.

Der gemeinsam verwendete Schlüssel oder das Zertifikat, mit dem das Token für die Windows-Anmeldung generiert wird, muss sicher in [Azure Key Vault](../key-vault/general/overview.md) gespeichert werden. Sie können das Geheimnis in einem vorhandenen Key Vault speichern oder einen neuen Key Vault zu diesem Zweck bereitstellen. In beiden Fällen müssen Sie sicherstellen, dass Sie die richtige Zugriffsrichtlinie festlegen, damit der Azure Virtual Desktop-Dienst darauf zugreifen kann.

Wenn Sie das Zertifikat wählen, können Sie ein beliebiges universelles Zertifikat verwenden. Es besteht keine Anforderung an den Antragstellernamen oder an den alternativen Antragstellernamen (SAN). Es wird empfohlen, ein Zertifikat zu erstellen, das von einer gültigen Zertifizierungsstelle ausgestellt wurde. Dies ist allerdings nicht obligatorisch. Das Zertifikat kann direkt in Azure Key Vault erstellt werden und muss über einen exportierbaren privaten Schlüssel verfügen. Der öffentliche Schlüssel kann exportiert und dazu genutzt werden, den AD FS-Server mithilfe des folgenden Skripts zu konfigurieren. Beachten Sie, dass sich dieses Zertifikat von dem AD FS-SSL-Zertifikat unterscheidet, das einen ordnungsgemäßen Antragstellernamen und eine gültige Zertifizierungsstelle beinhalten muss.

Das im [PowerShell-Katalog](https://www.powershellgallery.com/packages/ConfigureWVDSSO) verfügbare PowerShell-Skript **ConfigureWVDSSO.ps1** konfiguriert Ihren AD FS-Server für die Vertrauensstellung der vertrauenden Seite und installiert bei Bedarf das Zertifikat.

Dieses Skript verfügt nur über einen erforderlichen Parameter: *ADFSAuthority*. Dabei handelt es sich um die URL, die in Ihre AD FS aufgelöst wird und „/adfs“ als Suffix verwendet. Beispielsweise `https://adfs.contoso.com/adfs`.

1. Führen Sie auf den AD FS-VMs das folgende PowerShell-Cmdlet aus, um AD FS für die Verwendung der Zertifikatvorlagen aus dem vorherigen Abschnitt zu konfigurieren:
  
   ```powershell
   Set-AdfsCertificateAuthority -EnrollmentAgentCertificateTemplate "ADFSEnrollmentAgent" -LogonCertificateTemplate "ADFSSSO" -EnrollmentAgent
   ```
 
   > [!NOTE]
   > Wenn Sie bereits eine Zertifikatvorlage für den Registrierungs-Agent konfiguriert haben, stellen Sie sicher, dass Sie den vorhandenen Vorlagennamen anstelle von ADFSRegistrierungsAgent verwenden.

2. Führen Sie das ConfigureWVDSSO.ps1-Skript aus.
   > [!NOTE]
   > Sie benötigen die Variablenwerte von `$config`, um den nächsten Teil der Anleitung abzuschließen. Deshalb dürfen Sie das PowerShell-Fenster, das Sie zum Ausführen der vorherigen Anweisungen verwendet haben, noch nicht schließen. Sie können entweder weiterhin dasselbe PowerShell-Fenster verwenden oder es geöffnet lassen, während Sie eine neue PowerShell-Sitzung starten.
   
   * Wenn Sie einen gemeinsam verwendeten Schlüssel im Key Vault nutzen, führen Sie das folgende PowerShell-Cmdlet auf dem AD FS-Server aus. Dabei muss die ADFSServiceUrl mit der vollständigen URL ersetzt werden, damit Ihr AD FS-Dienst erreicht werden kann:

     ```powershell
     Install-Script ConfigureWVDSSO
     $config = ConfigureWVDSSO.ps1 -ADFSAuthority "<ADFSServiceUrl>" [-WvdWebAppAppIDUri "<WVD Web App URI>"] [-RdWebURL "<RDWeb URL>"]
     ```

     > [!NOTE]
     > Sie benötigen die Eigenschaften WvdWebAppAppIDUri und RdWebURL, um eine Umgebung in einer Sovereign Cloud wie Azure Government zu konfigurieren. In der Azure Commercial Cloud werden diese Eigenschaften automatisch auf `https://www.wvd.microsoft.com` bzw. `https://rdweb.wvd.microsoft.com` festgelegt.

   * Wenn Sie ein Zertifikat im Key Vault nutzen, führen Sie das folgende PowerShell-Cmdlet auf dem AD FS-Server aus. Dabei muss die ADFSServiceUrl mit der vollständigen URL ersetzt werden, damit Ihr AD FS-Dienst erreicht werden kann:

     ```powershell
     Install-Script ConfigureWVDSSO
     $config = ConfigureWVDSSO.ps1 -ADFSAuthority "<ADFSServiceUrl>" -UseCert -CertPath "<Path to the pfx file>" -CertPassword <Password to the pfx file> [-WvdWebAppAppIDUri "<WVD Web App URI>"] [-RdWebURL "<RDWeb URL>"]
     ```

     > [!NOTE]
     > Sie benötigen die Eigenschaften WvdWebAppAppIDUri und RdWebURL, um eine Umgebung in einer Sovereign Cloud wie Azure Government zu konfigurieren. In der Azure Commercial Cloud werden diese Eigenschaften automatisch auf `https://www.wvd.microsoft.com` bzw. `https://rdweb.wvd.microsoft.com` festgelegt.

3. Legen Sie die Zugriffsrichtlinie für den Azure Key Vault fest, indem Sie das folgende PowerShell-Cmdlet ausführen:

   ```powershell
   Set-AzKeyVaultAccessPolicy -VaultName "<Key Vault Name>" -ServicePrincipalName 9cdead84-a844-4324-93f2-b2e6bb768d07 -PermissionsToSecrets get -PermissionsToKeys sign
   ```

4. Speichern Sie den gemeinsam verwendeten Schlüssel oder das Zertifikat in Azure Key Vault mit einem Tag, das eine durch Trennzeichen getrennte Liste mit Abonnement-IDs enthält, die das Geheimnis verwenden dürfen.

   * Wenn Sie einen gemeinsam verwendeten Schlüssel im Key Vault nutzen, führen Sie das folgende PowerShell-Cmdlet aus, um diesen Schlüssel zu speichern und das Tag festzulegen:

     ```powershell
     $hp = Get-AzWvdHostPool -Name "<Host Pool Name>" -ResourceGroupName "<Host Pool Resource Group Name>" 
     $secret = Set-AzKeyVaultSecret -VaultName "<Key Vault Name>" -Name "adfsssosecret" -SecretValue (ConvertTo-SecureString -String $config.SSOClientSecret  -AsPlainText -Force) -Tag @{ 'AllowedWVDSubscriptions' = $hp.Id.Split('/')[2]}
     ```

   * Wenn sich Ihr Zertifikat bereits im Key Vault befindet, führen Sie das folgende PowerShell-Cmdlet aus, um das Tag festzulegen:

     ```powershell
     $hp = Get-AzWvdHostPool -Name "<Host Pool Name>" -ResourceGroupName "<Host Pool Resource Group Name>"
     $secret = Update-AzKeyVaultCertificate -VaultName "<Key Vault Name>" -Name "<Certificate Name>" -Tag @{ 'AllowedWVDSubscriptions' = $hp.Id.Split('/')[2]} -PassThru
     ```

   * Wenn Ihr Zertifikat lokal abgespeichert ist, führen Sie das folgende PowerShell-Cmdlet aus, um das Zertifikat in den Key Vault zu importieren und das Tag festzulegen:

     ```powershell
     $hp = Get-AzWvdHostPool -Name "<Host Pool Name>" -ResourceGroupName "<Host Pool Resource Group Name>" 
     $secret = Import-AzKeyVaultCertificate -VaultName "<Key Vault Name>" -Name "adfsssosecret" -Tag @{ 'AllowedWVDSubscriptions' = $hp.Id.Split('/')[2]} -FilePath "<Path to pfx>" -Password (ConvertTo-SecureString -String "<pfx password>"  -AsPlainText -Force)
     ```

> [!NOTE]
> Optional können Sie konfigurieren, wie oft Benutzer zur Eingabe von Anmeldeinformationen aufgefordert werden, indem Sie die [Einstellungen für das einmalige Anmelden mit AD FS](/windows-server/identity/ad-fs/operations/ad-fs-single-sign-on-settings#keep-me-signed-in-for-unauthenticated-devices) ändern. Standardmäßig werden Benutzer bei nicht registrierten Geräten alle 8 Stunden zur Eingabe von Anmeldeinformationen aufgefordert.

## <a name="configure-your-azure-virtual-desktop-host-pool"></a>Konfigurieren Ihres Azure Virtual Desktop-Hostpools

Es ist an der Zeit, die AD FS-SSO-Parameter auf Ihrem Azure Virtual Desktop-Hostpool zu konfigurieren. Hierzu müssen Sie Ihre [PowerShell-Umgebung für Azure Virtual Desktop einrichten](powershell-module.md), sofern dies noch nicht erfolgt ist, und eine Verbindung mit Ihrem Konto herstellen.

Aktualisieren Sie anschließend die SSO-Informationen für Ihren Hostpool, indem Sie eines der folgenden beiden Cmdlets im selben PowerShell-Fenster auf der AD FS-VM ausführen:

* Wenn Sie einen gemeinsam verwendeten Schlüssel im Key Vault nutzen, führen Sie das folgende PowerShell-Cmdlet aus:

  ```powershell
  Update-AzWvdHostPool -Name "<Host Pool Name>" -ResourceGroupName "<Host Pool Resource Group Name>" -SsoadfsAuthority "<ADFSServiceUrl>" -SsoClientId "<WVD Web App URI>" -SsoSecretType SharedKeyInKeyVault -SsoClientSecretKeyVaultPath $secret.Id
  ```

  > [!NOTE]
  > Sie müssen die SsoClientId-Eigenschaft so festlegen, dass sie mit der Azure-Cloud übereinstimmt, in der Sie SSO bereitstellen. In der Azure Commercial Cloud sollte diese Eigenschaft auf `https://www.wvd.microsoft.com` festgelegt werden. In anderen Clouds, z. B. in der Azure Government Cloud, muss bei dieser Eigenschaft jedoch eine andere Einstellung vorgenommen werden.

* Wenn Sie ein Zertifikat im Key Vault nutzen, führen Sie das folgende PowerShell-Cmdlet aus:

  ```powershell
  Update-AzWvdHostPool -Name "<Host Pool Name>" -ResourceGroupName "<Host Pool Resource Group Name>" -SsoadfsAuthority "<ADFSServiceUrl>" -SsoClientId "<WVD Web App URI>" -SsoSecretType CertificateInKeyVault -SsoClientSecretKeyVaultPath $secret.Id
  ```

  > [!NOTE]
  > Sie müssen die SsoClientId-Eigenschaft so festlegen, dass sie mit der Azure-Cloud übereinstimmt, in der Sie SSO bereitstellen. In der Azure Commercial Cloud sollte diese Eigenschaft auf `https://www.wvd.microsoft.com` festgelegt werden. In anderen Clouds, z. B. in der Azure Government Cloud, muss bei dieser Eigenschaft jedoch eine andere Einstellung vorgenommen werden.

### <a name="configure-additional-host-pools"></a>Konfigurieren zusätzlicher Hostpools

Wenn Sie zusätzliche Hostpools konfigurieren müssen, können Sie dafür die Einstellungen abrufen, die Sie zum Konfigurieren eines vorhandenen Hostpools verwendet haben.

Öffnen Sie ein PowerShell-Fenster und führen Sie das folgende Cmdlet aus, um die Einstellungen Ihres vorhandenen Hostpools abzurufen:

```powershell
Get-AzWvdHostPool -Name "<Host Pool Name>" -ResourceGroupName "<Host Pool Resource Group Name>" | fl *
```

Sie können die gleichen Schritte wie beim [Konfigurieren Ihres Azure Virtual Desktop-Hostpools](#configure-your-azure-virtual-desktop-host-pool) befolgen und die gleichen Werte für *SsoClientId*, *SsoClientSecretKeyVaultPath*, *SsoSecretType* und *SsoadfsAuthority* verwenden.

## <a name="removing-sso"></a>Entfernen von SSO

Führen Sie das folgende Cmdlet aus, um SSO auf dem Hostpool zu deaktivieren:

```powershell
Update-AzWvdHostPool -Name "<Host Pool Name>" -ResourceGroupName "<Host Pool Resource Group Name>" -SsoadfsAuthority ''
```

Wenn Sie auch das einmalige Anmelden auf Ihrem AD FS-Server deaktivieren möchten, führen Sie dieses Cmdlet aus:

```powershell
Install-Script UnConfigureWVDSSO
UnConfigureWVDSSO.ps1 -WvdWebAppAppIDUri "<WVD Web App URI>" -WvdClientAppApplicationID "a85cf173-4192-42f8-81fa-777a763e6e2c"
```

> [!NOTE]
> Die WvdWebAppAppIDUri-Eigenschaft muss mit der Azure-Cloud übereinstimmen, in der Sie die SSO bereitstellen. In der Azure Commercial Cloud wurde diese Eigenschaft auf `https://www.wvd.microsoft.com` festgelegt. Bei anderen Clouds, wie z. B. der Azure Government Cloud, ist dies anders.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie SSO konfiguriert haben, können Sie sich an einem unterstützten Azure Virtual Desktop-Client anmelden, um ihn als Teil einer Benutzersitzung zu testen. Weitere Informationen darüber, wie Sie mit Ihren neuen Anmeldeinformationen eine Verbindung mit einer Sitzung herstellen können, finden Sie in den folgenden Artikeln:

* [Herstellen einer Verbindung mit dem Windows-Desktopclient](./user-documentation/connect-windows-7-10.md)
* [Herstellen einer Verbindung mit dem Webclient](./user-documentation/connect-web.md)