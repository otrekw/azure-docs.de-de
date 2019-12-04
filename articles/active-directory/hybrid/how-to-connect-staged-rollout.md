---
title: 'Azure AD Connect: Cloudauthentifizierung – gestaffelter Rollout | Microsoft-Dokumentation'
description: Hier wird erläutert, wie Sie mithilfe eines gestaffelten Rollouts eine Migration von der Verbundauthentifizierung zur Cloudauthentifizierung durchführen.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/07/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2596091324acde5c4fdc3f7c467849f90266fec9
ms.sourcegitcommit: 16c5374d7bcb086e417802b72d9383f8e65b24a7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73847227"
---
# <a name="cloud-authentication-staged-rollout-public-preview"></a>Cloudauthentifizierung Gestaffelter Rollout (Public Preview)

Mit diesem Feature können Sie mithilfe eines gestaffelten Ansatzes eine Migration von der Verbundauthentifizierung zur Cloudauthentifizierung durchführen.

Eine Ablösung der Verbundauthentifizierung hat Auswirkungen. Nehmen wir beispielsweise an, dass einer der folgenden Punkte auf Sie zutrifft:
    
-  Sie verfügen über einen lokalen MFA-Server. 
-  Sie verwenden Smartcards für die Authentifizierung. 
-  Sie nutzen andere reine Verbundfeatures.

Diese Features sollten vor der Umstellung auf die Cloudauthentifizierung berücksichtigt werden.  Bevor Sie dieses Feature testen, empfehlen wir Ihnen, unseren Leitfaden zum Auswählen der richtigen Authentifizierungsmethode zu lesen. Weitere Informationen finden Sie in [dieser Tabelle](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn#comparing-methods).

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE3inQJ]



## <a name="prerequisites"></a>Voraussetzungen

-   Sie verfügen über einen Azure AD-Mandanten mit Verbunddomänen.

-   Sie haben sich entweder für die Umstellung auf Kennworthashsynchronisierung + nahtloses einmaliges Anmelden **(Option A)** oder Passthrough-Authentifizierung + nahtloses einmaliges Anmelden **(Option B)** entschieden. Obwohl nahtloses einmaliges Anmelden (Single Sign-On, SSO) optional ist, empfiehlt es sich, das nahtlose einmalige Anmelden zu aktivieren, damit Benutzer, die in die Domäne eingebundene Computer innerhalb des Unternehmensnetzwerks verwenden, von einer automatischen Anmeldung profitieren.

-   Sie haben alle geeigneten Richtlinien für Mandantenbranding und für bedingten Zugriff konfiguriert, die Sie für Benutzer benötigen, die zur Cloudauthentifizierung migriert werden.

-   Wenn Sie Azure Multi-Factor Authentication verwenden möchten, empfehlen wir die Verwendung der [kombinierten Registrierung](../authentication/concept-registration-mfa-sspr-combined.md) für die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) und Azure MFA, damit Ihre Benutzer ihre Authentifizierungsmethoden einmalig registrieren können.

-   Um dieses Feature verwenden zu können, müssen Sie globaler Administrator für Ihren Mandanten sein.

-   Um nahtloses einmaliges Anmelden für eine bestimmte AD-Gesamtstruktur aktivieren zu können, müssen Sie Domänenadministrator sein.

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Die folgenden Szenarien werden für gestaffelten Rollout unterstützt:

- Dieses Feature wird nur für Benutzer unterstützt, die mit Azure AD Connect für Azure AD bereitgestellt wurden, und kann nicht von reinen Cloudbenutzern verwendet werden.

- Dieses Feature funktioniert nur für den Datenverkehr bei der Benutzeranmeldung in Browsern und auf Clients mit moderner Authentifizierung. Für Anwendungen oder Clouddienste mit Legacyauthentifizierung werden wieder Verbundauthentifizierungsflows verwendet. (Beispiel: Exchange Online mit deaktivierter moderner Authentifizierung oder Outlook 2010, das keine moderne Authentifizierung unterstützt.)

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

Die folgenden Szenarien werden für gestaffelten Rollout nicht unterstützt:

- Bestimmte Anwendungen senden bei der Authentifizierung den Abfrageparameter „domain\_hint“ an Azure AD. Diese Flows werden fortgesetzt, und für den gestaffelten Rollout aktivierte Benutzer verwenden weiterhin den Verbund für die Authentifizierung.

<!-- -->

- Administratoren können den Rollout der Cloudauthentifizierung mithilfe von Sicherheitsgruppen ausführen. (Cloudsicherheitsgruppen werden empfohlen, um bei Verwendung lokaler AD-Sicherheitsgruppen Synchronisierungslatenzen zu vermeiden.)

    - Sie können **maximal 10 Gruppen pro Feature** verwenden, d. h. für jede Kennworthashsynchronisierung/Passthrough-Authentifizierung/nahtlose einmalige Anmeldung.

    - Geschachtelte Gruppen werden **nicht unterstützt**. Dieser Umfang gilt auch für die öffentliche Vorschau.

    - Dynamische Gruppen werden für den gestaffelten Rollout **nicht unterstützt**.

    - Kontaktobjekte innerhalb der Gruppe blockieren das Hinzufügen der Gruppe.

- Die endgültige Umstellung von der Verbundauthentifizierung auf die Cloudauthentifizierung muss weiterhin mithilfe von Azure AD Connect oder PowerShell erfolgen. Mit diesem Feature werden Domänen nicht von Verbund- auf verwaltete Domänen umgestellt.

- Wenn Sie zum ersten Mal eine Sicherheitsgruppe für den gestaffelten Rollout hinzufügen, ist sie auf 200 Benutzer begrenzt, um ein Timeout der Benutzeroberfläche zu vermeiden. Nachdem die Gruppe auf der Benutzeroberfläche hinzugefügt wurde, können Sie der Gruppe nach Bedarf weitere Benutzer direkt hinzufügen.

## <a name="get-started-with-staged-rollout"></a>Erste Schritte mit gestaffeltem Rollout

Wenn Sie die Anmeldung per Kennworthashsynchronisierung (Password Hash Sync, PHS) mit gestaffeltem Rollout testen möchten, führen Sie die unten aufgeführten vorbereitenden Schritte aus, um die Kennworthashsynchronisierung für den gestaffelten Rollout zu aktivieren.

Weitere Informationen zu den verwendeten PowerShell-Cmdlets finden Sie unter [AzureAD 2.0 (Vorschauversion)](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout).

## <a name="pre-work-for-password-hash-sync"></a>Vorbereitende Schritte für die Kennworthashsynchronisierung

1. Aktivieren Sie die Kennworthashsynchronisierung über die Seite [Optionale Features](how-to-connect-install-custom.md#optional-features) in Azure AD Connect. 

   ![Screenshot der Seite „Optionale Features“ in Azure Active Directory Connect](media/how-to-connect-staged-rollout/sr1.png)

1. Stellen Sie sicher, dass ein vollständiger Zyklus der Kennworthashsynchronisierung ausgeführt wurde, damit alle Kennworthashes der Benutzer mit Azure AD synchronisiert wurden. Mithilfe der [hier beschriebenen](tshoot-connect-password-hash-synchronization.md) PowerShell-Diagnose können Sie den Status der Kennworthashsynchronisierung überprüfen.

   ![Screenshot des AADConnect-Problembehandlungsprotokolls](./media/how-to-connect-staged-rollout/sr2.png)

Wenn Sie die Anmeldung per Passthrough-Authentifizierung (PTA) mit gestaffeltem Rollout testen möchten, führen Sie die unten aufgeführten vorbereitenden Schritte aus, um die Passthrough-Authentifizierung für den gestaffelten Rollout zu aktivieren.

## <a name="pre-work-for-pass-through-authentication"></a>Vorbereitende Schritte für die Passthrough-Authentifizierung

1. Identifizieren Sie einen Server unter Windows Server 2012 R2 (oder höher), auf dem der Passthrough-Authentifizierungs-Agent ausgeführt werden soll (**WÄHLEN SIE NICHT den Azure AD Connect-Server aus**). Stellen Sie sicher, dass der Server in die Domäne eingebunden ist, die ausgewählten Benutzer bei Active Directory authentifizieren und mit Azure AD über ausgehende Ports/URLs kommunizieren kann (siehe [Voraussetzungen](how-to-connect-sso-quick-start.md)).

1. [Laden Sie den Microsoft Azure AD Connect-Authentifizierungs-Agent herunter](https://aka.ms/getauthagent), und installieren Sie ihn auf dem Server. 

1. Installieren Sie zusätzliche Authentifizierungs-Agents auf anderen Servern, um [Hochverfügbarkeit](how-to-connect-sso-quick-start.md) zu ermöglichen.

1. Stellen Sie sicher, dass Sie die Einstellungen für die intelligente Sperre ([Smart Lockout](../authentication/howto-password-smart-lockout.md)) entsprechend konfiguriert haben. Dadurch wird sichergestellt, dass die lokalen Active Directory-Konten Ihrer Benutzer nicht von böswilligen Akteuren gesperrt werden.

Es wird empfohlen, nahtloses einmaliges Anmelden unabhängig von der für den gestaffelten Rollout ausgewählten Anmeldemethode (Kennworthashsynchronisierung oder Passthrough-Authentifizierung) zu aktivieren. Führen Sie die unten aufgeführten vorbereitenden Schritte aus, um nahtloses einmaliges Anmelden für den gestaffelten Rollout zu aktivieren.

## <a name="pre-work-for-seamless-sso"></a>Vorbereitende Schritte für nahtloses einmaliges Anmelden

Aktivieren Sie nahtloses SSO in den AD-Gesamtstrukturen mithilfe von PowerShell. Wenn Sie über mehrere AD-Gesamtstrukturen verfügen, aktivieren Sie das gleiche Feature für jede Gesamtstruktur einzeln. Nahtloses SSO wird nur für Benutzer ausgelöst, die für den gestaffelten Rollout ausgewählt wurden, und hat keine Auswirkungen auf Ihre bestehende Verbundeinrichtung.

**Zusammenfassung der Schritte**

1. Melden Sie sich zuerst beim Azure AD Connect-Server an.

2. Navigieren Sie zum Ordner „%Programme%\\Microsoft Azure Active Directory Connect“.

3. Importieren Sie das PowerShell-Modul für nahtloses SSO mit dem Befehl `Import-Module .\AzureADSSO.psd1`.

4. Führen Sie PowerShell als Administrator aus. Rufen Sie in PowerShell den Befehl `New-AzureADSSOAuthenticationContext` auf. Mit diesem Befehl sollte ein Dialogfeld geöffnet werden, in dem Sie die Anmeldeinformationen des globalen Administrators Ihres Mandanten eingeben können.

5. Rufen Sie `Get-AzureADSSOStatus | ConvertFrom-Json` auf. Dadurch erhalten Sie die Liste der AD-Gesamtstrukturen (siehe Liste \"Domänen\"), in denen dieses Feature aktiviert ist. Standardmäßig ist die Option auf der Mandantenebene auf „false“ festgelegt.

   > **Beispiel:** 
   > ![Beispiel der Windows PowerShell-Ausgabe](./media/how-to-connect-staged-rollout/sr3.png)

6. Rufen Sie `$creds = Get-Credential` auf. Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen des Domänenadministrators für die vorgesehene AD-Gesamtstruktur ein.

7. Rufen Sie `Enable-AzureADSSOForest -OnPremCredentials $creds` auf. Mit diesem Befehl wird das Computerkonto „AZUREADSSOACC“ vom lokalen Domänencontroller für diese spezielle Active Directory-Gesamtstruktur erstellt, die für nahtloses SSO erforderlich ist.

8. Nahtloses SSO erfordert, dass sich URLs in der Intranetzone befinden. Informationen zum Bereitstellen dieser URLs mithilfe von Gruppenrichtlinien finden Sie unter [Nahtloses einmaliges Anmelden mit Azure Active Directory: Schnellstart](how-to-connect-sso-quick-start.md#step-3-roll-out-the-feature).

9.  Sie können auch unsere [Bereitstellungspläne](https://aka.ms/SeamlessSSODPDownload) für nahtloses SSO herunterladen. Darin finden Sie eine vollständige exemplarische Vorgehensweise.

## <a name="enable-staged-rollout"></a>Aktivieren von gestaffeltem Rollout

Führen Sie die folgenden Schritte aus, um den Rollout eines bestimmten Features (Passthrough-Authentifizierung/Kennworthashsynchronisierung/nahtloses SSO) für ausgewählte Benutzer in einer Gruppe auszuführen:

### <a name="enable-the-staged-rollout-of-a-specific-feature-on-your-tenant"></a>Aktivieren des gestaffelten Rollouts eines bestimmten Features in Ihrem Mandanten

Sie können Rollouts für eine der folgenden Optionen ausführen:

-   Kennworthashsynchronisierung + nahtloses einmaliges Anmelden **(Option A)**

-   Passthrough-Authentifizierung + nahtloses einmaliges Anmelden **(Option B)**

-   Kennworthashsynchronisierung + Passthrough-Authentifizierung + nahtloses einmaliges Anmelden **-\>** ***nicht unterstützt***

Führen Sie die folgenden Schritte aus:

1. Melden Sie sich über die folgende URL beim Azure AD-Portal an, um auf die Vorschau-UX zuzugreifen.

   > <https://aka.ms/stagedrolloutux>

2. Klicken Sie auf „Gestaffelten Rollout für verwaltete Benutzeranmeldung aktivieren (Vorschau)“.

   *Beispiel:* (**Option B**) Wenn Sie die Kennworthashsynchronisierung und nahtloses einmaliges Anmelden aktivieren möchten, verschieben Sie den Schieberegler der Features „Kennworthashsynchronisierung“ und „Nahtloses einmaliges Anmelden“ auf **Ein**, wie unten dargestellt.

   ![](./media/how-to-connect-staged-rollout/sr4.png)

   ![](./media/how-to-connect-staged-rollout/sr5.png)

3. Fügen Sie dem Feature die entsprechenden Gruppen hinzu, um die Passthrough-Authentifizierung und nahtloses einmaliges Anmelden zu aktivieren. Stellen Sie sicher, dass die Sicherheitsgruppen anfangs nicht mehr als 200 Mitglieder umfassen, um ein Timeout der Benutzeroberfläche zu vermeiden.

   ![](./media/how-to-connect-staged-rollout/sr6.png)

   >[!NOTE]
   >Die Mitglieder einer Gruppe werden automatisch für den gestaffelten Rollout aktiviert. Geschachtelte und dynamische Gruppen werden für den gestaffelten Rollout nicht unterstützt.

## <a name="auditing"></a>Überwachung

Wir haben Überwachungsereignisse für die verschiedenen Aktionen aktiviert, die wir für gestaffelte Rollouts ausführen.

- Überwachungsereignis: Sie aktivieren den gestaffelten Rollout für Kennworthashsynchronisierung/Passthrough-Authentifizierung/nahtloses SSO.

  >[!NOTE]
  >Es wird ein Überwachungsereignis protokolliert, wenn nahtloses SSO mithilfe des gestaffelten Rollouts **aktiviert** wird.

  ![](./media/how-to-connect-staged-rollout/sr7.png)

  ![](./media/how-to-connect-staged-rollout/sr8.png)

- Überwachungsereignis: Eine Gruppe wird dem Feature „Kennworthashsynchronisierung“/„Passthrough-Authentifizierung“/„Nahtloses einmaliges Anmelden“ hinzugefügt.

  >[!NOTE]
  >Es wird ein Überwachungsereignis protokolliert, wenn eine Gruppe der Kennworthashsynchronisierung für den gestaffelten Rollout hinzugefügt wird.

  ![](./media/how-to-connect-staged-rollout/sr9.png)

  ![](./media/how-to-connect-staged-rollout/sr10.png)

- Überwachungsereignis: Ein der Gruppe hinzugefügter Benutzer wird für den gestaffelten Rollout aktiviert.

  ![](media/how-to-connect-staged-rollout/sr11.png)

  ![](./media/how-to-connect-staged-rollout/sr12.png)

## <a name="validation"></a>Überprüfen

Gehen Sie wie folgt vor, um die Anmeldung mit Kennworthashsynchronisierung oder Passthrough-Authentifizierung (Anmeldung mit Benutzername/Kennwort) zu testen:

1. Navigieren Sie in einer privaten Browsersitzung aus dem Extranet zu <https://myapps.microsoft.com>, und geben Sie den Benutzerprinzipalnamen (User Principal Name, UPN) des Benutzerkontos ein, das für den gestaffelten Rollout ausgewählt ist.

1. Wenn der Benutzer für den gestaffelten Rollout vorgesehen ist, wird der Benutzer nicht auf Ihre Verbundanmeldeseite umgeleitet, sondern stattdessen aufgefordert, sich auf der Mandantenbranding-Anmeldeseite von Azure AD anzumelden.

1. Stellen Sie sicher, dass die Anmeldung im [Azure AD-Bericht zu Anmeldeaktivitäten](../reports-monitoring/concept-sign-ins.md) erfolgreich angezeigt wird, indem Sie nach dem Benutzerprinzipalnamen (UserPrincipalName) filtern.

Gehen Sie wie folgt vor, um die Anmeldung mit nahtlosem SSO zu testen:

1. Navigieren Sie in einer privaten Browsersitzung aus dem Intranet zu <https://myapps.microsoft.com>, und geben Sie den Benutzerprinzipalnamen (User Principal Name, UPN) des Benutzerkontos ein, das für den gestaffelten Rollout ausgewählt ist.

1. Wenn der Benutzer für den gestaffelten Rollout des nahtlosen einmaligen Anmeldens vorgesehen ist, wird dem Benutzer die Meldung „Anmeldung wird versucht“ angezeigt, bevor er automatisch angemeldet wird.

1. Stellen Sie sicher, dass die Anmeldung im [Azure AD-Bericht zu Anmeldeaktivitäten](../reports-monitoring/concept-sign-ins.md) erfolgreich angezeigt wird, indem Sie nach dem Benutzerprinzipalnamen (UserPrincipalName) filtern.

Gehen Sie wie folgt vor, um Benutzeranmeldungen zu überprüfen, die weiterhin bei Verbundanbietern erfolgen:

In [diesen Anweisungen](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-logging#types-of-events) wird erläutert, wie Sie Benutzeranmeldungen nachverfolgen können, die weiterhin bei AD FS für ausgewählte Benutzer (gestaffelter Rollout) ausgeführt werden. Lesen Sie in der Dokumentation des Herstellers nach, wie dies für Drittanbieter-Verbundanbieter überprüft wird.

## <a name="roll-back"></a>Rollback

### <a name="remove-a-user-from-staged-rollout"></a>Entfernen eines Benutzers aus dem gestaffelten Rollout

1.  Durch das Entfernen des Benutzers aus der Gruppe wird der gestaffelte Rollout für den Benutzer deaktiviert.

2.  Wenn Sie das Feature für den gestaffelten Rollout deaktivieren möchten, verschieben Sie den Schieberegler des Features wieder auf **Aus**, um den gestaffelten Rollout zu deaktivieren.


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

-   **F: Kann ein Kunde dieses Feature in der Produktionsumgebung verwenden?**

-   A: Ja, dieses Feature kann in Ihrem Produktionsmandanten verwendet werden. Wir empfehlen Ihnen jedoch, dieses Feature zuerst in Ihrem Testmandanten auszuprobieren.

-   **F: Kann dieses Feature verwendet werden, um eine permanente „Koexistenz“ aufrechtzuerhalten, bei der einige Benutzer die Verbundauthentifizierung und andere die Cloudauthentifizierung verwenden?**

-   A: Nein, dieses Feature ist für die Migration von der Verbund- zur Cloudauthentifizierung in Phasen und dann schließlich zur Umstellung auf die Cloudauthentifizierung konzipiert. Von einem dauerhaften gemischten Zustand wird abgeraten, weil dies zu unerwarteten Authentifizierungsflows führen könnte.

-   **F: Kann PowerShell verwendet werden, um einen gestaffelten Rollout auszuführen?**

-   A: Ja. Informationen zum Verwenden von PowerShell zum Ausführen gestaffelter Rollouts finden Sie [hier](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout).

## <a name="next-steps"></a>Nächste Schritte
- [AzureAD 2.0 (Vorschauversion)](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout )
