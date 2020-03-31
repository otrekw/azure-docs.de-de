---
title: Bereitstellen von Azure-verwalteten Arbeitsstationen – Azure Active Directory
description: Erfahren Sie, wie Sie sichere, von Azure verwaltete Arbeitsstationen bereitstellen, um das Risiko von Verletzungen aufgrund von Fehlkonfigurationen oder Kompromissen zu reduzieren.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d02b0299b6267fdd9d880d5bc0fe8c93d0edadc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672600"
---
# <a name="deploy-a-secure-azure-managed-workstation"></a>Bereitstellen einer sicheren, von Azure verwalteten Arbeitsstation

Jetzt, da Sie sich mit [sicheren Arbeitsstationen auskennen](concept-azure-managed-workstation.md), ist es an der Zeit, mit der Bereitstellung zu beginnen. In dieser Anleitung verwenden Sie definierte Profile, um eine Arbeitsstation zu erstellen, die von Anfang an sicherer ist.

![Bereitstelleneiner sicheren Arbeitsstation](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

Wählen Sie vor der Bereitstellung der Lösung ein Profil aus. Sie können mehrere Profile gleichzeitig in einer Bereitstellung verwenden und sie mit Tags oder Gruppen zuzuweisen.

> [!NOTE]
> Wenden Sie eines der Profile nach Ihren Anforderungen an. Sie können zu einem anderen Profil wechseln, indem Sie es in Microsoft Intune zuweisen.

| Profil | Niedrig | Verbessert | High | Spezialisiert | Geschützt | Isolated |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Benutzer in Azure AD | Ja | Ja | Ja | Ja | Ja | Ja |
| Mit Intune verwaltet | Ja | Ja | Ja | Ja | Ja | Ja |
| Gerät bei Azure AD registriert | Ja |  |  |  |  | |   |
| Gerät in Azure AD eingebunden |   | Ja | Ja | Ja | Ja | Ja |
| Intune-Sicherheitsbaseline angewandt |   | Ja <br> (Erweitert) | Ja <br> (HighSecurity) | Ja <br> (NCSC) | Ja <br> (Geschützt) | Nicht verfügbar |
| Hardware erfüllt sichere Windows 10-Standards |   | Ja | Ja | Ja | Ja | Ja |
| Microsoft Defender ATP aktiviert |   | Ja  | Ja | Ja | Ja | Ja |
| Entfernen von Administratorrechten |   |   | Ja  | Ja | Ja | Ja |
| Bereitstellung mithilfe von Microsoft Autopilot |   |   | Ja  | Ja | Ja | Ja |
| Apps nur von Intune installiert |   |   |   | Ja | Ja |Ja |
| URLs auf genehmigte Liste beschränkt |   |   |   | Ja | Ja |Ja |
| Internet (eingehend/ausgehend blockiert) |   |   |   |  |  |Ja |

> [!NOTE]
> In der Anleitung für sichere Arbeitsstationen werden **Geräte** mit Profilen und Richtlinien zugewiesen. Die Richtlinien werden nicht direkt auf die Benutzer angewendet, wodurch die Gerätefreigabe (freigegebene Geräte) wirksam wird. Wenn eine sichere Arbeitsstation nicht in Ihrer Bereitstellung freigegeben ist oder individuelle Benutzerrichtlinien benötigt werden, kann die Zuweisung der Benutzerrichtlinienprofile zu Benutzer und Gerät erfolgen. 

## <a name="license-requirements"></a>Lizenzanforderungen

Die in diesem Handbuch erörterten Konzepte setzen Microsoft 365 Enterprise E5 oder eine entsprechende SKU voraus. Einige der Empfehlungen in diesem Handbuch können mit niedrigeren SKUs implementiert werden. Weitere Informationen finden Sie unter [Microsoft 365 Enterprise-Lizenzierung](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise).

Um die Lizenzbereitstellung zu automatisieren, sollten Sie eine[ gruppenbasierte Lizenzierung](../users-groups-roles/licensing-groups-assign.md) für Ihre Benutzer in Betracht ziehen.

## <a name="azure-active-directory-configuration"></a>Azure Active Directory-Konfiguration

Azure Active Directory (Azure AD) verwaltet Benutzer, Gruppen und Geräte für Ihre Administratorarbeitsstationen. Sie müssen die Identitätsdienste und Features mit einem [Administratorkonto](../users-groups-roles/directory-assign-admin-roles.md) aktivieren.

Wenn Sie das gesicherte Arbeitsstationsadministratorkonto erstellen, machen Sie das Konto für Ihre aktuelle Arbeitsstation verfügbar. Stellen Sie sicher, dass Sie für diese Erstkonfiguration und die gesamte globale Konfiguration ein bekanntes sicheres Gerät verwenden. Um die Angriffe bei der ersten Nutzung zu reduzieren, sollten Sie die [Hinweise zur Vermeidung von Malwareinfektionen](/windows/security/threat-protection/intelligence/prevent-malware-infection) beachten.

Zudem sollte zumindest für Ihre Administratoren eine mehrstufige Authentifizierung erforderlich sein. Einen Implementierungsleitfaden finden Sie unter [Planen einer cloudbasierten Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md).

### <a name="azure-ad-users-and-groups"></a>Azure AD-Benutzer und -Gruppen

1. Navigieren Sie im Azure-Portal zu **Azure Active Directory** > **Benutzer** > **Neuer Benutzer**.
1. Erstellen Sie Ihren Geräteadministrator, indem Sie den Schritten im [Schnellstart zum Erstellen eines Benutzers](/Intune/quickstart-create-user) folgen.
1. Geben Sie Folgendes ein:

   * **Name**: Administrator der sicheren Arbeitsstation
   * **Benutzername** - `secure-ws-admin@identityitpro.com`
   * **Verzeichnisrolle** - **Eingeschränkter Administrator** und wählen Sie die Rolle **Intune-Administrator** aus.

1. Klicken Sie auf **Erstellen**.

Als Nächstes erstellen Sie zwei Gruppen: Arbeitsstationbenutzer und Arbeitsstationgeräte.

Navigieren Sie vom Azure-Portal aus zu **Azure Active Directory** > **Gruppen** > **Neue Gruppe**.

1. Für die Gruppe der Arbeitsstationbenutzer können Sie eine [gruppenbasierte Lizenzierung](../users-groups-roles/licensing-groups-assign.md) konfigurieren, um die Bereitstellung von Lizenzen für Benutzer zu automatisieren.
1. Geben Sie für die Gruppe der Arbeitsstationbenutzer Folgendes ein:

   * **Gruppentyp**: Sicherheit
   * **Gruppenname**: Benutzer der sicheren Arbeitsstation
   * **Mitgliedschaftstyp**: Zugewiesen

1. Fügen Sie Ihren sicheren Administratorbenutzer für die Arbeitsstation hinzu: `secure-ws-admin@identityitpro.com`
1. Sie können alle anderen Benutzer hinzufügen, die sichere Arbeitsstationen verwalten.
1. Klicken Sie auf **Erstellen**.
1. Geben Sie für die Gruppe der Gerätegruppe Folgendes ein:

   * **Gruppentyp**: Sicherheit
   * **Gruppenname**: Sichere Arbeitsstationen
   * **Mitgliedschaftstyp**: Zugewiesen

1. Klicken Sie auf **Erstellen**.

### <a name="azure-ad-device-configuration"></a>Azure AD-Gerätekonfiguration

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Angeben, wer Geräte in Azure AD einbinden darf

Konfigurieren Sie Ihre Geräteeinstellungen in Active Directory, damit Ihre administrative Sicherheitsgruppe die Geräte mit Ihrer Domäne verknüpfen kann. So konfigurieren Sie diese Einstellung über das Azure-Portal:

1. Wechseln Sie zu **Azure Active Directory** > **Geräte** > **Geräteeinstellungen**.
1. Wählen Sie **Ausgewählte** unter **Benutzer können Geräte mit Azure AD verbinden** aus, und wählen Sie dann die Gruppe „Benutzer der sicheren Arbeitsstation“ aus.

#### <a name="removal-of-local-admin-rights"></a>Entfernen von lokalen Administratorrechten

Diese Methode erfordert, dass Benutzer der Arbeitsstationen VIP, DevOps und Secure-Level keine Administratorrechte auf ihren Computern haben. So konfigurieren Sie diese Einstellung über das Azure-Portal:

1. Wechseln Sie zu **Azure Active Directory** > **Geräte** > **Geräteeinstellungen**.
1. Wählen Sie unter **Weitere lokale Administratoren für in Azure AD eingebundene Geräte** die Option **Keine** aus.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Voraussetzen der Multi-Factor Authentication für das Einbinden von Geräten

Um den Prozess der Verknüpfung von Geräten mit Azure AD weiter zu verbessern:

1. Wechseln Sie zu **Azure Active Directory** > **Geräte** > **Geräteeinstellungen**.
1. Wählen Sie **Ja** unter **Mehrstufige Authentifizierung zum Hinzufügen von Geräten erforderlich** aus.
1. Wählen Sie **Speichern** aus.

#### <a name="configure-mobile-device-management"></a>Konfigurieren der mobilen Geräteverwaltung

Gehen Sie im Azure-Portal so vor:

1. Navigieren Sie zu **Azure Active Directory** > **Mobility (MDM und MAM)**  > **Microsoft Intune**.
1. Ändern Sie die Einstellung **MDM-Benutzerbereich** in **Alle**.
1. Wählen Sie **Speichern** aus.

Mit diesen Schritten können Sie jedes Gerät mit Intune verwalten. Weitere Informationen finden Sie unter [Schnellstart: Einrichten der automatischen Registrierung für Windows 10-Geräte](/Intune/quickstart-setup-auto-enrollment). Sie erstellen in einem späteren Schritt Intune-Konfigurations- und Compliancerichtlinien.

#### <a name="azure-ad-conditional-access"></a>Bedingter Azure AD-Zugriff

Bedingter Azure AD-Zugriff kann helfen, privilegierte administrative Aufgaben auf konforme Geräte zu beschränken. Vordefinierte Mitglieder der Gruppe **Benutzer der sicheren Arbeitsstation** müssen bei der Anmeldung an Cloudanwendungen eine mehrstufige Authentifizierung durchführen. Eine bewährte Methode ist es, Notfallzugriffskonten von der Richtlinie auszunehmen. Weitere Informationen finden Sie unter [Verwalten von Konten für den Notfallzugriff in Azure AD](../users-groups-roles/directory-emergency-access.md).

## <a name="intune-configuration"></a>Intune-Konfiguration

### <a name="configure-enrollment-status"></a>Konfigurieren des Registrierungsstatus

Es ist wichtig, dafür zu sorgen, dass Ihre sichere Arbeitsstation ein vertrauenswürdiges, sauberes Gerät ist. Beim Kauf neuer Geräte können Sie darauf bestehen, dass diese werkseitig auf [Windows 10 Pro im S-Modus](/Windows/deployment/Windows-10-pro-in-s-mode) eingestellt sind, was die Gefährdung durch Schwachstellen beim Supply Chain Management begrenzt. Nachdem Sie ein Gerät von Ihrem Lieferanten erhalten haben, können Sie es mit Autopilot aus dem S-Modus umstellen. Die folgende Anleitung enthält Informationen zum Anwenden des Transformationsprozesses.

Um sicherzustellen, dass die Geräte vor der Verwendung vollständig konfiguriert sind, bietet Intune die Option **Geräteverwendung blockieren, bis alle Apps und Profile installiert sind**.

Über das **Azure-Portal**:

1. Wechseln Sie zu **Microsoft Intune** > **Geräteregistrierung** > **Windows-Registrierung** > **Seite zum Registrierungsstatus** > **Standard** > **Einstellungen**.
1. Setzen Sie **Installationsfortschritt für Apps und Profile anzeigen** auf **Ja**.
1. Setzen Sie **Geräteverwendung blockieren, bis alle Apps und Profile installiert sind** auf **Ja**.

### <a name="create-an-autopilot-deployment-profile"></a>Erstellen eines Autopilot-Bereitstellungsprofils

Nachdem Sie eine Gerätegruppe erstellt haben, müssen Sie ein Bereitstellungsprofil erstellen, um die Autopilot-Geräte zu konfigurieren.

In Intune im Azure-Portal:

1. Wählen Sie in Intune im Azure-Portal **Geräteregistrierung** > **Windows-Registrierung** > **Bereitstellungsprofile** > **Profil erstellen** aus.
1. Geben Sie Folgendes ein:

   * Name: **Sicheres Bereitstellungsprofil für Arbeitsstation**.
   * Beschreibung: **Bereitstellung von sicheren Arbeitsstationen**.
   * Setzen Sie **Alle Zielgeräte in Autopilot-Geräte konvertieren** auf **Ja**. Diese Einstellung stellt sicher, dass alle Geräte in der Liste mit dem Autopilot-Bereitstellungsdienst registriert werden. Lassen Sie 48 Stunden zur Verarbeitung der Registrierung verstreichen.

1. Wählen Sie **Weiter** aus.

   * Wählen Sie im **Bereitstellungsmodus** die Option **Selbstbereitstellung (Vorschau)** . Geräte mit diesem Profil sind dem Benutzer zugeordnet, der das Gerät registriert. Anmeldeinformationen sind erforderlich, um das Gerät zu registrieren. Beachten Sie, dass die Bereitstellung eines Geräts im **Selbstbereitstellungsmodus** Ihnen die Bereitstellung von Laptops in einem freigegebenen Modell ermöglicht. Es findet keine Benutzerzuweisung statt, bis das Gerät zum ersten Mal einem Benutzer zugewiesen wird. Folglich werden alle Benutzerrichtlinien wie BitLocker erst dann aktiviert, wenn eine Benutzerzuweisung abgeschlossen ist. Weitere Informationen zum Anmelden bei einem gesicherten Gerät finden Sie unter [Ausgewählte Profile](/intune/device-profile-assign).
   * Das Feld **Azure AD beitreten als** sollte anzeigen, dass **In Azure AD eingebunden** ist und abgeblendet ist.
   * Wählen Sie Ihre Sprache (Region) und den Benutzerkontotyp **Standard** aus. 

1. Wählen Sie **Weiter** aus.

   * Wählen Sie eine Bereichsmarkierung, wenn Sie eine vorkonfiguriert haben.

1. Wählen Sie **Weiter** aus.
1. Wählen Sie **Zuweisungen** > **Zuweisen zu** > **Ausgewählte Gruppen** aus. Wählen Sie unter **Wählen Sie die Gruppen aus, die eingeschlossen werden sollen** die Option **Sichere Arbeitsstationen** aus.
1. Wählen Sie **Weiter** aus.
1. Wählen Sie **Erstellen**, um das Profil zu erstellen. Das Autopilot-Bereitstellungsprofil steht nun für die Zuweisung von Geräten zur Verfügung.

Die Registrierung von Geräten in Autopilot bietet eine je nach Gerätetyp und Rolle unterschiedliche Benutzererfahrung. In unserem Bereitstellungsbeispiel veranschaulichen wir ein Modell, bei dem die gesicherten Geräte in der Masse bereitgestellt werden und gemeinsam genutzt werden können, aber bei der ersten Verwendung wird das Gerät einem Benutzer zugewiesen. Weitere Informationen finden Sie unter [Was ist die Geräteregistrierung?](/intune/device-enrollment).

### <a name="configure-windows-update"></a>Konfigurieren des Windows-Updates

Windows 10 auf dem neuesten Stand zu halten, ist eines der wichtigsten Dinge, die Sie tun können. Um Windows in einem sicheren Zustand zu halten, setzen Sie einen Updatering ein, um die Geschwindigkeit zu steuern, mit der Updates auf die Arbeitsstationen angewendet werden. 

Diese Anleitung empfiehlt, einen neuen Updatering zu erstellen und die folgenden Standardeinstellungen zu ändern:

Führen Sie im Azure-Portal die folgenden Schritte aus:

1. Wechseln Sie zu **Microsoft Intune** > **Softwareupdates** > **Windows 10-Updateringe**.
1. Geben Sie Folgendes ein:

   * Name: **Updates für verwaltete Azure-Arbeitsstationen**
   * Wartungskanal: **Windows-Insider: schnell**
   * Rückstellungszeitraum für Qualitätsupdates (Tage): **3**
   * Rückstellungszeitraum für Funktionsupdates (Tage): **3**
   * Automatisches Updateverhalten: **Ohne Endbenutzersteuerung automatisch installieren und neu starten**
   * Anhalten von Windows-Updates durch Benutzer blockieren: **Blockieren**
   * Genehmigung des Benutzers für Neustart außerhalb der Arbeitszeiten erforderlich: **Erforderlich**
   * Benutzer (erzwungenen) Neustart ermöglichen: **Erforderlich**
   * Übergang von Benutzern nach einem automatischen Neustart zum erzwungenen Neustart (Tage) : **3**
   * Erinnerung zu engagiertem Neustart zurückstellen (Tage): **3**
   * Stichtag für ausstehende Neustarts festlegen (Tage): **3**

1. Klicken Sie auf **Erstellen**.
1. Fügen Sie auf der Registerkarte **Zuweisungen** die Gruppe **Sichere Arbeitsstationen** hinzu.

Weitere Informationen zu Windows-Updaterichtlinien finden Sie unter [Policy CSP – Update (Richtlinien-CSP – Update)](/windows/client-management/mdm/policy-csp-update).

### <a name="windows-defender-atp-intune-integration"></a>Integration von Windows Defender ATP in Intune

Windows Defender ATP und Microsoft Intune verhindern gemeinsam Sicherheitsverletzungen. Sie können auch die Auswirkungen von Sicherheitsverletzungen einschränken. ATP-Funktionen bieten eine Bedrohungserkennung in Echtzeit und ermöglichen eine umfassende Überprüfung und Protokollierung der Endgeräte.

Um die Integration von Windows Defender ATP und Intune zu konfigurieren, wechseln Sie zum Azure-Portal.

1. Navigieren Sie zu **Microsoft Intune** > **Gerätekonformität** > **Windows Defender ATP**.
1. Klicken Sie in Schritt 1 unter **Konfigurieren von Windows Defender ATP** auf **Verbindungsherstellung zwischen Windows Defender ATP und Microsoft Intune über Windows Defender Security Center**.
1. In Windows Defender Security Center:

   1. Wählen Sie **Einstellungen** > **Erweiterte Funktionen**.
   1. Wählen Sie für **Microsoft Intune-Verbindung** die Option **Ein** aus.
   1. Wählen Sie **Einstellungen speichern** aus.

1. Nachdem eine Verbindung hergestellt wurde, kehren Sie zu Intune zurück und wählen Sie oben **Aktualisieren** aus.
1. Setzen Sie **Verbindung von Windows-Geräten mit Version 10.0.15063 und höher mit Windows Defender ATP herstellen** auf **Ein**.
1. Wählen Sie **Speichern** aus.

Weitere Informationen finden Sie unter [Windows Defender Advanced Threat Protection](/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection).

### <a name="finish-workstation-profile-hardening"></a>Abschließen der Härtung von Arbeitsstationsprofilen

Um die Härtung der Lösung erfolgreich abzuschließen, laden Sie das entsprechende Skript herunter und führen Sie es aus. Hier finden Sie die Downloadlinks für Ihre gewünschte **Profilebene**:

| Profil | Downloadspeicherort | Dateiname |
| --- | --- | --- |
| Niedrige Sicherheit | – | – |
| Erweiterte Sicherheit | https://aka.ms/securedworkstationgit | Enhanced-Workstation-Windows10-(1809).ps1 |
| Hohe Sicherheit | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809).ps1 |
| Spezialisiert | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC – Windows10 (1803) SecurityBaseline.ps1 |
| Spezielle Compliance * | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10(1803).ps1 |
| Geschützt | https://aka.ms/securedworkstationgit | Secure-Workstation-Windows10-(1809)-SecurityBaseline.ps1 |

\* Specialized Compliance ist ein Skript, das die in der NCSC Windows10 SecurityBaseline bereitgestellte Specialized-Konfiguration erzwingt.

Nachdem das Skript erfolgreich ausgeführt wurde, können Sie Updates von Profilen und Richtlinien in Intune vornehmen. Die Skripts für erweiterte und sichere Profile erstellen Richtlinien und Profile für Sie, aber Sie müssen die Richtlinie Ihrer Gerätegruppe **Sichere Arbeitsstationen** zuweisen.

* Hier finden Sie die durch Skripts erstellten Intune-Gerätekonfigurationsprofile: **Azure-Portal** > **Microsoft Intune** > **Gerätekonfiguration** > **Profile**.
* Hier finden Sie die durch Skripts erstellten Intune-Gerätekonformitätsrichtlinien: **Azure-Portal** > **Microsoft Intune** > **Gerätekonformität** > **Profile**.

Um die von den Skripts vorgenommenen Änderungen zu überprüfen, können Sie die Profile exportieren. Auf diese Weise können Sie eine zusätzliche Härtung bestimmen, die gemäß der [SECCON-Dokumentation](/windows/security/threat-protection/windows-security-configuration-framework/windows-security-configuration-framework) erforderlich sein kann.

Führen Sie das Intune-Datenexportskript `DeviceConfiguration_Export.ps1` aus dem [GitHub-Repository „DeviceConfiguration“](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) aus, um alle aktuellen Intune-Profile zu exportieren.

## <a name="additional-configurations-and-hardening-to-consider"></a>Zu berücksichtigende zusätzliche Konfigurationen und Härtung

Wenn Sie den Anweisungen hier folgen, haben Sie eine sichere Arbeitsstation eingerichtet. Allerdings sollten Sie auch zusätzliche Steuerelemente in Betracht ziehen. Beispiel:

* Beschränken des Zugriffs auf andere Browser
* Zulassen von ausgehendem HTTP-Datenverkehr
* Blockieren von ausgewählten Websites
* Festlegen von Berechtigungen für die Ausführung benutzerdefinierter PowerShell-Skripte

### <a name="set-rules-in-the-firewall-configuration-service-provider-csp"></a>Festlegen von Regeln im Firewall-Konfigurationsdienstanbieter (CSP)

Sie können bei Bedarf zusätzliche Änderungen an der Verwaltung der Eingangs- und Ausgangsregeln für Ihre zulässigen und blockierten Endpunkte vornehmen. Während Sie die sichere Arbeitsstation weiter härten, können Sie die Einschränkung aufheben, die den gesamten ein- und ausgehenden Datenverkehr unterbindet. Sie können zulässige ausgehende Websites hinzufügen, um gemeinsame und vertrauenswürdige Websites einzubeziehen. Weitere Informationen finden Sie unter [Firewall-Konfigurationsdienst](/Windows/client-management/mdm/firewall-csp).

Eine restriktive Verwaltung des URL-Datenverkehrs umfasst Folgendes:

* Verweigern des gesamten eingehenden Datenverkehrs. Dies kann über das Skript für das Profil der sicheren Arbeitsstation festgelegt werden.
* Verweigern des gesamten ausgehenden Datenverkehrs mit Ausnahme ausgewählter Azure- und Microsoft-Dienste wie Azure Cloud Shell und der Option, das Zurücksetzen des Azure-Kennworts zu ermöglichen.

```
[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings]
"ProxyEnable"=dword:00000001
"ProxyServer"="127.0.0.1:80"
"ProxyOverride"="*.azure.com;*.azure.net;*.microsoft.com;*.windowsupdate.com;*.microsoftonline.com;*.microsoftonline.cn;*.windows.net;*.windowsazure.com;*.windowsazure.cn;*.azure.cn;*.loganalytics.io;*.applicationinsights.io;*.vsassets.io;*.azure-automation.net;*.visualstudio.com,portal.office.com;*.aspnetcdn.com;*.sharepointonline.com;*.msecnd.net;*.msocdn.com;*.webtrends.com"
"AutoDetect"=dword:00000000
```

Wenn Sie eine höhere Granularität Ihrer Blockierungsregeln erreichen möchten, können Sie sich auf das Spamhaus Project beziehen, das [die Domänenblockierungsliste (DBL)](https://www.spamhaus.org/dbl/) pflegt: eine gute Ressource, wenn Sie weitere Regeln zum Blockieren von Websites implementieren möchten.

### <a name="manage-local-applications"></a>Verwalten von lokalen Anwendungen

Die sichere Arbeitsstation befindet sich dann in einem wirklich gehärteten Zustand, wenn lokale Anwendungen, einschließlich Produktivitätsanwendungen, entfernt werden. Hier fügen Sie Chrome als Standardbrowser hinzu und verwenden Intune, um die Möglichkeit eines Benutzers einzuschränken, den Browser und seine Plug-Ins zu ändern.

#### <a name="deploy-applications-using-intune"></a>Bereitstellen von Anwendungen mit Intune

In einigen Fällen sind Anwendungen wie der Google Chrome-Browser auf der geschützten Arbeitsstation erforderlich. Das folgende Beispiel enthält Anweisungen zur Installation von Chrome auf Geräten der Sicherheitsgruppe **Sichere Arbeitsstationen**.

1. Laden Sie das Offlineinstallationsprogramm [Chrome-Paket für Windows (64-Bit-Version)](https://cloud.google.com/chrome-enterprise/browser/download/) herunter.
1. Entpacken Sie die Dateien, und notieren Sie sich den Speicherort der Datei `GoogleChromeStandaloneEnterprise64.msi`.
1. Navigieren Sie im **Azure-Portal** zu **Microsoft Intune** > **Clientanwendungen** > **Apps** > **Hinzufügen**.
1. Wählen Sie unter **App-Typ** die Option **Branchenspezifisch** aus.
1. Wählen Sie unter **App-Paketdatei** die Datei `GoogleChromeStandaloneEnterprise64.msi` am extrahierten Speicherort aus, und klicken Sie auf **OK**.
1. Geben Sie unter **App-Informationen** eine Beschreibung und einen Herausgeber an. Klicken Sie auf **OK**.
1. Wählen Sie **Hinzufügen**.
1. Wählen Sie auf der Registerkarte **Zuordnungen** unter **Zuordnungsart** die Option **Verfügbar für registrierte Geräte**.
1. Fügen Sie unter **Eingeschlossene Gruppen** die Gruppe **Sichere Arbeitsstationen** hinzu.
1. Wählen Sie **OK** und anschließend **Speichern** aus.

Weitere Informationen zur Konfiguration von Chrome-Einstellungen finden Sie unter [Verwalten des Chrome-Browsers mit Microsoft Intune](https://support.google.com/chrome/a/answer/9102677).

#### <a name="configuring-the-company-portal-for-custom-apps"></a>Konfigurieren des Unternehmensportals für benutzerdefinierte Apps

In einem gesicherten Modus ist die Anwendungsinstallation auf das Intune-Unternehmensportal beschränkt. Das Installieren des Portals erfordert jedoch Zugriff auf den Microsoft Store. In Ihrer gesicherten Lösung können Sie das Unternehmensportal über einen Offlinemodus für alle Geräte verfügbar machen.

Eine in Intune verwaltete Kopie des [Unternehmensportals](/Intune/store-apps-company-portal-app) bietet Ihnen bei Bedarf Zugriff auf zusätzliche Tools, die Sie an die Benutzer der gesicherten Arbeitsstationen weiterleiten können.

Möglicherweise müssen Sie Windows-Anwendungen (32-Bit-Version) oder andere Anwendungen installieren, deren Bereitstellung spezielle Vorbereitungen erfordert. In solchen Fällen kann das [Microsoft Win32 Content Prep Tool](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) (Vorbereitungstool für Microsoft-Win32-Inhalt) eine einsatzbereite `.intunewin`-Formatdatei für die Installation bereit.

### <a name="conditional-access-only-allowing-secured-workstation-ability-to-access-azure-portal"></a>Bedingter Zugriff ermöglicht nur gesicherten Arbeitsstationen den Zugriff auf das Azure-Portal

Azure AD bietet die Möglichkeit, die Zugriffsberechtigungen für Ihr Azure-Cloudverwaltungsportal zu verwalten und einzuschränken. Durch die Aktivierung der Option [Bedingter Zugriff](../conditional-access/overview.md) ist sichergestellt, dass Ressourcen nur über Ihre sichere Arbeitsstation verwaltet oder geändert werden können. Es ist wichtig, dass Sie bei der Bereitstellung dieser Funktion überlegen, ob der [Notfallzugriff](../users-groups-roles/directory-emergency-access.md) nur für Ausnahmefälle verwendet werden kann oder sollte und das Konto über eine Richtlinie verwaltet wird.

> [!NOTE]
> Sie müssen eine Benutzergruppe einschließlich des Notfallbenutzers erstellen, der die Richtlinie für bedingten Zugriff umgehen kann. Die Sicherheitsgruppe in diesem Beispiel heißt **Emergency BreakGlass**.

1. Browsen Sie zum **Azure-Portal** > **Microsoft Intune** > **Bedingter Zugriff – Richtlinien** > **Neue Richtlinie**.
1. Geben Sie einen **Namen** für die Richtlinie an.
1. Wählen Sie **Benutzer und Gruppen** > **Benutzer und Gruppen auswählen** aus. 
1. Wählen Sie **Einschließen** > **Verzeichnisrollen** > Rollen auswählen > Globaler Administrator, Administrator für privilegierte Rollen, Privilegierter Authentifizierungsadministrator, Sicherheitsadministrator, Complianceadministrator, Administrator für bedingten Zugriff, Anwendungsadministrator, Cloudanwendungsadministrator, Intune-Dienstadministrator.
1. Wählen Sie **Ausschließen** aus. Klicken Sie auf **Benutzer und Gruppen**. Wählen Sie **Ausgeschlossene Benutzer auswählen** und dann die Gruppe **Emergency BreakGlass** aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** die Option **Alle Cloud-Apps** aus.
1. Wählen Sie **Bedingungen**, dann **Geräteplattformen** und die Option **Ja** aus. Klicken Sie dann auf **Geräteplattformen auswählen**, und wählen Sie **Windows** aus.
1. Wählen Sie nacheinander **Zugriffskontrollen** > **Zugriff gewähren** **Ja** > **Markieren des Geräts als kompatibel erforderlich** aus. 
1. Wählen Sie **Richtlinie aktivieren** > **Ein** aus.
 
Dieser Richtliniensatz stellt sicher, dass Administratoren ein kompatibles Windows-Gerät verwenden müssen, das von Intune und WDATP festgelegt wurde. 

Organisationen sollten auch in Erwägung ziehen, Legacy-Authentifizierungsprotokolle in ihrer Umgebung zu blockieren. Es gibt mehrere Möglichkeiten, dies zu erreichen. Weitere Informationen zum Blockieren von Legacy-Authentifizierungsprotokollen finden Sie im Artikel [Vorgehensweise: Blockieren der Legacyauthentifizierung bei Azure AD mit bedingtem Zugriff](../conditional-access/block-legacy-authentication.md).

### <a name="use-powershell-to-create-custom-settings"></a>Verwenden von PowerShell zum Erstellen von benutzerdefinierten Einstellungen

Sie können auch PowerShell verwenden, um Hostverwaltungsfunktionen zu erweitern. Das Beispielskript richtet einen Standardhintergrund auf dem Host ein. Es handelt sich um eine Funktion, die auch über das Azure-Portal und die Profile verfügbar ist. Das Beispielskript stellt lediglich die PowerShell-Funktionen zur Veranschaulichung bereit.

Möglicherweise müssen Sie einige benutzerdefinierte Kontrollen und Einstellungen auf Ihren sicheren Arbeitsstationen einrichten. Dieses Beispiel ändert den Hintergrund der Arbeitsstation, indem es die Fähigkeit von PowerShell nutzt, das Gerät einfach als gebrauchsfertige, sichere Arbeitsstation zu identifizieren.

Das Skript [SetDesktopBackground.ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) aus dem Microsoft Scripting Center ermöglicht es Windows, dieses [kostenlose, generische Hintergrundbild](https://i.imgur.com/OAJ28zO.png) beim Start zu laden.

1. Laden Sie das Skript auf ein lokales Gerät herunter.
1. Aktualisieren Sie den KundenXXXXXX und den Downloadspeicherort des Hintergrundbilds. In unserem Beispiel ersetzen wir „customerXXXX“ mit „Hintergründe“.
1. Navigieren Sie zum **Azure-Portal** > **Microsoft Intune** > **Gerätekonfiguration** > **PowerShell-Skripte** > **Hinzufügen**.
1. Geben Sie einen **Namen** für das Skript sowie den **Skript-Speicherort** an.
1. Wählen Sie **Konfigurieren**aus.
   1. Setzen Sie **Dieses Skript mit den Anmeldeinformationen des angemeldeten Benutzers ausführen**  auf **Ja**.
   1. Klicken Sie auf **OK**.
1. Klicken Sie auf **Erstellen**.
1. Wählen Sie **Zuordnungen** > **Gruppen auswählen** aus.
   1. Fügen Sie die Sicherheitsgruppe **Sichere Arbeitsstationen** hinzu.
   1. Wählen Sie **Speichern** aus.

## <a name="enroll-and-validate-your-first-device"></a>Registrieren und Überprüfen Ihres ersten Geräts

1. Zum Registrieren Ihres Geräts benötigen Sie die folgenden Informationen:
   * **Seriennummer**: befindet sich auf dem Gerätegehäuse.
   * **Windows-Produkt-ID**: unter **System** > **Info** im Menü „Windows-Einstellungen“.
   * Sie können [Get-WindowsAutoPilotInfo](https://aka.ms/Autopilotshell) ausführen, um eine CSV-Hashdatei mit allen erforderlichen Informationen für die Geräteregistrierung zu erhalten.
   
     Führen Sie `Get-WindowsAutoPilotInfo – outputfile device1.csv` aus, um die Informationen als CSV-Datei auszugeben, die in Intune importiert werden können.

     > [!NOTE]
     > Das Skript erfordert erhöhte Rechte. Es wird als remote signiertes Skript ausgeführt. Der Befehl `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned` ermöglicht die korrekte Ausführung des Skripts.

   * Sie können diese Informationen sammeln, indem Sie sich bei einem Gerät der Windows 10-Version 1809 oder höher anmelden. Diese Informationen können auch von Ihrem Hardwarehändler zur Verfügung gestellt werden.
1. Wechseln Sie im **Azure-Portal** zu **Microsoft Intune** >  **Geräteregistrierung** > **Windows-Registrierung** >  **Geräte – Windows AutoPilot-Geräte verwalten**.
1. Wählen Sie **Importieren** und dann Ihre CSV-Datei aus.
1. Fügen Sie das Gerät der Sicherheitsgruppe **Sichere Arbeitsstationen** hinzu.
1. Navigieren Sie auf dem Windows 10-Gerät, das Sie konfigurieren möchten, zu **Windows-Einstellungen** > **Update und Sicherheit** > **Wiederherstellung**.
   1. Wählen Sie unter **Diesen PC zurücksetzen** die Option **Erste Schritte** aus.
   1. Befolgen Sie die Eingabeaufforderungen zum Zurücksetzen und erneuten Konfigurieren des Geräts mithilfe der konfigurierten Profil- und Konformitätsrichtlinien.

Nachdem Sie das Gerät konfiguriert haben, überprüfen Sie die Konfiguration. Vergewissern Sie sich, dass das erste Gerät ordnungsgemäß konfiguriert ist, bevor Sie Ihre Bereitstellung fortsetzen.

## <a name="assign-devices"></a>Zuweisen von Geräten

Um Geräte und Benutzer zuzuweisen, müssen Sie die [ausgewählten Profile](/intune/device-profile-assign) Ihrer Sicherheitsgruppe zuordnen. Alle neuen Benutzer, die Berechtigungen für den Dienst benötigen, müssen ebenfalls der Sicherheitsgruppe hinzugefügt werden.

## <a name="using-sentinel-and-windows-defender-atp-to-monitor-and-respond-to-security-incidents"></a>Überwachen von und Reagieren auf Sicherheitsvorfälle mit Sentinel und Windows Defender ATP

Die Überwachung der Bereitstellung einer sicheren Arbeitsstation kann durch Aktivieren von[Sentinel] und Verwenden von [Management von Bedrohungen und Sicherheitsrisiken](/windows/security/threat-protection/microsoft-defender-atp/next-gen-threat-and-vuln-mgt) durchgeführt werden. Die Anleitung wird keine erschöpfende Bedrohungssuche bereitstellen, sondern einen Beitrag zum gesunden Menschenverstand bei der Überwachung und Reaktion auf potenzielle Sicherheitsvorfälle leisten.

**Azure Sentinel** wird wie folgt genutzt: 

* Sammeln von Daten aus Intune, Azure Portal und Azure AD zur Benutzer- und Geräteüberwachung
* Unterstützung bei der Überprüfung von verdächtigen Aktivitäten bei der Benutzer- und Gerätekonfiguration
* Möglichkeit, Sicherheitskontrollen mit WDATP zu untersuchen

Die Sentinel-Überwachung setzt voraus, dass Connectors für Ihre Datenquellen, z.B. Azure AD, eingerichtet sind.

1. Wechseln Sie im **Azure-Portal** zu **Azure Sentinel (Vorschauversion)** , und wählen Sie **Hinzufügen** aus.
1. Wählen Sie in **Wählen Sie einen Arbeitsbereich zum Hinzufügen in Azure Sentinel** die Option **Neuen Arbeitsbereich erstellen** aus.
1. Geben Sie Folgendes ein:
   * **Log Analytics-Arbeitsbereich**: 'Überwachung der sicheren Arbeitsstation'
   * **Abonnement**: Wählen Sie Ihr aktives Abonnement aus.
   * **Ressourcengruppe**: Wählen Sie **Neu erstellen** > RG „Sichere Arbeitsstation“> **OK** aus.
   * **Standort**: Wählen Sie den Standort aus, der für Ihre Bereitstellung geografisch am besten geeignet ist.
   * **Tarif**: Wählen Sie **Pro GB (2018)** aus.
1. Klicken Sie auf **OK**.

Als Nächstes werden die verfügbaren sicheren Datenquellen der Arbeitsstation mit dem Überwachungssystem verbunden.

1. Wechseln Sie im **Azure-Portal** zu **Azure Sentinel-Arbeitsbereich**. Wählen Sie den Arbeitsbereich **Überwachung der sicheren Arbeitsstation** aus.
1. Wählen Sie **Datenconnectors** aus.
1. Wählen Sie nach dem Überprüfen der Voraussetzungen **Azure Active Directory** und dann „Connectorseite öffnen“ aus. Fahren Sie mit der Konfiguration fort, und wählen Sie **Verbinden** sowohl für Azure AD-Anmeldeprotokolle als auch Azure AD-Überwachungsprotokolle aus.
1. Wählen Sie nach dem Überprüfen der Voraussetzungen **Azure-Aktivität** und dann „Connectorseite öffnen“ aus. Fahren Sie mit „Azure-Aktivitätsprotokolle konfigurieren“ fort. Wählen Sie Ihr Abonnement aus, und klicken Sie auf **Verbinden**.

Da die Daten von Sentinel gesammelt werden, können Sie die Aktivität beobachten, indem Sie **Azure-Portal** auswählen. Wechseln Sie zu **Azure Sentinel-Übersicht**. 

**Windows Defender ATP (WDATP)** wird hier wie folgt eingesetzt:

* Kontinuierliches Beobachten und Überwachen von Schwachstellen und Fehlkonfigurationen
* Verwenden von WDATP zum Priorisieren dynamischer Bedrohungen in der Praxis
* Steuern der Korrelation von Sicherheitsrisiken mit Warnungen zu Endpunkterkennung und Reaktion (EDR)
* Verwenden des Dashboards zur Identifizierung von Sicherheitsrisiken auf Computerebene während Untersuchungen
* Senden von Korrekturen an Intune

Konfigurieren Sie Ihr [Defender ATP-Dashboard](https://securitycenter.windows.com/machines). Verwenden Sie den Leitfaden unter [Threat & Vulnerability Management dashboard overview (Dashboardübersicht zur Verwaltung von Bedrohungen und Sicherheitsrisiken)](/windows/security/threat-protection/microsoft-defender-atp/tvm-dashboard-insights).

## <a name="monitoring-application-activity-using-microsoft-monitoring-agent-mma"></a>Überwachen der Anwendungsaktivität mithilfe von Microsoft Monitoring Agent (MMA)
Ab der Specialized-Arbeitsstation ist der AppLocker für die Überwachung der Anwendungsaktivität auf einer Arbeitsstation aktiviert. Damit die Überwachung in Ihren Log Analytics-Arbeitsbereich integriert werden kann, ist ein MMA-Agent und die Einhaltung der Konfiguration erforderlich. 

> [!NOTE]
> Das Specialized-Arbeitsstationsprofil umfasst die AppLocker-Überwachungsrichtlinien. Die Bereitstellung der Richtlinien ist für die Überwachung der Anwendungsaktivität auf einem Client erforderlich.

Bereitstellen des MMA-Agents mit dem Intune-PowerShell-Skript

1. Laden Sie das [Setupskript auf ein lokales Gerät herunter](https://aka.ms/securedworkstationgit).
1. Aktualisieren Sie die Parameter **$WorkSpaceID** und **$WorkSpaceKey**.
1. Navigieren Sie zum **Azure-Portal** > **Microsoft Intune** > **Gerätekonfiguration** > **PowerShell-Skripte** > **Hinzufügen**.
1. Geben Sie einen **Namen** für das Skript sowie den **Skript-Speicherort** an.
1. Wählen Sie **Konfigurieren**aus.
   1. Setzen Sie **Dieses Skript mit den Anmeldeinformationen des angemeldeten Benutzers ausführen**  auf **Ja**.
   1. Klicken Sie auf **OK**.
1. Klicken Sie auf **Erstellen**.
1. Wählen Sie **Zuordnungen** > **Gruppen auswählen** aus.
   1. Fügen Sie die Sicherheitsgruppe **Sichere Arbeitsstationen** hinzu.
   1. Wählen Sie **Speichern** aus.

Als Nächstes müssen Sie Log Analytics einrichten, um die neuen Protokolle zu erhalten.
1. Wechseln Sie im **Azure-Portal** zu **Log Analytics-Arbeitsbereich**. Wählen Sie „Überwachung der sicheren Arbeitsstation“ aus.
1. Wählen Sie **Erweiterte Einstellungen** > **Daten** > **Windows-Ereignisprotokolle** aus.
1. Wechseln Sie zu **Erfassen Sie Ereignisse aus den folgenden Ereignisprotokollen**. 
1. Geben Sie Folgendes ein:
   * 'Microsoft-Windows-AppLocker/EXE and DLL' > Deaktivieren Sie **Information**
   * 'Microsoft-Windows-AppLocker/MSI and Script' > Deaktivieren Sie **Information**
   * 'Microsoft-Windows-AppLocker/Packaged app-Deployment' > Deaktivieren Sie **Information**
   * 'Microsoft-Windows-AppLocker/Packaged app-Execution' > Deaktivieren Sie **Information**
1. Wählen Sie **Speichern** aus.

Die Anwendungsprotokollierung ist in Ihrem ausgewählten Log Analytics-Arbeitsbereich verfügbar.

## <a name="monitoring"></a>Überwachung

* Erfahren Sie mehr im [Tutorial: Standardmäßig verfügbare Erkennung von Bedrohungen](/azure/sentinel/tutorial-detect-threats).
* Lesen Sie [Untersuchen von Incidents mit Azure Sentinel](/azure/sentinel/tutorial-investigate-cases).
* Machen Sie sich mit dem [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Sentinel](/azure/sentinel/tutorial-respond-threats-playbook) vertraut.
* Informieren Sie sich, wie Sie Ihr [Exposure Score (Risikopotenzial)](/windows/security/threat-protection/microsoft-defender-atp/tvm-exposure-score) prüfen können.
* Lesen Sie mehr zu [Sicherheitsempfehlungen](/windows/security/threat-protection/microsoft-defender-atp/tvm-security-recommendation).
* Verwalten Sie [Empfehlungen](/windows/security/threat-protection/microsoft-defender-atp/tvm-remediation) für eine höhere Sicherheit.
* Verwalten Sie die [Endpunkterkennung und Reaktion](/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response).
* Überwachen Sie Profile mit der [Intune-Profilüberwachung](/intune/device-profile-monitor).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Microsoft Intune](/intune/index)
* Grundlegendes zu [Azure AD](../index.yml)
* Arbeiten mit [Microsoft Defender Advanced Threat Protection](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)
* Entdecken der Funktionen von [Azure Sentinel](/azure/sentinel/)
